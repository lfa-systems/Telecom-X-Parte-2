# 🎯 Missão

Missão do projeto é desenvolver modelos preditivos capazes de prever quais clientes têm maior chance de cancelar seus serviços.
A empresa quer antecipar o problema da evasão, e cabe a você construir um pipeline robusto para essa etapa inicial de modelagem.

# Análise e Predição de Churn em Telecomunicações (Parte 2)

Este projeto tem como objetivo desenvolver modelos preditivos para identificar clientes com alta probabilidade de cancelar seus serviços (churn) em uma empresa de telecomunicações. A missão é construir um pipeline robusto que inclua desde a preparação dos dados até a avaliação e interpretação dos modelos.

## 🗃️ Bibliotecas Utilizadas e suas Funcionalidades

O projeto utiliza as seguintes bibliotecas Python, com foco em manipulação de dados, visualização e Machine Learning.

  * **Pandas**: A "canivete suíço" para dados\! É a biblioteca mais importante para organizar e mexer com dados em formato de tabelas (que chamamos de DataFrames).

      * **Carregando e organizando os dados**: O notebook começa lendo os dados de um arquivo JSON. As informações vêm bagunçadas, com vários dados dentro de outros. O Pandas entra em ação para arrumar tudo.
        ```python
        # Carrega os dados de um arquivo JSON em uma URL
        dados = pd.read_json('https://path/to/data.json')
        # "Achata" as colunas com dicionários aninhados, criando colunas novas e arrumadas
        dados_normalizados = pd.json_normalize(dados.to_dict('records'))
        ```

  * **Numpy**: A base para cálculos com números. Ela trabalha com arrays (matrizes) de forma super rápida e eficiente. Muitas outras bibliotecas, como o Scikit-learn, usam o Numpy por baixo dos panos.

      * **Exemplo popular de uso**: O `numpy` é fundamental para operações matemáticas complexas, como as que acontecem dentro dos algoritmos de Machine Learning.

  * **Matplotlib.pyplot e Seaborn**: Para "desenhar" gráficos e visualizar os dados. O `Seaborn` é um complemento do `Matplotlib` que deixa os gráficos mais bonitos e fáceis de fazer.

      * **Visualizando os resultados**: Uma matriz de confusão é um gráfico importante para entender como o modelo acertou e errou.
        ```python
        # Plota a matriz de confusão
        sns.heatmap(matriz_confusao, annot=True, fmt='d', cmap='Blues')
        plt.show()
        ```

  * **Scikit-learn (sklearn)**: A "caixa de ferramentas" do Machine Learning. Com ela, fazemos tudo: preparamos os dados, treinamos os modelos e vemos o quanto eles acertaram.

      * **Preparando os dados para o modelo**:
          * `train_test_split`: Separa os dados em dois grupos: um para o modelo aprender (treino) e outro para testar se ele aprendeu direito (teste).
          * `StandardScaler`: Ajuda a padronizar os números. Por exemplo, se uma coluna tem valores de 1 a 10 e outra de 1 a 1000, o `StandardScaler` deixa ambas na mesma "escala" para que o modelo não dê mais importância a uma coluna só porque seus números são maiores.
          * `OneHotEncoder`: Transforma textos em números. Por exemplo, a coluna `Gênero` com valores `Feminino` e `Masculino` vira duas novas colunas, `Gênero_Feminino` e `Gênero_Masculino`, com valores `0` ou `1`.
      * **Juntando tudo em um pipeline**:
        ```python
        # Cria um "preparador de dados" que aplica diferentes transformações em diferentes colunas
        pre_processador = ColumnTransformer(
            transformers=[
                ('num', StandardScaler(), colunas_numericas),
                ('cat', OneHotEncoder(), colunas_categoricas)
            ])
        # Cria um "pipeline" que primeiro prepara os dados e depois treina o modelo
        pipeline = Pipeline(steps=[('preprocessor', pre_processador),
                                   ('classifier', RandomForestClassifier())])
        ```
      * **Otimizando o modelo**:
          * `GridSearchCV`: Ajuda a encontrar a melhor "receita" para o modelo, testando várias combinações de configurações para ver qual dá o melhor resultado.
      * **Avaliando o desempenho**:
          * `classification_report` e `confusion_matrix`: Depois que o modelo faz as previsões, esses métodos nos dão um relatório completo sobre o desempenho, mostrando quantos acertos, erros e o desempenho em cada categoria.

  * **Random Forest**: É um dos modelos de Machine Learning mais populares e eficazes para classificação. Pense nele como uma equipe de muitas "mini-decisões" (árvores de decisão). Cada árvore de decisão faz sua própria previsão, e o Random Forest combina as respostas da maioria das árvores para chegar a uma previsão final, mais robusta e precisa do que uma única árvore.

      * **Treinando o modelo Random Forest**:
        ```python
        # Define o modelo Random Forest
        rf_model = RandomForestClassifier(random_state=42)
        # Cria e treina o pipeline com o modelo
        pipeline = Pipeline(steps=[('preprocessor', pre_processador),
                                   ('classifier', rf_model)])
        pipeline.fit(X_train, y_train)
        ```
      * **Por que o Random Forest é bom?**: Ele lida bem com dados complexos, é menos propenso a "overfitting" (quando o modelo decora os dados de treino e não consegue prever novos dados) e a sua estrutura de "floresta" o torna uma ferramenta poderosa para prever o churn.

  * **XGBoost (xgboost)**: Um tipo de modelo de Machine Learning super poderoso e rápido, conhecido por ganhar muitas competições de ciência de dados. É uma ótima alternativa ao Random Forest.

      * **Treinando o modelo XGBoost**:
        ```python
        # Cria e treina um modelo XGBoost dentro do pipeline
        pipeline_xgb = Pipeline(steps=[('preprocessor', pre_processador),
                                       ('classifier', XGBClassifier())])
        pipeline_xgb.fit(X_train, y_train)
        ```

  * **Joblib**: Para salvar o modelo treinado. Pense nisso como "congelar" o modelo para poder usá-lo depois, sem precisar treiná-lo novamente.

      * **Salvando e carregando o modelo**:
        ```python
        # Salva o pipeline (o "preparador" + o modelo) em um arquivo
        joblib.dump(pipeline, 'modelo_churn.pkl')
        # Carrega o modelo salvo para usar mais tarde
        modelo_carregado = joblib.load('modelo_churn.pkl')
        ```

