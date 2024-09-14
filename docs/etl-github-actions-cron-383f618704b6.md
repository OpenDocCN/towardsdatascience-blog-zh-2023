# 如何使用 GitHub Actions 构建简单的 ETL 流水线

> 原文：[`towardsdatascience.com/etl-github-actions-cron-383f618704b6`](https://towardsdatascience.com/etl-github-actions-cron-383f618704b6)

## ETL 不一定要复杂。如果是这样的话，使用 GitHub Actions。

[](https://thuwarakesh.medium.com/?source=post_page-----383f618704b6--------------------------------)![Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----383f618704b6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----383f618704b6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----383f618704b6--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----383f618704b6--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----383f618704b6--------------------------------) ·阅读时间 6 分钟·2023 年 5 月 5 日

--

![](img/a24a64f85f7450f2509f91f3562ef550.png)

照片由 [Roman Synkevych 🇺🇦](https://unsplash.com/@synkevych?utm_source=medium&utm_medium=referral) 拍摄，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

如果你对软件开发感兴趣，你会知道 GitHub Actions 是什么。它是 GitHub 提供的一个自动化开发任务的工具。或者用流行的话来说，是一个 DevOps 工具。

但人们很少用它来构建 ETL 流水线。

讨论 ETL 时，第一个想到的工具是 Airflow、Prefect 或相关工具。毫无疑问，它们在任务编排方面是市场上最好的。但我们构建的许多 ETL 都很简单，托管一个单独的工具往往是多余的。

你可以改用 GitHub Actions。

本文重点介绍 GitHub Actions。但如果你使用 Bitbucket 或 GitLab，你也可以使用它们各自的替代方案。

[](/black-with-git-hub-actions-4ffc5c61b5fe?source=post_page-----383f618704b6--------------------------------) ## 使用 Black 和 GitHub Actions 维护干净的 Python 代码。

### 没有人想要一个混乱的代码库；也很少有人有耐心去清理它。

towardsdatascience.com

我们可以在 GitHub Actions 上运行我们的 Python、R 或 Julia 脚本。所以作为数据科学家，你不必为此学习一种新的语言或工具。你甚至可以在任何 ETL 任务失败时收到电子邮件通知。

如果你使用免费账户，你每月仍然可以享受 2000 分钟的计算时间。如果你能估算你的 ETL 工作负载在这个范围内，你可以尝试使用 GitHub Actions。

[](/remove-file-from-git-history-ce404d7463d3?source=post_page-----383f618704b6--------------------------------) ## 如何移除你不小心上传到 GitHub 的敏感数据。

### 那么如何避免它们被误入呢？——一种出乎意料的简单处理方法。

[towardsdatascience.com

## 我们如何开始在 GitHub Actions 上构建 ETL？

开始使用 GitHub Actions 很简单。你可以参考[官方文档](https://docs.github.com/en/actions/quickstart)。或者按照以下三个简单步骤操作。

在你的代码库中，创建一个目录 `.github/workflows`。然后在其中创建一个名为 `actions.yaml` 的 YAML 配置文件，内容如下。

```py
name: ETL Pipeline

on:
  schedule:
    - cron: '0 0 * * *'  # Runs at 12.00 AM every day

jobs:
  etl:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: '3.9'

      - name: Extract data
        run: python extract.py

      - name: Transform data
        run: python transform.py

      - name: Load data
        run: python load.py
```

上述 YAML 自动化了一个 ETL（提取、转换、加载）管道。该工作流每天 UTC 时间凌晨 12:00 触发，由一个在 `ubuntu-latest` 环境中运行的单个任务组成（具体环境以当时可用的为准）。

这些配置步骤很简单。

该工作包含五个步骤：前两个步骤分别用于检查代码和设置 Python 环境，接下来的三个步骤按顺序执行 `extract.py`、`transform.py` 和 `load.py` Python 脚本。

该工作流提供了一种自动化和高效的方法，使用 GitHub Actions 每天提取、转换和加载数据。

[](/python-decorators-for-data-science-6913f717669a?source=post_page-----383f618704b6--------------------------------) ## 我在几乎所有数据科学项目中使用的 5 个 Python 装饰器

### 装饰器提供了一种新的、方便的方法，用于缓存到发送通知等各种用途。

[towardsdatascience.com

Python 脚本可能会根据场景有所不同。这是众多方法中的一种。

```py
# extract.py
# --------------------------------
import requests

response = requests.get("https://api.example.com/data")
with open("data.json", "w") as f:
    f.write(response.text)

# transform.py
# --------------------------------
import json

with open("data.json", "r") as f:
    data = json.load(f)

# Perform transformation
transformed_data = [item for item in data if item["key"] == "value"]

# Save transformed data
with open("transformed_data.json", "w") as f:
    json.dump(transformed_data, f)

# load.py
# --------------------------------
import json
from sqlalchemy import create_engine, Table, Column, Integer, String, MetaData

# Connect to database
engine = create_engine("postgresql://myuser:mypassword@localhost:5432/mydatabase")

# Create metadata object
metadata = MetaData()

# Define table schema
mytable = Table(
    "mytable",
    metadata,
    Column("id", Integer, primary_key=True),
    Column("column1", String),
    Column("column2", String),
)

# Read transformed data from file
with open("transformed_data.json", "r") as f:
    data = json.load(f)

# Load data into database
with engine.connect() as conn:
    for item in data:
        conn.execute(
            mytable.insert().values(column1=item["column1"], column2=item["column2"])
        )
```

上述脚本从虚拟 API 读取数据并将其推送到 Postgres 数据库。

[](/fast-load-data-to-sql-from-python-2d67aea946c0?source=post_page-----383f618704b6--------------------------------) ## Python 到 SQL — 我现在可以将数据加载速度提高 20 倍

### 上传大量数据的好方法、坏方法和丑陋的方法

[towardsdatascience.com

## 部署 ETL 管道到 GitHub Actions 时需要考虑的事项。

**1\. 安全性：** 使用 GitHub 的秘密存储来保护你的秘密，避免将秘密硬编码到你的工作流中。

你是否已经注意到我上面给出的示例代码包含了数据库凭据？这在生产系统中是不对的。

我们还有其他安全嵌入秘密的方法，如数据库凭据。

如果你在 GitHub Actions 中不加密你的秘密，任何有权限访问代码库的人都能看到这些秘密。这意味着，如果攻击者获得了对代码库的访问权限或代码库的源代码泄露；攻击者将能够看到你的秘密值。

为了保护你的机密信息，GitHub 提供了一项名为加密机密的功能，这使你能够在仓库设置中安全地存储你的机密值。加密机密仅对授权用户可访问，并且在你的 GitHub Actions 工作流中从不以明文形式暴露。

下面是它的工作原理。

在仓库设置侧边栏中，你可以找到 Actions 的机密信息和变量。你可以在这里创建你的变量。

![](img/2a69869f31b645e2075fffa99085e609.png)

作者截图。

在这里创建的机密信息对任何人都不可见。它们是加密的，可以在工作流中使用。即使你也无法读取它们。但你可以用新的值更新它们。

一旦你创建了机密信息，你可以通过 GitHub Actions 配置将它们作为环境变量传递。下面是它的工作原理：

```py
name: ETL Pipeline

on:
  schedule:
    - cron: '0 0 * * *'  # Runs at 12.00 AM every day

jobs:
  etl:
    runs-on: ubuntu-latest
    steps:
      ...

      - name: Load data
        env: # Or as an environment variable
          DB_USER: ${{ secrets.DB_USER }}
          DB_PASS: ${{ secrets.DB_PASS }}
        run: python load.py
```

现在，我们可以修改 Python 脚本以从环境变量中读取凭证。

```py
# load.py
# --------------------------------
import json
import os
from sqlalchemy import create_engine, Table, Column, Integer, String, MetaData

# Connect to database
engine = create_engine(
    f"postgresql://{os.environ['DB_USER']}:{os.environ['DB_PASS']}@localhost:5432/mydatabase"
)
```

**2. 依赖关系**：确保使用正确版本的依赖项，以避免任何问题。

你的 Python 项目可能已经有一个 requirements.txt 文件，指定了依赖项及其版本。或者，对于更复杂的项目，你可能使用像 Poetry 这样的现代依赖管理工具。

在运行 ETL 的其他部分之前，你应该有一个步骤来设置你的环境。你可以通过在 YAML 配置中指定以下内容来做到这一点。

```py
- name: Install dependencies
  run: pip install -r requirements.txt
```

3. **时区设置**：GitHub Actions 使用 UTC 时区，在撰写本文时，你不能更改它。

因此，你必须确保使用正确的时区。你可以使用在线转换工具或在配置之前手动将本地时间调整为 UTC。

GitHub Actions 调度的最大陷阱是其执行时间的不确定性。即使你已配置为在特定时间点运行，如果那时需求量很高，你的任务将被排队。因此，实际任务开始时间会有短暂的延迟。

如果你的任务依赖于准确的执行时间，使用 GitHub Actions 调度可能不是一个好的选择。在 GitHub Actions 中使用 [自托管运行器](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/about-self-hosted-runners) 可能会有所帮助。

**4. 资源使用：** 避免过载 GitHub 提供的资源。

即使 [GitHub Actions，即使是免费账户，也有 2000 分钟](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions) 的免费运行时间，但如果你使用的是与 Linux 不同的操作系统，规则会有所不同。

如果你使用的是 Windows 运行时，你只会获得一半的资源。在 MacOS 环境中，你只会获得十分之一的资源。

[](/github-automated-testing-python-fdfe5aec9446?source=post_page-----383f618704b6--------------------------------) ## 如何在每次提交时使用 GitHub Actions 运行 Python 测试？

### 自动化乏味的任务，并通过 CI 流水线确保你的代码质量。

towardsdatascience.com

## 结论

GitHub Actions 是一个 DevOps 工具，但我们可以用它来运行任何计划任务。在这篇文章中，我们讨论了如何创建一个定期获取 API 并将数据推送到数据框的 ETL。

对于简单的 ETL，这种方法易于开发和部署。

但是，GitHub Actions 中的计划任务不必在完全相同的时间运行。因此，对于时间敏感的任务，这种方法并不适用。

> 感谢阅读，朋友！在 [**LinkedIn**](https://www.linkedin.com/in/thuwarakesh/)、[**Twitter**](https://twitter.com/Thuwarakesh) 和 [**Medium**](https://thuwarakesh.medium.com/) 上跟我打个招呼吧。
> 
> 还不是 Medium 会员？请使用此链接 [**成为会员**](https://thuwarakesh.medium.com/membership)，因为这样你无需额外支付费用，我可以通过推荐你赚取少量佣金。
