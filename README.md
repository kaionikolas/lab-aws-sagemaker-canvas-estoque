# 📊 Previsão de Estoque Inteligente na AWS com [SageMaker Canvas](https://aws.amazon.com/pt/sagemaker/canvas/)

Esse é o resultado o desafio de projeto "Previsão de Estoque Inteligente na AWS com SageMaker Canvas. Este Lab DIO teve como objetivo o aprendizado e o uso do SageMaker Canvas para criação de previsões de estoque baseadas em Machine Learning (ML).

## 🚀 Passo a Passo

### 1. Seleção de Dataset

-   Foi escolhido como dataset a ser estudado a fim de gerar previsãoes de estoque o arquivo "dataset-1000-com-preco-variavel-e-renovacao-estoque.csv" disponibilizado na pasta "datasets".
-   Foi realizado o upload do dataset no SageMaker Canvas.

### 2. Construção/Treinamento

-   No SageMaker Canvas, foi importado o dataset selecionado.
-   Realizou-se o tratamento dos dados seguindo a recomendação da plataforma:
    - All missing values in PRECO will be replaced with *MEDIAN*;
    - All missing values in QUANTIDADE_ESTOQUE will be replaced with *ZERO*.
-   Foi realizado o treinamento do modelo selecionando:
    - Target column: QUANTIDADE_ESTOQUE;
    - Model type: Time series forecasting;
    - Item ID column: ID_PRODUTO;
    - Time stamp column: DATA_EVENTO;
    - Number of days to forecast into the future: 9;
    - Holiday schedule: Brazil;
    - Training option: Quick build.

### 3. Analise

-   Após o treinamento, as métricas de performance do modelo obtidas foram:
    1) Avg. wQL -> 0.339;
    2) MAPE -> 1.446;
    3) WAPE -> 0.534;
    4) RMSE -> 34.803;
    5) MASE -> 0.807.
    - A interpretação de cada status é direta e pode ser verificada na documentação [Objective metrics - Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/timeseries-objective-metric.html)
-   Impacto das colunas na predição de estoque:
    - PRECO: 43.08%;
    - Holiday_BR: 1.96%.

### 4. Previsão

-   Foram gerados resultados de previsão singulares mediante cada ID. Cada produto em estoque esteve associado à quatro curvas durante seu período no sistema, sendo elas: Demanada histórica (Dados em registro), P10 (No contexto: Curva de previsão pessimista), P50 (No contexto: Curva de previsão realista) e P90 (No contexto: Curva de previsão otimista).
-   Nota-se em dados que ao decorrer dos nove dias a quantidade em estoque dos produtos, considerando quaisquer das curvas de predição, mora no intervalo entre o Máx-value e o Min-value da demanda histórica, sem que haja falta de produto no estoque.
-   É possivel considerar que a quantidade de produto em estoque é fortemente correlacionada com o preço do produto, e pode estar de alguma forma atrelada aos feriados do país. Como a correlação entre QUANTIDADE_ESTOQUE e Holiday_BR no modelo treinado foi de 1.96%, passa a ser interessante trabalhar o modelo com outros datasets para investigar aspectos de casualidade e/ou correlação.

