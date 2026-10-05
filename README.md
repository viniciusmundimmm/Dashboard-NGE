# Dashboard NGE — Análise Equipamentos TRIÂNGULO

Página única em HTML puro (`dashboard-nge.html`), sem bibliotecas externas. Abre com duplo
clique em qualquer navegador, funciona offline e pode ser projetada direto na reunião.

São dois ambientes, escolhidos pelas abas do topo:

- **Indisponibilidade** — equipamentos indisponíveis no SCADA, a partir da planilha semanal.
- **Execução** — guias de inspeção na carteira da Execução Regional, a partir dos arquivos
  `AbertoTR` e `AndamentoTR`.
- **Medidas SAP** — o volume de demandas em carteira que dá origem às guias, a partir das
  exportações de medidas do SAP (`Gestão Campo` e `Gestão Equipamento`).
- **Baterias** — o passivo de baterias vencidas dos religadores e quantas comprar, a partir do
  cadastro de religadores.
- **PSVT** — a compensação em R$ concentrada nos maiores PI, a partir da planilha de PSVT.

O escopo (gerência e polo) acompanha a troca de ambiente. Cada ambiente tem sua semana, seus
filtros e suas anotações; as imagens da Visão geral e o relatório em PDF são comuns aos dois.

## Ambiente Indisponibilidade

Toda a leitura é feita entre **duas datas da coluna A** (semana atual × semana de comparação,
por padrão a imediatamente anterior):

- **Resumo** — total de indisponíveis, variação, novos, regularizados, acima de 120 dias e média de dias.
- **Movimento** — cascata `semana anterior → regularizados → novos → semana atual` e evolução das 13 semanas.
  Quando o total não muda mas os equipamentos mudam, a diferença aparece como **Novos** e **Regularizados**
  (comparação por número de equipamento, não por quantidade).
- **Faixa de dias** — até 30, 30 a 90, 90 a 120 e acima de 120 dias, com variação e composição × semana anterior.
- **Responsabilidade** — carteira por grupo, um cruzamento que alterna entre **responsável × faixa
  de dias** e **responsável × polo** (com os polos agrupados pela gerência e o total de cada coluna),
  e um gráfico de
  colunas por responsável que abre em **TR**, **Gerência** ou **Polo** (a cor mostra a que grupo
  cada responsável pertence, ou a gerência de cada faixa).
- **Polo e tipo** — com TR selecionado o gráfico mostra as três gerências; ao escolher uma gerência,
  abre nos polos dela. Ao lado, a mistura por tipo de equipamento.
- **Detalhe** — todos os indisponíveis da semana, ordenados por dias, com filtros próprios de
  responsável (o texto exato da coluna F), gerência, polo, tipo e número do equipamento. Os quatro
  primeiros são de **múltipla escolha**: cada um abre uma lista de caixas com a contagem ao lado,
  dá para marcar quantas opções quiser e combinar os campos entre si. Qualquer coluna ordena ao ser
  clicada. As abas **Novos**, **Regularizados** e **Permanecem** mostram os
  mesmos recortes da comparação semanal.
- **Visão geral** — imagens e telas de apoio anexadas à reunião; abre o relatório em PDF.
- **Anotações** — observações da reunião e anotação por equipamento, agrupadas por polo.

Filtros de semana, comparação, gerência, polo, tipo e responsabilidade valem para a página inteira.
Cada gráfico tem o botão **Tabela**, que troca o desenho pelos números.

## Anotar, anexar print e gerar o PDF

- **Anotação por equipamento** — na lista de detalhe, clique na coluna **Anotação** da linha, escreva
  e clique fora (ou `Ctrl+Enter`). A linha fica marcada e a anotação aparece no bloco
  *Anotações e evidências*, junto com tipo, polo, gerência, dias e responsável do equipamento.
  As anotações aparecem **agrupadas por polo**, com a gerência e a contagem de cada um, para cobrar
  a pendência polo a polo. Dentro do polo, a ordem é do maior prazo para o menor.
- **Imagens (Visão geral)** — arraste a imagem para a área, cole com `Ctrl+V` ou selecione o arquivo.
  Cada imagem aceita uma legenda e abre ampliada ao ser clicada. São reduzidas para caber no navegador.
