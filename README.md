 
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

1.	Rede Neural,

2.	Random Forest.


A análise de resultados concentrou-se nas seguintes métricas de avaliação:


1. Mean Absolute Error (MAE) - O MAE é uma métrica que calcula a média das diferenças absolutas entre os valores reais e os valores previstos.

2. Mean Squared Error (MSE) - O MSE é uma métrica que calcula a média dos quadrados das diferenças entre os valores reais e os valores previstos. 

3. Mean Absolute Percentage Error (MAPE) - O MAPE é uma métrica que calcula a média das porcentagens das diferenças absolutas entre os valores reais e os valores previstos em relação aos valores reais. É uma métrica útil para avaliar a precisão das previsões em termos de porcentagem de erro em relação aos valores reais.

4. Coefficient of Determination (R² Score) - O R² Score é uma métrica que varia de 0 a 1 e quantifica a proporção da variância do valor real que é explicada pela variável do valor previsto em um modelo de regressão. Quanto mais próximo de 1, melhor o modelo se ajusta aos dados, indicando que ele explica uma maior parte da variabilidade nos dados reais..

Rede Neural

Na construção de modelo foi utiliza a biblioteca Keras para construir e treinar redes neurais com o auxílio da biblioteca TensorFlow. Além disso, a biblioteca MinMaxScaler do scikit-learn aplicou a normalização de dados que ajudou a evitar problemas de convergência e facilitou o treinamento com diferentes faixas de valores. O processo de construção de modelo incluiu a separação dos dados em conjuntos de treinamento e teste (usando 80% dos dados para treinamento), seguido pela normaliação dos dados de vendas para o intervalo [-1, 1] com o objetivo de padronizá-los. O resultado final foi criação de  conjuntos de dados de treinamento e teste normalizados e prontos para serem usados em modelo de rede neural.

            ---- Vamos separar os dados em treinamento e teste
            train_size = int(len(df) * 0.8)
            train_data = df[:train_size]
            test_data = df[train_size:]

            ---- Escalonar os dados para o intervalo [-1, 1]
            scaler = MinMaxScaler()
            train_scaled = scaler.fit_transform(train_data['Sales'].values.reshape(-1, 1))
            test_scaled = scaler.transform(test_data['Sales'].values.reshape(-1, 1))

Em seguida, foram definidos os parâmetros da rede neural:

