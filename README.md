# Radar Financeiro Brasil

Projeto de portfólio em Power BI com dados financeiros públicos brasileiros, construído em quatro fases, do básico ao avançado. O objetivo é praticar tratamento de dados (Power Query), modelagem e DAX, documentando cada decisão.

## Fases

| Fase | Nível | Pergunta de negócio | Fonte | Status |
| --- | --- | --- | --- | --- |
| [1](fase-1-macro/README.md) | Básico | Como Selic, inflação e dólar se comportaram nos últimos anos? | API do Banco Central (SGS) | Em andamento |
| [2](fase-2-renda-fixa/README.md) | Intermediário | Vale mais a pena Tesouro Selic, IPCA+ ou prefixado? | Tesouro Transparente | Não iniciada |
| [3](fase-3-empresas/README.md) | Avançado | Quais empresas de um setor têm melhor rentabilidade e menor endividamento? | CVM Dados Abertos (DFP) | Não iniciada |
| [4](fase-4-carteira/README.md) | Ponta a ponta | Como a carteira simulada performa e qual o risco dela? | Cotações históricas | Não iniciada |

## Tecnologias

- Power BI Desktop e Power BI Service
- Power Query (linguagem M)
- DAX
- Python (apenas para coleta de cotações, na Fase 4)

## Estrutura do repositório

```
radar-financeiro-brasil/
├── README.md
├── .gitignore
├── fase-1-macro/
├── fase-2-renda-fixa/
├── fase-3-empresas/
├── fase-4-carteira/
└── docs/
    └── imagens/
```

Cada pasta de fase contém:

- `README.md`: pergunta, fonte, tratamento, modelagem, medidas DAX e aprendizados
- `*.pbix`: arquivo do relatório
- `dados/`: amostras pequenas dos dados (arquivos grandes ficam de fora, veja o `.gitignore`)
- `prints/`: capturas de tela do relatório

## Como reproduzir

1. Clone o repositório e abra o `.pbix` da fase desejada no Power BI Desktop.
2. Se a fonte for uma API ou site, atualize os dados em **Página Inicial > Atualizar**.
3. Se a fonte for um arquivo, ajuste o caminho em **Transformar dados > Configurações da fonte de dados**.

## Autor

Seu nome, [LinkedIn](https://www.linkedin.com/) · Formado em Análise e Desenvolvimento de Sistemas.
