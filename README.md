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
### 1.	分析外送平台的各種指標含意：
  - 外送評分普遍分數較高，中位數為4.7，因此我們以4.5為界區分高分與低分，以做後續羅吉斯迴歸分析。
    <img width="400" alt="image" src="https://github.com/user-attachments/assets/54f424fb-862e-45ec-86d1-0c72484fc31f" />
### 2.	透過Anova分析四種交通密度對時間的影響程度：

