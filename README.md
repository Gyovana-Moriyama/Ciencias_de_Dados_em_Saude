# Project Mortality Prognosis

## Presentation

This project originated in the context of the activities of the course Science and Data Visualization in Health, offered in the first semester of 2022, at Unicamp.

| Name                    | RA     | Specialization |
| :---:                   | :---:  |  :---:         |
| Alexandre Dias Negretti | 233609 | Computing      |
| Daniel Godoy Marques    | 166213 | Computing      |
| Gyovana Mayara Moriyama | 216190 | Computing      |

## Contextualization of the Proposal

In this project we will predict whether patients who had a Acute deep venous thrombosis (disorder) condition (SNOMED-CT: 132281000119108) will die within a maximum of 5 years.
We chose this condition because according to a specialist: 
 * It is a fairly common one, which gives us a good amount of data to work with.
 * It affects both men and women of all ages.
 * It's severity is linked to a lot of different factors, which makes it a non-trivial problem.

We chose to look at a time window of 5 years because it is enough time to treat the underlying cause or to define that the patient belongs to a very high risk group. So the thrombosis would have a higher chance to contribute to the patient death.

### Tools
*  Pandas
*  Numpy
*  Matplotlib
*  Seaborn
*  SMOTE
*  Scikit-learn

## Methodology

### Bases Adopted for the Study

We choose these two bases because on scenario01 and scenario02 we found a really small number of ocurrencies of our patients of interest.

* scenario04
* scenario03

The data provided is from a synthetic medical database, Synthea. This one provides us with several tables with various information about the patient. We will use the following tables and informations:

**patients** - Patient demographic data

| Column    | Description                                                                    |
| :---      | :---                                                                           |
| Id        | key that identifies the patient                                                |
| BIRTHDATE | patient birthdate                                                              |
| DEATHDATE | patient death date                                                             |
| GENDER    | patient gender                                                                 |


 
**encounters** - Patient encounter data

| Column          | Description                                                                               |
| :---            | :---                                                                                      |
| Id              | key that identifies the encounter                                                         |
| START           | encounter start date                                                                      |
| STOP            | encounter end date                                                                        |
| PATIENT         | key that identifies the patient                                                           |
| ORGANIZATION    | key of the organization that the encounter took place                                     |
| ENCOUNTERCLASS  | encounter class, as: ambulatory, emergency, inpatient, wellness, ou urgentcare            |

 

**conditions** - Patient conditions or diagnoses

| Column          | Description                                                                               |
| :---            | :---                                                                                      |
| START           | condition start date                                                                      |
| STOP            | condition end date                                                                        |
| PATIENT         | key to that identifies the patient                                                        |
| ENCOUNTER       | key that identifies the encounter                                                         |
| CODE            | SNOMED-ST code of the condition                                                           |
| DESCRIPTION     | description of the condition                                                              |

 

**imaging_studies** - Patient imaging metadata

| Column          | Description                                                                               |
| :---            | :---                                                                                      |
| PATIENT         | key that identifies the patient                                                           |
| ENCOUNTER       | key that identifies the encounter                                                         |
| MODALITY_CODE   | code that identifies the type of imaging exam                                             |
| Id              | key that identifies each imaging exam                                                     |


 
**medications** - Patient medication data

| Column          | Description                                                                               |
| :---            | :---                                                                                      |
| START           | medication start date                                                                     |
| STOP            | medication end date                                                                       |
| PATIENT         | key that identifies the patient                                                           |
| ENCOUNTER       | key that identifies the encounter                                                         |
| CODE            | RxNorm medication code                                                                    |
| DESCRIPTION     | medication description                                                                    |


 
**organizations** - Provider organizations including hospitals

| Column          | Description                                                                               |
| :---            | :---                                                                                      |
| Id              | key that identifies the organization                                                      |
| NAME            | name of the organization                                                                  |


 
**procedures** - Patient procedure data including surgeries

| Column          | Description                                                                               |
| :---            | :---                                                                                      |
| START           | procedure start date                                                                      |
| STOP            | procedure end date                                                                        |
| PATIENT         | key that identifies the patient                                                           |
| ENCOUNTER       | key that identifies the encounter                                                         |
|  CODE           | SNOMED-CT code of the procedure                                                           |
| DESCRIPTION     | description of the procedure                                                              |

 
### Our Dataframe

#### Pre-processing

#### Feature Engineering
> Explicar as colunas do nosso df final (descrever as features)

