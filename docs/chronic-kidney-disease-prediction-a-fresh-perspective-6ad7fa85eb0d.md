# 慢性肾病预测：新视角

> 原文：[https://towardsdatascience.com/chronic-kidney-disease-prediction-a-fresh-perspective-6ad7fa85eb0d?source=collection_archive---------9-----------------------#2023-08-25](https://towardsdatascience.com/chronic-kidney-disease-prediction-a-fresh-perspective-6ad7fa85eb0d?source=collection_archive---------9-----------------------#2023-08-25)

## 利用SHAP构建一个与医学文献一致的可解释模型

[](https://medium.com/@diksha.senchaudhury?source=post_page-----6ad7fa85eb0d--------------------------------)[![Diksha Sen Chaudhury](../Images/65fb058d05306bc96ba2dd9ade94d52b.png)](https://medium.com/@diksha.senchaudhury?source=post_page-----6ad7fa85eb0d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6ad7fa85eb0d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6ad7fa85eb0d--------------------------------) [Diksha Sen Chaudhury](https://medium.com/@diksha.senchaudhury?source=post_page-----6ad7fa85eb0d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd7f3ce137d78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchronic-kidney-disease-prediction-a-fresh-perspective-6ad7fa85eb0d&user=Diksha+Sen+Chaudhury&userId=d7f3ce137d78&source=post_page-d7f3ce137d78----6ad7fa85eb0d---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6ad7fa85eb0d--------------------------------) ·8 min read·2023年8月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6ad7fa85eb0d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchronic-kidney-disease-prediction-a-fresh-perspective-6ad7fa85eb0d&user=Diksha+Sen+Chaudhury&userId=d7f3ce137d78&source=-----6ad7fa85eb0d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6ad7fa85eb0d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchronic-kidney-disease-prediction-a-fresh-perspective-6ad7fa85eb0d&source=-----6ad7fa85eb0d---------------------bookmark_footer-----------)![](../Images/9fe60a5388cc3784a0949a49f50f1983.png)

图片由 [Robina Weermeijer](https://unsplash.com/@averey?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## **引言**

肾脏努力从血液中去除任何废物、毒素和多余的液体，其正常功能对于健康至关重要。慢性肾病（CKD）是一种肾脏无法像应该那样过滤血液的状况，导致血液中液体和废物的积累，长期可能导致肾衰竭。[1] CKD影响了全球超过10%的人口，并预计到2040年将成为全球第五大生命年损失原因。[2]

在这篇文章中，我的目标不是建立一个最准确的模型来预测患者发生CKD的情况。而是检查使用标准机器学习算法开发的最佳模型是否也符合医学文献中的最有意义模型。我使用了SHAP（SHapley Additive exPlanations）的原则，这是一种博弈论方法，用于解释机器学习模型的输出。

## **医学文献怎么说？**

医学文献将CKD的发展和进展与一些关键症状相关联。

1.  **糖尿病和高血压：** 糖尿病和高血压是与CKD相关的两个最重要的风险因素。在2011-2014年在美国进行的一项研究中，糖尿病患者中CKD（3-4期）的患病率为24.5%，前糖尿病患者为14.3%，非糖尿病患者为4.9%。在同一项研究中，高血压患者的CKD患病率为35.8%，前高血压患者为14.4%，非高血压患者为10.2%。[2]

1.  **减少的血红蛋白和红细胞水平：** 肾脏产生一种叫做红细胞生成素（EPO）的激素，这种激素有助于红细胞的生成。在慢性肾病（CKD）中，肾脏无法产生足够的EPO，导致贫血的发展，即血液中的红细胞和血红蛋白水平下降。[3]

1.  **增加的血清（血液）肌酐：** 肌酐是正常肌肉和蛋白质分解的废物，过量的肌酐通过肾脏从血液中排出。在CKD中，肾脏无法有效排除过量的肌酐，导致血液中肌酐水平升高。[4]

1.  **尿液比重降低：** 尿液比重是肾脏浓缩尿液能力的指标。患有CKD的患者尿液比重降低，因为肾脏失去了有效浓缩尿液的能力。[5]

1.  **血尿和蛋白尿：** 血尿和蛋白尿分别指尿液中存在红细胞和白蛋白。正常情况下，肾脏的过滤器阻止血液和白蛋白进入尿液。然而，过滤器的损害可能导致血液（或红细胞）和白蛋白进入尿液。[6][7]

## 数据集

本文使用的数据集是Kaggle上的‘慢性肾脏疾病’数据集，最初由UCI在其机器学习库下提供。该数据集包含来自400名患者的数据，包括24个特征和1个二进制目标变量（CKD缺失 = 0，CKD存在 = 1）。特征的详细描述可以在[这里](https://github.com/dikshasen24/CKD-Prediction/blob/main/Medium_CKD_SHAP.ipynb)找到。

## 数据预处理

CKD数据集有很多缺失值，需要在进一步分析之前进行填补。此图显示了缺失数据的可视化表示，黄色线条指示了该列中的缺失值。

![](../Images/1266895e6109ef1aadc9d3bbd91803c8.png)

缺失数据的可视化表示（由黄色线条标示）

缺失值以以下方式进行了填补：

1.  对于数值特征，缺失值使用中位数填补。未使用均值，因为均值对异常值敏感，而中位数则不敏感。由于这些列中存在异常值，中位数更能准确反映中心值。

1.  分类特征‘rbc’和‘pc’分别缺失了38%和16.25% 的数据。由于这是一个较大的缺失数据量，缺失值被填补为‘未知’。在这种情况下使用众数并不是最佳选择，因为将这么大一组观察结果归为同一类别可能会有一定风险。

1.  其他所有分类特征的缺失数据都少于或等于1%。因此，使用各自的众数填补了缺失值。

## 使用SHAP构建模型并检查可解释性

在填补缺失值之后，将数据分为训练集和测试集（70–30拆分），并运行了一个简单的随机森林分类模型。测试准确率为100%，即模型能够在100%的时间里正确分类之前未见过的患者。混淆矩阵如下所示。

![](../Images/b8c36ff36a9a35739e29b0395ed0e186.png)

在测试数据上运行模型时生成的混淆矩阵

当然，我们现在有了一个很好的分类模型。但如果我们对解释性感兴趣，即每个特征如何对预测产生积极或消极的贡献呢？哪些特征是推动预测的最重要因素？结果是否与临床发现一致？这些问题是SHAP可以帮助我们回答的。

SHAP是一种基于博弈论的数学方法，可以通过计算每个特征对预测的贡献来解释任何机器学习模型的预测。它可以帮助我们确定那些对预测起关键作用的特征，以及它们对目标变量的影响方向。[8] 我们为测试数据拟合了一个SHAP解释器，并生成了如下所示的全局特征重要性图。

![](../Images/d04aa6203c56d77aca6767bd1675ea18.png)

使用SHAP生成的全局特征重要性图

驱动预测的前三个特征是血红蛋白水平（‘hemo’）、尿液的比重（‘sg’）和患者尿液中是否存在红血球（‘rbc_normal’）。由于特征重要性是通过计算该特征在所有样本中的绝对 SHAP 值的均值得出的，因此该图仅提供了重要性的顺序信息，而不涉及影响的方向。让我们生成一个更具信息性的图表，涵盖这两个目标。

![](../Images/6ee243e91e6d716555dd10cec7995e6d.png)

使用 SHAP 生成的蜜蜂图

这个蜜蜂图是展示数据集中顶级特征如何影响模型预测的绝佳方式。粉色点表示预测为 CKD 的患者，蓝色点表示预测为非 CKD 的患者。现在我们已经知道了驱动预测的顶级特征，让我们看看它们的影响方向是否与本文前面展示的临床发现一致。

1.  糖尿病（‘dm_yes’）和高血压（‘htn_yes’）的存在与 CKD 的出现有关。这与临床发现相匹配，尽管考虑到它们是与 CKD 相关的主要风险因素，预期它们在全球重要性中应位于更高的位置。

1.  低血红蛋白水平（‘hemo’）、低红细胞压积（‘pcv’：血液中红血球的体积分数）和低红血球计数（‘rc’）与 CKD 相关。这也与临床发现一致，因为 CKD 患者无法生产足够的红血球。

1.  低尿液比重（‘sg’）与 CKD 相关，这在临床上可以解释为肾脏失去浓缩尿液的能力。

1.  尿液中的高白蛋白（‘al’）和高血清肌酐（‘sc’）水平与 CKD 相关，这与临床发现一致，因为肾脏失去有效过滤血液的能力。

1.  尿液中红血球或异常尿液的存在（‘rbc_normal’；这是一个二元分类特征，其中值 = 1 表示正常尿液中没有 RBCs，值 = 0 表示异常尿液可能含有 RBCs）与 CKD 相关。这支持了临床发现，因为血尿在 CKD 患者中更为常见。

总之，顶级特征及其对预测的影响方向与医学文献一致。

## 结论

在这篇文章中，有两个主要收获：

1.  医学文献将 CKD 的发展和进展与 ML 模型用于分类的相同顶级特征相关联，以判断患者是否预测为 CKD。

1.  这些顶级特征对目标变量的影响方向支持临床发现，表明该模型不仅在预测 CKD 时 100% 准确，而且具有医学意义，结果完全可以解释。

本研究的一项可能局限性是样本量较小。一旦获得更多数据，应在更大的患者群体上测试模型，以检查其是否继续保持高精度。也有趣的是查看在更大患者群体中，特征的重要性排序是否发生变化。

在医学领域，最准确的模型可能并不总是最有意义的模型。在这项研究中，使用了 SHAP 来检查我们的模型是否符合医学文献。最终模型的优势在于它不仅具有高精度，而且易于解释，并得到临床发现的支持。该模型在远程医疗中具有很大用途，可以用于识别更高风险的慢性肾病患者。未来的研究可以深入个体观察，并查看哪些模型特征在个体层面上驱动了预测。

本项目的代码可以在 [这里](https://github.com/dikshasen24/CKD-Prediction/blob/main/Medium_CKD_SHAP.ipynb) 找到。本文中的所有图片均由我通过 Google Colab 生成。

## 参考文献

***原版数据集许可证:*** L. Rubini, P. Soundarapandian 和 P. Eswaran, [慢性肾病](https://doi.org/10.24432/C5G020) (2015), UCI 机器学习库 (CC BY 4.0)

***‘慢性肾病’ 数据集在 Kaggle 上:*** [https://www.kaggle.com/datasets/mansoordaku/ckdisease](https://www.kaggle.com/datasets/mansoordaku/ckdisease)

***原版 SHAP 文档:*** [https://shap.readthedocs.io/en/latest/api_examples.html#plots](https://shap.readthedocs.io/en/latest/api_examples.html#plots)

[1] [慢性肾病基础知识](https://www.cdc.gov/kidneydisease/basics.html) (2022), 《疾病控制与预防中心》

[2] C.P. Kovesdy, [慢性肾病流行病学：2022 更新](https://www.kisupplements.com/article/S2157-1716%2821%2900066-6/fulltext) (2022), 《肾脏国际补充期刊》

[3] H. Shaikh, M.F. Hashmi 和 N.R. Aeddula, [慢性肾病性贫血](https://www.ncbi.nlm.nih.gov/books/NBK539871/) (2023), 《国家医学图书馆》

[4] [血清（血液）肌酐](https://www.kidney.org/atoz/content/serum-blood-creatinine) (2023), 《国家肾脏基金会》

[5] J.A. Simerville, W.C. Maxted 和 J.J. Pahira, [尿液分析：综合评审](https://www.aafp.org/pubs/afp/issues/2005/0315/p1153.html?utm_medium=email&utm_source=transaction) (2005), 《美国家庭医生》

[6] P.F. Orlandi 等, [血尿作为慢性肾病和死亡进展的风险因素：来自慢性肾功能不全队列（CRIC）研究的发现](https://bmcnephrol.biomedcentral.com/articles/10.1186/s12882-018-0951-0) (2018), 《BMC 肾脏病学》

[7] [尿白蛋白](https://www.niddk.nih.gov/health-information/kidney-disease/chronic-kidney-disease-ckd/tests-diagnosis/albuminuria-albumin-urine) (2016), 美国国家糖尿病和消化与肾脏疾病研究所

[8] R. Bagheri, [SHAP 值及其在机器学习中的应用介绍](/introduction-to-shap-values-and-their-application-in-machine-learning-8003718e6827) (2022), Towards Data Science
