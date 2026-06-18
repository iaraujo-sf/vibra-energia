# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Sobre o Projeto

Assessment da solução de Negociação Comercial da Vibra Energia, construída sobre Salesforce Sales Cloud. O projeto documentou o estado atual (AS-IS), identificou 8 Big Rocks arquiteturais e definiu direcionamento para uma nova Org Salesforce (Greenfield) com arquitetura modular.

## Estrutura

- `docs/` — Documentação completa do assessment (01 a 12), numerada sequencialmente
- `raw/` — Arquivos originais do assessment (PPTX, XLSX, DOCX)
- `raw_text/` — Texto extraído dos arquivos originais para consulta

## Contexto de Negócio

A solução suporta: negociação comercial, gestão de preços, aprovações, propostas e contratos. O processo é crítico e utilizado por diversas áreas da Vibra (EVs B2B/B2C, Apoio a Vendas, Gestão de Contratos, Gestão de Vendas).

## Decisões Arquiteturais Principais

- **Plataforma:** Salesforce Sales Cloud (Nova Org — Greenfield)
- **Decisão ARB:** Single Org Nova (Cenário 2) — não refatorar a org atual
- **Estado transitório:** Multi-Org (Nova Sales + Atual Service) via Data Cloud + MuleSoft
- **Visão de futuro:** Single Org unificada (Sales + Service)

### Princípios
- Low Code First, Event Driven, API First, Separation of Concerns, DDD conceitual
- Customização < 40% da plataforma
- EPT < 3 segundos

### Domínios TO-BE
- Negociação (Sales Cloud + OmniStudio)
- Pricing (Revenue Cloud CPQ)
- Aprovação (OmniStudio + BRE + Advanced Approvals)
- Contratação (Revenue Cloud CLM + DocuSign)
- Territórios (Enterprise Territory Management)
- Segmentação (Data Cloud)
- Pedidos (Revenue Cloud Order)
- Consumo/Faturamento (Revenue Cloud Billing)

### Habilitadores
- Revenue Cloud (CLM/CPQ/Billing)
- OmniStudio (guided flows)
- Data Cloud (CDP, visão 360, sync multi-org)
- MuleSoft API-LED (elevar C4E de 1.80 para 4.0)
- Einstein AI (scoring, parecer automático)

## Métricas-Chave do AS-IS

- 142.607 governor limit errors/mês
- 80,7% execuções Apex > 5 segundos
- 17.724 callouts com falha/mês
- 993 row locks/mês
- Health Check: 36,4% vermelho
- Releases: 16–20 semanas (+35% custo/ponto funcionalidade)

## Idioma

Toda documentação e comunicação sobre este projeto é em **português brasileiro**.
