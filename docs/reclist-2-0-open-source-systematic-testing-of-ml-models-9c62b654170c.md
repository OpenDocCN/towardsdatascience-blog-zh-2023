# RecList 2.0：开源系统化测试 ML 模型

> 原文：[https://towardsdatascience.com/reclist-2-0-open-source-systematic-testing-of-ml-models-9c62b654170c?source=collection_archive---------9-----------------------#2023-08-08](https://towardsdatascience.com/reclist-2-0-open-source-systematic-testing-of-ml-models-9c62b654170c?source=collection_archive---------9-----------------------#2023-08-08)

## 一个新的 RecList 以提供更多灵活性和更好的评估支持

[](https://fede-bianchi.medium.com/?source=post_page-----9c62b654170c--------------------------------)[![Federico Bianchi](../Images/fa38ff2051af04df7803af7d84c5cd4d.png)](https://fede-bianchi.medium.com/?source=post_page-----9c62b654170c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9c62b654170c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9c62b654170c--------------------------------) [Federico Bianchi](https://fede-bianchi.medium.com/?source=post_page-----9c62b654170c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2aff872fe60e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freclist-2-0-open-source-systematic-testing-of-ml-models-9c62b654170c&user=Federico+Bianchi&userId=2aff872fe60e&source=post_page-2aff872fe60e----9c62b654170c---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9c62b654170c--------------------------------) ·7分钟阅读·2023年8月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9c62b654170c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freclist-2-0-open-source-systematic-testing-of-ml-models-9c62b654170c&user=Federico+Bianchi&userId=2aff872fe60e&source=-----9c62b654170c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9c62b654170c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freclist-2-0-open-source-systematic-testing-of-ml-models-9c62b654170c&source=-----9c62b654170c---------------------bookmark_footer-----------)![](../Images/d17b7a68ebe3a9dc2e4940a8b7867ef1.png)

[照片由 Lucas Pezeta 提供](https://www.pexels.com/photo/black-telescope-under-blue-and-blacksky-2034892/)

# 介绍

评估是一个复杂的事项。管理撰写评估管道中涉及的不同组件往往很困难，你需要在某个地方拥有模型，需要加载它，然后获取测试，运行测试等等。

然后呢？嗯，你需要将结果保存到某个地方，也许还需要在线记录输出，以便你可以跟踪它们。

由于这总是一个艰难的过程，我们最近尝试提供一种更结构化的测试方法。在这篇博客文章中，我们介绍并展示了如何使用 [RecList](https://github.com/RecList/reclist) beta，我们的开源评估包；RecList 是一种通用的即插即用方法，用于扩展测试，具有易于扩展的自定义用例接口。RecList 是一个自由开放的开源项目，您可以在 [GitHub](https://github.com/RecList/reclist) 上自由获取。

RecList 允许你将代码的评估部分分离出来，并封装在一个类中，这个类会自动处理其他几个方面的事情（例如，存储和日志记录）。

![](../Images/e3548430dfd118198443b185c1249228.png)

RecList 提供了一种简单的方法来系统化测试，并在你训练自己的模型后保存所有需要的信息。

我们在几年前开始开发 RecList，并且 RecList 的 alpha 版本在一年前左右发布。从那时起，RecList 已经收获了超过 400 个 GitHub stars。

我们已经使用 RecList 并进行了压力测试，在 2022 年的 CIKM 上举办了 RecSys 挑战，目前正在准备 2023 年的 KDD 挑战。RecList 使我们能够系统化所有参与者的评估。我们的想法是，一旦每个人都提供相同的 RecList，比较不同的评估将变得简单。我们的经验总结出现在我们的 [Nature Machine Intelligence](https://www.nature.com/articles/s42256-022-00606-0) 评论文章中。

RecList 最初是在一篇学术论文中介绍的，但我们也有一个在 Towards Data Science 出版的概述，你可以在这里阅读：

[](/ndcg-is-not-all-you-need-24eb6d2f1227?source=post_page-----9c62b654170c--------------------------------) [## NDCG 并不是你所需要的一切

### 使用 RecList 对推荐系统进行行为测试

towardsdatascience.com](/ndcg-is-not-all-you-need-24eb6d2f1227?source=post_page-----9c62b654170c--------------------------------)

> Chia, P. J., Tagliabue, J., Bianchi, F., He, C., & Ko, B. (2022年4月)。超越 nDCG：使用 RecList 对推荐系统进行行为测试。见 *Web Conference 2022 附录*（第 99–104 页）。

虽然我们最初设计 RecList 是为了推荐系统的测试，但没有什么能阻止你将 RecList 用于测试其他机器学习模型。那么，为什么会有一篇新的博客文章呢？好吧，在开发了第一个版本后，我们意识到它需要一些更新。

## 我们学到了什么：重新思考 API

通常只有在你构建了某样东西之后，你才会意识到如何改进它。

对于那些使用了 RecList 1.0 的用户，我们对 RecList API 进行了重大更新。最初，我们对代码结构和输入/输出对有更严格的约束。

事实上，当我们实现 RecList 时，我们的目标是提供一个更通用的 API，用于评估推荐系统，并提供了几种开箱即用的功能。然而，为了做到这一点，我们不得不创建多个抽象接口，用户需要实现这些接口。

例如，**原始的 RecList 1.0** 要求用户将自己的模型和数据集包装到预定义的抽象类中（即 RecModel 和 RecDataset）。这使我们能够实现一组由这些抽象连接的通用行为。然而，我们很快意识到，这可能会经常使流程复杂化，并需要额外的工作，这可能让一些人不喜欢。

在 **RecList 2.0** 中，我们决定让这些约束成为可选的：我们使测试更加灵活。用户定义自己的评估用例，将其包装在一个便捷的装饰器中，并且他们已经实现了元数据存储和日志记录。用户可以将测试接口分享给其他人，他们可以运行相同的实验。

**总结**：我们意识到在构建需要其他人使用的软件时，灵活性是多么重要。

# RecList 2.0 实践

现在，让我们探索一个如何使用 RecList 来编写和运行评估流程的简单用例。我们将使用非常简单的模型，这些模型随机输出数字，以减少机器学习项目中涉及的复杂性。

## 一个简单的用例

让我们创建一个非常简单的用例和一个非常简单的数据集。假设我们有一个整数目标序列，每个都有一个关联的类别。我们只是生成一些随机数据。

```py
n = 10000

target = [randint(0, 1) for _ in range(n)]
metadata = {"categories": [choice(["red", "blue", "yellow"]) 
                          for _ in range(n)]}
```

我们的非常简单的数据集应该看起来像这样：

```py
>>> target

[0, 1, 0, 1, 1, 0]

>>> metadata

{"categories" : ["red", "blue", "yellow", "blue", "yellow", "yellow"]}
```

## 一个简单的模型

现在假设我们有一个 DummyModel，它会随机输出整数。当然，正如我们所说，这不是一个“好”的模型，但它是一个很好的抽象，可以用来查看整个评估流程。

```py
class DummyModel:
  def __init__(self, n):
          self.n = n

      def predict(self):
          from random import randint
          return [randint(0, 1) for _ in range(self.n)]

simple_model = DummyModel(n)

# let's run some predictions
predictions = simple_model.predict()
```

现在，我们如何运行评估？

## 一个简单的 RecList

RecList 是一个 Python 类，继承了我们 RecList 抽象类的功能。RecList 实现了 RecTests，这是一些简单的抽象，允许你系统化评估。例如，这可能是一个可能的准确性测试。

```py
@rec_test(test_type="Accuracy", display_type=CHART_TYPE.SCALAR)
def accuracy(self):
    """
    Compute the accuracy
    """
    from sklearn.metrics import accuracy_score

    return accuracy_score(self.target, self.predictions)
```

我们正在采用 sklearn 准确性指标，并将其包装到另一个方法中。这与简单的准确性函数有何不同？好吧，装饰器允许我们引入一些额外的功能：例如，rectest **现在会自动将信息存储到本地文件夹中**。此外，定义一种图表类型**使我们能够为这些结果创建一些可视化。**

如果我们想要更复杂的测试会怎样？例如，我们想要查看在不同类别中我们的准确性有多稳定（例如，计算红色物体的准确性是否高于黄色物体的准确性？）

```py
@rec_test(test_type="SlicedAccuracy", display_type=CHART_TYPE.SCALAR)
def sliced_accuracy_deviation(self):
    """
    Compute the accuracy by slice
    """
    from reclist.metrics.standard_metrics import accuracy_per_slice

    return accuracy_per_slice(
        self.target, self.predictions, self.metadata["categories"])
```

现在让我们看看一个完整的 RecList 示例！

```py
class BasicRecList(RecList):

    def __init__(self, target, metadata, predictions, model_name, **kwargs):
        super().__init__(model_name, **kwargs)
        self.target = target
        self.metadata = metadata
        self.predictions = predictions

    @rec_test(test_type="SlicedAccuracy", display_type=CHART_TYPE.SCALAR)
    def sliced_accuracy_deviation(self):
        """
        Compute the accuracy by slice
        """
        from reclist.metrics.standard_metrics import accuracy_per_slice

        return accuracy_per_slice(
            self.target, self.predictions, self.metadata["categories"]
        )

    @rec_test(test_type="Accuracy", display_type=CHART_TYPE.SCALAR)
    def accuracy(self):
        """
        Compute the accuracy
        """
        from sklearn.metrics import accuracy_score

        return accuracy_score(self.target, self.predictions)

    @rec_test(test_type="AccuracyByCountry", display_type=CHART_TYPE.BARS)
    def accuracy_by_country(self):
        """
        Compute the accuracy by country
        """
        # TODO: note that is a static test, 
        # used to showcase the bin display

        from random import randint
        return {"US": randint(0, 100), 
                "CA": randint(0, 100), 
                "FR": randint(0, 100)}
```

几行代码即可将我们需要的内容集中在一个地方。我们可以重用这段代码用于新的模型，或添加测试并重新运行过去的模型。

只要你的指标返回了一些值，你就可以以任何你喜欢的方式实现它们。例如，这个 BasicRecList 在特定的上下文中评估特定的模型。但没有什么能阻止你生成更多模型特定的 reclists（例如，GPT-RecList）或数据集特定的 reclists（例如，IMDB-Reclist）。如果你想查看 RecList 上深度模型的示例，可以 [查看这个 Colab](https://colab.research.google.com/drive/1Ftl2B7BVFMfFjyjWweFCP_gA_LdCl7-a?usp=sharing)。

## 运行并获取输出

让我们运行 RecList。我们需要目标数据、元数据和预测。我们还可以指定一个日志记录器和一个元数据存储。

```py
rlist = BasicRecList(
    target=target,
    metadata=metadata,
    predictions=predictions,
    model_name="myRandomModel",
)

# run reclist
rlist(verbose=True)
```

这个过程的输出是什么？我们将在命令行中看到以下结果：对于每个测试，我们都有一个实际的得分。

![](../Images/e654f565771e8e18b5cbe8a9e7ee25e5.png)

指标也会自动绘制。例如，AccuracyByCountry 应该显示如下内容：

![](../Images/356054d8c1e7af98c49ace17f2c82382.png)

RecTest 生成的图示示例。

除此之外，RecList 还会保存一个 JSON 文件，其中包含我们刚刚运行的实验的所有信息：

```py
{
  "metadata": {
    "model_name": "myRandomModel",
    "reclist": "BasicRecList",
    "tests": [
      "sliced_accuracy",
      "accuracy",
      "accuracy_by_country"
    ]
  },
  "data": [
    {
      "name": "SlicedAccuracy",
      "description": "Compute the accuracy by slice",
      "result": 0.00107123176804103,
      "display_type": "CHART_TYPE.SCALAR"
    },
...
}
```

好的一点是，只需几行额外的代码，大部分日志记录工作就会自动处理！

## 使用在线日志记录器和元数据存储

默认情况下，RecList 运行器将使用以下日志记录器和元数据设置。

```py
logger=LOGGER.LOCAL,
metadata_store= METADATA_STORE.LOCAL,
```

然而，没有什么阻止我们使用在线和云解决方案。例如，我们将 CometML 和 Neptune API 封装起来，以便你可以直接在评估管道中使用它们。我们还提供 S3 数据存储的支持。

例如，向 BasicRecList 添加几个参数将允许我们在 [Neptune](https://www.loom.com/share/e2db2a15d77741f08fc84e2284bed8e2?sid=83290789-98d3-4d3e-9271-2f8e91d980e9) 上记录信息（我们对 [Comet.ml](https://www.loom.com/share/870d1b256a0e4bd18b98f5f7c9a78759?sid=3161eb5b-5ab1-4863-ba50-373757d5e0cc) 也提供类似支持）！

```py
rlist = BasicRecList(
    target=target,
    model_name="myRandomModel",
    predictions=predictions,
    metadata=metadata,
    logger=LOGGER.NEPTUNE,
    metadata_store= METADATA_STORE.LOCAL,
    NEPTUNE_KEY=os.environ["NEPTUNE_KEY"],
    NEPTUNE_PROJECT_NAME=os.environ["NEPTUNE_PROJECT_NAME"],
)

# run reclist
rlist(verbose=True)
```

以类似的方式，添加以下内容：

```py
bucket=os.environ["S3_BUCKET"]
```

将允许我们使用 S3 桶来存储元数据（当然，你还需要设置一些环境密钥）。

# 结论

就这些！我们创建 RecList 是为了使推荐系统的评估更加系统化和有序。我们希望这一大规模的 API 重构能帮助人们构建更可靠的评估管道！

## 致谢

在 2022 年 6 月至 12 月期间，我们的 beta 版开发得到了 Comet、Neptune、Gantry 的出色支持，并在 Unnati Patel 的帮助下完成。