- **Relatório PDF** — o botão **Relatório PDF** abre a impressão do navegador; escolha *Salvar como
  PDF*. A ordem do relatório é: **Visão geral** (as imagens, abrindo o documento), indicadores,
  gráficos com dois por página, a lista completa de indisponíveis e, por último, as anotações por polo.

  Na janela de impressão, em **Mais configurações**, é preciso **desmarcar "Cabeçalhos e rodapés"** —
  é essa opção do navegador, e não o dashboard, que imprime o caminho do arquivo e a data em cada
  página — e **marcar "Gráficos de segundo plano"**, senão as cores saem apagadas. O próprio
  dashboard lembra disso antes de abrir a impressão; o navegador guarda as escolhas para as
  próximas vezes.

Anotações, legendas e imagens ficam guardadas no navegador do computador em uso.

## Ambiente Execução

Cada linha dos arquivos é uma **guia de inspeção**, identificada pela solicitação; o `Tipo` é o
**tipo de serviço**. Os dois arquivos são carregados de uma vez e viram um conjunto só, com o
estágio como coluna:

- **Aberta** — em avaliação de programação.
- **Andamento** — em rota de execução, já com recursos avaliados e guia disponibilizada no G-DIS OP.

A data de cada linha é a **data da coleta** — é ela que monta o histórico. Há dois jeitos de
informá-la:

- **coluna A com a data** (recomendado): acrescente uma coluna `Data` ou `Semana` no início da
  planilha e empurre as colunas originais de B em diante. Assim um arquivo só pode carregar várias
  coletas de uma vez, e recarregá-lo não duplica nada — cada data presente no arquivo substitui
  inteira a foto daquele dia.
- **sem a coluna**: a data informada no painel é carimbada em todas as linhas, e o arquivo vale por
  uma coleta só.

Os dois formatos convivem, e o painel avisa na carga quantas linhas trouxeram data própria e quais
datas o arquivo repôs. A idade de cada guia é contada da data de cadastro até a data da foto, e não
até hoje, para que uma semana antiga continue mostrando as idades daquele dia.

As telas: resumo, cascata e evolução, idade em cinco faixas (até 15, 16 a 30, 31 a 60, 61 a 120 e
acima de 120 dias), estágio com o fluxo entre eles, tipo de serviço aberto por TR, gerência ou
polo, polo e gerência, mapa de tipo × idade, capacidade, programação, cruzamento e detalhe.

**O vocabulário do movimento é literal**: "saíram da execução" significa que a guia não está mais
na carteira — pode ter sido concluída, cancelada ou redirecionada. Em nenhum lugar o painel diz
"concluída". "Foram para rota de execução" conta as guias que passaram de Aberta para Andamento
na semana, que é a medida direta de programação realizada.

Equipamentos com mais de uma guia aberta aparecem marcados na lista, separando o caso de **duas
guias do mesmo tipo** (possível duplicidade, marca vermelha) do caso de **tipos diferentes**
(serviços distintos, marca roxa). A aba *Equip. com 2+ guias* isola esses casos.

### Capacidade e programação

Cadastre as equipes (nome e gerência). O painel calcula, por gerência, quantas equipes seriam
necessárias para zerar a fila em um mês e em quanto tempo ela zera, com dois parâmetros editáveis
e **guardados por semana**: serviços por equipe/dia (padrão 4) e dias úteis por mês (padrão 20),
mais a **% de dedicação** de cada gerência — sem ela o painel supõe equipes dedicadas em tempo
integral e mostra um prazo otimista demais.

O prazo aparece em duas versões: **bruto** (zerar o que está em tela) e **líquido** (descontando
as guias que entram por semana, medidas no histórico). Se a entrada superar a capacidade, o painel
diz que a fila não zera em vez de inventar uma data.

Na lista de detalhe, selecione as guias e atribua **equipe e semana prevista** em lote. O filtro
**Vínculo** isola as guias de equipamento indisponível, para programá-las primeiro.

Isso alimenta a tela de **Serviços previstos por equipe** — uma grade de 8 semanas com
`programadas / capacidade` em cada célula (capacidade = serviços por dia × 5 dias × dedicação;
vermelho acima dela). Clicando numa célula, abaixo aparece **a lista dos serviços previstos**
daquela equipe naquela semana: guia, tipo, polo, município, idade, equipamento e se ele está
indisponível.

A programação é local, feita à mão, e não vai para o G-DIS OP.

### Executado na semana · Inspeções SGE × G-DIS OP

