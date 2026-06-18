# 1. Objetivo e Contexto do Projeto

## Objetivo

Realizar um Assessment da solução de Negociação construída sobre Salesforce Sales Cloud, com o objetivo de identificar oportunidades de evolução arquitetural para tornar a plataforma:

- Mais escalável
- Mais manutenível
- Menos acoplada
- Mais aderente às boas práticas Salesforce
- Mais preparada para evolução funcional futura

## Contexto

A solução atual suporta processos críticos de negociação comercial da Vibra e sofreu diversas evoluções ao longo dos anos, incorporando customizações para atender demandas específicas do negócio.

Durante as análises iniciais foram identificados indícios de:

- Alto acoplamento entre componentes
- Complexidade excessiva de automações
- Dependências cruzadas entre objetos
- Duplicidade de responsabilidades funcionais
- Uso intensivo de lógica customizada
- Baixa rastreabilidade de determinadas regras de negócio
- Risco elevado para futuras evoluções

O Assessment tem como foco compreender o cenário atual (AS-IS), identificar riscos técnicos e funcionais e propor direcionamentos arquiteturais para uma futura modernização da solução.

## Visão Geral do Processo

A solução analisada suporta um dos processos mais críticos da Vibra:

- Construção de negociações comerciais
- Gestão de preços
- Aplicação de políticas comerciais
- Aprovação de condições especiais
- Geração de propostas
- Conversão em contratos e pedidos

O processo é utilizado por diferentes áreas da organização e representa um componente central da estratégia comercial da empresa.

## Motivadores do Assessment

### Evolução da Plataforma
Aumento progressivo da complexidade da solução ao longo dos anos.

### Sustentabilidade
Dificuldade crescente de manutenção e evolução.

### Escalabilidade
Necessidade de suportar novos produtos, regras comerciais e processos sem aumento proporcional da complexidade técnica.

### Governança
Necessidade de maior controle sobre:
- Mudanças
- Dependências
- Regras de negócio
- Arquitetura
