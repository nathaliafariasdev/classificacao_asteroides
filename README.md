# ANÁLISE COMPARATIVA DE MODELOS DE MACHINE LEARNING: DADOS DA NASA
  Este repositório contém o projeto de classificação de periculosidade de asteroides utilizando dados públicos da NASA. 
  
  
## ETAPA 1: ANÁLISE COMPARATIVA INICIAL
  O objetivo principal foi testar e comparar o desempenho de 4 modelos clássicos de Machine Learnig em uma divisão convencional de dados entre treino e teste, avaliando múltiplas métricas de classificação.

### DESEMPENHO DOS 4 MODELOS

| Classificador | Acurácia | Precision | Recall | F-measure |
| :--- | :---: | :---: | :---: | :---: |
| **Regressão Logística** | 0.7400 | 0.000000 | 0.000000 | 0.000000 |
| **Árvore de Decisão** | 1.0000 | 1.000000 | 1.000000 | 1.000000 |
| **KNN (k=5)** | 0.6975 | 0.311111 | 0.134615 | 0.187919 |
| **Random Forest** | 1.0000 | 1.000000 | 1.000000 | 1.000000 |

### Análise inicial:
* **O Fenômeno do 100% (Árvore de Decisão e Random Forest):** Ambos os modelos atingiram a pontuação máxima (1.0000) em todas as métricas. Em um cenário de treino e teste comum, isso indica um forte indício de **Overfitting** (o modelo decorou perfeitamente os dados de treino, perdendo a capacidade de generalizar para dados novos).
* **O Falso Desempenho da Regressão Logística:** Embora tenha apresentado 74% de acurácia, o modelo obteve 0.000000 em Precision, Recall e F-measure. Isso acontece porque o dataset é altamente desbalanceado (há muito mais asteroides seguros do que perigosos). O modelo limitou-se a classificar todos como não perigosos, falhando completamente em identificar os casos reais de perigo.
* **A Sensibilidade do KNN:** O KNN (k=5) obteve a menor acurácia (69.75%) e métricas baixas de precisão e recall. Como as variáveis brutas do dataset possuem escalas muito discrepantes (ex: distâncias na casa dos milhões vs. diâmetros pequenos), as distâncias geométricas foram distorcidas, prejudicando o algoritmo.

## ETAPA 2: OTIMIZAÇÃO DE HIPERPARÂMETROS E VALIDAÇÃO CRUZADA
  Com o objetivo de refinar a capacidade de generalização dos modelos e mitigar os efeitos observados na análise inicial, foi implementada a Validação Cruzada (Cross-Validation) com 5 folds, além do ajuste estratégico de hiperparâmetros.

### 📊 Resultados com Validação Cruzada (5 folds)

| Modelo | Fold 1 | Fold 2 | Fold 3 | Fold 4 | Fold 5 | Média Final |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Decision Tree** (`max_depth=3`) | 100% | 100% | 99.70% | 100% | 99.70% | **99.88%** |
| **KNN** (`n_neighbors=15`) | 72.18% | 71.25% | 72.81% | 71.56% | 71.25% | **71.81%** |

### Interpretação da etapa 2:
* **Mitigando o Overfitting:** Limitando a árvore a `max_depth=3`, ela manteve estabilidade (99.88% de média) sem decorar os dados, excluindo a hipótese de overfitting. Isso prova que o dataset tem regras físicas claras que precisam de poucas perguntas lógicas para classificar.
* **Robustez no KNN:** Mudando para `n_neighbors=15`, o modelo passou a consultar mais vizinhos, reduzindo o impacto de ruídos, o que aumentou a acurácia média para 71.81%.

## Tecnologias utilizadas:
* Python 3
* Scikit-Learn (Regressão Logística, Decision Tree, KNN, Random Forest, cross_val_score e train_test_split)
* Pandas & NumPy (Manipulação das bases de dados)
* Google Colab

## Como executar:
1. Abra os arquivos `.ipynb` presentes neste repositório.
2. Clique no botão **Open in Colab**.
