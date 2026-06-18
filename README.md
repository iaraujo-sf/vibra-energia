# Vibra Energia – Assessment Salesforce

Assessment da solução de Negociação Comercial construída sobre Salesforce Sales Cloud.

## Estrutura do Repositório

```
docs/
  01-objetivo-contexto.md       - Objetivo, contexto e motivadores
  02-escopo-metodologia.md      - Escopo e framework de avaliação (5 dimensões)
  03-achados-assessment.md      - 8 Big Rocks, métricas de saúde, dores
  04-arquitetura.md             - AS-IS detalhado, decisão Greenfield, TO-BE com domínios
  05-requisitos.md              - Processos detalhados, personas, dois ciclos de aplicação
  06-stack-tecnologica.md       - Stack completa, MuleSoft scores, custos comparativos
  07-riscos-roadmap.md          - Riscos, roadmap 4 fases, quick wins, KPIs
  08-conclusao-executiva.md     - Conclusão executiva
  09-integracoes.md             - 153 integrações, 5 pilares, sistemas, TO-BE
  10-analise-heuristica.md      - 20 problemas UX (Nielsen), severidade, recomendações
  11-salesforce-optimizer.md    - Optimizer results, métricas de governança
  12-storytelling-bsa.md        - Narrativa técnica, exemplos, cenários
  13-roadmap-nova-org.md        - Roadmap de implementação Nova Org (4 fases)

raw/                            - Arquivos originais (PPTX, XLSX, DOCX) — não versionados
raw_text/                       - Texto extraído dos arquivos originais — não versionados
```

## Contexto

A solução atual suporta processos críticos de negociação comercial da Vibra (propostas, pricing, aprovações, contratos). O assessment identificou 8 Big Rocks arquiteturais e recomenda migração para uma nova Org Salesforce (Greenfield) com arquitetura modular baseada em domínios.

## Decisão Principal

**Nova ORG Salesforce (Single Org)** com Revenue Cloud, OmniStudio, Data Cloud, ETM, e MuleSoft API-LED.

## Roadmap de Implementação (Nova Org)

1. **Fase 1 — Fundação + Vendas** – Hierarquia, Territórios, V360, Leads, NEGs, Orders, Forecasting
2. **Fase 2 — Integrações Comerciais** – SAP (Preços, NEGs, Pedidos)
3. **Fase 3 — Atendimento + Canais Digitais** – Cases, OmniChannel, Voice, WhatsApp, Agentforce, Einstein
4. **Fase 4 — Serviços Avançados** – Backoffice completo, IA Preditiva/Generativa
