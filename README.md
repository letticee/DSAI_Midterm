# DSAI_Midterm
Kaggle - [Predict Future Sales](https://www.kaggle.com/c/competitive-data-science-predict-future-sales/overview)

嘗試過
1. [2019.4/23 Best Score(Public Kernel)](https://www.kaggle.com/dhimananubhav/feature-engineering-xgboost)
    - 0.90646
 
2. all feature
    - 增加的 feature：
       - item_cnt_month_lag_ / date_item_avg_item_cnt_lag_ / date_shop_avg_item_cnt_lag_ 
          - 由原本的 [1,2,3,6,12] 改為 [1,2,3,4,5,6,7,8,9,10,11,12]
    - 修改的 feature：
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
    - 0.91849

3. all feature 
    - 不採用的 feature：
      - date_item_avg_item_cnt_lag_7-11
      - date_shop_avg_item_cnt_lag_7-11
      - city_code
    - no shuffle
    - 0.92212

4. all feature 
    - 不採用的 feature：
      - date_item_avg_item_cnt_lag_7-11
      - date_shop_avg_item_cnt_lag_7-11
      - city_code
      - item_shop_last_sale
      - item_last_sale
    - shuffle
    - 0.91116

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
    - 0.91485

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
    - shuffle 0.2 0.8 1000itr
    - 0.88437

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
    - shuffle 0.2 0.8 3000itr
    - 0.88114

8. model6 * 5 + model7 * 5
    - 0.87779

9. model6 * 4 + model7 * 6 - least RMSE
    - 0.87766

10. model6 * 3 + model7 * 7
    - 0.87794

11. model6 * 2 + model7 * 8
    - 0.87861

12. model6 * 1 + model7 * 9
    - 0.87967
