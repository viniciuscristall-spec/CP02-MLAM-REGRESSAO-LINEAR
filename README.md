# PIB x Fluxo de veículos pedagiados (ABCR)

Trabalho de Machine Learning que investiga se a atividade econômica do Brasil (PIB) acompanha o fluxo de veículos nas rodovias pedagiadas (Índice ABCR) e se uma **regressão linear simples** consegue prever o fluxo a partir do PIB.

## Integrantes

| RM | Nome |
|----|------|
| 569062 | André Debiazzi |
| 572718 | Kaique da Silva |
| 572049 | Vinicius Cristal |

## Pergunta do trabalho

> Quando o PIB cresce, o fluxo de veículos nas rodovias pedagiadas também cresce? E dá para estimar um a partir do outro?

## Dados

| Série | Fonte | Frequência | Base |
|-------|-------|-----------|------|
| PIB (índice de volume, Brasil) | IBGE / SIDRA, tabela 1620 (`tabela1620.csv`) | Trimestral | média de 1995 = 100 |
| Fluxo de veículos (Índice ABCR, série original) | ABCR (`abcr_0826.xlsx`, aba `(C) Original`) | Mensal | 1999 = 100 |

As duas séries foram convertidas em **média anual** e só entraram anos completos (4 trimestres e 12 meses), na janela **2006–2025** (20 anos).

## Metodologia

1. **Carregamento dos dados:** leitura do CSV do SIDRA e da planilha da ABCR.
2. **Tratamento:** média anual das duas séries e tabela final `Ano`, `PIB_indice`, `ABCR_indice`.
3. **Correlação:** Pearson, Spearman, Pearson sem 2020 e Pearson das variações anuais, mais gráfico de dispersão.
4. **Modelo:** `LinearRegression` com `X = PIB_indice` e `y = ABCR_indice`. Treino em 2006–2021 e teste em 2022–2025, mantendo a ordem cronológica (sem embaralhar).
5. **Avaliação:** MAE, MSE, RMSE e R² no conjunto de teste, comparação ano a ano e diagnóstico de viés e extrapolação.

## Principais resultados

- **Correlação forte:** Pearson ≈ 0,96 entre os índices anuais.
- **Previsão razoável, com ressalvas:** no teste, MAE ≈ 4,9 pontos (cerca de 3%) e R² ≈ 0,48.
- **Viés:** o modelo superestimou o fluxo nos quatro anos de teste. O PIB de 2022–2025 ficou acima de tudo que o modelo viu no treino (extrapolação) e a relação parece ter se deslocado para baixo.
- **Cuidado:** são só 4 anos de teste e correlação não é causalidade.

## Como executar

**No Google Colab (recomendado)**
1. Abra `PIB_x_ABCR_regressao.ipynb` no Colab.
2. Execute todas as células (`Ambiente de execução > Executar tudo`).
3. Se os arquivos de dados não puderem ser baixados do GitHub, o notebook pede o upload de `tabela1620.csv` e `abcr_0826.xlsx`.

**Localmente**
```bash
pip install pandas numpy matplotlib scikit-learn openpyxl jupyter
jupyter notebook PIB_x_ABCR_regressao.ipynb
```
Deixe `tabela1620.csv` e `abcr_0826.xlsx` na mesma pasta do notebook.

## Arquivos

| Arquivo | Descrição |
|---------|-----------|
| `PIB_x_ABCR_regressao.ipynb` | Notebook completo (dados, correlação, modelo, avaliação e conclusões) |
| `tabela1620.csv` | PIB trimestral (SIDRA/IBGE) |
| `abcr_0826.xlsx` | Índice ABCR mensal |
| `pib_abcr_anual.csv` | Tabela anual gerada pelo notebook |
| `dispersao_pib_abcr.png` | Gráfico de dispersão gerado pelo notebook |

## Tecnologias

Python, pandas, NumPy, Matplotlib, scikit-learn e openpyxl.
