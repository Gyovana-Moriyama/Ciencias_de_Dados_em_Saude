# Project Mortality Prognosis

## Presentation

This project originated in the context of the activities of the course Science and Data Visualization in Health, offered in the first semester of 2022, at Unicamp.

| Name                    | RA     | Specialization |
| :---:                   | :---:  |  :---:         |
| Alexandre Dias Negretti | 233609 | Computing      |
| Daniel Godoy Marques    | 166213 | Computing      |
| Gyovana Mayara Moriyama | 216190 | Computing      |

## Contextualization of the Proposal
> Apresentação da proposta de predição indicando os parâmetros adotados para a mesma com a justificativa (por que esses parâmetros foram adotados?). O ideal é que a proposta seja apresentada como uma pergunta de pesquisa.

In this project we will predict whether patients who had a Acute deep venous thrombosis (disorder) condition (SNOMED-CT: 132281000119108) will die within a maximum of 5 years.
We chose this condition because according to a specialist: 
 * It is a fairly common one, which gives us a good amount of data to work with.
 * It affects both men and women of all ages.
 * It's severity is linked to a lot of different factors, which makes it a non-trivial problem.

We chose to look at a time window of 5 years because it is enough time to treat the underlying cause or to define that the pacient belongs to a very high risk group. So the thrombosis would have a higher chance to contribute to the pacient death.

### Tools
*  Pandas
*  Numpy
*  Matplotlib
*  Seaborn
*  SMOTE
*  Scikit-learn

# Methodology

## Bases Adopted for the Study

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

 
## Our Dataframe

### Pre-processing

### Feature Engineering
> Explicar as colunas do nosso df final (descrever as features)

# Results 
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

# Project Evolution
> Seção opcional se houver histórico de mudanças e evolução relevantes.
> Relate aqui a evolução do projeto: possíveis problemas enfrentados e possíveis mudanças de trajetória. Relatar o processo para se alcançar os resultados é tão importante quanto os resultados.

# Discussion
> Fazer um breve debate sobre os resultados alcançados. Aqui pode ser feita a análise dos possíveis motivos que certos resultados foram alcançados. Por exemplo:
> * por que seu modelo alcançou (ou não) um bom resultado?
> * por que o modelo de um cenário não se desempenhou bem em outro?
>
> A discussão dos resultados também pode ser feita opcionalmente na seção de Resultados, na medida em que os resultados são apresentados. Aspectos importantes a serem discutidos: É possível tirar conclusões dos resultados? Quais? Há indicações de direções para estudo? São necessários trabalhos mais profundos?

# Conclusion
Explicar o motivo para ter usado cada tipo de encoder e suas limitações: por exemplo, atribuir o valor 0 para uma das categorias por ter "consequências"
## Future work

Trabalhos Futuros: criar uma feature "quantidade de condições" que diz quantas condições "ruins" aquela pessoa tem


> Destacar as principais conclusões obtidas no desenvolvimento do projeto.
>
> Destacar os principais desafios enfrentados.
>
> Principais lições aprendidas.
>
> Trabalhos Futuros:
> * o que poderia ser melhorado se houvesse mais tempo?
