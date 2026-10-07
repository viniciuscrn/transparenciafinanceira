# Ordem Cronológica de Pagamentos

## Visão Geral

Lista os pagamentos efetuados (um registro por item de pagamento), com data de liquidação, data de pagamento, beneficiário, unidade orçamentária, origem do recurso e elemento de despesa.

```http
GET /api/empenhos/ordem-cronologica-pagamentos
```

> Endpoint **público** (não exige token).

---

## Query Params

| Parâmetro             | Tipo                | Descrição                                              |
| --------------------- | ------------------- | ------------------------------------------------------ |
| `ano`                 | integer (2000–2100) | Ano do empenho                                         |
| `mes`                 | integer (1–12)      | Mês do empenho                                         |
| `data_liquidacao_ini` | date (`YYYY-MM-DD`) | Data de liquidação inicial                             |
| `data_liquidacao_fim` | date (`YYYY-MM-DD`) | Data de liquidação final                               |
| `elemento_despesa`    | string (CSV)        | Um ou mais elementos de despesa (ex.: `30,39`)         |
| `per_page`            | integer (1–200)     | Itens por página. Padrão `20`                          |
| `export`              | boolean             | Retorna todos os registros, sem paginação              |

## Ordenação

1. `data_pagamento` (mais recente primeiro)
2. `data_liquidacao`
3. `numero_empenho`
4. `parcela`

## Exemplos

```http
GET /api/empenhos/ordem-cronologica-pagamentos?ano=2025&mes=3
GET /api/empenhos/ordem-cronologica-pagamentos?data_liquidacao_ini=2025-01-01&data_liquidacao_fim=2025-03-31
GET /api/empenhos/ordem-cronologica-pagamentos?elemento_despesa=30,39&export=1
```

---

## Resposta (paginada)

Paginação padrão do Laravel:

```json
{
  "current_page": 1,
  "data": [
    {
      "ano": 2025,
      "data_liquidacao": "2025-02-25",
      "data_pagamento": "2025-02-28",
      "beneficiario": "EMPRESA EXEMPLO LTDA",
      "numero_empenho": "0000049",
      "parcela": "001",
      "valor_pago": "180.00",
      "unidade_orcamentaria": "CÂMARA MUNICIPAL",
      "origem_recurso": "Recursos não Vinculados de Impostos",
      "elemento_despesa": {
        "categoria": { "codigo": "3", "descricao": "Despesas Correntes" },
        "grupo": { "codigo": "3", "descricao": "Outras Despesas Correntes" },
        "modalidade": { "codigo": "90", "descricao": "Aplicações Diretas" },
        "elemento": { "codigo": "39", "descricao": "Outros Serviços de Terceiros - Pessoa Jurídica" },
        "subelemento": { "codigo": "999", "descricao": "..." }
      }
    }
  ],
  "per_page": 20,
  "total": 120
}
```

## Resposta com `export=1`

```json
{
  "total": 120,
  "data": [ { "...": "mesmo formato de cada item" } ]
}
```

## Campos

| Campo                  | Descrição                                                        |
| ---------------------- | ---------------------------------------------------------------- |
| `ano`                  | Ano do empenho                                                   |
| `data_liquidacao`      | Data da liquidação do empenho (pode ser `null`)                  |
| `data_pagamento`       | Data do pagamento                                                |
| `beneficiario`         | Nome do fornecedor, ou `"Não identificado"`                      |
| `numero_empenho`       | Número do empenho                                                |
| `parcela`              | Número sequencial do item de pagamento                           |
| `valor_pago`           | Valor do item de pagamento (string decimal)                      |
| `unidade_orcamentaria` | Denominação da unidade orçamentária                              |
| `origem_recurso`       | Descrição da fonte de recurso                                    |
| `elemento_despesa`     | Natureza da despesa detalhada (categoria, grupo, modalidade, elemento, subelemento) |

## Observações

- Quando um empenho tem mais de uma liquidação na mesma remessa, o item de pagamento pode aparecer uma vez por liquidação.
- Para empenhos reenviados em várias remessas, é usado o registro mais recente do empenho.
