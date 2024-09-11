# 使用 TOML 配置轻松管理深度学习模型

> 原文：[https://towardsdatascience.com/managing-deep-learning-models-easily-with-toml-configurations-fb680b9deabe?source=collection_archive---------4-----------------------#2023-06-15](https://towardsdatascience.com/managing-deep-learning-models-easily-with-toml-configurations-fb680b9deabe?source=collection_archive---------4-----------------------#2023-06-15)

## 你可能永远不需要那些长 CLI 参数来运行你的 train.py

[](https://equipintelligence.medium.com/?source=post_page-----fb680b9deabe--------------------------------)[![Shubham Panchal](../Images/d48aecd8b1ed27ab68fc2e7ff6716606.png)](https://equipintelligence.medium.com/?source=post_page-----fb680b9deabe--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fb680b9deabe--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fb680b9deabe--------------------------------) [Shubham Panchal](https://equipintelligence.medium.com/?source=post_page-----fb680b9deabe--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd45a9465f044&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmanaging-deep-learning-models-easily-with-toml-configurations-fb680b9deabe&user=Shubham+Panchal&userId=d45a9465f044&source=post_page-d45a9465f044----fb680b9deabe---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fb680b9deabe--------------------------------) ·4 分钟阅读·2023年6月15日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffb680b9deabe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmanaging-deep-learning-models-easily-with-toml-configurations-fb680b9deabe&source=-----fb680b9deabe---------------------bookmark_footer-----------)![](../Images/e4b95ed664b346a4ed6a66901d2d0454.png)

照片由[Scott Graham](https://unsplash.com/@homajob?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

管理深度学习模型可能很困难，因为需要为所有模块配置大量参数和设置。训练模块可能需要诸如`batch_size`或`num_epochs`的参数，或用于学习率调度器的参数。同样，数据预处理模块可能需要`train_test_split`或用于图像增强的参数。

管理或引入这些参数到管道中的一种天真的方法是将它们作为CLI参数传递给脚本。命令行参数可能很难输入，并且在单个文件中管理所有参数可能不切实际。TOML文件提供了一种更清洁的配置管理方式，脚本可以以Python `dict`的形式加载配置的必要部分，而无需读取/解析命令行参数的样板代码。

在这篇博客中，我们将深入探讨TOML在配置文件中的使用，以及如何在训练/部署脚本中高效地使用它们。

# 什么是TOML文件？

TOML，代表[*Tom’s Obvious Minimal Language*](https://toml.io/en/)，是一种专门为配置文件设计的文件格式。TOML文件的概念与[YAML/YML文件](https://yaml.org/)非常相似，这些文件能够以树状层次结构存储键值对。[TOML相对于YAML的一个优点是其可读性](https://www.reddit.com/r/devops/comments/6f82nu/yaml_vs_toml/)，当存在多个嵌套层级时，这一点尤为重要。

![](../Images/243acf211a4cf117dd43331068b4c1c7.png)

图1\. 同一模型配置以TOML（左）和YAML（右）编写。TOML允许我们在相同的缩进级别下编写键值对，无论层次结构如何。

> 就个人而言，除了增强的可读性，我找不到任何实用的理由来优先选择TOML而非YAML。使用YAML完全没问题，这里有一个[Python包](https://pypi.org/project/PyYAML/)用于解析YAML。

# 为什么我们需要在TOML中配置？

使用TOML存储模型/数据/部署配置有两个优势：

在单个文件中管理所有配置：通过TOML文件，我们可以为不同模块创建多个设置组。例如，在图1中，与模型训练过程相关的设置嵌套在`[train]`属性下，类似地，模型部署所需的`port`和`host`存储在`deploy`下。我们不需要在`train.py`或`deploy.py`之间跳转以更改参数，而是可以从单个TOML配置文件中全局化所有设置。

> 如果我们在虚拟机上训练模型，而没有代码编辑器或IDE可用来编辑文件，这可能会非常有帮助。单个配置文件可以通过大多数虚拟机上可用的`vim`或`nano`轻松编辑。

# 我们如何从TOML中读取配置？

要从TOML文件中读取配置，可以使用两个Python包，`[toml](https://github.com/uiri/toml)` 和 `[munch](https://github.com/uiri/toml)`。`toml`将帮助我们读取TOML文件并返回文件内容作为Python `dict`。`munch`将转换`dict`的内容以实现属性样式的元素访问。例如，不需要写`config[ "training" ][ "num_epochs" ]`，我们可以直接写`config.training.num_epochs`，这增强了可读性。

请考虑以下文件结构，

```py
- config.py
- train.py
- project_config.toml
```

`project_config.toml` 包含我们的 ML 项目的配置，如，

```py
[data]
vocab_size = 5589
seq_length = 10
test_split = 0.3
data_path = "dataset/"
data_tensors_path = "data_tensors/"

[model]
embedding_dim = 256
num_blocks = 5
num_heads_in_block = 3

[train]
num_epochs = 10
batch_size = 32
learning_rate = 0.001
checkpoint_path = "auto"
```

在 `config.py` 中，我们创建了一个函数，该函数返回使用 `toml` 和 `munch` 的 *munchified* 版本的配置，

```py
$> pip install toml munch
```

```py
import toml
import munch

def load_global_config( filepath : str = "project_config.toml" ):
    return munch.munchify( toml.load( filepath ) )

def save_global_config( new_config , filepath : str = "project_config.toml" ):
    with open( filepath , "w" ) as file:
        toml.dump( new_config , file )
```

现在，在我们项目的任何文件中，比如 `train.py` 或 `predict.py`，我们都可以加载这个配置，

```py
from config import load_global_config

config = load_global_config()

batch_size = config.train.batch_size
lr = config.train.learning_rate

if config.train.checkpoint_path == "auto":
    # Make a directory with name as current timestamp
    pass
```

`print( toml.load( filepath ) )` 的输出是，

```py
{'data': {'data_path': 'dataset/',
          'data_tensors_path': 'data_tensors/',
          'seq_length': 10,
          'test_split': 0.3,
          'vocab_size': 5589},
 'model': {'embedding_dim': 256, 'num_blocks': 5, 'num_heads_in_block': 3},
 'train': {'batch_size': 32,
           'checkpoint_path': 'auto',
           'learning_rate': 0.001,
           'num_epochs': 10}}
```

如果您正在使用像 W&B Tracking 或 MLFlow 这样的 MLOps 工具，将配置保持为 `dict` 可能会很有帮助，因为我们可以直接将其作为参数传递。

# 结束

希望您考虑在您的下一个 ML 项目中使用 TOML 配置！这是管理全局或局部于您的训练/部署或推理脚本的设置的一种清晰方式。

而不是编写长长的命令行参数，脚本可以直接从 TOML 文件中加载配置。如果我们希望用不同的超参数训练两个版本的模型，我们只需在 `config.py` 中更改 TOML 文件。我最近的项目中开始使用 TOML 文件，实验变得更快了。MLOps 工具也可以管理模型的版本以及它们的配置，但以上讨论的方法的简单性是独特的，并且需要最小的现有项目更改。

希望您喜欢阅读。祝您有个愉快的一天！