O terceiro arquivo é a lista do que as equipes **de fato executaram em campo**. São lidas as colunas
**C** (tipo), **D** (nº do serviço), **E** (data designação), **F** (**data término**, que é a data
de execução), **G** (veículo = **equipe**), **H** (situação), **I** (bairro), **K** (região = polo),
**L** (alimentador), **M** (equipamento) e **N** (**observação de fechamento**). O cabeçalho é
reconhecido pelo nome e, quando não bate, vale a posição da letra. Na carga, o painel mostra as
primeiras linhas lidas para conferência.

**Período ajustável.** As duas bases têm naturezas diferentes — as guias são uma foto numa data, os
executados são eventos ao longo de um intervalo. Por padrão o período dos executados acompanha as
duas fotos de guias; os campos **de / até** no alto da seção permitem fixar outro, e um botão
devolve o padrão.

**Serviços por dia útil** abre em quatro recortes — **por dia**, **por semana**, **por equipe** e
**por polo**. Equipe é o veículo da exportação. O denominador é sempre o número de dias úteis do
período, para que semanas de tamanhos diferentes se comparem.

**A observação de fechamento** aparece na lista de serviços, resumida na linha e inteira na lupa.
A busca da lista varre serviço, equipamento, equipe e o texto da observação.

Aqui a data já vem na própria linha (coluna F), então pode ser **um arquivo único acumulado**: cada
serviço entra pela sua data de execução e recarregar o mesmo arquivo não duplica (serviço + data +
equipamento repetidos são ignorados). O painel guarda tudo e o cruzamento usa a janela entre a foto
de guias comparada e a foto selecionada — sem foto anterior, os sete dias antes dela.

O cruzamento usa o **equipamento** e responde o que o planejamento sozinho não responde. Vale
registrar o limite: **não existe campo que ligue um serviço do G-DIS à guia que o originou**, então
serviço em equipamento com guia aberta é indício forte, não prova — e o painel diz isso na tela.

- quantos serviços foram executados em equipamento **com guia de inspeção** e quantos **sem guia
  nenhuma** — estes últimos são execução fora do controle;
- quantos foram feitos e a **guia continua aberta** (pode ser outra guia do mesmo equipamento, ou
  falta de encerramento) e quantos tiveram a **guia encerrada na semana**;
- **aderência à programação**: das guias programadas para aquela semana, quantas tiveram serviço
  executado no equipamento, quantas ficaram para trás e quantos serviços saíram sem estar
  programados;
- quantos tocaram **equipamento indisponível**;
- o mesmo recorte por **gerência**, **polo** e **veículo**, mais a lista completa dos serviços.

**Quem mais executou** põe esses recortes em ranking: a barra é o total de serviços, a parte cheia é
o que tinha guia de inspeção e a parte clara é o que foi executado sem guia. A cor é sempre a da
gerência — a posição mostra o ranking, a cor mostra de quem é o volume — e cada gráfico vem com a
tabela ao lado. No ranking de veículos aparecem os 12 maiores; a tabela traz todos.

**Ritmo: serviços por dia útil** compara a média diária do período com o esperado pela capacidade
cadastrada (equipes × serviços por equipe/dia). A barra é o realizado, a marca preta é o esperado e
o rótulo diz o percentual atingido. O esperado de cada polo é o da gerência rateado igualmente entre
os polos dela, porque as equipes são cadastradas por gerência e não por polo — está escrito na tela.
Sem equipe cadastrada não há esperado, e o painel avisa em vez de inventar um.

A comparação do que foi encerrado exige duas fotos de guias; com uma só, o painel avisa.

### Tipo de serviço e ligação com indisponibilidade

Na seção de tipo de serviço, uma **rosca** mostra a participação de cada tipo no total, **um tipo
por fatia**, com o percentual escrito na fatia e a legenda trazendo nome, quantidade e percentual
de todos. Ao lado, a **ligação com indisponibilidade**, que soma duas regras e não conta ninguém
duas vezes:

1. **pelo tipo de serviço** — `Indisponível - Bateria`, `Indisponível - Telecontrole` e
   `Indisponível - Equipamento` nascem da indisponibilidade, então contam sozinhas, independentemente
   de o equipamento ainda estar na lista;
2. **pelo equipamento** — guia de qualquer outro tipo cujo equipamento continue na lista de
   indisponíveis da semana.

