# 📑 Estrutura e Especificação dos Slides de Apresentação (5 a 10 min)

Este guia define a composição visual e o conteúdo de cada slide para a apresentação do **Projeto Aplicado IV (Ciência de Dados EaD - Mackenzie)**.

---

### **Slide 1: Capa do Projeto**
* **Título:** Sistema Preditivo de Demanda de Energia Elétrica para Cidades Sustentáveis (ODS 11)
* **Subtítulo:** Previsão de Séries Temporais com Aprendizado de Máquina no Subsistema Sudeste/Centro-Oeste
* **Disciplina:** Projeto Aplicado IV – Ciência de Dados EaD – 2026/02
* **Universidade:** Universidade Presbiteriana Mackenzie
* **Autora:** Letícia [Sobrenome]
* **Elementos Visuais:** Logo do Mackenzie e ícone do ODS 11 (Cidades e Comunidades Sustentáveis).

---

### **Slide 2: Contexto e Motivação**
* **Contexto:**
  * O Subsistema Sudeste/Centro-Oeste (SE/CO) concentra mais de **55% da carga elétrica nacional** e as maiores metrópoles do país (São Paulo, Rio de Janeiro e Belo Horizonte).
  * A energia é a espinha dorsal de serviços públicos urbanos: transporte metroferroviário, hospitais, saneamento e iluminação pública.
* **Problema:**
  * A transição energética e a eletrificação urbana aumentam a complexidade da demanda.
  * Flutuações térmicas e padrões semanais causam picos que, se mal previstos, geram sobrecarga na rede ou acionamento desnecessário de usinas térmicas caras e poluentes.
* **Elemento Visual:** Mapa dos 4 subsistemas elétricos do Brasil com destaque no Sudeste/Centro-Oeste.

---

### **Slide 3: Alinhamento aos Objetivos de Desenvolvimento Sustentável (ODS 11)**
* **ODS 11: Cidades e Comunidades Sustentáveis**
  * **Meta 11.6:** Redução do impacto ambiental adverso per capita nas cidades. Previsões acuradas reduzem a queima de combustíveis fósseis (gás/óleo) e priorizam o despacho de fontes limpas (hídrica, solar e eólica).
  * **Meta 11.b:** Resiliência de infraestruturas urbanas frente a eventos climáticos e estresse de rede.
* **Cliente / Beneficiário da Solução:**
  * Secretaria de Infraestrutura e Meio Ambiente / Concessionárias e Gestores de *Smart Grids* urbanas.
* **Elemento Visual:** Card oficial do ODS 11 da ONU e resumo dos benefícios sustentáveis.

---

### **Slide 4: Fonte de Dados Primários (ONS)**
* **Origem dos Dados:**
  * Dados abertos primários e oficiais do **Operador Nacional do Sistema Elétrico (ONS)** (`dados.ons.org.br` / AWS S3 Open Data).
* **Características do Dataset:**
  * Série temporal diária de **01/01/2021 a 06/11/2026** (**2.136 observações diárias** contínuas).
  * Variável-alvo: `val_cargaenergiamwmed` (Carga diária em Megawatts médios).
  * Qualidade: 100% íntegra, sem descontinuidades ou dados faltantes.
* **Elemento Visual:** Print da interface de dados abertos do ONS e tabela com primeiras linhas dos dados.

---

### **Slide 5: Análise Exploratória e Decomposição Sazonal**
* **Principais Padrões Identificados:**
  1. **Sazonalidade Semanal Acentuada:** Queda média de **25% a 30%** no consumo aos sábados e domingos em comparação aos dias úteis (efeito do fechamento de comércios e indústrias metropolitanas).
  2. **Tendência Anual e Ciclos Térmicos:** Picos históricos nos meses mais quentes (janeiro a março) decorrentes do uso intensivo de climatização nas cidades.
* **Decomposição Aditiva:** Separação precisa em componente de Tendência, Sazonalidade (período $m=7$ dias) e Resíduo Estocástico.
* **Elemento Visual:** Gráficos `eda_serie_historica.png` e `eda_sazonalidade_semanal.png`.

---

### **Slide 6: Pipeline da Solução e Engenharia de Atributos**
* **Arquitetura Metodológica:**
  * Ingestão $\to$ Engenharia de Atributos $\to$ Divisão Cronológica $\to$ Treinamento $\to$ Avaliação de Erro.
