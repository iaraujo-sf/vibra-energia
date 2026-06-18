# 4. Arquitetura

## Arquitetura Atual (AS-IS)

### Visão Geral

A arquitetura atual pode ser caracterizada como:

- **Monolítica:** Grande concentração de responsabilidades dentro da própria org
- **Orientada a Customização:** Predominância de componentes customizados (meta: manter abaixo de 40%)
- **Acoplada:** Dependências entre objetos, processos e integrações
- **Performance degradada:** 80,7% das execuções Apex acima de 5 segundos

### Modelo de Dados AS-IS

| Objeto | Custom Fields | Page Layouts | Validation Rules | Record Types |
|--------|--------------|--------------|-----------------|--------------|
| Conta (Account) | 268 | 23 | 88 | 20 |
| Oportunidade (Opportunity) | 349 | 38 | 57 | 31 |
| Cotação (Quote/Cotacao) | 237 | 20 | 34 | 24 |

**Campos no objeto errado (AS-IS):**

| Campo/Entidade | Localização Atual | Localização Correta |
|----------------|-------------------|---------------------|
| Situação do Posto/Imóvel | Cotação | Conta |
| Área de Influência | Cotação | Conta |
| Fotos do Posto | Arquivos da Oportunidade | Conta |
| Investimentos | Cotação | Oportunidade |
| Recursos Financeiros | Cotação | Oportunidade |
| Franquias | Cotação | Oportunidade |
| Receitas e Despesas | Cotação | Oportunidade |
| Garantias | Cotação | Oportunidade |
| Sócios da Cotação | Cotação | Oportunidade |
| Exposições Financeiras | Cotação | Oportunidade |
| Dados do Contrato | Campos da Cotação | Campos do Contrato |

### Volumes de Dados (Org Atual)

| Objeto | Volume Aproximado |
|--------|-------------------|
| Cases | ~5 M |
| Contracts | ~9 M |
| Quotes | ~2,8 M |
| Opportunities | ~2,8 M |
| Tasks | ~3,6 M |
| Accounts | ~1,3 M |
| Orders | ~871 K |
| DD_ItemEntrega__c | LDV |
| DD_Programacao__c | LDV |
| Produto_Do_Contrato__c | LDV |
| Investimento_Da_Cotacao__c | LDV |

### Automações Apex com Problemas Críticos

| Método | Tipo | Count (30d) | Over 5s | Avg Runtime | Max Runtime |
|--------|------|-------------|---------|-------------|-------------|
| SimulacaoContainerController.finishAndUpdateOpportunity | Sync | 793 | 550 | 5,6s | 14s |
| SincronizarCotacaoController.syncOpportunity | Sync | — | — | — | 14s |
| SimulacaoContainerController.syncRecordsWithOpportunityAsync | Future | 2.246 | 1.786 | 6,3s | 16,7s |

### Indicadores de Saúde (últimos 30 dias)

| Indicador | Valor |
|-----------|-------|
| Governor Limit Errors | 142.607 |
| Row Locks | 993 |
| Unsuccessful Callouts | 17.724 |
| BulkAPI Requests | 314K |
| RestAPI Requests | 14M |
| Callouts | 8M |

---

## Decisão Estratégica: Nova ORG (Greenfield)

### Cenários Avaliados

**Cenário 1 – Refactoring na Org Atual — NÃO RECOMENDADO**
- 1.871 classes Apex (76,6% de uso)
- 82 perfis + 558 permission sets sem governança
- 17.000 callouts falhos/mês
- +140.000 governor errors/mês
- Health Check < 55%
- 993 row-locks/mês
- Custo de refactoring maior que migração no médio/longo prazo

**Cenário 2 – Single Org Nova — RECOMENDADO**
- Arquitetura limpa com boas práticas desde o início
- Menor custo de licenciamento vs. Multi-Org
- Relatórios e painéis unificados
- Colaboração B2B/B2C mais simples
- Governança, LWC, OmniStudio, Revenue Cloud, Data Cloud

**Cenário 3 – Multi-Org — Apenas transitório**
- Aplicável durante coexistência (Org Nova Sales + Org Atual Service Cloud)
- Maior custo e complexidade
- Convergir para Single Org após migração

### Evolução dos Estados

1. **Hoje:** Single Org atual (monolito Sales + Service)
2. **Transitório:** Multi-Org (Org Nova Sales/Negociação + Org Atual Service/Suporte) — via Data Cloud + MuleSoft
3. **Futuro:** Single Org unificada (Sales + Service Cloud migrado)

---

## Arquitetura Alvo (TO-BE)