1. Window_size - tamanho de sequiência de dados originais,   
2. Modelo de rede - modelo pode conter uma ou mais camadas o que define a complexidade de modelo,
3. Epochs - número de vezes que um algoritmo de aprendizado percorre o conjunto completo de dados de treinamento durante o processo de treinamento de um modelo de machine learning,
4. Batch_size - representa a quantidade de dados usados em cada iteração do algoritmo,
5. Otimize='adam' - O 'adam' é um otimizador que ajusta automaticamente a taxa de aprendizado durante o treinamento para acelerar a convergência.


            ---- Vamos preparar os dados para treinamento
            window_size = 100
            X_train = []
            y_train = []
            for i in range(window_size, len(train_scaled)):
                X_train.append(train_scaled[i-window_size:i, 0])
                y_train.append(train_scaled[i, 0])
            X_train, y_train = np.array(X_train), np.array(y_train)


            ---- Vamos construir o modelo de rede neural simples
            model = tf.keras.models.Sequential([
                tf.keras.layers.LSTM(64, activation='relu', input_shape=(X_train.shape[1], 1)),
                tf.keras.layers.Dense(32, activation='relu'),
                tf.keras.layers.Dense(1)


             ---- Vamos compilar o modelo
             model.compile(optimizer='adam', loss='mean_squared_error')
             
             
             ---- Vamos treinar o modelo sem callbacks
             history = model.fit(X_train, y_train, epochs=500, batch_size=14, validation_split=0.1)


Random Forest

O modelo Random Forest é um algoritmo de aprendizado de máquina baseado em combinações de várias árvores de decisão para melhorar a precisão e reduzir o overfitting. Esse método utiliza amostragem aleatória para criar subconjuntos de dados de treinamento tornando cada árvore diferente. A combinação de árvores ajuda a reduzir o overfitting, tornando o modelo mais generalizável.

Para esse modelo foi utilizada a biblioteca scikit-learn para dividir os dados em conjuntos de treinamento e teste, onde o parâmetro test_size determina a proporção dos dados reservados para teste (10% neste caso). Após a separação de dados foi criado o modelo de regressão baseado em Random Forest. Este modelo é configurado com 9 árvores de decisão e um número mínimo de dados (folhas) igual a 10. O parâmetro random_state é fixado em 0 para garantir reprodutibilidade. Posteriormente, o modelo foi treinado usando os dados de treinamento (x_train e y_train) e o resultado foi um modelo de regressão Random Forest pronto para fazer previsões com base nos dados de teste.


               ---- O conjunto de teste será definido pelo parâmetro test_size que costuma variar de acordo com a natureza de dados.
               from sklearn.model_selection import train_test_split
               x_train, x_test, y_train, y_test = train_test_split(X,
                                                                   y,
                                                                   test_size=0.1)                                                                              

                ---- O modelo criado terá 9 árvores de decisão e o número mínimo de dados (folhas) de 10.
                
                from sklearn.ensemble import RandomForestRegressor
                regressor = RandomForestRegressor(n_estimators=9, min_samples_leaf = 10, random_state=0)
                regressor.fit(x_train, y_train);



### 3. Resultados

Os resultados revelam que o modelo Random Forest teve uma capacidade boa de se ajustar aos dados e que conseguiu capturar aproximadamente 80% da variabilidade dos dados.
É importante ressaltar que apenas após a inclusão do parâmetro "min_samples_leaf" foi possível eliminar overfitting do modelo.

Seguem os parâmetros usados no treinamento desse modelo que geraram melhores resultados:
1. window_size - 21,
2. n_estimators - 9 (indica o número de árvores usadas no treino e treinadas separadamente),
3. min_samples_leaf - 10 (indica o número mínimo de amostras em último nó da árvore).

Os resultados obtidos com parâmetros usados são os seguintes:

                  ---- Resultados de Treino
                  RMSE:  1412.730234027476
                  MSE:  1995806.714135327
                  MAPE:  21.337064011513917 %
                  R2:  0.7970351728972613

![image](https://github.com/natasariobgd/TCC-BI-PUC/assets/97364314/2a91fafb-2f28-4bfc-964a-e12ee22dfdf2)



                  
                  ---- Resultados de Teste
                  RMSE:  1314.4153478503997
                  MSE:  1727687.7066646875
                  MAPE:  21.68674673658258 %
                  R2:  0.7788494585142555

![image](https://github.com/natasariobgd/TCC-BI-PUC/assets/97364314/cae367f7-b741-4e72-a7f9-b19cc35c8367)


### 4. Conclusões

Diferente do modelo Random Forest, o modelo baseado em Rede Neural mostrou-se complexo para o problema e sem capacidade significativa de adaptação.
Foi usada a biblioteca Keras e aplicado o modelo composto de uma primeira camada da rede LSTM (Long Short-Term Memory) com 64 unidades (neurônios), a segunda camada totalmente conectada (dense) com 32 unidades e uma camada se saída.

Exp: model = tf.keras.models.Sequential([ tf.keras.layers.LSTM(64, activation='relu', input_shape=(X_train.shape[1], 1)), tf.keras.layers.Dense(32, activation='relu'), tf.keras.layers.Dense(1) ])

Este estudo contribuiu para uma compreensão melhor como funcionam os modelos de regressão e como a escolha dos hiper parâmetros desempenhou um papel importante na modelagem.

----------------------------------

Matrícula: 212.100.499

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação Business Intelligence Master

