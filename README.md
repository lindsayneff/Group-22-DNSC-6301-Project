# Group-22-DNSC-6301-Project

# Credit Line Increase Model Card

### Basic Information

* **Person or organization developing model**: Katie McQuiddy, Lindsay Neff, Mallika Yadav, Wenxuan Xue, `kmcquiddy@gwu.edu`,`lneff22@gwu.edu`,`mallika@gwu.edu`,`wenxuan.xue@gwu.edu`
* **Model date**: August 27, 2022
* **Model version**: 1.0
* **License**: MIT
* **Model implementation code**: [https://github.com/lindsayneff/Group-22-DNSC-6301-Project/blob/main/DNSC_6301_Project.ipynb](DNSC-6301-Project.ipynb)

### Intended Use
* **Primary intended uses**: This model is an *example* probability of default classifier, with an *example* use case for determining eligibility for a credit line increase.
* **Primary intended users**: Students in GWU DNSC 6301 bootcamp.
* **Out-of-scope use cases**: Any use beyond an educational example is out-of-scope.

### Training Data

* Data dictionary: 

| Name | Modeling Role | Measurement Level| Description|
| ---- | ------------- | ---------------- | ---------- |
|**ID**| ID | int | unique row indentifier |
| **LIMIT_BAL** | input | float | amount of previously awarded credit |
| **SEX** | demographic information | int | 1 = male; 2 = female
| **RACE** | demographic information | int | 1 = hispanic; 2 = black; 3 = white; 4 = asian |
| **EDUCATION** | demographic information | int | 1 = graduate school; 2 = university; 3 = high school; 4 = others |
| **MARRIAGE** | demographic information | int | 1 = married; 2 = single; 3 = others |
| **AGE** | demographic information | int | age in years |
| **PAY_0, PAY_2 - PAY_6** | inputs | int | history of past payment; PAY_0 = the repayment status in September, 2005; PAY_2 = the repayment status in August, 2005; ...; PAY_6 = the repayment status in April, 2005. The measurement scale for the repayment status is: -1 = pay duly; 1 = payment delay for one month; 2 = payment delay for two months; ...; 8 = payment delay for eight months; 9 = payment delay for nine months and above |
| **BILL_AMT1 - BILL_AMT6** | inputs | float | amount of bill statement; BILL_AMNT1 = amount of bill statement in September, 2005; BILL_AMT2 = amount of bill statement in August, 2005; ...; BILL_AMT6 = amount of bill statement in April, 2005 |
| **PAY_AMT1 - PAY_AMT6** | inputs | float | amount of previous payment; PAY_AMT1 = amount paid in September, 2005; PAY_AMT2 = amount paid in August, 2005; ...; PAY_AMT6 = amount paid in April, 2005 |
| **DELINQ_NEXT**| target | int | whether a customer's next payment is delinquent (late), 1 = late; 0 = on-time |

* **Source of training data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **How training data was divided into training and validation data**: 50% training, 25% validation, 25% test
* **Number of rows in training and validation data**:
  * Training rows: 15,000
  * Validation rows: 7,500

### Test Data
* **Source of test data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **Number of rows in test data**: 7,500
* **State any differences in columns between training and test data**: None

### Model details
* **Columns used as inputs in the final model**: 'LIMIT_BAL',
       'PAY_0', 'PAY_2', 'PAY_3', 'PAY_4', 'PAY_5', 'PAY_6', 'BILL_AMT1',
       'BILL_AMT2', 'BILL_AMT3', 'BILL_AMT4', 'BILL_AMT5', 'BILL_AMT6',
       'PAY_AMT1', 'PAY_AMT2', 'PAY_AMT3', 'PAY_AMT4', 'PAY_AMT5', 'PAY_AMT6'
* **Column(s) used as target(s) in the final model**: 'DELINQ_NEXT'
* **Type of model**: Decision Tree 
* **Software used to implement the model**: Python, scikit-learn
* **Version of the modeling software**: 0.22.2.post1
* **Hyperparameters or other settings of your model**: 
```
DecisionTreeClassifier(ccp_alpha=0.0, class_weight=None, criterion='gini',
                       max_depth=6, max_features=None, max_leaf_nodes=None,
                       min_impurity_decrease=0.0, min_impurity_split=None,
                       min_samples_leaf=1, min_samples_split=2,
                       min_weight_fraction_leaf=0.0, presort='deprecated',
                       random_state=12345, splitter='best')`
```
### Quantitative Analysis
#### Correlation Heatmap
![image](https://user-images.githubusercontent.com/111469896/186174958-6faa4ef6-80bc-492e-b1de-5138dd9d7b87.png)

#### Iteration Plot: Final Model 
![image](https://user-images.githubusercontent.com/111469896/186176145-4f4bd87c-b0dd-4bea-b3e2-53b9f9666e5d.png)
* **Plot Description**: The above plot demonstrates the AUC lines for both training and validation data as well as the AIR curve for Hispanic-White selection. The training data approaches values close to one, which indicate that increases in model depth potentially can cause overfittin of the model using training data. This is made apparent as the validation AUC peaks and then drops off and becomes downward sloping. The AIR score for hispanic to white was selected because it initially had a low level which fell below 0.80 ratio guidelines for adverse selection. It is necessary to select a model depth with a ratio above 0.80 with possible choices made clear in the above graph.


* **Model Depth**: 6
* **Training AUC**: 0.783722
* **Validation AUC**: 0.749610
* **5-Fold SD Score**: 0.017665
* **AIR Score**: 0.833205
* **Justification for Model Selection**: The iteration plot demonstrates peak model fairness, judged by the Hispanic to White AIR curve between a tree depth of 6 or 7. In this segment, accuracy also peaks, as shown by the validation AUC curve around a depth of 6. Both of these scores indicate that a tree of depth 6 or 7 could be considered as the final model. For the purposes of this project, a final model of depth 6 was selected, prioritizing slight accuracy over fairness as the model of depth 7 demonstrated slightly higher fairness with a higher Hispanic to White AIR score of 0.835886, but a lower validation AUC score of 0.742115. 

### Ethical Considerations:
#### Potential negative impacts of using model:
* ADD MORE:
* used for credit lending and has racial bias (accounted for but still present)
* AIR just meets guidelines, not necessarily a good score if just hitting 80
* does not consider intersectional protected groups -- extremely one dimensional in only considering race as one identity group or gender alone rather than the compound impact of race, gender, sexuality, religion and other demographic factors that interact to form a person's lived experience. The failure to consider intersectional groups could result in further marginalization and has real life implications for wealth accummulation in the US.
* Math or software considerations: the cutoff used could be changed which would change lending behavior, there could also be changes in the split of training, testing, and validation. There is not an indication of how the three datasets are split so a random split might not be useful, would prefer splitting by using most recent data for testing rather than training.
#### Potential Uncertainties:
* again uncertainty with respect to how the model performs on certain identity intersections. Protected groups are only tested to one dimension
* Model data could be improved in prediction of credit delinquency. The model does not indicate whether certain factors in the economy or natural events could contribute to credit delinquency or failure to pay bills for certain year or time periods. For example, a unique effect like the global pandemic could affect ability to pay or past payment behavior. An event like this might not be indicative of overall credit history but would not be accounted for in the model. If used for lending, humans should consider external factors which might immpact behavior.
#### Any Unexpected or Results:
* Focus on a single dimension of a specific group, resulting in future credit data collection and processing results are not universal, which is not conducive to the perpetual development of this model card. 

##Ethical considerations (6 pts.):
○ Describe potential negative impacts of using your model:
The model should take into consideration credit score history of an individual and not only focus on the payment  history that for many reasons might change according to recent situations.

1.The goal of the model is to reduce bias, however that could only be achieved by thoughtful inputs when evaluating the credit line increase.Currently, model pose risk to minority group due to inherent bias in the model.

2.The model may deny credit and future financial service access to potential candidate due to current payment model.

3.The model in itself is not sufficient to draw any conclusion for credit line increase and any policy changes shall reap no benefits to the company and might lead to loose customer base in future.
