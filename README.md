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

### 4/16 不同K SBERT
|     K    |  5           | 20         | 50         | 200 | 250 
| -------- | ----------   | ---------- | ---------- | --- | ---
| MSE      | 22384.2796   | 16835.2278 | 16769.6441 | 16147.3331 | 16297.4216
| RMSE     | 149.6138     | 129.7506   | 129.4977   | 127.0721 | 127.6613
| MAE      | 104.5740     | 92.2940    | 89.2450    | 86.0693 | 87.0975

### 4/22
|          | QA+dense     | summary + dense | QAre+dense
| -------- | ----------   | ---------- | ----------
| MSE      | 19523.7932   | 19729.7984 | 20281.3576
| RMSE     | 139.7276     | 140.4628   | 142.4126
| MAE      | 95.5500      | 97.1240    | 96.3640

### avg
|          | avg | 
| -------- | -------- | 
| MSE     | 16325.8171     | 
| RMSE     | 127.7725     | 
| MAE     | 89.1020     | 

## vs gpt
![image](https://github.com/flora0110/sentence_prediction/blob/main/GPT_TFIDF.png)
![image](https://github.com/flora0110/sentence_prediction/blob/main/GPT_Breeze.png)

# 0508
## prompt
- old_factors（50個）
    - 犯罪手段、損害程度、影響範圍、法律適用、涉及債權的性質不同、應訊時的自白情況不同、涉及當事人不同、被涉案人的不同身份、涉及毒品的案件類型、偽造文件的種類和使用方式、不詳人士的參與程度、偽造手法不同、犯罪手段細節不同、指紋比對的結果不同、檢測日期的不同、車輛歸屬不同、影響的範圍不同、交易手法不同、詐欺對象不同、監護人身份、前科記錄、行為主體、犯罪手段和程度、犯罪動機和意圖、涉及的被害人和金額、被告的關係不同、刑事訴訟法的不同適用、偽造的本票及聲請狀數量不同、裁定執行與分配的行為不同、偽造文書使用對象不同、取款金額、相關證據的核對與提供、案件的重要性、損害程度不同、盜竊時間和地點、盜領款項數目和頻率、文書內容的真實性、貸款金額及償還行為、虛偽信託方式、人數及組織形式、銀行職員的警覺、變造文書程度不同、公司涉及、盜領之後的處理不同、受害者的數量和身份、信用卡的數量和使用情況、未經全體繼承人同意或授權、廢棄物種類、債權關係、不動產登記事項。

- new_factors（50個）
    - 使用假造的车牌 、 指派其他成員擔任車手 、 逃避追訴 、 財產損害 、 伪造取款凭条并使用虚假文件进行金融交易 、 損害祭祀公業和派下員的利益 、 涉及公司和銀行的損害 、 網路交易詐騙 、 累犯 、 偽造文書行為 、 委託他人協助偽造 、 恐嚇和毆打他人 、 多次犯罪顯示不悔改的態度 、 實施剝奪他人行動自由的犯意 、 違反資金管理規定 、 冒用他人名義投標詐欺行為 、 購買贓物 、 變造國民身分證及特種文書 、 損害了土地登記機關的信任 、 前科 、 偽造文書的數量和程度 、 損害公文管理之正確性 、 冒充警察人員進行犯罪活動 、 是否涉及其他犯罪行為 、 侵害著作財產權 、 申請開立虛假存款帳戶 、 印信管理的正確性 、 不交付應取得的款項 、 侵占 、 盜用信用卡資料 、 緩刑 、 進入別人的住處進行竊盜，盗取個人資料和財產 、 冒充公務員行使職權 、 偽造私文書 、 暴力侵害 、 引起財富損失 、 偽冒會員身份 、 數額大 、 損害車籍管理之正確性 、 施用毒品的情節和罪行程度 、 拒絕還款 、 造成經濟損失 、 擅自修改報單內容，偽造了報關手續 、 具有法定職務權限 、 對函稿管理的正確性造成損害 、 共同犯罪 、 損害主管對函稿管理之正確性 、 加入詐欺集團並參與多項犯罪行為 、 酒後駕駛 、 欺詐他人並取得不法利益
- A prompt
    - '我會給你一篇法律判決書內容，請列出這篇判決書的{forgery_factors}。判決書內容如下：{judgement}'
- B prompt
    - '你現在是一位專業的臺灣法官，我會給你一篇法律判決書內容，請檢查是否存在隱含要件項目，提取判決書內容並以「隱含要件項目：內容」呈現，如「犯罪手段：內容」、「損害程度：內容」等。隱含要件項目如下：{forgery_factors}判決書內容如下：{judgement}'

### 組合
- old_factors with A prompt
- old_factors with B prompt
- new_factors with A prompt
- new_factors with B prompt

## Test1 50個factors + 完整judgement
- old_factors with A prompt: 4/20
- old_factors with B prompt: 6/20
- new_factors with A prompt: 4/20
- new_factors with B prompt: 0/20


## Test2 50個factors + judgement[:6500]
- old_factors with A prompt: 4/20
- old_factors with B prompt: 6/20
- new_factors with A prompt: 4/20
- new_factors with B prompt: 0/20

## Test3 25, 25個factors + judgement[:6500]
- old_factors with A prompt: 5/20
- old_factors with B prompt: 9/20
- new_factors with A prompt: 1/20
- new_factors with B prompt: 3/20
