# 如何使用 Python 列出所有 BigQuery 数据集和表

> 原文：[`towardsdatascience.com/list-bigquery-ds-tables-python-b92063ad0be3`](https://towardsdatascience.com/list-bigquery-ds-tables-python-b92063ad0be3)

## 使用 BigQuery API 和 Python 程序化列出所有数据集和表

[](https://gmyrianthous.medium.com/?source=post_page-----b92063ad0be3--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----b92063ad0be3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b92063ad0be3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b92063ad0be3--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----b92063ad0be3--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b92063ad0be3--------------------------------) ·5 分钟阅读·2023 年 5 月 15 日

--

![](img/c7fe779b864a72382bc66ab76d39ab86.png)

图片由 [Joanna Kosinska](https://unsplash.com/@joannakosinska?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/PbgY3ptgA4A?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

BigQuery 是 Google Cloud Platform 上的托管数据仓库服务，允许用户存储、管理和查询数据。数据和/或分析工程的一个重要部分是某些任务的自动化，包括与 BigQuery 的一些交互。这些自动化通常需要我们利用 BigQuery API，使开发人员能够以编程方式与 GCP 上的服务进行交互。

在今天的文章中，我们将演示如何使用 BigQuery API 和 Google Python 客户端以编程方式获取单个 GCP 项目下的所有表和数据集。

## 前提条件

为了跟随本教程中的步骤，确保安装 Google 提供的 Python 客户端库。为此，我们需要通过 `pip` 安装 `google-cloud-bigquery`：

```py
$ python3 -m pip install --upgrade google-cloud-bigquery
```

此外，你需要通过应用默认凭据（ADC）进行客户端身份验证。为此，你首先需要确保已经[安装](https://cloud.google.com/sdk/gcloud) `[gcloud](https://cloud.google.com/sdk/gcloud)` [命令行界面（CLI）](https://cloud.google.com/sdk/gcloud)。由于我们在本地机器上开发代码，我们只需通过 ADC 进行身份验证，如下所述：

```py
$ gcloud auth application-default login
```

运行命令后，你的浏览器将打开一个新标签页，要求你使用 Google 帐号登录。请确保你的个人 Google Cloud 帐号具有足够的权限以执行接下来几个部分中的操作。

## 列出所有 BigQuery 数据集

首先，让我们开始创建一个 BigQuery 客户端实例：

```py
from google.cloud import bigquery

# The client we'll use to interact with BigQuery
client = bigquery.Client(project='my-gcp-project')
```

我们现在可以使用客户端通过调用 `list_datasets()` 方法来获取所有 BigQuery 数据集，该方法返回一个指向类型为 `[DaasetListItem](https://cloud.google.com/python/docs/reference/bigquery/latest/google.cloud.bigquery.dataset.DatasetListItem)` 对象的迭代器。

```py
# Get an iterator pointing to `DaasetListItem` objects
bq_datasets = list(client.list_datasets())
```

现在我们可以遍历 `bq_datasets` 对象并打印我们在实例化客户端时使用的项目的 BigQuery 数据集。

```py
# Print BigQuery datasets in project `my-gcp-project` 
for dataset in bq_datasets:
  print(f'{client.project=}, {dataset.dataset_id=}')
```

这是代码的完整版本，结合了我们之前讨论的所有代码片段。

```py
"""
Script used to iterate over BigQuery datasets in a single 
BigQuery project.
"""
from google.cloud import bigquery

# The client we'll use to interact with BigQuery
client = bigquery.Client(project='my-gcp-project')

# Get an iterator pointing to `DaasetListItem` objects
bq_datasets = client.list_datasets()

# Print BigQuery datasets in project `my-gcp-project`
for dataset in bq_datasets:
  print(f'{client.project=}, {dataset.dataset_id=}')
```

最后，这是我们脚本生成的示例输出：

```py
client.project='my-gcp-project', dataset.dataset_id='my_dataset'
client.project='my-gcp-project', dataset.dataset_id='another_dataset'
client.project='my-gcp-project', dataset.dataset_id='oh_heres_another_one'
```

## 列出所有 BigQuery 表

同样，为了列出感兴趣的 BigQuery 项目中的所有表，我们将再次迭代数据集，然后列出每个数据集中的所有表。

为此，我们需要对每个数据集调用 `client.list_tables()`，以获取指向类型为 `[TableListItem](https://cloud.google.com/python/docs/reference/bigquery/latest/google.cloud.bigquery.table.TableListItem)` 对象的迭代器。

```py
for table in client.list_tables(dataset.dataset_id):
    print(f'{table.table_id=}, {dataset.dataset_id=}, {client.project=}')
```

这是包含所有步骤的完整代码，用于将 BigQuery 项目中所有表的名称打印到标准输出。

```py
"""
Script used to iterate over BigQuery table names for every single 
dataset in a particular BigQuery project.
"""
from google.cloud import bigquery

# The client we'll use to interact with BigQuery
client = bigquery.Client(project='my-gcp-project')

# Get an iterator pointing to `DaasetListItem` objects
bq_datasets = client.list_datasets()

# Print BigQuery table and dataset names in project `my-gcp-project`
for dataset in bq_datasets:
  for table in client.list_tables(dataset.dataset_id):
    print(f'{table.table_id=}, {dataset.dataset_id=}, {client.project=}')
```

**输出：**

```py
table.table_id='my_table', dataset.dataset_id='my_dataset', client.project='my-gcp-project'
table.table_id='another_table', dataset.dataset_id='my_dataset', client.project='my-gcp-project'
table.table_id='temp_table', dataset.dataset_id='temp_ds', client.project='my-gcp-project'
table.table_id='temp_table_2', dataset.dataset_id='temp_ds', client.project='my-gcp-project'
```

## 计算 BigQuery 中的数据集和表的数量

现在您知道如何使用 Python 客户端与 BigQuery 交互，您还可以推断出单个项目中的数据集数量。

```py
"""
Script used to count number of datasets for a BigQuery project
"""
from google.cloud import bigquery

# The client we'll use to interact with BigQuery
client = bigquery.Client(project='my-gcp-project')

# Turn iterator into list, and count its length
dataset_count = len(list(client.list_datasets()))

print(f'Number of datasets in project {client.project}: {dataset_count}')
```

同样，我们甚至可以计算每个数据集或每个项目中的表数量：

```py
"""
Script used to count number of tables per dataset for a BigQuery project
"""
from google.cloud import bigquery

# The client we'll use to interact with BigQuery
client = bigquery.Client(project='my-gcp-project')

# Turn iterator into list, and count its length
table_total_count = 0

for dataset in client.list_datasets():
  table_count = len(list(client.list_tables(dataset.dataset_id)))
  table_total_count += table_count
  print(f'No. of tables for dataset {dataset.dataset_id}: {table_count}')

print(f'Number of tables in project {client.project}: {table_total_count}')
```

## 最终想法

在这个简短的教程中，我们概述了使用 Google 提供的 Python 客户端来利用 BigQuery API 的步骤，包括安装和身份验证步骤。

此外，我们演示了如何使用 BigQuery Python 客户端来编程列出 BigQuery GCP 项目中的数据集和表。最后，我们还创建了一些脚本，您可以使用这些脚本来计算数据集或表的数量。

希望您觉得这篇文章有用。如果您在运行教程中分享的代码时遇到任何问题，请在评论中告知我，我会尽力帮助您调试并成功运行代码。

👉 [**成为会员**](https://gmyrianthous.medium.com/membership) **并阅读 Medium 上的每个故事。您的会员费直接支持我和您阅读的其他作者。您还将完全访问 Medium 上的每个故事。**

[](https://gmyrianthous.medium.com/membership?source=post_page-----b92063ad0be3--------------------------------) [## 使用我的推荐链接加入 Medium — Giorgos Myrianthous

### 作为 Medium 会员，您的会员费用的一部分会用于支持您阅读的作者，并且您可以完全访问每一个故事……

gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership?source=post_page-----b92063ad0be3--------------------------------)

👇**相关的文章您可能也会喜欢** 👇

[](/dbt-55b35c974533?source=post_page-----b92063ad0be3--------------------------------) ## 什么是 dbt（数据构建工具）

### 对 dbt 的温和介绍，它正在主导数据世界

towardsdatascience.com [](/bigquery-anti-patterns-dacb61f8a3f?source=post_page-----b92063ad0be3--------------------------------) ## BigQuery 的 SQL 反模式

### 在 Google Cloud BigQuery 上运行 SQL 时的最佳实践和应避免的事项

towardsdatascience.com [](/pyproject-python-9df8cc092f61?source=post_page-----b92063ad0be3--------------------------------) ## 什么是 Python 中的 pyproject.toml

### 在 pyproject.toml 文件中管理 Python 项目的依赖关系

towardsdatascience.com
