# Empenhos — Documentação da API para Consumo do Front-end

## Visão Geral

O `EmpenhoController` disponibiliza endpoints de consulta de empenhos com dados agregados de:

- liquidações
- pagamentos
- itens de pagamento
- resumo financeiro da despesa

Além disso, oferece um endpoint para consulta de fornecedor no TCE a partir de CPF ou CNPJ.

Base URL de exemplo:

```txt
/api/empenhos
```

---

## Endpoints Disponíveis

| Método     | Endpoint                              | Descrição                                                     |
| ---------- | ------------------------------------- | ------------------------------------------------------------- |
| GET        | `/api/empenhos`                       | Lista empenhos com filtros, resumos, liquidações e pagamentos |
| GET        | `/api/empenhos/{id}`                  | Retorna o detalhe completo de um empenho                      |
| GET/POST\* | `/api/empenhos/buscar-fornecedor-tce` | Consulta fornecedor externo por CPF/CNPJ                      |

> \*O método do endpoint `buscarFornecedorTce` depende da rota cadastrada no projeto. Pelo controller, ele recebe `Request`, então normalmente é exposto como `GET` ou `POST`.

---

# 1. Listar Empenhos

## Endpoint

```http
GET /api/empenhos
```

## Descrição

Retorna uma lista paginada de empenhos com:

- dados principais do empenho
- resumo da liquidação
- resumo do pagamento
- lista de liquidações relacionadas
- lista de pagamentos relacionadas

---

## Query Params Aceitos

| Parâmetro        | Tipo    | Obrigatório | Descrição                                                |
| ---------------- | ------- | ----------: | -------------------------------------------------------- |
| `competencia`    | string  |         não | Competência no formato `YYYY-MM`                         |
| `ano`            | integer |         não | Ano da remessa/empenho                                   |
| `mes`            | integer |         não | Mês da remessa/empenho                                   |
| `remessa_id`     | integer |         não | ID da remessa                                            |
| `unidade_codigo` | string  |         não | Código da unidade orçamentária                           |
| `numero_empenho` | string  |         não | Número do empenho                                        |
| `cpf_cnpj`       | string  |         não | CPF ou CNPJ do credor, com ou sem máscara                |
| `data_ini`       | string  |         não | Data inicial no formato `YYYY-MM-DD`                     |
| `data_fim`       | string  |         não | Data final no formato `YYYY-MM-DD`                       |
| `min_valor`      | number  |         não | Valor mínimo do empenho                                  |
| `max_valor`      | number  |         não | Valor máximo do empenho                                  |
| `q`              | string  |         não | Busca textual por número, descrição, unidade ou CPF/CNPJ |
| `per_page`       | integer |         não | Quantidade por página. Padrão: `20`, máximo: `200`       |

---

## Regras dos filtros

### `cpf_cnpj`

- remove automaticamente qualquer caractere não numérico
- aceita:
  - CPF com 11 dígitos
  - CNPJ com 14 dígitos

Exemplos válidos:

- `12345678900`
- `123.456.789-00`
- `12345678000199`
- `12.345.678/0001-99`

### `q`

Busca textual simples nos campos:

- `numero_empenho`
- `descricao`
- `unidade_codigo`
- `cpf_cnpj_credor`

Quando `q` contiver um CPF/CNPJ válido, a busca usa a versão sem máscara.

---

## Exemplos de uso

### Filtrar por competência

```http
GET /api/empenhos?competencia=2025-02
```

### Filtrar por ano e mês

```http
GET /api/empenhos?ano=2025&mes=2
```

### Filtrar por CPF/CNPJ

```http
GET /api/empenhos?cpf_cnpj=40.628.309/0001-45
```

### Filtrar por número do empenho

```http
GET /api/empenhos?numero_empenho=0000049
```

### Busca textual

```http
GET /api/empenhos?q=carimbos
```

### Paginação

```http
GET /api/empenhos?competencia=2025-02&per_page=50
```

---

## Resposta de sucesso

### Status

```http
200 OK
```

### Exemplo