## 📈 Tratamento e Treinamento dos Dados

O processo de tratamento e treinamento dos dados é estruturado em um fluxo de trabalho claro, garantindo a qualidade e a reprodutibilidade dos resultados:

1.  **Carregamento e Preparação dos Dados**:

      * Os dados são carregados de um arquivo JSON.
      * As colunas que contêm dicionários aninhados são "aplanadas" com `pd.json_normalize()` para que cada informação vire uma coluna separada e fácil de usar.
      * A coluna `Churn` é a nossa "resposta" (o que queremos prever). Ela é convertida de "Yes"/"No" para `1`/`0`, que são valores que o modelo consegue entender.

2.  **Pré-processamento**:

      * Os dados são divididos em `features` (as informações dos clientes) e a variável alvo (`Churn`).
      * O `train_test_split` separa os dados para treino e teste.
      * O `ColumnTransformer` e o `Pipeline` entram em cena para garantir que os dados sejam preparados (normalizados e codificados) de forma consistente antes de serem enviados para o modelo.

3.  **Treinamento e Avaliação dos Modelos**:

      * São treinados dois modelos: Random Forest e XGBoost.
      * O `GridSearchCV` é usado para encontrar os melhores "ajustes" para o Random Forest, garantindo que ele tenha a melhor performance possível.
      * Por fim, o desempenho é avaliado com o `classification_report` e a `confusion_matrix`, para sabermos quão bem o modelo está prevendo o churn.

### **Por que usar Random Forest e XGBoost em conjunto?**

No mundo do Machine Learning, é comum não se contentar com apenas um modelo. A melhor prática é treinar e comparar diferentes modelos para ver qual deles se adapta melhor aos seus dados e ao seu problema. É por isso que o projeto utiliza tanto o **Random Forest** quanto o **XGBoost**.

Ambos são modelos de "ensemble", ou seja, eles combinam a força de várias árvores de decisão para criar uma previsão final mais precisa. No entanto, eles fazem isso de maneiras diferentes:

  * **Random Forest**: É um método de "bagging" (do inglês, *bootstrap aggregating*). Ele treina várias árvores de forma independente em diferentes subconjuntos de dados. A previsão final é a média ou a votação da maioria das árvores. É um modelo robusto, fácil de paralelizar (o que o torna rápido em máquinas com vários núcleos) e menos propenso a overfitting.

  * **XGBoost**: É um método de "boosting" (do inglês, *gradiente boosting*). Ele treina as árvores de forma sequencial, onde cada nova árvore é criada para corrigir os erros cometidos pelas árvores anteriores. Isso permite que o modelo aprenda com seus próprios erros e se torne progressivamente mais preciso. É conhecido por sua alta performance e por ser um dos modelos mais utilizados em competições de ciência de dados.

Ao usar os dois modelos no mesmo projeto, o desenvolvedor pode **comparar seus desempenhos**, analisando métricas como precisão, recall e F1-score no conjunto de teste. O objetivo é escolher o modelo que oferece o melhor equilíbrio entre precisão e interpretabilidade para a tarefa de prever o churn.

### **Entendendo as Métricas de Avaliação: Precisão, Recall e F1-score**

