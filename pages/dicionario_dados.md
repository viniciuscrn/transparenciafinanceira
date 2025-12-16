# DICIONÁRIO DE DADOS E MODELO ENTIDADE–RELACIONAMENTO

## Layout dos Arquivos – Exercício 2025

**Versão 1.0 – Conforme planilha “Layouts”**

---

## 1. OBJETIVO

Este documento tem por objetivo estabelecer, de forma formal e técnica, a definição das entidades de dados, periodicidades e relacionamentos lógicos entre as tabelas previstas no Layout de Arquivos – Versão 1.0 – Exercício 2025.

O documento serve como:

- base para implementação de sistemas de informação;
- referência para editais, termos de referência e contratos de software;
- apoio à integração com órgãos de controle;
- documentação de apoio para auditoria e controle interno.

---

## 2. VISÃO GERAL DO MODELO

O layout é organizado em **41 entidades-tabela**, cada uma representando um arquivo de dados a ser gerado e transmitido. Entre essas entidades existem relações lógicas (equivalentes a chaves estrangeiras em modelo relacional).

Principais blocos funcionais:

1. **Orçamento e Estrutura Orçamentária**

   - Orçamento
   - UnidadeOrcamentaria
   - Programas
   - Acao
   - Dotacao
   - PrevisaoReceita
   - AtualizacaoOrcamentaria
   - NormaAtualizacao

2. **Execução da Despesa (empenho, liquidação, pagamento e estornos)**

   - Empenhos
   - EmpenhoEstorno
   - EmpenhoReforco
   - Liquidação
   - Pagamentos
   - ItemPagamento
   - Retenção
   - PagamentoEstorno
   - LiquidacaoEstorno

3. **Receita Orçamentária e Extraorçamentária**

   - ReceitaOrcamentaria
   - ReceitaExtra
   - DespesaExtra
   - TransferenciaRecebida
   - TransferenciaConcedida

4. **Restos a Pagar e Operações Correlatas**

   - RestosInscritos
   - EstornoRestos
   - PagamentosRestos
   - ItemPagamentosRestos
   - RetencaoRestos
   - PagamentoRestoEstorno
   - LiquidacaoRestos
   - LiquidacaoRestosEstorno

5. **Contas Bancárias, Saldos e Conciliação**

   - CadastroContas
   - SaldoInicial
   - SaldoMensal
   - ConciliacaoBancaria

6. **Cadastro de Fornecedores e Agentes**

   - Fornecedores
   - AgentePolitico
   - Ordenador
   - Gestor
   - TecnicoResponsavel

7. **Contribuições Previdenciárias**
   - ContribuicaoPrevidenciariaPatronal
   - ContribuicaoPrevidenciariaSegurado

Cada uma das entidades a seguir contém:

- Nome lógico da tabela;
- Periodicidade de envio;
- Situação no layout (mantida / excluída nesta versão);
- Finalidade;
- Relacionamentos lógicos com outras entidades (quando identificados na coluna **“Observação / Origem”** da planilha de layout).

---

## 3. DICIONÁRIO DE ENTIDADES

### 3.1. Orçamento

- **Nome da entidade:** `Orçamento`
- **Periodicidade:** Anual
- **Situação no layout:** Excluído nesta versão
- **Finalidade:** Tabela originalmente destinada a conter as informações da LOA (Lei Orçamentária Anual), incluindo dados de aprovação e parâmetros gerais do orçamento.
- **Relacionamentos lógicos:**
  - Não há relacionamentos diretos explícitos com outras entidades na coluna de Observação/Origem.
- **Observação:** Apesar de excluída nesta versão, o seu conteúdo conceitual é em grande parte substituído ou complementado pelas tabelas `Dotacao` e `AtualizacaoOrcamentaria`.

---

### 3.2. UnidadeOrcamentaria

