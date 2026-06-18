# 9. Integrações

## Visão Geral

- **153 integrações analisadas** no escopo do CRM
- **83% síncronas** (sem retry ou processamento assíncrono)
- **40 integrações sob HTTP** (sem HTTPS, sem autenticação adequada)
- **17.724 callouts com falha** nos últimos 30 dias

---

## Sistemas do Ecossistema

| Sistema | Função | Direção | Problemas Identificados |
|---------|--------|---------|------------------------|
| SAP | ERP — NEG, contratos, pedidos, preços, territórios | Bidirecional | Row locks; dados alterados via API SF com usuário genérico |
| ARP | Simulação de preços, TIR | SF → ARP | Credenciais na URL; orquestração síncrona bloqueante |
| MuleSoft | Camada de integração (ESB/iPaaS) | Hub | C4E score 1.80 (Baixa); falta governança |
| Netlex | Minutas de contrato, DDI | SF → Netlex | Consulta DDI para recursos > R$1M |
| DocuSign | Assinatura eletrônica | SF ↔ DocuSign | Processo manual (deveria ser nativo) |
| SGC | Gestão de consumo/contratos | Consulta | Consultado manualmente; dados desatualizados |
| Tableau | Analytics/BI | Consulta | Usado como fallback quando SGC desatualizado |
| ServiceNow | ITSM | SF → SN | Integração falha; dificulta acompanhamento |
| Azure | Reporte de erros, Data Lake | Vários | Usado para reportar erros de contratos |
| Canal de Negócios | Canal digital | Via MuleSoft | Cache atualizado apenas 2x/dia |
| App +Negócios | Mobile B2B | Via MuleSoft | — |
| App +Transportes | Mobile transporte | Via MuleSoft | — |
| Canal CTV | Canal de vendas | Via MuleSoft | — |
| Portal do Cliente | Canal digital B2B/B2C | Via MuleSoft | — |
| BRAX/Robbu | Robô de NEG em massa | Excel → SF | Workaround não-corporativo ativo |
| Simulador de DNN | Simulação interna | — | — |
| Mereu | Sistema legado | — | — |
| VMM | Sistema legado | — | — |
| PDI | Sistema legado | — | — |
| Cockpit (SAP) | Dashboard SAP | — | — |
| Interaxxa | Sistema legado | — | — |
| Neoway | Inteligência de mercado | — | — |
| Excel / Access | Planilhas paralelas | — | Usados para contornar limitações do CRM |

---

## Gaps Críticos (5 Pilares)

### 1. Segurança
- 40 integrações rodando sobre HTTP (sem HTTPS)
- Múltiplos métodos de autenticação inconsistentes: HTTP Basic, Custom Header com valor fixo, credenciais na URL
- Credenciais expostas em logs e URLs (ex: serviço de preço ARP)
- **Recomendação TO-BE:** Padronizar OAuth 2.0, eliminar Basic Auth e credenciais em URL

### 2. Confiabilidade
- 83% das integrações são síncronas (causa bloqueios)
- Falta de mecanismos de retry (ex: geração de cotação → perda de dados)
- Cache desatualizado: Canal de Negócios atualiza 2x/dia
- Timeouts frequentes: SF → SAP, SF → Legados
- **Recomendação TO-BE:** Padrão assíncrono, retry com backoff exponencial

### 3. Governança
- Múltiplos sistemas alterando dados usando **mesmo usuário** (`0052E00000FtwuH` — 35% dos row locks)
- APIs não documentadas (dependência de conhecimento informal)
- Duplicação de regras de negócio entre sistemas
- Código de cliente inconsistente: SAP `"00009887"` / SF `"9887"` / ARP `"9887"` / Canais `"00009887"`
- **Recomendação TO-BE:** Um usuário por sistema, documentação no API Portal, mapeamento canônico de IDs

### 4. Reusabilidade
- Diversidade tecnológica sem padronização: SOAP, REST, HTTP puro
- Lógica de integração embarcada nos sistemas (aumenta acoplamento)
- Falta de serviços reutilizáveis
- **Recomendação TO-BE:** Padronizar REST/JSON, criar mapeamento canônico, API-LED

### 5. Performance
- Orquestrações complexas (ex: integração TIR — síncrona e bloqueante)
- 83% síncronas = trava escalabilidade
- **Recomendação TO-BE:** Event-Driven Architecture, processamento assíncrono

---

## Indicadores de Falha (últimos 30 dias)

| Métrica | Quantidade |
|---------|-----------|
| Unsuccessful Integration Callouts | 17.724 |
| HTTP 500 (Internal Server Error) | 10.754 |
| HTTP 404 (Not Found) | 3.359 |
| HTTP 400 (Bad Request) | 1.088 |
| HTTP 502 (Bad Gateway) | 971 |
| HTTP 504 (Gateway Timeout) | 249 |

---

## Arquitetura de Integração TO-BE

### Padrão: API-LED Connectivity via MuleSoft

```
Experience Layer  →  Process Layer  →  System Layer
(Canais, Apps)       (Orquestração)     (SAP, ARP, Azure)
```

### Integrações TO-BE (Primeira Frente)

| Integração | Tipo | Descrição |
|------------|------|-----------|
| Gestão de Territórios | Consulta/Criação/Atualização | SAP → Salesforce ETM |
| Segmentação de Cliente | Consulta | Azure Data Lake + SAP → Data Cloud |
| Gestão de Contratos/Venda | Criação | SF → SAP |
| Gestão de Pedido e Preço | Simulação de preço | Azure / SAP |
| Nova Pricing | Consulta | Motor de preços centralizado |
| Consumo e Faturamento | Consulta | Sistemas externos → SF |
| Integrações SF Async | Redirecionamento | Resposta para nova Org |
| Visão 360 (Batch) | Job | Pedidos criados fora do SF |

### Padrões Disponíveis para Coexistência Multi-Org

| Padrão | Uso Recomendado |
|--------|-----------------|
| Data Cloud | **RECOMENDADO** — Sincronização de clientes entre orgs |
| MuleSoft | **RECOMENDADO** — Integrações transacionais |
| Salesforce Connect | Data Virtualization |
| Request & Reply | Nativos SF |
| Fire & Forget | Nativos SF |
| ETL | Cargas batch |

---

## Roadmap de Integração

1. Revisar recomendações PS do TDA (Technical Delivery Assessment)
2. Mapear pontos de dor SF vs. Assessment vs. Recomendações PS
3. Consolidar problemas como inputs para integrações MuleSoft
4. Definir Roadmap TO-BE de integração
5. Implementar observabilidade (atualmente inexistente)
6. Estruturar C4E (Center for Enablement) para MuleSoft
7. Migrar 153 integrações não-MuleSoft para a plataforma (nice to have)
