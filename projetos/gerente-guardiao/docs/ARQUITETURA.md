# Arquitetura — Gerente Guardião

Decisões estruturais do sistema. Documento de vitrine: descreve o design sem expor implementação.

## Fluxo de decisão de alerta

```
evento de câmera (SEAT_ASSIGN / TRACK_UPDATE / EXIT_ATTEMPT)
  → identidade do track (reassociação ReID multi-câmera)
  → mesa associada? (sem mesa = sem avaliação de evasão)
  → estado da conta no POS (polling + webhook; fonte de verdade)
  → decisão:
      PAID           → saída liberada, sem alerta
      OPEN + saída   → EVASAO_ALERTA (critical) → WebSocket + webhook externo
      UNKNOWN        → SAIDA_PREVENTIVA (warning) — fail-closed
      mesa parada    → CONTA_ABANDONADA (agendado por maintenance)
```

## Por que fail-closed

A decisão envolve dinheiro. Se o POS está fora do ar, a regra conservadora é "tratar como conta em aberto até confirmação do garçom" — `pos_state: UNKNOWN` gera alerta preventivo (`waiter_confirm_required`), nunca liberação silenciosa. O inverso (fail-open) perderia evasões reais durante a falha — o pior momento possível.

## ReID sem reconhecimento facial

Descrição de aparência por pessoa, não identidade:

| Sinal | Peso típico | Papel |
|---|---|---|
| Vetor OSNet (512d, pgvector) | dominante | aparência global |
| Histograma de cor | médio | roupa/cor predominante |
| Altura estimada | baixo | desempate |
| Assinatura de trajetória | baixo | coerência espacial salão→saída |
| Atributos (texto livre do operador/visão) | médio | "camisa azul" etc. |

Score = fusão ponderada com pesos por tenant. Gait (marcha) atua como **veto**, não como sinal positivo — trajetória incompatível rejeita o match mesmo com aparência alta.

`vector` é indexado com **pgvector** (`ef_search` tunável) — re-match multi-câmera consulta embeddings direto no Postgres, sem serviço de vetor separado.

## Loop de aprendizado com governança

1. Gerente resolve o alerta: `true_positive` / `false_positive` / `incerto`
2. `AlertFeedback` persistido → `applyFeedback` re-pondera os pesos de fusão e sugere novo `match_threshold`
3. Sugestão **conservadora**: `confidence = min(1, feedbacks_decididos/10)`, blend com o valor atual, passo máximo ±0.15
4. Toda mudança — manual ou por feedback — grava `ReidConfigAudit` (actor, before/after, source)
5. Rollback em 1 clique restaura `prev_*` (e o rollback vira nova entrada de auditoria)
6. Drift: `fp_rate` acima do limiar operacional gera banner proativo no dashboard

## Canal de alerta externo

```
alerta critical
  → dedup persistido (mesa+tipo na janela — alerts.notified_at, sobrevive a restart)
  → POST webhook com retry (3 tentativas, backoff 1s/3s; 4xx não retenta)
  → notified_at persistido no sucesso
  → destinos: Slack/Discord/n8n/Twilio/whatsapp-bridge local (CDP, fila serializada)
```

Métricas expostas: `notifications.sent` (verdade do banco), `suppressed`, `failed`.

## Multi-tenant

JWT com `tenant_id`, RBAC por papel (manager/vision/admin), `assertRestaurant` em todas as rotas de dados — nenhum endpoint cruza tenant. Configurações ReID, thresholds por câmera e adaptadores POS são por tenant.

## Estratégia de testes

| Camada | O que prova |
|---|---|
| Jest (31 specs) | comportamento: retry, dedup pós-restart, paginação, fail-closed |
| pytest (10 specs) | descritor ONNX real no pipeline, estabilidade temporal, captura |
| e2e-smoke (12 checks) | cadeia inteira contra stack real: login→evento→alerta→feedback→report |
| ws-smoke (7 checks) | handshake socket, auth, alerta em tempo real, ACK |
| CI | compose real + vision container `--simulate` + assert de alerta na API |