- **Nome da entidade:** `UnidadeOrcamentaria`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Contém a relação das unidades orçamentárias e/ou gestoras integrantes da estrutura institucional da unidade jurisdicionada.
- **Relacionamentos (é referenciada por):**
  - `Dotacao`
  - `AtualizacaoOrcamentaria`
  - `Empenhos`
  - `EmpenhoEstorno`
  - `EmpenhoReforco`
  - `Liquidação`
  - `Pagamentos`
  - `ItemPagamento`
  - `Retenção`
  - `PagamentoEstorno`
  - `LiquidacaoEstorno`
  - `RestosInscritos` (via campos de unidade)
  - `PagamentosRestos`, `ItemPagamentosRestos`, `RetencaoRestos` (dependendo dos campos)
  - `Ordenador`
  - `Gestor`
- **Interpretação:** Entidade central de classificação institucional; funciona como dimensão chave para toda a execução orçamentária e financeira.

---

### 3.3. Programas

- **Nome da entidade:** `Programas`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registro dos programas constantes no PPA/LOA, com respectivos códigos e denominações.
- **Relacionamentos (é referenciada por):**
  - `Dotacao` (campo de código do programa ligado à dotação).

---

### 3.4. Acao

- **Nome da entidade:** `Acao`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Descreve as ações orçamentárias (projetos, atividades, operações especiais) vinculadas a programas.
- **Relacionamentos (é referenciada por):**
  - `Dotacao`
  - `AtualizacaoOrcamentaria`

---

### 3.5. Dotacao

- **Nome da entidade:** `Dotacao`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Representa as dotações orçamentárias (rubricas de despesa) autorizadas na LOA, vinculando unidade, programa e ação.
- **Relacionamentos (possui FKs lógicas para):**
  - `UnidadeOrcamentaria`
  - `Programas`
  - `Acao`

---

### 3.6. CadastroContas

- **Nome da entidade:** `CadastroContas`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Contém o cadastro das contas bancárias utilizadas pela unidade jurisdicionada (bancos, agência, conta, tipo, etc.).
- **Relacionamentos (é referenciada por):**
  - `ItemPagamento`
  - `ItemPagamentosRestos`
  - `SaldoInicial`
  - `SaldoMensal`

---

### 3.7. PrevisaoReceita

- **Nome da entidade:** `PrevisaoReceita`
- **Periodicidade:** Anual
- **Situação:** Mantido
- **Finalidade:** Registrar a previsão de receita orçamentária aprovada, por categoria de classificação.
- **Relacionamentos:**
  - Não há FKs explícitas para outras entidades no layout analisado (os vínculos são essencialmente por códigos contábeis e de classificação).

---

### 3.8. AtualizacaoOrcamentaria

- **Nome da entidade:** `AtualizacaoOrcamentaria`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra as alterações da LOA ocorridas após o envio do orçamento (créditos adicionais, anulações, suplementações, etc.).
- **Relacionamentos (possui FKs lógicas para):**
  - `UnidadeOrcamentaria`
  - `Acao`

---

### 3.9. NormaAtualizacao

- **Nome da entidade:** `NormaAtualizacao`
- **Periodicidade:** Mensal
- **Situação:** Excluído nesta versão
- **Finalidade:** Tabela originalmente destinada a registrar normas legais que fundamentam as atualizações orçamentárias (ex.: decretos, leis específicas).
- **Relacionamentos:**
  - Conceitualmente se relacionaria com `AtualizacaoOrcamentaria`, mas está excluída na versão atual do layout.

---

### 3.10. Empenhos

- **Nome da entidade:** `Empenhos`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra todos os empenhos realizados no mês, incluindo classificação orçamentária, unidade, data, valores e outras informações.
- **Relacionamentos (possui FKs lógicas para):**
  - `UnidadeOrcamentaria`
  - `Dotacao` (por meio dos códigos de classificação)
  - `Ordenador` (campo CPF do ordenador)
