# Diárias — Dados Complementares

## Visão Geral

Empenhos de diárias (elemento de despesa `14`, ex.: `3.3.90.14.xx`) precisam de dados que não vêm do SAGRES. Eles são informados manualmente:

| Dado | Onde fica | Observação |
| --- | --- | --- |
| Cargo do beneficiário | `fornecedores.cargo` | Cargo atual da pessoa |
| Cargo na época da diária | `empenho_diarias.cargo` | Preenchido com o cargo atual do beneficiário no primeiro cadastro, se não for enviado |
| Nº de diárias | `empenho_diarias.quantidade_diarias` | Aceita meia diária (ex.: `2.5`) |
| Período de afastamento | `empenho_diarias.data_inicio` / `data_fim` | |
| Local | `empenho_diarias.local` | Destino |
| Motivo | `empenho_diarias.motivo` | Opcional |

Os dados da diária são ligados ao `empenho_key`, e não ao `id` do empenho. Assim, não se perdem quando uma remessa é reimportada ou excluída.

---

## Endpoints

| Método | Endpoint | Autenticação | Descrição |
| --- | --- | --- | --- |
| PUT | `/api/fornecedores/{id}` | Token | Define o cargo do beneficiário (campo `cargo`) |
| PUT | `/api/empenhos/{id}/diaria` | Token | Cadastra ou atualiza os dados da diária |
| DELETE | `/api/empenhos/{id}/diaria` | Token | Remove os dados da diária |
| GET | `/api/empenhos` e `/api/empenhos/{id}` | Pública | Retornam os blocos `diaria` e `fornecedor.cargo` |

---

# 1. Cargo do beneficiário

```http
PUT /api/fornecedores/13
Authorization: Bearer {token}
Content-Type: application/json
```

```json
{
  "cargo": "Vereador"
}
```

O campo `cargo` também é aceito no cadastro (`POST /api/fornecedores`). O `id` do fornecedor vem em `fornecedor.id` na resposta de `GET /api/empenhos`.

---

# 2. Cadastrar / atualizar diária

```http
PUT /api/empenhos/97/diaria
Authorization: Bearer {token}
Content-Type: application/json
```

```json
{
  "cargo": "Presidente da Câmara",
  "quantidade_diarias": 2.5,
  "data_inicio": "2026-01-10",
  "data_fim": "2026-01-12",
  "local": "Recife/PE",
  "motivo": "Participação em capacitação no TCE-PE"
}
```

## Campos

| Campo | Tipo | Obrigatório | Regra |
| --- | --- | ---: | --- |
| `cargo` | string | não | Máx. 150. Se omitido no primeiro cadastro, usa o cargo atual do beneficiário |
| `quantidade_diarias` | number | sim | Mín. `0.5` |
| `data_inicio` | date | sim | Formato `YYYY-MM-DD` |
| `data_fim` | date | sim | Formato `YYYY-MM-DD`, igual ou posterior a `data_inicio` |
| `local` | string | sim | Máx. 255 |
| `motivo` | string | não | Máx. 5000 |

## Respostas

| Status | Quando |
| --- | --- |
| `201` | Dados cadastrados |
| `200` | Dados atualizados |
| `401` | Sem token |
| `404` | Empenho não encontrado |
| `422` | Empenho não é de diárias, ou erro de validação |

```json
{
  "message": "Dados da diária cadastrados com sucesso.",
  "data": {
    "id": 1,
    "empenho_key": "2026|0000001001|0000040",
    "cargo": "Presidente da Câmara",
    "quantidade_diarias": "2.50",
    "data_inicio": "2026-01-10",
    "data_fim": "2026-01-12",
    "local": "Recife/PE",
    "motivo": "Participação em capacitação no TCE-PE",
    "created_at": "2026-10-07T19:20:47.000000Z",
    "updated_at": "2026-10-07T19:20:47.000000Z"
  }
}
```

---

# 3. Remover diária

```http
DELETE /api/empenhos/97/diaria
Authorization: Bearer {token}
```

Retorna `200` quando remove e `404` quando o empenho não tem dados de diária.

---

# 4. Consulta pública

`GET /api/empenhos` e `GET /api/empenhos/{id}` passam a retornar:

```json
{
  "fornecedor": {
    "id": 13,
    "cpf_cnpj": "03440107400",
    "nome": "FULANO DE TAL",
    "tipo_pessoa": "PF",
    "cargo": "Vereador"
  },
  "diaria": {
    "cargo": "Presidente da Câmara",
    "quantidade_diarias": "2.50",
    "data_inicio": "2026-01-10",
    "data_fim": "2026-01-12",
    "local": "Recife/PE",
    "motivo": "Participação em capacitação no TCE-PE",
    "updated_at": "2026-10-07T19:20:47.000000Z"
  }
}
```

`diaria` é `null` quando o empenho não tem dados cadastrados. Para exibir o cargo na diária, use `diaria.cargo`; `fornecedor.cargo` é o cargo atual da pessoa.

## Filtros úteis

| Parâmetro | Descrição |
| --- | --- |
| `somente_diarias=1` | Apenas empenhos de diárias |
| `diaria_pendente=1` | Empenhos de diárias que ainda não têm os dados complementares cadastrados |

```http
GET /api/empenhos?diaria_pendente=1&per_page=50
```