```json
{
  "current_page": 1,
  "data": [
    {
      "id": 88,
      "remessa_id": 8,
      "ano": 2025,
      "mes": 2,
      "competencia": "2025-02",
      "unidade_codigo": "0000008001",
      "numero_empenho": "0000049",
      "empenho_key": "2025|0000008001|0000049",
      "data_empenho": "2025-02-24",
      "valor_empenhado": "180.00",
      "cpf_cnpj_credor": "40628309000145",
      "descricao": "REFERENTE A SERVIÇOS DE CONFECÇÃO...",
      "natureza_despesa": "3.3.90.39.999",
      "fonte_recurso": "15000000",
      "programa_codigo": "7001",
      "acao_codigo": "000017",
      "created_at": "2026-01-21T19:44:44.000000Z",
      "updated_at": "2026-01-21T19:44:44.000000Z",

      "resumo_liquidacao": {
        "quantidade": 1,
        "total_liquidado": "180.00",
        "saldo_a_liquidar": "0.00"
      },

      "resumo_pagamento": {
        "quantidade_pagamentos": 1,
        "quantidade_itens": 1,
        "total_pago": "180.00",
        "saldo_a_pagar": "0.00"
      },

      "liquidacoes": [
        {
          "id": 10,
          "remessa_id": 8,
          "ano": 2025,
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
          "fonte_recurso": "15000000",
          "created_at": "2026-01-21T20:10:00.000000Z",
          "updated_at": "2026-01-21T20:10:00.000000Z"
        }
      ],

      "pagamentos": [
        {
          "id": 3,
          "remessa_id": 8,
          "ano": 2025,
          "competencia": "2025-02",
          "unidade_codigo": "0000008001",
          "numero_empenho": "0000049",
          "empenho_key": "2025|0000008001|0000049",
          "numero_pagamento": "0000001",
          "numero_parcela": "0000001",
          "pagamento_key": "2025|0000008001|0000049|0000001",
          "data_pagamento": "2025-02-28",
          "created_at": "2026-01-21T20:15:00.000000Z",
          "updated_at": "2026-01-21T20:15:00.000000Z"
        }
      ]
    }
  ],
  "per_page": 20,
  "total": 1
}
```

---

## Campos retornados na listagem

### Dados do empenho

| Campo              | Tipo        | Descrição                      |
| ------------------ | ----------- | ------------------------------ |
| `id`               | integer     | ID interno do empenho          |
| `remessa_id`       | integer     | ID da remessa de origem        |
| `ano`              | integer     | Ano do empenho/remessa         |
| `mes`              | integer     | Mês do empenho/remessa         |
| `competencia`      | string      | Competência `YYYY-MM`          |
| `unidade_codigo`   | string      | Código da unidade orçamentária |
| `numero_empenho`   | string      | Número do empenho              |
| `empenho_key`      | string      | Chave técnica do empenho       |
| `data_empenho`     | string/null | Data do empenho                |
| `valor_empenhado`  | string      | Valor empenhado com 2 casas    |
| `cpf_cnpj_credor`  | string/null | CPF ou CNPJ do credor          |
| `descricao`        | string/null | Histórico do empenho           |
| `natureza_despesa` | string/null | Natureza da despesa formatada  |
| `fonte_recurso`    | string/null | Fonte de recurso               |
| `programa_codigo`  | string/null | Código do programa             |
| `acao_codigo`      | string/null | Código da ação                 |

### Resumo da liquidação

| Campo              | Tipo    | Descrição                         |
| ------------------ | ------- | --------------------------------- |
| `quantidade`       | integer | Quantidade de liquidações         |
| `total_liquidado`  | string  | Soma dos valores liquidados       |
| `saldo_a_liquidar` | string  | Valor empenhado - valor liquidado |

### Resumo do pagamento

| Campo                   | Tipo    | Descrição                                 |
| ----------------------- | ------- | ----------------------------------------- |
| `quantidade_pagamentos` | integer | Quantidade de registros em pagamentos     |
| `quantidade_itens`      | integer | Quantidade de itens de pagamento          |
| `total_pago`            | string  | Soma de `itens_pagamento.valor_pagamento` |
| `saldo_a_pagar`         | string  | Valor liquidado - valor pago              |

---

## Erros de validação

### Status

```http
422 Unprocessable Entity
```

### Exemplo

```json
{
  "message": "The competencia field format is invalid.",
  "errors": {
    "competencia": ["The competencia field format is invalid."]
  }
}
```

### Casos comuns

- `competencia` fora do formato `YYYY-MM`
- `ano` fora do intervalo permitido
- `mes` fora do intervalo `1..12`
- `data_ini` ou `data_fim` inválidas
- `per_page` menor que 1 ou maior que 200

---

# 2. Detalhar Empenho

## Endpoint

```http
GET /api/empenhos/{id}
```

## Descrição

Retorna os dados completos de um empenho específico, incluindo:

- resumo da liquidação
- resumo do pagamento
- lista de liquidações
- lista de pagamentos
- lista de itens de pagamento

---

## Exemplo de requisição

```http
GET /api/empenhos/88
```

---

