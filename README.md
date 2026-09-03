# Dashboard NGE — Análise Equipamentos TRIÂNGULO

Página única em HTML puro (`dashboard-nge.html`), sem bibliotecas externas e com os dados
embutidos no próprio arquivo. Abre com duplo clique em qualquer navegador, funciona offline
e pode ser projetada direto na reunião.

## O que o dashboard mostra

Toda a leitura é feita entre **duas datas da coluna A** (semana atual × semana de comparação,
por padrão a imediatamente anterior):

- **Resumo** — total de indisponíveis, variação, novos, regularizados, acima de 120 dias e média de dias.
- **Movimento** — cascata `semana anterior → regularizados → novos → semana atual` e evolução das 13 semanas.
  Quando o total não muda mas os equipamentos mudam, a diferença aparece como **Novos** e **Regularizados**
  (comparação por número de equipamento, não por quantidade).
- **Faixa de dias** — até 30, 30 a 90, 90 a 120 e acima de 120 dias, com variação e composição × semana anterior.
- **Responsabilidade** — carteira por grupo, cruzamento responsável × faixa de dias e um gráfico de
  colunas por responsável que abre em **TR**, **Gerência** ou **Polo** (a cor mostra a que grupo
  cada responsável pertence, ou a gerência de cada faixa).
- **Polo e tipo** — com TR selecionado o gráfico mostra as três gerências; ao escolher uma gerência,
  abre nos polos dela. Ao lado, a mistura por tipo de equipamento.
- **Detalhe** — todos os indisponíveis da semana, ordenados por dias, com filtros próprios de
  responsável (o texto exato da coluna F), gerência, polo, tipo e número do equipamento. Qualquer
  coluna ordena ao ser clicada. As abas **Novos**, **Regularizados** e **Permanecem** mostram os
  mesmos recortes da comparação semanal.
- **Anotações e evidências** — observações da reunião, anotação por equipamento e imagens.

Filtros de semana, comparação, gerência, polo, tipo e responsabilidade valem para a página inteira.
Cada gráfico tem o botão **Tabela**, que troca o desenho pelos números.

## Anotar, anexar print e gerar o PDF

- **Anotação por equipamento** — na lista de detalhe, clique na coluna **Anotação** da linha, escreva
  e clique fora (ou `Ctrl+Enter`). A linha fica marcada e a anotação aparece no bloco
  *Anotações e evidências*, junto com tipo, polo, gerência, dias e responsável do equipamento.
- **Prints** — arraste a imagem para a área de evidências, cole com `Ctrl+V` ou selecione o arquivo.
  Cada imagem aceita uma legenda. As imagens são reduzidas para caber no navegador.
- **Relatório PDF** — o botão **Relatório PDF** abre a impressão do navegador; escolha *Salvar como
  PDF*. Sai em A4 paisagem com cabeçalho (semana, comparação, escopo e data de emissão), todos os
  indicadores e gráficos, a lista de detalhe como estiver filtrada na tela e, na última página, as
  anotações e as evidências. Para um relatório de um responsável específico, filtre a lista antes
  de imprimir.

Anotações, legendas e imagens ficam guardadas no navegador do computador em uso.

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
