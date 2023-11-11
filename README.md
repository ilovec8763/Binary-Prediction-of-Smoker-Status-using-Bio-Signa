# Binary-Prediction-of-Smoker-Status-using-Bio-Signa

此專案的任務是利用二元分類來預測患者的吸煙狀態，根據各種健康指標。該數據集來自一個深度學習模型，該模型是在使用生物信號數據集進行吸煙狀態預測的基礎上訓練而成，其特徵分佈與原始數據集非常接近。然而，也鼓勵參與者探索兩者之間潛在的差異，並評估是否將原始數據集納入模型可以提升性能。比賽涉及分析提供的訓練和測試數據集，提交的作品將根據預測概率與實際目標之間的ROC曲線下面積進行評估。

統計摘要:

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/summary_gradient_style.png)

類別資料的分組histogram (吸菸與不吸菸比較):

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/Histogram%20of%20categorical%20features%20by%20group.png)

數值資料的分組histogram 和 kde (吸菸與不吸菸比較):

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/Histogram%20of%20numerical%20features%20by%20group.png)

CatBoost 節點分裂用到的特徵的頻率分布: 

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/cbc%20Feature%20Importance.png)

XGBoost 節點分裂用到的特徵的頻率分布: 

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/xgbc%20Feature%20Importance.png)

LightGBM 節點分裂用到的特徵的頻率分布: 

![image](https://github.com/ilovec8763/Binary-Prediction-of-Smoker-Status-using-Bio-Signa/blob/main/lightgbm%20Feature%20Importance.png)

建模方法比較 :
三種tree based 的方法的AUC分數在樹的規模相同的情形下相差不遠。若是使用三者的voting model做預測的話，test set上的分數87.068%會比三者的各自的分數(86%左右)來得高。再者，如果再加入DNN model做voting的話，卻會得到比tree-based model們略低的分數87.054%以及遠比其他方法更長的訓練時間(超過10分鐘>2分鐘)。