O card mostra o total ligado, a quebra por tipo e o quanto vem de cada regra. Sem a planilha de
indisponibilidade carregada na outra aba, só a primeira regra é contada, e o card diz isso. Essa
mesma classificação em três níveis colore as colunas por tipo de serviço no modo TR, marca as guias
no detalhe (`tipo ind.` / `equip. ind.`), alimenta o filtro **Vínculo** e separa os grupos do bloco
Prioridade na programação.

## Ambiente Medidas SAP

Cada linha da exportação é **uma medida**: um código (`CóMd`) preso a uma nota de serviço (`Nota`).
A mesma NS pode carregar mais de uma medida, então a chave de cada registro é
**processo + NS + código** — é por ela que o painel deduplica e descobre o que entrou e o que saiu
entre duas fotos.

As duas exportações têm o **mesmo layout**, e o que as separa é o **processo**, lido do rodapé
“Filtros aplicados” da própria planilha (`Processo é Gestão Campo`). Não achando o rodapé, vale o
nome do arquivo. Por isso os dois arquivos entram de uma vez e viram uma base só, com o processo
como dimensão e como filtro.

Colunas lidas: `CóMd`, `Nota`, `Status`, `StatUsuár.`, `Iníc/planj`, `Responsável`, `Regional` e
`Localiz.`. A `Localiz.` vira o polo do painel (`REG-PT` → **PO**, `REG-IT` → **TB**, os outros sete
são diretos), e a `Regional` confere com a gerência — então o **filtro de escopo do topo vale nas
três abas** e trocar de aba mantém o recorte.

As telas:

- **Resumo** — medidas em carteira, NS distintas, NS com 2+ medidas, entraram e saíram.
- **Movimento** — cascata `foto anterior → saíram → entraram → foto atual` e evolução semanal.
  “Saíram” é o que não aparece mais na exportação: pode ter sido concluída, cancelada ou trocada de
  processo, e a planilha não diz qual. Está escrito na tela.
- **Status do usuário** — a tela principal. Colunas por `StatUsuár.`, que abrem em TR, gerência ou
  processo. A cor separa as **famílias** `ABER` e `ANDM` (o prefixo do código); os sufixos
  (`PEND`, `SUSP`, `CORE`, `ENVI`, `RTEC`) aparecem como status próprios, cada um na sua coluna.
- **Rótulo de cada status** — o SAP entrega o código cru. Escreva ao lado o nome que a reunião
  entende e ele passa a valer no gráfico, nos filtros, na lista e no PDF. Fica salvo no navegador.
- **Status de prazo** — `EM ATRASO`, `VENCE HOJE`, `VENCE 7 DIAS` e `NO PRAZO`, como o SAP entrega.
  O painel **não recalcula** isso: a data de vencimento não vem na exportação, só o início
  planejado, então recalcular seria inventar. O início planejado aparece na lista de detalhe e em
  mais nada.
- **NS e medidas** — quantas NS carregam 1, 2, 3 ou mais medidas, quantas medidas vêm de NS
  compartilhada, e a lista das NS que carregam mais de uma, com os códigos de cada.
- **Códigos de medida** — ranking por volume, com o número de NS de cada código, e a tabela onde
  você escreve o **nome de cada medida** (também salvo no navegador).
- **Polo, gerência e responsável** — volume por cada um, com a variação contra a foto anterior.
- **Detalhe** — todas as medidas, com filtros de **múltipla escolha** (processo, medida, status,
  responsável, gerência, polo), busca por NS ou código e ordenação por qualquer coluna. As abas
  mostram o que entrou, o que saiu e as medidas de NS com 2+ medidas.

Este módulo **não cruza com as guias de inspeção**: a exportação não traz número de equipamento nem
de guia, então qualquer ligação registro a registro seria invenção.

## Ambiente Baterias

A pergunta do módulo não é o percentual vencido: é **quantas baterias comprar**. Cada linha da
exportação é um religador; o que vira pedido é a **bateria**, e cada modelo de relé leva de 1 a 6
unidades — por isso o número de baterias é sempre bem maior que o de religadores.

Colunas lidas: `N_Serie`, `Fabricante`, `Modelo`, `Dispositivo`, `Conjunto`, `Data baterias` e
`Gerência`. O conjunto vira o polo tirando o **T** da frente (`TFR` → FR, `TPM` → PM), então o
filtro de escopo do topo vale nas quatro abas.

**As duas regras que definem o passivo:**

