# 交叉验证

## 目的

最大化利用数据进行训练、评估模型，从而估计模型效果、并进行调参

## 方法

### Train/Valid数据集分割方法

使用Cross-validation iterators

- **常用**：KFold, RepeatedKFold, ShuffleSplit
- **针对目标值为分类数据**：StratifiedKFold, StratifiedShuffleSplit
- **针对特征值为分类数据**：GroupKFold...（用来验证对分类的敏感性，在同一分类内的数据永远不会同时出现在一份train&valid set里）

### Time Series 数据集分割方法

因为time-series数据最明显的特征是在时间上有关联性，与CV ShuffleSplit的基本假设：数据iid相悖，因此训练数据必须都在验证数据之前出现

- Rolling：有model leakage之嫌（但是sklearn.[TimeSeriesSplit](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.TimeSeriesSplit.html#sklearn.model_selection.TimeSeriesSplit) 采用了该方法
- Blocked：如果是time-series，则时间跨度太大，可能会影响特征分布



### 对于需要在CV中调整超参的模型

- Nested: 在CV中套CV![超参](https://cdn-images-1.medium.com/max/800/1*N8PoOZP6qH6VXsX4LnS6Fg.png)

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

