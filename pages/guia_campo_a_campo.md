# GUIA COMPLETO DE INTERPRETAÇÃO DOS ARQUIVOS TXT (CAMPO A CAMPO)

## Layout dos Arquivos – Versão 1.0 – Exercício 2025

**Data de geração:** 2025-12-16

Este documento foi gerado a partir da aba **“Layouts”** do arquivo oficial enviado e descreve, para **cada entidade**, como interpretar o registro TXT de largura fixa: **posição inicial/final**, **tipo**, **tamanho**, **obrigatoriedade**, **regras/origem** e **exemplo de leitura**.

---

## Regras gerais de leitura (TXT de largura fixa)

- Cada linha do arquivo representa **um registro** da entidade.
- Não existem separadores de campo (vírgula, ponto-e-vírgula, TAB).
- As posições são **base 1** (primeiro caractere é posição 1).
- O tamanho do campo é calculado por: `Tam = P.Final - P.Inicial + 1`.
- Campos **numéricos**: preencher com zeros à esquerda (quando aplicável).
- Campos **alfanuméricos**: preencher com espaços à direita (quando aplicável).
- **Exemplos** neste documento são ilustrativos (placeholders) e servem para demonstrar o fatiamento por posição.

---

## Sumário de entidades

- [1. Orçamento](#or-amento)
- [2. UnidadeOrcamentaria](#unidadeorcamentaria)
- [3. Programas](#programas)
- [4. Acao](#acao)
- [5. Dotacao](#dotacao)
- [6. CadastroContas](#cadastrocontas)
- [7. PrevisaoReceita](#previsaoreceita)
- [8. AtualizacaoOrcamentaria](#atualizacaoorcamentaria)
- [9. NormaAtualizacao](#normaatualizacao)
- [10. Empenhos](#empenhos)
- [11. EmpenhoEstorno](#empenhoestorno)
- [12. EmpenhoReforco](#empenhoreforco)
- [13. Liquidação](#liquida-o)
- [14. Pagamentos](#pagamentos)
- [15. ItemPagamento](#itempagamento)
- [16. Retenção](#reten-o)
- [17. ReceitaOrcamentaria](#receitaorcamentaria)
- [18. ReceitaExtra](#receitaextra)
- [19. DespesaExtra](#despesaextra)
- [20. RestosInscritos](#restosinscritos)
- [21. EstornoRestos](#estornorestos)
- [22. PagamentosRestos](#pagamentosrestos)
- [23. ItemPagamentosRestos](#itempagamentosrestos)
- [24. RetencaoRestos](#retencaorestos)
- [25. ConciliacaoBancaria](#conciliacaobancaria)
- [26. SaldoInicial](#saldoinicial)
- [27. SaldoMensal](#saldomensal)
- [28. Fornecedores](#fornecedores)
- [29. PagamentoEstorno](#pagamentoestorno)
- [30. LiquidacaoEstorno](#liquidacaoestorno)
- [31. PagamentoRestoEstorno](#pagamentorestoestorno)
- [32. AgentePolitico](#agentepolitico)
- [33. Ordenador](#ordenador)
- [34. Gestor](#gestor)
- [35. TecnicoResponsavel](#tecnicoresponsavel)
- [36. TransferenciaRecebida](#transferenciarecebida)
- [37. TransferenciaConcedida](#transferenciaconcedida)
- [38. LiquidacaoRestos](#liquidacaorestos)
- [39. LiquidacaoRestosEstorno](#liquidacaorestosestorno)
- [40. ContribuicaoPrevidenciariaPatronal](#contribuicaoprevidenciariapatronal)
- [41. ContribuicaoPrevidenciariaSegurado](#contribuicaoprevidenciariasegurado)

---

## 1. Orçamento

**Periodicidade:** Periodicidade: Anual
**Situação:** Situação: Excluído nesta versão
**Explicação funcional:** Esta tabela deverá conter as informações da LOA, LDO e PPA para o exercício. As alterações na LOA deverão ser enviadas ao TCE através do controle da tabela AtualizacaoOrcamentaria.
**Tamanho do registro (linha):** 61 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                               | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem |
| ----------------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ------------------- |
| Ano de vigência da lei orçamentária | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)   | AAAA                |
| Data de aprovação da LOA            | Não   | Sim    |        11 |      18 |   8 | Data (8)       | DDMMAAAA            |
| Número da lei orçamentária          | Não   | Sim    |        19 |      27 |   9 | Numérico (9)   | NNNNNAAAA           |
| Data de aprovação da LDO            | Não   | Sim    |        28 |      35 |   8 | Data (8)       | DDMMAAAA            |
| Número da LDO                       | Não   | Sim    |        36 |      44 |   9 | Numérico (9)   | NNNNNAAAA           |
| Data de aprovação do PPA            | Não   | Sim    |        45 |      52 |   8 | Data (8)       | DDMMAAAA            |
| Número do PPA                       | Não   | Sim    |        53 |      61 |   9 | Numérico (9)   | NNNNNAAAA           |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      0001202501010000000012025010100000000120250101000000001
```

Fatiamento por posição (exemplo):

- **Ano de vigência da lei orçamentária**: posições **7–10** (`linha[7:10]` em base 1).
- **Data de aprovação da LOA**: posições **11–18** (`linha[11:18]` em base 1).
- **Número da lei orçamentária**: posições **19–27** (`linha[19:27]` em base 1).
- **Data de aprovação da LDO**: posições **28–35** (`linha[28:35]` em base 1).
- **Número da LDO**: posições **36–44** (`linha[36:44]` em base 1).
- **Data de aprovação do PPA**: posições **45–52** (`linha[45:52]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 2. UnidadeOrcamentaria

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter a relação das unidades orçamentárias constantes na estrutura institucional da unidade jurisdicionada.
**Tamanho do registro (linha):** 72 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                                             | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem    |
| ------------------------------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ---------------------- |
| Código da unidade orçamentária utilizada pela LOA | Sim   | Sim    |         7 |      16 |  10 | Numérico (10)  | -                      |
| Denominação da unidade orçamentária               | Não   | Sim    |        17 |      66 |  50 | Caractere (50) | -                      |
| Número da unidade jurisdicionada                  | Não   | Sim    |        67 |      72 |   6 | Numérico (6)   | Número da UJ no TCE-PE |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      0000000001XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX000001
```

Fatiamento por posição (exemplo):

- **Código da unidade orçamentária utilizada pela LOA**: posições **7–16** (`linha[7:16]` em base 1).
- **Denominação da unidade orçamentária**: posições **17–66** (`linha[17:66]` em base 1).
- **Número da unidade jurisdicionada**: posições **67–72** (`linha[67:72]` em base 1).

---

## 3. Programas

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter informações sobre os programas presentes no orçamento ou nas alterações orçamentárias
**Tamanho do registro (linha):** 230 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                                 | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho  | Observação / Origem |
| ------------------------------------- | ----- | ------ | --------: | ------: | --: | --------------- | ------------------- |
| Código do programa utilizado pela LOA | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)    | -                   |
| Denominação do programa               | Não   | Sim    |        11 |      80 |  70 | Caractere (70)  | -                   |
| Descrição do objetivo do programa     | Não   | Sim    |        81 |     230 | 150 | Caractere (150) | -                   |

### Exemplo de leitura (ilustrativo)

Exemplo de linha (recorte):

```txt
      0001XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX ... XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

> Observação: a linha completa possui 230 caracteres; acima é exibido um recorte para facilitar leitura.

Fatiamento por posição (exemplo):

- **Código do programa utilizado pela LOA**: posições **7–10** (`linha[7:10]` em base 1).
- **Denominação do programa**: posições **11–80** (`linha[11:80]` em base 1).
- **Descrição do objetivo do programa**: posições **81–230** (`linha[81:230]` em base 1).

---

## 4. Acao

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter informações sobre as ações (Projetos, Atividades e Op. Especiais) presentes no orçamento ou nas alterações orçamentárias.
**Tamanho do registro (linha):** 83 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                             | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem |
| --------------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ------------------- |
| Código da ação utilizado pela LOA | Sim   | Sim    |         7 |      12 |   6 | Numérico (6)   | -                   |
| Denominação da ação               | Não   | Sim    |        13 |      82 |  70 | Caractere (70) | -                   |
| Identificação da ação             | Sim   | Sim    |        83 |      83 |   1 | Numérico (1)   | Tabela Interna 12   |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      000001XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX1
```

Fatiamento por posição (exemplo):

- **Código da ação utilizado pela LOA**: posições **7–12** (`linha[7:12]` em base 1).
- **Denominação da ação**: posições **13–82** (`linha[13:82]` em base 1).
- **Identificação da ação**: posições **83–83** (`linha[83:83]` em base 1).

---

## 5. Dotacao

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter informações das dotações/previsões de despesas para o exercício em nível de unidade orçamentária. As dotações deverão ser detalhadas até elemento de despesa.
**Tamanho do registro (linha):** 72 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                                  | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem |
| -------------------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ------------------- |
| Ano de vigência da lei orçamentária    | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)   | AAAA                |
| Código da unidade orçamentária         | Sim   | Sim    |        11 |      20 |  10 | Numérico (10)  | UnidadeOrcamentaria |
| Código da função                       | Sim   | Sim    |        21 |      22 |   2 | Numérico (2)   | Tabela Interna 05   |
| Código da subfunção                    | Sim   | Sim    |        23 |      25 |   3 | Numérico (3)   | Tabela Interna 11   |
| Código do programa                     | Sim   | Sim    |        26 |      29 |   4 | Numérico (4)   | Programas           |
| Código da ação                         | Sim   | Sim    |        30 |      35 |   6 | Numérico (6)   | Acao                |
| Identificação da ação                  | Sim   | Sim    |        36 |      36 |   1 | Numérico (1)   | Tabela Interna 12   |
| Código da fonte de recurso             | Sim   | Sim    |        37 |      44 |   8 | Numérico (8)   | Tabela Interna 29   |
| Código da categoria econômica          | Sim   | Sim    |        51 |      51 |   1 | Numérico (1)   | Tabela Interna 02   |
| Código do grupo da natureza de despesa | Sim   | Sim    |        52 |      52 |   1 | Numérico (1)   | Tabela Interna 07   |
| Código da modalidade de aplicação      | Sim   | Sim    |        53 |      54 |   2 | Numérico (2)   | Tabela Interna 06   |
| Código do elemento de despesa          | Sim   | Sim    |        55 |      56 |   2 | Numérico (2)   | Tabela Interna 04   |
| Valor fixado na lei orçamentária       | Não   | Sim    |        57 |      72 |  16 | Valor (16)     | -                   |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      00010000000001010010001000001100000001      1101010000000000000000
```

Fatiamento por posição (exemplo):

- **Ano de vigência da lei orçamentária**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da unidade orçamentária**: posições **11–20** (`linha[11:20]` em base 1).
- **Código da função**: posições **21–22** (`linha[21:22]` em base 1).
- **Código da subfunção**: posições **23–25** (`linha[23:25]` em base 1).
- **Código do programa**: posições **26–29** (`linha[26:29]` em base 1).
- **Código da ação**: posições **30–35** (`linha[30:35]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 6. CadastroContas

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter todas as contas bancárias utilizadas pela unidade jurisdicionada.
**Tamanho do registro (linha):** 94 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                                                | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem    |
| ---------------------------------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ---------------------- |
| Número da conta bancária                             | Sim   | Sim    |         7 |      18 |  12 | Caractere (12) | -                      |
| Código do banco (FEBRABAN)                           | Não   | Sim    |        19 |      21 |   3 | Numérico (3)   | Tabela Interna 01      |
| Código da agência                                    | Não   | Sim    |        22 |      27 |   6 | Caractere (6)  | -                      |
| Título da conta, que deverá identificar a sua origem | Não   | Sim    |        28 |      87 |  60 | Caractere (60) | -                      |
| Tipo da conta bancária                               | Sim   | Sim    |        88 |      88 |   1 | Numérico (1)   | Tabela interna 28      |
| Número da unidade jurisdicionada                     | Não   | Sim    |        89 |      94 |   6 | Numérico (6)   | Número da UJ no TCE-PE |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      XXXXXXXXXXXX001XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX1000001
```

Fatiamento por posição (exemplo):

- **Número da conta bancária**: posições **7–18** (`linha[7:18]` em base 1).
- **Código do banco (FEBRABAN)**: posições **19–21** (`linha[19:21]` em base 1).
- **Código da agência**: posições **22–27** (`linha[22:27]` em base 1).
- **Título da conta, que deverá identificar a sua origem**: posições **28–87** (`linha[28:87]` em base 1).
- **Tipo da conta bancária**: posições **88–88** (`linha[88:88]` em base 1).
- **Número da unidade jurisdicionada**: posições **89–94** (`linha[89:94]` em base 1).

---

## 7. PrevisaoReceita

**Periodicidade:** Periodicidade: Anual
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter os valores da receita orçamentária para o exercício, previstas na lei orçamentária anual.
**Tamanho do registro (linha):** 43 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                                   | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem                                              |
| --------------------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ---------------------------------------------------------------- |
| Ano da lei orçamentária                 | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)   | AAAA                                                             |
| Código da receita orçamentária prevista | Sim   | Sim    |        11 |      21 |  11 | Número (11)    | Seguir ementário e orientações da Secretaria do Tesouro Nacional |
| Valor previsto do orçamento             | Não   | Sim    |        22 |      37 |  16 | Valor (16)     | -                                                                |
| Número da unidade jurisdicionada        | Sim   | Sim    |        38 |      43 |   6 | Numérico (6)   | Número da UJ no TCE-PE                                           |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      0001000000000010000000000000000000001
```

Fatiamento por posição (exemplo):

- **Ano da lei orçamentária**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da receita orçamentária prevista**: posições **11–21** (`linha[11:21]` em base 1).
- **Valor previsto do orçamento**: posições **22–37** (`linha[22:37]` em base 1).
- **Número da unidade jurisdicionada**: posições **38–43** (`linha[38:43]` em base 1).

---

## 8. AtualizacaoOrcamentaria

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter as alterações da LOA ocorridas após o envio do orçamento.
**Tamanho do registro (linha):** 84 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                                  | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem |
| -------------------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ------------------- |
| Ano da atualização orçamentária        | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)   | AAAA                |
| Código da unidade orçamentária         | Sim   | Sim    |        11 |      20 |  10 | Numérico (10)  | UnidadeOrcamentaria |
| Código da função                       | Sim   | Sim    |        21 |      22 |   2 | Numérico (2)   | Tabela Interna 05   |
| Código da subfunção                    | Sim   | Sim    |        23 |      25 |   3 | Numérico (3)   | Tabela Interna 11   |
| Código do programa                     | Sim   | Sim    |        26 |      29 |   4 | Numérico (4)   | Programa            |
| Código da ação                         | Sim   | Sim    |        30 |      35 |   6 | Numérico (6)   | Acao                |
| Identificação da ação                  | Sim   | Sim    |        36 |      36 |   1 | Numérico (1)   | Tabela Interna 12   |
| Código da fonte de recurso             | Sim   | Sim    |        37 |      44 |   8 | Numérico (8)   | Tabela Interna 29   |
| Tipo da norma de atualização           | Sim   | Sim    |        51 |      51 |   1 | Numérico (1)   | Tabela interna 31   |
| Nº da norma de abertura do crédito     | Sim   | Sim    |        52 |      60 |   9 | Numérico (9)   | -                   |
| Tipo de alteração orçamentária         | Sim   | Sim    |        61 |      62 |   2 | Numérico (2)   | Tabela Interna 13   |
| Código da categoria econômica          | Sim   | Sim    |        63 |      63 |   1 | Numérico (1)   | Tabela Interna 02   |
| Código do grupo da natureza de despesa | Sim   | Sim    |        64 |      64 |   1 | Numérico (1)   | Tabela Interna 07   |
| Código da modalidade de aplicação      | Sim   | Sim    |        65 |      66 |   2 | Numérico (2)   | Tabela Interna 06   |
| Código do elemento de despesa          | Sim   | Sim    |        67 |      68 |   2 | Numérico (2)   | Tabela Interna 04   |
| Valor autorizado                       | Não   | Sim    |        69 |      84 |  16 | Valor (16)     | -                   |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      00010000000001010010001000001100000001      1000000001011101010000000000000000
```

Fatiamento por posição (exemplo):

- **Ano da atualização orçamentária**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da unidade orçamentária**: posições **11–20** (`linha[11:20]` em base 1).
- **Código da função**: posições **21–22** (`linha[21:22]` em base 1).
- **Código da subfunção**: posições **23–25** (`linha[23:25]` em base 1).
- **Código do programa**: posições **26–29** (`linha[26:29]` em base 1).
- **Código da ação**: posições **30–35** (`linha[30:35]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 9. NormaAtualizacao

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Excluído nesta versão
**Explicação funcional:** Esta tabela deverá conter informações das normas de atualização orçamentária, decretos ou não, utilizados na tabela de atualização orçamentária.
**Tamanho do registro (linha):** 37 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                        | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem |
| ---------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ------------------- |
| Ano                          | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)   | AAAA                |
| Número                       | Sim   | Sim    |        11 |      19 |   9 | Numérico (9)   | NNNNNAAAA           |
| Número da lei que autorizou  | Não   | Sim    |        20 |      28 |   9 | Numérico (9)   | NNNNNAAAA           |
| Data                         | Não   | Sim    |        29 |      36 |   8 | Data (8)       | DDMMAAAA            |
| Tipo da norma de atualização | Sim   | Sim    |        37 |      37 |   1 | Numérico (1)   | Tabela interna 31   |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      0001000000001000000001202501011
```

Fatiamento por posição (exemplo):

- **Ano**: posições **7–10** (`linha[7:10]` em base 1).
- **Número**: posições **11–19** (`linha[11:19]` em base 1).
- **Número da lei que autorizou**: posições **20–28** (`linha[20:28]` em base 1).
- **Data**: posições **29–36** (`linha[29:36]` em base 1).
- **Tipo da norma de atualização**: posições **37–37** (`linha[37:37]` em base 1).

---

## 10. Empenhos

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter informações dos empenhos realizados no mês.
**Tamanho do registro (linha):** 639 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                                       | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho  | Observação / Origem                                         |
| ------------------------------------------- | ----- | ------ | --------: | ------: | --: | --------------- | ----------------------------------------------------------- |
| Ano de emissão do empenho                   | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)    | AAAA                                                        |
| Código da unidade orçamentária              | Sim   | Sim    |        11 |      20 |  10 | Numérico (10)   | UnidadeOrcamentaria                                         |
| Código da função                            | Não   | Sim    |        21 |      22 |   2 | Numérico (2)    | Tabela Interna 05                                           |
| Código da subfunção                         | Não   | Sim    |        23 |      25 |   3 | Numérico (3)    | Tabela Interna 11                                           |
| Código do programa                          | Não   | Sim    |        26 |      29 |   4 | Numérico (4)    | Programa                                                    |
| Código da ação                              | Não   | Sim    |        30 |      35 |   6 | Numérico (6)    | Ação                                                        |
| Identificação da ação                       | Não   | Sim    |        36 |      36 |   1 | Numérico (1)    | Tabela Interna 12                                           |
| Código da categoria econômica               | Não   | Sim    |        43 |      43 |   1 | Numérico (1)    | Tabela Interna 02                                           |
| Código do grupo da natureza de despesa      | Não   | Sim    |        44 |      44 |   1 | Numérico (1)    | Tabela Interna 07                                           |
| Código da modalidade de aplicação           | Não   | Sim    |        45 |      46 |   2 | Numérico (2)    | Tabela Interna 06                                           |
| Código do elemento de despesa da dotação \* | Não   | Sim    |        47 |      48 |   2 | Numérico (2)    | Tabela Interna 04                                           |
| Código do subelemento de despesa            | Não   | Sim    |        49 |      51 |   3 | Numérico (3)    | Tabela Interna 10 – Usar 999 quando não possuir subElemento |
| Modalidade de licitação                     | Não   | Sim    |        52 |      53 |   2 | Numérico (2)    | Tabela Interna 20                                           |
| Número do empenho                           | Sim   | Sim    |        54 |      60 |   7 | Numérico (7)    | -                                                           |
| Tipo de empenho                             | Não   | Sim    |        61 |      61 |   1 | Numérico (1)    | Tabela Interna 18                                           |
| Data de emissão do empenho                  | Não   | Sim    |        62 |      69 |   8 | Data (8)        | DDMMAAAA                                                    |
| Valor empenhado                             | Não   | Sim    |        70 |      85 |  16 | Valor (16)      | -                                                           |
| Histórico do empenho                        | Não   | Sim    |        86 |     595 | 510 | Caractere (510) | -                                                           |
| CPF / CNPJ do credor                        | Não   | Sim    |       596 |     609 |  14 | Numérico (14)   | Informar CPF ou CNPJ do fornecedor                          |
| Nº do procedimento licitação ref. à despesa | Não   | Não    |       610 |     618 |   9 | Caractere (9)   | NNNNNAAAA                                                   |
| Código da fonte de recurso                  | Não   | SIM    |       619 |     626 |   8 | Numérico (8)    | Tabela Interna 29                                           |
| CPF do ordenador de despesa                 | Não   | Sim    |       627 |     637 |  11 | Numérico (11)   | Ordenador                                                   |
| Código do elemento de despesa do empenho \* | Não   | Sim    |       638 |     639 |   2 | Numérico (2)    | Tabela interna 04                                           |

### Exemplo de leitura (ilustrativo)

Exemplo de linha (recorte):

```txt
      000100000000010100100010000011      1101010010100000011202501010000000000000000XXXXXXXXXXXXXXXXXXXXXXXXX ... XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX00000000000001XXXXXXXXX000000010000000000101
```

> Observação: a linha completa possui 639 caracteres; acima é exibido um recorte para facilitar leitura.

Fatiamento por posição (exemplo):

- **Ano de emissão do empenho**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da unidade orçamentária**: posições **11–20** (`linha[11:20]` em base 1).
- **Código da função**: posições **21–22** (`linha[21:22]` em base 1).
- **Código da subfunção**: posições **23–25** (`linha[23:25]` em base 1).
- **Código do programa**: posições **26–29** (`linha[26:29]` em base 1).
- **Código da ação**: posições **30–35** (`linha[30:35]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 11. EmpenhoEstorno

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter informações das anulações dos empenhos realizadas no mês.
**Tamanho do registro (linha):** 208 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                          | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho  | Observação / Origem |
| ------------------------------ | ----- | ------ | --------: | ------: | --: | --------------- | ------------------- |
| Ano de emissão do empenho      | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)    | AAAA                |
| Código da unidade orçamentária | Sim   | Sim    |        11 |      20 |  10 | Numérico (10)   | UnidadeOrcamentaria |
| Número do empenho              | Sim   | Sim    |        21 |      27 |   7 | Numérico (7)    | Empenhos            |
| Número do estorno              | Sim   | Sim    |        28 |      34 |   7 | Numérico (7)    | -                   |
| Data do estorno                | Sim   | Sim    |        35 |      42 |   8 | Data (8)        | DDMMAAAA            |
| Valor estornado                | Não   | Sim    |        43 |      58 |  16 | Valor (16)      | -                   |
| Histórico do estorno           | Não   | Sim    |        59 |     208 | 150 | Caractere (150) | -                   |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      0001000000000100000010000001202501010000000000000000XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

Fatiamento por posição (exemplo):

- **Ano de emissão do empenho**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da unidade orçamentária**: posições **11–20** (`linha[11:20]` em base 1).
- **Número do empenho**: posições **21–27** (`linha[21:27]` em base 1).
- **Número do estorno**: posições **28–34** (`linha[28:34]` em base 1).
- **Data do estorno**: posições **35–42** (`linha[35:42]` em base 1).
- **Valor estornado**: posições **43–58** (`linha[43:58]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 12. EmpenhoReforco

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter informações dos reforços dos empenhos realizados no mês.
**Tamanho do registro (linha):** 208 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                          | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho  | Observação / Origem |
| ------------------------------ | ----- | ------ | --------: | ------: | --: | --------------- | ------------------- |
| Ano de emissão do empenho      | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)    | AAAA                |
| Código da unidade orçamentária | Sim   | Sim    |        11 |      20 |  10 | Numérico (10)   | UnidadeOrcamentaria |
| Número do empenho              | Sim   | Sim    |        21 |      27 |   7 | Numérico (7)    | Empenhos            |
| Número do reforço              | Sim   | Sim    |        28 |      34 |   7 | Numérico (7)    | -                   |
| Data do reforço                | Sim   | Sim    |        35 |      42 |   8 | Data (8)        | DDMMAAAA            |
| Valor reforçado                | Não   | Sim    |        43 |      58 |  16 | Valor (16)      | -                   |
| Histórico do reforço           | Não   | Sim    |        59 |     208 | 150 | Caractere (150) | -                   |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      0001000000000100000010000001202501010000000000000000XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

Fatiamento por posição (exemplo):

- **Ano de emissão do empenho**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da unidade orçamentária**: posições **11–20** (`linha[11:20]` em base 1).
- **Número do empenho**: posições **21–27** (`linha[21:27]` em base 1).
- **Número do reforço**: posições **28–34** (`linha[28:34]` em base 1).
- **Data do reforço**: posições **35–42** (`linha[35:42]` em base 1).
- **Valor reforçado**: posições **43–58** (`linha[43:58]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 13. Liquidação

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter informações das liquidações ocorridas no mês, referentes a empenhos emitidos no exercício.
**Tamanho do registro (linha):** 621 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                               | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho  | Observação / Origem                                                                      |
| ----------------------------------- | ----- | ------ | --------: | ------: | --: | --------------- | ---------------------------------------------------------------------------------------- |
| Ano do empenho                      | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)    | AAAA                                                                                     |
| Código da unidade orçamentária      | Sim   | Sim    |        11 |      20 |  10 | Numérico (10)   | UnidadeOrcamentaria                                                                      |
| Número do empenho                   | Sim   | Sim    |        21 |      27 |   7 | Numérico (7)    | Empenhos                                                                                 |
| Número da liquidação                | Sim   | Sim    |        28 |      34 |   7 | Numérico (7)    | -                                                                                        |
| Data da liquidação                  | Sim   | Sim    |        35 |      42 |   8 | Data (8)        | DDMMAAAA                                                                                 |
| Valor da liquidação                 | Não   | Sim    |        43 |      58 |  16 | Valor (16)      | -                                                                                        |
| Tipo de documento                   | Não   | Sim    |        59 |      59 |   1 | Numérico(1)     | 1 - Nota fiscal eletrônica (ICMS); 2 - Nota fiscal eletrônica serviços (ISS); 9 - Outros |
| Número da chave de acesso da NFE \* | Não   | Não    |        60 |     103 |  44 | Numérico (44)   | Somente para tipo de documento 1                                                         |
| Histórico da liquidação             | Não   | Sim    |       104 |     613 | 510 | Caractere (510) | -                                                                                        |
| Código da fonte de recurso          | Não   | Sim    |       614 |     621 |   8 | Numérico (8)    | Tabela Interna 29                                                                        |

### Exemplo de leitura (ilustrativo)

Exemplo de linha (recorte):

```txt
      0001000000000100000010000001202501010000000000000000100000000000000000000000000000000000000000001XXXXXXX ... XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX00000001
```

> Observação: a linha completa possui 621 caracteres; acima é exibido um recorte para facilitar leitura.

Fatiamento por posição (exemplo):

- **Ano do empenho**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da unidade orçamentária**: posições **11–20** (`linha[11:20]` em base 1).
- **Número do empenho**: posições **21–27** (`linha[21:27]` em base 1).
- **Número da liquidação**: posições **28–34** (`linha[28:34]` em base 1).
- **Data da liquidação**: posições **35–42** (`linha[35:42]` em base 1).
- **Valor da liquidação**: posições **43–58** (`linha[43:58]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 14. Pagamentos

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter informações dos pagamentos ocorridos no mês, referentes a empenhos emitidos no exercício.
**Tamanho do registro (linha):** 42 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                          | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem |
| ------------------------------ | ----- | ------ | --------: | ------: | --: | -------------- | ------------------- |
| Ano do empenho                 | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)   | AAAA                |
| Código da unidade orçamentária | Sim   | Sim    |        11 |      20 |  10 | Numérico (10)  | UnidadeOrcamentaria |
| Número do empenho a ser pago   | Sim   | Sim    |        21 |      27 |   7 | Numérico (7)   | Empenhos            |
| Número da parcela do pagamento | Sim   | Sim    |        28 |      34 |   7 | Numérico (7)   | -                   |
| Data do pagamento              | Não   | Sim    |        35 |      42 |   8 | Data (8)       | DDMMAAAA            |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      000100000000010000001000000120250101
```

Fatiamento por posição (exemplo):

- **Ano do empenho**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da unidade orçamentária**: posições **11–20** (`linha[11:20]` em base 1).
- **Número do empenho a ser pago**: posições **21–27** (`linha[21:27]` em base 1).
- **Número da parcela do pagamento**: posições **28–34** (`linha[28:34]` em base 1).
- **Data do pagamento**: posições **35–42** (`linha[35:42]` em base 1).

---

## 15. ItemPagamento

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter informações de cada parte que compõem os pagamentos ocorridos no mês.
**Tamanho do registro (linha):** 195 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                                            | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem |
| ------------------------------------------------ | ----- | ------ | --------: | ------: | --: | -------------- | ------------------- |
| Ano do empenho                                   | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)   | AAAA                |
| Código da unidade orçamentária                   | Sim   | Sim    |        11 |      20 |  10 | Numérico (10)  | UnidadeOrcamentaria |
| Número do empenho a ser pago                     | Sim   | Sim    |        21 |      27 |   7 | Numérico (7)   | Empenhos            |
| Número da parcela do pagamento                   | Sim   | Sim    |        28 |      34 |   7 | Numérico (7)   | Pagamentos          |
| Valor do pagamento                               | Não   | Sim    |        35 |      50 |  16 | Valor (16)     | -                   |
| Nº da conta bancária de débito                   | Não   | Sim    |        51 |      62 |  12 | Caractere (12) | CadastroContas      |
| Nº do cheque emitido                             | Não   | Não    |        63 |      68 |   6 | Caractere (6)  | -                   |
| Nº do doc. de débito automático                  | Não   | Não    |        69 |      79 |  11 | Caractere (11) | -                   |
| Código do banco para crédito em conta (FEBRABAN) | Não   | Não    |        80 |      82 |   3 | Caractere (3)  | Tabela interna 01   |
| Código da agência para crédito em conta          | Não   | Não    |        83 |      88 |   6 | Caractere (6)  | -                   |
| Nº da conta bancária para crédito                | Não   | Não    |        89 |     100 |  12 | Caractere (12) | -                   |
| Código da fonte de recurso                       | Não   | Sim    |       101 |     108 |   8 | Numérico (8)   | Tabela Interna 29   |
| Tipo da conta bancária de débito                 | Não   | Sim    |       109 |     109 |   1 | Numérico (1)   | Tabela interna 28   |
| Número sequencial do pagamento \*                | Sim   | Sim    |       110 |     116 |   7 | Numérico (7)   | -                   |
| Tipo do pagamento                                | Não   | Sim    |       117 |     118 |   2 | Numérico (2)   | Tabela interna 35   |
| Chave PIX                                        | Não   | Não    |       119 |     195 |  77 | Caractere (77) | -                   |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      00010000000001000000100000010000000000000000XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX000000011000000101XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

Fatiamento por posição (exemplo):

- **Ano do empenho**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da unidade orçamentária**: posições **11–20** (`linha[11:20]` em base 1).
- **Número do empenho a ser pago**: posições **21–27** (`linha[21:27]` em base 1).
- **Número da parcela do pagamento**: posições **28–34** (`linha[28:34]` em base 1).
- **Valor do pagamento**: posições **35–50** (`linha[35:50]` em base 1).
- **Nº da conta bancária de débito**: posições **51–62** (`linha[51:62]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 16. Retenção

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter informações dos valores retidos quando da realização dos pagamentos referentes a empenhos emitidos no exercício.
**Tamanho do registro (linha):** 53 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                          | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem                          |
| ------------------------------ | ----- | ------ | --------: | ------: | --: | -------------- | -------------------------------------------- |
| Ano do empenho                 | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)   | AAAA                                         |
| Código da unidade orçamentária | Sim   | Sim    |        11 |      20 |  10 | Numérico (10)  | UnidadeOrcamentaria                          |
| Número do empenho de origem    | Sim   | Sim    |        21 |      27 |   7 | Numérico (7)   | Empenhos                                     |
| Número da parcela do pagamento | Sim   | Sim    |        28 |      34 |   7 | Numérico (7)   | Pagamentos                                   |
| Valor da retenção              | Não   | Sim    |        35 |      50 |  16 | Valor (16)     | -                                            |
| Tipo de retenção efetuada      | Sim   | Sim    |        51 |      52 |   2 | Numérico (2)   | Tabela Interna 26                            |
| Tipo de lançamento             | Sim   | Sim    |        53 |      53 |   1 | Numérico (1)   | 1-Registro de Retenção 2-Estorno de Retenção |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      00010000000001000000100000010000000000000000011
```

Fatiamento por posição (exemplo):

- **Ano do empenho**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da unidade orçamentária**: posições **11–20** (`linha[11:20]` em base 1).
- **Número do empenho de origem**: posições **21–27** (`linha[21:27]` em base 1).
- **Número da parcela do pagamento**: posições **28–34** (`linha[28:34]` em base 1).
- **Valor da retenção**: posições **35–50** (`linha[35:50]` em base 1).
- **Tipo de retenção efetuada**: posições **51–52** (`linha[51:52]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 17. ReceitaOrcamentaria

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter informações dos valores (soma mensal) da receita orçamentária registrada no mês.
**Tamanho do registro (linha):** 48 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                            | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem                                              |
| -------------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ---------------------------------------------------------------- |
| Código da receita orçamentária   | Sim   | Sim    |         7 |      17 |  11 | Número (11)    | Seguir ementário e orientações da Secretaria do Tesouro Nacional |
| Tipo de registro                 | Sim   | Sim    |        18 |      18 |   1 | Numérico (1)   | Tabela Interna 24                                                |
| Valor mensal lançado             | Não   | Sim    |        19 |      34 |  16 | Valor (16)     | -                                                                |
| Número da unidade jurisdicionada | Sim   | Sim    |        35 |      40 |   6 | Numérico (6)   | Número da UJ no TCE-PE                                           |
| Código da fonte de recurso       | Sim   | Sim    |        41 |      48 |   8 | Numérico (8)   | Tabela Interna 29                                                |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      000000000011000000000000000000000100000001
```

Fatiamento por posição (exemplo):

- **Código da receita orçamentária**: posições **7–17** (`linha[7:17]` em base 1).
- **Tipo de registro**: posições **18–18** (`linha[18:18]` em base 1).
- **Valor mensal lançado**: posições **19–34** (`linha[19:34]` em base 1).
- **Número da unidade jurisdicionada**: posições **35–40** (`linha[35:40]` em base 1).
- **Código da fonte de recurso**: posições **41–48** (`linha[41:48]` em base 1).

---

## 18. ReceitaExtra

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Excluído nesta versão
**Explicação funcional:** Esta tabela deverá conter informações dos valores (soma mensal) da receita extra–orçamentária registrada no mês.
**Tamanho do registro (linha):** 37 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                            | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem    |
| -------------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ---------------------- |
| Tipo de lançamento               | Sim   | Sim    |         7 |       7 |   1 | Numérico (1)   | Tabela Interna 24      |
| Valor mensal lançado             | Não   | Sim    |         8 |      23 |  16 | Valor (16)     | -                      |
| Número da unidade jurisdicionada | Sim   | Sim    |        24 |      29 |   6 | Numérico (6)   | Número da UJ no TCE-PE |
| Código de receita extra padrão   | Sim   | Sim    |        30 |      37 |   8 | Numérico (8)   | Tabela Interna 08      |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      1000000000000000000000100000001
```

Fatiamento por posição (exemplo):

- **Tipo de lançamento**: posições **7–7** (`linha[7:7]` em base 1).
- **Valor mensal lançado**: posições **8–23** (`linha[8:23]` em base 1).
- **Número da unidade jurisdicionada**: posições **24–29** (`linha[24:29]` em base 1).
- **Código de receita extra padrão**: posições **30–37** (`linha[30:37]` em base 1).

---

## 19. DespesaExtra

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Excluído nesta versão
**Explicação funcional:** Esta tabela deverá conter informações dos valores de despesa extra–orçamentária registrada no mês.
**Tamanho do registro (linha):** 37 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                            | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem    |
| -------------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ---------------------- |
| Tipo de registro                 | Sim   | Sim    |         7 |       7 |   1 | Numérico (1)   | Tabela Interna 25      |
| Valor mensal lançado             | Não   | Sim    |         8 |      23 |  16 | Valor (16)     | -                      |
| Número da unidade jurisdicionada | Sim   | Sim    |        24 |      29 |   6 | Numérico (6)   | Número da UJ no TCE-PE |
| Código de despesa extra padrão   | Sim   | Sim    |        30 |      37 |   8 | Numérico (8)   | Tabela Interna 03      |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      1000000000000000000000100000001
```

Fatiamento por posição (exemplo):

- **Tipo de registro**: posições **7–7** (`linha[7:7]` em base 1).
- **Valor mensal lançado**: posições **8–23** (`linha[8:23]` em base 1).
- **Número da unidade jurisdicionada**: posições **24–29** (`linha[24:29]` em base 1).
- **Código de despesa extra padrão**: posições **30–37** (`linha[30:37]` em base 1).

---

## 20. RestosInscritos

**Periodicidade:** Periodicidade: Anual
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter informações dos empenhos em restos inscritos.
**Tamanho do registro (linha):** 674 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                                       | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho  | Observação / Origem                                          |
| ------------------------------------------- | ----- | ------ | --------: | ------: | --: | --------------- | ------------------------------------------------------------ |
| Ano de emissão do empenho                   | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)    | AAAA                                                         |
| Código da unidade orçamentária              | Sim   | Sim    |        11 |      20 |  10 | Numérico (10)   | -                                                            |
| Código da função                            | Não   | Sim    |        21 |      22 |   2 | Numérico (2)    | Tabela Interna 05                                            |
| Código da subfunção                         | Não   | Sim    |        23 |      25 |   3 | Numérico (3)    | Tabela Interna 11                                            |
| Código do programa                          | Não   | Sim    |        26 |      29 |   4 | Numérico (4)    | -                                                            |
| Código da ação                              | Não   | Sim    |        30 |      35 |   6 | Numérico (6)    | -                                                            |
| Identificação da ação                       | Não   | Sim    |        36 |      36 |   1 | Numérico (1)    | Tabela Interna 12                                            |
| Código da categoria econômica               | Não   | Sim    |        43 |      43 |   1 | Numérico (1)    | Tabela Interna 02                                            |
| Código do grupo da natureza de despesa      | Não   | Sim    |        44 |      44 |   1 | Numérico (1)    | Tabela Interna 07                                            |
| Código da modalidade de aplicação           | Não   | Sim    |        45 |      46 |   2 | Numérico (2)    | Tabela Interna 06                                            |
| Código do elemento de despesa da dotação \* | Não   | Sim    |        47 |      48 |   2 | Numérico (2)    | Tabela Interna 04                                            |
| Código do subelemento de despesa            | Não   | Sim    |        49 |      51 |   3 | Numérico (3)    | Tabela Interna 10 – Usar 999 quando não possuir subelenmento |
| Modalidade de licitação                     | Não   | Sim    |        52 |      53 |   2 | Numérico (2)    | Tabela Interna 20                                            |
| Número do empenho                           | Sim   | Sim    |        54 |      63 |  10 | Numérico (10)   | -                                                            |
| Tipo de empenho                             | Não   | Sim    |        64 |      64 |   1 | Numérico (1)    | Tabela Interna 18                                            |
| Data de emissão do empenho                  | Não   | Sim    |        65 |      72 |   8 | Data (8)        | DDMMAAAA                                                     |
| Valor inscrito (saldo a pagar)              | Não   | Sim    |        73 |      88 |  16 | Valor (16)      | -                                                            |
| Histórico do empenho                        | Não   | Sim    |        89 |     598 | 510 | Caractere (510) | -                                                            |
| CPF / CNPJ do credor                        | Não   | Sim    |       599 |     612 |  14 | Numérico (14)   | Fornecedores                                                 |
| Código da fonte de recurso                  | Não   | Sim    |       613 |     620 |   8 | Numérico (8)    | Tabela Interna 29                                            |
| Valor processado                            | Não   | Sim    |       621 |     636 |  16 | Valor (16)      | -                                                            |
| Valor não processado                        | Não   | Sim    |       637 |     652 |  16 | Valor (16)      | -                                                            |
| CPF do ordenador de despesa                 | Não   | Sim    |       653 |     663 |  11 | Numérico (11)   | CPF do ordenador de despesa da unidade orçamentária          |
| Nº do procedimento licitação ref. à despesa | Não   | Não    |       664 |     672 |   9 | Caractere (9)   | NNNNNAAAA                                                    |
| Código do elemento de despesa do empenho \* | Não   | Sim    |       673 |     674 |   2 | Numérico (2)    | Tabela interna 04                                            |

### Exemplo de leitura (ilustrativo)

Exemplo de linha (recorte):

```txt
      000100000000010100100010000011      1101010010100000000011202501010000000000000000XXXXXXXXXXXXXXXXXXXXXX ... XXXXXXXXXXXXXX00000000000001000000010000000000000000000000000000000000000000001XXXXXXXXX01
```

> Observação: a linha completa possui 674 caracteres; acima é exibido um recorte para facilitar leitura.

Fatiamento por posição (exemplo):

- **Ano de emissão do empenho**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da unidade orçamentária**: posições **11–20** (`linha[11:20]` em base 1).
- **Código da função**: posições **21–22** (`linha[21:22]` em base 1).
- **Código da subfunção**: posições **23–25** (`linha[23:25]` em base 1).
- **Código do programa**: posições **26–29** (`linha[26:29]` em base 1).
- **Código da ação**: posições **30–35** (`linha[30:35]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 21. EstornoRestos

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter informações dos estornos/cancelamentos e baixas (exceto pagamentos) realizados no mês, referentes aos restos a pagar inscritos.
**Tamanho do registro (linha):** 181 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                          | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho  | Observação / Origem |
| ------------------------------ | ----- | ------ | --------: | ------: | --: | --------------- | ------------------- |
| Ano do empenho estornado       | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)    | AAAA                |
| Código da unidade orçamentária | Sim   | Sim    |        11 |      20 |  10 | Numérico (10)   | -                   |
| Número do empenho estornado    | Sim   | Sim    |        21 |      30 |  10 | Numérico (10)   | RestosInscritos     |
| Número do estorno              | Sim   | Sim    |        31 |      37 |   7 | Numérico (7)    | -                   |
| Data do estorno                | Sim   | Sim    |        38 |      45 |   8 | Data (8)        | DDMMAAAA            |
| Valor do estorno               | Não   | Sim    |        46 |      61 |  16 | Valor (16)      | -                   |
| Motivo do estorno              | Não   | Sim    |        62 |     181 | 120 | Caractere (120) | -                   |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      0001000000000100000000010000001202501010000000000000000XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

Fatiamento por posição (exemplo):

- **Ano do empenho estornado**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da unidade orçamentária**: posições **11–20** (`linha[11:20]` em base 1).
- **Número do empenho estornado**: posições **21–30** (`linha[21:30]` em base 1).
- **Número do estorno**: posições **31–37** (`linha[31:37]` em base 1).
- **Data do estorno**: posições **38–45** (`linha[38:45]` em base 1).
- **Valor do estorno**: posições **46–61** (`linha[46:61]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 22. PagamentosRestos

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter informações dos pagamentos realizados no mês, referentes aos restos a pagar inscritos.
**Tamanho do registro (linha):** 45 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                          | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem |
| ------------------------------ | ----- | ------ | --------: | ------: | --: | -------------- | ------------------- |
| Ano do empenho                 | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)   | AAAA                |
| Código da unidade orçamentária | Sim   | Sim    |        11 |      20 |  10 | Numérico (10)  | -                   |
| Número do empenho a ser pago   | Sim   | Sim    |        21 |      30 |  10 | Numérico (10)  | RestosIncritos      |
| Número da parcela do pagamento | Sim   | Sim    |        31 |      37 |   7 | Numérico (7)   | -                   |
| Data do pagamento              | Não   | Sim    |        38 |      45 |   8 | Data (8)       | DDMMAAAA            |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      000100000000010000000001000000120250101
```

Fatiamento por posição (exemplo):

- **Ano do empenho**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da unidade orçamentária**: posições **11–20** (`linha[11:20]` em base 1).
- **Número do empenho a ser pago**: posições **21–30** (`linha[21:30]` em base 1).
- **Número da parcela do pagamento**: posições **31–37** (`linha[31:37]` em base 1).
- **Data do pagamento**: posições **38–45** (`linha[38:45]` em base 1).

---

## 23. ItemPagamentosRestos

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter informações de cada parte que compõe os pagamentos realizados no mês, referentes aos restos a pagar inscritos.
**Tamanho do registro (linha):** 198 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                                            | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem |
| ------------------------------------------------ | ----- | ------ | --------: | ------: | --: | -------------- | ------------------- |
| Ano do empenho                                   | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)   | AAAA                |
| Código da unidade orçamentária                   | Sim   | Sim    |        11 |      20 |  10 | Numérico (10)  | -                   |
| Número do empenho a ser pago                     | Sim   | Sim    |        21 |      30 |  10 | Numérico (10)  | RestosInscritos     |
| Número da parcela do pagamento                   | Sim   | Sim    |        31 |      37 |   7 | Numérico (7)   | PagamentosRestos    |
| Valor do pagamento                               | Não   | Sim    |        38 |      53 |  16 | Valor(16)      | -                   |
| Nº da conta bancária de débito                   | Não   | Sim    |        54 |      65 |  12 | Caractere (12) | CadastroContas      |
| Nº do cheque emitido                             | Não   | Não    |        66 |      71 |   6 | Caractere (6)  | -                   |
| Nº do doc. de débito automático                  | Não   | Não    |        72 |      82 |  11 | Caractere (11) | -                   |
| Código do banco para crédito em conta (FEBRABAN) | Não   | Não    |        83 |      85 |   3 | Caractere (3)  | Tabela Interna 01   |
| Código da agência para crédito em conta          | Não   | Não    |        86 |      91 |   6 | Caractere (6)  | -                   |
| Nº da conta bancária para crédito                | Não   | Não    |        92 |     103 |  12 | Caractere (12) | -                   |
| Código da fonte de recurso                       | Não   | Sim    |       104 |     111 |   8 | Numérico (8)   | Tabela Interna 29   |
| Tipo da conta bancária                           | Não   | Sim    |       112 |     112 |   1 | Numérico (1)   | Tabela interna 28   |
| Número sequencial do pagamento                   | Sim   | Sim    |       113 |     119 |   7 | Numérico (7)   | -                   |
| Tipo do pagamento                                | Não   | Sim    |       120 |     121 |   2 | Numérico (2)   | Tabela interna 35   |
| Chave PIX                                        | Não   | Não    |       122 |     198 |  77 | Caractere (77) | -                   |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      00010000000001000000000100000010000000000000000XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX000000011000000101XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

Fatiamento por posição (exemplo):

- **Ano do empenho**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da unidade orçamentária**: posições **11–20** (`linha[11:20]` em base 1).
- **Número do empenho a ser pago**: posições **21–30** (`linha[21:30]` em base 1).
- **Número da parcela do pagamento**: posições **31–37** (`linha[31:37]` em base 1).
- **Valor do pagamento**: posições **38–53** (`linha[38:53]` em base 1).
- **Nº da conta bancária de débito**: posições **54–65** (`linha[54:65]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 24. RetencaoRestos

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter informações dos valores retidos quando da realização dos pagamentos dos restos a pagar
**Tamanho do registro (linha):** 56 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                          | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem                           |
| ------------------------------ | ----- | ------ | --------: | ------: | --: | -------------- | --------------------------------------------- |
| Ano do empenho                 | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)   | AAAA                                          |
| Código da unidade orçamentária | Sim   | Sim    |        11 |      20 |  10 | Numérico (10)  | -                                             |
| Número do empenho de origem    | Sim   | Sim    |        21 |      30 |  10 | Numérico (10)  | RestosInscritos                               |
| Número da parcela do pagamento | Sim   | Sim    |        31 |      37 |   7 | Numérico (7)   | PagamentosRestos                              |
| Valor da retenção              | Não   | Sim    |        38 |      53 |  16 | Valor (16)     | -                                             |
| Tipo de retenção efetuada      | Sim   | Sim    |        54 |      55 |   2 | Numérico (2)   | Tabela interna 26                             |
| Tipo de lançamento             | Sim   | Sim    |        56 |      56 |   1 | Numérico (1)   | 1-Registro de Retenção; 2-Estorno de Retenção |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      00010000000001000000000100000010000000000000000011
```

Fatiamento por posição (exemplo):

- **Ano do empenho**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da unidade orçamentária**: posições **11–20** (`linha[11:20]` em base 1).
- **Número do empenho de origem**: posições **21–30** (`linha[21:30]` em base 1).
- **Número da parcela do pagamento**: posições **31–37** (`linha[31:37]` em base 1).
- **Valor da retenção**: posições **38–53** (`linha[38:53]` em base 1).
- **Tipo de retenção efetuada**: posições **54–55** (`linha[54:55]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 25. ConciliacaoBancaria

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Excluído nesta versão
**Explicação funcional:** Esta tabela deverá conter informações relativas às conciliações dos saldos bancários
**Tamanho do registro (linha):** 214 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                                  | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho  | Observação / Origem |
| -------------------------------------- | ----- | ------ | --------: | ------: | --: | --------------- | ------------------- |
| Nº da conta bancária                   | Sim   | Sim    |         7 |      18 |  12 | Caractere (12)  | SaldoMensal         |
| Nº sequencial                          | Sim   | Sim    |        19 |      26 |   8 | Numérico (8)    | -                   |
| Forma de conciliação                   | Não   | Sim    |        27 |      27 |   1 | Numérico (1)    | Tabela Interna 15   |
| Descrição sobre o valor conciliado     | Não   | Sim    |        28 |     177 | 150 | Caractere (150) | -                   |
| Data do fato que está sendo conciliado | Não   | Sim    |       178 |     185 |   8 | Data (8)        | DDMMAAAA            |
| Nº do documento                        | Não   | Sim    |       186 |     196 |  11 | Caractere (11)  | -                   |
| Valor conciliado                       | Não   | Sim    |       197 |     212 |  16 | Valor (16)      | -                   |
| Tipo da conta bancária                 | Sim   | Sim    |       213 |     213 |   1 | Numérico (1)    | Tabela interna 28   |
| Tipo de documento bancário             | Sim   | Sim    |       214 |     214 |   1 | Numérico (1)    | Tabela interna 32   |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      XXXXXXXXXXXX000000011XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX20250101XXXXXXXXXXX000000000000000011
```

Fatiamento por posição (exemplo):

- **Nº da conta bancária**: posições **7–18** (`linha[7:18]` em base 1).
- **Nº sequencial**: posições **19–26** (`linha[19:26]` em base 1).
- **Forma de conciliação**: posições **27–27** (`linha[27:27]` em base 1).
- **Descrição sobre o valor conciliado**: posições **28–177** (`linha[28:177]` em base 1).
- **Data do fato que está sendo conciliado**: posições **178–185** (`linha[178:185]` em base 1).
- **Nº do documento**: posições **186–196** (`linha[186:196]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 26. SaldoInicial

**Periodicidade:** Periodicidade: Anual
**Situação:** Situação: Excluído nesta versão
**Explicação funcional:** Esta tabela deverá conter os valores dos saldos do início do exercício já conciliados.
**Tamanho do registro (linha):** 35 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                     | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem |
| ------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ------------------- |
| Nª da conta bancária      | Sim   | Sim    |         7 |      18 |  12 | Caractere (12) | CadastroContas      |
| Valor do saldo conciliado | Não   | Sim    |        19 |      34 |  16 | Valor (16)     | -                   |
| Tipo da conta bancária    | Sim   | Sim    |        35 |      35 |   1 | Numérico (1)   | Tabela interna 28   |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      XXXXXXXXXXXX00000000000000001
```

Fatiamento por posição (exemplo):

- **Nª da conta bancária**: posições **7–18** (`linha[7:18]` em base 1).
- **Valor do saldo conciliado**: posições **19–34** (`linha[19:34]` em base 1).
- **Tipo da conta bancária**: posições **35–35** (`linha[35:35]` em base 1).

---

## 27. SaldoMensal

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Excluído nesta versão
**Explicação funcional:** Esta tabela deverá conter os valores dos saldos de extrato das contas bancárias do final de cada mês.
**Tamanho do registro (linha):** 35 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                     | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem |
| ------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ------------------- |
| Nª da conta bancária      | Sim   | Sim    |         7 |      18 |  12 | Caractere (12) | CadastroContas      |
| Valor do saldo de extrato | Não   | Sim    |        19 |      34 |  16 | Valor (16)     | -                   |
| Tipo da conta bancária    | Sim   | Sim    |        35 |      35 |   1 | Numérico (1)   | Tabela interna 28   |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      XXXXXXXXXXXX00000000000000001
```

Fatiamento por posição (exemplo):

- **Nª da conta bancária**: posições **7–18** (`linha[7:18]` em base 1).
- **Valor do saldo de extrato**: posições **19–34** (`linha[19:34]` em base 1).
- **Tipo da conta bancária**: posições **35–35** (`linha[35:35]` em base 1).

---

## 28. Fornecedores

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Excluído nesta versão
**Explicação funcional:** Esta tabela deverá conter a relação de todas as pessoas físicas ou jurídicas que possuam alguma relação de ordem econômica com a unidade jurisdicionada.
**Tamanho do registro (linha):** 163 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                               | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem |
| ----------------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ------------------- |
| Nº do CPF / CNPJ                    | Sim   | Sim    |         7 |      20 |  14 | Numérico (14)  | -                   |
| Nome, denominação ou razão jurídica | Não   | Sim    |        21 |     100 |  80 | Caractere (80) | -                   |
| Tipo de credor                      | Não   | Sim    |       101 |     101 |   1 | Numérico (1)   | Tabela Interna 17   |
| Sigla da UF                         | Sim   | Sim    |       102 |     103 |   2 | Caractere(2)   | -                   |
| Município                           | Não   | Sim    |       104 |     163 |  60 | Caractere(60)  | -                   |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      00000000000001XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX1XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

Fatiamento por posição (exemplo):

- **Nº do CPF / CNPJ**: posições **7–20** (`linha[7:20]` em base 1).
- **Nome, denominação ou razão jurídica**: posições **21–100** (`linha[21:100]` em base 1).
- **Tipo de credor**: posições **101–101** (`linha[101:101]` em base 1).
- **Sigla da UF**: posições **102–103** (`linha[102:103]` em base 1).
- **Município**: posições **104–163** (`linha[104:163]` em base 1).

---

## 29. PagamentoEstorno

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Deverá conter os estornos de pagamentos referentes aos empenhos do exercício.
**Tamanho do registro (linha):** 208 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                          | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho  | Observação / Origem |
| ------------------------------ | ----- | ------ | --------: | ------: | --: | --------------- | ------------------- |
| Ano do empenho                 | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)    | AAAA                |
| Código da unidade orçamentária | Sim   | Sim    |        11 |      20 |  10 | Numérico (10)   | UnidadeOrcamentaria |
| Número do empenho              | Sim   | Sim    |        21 |      27 |   7 | Numérico (7)    | Empenhos            |
| Número do estorno              | Sim   | Sim    |        28 |      34 |   7 | Numérico (7)    | -                   |
| Data do estorno                | Não   | Sim    |        35 |      42 |   8 | Data (8)        | DDMMAAAA            |
| Valor do estorno               | Não   | Sim    |        43 |      58 |  16 | Valor (16)      | -                   |
| Histórico do estorno           | Não   | SIm    |        59 |     208 | 150 | Caractere (150) | -                   |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      0001000000000100000010000001202501010000000000000000XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

Fatiamento por posição (exemplo):

- **Ano do empenho**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da unidade orçamentária**: posições **11–20** (`linha[11:20]` em base 1).
- **Número do empenho**: posições **21–27** (`linha[21:27]` em base 1).
- **Número do estorno**: posições **28–34** (`linha[28:34]` em base 1).
- **Data do estorno**: posições **35–42** (`linha[35:42]` em base 1).
- **Valor do estorno**: posições **43–58** (`linha[43:58]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 30. LiquidacaoEstorno

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Deverá conter os estornos de liquidações referentes aos empenhos do exercício.
**Tamanho do registro (linha):** 208 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                          | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho  | Observação / Origem |
| ------------------------------ | ----- | ------ | --------: | ------: | --: | --------------- | ------------------- |
| Ano de emissão do empenho      | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)    | AAAA                |
| Código da unidade orçamentária | Sim   | Sim    |        11 |      20 |  10 | Numérico (10)   | UnidadeOrcamentaria |
| Número do empenho              | Sim   | Sim    |        21 |      27 |   7 | Numérico (7)    | Empenhos            |
| Número do estorno              | Sim   | Sim    |        28 |      34 |   7 | Numérico (7)    | -                   |
| Data do estorno                | Sim   | Sim    |        35 |      42 |   8 | Data (8)        | DDMMAAAA            |
| Valor estornado                | Não   | Sim    |        43 |      58 |  16 | Valor (16)      | -                   |
| Histórico do estorno           | Não   | Sim    |        59 |     208 | 150 | Caractere (150) | -                   |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      0001000000000100000010000001202501010000000000000000XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

Fatiamento por posição (exemplo):

- **Ano de emissão do empenho**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da unidade orçamentária**: posições **11–20** (`linha[11:20]` em base 1).
- **Número do empenho**: posições **21–27** (`linha[21:27]` em base 1).
- **Número do estorno**: posições **28–34** (`linha[28:34]` em base 1).
- **Data do estorno**: posições **35–42** (`linha[35:42]` em base 1).
- **Valor estornado**: posições **43–58** (`linha[43:58]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 31. PagamentoRestoEstorno

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Deverá conter os estornos de pagamentos referentes aos restos a pagar.
**Tamanho do registro (linha):** 211 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                          | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho  | Observação / Origem |
| ------------------------------ | ----- | ------ | --------: | ------: | --: | --------------- | ------------------- |
| Ano do empenho                 | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)    | AAAA                |
| Código da unidade orçamentária | Sim   | Sim    |        11 |      20 |  10 | Numérico (10)   | -                   |
| Número do empenho              | Sim   | Sim    |        21 |      30 |  10 | Numérico (10)   | RestosInscritos     |
| Número do estorno              | Sim   | Sim    |        31 |      37 |   7 | Numérico (7)    | -                   |
| Data do estorno                | Não   | Sim    |        38 |      45 |   8 | Data (8)        | DDMMAAAA            |
| Valor do estorno               | Não   | Sim    |        46 |      61 |  16 | Valor (16)      | -                   |
| Histórico do estorno           | Não   | Sim    |        62 |     211 | 150 | Caractere (150) | -                   |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      0001000000000100000000010000001202501010000000000000000XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

Fatiamento por posição (exemplo):

- **Ano do empenho**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da unidade orçamentária**: posições **11–20** (`linha[11:20]` em base 1).
- **Número do empenho**: posições **21–30** (`linha[21:30]` em base 1).
- **Número do estorno**: posições **31–37** (`linha[31:37]` em base 1).
- **Data do estorno**: posições **38–45** (`linha[38:45]` em base 1).
- **Valor do estorno**: posições **46–61** (`linha[46:61]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 32. AgentePolitico

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Excluído nesta versão
**Explicação funcional:** Deverá conter os dados pessoais que identifiquem cada pessoa física (agente político) da unidade jurisdicionada que sejam responsáveis ou ordenadores de despesa de uma unidade orçamentária.
**Tamanho do registro (linha):** 211 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                   | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem                       |
| ----------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ----------------------------------------- |
| CPF                     | Sim   | Sim    |         7 |      17 |  11 | Numérico (11)  | CPF do agente político                    |
| RG                      | Não   | Sim    |        18 |      28 |  11 | Numérico (11)  | Nº de identidade do agente político       |
| Órgão expedidor         | Não   | Sim    |        29 |      38 |  10 | Caractere (10) | Órgão expedidor da carteira de Identidade |
| Nome                    | Não   | Sim    |        39 |      98 |  60 | Caractere (60) | Nome do agente político                   |
| Município               | Não   | Sim    |        99 |     158 |  60 | Caractere (60) | -                                         |
| UF                      | Não   | Sim    |       159 |     160 |   2 | Caractere (2)  | -                                         |
| Email                   | Não   | Sim    |       161 |     210 |  50 | Caractere (50) | -                                         |
| Tipo de agente politico | Não   | Sim    |       211 |     211 |   1 | Numérico (1)   | Tabela Interna 30                         |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      0000000000100000000001XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX1
```

Fatiamento por posição (exemplo):

- **CPF**: posições **7–17** (`linha[7:17]` em base 1).
- **RG**: posições **18–28** (`linha[18:28]` em base 1).
- **Órgão expedidor**: posições **29–38** (`linha[29:38]` em base 1).
- **Nome**: posições **39–98** (`linha[39:98]` em base 1).
- **Município**: posições **99–158** (`linha[99:158]` em base 1).
- **UF**: posições **159–160** (`linha[159:160]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 33. Ordenador

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Excluído nesta versão
**Explicação funcional:** Deverá conter a lista de ordenadores de despesa de cada unidade orçamentária, separada por período de vigência.
**Tamanho do registro (linha):** 36 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                             | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem                        |
| --------------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ------------------------------------------ |
| CPF                               | Sim   | Sim    |         7 |      17 |  11 | Numérico (11)  | AgentePolitico                             |
| Código da unidade orçamentária    | Sim   | Sim    |        18 |      27 |  10 | Numérico (10)  | UnidadeOrcamentaria                        |
| Data do início ou fim de vigência | Não   | Sim    |        28 |      35 |   8 | Data (08)      | DDMMAAAA                                   |
| Tipo da data                      | Sim   | Sim    |        36 |      36 |   1 | Numérico (1)   | 1 - Início de vigência 2 - Fim de vigência |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      000000000010000000001202501011
```

Fatiamento por posição (exemplo):

- **CPF**: posições **7–17** (`linha[7:17]` em base 1).
- **Código da unidade orçamentária**: posições **18–27** (`linha[18:27]` em base 1).
- **Data do início ou fim de vigência**: posições **28–35** (`linha[28:35]` em base 1).
- **Tipo da data**: posições **36–36** (`linha[36:36]` em base 1).

---

## 34. Gestor

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Excluído nesta versão
**Explicação funcional:** Deverá conter a lista de gestores da unidade jurisdicionada, identificando o período de vigência.
**Tamanho do registro (linha):** 36 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                             | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem                        |
| --------------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ------------------------------------------ |
| CPF do agente político            | Sim   | Sim    |         7 |      17 |  11 | Numérico (11)  | AgentePolitico                             |
| Data do início ou fim de vigência | Não   | Sim    |        18 |      25 |   8 | Data (08)      | DDMMAAAA                                   |
| Tipo da data                      | Sim   | Sim    |        26 |      26 |   1 | Numérico (1)   | 1 – Início de vigência 2 – Fim de vigência |
| Código da unidade orçamentária    | Sim   | Sim    |        27 |      36 |  10 | Numérico (10)  | UnidadeOrcamentaria                        |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      000000000012025010110000000001
```

Fatiamento por posição (exemplo):

- **CPF do agente político**: posições **7–17** (`linha[7:17]` em base 1).
- **Data do início ou fim de vigência**: posições **18–25** (`linha[18:25]` em base 1).
- **Tipo da data**: posições **26–26** (`linha[26:26]` em base 1).
- **Código da unidade orçamentária**: posições **27–36** (`linha[27:36]` em base 1).

---

## 35. TecnicoResponsavel

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Excluído nesta versão
**Explicação funcional:** Deverá conter os dados pessoais que identifiquem o responsável técnico pelo envio dos dados da unidade jurisdicionada.
**Tamanho do registro (linha):** 515 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                       | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem                        |
| --------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ------------------------------------------ |
| CPF                         | Sim   | Sim    |         7 |      17 |  11 | Numérico (11)  | CPF do técnico responsável                 |
| Nome do técnico responsável | Não   | Sim    |        18 |      77 |  60 | Caractere (60) | -                                          |
| Razao social                | Não   | Não    |        78 |     147 |  70 | Caractere (70) | Nome da Empresa                            |
| Provedor de sistema         | Não   | Não    |       148 |     217 |  70 | Caractere(70)  | -                                          |
| Email                       | Não   | Não    |       218 |     267 |  50 | Caractere (50) | Email do técnico responsável               |
| Logradouro                  | Não   | Sim    |       268 |     317 |  50 | Caractere (50) | -                                          |
| Número                      | Não   | Sim    |       318 |     321 |   4 | Numerico(4)    | -                                          |
| Complemento                 | Não   | Não    |       322 |     371 |  50 | Caractere (50) | -                                          |
| Bairro                      | Não   | Sim    |       372 |     401 |  30 | Caractere (30) | -                                          |
| Município                   | Não   | Sim    |       402 |     461 |  60 | Caractere(60)  | -                                          |
| Estado                      | Não   | Sim    |       462 |     463 |   2 | Caractere(2)   | -                                          |
| CEP                         | Não   | Sim    |       464 |     471 |   8 | Caractere (8)  | -                                          |
| DDD tefelone                | Não   | Não    |       472 |     473 |   2 | Caractere(2)   | -                                          |
| Telefone fixo               | Não   | Não    |       474 |     481 |   8 | Caractere(8)   | -                                          |
| Celular                     | Não   | Não    |       482 |     490 |   9 | Caractere(9)   | -                                          |
| CNPJ                        | Não   | Não    |       491 |     504 |  14 | Caractere(14)  | -                                          |
| CRC                         | Não   | Sim    |       505 |     514 |  10 | Caractere(10)  | -                                          |
| Tipo técnico                | Não   | Sim    |       515 |     515 |   1 | Numérico (1)   | 1 - Contador, 2 - Técnico de Contabilidade |

### Exemplo de leitura (ilustrativo)

Exemplo de linha (recorte):

```txt
      00000000001XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX ... XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX1
```

> Observação: a linha completa possui 515 caracteres; acima é exibido um recorte para facilitar leitura.

Fatiamento por posição (exemplo):

- **CPF**: posições **7–17** (`linha[7:17]` em base 1).
- **Nome do técnico responsável**: posições **18–77** (`linha[18:77]` em base 1).
- **Razao social**: posições **78–147** (`linha[78:147]` em base 1).
- **Provedor de sistema**: posições **148–217** (`linha[148:217]` em base 1).
- **Email**: posições **218–267** (`linha[218:267]` em base 1).
- **Logradouro**: posições **268–317** (`linha[268:317]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 36. TransferenciaRecebida

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter o valor recebido a título de transferência financeira de outras entidades do município.
**Tamanho do registro (linha):** 43 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                                          | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem    |
| ---------------------------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ---------------------- |
| Número da unidade jurisdicionada beneficiada   | Sim   | Sim    |         7 |      12 |   6 | Numérico (6)   | Número da UJ no TCE-PE |
| Número da unidade jurisdicionada transferidora | Sim   | Sim    |        13 |      18 |   6 | Numérico (6)   | Número da UJ no TCE-PE |
| Tipo de transferência                          | Sim   | Sim    |        19 |      19 |   1 | Numérico (1)   | Tabela Interna 33      |
| Valor mensal recebido                          | Não   | Sim    |        20 |      35 |  16 | Valor (16)     | -                      |
| Código da fonte de recurso                     | Sim   | Sim    |        36 |      43 |   8 | Numérico (8)   | Tabela Interna 29      |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      0000010000011000000000000000000000001
```

Fatiamento por posição (exemplo):

- **Número da unidade jurisdicionada beneficiada**: posições **7–12** (`linha[7:12]` em base 1).
- **Número da unidade jurisdicionada transferidora**: posições **13–18** (`linha[13:18]` em base 1).
- **Tipo de transferência**: posições **19–19** (`linha[19:19]` em base 1).
- **Valor mensal recebido**: posições **20–35** (`linha[20:35]` em base 1).
- **Código da fonte de recurso**: posições **36–43** (`linha[36:43]` em base 1).

---

## 37. TransferenciaConcedida

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter o valor repassado a título de transferência financeira a outras entidades do município.
**Tamanho do registro (linha):** 43 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                                          | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem    |
| ---------------------------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | ---------------------- |
| Número da unidade jurisdicionada transferidora | Sim   | Sim    |         7 |      12 |   6 | Numérico (6)   | Número da UJ no TCE-PE |
| Número da unidade jurisdicionada beneficiada   | Sim   | Sim    |        13 |      18 |   6 | Numérico (6)   | Número da UJ no TCE-PE |
| Tipo de transferência                          | Sim   | Sim    |        19 |      19 |   1 | Numérico (1)   | Tabela Interna 33      |
| Valor mensal concedido                         | Não   | Sim    |        20 |      35 |  16 | Valor (16)     | -                      |
| Código da fonte de recurso                     | Sim   | Sim    |        36 |      43 |   8 | Numérico (8)   | Tabela Interna 29      |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      0000010000011000000000000000000000001
```

Fatiamento por posição (exemplo):

- **Número da unidade jurisdicionada transferidora**: posições **7–12** (`linha[7:12]` em base 1).
- **Número da unidade jurisdicionada beneficiada**: posições **13–18** (`linha[13:18]` em base 1).
- **Tipo de transferência**: posições **19–19** (`linha[19:19]` em base 1).
- **Valor mensal concedido**: posições **20–35** (`linha[20:35]` em base 1).
- **Código da fonte de recurso**: posições **36–43** (`linha[36:43]` em base 1).

---

## 38. LiquidacaoRestos

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter informações das liquidações realizadas no mês, referentes aos restos a pagar inscritos.
**Tamanho do registro (linha):** 264 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                               | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho  | Observação / Origem                                                                      |
| ----------------------------------- | ----- | ------ | --------: | ------: | --: | --------------- | ---------------------------------------------------------------------------------------- |
| Ano do empenho                      | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)    | AAAA                                                                                     |
| Código da unidade orçamentária      | Sim   | Sim    |        11 |      20 |  10 | Numérico (10)   | -                                                                                        |
| Número do empenho                   | Sim   | Sim    |        21 |      30 |  10 | Numérico (10)   | RestosInscritos                                                                          |
| Número da liquidação                | Sim   | Sim    |        31 |      37 |   7 | Numérico (7)    | -                                                                                        |
| Data da liquidação                  | Sim   | Sim    |        38 |      45 |   8 | Data (8)        | DDMMAAAA                                                                                 |
| Valor da liquidação                 | Não   | Sim    |        46 |      61 |  16 | Valor (16)      | -                                                                                        |
| Tipo de documento                   | Não   | Sim    |        62 |      62 |   1 | Numérico(1)     | 1 - Nota fiscal eletrônica (ICMS); 2 - Nota fiscal eletrônica serviços (ISS); 9 - Outros |
| Número da chave de acesso da NFE \* | Não   | Não    |        63 |     106 |  44 | Numérico (44)   | Somente para tipo de documento 1                                                         |
| Código da fonte de recurso          | Não   | Sim    |       107 |     114 |   8 | Numérico (8)    | Tabela Interna 29                                                                        |
| Histórico da liquidação             | Não   | Sim    |       115 |     264 | 150 | Caractere (150) | -                                                                                        |

### Exemplo de leitura (ilustrativo)

Exemplo de linha (recorte):

```txt
      00010000000001000000000100000012025010100000000000000001000000000000000000000000000000000000000000010000 ... XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

> Observação: a linha completa possui 264 caracteres; acima é exibido um recorte para facilitar leitura.

Fatiamento por posição (exemplo):

- **Ano do empenho**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da unidade orçamentária**: posições **11–20** (`linha[11:20]` em base 1).
- **Número do empenho**: posições **21–30** (`linha[21:30]` em base 1).
- **Número da liquidação**: posições **31–37** (`linha[31:37]` em base 1).
- **Data da liquidação**: posições **38–45** (`linha[38:45]` em base 1).
- **Valor da liquidação**: posições **46–61** (`linha[46:61]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 39. LiquidacaoRestosEstorno

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Mantido
**Explicação funcional:** Esta tabela deverá conter os estornos das liquidações realizadas no mês, referentes aos restos a pagar inscritos.
**Tamanho do registro (linha):** 211 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                          | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho  | Observação / Origem |
| ------------------------------ | ----- | ------ | --------: | ------: | --: | --------------- | ------------------- |
| Ano do empenho                 | Sim   | Sim    |         7 |      10 |   4 | Numérico (4)    | AAAA                |
| Código da unidade orçamentária | Sim   | Sim    |        11 |      20 |  10 | Numérico (10)   | -                   |
| Número do empenho              | Sim   | Sim    |        21 |      30 |  10 | Numérico (10)   | RestosInscritos     |
| Número do estorno              | Sim   | Sim    |        31 |      37 |   7 | Numérico (7)    | -                   |
| Data do estorno                | Não   | Sim    |        38 |      45 |   8 | Data (8)        | DDMMAAAA            |
| Valor do estorno               | Não   | Sim    |        46 |      61 |  16 | Valor (16)      | -                   |
| Histórico do estorno           | Não   | Sim    |        62 |     211 | 150 | Caractere (150) | -                   |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      0001000000000100000000010000001202501010000000000000000XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

Fatiamento por posição (exemplo):

- **Ano do empenho**: posições **7–10** (`linha[7:10]` em base 1).
- **Código da unidade orçamentária**: posições **11–20** (`linha[11:20]` em base 1).
- **Número do empenho**: posições **21–30** (`linha[21:30]` em base 1).
- **Número do estorno**: posições **31–37** (`linha[31:37]` em base 1).
- **Data do estorno**: posições **38–45** (`linha[38:45]` em base 1).
- **Valor do estorno**: posições **46–61** (`linha[46:61]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 40. ContribuicaoPrevidenciariaPatronal

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Excluído nesta versão
**Explicação funcional:** Esta tabela deverá conter informações sobre as contribuições previdenciárias patronais. Deve ser enviado um registro (soma mensal) para cada tipo de regime e tipo de folha.
**Tamanho do registro (linha):** 120 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                                    | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem                     |
| ---------------------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | --------------------------------------- |
| Tipo de regime previdenciário            | Sim   | Sim    |         7 |       7 |   1 | Numérico (1)   | RGPS=1;RPPS=2                           |
| Tipo de folha de pagamento               | Sim   | Sim    |         8 |       8 |   1 | Numérico (1)   | Folha normal = 1; Folha 13○ salário = 2 |
| Valor da base de cálculo                 | Não   | Sim    |         9 |      24 |  16 | Valor (16)     | -                                       |
| Valor total das vantagens remuneratórias | Não   | Sim    |        25 |      40 |  16 | Valor (16)     | -                                       |
| Valor devido                             | Não   | Sim    |        41 |      56 |  16 | Valor (16)     | -                                       |
| Valor contabilizado                      | Não   | Sim    |        57 |      72 |  16 | Valor (16)     | -                                       |
| Valor dos benefícios pagos diretamente   | Não   | Sim    |        73 |      88 |  16 | Valor (16)     | -                                       |
| Valor recolhido do principal             | Não   | Sim    |        89 |     104 |  16 | Valor (16)     | -                                       |
| Valor recolhido de multas/juros          | Não   | Sim    |       105 |     120 |  16 | Valor (16)     | -                                       |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      110000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
```

Fatiamento por posição (exemplo):

- **Tipo de regime previdenciário**: posições **7–7** (`linha[7:7]` em base 1).
- **Tipo de folha de pagamento**: posições **8–8** (`linha[8:8]` em base 1).
- **Valor da base de cálculo**: posições **9–24** (`linha[9:24]` em base 1).
- **Valor total das vantagens remuneratórias**: posições **25–40** (`linha[25:40]` em base 1).
- **Valor devido**: posições **41–56** (`linha[41:56]` em base 1).
- **Valor contabilizado**: posições **57–72** (`linha[57:72]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---

## 41. ContribuicaoPrevidenciariaSegurado

**Periodicidade:** Periodicidade: Mensal
**Situação:** Situação: Excluído nesta versão
**Explicação funcional:** Esta tabela deverá conter informações sobre as contribuições previdenciárias dos segurados. Deve ser enviado um registro (soma mensal) para cada tipo de regime e tipo de folha.
**Tamanho do registro (linha):** 88 caracteres (maior `P.Final`).

### Estrutura do registro (campos)

| Campo                           | Chave | Obrig. | P.Inicial | P.Final | Tam | Tipo / Tamanho | Observação / Origem                     |
| ------------------------------- | ----- | ------ | --------: | ------: | --: | -------------- | --------------------------------------- |
| Tipo de regime previdenciário   | Sim   | Sim    |         7 |       7 |   1 | Numérico (1)   | RGPS=1;RPPS=2                           |
| Tipo de folha de pagamento      | Sim   | Sim    |         8 |       8 |   1 | Numérico (1)   | Folha normal = 1; Folha 13○ salário = 2 |
| Valor da base de cálculo        | Não   | Sim    |         9 |      24 |  16 | Valor (16)     | -                                       |
| Valor retido                    | Não   | Sim    |        25 |      40 |  16 | Valor (16)     | -                                       |
| Valor contabilizado             | Não   | Sim    |        41 |      56 |  16 | Valor (16)     | -                                       |
| Valor recolhido do principal    | Não   | Sim    |        57 |      72 |  16 | Valor (16)     | -                                       |
| Valor recolhido de multas/juros | Não   | Sim    |        73 |      88 |  16 | Valor (16)     | -                                       |

### Exemplo de leitura (ilustrativo)

Exemplo de linha:

```txt
      1100000000000000000000000000000000000000000000000000000000000000000000000000000000
```

Fatiamento por posição (exemplo):

- **Tipo de regime previdenciário**: posições **7–7** (`linha[7:7]` em base 1).
- **Tipo de folha de pagamento**: posições **8–8** (`linha[8:8]` em base 1).
- **Valor da base de cálculo**: posições **9–24** (`linha[9:24]` em base 1).
- **Valor retido**: posições **25–40** (`linha[25:40]` em base 1).
- **Valor contabilizado**: posições **41–56** (`linha[41:56]` em base 1).
- **Valor recolhido do principal**: posições **57–72** (`linha[57:72]` em base 1).
- _(demais campos seguem o mesmo critério de fatiamento, conforme tabela acima)_

---