1. A bateria vence **2 anos** depois da data em `Data baterias`. Regra fixa.
2. Linha **sem data legível conta como vencida**, por falta de atualização do cadastro. Isso infla o
   passivo de propósito, para não esconder risco, e a faixa aparece separada em todas as telas.

**Sem série de fotos semanais.** Cada carga entra como um arquivo; a comparação é entre dois
arquivos e responde só uma coisa: *avançou?*. A chave é **série + dispositivo** — a mesma série
aparece em equipamentos diferentes no cadastro, então só a série não serve.

As telas:

- **Resumo** — baterias a comprar, religadores vencidos, passivo, o que vence no horizonte, trocadas.
- **Plano de compra** — por tipo de bateria: passivo, o que vence dentro do horizonte, total e
  quantas por mês. O **horizonte é editável** (1 a 36 meses): é a resposta para "em quanto tempo
  quero zerar o passivo".
- **Curva de vencimento** — quantas baterias vencem em cada um dos próximos 24 meses, para o pedido
  não ser dimensionado duas vezes. O acumulado fica na tabela.
- **Passivo por polo/gerência** e **idade da bateria**, em faixas.
- **Vencimento por modelo de relé** — % vencido de cada modelo, com a bateria que ele usa ao lado.
  Mostra quando o problema é de lote ou fabricante, e não só de idade.
- **Avanço** — entre dois arquivos: trocadas, quantas venceram no intervalo, datas preenchidas,
  variação do passivo, e a lista das trocas com data anterior × atual.
- **Cruzamento** — pelo número do dispositivo: bateria vencida que também está indisponível no
  SCADA, que tem guia aberta (e quantas dessas são guias do tipo `Indisponível - …`), e as que estão
  **sem tratamento nenhum**.
- **De-para de baterias** — quantas unidades e de que tipo cada modelo leva. Já vem com o que a
  engenharia informou; o que você escrever vale por cima e fica salvo no navegador. Modelo sem
  de-para **não entra na conta de compra** e aparece em vermelho, para o número nunca ficar menor do
  que deveria sem aviso.
- **Divergências de cadastro** — sem data, fabricante fora do esperado para o modelo, dispositivo
  repetido, data no futuro, data anterior a 2010, sem número de dispositivo e conjunto fora do
  Triângulo. É a lista de correção da base.
- **Detalhe** — todos os religadores, com filtros de múltipla escolha e abas para vencidas, sem data
  e trocadas.

## Ambiente PSVT

Cada linha é um **PI** com um valor de **compensação em R$**. A pergunta é quanto dinheiro está
concentrado nos maiores casos e se esse número sobe ou desce de uma semana para a outra.

Colunas lidas: **B** (polo), **C** (cidade), **D** (PI), **E** (cliente), **F** (compensação em R$),
**G** (ações propostas), **H** (responsável), **I** (previsão) e **J** (observação). Com uma coluna
**Semana** ou **Data** no início, a data vale linha a linha e um arquivo acumulado entra de uma vez;
sem ela, vale a data carimbada no painel. Cada data presente no arquivo substitui a foto daquele dia,
e a chave de um registro é **data + PI**.

**Se a coluna PI vier vazia**, o painel monta uma chave com **polo + cidade**, numerando quando o par
se repete (`S/PI · PO/Coromandel #2`), e avisa quantas linhas entraram assim. É o que permite comparar
semanas e guardar a tratativa mesmo sem número de PI — mas duas semanas só se encontram enquanto o
polo, a cidade e a ordem das linhas forem os mesmos. Com a coluna PI preenchida, o acompanhamento
fica firme.

As telas:

- **Resumo** — o TOP em compensação e a variação contra a foto anterior, o total da tela, o peso do
  TOP no total, o maior PI e quantos estão sem data de previsão. Abaixo dos indicadores, a
  **participação de cada responsável**: uma pizza com o percentual que cada um carrega da compensação
  em tela, o valor em R$ ao lado de cada nome e, no miolo, o total. A partir do nono responsável as
  fatias menores entram somadas em cinza (`+ N responsáveis`) — a tabela do botão **Tabela** traz
  cada um, com valor, percentual e a foto anterior para comparar.
- **Evolução do TOP** — uma coluna por foto com o valor do TOP e a variação escrita embaixo de cada
  uma. É o acompanhamento semanal: de R$ 1,5 mi para R$ 1,7 mi é +R$ 222 mil, e aparece assim.
  Aumento sai em vermelho, porque aqui subir é ruim.
