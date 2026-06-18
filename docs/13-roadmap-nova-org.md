# 13. Roadmap Nova Org — Implementação

## Visão Geral

A Vibra adquiriu o projeto de implementação da nova Org Salesforce e definiu um roadmap em 4 fases cobrindo três grandes domínios: **Fundacional, Sales e Services**.

---

## Fases do Roadmap

### Fase 1 — Plan & Design + Fundação + Vendas

#### Plan & Design
- Planejamento e desenho da solução

#### Fundação
- Hierarquia de Papéis (Usuários)
- Estrutura Account Team (Papéis)
- Configurações OWD
- Estrutura de Territórios (Vínculo Área de Vendas)
- Contas & Contatos (V360)
- Relacionamento Conta × Contatos × Papéis
- Contact Point Address
- Contact Point Phone
- Contact Point Email
- Dados ANP
- Participação Societária
- Títulos Financeiros (Zero Copy)
- Fluxo Guiado de Cadastro de Cliente

#### Integrações Fundacionais
- Integração Receita Federal (Cadastro Clientes)
- Integração ANP (Cadastro Clientes)
- Processo Periódico de Validação Clientes (RFB / ANP)
- Integração SAP (Account)
- Integração SAP (Contact)

#### Vendas — Processos Comerciais
- Leads & Prospects
- Catálogo de Produtos
- Opportunities (NEGs)
- Orders (Pedido Direto / Pedido Spot)
- Atividades & Tarefas
- Colaboração & Comunicação
- Forecasting
- Carteirização

---

### Fase 2 — Integrações Processos Comerciais

- Integração SAP (Simulação de Preços)
- Integração SAP (NEGs)
- Integração SAP (Pedidos)

---

### Fase 3 — Atendimento + Canais Digitais + Marketing

#### Atendimento — Serviços Básicos
- Configuração de Filas
- Configuração de Skills
- Entitlements
- Milestones
- Cases
- OmniChannel
- Voice — Integração CTI Genesys
- Voice — Integração CTI Avaya
- OmniFlow
- Knowledge

#### Integrações — Atendimento
- Integrações Org Premmia

#### Canais e Agentes Digitais
- Digital Engagement (Enhanced Channels — WhatsApp)
- Chatbots
- Agentforce
- Einstein for Service

#### Comunicação Ativa — Marketing
- UCP
- Jornadas de relacionamento com clientes
- Processos de cobrança e inadimplência
- Processos de ofertas de produtos
- Call center, agências, chat, chatbot, WhatsApp, email e mídias sociais

---

### Fase 4 — Atendimento Serviços Avançados

- Gestão de todos os serviços e etapas de backoffice
- Integração com todos os processos e sistemas
- IA Preditiva e Generativa para redução do TMA e custos de atendimento

---

## Premissas

### Sales Cloud

| Premissa | Detalhes |
|----------|----------|
| Alçadas simplificadas | Processo de identificação de alçadas simplificado |
| Aprovação externa | Determinação de alçadas e etapas via integração síncrona (fora do Salesforce) |
| Separação NEG × Pedidos | Processos de NEG (condições comerciais) separados dos processos de Pedidos |
| Produtos sem limite por área | NEGs e Pedidos permitem qualquer produto; app faz split por segmento automaticamente |
| Funil/Forecast | Implementação de conceitos de oportunidades de vendas/negociação para utilização de funil e forecast |
| Contratos externos | Contratos tratados por ferramentas externas; mapear e implementar integrações necessárias |
| Territórios | Estruturas de territórios para contextualizar áreas de vendas e carteirização |
| Pedidos no Order | Pedidos Direto e Spot tratados diretamente no objeto Order |
| NEGs na Opportunity | Indicação de uso do objeto Oportunidade (pendente estudo de caso) |
| Leads & Prospects | Implementação de processos de conversão + Oportunidades de Vendas e Contratação |

### Service Cloud

| Premissa | Detalhes |
|----------|----------|
| Dependência de Sales | Processos de atendimento dependem dos processos comerciais implementados |
| Migração de voz | Possível migração OpenCTI → Service Voice |
| Org Premmia | Integração com Org Premmia (conceito headless) |
| SLA reformulado | Aderência aos processos OOB via Entitlements & Milestones |
| Skills-based routing | Reformulação de fluxo e atribuição de casos usando Skills |
| Omni Flow | Migração das Assignment Rules para Omni Flow (atribuição por Skills) |
| Knowledge Base | Artigos estruturados como insumo para agentes autônomos e co-pilots |
| Chatbots/Agentforce | Implementação de agentes de auto-serviço (mapear casos de uso) |
| Einstein for Service | Case Classification, Work Summary e Replies (depende da Knowledge Base) |

### Integrações

| Premissa | Detalhes |
|----------|----------|
| Event Driven Architecture | Priorizar Platform Events (possível necessidade de High Volume Platform Events AddOn). Padrão confiável, escalável e tolerante a falhas. Delega responsabilidades para ferramentas externas. |
| Precificação síncrona | Reformular integração de Precificação e Consulta de Condições Comerciais para respostas síncronas. Microsserviços em ambientes OnCloud de alta disponibilidade. |
| SAP PI → CPI | Sugestão de migração do SAP PI para CPI (Cloud Platform Integration) — estrutura OnCloud de alta disponibilidade. |

---

## Diferenças em Relação ao Assessment TO-BE

| Aspecto | Assessment TO-BE | Roadmap Nova Org |
|---------|-----------------|-----------------|
| Contratos | Revenue Cloud CLM + DocuSign nativo | Ferramentas externas + integrações |
| Cotações/Pricing | Revenue Cloud CPQ | Integração SAP Simulação (síncrona) |
| Aprovações | OmniStudio + BRE + Advanced Approvals | Integração síncrona externa |
| Middleware | MuleSoft API-LED | Event Driven (Platform Events) + SAP CPI |
| Guided Flows | OmniStudio | Fluxo Guiado de Cadastro (a definir tecnologia) |
| Data Cloud | Segmentação + sync multi-org | Títulos Financeiros (Zero Copy) — uso pontual |
| Service Cloud | Fora do escopo do assessment | Incluído nas Fases 3 e 4 |
| IA | Einstein AI (scoring, parecer) | Einstein for Service + Agentforce + IA Preditiva/Generativa |
| Scope | Apenas Negociação Comercial | Fundacional + Sales + Services + Marketing |
