# Dashboard NGE — Análise Equipamentos TRIÂNGULO

Página única em HTML puro (`dashboard-nge.html`), sem bibliotecas externas. Abre com duplo
clique em qualquer navegador, funciona offline e pode ser projetada direto na reunião.

São dois ambientes, escolhidos pelas abas do topo:

- **Indisponibilidade** — equipamentos indisponíveis no SCADA, a partir da planilha semanal.
- **Execução** — guias de inspeção na carteira da Execução Regional, a partir dos arquivos
  `AbertoTR` e `AndamentoTR`.

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

Isso alimenta duas telas:

- **Serviços previstos por equipe** — uma grade de 8 semanas com `programadas / capacidade` em cada
  célula (capacidade = serviços por dia × 5 dias × dedicação; vermelho acima dela). Clicando numa
  célula, abaixo aparece **a lista dos serviços previstos** daquela equipe naquela semana: guia,
  tipo, polo, município, idade, equipamento e se ele está indisponível.
- **Projeção da fila** — parte do total de hoje e, a cada semana, subtrai o que sai e soma o que
  entra. Três linhas: pelo programado, pela capacidade teórica e uma referência cinza de não
  executar nada. Quando uma delas chega a zero dentro do horizonte, o gráfico marca a semana.

A programação é local, feita à mão, e não vai para o G-DIS OP.

### Executado na semana · G-DIS OP

O terceiro arquivo é a lista do que as equipes **de fato executaram em campo**. É lido pelas colunas
**D** (nº do serviço), **F** (data e hora), **G** (veículo), **K** (polo executora) e **M**
(equipamento) — se o cabeçalho trouxer esses nomes, ele é reconhecido; senão vale a posição da
letra. Na carga, o painel mostra as primeiras linhas lidas para conferência.

Aqui a data já vem na própria linha (coluna F), então pode ser **um arquivo único acumulado**: cada
serviço entra pela sua data de execução e recarregar o mesmo arquivo não duplica (serviço + data +
equipamento repetidos são ignorados). O painel guarda tudo e o cruzamento usa a janela entre a foto
de guias comparada e a foto selecionada — sem foto anterior, os sete dias antes dela.

O cruzamento usa o **equipamento** e responde o que o planejamento sozinho não responde:

- quantos serviços foram executados em equipamento **com guia de inspeção** e quantos **sem guia
  nenhuma** — estes últimos são execução fora do controle;
- quantos foram feitos e a **guia continua aberta** (pode ser outra guia do mesmo equipamento, ou
  falta de encerramento) e quantos tiveram a **guia encerrada na semana**;
- **aderência à programação**: das guias programadas para aquela semana, quantas tiveram serviço
  executado no equipamento, quantas ficaram para trás e quantos serviços saíram sem estar
  programados;
- quantos tocaram **equipamento indisponível**;
- o mesmo recorte por **polo** e por **veículo**, mais a lista completa dos serviços.

A comparação do que foi encerrado exige duas fotos de guias; com uma só, o painel avisa.

### Cruzamento com a indisponibilidade

A regra: **equipamento indisponível sob `Regional - Automação` deve ter guia**; nas demais
responsabilidades, não. O painel confere isso pelo número do equipamento (nunca pelo tipo de
serviço) e mostra a cobertura, a lista de quem está sem guia — com uma **tolerância em dias**
configurável, para não cobrar guia de equipamento que ficou indisponível ontem — e a lista de quem
já tem, com os dias de indisponibilidade ao lado da idade da guia.

O foco do painel é saber **quais indisponíveis têm guia e se essa guia está programada**: além da
cobertura, há o indicador *com guia, sem programação* e a coluna de programação na lista. O bloco
**Prioridade na programação** compara as guias de equipamento indisponível com as demais — quantas
estão programadas, o percentual, quantas caem na próxima semana e a semana média — que é a
evidência de que elas estão mesmo na frente.

A projeção também estima a redução de indisponíveis, mas por uma **taxa de conversão** editável:
atender a guia não garante o equipamento voltar a ficar disponível, e o painel diz isso.

Na seção de tipo de serviço, uma **rosca** mostra a participação de cada tipo no total, **um tipo
por fatia**, com o percentual escrito na fatia e a legenda trazendo nome, quantidade e percentual
de todos. Ao lado, quanto das guias é de **equipamento que continua indisponível** — vale qualquer
tipo de guia, o que conta é o equipamento ainda estar na lista de indisponíveis. Esse mesmo vínculo
divide as colunas por tipo de serviço no modo TR.

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

## Qual versão do arquivo está aberta

O rodapé de qualquer uma das abas começa com **Painel versão dd/mm/aaaa**. Use isso para confirmar
que o arquivo aberto é o mais recente antes de procurar uma tela nova.

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
