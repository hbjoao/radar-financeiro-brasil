# Fase 4 (Ponta a ponta): carteira de ações, risco e publicação

**Status:** não iniciada

## Pergunta de negócio

Como a carteira simulada performa e qual o risco dela?

## Fonte dos dados

- Cotações históricas das ações do setor analisado na Fase 3 (Yahoo Finance via script Python no Power Query, ou CSV exportado)
- CDI da Fase 1 como taxa livre de risco

## Tratamento

- [ ] Coleta das cotações (script ou CSV)
- [ ] Tratamento de dias sem pregão e ajustes por proventos
- [ ] Definição da carteira simulada (ativos e pesos)

## Medidas DAX

```dax
-- Retorno diário e acumulado, volatilidade anualizada, drawdown máximo, índice de Sharpe, comparação com o Ibovespa
```

## Publicação

- [ ] Publicado no Power BI Service
- [ ] Atualização agendada (ou documentada)
- [ ] Navegação entre páginas com botões e marcadores

## Resultados e insights

3 a 5 conclusões que o relatório permite tirar.

## Prints

![Visão geral](prints/visao-geral.png)

## Aprendizados e dificuldades

O que foi novo, o que deu errado e como resolveu.

## Arquivo

[`fase-4-carteira.pbix`](fase-4-carteira.pbix)
