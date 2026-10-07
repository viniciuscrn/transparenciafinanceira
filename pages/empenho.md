# Empenhos — Documentação da API

## Visão Geral

O `EmpenhoController` disponibiliza a consulta de empenhos com os dados agregados de:

- liquidações
- pagamentos e itens de pagamento
- resumo financeiro (liquidado, pago e saldos)
- unidade orçamentária
- fornecedor (beneficiário)
- dados complementares de diárias
- descrições das tabelas internas (função, subfunção, natureza da despesa, fonte de recurso etc.)

Também expõe a **ordem cronológica de pagamentos** e a consulta de fornecedor por CPF/CNPJ.

> Todos os endpoints de consulta de empenhos são **públicos** (não exigem token).

---

## Endpoints Disponíveis

| Método | Endpoint                                         | Autenticação | Descrição                                                    |
| ------ | ------------------------------------------------ | ------------ | ------------------------------------------------------------ |
| GET    | `/api/empenhos`                                  | Pública      | Lista empenhos com filtros, totais, liquidações e pagamentos |
| GET    | `/api/empenhos/{id}`                             | Pública      | Detalhe completo de um empenho                               |
| GET    | `/api/empenhos/ordem-cronologica-pagamentos`     | Pública      | Ordem cronológica de pagamentos                              |
| GET    | `/api/empenhos/buscarFornecedorTce/{cpfCnpj}`    | Pública      | Consulta fornecedor (banco local → TCE)                      |
| PUT    | `/api/empenhos/{id}/diaria`                      | Token        | Cadastra/atualiza dados da diária                            |
| DELETE | `/api/empenhos/{id}/diaria`                      | Token        | Remove dados da diária                                       |

- Ordem cronológica: ver [ordem_cronologica_pagamentos.md](./ordem_cronologica_pagamentos.md).
- Diárias: ver [diarias.md](./diarias.md).
- Referência completa dos filtros: ver [empenhos-filtros.md](./empenhos-filtros.md).

---

# 1. Listar Empenhos

```http
GET /api/empenhos
```

Retorna uma lista paginada de empenhos, ordenada por `data_empenho` (mais recente primeiro), com o bloco `totais` no topo.

## Query Params

| Parâmetro              | Tipo                  | Descrição                                                   |
| ---------------------- | --------------------- | ----------------------------------------------------------- |
| `competencia`          | string (`YYYY-MM`)    | Competência exata                                           |
| `ano`                  | integer (2000–2100)   | Ano do empenho                                              |
| `mes`                  | integer (1–12)        | Mês do empenho                                              |
| `remessa_id`           | integer               | ID da remessa                                               |
| `unidade_codigo`       | string                | Código da unidade orçamentária                              |
| `unidade_orcamentaria` | string                | Código da unidade (alias para selects)                      |
| `numero_empenho`       | string                | Número do empenho (match exato)                             |
| `cpf_cnpj`             | string                | CPF ou CNPJ do credor, com ou sem máscara                   |
| `beneficiario`         | string                | Nome do fornecedor (busca parcial)                          |
| `data_ini`             | date (`YYYY-MM-DD`)   | Data inicial do empenho                                     |
| `data_fim`             | date (`YYYY-MM-DD`)   | Data final do empenho                                       |
| `min_valor`            | numeric               | Valor mínimo empenhado                                      |
| `max_valor`            | numeric               | Valor máximo empenhado                                      |
| `categoria_economica`  | string (1 char)       | 1º dígito da natureza da despesa                            |
| `grupo_natureza`       | string (1 char)       | 2º dígito da natureza da despesa                            |
| `elemento_despesa`     | string                | Um elemento de despesa (ex.: `14`)                          |
| `elementos`            | string (CSV)          | Vários elementos (ex.: `14,30,39`)                          |
| `modalidade_licitacao` | string                | Código da modalidade de licitação                           |
| `somente_diarias`      | boolean               | Apenas empenhos de diárias (elemento `14`)                  |
| `diaria_pendente`      | boolean               | Diárias ainda sem dados complementares cadastrados          |
| `q`                    | string (máx. 100)     | Busca livre em número, descrição, unidade e CPF/CNPJ        |
| `per_page`             | integer (1–200)       | Itens por página. Padrão `20`                               |
| `export`               | boolean               | Retorna todos os registros, sem paginação                   |

### Regras de `cpf_cnpj` e `q`

- Caracteres não numéricos são removidos.
- CPF (11 dígitos) também é procurado com zeros à esquerda (14 dígitos), pois `empenhos.cpf_cnpj_credor` é gravado com 14 dígitos.
- `q` busca em `numero_empenho`, `descricao`, `unidade_codigo` e `cpf_cnpj_credor`.

