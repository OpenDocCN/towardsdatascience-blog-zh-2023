# 数据科学中的认知偏见：类别规模偏见

> 原文：[`towardsdatascience.com/cognitive-biases-in-data-science-the-category-size-bias-8dbd851608c3`](https://towardsdatascience.com/cognitive-biases-in-data-science-the-category-size-bias-8dbd851608c3)

## [数据偏见黑客](https://towardsdatascience.com/tagged/data-cognitive-bias)

## 数据科学家的偏见破解指南

[](https://medium.com/@MahamsMultiverse?source=post_page-----8dbd851608c3--------------------------------)![Maham Haroon](https://medium.com/@MahamsMultiverse?source=post_page-----8dbd851608c3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8dbd851608c3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8dbd851608c3--------------------------------) [Maham Haroon](https://medium.com/@MahamsMultiverse?source=post_page-----8dbd851608c3--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8dbd851608c3--------------------------------) ·阅读时间 8 分钟·2023 年 11 月 29 日

--

![](img/eee2dc275bfeecb4dd134047e6cab03f.png)

[Andy Li](https://unsplash.com/@andylid0?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 拍摄的照片，来源于 [Unsplash](https://unsplash.com/photos/man-in-white-dress-shirt-standing-in-front-of-brown-wooden-shelf-RndRFJ1v1kk?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

想象一下，你发现自己置身于一个风景如画的街区，那里有两家面包店。第一家是一家小型家族经营的面包店，温馨地坐落在街角。第二家则是一座宏伟的三层大楼，招牌上展示了其丰富的选择和先进的烤箱。

当你开始寻找完美的面包时，你被那家高大的面包店所吸引。建筑的巨大和宏伟给人留下了深刻的印象，使你假设较大的面包店一定生产出最好的面包。

在这个情境中，你无意中屈服于一种被称为类别规模偏见的心理倾向。这种偏见让你相信，较大的面包店更可能提供优质的面包。

实际上，面包店的规模与其面包的质量并不一定相关。较小的家族经营的面包店可能拥有一份代代相传的秘密配方，而较大的面包店则可能更注重数量而非手工艺。

这种偏见反映了我们将更大类别与更好结果联系起来的倾向，即使这些类别中的具体特征可能与我们的假设不符。这种现象被称为类别规模偏见。

> 类别大小偏差指的是我们倾向于将结果视为更可能发生的情况，当它们属于较大类别时，而不是较小类别，即使每个结果的可能性是相等的。

[尽管这种偏差基于经过实验验证的研究，但对证据的解释仍然存在持续的变化。](https://thedecisionlab.com/biases/category-size-bias)

在数据科学领域，类别大小偏差可能通过特定假设表现出来。例如：

## 假设 1：较大、更复杂的模型总是比较小的模型提供更好的预测。

在类别大小偏差的背景下，通常认为神经网络或机器学习模型的性能随着其规模或复杂性而提高。因此，无论数据或任务是否符合模型的特性，人们往往会关注复杂模型。这种倾向类似于[从众效应](https://thedecisionlab.com/biases/bandwagon-effect)，即认为更新、更复杂和更知名的算法是最前沿的，即使它们并不适合某些任务。例如，考虑使用大型语言模型（LLMs）来处理相对简单的任务。

例如，向神经网络中添加额外的层，即使它们实际上并没有使模型变得更好，因为大多数任务只需两层就能很好地处理。

![](img/714b2e09674794c09c497f79cdd11841.png)

图片来源于作者

**警告：** 需要注意的是，大型、更复杂的模型往往能够提供更好的预测，这在许多情况下是成立的，特别是在处理需要高度精确度或涉及庞大且多样化数据集的复杂任务时。

**权衡：** 在这种情况下的权衡包括性能和资源消耗。尽管复杂或更大的模型需要更多的资源，但增加的资源消耗并不会自动转化为更好的模型性能。尽管这在所有情况下可能不是必要的或有影响的，但在关键问题中，考虑这些因素确实可能会带来显著的差异。

## 假设 2：仅依赖较高的准确率忽视类别不平衡

这个问题是一个被广泛认可的问题。在使用诸如准确率之类的指标时，更容易忽视潜在的偏差。举例来说，考虑一个包含 1000 名患者的数据库，其中 5 名患者有某种疾病。如果一个模型通过始终将几乎所有实例都分类为阴性（因为大多数实例是阴性的）来实现 99.5%的准确率，那么可能会认为这个模型表现良好。然而，实际上，这个模型的表现是不足的，因为它没有将任何实例分类为阳性。

进一步说明，我们来看一个基本的例子。我们将生成 100 个 0 到 1 之间的随机数。如果一个数字超过 0.92，我们称其为正类；否则为负类。我们将使用逻辑回归作为我们的模型。下面的代码片段演示了这种情况：

```py
import numpy as np
import matplotlib.pyplot as plt
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, roc_auc_score

# Generate synthetic data
np.random.seed(42)
thresh = 0.92
X = np.random.uniform(0, 1, 100).reshape(-1, 1)
y = (X > thresh).astype(int).ravel()

model = LogisticRegression()
model.fit(X, y)

y_pred = model.predict(X)
y_prob = model.predict_proba(X)[:, 1]

#Calulcate performance metrics
accuracy = accuracy_score(y, y_pred)
precision, recall, _ = precision_recall_curve(y, y_prob)
average_precision = average_precision_score(y, y_prob)
f1 = f1_score(y, y_pred)
```

尽管模型达到令人满意的准确率 0.92，但检查其他指标如召回率和 f1-score 却显示性能远远不如。在左侧绘制的图表中，明显没有任何正实例被正确分类为正类。

![](img/3327d6d5722a6ce1b4638fdb362ea840.png)

作者提供的图像

**警告：** 尽管准确性通常是一个很好的衡量标准，但在某些情况下，优先考虑更高的准确性是合理的。在类不平衡最小且误分类成本相对较低的情况下，优先考虑准确性可能是一个合理的选择。

**权衡：** 在这种情况下的权衡集中在性能本身，涉及到相当大的风险。然而，在轻微不平衡或数据已处理的情况下，这种担忧可能不那么重要。

## 假设 3：将更大的数据集与性能改进等同起来

虽然较大的数据集通常带来更多特征、附加信息和更高的现实预测概率，但这种情况只在一定程度上成立。所需的数据量取决于具体问题。例如，在分类任务中，类较少或有很多有用特征的情况可能适用于较小的数据集。同样，涉及低阶函数近似的任务可能对较小的数据集感到满意。

让我们通过一个例子来理解这个问题。首先，我们生成虚假的数据，其中一半是有用的，包含 5 个类别，大约 20,000 个样本。

```py
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.svm import SVC
from sklearn.datasets import make_classification
from sklearn.metrics import accuracy_score

# Generate a synthetic dataset
samples = 20000
X, y = make_classification(n_samples=samples, n_features=10, n_informative=5, n_clusters_per_class=5, random_state=42)

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
indices = np.arange(X_train.shape[0])
np.random.shuffle(indices) 
```

随后，我们使用支持向量分类（SVC）对数据子集进行类别分离，特别是 9,000 个实例：

```py
count_small = 9000
X_train_small = X_train[:count_small]
y_train_small = y_train[:count_small]
start_time = time.time()

model_small = SVC(probability =True)
model_small.fit(X_train_small, y_train_small)  # Using only a small subset for training

y_pred_small = model_small.predict(X_test)
accuracy_small = accuracy_score(y_test, y_pred_small) 
```

在测量该任务所用的时间时，我们观察到 6.9590 秒，准确率为 0.8430。随后，我们利用整个数据集来训练 SVC，包括大约 16,000 个实例用于训练和 4,000 个实例用于测试：

```py
model_large = SVC(probability =True)
model_large.fit(X_train, y_train)

y_pred_large = model_large.predict(X_test)
accuracy_large = accuracy_score(y_test, y_pred_large)
```

这次，算法花费了 20.4149 秒，约为之前时间的 3 倍，准确率为 0.8452——尽管数据量几乎翻倍，但结果非常接近。对两个模型的 ROC 曲线进行比较，结果几乎相同。

![](img/a3de088294ad99b83d639441dc7a8256.png)

作者提供的图像

**警告：** 复杂任务通常需要更多数据，但有一个点是更多的数据不一定意味着更好的结果。

**权衡：** 尽管额外的数据很少会对模型造成伤害，但权衡在于计算成本，而不是性能。当模型中添加无关的信息时，这种成本可能很高，这突显了数据预处理和清理的重要性，特别是在复杂任务中。

## 假设 4：将较长、更复杂的算法等同于更优性能

尽管并非总是如此，但存在一种观点，认为较长、复杂的算法优于较短、简单的算法。有时，即使一个简单的算法可以完美地完成任务，仍会偏向于使用更复杂的算法。尽管这种观点并非总是毫无根据，但问题在于这种信念的根本理由。如果算法确实需要复杂性和长度，那么应用这些特性也无可厚非。

为了说明这一假设，我们比较两个代码块。第一个函数看起来相当简单：

```py
def is_even_simple(num):
    return num % 2 == 0
```

另一方面，第二个函数似乎涉及更多工作，时间和空间复杂度均有所体现。然而，两个函数的*最终目标*是完全相同的：

```py
def is_even_complex(num):
    if num < 0:
        return False
    elif num == 0:
        return True
    else:
        while num >= 2:
            num -= 2
        if num == 0:
            return True
        else:
            return False
```

我故意将这个例子保持简单，但这一理念同样适用于可能过于关注特定问题或做不同事情的更复杂的代码。

**警告：** 这不适用于添加单元测试、注释或真正提升代码质量的改进。此外，较长且复杂的算法在某些情况下并非毫无依据地被认为更优。对于那些本质上需要细致决策边界或涉及复杂关系的任务，更复杂的算法可能确实是必要的。

**权衡：** 这种偏差的代价通常表现为计算资源的消耗。虽然它可能不会显著影响较简单的任务，但对于更复杂的任务，开销变得更加明显。

# 避免类别大小偏差：提高意识和缓解策略

尽管类别大小偏差可能不是最具破坏性的偏差，但它可能导致资源消耗。

***将问题分解为更简单、更小的任务，并在可能的情况下从那里开始，以更清晰地理解潜在问题，这是更好的做法。***

意识到我们潜意识中倾向于偏袒某个选项是避免陷入偏见的关键。一种有价值的方法是挑战假设，利用苏格拉底式提问技术。

[理解苏格拉底式提问：全面指南](https://www.verywellmind.com/socratic-questioning-8350838?source=post_page-----8dbd851608c3--------------------------------) [## 理解苏格拉底式提问：全面指南

### 苏格拉底式提问鼓励人们更深入地思考问题，超越自身的视角……

[理解苏格拉底式提问：全面指南](https://www.verywellmind.com/socratic-questioning-8350838?source=post_page-----8dbd851608c3--------------------------------)

***另一种方法是尝试证明与最初偏好的假设相反的假设。***

花时间逐个评估数据，并将每个问题区别对待，可以提供宝贵的见解。

## 总结：

总之，本文突出了类别规模偏差在数据科学领域中的影响。关键的结论是，要时刻留意这些偏差，并始终考虑问题的背景。

## 即将到来…

我们的分析不仅仅依赖于算法和模型，还受到深层次的认知偏差的重大影响。本文探讨了类别规模偏差及其对数据科学决策的影响。然而，这只是个开始。在本系列的后续文章中，我将专注于揭示更多认知偏差及其对数据研究和分析的影响。从因果关系的假设到轶事证据的吸引力，从确认偏差到从众效应，这些文章将探索人类偏差与数据科学的交集。

我的目标是对可能影响我们分析追求的偏差进行更深入的研究，并促进更细致和偏差意识的数据科学实践。

我添加了一些额外的资源或进一步的探索。

# 资源

[](https://thedecisionlab.com/biases?source=post_page-----8dbd851608c3--------------------------------) [## 认知偏差和启发式列表 - 认知决策实验室

### 以下是行为科学领域中最重要的认知偏差和启发式的列表。

[认知偏差列表：180 多种启发式的可视化](https://thedecisionlab.com/biases?source=post_page-----8dbd851608c3--------------------------------) [](https://www.teachthought.com/critical-thinking/cognitive-biases/?source=post_page-----8dbd851608c3--------------------------------) [## 认知偏差列表：180 多种启发式的可视化

### 认知偏差是指倾向于选择性地搜索或解释数据，以确认现有的观点…

[www.teachthought.com](https://www.teachthought.com/critical-thinking/cognitive-biases/?source=post_page-----8dbd851608c3--------------------------------) [](https://www.mindtools.com/aipz4vt/the-ladder-of-inference?source=post_page-----8dbd851608c3--------------------------------) [## 推理阶梯 - 如何避免仓促得出结论

### 使用**推理阶梯**来探索我们在思维中从事实到决策或结论所经过的七个步骤…

[www.mindtools.com](https://www.mindtools.com/aipz4vt/the-ladder-of-inference?source=post_page-----8dbd851608c3--------------------------------)
