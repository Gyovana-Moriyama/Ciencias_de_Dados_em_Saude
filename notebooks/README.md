# Notebooks

The following order should be followed for running the notebooks:

## Pre-processing
This notebook consists of all the initial pre-processing made to generate the input tables (scenario04.csv and scenario03.csv).

## EDA

This notebook consists of the exploratory data analysis of the pre-processed tables (scenario04.csv and scenario03.csv).

## Models

Here we have three notebooks: 
* Models_04_03: models trained on scenario04.csv and tested on scenario03.csv
* Models_03_04: models trained on scenario03.csv and tested on scenario04.csv
* Models_mixed: scenario04.csv and scenario03.csv are mixed and we use a train test split to get train and test datasets

These notebooks consists of the experiments and final results for the machine learning models.
