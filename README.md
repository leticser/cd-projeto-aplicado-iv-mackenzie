# ⚡ Sistema Preditivo de Demanda de Energia Elétrica para Cidades Sustentáveis (ODS 11)

> **Universidade Presbiteriana Mackenzie**  
> **Curso de Tecnologia em Ciência de Dados EaD**  
> **Disciplina:** PROJETO APLICADO IV (2026/02)  
> **Autora:** Letícia [Sobrenome]  
> **Tema:** Previsão de Séries Temporais de Demanda Energética no Subsistema Sudeste/Centro-Oeste (ONS)

---

## 📌 1. Visão Geral do Projeto

Este projeto desenvolve um **produto analítico preditivo de séries temporais** para estimar a carga diária de energia elétrica no Subsistema Sudeste/Centro-Oeste (SE/CO), polo que concentra mais de 55% da demanda nacional e as maiores regiões metropolitanas do país (São Paulo, Rio de Janeiro e Belo Horizonte).

O projeto atende com rigor aos critérios do **Projeto Aplicado IV do Mackenzie**:
1. **Problema Relevante e Aplicado:** Estabilidade e planejamento da matriz energética de metrópoles (*Smart Grids*).
2. **Dados Próprios e Primários:** Ingestão de **2.136 registros diários** diretamente da infraestrutura aberta oficial do **Operador Nacional do Sistema Elétrico (ONS)** (`dados.ons.org.br`), cobrindo de **01/01/2021 a 06/11/2026**.
3. **Modelos Estatísticos e de Aprendizado de Máquina:** Comparação empírica entre modelos clássicos (*Baseline Naive Sazonal* e *Holt-Winters*) e algoritmos supervisionados baseados em árvores (*Random Forest*, *XGBoost* e *LightGBM*).
4. **Caráter Extensionista e Vínculo ao ODS 11:** Alinhamento direto com o **Objetivo de Desenvolvimento Sustentável 11 (Cidades e Comunidades Sustentáveis)** da Agenda 2030 da ONU.
5. **Experimento 100% Reprodutível:** Código e dados disponibilizados em Jupyter Notebook totalmente executável.

---

## 🎯 2. Alinhamento com o ODS 11 da ONU

* **Meta 11.6:** Reduzir o impacto ambiental adverso per capita das cidades. A previsão assertiva da curva de carga reduz o acionamento emergencial de usinas termelétricas a combustíveis fósseis (altamente emissoras de $CO_2$ e material particulado), maximizando a utilização das fontes limpas e renováveis (hidrelétrica, eólica e solar fotovoltaica).
* **Meta 11.b:** Aumentar a resiliência das infraestruturas urbanas frente a eventos climáticos extremos. A antecipação da demanda elétrica permite às concessionárias e distribuidoras metropolitanas prevenir gargalos de transmissão e falhas de fornecimento (*apagões*).

**Cliente Fictício / Beneficiário:** Secretaria de Infraestrutura e Meio Ambiente do Estado de São Paulo / Operadores de Redes Elétricas Inteligentes (*Smart Grids*) em Regiões Metropolitanas.

---

## 🔬 3. Pipeline da Solução

```
+-----------------------------------------------------------------------------------+
|                        1. INGESTÃO DE DADOS PRIMÁRIOS                             |
| Operador Nacional do Sistema Elétrico (ONS) - Dados Abertos AWS S3 (2021 a 2026)  |
| Subsistema Sudeste/Centro-Oeste (SE) - 2.136 Observações Diárias em MWmed         |
+-----------------------------------------------------------------------------------+
                                         │
                                         ▼
+-----------------------------------------------------------------------------------+
|                 2. PRÉ-PROCESSAMENTO & ENGENHARIA DE ATRIBUTOS                    |
| • Verificação de Integridade e Ausência de Nulos                                  |
| • Defasagens Temporais: Lags 1, 2, 7 (Sazonalidade Semanal), 14 e 28              |
| • Estatísticas Móveis: Médias e Desvios Padrão Móveis em Janelas de 7, 14 e 30 d  |
| • Variáveis de Calendário: Dia da Semana, Mês, Dia do Ano, Indicador Fim de Semana|
+-----------------------------------------------------------------------------------+
                                         │
                                         ▼
+-----------------------------------------------------------------------------------+
|                    3. MODELAGEM & VALIDAÇÃO TEMPORAL ESTRITA                      |
| Divisão Cronológica: Treino (2021 a 2025, N=1.796) | Teste (2026, N=310)         |
|                                                                                   |
|  [Baseline Estatístico]        [Séries Temporais]           [Machine Learning]    |
|   • Naive Sazonal (Lag 7)       • Holt-Winters Additive       • Random Forest     |
|                                                               • XGBoost Regressor |
|                                                               • LightGBM Regressor|
+-----------------------------------------------------------------------------------+
                                         │
                                         ▼
+-----------------------------------------------------------------------------------+
|                  4. AVALIAÇÃO COMPARATIVA & PRODUTO ANALÍTICO                     |
| • Métricas: MAE, RMSE, MAPE (%) e R² Score                                        |
| • Visualizações: Análise de Resíduos, Curva Previsão vs. Real 2026                |
| • Subsídio para Tomada de Decisão em Cidades Sustentáveis (Smart Grids / ODS 11)   |
+-----------------------------------------------------------------------------------+
```

