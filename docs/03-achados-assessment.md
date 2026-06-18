# 3. Principais Achados do Assessment

## Big Rocks Arquiteturais (8 Problemas Críticos)

### BR1 – Quebra do Monolito
Bloco único rígido onde tudo acontece em um único fluxo, difícil de adaptar, manter ou escalar. Grande concentração de lógica de negócio dentro da própria org Salesforce.

**Impactos:**
- Alto acoplamento
- Dificuldade de evolução
- Aumento do risco de regressão
- Releases alongados para 16–20 semanas (queda de -60% de velocidade)

### BR2 – Violações dos Limites da Plataforma
Governor Limit Errors: **142.607** nos últimos 30 dias.

**Impactos:**
- CPU Time excedido
- DMLs excessivas
- SOQLs encadeadas
- Performance degradada

### BR3 – Abordagem por Projetos (vs. Capacidades de Negócio)
Arquitetura orientada a projetos isolados em vez de capacidades empresariais.

**Impactos:**
- Sem ownership claro por domínio funcional
- Evolução fragmentada
- Backlog reativo sem priorização estratégica

### BR4 – Alto Índice de Customização
Campos, layouts, validation rules e record types em excesso.

| Objeto | Custom Fields | Page Layouts | Validation Rules | Record Types |
|--------|--------------|--------------|-----------------|--------------|
| Conta | 268 | 23 | 88 | 20 |
| Oportunidade | 349 | 38 | 57 | 31 |
| Cotação | 237 | 20 | 34 | 24 |

### BR5 – Plataforma Não Governada
Modelo de dados sem governança.

**Impactos:**
- Sem CoE estruturado
- Sem critérios de priorização
- Sem gestão de portfólio
- Customizações não rastreáveis

### BR6 – Fragilidade na Arquitetura de Integração
153 integrações analisadas com problemas críticos em 5 pilares.

**Indicadores (últimos 30 dias):**
| Métrica | Valor |
|---------|-------|
| Unsuccessful Integration Callouts | 17.724 |
| HTTP 500 (Internal Server Error) | 10.754 |
| HTTP 404 (Not Found) | 3.359 |
| HTTP 400 (Bad Request) | 1.088 |
| HTTP 502 (Bad Gateway) | 971 |
| HTTP 504 (Gateway Timeout) | 249 |

### BR7 – Modelo de Segurança Complexo
Não segue boas práticas, não está otimizado.

**Problemas:**
- 34 regras de compartilhamento de Conta
- 16 regras de compartilhamento de Oportunidade
- Regras dispersas baseadas em campos texto/booleanos preenchidos via código
- Territórios associados via Apex sem gerenciamento estruturado
- Excesso de usuários com Modify All Data / View All Data
- Campos sensíveis sem Field-Level Security
- Hardcode em lógica de visibilidade (Profile Name, User ID)

### BR8 – Problemas de Desempenho
Múltiplas funcionalidades com execução ineficiente.

**Métricas (últimos 30 dias):**
| Indicador | Valor |
|-----------|-------|
| Apex Síncrono — Count | 17.897 |
| Apex Síncrono — Over 5 seg | 14.439 (80,7%) |
| Apex Síncrono — Max Runtime | 16,3 seg |
| Apex Assíncrono — Count | 2.246 |
| Apex Assíncrono — Over 5 seg | 1.786 (79,5%) |
| Apex Assíncrono — Max Runtime | 16,7 seg |
| DML via API — Count | 100.126 |
| DML via API — Over 5 seg | 50.813 (50,7%) |
| Row Locks totais | 993 |

---

## Salesforce Health Check

- **36,4% em Vermelho** (crítico)
- 43,2% em Amarelo (atenção)
- Apenas 20,5% em Verde
- Nota geral abaixo de 54% = Muito Ruim

---

## Mapa de Dores Identificadas

### Dados
- Duplicidade entre Cotação e Oportunidade
- Campos no objeto errado (ver seção Modelo de Dados)
- Código de cliente inconsistente: SAP `00009887` / SF `9887` / ARP `9887` / Canais `00009887`
- Relacionamentos complexos entre objetos

### Processo
- Parecer construído manualmente em PowerPoint
- Workflow de tarefas gerido por uma única pessoa (100% manual)
- Sincronização manual de cotação obrigatória mesmo com apenas 1 cotação
- Múltiplas exceções e fluxos alternativos
- Aprovações excessivas sem informação adequada na tela

### Tecnologia
- 80,7% das execuções Apex acima de 5 segundos
- 142.607 governor limit errors em 30 dias
- 40 integrações sem HTTPS
- 83% das integrações síncronas sem retry
- Usuário de integração único (`0052E00000FtwuH`) gera 35% dos row locks

### Governança
- Pouca padronização
- Baixa observabilidade
- Ausência de métricas arquiteturais
- Sem CoE estruturado
- APIs não documentadas

---

## Hipóteses Validadas

1. Oportunidade e Cotação possuem sobreposição de responsabilidades — **CONFIRMADO** (campos que deveriam estar na Oportunidade estão na Cotação)
2. Acoplamento excessivo entre componentes — **CONFIRMADO** (monolito com cascateamento de DMLs)
3. Regras de negócio distribuídas em múltiplos pontos — **CONFIRMADO** (Triggers, Classes Apex, Flows, Integrações)
4. Modelo de dados com oportunidades de simplificação — **CONFIRMADO** (349 campos custom na Oportunidade, 31 Record Types)
5. Automação aumentando complexidade — **CONFIRMADO** (80,7% execuções acima de 5s)
6. Integrações dependentes da estrutura interna — **CONFIRMADO** (83% síncronas, acopladas)
7. Oportunidade de arquitetura modular — **CONFIRMADO** (recomendação de nova ORG)