## Exemplos

```http
GET /api/empenhos?competencia=2025-02
GET /api/empenhos?ano=2025&mes=2&per_page=50
GET /api/empenhos?cpf_cnpj=40.628.309/0001-45
GET /api/empenhos?beneficiario=construtora&categoria_economica=4
GET /api/empenhos?elementos=14,30&data_ini=2025-01-01&data_fim=2025-06-30
GET /api/empenhos?ano=2025&export=1
```

## Resposta (paginada)

```json
{
  "totais": {
    "total_empenhado": "15240431.88",
    "total_liquidado": "13098610.78",
    "total_pago": "13049136.49"
  },
  "current_page": 1,
  "data": [
    {
      "id": 88,
      "remessa_id": 8,
      "ano": 2025,
      "mes": 2,
      "competencia": "2025-02",

      "unidade_codigo": "0000008001",
      "unidade_orcamentaria": {
        "codigo": "0000008001",
        "denominacao": "CÂMARA MUNICIPAL",
        "unidade_jurisdicionada": "..."
      },

      "numero_empenho": "0000049",
      "empenho_key": "2025|0000008001|0000049",
      "data_empenho": "2025-02-24",
      "valor_empenhado": "180.00",
      "cpf_cnpj_credor": "40628309000145",
      "descricao": "REFERENTE A SERVIÇOS DE CONFECÇÃO...",

      "funcao": "01",
      "funcao_descricao": "Legislativa",
      "subfuncao": "031",
      "subfuncao_descricao": "Ação Legislativa",
      "programa_codigo": "7001",
      "programa_descricao": null,
      "acao_codigo": "000017",
      "acao_descricao": null,
      "identificacao_acao": "2",
      "identificacao_acao_descricao": "Atividade",
      "natureza_despesa": "3.3.90.39.999",
      "natureza_despesa_detalhada": {
        "categoria": { "codigo": "3", "descricao": "Despesas Correntes" },
        "grupo": { "codigo": "3", "descricao": "Outras Despesas Correntes" },
        "modalidade": { "codigo": "90", "descricao": "Aplicações Diretas" },
        "elemento": { "codigo": "39", "descricao": "Outros Serviços de Terceiros - Pessoa Jurídica" },
        "subelemento": { "codigo": "999", "descricao": "..." }
      },
      "fonte_recurso": "15000000",
      "fonte_recurso_descricao": "Recursos não Vinculados de Impostos",
      "tipo_empenho": "1",
      "tipo_empenho_descricao": "Ordinário",
      "modalidade_licitacao": "8",
      "modalidade_licitacao_descricao": "Dispensa",
      "numero_procedimento": "000012025",
      "cpf_ordenador": "00000000000",

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

      "fornecedor": {
        "id": 3,
        "cpf_cnpj": "40628309000145",
        "nome": "EMPRESA EXEMPLO LTDA",
        "tipo_pessoa": "PJ",
        "cargo": null
      },

      "diaria": null,

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
          "created_at": "...",
          "updated_at": "..."
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
          "valor_pago": "180.00",
          "created_at": "...",
          "updated_at": "..."
        }
      ]
    }
  ],
  "per_page": 20,
  "total": 803
}
```

> Os valores das descrições acima são ilustrativos; vêm das tabelas internas em `resources/tabelas_internas`.

## Resposta com `export=1`

Sem paginação. Retorna todos os registros que atendem aos filtros:

```json
{
  "exported": true,
  "total_registros": 803,
  "totais": {
    "total_empenhado": "15240431.88",
    "total_liquidado": "13098610.78",
    "total_pago": "13049136.49"
  },
  "dados": [ { "...": "mesmo formato de cada item de data" } ]
}
```

## Bloco `totais`

| Campo             | Descrição                                                          |
| ----------------- | ------------------------------------------------------------------ |
| `total_empenhado` | Soma de `valor_empenhado` de **todos** os empenhos filtrados       |
| `total_liquidado` | Soma das liquidações desses empenhos (por `empenho_key`)           |
| `total_pago`      | Soma dos itens de pagamento desses empenhos (por `empenho_key`)    |

Os totais não dependem da página atual: mudar de página não altera os valores; mudar um filtro altera. Sem resultados, vêm como `"0.00"`.

## Bloco `resumo_liquidacao`

| Campo              | Tipo    | Descrição                       |
| ------------------ | ------- | ------------------------------- |
| `quantidade`       | integer | Quantidade de liquidações       |
| `total_liquidado`  | string  | Soma de `valor_liquidado`       |
| `saldo_a_liquidar` | string  | Valor empenhado − liquidado     |