## Results 
> Esta seção pode opcionalmente ser apresentada em conjunto com a metodologia, intercalando método e resultados.
>
> Descreva etapas para obtenção do modelo, incluindo tratamento de dados, se houve.
>
> Apresente o seu modelo de predição e resultados alcançados.
> Para cada modelo, apresente no mínimo:
> * quais os dados sobre o paciente que serão usados para a predição;
> * qual a abordagem/modelo adotado;
> * resultados do preditor (apresente da forma mais rica possível, usando tabelas e gráficos - métricas e curva ROC são bem vindos);
> * breve discussão sobre os resultados obtidos.
>
> Nesta seção, lembre-se das sugestões de análise:
> * analisar diferentes composições de treinamento e análise do modelo:
>   * um modelo treinado/validado no cenário 1 e testado no cenário 2 e vice-versa;
>   * um modelo treinado e validado com os dados dos dois cenários;
>   * nos modelos dos dois itens anteriores:
>     * houve diferença de resultados?
>     * como analisar e interpretar as diferenças?
> * testar diferentes composições de dados sobre o paciente para a predição (por exemplo, quantidade diversificadas de número de itens).

### The problem: 

As we were trying to predict if the patient died within a maximum of 5 years, we had a **classification** problem where 1 indicates that the patient died and 0 that the patient is alive or died after the 5 years interval. 

### Input: 

**scenario03.csv and scenario04.csv**

With this input we've made different compositions, as follows:
 * Scenario04 as train and Scenario03 as test
 * Scenario03 as train and Scenario04 as test
 * Mixing both and doing a train-test split

For each of this, we still have this subdivisions: 
* Unbalanced data, using all features
* Unbalanced, using only features that already existed on the original tables
* Unbalanced, removing only the feature "condition_dur"
* Oversampled data, using all features
* Oversampled, using only features that already existed on the original tables
* Oversampled, removing only the feature "condition_dur"

On our exploratory data analysis, we saw that the target was imbalanced:

