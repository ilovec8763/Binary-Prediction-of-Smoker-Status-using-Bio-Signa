# Binary-Prediction-of-Smoker-Status-using-Bio-Signa

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/Zwei_zigaretten.jpg)

# Project Purpose

The task of this project is to predict the smoking status of patients using binary classification based on various health indicators. The dataset originates from a deep learning model trained on smoking status prediction using bio-signal data, and its feature distribution closely resembles that of the original dataset. However, participants are encouraged to explore potential differences between the two and assess whether incorporating the original dataset into the model can improve performance. The competition involves analyzing the provided training and testing datasets, and submissions are evaluated based on the area under the ROC curve between predicted probabilities and actual targets.

# Exploratory Data Analysis and Visualization

To facilitate the removal of duplicate and missing values in the data, as well as to gain a more intuitive understanding of the data, we can start by providing a statistical summary. Subsequently, we visualize the data between smoking and non-smoking groups for comparison.

Statistical Summary:

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/summary_gradient_style.png)

Grouped histogram of categorical features (smoking vs. non-smoking):

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/Histogram%20of%20categorical%20features%20by%20group.png)

Grouped histogram and KDE of numerical (continuous) features (smoking vs. non-smoking):

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/Histogram%20of%20numerical%20features%20by%20group.png)

# Statistical Test Results

Here, we examine the correlation between physiological features and smoking existence.

For categorical data, the chi-square independence test indicates that 'Urine protein,' 'dental caries,' 'hearing(left),' and 'hearing(right)' are all correlated with smoking.

For numerical data, as shown in the above plots, it is likely that we cannot consider the population as normally distributed. Therefore, we use the non-parametric Mann-Whitney U test to determine if the data comes from the same population:

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/U_test_pic.png)

It can be observed that we cannot reject the significant differences between smoking and non-smoking for all features.

Next, by considering smoking labels as numerical labels in an "inspirational" manner, we visualize the top 10 correlated features using a heatmap of the correlation matrix:

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/heat_map_smoker2.png)

We can see that hemoglobin, height, weight, GTP, triglycerides, creatinine, HDL, waist circumference, and age are likely the optimal factors for predicting whether a person smokes. Note that "height" itself is unlikely to change due to smoking status, suggesting that smokers are likely to be "males." However, it's important to note that this is not a rigorous approach but may provide insights when seeking important features.

# Feature Frequency Distribution of Tree-Based Model Node Splits

CatBoost feature frequency distribution for node splits:

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/cbc%20Feature%20Importance.png)

XGBoost feature frequency distribution for node splits:

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/xgbc%20Feature%20Importance.png)

LightGBM feature frequency distribution for node splits:

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/lightgbm%20Feature%20Importance.png)

# Model Comparison

From the previous section, we can see that the importance distribution of different features is different for various models. It's noteworthy that for XGBoost and CatBoost, the top five features are identical. This is significantly different from the ranking of LightGBM, so this type of feature importance metric may not be generalized to other cases, such as DNN. Interestingly, the top five results of these three models overlap with at least three features when we treat smoking status as numerical data and then create a correlation matrix.

The AUC scores of the three tree-based methods are close when the tree size is the same. If we use a voting model of the three, the score on the test set (87.068%) will be higher than the individual scores of each (around 86%). Furthermore, adding a DNN model for voting will yield a slightly lower score of 87.054% and significantly longer training time (over 10 minutes vs. 2 minutes).

The confusion matrices for the three tree-based models:
![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/voting_model_normalized_confusion(1).png)

The confusion matrix for the DNN model:
![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/dnn_normalized_confusion.png)

Comparing the two confusion matrices, we observe:

1. The DNN model has a higher True Positive Rate (TPR) (Recall or Sensitivity), making it easier to capture smoking instances and reduce cases of missed reports.

2. Tree-based models have a lower False Negative Rate (FNR), statistically making it less likely to produce type II errors and having a higher power of detection.

Reference:
Two-Independent-Samples Test Types: https://www.ibm.com/docs/zh-tw/spss-statistics/saas?topic=tests-two-independent-samples-test-types