- **TOP** — os maiores PI em barras, coloridos pela gerência, identificados por **nome do cliente e
  número do PI**, com polo e cidade ao lado, a variação de cada um contra a foto anterior e a marca
  **novo** para quem entrou. Sem nome e sem PI na planilha, a barra cai na ação proposta, para o
  ranking não ficar com todos os rótulos iguais. Abaixo, a tabela de tratativa.
- **Composição** — compensação somada por **polo**, **responsável**, **cidade** ou **ação proposta**,
  com a variação de cada recorte.
- **Detalhe** — todos os PI, com filtros de múltipla escolha e abas para o TOP, os que entraram, os
  que saíram e os sem data de previsão.

**O valor é lido em qualquer formato**: número puro com formatação de moeda, `R$ 1.234,56`,
`1,234.56`, negativo entre parênteses, e o `529.80999999999995` que o Excel grava quando a célula é
moeda (vira R$ 529,81, arredondado em duas casas). Ponto só é separador de milhar quando se repete
(`1.234.567`) ou quando sobram exatamente três casas (`1.234`). Se a coluna mapeada vier vazia ou zerada, o painel **procura
sozinho** a coluna que realmente tem dinheiro e avisa qual usou. A mensagem de carga lista de que
coluna veio cada campo, e se nenhum valor for lido ela diz isso em vermelho em vez de mostrar
R$ 0 calado.

**O tamanho do TOP é editável** no alto da página (1 a 200, padrão 20), e todo o módulo recalcula —
inclusive a série histórica. Os filtros de gerência, polo, cidade, responsável e ação **recompõem o
TOP dentro da seleção**: o TOP 20 de um polo é o dos 20 maiores daquele polo, não um recorte do TOP
geral.

**Previsão e Observação são editáveis** na tabela do TOP e no detalhe. A previsão aceita **data**
(`12/03/2026`, que vira data) ou **texto livre** (`Pendente`, `a definir`) — o KPI e a aba contam
quem está **sem data**, então um `Pendente` aparece ali. O que você escreve vale por
cima do que veio na planilha, fica preso à **chave da linha**, sobrevive às próximas cargas e ao
**Apagar base PSVT**, e a célula editada ganha uma marca verde à esquerda. Apagar o conteúdo devolve
o valor original da planilha.

## Regras de negócio embutidas

| Estrutura | Definição |
|---|---|
| Superintendência | `TR` — todos os polos |
| Gerência `TR/PM` | PM, PO, BD |
| Gerência `TR/UA` | UR, AX, FR |
| Gerência `TR/UD` | UL, AG, TB |
| Execução Regional | Regional - Automação, Regional - Manutenção |
| NGE-TR | Não Lançado, Regional - RD, Regional - Transporte, Regional - Projetos, Oficina |
| Demais responsáveis | apresentados individualmente, com o texto da coluna F |
| `Localiz.` do SAP | REG-PM→PM, REG-PT→PO, REG-BD→BD, REG-UR→UR, REG-AX→AX, REG-FR→FR, REG-UL→UL, REG-AG→AG, REG-IT→TB |
| Família do `StatUsuár.` | o prefixo: `ABER…` ou `ANDM…` |
| Chave de uma medida | processo + NS + código (`CóMd`) |
| `Conjunto` do cadastro | o polo sem o T da frente: TFR→FR, TPM→PM, TTB→TB |
| Vida útil da bateria | 24 meses a partir de `Data baterias` |
| Bateria sem data | conta como vencida, por falta de atualização do cadastro |
| Chave de um religador | série + dispositivo |
| Chave de um PI (PSVT) | data da foto + número do PI; sem número de PI, data + polo + cidade + ordem |
| TOP do PSVT | os N maiores valores de compensação dentro dos filtros em uso |

## Rotina semanal

Toda semana, na mesma ordem. O escopo (gerência e polo) do topo vale para as quatro abas, então
escolha o recorte uma vez e ele acompanha a troca de aba.

