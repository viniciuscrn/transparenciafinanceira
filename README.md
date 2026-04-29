# Projeto de Transparência para Órgãos Públicos — API

API do **Projeto de Transparência**, focada em autenticação (Laravel Sanctum), gestão de usuários e exposição de dados públicos (empenhos, receitas, remessas, etc.).

## Base URL

- `{{BASE_URL}}/api`

> Substitua `{{BASE_URL}}` pela URL do seu ambiente (ex.: `http://localhost:8000`).

## Autenticação

A API usa **Bearer Token** (Laravel Sanctum).

- Header obrigatório nos endpoints protegidos:
  - `Authorization: Bearer <TOKEN>`
  - `Accept: application/json`

---

## Documentação

A documentação detalhada de cada módulo está na pasta [`docs/`](./docs):

| Módulo                    | Arquivo                                                          | Descrição                                               |
| ------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------- |
| Autenticação              | [autenticacao.md](./pages/autenticacao.md)                       | Login, logout e dados do usuário autenticado            |
| Usuários                  | [usuarios.md](./pages/usuarios.md)                               | CRUD de usuários (UserController)                       |
| Remessas                  | [remessa.md](./pages/remessa.md)                                 | Endpoints de remessas                                   |
| Empenhos                  | [empenho.md](./pages/empenho.md)                                 | Endpoints de empenhos                                   |
| Empenhos — Filtros        | [empenhos-filtros.md](./pages/empenhos-filtros.md)               | Referência completa dos filtros do endpoint de empenhos |
| Receitas Previstas        | [receitas_previstas.md](./pages/receitas_previstas.md)           | Endpoints de receitas previstas                         |
| Receitas de Transferência | [receitas_transferencias.md](./pages/receitas_transferencias.md) | Endpoints de receitas de transferência                  |

---

## Endpoints (Resumo)

### Auth

- `POST /login` — Autentica e retorna token + dados do usuário
- `POST /logout` — Revoga o token atual (**protegido**)
- `GET /me` — Retorna o usuário autenticado (**protegido**)

📄 Detalhes em [docs/autenticacao.md](./docs/autenticacao.md).

### Usuários

> Recomendação: proteger com `auth:sanctum` e, quando aplicável, restringir por perfil (ex.: `role:admin`).

- `GET /users` — Lista usuários (paginação)
- `POST /users` — Cria usuário e atribui perfil (role)
- `GET /users/{user}` — Detalha usuário
- `PUT|PATCH /users/{user}` — Atualiza usuário (e role opcional)
- `DELETE /users/{user}` — Remove usuário (bloqueia autoexclusão)

📄 Detalhes em [docs/usuarios.md](./docs/usuarios.md).

---

## Convenções de Resposta

- `200 OK` para operações de leitura/atualização
- `201 Created` para criação
- `204 No Content` para remoção
- Erros de validação: `422 Unprocessable Entity`

## Paginação

Endpoints de listagem (ex.: `GET /users`) retornam paginação padrão do Laravel (`current_page`, `data`, `per_page`, `total`, etc.).

---

## Referência rápida de headers

```http
Accept: application/json
Authorization: Bearer <TOKEN>
Content-Type: application/json
```
