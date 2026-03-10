# FornecedorController — Documentação da API

Esta documentação descreve os endpoints disponíveis no **FornecedorController**, responsáveis pela gestão e consulta de fornecedores.

Base URL de exemplo:

```
/api/fornecedores
```

---

# Estrutura da Entidade

Campos da tabela `fornecedores`:

| Campo       | Tipo       | Descrição                                  |
| ----------- | ---------- | ------------------------------------------ |
| id          | integer    | Identificador interno                      |
| cpf_cnpj    | string(14) | CPF ou CNPJ do fornecedor (apenas números) |
| nome        | string     | Nome ou razão social                       |
| tipo_pessoa | string(2)  | PF (Pessoa Física) ou PJ (Pessoa Jurídica) |
| created_at  | datetime   | Data de criação                            |
| updated_at  | datetime   | Data de atualização                        |

---

# Endpoints

## 1. Listar fornecedores

### Endpoint

```
GET /api/fornecedores
```

### Parâmetros de filtro

| Parâmetro   | Tipo    | Descrição                                     |
| ----------- | ------- | --------------------------------------------- |
| cpf_cnpj    | string  | Busca por CPF ou CNPJ                         |
| nome        | string  | Busca parcial por nome                        |
| tipo_pessoa | string  | PF ou PJ                                      |
| q           | string  | Busca geral por nome ou CPF/CNPJ              |
| per_page    | integer | Quantidade por página (padrão 20, máximo 200) |

### Exemplo

```
GET /api/fornecedores?q=vale
```

### Resposta

```json
{
  "current_page": 1,
  "data": [
    {
      "id": 3,
      "cpf_cnpj": "12223678000201",
      "nome": "VALE DO PAJEU IGUARACI",
      "tipo_pessoa": "PJ",
      "created_at": "2026-03-10T23:01:13.000000Z",
      "updated_at": "2026-03-10T23:01:13.000000Z"
    }
  ],
  "per_page": 20,
  "total": 1
}
```

---

# 2. Detalhar fornecedor

### Endpoint

```
GET /api/fornecedores/{id}
```

### Exemplo

```
GET /api/fornecedores/3
```

### Resposta

```json
{
  "id": 3,
  "cpf_cnpj": "12223678000201",
  "nome": "VALE DO PAJEU IGUARACI",
  "tipo_pessoa": "PJ",
  "created_at": "2026-03-10T23:01:13.000000Z",
  "updated_at": "2026-03-10T23:01:13.000000Z"
}
```

---

# 3. Cadastrar fornecedor

### Endpoint

```
POST /api/fornecedores
```

### Body

```json
{
  "cpf_cnpj": "12223678000201",
  "nome": "VALE DO PAJEU IGUARACI",
  "tipo_pessoa": "PJ"
}
```

### Resposta

```json
{
  "message": "Fornecedor cadastrado com sucesso.",
  "data": {
    "id": 4,
    "cpf_cnpj": "12223678000201",
    "nome": "VALE DO PAJEU IGUARACI",
    "tipo_pessoa": "PJ"
  }
}
```

---

# 4. Atualizar fornecedor

### Endpoint

```
PUT /api/fornecedores/{id}
```

ou

```
PATCH /api/fornecedores/{id}
```

### Body

```json
{
  "nome": "VALE DO PAJEU IGUARACI LTDA"
}
```

### Resposta

```json
{
  "message": "Fornecedor atualizado com sucesso.",
  "data": {
    "id": 4,
    "cpf_cnpj": "12223678000201",
    "nome": "VALE DO PAJEU IGUARACI LTDA",
    "tipo_pessoa": "PJ"
  }
}
```

---

# 5. Excluir fornecedor

### Endpoint

```
DELETE /api/fornecedores/{id}
```

### Exemplo

```
DELETE /api/fornecedores/4
```

### Resposta

```json
{
  "message": "Fornecedor excluído com sucesso.",
  "id": 4,
  "cpf_cnpj": "12223678000201"
}
```

---

# 6. Buscar fornecedor por CPF/CNPJ (TCE + Banco Local)

Este endpoint utiliza o **FornecedorService**, que segue o fluxo:

```
1. Consulta banco local
2. Se não encontrar → consulta API do TCE
3. Se encontrado no TCE → salva no banco
4. Retorna o fornecedor
```

### Endpoint

```
GET /api/fornecedores/buscar/{cpfCnpj}
```

### Exemplo

```
GET /api/fornecedores/buscar/12223678000201
```

### Resposta

```json
{
  "success": true,
  "origem": "TCE",
  "data": {
    "cpf_cnpj": "12223678000201",
    "nome": "VALE DO PAJEU IGUARACI",
    "tipo_pessoa": "PJ"
  }
}
```

### Possíveis origens

| Origem | Descrição                               |
| ------ | --------------------------------------- |
| LOCAL  | encontrado no banco local               |
| TCE    | encontrado na API do Tribunal de Contas |

---

# Regras importantes

- `cpf_cnpj` é armazenado **apenas com números**
- CPF possui **11 dígitos**
- CNPJ possui **14 dígitos**
- `tipo_pessoa` é inferido automaticamente quando não informado

---

# Fluxo de dados

```
FornecedorController
        ↓
FornecedorService
        ↓
Banco Local
        ↓
API TCE (fallback)
```

---

# Observação

O endpoint de busca (`/buscar/{cpfCnpj}`) pode consultar serviços externos.
Após a primeira consulta bem‑sucedida, o fornecedor fica salvo no banco local para melhorar a performance das próximas requisições.
