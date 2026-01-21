# Autenticação — API (Laravel Sanctum)

Esta seção documenta os endpoints do **AuthController**.

## Visão geral

-   A autenticação é feita via **token pessoal** (Sanctum).
-   O login gera um token e o retorna no campo `token`.
-   Para acessar rotas protegidas, envie o token como **Bearer** no header `Authorization`.

### Headers padrão

```http
Accept: application/json
Content-Type: application/json
Authorization: Bearer <TOKEN>
```

---

## POST `/api/login`

Autentica o usuário e devolve um **token Sanctum** e dados básicos do usuário.

### Body (JSON)

| Campo         | Tipo   | Obrigatório | Regras              |
| ------------- | ------ | ----------- | ------------------- |
| `email`       | string | sim         | `email`             |
| `password`    | string | sim         | —                   |
| `device_name` | string | não         | `string` (opcional) |

> `device_name` é usado como nome do token (ex.: `"Web"`, `"iPhone"`). Se não for enviado, o token será criado com o nome `"api_token"`.

### Exemplo de request

```bash
curl -X POST "{{BASE_URL}}/api/login" \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "admin@exemplo.gov.br",
    "password": "senha_aqui",
    "device_name": "Web"
  }'
```

### Response `200 OK` (exemplo)

```json
{
    "token": "1|xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "user": {
        "id": 1,
        "name": "Admin",
        "email": "admin@exemplo.gov.br",
        "role": "admin"
    }
}
```

### Erros possíveis

-   **`422 Unprocessable Entity`** — validação do request (campos obrigatórios/formato).
-   **`422 Unprocessable Entity`** — credenciais inválidas (retorna mensagem associada ao campo `email`).

Exemplo de erro (credenciais inválidas):

```json
{
    "message": "The given data was invalid.",
    "errors": {
        "email": ["As credenciais fornecidas estão incorretas."]
    }
}
```

---

## POST `/api/logout` (protegido)

Revoga o **token atual** (o token usado no header `Authorization` deixa de funcionar).

### Headers obrigatórios

```http
Authorization: Bearer <TOKEN>
Accept: application/json
```

### Exemplo de request

```bash
curl -X POST "{{BASE_URL}}/api/logout" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer 1|xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```

### Response `200 OK`

```json
{ "message": "Logout realizado com sucesso" }
```

### Erros possíveis

-   **`401 Unauthorized`** — token ausente ou inválido.

---

## GET `/api/me` (protegido)

Retorna o usuário autenticado (útil para o frontend validar sessão/token).

### Exemplo de request

```bash
curl "{{BASE_URL}}/api/me" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer 1|xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```

### Response `200 OK` (exemplo)

> O conteúdo exato depende do seu Model/Resource. No controller atual é retornado diretamente `$request->user()`.

```json
{
    "id": 1,
    "name": "Admin",
    "email": "admin@exemplo.gov.br"
}
```

### Erros possíveis

-   **`401 Unauthorized`** — token ausente ou inválido.

---

## Observações de implementação (baseado no controller)

-   O login procura usuário por `email` e valida senha usando `Hash::check`.
-   A role retornada vem do primeiro item de `getRoleNames()` (Spatie Permission).
-   O logout usa `currentAccessToken()->delete()` para revogar somente o token atual.
