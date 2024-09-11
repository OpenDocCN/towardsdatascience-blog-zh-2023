# ETL 的 Docker Compose：Meerschaum Compose

> 原文：[`towardsdatascience.com/the-docker-compose-of-etl-meerschaum-compose-777e0e7304d1?source=collection_archive---------15-----------------------#2023-06-19`](https://towardsdatascience.com/the-docker-compose-of-etl-meerschaum-compose-777e0e7304d1?source=collection_archive---------15-----------------------#2023-06-19)

![](img/29603b9f71a9d9ec6dd01e06d9c2f749.png)

图片由[CHUTTERSNAP](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral)拍摄，刊登于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

[](https://bmeares.medium.com/?source=post_page-----777e0e7304d1--------------------------------)![本尼特·米尔斯](https://bmeares.medium.com/?source=post_page-----777e0e7304d1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----777e0e7304d1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----777e0e7304d1--------------------------------) [本尼特·米尔斯](https://bmeares.medium.com/?source=post_page-----777e0e7304d1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff9b22e04ae69&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-docker-compose-of-etl-meerschaum-compose-777e0e7304d1&user=Bennett+Meares&userId=f9b22e04ae69&source=post_page-f9b22e04ae69----777e0e7304d1---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----777e0e7304d1--------------------------------) · 6 分钟阅读 · 2023 年 6 月 19 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F777e0e7304d1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-docker-compose-of-etl-meerschaum-compose-777e0e7304d1&user=Bennett+Meares&userId=f9b22e04ae69&source=-----777e0e7304d1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F777e0e7304d1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-docker-compose-of-etl-meerschaum-compose-777e0e7304d1&source=-----777e0e7304d1---------------------bookmark_footer-----------)

> 本文介绍了[**Meerschaum Compose**](https://meerschaum.io/reference/compose/)，这是一个用于在 YAML 中定义 ETL 流水线的工具，以及一个数据工程框架[Meerschaum](https://meerschaum.io/)的插件。

Docker 的出现彻底改变了我们设计、构建和运行云应用程序的方式。然而，开发者很快意识到其灵活性使得协作变得困难，因此 `docker-compose` 成为管理环境和多容器项目的首选工具。

类似地，这个一致环境的问题也出现在 ETL 框架 [Meerschaum](https://meercshaum.io) 中。随着越来越多的数据工程师使用该平台构建他们的管道，管道的动态特性意味着需要一个解决方案来提供项目级的隔离。

受到 Docker Compose 的启发，这个解决方案以一个名为 [Meerschaum Compose](https://meerschaum.io/reference/compose/) 的项目形式出现。我每天在工作和个人项目中使用 Compose 来构建和管理数据管道，今天我想展示如何使用 Compose 构建 ETL 项目。

# Compose 如何驯服引擎

权力越大，责任越大。在 Docker 的情况下，这种责任由一个名为`docker-compose.yml`的清单文件处理，该文件描述了应用程序内的服务应该如何运行。该文件充当了活文档，便于原型设计并向 CloudOps 描述预期环境。通过一个简单的标准，Docker Compose 通过提供一种方便的方式来标准化和共享多服务项目的环境，从而填补了开发过程中的空白。

**Meerschaum Compose** 服务于类似的目的：在`mrsm-compose.yml`文件中，你可以指定项目可能需要的一切：预期的 [环境](https://meerschaum.io/reference/environment/)、[插件](https://meerschaum.io/reference/plugins/)、[管道](https://meerschaum.io/reference/pipes/)和 [连接器](https://meerschaum.io/reference/connectors/)。

## 命令

当你遇到一个新的 Compose 项目（在下面的屏幕截图中使用 [Tech Slam ‘N Eggs 演示项目](https://github.com/bmeares/techslamneggs)）时，尝试这些命令来熟悉环境：

> **注意：** 如果你想在 Docker 容器中运行 Compose，请参见 [这个仓库](https://github.com/bmeares/mrsm-compose-template)。

`**mrsm compose run**`

注册管道并逐一同步它们。这对于确保执行顺序以及使用数据库更新管道参数非常有用。一个常见的模式是将不同阶段的 Compose 文件链接在一起，作为更大的 ETL 过程的一部分：

```py
mrsm compose run --file mrsm-compose-00-extract.yaml && \
mrsm compose run --file mrsm-compose-01-transform.yaml && \
mrsm compose run --file mrsm-compose-02-load.yaml
```

> **注意：** 命令`mrsm compose sync pipes`是并行执行的，最适用于 Compose 文件中的管道彼此独立的情况。

![](img/cd955aae03970a39cc952d87fcb61b12.png)

Compose 运行命令一次同步一个管道。

`**mrsm compose explain**`

解析 Compose 文件并打印定义管道的当前环境和状态。这在故障排除和理解项目结构时非常有用。

![](img/160134a0ed611d60d212a4e13e6ace5d.png)

Compose explain 命令打印定义管道的状态。

`**mrsm compose up --dry**`

注册并更新远程管道的参数。这隐含地作为`mrsm compose run`的一部分运行，并应在标准的 Meerschaum 操作（如`mrsm compose sync pipes`）之前运行。`--dry`标志可以防止同步作业运行。

> **注意：** Compose 将 [标记管道](https://meerschaum.io/reference/pipes/tags/) 与项目名称。如果在一个项目中使用多个 compose 文件，请设置键 `project_name`。

`**mrsm compose down -v**`

停止作业并删除管道。这类似于 `docker compose down -v`（即 `-v` 表示“卷”）。

`**mrsm compose <action>**`

从项目环境中执行标准的 Meerschaum 操作（例如 `sync pipes`、`delete pipes`、自定义操作）。我经常运行 `mrsm compose python` 以跳入项目环境中的 REPL。

每当你运行 `mrsm compose` 命令时，标志 `--tags {project_name}` 会被附加（除非被覆盖），以确保你只与项目中的管道进行交互。

![](img/1ca148ed2498fd40a4211df8da5c9489.png)

一个 Meerschaum Compose 项目的示例，用于天气数据的 ETL。

## 管道

谦逊的 [管道](https://meerschaum.io/reference/pipes/) 是 Meerschaum 对增量 ETL 的抽象。管道具有输入和输出 [连接器](https://meerschaum.io/reference/connectors/) 并存储 [参数](https://meerschaum.io/reference/pipes/#parameters) 来配置其同步过程的行为。这可以是简单的 SQL 查询，也可以包括用于插件的自定义键。

![](img/47b0e7e17488cdfa1760b18a6662361f.png)

上述 Compose 项目的管道通过 web UI 展示

由于管道的元数据与其表一起存储，它们易于编辑（无论是通过 `edit pipes` 还是在 web UI 上），这有助于原型设计。但这种动态特性引入了本文开头描述的同样问题：为了扩展开发，需要一个 Compose 文件来定义项目组件，以便于版本控制。

根据 [Meerschaum Compose 规范](https://meerschaum.io/reference/compose/#-schema)，管道在 `sync:pipes` 键下的列表中定义。每个项目定义了构建管道所需的键和参数，就像你期望数据库中的管道所反映的蓝图。

例如，以下代码片段将定义一个管道，该管道会将一个 `weather` 表从远程 PostgreSQL 数据库（在下面定义为 `sql:source`）同步到本地 SQLite 文件（在本项目中定义为 `sql:dest`）。

```py
sync:
  pipes:
    - connector: "sql:source"
      metric: "weather"
      target: "weather"
      columns:
        datetime: "timestamp"
        station: "station"
      parameters:
        fetch:
          backtrack_minutes: 1440
        query: |-
          SELECT timestamp, station, temperature
          FROM weather

config:
  meerschaum:
    instance: "sql:dest"
    connectors:
      sql:
        source: "postgresql://user:pass@host:5432/db"
        dest: "sqlite:////tmp/dest.db"
```

这个示例将使用日期时间轴 `timestamp` 进行范围边界（向回追溯 1 天）逐步更新一个名为 `weather` 的表，这一列加上 ID 列 `station` 一起构成一个复合主键，用于去重。

> URI 是字面上写的示例；如果你正在提交一个 compose 文件，请引用环境变量（例如 `$SECRET_URI`）或你的主 Meerschaum 配置（例如 `MRSM{meerschaum:connectors:sql:source}`）。

## 连接器

首先，简单回顾一下[Meerschaum 连接器](https://meerschaum.io/reference/connectors/)：你可以通过几种方式定义连接器，其中最受欢迎的是通过[环境变量](https://meerschaum.io/reference/connectors/#-environment-connectors)。假设你在环境文件中定义了连接秘密：

```py
export MRSM_SQL_REMOTE='postgresql://user:pass@host:5432/db'
export MRSM_FOO_BAR='{
    "user": "abc",
    "password": "def"
}'
```

第一个环境变量`MRSM_SQL_REMOTE`定义了连接器`sql:remote`。如果你引入了这个文件，你可以通过命令`mrsm show connectors sql:remote`来验证这个连接器。

第二个变量是如何定义自定义`FooConnector`的示例，你可以通过插件中的`@make_connector`装饰器来创建它。自定义连接器是一个强大的工具，但现在，这里是基本结构：

```py
from meerschaum.connectors import make_connector, Connector

@make_connector
class FooConnector(Connector):
    REQUIRED_ATTRIBUTES = ['username', 'password']

    def fetch(pipe, **kwargs):
        docs = []
        return docs
```

所以我们刚刚回顾了如何在主机环境中定义连接器。让我们看看如何在 Meerschaum 项目中使这些主机连接器可用。在 Compose 文件中，我们项目所需的所有连接器都在`config:meerschaum:connectors`下定义。使用`MRSM{}`语法来引用主机环境中的键，并将它们传递到项目中。

```py
config:
  meerschaum:
    instance: "sql:app"
    connectors:
      sql:
        app: MRSM{meerschaum:connectors:sql:remote}
      foo:
        bar: MRSM{meerschaum:connectors:foo:bar}
```

## 插件

Meerschaum 可以通过[插件](https://meerschaum.io/reference/plugins/)进行轻松扩展，这些插件是 Python 模块。插件可以获取数据、实现自定义连接器和/或扩展 Meerschaum（例如，自定义操作、标志、API 端点等）。

> Meerschaum 支持多个插件目录（通过`MRSM_PLUGINS_DIR`），可以在`mrsm-compose.yaml`中的`plugins_dir`键下设置（默认是`plugins`目录）。

将你的插件存储在 Compose 项目中，可以清楚地说明你期望插件的使用方式。例如，[MongoDBConnector 项目中的 Compose 文件](https://github.com/bmeares/mongodb-connector/blob/main/mrsm-compose.yaml)演示了自定义连接器如何同时作为连接器和实例使用。

## 包管理

当你第一次开始使用 Meerschaum Compose 时，首先会注意到它会开始安装大量的 Python 包。不要担心你的环境——所有东西都安装在你项目的`root`子目录中的虚拟环境中（有点讽刺，对吧？）。你可以用`mrsm compose init`安装插件的依赖项。

为了在项目之间共享包，请在`mrsm-compose.yml`中设置`root_dir`键为新的路径。删除这个`root`目录将有效卸载 Compose 下载的所有包，同时保持你的主机环境不变。

# 结论

Meerschaum Compose 已经成为我构建中型 ETL 项目的首选工具。它的工作流程类似于[Meltano](https://meltano.com/)或[dbt](https://www.getdbt.com/)，但入门门槛更低，对 ETL 过程的控制更为动态。这是一种组织插件、连接器和管道的整洁工作流程，非常适合团队合作。

你可以通过[Meerschaum Compose 模板仓库](https://github.com/bmeares/mrsm-compose-template)快速启动一个新项目——查看[MongoDBConnector 插件](https://github.com/bmeares/mongodb-connector)或[Tech Slam ‘N Eggs 演示](https://github.com/bmeares/techslamneggs)以获取实际示例。

> 随意将你的项目添加到[Awesome Meerschaum 列表](https://github.com/bmeares/awesome-meerschaum)！
