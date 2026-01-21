# EmpenhoController — Documentação Técnica

## Visão Geral

O **EmpenhoController** é responsável por disponibilizar a **consulta e exploração dos dados de empenhos orçamentários** importados a partir das remessas SAGRES.

Este controller atua exclusivamente em modo **leitura (read-only)**, fornecendo mecanismos de filtragem, paginação e busca textual para consumo por dashboards, relatórios e interfaces web.

---

## Rotas Associadas

| Método | Rota                 | Descrição                     |
| ------ | -------------------- | ----------------------------- |
| GET    | `/api/empenhos`      | Lista empenhos com filtros    |
| GET    | `/api/empenhos/{id}` | Retorna um empenho específico |

---

## Características Gerais

- Não cria, altera ou exclui registros
- Opera apenas sobre dados previamente importados
- Suporta múltiplos filtros combináveis
- Respostas paginadas
- Adequado para consumo por aplicações frontend e APIs públicas

---

## Endpoint: Listagem de Empenhos

### GET `/api/empenhos`

Retorna uma lista paginada de empenhos conforme filtros informados.

### Parâmetros de Consulta (Query Params)

| Parâmetro      | Tipo                | Descrição                          |
| -------------- | ------------------- | ---------------------------------- |
| competencia    | string (`YYYY-MM`)  | Filtra pela competência da remessa |
| remessa_id     | integer             | Filtra pelo ID da remessa          |
| unidade_codigo | string              | Código da unidade orçamentária     |
| numero_empenho | string              | Número do empenho                  |
| data_ini       | date (`YYYY-MM-DD`) | Data inicial do empenho            |
| data_fim       | date (`YYYY-MM-DD`) | Data final do empenho              |
| min_valor      | decimal             | Valor mínimo empenhado             |
| max_valor      | decimal             | Valor máximo empenhado             |
| q              | string              | Busca textual simples              |
| per_page       | integer             | Quantidade por página (1–200)      |

---

### Busca Textual (`q`)

O parâmetro `q` realiza busca parcial (`LIKE`) nos seguintes campos:

- número do empenho
- descrição do empenho
- CPF/CNPJ do credor
- código da unidade orçamentária

---

### Ordenação

A listagem é ordenada por:

1. `data_empenho` (decrescente)
2. `id` (decrescente)

---

### Paginação

- Valor padrão: **20 registros por página**
- Valor máximo permitido: **200 registros**
- Utiliza paginação padrão do Laravel (`LengthAwarePaginator`)

---

### Campos Retornados

```json
{
    "id": 88,
    "remessa_id": 8,
    "competencia": "2025-02",
    "unidade_codigo": "0000008001",
    "numero_empenho": "0000049",
    "empenho_key": "2025|0000008001|0000049",
    "data_empenho": "2025-02-24",
    "valor_empenhado": "180.00",
    "cpf_cnpj_credor": "40628309000145",
    "descricao": "REFERENTE A SERVIÇOS ...",
    "natureza_despesa": "3.3.90.39.999",
    "fonte_recurso": "15000000",
    "programa_codigo": "7001",
    "acao_codigo": "000017",
    "created_at": "2026-01-21T19:44:44.000000Z"
}
```

---

## Endpoint: Detalhe do Empenho

### GET `/api/empenhos/{id}`

Retorna os dados completos de um empenho específico.

### Parâmetro de Rota

| Campo | Tipo    | Descrição                      |
| ----- | ------- | ------------------------------ |
| id    | integer | Identificador único do empenho |

### Resposta

Retorna o objeto completo do empenho conforme persistido no banco.

---

## Validações Aplicadas

- Formato de competência (`YYYY-MM`)
- Datas no formato ISO (`YYYY-MM-DD`)
- Valores monetários positivos
- Limite máximo de paginação
- Strings com tamanho máximo controlado

---

## Regras de Negócio

- Empenhos são imutáveis via API
- Sempre vinculados a uma remessa
- Identificados tecnicamente por `empenho_key`
- A competência é herdada da remessa de origem

---

## Considerações de Performance

- Índices recomendados:
    - `competencia`
    - `remessa_id`
    - `empenho_key`
    - `data_empenho`
- Paginação obrigatória evita retornos massivos
- Busca textual simples para manter baixo custo de consulta

---

## Segurança

Recomenda-se proteger este controller com:

- autenticação (`auth:sanctum`)
- rate limiting
- cache em camada HTTP quando exposto publicamente

---

## Responsabilidades do Controller

✔ Filtragem e paginação
✔ Validação de parâmetros
✔ Exposição segura dos dados

❌ Importação de dados
❌ Processamento de arquivos
❌ Alteração de registros

---

## Status da Implementação

- [x] Listagem com filtros
- [x] Paginação configurável
- [x] Busca textual
- [x] Endpoint de detalhe
- [x] Pronto para integração frontend

---

Documento gerado para documentação técnica, auditoria e apoio ao desenvolvimento.
