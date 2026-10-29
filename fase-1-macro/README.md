# Fase 1 (Básico): indicadores macroeconômicos

**Status:** em andamento

## Pergunta de negócio

Como Selic, inflação e dólar se comportaram nos últimos anos?

## Fonte dos dados

- API do Banco Central (SGS), sem cadastro
- Exemplo de endpoint: `https://api.bcb.gov.br/dados/serie/bcdata.sgs.432/dados?formato=json`

| Série | Código SGS | Periodicidade |
| --- | --- | --- |
| Selic meta | 432 | Por decisão do Copom |
| IPCA (variação mensal) | 433 | Mensal |
| Dólar PTAX (venda) | 1 | Diária |
| CDI | 12 | Diária |

## Tratamento (Power Query)

Documente aqui cada passo e o motivo:

- [ ] Conexão via *Web* e expansão do JSON
- [ ] Conversão de tipos (data no formato dd/MM/yyyy e valor com separador decimal)
- [ ] Tratamento de erros e valores nulos
- [ ] Criação da tabela calendário

## Modelagem

Descreva as tabelas e os relacionamentos (inclua um print do modelo em `prints/`).

## Medidas DAX

```dax
-- Cole aqui as medidas e explique em uma linha o que cada uma faz
Último valor = 
CALCULATE ( MAX ( 'Selic'[Valor] ), LASTDATE ( 'Calendário'[Data] ) )
```

## Resultados e insights

Escreva 3 a 5 conclusões que o relatório permite tirar. Exemplo: "A Selic subiu X pontos entre A e B, enquanto o IPCA acumulado em 12 meses chegou a Y%."

## Prints

![Visão geral](prints/visao-geral.png)

## Aprendizados e dificuldades

O que foi novo, o que deu errado e como resolveu. Isso mostra raciocínio a quem avalia o portfólio.

## Arquivo

[`fase-1-macro.pbix`](fase-1-macro.pbix)
