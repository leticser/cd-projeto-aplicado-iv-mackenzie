# 🎙️ Roteiro de Fala para o Vídeo de Apresentação (5 a 10 Minutos)

> **Instruções de Gravação:**
> * **Duração Alvo:** Entre 6 e 8 minutos (dentro do intervalo exigido de 5 a 10 min).
> * **Formato:** Gravação de tela com os slides compartilhados e sua câmera visível no canto superior/inferior.
> * **Tom de Voz:** Seguro, técnico, fluido e objetivo.
> * **Dica:** Pratique uma vez lendo o roteiro em voz alta antes de gravar a versão final!

---

### **[00:00 - 00:45] Slide 1: Abertura e Identificação**
> *"Olá a todos, professores, avaliadores e colegas. Meu nome é Letícia, aluna do curso de Tecnologia em Ciência de Dados da Universidade Presbiteriana Mackenzie. Hoje apresento o trabalho final da disciplina de Projeto Aplicado IV, referente ao segundo semestre de 2026.*  
> *O tema do nosso projeto é o desenvolvimento de um **Sistema Preditivo de Demanda de Energia Elétrica para Cidades Sustentáveis**, utilizando técnicas avançadas de Aprendizado de Máquina e Séries Temporais aplicadas ao Subsistema Sudeste e Centro-Oeste do Brasil."*

---

### **[00:45 - 01:45] Slides 2 e 3: Contexto, Motivação e Vinculação ao ODS 11**
> *"Para contextualizar o problema: a energia elétrica é a infraestrutura invisível que sustenta todas as atividades das nossas metrópoles — desde o transporte metroferroviário e o saneamento até hospitais, indústrias e iluminação pública. No Brasil, o subsistema Sudeste/Centro-Oeste é o mais crítico do país, sendo responsável por mais de 55% de toda a carga de energia consumida nacionalmente, abrangendo cidades como São Paulo, Rio de Janeiro e Belo Horizonte.*  
> *Com o avanço da eletrificação e das metas de descarbonização, prever com precisão quanta energia a cidade vai consumir no dia seguinte é essencial. Se superestimamos a demanda, recursos e combustíveis fósseis caros são acionados desnecessariamente. Se subestimamos, corremos risco de sobrecargas e apagões na rede urbana.*  
> *Por isso, este trabalho tem cariz extensionista e vincula-se diretamente ao **ODS 11 da ONU — Cidades e Comunidades Sustentáveis**, em especial às metas 11.6 e 11.b, fornecendo um produto analítico para apoiar secretarias municipais de infraestrutura, concessionárias e gestores de Smart Grids no planejamento eficiente e limpo das nossas cidades."*

---

### **[01:45 - 02:45] Slide 4: Origem dos Dados Primários (ONS)**
> *"Em estrito cumprimento às diretrizes acadêmicas do Mackenzie de evitar bases pré-processadas secundárias, trabalhamos exclusivamente com **dados primários e oficiais do Operador Nacional do Sistema Elétrico (ONS)**, consumidos diretamente do portal de dados abertos na AWS S3.*  
> *Coletamos uma série temporal diária ininterrupta de **01 de janeiro de 2021 até 06 de novembro de 2026**, totalizando **2.136 registros diários** da carga de energia medida em Megawatts médios. É uma base com 100% de integridade, sem registros nulos, cobrindo todo o ciclo pós-pandemia e refletindo o comportamento real recente das redes metropolitanas."*

---

### **[02:45 - 04:00] Slides 5 e 6: Análise Exploratória, Sazonalidade e Engenharia de Atributos**
> *"Ao realizarmos a análise exploratória e a decomposição da série temporal, dois comportamentos ficaram muito evidentes:*  
> *Primeiro, uma **sazonalidade semanal extremamente forte**. Como podemos ver no gráfico de boxplot, há uma queda expressiva de cerca de 25% a 30% na demanda aos sábados e domingos em comparação aos dias úteis, refletindo o fechamento de indústrias e escritórios nas grandes cidades.*  
> *Segundo, ciclos térmicos anuais, com picos de consumo nos meses de verão, causados pelo uso massivo de climatização e ar-condicionado nas capitais.*  
> *Com base nisso, estruturamos uma engenharia de atributos rica, criando defasagens temporais — os famosos lags de 1, 2, 7, 14 e 28 dias —, estatísticas de médias móveis e desvios padrão em janelas deslizantes de 7, 14 e 30 dias para capturar a inércia recente do consumo, além de variáveis de calendário que indicam o dia da semana e finais de semana."*

