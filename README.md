# sentence predict
## dataset 3/15更新
- 去除重複判決書
- 刪除20000字以上判決書

|  label | train data | test data |
| -------- | -------- | -------- |
|     size     | 500     | 100     |
|     Max Length     | 19745     | 19277     |
|     Min Length     | 328     | 280     |
|     Avg Length     | 7933.656     | 8231.62     |

## method
### SBERT_KNN.ipynb
- 去除人名地名
- 使用SBERT(distiluse-base-multilingual-cased-v2)轉成向量進行KNN
### SBERT_KNN_IDEA-CCNL/Randeng-BART-139M-SUMMARY.ipynb
- 基於SBERT_KNN.ipynb的方法上，在轉成向量前先用IDEA-CCNL/Randeng-BART-139M-SUMMARY做summerize

### Extraction-based summarization.ipynb
- 基於SBERT_KNN.ipynb的方法上，在轉成向量前先做Extraction-based summarization
- 分為四種方法
    - **SBERT** 將句子轉成向量， 取分數(和所有句子相似度分數相加)最高的 **3** 個句子
    - **SBERT** 將句子轉成向量， 取分數最高的 **5** 個句子
    - **TFIDF** 將句子轉成向量， 取分數最高的 **3** 個句子
    - **TFIDF** 將句子轉成向量， 取分數最高的 **5** 個句子
## result

## SBERT
|          | SBERT_KNN | Randeng-BART-139M-SUMMARY | SBERT, 3 |SBERT, 5 |TFIDF, 3 |TFIDF, 5 |
| -------- | -------- | -------- |-------- |-------- |-------- |-------- |
| MSE     | 18152.2428     | 19345.2496     |22504.1016|21904.2272|23639.0432|22577.1848|
| RMSE     | 134.7303     | 139.0872     |150.0137|148.0008|153.7499|150.2571|
| MAE     | 89.7740     | 103.4240     |107.5840|104.4040|102.7760|103.3440|

### 3/25 Extraction-based summarization (using Breeze for summarization with different encoding methods for KNN)
|          | SBERT        | bge-m3+dense | bge-m3+colbert 
| -------- | ----------   | ------------ | -------------
| MSE      | 22384.2796   | 21656.7064   | 20205.8244
| RMSE     | 149.6138     | 147.1622     | 142.1472
| MAE      | 104.5740     | 98.2520      | 93.4380

### 4/22
|          | QA+dense        | 
| -------- | ----------   | 
| MSE      | 19523.7932   | 
| RMSE     | 139.7276     | 
| MAE      | 95.5500     | 

### avg
|          | avg | 
| -------- | -------- | 
| MSE     | 16325.8171     | 
| RMSE     | 127.7725     | 
| MAE     | 89.1020     | 

## vs gpt
![image](https://github.com/flora0110/sentence_prediction/blob/main/GPT_TFIDF.png)
![image](https://github.com/flora0110/sentence_prediction/blob/main/GPT_Breeze.png)
