# 📊 API – Receitas Previstas

Documentação completa da API REST responsável pelo gerenciamento de **Receitas Previstas**, incluindo o endpoint especial para obtenção da **última receita prevista (maior ano)**.

---

## 📌 Base URL

```
/api/receitas-previstas
```

---

## 🔐 Autenticação

Opcional (caso utilize Laravel Sanctum ou JWT).

```
Authorization: Bearer {token}
```

---

## 📂 Endpoints Disponíveis

---

## 🔹 Listar Receitas Previstas

### GET /api/receitas-previstas

Retorna todas as receitas previstas, ordenadas por ano (descendente).

### Resposta (200)

```json
[
    {
        "id": 3,
        "ano": 2026,
        "titulo": "Receita anual prevista ano 2026",
        "valor_estimado": "3676876.98"
    }
]
```

---

## 🔹 Criar Receita Prevista

### POST /api/receitas-previstas

### Corpo da Requisição

```json
{
    "ano": 2026,
    "titulo": "Receita anual prevista ano 2026",
    "valor_estimado": 3676876.98
}
```

### Resposta (201)

```json
{
    "message": "Receita prevista cadastrada com sucesso.",
    "data": {}
}
```

---

## 🔹 Detalhar Receita Prevista

### GET /api/receitas-previstas/{id}

### Resposta (200)

```json
{
    "data": {
        "id": 3,
        "ano": 2026,
        "titulo": "Receita anual prevista ano 2026",
        "valor_estimado": "3676876.98"
    }
}
```

---

## 🔹 Atualizar Receita Prevista

### PUT /api/receitas-previstas/{id}

```json
{
    "valor_estimado": 3800000.0
}
```

### Resposta (200)

```json
{
    "message": "Receita prevista atualizada com sucesso.",
    "data": {}
}
```

---

## 🔹 Remover Receita Prevista

### DELETE /api/receitas-previstas/{id}

### Resposta

- **204 No Content**

---

## ⭐ Endpoint Especial – Última Receita Prevista

### GET /api/receitas-previstas/ultima

Retorna a **Receita Prevista mais recente**, considerando o maior valor do campo `ano`.

### Exemplo de Requisição

```http
GET /api/receitas-previstas/ultima
```

### Resposta de Sucesso (200)

```json
{
    "data": {
        "id": 3,
        "ano": 2026,
        "titulo": "Receita anual prevista ano 2026",
        "valor_estimado": "3676876.98",
        "created_at": "2025-01-10T14:30:00Z",
        "updated_at": "2025-01-10T14:30:00Z"
    }
}
```

### Resposta de Erro (404)

```json
{
    "message": "Nenhuma receita prevista encontrada."
}
```

---

## 🧾 Estrutura do Objeto `ReceitaPrevista`

```json
{
    "ano": "int",
    "titulo": "string",
    "valor_estimado": "decimal(14,2)"
}
```

---

## ⚠️ Códigos HTTP Utilizados

| Código | Descrição               |
| ------ | ----------------------- |
| 200    | Sucesso                 |
| 201    | Criado                  |
| 204    | Sem conteúdo            |
| 400    | Requisição inválida     |
| 401    | Não autenticado         |
| 404    | Registro não encontrado |
| 422    | Erro de validação       |
| 500    | Erro interno            |

---

## 📌 Observações Técnicas

- O campo `ano` é **único**
- API RESTful
- Respostas em JSON
- Pronta para dashboards e planejamento orçamentário
- Endpoint `/ultima` não recebe parâmetros
- Implementação principal:
    ```php
    ReceitaPrevista::orderBy('ano', 'desc')->first();
    ```
