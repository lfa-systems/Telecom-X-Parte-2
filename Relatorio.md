## Relatório Detalhado: Análise Preditiva de Churn e Fatores de Evasão em Telecomunicações

### 1. Introdução
Este relatório detalha os resultados obtidos a partir da análise preditiva de churn de clientes, utilizando modelos de Machine Learning (Random Forest e XGBoost). O objetivo principal é identificar os clientes com maior probabilidade de cancelar seus serviços, entender os principais motivos que levam a essa decisão e propor estratégias de retenção eficazes.

### 2. Metodologia e Modelos Utilizados
O projeto utilizou um pipeline robusto que incluiu o pré-processamento dos dados (normalização de variáveis numéricas e codificação de variáveis categóricas) e o treinamento de dois modelos de classificação:

* **Random Forest**: Um modelo de "floresta de árvores de decisão" que combina o resultado de múltiplas árvores para gerar uma previsão mais precisa e robusta. Ele é excelente para lidar com dados complexos e identificar a importância das variáveis.
* **XGBoost**: Um modelo de "gradient boosting" conhecido por sua alta performance e precisão. Ele constrói árvores de decisão de forma sequencial, onde cada nova árvore corrige os erros da anterior, otimizando o resultado.

Ambos os modelos foram treinados em um conjunto de dados históricos e avaliados em um conjunto de teste para garantir que suas previsões são confiáveis e não "decoraram" os dados de treinamento (overfitting).

### 3. Análise de Desempenho dos Modelos
O desempenho dos modelos foi avaliado com métricas-chave para problemas de classificação:

* **Precisão**: Dos clientes que o modelo previu que iriam cancelar, quantos realmente cancelaram? Uma alta precisão significa que, quando o modelo "dispara o alarme", ele está certo na maioria das vezes.
* **Recall**: Dos clientes que realmente cancelaram, quantos o modelo conseguiu identificar? Um alto recall é crucial em nossa estratégia, pois queremos encontrar o maior número possível de clientes em risco para poder agir.
* **F1-score**: Uma métrica que equilibra precisão e recall, sendo especialmente útil em problemas onde as classes (churn e não-churn) são desbalanceadas.

A matriz de confusão forneceu uma visão detalhada de acertos e erros, permitindo a comparação entre o desempenho do Random Forest e do XGBoost para determinar qual modelo é mais adequado para nossa estratégia de retenção.

### 4. Principais Fatores que Influenciam a Evasão (Churn)
Com base na análise das variáveis do conjunto de dados, os fatores que mais influenciam a probabilidade de um cliente cancelar seu serviço são:

* **Tipo de Contrato (`account_Contract`)**: Clientes com **contratos mensais** apresentam uma taxa de churn significativamente mais alta do que aqueles com contratos de um ou dois anos. A falta de um compromisso de longo prazo torna a decisão de cancelamento mais fácil e frequente.
    * **Exemplo**: Um cliente com contrato mensal pode cancelar a qualquer momento sem penalidade, o que o torna mais propenso a buscar ofertas da concorrência. Já um cliente com contrato de 24 meses tem uma barreira para a evasão.

* **Serviços Adicionais (`internet_OnlineSecurity`, `internet_TechSupport`)**: Clientes que não assinam serviços de segurança online ou suporte técnico tendem a ter uma taxa de churn maior. A ausência desses serviços pode indicar uma menor satisfação ou uma percepção de menor valor agregado ao pacote, resultando em insatisfação e cancelamento.
    * **Exemplo**: Um cliente sem suporte técnico pode se sentir desamparado ao enfrentar problemas, enquanto outro com suporte se sente mais seguro e valorizado.

* **Método de Pagamento (`account_PaymentMethod`)**: O método de pagamento de **cheque eletrônico** está frequentemente associado a uma alta taxa de churn. Isso pode sugerir que esses clientes estão menos engajados com a empresa e mais propensos a usar serviços digitais que os levam a outras plataformas e concorrentes.
    * **Exemplo**: A praticidade e a automação do débito automático ou do cartão de crédito podem criar uma inércia positiva, enquanto o cheque eletrônico exige uma ação mais ativa e pode ser facilmente interrompido.

* **Cobrança Mensal (`account_MonthlyCharges`)**: Clientes com faturas mensais mais elevadas têm uma probabilidade maior de churn. Isso pode ser um indicativo de que estão pagando mais por serviços que consideram que não valem o custo, ou que encontraram uma oferta similar ou melhor na concorrência.
    * **Exemplo**: Um cliente que paga R$ 150 por um plano básico pode se sentir sobrecarregado e insatisfeito, enquanto outro que paga R$ 50 pelo mesmo plano considera a oferta um bom custo-benefício.

### 5. Estratégias de Retenção Propostas
Com base nos fatores de churn identificados, as seguintes estratégias de retenção são propostas para mitigar a evasão de clientes:

1.  **Foco em Contratos de Longo Prazo**: Crie ofertas atrativas para migrar clientes de contratos mensais para contratos anuais ou de dois anos. Ofereça descontos significativos nas mensalidades ou benefícios adicionais (serviços de streaming, upgrade de velocidade, etc.) para clientes que se comprometem por um período mais longo.
2.  **Pacotes de Serviços e Valor Agregado**: Promova ativamente os serviços de segurança online e suporte técnico. Crie pacotes promocionais que incluam esses serviços sem custo adicional por um período, mostrando ao cliente o valor agregado e aumentando o engajamento.
3.  **Incentivo a Métodos de Pagamento Automáticos**: Estimule a migração de clientes que usam cheque eletrônico para métodos de pagamento mais automatizados, como débito em conta ou cartão de crédito. Ofereça pequenos descontos ou benefícios exclusivos para aqueles que adotarem essas formas de pagamento.
4.  **Campanhas de Personalização de Preços**: Identifique clientes com cobranças mensais elevadas e ofereça revisões de plano personalizadas. Negocie com clientes de alto risco de churn para ajustar seus pacotes, garantindo que eles sintam que estão recebendo um bom valor pelo que pagam.


## 🤝 Contato

Se você tiver dúvidas, sugestões ou quiser discutir o projeto, sinta-se à vontade para entrar em contato:

  * **Nome:** Luciano Azevedo (Especialista Data Science)
  * **Email:** lucianocomputador@gmail.com
  * **LinkedIn:** [Meu Perfil LinkedIn](https://www.linkedin.com/in/luciano-devops/)
  * **GitHub:** [Meu Perfil GitHub](https://github.com/lfa-systems)
