# Project 3 - Reproducing a Scientific Paper Experiment

The general objective of the project is to reproduce an experiment (totally or partially) from a scientific paper. The topic of the article has to be related to Network Science and Health.

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

The paper proposes a Cytoscape plug-in called CentiScape. This plug-in calculates many network centrality parameters and allows users to analyze existing correlations between providen data and node centrality values calculated by the plug-in. To demonstrate the proposed plug-in the author's test the plug-in on the human-phosphatome base, a global human protein interactome dataset compiled from some public datasets. They extract a subset of this network, with only known interactions between human protein kinases and phosphatases and compute several centrality values and plots.

# Brief description of the experiment/analysis of the article that was replicated

The experiment was done using the human-phosphatome base (bioinf-2009-0193-File021.sif), extracting a subset of this network, keeping only known interactions between human protein kinases and phosphatases (the extraction was done using bioinf-2009-0193-File013.xls file). So after the filtering, the network had 549 nodes and 3844 edges.

The authors use CentiScape to calculate centrality parameters. Firstly, they make a general overview of the global topological properties, that comes from min, max and average values of all computed centralities along with the diameter and the average distance of the network. With these centralities values, they make a first ranking of human kinases and phosphatases according to their central role in the network. After, they plot degree over degree of the network, which shows that the distribution is not uniform. To confirm this suggestion, they analyze the centroid, so they plot centroid over centroid, which provided a linear distribution and as for degree, the distribution was not uniform. To extract the most relevant nodes, they used CentiScaPe to select all nodes having centrality values over the average.

Finally, the authors test the regulatory role of proteins in the kino-phosphatome network in a context-selective manner. To make this test, they focus on the analysis on human PMNs (phosphorylation data: bioinf-2009-0193-File017.NA, bioinf-2009-0193-File018.NA, bioinf-2009-0193-File019.NA). These experimental data were loaded as node attributes in Cytoscape and the computed centrality values were plotted over values of protein phosphorylation. In this experiment, each node is represented with two coordinates consisting of calculated centrality and of experimental data regarding protein phosphorylation. Considering all this, they analyze the plot of centroid values over intensity of protein phosphorylation.

## Data used as input
Dataset | Web address | Descriptive summary
----- | ----- | -----
Human kino-phosphatome network (2009) | https://academic.oup.com/bioinformatics/article/25/21/2857/227713?login=false#supplementary-data | A bipartite network of known human protein kinases and the phosphatases they interact with, as extracted from a global human protein interactome dataset complied from various public databases. 

# Method

