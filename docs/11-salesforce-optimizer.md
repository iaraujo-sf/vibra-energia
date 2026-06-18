# 11. Salesforce Optimizer Results (20/03/2025)

## Resumo

- **42 métricas avaliadas**
- 1 ação imediata necessária
- 5 ações necessárias
- 20 revisões necessárias
- 2 não disponíveis no momento
- 14 nenhuma ação necessária

---

## Ação Imediata Necessária

| Item | Tipo | Esforço | Volume | Detalhes |
|------|------|---------|--------|----------|
| Níveis de acesso externo padrão inseguros | Segurança | > 2h | 43 objetos | Objetos com Leitura/gravação pública externa |

---

## Ações Necessárias (> 2h)

| Item | Tipo | Volume | Detalhes |
|------|------|--------|----------|
| Migrar regras de fluxo de trabalho para Flow Builder | Fluxo de trabalho | 42 regras | — |
| Atualizações de versão pendentes | Segurança | 8 | — |

## Ações Necessárias (30-60 min)

| Item | Tipo | Volume | Detalhes |
|------|------|--------|----------|
| Tipos de registro por objeto | Layouts | 5 objetos | Opportunity (30), Plano de Contas B2B (20), Account (19), Case (18), Checklist (13) |
| Configurações de compartilhamento da comunidade não seguras | Segurança | 2 | Visibilidade do usuário do portal/site |
| Campos em layouts de página | Campos | 38 layouts | 49–131 campos por layout |

---

## Revisões Necessárias

| Item | Tipo | Esforço | Volume | Detalhes |
|------|------|---------|--------|----------|
| Vários Acionadores do Apex por objeto | Código | 1-2h | 1 | Recurso Financeiro da Oportunidade (2 triggers) |
| Versões da API | Código | 1-2h | 61 | Classes Apex com versão desatualizada |
| Adoção do Files | Uso | 1-2h | 7% | Uso nos últimos 30 dias |
| Usuários inativos do Chatter | Uso | 30-60 min | 41% | Não contribuição nos últimos 30 dias |
| Uso do campo | Campos | 30-60 min | 37 | Campos não usados |
| Listas relacionadas (Links rápidos) | Layouts | 30-60 min | 7 | Account (35), BlueSheet (24), Opportunity (24) |
| Lista relacionada de Notas e anexos | Código | 30-60 min | 61 | — |
| Limites da regra de validação ativa | Limites | 30-60 min | 11 | Account (76), Sit. Posto/Imóvel (70), Case (63), Opportunity (53) |
| Limites da regra de compartilhamento ativo | Limites | 30-60 min | 34 | Account |
| Layouts de página por objeto | Layouts | 30-60 min | 4 | Opportunity (29), Account (21), Plano de Contas B2B (20), Case (20) |
| Layouts de página não atribuídos | Layouts | 30-60 min | 30 | — |
| Converter anexo em arquivos | Código | 30-60 min | 5 | — |
| Componentes Lightning em páginas | Layouts | 30-60 min | 2 | Case (17), Contact (17) |
| Regras de validação inativas | Fluxo de trabalho | < 30 min | 212 | — |
| Regras de fluxo de trabalho inativas | Fluxo de trabalho | < 30 min | 74 | — |
| Papéis não atribuídos | Usuários | < 30 min | 107 | — |
| Guia Detalhes em páginas de registro | Campos | < 30 min | 15 | 52–131 campos |
| Desativar modo de depuração | Código | < 30 min | — | VERIFICAR |
| Conjuntos de permissões não atribuídos | Usuários | < 30 min | 104 | — |
| Conjuntos de permissões com baixo uso | Usuários | < 30 min | 86 | A partir de 1 usuário |

---

## Números-Chave de Governança

| Métrica | Criados | Não utilizados |
|---------|---------|---------------|
| Perfis | 82 | 26 |
| Permission Sets | 558 | 146 |
| Roles | 240 | 103 |
| Layouts de página não atribuídos | — | 30 |
| Regras de validação inativas | — | 212 |
| Workflow Rules inativas | — | 74 |
| Papéis não atribuídos | — | 107 |
