# Despesas Consolidadas

## Visão Geral

Retorna os totais de despesa (empenhado, liquidado e pago) e os saldos, aplicando os filtros informados, junto com o nome da unidade orçamentária.

```http
GET /api/despesas-consolidadas
```

> Endpoint **público** (não exige token).

---

## Query Params

| Parâmetro        | Tipo                | Descrição                                   |
| ---------------- | ------------------- | ------------------------------------------- |
| `ano`            | integer (2000–2100) | Ano                                         |
| `mes`            | integer (1–12)      | Mês                                         |
| `data_ini`       | date (`YYYY-MM-DD`) | Início do período                           |
| `data_fim`       | date (`YYYY-MM-DD`) | Fim do período                              |
| `unidade_codigo` | string              | Código de uma unidade orçamentária          |
| `remessa_id`     | integer             | ID da remessa                               |

### Como cada total é filtrado

| Total          | Tabela             | Campo de data usado em `data_ini`/`data_fim`        |
| -------------- | ------------------ | --------------------------------------------------- |
| Empenhado      | `empenhos`         | `data_empenho`                                      |
| Liquidado      | `liquidacoes`      | `data_liquidacao`                                   |
| Pago           | `itens_pagamento`  | `pagamentos.data_pagamento` (ligado por `pagamento_key`) |

`ano`, `mes`, `remessa_id` e `unidade_codigo` são aplicados em cada tabela.

> O total pago inclui itens enviados em remessas posteriores à do cabeçalho do pagamento (o SAGRES pode enviar itens novos de um pagamento antigo sem reenviar o cabeçalho).

## Exemplos

```http
GET /api/despesas-consolidadas?ano=2025
GET /api/despesas-consolidadas?ano=2025&mes=3&unidade_codigo=0000008001
GET /api/despesas-consolidadas?data_ini=2025-01-01&data_fim=2025-06-30
```

---

## Resposta (`200`)

```json
{
  "total_geral": {
    "valor_total_empenhado": "15240431.88",
    "valor_total_liquidado": "13098610.78",
    "valor_total_pago": "13049136.49",
    "saldo_a_liquidar": "2141821.10",
    "saldo_a_pagar": "49474.29"
  },
  "unidade_orcamentaria": "CÂMARA MUNICIPAL"
}
```

| Campo                   | Descrição                         |
| ----------------------- | --------------------------------- |
| `valor_total_empenhado` | Soma de `valor_empenhado`         |
| `valor_total_liquidado` | Soma de `valor_liquidado`         |
| `valor_total_pago`      | Soma de `valor_pagamento` dos itens |
| `saldo_a_liquidar`      | Empenhado − liquidado             |
| `saldo_a_pagar`         | Liquidado − pago                  |
| `unidade_orcamentaria`  | Denominação da unidade, ou `null` |

### Regra de `unidade_orcamentaria`

- Com `unidade_codigo`: busca a denominação dessa unidade (com ou sem zeros à esquerda).
- Sem `unidade_codigo`: se todos os empenhos filtrados forem da mesma unidade, retorna a denominação dela; se houver mais de uma unidade, retorna `null`.

Valores monetários vêm como string decimal com ponto.
