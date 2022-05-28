# Project 2 - Mortality Prognosis

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
 * Its severity is linked to a lot of different factors, which makes it a non-trivial problem.

We chose to look at a time window of 5 years because it is enough time to treat the underlying cause or to define that the patient belongs to a very high risk group. So the thrombosis would have a higher chance to contribute to the patient's death.

### Tools
*  Pandas
*  Numpy
*  Matplotlib
*  Seaborn
*  SMOTE
*  Scikit-learn

## Methodology

### Bases Adopted for the Study

We choose these two bases because on scenario01 and scenario02 we found a really small number of occurrences of our patients of interest.

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
| ENCOUNTERCLASS  | encounter class, as: ambulatory, emergency, inpatient, wellness, ou urgent care            |

 

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

![image](https://user-images.githubusercontent.com/38329077/168708137-d451be18-530b-431b-aa15-065f9f4a3c43.png)

#### Pre-processing

The dataset used the tables *patients*, *encounters*, *organizations*, *conditions*, *images*, *medications* and *procedures*, all merged by the patient id.
Our first processing was to convert data columns to *datetime* type, in order to facilitate operations with these features. We also replaced empty values with 0.

#### Feature Engineering

**cts:**

For this feature we focused specifically on the CT (Computed Tomography) exam. When the patient is submitted to CT we can infer that it belongs to a higher risk group. So we used the images table, and counted every different occurrence for code = 'CT' for the patient. The count can be more relevant than a boolean because it not only indicates if the patient was submitted to the exam, but how many times.

**contraceptive, anticoagulant and cnt_medications:**

These 3 features were all created using the medications table. Contraceptives and anticoagulants are medicines commonly taken by patients with thrombosis. If a patient has taken contraceptives it is more susceptible to develop thrombosis, and if a patient has thrombosis but is taking anticoagulants, it is more likely to recover from the condition as anticoagulant is a medicine commonly used to treat mild cases of thrombosis. We count how many distinct contraceptives and anticoagulants were taken by the patient, it gives more information than just if the patient has taken or not, similar to cts.

For cnt_medications we counted the amount of different medications taken by each patient, so it may indicate other health issues.

**cnt_procedures:**

Venous thrombosis may be a result of complications from a surgery. This feature uses the procedures table and counts how many surgeries the patient has undergone, the idea here is that maybe patients with a lot of surgeries are more likely to have other serious health issues.

**age:**

This feature may be the actual age of the patient or, if the patient has died, it is the age it had when it died. If the patient does not have a value for deathdate, we consider the patient is still alive, so we do *(today - birthdate)*, otherwise we do *(deathdate - birthdate)*.

**cnt_encounters:**

Patients with a big number of medical encounters can be patients with more health issues and therefore it has a greater chance of death. So we counted how many different medical encounters the patient had.

**last_encounter_dur and condition_dur:**

For these features we are interested in the treatment time during the medical encounter for each patient. The former, last_encounter_dur, we search when the last medical encounter with thrombosis has started and ended. The difference in days between the two dates is the feature we are looking for. If the encounter has not ended yet, it is the difference of days between the start of the encounter and today. The latter, condition_dur, is the difference of days between the start and the end of the condition from the last encounter. If the condition has not ended yet, it is the difference of days between the start of the condition and today.

**target:**

Our ground truth indicates if the patient died within 5 years after the end of the last encounter with the condition of thrombosis.


## Results 

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

For "all features" we mean that we are using the following features: *organization*, *age*, *cnt_encounters*, *cts*, *contraceptive*, *anticoagulant*, *cnt_medications*, *cnt_procedures*, *last_encounter_dur*, *condition_dur*, *gender_M*, *encounterclass_ambulatory*, *encounterclass_emergency*, *encounterclass_inpatient*, *encounterclass_outpatient*.

For "original features" we mean we are using the following features: *gender* and *encounterclass*.

On our exploratory data analysis, we saw that the target was imbalanced:

![image](https://user-images.githubusercontent.com/38329077/168669739-ba837c90-766b-4cf6-8cb7-708c8b73e5b1.png)

(target plot of scenario04.csv)

Therefore, we did some tests using SMOTE to do an oversample on the minority class (in this case, class 1), so we have a balanced dataset to train our models. 

### Models: 

We tried different machine learning models for each subdivision of each dataset composition, such as Random Forest, LGBM, XGBoost and Logistic Regression. We also made some baselines for each dataset composition, the baseline consists of always guessing that the patient survives.

### Results

Here we are presenting metrics results for the test.

**Scenario04 (630 patients) as train and Scenario03 (70 patients) as test:**

![04_03_new](https://user-images.githubusercontent.com/38329077/169281372-36907a37-15e1-4bf0-8179-5943e9962f71.png)

On this composition we can see that in imbalanced with all features and SMOTE with all features datasets, Random Forest, LGBM and XGBoost achieved 1.0 on all three metrics, which means the models really predicted correct all labels, we believe this happened because of the correlation of the features with target and that the models trained on a larger base compared to the test, so the models really learned how to separate the positive and negative classes, as we can see for AUC = 1.0.

For the tests with just the original features we can see that either on imbalanced and with SMOTE, the models performed poorly, we believe this happened because of the low quantity of features and that the features are not explanatory for the problem, so the models did not learn to separate the positive and negative classes, as we can see for AUC around 0.5. Also, we can see that the models do not really predict positive classes right, as we have really low values for F1 score and high values of accuracy.

Finally, for tests without 'condition_dur' feature, in the imbalanced case most of the models still perform well, but for the tests with SMOTE, half of models had a low accuracy and F1 score, so we can see that they are not predicting well for both classes, although all of them have high AUC. Therefore we can conclude that even if the models learned how to separate the classes (high AUC), they did not correctly predict both classes (low accuracy and low F1 score).

In conclusion for this composition we can see that the best option is to use all features and imbalanced data, we can also conclude that the data augmentation did not help the models to improve, probably because it didn't do a good job on creating new samples, which could happen since we didn't tune its hyper parameters.

**Scenario03 (70 patients) as train and Scenario04 (630 patients) as test:**

![03_04_new](https://user-images.githubusercontent.com/38329077/169281397-8b71e0ba-ad35-4ae5-a5a7-d41c660f300f.png)

On this composition the tests on either imbalanced and with SMOTE data and with all features were the best, this time they did not get 1.0 on any metrics, but got really close. Here the models got high values for all three metrics, so we can see that the models learned how to distinguish between the classes (high AUC) and predicted almost all correctly (high accuracy and F1 score).

For the tests with just the original features we can see that either on imbalanced and with SMOTE data, the models performed poorly. For the tests on imbalanced data, we have a high accuracy but low AUC and F1 score, so we can say that the models might be predicting the majority class (class 0), so they are did not learn how to distinguish between the classes (AUC around 0.5) and are not predicting the positive class right (low F1 score).

Finally, for the tests without 'condition_dur', we can see that most of the models performed well and both imbalanced and with SMOTE had similar results for each model except for Logistic Regression that even with a high AUC, had a low F1 score and accuracy, so we can say that this model learned to distinguish between the classes but did not predict right the classes.

In conclusion for this composition, we can see that as the train data was smaller, the models did not learn as well as the composition above. Therefore the best results were produced by the models on imbalanced data, which is similar to the composition above.

**Mixing both and doing a train-test split (630 for train and 70 for test):**

![mixed_new](https://user-images.githubusercontent.com/38329077/169281422-4d89588f-4921-48b4-9824-ad4d68cf64c5.png)

On this composition the tests on either imbalanced or with SMOTE data, with all features all models except Logistic Regression achieved 1.0 on all three metrics, which means that the these models learned how to distinguish between the classes (AUC = 1.0) and predicted both classes right for all cases (accuracy = 1.0 and F1 score = 1.0).

For the test with just the original features we can see that all the models produced the same scores on either imbalanced and with SMOTE data. For the imbalanced data, we have high accuracy, but low AUC and F1 score, which means that the models did not learn how to distinguish between the classes (AUC around 0.5) and that they are not predicting the positive class right (F1 score low or equals zero), so we can say that the models might be predicting the target as the majority class (class 0), what would explain the high accuracy and low AUC and F1 score. Considering the data with SMOTE, we have all the three metrics with low values, which indicates that the models are not predicting the majority class as the target, and did not learn to distinguish between the classes and to predict the positive class right.

Finally, for the tests without 'condition_dur', we can see that all models performed well on both imbalanced and with SMOTE data, for all three metrics. Even though the models did not have the best result for this composition, the subdivision got really satisfactory results.

In conclusion for this composition, we can see that the models performed the best with all features on both imbalanced and with SMOTE data.

#### Discussion

With the results above we can conclude that in all cases, using all features is the best option because the created features are the ones that are more correlated to the target. As for the different compositions, the composition with the best results was the one with mixed data from both tables. This might have happened because in this composition we have a bigger variety of data.

If we were to choose a model, it would be Random Forest considering that on a overall, this model performed the best on most subdivisions for all compositions, even on a reduced train size. But the composition with the best results was the one with mixed data from both tables, this might have happened because in this composition we have a bigger variety of data.

After testing the models using all features, we noticed that the feature "condition_dur" that we've created was extremely correlated to the target (0.97). Consulting the specialist we also discovered that in a real environment we probably wouldn't have the "stop_condition" value that is used to create the "condition_dur" feature.
So we've opted to test the models' performances excluding this feature as well.

It is important to highlight a disclaimer in regard to the stop_condition information. Acute deep venous thrombosis is a condition that often requires long-term anticoagulation therapy and follow-up. The criteria to consider the "stop" is unclear in the data provided (e.g.: cease anticoagulant therapy, absence of clinical signs or symptoms of thrombosis, adequate pain and edema management, etc.) and may not be applicable in other scenarios.

Here we can see the heatmap of the features correlation:

![image](https://user-images.githubusercontent.com/38329077/168676969-9f1cb909-da1c-42a7-b157-7d5e1a6b6177.png)

Therefore, it can be seen that the features with higher correlation are the ones created by us, this can explain why the tests with just original features performed so poorly.

## Limitations and Future Work

Due to a time limitation, there are many aspects of the project that we would want to develop further. 

* For the "Organization" feature, we did an Ordinal Encoder because there were 81 distinct organizations on scenario04 alone and we did not want to create one feature for each possible one (one-hot encoding). Doing an Ordinal Encoder means that all categories are contained in a single feature, represented by a numerical value. This implies for the models that there is a numerical ordination between them and some correlations that are actually not true. This is troublesome for some models, like the Logistic Regression, but we believe that not as much for the ones based on decision trees. Another reason that we did Ordinal Encoder was because we had categories for the feature "Organizations" on the test group that was never seen on the train group and doing this way was easier to bypass this situation: we gave every organization unseen before the same value, distinct from every one generated on the train group.

* The feature "Organizations" ended up being useless on the first two formats of splitting because the organizations contained in each group were mutually exclusive.

* We had the same problem with the "Encounterclass" feature, but this feature only had 4 possible categories, so we did a one-hot encoding and inputted zeros to the category that was nonexistent on the testing set.

* The "cts" feature represents how many CT scans the patient has done. Based on a specialist this is super useful, because it indicates that this patient belongs to a high risk group. But in our data we only have one patient that was submitted to this exam, so this feature didn't help us in the end.

* Doing a cross validation would help us see if our models are robust through a lot of different variations of data and remove some of that doubt that the models are only performing well for a specific group of data.

* With more time, we would like to investigate some of our results, plot the ROC curve to have a better understanding of the model's behavior. We would also want to look deeper into the model's metrics for **each** target class.

* Seeing that the data augmentation through SMOTE didn't help us at all, we are curious to see if optimizing the hyper-parameters would change the results obtained. Considering that our features are all categorical or integers, it would also be interesting to do a post processing on the data created to guarantee that the SMOTE algorithm did not generate float values.

* It would be interesting to also see if tuning the hyper-parameters for the models that we chose would give us different results.

* We would like to create a feature that indicates the number of conditions that can contribute to death, because it can indicate the severity of the patient's situation.

* It would be interesting to test scaling the data with different types of scalers, like Min Max Scaler and Standard Scaler and compare the results that we already have, because some of our features have different ranges.


## Conclusion

One of the biggest learnings during this project was the importance of consulting a specialist in order to understand our data better. 
The first assistance was the directions in which condition to choose. By choosing acute deep venous thrombosis (disorder) we were able to work with a reasonable amount of data, since it is a relatively common condition; also, our data had age and gender balanced, since it affects both men and women of all ages. These characteristics are highly desirable during data science work. The specialist also was really helpful during feature engineering. It played a big part by helping us understand how data should behave on patients with this condition, so we were able to create features that could bring out these specificities. It was noticeable that the features we created were the features with the greatest correlations to the target. Understanding which feature to create and use, also helped us to choose which table to use. Tables that are not important to the issue could bring uncertainty and noise to the models.

Regarding train and test steps, we could observe the importance of our features to metric values. Models tested only with original features, balanced and imbalanced, had the weakest results for all batches of each dataset composition. This is also noticeable when we trained using the smaller dataset (Scenario03) and tested using the bigger dataset (Scenario04); even with less data, the model metrics were pretty good. That may indicate that the features were explanatory enough, so the models could learn well even with less data. Lastly, our greatest results came from mixing both datasets, which brought a bigger variety of data for training, and also a higher equity between the train and test sets. We found Random Forest as the best model because this model performed the best on most subdivisions for all compositions.

Finally, another thing to consider was the quality of the data. Since the dataset had data from a large range of conditions and was synthetically created, it may have features that are not important or even are absent from certain conditions, while these features may be crucial for a distinct set of conditions. This led us to a problem with the condition_dur feature; compared to the other features, it had the highest correlation with the target, but it was created using a feature that is not present for the disease in real life, stop_condition. This feature helped us to achieve the best metrics, but we decided to train the model without the feature as well, so it could show a prediction more relatable to real life.
