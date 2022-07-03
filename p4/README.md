# Project 4 –  Classification of white matter lesions in lupus



# Presentation

This project originated in the context of the activities of the course Science and Data Visualization in Health, offered in the first semester of 2022, at Unicamp.

| Name                    | RA     | Specialization |
| :---:                   | :---:  |  :---:         |
| Alexandre Dias Negretti | 233609 | Computing      |
| Daniel Godoy Marques    | 166213 | Computing      |
| Gyovana Mayara Moriyama | 216190 | Computing      |

# Contextualization of the Proposal
> Apresentação de forma resumida do problema (contexto) e a pergunta que se quer responder.

The objective of this project is, given resonance images of ischemic and demyelinating lesions in the brain white matter, train a classifier that learns how to differentiate this lesions. So, using this trained classifier we need to answer the following question:

Lupus white matter lesions etiology are more similar to demyelinating or ischemic lesions?

## Tools
*  Numpy
*  Pandas 
*  Cv2
*  Matplotlib
*  Pillow
*  Scipy
*  glrlm
*  Scikit-Image
*  Scikit-Learn

## Pre-processing and data usage

> Descreva o pipeline de pré-processamento dos dados:
* normalização (se houver)
* outros processamentos
* uso das máscaras (se houver)
* extração de atributos (se houver)
* seleção de atributos (se houver)
> Justificar as escolhas

###  Min Max Normalization on raw images:

### Mask application:

### Dataset creation:

* Label and id creation
* Feature extraction:
  * Features extracted from histogram
  * Features extracted from GLCM
  * Features extracted rom GLRLM
  * Features extracted from LBP
 * Features selection: remove lbp features that the ratio between the sum of the column and the size of the dataset if less than 10. We do this to remove lbp columns that the majority value is 0.


# Metodologia
> Descreva o classificador escolhido e o pipeline de treinamento:
* split dos dados de treinamento
* escolha de parâmetros do classificador
* validação cruzada
* métricas de avaliação
* resultados do treinamento do classificador usando tabelas e gráficos
>
> Justificar as escolhas.
> Esta parte do relatório pode ser copiada da Atividade 11, caso o grupo opte por usar o SVM já treinado.

# Resultados Obtidos e Discussão
> Esta seção deve apresentar o resultado de predição das lesões de LES usando o classificador treinado. Também deve tentar explicar quais os atributos relevantes usados na classificação obtida
> * apresente os resultados de forma quantitativa e qualitativa
> * tenha em mente que quem irá ler o relatório é uma equipe multidisciplinar. Descreva questões técnicas, mas também a intuição por trás delas.

# Conclusão
> Destacar as principais conclusões obtidas no desenvolvimento do projeto.
>
> Destacar os principais desafios enfrentados.
>
> Principais lições aprendidas.
>
> Trabalhos Futuros:
> * o que poderia ser melhorado se houvesse mais tempo?

# Referências Bibliográficas
> Lista de artigos, links e referências bibliográficas (se houver).
>
> Fiquem à vontade para escolher o padrão de referenciamento preferido pelo grupo.