---

## 📊 4. Resultados do Benchmark Experimental (Teste Fora da Amostra - 2026)

A validação foi conduzida com corte cronológico estrito (dados de treino: 2021 a 2025; dados de teste: 01/01/2026 a 06/11/2026):

| Modelo | MAE (MWmed) ↓ | RMSE (MWmed) ↓ | MAPE (%) ↓ | $R^2$ Score ↑ |
| :--- | :---: | :---: | :---: | :---: |
| **Random Forest Regressor** | **970,31** | **1.361,48** | **2,20%** | **0,8722** |
| **XGBoost Regressor** | 989,16 | 1.394,22 | 2,24% | 0,8660 |
| **LightGBM Regressor** | 1.026,82 | 1.436,04 | 2,32% | 0,8579 |
| **Baseline Naive (Sazonal Lag-7)** | 2.277,30 | 2.857,52 | 5,15% | 0,4372 |
| **Holt-Winters ExpSmoothing** | 2.515,51 | 3.034,02 | 5,64% | 0,3656 |

### 💡 Destaques:
* O modelo **Random Forest Regressor** alcançou a liderança com **MAPE de 2,20%** e $R^2$ de **0,8722**, reduzindo o erro médio absoluto em mais de **57%** em relação ao baseline ingênuo.
* O algoritmo aprendeu com precisão a queda típica de 25% a 30% na carga aos sábados e domingos em comparação aos dias úteis nas metrópoles.

---

## 📁 5. Estrutura do Repositório

```
cd-projeto-aplicado-iv-mackenzie/
│
├── README.md                      # Documentação completa do projeto
├── requirements.txt               # Dependências Python necessárias
│
├── data/
│   ├── ons_energia_sudeste_2021_2026.csv # Base histórica limpa do ONS
│   └── metricas_benchmark.csv    # Tabela consolidada com os resultados
│
├── figuras/                       # Gráficos em alta resolução (300 DPI)
│   ├── eda_serie_historica.png
│   ├── eda_sazonalidade_semanal.png
│   ├── eda_decomposicao_sazonal.png
│   ├── resultados_previsao_vs_real.png
│   └── resultados_comparativo_metricas.png
│
├── notebooks/
│   └── cd_projeto_aplicado_IV_doc.ipynb # Notebook Oficial (Template Mackenzie)
│
└── apresentacao/
    ├── roteiro_video_apresentacao.md # Roteiro de fala para o vídeo de 5-10 min
    └── estrutura_slides.md        # Especificação detalhada dos slides
```

---

## 🚀 6. Como Executar

### Opção A: Execução Local
1. Clone o repositório:
```bash
git clone https://github.com/seu-usuario/cd-projeto-aplicado-iv-mackenzie.git
cd cd-projeto-aplicado-iv-mackenzie
```
2. Instale as dependências:
```bash
pip install -r requirements.txt
```
3. Abra e execute o notebook:
```bash
jupyter notebook notebooks/cd_projeto_aplicado_IV_doc.ipynb
```

### Opção B: Google Colab
Clique no botão abaixo para abrir o notebook diretamente no Google Colab com 1 clique:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

---

## 📹 7. Apresentação do Projeto
* **Vídeo no YouTube (5 a 10 min):** `[Inserir Link do YouTube]`
* **Slides da Apresentação:** `[Inserir Link dos Slides]`

---

## 📚 8. Referências Bibliográficas (Normas ABNT)

BOX, G. E. P.; JENKINS, G. M. **Time series analysis: forecasting and control**. San Francisco: Holden-Day, 1976.

BREIMAN, L. Random forests. **Machine Learning**, v. 45, n. 1, p. 5–32, 2001.

CHEN, T.; GUESTRIN, C. XGBoost: A scalable tree boosting system. In: **ACM SIGKDD International Conference on Knowledge Discovery and Data Mining**, 22., 2016. Proceedings [...]. New York: ACM, 2016. p. 785–794.

HOLT, C. C. Forecasting seasonals and trends by exponentially weighted moving averages. **International Journal of Forecasting**, v. 20, n. 1, p. 5–10, 2004.

HYNDMAN, R. J.; ATHANASOPOULOS, G. **Forecasting: principles and practice**. 3. ed. Melbourne: OTexts, 2021. Disponível em: <https://otexts.com/fpp3/>.

KE, G. et al. LightGBM: A highly efficient gradient boosting decision tree. In: **Advances in Neural Information Processing Systems (NeurIPS)**, 30., 2017. Proceedings [...]. Red Hook: Curran Associates, 2017. p. 3146–3154.

OPERADOR NACIONAL DO SISTEMA ELÉTRICO (ONS). **Dados abertos do ONS: série histórica de carga de energia diária por subsistema**. Brasília: ONS, 2026. Disponível em: <https://dados.ons.org.br/dataset/carga-energia>.

UNITED NATIONS. **Sustainable Development Goals (SDGs): Goal 11 - Make cities and human settlements inclusive, safe, resilient and sustainable**. New York: UN, 2015. Disponível em: <https://sdgs.un.org/goals/goal11>.