## Resposta de sucesso

### Status

```http
200 OK
```

### Exemplo

```json
{
  "id": 88,
  "remessa_id": 8,
  "ano": 2025,
  "mes": 2,
  "competencia": "2025-02",
  "unidade_codigo": "0000008001",
  "numero_empenho": "0000049",
  "empenho_key": "2025|0000008001|0000049",
  "data_empenho": "2025-02-24",
  "valor_empenhado": "180.00",
  "cpf_cnpj_credor": "40628309000145",
  "descricao": "REFERENTE A SERVIÇOS DE CONFECÇÃO...",
  "natureza_despesa": "3.3.90.39.999",
  "fonte_recurso": "15000000",
  "programa_codigo": "7001",
  "acao_codigo": "000017",

  "resumo_liquidacao": {
    "quantidade": 1,
    "total_liquidado": "180.00",
    "saldo_a_liquidar": "0.00"
  },

  "resumo_pagamento": {
    "quantidade_pagamentos": 1,
    "quantidade_itens": 1,
    "total_pago": "180.00",
    "saldo_a_pagar": "0.00"
  },

  "liquidacoes": [],
  "pagamentos": [],
  "itens_pagamento": []
}
```

---

## Estrutura adicional do `show()`

### `liquidacoes`

Lista de liquidações relacionadas ao empenho, contendo:

- `ano`
- `numero_liquidacao`
- `data_liquidacao`
- `valor_liquidado`
- `tipo_documento`
- `chave_nfe`
- `descricao`
- `fonte_recurso`

### `pagamentos`

Lista de pagamentos relacionados ao empenho, contendo:

- `ano`
- `numero_empenho`
- `numero_pagamento`
- `numero_parcela`
- `data_pagamento`

### `itens_pagamento`

Lista de itens financeiros do pagamento, contendo:

- `numero_sequencial`
- `valor_pagamento`
- `conta_debito`
- `numero_cheque`
- `numero_doc_debito`
- `banco_credito`
- `agencia_credito`
- `conta_credito`
- `fonte_recurso`
- `tipo_conta_debito`
- `tipo_pagamento`
- `chave_pix`

---

## Resposta quando não encontrado

```http
404 Not Found
```

---

# 3. Buscar fornecedor no TCE

## Endpoint

```http
GET /api/empenhos/buscar-fornecedor-tce?cpf_cnpj=40628309000145
```

> A rota exata depende do arquivo `routes/api.php`. Este exemplo assume exposição via GET.

## Descrição

Consulta um fornecedor em serviço externo do TCE usando CPF ou CNPJ.

---

## Query params

| Parâmetro  | Tipo   | Obrigatório | Descrição                      |
| ---------- | ------ | ----------: | ------------------------------ |
| `cpf_cnpj` | string |         sim | CPF ou CNPJ com ou sem máscara |

---

## Regras

- remove máscara automaticamente
- aceita:
  - CPF com 11 dígitos
  - CNPJ com 14 dígitos

---

## Resposta de sucesso

Retorna o JSON bruto da API externa do TCE.

### Status

```http
200 OK
```

### Exemplo

```json
{
  "documento": "40628309000145",
  "nome": "EMPRESA EXEMPLO LTDA",
  "situacao": "ATIVO"
}
```

---

## Erros possíveis

### CPF/CNPJ inválido

```http
422 Unprocessable Entity
```

```json
{
  "error": "CPF ou CNPJ inválido"
}
```

### Falha na API externa

```http
500 Internal Server Error
```

```json
{
  "error": "Erro ao consultar fornecedor no TCE"
}
```

---

# Observações importantes para o front-end

- `valor_empenhado`, `total_liquidado`, `total_pago`, `saldo_a_liquidar` e `saldo_a_pagar` vêm como **string decimal**.
- `cpf_cnpj` pode ser enviado com ou sem máscara.
- O campo `numero_parcela` em pagamentos é retornado com o mesmo valor de `numero_pagamento`.
- O valor efetivamente pago é calculado a partir de `itens_pagamento.valor_pagamento`.
- O endpoint `index()` retorna listas embutidas de `liquidacoes` e `pagamentos`, então o payload pode crescer bastante para páginas maiores.

---

# Resumo do fluxo financeiro

```txt
Empenho
   ↓
Liquidação
   ↓
Pagamento
   ↓
ItemPagamento
```

### Interpretação

- **Empenho**: autorização da despesa
- **Liquidação**: reconhecimento da despesa
- **Pagamento**: registro da parcela do pagamento
- **ItemPagamento**: valor financeiro efetivamente pago
