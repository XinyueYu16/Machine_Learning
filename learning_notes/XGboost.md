# XGBoost



**标签**：Ensemble/集成模型、Boosting、基于误差的弱学习器集成模型，结果为**多个弱学习器结果之和**

**应用**：

**风险**：XGboost可以把inS 准确率拉满，但是泛化能力需要人工干预/CVS验证

**参数选择**：太多了，desu

**模型优化目标**：

**过程**：

**常用包**：

- ##### **xgboost**

  - **xgb**: 设置DMatrix，设置params = {如'binary:logistic'}，xgb.train(param, dtrain, num_round)

  - **sklearn API**: xgb.XGBRegressor or XGBClassificator

    - 回归树（xgboost更擅长）：XGBRegressor

    - 分类树：XGBClassificator，如果样本不均衡，需要设定**scale_pos_weight**(0:1的倍数)

  - 两种模式参数及回归结果不一样，详见案例 [classify unbalanced data](#classify unbalanced data)

**案例：**

1. ##### classify unbalanced data:

   - ###### params

     - sklearn API & xgb: 设定**scale_pos_weight**(0:1的倍数)
     - xgb: {'objective':  'binary:logistic', 'scale_pos_weight': 10}

   - ###### res

     - sklearn: 返回结果(0或1)

     - xgb: 返回概率，需要手动设定阈值

       ```python
       preds = bst.predict(dtest)
       ypred = preds.copy()
       ypred[preds > 0.5] = 1
       ypred[ypred != 1] = 0
       
       # 在设定好了scale_pos_weight后，阈值设置为0.5就ok，在grid_search的时候可以固定scale_pos_weight
       # 同样，也可以只调整阈值
       ```

       

**参考**：

- 菜菜的sklearn机器学习



Willa

2021.11.25