# 📊 API – Receitas de Transferência

Documentação completa da API REST responsável pelo gerenciamento de **Receitas de Transferência**.

---

## 📌 Base URL

```
/api/receitas-transferencias
```

---

## 🔐 Autenticação

Opcional (caso utilize Laravel Sanctum ou JWT).

```
Authorization: Bearer {token}
```

---

## 📂 Endpoints

---

## 🔹 Listar Receitas

### GET /api/receitas-transferencias

Retorna uma lista paginada de receitas.

### Parâmetros de Query (opcionais)

| Parâmetro          | Tipo   | Descrição                    |
| ------------------ | ------ | ---------------------------- |
| ano                | int    | Filtra pelo ano              |
| mes                | int    | Filtra pelo mês (1–12)       |
| descricao          | string | Busca parcial pela descrição |
| unidade_recebedora | string | Busca parcial pela unidade   |
| per_page           | int    | Itens por página (1–100)     |

### Exemplo

```http
GET /api/receitas-transferencias?ano=2025&mes=3
```

### Resposta 200

```json
{
    "current_page": 1,
    "data": [],
    "total": 0
}
```

---

## 🔹 Criar Receita

### POST /api/receitas-transferencias

### Corpo da Requisição

```json
{
    "ano": 2025,
    "mes": 3,
    "descricao": "Transferência Estadual",
    "unidade_recebedora": "Secretaria de Educação",
    "data_recebimento": "2025-03-20",
    "receita_mensal_prevista": 120000.0,
    "receita_extra_orcamentaria": 8000.0,
    "receita_realizada": 128000.0,
    "receita_acumulada": 350000.0,
    "acumulada_com_extra_orcamentaria": 358000.0
}
```

### Resposta 201

```json
{
    "message": "Receita cadastrada com sucesso.",
    "data": {}
}
```

---

## 🔹 Detalhar Receita

### GET /api/receitas-transferencias/{id}

### Resposta 200

```json
{
    "data": {}
}
```

---

## 🔹 Atualizar Receita

### PUT /api/receitas-transferencias/{id}

```json
{
    "receita_realizada": 130000.0
}
```

### Resposta 200

```json
{
    "message": "Receita atualizada com sucesso.",
    "data": {}
}
```

---

## 🔹 Remover Receita

### DELETE /api/receitas-transferencias/{id}

### Resposta

204 No Content

---

## ⚠️ Códigos de Erro

| Código | Descrição               |
| ------ | ----------------------- |
| 400    | Requisição inválida     |
| 401    | Não autenticado         |
| 403    | Não autorizado          |
| 404    | Registro não encontrado |
| 422    | Erro de validação       |
| 500    | Erro interno            |

---

## 🧾 Estrutura do Objeto

```json
{
    "ano": "int",
    "mes": "int",
    "descricao": "string",
    "unidade_recebedora": "string",
    "data_recebimento": "date",
    "receita_mensal_prevista": "decimal(12,2)",
    "receita_extra_orcamentaria": "decimal(12,2)",
    "receita_realizada": "decimal(12,2)",
    "receita_acumulada": "decimal(12,2)",
    "acumulada_com_extra_orcamentaria": "decimal(12,2)"
}
```

---

## 📌 Observações

-   API RESTful
-   Respostas em JSON
-   Paginação nativa do Laravel
-   Pronta para uso institucional