Depois que o modelo faz as previsões, é crucial entender se ele está fazendo um bom trabalho. Para isso, utilizamos as seguintes métricas, que são calculadas a partir da matriz de confusão:

  * **Precisão (Precision)**: Responde à pergunta: **"Dos clientes que o modelo previu que iriam 'churnar', quantos realmente churnaram?"**

      * É uma métrica importante quando o custo de um falso positivo é alto. Por exemplo, se a empresa gasta muito dinheiro em uma campanha de retenção para clientes que não iriam embora, uma alta precisão é fundamental.
      * **Fórmula**: `Precisão = (Verdadeiros Positivos) / (Verdadeiros Positivos + Falsos Positivos)`
      * **Exemplo**: Se o modelo previu 100 clientes com churn e, na verdade, 80 deles realmente cancelaram, a precisão é de 80%. Isso significa que, quando o modelo "dispara o alarme", ele está certo em 80% das vezes.

  * **Recall (Sensibilidade)**: Responde à pergunta: **"Dos clientes que realmente churnaram, quantos o modelo conseguiu identificar?"**

      * É crucial quando o custo de um falso negativo é alto. No nosso caso, um falso negativo é um cliente que o modelo previu que ficaria, mas ele acabou cancelando. A empresa quer evitar ao máximo perder clientes, então um alto recall é muito importante para capturar a maior quantidade possível de clientes em risco.
      * **Fórmula**: `Recall = (Verdadeiros Positivos) / (Verdadeiros Positivos + Falsos Negativos)`
      * **Exemplo**: Se 120 clientes realmente churnaram, mas o modelo só identificou 80 deles, o recall é de aproximadamente 66%. Isso significa que o modelo "perdeu" 40 clientes que estavam em risco.

  * **F1-score**: É uma média harmônica entre a precisão e o recall. Ele busca o equilíbrio entre as duas métricas.

      * O F1-score é particularmente útil quando as classes estão desbalanceadas (existem muito mais clientes que não churnam do que clientes que churnam). Ele penaliza modelos que favorecem uma métrica em detrimento da outra.
      * **Exemplo**: Se um modelo tem precisão de 95% mas recall de apenas 10%, ele é muito preciso, mas não consegue identificar a maioria dos clientes em risco. O F1-score seria baixo, mostrando que o modelo não é bom no geral. Um bom F1-score indica que o modelo tem um bom equilíbrio entre capturar a maioria dos clientes em risco e ser preciso em suas previsões.

### **Como testamos o modelo com dados de mentira?**

Depois que o modelo está treinado, ele precisa ser testado com dados novos, como se um cliente real estivesse ligando. Para isso, o notebook usa dados fictícios.

1.  **Criação dos Dados Fictícios**:
      * O notebook cria um novo `DataFrame` com informações de clientes que não foram usadas no treinamento.
    <!-- end list -->
    ```python
    # Cria um novo DataFrame com dados de exemplo
    df_novo_cliente = pd.DataFrame([
      {'customer_gender': 'Male', 'customer_SeniorCitizen': 'No', ...},
      {'customer_gender': 'Female', 'customer_SeniorCitizen': 'Yes', ...}
    ])
    ```
2.  **Fazendo a Predição**:
      * O pipeline, que já foi salvo e carregado, é usado para fazer a previsão. Ele recebe os dados de mentira e, magicamente, aplica toda a preparação dos dados e o modelo em um único passo.
    <!-- end list -->
    ```python
    # Carrega o modelo treinado
    modelo_carregado = joblib.load('modelo_churn.pkl')
    # Faz a previsão de churn para os novos clientes
    previsoes = modelo_carregado.predict(df_novo_cliente)
    ```
3.  **Interpretando o Resultado**:
      * O resultado é um array com `0`s e `1`s. Um `1` significa que o modelo acha que o cliente vai cancelar, e um `0` significa que ele deve ficar. Essa previsão é adicionada de volta ao `DataFrame` para que a equipe de negócios possa ver quais clientes têm maior risco e tomar uma atitude.

### **Por que a coluna `Churn` é tão importante?**

A coluna `Churn` é a **variável-alvo**, a "resposta" que o modelo precisa aprender a adivinhar.

  * **Para o Treinamento**: A coluna `Churn` é usada como o "gabarito" do nosso modelo. O algoritmo olha para todas as características dos clientes (idade, tipo de contrato, etc.) e a associa com o resultado `Churn` (`1` para cancelou, `0` para não cancelou). É assim que ele aprende quais padrões levam um cliente a ir embora.
  * **Para a Predição**: Quando vamos prever o churn de um cliente novo, essa coluna não existe. O modelo, com base em tudo o que aprendeu, usa as outras informações do cliente para dar uma resposta (`1` ou `0`). Se a coluna `Churn` já estivesse lá, não precisaríamos de um modelo\!

Em resumo, a coluna `Churn` é necessária para ensinar o modelo, mas não é usada para fazer as previsões, pois é o que estamos tentando descobrir.


## 🤝 Contato

Se você tiver dúvidas, sugestões ou quiser discutir o projeto, sinta-se à vontade para entrar em contato:

  * **Nome:** Luciano Azevedo (Especialista Data Science)
  * **Email:** lucianocomputador@gmail.com
  * **LinkedIn:** [Meu Perfil LinkedIn](https://www.linkedin.com/in/luciano-devops/)
  * **GitHub:** [Meu Perfil GitHub](https://github.com/lfa-systems)


-----

## 📄 Licença

Este projeto está licenciado sob a licença **MIT License** - veja o arquivo [LICENSE](https://www.google.com/search?q=MIT+License&oq=MIT+License) para detalhes.

-----