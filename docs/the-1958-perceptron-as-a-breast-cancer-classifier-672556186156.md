# 1958年的感知机作为肿瘤分类器

> 原文：[https://towardsdatascience.com/the-1958-perceptron-as-a-breast-cancer-classifier-672556186156?source=collection_archive---------18-----------------------#2023-01-03](https://towardsdatascience.com/the-1958-perceptron-as-a-breast-cancer-classifier-672556186156?source=collection_archive---------18-----------------------#2023-01-03)

## 在Mathematica中实现的实际示例

[](https://medium.com/@m.emmanuel?source=post_page-----672556186156--------------------------------)[![Mario Emmanuel](../Images/e6461a139937d40d09ba6af7078b3b8e.png)](https://medium.com/@m.emmanuel?source=post_page-----672556186156--------------------------------)[](https://towardsdatascience.com/?source=post_page-----672556186156--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----672556186156--------------------------------) [Mario Emmanuel](https://medium.com/@m.emmanuel?source=post_page-----672556186156--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7728df51142e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-1958-perceptron-as-a-breast-cancer-classifier-672556186156&user=Mario+Emmanuel&userId=7728df51142e&source=post_page-7728df51142e----672556186156---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----672556186156--------------------------------) ·14分钟阅读·2023年1月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F672556186156&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-1958-perceptron-as-a-breast-cancer-classifier-672556186156&user=Mario+Emmanuel&userId=7728df51142e&source=-----672556186156---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F672556186156&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-1958-perceptron-as-a-breast-cancer-classifier-672556186156&source=-----672556186156---------------------bookmark_footer-----------)![](../Images/102748f6bc62bb86ea6dbffa91874926.png)

美国海军使用Mark I Perceptron读取字母。美国海军国家博物馆，1960年。图片来源于维基共享资源（公共领域）。

# 介绍

1958年由**弗兰克·罗森布拉特**开发的罗森布拉特感知机[1][2]，被认为是神经网络的起源，因为它是第一个展示机器从数据中学习能力的算法。

感知机是一个简单的模型，由一层人工神经元或单元组成，这些单元可以被训练来识别输入数据中的模式。这标志着人工智能领域的开始，并为更复杂的神经网络的发展铺平了道路，这些神经网络已被用于各种应用。

第一个实现是1950年代末由麻省理工学院林肯实验室开发的Mark I感知机。它是第一个能够从示例中学习的机器，用于执行模式识别任务，如读取手写信件和识别口语。Mark I感知机使用真空管和其他电子组件构建，由一系列按层结构连接的单元组成。每个单元能够处理输入数据，并调整其内部权重和偏置，以识别数据中的模式。

![](../Images/d8c9547ec9fa34d7bacbb8857ac7f3f1.png)

图1\. Mark 1的细节。来源：维基共享资源。

这台机器在1960年代被美国海军用于读取手写信件。那时，海军接收到大量的信件，需要一种快速而准确的处理信件的方法。他们转向了感知机，这种机器能够从示例中学习并识别输入数据中的模式。通过在大量手写信件数据集上训练感知机，海军能够开发出一个能够准确读取和分类信件的系统，且只需最少的人力干预。这在当时是一个重要的成就，因为它展示了人工智能和机器学习在自动化先前由人类完成的任务方面的潜力。感知机在这一应用中的成功帮助确立了这项技术作为一种强大的模式识别工具，并为其在各种应用中的使用打开了大门。

# 单神经元网络

罗斯伯拉特感知机是一个简单的人工神经元模型。在这个模型中，单个神经元有多个输入，这些输入是输入数据中特征的值，以及一个输出，这是一个二进制值，指示输入数据是否属于两个类别之一。感知机通过为每个输入分配权重来工作，权重表示该输入在确定输出中的重要性。然后，使用一个数学函数来计算输出，这个函数将加权输入与偏置项结合在一起。偏置项是一个可以调整的固定值，用来移动感知机的输出。通过根据预测中的误差调整权重和偏置，感知机可以被训练以识别输入数据中的模式。这种调整权重和偏置以最小化误差的过程称为训练感知机。

![](../Images/cb1d39036ae60dac0006ed76d1f6a917.png)

图2\. 感知器对应于一个神经元神经网络。在图像中未显示偏置，偏置必须单独添加，作为一个常数应用到一个权重中，或完全不添加。图片来自[Chrislb，维基共享资源](https://commons.wikimedia.org/wiki/File:ArtificialNeuronModel_english.png)。

该模型由一个单层单元组成，每个单元具有*d*个特征作为输入，和一个可以取值为*-1*和*+1*的二进制输出。感知器利用这些输入和输出学习如何将数据分类为两个类别。

![](../Images/bfac8c1d1aa0eaab69f8d4128677c41e.png)

图3\. 训练观测方程。

为了训练感知器，我们需要一个训练集，其中包含多个观测值，每个观测值包括d个特征（**X**向量）和实际输出（**y**）。感知器利用这些观测值来学习如何根据特征预测输出。感知器的输出通过权重向量和特征向量的点积的符号计算得出。

# 预测函数

罗斯布拉特感知器的预测函数用于根据输入数据和单位的内部权重及偏置计算感知器的输出。感知器的输出是一个二进制值，表示输入数据是否属于两个类别之一。预测函数定义如下：

![](../Images/cab0f212948c62ab66919af75f2a2807.png)

图4\. 无偏置的预测函数。

![](../Images/f07eac91f3fbf969e6c77d542ab76d36.png)

图5\. 带偏置的预测函数。

其中**W**是与输入特征对应的权重向量，**X**是特征的输入值向量。模型可以有偏置或没有偏置，其中**b**是偏置项。符号函数如果括号中的值为负数，则返回**-1**，如果为正数，则返回+1。感知器在训练过程中调整权重和偏置，以最小化预测中的误差并提高准确性。

# 偏置

罗斯布拉特感知器中的偏置项代表了应用于所有输入的额外权重。它用于移动感知器的输出，并可以在训练过程中进行调整以提高模型的准确性。偏置可以通过两种方式之一实现：作为一个常数+1添加到一个特征中，或作为一个单独调整的外部参数。

如果偏置作为常数+1特征（*特征技巧*）实现，它将像任何其他输入特征一样处理，并赋予其自己的权重。这意味着偏置项包含在输出计算中。

另外，偏置也可以作为一个外部参数来实现，与其他权重单独调整。然后将偏置添加到通过权重向量和特征向量计算的输出中。

当特征变量的均值已居中，但二元类预测的均值不为 0 时，Rosenblatt 感知器中的偏置项是有用的，因为它允许模型调整决策边界，以更好地适应数据。当二元类别分布高度不平衡时，这一点尤为重要，因为模型可能倾向于更频繁地预测多数类，以最小化错误。在这种情况下，偏置可以用来调整决策边界的位置，提升模型正确分类少数类的能力。

# 损失函数

Rosenblatt 感知器模型没有包含损失函数的正式定义，即使其目标是最小化预测值和实际值之间的误差。为了实现这一点，感知器根据预测中的误差调整模型的权重和偏置。定义感知器模型中的误差的一种方法是使用最小二乘法，即最小化预测值和实际值之间平方差的总和。这个损失函数可以用数学形式表示如下：

![](../Images/197b6c5a61a83f23cc86d87a846aa239.png)

图 6\. 感知器的损失函数。

尽管梯度下降是机器学习中最常用的损失函数最小化方法，但它不能应用于 Rosenblatt 感知器，原因在于该函数不连续。相反，感知器使用了一种称为感知器收敛定理的学习规则，该规则基于调整模型的权重和偏置以最小化预测误差的思想。

![](../Images/dea74c6d89c3cac39287de9a69823209.png)

图 7\. 相当于损失函数的梯度下降。

![](../Images/37cd374c01c82be42f8406c75a4ef80e.png)

图 8\. 优化权重向量的迭代方程。

Rosenblatt 感知器模型中的学习参数是一个超参数，它决定了学习算法的步长。它用于控制模型权重和偏置根据预测误差的更新速度。较大的学习参数会导致权重和偏置的更新幅度较大，这可能导致更快的学习，但也可能增加过拟合的风险。较小的学习参数会导致较小的更新幅度，这可能导致学习速度较慢，但也可能减少过拟合的风险。

尽管感知机模型是在引入梯度下降概念之前开发的，但通常描述为具有随机梯度下降（SGD）学习算法。这是因为感知机学习规则，即感知机收敛定理，精神上类似于梯度下降，因为它涉及迭代调整模型的权重和偏置，以最小化预测误差。与梯度下降一样，感知机使用学习参数来控制更新的步长，并且可以被视为一种在线学习形式，因为它一次处理一个训练样本。

总的来说，感知机模型中的学习参数在控制学习过程的速度和准确性方面起着至关重要的作用。通过调整学习参数，可以微调感知机模型的性能，并在各种分类任务中获得更好的结果。

# 感知机在行动中：一个乳腺癌预测器

威斯康星州乳腺癌数据集[3]是一个常用的数据集，用于展示罗森布拉特感知机模型的能力。该数据集包含 1989 年至 1992 年期间拍摄的 699 个乳腺癌活检图像样本，这些样本根据某些特征的存在被分类为良性或恶性。数据集中包括从图像中计算出的 9 个特征，包括肿块厚度、细胞大小的均匀性、细胞形状的均匀性、边缘粘附、单个上皮细胞大小、裸核、平淡的染色质、正常核仁和肿瘤的有丝分裂。

该数据集是观察感知机实际应用的一个好例子，因为它是一个相对简单的数据集，良性和恶性类别之间有明显的分隔。这意味着感知机应该能够以较高的准确度正确分类样本。此外，数据集中的特征定义明确且易于理解，这使得解释感知机模型的结果变得容易。

威斯康星州乳腺癌数据集包括以下特征：

```py
 #  Attribute                     Domain
   -- -----------------------------------------
   1\. Sample code number            id number
   2\. Clump Thickness               1 - 10
   3\. Uniformity of Cell Size       1 - 10
   4\. Uniformity of Cell Shape      1 - 10
   5\. Marginal Adhesion             1 - 10
   6\. Single Epithelial Cell Size   1 - 10
   7\. Bare Nuclei                   1 - 10
   8\. Bland Chromatin               1 - 10
   9\. Normal Nucleoli               1 - 10
  10\. Mitoses                       1 - 10
  11\. Class :                       (2 for benign, 4 for malignant)
```

在这个示例中，感知机将使用 Wolfram Mathematica 语言实现（可以很容易地适应任何其他语言）。

实现的步骤包括：

1.  定义特征。

1.  加载数据（包括清理）。

1.  将数据集划分为训练集和测试集。

1.  分配初始权重向量。

1.  训练模型（优化权重向量）。

1.  使用训练好的模型比较测试数据集。

1.  计算准确率。

1.  计算召回率。

1.  计算混淆矩阵。

## 第 1 步：定义特征

```py
features = {
   "Sample code number", 
   "Clump Thickness", 
   "Uniformity of Cell Size", 
    "Uniformity of Cell Shape",
    "Marginal Adhesion",  
   "Single Epithelial Cell Size" ,
   "Bare Nuclei",
   "Bland Chromatin",
   "Normal Nucleoli",
   "Mitoses",
   "Class"
   };
features = ToUpperCase[features];
features = StringReplace[features, " " -> "_"]

---
{"SAMPLE_CODE_NUMBER", "CLUMP_THICKNESS", "UNIFORMITY_OF_CELL_SIZE", \
"UNIFORMITY_OF_CELL_SHAPE", "MARGINAL_ADHESION", \
"SINGLE_EPITHELIAL_CELL_SIZE", "BARE_NUCLEI", "BLAND_CHROMATIN", \
"NORMAL_NUCLEOLI", "MITOSES", "CLASS"}
```

## 第 2 步：加载数据并清理

```py
SetDirectory[
  "/data/uci_breast_cancer/"];
data = Import[
   "/data/uci_breast_cancer/breast-cancer-wisconsin.data"];
data = DeleteCases[data, {___, x_ /; ! NumberQ[x], ___}];
```

## 第 3 步：将数据集划分为训练集和测试集

```py
(* OBTAIN DATA LENGTH *)
n = Length[data];

(* SET THE RATIO BETWEEN TEST AND TRAINING, TEST IS 20 PERCENT *)
testFraction = 0.2;

(* SET THE ALPHA VALUE *)
alpha = 0.9;

(* SHUFFLE THE DATA *)
randomizedData = RandomSample[data];

(* EXTRACT THE TRAINING AND TEST SETS *)
testData = Take[randomizedData, Round[testFraction*n]];
trainingData = Drop[randomizedData, Round[testFraction*n]];

(* GET THE LENGTHS OF EACH DATASET *)
lengthTestData = Length[testData]
lengthTrainingData = Length[trainingData]

---
137

546
```

## 第 4 步：分配初始权重向量

```py
W = ConstantArray[1, Length[features[[2 ;; 10]]]]

---
{1, 1, 1, 1, 1, 1, 1, 1, 1}
```

## 第 5 步：训练模型（优化权重向量）

```py
nonZeroSign[x_] := If[x > 0.0, 1.0, -1.0];
Do[
  X = trainingData[[i, 2 ;; 10]];
  Y = trainingData[[i, 11]] - 3;
  EY = nonZeroSign[Dot[X, W]];
  W = (W  + alpha (Y - EY) X);
  , {i, 1, lengthTrainingData}
  ];
W

---
{-2.6, 42.4, 6.4, 20.8, -78.2, 19., -26., 53.2, 6.4}
```

## 第 6 步：使用训练好的模型比较测试数据集

```py
results = ConstantArray[0, lengthTestData];
Do[
  X = testData[[i, 2 ;; 10]];
  results[[i]] = nonZeroSign[Dot[X, W]];
  , {i, 1, lengthTestData}
  ];
Y = testData[[All, 11]] - 3.0
EY = results

---
{-1., -1., 1., -1., -1., -1., 1., 1., -1., -1., -1., -1., -1., -1., \
-1., -1., -1., -1., -1., -1., 1., 1., -1., -1., -1., -1., 1., -1., \
1., -1., -1., -1., -1., -1., -1., -1., 1., -1., -1., -1., -1., -1., \
-1., -1., -1., -1., 1., -1., -1., 1., 1., -1., 1., 1., 1., -1., 1., \
-1., 1., -1., -1., -1., -1., 1., -1., 1., -1., -1., -1., 1., -1., \
-1., -1., 1., 1., 1., 1., 1., -1., 1., -1., 1., -1., -1., 1., 1., \
-1., -1., 1., 1., -1., -1., 1., 1., 1., -1., -1., -1., 1., -1., 1., \
-1., -1., 1., 1., 1., -1., -1., -1., -1., -1., -1., -1., -1., -1., \
-1., -1., 1., -1., 1., 1., -1., 1., -1., 1., -1., -1., 1., -1., -1., \
-1., -1., -1., -1., -1., -1., -1.}

{-1., -1., -1., -1., -1., -1., 1., 1., -1., -1., -1., -1., -1., -1., \
-1., -1., -1., -1., -1., 1., 1., 1., -1., -1., -1., -1., 1., -1., 1., \
-1., -1., -1., -1., -1., 1., 1., 1., -1., -1., -1., -1., 1., -1., 1., \
-1., 1., -1., 1., -1., -1., 1., -1., 1., 1., 1., -1., 1., -1., 1., \
1., -1., -1., -1., 1., 1., 1., 1., -1., -1., -1., -1., -1., -1., 1., \
1., 1., 1., -1., -1., 1., -1., 1., 1., -1., 1., 1., -1., 1., 1., 1., \
-1., -1., 1., 1., 1., -1., -1., 1., 1., -1., 1., -1., -1., 1., 1., \
-1., -1., -1., 1., -1., -1., 1., 1., 1., -1., -1., -1., 1., -1., 1., \
1., -1., 1., -1., 1., -1., -1., 1., -1., -1., -1., -1., -1., -1., 1., \
1., -1.}
```

## 第 7 步：计算准确率

```py
testDataHit = MapThread[Equal, {EY, Y}]
testDataHitCount = Count[testDataHit, True]
EYAccuracy = testDataHitCount/Length[Y]*1.0

---
{True, True, False, True, True, True, True, True, True, True, True, \
True, True, True, True, True, True, True, True, False, True, True, \
True, True, True, True, True, True, True, True, True, True, True, \
True, False, False, True, True, True, True, True, False, True, False, \
True, False, False, False, True, False, True, True, True, True, True, \
True, True, True, True, False, True, True, True, True, False, True, \
False, True, True, False, True, True, True, True, True, True, True, \
False, True, True, True, True, False, True, True, True, True, False, \
True, True, True, True, True, True, True, True, True, False, True, \
True, True, True, True, True, True, False, True, True, False, True, \
True, False, False, False, True, True, True, True, True, True, True, \
True, True, True, True, True, True, True, True, True, True, True, \
True, True, False, False, True}

112

0.817518
```

## 第 8 步：计算召回率

```py
recall =
 Count[
    Thread[Thread[Y == 1.0] && Thread[EY == 1.0]]
    , True]/(
    Count[
      Thread[Thread[Y == 1.0] && Thread[EY == 1.0]]
      , True]
     +
     Count[
      Thread[Thread[Y == 1.0] && Thread[EY == -1.0]]
      , True]
    )*1.0

---
0.863636
```

## 第 9 步：混淆矩阵

```py
beningPredictedBening = 
 Count[Thread[Thread[Y == -1.0] && Thread[EY == -1.0]], True]
beningPredictedMalignant = 
 Count[Thread[Thread[Y == -1.0] && Thread[EY == 1.0]], True]
malignantPredictedBening = 
 Count[Thread[Thread[Y == 1.0] && Thread[EY == -1.0]], True]
malignantPredictedMalignant = 
 Count[Thread[Thread[Y == 1.0] && Thread[EY == 1.0]], True]
MatrixPlot[
 {
  {beningPredictedBening, beningPredictedMalignant},
  {malignantPredictedBening, malignantPredictedMalignant}
  },
 ImageSize -> 300,
 ColorFunction -> "TemperatureMap",
 FrameTicks -> {
   {{1, "TUMOUR\nBENING"}, {2, "TUMOUR\nMALIGNANT"}},
   {{1, "PREDICTED\nBENING"}, {2, "PREDICTED\nMALIGNANT"}},
   {{1, ""}, {2, ""}},
   {{1, ""}, {2, ""}}
   },
 PlotLabel -> "CONFUSSION MATRIX PERCEPTRON",
 Epilog -> {
   Text[beningPredictedBening, {1/2, 3/2}],
   Text[beningPredictedMalignant, {3/2, 3/2}],
   Text[malignantPredictedBening, {1/2, 1/2}],
   Text[malignantPredictedMalignant, {3/2, 1/2}]}
 ]

---
74

19

6

38
```

![](../Images/1c94e665e64e9aac879f20cfc3a1abcb.png)

图9. 我们感知机的混淆矩阵

# 测量我们分类器的性能

测量一个感知机（或任何其他分类器）表现的一个方法是评估其在测试数据集上的表现。可以用来评估分类器表现的几个指标有准确率、精确度、召回率和F1分数。

准确率是分类器做出正确预测的百分比。它是通过将正确预测的数量除以总预测数量来计算的。然而，当类别不平衡（即一个类别比另一个类别更常见）时，准确率可能会具有误导性。

召回率是分类器正确识别的正例的百分比。它是通过将真实正例预测的数量除以总正例数量来计算的。召回率在需要最小化假阴性数量的应用中尤为重要（例如癌症检测器）。在我们的例子中，我们获得了86%的召回率，这意味着我们的预测器遗漏了14%的恶性肿瘤。

混淆矩阵是一个表格，显示了分类器做出的真实正例、真实负例、假正例和假负例的预测数量。它是理解分类器优缺点以及比较不同分类器性能的有用工具。

在癌症检测器中，混淆矩阵特别重要，因为它可以帮助识别分类器做出错误预测的情况。例如，如果分类器产生了大量的假阴性（即漏掉了很多癌症病例），可能需要调整分类器或收集更多的训练数据以提高其性能。另一方面，如果分类器产生了大量的假阳性（即将许多良性病例误判为癌症），可能需要调整分类器以使其在预测时更加保守。

# 总结

这篇文章提供了对感知机的基本数学描述，感知机是一种在1950年代开发的单层神经网络。它解释了感知机背后的数学原理，包括它们如何用于将数据分类到不同的类别中。

感知机被应用于一个在数据科学中广为人知的数据集，即1995年的*威斯康星乳腺癌数据集*，并演示了如何使用不同的指标来评估分类器的性能。感知机在Mathematica中的实现展示了如何在现代编程语言中轻松表示这些概念，实施中的不同步骤展示了如何从头设计分类器并评估其性能。

尽管感知器由于更先进的神经网络架构的出现而不再被现代机器学习实践使用，但文章显示它们仍然是理解神经网络基本原理的*有价值*工具。

# 参考文献

[1] [https://news.cornell.edu/stories/2019/09/professors-perceptron-paved-way-ai-60-years-too-soon](https://news.cornell.edu/stories/2019/09/professors-perceptron-paved-way-ai-60-years-too-soon)

[2] [https://psycnet.apa.org/record/1959-09865-001](https://psycnet.apa.org/record/1959-09865-001)

[3] [https://archive.ics.uci.edu/ml/datasets/breast+cancer+wisconsin+(diagnostic)](https://archive.ics.uci.edu/ml/datasets/breast+cancer+wisconsin+(diagnostic)) | [https://archive-beta.ics.uci.edu/dataset/15/breast+cancer+wisconsin+original](https://archive-beta.ics.uci.edu/dataset/15/breast+cancer+wisconsin+original) (CC BY 4.0 许可，见致谢)。

# 致谢

使用的数据集由UCI机器学习库提供。数据集由以下人员创建：

1\. Dr. William H. Wolberg，普通外科系。

威斯康星大学，临床科学中心

麦迪逊，WI 53792

2\. W. Nick Street，计算机科学系。

威斯康星大学，1210 West Dayton St.，麦迪逊，WI 53706

3\. Olvi L. Mangasarian，计算机科学系。

威斯康星大学，1210 West Dayton St.，麦迪逊，WI 53706

捐赠者：Nick Street。

UCI机器学习库 [http://archive.ics.uci.edu/ml](http://archive.ics.uci.edu/ml)。加州欧文：加州大学信息与计算机科学学院。 ([https://archive.ics.uci.edu/ml/about.html](https://archive.ics.uci.edu/ml/about.html) / [https://archive.ics.uci.edu/ml/citation_policy.html](https://archive.ics.uci.edu/ml/citation_policy.html))。