- **É referenciado por:**
  - `EmpenhoEstorno`
  - `EmpenhoReforco`
  - `Liquidação`
  - `Pagamentos`
  - `ItemPagamento`
  - `Retenção`
  - `PagamentoEstorno`
  - `LiquidacaoEstorno`

---

### 3.11. EmpenhoEstorno

- **Nome da entidade:** `EmpenhoEstorno`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra estornos ou cancelamentos de empenhos realizados no exercício.
- **Relacionamentos (possui FKs lógicas para):**
  - `Empenhos` (nº do empenho estornado)
  - `UnidadeOrcamentaria`

---

### 3.12. EmpenhoReforco

- **Nome da entidade:** `EmpenhoReforco`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra reforços de empenho (aumento de valor do empenho original).
- **Relacionamentos (possui FKs lógicas para):**
  - `Empenhos` (nº do empenho reforçado)
  - `UnidadeOrcamentaria`

---

### 3.13. Liquidação

- **Nome da entidade:** `Liquidação`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Contém as liquidações de empenho, ou seja, o reconhecimento da entrega de bens/serviços.
- **Relacionamentos (possui FKs lógicas para):**
  - `Empenhos`
  - `UnidadeOrcamentaria`

---

### 3.14. Pagamentos

- **Nome da entidade:** `Pagamentos`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra os pagamentos efetuados contra liquidações de empenho, usualmente por parcela.
- **Relacionamentos (possui FKs lógicas para):**
  - `Empenhos`
  - `UnidadeOrcamentaria`
- **É referenciado por:**
  - `ItemPagamento`
  - `Retenção`

---

### 3.15. ItemPagamento

- **Nome da entidade:** `ItemPagamento`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Detalha cada parcela de pagamento, incluindo informações bancárias e outros desdobramentos.
- **Relacionamentos (possui FKs lógicas para):**
  - `Pagamentos` (nº da parcela do pagamento)
  - `Empenhos`
  - `CadastroContas` (nº da conta bancária de débito)
  - `UnidadeOrcamentaria`

---

### 3.16. Retenção

- **Nome da entidade:** `Retenção`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra valores retidos (tributos, contribuições, etc.) quando da realização de pagamentos de empenhos.
- **Relacionamentos (possui FKs lógicas para):**
  - `Pagamentos`
  - `Empenhos`
  - `UnidadeOrcamentaria`

---

### 3.17. ReceitaOrcamentaria

- **Nome da entidade:** `ReceitaOrcamentaria`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra os valores (soma mensal) da receita orçamentária registrada no mês.
- **Relacionamentos:**
  - Sem FKs lógicas explícitas para outras tabelas (os vínculos são via códigos de classificação da receita).

---

### 3.18. ReceitaExtra

- **Nome da entidade:** `ReceitaExtra`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra os valores (soma mensal) da receita extraorçamentária registrada no mês.

---

### 3.19. DespesaExtra

- **Nome da entidade:** `DespesaExtra`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra os valores de despesa extraorçamentária registrados no mês.

---

### 3.20. RestosInscritos

- **Nome da entidade:** `RestosInscritos`
- **Periodicidade:** Anual
- **Situação:** Mantido
- **Finalidade:** Registra os empenhos que foram inscritos em restos a pagar (não pagos até o encerramento do exercício).
- **Relacionamentos (possui FKs lógicas para):**
  - `Fornecedores` (campo CPF/CNPJ do credor)
- **É referenciado por:**
  - `EstornoRestos`
  - `PagamentosRestos`
  - `ItemPagamentosRestos`
  - `RetencaoRestos`
  - `PagamentoRestoEstorno`
  - `LiquidacaoRestos`
  - `LiquidacaoRestosEstorno`

---

### 3.21. EstornoRestos

- **Nome da entidade:** `EstornoRestos`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra estornos/cancelamentos de restos a pagar inscritos.
- **Relacionamentos (possui FKs lógicas para):**
  - `RestosInscritos`

