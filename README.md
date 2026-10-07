# Projeto de Transparência para Órgãos Públicos — API

API do **Projeto de Transparência**: autenticação (Laravel Sanctum), gestão de usuários, importação das remessas SAGRES e exposição de dados públicos (empenhos, liquidações, pagamentos, despesas consolidadas, receitas, fornecedores e diárias).

## Base URL

- `{{BASE_URL}}/api`

> Substitua `{{BASE_URL}}` pela URL do seu ambiente (ex.: `http://localhost:8000`).

## Autenticação

A API usa **Bearer Token** (Laravel Sanctum). Rotas marcadas como **protegidas** exigem:

```http
Authorization: Bearer <TOKEN>
Accept: application/json
```

As consultas de empenhos, a ordem cronológica de pagamentos e as despesas consolidadas são **públicas**.

---

## Documentação

| Módulo                        | Arquivo                                                                | Descrição                                                   |
| ----------------------------- | ---------------------------------------------------------------------- | ----------------------------------------------------------- |
| Autenticação                  | [autenticacao.md](./pages/autenticacao.md)                             | Login, logout e dados do usuário autenticado                |
| Usuários                      | [usuarios.md](./pages/usuarios.md)                                     | CRUD de usuários                                            |
| Remessas                      | [remessa.md](./pages/remessa.md)                                       | Importação e exclusão das remessas mensais SAGRES           |
| Empenhos                      | [empenho.md](./pages/empenho.md)                                       | Listagem com totais, detalhe e busca de fornecedor          |
| Empenhos — Filtros            | [empenhos-filtros.md](./pages/empenhos-filtros.md)                     | Referência completa dos filtros de empenhos                 |
| Ordem cronológica             | [ordem_cronologica_pagamentos.md](./pages/ordem_cronologica_pagamentos.md) | Ordem cronológica de pagamentos                         |
| Despesas consolidadas         | [despesas_consolidadas.md](./pages/despesas_consolidadas.md)           | Totais empenhado / liquidado / pago e saldos                |
| Liquidações e Pagamentos      | [liquidacoes_pagamentos.md](./pages/liquidacoes_pagamentos.md)         | Consulta direta de liquidações e pagamentos                 |
| Diárias                       | [diarias.md](./pages/diarias.md)                                       | Dados complementares de empenhos de diárias                 |
| Fornecedores                  | [fornecedores.md](./pages/fornecedores.md)                             | CRUD e busca de fornecedores (banco local + TCE)            |
| Receitas Previstas            | [receitas_previstas.md](./pages/receitas_previstas.md)                 | Receitas previstas por ano                                  |
| Receitas de Transferência     | [receitas_transferencias.md](./pages/receitas_transferencias.md)       | Receitas de transferência                                   |
| Dicionário de dados           | [dicionario_dados.md](./pages/dicionario_dados.md)                     | Entidades e relacionamentos do layout SAGRES                |
| Guia campo a campo            | [guia_campo_a_campo.md](./pages/guia_campo_a_campo.md)                 | Interpretação dos arquivos TXT do layout                    |

---

## Endpoints (Resumo)

### Públicos

| Método | Endpoint                                       | Descrição                                       |
| ------ | ---------------------------------------------- | ----------------------------------------------- |
| POST   | `/login`                                       | Autentica e retorna token + dados do usuário    |
| GET    | `/empenhos`                                    | Lista empenhos (filtros, totais, paginação)     |
| GET    | `/empenhos/{id}`                               | Detalhe do empenho                              |
| GET    | `/empenhos/ordem-cronologica-pagamentos`       | Ordem cronológica de pagamentos                 |
| GET    | `/empenhos/buscarFornecedorTce/{cpfCnpj}`      | Busca fornecedor (banco local → TCE)            |
| GET    | `/despesas-consolidadas`                       | Totais consolidados da despesa                  |

### Protegidos

| Método           | Endpoint                                   | Descrição                                   |
| ---------------- | ------------------------------------------ | ------------------------------------------- |
| POST             | `/logout`                                  | Revoga o token atual                        |
| GET              | `/me`                                      | Usuário autenticado                         |
| GET/POST         | `/users`                                   | Lista / cria usuários                       |
| GET/PUT/DELETE   | `/users/{user}`                            | Detalha / atualiza / remove usuário         |
| GET/POST         | `/remessas`                                | Lista / importa remessa (ZIP)               |
| GET/DELETE       | `/remessas/{remessa}`                      | Detalha / exclui remessa                    |
| DELETE           | `/remessas/competencia/{YYYY-MM}`          | Exclui remessa pela competência             |
| GET              | `/liquidacoes`, `/liquidacoes/{id}`        | Consulta liquidações                        |
| GET              | `/pagamentos`, `/pagamentos/{id}`          | Consulta pagamentos                         |
| PUT/DELETE       | `/empenhos/{id}/diaria`                    | Cadastra-atualiza / remove dados da diária  |
| CRUD             | `/fornecedores`                            | Gestão de fornecedores                      |
| GET              | `/fornecedores/buscar/{cpfCnpj}`           | Busca fornecedor (banco local → TCE)        |
| GET              | `/receitas-previstas/ultima`               | Receita prevista mais recente               |
| CRUD             | `/receitas-previstas`                      | Receitas previstas                          |
| CRUD             | `/receitas-transferencias`                 | Receitas de transferência                   |

---

## Convenções de Resposta

- `200 OK` para leitura/atualização
- `201 Created` para criação
- `204 No Content` para remoção (quando não há corpo)
- `401 Unauthorized` sem token em rota protegida
- `404 Not Found` para registro inexistente
- `422 Unprocessable Entity` para erros de validação
- Valores monetários vêm como **string decimal** com ponto (ex.: `"180.00"`)

## Paginação

Endpoints de listagem retornam a paginação padrão do Laravel (`current_page`, `data`, `per_page`, `total`, etc.). Parâmetro `per_page` entre 1 e 200 (padrão 20). Empenhos e ordem cronológica aceitam `export=1` para retornar todos os registros sem paginação.

---

## Referência rápida de headers

```http
Accept: application/json
Authorization: Bearer <TOKEN>
Content-Type: application/json
```
