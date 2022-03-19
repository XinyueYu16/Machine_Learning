# 交叉验证

## 目的

最大化利用数据进行训练、评估模型，从而估计模型效果、并进行调参

## 方法

### Train/Valid数据集分割方法

使用Cross-validation iterators

- **常用**：KFold, RepeatedKFold, ShuffleSplit
- **针对目标值为分类数据**：StratifiedKFold, StratifiedShuffleSplit
- **针对Grouped数据**：GroupKFold...（用来验证对Group的敏感性，在同一Group内的数据永远不会同时出现在一份train&valid set里，我之前把‘Group’理解成了X特征值里的分类，这是不正确的，一种正确的例子是：假设每一份样本都是由xx病人提供的，而一个病人可能提供>1个样本，那么这些来自于同一个病人的样本就是属于一个Group）

### Time Series 数据集分割方法

因为time-series数据最明显的特征是在时间上有关联性，与CV ShuffleSplit的基本假设：数据iid相悖，因此训练数据必须都在验证数据之前出现

- Rolling：有model leakage之嫌（但是sklearn.[TimeSeriesSplit](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.TimeSeriesSplit.html#sklearn.model_selection.TimeSeriesSplit) 采用了该方法
- Blocked：如果是time-series，则时间跨度太大，可能会影响特征分布



### 对于需要在CV中调整超参的模型

- Nested: 在CV中套CV![超参](https://github.com/XinyueYu16/Machine_Learning/blob/master/Assets/nestedCV.png)

# 操作

### 注意数据预处理的顺序

- 不要先对数据集进行预处理（如normalization），再切分，不然涉嫌data leakage；但是比如MinMaxScaler像model本身一样，需要train on trainset

- 对于CV，可以把模型scaler+model串联起来，调用[sklean.cross_val_score](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_val_score.html)

  ```python
  steps = list()
  steps.append(('scaler', MinMaxScaler()))
  steps.append(('model', LogisticRegression()))
  pipeline = Pipeline(steps=steps)
  
  # define the evaluation procedure
  cv = RepeatedStratifiedKFold(n_splits=10, n_repeats=3, random_state=1)
  ```

### 对于模型在CV中的得分
- sklearn, cross_val_score：返回score，可输入特定cv
- sklearn, cross_validate: 根据指定的score string list或score dict对模型进行评分
- sklearn, *cross_val_predict*: 可以返回数据中当该数据被作为test data时的预测值，由此计算出的得分可能与cross_val_score不同，因此不能用于计算得分
  - 原因：cross_val_score是对CV中k个模型得分的平均，而cross_val_predict的结果由于无法得知哪个预测值属于哪个模型，只能计算在整体数据上的得分
  - 应用场景：绘图或者作为ensemble模型输入的结果
  ![cross_val_predict plot](https://github.com/XinyueYu16/Machine_Learning/blob/master/Assets/corss_val_predict.png)

### CV & Nested CV

```python
inner_cv = KFold(n_splits=4, shuffle=True, random_state=i)
outer_cv = KFold(n_splits=4, shuffle=True, random_state=i)

# Non_nested parameter search and scoring
clf = GridSearchCV(estimator=svm, param_grid=p_grid, cv=outer_cv)
clf.fit(X_iris, y_iris)
non_nested_scores[i] = clf.best_score_

# Nested CV with parameter optimization
clf = GridSearchCV(estimator=svm, param_grid=p_grid, cv=inner_cv)
nested_score = cross_val_score(clf, X=X_iris, y=y_iris, cv=outer_cv)
nested_scores[i] = nested_score.mean()
```



**_参考_**

- [Nested CV图](https://alexforrest.github.io/you-might-be-leaking-data-even-if-you-cross-validate.html)

- [CV的正确数据处理方法](https://machinelearningmastery.com/data-preparation-without-data-leakage/)

- [sklearn:Nested CV结果与Non-Nested对比](https://scikit-learn.org/stable/auto_examples/model_selection/plot_nested_cross_validation_iris.html)
- [⭐](https://emojipedia.org/star/)[sklearn:CV完整文档](https://scikit-learn.org/stable/modules/cross_validation.html#cross-validation)

