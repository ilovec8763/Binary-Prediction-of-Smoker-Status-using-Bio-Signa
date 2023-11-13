# Binary-Prediction-of-Smoker-Status-using-Bio-Signa

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/Zwei_zigaretten.jpg)

# 專案目的

此專案的任務是利用二元分類來預測患者的吸煙狀態，根據各種健康指標。該數據集來自一個深度學習模型，該模型是在使用生物信號數據集進行吸煙狀態預測的基礎上訓練而成，其特徵分佈與原始數據集非常接近。然而，也鼓勵參與者探索兩者之間潛在的差異，並評估是否將原始數據集納入模型可以提升性能。比賽涉及分析提供的訓練和測試數據集，提交的作品將根據預測概率與實際目標之間的ROC曲線下面積進行評估。

# 探索性資料分析與可視化

為了方便去除數劇中的重複值及缺失值，以及對數據有更直觀的認識，我們可以簡單對資料先進行一個統計摘要，接者再對吸菸與非吸菸族群之間的數據做可視化，好方便做比較。

統計摘要:

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/summary_gradient_style.png)

類別特徵的分組histogram (吸菸與不吸菸比較):

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/Histogram%20of%20categorical%20features%20by%20group.png)

數值(連續)特徵的分組histogram 和 kde (吸菸與不吸菸比較):

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/Histogram%20of%20numerical%20features%20by%20group.png)

# 線性相關性和統計檢定的結果:
這裡檢查生理特徵與somking 存在相關性。

類別資料的部分，卡方獨立性檢驗指出'Urine protein','dental caries','hearing(left)','hearing(right)' 都跟吸菸與否存在關聯性。

數值資料的部分，由上方的作圖告訴我們很可能無法將統計母體視作常態分佈，故使用無母數 Mann-Whitney U 檢定法檢測是否來自相同的母體:

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/U_test_pic.png)

可以發現到，在所有特徵上我們都無法拒絕吸菸與否對於這些數據具有顯著差異。

接著，"啟發性"的將吸菸的標籤看作數值標籤，我們可以用關聯矩陣的heat map將前10個相關的特徵可視化為下圖 :

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/heat_map_smoker2.png)

我們可以看到血紅素、身高、體重、Gtp、三酸甘油酯、肌酸酐、HDL、腰圍、年齡有可能是預測一個人是否抽菸的最佳因素，其中"身高"本身不太可能因
為抽菸與否而改變，推測應為抽菸者多為"男性"的緣故。但需要注意的是，這並非很嚴謹的做法，但對於尋求重要特徵時可能會提供一些觀點。

# Tree-based model 節點分裂之特徵頻率分布

CatBoost 節點分裂用到的特徵的頻率分布: 

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/cbc%20Feature%20Importance.png)

XGBoost 節點分裂用到的特徵的頻率分布: 

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/xgbc%20Feature%20Importance.png)

LightGBM 節點分裂用到的特徵的頻率分布: 

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/lightgbm%20Feature%20Importance.png)

# 建模方法比較

由上一個部分可以看到不同的特徵之重要性分佈，對不同的模型來說是不一樣的。值得注意的是，對於XGBoost和CatBoost而言，排行前五名的特徵是完全相同的。這和LightGBM的排法有很大的不同，因此這類特徵重要性的度量可能無法推廣到其他場合，如:DNN。有趣的是，這三個模型前五名的結果和我們將抽菸與否視作數值資料、再做關聯矩陣的方式有大量重疊。

三種tree based 的方法的AUC分數在樹的規模相同的情形下相差不遠。若是使用三者的voting model做預測的話，test set上的分數87.068%會比三者的各自的分數(86%左右)來得高。再者，如果再加入DNN model做voting的話，卻會得到比tree-based model們略低的分數87.054%以及遠比其他方法更長的訓練時間(超過10分鐘>2分鐘)。

三種tree-based方法之間具有非常相似的混淆矩陣，所以這邊直接拿它們的voting model和DNN做比較。

tree-based model的混淆矩陣 :
![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/voting_model_normalized_confusion(1).png)

DNN的混淆矩陣 :
![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/dnn_normalized_confusion.png)

我們比較一下兩者的混淆矩陣可觀察到:

1 . DNN model 具有較高的 True Positive Rate (TPR) (Recall or Sensitivity)，較容易捕捉到smoking的情形，減少漏報案例的情況。

2. Tree-based model  具有較低的 False Negative Rate (FNR)，統計上較不易產生type II error，具有較高的檢定力。 

參考資料:
Two-Independent-Samples Test Types : https://www.ibm.com/docs/zh-tw/spss-statistics/saas?topic=tests-two-independent-samples-test-types
