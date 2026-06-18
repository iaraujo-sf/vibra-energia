# 10. Análise Heurística (UX)

## Metodologia

- **10 Heurísticas de Nielsen** adaptadas ao contexto Salesforce
- Não cobre WCAG diretamente (requer análise separada)

## Escala de Severidade

| Score | Nível | Descrição |
|-------|-------|-----------|
| 5 | Catástrofe | Usuário não consegue completar tarefa; impede lançamento |
| 4 | Grave | Atrasa o usuário 1-5 min; pode causar catástrofes ocasionais |
| 3 | Moderado | Usuário consegue usar com esforço moderado |
| 2 | Menor | Baixa prioridade; efeito menor na usabilidade |
| 1 | Mínimo | Não é problema de usabilidade; pode ser estético |

## Distribuição dos Problemas

| Severidade | Quantidade |
|------------|-----------|
| Catástrofe (5) | 1 |
| Grave (4) | 6 |
| Moderado (3) | 7 |
| Menor (2) | 6 |
| Mínimo (1) | 0 |
| **TOTAL** | **20 problemas** |

---

## Problemas Identificados

### Score 5 — Catástrofe

**Heurística: Reconhecer, diagnosticar e recuperar-se de erros**
- Mensagens de erro não esclarecem qual aspecto está errado, o que precisa ser corrigido, nem sugerem curso de ação — usuário fica bloqueado no fluxo.

### Score 4 — Grave (6 problemas)

**Heurística: Prevenção de Erro**
- Criação de cotação: interface não auxilia usuários na compreensão dos parâmetros de data; campos com parâmetros desconhecidos geram erros.
- Personas afetadas: Executivo de Vendas

### Score 3 — Moderado (7 problemas)

**Heurística: Estética e Design Minimalista**
- Abas clicáveis e listas relacionadas empilhadas geram sobrecarga cognitiva; dificultam navegação.

**Heurística: Reconhecimento ao invés de Lembrança**
- Texto de botões dificulta reconhecimento da função; impacto maior em usuários inexperientes.

**Heurística: Flexibilidade e Eficiência de Uso**
- Preenchimento não automatizado conforme tipo de contrato selecionado — gera atrasos.
- Preenchimento de informações da cotação não automatizado conforme tipo de proposta.

**Heurística: Consistência e Padrões**
- Identificação do Cliente via Código SAP inconsistente com nome fantasia — gera confusão.
- Personas afetadas: Executivo de Vendas, Apoio a Vendas

### Score 2 — Menor (6 problemas)

**Heurística: Estética e Design Minimalista**
- Botões sem utilidade para todos os usuários geram sobrecarga.
- Campo "Área de Vendas" aparece em oportunidades de contratação onde não se aplica.
- Personas afetadas: Executivo de Vendas, Apoio a Vendas

---

## KPIs de UX Propostos

1. **Taxa de Satisfação do Usuário** — Reduzir tempo médio de realização de tarefas
2. **Satisfação do Cliente (CSAT)** — Melhorias de navegação liberam tempo do EV para tarefas estratégicas
3. **Índice de Erros de Navegação** — Diminuir taxa de erros e aumentar eficiência
4. **Taxa de Erros de Entrada de Dados** — Reduzir com automatizações e mensagens mais claras

---

## Recomendações UX TO-BE

- Aplicar Dynamic Forms para segregar campos por perfil
- OmniStudio para reduzir jornada de cotação (16 abas → fluxo guiado)
- Mensagens de erro claras e acionáveis
- Mobile-First para aprovações e consultas
- Cockpit gerencial com KPIs táticos e preditivos
- Notificações push relevantes para aprovações
- Console App para reduzir abas abertas
- Tela de início customizada por segmento (B2C/B2B)