![image](https://user-images.githubusercontent.com/38329077/168669739-ba837c90-766b-4cf6-8cb7-708c8b73e5b1.png)

Therefore, we did some tests using SMOTE to do a oversample on the minority class (in this case, class 1), so we have a balanced dataset to train our models. 

### Models: 

We tried different machine learning models for each subdivision of each dataset composition, such as Random Forest, LGBM, XGBoost and Logistic Regression. We also made some baselines for each dataset composition.

### Results

Here we are presenting metrics results for the test.

**Scenario04 (630 patients) as train and Scenario03 (70 patients) as test:**

![04_03_new](https://user-images.githubusercontent.com/38329077/168700205-3a43b0b0-c175-43ae-b39b-486b4c5644fe.png)

On this composition we can see that in Imbalanced with all features and SMOTE with all features, Random Forest, LGBM and XGBoost achieved 1.0 on all three metrics, which means the models really predicted correct all labels, we believe this happened because of the correlation of the features with target and that the models trained on a larger base compared to the test, so the models really learned how to separate the positive and negative classes, as we can see for AUC = 1.0.

For the tests with just the original features we can see that either on imbalanced and with SMOTE, the models performed poorly, we believe this happened because of the low quantity of features and that the features are not explanatory for the problem, so the models do not learn to separate the positive and negative classes, as we can see for AUC around 0.5. Also, we can see that the models do not really predict positive classes right, as we have really low values for F1 score.

Finally, for tests without 'condition_dur' feature, in the imbalanced case most of the models still perform well, but for the tests with SMOTE, half of models had a low accuracy and F1 score, so we can see that they are not predicting well for both classes, although all of them have high AUC. Therefore we can conclude that even if the models learned how to separate the classes (high AUC), they did not predicted right both classes (low accuracy and low F1 score).

In conclusion for this composition we can see that the best option is to use all features and imbalanced data, 

> tentar falar o pq do com SMOTE nao ter sido o melhor

**Scenario03 (70 patients) as train and Scenario04 (630 patients) as test:**

![03_04_new](https://user-images.githubusercontent.com/38329077/168700183-25bf90ef-1443-43c1-8311-f580aca87ace.png)

On this composition the tests on either imbalanced and with SMOTE data and with all features were the best, this time they did not get 1.0 on any metrics, but got really close. Here the models got high values for all three metrics, so we can see that the models learned how to distinguish between the classes (high AUC) and predicted almost all correct (high accuracy and F1 score).

For the tests with just the original features we can see that either on imbalanced and with SMOTE data, the models performed poorly. For the tests on imbalanced data, we have a high accuracy but low AUC and F1 score, so we can say that the models might be predicting the majority class (class 0), so they are did not learn how to distinguish between the classes (AUC around 0.5) and are not predicting the positive class right (low F1 score).

Finally, for the tests without 'condition_dur', we can see that most of the models performed well and both imbalanced and with SMOTE had similar results for each model except for Logistic Regression that even with a high AUC, had a low F1 score and accuracy, so we can say that this model learned to distinguish between the classes but did not predict right the classes.

In conclusion for this composition, we can see that as the train data was smaller, the models did not learn as well as the composition above. Therefore the best results were produced by the models on imbalanced data, which is similar to the composition above.

**Mixing both and doing a train-test split (630 for train and 70 for test):**

![mixed_new](https://user-images.githubusercontent.com/38329077/168700216-6037a8d5-689e-4175-a76e-a6290b42110b.png)

On this composition the tests on either imbalanced and with SMOTE data and with all features were the best Random Forest, LGBM and XGBoost achieved 1.0 on all three metrics, which means that the these models learned how to distinguish between the classes (AUC = 1.0) and predicted both classes right for all cases (accuracy = 1.0 and F1 score = 1.0).

For the test with just the original features we can see that all the models produced the same scores on either imbalanced and with SMOTE data. For the imbalanced data, we have high accuracy, but low AUC and F1 score, which means that the models did not learn how to distinguish between the classes (AUC around 0.5) and that they are not predicting the positive class right (F1 score low or equals zero), so we can say that the models might be predicting the target ad the majority class (class 0), what would explain the high accuracy and low AUC and F1 score. Considering the data with SMOTE, we have all the three metrics with low values, which indicates that the models are not predicting the majority class as the target, and did not learn to distinguish between the classes and to predict the positive class right.

Finally, for the tests without 'condition_dur', we can see that all models performed well on both imbalanced and with SMOTE data, for all three metrics. Even though the models did not have the best result for this composition, the subdivision got really satisfactory results.

In conclusion for this composition, we can see that the models performed the best with all features on both imbalanced and with SMOTE data.

> Colocar mais alguma coisa na conclusao 

#### Discussion
> Fazer um breve debate sobre os resultados alcançados. Aqui pode ser feita a análise dos possíveis motivos que certos resultados foram alcançados. Por exemplo:
> * por que seu modelo alcançou (ou não) um bom resultado?
> * por que o modelo de um cenário não se desempenhou bem em outro?
>
> A discussão dos resultados também pode ser feita opcionalmente na seção de Resultados, na medida em que os resultados são apresentados. Aspectos importantes a serem discutidos: É possível tirar conclusões dos resultados? Quais? Há indicações de direções para estudo? São necessários trabalhos mais profundos?

With the results above we can conclude that on all cases, using all features is the best option

> pq?

but the composition with the best results was the one with mixed data from both tables, this might have happened because in this composition we have a bigger variety of data

> mais algo sobre o pq do mixed ter sido melhor?

> tentar hipotetizar do pq o SMOTE foi pior em todos os casos

If we were to choose a model, it would be Random Forest considering that on a overall, this model performed the best on most subdivisions for all compositions.

After testing the models using all features, we noticed that the feature "condition_dur" that we've created was extremely correlated to the target (0.97). Consulting the specialist we also discovered that on a real environment we probably wouldn't have the "stop_condition" value that is used to create the "condition_dur" feature.
So we've opted to test the models performances excluding this feature as well.

It is important to highlight a disclaimer in regard to the stop_condition information. Acute deep venous thrombosis is a condition that often requires long-term anticoagulation therapy and follow-up. The criteria to consider the "stop" is unclear in the data provided (e.g.: cease anticoagulant therapy, absence of clinical signs or symptoms of thrombosis, adequate pain and edema management, etc.) and may not be applicable in other scenarios.

Here we can see the heatmap of the features correlation:

![image](https://user-images.githubusercontent.com/38329077/168676969-9f1cb909-da1c-42a7-b157-7d5e1a6b6177.png)

Therefore, it can be seen that the features with higher correlation are the ones created by us, this can explain why the tests with just original features performed so poorly.

## Project Evolution
> Seção opcional se houver histórico de mudanças e evolução relevantes.
> Relate aqui a evolução do projeto: possíveis problemas enfrentados e possíveis mudanças de trajetória. Relatar o processo para se alcançar os resultados é tão importante quanto os resultados.

Limitações:

ordinal encoder
todos os hospitais do df_train não existem em df_test e vice versa, portanto essa feature será inutil a nao ser que juntemos os 2 dfs e depois façamos um split dessa junçao

ha um encounterclass no df_train que não existe no df_test

cts -> na vida real seria bom, mas como são dados sintéticos não dá pois a maioria dos dados é 0

fazer cross validation -> Limitações

Explicar o motivo para ter usado cada tipo de encoder e suas limitações: por exemplo, atribuir o valor 0 para uma das categorias por ter "consequências"

## Conclusion


### Future work

Trabalhos Futuros: criar uma feature "quantidade de condições" que diz quantas condições "ruins" aquela pessoa tem
testar standard e minmax e comparar as performances

fazer tuning dos hyper parameters através de GridSearch, RandomSearch e optuna

seeds diferentes(futuro) no dataset misturado

testar sem o cts

ver importância das features


> Destacar as principais conclusões obtidas no desenvolvimento do projeto.
>
> Destacar os principais desafios enfrentados.
>
> Principais lições aprendidas.
>
> Trabalhos Futuros:
> * o que poderia ser melhorado se houvesse mais tempo?
