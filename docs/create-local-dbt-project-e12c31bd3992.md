# 创建一个本地 dbt 项目

> 原文：[`towardsdatascience.com/create-local-dbt-project-e12c31bd3992`](https://towardsdatascience.com/create-local-dbt-project-e12c31bd3992)

## 如何使用 Docker 创建一个带有虚拟数据的本地 dbt 项目以用于测试

[](https://gmyrianthous.medium.com/?source=post_page-----e12c31bd3992--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----e12c31bd3992--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e12c31bd3992--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e12c31bd3992--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----e12c31bd3992--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e12c31bd3992--------------------------------) ·阅读时间 7 分钟·2023 年 1 月 12 日

--

![](img/481b848f924c744cbd3a67c44800e6fb.png)

照片由 [Daniel K Cheung](https://unsplash.com/@danielkcheung?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，来源于 [Unsplash](https://unsplash.com/photos/ZqqlOZyGG7g?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

dbt（数据构建工具）是数据工程和分析领域最热门的技术之一。最近，我一直在处理一个任务，该任务对 dbt 生成的产物进行一些后处理，并且想编写一些测试。为此，我不得不创建一个可以在本地（或在 docker 容器中）运行的示例项目，以便我不必与实际的数据仓库进行交互。

在这篇文章中，我们将逐步介绍创建 dbt 项目并将其与容器化的 Postgres 实例连接的过程。你可以使用这样的项目进行测试，甚至可以用来尝试 dbt 本身的功能，或者练习你的技能。

[**订阅数据管道**](https://thedatapipeline.substack.com/welcome)**，这是一个专注于数据工程的新闻通讯**

## 步骤 1：创建一个 dbt 项目

我们将向 Postgres 数据库中填充一些数据，因此我们首先需要从 PyPI 安装 dbt Postgres 适配器：

```py
pip install dbt-postgres==1.3.1
```

请注意，命令还会安装 `dbt-core` 包以及运行 dbt 所需的其他依赖项。

现在，让我们继续创建一个 dbt 项目——为此，我们可以通过在终端中运行 `dbt init` 命令来初始化一个新的 dbt 项目：

```py
dbt init test_dbt_project
```

然后系统会提示你选择要使用的数据库（根据你本地安装的适配器，你可能会看到不同的选项）：

```py
16:43:08  Running with dbt=1.3.1
Which database would you like to use?
[1] postgres

(Don't see the one you want? https://docs.getdbt.com/docs/available-adapters)

Enter a number: 1
```

确保输入与 Postgres 适配器对应的数字，如输出列表中所示。现在，`init` 命令应该在你执行的目录中创建了以下基本结构：

![](img/77dcaab10f55daf3e04ee716be8e6f9f.png)

由 `dbt init` 命令创建的 dbt 项目结构 — 来源：作者

## 步骤 2：创建一个 Docker Compose 文件

现在让我们创建一个 `docker-compose.yml` 文件（将文件放在与 `test_dbt_project` 目录相同的级别），在该文件中我们将指定两个服务 — 一个对应于现成的 Postgres 镜像，另一个对应于我们将在下一步的 `Dockerfile` 中定义的 `dbt` 镜像：

```py
version: "3.9"

services:
  postgres:
    container_name: postgres
    image: frantiseks/postgres-sakila
    ports:
      - '5432:5432'
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 5s
      retries: 5
  dbt:
    container_name: dbt
    build: .
    image: dbt-dummy
    volumes:
      - ./:/usr/src/dbt
    depends_on:
      postgres:
        condition: service_healthy
```

正如你所见，对于 Postgres 容器，我们将使用一个名为 `frantiseks/postgres-sakila` 的镜像，该镜像在 Docker Hub 上公开可用。此镜像将填充 Postgres 实例上的 Sakila 数据库。该数据库模型化了一个 DVD 租赁店，由多个表组成，这些表经过规范化并对应于电影、演员、客户和付款等实体。在接下来的几个部分中，我们将利用这些数据来构建一些示例 dbt 数据模型。

第二个服务，称为 `dbt`，将创建一个我们将构建数据模型的环境。请注意，我们将当前目录挂载到 Docker 容器中。这将使容器能够访问我们对数据模型所做的任何更改，而无需重新构建镜像。此外，任何由 dbt 命令生成的元数据（如 `manifet.json`）将在主机机器上立即出现。

## 步骤 3：创建一个 Dockerfile

现在，让我们指定一个将用于构建镜像的`Dockerfile`，在这个镜像上运行的容器将构建我们示例 dbt 项目中指定的模型。

```py
FROM python:3.10-slim-buster

RUN apt-get update \
    && apt-get install -y --no-install-recommends

WORKDIR /usr/src/dbt/dbt_project

# Install the dbt Postgres adapter. This step will also install dbt-core
RUN pip install --upgrade pip
RUN pip install dbt-postgres==1.3.1

# Install dbt dependencies (as specified in packages.yml file)
# Build seeds, models and snapshots (and run tests wherever applicable)
CMD dbt deps && dbt build --profiles-dir profiles && sleep infinity
```

注意，在最后一个 `CMD` 命令中，我们故意添加了一个额外的 `&& sleep infinity` 命令，以使容器在运行完 `Dockerfile` 中指定的步骤后不会退出，这样我们就可以访问容器并运行额外的 dbt 命令（如果需要）。

## 步骤 4：为 Postgres 数据库创建一个 dbt 配置文件

既然我们已经为主机机器创建了所需的基础设施，以创建一个 Postgres 数据库、填充一些虚拟数据以及创建一个用于 dbt 环境的镜像，让我们专注于 dbt 部分。

我们首先需要创建一个 dbt 配置文件，用于与目标 Postgres 数据库交互。在 `test_dbt_project` 目录内，创建另一个名为 `profiles` 的目录，然后创建一个名为 `profiles.yml` 的文件，内容如下：

```py
test_profile:
  target: dev
  outputs:
    dev:
      type: postgres
      host: postgres
      user: postgres
      password: postgres
      port: 5432
      dbname: postgres
      schema: public
      threads: 1
```

## 步骤 5：定义一些数据模型

下一步是根据 Postgres 容器填充的 Sakila 数据创建一些数据模型。如果你计划将这个项目用于测试目的，我建议你至少创建一个种子数据，一个模型和一个快照（如果可能的话带有测试），以便全面覆盖所有 dbt 实体（不包括宏）。

我已经创建了一些数据模型、种子数据和快照，你可以在这个仓库中访问它们。

[](https://github.com/gmyrianthous/dbt-dummy/tree/main/dbt_project?source=post_page-----e12c31bd3992--------------------------------) [## dbt-dummy/dbt_project at main · gmyrianthous/dbt-dummy

### 你现在无法执行该操作。你在另一个标签页或窗口中登录了。你在另一个标签页中注销了...

github.com](https://github.com/gmyrianthous/dbt-dummy/tree/main/dbt_project?source=post_page-----e12c31bd3992--------------------------------)

## 第 6 步：运行 Docker 容器

现在我们已经拥有了启动 `docker-compose.yml` 文件中指定的两个 Docker 容器，并构建我们示例 dbt 项目中定义的数据模型所需的一切。

首先，构建镜像：

```py
docker-compose build
```

现在让我们启动运行的容器：

```py
docker-compose up
```

这个命令应该已经使用 Sakila 数据库初始化了一个 Postgres 数据库，并创建了指定的 dbt 模型。现在，让我们确保你有两个正在运行的容器：

```py
docker ps
```

应该输出包括一个名为 `dbt` 的容器和另一个名为 `postgres` 的容器。

## 第 7 步：查询 Postgres 数据库上的模型

要访问 Postgres 容器，首先需要推断容器 id。

```py
docker ps
```

然后运行：

```py
docker exec -it <container-id> /bin/bash
```

然后我们需要使用 `psql`，这是一个命令行接口，可以访问 Postgres 实例：

```py
psql -U postgres
```

如果你已经使用了我在前面部分分享的数据模型，现在可以使用以下查询来查询在 Postgres 上创建的每个模型。

```py
-- Query seed tables
SELECT * FROM customer_base;

-- Query staging views
SELECT * FROM stg_payment;

-- Query intermediate views
SELECT * FROM int_customers_per_store;
SELECT * FROM int_revenue_by_date;

-- Query mart tables
SELECT * FROM cumulative_revenue;

-- Query snapshot tables
SELECT * FROM int_stock_balances_daily_grouped_by_day_snapshot;
```

## 第 8 步：创建额外的或修改现有的模型

如前所述，`Dockerfile` 和 `docker-compose.yml` 文件被编写成 dbt 容器仍然会保持运行。因此，无论何时你修改或创建数据模型，你仍然可以使用该容器重新构建种子数据、模型、快照和/或测试。

要做到这一点，首先推断 `dbt` 容器的 id：

```py
docker ps
```

然后通过运行以下命令进入正在运行的容器：

```py
docker exec -it <container-id> /bin/bash
```

最后运行你希望的任何 dbt 命令，这取决于你对示例 dbt 项目所做的修改。以下是这些用途最常用命令的快速参考：

```py
# Install dbt deps
dbt deps

# Build seeds
dbt seeds --profiles-dir profiles

# Build data models
dbt run --profiles-dir profiles

# Build snapshots
dbt snapshot --profiles-dir profiles

# Run tests
dbt test --profiles-dir profiles
```

## 如何获取本教程的完整代码。

我创建了一个名为 `dbt-dummy` 的 GitHub 仓库，其中包含了你需要的所有部分，以便快速创建一个使用 Postgres 的容器化 dbt 项目。你可以通过以下链接访问它。

[](https://github.com/gmyrianthous/dbt-dummy?source=post_page-----e12c31bd3992--------------------------------) [## GitHub - gmyrianthous/dbt-dummy: 这是一个虚拟的 dbt（数据构建工具）项目，你可以用于…

### 这是一个虚拟的 dbt（数据构建工具）项目，你可以用它来填充 dbt seeds、模型、快照和测试……

[github.com](https://github.com/gmyrianthous/dbt-dummy?source=post_page-----e12c31bd3992--------------------------------)

这个项目也可以在[官方 dbt 文档的示例项目部分](https://docs.getdbt.com/faqs/project/example-projects)找到！

## 最终思考

在今天的教程中，我们逐步讲解了如何在本地机器上使用 Docker 创建 dbt 项目。我们构建了两个镜像，一个用于 Postgres 数据库，同时填充了 Sakila 数据库，另一个用于我们的 dbt 环境。

能够快速构建一些示例项目是很重要的，这些项目可以作为测试环境，甚至是实验和学习的游乐场。

[**订阅 Data Pipeline**](https://thedatapipeline.substack.com/welcome)**，这是一个专注于数据工程的新闻通讯**

**相关的文章你可能也会喜欢**

towardsdatascience.com ## 如何构建你的 dbt 项目和数据模型

### 对 dbt 数据模型实施有意义的结构

[towardsdatascience.com [](/staging-intermediate-mart-models-dbt-2a759ecc1db1?source=post_page-----e12c31bd3992--------------------------------) ## dbt 中的 Staging vs Intermediate vs Mart 模型

### 了解在数据构建工具（dbt）背景下，staging、intermediate 和 mart 模型的目的

[towardsdatascience.com [](/install-dbt-1bd6a4259a14?source=post_page-----e12c31bd3992--------------------------------) ## 如何安装 dbt（数据构建工具）

### 为你的特定数据仓库安装数据构建工具

[towardsdatascience.com