* **Feature Engineering:**
  * **Defasagens (*Lags*):** 1, 2, 7 (mesmo dia da semana anterior), 14 e 28 dias.
  * **Janelas Deslizantes (*Rolling Windows*):** Médias e desvios padrão móveis de 7, 14 e 30 dias (capturam inércia recente).
  * **Atributos de Calendário:** Dia da semana, mês, dia do ano e indicador binário de final de semana (`is_weekend`).
* **Elemento Visual:** Diagrama de blocos do pipeline de dados.

---

### **Slide 7: Metodologia e Modelos Comparados**
* **Divisão Cronológica Estrita (Sem Data Leakage):**
  * **Treinamento:** 2021 a 2025 ($N = 1.796$ dias).
  * **Teste (Fora da Amostra):** 01/01/2026 a 06/11/2026 ($N = 310$ dias).
* **Famílias de Modelos Implementadas:**
  1. *Baseline Naive Sazonal:* Previsão baseada no valor observado 7 dias antes.
  2. *Holt-Winters Exponential Smoothing:* Modelo estatístico com tendência e sazonalidade aditivas ($m=7$).
  3. *Random Forest Regressor:* Comitê de árvores com amostragem *bagging*.
  4. *XGBoost Regressor:* Gradient boosting otimizado com regularização.
  5. *LightGBM Regressor:* Gradient boosting veloz com particionamento *leaf-wise*.
* **Elemento Visual:** Tabela de configuração de hiperparâmetros dos modelos.

---

### **Slide 8: Resultados do Benchmark Preditivo**
* **Tabela Comparativa de Métricas (Teste 2026):**

| Modelo | MAE (MWmed) ↓ | RMSE (MWmed) ↓ | MAPE (%) ↓ | $R^2$ Score ↑ |
| :--- | :---: | :---: | :---: | :---: |
| **Random Forest Regressor** | **970,31** | **1.361,48** | **2,20%** | **0,8722** |
| **XGBoost Regressor** | 989,16 | 1.394,22 | 2,24% | 0,8660 |
| **LightGBM Regressor** | 1.026,82 | 1.436,04 | 2,32% | 0,8579 |
| **Baseline Naive (Lag-7)** | 2.277,30 | 2.857,52 | 5,15% | 0,4372 |
| **Holt-Winters** | 2.515,51 | 3.034,02 | 5,64% | 0,3656 |

* **Elemento Visual:** Gráfico de barras `resultados_comparativo_metricas.png`.

---

### **Slide 9: Desempenho Visual (Previsão vs. Carga Real)**
* **Aderência Preditiva ao Longo de 2026:**
  * O modelo *Random Forest* acompanha com extrema precisão os picos de dias úteis e as quedas de finais de semana.
  * O erro percentual de **2,20%** é considerado de excelência na literatura do setor elétrico para horizontes de planejamento operacional.
* **Elemento Visual:** Gráfico `resultados_previsao_vs_real.png`.

---

### **Slide 10: Desafios Encontrados e Potenciais Melhorias**
* **Principais Desafios:**
  * Tratamento da sazonalidade semanal combinada com oscilações climáticas intra-anuais sem causar sobreajuste.
  * Garantir a reprodutibilidade 100% executável dos dados diretamente da nuvem AWS S3 do ONS.
* **Melhorias Futuras para o Produto Analítico:**
  1. Incorporação de covariáveis exógenas de temperatura e umidade relativa do ar (dados do INMET).
  2. Expansão para a curva de carga horária (24 pontos/dia) para apoiar o gerenciamento de ponta em cidades inteligentes.
  3. Disponibilização de API REST (FastAPI) e dashboard em tempo real (Streamlit).

---

### **Slide 11: Conclusão e Entrega de Valor**
* **Síntese:**
  * O projeto entregou um produto analítico completo, reproduzível e fundamentado em dados oficiais primários.
  * Demonstrou-se que algoritmos de Machine Learning superam em mais de 57% a acurácia dos modelos estatísticos clássicos para demanda de energia.
* **Impacto no ODS 11:**
  * Fornece às cidades uma ferramenta quantitativa para otimizar o uso de energia, evitar apagões e acelerar a sustentabilidade urbana.
* **Elemento Visual:** QR Code para o repositório GitHub e link do notebook no Google Colab.

---

### **Slide 12: Referências Bibliográficas (ABNT)**
* Lista com os principais autores: Box & Jenkins (1976), Hyndman & Athanasopoulos (2021), Breiman (2001), Chen & Guestrin (2016), ONS (2026) e Nações Unidas (2015).