| # | Aba | Arquivo | Modelo de atualização | Como a comparação funciona |
|---|---|---|---|---|
| 1 | **Indisponibilidade** | a planilha única, com a semana na coluna A | substitui a base inteira | duas datas da coluna A, escolhidas nos seletores |
| 2 | **Execução** · guias | `AbertoTR` + `AndamentoTR`, data da coleta na coluna A | repõe os pares **data + polo** do arquivo | foto atual × foto anterior |
| 3 | **Execução** · executados | arquivo único acumulado do G-DIS OP | acrescenta, ignorando repetidos | janela entre as duas fotos de guias |
| 4 | **Medidas SAP** | Gestão Campo + Gestão Equipamento, data na coluna A | repõe os pares **data + processo** | foto atual × foto anterior |
| 5 | **Baterias** | cadastro de religadores | cada carga vira um arquivo novo | dois arquivos escolhidos nos seletores |
| 6 | **PSVT** | a planilha de PSVT, data na coluna A | substitui as datas presentes no arquivo | foto atual × foto anterior |

Regras que valem para as quatro:

- **Recarregar o mesmo arquivo não duplica nada.** Cada módulo tem a sua chave: equipamento
  (indisponibilidade), solicitação (guias), serviço + data + equipamento (executados), processo +
  NS + código (medidas), série + dispositivo (baterias).
- **Não é preciso arrumar a planilha.** Cabeçalho fora da primeira linha, célula mesclada, várias
  abas, `.xls` que é tabela HTML, data como número de série do Excel e acento no nome da coluna já
  são tratados.
- **O formato é reconhecido pelo conteúdo, não pela extensão.** Um `.xlsx` salvo com o nome `.xls`
  abre normalmente, e o mesmo vale para “Texto Unicode” (UTF-16) e CSV. Campo entre aspas com
  quebra de linha dentro — o caso do campo de observações — é lido inteiro.
- **Ao acrescentar a coluna de data, salve como `.xlsx`.** Os arquivos do sistema (`AbertoTR`,
  `AndamentoTR`) são tabelas HTML com extensão `.xls`; ao editá-los no Excel, dois formatos de
  saída quebram a leitura, e o painel reconhece os dois e explica o que fazer:
  - **Pasta de Trabalho do Excel 97-2003 (`.xls` binário)** — formato proprietário antigo, que o
    painel não abre.
  - **Página da Web (`.htm`)** — guarda só a casca: os dados vão para uma pasta
    `<nome>_arquivos` ao lado, e o arquivo selecionado fica sem nenhuma linha. Nesse caso dá para
    carregar direto o `sheet001.htm` de dentro dessa pasta, que funciona.
- **Um arquivo por polo ou um só com todos dá no mesmo** nas guias e nas medidas, porque a reposição
  é por data + polo e por data + processo.
- **Antes da reunião**, confira o rodapé: ele diz a origem, a quantidade de registros e as datas de
  cada base carregada.
- **Tudo fica guardado no navegador daquela máquina.** Trocar de computador ou limpar os dados do
  navegador zera o histórico, a programação e as anotações.

## Qual versão do arquivo está aberta

O rodapé de qualquer uma das abas começa com **Painel versão dd/mm/aaaa**. Use isso para confirmar
que o arquivo aberto é o mais recente antes de procurar uma tela nova.

## Como carregar as baterias

Na aba **Baterias**, botão **Carregar dados**, e selecione o cadastro de religadores. Aceita
`.xls` (inclusive o `.xls` que é tabela HTML), `.xlsx` e `.csv`. Cada carga vira um arquivo na
base, identificado pelo nome e pela data em que foi carregado; escolha dois nos seletores do topo
para ver o avanço. O painel lista os arquivos e permite remover qualquer um. **Apagar base de
baterias** limpa os dados e **preserva o de-para** que você ajustou.

## Como carregar as medidas SAP

Na aba **Medidas SAP**, botão **Carregar dados**: selecione as exportações (`Gestão Campo` e
`Gestão Equipamento` podem ir juntas). Aceita `.xlsx`, `.xls` e `.csv`.

- Com uma coluna **Semana** ou **Data** no início da planilha (colunas originais de B em diante), a
  data vale linha a linha e um arquivo acumulado carrega várias coletas de uma vez. Sem ela, vale a
  data confirmada no painel.
- A carga repõe apenas os pares **data + processo** presentes no arquivo, então carregar Gestão
  Campo não apaga Gestão Equipamento da mesma data, e recarregar o mesmo arquivo não duplica nada.
- O painel lista as fotos carregadas e permite remover qualquer uma. **Apagar base de medidas**
  limpa os dados e **preserva os rótulos** que você escreveu.

