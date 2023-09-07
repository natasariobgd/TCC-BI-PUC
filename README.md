## 
## Previsão de Séries Temporais Aplicada a Vendas no Varejo: Um Estudo de Caso

#### - Aluna: [Natasa Marinkovic](https://github.com/natasariobgd).

#### - Orientadora: [Manoela Kohler](https://github.com/manoelakohler).

-----------------------------------------------

#### Trabalho apresentado ao curso [BI-MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

----------------------------------------------------------------------

### Resumo

A análise e a previsão de séries temporais, cada vez mais, têm desempenhado um papel importante no processo de definição de estratégias empresariais.
Este trabalho se propõe a explorar a aplicação das técnicas de previsão de séries temporais em um cenário específico: setor de varejo.
No contexto do varejo, entender e tentar antecipar a procura de clientes abre a possibilidade para melhor otimização de estoques e alocação de recursos tanto financeiros quanto humanos.

Nosso objetivo é aplicar alguns modelos de previsão de séries temporais e verificar os resultados que cada um vai atingir analisando um conjunto de dados históricos de vendas de uma loja de brinquedos ao longo de um período de cinco anos.
   
Inicialmente foi feita uma análise exploratório de dados que mostra a estrutura, as estatísticas principais e possíveis sazonalidade e tendências.

Modelos aplicados foram:

1.	modelo de Rede Neural,
2.	modelo Random Forest.

A análise de resultados concentrou-se nas seguintes métricas de avaliação:

1. Mean Absolute Error (MAE),
2. Mean Squared Error (MSE),
3. Mean Absolute Percentage Error (MAPE), e
4. Coefficient of Determination (R² Score).

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

