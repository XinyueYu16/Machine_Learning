# 相关指标

**Before Everything, Know Confusion_Matrix**：以下所有指标都通过confusion matrix的四个象限计算出来。

**Before Confusion_Matrix, Know *Hypothesis & Inference & [Type I, II Error](/.TypeError.md)*.**

<br>



**Confusion_Matrix**

模型结果（假设binary）有四种情况，TP/TN/FP/FN

TP：真实为正，预测为正

TN：真实为负，预测为负

FP：真实为负，预测为正

FN：真实为正，预测为负



<br>

**Precision**: TP/(TP + FP)，预测正结果中**precision%**为正确的

- 'how relevant is your answer?'
- 当更在意Type I Error (FP), 希望提高在有限次数里拒绝原假设（采取行动）的效率，因为行动有代价，比如零件质量检测。

**Recall**: TP/(TP + FN)，真实正结果中**recall%**为被我们预测出来的。

- 'how extensive is your answer?'
- 当更在意Type II Error (FN), 希望不要错过采取行动的机会，因为不采取行动的代价更大，比如核酸检验。
- 再例：做误差分析的时候，想要检测出导致误差超过10%的特征，可以构建以recall_score为target的tree。

> Let’s say I searched on Google for “what is precision and recall?” and in less than a minute I have about 15,600,000 results.
>
> Let’s say out of these 15.6 million results, the relevant links to my question were about 2 million. Assuming there were also about 6 million more results that were relevant but weren’t returned by Google, for such system we would say that it has a precision of 2M/15.6M and a recall of 2M/8M.
>
> [- Model Evaluation I: Precision And Recall](https://towardsdatascience.com/model-evaluation-i-precision-and-recall-166ddb257c7b)

<br>

**Accuracy**:

**F1-Score:**



Willa

2021.11.23