## Como carregar as guias de execução

Na aba **Execução**, botão **Carregar dados** (no alto da página): confirme a data da foto e use
**1 · AbertoTR e AndamentoTR** para as guias, ou **2 · Executados G-DIS OP** para a lista do que foi
executado em campo. Os arquivos são lidos como saem do sistema — eles são tabelas HTML com extensão `.xls`, e o
leitor abre esse formato direto, sem precisar reabrir e salvar no Excel. Também aceita `.xlsx` e
`.csv`.

- **Guias**: se a planilha tiver a data na coluna A, a data do painel é ignorada e cada coleta do
  arquivo vira uma foto; senão vale a data confirmada no painel. Carregar de novo uma data já
  existente substitui a foto daquele dia.
- **Executados**: a data está na coluna F de cada linha, então basta manter um arquivo só e ir
  acrescentando as semanas — o painel acumula e ignora repetidos.
- **Um arquivo por polo ou um só com vários polos**: tanto faz. O polo vem da própria linha e a
  carga de guias repõe apenas os pares **data + polo** presentes no arquivo, então seis arquivos de
  polos diferentes com a mesma data dão o mesmo resultado de um arquivo único, e recarregar o de um
  polo não apaga os outros.

O painel lista as fotos carregadas e permite remover qualquer uma. **Carregar exemplo** gera quatro
semanas fictícias para conhecer as telas antes de ter histórico real; **Apagar base de execução**
limpa tudo antes de subir a planilha de verdade.

## Como analisar com os dados reais

Não é preciso editar o arquivo. Abra o dashboard e clique em **Dados da planilha**
(ao lado de "Limpar filtros"). Duas formas:

1. **Colar do Excel** — selecione as seis colunas na planilha (A a F, sem precisar tirar o
   cabeçalho), `Ctrl+C`, cole na caixa e clique em **Carregar**.
2. **Abrir arquivo** — selecione a planilha em `.xlsx` (ou `.csv`). No `.xlsx` a leitura procura a
   primeira aba com dados válidos, então uma aba de instruções antes da base não atrapalha.

A página lê a coluna A como data da coleta, monta as semanas e recalcula tudo: variação,
novos, regularizados, faixas, responsabilidade, polos e gerências.

O leitor aceita, sem configuração:

- planilha `.xlsx` direto do Excel, ou texto separado por tabulação (colagem), `;` ou `,`,
  com campos entre aspas;
- datas como data do Excel, `dd/mm/aaaa`, `aaaa-mm-dd`, `dd-mm-aaaa` ou número de série;
- células vazias no meio da linha, sem deslocar as colunas;
- dias como `42`, `1.234` ou `42,0`;
- acento, caixa alta e espaço extra nos textos — `REGIONAL - AUTOMAÇÃO` cai em Execução
  Regional do mesmo jeito que `Regional - Automação`.

Depois de carregar, o painel informa quantas linhas entraram e o que ficou de fora:
linha com data inválida, equipamento repetido na mesma data, polo fora do mapa de gerências
(vai para "Não mapeado") e responsável novo (passa a aparecer sozinho nos gráficos).
Nada é descartado em silêncio.

Com **Guardar neste navegador** marcado, os dados continuam ao reabrir a página no mesmo
computador. **Voltar à demonstração** desfaz. **Copiar bloco para o arquivo** gera o trecho
pronto para colar no lugar da seção `BASE DE DADOS` dentro do HTML — use quando quiser
distribuir o arquivo já com os dados dentro, para quem for abrir não precisar colar nada.

## Formato do bloco embutido

Quem preferir editar o arquivo direto substitui a seção `BASE DE DADOS`:

```js
const DEMO_SEMANAS      = ["2026-06-08", ...];             // coluna A, uma entrada por data de coleta
const DEMO_TIPOS        = ["Religador", ...];              // coluna C
const DEMO_POLOS        = ["PM", "PO", ...];               // coluna D
const DEMO_RESPONSAVEIS = ["Regional - Automação", ...];   // coluna F
// [semana, equipamento, tipo, polo, dias, responsável] — índices das listas acima
const DEMO_LINHAS = [ [0, 105321, 0, 3, 42, 1], ... ];
```

Para mudar o agrupamento de responsabilidades, edite `MAPA_RESP`; para mudar polos e
gerências, `MAPA_GERENCIA`. São os dois únicos pontos de configuração da estrutura.
