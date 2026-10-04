# Deteccao_anomalias_transasoes

# Detecção de Fraude em Cartões de Crédito 💳🕵️‍♂️

Este é um projeto prático focado em resolver um dos problemas mais comuns e complexos do mercado financeiro: a detecção de fraudes em transações de cartão de crédito usando Machine Learning.

## 🎯 O Problema
O grande desafio deste projeto não é apenas criar um modelo de predição, mas sim lidar com os **dados extremamente desbalanceados**. 
Nesta base de dados, descobri que existe cerca de Existe cerca de 578 transações normais para cada 1 fraude. 

Por causa disso, usar a "Acurácia" como métrica é uma armadilha. Um modelo que chutar que *nenhuma* transação é fraude acertaria 99,8% das vezes, mas seria inútil para o negócio. Por isso, a avaliação deste projeto foi focada no **Recall** (capacidade de encontrar as fraudes) e na **Precisão**.

## 🛠️ Preparação dos Dados
Para garantir que os modelos pudessem aprender corretamente, realizei as seguintes etapas:
- Carregamento dos dados originais protegidos por PCA (variáveis V1 a V28).
- Padronização (StandardScaler) da variável de valor da transação (`Amount`).
- Separação de treino e teste mantendo a mesma proporção de fraudes (`stratify=y`).
- [ PREENCHA: Cite que você usou o SMOTE para criar dados sintéticos e equilibrar o treino ]

## 🤖 Comparação dos Modelos
Testei diferentes abordagens e algoritmos. Aqui estão os resultados focados na Classe 1 (Fraudes):

1. **Regressão Logística (Baseline):** 
2. **Random Forest (com class_weight='balanced'):** 
3. **XGBoost (com limiar ajustado para 0.20):** 
4. **XGBoost (treinado com SMOTE):** 
O melhor modelo foi o XXXYYYY, pois conseguiu aumentar a detecção de fraudes sem derrubar tanto a precisão.

## 🧠 Explicabilidade com SHAP
Para não deixar o modelo como uma "caixa preta", utilizei a biblioteca SHAP para entender como as decisões foram tomadas.
- O gráfico Beeswarm revelou que as variáveis são as que mais impactam na decisão de apontar uma transação como fraude.

## 🚀 O que fiz de diferente (Meu toque pessoal)
Em relação à abordagem inicial sugerida na aula, eu adicionei:
- O cálculo dinâmico da proporção de classes para ajustar o peso no XGBoost.
- O ajuste manual do limiar de decisão (`predict_proba`) para ser mais rígido com possíveis fraudes.
- A implementação do SMOTE apenas no conjunto de treino, evitando vazamento de dados (*Data Leakage*) no momento da avaliação final.
