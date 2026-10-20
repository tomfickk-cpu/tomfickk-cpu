# Wellington Carlos Fick

### Desenvolvedor de Software · Arquitetura Backend · AI-Native Engineering

[![LinkedIn](https://img.shields.io/badge/LinkedIn-conectar-0A66C2)](https://www.linkedin.com/in/wellington-carlos-fick-416111431)
[![GitHub](https://img.shields.io/badge/GitHub-tomfickk--cpu-181717)](https://github.com/tomfickk-cpu)

Desenvolvedor focado em **backend, arquitetura de sistemas distribuídos e engenharia orientada a IA**. Projeto e mantenho em produção sistemas reais — não protótipos: uma plataforma de orquestração multiagente e um serviço de visão computacional para operação de restaurantes.

Trabalho diariamente com o que o mercado chama de *AI-native development*: **context engineering, MCP, subagentes tipados, orquestração multiagente e spec-first** — como sistema real em operação, com rigor, testes e autonomia.

---

## 🧠 [ORCHESTRATOR](https://github.com/tomfickk-cpu/orchestrator)

**894 agentes de IA · 960 testes · cobertura 100% do núcleo · v4.51.0**

Plataforma de orquestração neural multiagente em produção:

- **Roteamento por intenção** com confiança explícita e fallback 3+1 camadas
- **DAG waves** — delegação paralela com subagentes validados por registry
- **Tiering cognitivo** — modelo econômico primeiro, escalonamento automático via avaliador de qualidade
- **Fila durável** SQLite com leases, retry, idempotência e resume
- **Ledger append-only** com replay completo de cada execução
- **Governança de custo** — 4 tetos de budget com metering real
- **Observabilidade** — Prometheus + 21 dashboards Grafana
- **Compliance nativa** — LGPD, eMAG e verticais brasileiras (Pix/BACEN, licitações)

📊 **Métricas de produção:** 364 runs no ledger · 97,25% de sucesso · roteamento < 1s · 51 releases versionadas

---

## 🛡️ [GERENTE GUARDIÃO](https://github.com/tomfickk-cpu/gerente-guardiao)

**Visão computacional real · 41 testes · 19 checks E2E · separação ReID 0.64**

Serviço anti-evasão para restaurantes — detecta saídas sem pagamento em tempo real:

- **ReID por aparência sem reconhecimento facial** — OSNet x1_0 (ONNX) + cor + altura + trajetória → LGPD-friendly por design
- **POS como fonte de verdade, fail-closed** — integração por polling + webhook; indisponibilidade nunca libera uma conta
- **Multi-câmera com re-identificação** — limiar calibrável por câmera, por tenant
- **Loop de aprendizado auditado** — feedback do gerente ajusta o modelo; toda mudança de configuração tem trilha de auditoria e rollback em 1 clique
- **Alerta que chega** — WebSocket no dashboard + WhatsApp/webhook, com deduplicação persistida e retry — alarme confiável, não spam
- **CI containerizado que prova a ponta** — a simulação de visão precisa gerar um alerta real consultável na API para o pipeline passar

**Stack:** NestJS · PostgreSQL + pgvector · Python/ONNX/OpenCV · Next.js · Socket.IO · Docker

---

## Competências técnicas

| Área | Stack |
|---|---|
| **Backend** | Python (avançado), Node.js/TypeScript, NestJS, Express |
| **Arquitetura** | Clean/Hexagonal, DDD, event sourcing, sagas, filas duráveis, multi-tenant |
| **IA aplicada** | Orquestração multiagente, MCP, context engineering, avaliadores de qualidade, RAG |
| **Visão computacional** | OpenCV, ONNX Runtime, ReID (OSNet), tracking multi-câmera |
| **Dados** | PostgreSQL + pgvector, SQLite, Redis, JSONL append-only, JSON Schema como contrato |
| **Qualidade** | Jest, pytest, TDD, coverage gates, smoke E2E, code review |
| **DevOps** | Docker, GitHub Actions, CI/CD, releases semânticas, Prometheus/Grafana, Linux |
| **Frontend** | React, Next.js, Vue, HTML/CSS responsivo |
| **Compliance** | LGPD, eMAG/WCAG, OWASP |

## Como eu trabalho

- **Verificação antes de afirmação** — métricas de produção vêm do ledger/banco, não de estimativa
- **CI que prova comportamento** — pipeline não passa com exit 0; passa quando o efeito real é observável
- **Fail-closed em dinheiro e segurança** — dúvida nunca libera recurso
- **Auditabilidade** — decisões automatizadas deixam trilha reversível
- **Documentação viva** — contratos e ADRs atualizados junto com o código

---

*Código-fonte dos projetos é proprietário. Repositórios públicos são vitrines técnicas: arquitetura, decisões e evidências de produção. Demos e detalhes sob NDA.*