---

### 3.22. PagamentosRestos

- **Nome da entidade:** `PagamentosRestos`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra os pagamentos realizados no mês relativos a restos a pagar.

---

### 3.23. ItemPagamentosRestos

- **Nome da entidade:** `ItemPagamentosRestos`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Detalha cada parte que compõe os pagamentos realizados no mês referentes aos restos a pagar.
- **Relacionamentos (possui FKs lógicas para):**
  - `PagamentosRestos` (nº da parcela do pagamento de restos)
  - `RestosInscritos` (nº do empenho a ser pago como resto)
  - `CadastroContas` (nº da conta bancária de débito)
  - eventualmente `Pagamentos` (quando houver vinculação no layout)

---

### 3.24. RetencaoRestos

- **Nome da entidade:** `RetencaoRestos`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra valores retidos quando da realização dos pagamentos dos restos a pagar.
- **Relacionamentos (possui FKs lógicas para):**
  - `PagamentosRestos`
  - `RestosInscritos`
  - `Pagamentos` (quando houver referência cruzada)
  - `Retenção` (em alguns campos derivados)

---

### 3.25. ConciliacaoBancaria

- **Nome da entidade:** `ConciliacaoBancaria`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra informações relativas à conciliação dos saldos bancários.
- **Relacionamentos (possui FKs lógicas para):**
  - `SaldoMensal`

---

### 3.26. SaldoInicial

- **Nome da entidade:** `SaldoInicial`
- **Periodicidade:** Anual
- **Situação:** Mantido
- **Finalidade:** Registra o saldo inicial das contas bancárias no início do exercício.
- **Relacionamentos (possui FKs lógicas para):**
  - `CadastroContas`

---

### 3.27. SaldoMensal

- **Nome da entidade:** `SaldoMensal`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra o saldo mensal de cada conta bancária.
- **Relacionamentos (possui FKs lógicas para):**
  - `CadastroContas`
- **É referenciado por:**
  - `ConciliacaoBancaria`

---

### 3.28. Fornecedores

- **Nome da entidade:** `Fornecedores`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Cadastro de fornecedores/credores (pessoas físicas/jurídicas).
- **Relacionamentos (é referenciado por):**
  - `RestosInscritos` (campo CPF/CNPJ do credor)

---

### 3.29. PagamentoEstorno

- **Nome da entidade:** `PagamentoEstorno`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra estornos/cancelamentos de pagamentos de empenhos.
- **Relacionamentos (possui FKs lógicas para):**
  - `Empenhos`
  - `UnidadeOrcamentaria`

---

### 3.30. LiquidacaoEstorno

- **Nome da entidade:** `LiquidacaoEstorno`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra estornos de liquidação de empenhos.
- **Relacionamentos (possui FKs lógicas para):**
  - `Empenhos`
  - `UnidadeOrcamentaria`

---

### 3.31. PagamentoRestoEstorno

- **Nome da entidade:** `PagamentoRestoEstorno`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra estornos de pagamentos de restos a pagar.
- **Relacionamentos (possui FKs lógicas para):**
  - `RestosInscritos`

---

### 3.32. AgentePolitico

- **Nome da entidade:** `AgentePolitico`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Cadastro de agentes políticos da unidade jurisdicionada (prefeito, secretários, etc.).
- **Relacionamentos (é referenciado por):**
  - `Ordenador`
  - `Gestor`

---

### 3.33. Ordenador

- **Nome da entidade:** `Ordenador`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra os ordenadores de despesa vinculados às unidades orçamentárias.
- **Relacionamentos (possui FKs lógicas para):**
  - `AgentePolitico` (CPF do agente político)
  - `UnidadeOrcamentaria`
- **É referenciado por:**
  - `Empenhos` (CPF do ordenador da despesa)

---

### 3.34. Gestor

