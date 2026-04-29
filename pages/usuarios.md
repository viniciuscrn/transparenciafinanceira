# Usuários — Documentação de API (UserController)

Endpoints para gestão de usuários do sistema.

---

## Sumário

- [Rotas](#rotas)
- [Endpoints](#endpoints)
  - [GET `/api/users`](#get-apiusers-protegido) — Lista usuários
  - [POST `/api/users`](#post-apiusers-protegido) — Cria usuário
  - [GET `/api/users/{user}`](#get-apiusersuser-protegido) — Detalha usuário
  - [PUT/PATCH `/api/users/{user}`](#putpatch-apiusersuser-protegido) — Atualiza usuário
  - [DELETE `/api/users/{user}`](#delete-apiusersuser-protegido) — Remove usuário

---

## Rotas

> Rotas sugeridas (Laravel):
>
> ```php
> Route::middleware('auth:sanctum')->group(function () {
>   Route::apiResource('users', UserController::class);
> });
> ```
>
> _(Opcional/recomendado)_: restringir criação/edição/remoção para perfil `admin`.

---

## Endpoints

### GET `/api/users` (protegido)

Lista usuários com paginação (`paginate(10)`).
A resposta é transformada para expor `role` (string) e remover o objeto `roles` para simplificar o consumo no frontend.

#### Query params

| Parâmetro | Tipo | Obrigatório | Descrição           |
| --------- | ---- | ----------: | ------------------- |
| `page`    | int  |         não | Página da paginação |

#### Exemplo

```bash
curl "{{BASE_URL}}/api/users?page=1" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <TOKEN>"
```

#### Response `200 OK` (exemplo resumido)

```json
{
    "current_page": 1,
    "data": [
        {
            "id": 1,
            "name": "Admin",
            "email": "admin@exemplo.gov.br",
            "role": "admin"
        }
    ],
    "per_page": 10,
    "total": 1
}
```

---

### POST `/api/users` (protegido)

Cria um usuário e atribui um perfil (role).

#### Body (JSON)

| Campo                   | Tipo   | Obrigatório | Regras                        |
| ----------------------- | ------ | ----------- | ----------------------------- |
| `name`                  | string | sim         | `max:255`                     |
| `email`                 | string | sim         | `email`, `unique:users,email` |
| `password`              | string | sim         | `confirmed`, `min:6`          |
| `password_confirmation` | string | sim         | deve ser igual a `password`   |
| `role`                  | string | sim         | `exists:roles,name`           |

#### Exemplo

```bash
curl -X POST "{{BASE_URL}}/api/users" \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <TOKEN>" \
  -d '{
    "name": "Novo Usuário",
    "email": "novo@exemplo.gov.br",
    "password": "123456",
    "password_confirmation": "123456",
    "role": "admin"
  }'
```

#### Response `201 Created` (exemplo)

```json
{
    "id": 2,
    "name": "Novo Usuário",
    "email": "novo@exemplo.gov.br",
    "role": "admin"
}
```

---

### GET `/api/users/{user}` (protegido)

Retorna um usuário específico.
O controller injeta `role` e remove `roles` antes de retornar.

#### Exemplo

```bash
curl "{{BASE_URL}}/api/users/2" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <TOKEN>"
```

#### Response `200 OK` (exemplo)

```json
{
    "id": 2,
    "name": "Novo Usuário",
    "email": "novo@exemplo.gov.br",
    "role": "admin"
}
```

---

### PUT/PATCH `/api/users/{user}` (protegido)

Atualiza dados do usuário. Campos são opcionais (quando enviados, são validados).

#### Body (JSON)

| Campo                   | Tipo   | Obrigatório | Regras                                    |
| ----------------------- | ------ | ----------- | ----------------------------------------- |
| `name`                  | string | não         | `max:255`                                 |
| `email`                 | string | não         | `email`, único (ignora o próprio usuário) |
| `password`              | string | não         | `confirmed`, `min:6` (pode ser `null`)    |
| `password_confirmation` | string | cond.       | se enviar `password`                      |
| `role`                  | string | não         | `exists:roles,name`                       |

> Se `password` for enviada e não estiver vazia, será criptografada com `bcrypt`.

#### Exemplo

```bash
curl -X PATCH "{{BASE_URL}}/api/users/2" \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <TOKEN>" \
  -d '{
    "name": "Usuário Atualizado",
    "role": "admin"
  }'
```

#### Response `200 OK` (exemplo)

```json
{
    "id": 2,
    "name": "Usuário Atualizado",
    "email": "novo@exemplo.gov.br",
    "role": "admin"
}
```

---

### DELETE `/api/users/{user}` (protegido)

Remove um usuário.

- Se o usuário tentar excluir a si mesmo (`$user->id === auth()->id()`), retorna **403**.

#### Exemplo

```bash
curl -X DELETE "{{BASE_URL}}/api/users/2" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <TOKEN>"
```

#### Response `204 No Content`

_(sem corpo)_

#### Erros possíveis

- **`403 Forbidden`** — tentativa de autoexclusão:

```json
{ "message": "Você não pode excluir sua própria conta." }
```

---

## Resumo dos endpoints

- `GET /users` — Lista usuários (paginação)
- `POST /users` — Cria usuário e atribui perfil (role)
- `GET /users/{user}` — Detalha usuário
- `PUT|PATCH /users/{user}` — Atualiza usuário (e role opcional)
- `DELETE /users/{user}` — Remove usuário (bloqueia autoexclusão)
