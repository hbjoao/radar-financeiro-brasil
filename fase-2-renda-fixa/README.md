# Fase 2 (Intermediário): renda fixa com Tesouro Direto

**Status:** não iniciada

## Pergunta de negócio

Vale mais a pena Tesouro Selic, IPCA+ ou prefixado?

## Fonte dos dados

- Tesouro Transparente: "Preços e Taxas dos Títulos do Tesouro Direto" (CSV)
- Séries da Fase 1 (IPCA e Selic) para calcular o juro real

## Tratamento (Power Query)

- [ ] Separar tipo de título e data de vencimento
- [ ] Tratar a tabela grande (milhares de linhas)
- [ ] Mesclar com as séries da Fase 1

## Modelagem

Primeira tabela fato, com dimensões Calendário e Título (inclua print do modelo em `prints/`).

## Medidas DAX

```dax
-- Rentabilidade acumulada, juro real, comparação com o mesmo período do ano anterior
```

## Resultados e insights

3 a 5 conclusões que o relatório permite tirar.

## Prints

![Visão geral](prints/visao-geral.png)

## Aprendizados e dificuldades

O que foi novo, o que deu errado e como resolveu.

## Arquivo

[`fase-2-renda-fixa.pbix`](fase-2-renda-fixa.pbix)