---

### **[04:00 - 05:15] Slide 7: Metodologia e Modelagem Comparativa**
> *"Para a modelagem, adotamos uma **divisão temporal estrita**, sem vazamento de dados futuros: utilizamos o período de 2021 a 2025 para treinamento dos modelos, totalizando 1.796 dias, e reservamos o ano de 2026 completo — com 310 dias — exclusivamente para teste fora da amostra.*  
> *Comparamos cinco abordagens:*  
> *1. Um **Baseline Naive Sazonal**, que assume que o consumo de hoje será igual ao do mesmo dia da semana anterior (Lag 7);*  
> *2. O modelo estatístico clássico de **Suavização Exponencial de Holt-Winters**, com componentes aditivos de tendência e sazonalidade semanal;*  
> *3. Três modelos modernos de Aprendizado de Máquina baseados em árvores: **Random Forest**, **XGBoost** e **LightGBM**."*

---

### **[05:15 - 06:45] Slides 8 e 9: Resultados e Comparativo de Desempenho**
> *"Os resultados empíricos demonstraram a clara superioridade dos modelos de Machine Learning:*  
> *O modelo campeão foi o **Random Forest Regressor**, que atingiu um **MAPE de apenas 2,20%**, com um Coeficiente de Determinação **R² de 0,8722** e erro médio absoluto de **970 Megawatts médios**. Em uma rede que opera na faixa de 40 a 55 mil MWmed, errar apenas 2,2% na projeção diária representa um nível de precisão de excelência para a indústria de energia.*  
> *O **XGBoost** e o **LightGBM** vieram logo em seguida, com MAPEs respectivos de **2,24%** e **2,32%**.*  
> *Por outro lado, os modelos estatísticos puros demonstraram suas limitações: o Baseline Naive obteve MAPE de 5,15% e R² de 0,43, e o Holt-Winters atingiu MAPE de 5,64% e R² de 0,36.*  
> *Como podemos ver no gráfico de comparação entre a linha preta (a carga real de 2026) e as linhas verde e vermelha dos nossos modelos de Machine Learning, os algoritmos conseguem antecipar tanto as quedas de fim de semana quanto as oscilações de dias úteis com impressionante aderência."*

---

### **[06:45 - 07:45] Slide 10: Desafios e Próximos Passos**
> *"Dentre os principais desafios do projeto, destacamos o isolamento da forte sazonalidade semanal sem induzir overfitting nos dias atípicos e feriados.*  
> *Como oportunidades de aprimoramento futuro para este produto analítico, identificamos:*  
> *1. A incorporação de covariáveis meteorológicas exógenas, como temperatura máxima e umidade do ar medidas pelas estações do INMET nas capitais;*  
> *2. A extensão do modelo para previsões na escala horária (curva de carga em 24 pontos por dia);*  
> *3. O encapsulamento do modelo preditivo em uma API REST com dashboard interativo em tempo real para os operadores do sistema."*

---

### **[07:45 - 08:30] Slides 11 e 12: Conclusão e Encerramento**
> *"Em conclusão, o projeto atendeu integralmente a todos os requisitos propostos para o Projeto Aplicado IV:*  
> *Partimos de um problema real com dados primários governamentais, aplicamos métodos de Ciência de Dados e Aprendizado de Máquina de ponta, superamos com folga os métodos de referência tradicionais e entregamos uma solução com alto valor para a sociedade e para o ODS 11 de Cidades e Comunidades Sustentáveis.*  
> *Todo o código, dados e documentação estão públicos e 100% reproduzíveis no nosso repositório no GitHub e no Jupyter Notebook configurado para execução com um clique no Google Colab.*  
> *Agradeço imensamente aos professores e orientadores pela jornada ao longo deste curso. Muito obrigada!"*
