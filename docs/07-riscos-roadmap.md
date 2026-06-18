# 7. Riscos e Roadmap

## Mapa de Riscos

### Risco Alto

| Risco | Impacto | Probabilidade | Detalhes |
|-------|---------|---------------|----------|
| Dependência de Código Customizado | Alto | Alta | 1.871 classes Apex (76,6% uso); 80,7% execuções > 5s |
| Dependência de Especialistas | Alto | Alta | Conhecimento tácito concentrado; workflow manual por 1 pessoa |
| Violações Governor Limits | Alto | Alta | 142.607 errors/mês; EPT acima de 3s |
| Integrações inseguras | Alto | Alta | 40 integrações HTTP sem autenticação adequada; credenciais em URL |

### Risco Médio

| Risco | Impacto | Probabilidade | Detalhes |
|-------|---------|---------------|----------|
| Crescimento do Modelo de Dados | Médio | Alta | 349 campos custom em Oportunidade; 31 Record Types |
| Evoluções Regulatórias | Médio | Média | Compliance de aprovações e auditoria |
| Inconsistência de dados entre Orgs | Médio | Média | Mitigação: Data Cloud + MuleSoft |
| Baixa adoção na nova Org | Médio | Média | Mitigação: treinamento por função |

### Riscos de Segurança Específicos

- Credenciais expostas em logs e URLs (ex: serviço de preço ARP)
- Múltiplos sistemas usando mesmo usuário genérico de integração (sem auditoria)
- Inconsistência entre Role Hierarchy e estrutura organizacional real
- Sharing Rules genéricas — exposição indevida de dados
- Excesso de `Modify All Data` / `View All Data` por conveniência
- Campos sensíveis (financeiros) sem Field-Level Security
- Hardcode em lógica de visibilidade (Profile Name, User ID)
- Ausência de rotinas formais de revisão periódica de acessos

### Riscos de Confiabilidade

- Timeouts frequentes SF → SAP e SF → Legados
- Falta de retry em integrações críticas (geração de cotação)
- Cache desatualizado: Canal de Negócios atualiza apenas 2x/dia
- Usuário de integração (`0052E00000FtwuH`) gera 35% dos row locks

### Riscos no Territory Management

- Hardcodes nos nomes das zonas e territórios
- Uso inadequado do Opportunity Team como mecanismo de delegação
- Atributos hierárquicos aplicados diretamente na Oportunidade sem abstração

### Riscos no Account Management

- Ambiguidade sobre Master Data (SAP x SF x Legado)
- Duplicidade de registros de clientes
- Ausência de governança de Grupos Econômicos

---

## Roadmap Recomendado

### Fase 1 – Saneamento e Fundação

**Objetivo:** Reduzir débito técnico e estabelecer pré-requisitos.

**Ações:**
- Estruturar CoE Salesforce com papéis e responsabilidades
- Provisionar instância Data Cloud dedicada
- Habilitar MuleSoft Anypoint Platform (CH 1.0/2.0)
- Criar novo Business Group MuleSoft para nova Org
- Resolver 21 Release Updates pendentes
- Endereçar item de "Ação Imediata" do Optimizer
- Definir modelo de coexistência (processos/usuários por Org)
- Definir Master Data (SAP vs SF como fonte de verdade)

### Fase 2 – Setup da Nova Org + Primeira Onda

**Objetivo:** Implementar fundação e primeiro domínio.

**Ações:**
- Setup da Nova Org com modelo de segurança (Princípio Menor Privilégio)
- Implementar Gestão de Territórios (ETM) integrado ao SAP
- Implementar Segmentação de Cliente com Data Cloud (visão 360)
- Migração de metadados essenciais
- Piloto com 1 BU (B2B recomendado)
- Modelo de segurança: SSO, acessos, Permission Sets
- Sincronização de dados entre Orgs via Data Cloud

### Fase 3 – Revenue Cloud + OmniStudio

**Objetivo:** Modernizar core comercial.

**Ações:**
- Revenue Cloud (CPQ + CLM + Billing): cotações, contratos, pedidos, precificação
- OmniStudio para jornadas guiadas (contratação, NEG)
- Migração das Alçadas de Aprovação para BRE + Advanced Approvals
- DocuSign nativo integrado ao CLM
- Einstein AI para scoring e geração de parecer

### Fase 4 – Expansão e Consolidação