- **Nome da entidade:** `Gestor`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra gestores administrativos/financeiros vinculados à unidade orçamentária.
- **Relacionamentos (possui FKs lógicas para):**
  - `AgentePolitico`
  - `UnidadeOrcamentaria`

---

### 3.35. TecnicoResponsavel

- **Nome da entidade:** `TecnicoResponsavel`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Cadastro de técnicos responsáveis pelas informações contábeis/financeiras enviadas.
- **Relacionamentos:**
  - Não há relacionamentos diretos explícitos com outras entidades no layout (pode ser associado logicamente a toda a remessa).

---

### 3.36. TransferenciaRecebida

- **Nome da entidade:** `TransferenciaRecebida`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra transferências de recursos recebidas (intergovernamentais ou de outras fontes).

---

### 3.37. TransferenciaConcedida

- **Nome da entidade:** `TransferenciaConcedida`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra transferências de recursos concedidas a outros entes ou entidades.

---

### 3.38. LiquidacaoRestos

- **Nome da entidade:** `LiquidacaoRestos`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra liquidações de restos a pagar.
- **Relacionamentos (possui FKs lógicas para):**
  - `RestosInscritos`

---

### 3.39. LiquidacaoRestosEstorno

- **Nome da entidade:** `LiquidacaoRestosEstorno`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra estornos de liquidações de restos a pagar.
- **Relacionamentos (possui FKs lógicas para):**
  - `RestosInscritos`

---

### 3.40. ContribuicaoPrevidenciariaPatronal

- **Nome da entidade:** `ContribuicaoPrevidenciariaPatronal`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra contribuições previdenciárias de responsabilidade patronal.
- **Relacionamentos:**
  - Sem relacionamentos diretos explicitados no layout (vínculo normalmente se dá por classificação e natureza da despesa/folha).

---

### 3.41. ContribuicaoPrevidenciariaSegurado

- **Nome da entidade:** `ContribuicaoPrevidenciariaSegurado`
- **Periodicidade:** Mensal
- **Situação:** Mantido
- **Finalidade:** Registra contribuições previdenciárias de responsabilidade do segurado (descontos em folha).
- **Relacionamentos:**
  - Sem relacionamentos diretos explicitados no layout (vínculos são normalmente por cadastro de pessoal/folha, fora do escopo deste layout).

---

## 4. RESUMO TEXTUAL DO MODELO ENTIDADE–RELACIONAMENTO

De forma sintética:

- `UnidadeOrcamentaria` é uma dimensão institucional central, referenciada por praticamente toda a cadeia de execução da despesa.
- `Programas` e `Acao` estruturam a programação orçamentária, vinculando-se a `Dotacao` e `AtualizacaoOrcamentaria`.
- `Empenhos` é a entidade núcleo da despesa, da qual derivam liquidações, pagamentos, estornos e retenções.
- `RestosInscritos` espelha os empenhos que passam a restos a pagar, com sua própria cadeia de liquidação, pagamento, estorno e retenções.
- `CadastroContas`, `SaldoInicial`, `SaldoMensal` e `ConciliacaoBancaria` cobrem a visão bancária e de saldos.
- `AgentePolitico`, `Ordenador`, `Gestor` e `TecnicoResponsavel` formam o conjunto de responsáveis formais.
- As entidades de contribuições previdenciárias e transferências completam o modelo de receitas, despesas e obrigações.

---

## 5. OBSERVAÇÕES FINAIS

1. Os relacionamentos aqui descritos baseiam-se nas referências textuais da coluna **“Observação / Origem”** da planilha de layout.
2. Em um modelo relacional físico (banco de dados), recomenda-se:
   - implementação explícita de chaves primárias e estrangeiras;
   - controle de integridade referencial entre as entidades descritas;
   - documentação adicional dos **campos** de cada tabela (nome, tipo, tamanho, obrigatoriedade), tomando a própria planilha como fonte.
