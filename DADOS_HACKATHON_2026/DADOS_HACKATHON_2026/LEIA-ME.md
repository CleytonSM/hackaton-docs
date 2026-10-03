# Dados do Desafio — Cocapec

**X Hackathon Uni-FACEF · 3 e 4 de outubro de 2026**

Todos os arquivos deste pacote vêm da operação real da COCAPEC — Cooperativa de Cafeicultores e Agropecuaristas, de Franca.

---

## Aviso importante sobre os dados

Estes arquivos foram exportados diretamente dos sistemas e dos arquivos da cooperativa. **Não foram limpos, padronizados, corrigidos nem validados pela organização do evento.** Eles chegaram até vocês exatamente como existem lá dentro.

Isso significa que podem conter campos vazios, inconsistências entre fontes, unidades de medida misturadas, duplicidades, erros de digitação e registros que não fazem sentido à primeira vista.

Nada disso é defeito do material. É a condição normal de quem trabalha com dado de empresa, e lidar com isso é parte do desafio. **A qualidade do tratamento que vocês derem a esses problemas será avaliada** — inclusive a capacidade de perceber que um número está errado antes de construir uma conclusão em cima dele.

Desconfiem dos dados. Cruzem as fontes. Confiram as ordens de grandeza.

---

## Uso permitido

Os dados são cedidos pela Cocapec **exclusivamente para uso durante o evento**. É vedada a redistribuição, publicação ou utilização para qualquer outra finalidade.

**Não incluam os arquivos originais nos repositórios de vocês.** Trabalhem com eles localmente.

A folha de pagamento dos ensacadores teve os nomes das pessoas substituídos por identificadores (CHAPA_01, CHAPA_02, …) por proteção de dados pessoais. Toda a informação operacional foi preservada, e o mesmo identificador corresponde à mesma pessoa nos dois anos.

---

## O que tem em cada pasta

### `01_notas_fiscais/`

**`xml/`** — 460 notas fiscais eletrônicas de entrada, em XML, de 2022 a 2026. São o documento fiscal completo, com emitente e seu endereço, data e hora de emissão e de saída, transportadora, modalidade de frete, volumes com quantidade e espécie declarada, pesos líquido e bruto, e os itens com código do fornecedor, descrição, quantidade, unidade e valor.

**`danfe_pdf/`** — as mesmas notas em PDF (DANFE), para consulta visual.

> A espécie declarada e os pesos são informados pelo **fornecedor** que emitiu a nota, não pela Cocapec.

### `02_cadastros/`

**`produtos.xlsx`** — 10.604 produtos com número do item, descrição, unidade de medida, peso unitário, grupo e depósito de destino. Um mesmo produto pode constar em mais de um depósito.

**`fornecedores.xlsx`** — 859 fornecedores com código, razão social e CNPJ.

**`estoque_por_armazem/`** — posição de estoque atual dos quatro armazéns: defensivos, fertilizantes e adubos, geral e máquinas.

### `03_movimentacao/`

**`pedido_recebimento_notafiscal.xlsx`** — o arquivo central. 41.779 linhas cobrindo junho de 2022 a setembro de 2026, com a cadeia completa item a item: pedido de compra, datas, fornecedor, código e descrição do item, quantidade, peso, depósito de destino, número e data do recebimento, número da nota fiscal e chave de acesso.

Cada linha é **um item de um pedido de compra**. São 12.073 pedidos distintos, com mediana de 2 itens e média de 3,5 — quase metade tem um único item, e o maior tem 154. Em 4% dos pedidos, os itens vão para mais de um depósito.

> **Importante:** o **código de item desta planilha é o da Cocapec** (`MAQ000851`, `FER000003`, `AGR000596`) e liga direto ao cadastro de produtos e ao depósito. Já o **código que aparece dentro do XML da nota fiscal é o do fornecedor** — os dois não se correspondem. Dos 838 itens das notas da amostra, apenas um casa por código com o catálogo da cooperativa.

