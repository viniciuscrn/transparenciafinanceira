# Liquidações e Pagamentos

## Visão Geral

Consulta direta das tabelas de liquidações e pagamentos importadas das remessas SAGRES. Para a visão consolidada por empenho, use [`GET /api/empenhos`](./empenho.md).

> Endpoints **protegidos** (exigem `Authorization: Bearer <TOKEN>`).

| Método | Endpoint                  | Descrição                     |
| ------ | ------------------------- | ----------------------------- |
| GET    | `/api/liquidacoes`        | Lista liquidações (paginada)  |
| GET    | `/api/liquidacoes/{id}`   | Detalha uma liquidação        |
| GET    | `/api/pagamentos`         | Lista pagamentos (paginada)   |
| GET    | `/api/pagamentos/{id}`    | Detalha um pagamento          |

---

# 1. Liquidações

```http
GET /api/liquidacoes
```

Ordenação: `data_liquidacao` (mais recente primeiro).

| Parâmetro           | Tipo                | Descrição                          |
| ------------------- | ------------------- | ---------------------------------- |
| `competencia`       | string (`YYYY-MM`)  | Competência                        |
| `remessa_id`        | integer             | ID da remessa                      |
| `unidade_codigo`    | string              | Código da unidade orçamentária     |
| `numero_empenho`    | string              | Número do empenho                  |
| `numero_liquidacao` | string              | Número da liquidação               |
| `data_ini`          | date (`YYYY-MM-DD`) | `data_liquidacao` inicial          |
| `data_fim`          | date (`YYYY-MM-DD`) | `data_liquidacao` final            |
| `per_page`          | integer (1–200)     | Itens por página. Padrão `20`      |

```http
GET /api/liquidacoes?competencia=2025-02&numero_empenho=0000049
```

### Resposta

Paginação padrão do Laravel, com os registros da tabela `liquidacoes`:

```json
{
  "current_page": 1,
  "data": [
    {
      "id": 10,
      "remessa_id": 8,
      "ano": 2025,
      "mes": 2,
      "competencia": "2025-02",
      "unidade_codigo": "0000008001",
      "numero_empenho": "0000049",
      "empenho_key": "2025|0000008001|0000049",
      "numero_liquidacao": "0000001",
      "liquidacao_key": "2025|0000008001|0000049|0000001",
      "data_liquidacao": "2025-02-25",
      "valor_liquidado": "180.00",
      "tipo_documento": "2",
      "chave_nfe": null,
      "descricao": "REFERENTE A SERVIÇOS...",
      "documento": "...",
      "cpf_cnpj_credor": "40628309000145",
      "fonte_recurso": "15000000",
      "created_at": "...",
      "updated_at": "..."
    }
  ],
  "per_page": 20,
  "total": 1
}
```

`GET /api/liquidacoes/{id}` retorna um único registro nesse formato, ou `404`.

---

# 2. Pagamentos

```http
GET /api/pagamentos
```

Ordenação: `data_pagamento` (mais recente primeiro).

| Parâmetro           | Tipo                | Descrição                          |
| ------------------- | ------------------- | ---------------------------------- |
| `competencia`       | string (`YYYY-MM`)  | Competência                        |
| `remessa_id`        | integer             | ID da remessa                      |
| `unidade_codigo`    | string              | Código da unidade orçamentária     |
| `numero_empenho`    | string              | Número do empenho                  |
| `numero_pagamento`  | string              | Número do pagamento                |
| `data_ini`          | date (`YYYY-MM-DD`) | `data_pagamento` inicial           |
| `data_fim`          | date (`YYYY-MM-DD`) | `data_pagamento` final             |
| `per_page`          | integer (1–200)     | Itens por página. Padrão `20`      |

```http
GET /api/pagamentos?competencia=2025-02
```

### Resposta

Paginação padrão do Laravel, com os registros da tabela `pagamentos`:

```json
{
  "current_page": 1,
  "data": [
    {
      "id": 3,
      "remessa_id": 8,
      "ano": 2025,
      "mes": 2,
      "competencia": "2025-02",
      "unidade_codigo": "0000008001",
      "numero_empenho": "0000049",
      "empenho_key": "2025|0000008001|0000049",
      "numero_pagamento": "0000001",
      "pagamento_key": "2025|0000008001|0000049|0000001",
      "data_pagamento": "2025-02-28",
      "created_at": "...",
      "updated_at": "..."
    }
  ],
  "per_page": 20,
  "total": 1
}
```

`GET /api/pagamentos/{id}` retorna um único registro, ou `404`.

> O registro de pagamento não traz valor: o valor pago fica nos itens de pagamento (`itens_pagamento`), retornados em `GET /api/empenhos/{id}`.
