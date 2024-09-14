# 停止在数据科学项目中硬编码——改用配置文件

> 原文：[`towardsdatascience.com/stop-hard-coding-in-a-data-science-project-use-config-files-instead-479ac8ffc76f`](https://towardsdatascience.com/stop-hard-coding-in-a-data-science-project-use-config-files-instead-479ac8ffc76f)

## 如何在 Python 中高效地与配置文件交互

[](https://khuyentran1476.medium.com/?source=post_page-----479ac8ffc76f--------------------------------)![Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----479ac8ffc76f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----479ac8ffc76f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----479ac8ffc76f--------------------------------) [Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----479ac8ffc76f--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----479ac8ffc76f--------------------------------) ·6 分钟阅读·2023 年 5 月 26 日

--

![](img/d8f861a64c002d8fcc6f28868a2a7e65.png)

作者提供的图片

*最初发布于* [*https://mathdatasimplified.com*](https://mathdatasimplified.com/2023/05/25/stop-hard-coding-in-a-data-science-project-use-configuration-files-instead/) *2023 年 5 月 26 日。*

# 问题

在你的数据科学项目中，某些值经常会变化，例如文件名、选定的特征、训练-测试分割比例和模型的超参数。

![](img/8095b45a4fe0e495526e4a29a2bea6ff.png)

作者提供的图片

在编写临时代码用于假设测试或演示目的时，硬编码这些值是可以接受的。然而，随着代码库和团队的扩展，避免硬编码变得至关重要，因为它可能导致各种问题：

+   **可维护性**：如果值在代码库中分散，保持一致地更新它们会变得更加困难。这可能导致在值需要更新时出现错误或不一致。

![](img/427844077270ffd8dd2ac34f6550b160.png)

作者提供的图片

+   **可重用性**：硬编码值限制了代码在不同场景下的重用性。

![](img/209e15a5b347e25b91db629784d90867.png)

作者提供的图片

+   **安全问题**：将敏感信息如密码或 API 密钥直接硬编码到代码中可能会带来安全风险。如果代码被共享或暴露，可能会导致未经授权的访问或数据泄露。

```py
def connect_to_database():
    # Hardcoded sensitive information
    host = 'localhost'
    username = 'admin'
    password = 'secretpassword'
    database = 'mydatabase'

    # Code to connect to the database
```

+   **测试和调试**：硬编码的值可能使测试和调试变得更加困难。如果值被硬编码到代码中，就很难有效地模拟不同的场景或测试边界情况。

```py
def divide(num1: int):
    num2 = 3
    return num1 / num2

def test_divide():
    assert divide(3) == 1.0 # passed
    # We can't test num2 = 0 because it is hard-coded
```

# 解决方案——配置文件

配置文件通过提供以下好处来解决这些问题：

+   **配置与代码分离**：配置文件允许你将参数与代码分开存储，这提高了代码的可维护性和可读性。

![](img/c9991ee64b39f9a28901cb33abc563d7.png)

作者提供的图片

+   **灵活性和可修改性**：通过配置文件，你可以轻松修改项目配置，而不必修改代码本身。这种灵活性允许快速实验、参数调整，并将项目适应不同的场景或环境。

![](img/b145f5c2dcad49879af0aa8b8da7f7f2.png)

作者提供的图片

+   **版本控制**：将配置文件存储在版本控制中可以跟踪配置的更改。这有助于维护项目配置的历史记录，并促进团队成员之间的协作。

![](img/8d676784b28c8530a239a2338e686682.png)

作者提供的图片

+   **部署**：当将数据科学项目部署到生产环境时，配置文件可以轻松自定义生产环境的特定设置，而无需修改代码。这种配置与代码的分离简化了部署过程。

# Hydra 简介

有许多 Python 库可以用来创建配置文件，如 pyyaml、configparser、ConfigObj。然而，[Hydra](https://hydra.cc/)这款开源 Python 库脱颖而出，成为我首选的配置管理工具，因为它拥有一套令人印象深刻的功能，包括：

+   便捷的参数访问

+   命令行配置覆盖

+   从多个来源组合配置

+   执行具有不同配置的多个作业

让我们深入探讨这些功能。

随意玩耍并在这里分叉本文的源代码：

[](https://github.com/khuyentran1401/hydra-demo?source=post_page-----479ac8ffc76f--------------------------------) [## GitHub - khuyentran1401/hydra-demo

### 你现在不能执行该操作。你在另一个标签页或窗口中登录了。你在另一个标签页或…

github.com](https://github.com/khuyentran1401/hydra-demo?source=post_page-----479ac8ffc76f--------------------------------)

# 便捷的参数访问

假设所有的配置文件都存储在`conf`文件夹下，所有的 Python 脚本都存储在`src`文件夹下。

```py
.
├── conf/
│   └── main.yaml
└── src/
    ├── __init__.py
    ├── process.py
    └── train_model.py
```

`main.yaml`文件如下所示：

```py
# main.yaml
data:
  raw: data/raw/winequality-red.csv
  intermediate: data/intermediate

model: models

processs:
  cols_to_drop:
  - free sulfur dioxide
  feature: quality
  test_size: 0.2
```

在 Python 脚本中访问配置文件就像在你的 Python 函数上应用一个装饰器一样简单。

```py
import hydra
from omegaconf import DictConfig

@hydra.main(config_path="../conf", config_name="main", version_base=None)
def process_data(config: DictConfig):
    ...
```

要从配置文件中访问特定参数，我们可以使用点符号（例如，`config.process.cols_to_drop`），这比使用括号（例如，`config['process']['cols_to_drop']`）更简洁直观。

![](img/43626352d44f39a0e66ca2293d25e36a.png)

作者提供的图片

这种直接的方法使你可以轻松地检索所需的参数。

# 命令行配置覆盖

假设你正在尝试不同的`test_size`。一遍遍打开配置文件并修改`test_size`值是很耗时的。

![](img/038d2f6da0be792732625ec2d9a30b83.png)

作者图片

幸运的是，Hydra 使得直接从命令行覆盖配置变得简单。这种灵活性允许快速调整和微调，而无需修改底层配置文件。

```py
python src/process_data.py processs.test_size=0.3
```

# 从多个来源组合配置

想象一下你希望尝试各种数据处理方法和模型超参数的组合。虽然你可以每次运行新实验时手动编辑配置文件，但这种方法可能非常耗时。

![](img/8d675b426a99701bef7dec3699ad5c77.png)

作者图片

Hydra 通过配置组支持从多个来源组合配置。要创建一个数据处理的配置组，创建一个名为`process`的目录来保存每种处理方法的文件：

```py
.
└── conf/
    ├── process/
    │   ├── process1.yaml
    │   └── process2.yaml
    └── main.yaml
```

![](img/74d2af88f2a45074bd2e760c7d20f395.png)

作者图片

如果你想默认使用`process1.yaml`文件，请将其添加到 Hydra 的默认列表中。

![](img/4cd99301d6e3ead442bda1add9e6cda2.png)

作者图片

按照相同的程序创建训练超参数的配置组：

```py
.
└── conf/
    ├── process/
    │   ├── process1.yaml
    │   └── process2.yaml
    ├── train/
    │   ├── train1.yaml
    │   └── train2.yaml
    └── main.yaml
```

![](img/14963f59d7ac3908e28e452c78d55a89.png)

作者图片

将`train1`设置为默认配置文件：

![](img/2ee302e645430c5fb0ba6034b6a50b46.png)

作者图片

现在运行应用程序将默认使用`process1.yaml`文件和`model1.yaml`文件中的参数：

```py
$ python src/process.py --help

process:
  cols_to_drop:
   - free sulfur dioxide  
  feature: quality
  test_size: 0.2
train:
  hyperparameters:
    svm__kernel:
      - rbf
    svm__C:
      - 0.1
      - 1
```

这种功能特别有用，当需要无缝组合不同的配置文件时。

# 多次运行

假设你想对多种处理方法进行实验，一个一个地应用每个配置可能是一个耗时的任务。

```py
$ python src/process.py process=process1 # wait for this to finish

$ python src/process.py process=process2 # then run the application with another config
```

幸运的是，Hydra 允许你同时使用不同的配置运行相同的应用程序。

```py
$ python src/process.py --multirun process=process1,process2
```

这种方法简化了使用各种参数运行应用程序的过程，*最终节省了宝贵的时间和精力*。

# 结论

恭喜！你刚刚了解了使用配置文件的重要性以及如何使用 Hydra 创建配置文件。我希望这篇文章能为你提供创建自己配置文件所需的知识。

我喜欢撰写关于数据科学概念的文章，并玩弄各种数据科学工具。你可以通过以下方式获取我最新的帖子：

+   订阅我在[数据科学简化](https://mathdatasimplified.com/)上的新闻通讯。

+   在[LinkedIn](https://www.linkedin.com/in/khuyen-tran-1401/)和[Twitter](https://twitter.com/KhuyenTran16)上与我联系。