**Objetivo:** Migrar todas as BUs e consolidar.

**Ações:**
- Migração progressiva: B2C, Lubrax, BRMania
- Migração do Service Cloud (com refactor) para Single Org definitiva
- Otimização pós-migração (performance, limpeza)
- Consolidação do backlog gerenciado pelo COE
- Desativação progressiva da Org anterior

---

## Plano de Migração Detalhado

| Fase | Atividades |
|------|-----------|
| 1. Planejamento | Avaliação AS-IS; confirmar TO-BE; análise de gaps; due diligence (dados inconsistentes, metadados) |
| 2. Definição de Arquitetura | Modelo de coexistência; SSO; Data Cloud; MuleSoft; governança compartilhada; estratégia Faseada |
| 3. Execução | Setup Org; criar usuários; modelo segurança; migração metadados/dados; reimplementação; testes; treinamento |
| 4. Coexistência Controlada | Monitoramento; gestão de exceções; suporte dual; métricas de sucesso; checkpoints |
| 5. Rollout | Validação final; migração processos/dados restantes; desativação progressiva Org anterior |

### Estratégia de Rollout Faseado

1. Definir por qual BU iniciar (B2B, B2C, Lubrax, BRMania)
2. Dentro da BU: por região, sub-região, zona de vendas ou carteira de EVs
3. Extração → carga na nova Org → ativação de usuários
4. Piloto bem-sucedido → repetir para todas as BUs

### Estratégia de Migração: Lift, Transform & Move
- NÃO é Lift & Shift
- Revenue Cloud implementa processos nativamente
- Elimina necessidade de manter customizações legadas

---

## Quick Wins (Alto Impacto, Baixo Esforço)

1. **Cotação primária automática** — elimina sincronização manual
2. **Valores padrão em campos** — preencher dados repetitivos automaticamente
3. **Tela de aprovação com informações completas** — incluir nome do cliente e dados mínimos
4. **Separação de visões B2B vs B2C** — ocultar campos irrelevantes por perfil
5. **Campos fórmula para cálculo de saldo devedor × margem** — eliminar recálculo manual SNEG
6. **Alertas de vencimento de contrato** — funcionalidade nativa existente
7. **Document Checklist nativo** — substituir gestão manual de documentos
8. **Produtos por segmento** — evitar seleção incorreta de produtos
9. **Endereçar Release Updates críticos** — 21 pendentes, 1 de ação imediata
10. **Objeto Case para workflow** — substituir dependência de 1 pessoa

---

## Indicadores de Sucesso

### Macro Processo Contratação
- ▲ Valor Médio do Contrato (ACV)
- ▲ Taxa de Renovação de Contratos
- ▲ Valor Vitalício do Cliente (CLTV)
- ▲ Número de Contratos Criados
- ▼ Ciclo do Contrato (média de dias até fechamento)
- ▼ Tempo em Cada Estágio
- ▼ Tempo de Onboarding (Novos Usuários)
- ▼ Tempo de Entrega de Funcionalidade / Custo de Manutenção

### Macro Processo Venda
- ▲ Número de Oportunidades Fechadas/Ganhas
- ▲ Taxa de Ganho (Win Rate)
- ▲ Tamanho Médio dos Pedidos
- ▼ Ciclo de Vendas (dias para fechar)
- ▼ Tempo em Cada Estágio

### Redução de Ciclo de Contratação (Meta)

| Etapa | Tempo Atual | Tempo Proposto |
|-------|-------------|----------------|
| Análise de Oportunidade | 54–89 dias | 1–5 dias |
| Produto | 19–25 dias | 3 dias |
| TIR | 18–27 dias | 2 dias |
| Aprovação | — | 1–3 dias |

### Técnicos
- Redução de automações redundantes
- Redução de dependências cruzadas
- Redução de governor limit errors
- EPT < 3 segundos
- Customização < 40%

### Operacionais
- Menor esforço de manutenção
- Releases de 8–12 semanas (vs. 16–20 atual)
- Menor volume de incidentes

---

## Resultado Esperado do Assessment

Ao final, a Vibra deverá possuir:

- Visão clara dos riscos técnicos atuais
- Inventário de débitos técnicos
- Mapa de dependências da solução
- Direcionamento arquitetural futuro (Single Org Nova)
- Roadmap de evolução priorizado
- Recomendações priorizadas por impacto e esforço
- Estratégia para saneamento e modernização