To reproduce the article experiments we used Cytoscape 3.9.1 for Windows 64bit (https://cytoscape.org/) with the plug-in proposed in the paper, CentiScaPe 2.2 (https://apps.cytoscape.org/apps/centiscape). And to learn how to use Cytoscape and CentiScaPe, we used the following links:

- [Cytoscape: Working with attributes](http://miriamposner.com/classes/dh201w19/tutorials-guides/network-analysis/cytoscape-working-with-attributes/).
- [CentiScape User Guide](http://www.cbmc.it/~scardonig/centiscape/CentiScaPefiles/CentiScaPeUserGuide.pdf)

First, to obtain the same network used in the article, we filtered the network on (bioinf-2009-0193-File021.sif) using (bioinf-2009-0193-File013.xls) file. After the filter, we got 543 nodes and 3777 edges, 6 nodes and 67 edges less than the reported in the article.

The filtered network was imported through File > Import > Network from File. To configure the nodes and edges, the following setup was made:

![Network import settings](https://i.imgur.com/NBMhmai.png)

After importing the network, it is possible to import the Nodes Attributes through File > Import > Table from File. In order to do that, we changed the PMNs files (bioinf-2009-0193-File017.NA, bioinf-2009-0193-File018.NA, bioinf-2009-0193-File019.NA) replacing equal signal for comma and saving it as csv. At the import settings, the following configuration was used:

![Import Table settings](https://i.imgur.com/J0BKuyK.png)

After importing the three tables, Cytoscape assigns the weight for each node adding a column for each weight at the Nodes Table.

CentiScape is available in the Apps tab, after being installed through jar file or App Manager.
At the date the paper was written, CentiScaPe was able to calculate the following centrality parameters:  Average Distance, Diameter, Degree, Stress, Betweenness, Radiality, Closeness, Centroid Value and Eccentricity. But the version used in this replication is able to calculate the same parameters plus EigenVector, Bridging, Edge Betweenness. To calculate all the parameters, the following setup was made:

![CentiScaPe settings](https://i.imgur.com/ZlHgEEm.png)

The process finished in around 5 minutes. It didn't take too long because of the low number of nodes and edges, even calculating all the parameters the plug-in offers. A column is added to the Node Table for each parameter calculated.

After the process has been completed, the CentiScape Results panel becomes visible, showing a filter for each parameter, followed by two plot tools. 

![CentiScaPe results panel](https://i.imgur.com/gj58x5K.png)

The first plot tool manages to plot all parameter metrics for each node in the network, and the second plot tool enables to compare two parameters using a scatter plot.

# Results

##  Global Topological Properties Overview

![image](https://user-images.githubusercontent.com/38329077/172261948-fac17258-3848-4569-af3b-62481ddc5241.png)

First we reproduced the tables S5 (bioinf-2009-0193-File014.xls) and S6 (bioinf-2009-0193-File015.xls) as table 1 (table_1.xls) and table 2 (table_2.xls) respectively. For both tables, we got similar results for global centrality values. As for the values for each node, some centrality values like, centroid, eccentricity, stress and node degree were similar, but the others had big differences on the results. We can say that this might have happened because of the missing nodes and edges after our filter.

We were able to reproduce the same graphs the author created using the plot by centralities and plot by node tools. The following graph shows nodes Degree versus its Centroid value. On the left is the original graph and on the right our replica:

![](https://i.imgur.com/SffIQps.png)

It's possible to see that we achieved similar results, considering the dot shapes are random and not correspondents between both graphs. The node with the highest degree and centroid is MAPK1, whose metrics are shown below. At the top is the original graph and at the bottom is our replica:

![](https://i.imgur.com/wrTPajl.png)

We were able to replicate the same results, with the addition of more metrics due to the new version of the plug-in. These new parameters can help to understand the significance of this node.

The following graphs show centroid over centroid and degree over degree, both also replicated by us. According to the author, the distribution is not uniform: many nodes display low degree/centroid and only a few nodes with high degree/centroid, according to the scale-free architecture of biological networks.

![](https://i.imgur.com/MVRxvdg.png)

![](https://i.imgur.com/mqjho2O.png)

Finally, an analysis of PTPN1, which presents the highest degree among all the phosphatases and a rather high score for other centralities. Similar to MAPK1, we were able to replicate the same results, with the addition of more metrics.

![](https://i.imgur.com/DXyUsU9.png)

## Phosphorylation Node Weights

![image](https://user-images.githubusercontent.com/38329077/172261858-dd69707c-d23a-4657-8486-e5d48229882b.png)

As discussed before, we were able to import the Nodes Weights, which are visible in the Node Table by displaying the parameters filters. So for this part of the experiment we imported (bioinf-2009-0193-File020.sif) that corresponds to the Phosphorylation nodes and edges file (97 nodes and 962 interactions). 

We reproduced table S7 (bioinf-2009-0193-File016.xls) as table 3 (table_3.xls), but we were not able to plot the same graphs. For some reason, the plot tool didn't work, even with the same axis as shown in the graph.

Following, at the left, is the original scatter plot and at the right is a sample of our Node Table:

![img1](https://user-images.githubusercontent.com/38329077/172261538-77b37292-38cb-4ae7-a6fd-fb6161ede53f.png)

It's possible to match the coordinates between the plot and the table. That means that we achieved the same results as the author, but without the graph that the CentiScaPe couldn't generate.
