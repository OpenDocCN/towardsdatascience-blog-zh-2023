# Moto、Pytest 和 AWS 数据库：质量与数据工程的交汇点

> 原文：[https://towardsdatascience.com/moto-pytest-and-aws-databases-a-quality-and-data-engineering-crossroads-ae58f9e7b265?source=collection_archive---------3-----------------------#2023-01-03](https://towardsdatascience.com/moto-pytest-and-aws-databases-a-quality-and-data-engineering-crossroads-ae58f9e7b265?source=collection_archive---------3-----------------------#2023-01-03)

## Moto 和 Pytest 如何与 AWS 数据库协同工作

[](https://medium.com/@taywag?source=post_page-----ae58f9e7b265--------------------------------)[![Taylor Wagner](../Images/5bb000c13c40b17049632a10982ee32b.png)](https://medium.com/@taywag?source=post_page-----ae58f9e7b265--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ae58f9e7b265--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ae58f9e7b265--------------------------------) [Taylor Wagner](https://medium.com/@taywag?source=post_page-----ae58f9e7b265--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd40c65f02fb7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmoto-pytest-and-aws-databases-a-quality-and-data-engineering-crossroads-ae58f9e7b265&user=Taylor+Wagner&userId=d40c65f02fb7&source=post_page-d40c65f02fb7----ae58f9e7b265---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ae58f9e7b265--------------------------------) ·9分钟阅读·2023年1月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fae58f9e7b265&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmoto-pytest-and-aws-databases-a-quality-and-data-engineering-crossroads-ae58f9e7b265&user=Taylor+Wagner&userId=d40c65f02fb7&source=-----ae58f9e7b265---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fae58f9e7b265&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmoto-pytest-and-aws-databases-a-quality-and-data-engineering-crossroads-ae58f9e7b265&source=-----ae58f9e7b265---------------------bookmark_footer-----------)![](../Images/f2014e6ddd769007fc5d5973991a3b7b.png)

图片由[Christina @ wocintechchat.com](https://unsplash.com/@wocintechchat?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 概述

我最近花了很多时间使用Pytest来测试AWS服务，通过Boto3。Boto3是一个非常强大的AWS SDK，用于处理AWS服务。我的研究和实践经验让我发现了一个叫做Moto的补充工具！Moto与Boto3配合使用，成为我在最近项目中的测试策略规划以及维护干净生产数据的首选工具。在本文中，我将分享如何以及为何使用Moto（一个虚假的Boto3）和Pytest来模拟测试AWS数据库。我甚至会一步步带你通过使用Moto和Pytest测试AWS NoSQL数据库服务——Amazon DynamoDB的过程。

*请注意：除非另有说明，所有图像均由作者提供。*

## Moto

[Moto](https://docs.getmoto.org/en/latest/docs/getting_started.html#recommended-usage)是一个能够模拟AWS服务编程使用的Python库。简单来说 — [Boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)是一个面向对象的API，用于创建、配置和管理*真实*的AWS服务和账户，而**Moto是Boto3的模拟版本**。虽然Moto没有包括Boto3提供的每一个方法，[Moto确实提供了相当一部分](https://docs.getmoto.org/en/latest/docs/services/index.html)的Boto3方法，以便在本地运行测试。在使用Moto时，我强烈建议结合使用Boto3文档和Moto文档，以获得更详细的方法解释。

**Moto的优点：**

+   不需要AWS账户

+   避免在AWS生产账户中更改实际数据或资源的风险

+   可轻松将Moto模拟设置转换为Boto3以用于实际应用场景

+   测试运行迅速 — 无延迟问题

+   易于学习和上手

+   免费！— 不会产生AWS费用

**Moto的一个缺点：**

+   不包括Boto3的*所有*方法 — Boto3提供了对AWS服务的更广泛覆盖

![](../Images/326ba1e89583c009255200fef02f8ee3.png)

Moto文档截图：可用于Amazon DynamoDB的Boto3方法的完整列表，并标记了Moto “X”可用的方法。欲查看完整列表，请点击[这里](https://docs.getmoto.org/en/latest/docs/services/dynamodb.html)。

Moto文档提供了哪些Boto3方法可以使用的洞察。例如，Boto3中略多于一半的[DynamoDB方法](https://docs.getmoto.org/en/latest/docs/services/dynamodb.html)可用于Moto进行模拟。

## Pytest

[Pytest](https://docs.pytest.org/en/7.2.x/contents.html)是一个用于编写简洁易读测试的Python框架。此框架使用详细的断言检查，使得普通的`assert`语句非常用户友好。Pytest具有强大的功能，可以扩展以支持应用程序和库的复杂功能测试。

![](../Images/76a8a7a85fe9a725d92f962ddf344e89.png)

终端截图1：执行`pytest`命令

测试可以通过一个简单的`pytest`命令来执行。`pytest`命令会运行所有测试，但可以通过在`pytest`后面添加文件路径来运行单个测试文件，如：`pytest tests/test_dynamodb.py`。运行`pytest`会给出准确的结果，每个测试会显示绿色的“.”表示通过，红色的“F”表示失败，或黄色的“S”表示跳过（如适用）。有两个非常有用的标志我将分享并强烈建议在使用Pytest执行测试时使用。*请注意：在本节的屏幕截图中，这些不同命令运行之间的代码没有任何变化。唯一的区别是标志。*

![](../Images/9f4a50e278f9cd902aa7758b132757c7.png)

终端截图2：使用-s标志执行`pytest`命令

首先是`-s`（stdout/stderr输出）标志。运行`pytest -s`会在测试运行时在终端显示所有打印语句。如果没有添加`-s`标志，即使代码中有打印语句，终端中也不会出现打印语句。这对于调试非常有用。

![](../Images/74dec82722eb3bd6e0bd02a48720287b.png)

终端截图3：使用-v标志执行`pytest`命令

另一个有用的标志是`-v`（详细）标志。使用`pytest -v`会在测试运行时在终端提供更多详细信息。这个标志将提供每个测试的类和方法名称，以及所有待运行测试的每个测试完成的累积百分比，以及每个测试的绿色“通过”、红色“失败”或黄色“跳过”指示器。*注意，这个标志不会显示打印语句。*

![](../Images/8d2243add06f462049efde4e40efbf1f.png)

终端截图4：使用组合的-sv标志执行`pytest`命令

**专业提示**：在运行测试执行命令时，可以组合标志！在使用Pytest时，我通常运行`pytest -sv`。`-s`和`-v`标志的组合提供了更具可读性的细节，并且对打印语句有更多的洞察力。

## 开始使用

Pytest要求使用Python3.7或更高版本。您可以[在这里](https://www.python.org/downloads/)下载最新版本的Python。然后，需要三个库才能开始。打开您选择的IDE，在一个全新的目录中，运行以下终端命令以下载所需的依赖项：

```py
pip install boto3 moto pytest
```

接下来，我们设置“客户端连接”。设置Moto连接的过程与Boto3非常相似。对于Boto3，要求提供AWS凭证。为了使用Moto模拟这个过程，我们将使用虚假的凭证。可以随意设置值，只要类型是字符串即可。

*请注意：在实际使用中，保护机密凭证值非常重要，不应将其硬编码到应用程序中。可以通过命令行导出变量或将其保存在环境文件中以限制访问性。* *此外，任何区域都可以用于连接，但“us-east-1”通常用于此区域，因为该区域包括所有 AWS 服务和产品。*

Pytest 在名为 `conftest.py` 的文件中利用[夹具](https://docs.pytest.org/en/6.2.x/fixture.html)在模块之间共享。在你的应用程序根目录中，通过运行以下命令创建该文件：

```py
touch conftest.py
```

并添加以下内容以配置客户端连接：

```py
import boto3
import os
import pytest

from moto import mock_dynamodb

@pytest.fixture
def aws_credentials():
    """Mocked AWS Credentials for moto."""
    os.environ["AWS_ACCESS_KEY_ID"] = "testing"
    os.environ["AWS_SECRET_ACCESS_KEY"] = "testing"
    os.environ["AWS_SECURITY_TOKEN"] = "testing"
    os.environ["AWS_SESSION_TOKEN"] = "testing"

@pytest.fixture
def dynamodb_client(aws_credentials):
    """DynamoDB mock client."""
    with mock_dynamodb():
        conn = boto3.client("dynamodb", region_name="us-east-1")
        yield conn
```

AWS 凭证函数和模拟 DynamoDB 客户端函数都实现了 Pytest 夹具装饰器，以便可以在应用程序的其他文件中使用。模拟 DynamoDB 客户端函数使用 Moto 创建与 AWS 的虚假客户端连接，类似于在 Boto3 中的操作。为了本文的目的，我们将以 DynamoDB 为示例 AWS 服务尝试 Moto。如果你有兴趣将 Moto 用于其他 AWS 服务，请查看 Moto 文档。

## 使用 Moto 测试 DynamoDB

使用以下命令在项目根目录中创建另一个名为 `test_dynamodb.py` 的文件：

```py
touch test_dynamodb.py
```

测试文件的第一步是创建一个 DynamoDB 表。由于 Moto 中的许多方法需要已经创建的表，我们将创建一个非常基础的测试表，在使用各种方法时会用到，以避免在每个测试用例中都需要创建一个表。创建此测试表有几种策略，但我将向你展示如何通过实现一个[上下文管理器](https://docs.python.org/3/library/contextlib.html)来创建该表。好消息是，上下文管理器来自 Python 的本地库，无需额外下载软件包。测试表可以这样创建：

在单个函数中使用上下文管理器创建测试表后，我们可以继续测试*这个*测试表是否确实已创建。测试表的*表名*将是许多 Moto DynamoDB 方法所需的参数，因此我会将其创建为整个测试类的变量，以避免重复并提高可读性。此步骤不是必需的，但有助于节省时间，并减少因在调用不同方法时需要不断重新编写表名值而产生的语法错误的机会。

在相同的 `test_dynamodb.py` 文件中，在上下文管理器下方创建一个测试类，以便将所有相关测试（*“my-test-table”*的测试）分组在一起。

如上所示，`dynamodb_client` 参数作为参数传递给测试方法。这来自于 `conftest.py` 文件，在该文件中我们已经建立了与 Amazon DynamoDB 的模拟连接。第 4 行显示了表名变量的可选声明。然后在第 9 行，我们调用上下文管理器进入 `test_create_table` 方法，以便访问“*my-test-table*”。

从那里，我们可以编写一个测试来验证表是否成功创建，通过调用 Moto 的 *.describe_table()* 和 *.list_tables()* 方法，并断言表名是否存在于响应输出中。要运行测试，只需执行以下命令（如果需要，您可以添加额外的标志）：

```py
pytest
```

这个命令应该会返回测试通过的结果：

![](../Images/209a361b6ec8d0bf561bad0b9117c1d9.png)

终端截图 5：执行 `pytest` 命令以测试表的创建

## 使用 Moto 进一步测试 DynamoDB

现在，您的测试文件已准备好进行任何额外的 DynamoDB 测试方法，您可以使用 Moto 探索，例如向“*my-test-table*”添加项：

你看到的测试可以直接放在 TestDynamoDB 类中 `test_create_table` 方法下。`test_put_item` 方法也使用了上下文管理器，以使用最初在文件顶部创建的相同测试表。在每个方法开始时，以及在每个测试套件中调用上下文管理器时，“*my-test-table*” 都处于初次创建时的原始状态。*请注意：状态在测试方法之间不会持久化。*

我创建了一个 GitHub 存储库，其中包含了本文中的示例测试，并且还增加了更多内容！我的示例代码扩展了文章中的测试，包括以下 DynamoDB 测试：

+   创建表

+   将项添加到表中

+   从表中删除项

+   删除表

+   添加表标签

+   移除表标签

[](https://github.com/taylorwagner/moto-aws-data?source=post_page-----ae58f9e7b265--------------------------------) [## GitHub - taylorwagner/moto-aws-data

### 通过在 GitHub 上创建帐户来贡献 taylorwagner/moto-aws-data 的开发。

github.com](https://github.com/taylorwagner/moto-aws-data?source=post_page-----ae58f9e7b265--------------------------------)

这个存储库还包括一些 AWS RDS 的测试。在我的示例代码中，我将测试文件组织到自己的目录中，并对测试进行了分组以确保测试的一致性。我挑选了每个服务的不同方法来展示 Moto 提供的范围。Moto 还有许多其他方法未包含在我的示例代码中。请务必查看 Moto 的文档！

## 最终想法

将 Pytest 固件和 Python 上下文管理器结合使用，Pytest 的易用性和可扩展性框架与 Moto 的结合是验证 AWS 数据库服务使用情况的绝佳组合。尽管 Moto/Pytest 的组合可能不适合测试真实的 AWS 服务和账户，但它是练习和更好地理解如何测试真实 AWS 账户的足够选项，且不会带来安全漏洞、篡改重要数据或产生昂贵的、不必要的账单。只需记住，与 Boto3 相比，Moto 的方法范围有限。我发现，与 Moto 的模拟响应一起工作会为你提供利用 Boto3 处理真实 AWS 用例所需的理解。我希望我的文章和示例代码能帮助你满足 AWS 数据库的需求。

## 资源

+   Moto 的 GitHub 仓库：

[](https://github.com/spulec/moto?source=post_page-----ae58f9e7b265--------------------------------) [## GitHub - spulec/moto: 一个允许你轻松模拟基于 AWS 的测试的库…]

### Moto 是一个库，使你的测试可以轻松模拟 AWS 服务。假设你有以下的 Python 代码…

[GitHub 页面](https://github.com/spulec/moto?source=post_page-----ae58f9e7b265--------------------------------)

+   我的示例代码托管在 GitHub 上。你可以查看测试并在你的机器上运行。我还包括了 AWS RDS 测试：

[](https://github.com/taylorwagner/moto-aws-data?source=post_page-----ae58f9e7b265--------------------------------) [## GitHub - taylorwagner/moto-aws-data]

### 通过在 GitHub 上创建账户来为 taylorwagner/moto-aws-data 的开发做出贡献。

[GitHub 页面](https://github.com/taylorwagner/moto-aws-data?source=post_page-----ae58f9e7b265--------------------------------)

+   [Moto 文档主页](https://docs.getmoto.org/en/latest/docs/getting_started.html#recommended-usage)

+   [Boto3 主页](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)

+   [Moto 实现的服务列表](https://docs.getmoto.org/en/latest/docs/services/index.html)

+   [Moto 中可用的 DynamoDB Boto3 方法列表](https://docs.getmoto.org/en/latest/docs/services/dynamodb.html)

+   [Pytest 主页](https://docs.pytest.org/en/7.2.x/contents.html)

+   [Python 下载页面](https://www.python.org/downloads/)

+   [Pytest 固件](https://docs.pytest.org/en/6.2.x/fixture.html)

+   [Python 的上下文管理器](https://docs.python.org/3/library/contextlib.html)
