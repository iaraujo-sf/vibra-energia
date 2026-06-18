# 12. Storytelling Técnico / BSA

## Narrativa Central

> "A complexidade atual gera perda de agilidade comercial e dificuldade de escala."
> "O sistema atua mais como registrador do que como motor de valor e estratégia."
> "A ausência de arquitetura modular impede ganhos incrementais de eficiência."

---

## Exemplo 1 — "Cada exceção se tornou uma nova regra"

### Problema AS-IS (Técnico)
- Lógica de negócio embutida em flows e triggers
- Ausência de fallback processual para exceções legítimas
- BRAX (robô) resolve o que a plataforma não suporta
- Acoplamento forte com integrações síncronas (ex: SAP, ARP)

### Solução TO-BE
- Modelo baseado em regras de negócio parametrizáveis (rule-driven logic)
- Separação de fluxo regular x exceções, com workflows desacoplados
- Desacoplamento transacional via eventos ou filas para orquestração de exceções
- Domínio técnico para simulação e pré-negociação, com APIs e persistência própria

---

## Exemplo 2 — "Recalcular tudo para confiar em algo"

### Problema AS-IS (Técnico)
- Lógica de simulação (TIR, margem, preço alvo) de difícil rastreabilidade, executada externamente
- Ausência de evidência clara da simulação leva times a recalcular manualmente
- Resultado da simulação não é versionado nem auditável
- Input manual de valores sem referência técnica compromete integridade

### Solução TO-BE
- Lógica de simulação concentrada em serviço externo centralizado (Vibra), integrado via MuleSoft
- Salesforce como consumidor da simulação (não recalcula), armazenando resultado com versionamento
- Persistência junto à proposta — áreas de validação consultam parâmetros originais sem nova execução
- Histórico de simulações com carimbo de data, hora e responsável
- Cache inteligente para evitar chamadas redundantes

**Resumo:** Separa inteligência de cálculo (SAP/Vibra) da jornada operacional (Salesforce).

---

## Exemplo 3 — "Aprovar no escuro"

### Problema AS-IS (Técnico)
- Falta de tela responsiva consolidando dados-chave
- Ausência de contexto analítico por proposta
- Necessidade de acessar múltiplas entidades e sistemas
- Processo de aprovação fragmentado, sem interface de apoio à decisão

### Solução TO-BE
- Interface unificada de aprovação com componentes de contexto (responsiva mobile)
- Componente técnico de visualização contextual (resumo da proposta + histórico)
- Orquestração assíncrona de etapas de aprovação (múltiplas alçadas paralelas)
- Eventos para atualização em tempo real (status e riscos da proposta)

---

## Proposição Estratégica: Dois Ciclos de Aplicação

### Diagnóstico
O problema não é apenas técnico (monolito de objetos acoplados). É **conceitual e arquitetônico**: a Vibra tenta operar negociação e gestão de contrato dentro da mesma aplicação.

### Consequências
- Sobrecarga ao EV (campos de operação de longo prazo no momento da venda)
- Rigidez na negociação (validações que não fazem sentido no momento comercial)
- Dificuldade de evolução (mudanças em contrato impactam ciclo de vendas)
- Baixa governança (aprovado ≠ executado, sem controle formal)

### Solução
Separar tecnicamente dois ciclos de vida:

**Aplicação de Proposta** (baseada em Opportunity, Proposta__c, Simulação__c, Aprovacao__c, Contract)

**Aplicação de Gestão de Contrato** (ativada após assinatura, com Reajuste__c, Entrega__c, Aditivo__c)

Transição via eventos controlados. Cada ciclo com seu modelo de permissão, tela, automação e status.

---

## Cenários de Evolução

### Cenário 1 – Refatoração na Org Atual
- **+**: reaproveitamento de ativos, menor esforço inicial
- **−**: maior risco técnico, +35% esforço/história, ciclos de 16–20 semanas/release

### Cenário 2 – Solução Modular com Arquitetura Escalável
- **+**: domínio desacoplado, ganhos de performance, governança e UX nativa
- **−**: requer redesenho funcional e técnico orientado a capacidades

### Cenário 3 – Híbrido Temporário com Componentes de Transição
- **+**: menor ruptura, teste controlado de novos domínios
- **−**: risco de prolongar convivência com erros estruturais

---

## Requisitos Essenciais para Evoluir

1. Jornada digital padronizada com governança de dados
2. Orquestração de integrações com resiliência e fallback
3. Fluxos de aprovação nativamente rastreáveis e auditáveis
4. Hierarquias comerciais consistentes e reaproveitáveis
5. Visão analítica e cockpit tático para gestores

---

## Encaminhamento

> "Recomenda-se iniciar por domínios com maior impacto e visibilidade para o negócio, com foco em modularização e independência evolutiva. O desenho técnico deve seguir princípios de desacoplamento, reutilização, observabilidade e governança."
