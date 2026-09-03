# Dashboard NGE — Equipamentos indisponíveis SCADA

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
- **Responsabilidade** — carteira por responsável e cruzamento responsável × faixa de dias.
- **Polo e tipo** — polos coloridos por gerência, totais por gerência e mistura por tipo de equipamento.
- **Detalhe** — listas de novos, regularizados, maiores prazos e os que permanecem.

Filtros de semana, comparação, gerência, polo, tipo e responsabilidade valem para a página inteira.
Cada gráfico tem o botão **Tabela**, que troca o desenho pelos números.

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

## Como trocar pelos dados reais

Os dados de hoje são uma **base de demonstração** (2.998 registros, 13 semanas) com o mesmo formato
da planilha. Para usar a planilha de verdade, edite o bloco marcado como `BASE DE DADOS` dentro do
arquivo e substitua as quatro listas:

```js
const SEMANAS       = ["2026-06-08", ...];                 // coluna A, uma entrada por data de coleta
const TIPOS         = ["Religador", ...];                  // coluna C
const POLOS         = ["PM", "PO", ...];                   // coluna D
const RESPONSAVEIS  = ["Regional - Automação", ...];       // coluna F
// [semana, equipamento, tipo, polo, dias, responsável] — índices das listas acima
const LINHAS = [ [0, 105321, 0, 3, 42, 1], ... ];
```

Cada linha da planilha vira uma linha de `LINHAS`, com índices das listas em vez de texto repetido.
Se surgir um responsável novo que não seja de Execução Regional nem do NGE-TR, basta incluí-lo em
`RESPONSAVEIS`: ele passa a aparecer sozinho nos gráficos, sem mais nenhum ajuste.
Para mudar o agrupamento, edite `RESP_GRUPO`; para mudar polos e gerências, `GER_POLOS`.