## Bloco `resumo_pagamento`

| Campo                   | Tipo    | Descrição                                 |
| ----------------------- | ------- | ----------------------------------------- |
| `quantidade_pagamentos` | integer | Quantidade de pagamentos                  |
| `quantidade_itens`      | integer | Quantidade de itens de pagamento          |
| `total_pago`            | string  | Soma de `itens_pagamento.valor_pagamento` |
| `saldo_a_pagar`         | string  | Valor liquidado − pago                    |

## Bloco `fornecedor`

Buscado no cadastro local de fornecedores; se não existir, é consultado no TCE (limitado a ~10 s de consultas por requisição na listagem). Quando não encontrado, `id`, `nome`, `tipo_pessoa` e `cargo` vêm `null`, e `cpf_cnpj` traz o documento do credor.

| Campo         | Descrição                                   |
| ------------- | ------------------------------------------- |
| `id`          | ID do fornecedor (usado em `/fornecedores`) |
| `cpf_cnpj`    | CPF (11) ou CNPJ (14)                       |
| `nome`        | Nome ou razão social                        |
| `tipo_pessoa` | `PF` ou `PJ`                                |
| `cargo`       | Cargo atual do beneficiário (diárias)       |

## Bloco `diaria`

Dados complementares de empenhos de diárias, ou `null`. Ver [diarias.md](./diarias.md).

## Erros de validação (`422`)

```json
{
  "message": "The competencia field format is invalid.",
  "errors": {
    "competencia": ["The competencia field format is invalid."]
  }
}
```

Casos comuns: `competencia` fora de `YYYY-MM`, `ano` fora de 2000–2100, `mes` fora de 1–12, datas inválidas, `per_page` fora de 1–200.

---

# 2. Detalhar Empenho

```http
GET /api/empenhos/{id}
```

Retorna os mesmos campos de um item da listagem (sem o bloco `totais`), mais a lista `itens_pagamento`.

### `itens_pagamento`

```json
{
  "id": 5,
  "remessa_id": 8,
  "competencia": "2025-02",
  "unidade_codigo": "0000008001",
  "numero_empenho": "0000049",
  "empenho_key": "2025|0000008001|0000049",
  "numero_pagamento": "0000001",
  "pagamento_key": "2025|0000008001|0000049|0000001",
  "numero_sequencial": "001",
  "item_pagamento_key": "...",
  "valor_pagamento": "180.00",
  "conta_debito": "...",
  "numero_cheque": null,
  "numero_doc_debito": "...",
  "banco_credito": "001",
  "agencia_credito": "...",
  "conta_credito": "...",
  "fonte_recurso": "15000000",
  "tipo_conta_debito": "...",
  "tipo_pagamento": "...",
  "chave_pix": null,
  "created_at": "...",
  "updated_at": "..."
}
```

Retorna `404` quando o empenho não existe.

---

# 3. Buscar fornecedor (TCE)

```http
GET /api/empenhos/buscarFornecedorTce/{cpfCnpj}
```

Mesmo fluxo de `GET /api/fornecedores/buscar/{cpfCnpj}`, mas público: consulta o banco local e, se não encontrar, a API do TCE (salvando o resultado localmente).

### Sucesso (`200`)

```json
{
  "success": true,
  "origem": "TCE",
  "data": {
    "id": 4,
    "cpf_cnpj": "40628309000145",
    "nome": "EMPRESA EXEMPLO LTDA",
    "tipo_pessoa": "PJ",
    "cargo": null
  }
}
```

`origem` é `LOCAL` ou `TCE`.

### Não encontrado ou documento inválido (`404`)

```json
{
  "success": false,
  "message": "Fornecedor não encontrado."
}
```

---

# Observações para o front-end

- Valores monetários vêm como **string decimal** com ponto (`"180.00"`). Converta com `Number(...)` antes de formatar.
- `unidade_codigo` é retornado sempre com 10 dígitos (zeros à esquerda).
- `numero_parcela` em pagamentos tem o mesmo valor de `numero_pagamento`.
- O valor pago é calculado a partir de `itens_pagamento.valor_pagamento`.
- `programa_descricao` e `acao_descricao` ainda vêm sempre `null`.
- A listagem traz `liquidacoes` e `pagamentos` embutidos; o payload cresce com `per_page` alto.

---

# Fluxo financeiro

```txt
Empenho → Liquidação → Pagamento → ItemPagamento
```

- **Empenho**: autorização da despesa
- **Liquidação**: reconhecimento da despesa
- **Pagamento**: registro da parcela do pagamento
- **ItemPagamento**: valor financeiro efetivamente pago