### Princípios Arquiteturais

- Low Code First
- Event Driven quando aplicável
- API First
- Separation of Concerns
- Domain Driven Design (conceitualmente)
- Reutilização de componentes
- Governança centralizada
- Minimização de código customizado (meta: < 40%)

### Camadas da Arquitetura Alvo

#### Camada de Experiência
- Salesforce Lightning Experience
- OmniStudio (fluxos complexos guiados)
- Lightning Pages + Console (fluxos simples)
- Mobile-First para aprovações

#### Camada de Processo
- Salesforce Flow como mecanismo principal
- OmniStudio para jornadas guiadas
- Approval Processes nativos + Advanced Approvals
- Business Rule Engine (BRE) para regras declarativas

#### Camada de Domínio

| Domínio | Componentes | Responsabilidade |
|---------|-------------|------------------|
| Negociação | Sales Cloud + OmniStudio | Propostas, condições comerciais, simulação |
| Pricing | Revenue Cloud CPQ | Políticas, cálculos, regras de preço |
| Aprovação | OmniStudio + BRE + Advanced Approvals | Alçadas, governança, parecer |
| Contratação | Revenue Cloud CLM + DocuSign | Formalização, ciclo contratual, renovação |
| Territórios | Enterprise Territory Management (ETM) | Hierarquia comercial, atribuição |
| Segmentação | Data Cloud | Visão 360, perfil unificado de clientes |
| Pedidos | Revenue Cloud (Order) | Pedido SPOT, Pedido DIRETO, fulfillment |
| Consumo/Faturamento | Revenue Cloud Billing | Consulta, cálculo, faturamento |

#### Camada de Integração
- MuleSoft API-LED Connectivity (Experience / Process / System)
- APIs desacopladas com contratos estáveis
- Governança via Anypoint Platform (API Manager, Runtime Manager)
- Observabilidade: Log Search via Correlation ID, Alerts (CPU, Memory, Threads)

#### Camada de Dados
- Data Cloud como plataforma de dados estratégica
- Redução de redundâncias
- Responsabilidade única por entidade
- Identity Resolution para clientes

### Modelo de Dados TO-BE

#### Objetos Standard Redesenhados

| Objeto | Papel TO-BE |
|--------|-------------|
| Opportunity | Objeto principal da jornada de contratação |
| Quote | Cotação vinculada à Oportunidade via Revenue Cloud |
| Contract | Gestão de contratos via Revenue Cloud CLM |
| Order | Pedidos SPOT e DIRETO (sem nova Oportunidade/Cotação) |
| Case | Suporte + análises financeiras |
| QuoteLineItem | Itens de linha (Revenue Cloud) |

#### Objetos Customizados TO-BE (em Opportunity)

- Nota PM da Oportunidade
- Investimentos
- Recursos Financeiros
- Franquias
- Receitas e Despesas
- Garantia
- TIR (parâmetro Revenue Cloud)
- Área de Influência
- Exposição Financeira do Grupo Econômico

### Diagramas de Solução por Capacidade

| Capacidade | Componentes |
|------------|-------------|
| Gestão de Territórios | Salesforce ETM ← SAP via MuleSoft → Data Cloud |
| Segmentação de Cliente | Data Ingestion → Data Modeling → Identity Resolution → Calculated Insights → Activation |
| Gestão de Contratos | Oportunidade → Revenue Cloud CLM → E-Signature → sincronização SAP |
| Gestão de Pedido/Preço | CPQ → MuleSoft (SAP + Azure) → Order Management → SAP |
| Alçadas de Aprovação | OmniStudio (UI) → BRE → Apex (ordenação) → Advanced Approvals |
| Regras de Negócio | CPQ Price/Product Rules + Flow Automation + Validation Rules |
| Consumo/Faturamento | MuleSoft → SF calcula/consulta → exibe em Cliente/Contrato; Revenue Cloud billing |
| Segurança | Menor Privilégio → Core Access Control → Sharing & Visibility → Team-based → Auditing |

### Decisões Tecnológicas (ARB)

**Segmentação e Perfis de Clientes:**
- Salesforce Data Cloud (RECOMENDADO) — visão 360, zero copy, 200+ conectores

**Camada de Front-End / Experiência:**
- Abordagem híbrida: OmniStudio (fluxos complexos) + Lightning Pages (fluxos simples)

**Gestão de Contratos e Pedidos:**
- Revenue Cloud (CLM + CPQ + Billing)

**Negociação em Massa:**
- Execução direta no SAP (solicitada pelo CRM) — sem registros massivos no Salesforce
