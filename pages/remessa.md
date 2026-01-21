# RemessaController — Documentação Técnica

## Visão Geral

O **RemessaController** é responsável por gerenciar o ciclo de vida das **remessas mensais de dados SAGRES**, incluindo:

- listagem de remessas processadas
- consulta detalhada de uma remessa
- criação de uma nova remessa via upload de arquivo ZIP
- exclusão de remessas (por ID ou competência)

A remessa representa a **unidade mensal de processamento**, sendo identificada unicamente pela **competência (YYYY-MM)**.

Este controller atua como camada de orquestração, delegando a lógica de negócio e processamento de arquivos ao **RemessaService**.

---

## Rotas Associadas

| Método | Rota                                  | Descrição                           |
| ------ | ------------------------------------- | ----------------------------------- |
| GET    | `/api/remessas`                       | Lista todas as remessas             |
| GET    | `/api/remessas/{id}`                  | Detalha uma remessa                 |
| POST   | `/api/remessas`                       | Cria e processa uma nova remessa    |
| DELETE | `/api/remessas/{id}`                  | Exclui uma remessa pelo ID          |
| DELETE | `/api/remessas/competencia/{YYYY-MM}` | Exclui uma remessa pela competência |

---

## Métodos

### GET /api/remessas

Lista todas as remessas cadastradas, ordenadas pela mais recente.

### GET /api/remessas/{id}

Retorna os detalhes completos de uma remessa específica, incluindo logs e resumo do processamento.

### POST /api/remessas

Cria e processa uma nova remessa mensal a partir de um arquivo ZIP.

**Parâmetros (multipart/form-data):**

- `ano` (integer, obrigatório)
- `mes` (integer, obrigatório)
- `arquivo` (file ZIP, obrigatório)

**Códigos HTTP:**

- `201` — Remessa importada com sucesso
- `409` — Remessa já existente para a competência
- `422` — Erro no processamento

### DELETE /api/remessas/{id}

Exclui uma remessa pelo ID, removendo automaticamente os dados vinculados.

### DELETE /api/remessas/competencia/{YYYY-MM}

Exclui uma remessa informando diretamente a competência.

---

## Regras de Negócio

- Apenas uma remessa por competência
- Exclusão em cascata dos dados associados
- Arquivos não são armazenados permanentemente
- Logs e resumos são mantidos para auditoria

---

## Considerações Finais

Este controller é o ponto central do fluxo mensal de ingestão de dados do sistema, garantindo controle, rastreabilidade e segurança no processamento das informações financeiras.

Documento gerado para fins de documentação técnica e apoio ao desenvolvimento.
