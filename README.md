# 進階 R 資料分析與應用期中報告 - 外送時間與評分分析
本專案藉由 Kaggle 平台取得的國外外送資料，來分析外送的各種指標，探討何種因素能創造高評價及何項因素能影響外送時間。

---

## 專案簡介

- **目標**：分析外送時間及評分，找出影響因素
- **資料來源**：[Kaggle - Food Delivery Dataset](https://www.kaggle.com/datasets/gauravmalik26/food-delivery-dataset)
- **資料量**：11,400 筆資料、20 個欄位

**主要分析內容**：
1.	分析外送平台的各種指標含意。
2.	透過Anova分析及LM回歸模型，分析四種類型的道路交通密度（Jam、High、Medium、Low），藉由其信賴區間對外送時間是否具有顯著性，評斷對時間的影響程度。
3.	以多元回歸模型分析四種指標（外送距離、天氣狀況、交通路況、其他訂單）對於外送時間的影響程度。
4.	以羅吉斯回歸分析何種因素將影響外送評分高低（以評分4.8以上為高分）。

---

## 資料結構

使用到的資料欄位包含：
- `Delivery_person_Ratings`: 外送人員的評分
- `Restaurant_latitude`: 餐廳的緯度
- `Restaurant_longitude`: 餐廳的經度
- `Delivery_location_latitude`: 交付位置的緯度
- `Delivery_location_longitude`: 交付位置的經度
- `Weatherconditions`: 天氣狀況
- `Road_traffic_density`: 道路交通密度
- `Type_of_order`: 訂單類型
- `multiple_deliveries`：是否有其他訂單外送
- `Time_taken (min)`: 完成訂單所需時間（分鐘）
---

## 結果說明
### 1.	分析外送平台的各種指標含意
  - 外送評分普遍分數較高，中位數為4.7，因此我們以4.5為界區分高分與低分，以做後續羅吉斯迴歸分析。
    <img width="350" alt="image" src="https://github.com/user-attachments/assets/54f424fb-862e-45ec-86d1-0c72484fc31f" />

### 2.	透過Anova分析四種交通密度對時間的影響程度
由Anova四種道路交通密度對外送時間圖可以整理出以下結論：
- High 跟 Medium 的信賴區間重疊，對外送時間的影響不顯著。
- Low 與 Jam 沒有信賴區間重疊，因此對外送時間影響顯著。  
  <img width="400" alt="image" src="https://github.com/user-attachments/assets/a8aeb0ad-3486-49e8-be6c-856f1372a829" />

### 3.	以多元回歸、lm模型分析四種指標（天氣狀況、交通路況、外送距離、額外接單）對外送時間的影響程度
  - **天氣狀況**
    - 從外送時間很短(<20)可以發現六種天氣狀況都有峰值，所以天氣不影響。
    - 外送時間很長時(>35)可以發現六種天氣狀況也都有峰值，所以天氣不影響。
    - 從各別直方圖可以看出，Cloudy與Fog會導致時間較長，其他四種情況則直方圖偏左，會使時間較短。
    <img width="350" alt="image" src="https://github.com/user-attachments/assets/dc211f3d-aee0-4029-be4e-fd66c9771c82" /> <img width="350" alt="image" src="https://github.com/user-attachments/assets/ec5c16e2-91da-4a16-b89b-84885e64fc91" />
  - **交通路況**
    - 當路況Jam與Medium時，在時間多(>35)有峰值，理解為路況普通或是壅擠時會使時間變長。
    - 當路況為Low時，在時間少(<20)時有峰值，可以理解為路況良好會使時間明顯變短。
    - 反而路況為High時對時間影響程度不大。  
    <img width="350" alt="image" src="https://github.com/user-attachments/assets/d532f42c-6193-4bd0-9fb7-be2f7147e224" /> <img width="350" alt="image" src="https://github.com/user-attachments/assets/1d566325-316a-49a8-97f7-4a6d1e42911b" />
  - **外送距離**
    - 當外送距離很近(為1)，在時間少(<20)時有峰值，理解為距離很短會使時間明顯變短。
    - 在外送距離為中長距離(為3、4)時，在時間多(>35)時有峰值，理解為距離為中或較長距離時，會使時間變長。
    - 反而距離很長時，因資料不足導致無法斷定是否會影響時間。  
    <img width="350" src="https://github.com/user-attachments/assets/d3944c6a-b333-49b7-9dd8-86f95e3abf5b" /> <img width="350" alt="image" src="https://github.com/user-attachments/assets/bc24543e-fd8d-46e3-9d7c-8f16dc0a96a6" />
  - **額外接單**
    - 其他訂單可以忽略轉接2次以上，因數據量少。
    - 整個直方圖受0與1次轉接影響，又1次轉接佔了幾乎大量的外送數據。  
    <img width="350" alt="image" src="https://github.com/user-attachments/assets/c422c4cb-7bc9-431a-99ae-d31b441e7b0c" /> <img width="350" alt="image" src="https://github.com/user-attachments/assets/9fc157dc-5e40-44d3-b99d-793cecd26318" />

  - **模型建立（僅列出表現較佳的多元回歸模型2）**
    - 模型一：（外送距離 + 天氣狀況 + 交通路況 + 其他訂單）可以看出是否有其他訂單及路況壅擠的情況將會大大影響外送時間。
    - 模型二：（外送距離 + 天氣狀況 * 交通路況 + 其他訂單）可以看出天氣（雲/霧）及交通狀況（擁擠）對外送時間影響較大，相比模型一，這個模型的天氣狀況 x 交通路況因變數有較好的模型表現。  
      <img width="400" height="824" alt="image" src="https://github.com/user-attachments/assets/605ac741-2da0-48a9-b520-e0c43616cfcd" /> <img width="500" height="515" alt="image" src="https://github.com/user-attachments/assets/9071949a-9528-4266-8200-20e10d112994" />  
    - 模型三：（天氣狀況 * 交通路況 + 外送距離 * 其他訂單）可以看出天氣（雲/霧）及交通狀況（擁擠）對外送時間影響較大，相比模型二，外送距離 x 其他訂單較沒有顯著性。
    - 模型四：（外送距離 * 天氣狀況 * 交通路況 + 其他訂單）可以看出僅有其他訂單狀況對外送時間影響較大，相比其他模型，綜合外送距離 * 天氣狀況 * 交通路況反而使顯著性降低。
    - 模型五：（外送距離 + 交通路況 + 天氣狀況 * 其他訂單）可以看出天氣狀況 * 其他訂單有較高的顯著性，但是相比模型二並沒有更佳。

  - **5個模型驗證及比較**
    - QQ圖  
      <img width="500" alt="image" src="https://github.com/user-attachments/assets/8ad217dd-5a72-4586-aff9-d0f70210cce8" />
    - 殘插直方圖  
      <img width="500" alt="image" src="https://github.com/user-attachments/assets/140f1342-f8a4-4523-af01-afb4e754f60e" />
    - Coefficient  
      <img width="500" alt="image" src="https://github.com/user-attachments/assets/3aab1846-28a8-4c18-ae4a-1c98dc795af7" />
    - AIC/BIC  
      <img width="350" alt="image" src="https://github.com/user-attachments/assets/3a675f8f-adcd-4e82-b599-878d3b33970e" />
      <img width="350" alt="image" src="https://github.com/user-attachments/assets/af63d51b-e9dd-49b8-bb3a-db11b4cbce64" />
    - 5-fold cross validation  
      <img width="350" height="199" alt="image" src="https://github.com/user-attachments/assets/6243cc82-e72b-4f44-b12d-d984686d9391" />
    - 視覺化呈現交叉驗證、ANOVA 和 AIC 結果  
      <img width="350" height="480" alt="image" src="https://github.com/user-attachments/assets/27f7fa62-a0ac-4ca6-b711-cfda0078fe3a" />

### 4.	以羅吉斯迴歸分析何種因素將影響外送評分高低（以評分高於4.8為高分）
  - Intercept靠左且無遠於他的plot，說明此模型沒有overfitting或underfitting。
  - 可以看出天氣狀況對外送評分高低沒有關係在0右邊的點可以看出以外送食物的種類為關鍵因素，代表無論是交通狀況、天氣狀況甚至於外送時間都不會對評分有影響，真正影響評分的因素應該是食物送來時的狀況，或是這間店好不好吃，跟外送員沒有太大的關係。  
    <img width="350" height="538" alt="image" src="https://github.com/user-attachments/assets/4a9de4e9-f34c-42b4-950b-3f00c69a704c" /> <img width="350" height="538" alt="image" src="https://github.com/user-attachments/assets/186b3007-fef1-4700-bc2b-02e732d30b20" />


