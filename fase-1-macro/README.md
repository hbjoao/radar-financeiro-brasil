# Fase 1 (Básico): indicadores macroeconômicos

**Status:** relatório base concluído (cartões e segmentador). Gráficos de linha em andamento.

## Pergunta de negócio

Como Selic, inflação e dólar se comportaram nos últimos anos?

## Fonte dos dados

API de séries temporais (SGS) do Banco Central do Brasil, sem necessidade de cadastro.

| Série | Código SGS | Periodicidade |
| --- | --- | --- |
| Selic meta | 432 | Por decisão do Copom |
| IPCA (variação mensal) | 433 | Mensal |
| Dólar PTAX (venda) | 1 | Diária |
| CDI | 12 | Diária |

Exemplo de endpoint: `https://api.bcb.gov.br/dados/serie/bcdata.sgs.432/dados?formato=json`

## Tratamento (Power Query)

- [x] Função `fnSGS` que recebe o código da série e o período, consulta a API e devolve uma tabela com `Data` e `Valor`
- [x] Função `fnSGSAnos` que chama a `fnSGS` ano a ano e junta os resultados, para contornar o limite de 1.000 registros por consulta das séries diárias
- [x] Conversão de tipos: data no formato `dd/MM/yyyy` (cultura `pt-BR`) e valor com ponto decimal (cultura `en-US`)
- [x] Repetição automática da chamada em caso de falha temporária da API (erro 502)
- [x] Tabela calendário criada em DAX e marcada como tabela de datas

### Função `fnSGS`

```m
(codigo as number, dataInicial as text, dataFinal as text) as table =>
let
    buscar = (tentativas as number) =>
        let
            r = try Json.Document(
                    Web.Contents("https://api.bcb.gov.br",
                        [RelativePath = "dados/serie/bcdata.sgs." & Number.ToText(codigo) & "/dados",
                         Query = [formato = "json", dataInicial = dataInicial, dataFinal = dataFinal]]))
        in
            if not r[HasError] then r[Value]
            else if tentativas > 0 then Function.InvokeAfter(() => @buscar(tentativas - 1), #duration(0, 0, 0, 3))
            else error r[Error],
    resposta = buscar(4),
    tabela = Table.FromList(resposta, Splitter.SplitByNothing(), {"Registro"}),
    expandido = Table.ExpandRecordColumn(tabela, "Registro", {"data", "valor"}, {"Data", "Valor"}),
    tipos = Table.TransformColumns(expandido, {
        {"Data", each Date.FromText(_, [Format = "dd/MM/yyyy", Culture = "pt-BR"]), type date},
        {"Valor", each Number.FromText(_, "en-US"), type number}})
in
    tipos
```

### Função `fnSGSAnos`

```m
(codigo as number, anoInicial as number, anoFinal as number) as table =>
let
    anos = List.Numbers(anoInicial, anoFinal - anoInicial + 1),
    tabelas = List.Transform(
        anos,
        each fnSGS(codigo, "01/01/" & Text.From(_), "31/12/" & Text.From(_))
    ),
    combinado = Table.Combine(tabelas)
in
    combinado
```

## Modelagem

Modelo simples: uma tabela `Calendario` (dimensão) ligada, por data, a cada uma das quatro séries (`SELIC`, `IPCA`, `DOLAR`, `CDI`), com relacionamentos de um para muitos. As medidas ficam numa tabela à parte, chamada `Medidas`.

```dax
Calendario =
ADDCOLUMNS (
    CALENDAR ( DATE ( 2023, 1, 1 ), DATE ( 2026, 12, 31 ) ),
    "Ano", YEAR ( [Date] ),
    "Mes Num", MONTH ( [Date] ),
    "Mes", FORMAT ( [Date], "MMM", "pt-BR" ),
    "Ano-Mes", FORMAT ( [Date], "yyyy-MM" )
)
```

![Modelo de dados](prints/modelo.png)

## Medidas DAX

```dax
Selic Atual =
VAR UltimaData = MAX ( SELIC[Data] )
RETURN
    CALCULATE ( MAX ( SELIC[Valor] ), SELIC[Data] = UltimaData ) / 100

Dolar Ultimo =
VAR UltimaData = MAX ( DOLAR[Data] )
RETURN
    CALCULATE ( MAX ( DOLAR[Valor] ), DOLAR[Data] = UltimaData )

Dolar Medio = AVERAGE ( DOLAR[Valor] )

IPCA Acum 12m =
VAR UltimaData = MAX ( IPCA[Data] )
VAR Janela =
    FILTER (
        ALL ( IPCA ),
        IPCA[Data] > EDATE ( UltimaData, -12 ) && IPCA[Data] <= UltimaData
    )
RETURN
    IF ( COUNTROWS ( Janela ) = 12, PRODUCTX ( Janela, 1 + IPCA[Valor] / 100 ) - 1 )
```

Pontos que valem registro:

- A Selic vem da API em pontos percentuais (13,75). A divisão por 100 permite usar o formato de porcentagem do Power BI sem multiplicar o valor por 100 de novo.
- A inflação acumulada é calculada multiplicando os fatores mensais (`1 + taxa`), e não somando as taxas. A medida devolve vazio quando há menos de 12 meses de histórico, para não mostrar um acumulado incompleto.

## Resultados

Os indicadores abaixo foram conferidos com fontes públicas (Copom e IBGE):

- **Selic meta: 13,75% ao ano**, após o corte de 0,25 ponto percentual em setembro de 2026, que levou a taxa de 14% a 13,75%.
- **IPCA acumulado em 12 meses: 4,58%** (até setembro de 2026).
- **Dólar PTAX:** R$ 5,3744 na última data disponível e média de R$ 5,2867 no período carregado.
- Com Selic em 13,75% e inflação em 4,58%, o juro real aproximado é de cerca de 8,8% ao ano, calculado como `(1 + Selic) / (1 + IPCA) - 1`. É uma estimativa simples que compara a taxa nominal de hoje com a inflação passada.

## Prints

![Visão geral do relatório](prints/visao-geral.png)

## Próximos passos da fase

- [ ] Gráfico de linhas da Selic meta ao fim de cada mês
- [ ] Gráfico de linhas do IPCA acumulado em 12 meses
- [ ] Gráfico de linhas do dólar
- [ ] Escrever as conclusões a partir dos gráficos (por exemplo, o período de maior alta da Selic e a relação com a inflação)

## Aprendizados e dificuldades

- **Limite de 1.000 registros da API:** as séries diárias passam desse limite em poucos anos. A solução foi buscar um ano por vez e combinar os resultados.
- **Função colada na barra de fórmulas:** o Power Query tratou o código como texto. A função só funciona quando colada no Editor Avançado, substituindo todo o conteúdo padrão.
- **Instabilidade da API (erro 502 e resposta em HTML):** o excesso de chamadas simultâneas aumenta as falhas. Ajudou desativar o carregamento paralelo de tabelas, reduzir o período e incluir repetição automática.
- **Formato de porcentagem:** o botão `%` do Power BI multiplica por 100, então uma Selic de 13,75 aparecia como 1375%. A correção foi dividir por 100 na medida.
- **Aviso "limite de 1000 valores" no filtro do Power Query:** é só um limite da lista de seleção da interface, não dos dados carregados.

## Arquivo

[`fase-1-macro.pbix`](fase-1-macro.pbix)
