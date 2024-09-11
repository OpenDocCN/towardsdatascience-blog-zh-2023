# 临床试验结果预测

> 原文：[https://towardsdatascience.com/clinical-trial-outcome-prediction-7ce6c27831f9?source=collection_archive---------9-----------------------#2023-10-04](https://towardsdatascience.com/clinical-trial-outcome-prediction-7ce6c27831f9?source=collection_archive---------9-----------------------#2023-10-04)

## 第2部分：使用XGBoost预测临床试验结果

[](https://medium.com/@lenlan?source=post_page-----7ce6c27831f9--------------------------------)[![Lennart Langouche](../Images/ca35c7112cb845d17e1148b4f282600e.png)](https://medium.com/@lenlan?source=post_page-----7ce6c27831f9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7ce6c27831f9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7ce6c27831f9--------------------------------) [Lennart Langouche](https://medium.com/@lenlan?source=post_page-----7ce6c27831f9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4bee88ffae4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclinical-trial-outcome-prediction-7ce6c27831f9&user=Lennart+Langouche&userId=4bee88ffae4&source=post_page-4bee88ffae4----7ce6c27831f9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7ce6c27831f9--------------------------------) · 5分钟阅读 · 2023年10月4日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7ce6c27831f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclinical-trial-outcome-prediction-7ce6c27831f9&user=Lennart+Langouche&userId=4bee88ffae4&source=-----7ce6c27831f9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7ce6c27831f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclinical-trial-outcome-prediction-7ce6c27831f9&source=-----7ce6c27831f9---------------------bookmark_footer-----------)

在[本系列的第一部分](https://medium.com/@lennart.langouche/clinical-trial-outcome-prediction-a4c6d279fd42)中，我重点讨论了如何嵌入从[ClinicalTrials.gov](http://clinicaltrials.gov)获取的多模态现实世界数据。在这篇文章中，我将实现一个基本的XGBoost模型，用我们在[第1部分](https://medium.com/@lennart.langouche/clinical-trial-outcome-prediction-a4c6d279fd42)中创建的嵌入进行训练，并将其性能与HINT模型（一个分层图神经网络）的性能进行比较，HINT模型是本项目的灵感来源。

![](../Images/3562ccfd368279055d7b53f61345d436.png)

工作流程示意图（图像由作者提供）

这是我在本文中将遵循的步骤：

+   加载训练、验证和测试数据集

+   嵌入*药物分子、纳入/排除标准、疾病指示、试验赞助商*和*参与者人数*

+   定义评估指标

+   训练XGBoost模型并简要比较与HINT模型性能

![](../Images/f95a6a52f07b976351970569dbb08655.png)

本系列第2部分的重点：基于在[第1部分](https://medium.com/@lennart.langouche/clinical-trial-outcome-prediction-a4c6d279fd42)中创建的特征嵌入预测临床试验结果（图片由作者提供）

你可以按照这个Jupyter notebook中的所有步骤操作：[临床试验嵌入教程](https://github.com/lenlan/clinical-trial-prediction/blob/main/predict_clinical_trial_outcome_using_XGBoost.ipynb)。

# 加载训练、验证和测试数据集

```py
import os
import pandas as pd
import numpy as np
import pickle

# Import toy dataset
toy_df = pd.read_pickle('data/toy_df_full.pkl')

train_df = toy_df[toy_df['split'] == 'train']
val_df = toy_df[toy_df['split'] == 'valid']
test_df = toy_df[toy_df['split'] == 'test']

y_train = train_df['label']
y_val = val_df['label']
y_test = test_df['label']

print(train_df.shape, val_df.shape, test_df.shape)
print(y_train.shape, y_val.shape, y_test.shape)

### Output:
# (1028, 14) (146, 14) (295, 14)
# (1028,) (146,) (295,)
```

# 嵌入药物分子、方案、指示和试验赞助商

在本节中，我们加载了在[第1部分](https://medium.com/@lennart.langouche/clinical-trial-outcome-prediction-a4c6d279fd42)中创建的字典，并使用它们将训练、验证和测试集中的值映射到相应的嵌入中。

```py
def embed_all(df):
    print('input shape: ', df.shape)
    ### EMBEDDING MOLECULES ###
    print('embedding drug molecules..')
    nctid2molecule_embedding_dict = load_nctid2molecule_embedding_dict()
    h_m = np.stack(df['nctid'].map(nctid2molecule_embedding_dict)) 
    print(f"drug molecules successfully embedded into {h_m.shape} dimensions")
    ### EMBEDDING PROTOCOLS ###
    print('embedding protocols..')
    nctid2protocol_embedding_dict = load_nctid2protocol_embedding_dict()
    h_p = np.stack(df['nctid'].map(nctid2protocol_embedding_dict))
    print(f"protocols successfully embedded into {h_p.shape} dimensions")
    ### EMBEDDING DISEASE INDICATIONS ###
    print('embedding disease indications..')
    nctid2disease_embedding_dict = load_nctid2disease_embedding_dict()
    h_d = np.stack(df['nctid'].map(nctid2disease_embedding_dict))
    print(f"disease indications successfully embedded into {h_d.shape} dimensions")
    ### EMBEDDING TRIAL SPONSORS ###
    print('embedding sponsors..')
    sponsor2embedding_dict = load_sponsor2embedding_dict()
    h_s = np.stack(df['lead_sponsor'].map(sponsor2embedding_dict))
    print(f"sponsors successfully embedded into {h_s.shape} dimensions")
    ### EMBEDDING ENROLLMENT ###
    print('normalizing enrollment numbers..')
    enrollment = pd.to_numeric(df['enrollment'] , errors='coerce')
    if enrollment.isna().sum() != 0:
        print(f"filling {enrollment.isna().sum()} NaNs with median value")
        enrollment.fillna(int(enrollment.median()), inplace=True)
        print(f"succesfully filled NaNs with median value: {enrollment.isna().sum()} NaNs left")
    enrollment = enrollment.astype(int)
    h_e = np.array((enrollment - enrollment.mean())/enrollment.std()).reshape(len(df),-1)
    print(f"enrollment successfully embedded into {h_e.shape} dimensions")
    ### COMBINE ALL EMBEDDINGS ###
    embedded_df = pd.DataFrame(data=np.column_stack((h_m, h_p, h_d, h_s, h_e)))
    print('output shape: ', embedded_df.shape)
    return embedded_df

# Embed data
X_train = embed_all(train_df)
X_val = embed_all(val_df)
X_test = embed_all(test_df)
```

# 定义评估指标

我们将使用与[HINT文章](https://doi.org/10.1016/j.patter.2022.100445)中提出的相同的评估指标：ROC AUC、F1、PR-AUC、精确度、召回率和准确率。

# 训练XGBoost模型，并预测训练、验证和测试标签

```py
import xgboost as xgb
# Create an XGBoost classifier with specified hyperparameters
xgb_classifier = xgb.XGBClassifier(
    learning_rate=0.1,
    max_depth=3,
    n_estimators=200,
    objective='binary:logistic',  # for binary classification
    random_state=42
)

# Train the XGBoost model
xgb_classifier.fit(X_train, y_train)
# Make predictions
y_train_pred = xgb_classifier.predict(X_train)
y_val_pred = xgb_classifier.predict(X_val)
y_test_pred = xgb_classifier.predict(X_test)
print('-----------Results on training data:-----------')
print_results(y_train_pred, y_train)
print('-----------Results on validation data:-----------')
print_results(y_val_pred, y_val)
print('-----------Results on test data:-----------')
print_results(y_test_pred, y_test)

### Output:
#-----------Results on training data:-----------
# ROC AUC: 1.0
# F1: 1.0
# PR-AUC: 1.0
# Precision: 1.0
# recall: 1.0
# accuracy: 1.0
# predict 1 ratio: 0.661
# label 1 ratio: 0.661
# -----------Results on validation data:-----------
# ROC AUC: 0.765
# F1: 0.817
# PR-AUC: 0.799
# Precision: 0.840
# recall: 0.795
# accuracy: 0.773
# predict 1 ratio: 0.602
# label 1 ratio: 0.636
# -----------Results on test data:-----------
# ROC AUC: 0.742
# F1: 0.805
# PR-AUC: 0.757
# Precision: 0.790
# recall: 0.821
# accuracy: 0.759
# predict 1 ratio: 0.630
# label 1 ratio: 0.606
```

# 与HINT模型比较性能

这个简单的XGBoost模型在*药物分子、纳入/排除标准、疾病指示、试验赞助商*和*参与者人数*的特征嵌入上进行了训练，而HINT作者没有使用最后两个特征：*试验赞助商*和*参与者人数*。我们使用了几个大型语言模型嵌入工具，如BioBERT和SBERT，并采用了Morgan编码进行药物表示，而HINT作者使用了多种神经网络进行所有的嵌入。

从下面的图中可以看到，我们的特征嵌入在简单的XGBoost模型下的表现相比于更复杂的HINT模型相当好。我们的项目在这个数据集上的精确度和准确性更高，但召回率较低。

![](../Images/71d338ef8e870b08ccb41d018045cbe7.png)

本项目性能与HINT项目的比较（图片由作者提供）

# 结论

下一步可能包括分析以确定添加特征*试验赞助商*和*参与者人数*在提高性能（在某些指标上）方面的贡献程度，相较于其他因素，如模型选择和嵌入技术。直观上，这些特征似乎可以提高预测性能，因为某些赞助商的历史表现优于其他人，而且也可以预期试验规模与结果之间存在一定的关系。

现在你可能会想：“这样的预测模型有什么用处？我们不能仅凭这种模型而放弃进行试验吗？”你的想法是正确的（尽管一些公司正在创建患者的数字双胞胎，以期实现虚拟试验）。例如，本系列中展示的模型可以用于改进临床试验的效能分析，这是一种相关的统计实践。效能分析用于确定特定试验中招募参与者的最佳数量，并且需要对治疗效果做出强假设才能进行这种分析。利用试验信息如药物分子结构、疾病指征和试验资格标准的预测模型（例如我们在此实现的模型）可以有助于创建更准确的效能分析。

# 参考文献

+   Fu, Tianfan, 等. “提示：用于临床试验结果预测的层级交互网络。” *Patterns* 3.4 (2022).
