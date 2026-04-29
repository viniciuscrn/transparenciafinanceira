# API de Empenhos — Filtros

Documentação dos filtros disponíveis no endpoint de listagem de empenhos.

**Endpoint:** `GET /api/empenhos`

---

## Sumário

- [Novos filtros](#novos-filtros)
  - [`categoria_economica`](#1-categoria_economica)
  - [`grupo_natureza`](#2-grupo_natureza)
  - [`unidade_orcamentaria`](#3-unidade_orcamentaria)
  - [`beneficiario`](#4-beneficiario)
- [Exemplos combinados](#exemplos-combinados)
- [Tabelas de referência](#tabelas-de-referência)
- [Lista completa de filtros](#lista-completa-de-filtros)

---

## Novos filtros

### 1. `categoria_economica`

Filtra empenhos pela categoria econômica da natureza de despesa (primeiro dígito do código).

| Atributo | Valor |
|---|---|
| Tipo | `string` |
| Tamanho máximo | 1 caractere |
| Obrigatório | Não |
| Aplicação | Match exato no primeiro dígito de `natureza_despesa` |
| Uso recomendado | Select / dropdown |

**Exemplo:**

```http
GET /api/empenhos?categoria_economica=3
```

Retorna empenhos com natureza de despesa começando com `3.` (Despesas Correntes).

---

### 2. `grupo_natureza`

Filtra empenhos pelo grupo de natureza da despesa (segundo dígito do código).

| Atributo | Valor |
|---|---|
| Tipo | `string` |
| Tamanho máximo | 1 caractere |
| Obrigatório | Não |
| Aplicação | Match exato no segundo dígito de `natureza_despesa` |
| Uso recomendado | Select / dropdown |

**Exemplo:**

```http
GET /api/empenhos?grupo_natureza=3
```

Retorna empenhos cujo grupo de natureza é `3` (Outras Despesas Correntes), independentemente da categoria econômica.

---

### 3. `unidade_orcamentaria`

Filtra empenhos pelo código da unidade orçamentária.

| Atributo | Valor |
|---|---|
| Tipo | `string` |
| Tamanho máximo | 20 caracteres |
| Obrigatório | Não |
| Aplicação | Match exato em `unidade_codigo` |
| Uso recomendado | Select / dropdown |

**Exemplo:**

```http
GET /api/empenhos?unidade_orcamentaria=0101000000
```

> **Nota:** O valor enviado deve ser o **código** da unidade (não a denominação). O front-end deve montar o select com o código como `value` e a denominação como `label`.

---

### 4. `beneficiario`

Filtra empenhos pelo nome do beneficiário (fornecedor). Busca parcial usando `LIKE`.

| Atributo | Valor |
|---|---|
| Tipo | `string` |
| Tamanho máximo | 150 caracteres |
| Obrigatório | Não |
| Aplicação | Busca parcial (`LIKE %valor%`) em `fornecedores.nome` |
| Uso recomendado | Input de texto livre |

**Exemplo:**

```http
GET /api/empenhos?beneficiario=PETROBRAS
```

```http
GET /api/empenhos?beneficiario=construtora silva
```

> **Comportamento:** O filtro busca todos os fornecedores cujo nome contém o termo informado (case-insensitive, dependendo do collation do banco) e filtra os empenhos pelos `cpf_cnpj` correspondentes. Se nenhum fornecedor for encontrado, o resultado será vazio.

---

## Exemplos combinados

Os novos filtros podem ser combinados livremente entre si e com os filtros já existentes.

### Despesas de capital de uma unidade específica

```http
GET /api/empenhos?categoria_economica=4&unidade_orcamentaria=0101000000
```

### Investimentos de um beneficiário no ano de 2025

```http
GET /api/empenhos?ano=2025&categoria_economica=4&grupo_natureza=4&beneficiario=construtora
```

### Outras despesas correntes em um período, com valor mínimo

```http
GET /api/empenhos?categoria_economica=3&grupo_natureza=3&data_ini=2025-01-01&data_fim=2025-06-30&min_valor=1000&per_page=50
```

### Filtro completo para um relatório

```http
GET /api/empenhos?ano=2025&mes=4&unidade_orcamentaria=0101000000&categoria_economica=3&grupo_natureza=3&beneficiario=silva&min_valor=500&max_valor=50000&per_page=20
```

---

## Tabelas de referência

> Os códigos abaixo são os mais comuns no plano de contas público brasileiro. A lista oficial e completa do sistema está nos arquivos `resources/tabelas_internas/categorias_economicas.json` e `resources/tabelas_internas/grupos_naturezas.json`.

### Categorias Econômicas

| Código | Descrição |
|:---:|---|
| `3` | Despesas Correntes |
| `4` | Despesas de Capital |

### Grupos de Natureza

| Código | Descrição |
|:---:|---|
| `1` | Pessoal e Encargos Sociais |
| `2` | Juros e Encargos da Dívida |
| `3` | Outras Despesas Correntes |
| `4` | Investimentos |
| `5` | Inversões Financeiras |
| `6` | Amortização da Dívida |

### Estrutura do código `natureza_despesa`

O campo `natureza_despesa` segue o formato `C.G.MM.EE.SSS`:

```
3 . 3 . 90 . 14 . 00
│   │    │    │    │
│   │    │    │    └─ Subelemento (3 dígitos)
│   │    │    └────── Elemento de despesa (2 dígitos)
│   │    └─────────── Modalidade de aplicação (2 dígitos)
│   └──────────────── Grupo de natureza (1 dígito) ◄── grupo_natureza
└──────────────────── Categoria econômica (1 dígito) ◄── categoria_economica
```

---

## Lista completa de filtros

Todos os filtros aceitos pelo endpoint `GET /api/empenhos`:

| Parâmetro | Tipo | Descrição |
|---|---|---|
| `competencia` | `string` (`YYYY-MM`) | Competência exata |
| `ano` | `integer` (2000–2100) | Ano do empenho |
| `mes` | `integer` (1–12) | Mês do empenho |
| `remessa_id` | `integer` | ID da remessa |
| `unidade_codigo` | `string` | Código da unidade orçamentária |
| `unidade_orcamentaria` | `string` | **(novo)** Código da unidade — alias para uso em selects |
| `numero_empenho` | `string` | Número do empenho (match exato) |
| `cpf_cnpj` | `string` | CPF ou CNPJ do credor (com normalização) |
| `data_ini` | `date` (`YYYY-MM-DD`) | Data inicial do empenho |
| `data_fim` | `date` (`YYYY-MM-DD`) | Data final do empenho |
| `min_valor` | `numeric` | Valor mínimo empenhado |
| `max_valor` | `numeric` | Valor máximo empenhado |
| `categoria_economica` | `string` (1 char) | **(novo)** Categoria econômica |
| `grupo_natureza` | `string` (1 char) | **(novo)** Grupo de natureza |
| `beneficiario` | `string` | **(novo)** Nome do beneficiário (busca parcial) |
| `elemento_despesa` | `string` | Elemento de despesa (match único) |
| `elementos` | `string` (CSV) | Múltiplos elementos separados por vírgula |
| `somente_diarias` | `boolean` | Apenas empenhos de diárias |
| `q` | `string` (max 100) | Busca livre em número, descrição, unidade e CPF/CNPJ |
| `per_page` | `integer` (1–200) | Itens por página (default 20) |
| `export` | `boolean` | Quando `true`, retorna todos os registros sem paginação |

---

## Observações

- Todos os filtros são **opcionais** e podem ser combinados livremente.
- Filtros vazios ou ausentes são ignorados.
- A resposta padrão é paginada (formato Laravel paginator). Para exportação completa, use `export=true`.
