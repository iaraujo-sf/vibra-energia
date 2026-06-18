# 6. Stack Tecnológica

## Plataforma Principal

- Salesforce Sales Cloud (Org Nova — Greenfield)

## Habilitadores Salesforce TO-BE

| Componente | Função | Status |
|------------|--------|--------|
| Revenue Cloud (CLM/CPQ/Billing) | Contratos, cotações, pedidos, precificação | Novo |
| OmniStudio | Fluxos guiados complexos, guided selling | Novo |
| Data Cloud (CDP) | Segmentação, visão 360, sincronização multi-org | Novo |
| Enterprise Territory Management (ETM) | Gestão de territórios, hierarquia comercial | Novo |
| Advanced Approvals | Motor de aprovação dinâmico | Novo |
| Einstein AI | Scoring, recomendações, geração de parecer | Novo |
| DocuSign (integração nativa) | Assinatura eletrônica automatizada | Evolução |
| MuleSoft | Integração API-LED | Evolução (elevar maturidade) |

## Tecnologias Salesforce Core

### Plataforma
- Apex (redução gradual — apenas onde Flow não atende)
- Lightning Platform
- Lightning Web Components (LWC)
- OmniStudio (Guided Flows)

### Automação
- Flow (Record-Triggered, Screen, Scheduled) — mecanismo principal
- Approval Processes + Advanced Approvals
- Business Rule Engine (BRE) para regras declarativas
- Queueable / Batch Apex (operações assíncronas pesadas)

### Segurança
- Profiles (reduzir de 82 para mínimo necessário)
- Permission Sets (granularidade)
- Sharing Rules (simplificar)
- Enterprise Territory Management
- Princípio do Menor Privilégio

### Integração
- MuleSoft API-LED (Experience / Process / System layers)
- REST APIs padronizadas
- Platform Events (Event-Driven)
- Named Credentials
- OAuth 2.0 (padronizar — eliminar Basic Auth e credenciais em URL)

### Analytics
- Tableau + Tableau CRM (Einstein Analytics)
- Pipeline Inspection nativo

### Colaboração
- Slack (integrado)
- Salesforce Mobile (aprovações, consultas em campo)

---

## MuleSoft — Maturidade Atual vs. Alvo

| Dimensão | Score AS-IS | Maturidade | Score TO-BE |
|----------|-------------|------------|-------------|
| C4E (Center for Enablement) | 1,80 | Baixa | 4,0 (Alta) |
| Plataforma | 3,13 | Média | 4,0 (Alta) |
| Projetos | 3,24 | Média | 4,0 (Alta) |
| Treinamentos | 3,00 | Média | 4,0 (Alta) |

### Configurações MuleSoft TO-BE
- Novo Business Group para a nova Org
- Novo Data Model independente do atual
- Novas Client Credentials para a nova Org
- APIs existentes: incrementar versão major
- DevOps: ajustar pipeline GIT para novo Business Group
- Anypoint Platform: CH 1.0 / CH 2.0
- API Manager: Govern APIs
- Runtime Manager: Govern Apps
- Log Search: via Correlation ID
- Alerts: CPU, Memory, Threads
- Functional Monitoring: Test Cases

---

## Ferramentas de Governança e DevOps

- Git (versionamento)
- CI/CD (pipeline estruturado)
- Anypoint Platform (governança de APIs)
- Salesforce DevOps Center ou equivalente
- Estratégia de ambientes (Sandbox → UAT → PROD)
- Observabilidade operacional (logs, alertas, dashboards)

---

## Sistemas do Ecossistema

| Sistema | Função | Integração |
|---------|--------|------------|
| SAP | ERP — NEG, contratos, pedidos, preços | MuleSoft |
| ARP | Simulação de preços | MuleSoft |
| Azure Data Lake | Dados para segmentação | Data Cloud |
| Netlex | Gestão de minutas/contratos | MuleSoft |
| DocuSign | Assinatura eletrônica | Nativa SF |
| SGC | Gestão de consumo | MuleSoft |
| Tableau | Analytics/BI | Nativo SF |
| Portal do Cliente | Canal digital | MuleSoft |
| App +Negócios | Mobile B2B | MuleSoft |
| App +Transportes | Mobile transporte | MuleSoft |
| Canal CTV | Canal de vendas | MuleSoft |
| BRAX/Robbu | Robô NEG em massa | **Depreciar** (substituir por nativo) |
| Excel/Access | Planilhas paralelas | **Eliminar** (substituir por SF) |
| Neoway | Inteligência de mercado | Avaliar |

---

## Comparativo de Releases

| Contexto | Tempo de Release |
|----------|-----------------|
| ORG Atual | 16–20 semanas |
| Arquitetura Modular TO-BE | 8–12 semanas |
| Redução estimada | ~40-60% |

## Custo por Ponto de Funcionalidade

- Org atual consome **+35% mais tempo** de análise, dev e QA por ponto de funcionalidade
- Cada nova feature exige refatorar Apex em média **2 vezes** (+25% esforço dev)
- +15% de horas de QA por release para validação de acessos
- +3 semanas em ciclos de mudança por retrabalho em perfis
- Custo de hotfixes +40% devido à complexidade de rollback
