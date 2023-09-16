 
# Previsão de Séries Temporais Aplicada a Vendas no Varejo: Um Estudo de Caso

#### Aluna: [Natasa Marinkovic](https://github.com/natasariobgd).
#### Orientadora: [Manoela Kohler](https://github.com/manoelakohler).

Trabalho apresentado ao curso [BI-MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

### Resumo

A análise e a previsão de séries temporais, cada vez mais, têm desempenhado um papel importante no processo de definição de estratégias empresariais.
Este trabalho se propõe a explorar a aplicação das técnicas de previsão de séries temporais em um cenário específico: setor de varejo.

Nosso objetivo é aplicar alguns modelos de previsão e verificar os resultados que cada um vai atingir utilizando um conjunto de dados históricos de vendas de uma loja de brinquedos ao longo de um período de cinco anos.

A escolha e a construção de modelos são feitos após uma análise exploratória de dados. 
Esse estudo apresenta os modelos de Rede Neural e de Random Forest. Aborda as bibliotecas utilizadas, os parâmetros aplicados nos treinamentos feitos e os resultados dos testes realizados.

A enfase do estudo é o modelo Random Fortest, por ter apresentado melhores resultados.

### Abstract

The analysis and forecasting of time series data have increasingly played an important role in the process of defining business strategies.

This work aims to explore the application of time series forecasting techniques in a specific scenario: the retail sector.

Our objective is to apply some forecasting models and assess the results that each one will achieve using a historical dataset of toy store sales over a five-year period.

The choice and construction of models are made after an exploratory data analysis. This study presents the Neural Network and Random Forest models, discusses the libraries used, the parameters applied in the training, and the results of the tests conducted.

The emphasis of the study is on the Random Forest model, as it has shown better results.


### 1. Introdução

No contexto do varejo, entender e tentar antecipar a procura de clientes abre a possibilidade para melhor otimização de estoques e alocação de recursos tanto financeiros quanto humanos.
Para isso se tornar possível, é importante fazer uma análise exploratória de dados históricos que mostra a estrutura, as estatísticas principais e possíveis sazonalidades e tendências.

A base de dados utilizada nesse estudo de caso contem dados de venda, de estoque e de preços. 
A primeirá dúvida foi se essas três informações tem ou não alguma correlação e utilizando a matriz de correlação foi detectada a ausência de correlação entre os dados definindo que o foco deve ser apenas na série de venda.

Agora, com foco definido, iniciamos uma nova exploração de dados identificando as tendências, sezonalidades e ruidos dos dados de apenas série de venda.

![image](https://github.com/natasariobgd/TCC-BI-PUC/assets/97364314/c57df118-6c3a-4862-b6e0-7d03bda46991)

A identificação de sezonalidades semanais e anuais, de tendencias de crecimento durante o peridodo analisado e existencia de ruidos foi importante no processo de definição de parâmentros de modelos aplicados. 

### 2. Modelagem

Nesse estudo foram aplicados os seguintes modelos:

1.	modelo de Rede Neural,

2.	modelo Random Forest.


A análise de resultados concentrou-se nas seguintes métricas de avaliação:


1. Mean Absolute Error (MAE) - O MAE é uma métrica que calcula a média das diferenças absolutas entre os valores reais e os valores previstos.

2. Mean Squared Error (MSE) - O MSE é uma métrica que calcula a média dos quadrados das diferenças entre os valores reais e os valores previstos. 

3. Mean Absolute Percentage Error (MAPE) - O MAPE é uma métrica que calcula a média das porcentagens das diferenças absolutas entre os valores reais e os valores previstos em relação aos valores reais. É uma métrica útil para avaliar a precisão das previsões em termos de porcentagem de erro em relação aos valores reais.

4. Coefficient of Determination (R² Score) - O R² Score é uma métrica que varia de 0 a 1 e quantifica a proporção da variância do valor real que é explicada pela variável do valor previsto em um modelo de regressão. Quanto mais próximo de 1, melhor o modelo se ajusta aos dados, indicando que ele explica uma maior parte da variabilidade nos dados reais..

Rede Neural

Na construção de modelo foi utiliza a biblioteca Keras para construir e treinar redes neurais, com o auxílio da biblioteca TensorFlow. Além disso, a biblioteca MinMaxScaler do scikit-learn aplicou a normalização de dados que ajuda a evitar problemas de convergência e facilita o treinamento com diferentes faixas de valores. O processo de construção de modelo inclui a separação dos dados em conjuntos de treinamento e teste (usando 80% dos dados para treinamento), seguido pela normaliação dos dados de vendas para o intervalo [-1, 1] com o objetivo de padronizá-los. O resultado final são conjuntos de dados de treinamento e teste normalizados e prontos para serem usados em um modelo de rede neural.

from keras.callbacks import History #biblioteca para construir e treinar redes neurais mostrando o histórico do progresso.
import tensorflow as tf #biblioteca para desenvolvimento de redes neurais
from sklearn.preprocessing import MinMaxScaler #biblioteca para normalização de dados dentro de uma intervalo, geralmente entre 0 e 1.


            ---- Vamos eeparar os dados em treinamento e teste
            train_size = int(len(df) * 0.8)
            train_data = df[:train_size]
            test_data = df[train_size:]

            ---- Escalonar os dados para o intervalo [-1, 1]
            scaler = MinMaxScaler()
            train_scaled = scaler.fit_transform(train_data['Sales'].values.reshape(-1, 1))
            test_scaled = scaler.transform(test_data['Sales'].values.reshape(-1, 1))




### 3. Resultados

Os resultados revelam que o modelo Random Forest teve uma capacidade boa de se ajustar aos dados e que conseguiu capturar aproximadamente 80% da variabilidade dos dados.
É importante ressaltar que apenas após a inclusão do parâmetro "min_samples_leaf" foi possível eliminar overfitting do modelo.

Seguem os parâmetros usados no treinamento desse modelo que geraram melhores resultados:

1. window_size - 21,
2. n_estimators - 9 (indica o número de árvores usadas no treino e treinadas separadamente),
3. min_samples_leaf - 10 (indica o número mínimo de amostras em último nó da árvore).

Diferente do modelo Random Forest, o modelo baseado em Rede Neural mostrou-se complexo para o problema e sem capacidade significativa de adaptação.
Foi usada a biblioteca Keras e aplicado o modelo composto de uma primeira camada da rede LSTM (Long Short-Term Memory) com 64 unidades (neurônios), a segunda camada totalmente conectada (dense) com 32 unidades e uma camada se saída.

Exp: model = tf.keras.models.Sequential([ tf.keras.layers.LSTM(64, activation='relu', input_shape=(X_train.shape[1], 1)), tf.keras.layers.Dense(32, activation='relu'), tf.keras.layers.Dense(1) ])

Este estudo contribuiu para uma compreensão melhor como funcionam os modelos de regressão e como a escolha dos hiper parâmetros desempenhou um papel importante na modelagem.

----------------------------------

Matrícula: 212.100.499

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação Business Intelligence Master