### `04_mao_de_obra/`

**`chapas_por_dia_2025.xlsx`** e **`chapas_por_dia_2026.xlsx`** — folha diária dos ensacadores, no formato original da cooperativa. Uma aba por mês, dividida em duas quinzenas. Nas colunas à direita há **uma coluna por dia do mês**: valor maior que zero indica que aquela pessoa trabalhou naquele dia. As demais colunas trazem as rubricas de remuneração e encargos.

**`chapas_por_dia.csv`** — os mesmos dados, já consolidados por dia pela organização do evento, para poupar o trabalho de extração. Uma linha por data, com dia da semana, quantidade de pessoas presentes, quantas delas estavam na operação de café e o valor total pago no dia.

> Os arquivos Excel originais continuam no pacote: se quiserem conferir a consolidação ou extrair algo que o CSV não traz — quem trabalhou, quanto cada um recebeu, a distribuição por quinzena —, a fonte está ali.
>
> **Atenção:** não há registro de **agosto e dezembro de 2025**. As abas com esses nomes na planilha original não contêm dados desses meses. O CSV simplesmente não os inclui.

### `05_operacao/`

**`equipamentos_descarga.xlsx`** — relação dos equipamentos utilizados na descarga e sua finalidade.

**`registro_manual_recebimento.pdf`** — digitalização do formulário de papel em que a operação registra hoje os recebimentos. Cerca de 200 lançamentos entre abril e julho de 2026.

**`formulario_registro_em_branco.doc`** — o mesmo formulário, em branco.

**`boletim_diario_chapas.xlsx`** — o Boletim Diário de Serviços dos Ensacadores, preenchido à mão todo dia em cada armazém para apurar a produção da equipe de chapas e o custo do dia. Traz a tabela de preços unitários por tipo de item, o valor da diária, a regra do piso implementada em fórmulas, e **um dia de exemplo já preenchido** (armazém de Adubo, 17/11/2025). Os nomes dos funcionários foram substituídos por identificadores. A seção 8 do dossiê descreve o funcionamento.

### `06_especificacao/`

**`recebimento_inteligente_cocapec.pdf`** — o documento escrito pela própria Cocapec descrevendo a plataforma que ela deseja.

> Esse documento foi escrito no **início das conversas** e registra a visão original da cooperativa. É anterior ao levantamento detalhado e, em vários pontos — grade de horários, tempos de descarga, organização dos locais, tratamento do custo de mão de obra —, já não corresponde ao que foi definido depois. **Onde houver divergência, prevalece o dossiê do desafio.**

**`mapa_relacoes_sap.jpeg`** — diagrama do encadeamento dos documentos no sistema da cooperativa.

---

## Como os arquivos se ligam

- A **chave de acesso** liga cada linha de `pedido_recebimento_notafiscal.xlsx` ao XML correspondente em `01_notas_fiscais/xml/`. As 460 notas em XML são um subconjunto das notas presentes na planilha de movimentação.
- O **CNPJ** liga o emitente da nota fiscal ao cadastro de fornecedores.
- O **número do item** liga a planilha de movimentação ao cadastro de produtos e às planilhas de estoque.
- O campo **depósito** identifica o armazém de destino.
- A **data de recebimento** da planilha de movimentação pode ser cruzada com a folha de mão de obra pela data.

Atenção a um ponto: o código de produto que aparece **dentro do XML** é o código do **fornecedor**, não o da Cocapec. Os dois catálogos não usam a mesma codificação.

---

## Sobre o que não está aqui

A cooperativa **nunca registrou** horário de chegada de caminhão, início ou fim de descarga, quantidade de pessoas empregadas em cada recebimento ou equipamento utilizado. Esses dados não existem em nenhum sistema.

O que existe sobre a operação está no que foi entregue aqui — e é com isso que vocês vão trabalhar.

---

Dúvidas sobre os dados devem ser encaminhadas à comissão organizadora, que consolidará e publicará as respostas a todas as equipes.
