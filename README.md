# Detecção de Fraude em Cartões de Crédito 💳🕵️‍♂️

Este é um projeto prático focado em resolver um dos problemas mais comuns e complexos do mercado financeiro: a detecção de fraudes em transações de cartão de crédito usando Machine Learning.

## 🎯 O Problema
O grande desafio deste projeto não é apenas criar um modelo de predição, mas sim lidar com os **dados extremamente desbalanceados**. 
Nesta base de dados, descobri que existe cerca de 578 transações normais para cada 1 fraude. 

Por causa disso, usar a "Acurácia" como métrica é uma armadilha. Um modelo que chutar que *nenhuma* transação é fraude acertaria 99,8% das vezes, mas seria inútil para o negócio. Por isso, a avaliação deste projeto foi focada no **Recall** (capacidade de encontrar as fraudes) e na **Precisão**.

O script foi separado da seguinte forma:

## 🛠️ Extração e Preparação de Dados (Blocos 01 a 03):
- O script começa por importar dados de um ficheiro CSV alojado na internet utilizando a biblioteca pandas.
- Calcula a proporção que evidencia o desequilíbrio entre transações normais e fraudulentas.
- Posteriormente, os dados são divididos numa proporção de 70% para treino e 30% para teste.
- São também aplicadas técnicas para lidar com classes desbalanceadas, especificamente o Undersampling e o Oversampling através do método SMOTE.

## 🤖 Treino de Modelos (Blocos 04 a 07):
Testei diferentes abordagens e algoritmos. Aqui estão os resultados focados na Classe 1 (Fraudes):

1. **Regressão Logística (utilizando um Pipeline com StandardScaler e SMOTE):** Obteve um Recall de 0.63.
3. **Random Forest (com class_weight='balanced'):** Obteve um Recall de 0.80.
4. **XGBoost (com limiar ajustado para 0.20):** Obteve um Recall de 0.78 com precisão de 0.86.
5. **XGBoost (com a técnida de desbalanciamento Undersampling):** Obteve um Recall de 0.90, porém com uma precisão de 0.04.
6.  **XGBoost (com a técnida de desbalanciamento Oversampling):** Obteve um Recall de 0.82 com uma precisão de 0.82.

**O modelo XGBoost é destacado como tendo o melhor desempenho! O modelo com Undersampling obteve o maior Recall (0.90), se tornando a melhor opção para priorizar a detecção de fraudes a qualquer custo. No entanto, o XGBoost com SMOTE (0.82) apresenta um equilíbrio melhor se o objetivo for não incomodar tantos clientes legítimos.**

## 📈 Avaliação de Desempenho para medir a eficácia dos modelos(Blocos 08 e 09):

- Sscript gera gráficos da Curva ROC, analisando as taxas de falsos positivos e verdadeiros positivos:
<img width="619" height="455" alt="image" src="https://github.com/user-attachments/assets/75838521-0eb3-4b61-97fc-e8e4d469ca9e" />

- Curva Precision-Recall, sendo como a mais importante para a análise de fraudes.
<img width="567" height="454" alt="image" src="https://github.com/user-attachments/assets/250c5a09-36df-42ef-a269-3e3af397c311" />

## 📊 Explicabilidade do Modelo com SHAP(Blocos 10 a 12):

Com o auxílio da biblioteca SHAP, o código analisa o comportamento interno do modelo XGBoost nas decisões tomadas e demonstram quais as características ou colunas que mais denunciam a ocorrência de uma fraude.
Para isso, desenha gráficos de:
- Barras
<img width="872" height="569" alt="image" src="https://github.com/user-attachments/assets/546086e3-8640-4606-a410-cc93e5d2217f" />

- diagramas de dispersão (beeswarm)
<img width="864" height="498" alt="image" src="https://github.com/user-attachments/assets/27f9e9aa-b4a8-450f-8c44-0940d2baffc6" />

- E gráficos em cascata (waterfall):
<img width="911" height="598" alt="image" src="https://github.com/user-attachments/assets/fd5cd8c9-9b38-4a68-b9ac-f67b50f7ac34" />


**O gráfico Beeswarm revelou que as variáveis V14 e V4 são as que mais impactam na decisão de apontar uma transação como fraude.**

## 🧠 Validação e Ajuste de Regras de Negócio (Blocos 13 a 15):

O script destes blocos implementa uma validação cruzada estratificada em 5 cortes para testar a consistência do modelo com base na métrica de Área sob a Curva Precision-Recall (AUPRC). 
Por fim, treina o modelo final gerando matrizes de confusão e efetua um ajuste no limiar de probabilidade de 50% para 20%, priorizando a deteção de um maior número de fraudes, mesmo que isso implique o aumento de alarmes falsos.

## 🚀 O que fiz de diferente (Meu toque pessoal)
Em relação à abordagem inicial sugerida na aula, eu adicionei:
- O cálculo dinâmico da proporção de classes para ajustar o peso no XGBoost.
- O ajuste manual do limiar de decisão (`predict_proba`) para ser mais rígido com possíveis fraudes.
- A implementação do SMOTE apenas no conjunto de treino, evitando vazamento de dados (*Data Leakage*) no momento da avaliação final.
- Adicionei uma validação cruzada rigorosa nos dados provando que o modelo é estável.
- Implementei uma métrica de negócio, onde foi utilizado uma Matriz de Confusão para ilustrar o trade-off (a troca) entre bloquear transações reais e barrar fraudes, ajustando a nota de corte visualmente.
