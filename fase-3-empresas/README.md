# Fase 3 (Avançado): análise de empresas com dados da CVM

**Status:** não iniciada

## Pergunta de negócio

Quais empresas de um setor têm melhor rentabilidade e menor endividamento?

## Fonte dos dados

- CVM Dados Abertos: DFP (demonstrações anuais), com Balanço (BPA/BPP) e DRE
- Cadastro de companhias abertas

## Tratamento (Power Query)

- [ ] Importar vários CSVs com *Pasta* e função customizada
- [ ] Filtrar `ORDEM_EXERC = "ÚLTIMO"`
- [ ] Ajustar `ESCALA_MOEDA`
- [ ] Pivotar contas (`CD_CONTA`) em colunas

## Modelagem

Esquema estrela: Fato Demonstrações, Dim Empresa, Dim Conta e Dim Calendário (inclua print do modelo em `prints/`).

## Medidas DAX

```dax
-- Margem líquida, ROE, ROA, dívida líquida/EBITDA, liquidez corrente, ranking por setor (RANKX)
```

## Resultados e insights

3 a 5 conclusões que o relatório permite tirar.

## Prints

![Visão geral](prints/visao-geral.png)

## Aprendizados e dificuldades

O que foi novo, o que deu errado e como resolveu.

## Arquivo

[`fase-3-empresas.pbix`](fase-3-empresas.pbix)
