# Regressão Linear com Dados de Energia Solar (PVGIS)

Exercício de Machine Learning que estima a **potência gerada por um sistema fotovoltaico** a partir de dados do tempo, usando Regressão Linear em Python.

## Nomes dos integrantes

1- Murillo Boyadjian  RM: 570774
2- Renan Eskildssen   RM: 571097
3- Lucas Barros       RM: 571528

## Sobre o projeto

Os dados horários vêm da API pública do **PVGIS** (Joint Research Centre, Comissão Europeia), ferramenta `seriescalc`. O notebook faz o fluxo completo: consulta à API, montagem do DataFrame, inspeção, gráficos, correlação, treino de dois modelos e comparação das métricas.

- **Local:** São Paulo - SP (lat -23,5505 / lon -46,6333)
- **Período:** 2022 (8.760 registros horários)
- **Sistema simulado:** 1 kWp, inclinação de 23°, voltado para o Norte, perdas de 14%

## Variáveis

| Variável | Significado |
|---|---|
| `P` | Potência do sistema (W) — **alvo (y)** |
| `G(i)` | Irradiância no plano do painel (W/m²) |
| `H_sun` | Altura do Sol (graus) |
| `T2m` | Temperatura do ar a 2 m (°C) |
| `WS10m` | Velocidade do vento a 10 m (m/s) |

## Como funciona

1. Requisição à API do PVGIS com `requests`
2. Criação do DataFrame `df` e inspeção (`shape`, `info()`, `isnull()`, `describe()`)
3. Remoção dos registros noturnos (`G(i) = 0`), para o R² não ser inflado por zeros fáceis de prever
4. Gráficos de dispersão e matriz de correlação
5. Separação treino/teste (80% / 20%, `random_state=42`)
6. Treino de dois modelos de Regressão Linear
7. Comparação com MAE, MSE e R²

## Modelos

- **Modelo 1 (`modeloLR1`):** `G(i)`
- **Modelo 2 (`modeloLR2`):** `G(i)`, `H_sun`, `T2m`, `WS10m`

## Resultados

| Modelo | Variáveis utilizadas | MAE | MSE | R² |
|---|---|---|---|---|
| Modelo 1 | G(i) | 9,934 | 203,379 | 0,9969 |
| Modelo 2 | G(i), H_sun, T2m, WS10m | 9,178 | 141,700 | 0,9978 |

O **Modelo 2** foi melhor nas três métricas. A diferença é pequena porque o PVGIS calcula a potência principalmente a partir da irradiância, que sozinha já explica quase tudo. A temperatura e a altura do Sol acrescentam informação nova, e o vento quase não ajuda (correlação de 0,036 com a potência).

## Como executar

1. Abra `Regressao_Linear_Energia_Solar_PVGIS.ipynb` no [Google Colab](https://colab.research.google.com) ou no Jupyter.
2. Execute todas as células (**Ambiente de execução > Executar tudo**). É necessário ter internet para consultar a API.
3. Se a requisição falhar, troque `v5_3` por `v5_2` na variável `URL`.

Para usar outra cidade, altere `cidade`, `latitude` e `longitude` na célula de requisição.

## Tecnologias

Python, Pandas, NumPy, Matplotlib, scikit-learn e Requests.

## Estrutura do repositório

```
├── Regressao_Linear_Energia_Solar_PVGIS.ipynb
└── README.md
```

## Fonte dos dados

[PVGIS - European Commission JRC](https://re.jrc.ec.europa.eu/pvg_tools/en/)
