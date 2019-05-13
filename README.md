# DSAI_Midterm
Kaggle - [Predict Future Sales](https://www.kaggle.com/c/competitive-data-science-predict-future-sales/overview)

## 嘗試過的 model
1. [2019.4/23 Best Score(Public Kernel)](https://www.kaggle.com/dhimananubhav/feature-engineering-xgboost)
    - Public Score：0.90646
 
2. 所有 feature
    - [2019.4/23 Best Score(Public Kernel)](https://www.kaggle.com/dhimananubhav/feature-engineering-xgboost) 所產生的 40 個 feature 中的 39 個（delta_revenue_lag_1 未使用）
    - 增加的 feature（14個）：
       - item_cnt_month_lag_ / date_item_avg_item_cnt_lag_ / date_shop_avg_item_cnt_lag_ 
          - 由原本的 [1,2,3,6,12] 改為 [1,2,3,4,5,6,7,8,9,10,11,12]
    - 修改計算過程的 feature：
       - item_shop_last_sale (item_shop 的組合上一次賣出數量不為 0 距離現在的月份數)
       ```python
         cache = {}
         matrix['item_shop_last_sale'] = -1
         matrix['item_shop_last_sale'] = matrix['item_shop_last_sale'].astype(np.int8)
         for idx, row in matrix.iterrows():    
            key = str(row.item_id)+' '+str(row.shop_id)
             if key not in cache:
                 if row.item_cnt_month!=0:
                     cache[key] = row.date_block_num
             else:
                 last_date_block_num = cache[key]
                 matrix.at[idx, 'item_shop_last_sale'] = row.date_block_num - last_date_block_num
                 if row.item_cnt_month!=0: #多加入這行判斷
                     cache[key] = row.date_block_num         
       ```
       - item_last_sale (item 上一次賣出數量不為 0 距離現在的月份數)
       ```python
         cache = {}
         matrix['item_last_sale'] = -1
         matrix['item_last_sale'] = matrix['item_last_sale'].astype(np.int8)
         for idx, row in matrix.iterrows():    
             key = row.item_id
             if key not in cache:
                 if row.item_cnt_month!=0:
                     cache[key] = row.date_block_num
             else:
                 last_date_block_num = cache[key]
                 if row.date_block_num>last_date_block_num:
                     matrix.at[idx, 'item_last_sale'] = row.date_block_num - last_date_block_num
                     if row.item_cnt_month!=0: #多加入這行判斷
                         cache[key] = row.date_block_num         
       ```
 
    - no shuffle
    - n_estimators = 1000
    - 0.91849
    - feature importance
    ![](https://imgur.com/cxh1kYr.png)

3. all feature 
    - 不採用的 feature：
      - date_item_avg_item_cnt_lag_7-11
      - date_shop_avg_item_cnt_lag_7-11
      - city_code
    - no shuffle
    - n_estimators = 1000
    - 0.92212

4. all feature 
    - 不採用的 feature：
      - date_item_avg_item_cnt_lag_7-11
      - date_shop_avg_item_cnt_lag_7-11
      - city_code
      - item_shop_last_sale
      - item_last_sale
    - shuffle
    - Public Score：0.91116

5. all feature 
    - 不採用的 feature：
      - date_item_avg_item_cnt_lag_7-11
      - date_shop_avg_item_cnt_lag_7-11
      - city_code
      - item_shop_last_sale
      - item_last_sale
      - date_shop_type_avg_item_cnt_lag_1
      - date_shop_subtype_avg_item_cnt_lag_1
      - date_type_avg_item_cnt_lag_1
      - date_subtype_avg_item_cnt_lag_1
    - no shuffle
    - n_estimators = 1000
    - Public Score：0.91485

6. all feature 
    - 不採用的 feature：
      - date_item_avg_item_cnt_lag_7-11
      - date_shop_avg_item_cnt_lag_7-11
      - city_code
      - item_shop_last_sale
      - item_last_sale
      - date_shop_type_avg_item_cnt_lag_1
      - date_shop_subtype_avg_item_cnt_lag_1
      - date_type_avg_item_cnt_lag_1
      - date_subtype_avg_item_cnt_lag_1
    - data shuffle
      - train 0.8
      - valid 0.2
    - n_estimators = 1000
    - Public Score：0.88437
        - ([999] train-rmse:0.762668	validation-rmse:0.79964)
    - feature importance
    ![](https://imgur.com/jQJhWqu.png)
    

7. all feature 
    - 不採用的 feature：
      - date_item_avg_item_cnt_lag_7-11
      - date_shop_avg_item_cnt_lag_7-11
      - city_code
      - item_shop_last_sale
      - item_last_sale
      - date_shop_type_avg_item_cnt_lag_1
      - date_shop_subtype_avg_item_cnt_lag_1
      - date_type_avg_item_cnt_lag_1
      - date_subtype_avg_item_cnt_lag_1
    - data shuffle
      - train 0.8
      - valid 0.2
    - n_estimators = 3000
    - Public Score：0.88114
        - ([2179] train-rmse:0.732121	validation-rmse:0.778347)
    - feature importance
    ![](https://imgur.com/trZUV3y.png)
    

8. model6 result * 0.5 + model7 result * 0.5
    - Public Score：0.87779

9. model6 result * 0.4 + model7 result * 0.6 - **least RMSE**
    - Public Score：0.87766

10. model6 result * 0.3 + model7 result * 0.7
    - Public Score：0.87794

11. model6 result * 0.2 + model7 result * 0.8
    - Public Score：0.87861

12. model6 result * 0.1 + model7 result * 0.9
    - Public Score：0.87967
