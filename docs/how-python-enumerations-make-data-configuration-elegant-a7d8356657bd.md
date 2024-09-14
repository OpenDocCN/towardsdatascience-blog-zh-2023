# Python 枚举如何使数据配置优雅

> 原文：[`towardsdatascience.com/how-python-enumerations-make-data-configuration-elegant-a7d8356657bd`](https://towardsdatascience.com/how-python-enumerations-make-data-configuration-elegant-a7d8356657bd)

## Python 中用于机器学习项目的枚举简介

[](https://medium.com/@ebbeberge?source=post_page-----a7d8356657bd--------------------------------)![Eirik Berge, PhD](https://medium.com/@ebbeberge?source=post_page-----a7d8356657bd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a7d8356657bd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a7d8356657bd--------------------------------) [Eirik Berge, PhD](https://medium.com/@ebbeberge?source=post_page-----a7d8356657bd--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a7d8356657bd--------------------------------) ·阅读时间 7 分钟·2023 年 5 月 28 日

--

![](img/f50c10b5f705134f0d6b76ef93de6254.png)

图片由 [Joshua Woroniecki](https://unsplash.com/@joshua_j_woroniecki?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 你的旅程概述

1.  简介

1.  一种天真的方法

1.  枚举的救援！

1.  总结

# 1 — 简介

在机器学习项目中编写 Python 代码时，几乎总是需要**配置代码**。这段代码跟踪用于配置其他代码组件的信息。虽然这个定义可能对那些不明白的人没有帮助，但几个例子会非常有用：

+   **超参数：** 在机器学习中，超参数用于训练多个略有不同的模型。超参数可能是随机森林中的树木数量。另一个例子是 Ridge 或 Lasso 回归的松弛参数。

+   **访问信息：** 在连接到数据库或云托管存储账户时，需要访问信息。这包括存储系统的名称、密码以及其他附加属性。类似地，假设你订阅了像 Kafka 或 MQTT 经纪人这样的发布/订阅系统。那么主题名称、密码和端口号就是必要的配置代码。

+   **管道信息：** 在构建数据工程管道时，需要配置代码。这可能包括用于拆分测试集和训练集的比例，或用于运行管道的 CRON 表达式。

总之，在某些情况下，配置代码可以帮助访问外部系统，如数据库和发布/订阅系统。在其他情况下，它可以帮助配置内部系统，如机器学习模型和数据工程管道。

将配置代码存储在单独的文件中（如`.env`、`.yaml`或`.json`格式）通常是有意义的。然而，这些配置代码仍需要在 Python 代码中访问。有许多方法可以在你的 Python 脚本中组织配置代码 😕

在这篇博客文章中，我将向你展示为什么**Python 枚举**比一些更明显的方法更好。我之前制作了一个关于 Python 枚举的 YouTube 视频。如果你更喜欢视频版本，可以查看那个：

# 2 — 一个简单的方法

在我们深入了解 Python 枚举如何使配置代码更具可读性之前，先来看一种常用的简单方法。存储配置信息的一种常见方法是使用全局变量。假设你有一个使用`[RandomForestClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)`来自`[scikit-learn](https://scikit-learn.org/stable/index.html)`的机器学习模型。你想将树的数量存储在一个名为`N_ESTIMATORS`的全局变量中。这是与`RandomForestClassifier`所需的参数名称配合使用的。你可以在脚本的顶部定义这个变量：

```py
# Configuration for RandomForestClassifier
N_ESTIMATORS = 100
```

只有一个配置参数时，这没有问题。然而，让我们引入更多参数：

```py
# Configuration for RandomForestClassifier
N_ESTIMATORS = 100
MAX_DEPTH = 8
N_JOBS = 3
```

第一个小问题是*Python 无法理解这些全局变量之间的关系*。它们只是不同的全局变量。因此，除非引入更多代码，否则无法对它们进行迭代或集体执行安全检查。

当你尝试在脚本中使用另一个模型时，会出现更严重的问题。假设你想尝试使用`[AdaBoostClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.AdaBoostClassifier.html)`。那么你可能会写：

```py
# Configuration for RandomForestClassifier
N_ESTIMATORS = 100
MAX_DEPTH = 8
N_JOBS = 3

# Configuration for AdaBoostClassifier
N_ESTIMATORS = 50
LEARNING_RATE = 1.0
```

你看到发生了什么了吗？*你不小心覆盖了*`*N_ESTIMATORS*`*全局变量！*虽然这里很容易发现，但假设 Python 脚本有 500 行，由其他人编写。也许你会忽略这个问题。Python 可能不会给出任何错误提示。设置`N_ESTIMATORS = 50`对于`RandomForestClassifier`并不会导致语法错误 😧

你如何解决这个问题？你可以引入一个延长的名称来表示`N_ESTIMATORS`全局变量属于哪个模型：

```py
# Configuration for RandomForestClassifier
N_ESTIMATORS_RANDOM_FOREST_CLASSIFIER = 100
MAX_DEPTH = 8
N_JOBS = 3

# Configuration for AdaBoostClassifier
N_ESTIMATORS_ADA_BOOST_CLASSIFIER = 50
LEARNING_RATE = 1.0
```

现在看起来`LEARNING_RATE`全局变量与`AdaBoostClassifier`不再相关。那么，不妨为所有变量添加后缀以避免混淆：

```py
# Configuration for RandomForestClassifier
N_ESTIMATORS_RANDOM_FOREST_CLASSIFIER = 100
MAX_DEPTH_RANDOM_FOREST_CLASSIFIER = 8
N_JOBS_RANDOM_FOREST_CLASSIFIER = 3

# Configuration for AdaBoostClassifier
N_ESTIMATORS_ADA_BOOST_CLASSIFIER = 50
LEARNING_RATE_ADA_BOOST_CLASSIFIER = 1.0
```

除了变量名过长，你会开始看到配置代码中遇到的细微问题。这些问题很快会造成难以理解的错误，最糟糕的情况下是难以理解的错误，最好的情况下是糟糕的变量名。

对于数据库调用，通常在一个脚本中调用多个数据库也是很常见的。*你真的想要以像* `*PRODUCTION_DATABASE_INGESTION_ACCESS_KEY*` *这样的变量名结束吗？*

是否有更好的方法来处理 Python 中的配置代码？

# 3 — 枚举来拯救！

![](img/b9774578a46a637bbf2e28120ddbd426.png)

[Zac Durant](https://unsplash.com/ja/@zacdurant?utm_source=medium&utm_medium=referral) 拍摄的照片，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

[Python 枚举](https://docs.python.org/3/library/enum.html) 提供了一种更优雅的解决方案来存储配置信息。枚举（枚举的缩写）本质上是一种定义命名常量集合的方式。快速理解枚举的最佳方式是调整前一节的机器学习配置代码。你之前的代码是：

```py
# Configuration for RandomForestClassifier
N_ESTIMATORS_RANDOM_FOREST_CLASSIFIER = 100
MAX_DEPTH_RANDOM_FOREST_CLASSIFIER = 8
N_JOBS_RANDOM_FOREST_CLASSIFIER = 3

# Configuration for AdaBoostClassifier
N_ESTIMATORS_ADA_BOOST_CLASSIFIER = 50
LEARNING_RATE_ADA_BOOST_CLASSIFIER = 1.0
```

现在可以这样写：

```py
from enum import Enum

class RandomForest(Enum):
  """An Enum for tracking the configuration of random forest classifiers."""
  N_ESTIMATORS = 100
  MAX_DEPTH = 8
  N_JOBS = 3

class AdaBoost(Enum):
  """An Enum for tracking the configuration of Ada boost classifiers."""
  N_ESTIMATORS = 50
  LEARNING_RATE = 1.0
```

## 一些优点

让我们快速看看 Python 枚举给我们带来了什么优势：

+   每个参数（例如，`MAX_DEPTH`）现在按层次结构存储在它们所使用的模型中。这确保了在引入更多配置代码时不会被覆盖。因此，也不需要过长的变量名。

+   `RandomForestClassifier` 中使用的不同参数现在被分组到 `RandomForest` 枚举中。因此，它们可以被迭代，并且可以集体分析以确保类型安全。

+   由于枚举是类，它们可以像我上面所示那样拥有**文档字符串**。虽然对于这个示例来说可能不严格需要，但在其他示例中，这可能会澄清枚举所指的内容。将其作为与类相关联的文档字符串，而不是自由流动的注释，效果要好得多。一方面，自动文档生成软件现在会检测到文档字符串属于枚举。对于自由流动的注释，这可能会被忽略。

如果你现在想在脚本中进一步访问配置代码，可以简单地写：

```py
from sklearn.ensemble import RandomForestClassifier

RandomForestClassifier(
  n_estimators=RandomForest.N_ESTIMATORS.value,
  max_depth=RandomForest.MAX_DEPTH.value,
  n_jobs=RandomForest.N_JOBS.value
)
```

这看起来很干净，易于阅读 😍

## 一些 Python 枚举的功能

让我们通过查看这个玩具示例来说明 Python 枚举的一些简单功能：

```py
from enum import Enum
class HTTPStatusCodes(Enum):
    """An Enum that keeps track of status codes for HTTP(s) requests."""
    OK = 200
    CREATED = 201
    BAD_REQUEST = 400
    NOT_FOUND = 404
    SERVER_ERROR = 500
```

显然，枚举中的信息是相关的；它们都关于 HTTP(s) 状态码。鉴于此，你现在可以使用以下简单代码来提取名称和值：

```py
print(HTTPStatusCodes.OK.name)
>>> OK

print(HTTPStatusCodes.OK.value)
>>> 200
```

Python 枚举还允许你向后查找：给定状态码值为 `404`，你可以通过简单地写来找到状态码名称：

```py
print(HTTPStatusCodes(200).name)
>>> OK
```

你可以通过例如使用列表构造函数 `list()` 来集体处理枚举中的名称/值对：

```py
print(list(HTTPStatusCodes))
>>> [
  <HTTPStatusCodes.OK: 200>, 
  <HTTPStatusCodes.CREATED: 201>, 
  <HTTPStatusCodes.BAD_REQUEST: 400>, 
  <HTTPStatusCodes.NOT_FOUND: 404>,
  <HTTPStatusCodes.SERVER_ERROR: 500>
]
```

最后，你可以在 Python 中对枚举进行序列化和反序列化。为此，只需像使用 Python 中的其他熟悉对象一样使用 `pickle` 模块：

```py
from pickle import dumps, loads
print(HTTPStatusCodes is loads(dumps(HTTPStatusCodes)))
>>> True
```

这对于机器学习模型的配置代码尤为重要。在这种情况下，也有像 [MLflow](https://mlflow.org/) 这样的知名库可以优雅地为你保存配置代码。

## 敏感配置代码

使用敏感配置代码（如密码或访问密钥）时，*你绝不应该在代码中明文书写它们*。它们应该存放在一个被版本控制系统忽略的单独文件中，并导入到脚本中。

这并不意味着它们不能存储在代码中的枚举（Enums）里。枚举是关于组织的，即使是被审查过的信息也可以被组织。例如，这里有一个表示连接到[Microsoft Azure 存储账户](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-overview)的枚举：

```py
from enum import Enum
import os

class StorageAccount(Enum):
  ACCOUNT_NAME = "my account name"
  ACCESS_KEY = os.environ.get('ACCESS_KEY')
  CONTAINER_NAME = "my container name"
```

在这里，`StorageAccount`枚举从环境变量中获取`ACCESS_KEY`。这可以设置在例如`.env`文件中。请注意，在 Python 脚本中没有泄露敏感信息。然而，所有关于存储账户的信息都被整齐地组织成一个枚举。

# 4— 总结

![](img/4a06c775ef80f575c5d7cdfe36d7a151.png)

图片由[Spencer Bergen](https://unsplash.com/@spencerbergen?utm_source=medium&utm_medium=referral)拍摄，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇博客文章中，我们已经看到 Python 枚举如何使配置代码变得易读、自我文档化，并且减少错误。如果你想了解更多关于枚举的内容，我推荐官方的[Enum HOWTO 指南](https://docs.python.org/3/howto/enum.html#pickling)和博客文章[用 Python 的 Enum 构建常量枚举](https://realpython.com/python-enum/)。

如果你对数据科学、编程或两者之间的任何内容感兴趣，可以随时在[LinkedIn](https://www.linkedin.com/in/eirik-berge/)上添加我，并打个招呼✋

**喜欢我的写作吗？** 查看我其他的一些帖子，获取更多 Python 内容：

+   用漂亮的类型提示现代化你的罪恶 Python 代码

+   在 Python 中可视化缺失值是令人震惊的简单

+   用 PyOD 在 Python 中引入异常/离群点检测 🔥

+   5 个超棒的 NumPy 函数，能在关键时刻救你一命

+   5 个专家技巧，让你的 Python 字典技能飞跃 🚀
