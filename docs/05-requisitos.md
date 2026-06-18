# 5. Requisitos e Regras de Negócio

## Capacidades de Negócio — Maturidade

- **18 Capacidades Totais Requeridas**
- 66% (12 capacidades): Vibra já atende
- 34% (6 capacidades): Vibra não faz hoje dentro do Sales Cloud

Breakdown por nível:
- 6% Maturidade Alta
- 50% Maturidade Média
- 11% Maturidade Baixa
- 33% Não faz hoje

---

## Capacidades Críticas

### Gestão de Oportunidades
Permitir criação, evolução, qualificação e conversão de oportunidades comerciais.

**Tipos de Oportunidade:**
1. Oportunidade de Contratação (B2B e B2C)
2. Oportunidade de Vendas — NEG (B2B e B2C)
3. Oportunidade de Vendas — Pedido Spot (B2B e B2C)
4. Oportunidade de Vendas — Pedido Direto (B2B e B2C)

### Processo de Negociação
Controlar condições comerciais, descontos, aprovações e exceções.

### Gestão de Cotações
Suportar criação de propostas, simulações, versionamento e aprovação.

### Pricing
Garantir aplicação correta das políticas comerciais e consistência entre negociação e precificação.

### Governança Comercial
Permitir auditoria, rastreabilidade, histórico de alterações e compliance de aprovações.

---

## Processo Detalhado — Oportunidade de Contratação

### Etapa 1: Criação de Oportunidade
- Seleciona tipo de registro
- Identifica cliente e área de vendas
- Informa situação atual Posto/Imóvel
- Cria Nota PM (Engenharia)
- Revisa/cria Recursos Financeiros
- Vinculação manual das franquias existentes
- Preenchimento de receitas e despesas, garantias, contratos vigentes, área de influência

### Etapa 2: Criação de Cotação
- Seleção e confirmação de produtos existentes
- Alteração de parâmetros
- Simulação (integração com ARP)
- Preenchimento de informações da cotação
- Atualização de informações de contrato

### Etapa 3: Simulação / Cálculo TIR
- Valida informações imputadas e simulação
- Integra com ARP
- Calcula ajuste financeiro
- Recalcula TIR

### Etapa 4: Envio para Aprovação
- Sincronização de cotação (manual)
- Envio para aprovação

### Etapa 5: Análise de Oportunidade (Apoio a Vendas)
- Verifica saldo de consumo no SGC
- Verifica ajuste financeiro de volume devedor
- Se SGC desatualizado, consulta Tableau
- Recalcula saldo devedor × Margem média manualmente
- Verifica notas da oportunidade e recursos financeiros
- Verifica lançamentos de Engenharia (Nota PM + Recursos Financeiros)
- Abre ticket de Consulta DDI no Netlex (recursos financeiros > R$1M)
- Verifica Garantias e processo de Hipoteca

### Etapa 6: Geração de Parecer (Manual)
- Coleta informações da oportunidade no Salesforce
- Coleta dados da Inteligência de mercado (planilha externa)
- Coleta dados no SGC da negociação anterior
- Inclui dados da Consulta DDI
- Baixa fotos anexas da oportunidade
- **Constrói PPT manualmente** com o parecer

### Etapa 7: Workflow de Tarefas (100% Manual)
- Gestor de Workflow cria tarefa e direciona para o analista
- Cria tarefa para EV anexar documentos de contrato
- Verifica se tarefa do EV está concluída
- Cria tarefa para equipe SNEG fazer minuta
- **Todo o workflow gerido manualmente por uma única pessoa**

### Etapa 8: Emissão de Contrato
- Emissão de minutas no Netlex
- Download de todas as minutas
- Montagem de envelope único no DocuSign (manual)
- Aguarda retorno de contratos assinados
- Download das minutas assinadas
- Anexo manual no Salesforce
- Cadastra o contrato no Salesforce manualmente

### Etapa 9: Ativação de Contrato
- Clica em "Ativa Contrato"
- Aguarda retorno da integração com SAP
- Trata erros manualmente
- Muda status do Contrato para Ativo manualmente
- Muda status da Oportunidade para Concluída manualmente
- Equipe TI intervém em erros via Azure

---

## Atores / Personas

| Persona | Responsabilidades |
|---------|-------------------|
| Executivo de Vendas (EV) B2B/B2C | Relacionamento com clientes, simulação de preços, cálculo de TIR, criação de oportunidades, cadastro de clientes, pedidos |
| Apoio a Vendas (SNEG) | Conferência e análise, criação de contratos, workflow de tarefas, ponte entre negócios e TI |
| Gestão de Contratos (SNEG/SPCOM) | Análise e ativação de contratos, gestão de consumo e bonificações, limites de crédito/débito |
| Gestão de Vendas | Validação e aprovação de propostas, acompanhamento de metas, análise de indicadores |
| Gestor de Workflow | Criação e distribuição manual de todas as tarefas — gargalo crítico |
| Lubrax+ / BR Mania | Franquias B2C com processos desconectados; Lubrax+ usa SF apenas para aprovações |

---

## Requisitos Não Funcionais

| Requisito | Descrição | Benchmark |
|-----------|-----------|-----------|
| Performance | EPT (Experience Page Time) | < 3 segundos |
| Customização | Índice máximo de customização | < 40% da plataforma |
| Escalabilidade | Crescimento sem degradação | Suportar novos produtos/regras |
| Manutenibilidade | Facilidade de evolução | Releases de 8–12 semanas (vs. 16–20 atual) |
| Observabilidade | Monitoramento | Logs, alertas, métricas |
| Confiabilidade | Menor dependência de componentes únicos | Retry, fallback, assíncrono |

---

## Proposição Estratégica: Dois Ciclos de Aplicação

### Problema Conceitual
A Vibra hoje tenta operar a **negociação** e a **gestão do contrato** dentro da mesma aplicação, com os mesmos objetos, layouts, automações e perfis.

### Solução: Separar Dois Ciclos de Vida

**Aplicação de Proposta:**
- Objetos: Opportunity, Proposta__c, Simulação__c, Aprovacao__c, Contract
- Foco: fechamento comercial
- Persona principal: EV

**Aplicação de Gestão de Contrato:**
- Ativada somente após assinatura
- Objetos: Reajuste__c, Entrega__c, Aditivo__c, etc.
- Foco: operação contratual
- Persona principal: Gestão de Contratos

**Transição entre ciclos:** via eventos controlados, garantindo rastreabilidade do que foi aprovado vs. executado.

### Benefícios
- Desburocratiza o processo de vendas
- Governança real sobre contrato em produção
- Evita retrabalho por campos obrigatórios fora de contexto
- Squads evoluem cada ciclo separadamente
