# 相关指标

**Before Everything, Know Confusion_Matrix**：以下所有指标都通过confusion matrix的四个象限计算出来。

**Before Confusion_Matrix, Know *Hypothesis & Inference & [Type I, II Error](/.TypeError.md)*.**

*注：本文中Pos，Neg指真实样本中的True和False*

<br>



## **Confusion Matrix**

模型结果（假设binary）有四种情况，TP/TN/FP/FN

TP：真实为正，预测为正

TN：真实为负，预测为负

FP：真实为负，预测为正

FN：真实为正，预测为负



<br>

## 模型结果评价（在决定threshold后）

### **Precision**: TP/(TP + FP)，预测正结果中**precision%**为正确的

- 'how relevant is your answer?'
- 当更在意Type I Error (FP), 希望提高在有限次数里拒绝原假设（采取行动）的效率，因为行动有代价，比如零件质量检测。

### **Recall**: TP/(TP + FN)，真实正结果中**recall%**为被我们预测出来的。

- 'how extensive is your answer?'
- 当更在意Type II Error (FN), 希望不要错过采取行动的机会，因为不采取行动的代价更大，比如核酸检验。
- 再例：做误差分析的时候，想要检测出导致误差超过10%的特征，可以构建以recall_score为target的tree。

> Let’s say I searched on Google for “what is precision and recall?” and in less than a minute I have about 15,600,000 results.
>
> Let’s say out of these 15.6 million results, the relevant links to my question were about 2 million. Assuming there were also about 6 million more results that were relevant but weren’t returned by Google, for such system we would say that it has a precision of 2M/15.6M and a recall of 2M/8M.
>
> [- Model Evaluation I: Precision And Recall](https://towardsdatascience.com/model-evaluation-i-precision-and-recall-166ddb257c7b)

<br>



### **Accuracy**: 总体上预测准确的数量：(TP + TF)/N(samples)

- 当样本extremely unbalanced时候，Accuracy == bullshit，因为在那种情况下全部预测为0或为1，Precision或Recall就已经很高，但是无法有效预测小样本类别

**Fβ-Score:** (1 + β²)Precision * Recall / β² Precision + Recall，当面对inbalanced data时更有效

- 当β = 1时，F1-Score为Recall和Precision的Harmonic Mean

![HarmonicMean](../assets/Harmonic_mean_3D_plot_from_0_to_100.png)

- 当β <> 1时，Precision 和 Recall 对该指标的重要性不同：
  - 当β < 1时，更重视Precision，更想降低 False Positive
  - 当β > 1时，更重视Recall，更想降低 False Negative



<br>

## 模型本身评价（考虑多种threshold的情况）

可以通过下列指标对不同模型进行评价（如不同特征的Logistic Regression, Logistic和Tree等），主要考察的是在不同threshold下模型的整体表现（roustness），但在决定模型后依然需要谨慎处理threshold。

### ROC: True Positive Rate vs False Positive Rate

- 学名：Receiver Operating Characteristics

- TPR = TP/(TP + FN) = Recall —— 所有真实Pos中，被识别的比例

- FPR = FP/(TN + FP)  —— 所有真实Neg中，未被识别的比例

  ROC如下图：

  T为Threshold，当识别为Pos的proba >= T时，将样本标记为Pos，图中T=0时，Recall为1，因为将所有样本识别为Pos，Recall === 1

  - 最佳表现：T 逼近 1（左下角）时，Recall = 1，说明classifier对于分辨Pos和Neg非常敏感**（AUC=1）**

  ![ROC](../assets/ROC_1.png)

### ROC-AUC: Area Under Curve

- **最佳表现：AUC = 1**

- ROC-AUC作为指标的优点：

  - ROC图难以在多个模型间进行比较

  - > AUCROC can be interpreted as the probability that the scores given by a classifier will rank a randomly chosen positive instance higher than a randomly chosen negative one.
    >
    > — Page 54, [Learning from Imbalanced Data Sets](https://amzn.to/307Xlva), 2018.

- 缺点：对于样本量极少的imbalanced data（尤指Pos）情况容易高估模型表现，此时FPR表现持续偏好（*tabun*）

  > For imbalanced classification with a severe skew and few examples of the minority class, the ROC AUC can be misleading. This is because **a small number of correct or incorrect predictions can result in a large change in the ROC Curve or ROC AUC score.**
  >
  > > Although ROC graphs are widely used to evaluate classifiers under presence of class imbalance, it has a drawback: under class rarity, that is, when the problem of class imbalance is associated to the presence of a low sample size of minority instances, as the estimates can be unreliable.
  > >
  > > — Page 55, [Learning from Imbalanced Data Sets](https://amzn.to/307Xlva), 2018.
  >
  > — https://machinelearningmastery.com/roc-curves-and-precision-recall-curves-for-imbalanced-classification/

- 替代方案：Precision-Recall - AUC



### PR-AUC: Perfect for imbalanced data

- **PR仅关注minority class(Positive)**，而ROC关注了Pos和Neg（Minority & Majority）

  > Both the precision and the recall are **focused on the positive class (the minority class)** and are **unconcerned with the true negatives (majority class).** 

- PR与ROC方向相反，但AUC最佳表现依然为1

  ![PR](../assets/Precision-Recall-Curve-of-a-Logistic-Regression-Model-and-a-No-Skill-Classifier2.png)

- [在极端imbalance情况下对比PR与ROC](https://machinelearningmastery.com/roc-curves-and-precision-recall-curves-for-imbalanced-classification/): 参考最后一部分

<br>

Willa	2021.11.23

Updated: 

- 2021.11.23 -- Precision, Recall
- 2021.12.1 -- accuracy, Fβ-Score, ROC, PR
