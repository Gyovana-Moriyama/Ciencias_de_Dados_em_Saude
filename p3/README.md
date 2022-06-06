# Project 3 - Reproducing a Scientific Paper Experiment

The general objective of the project is to reproduce an experiment (totally or partially) from a scientific paper. The topic of the article have to be related to Network Science and Health.

# Presentation

This project originated in the context of the activities of the course Science and Data Visualization in Health, offered in the first semester of 2022, at Unicamp.

| Name                    | RA     | Specialization |
| :---:                   | :---:  |  :---:         |
| Alexandre Dias Negretti | 233609 | Computing      |
| Daniel Godoy Marques    | 166213 | Computing      |
| Gyovana Mayara Moriyama | 216190 | Computing      |

# Bibliographic reference of the article read

[1] G. Scardoni, M. Petterlini, & C. Laudanna, "Analyzing biological network parameters with CentiScaPe." Bioinformatics 25(21), 2857–2859 (2009)

https://academic.oup.com/bioinformatics/article/25/21/2857/227713?login=false

# Summary
> Escreva um breve do artigo (com as suas palavras, não deve ser copiado texto do artigo).

The paper proposes a Cytoscape plug-in called CentiScape. This plug-in calculates many network centrality parameters and allows users to analyze existing correlations between providen data and node centrality values calculated by the plug-in. To demonstrate the proposed plug-in the authors test the plug-in on the human-phosphatome base, a global human protein interactome dataset complied from some public datatsets. They extract a subset of this network, with only known interactions between human protein kinases and phosphatases and compute several centrality values and plots.

# Brief description of the experiment/analysis of the article that was replicated
> Descreva brevemente a parte do artigo cujo experimento ou análise foi reproduzido. Explique o que foi usado como entrada e saída.

The experiment was done using the human-phosphatome base (bioinf-2009-0193-File021.sif), extracting a subset of this network, keeping only known interactions between human protein kinases and phosphatases (the extraction was done using bioinf-2009-0193-File013.xls file). So after the filtering, the network had 549 nodes and 3844 edges.

The authors use CentiScape to calculate centrality paramenters. Firstly, they make a general overview of the global topological properties, that comes from min, max and average values of all computed centralities along with the diameter and the average distance of the network. With these centralities values, they make a first ranking of human kinases and phosphatases according to their central role in the network. After, they plot degree over degree of the network, which shows that the distribution is not uniform. To confirm this suggestion, they analyze the centroid, so they plot centroid over centroid, which provided a linear distribution and as for degree, the distribution was not uniform. To extract the most relevant nodes, they used CentiScaPe to select all nodes having centrality values over the average.

Finally, the authors test the regulatory role of proteins in the kino-phosphatome network in a context-selective manner. To make this test, they focus on the analysis on human PMNs (phosphorylation data: bioinf-2009-0193-File017.NA, bioinf-2009-0193-File018.NA, bioinf-2009-0193-File019.NA). These experimental data were loaded as node attributes in Cytoscape and the computed centrality values were plotted over values of protein phosphorylation. In this experiment, each node is represented with two coordinates consisting of calculated centrality and of experimental data regarding protein phosphorylation. Considering all this, they analyze the plot of centroid values over intensity of protein phosphorylation.

## Data used as input
Dataset | Web address | Descriptive summary
----- | ----- | -----
Human kino-phosphatome network (2009) | https://academic.oup.com/bioinformatics/article/25/21/2857/227713?login=false#supplementary-data | A bipartite network of known human protein kinases and the phosphatases they interact with, as extracted from a global human protein interactome dataset complied from various public databases. 

(Inside the supplement .zip file, see bioinf-2009-0193-File021.sif, which contains a superset of the interactions; extract those interactions among the names in the bioinf-2009-0193-File013.xls file to get the relevant network.)

# Method
> Método usado para a análise -- adaptações feitas, ferramentas utilizadas, abordagens de análise adotadas e respectivos algoritmos.
> Etapas do processo reproduzido.

To reproduce the article experiments we used Cytoscape 3.9.1 for Windows 64bit (https://cytoscape.org/) with the plug-in proposed in the paper, CentiScaPe 2.2 (https://apps.cytoscape.org/apps/centiscape). 

The filtered network was imported through File > Import > Network from File. To configure the nodes and edges, the following setup was made:

![Network import settings](https://i.imgur.com/NBMhmai.png)

After importing the network, it is possible to import the Nodes Attributes through File > Import > Table from File. In order to do that, we changed the PMNs files (bioinf-2009-0193-File017.NA, bioinf-2009-0193-File018.NA, bioinf-2009-0193-File019.NA) replacing equal signal for comma and saving it as csv. At the import settings, the following configuration was used:

![Import Table settings](https://i.imgur.com/J0BKuyK.png)

After importing the three tables, Cytoscape assign the weight for each node adding a column for each weight at the Nodes Table.

CentiScape is available in the Apps tab, after being installed through jar file or App Manager.
At the date the paper was written, CentiScaPe was able to calculate the following centrality parameters:  Average Distance, Diameter, Degree, Stress, Betweenness, Radiality, Closeness, Centroid Value and Eccentricity. But the version used in this replication is able to calculate the same parameters plus EigenVector, Bridging, Edge Betweenness. To calculate all the parameters, the following setup was made:

![CentiScaPe settings](https://i.imgur.com/ZlHgEEm.png)

The process finished in around 5 minutes. It didn't take too long because of the low number of nodes and edges, even calculating all the parameters the plug-in offers. A column is added to the Node Table for each parameter calculated.

After the process has been completed, CentiScape Results panel becomes visible, showing a filter for each parameter, followed by two plot tools. 

![CentiScaPe results panel](https://i.imgur.com/gj58x5K.png)

The first plot tool manages to plot all parameter metrics for each node in the network, and the second plot tool enables to compare two parameters using a scatter plot.

# Results
> Apresente os resultados obtidos pela sua adaptação.
> Confronte os seus resultados com aqueles do artigo.
> Esta seção opcionalmente pode ser apresentada em conjunto com o método.

##  Global Topological Properties Overview

We were able to reproduce the same graphs the author created using the plot by centralities and plot by node tools. The following graph shows nodes Degree versus its Centroid value. On the left is the original graph and on the right our replica:

![](https://i.imgur.com/SffIQps.png)

It's possible to see that we achieved similar results, considering the dot shapes are random and not correspondents between both graphs. The node with the highest degree and centroid is MAPK1, whose metrics are shown below. At the top is the original graph and at the bottom is our replica:

![](https://i.imgur.com/wrTPajl.png)

We were able to replicate the same results, with the addition of more metrics due to the new version of the plug-in. These new parameters can help to understand the significance of this node.

The following graphs show centroid over centroid and degree over degree, both also replicated by us. According to the author, the distribution is not uniform: many nodes display low degree/centroid and only a few nodes with high degree/centroid, according to the scale-free architecture of biological networks.

![](https://i.imgur.com/MVRxvdg.png)

![](https://i.imgur.com/mqjho2O.png)

Finally, an analysis of PTPN1, which presents the highest degree among all the phosphatases and a rather high score for other centralities. Similar to MAPK1, we were able to replicate the same results, with the additional of more metrics.

![](https://i.imgur.com/DXyUsU9.png)

## Phosphorylation Node Weights

