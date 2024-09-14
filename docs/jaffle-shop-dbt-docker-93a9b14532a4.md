# 在 Docker 中运行 Jaffle Shop dbt 项目

> 原文：[`towardsdatascience.com/jaffle-shop-dbt-docker-93a9b14532a4`](https://towardsdatascience.com/jaffle-shop-dbt-docker-93a9b14532a4)

## 流行的 Jaffle Shop dbt 项目的容器化版本

[](https://gmyrianthous.medium.com/?source=post_page-----93a9b14532a4--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----93a9b14532a4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----93a9b14532a4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----93a9b14532a4--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----93a9b14532a4--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----93a9b14532a4--------------------------------) ·8 分钟阅读·2023 年 4 月 28 日

--

![](img/90bb3052f4f1d0f374def43154a20afe.png)

[Ryan Howerter](https://unsplash.com/@rhowerter?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)的照片，来自[Unsplash](https://unsplash.com/photos/JXIFjYVbAS8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果你是数据构建工具（dbt）的新手，你可能已经遇到过所谓的 Jaffle Shop，这是一个用于测试的项目。

> `jaffle_shop` 是一个虚构的电子商务商店。这个 dbt 项目将应用数据库中的原始数据转换成一个适合分析的客户和订单模型。
> 
> - [Jaffle Shop GitHub 项目](https://github.com/dbt-labs/jaffle_shop)

我观察到的 Jaffle Shop 项目的一个根本问题是，它期望用户（可能是 dbt 的新手）配置并托管一个本地数据库，以便 dbt 模型能够生成。

在本教程中，我将演示如何使用 Docker 创建项目的容器化版本。这将使我们能够部署一个 Postgres 实例，并将 dbt 项目配置为从该数据库读取和写入。我还会提供一个我创建的 GitHub 项目链接，它将帮助你迅速启动所有服务。

[**订阅数据管道**](https://thedatapipeline.substack.com/welcome)**，这是一个专注于数据工程的新闻通讯**

## 创建 Dockerfile 和 docker-compose.yml

让我们开始定义我们想要通过 Docker 运行的服务。首先，我们将创建一个`[docker-compose.yml](https://github.com/gmyrianthous/jaffle_shop/blob/main/docker-compose.yml)`文件，在其中定义两个服务。第一个服务是 Postgres 数据库，第二个服务是我们将在下一步中使用 Dockerfile 创建的自定义服务。

```py
# docker-compose.yml

version: "3.9"

services:
  postgres:
    container_name: postgres
    image: postgres:15.2-alpine
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
    ports:
      - 5432
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 5s
      retries: 5
  dbt:
    container_name: dbt
    build: .
    image: dbt-jaffle-shop
    volumes:
      - ./:/usr/src/dbt
    depends_on:
      postgres:
        condition: service_healthy
```

该文件指定了使用的 Docker Compose 版本（3.9 版）。它定义了两个服务，`postgres` 和 `dbt`，每个服务都有自己的设置。

`postgres`服务基于官方的`postgres` Docker 镜像版本 15.2-alpine。它将容器名称设置为`postgres`，将端口 5432（Postgres 的默认端口）映射到主机，并为 Postgres 用户和密码设置环境变量。`healthcheck`部分指定了一个命令来测试容器是否健康，并设置了检查的超时时间和重试次数。

`dbt`服务指定了一个`dbt`容器，该容器使用当前目录的 Docker 镜像（使用 Dockerfile）。它将当前目录挂载为容器中的一个卷，并指定它依赖于`postgres`服务，并且只有在`postgres`服务健康时才会启动。

为了容器化 Jaffle Shop 项目，我们需要创建一个`[Dockerfile](https://github.com/gmyrianthous/jaffle_shop/blob/main/Dockerfile)`，该文件安装 Python 和 dbt 所需的依赖项，并确保环境设置完毕后容器保持活动状态。

```py
# Dockerfile

FROM --platform=linux/amd64 python:3.10-slim-buster

RUN apt-get update \
    && apt-get install -y --no-install-recommends

WORKDIR /usr/src/dbt

# Install the dbt Postgres adapter. This step will also install dbt-core
RUN pip install --upgrade pip
RUN pip install dbt-postgres==1.2.0
RUN pip install pytz

# Install dbt dependencies (as specified in packages.yml file)
# Build seeds, models and snapshots (and run tests wherever applicable)
CMD dbt deps && dbt build --profiles-dir ./profiles && sleep infinity
```

## 配置 Postgres 与 dbt

要与 dbt 交互，我们将使用 dbt 命令行界面（CLI）。包含`[dbt_project.yml](https://github.com/gmyrianthous/jaffle_shop/blob/main/dbt_project.yml)`文件的目录被 dbt CLI 视为一个 dbt 项目。

我们将创建一个并指定一些基本配置，如 dbt 项目名称和要使用的`profile`（我们将在下一步中创建）。此外，我们将指定包含各种 dbt 实体的路径，并提供关于它们的物化配置。

```py
# dbt_project.yml

name: 'jaffle_shop'

config-version: 2
version: '0.1'

profile: 'jaffle_shop'

model-paths: ["models"]
seed-paths: ["seeds"]
test-paths: ["tests"]
analysis-paths: ["analysis"]
macro-paths: ["macros"]

target-path: "target"
clean-targets:
    - "target"
    - "dbt_modules"
    - "logs"

require-dbt-version: [">=1.0.0", "<2.0.0"]

models:
  jaffle_shop:
      materialized: table
      staging:
        materialized: view
```

现在`[profiles.yml](https://github.com/gmyrianthous/jaffle_shop/blob/main/profiles/profiles.yml)`文件用于存储 dbt 配置文件。一个配置文件包含多个目标，每个目标指定了数据库或数据仓库的连接详细信息和凭据。

```py
# profiles.yml

jaffle_shop:
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

该文件定义了一个名为`jaffle_shop`的配置文件，指定了运行在名为`postgres`的 Docker 容器上的 Postgres 数据库的连接详细信息。

+   `jaffle_shop`：这是配置文件的名称。它是用户为了识别配置文件而选择的任意名称。

+   `target: dev`：这指定了配置文件的默认目标，在这种情况下名为`dev`。

+   `outputs`：这一部分列出了配置文件的输出配置，默认的输出配置名为`dev`。

+   `dev`：这指定了`dev`目标的连接详细信息，该目标使用 Postgres 数据库。

+   `type: postgres`：这指定了输出的类型，在这种情况下是 Postgres 数据库。

+   `host: postgres`：这指定了 Postgres 数据库服务器的主机名或 IP 地址。

+   `user: postgres`：这指定了用于连接到 Postgres 数据库的用户名。

+   `password: postgres`：这指定了用于认证 Postgres 数据库的密码。

+   `port: 5432`：这指定了 Postgres 数据库监听的端口号。

+   `dbname: postgres`：这指定了要连接的 Postgres 数据库的名称。

+   `schema: public`：这指定了在对数据库执行查询时要使用的模式名称。

+   `threads: 1`：这指定了在运行 dbt 任务时要使用的线程数。

## Jaffle Shop dbt 模型和 seeds

Jaffle Shop 项目的源数据包括客户、支付和订单的 csv 文件。在 dbt 中，我们可以通过 [seeds](https://github.com/gmyrianthous/jaffle_shop/tree/main/seeds) 将这些数据加载到数据库中。然后，我们利用这些源数据在其基础上构建 [dbt 模型](https://github.com/gmyrianthous/jaffle_shop/tree/main/models)。

这是一个示例模型，它生成一些 [客户的指标](https://github.com/gmyrianthous/jaffle_shop/blob/main/models/customers.sql)：

```py
with customers as (

    select * from {{ ref('stg_customers') }}

),

orders as (

    select * from {{ ref('stg_orders') }}

),

payments as (

    select * from {{ ref('stg_payments') }}

),

customer_orders as (

        select
        customer_id,

        min(order_date) as first_order,
        max(order_date) as most_recent_order,
        count(order_id) as number_of_orders
    from orders

    group by customer_id

),

customer_payments as (

    select
        orders.customer_id,
        sum(amount) as total_amount

    from payments

    left join orders on
         payments.order_id = orders.order_id

    group by orders.customer_id

),

final as (

    select
        customers.customer_id,
        customers.first_name,
        customers.last_name,
        customer_orders.first_order,
        customer_orders.most_recent_order,
        customer_orders.number_of_orders,
        customer_payments.total_amount as customer_lifetime_value

    from customers

    left join customer_orders
        on customers.customer_id = customer_orders.customer_id

    left join customer_payments
        on  customers.customer_id = customer_payments.customer_id

)

select * from final
```

## 通过 Docker 运行服务

现在让我们构建并启动我们的 Docker 服务。为此，我们只需运行以下命令：

```py
$ docker-compose build
$ docker-compose up
```

上述命令将运行一个 Postgres 实例，然后构建 Jaffle Shop 的 dbt 资源，如仓库中所述。这些容器将保持运行，以便你可以：

+   查询 Postgres 数据库及从 dbt 模型创建的表

+   通过 dbt CLI 运行进一步的 dbt 命令

## 通过 CLI 运行 dbt 命令

dbt 容器已经构建了指定的模型。然而，我们仍然可以访问容器并通过 dbt CLI 运行 dbt 命令，无论是对于新模型还是修改过的模型。为此，我们需要首先访问容器。

以下命令将列出所有活动的容器：

```py
$ docker ps
```

复制 `dbt` 容器的 id，然后在运行下一个命令时输入它：

```py
$ docker exec -it <container-id> /bin/bash
```

上面的命令将基本上为你提供对容器的 bash 访问权限，这意味着你现在可以运行 dbt 命令。

```py
# Install dbt deps (might not required as long as you have no -or empty- `dbt_packages.yml` file)
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

请注意，由于我们已将本地目录挂载到正在运行的容器中，因此本地目录中的任何更改将立即反映到容器中。这意味着你也可以创建新的模型或修改现有的模型，然后进入正在运行的容器并构建模型、运行测试等。

## 查询 Postgres 数据库中的 dbt 模型

你还可以查询 postgres 数据库及其上创建的 dbt 模型或快照。同样，我们将需要进入正在运行的 postgres 容器，以便直接查询数据库。

```py
# Get the container id for `postgres` service
$ docker ps

# Then copy the container id to the following command to enter the 
# running container
$ docker exec -it <container-id> /bin/bash
```

然后我们将使用 `psql`，这是一个基于终端的 PostgreSQL 界面，允许我们查询数据库：

```py
$ psql -U postgres
```

以下两个命令分别用于列出表和视图：

```py
postgres=# \dt
             List of relations
 Schema |     Name      | Type  |  Owner   
--------+---------------+-------+----------
 public | customers     | table | postgres
 public | orders        | table | postgres
 public | raw_customers | table | postgres
 public | raw_orders    | table | postgres
 public | raw_payments  | table | postgres
(5 rows)

postgres=# \dv
            List of relations
 Schema |     Name      | Type |  Owner   
--------+---------------+------+----------
 public | stg_customers | view | postgres
 public | stg_orders    | view | postgres
 public | stg_payments  | view | postgres
(3 rows)
```

你现在可以通过 `SELECT` 查询来查询 dbt 模型：

```py
SELECT * FROM <table_or_view_name>;
```

## 获取完整代码

我已经创建了一个 GitHub 仓库，你可以在本地机器上克隆它，并快速运行容器化的 Jaffle Shop dbt 项目。你可以在以下链接中找到项目及本教程中共享的代码。

[](https://github.com/gmyrianthous/jaffle_shop?source=post_page-----93a9b14532a4--------------------------------) [## GitHub - gmyrianthous/jaffle_shop：这是 Jaffle Shop dbt 项目的容器化版本

### 这是由 dbt Labs 发布的流行 Jaffle Shop dbt 项目的容器化版本。你可以使用这个项目…

[github.com](https://github.com/gmyrianthous/jaffle_shop?source=post_page-----93a9b14532a4--------------------------------)

## 最后的思考

数据构建工具 (dbt) 是现代数据技术栈中快速增长的技术之一。如果你刚开始学习如何使用 dbt，我强烈推荐尝试 Jaffle Shop 项目。这是一个由 dbt Labs 创建的自包含项目，用于测试和实验目的。

dbt 是数据分析师和分析工程师（除了数据工程师）常用的工具，它需要连接到数据库或数据仓库。然而，许多分析师可能不习惯配置和初始化本地数据库。

在这篇文章中，我们演示了如何开始使用 dbt 并运行所有必要的服务，以在本地 Postgres 数据库上实现 dbt 模型。我希望这个教程能帮助你尽快启动你的 dbt 项目和数据库。如果在运行项目时遇到任何问题，请在评论中告知我，我会尽力帮助你调试代码和配置。

👉 [**订阅 Data Pipeline**](https://thedatapipeline.substack.com/welcome)**，这是一个专注于数据工程的新闻通讯**

👇**相关的文章你也可能喜欢** 👇

[](/dbt-cli-model-selection-52ddd038d8b2?source=post_page-----93a9b14532a4--------------------------------) ## dbt CLI 的模型选择

### 运行 dbt 命令时选择特定模型的完整备忘单

towardsdatascience.com [](/etl-vs-elt-68e221d78719?source=post_page-----93a9b14532a4--------------------------------) ## ETL 与 ELT：有什么区别？

### 数据工程背景下的 ETL 与 ELT 比较

towardsdatascience.com
