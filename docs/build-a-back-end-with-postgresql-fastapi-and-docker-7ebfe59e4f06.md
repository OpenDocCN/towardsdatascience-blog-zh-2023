# 使用 PostgreSQL、FastAPI 和 Docker 构建一个后端

> 原文：[`towardsdatascience.com/build-a-back-end-with-postgresql-fastapi-and-docker-7ebfe59e4f06`](https://towardsdatascience.com/build-a-back-end-with-postgresql-fastapi-and-docker-7ebfe59e4f06)

## 开发基于地图的应用程序的逐步指南（第四部分）

[](https://medium.com/@jacky.kaub?source=post_page-----7ebfe59e4f06--------------------------------)![Jacky Kaub](https://medium.com/@jacky.kaub?source=post_page-----7ebfe59e4f06--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7ebfe59e4f06--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7ebfe59e4f06--------------------------------) [Jacky Kaub](https://medium.com/@jacky.kaub?source=post_page-----7ebfe59e4f06--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7ebfe59e4f06--------------------------------) ·阅读时间 28 分钟·2023 年 3 月 14 日

--

![](img/0c94c9aabf27aae32cad325a9448c7da.png)

图片来自 [Caspar Camille Rubin](https://unsplash.com/@casparrubin?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

地图是可视化和理解地理数据的强大工具，但需要特定的技能才能高效设计。

在这个逐步指南中，我们将深入探讨如何构建一个基于地图的应用程序，以展示客户周围加油站的价格。我们将涵盖产品的不同关键步骤，从最初的概念验证（POC）到最小可行产品（MVP）。

# 系列文章：

第一部分：概念验证——构建一个简约的演示

第二部分：如何使用 React 构建网页应用（静态布局）

第三部分：使用 React 为你的网页应用添加互动性

第四部分：使用 PostgreSQL、FastAPI 和 Docker 构建后端

## 关于这篇文章的一些背景信息

在系列文章的前几部分，我们使用 **React** 构建了加油站查找器的前端，并将后端视为一个仅提供相关数据的“黑箱”。

在这一部分，我们将详细介绍如何使用强大的工具，如 **PostgreSQL** 或 **FastAPI**，一步一步构建后端。

你可以在我的 [Github 页面](https://github.com/jkaub/station-prices-app-full) 找到该项目的完整代码。

## 为什么我们需要一个干净的后端？

在本系列的第一部分中，我们创建了一些实用函数，以便从公共提供商直接获取燃料站的数据。虽然这对于我们的概念验证足够了，但由于多种原因，我们现在需要一个更强大的系统：

+   性能与延迟：实时处理数据，包括解析 XML、格式化和过滤，计算成本高，对于预期频繁使用的应用程序来说，可能不切实际。

+   可靠性：确保我们的应用程序不会受到第三方数据源的意外更改或停机的影响。仅依赖外部门户的数据会使我们的应用程序面临风险，因为即使提供商简单地更改字段名称也可能导致我们这边的错误和停机，而我们需要修补这些变化。通过构建我们自己的数据库，我们可以对数据有更大的控制权，进行必要的更新和维护，而无需依赖外部方。

+   自定义：通过我们自己的数据库，我们可以定制数据以满足技术规格，添加其他外部数据源，为不同的用例构建自定义数据视图等……

为了满足这些需求，我们将构建自己的数据库和 API，处理数据获取、处理和前端交付。这将包括使用 **Docker** 运行 PostgreSQL 数据库，使用 **Python** 和 **sqlalchemy** 与数据库交互，并使用 **PostGIS** 扩展进行地理查询。我们还将探索如何使用 **FastAPI** 和 **SQLmodel** 构建一个简单的 API。

下图是应用程序不同组件的简单示意图：

![](img/82f48ca69d17fa4fea7c496bd09b7ad1.png)

我们应用程序不同组件的简单视图，作者插图

## 本文涵盖的内容

在本文中，我们专注于内部数据库和 API 的创建。具体来说，我们将：

+   使用 Docker 运行 PostgreSQL 数据库

+   使用 python 和 sqlalchemy 与数据库交互

+   使用 PostGIS 扩展进行地理查询

+   使用 FastAPI 和 SQLmodel 构建一个简单的 API

+   使用 **docker-compose** 容器化我们的项目并运行

# 使用 Docker 创建本地 PostgreSQL 实例

Docker 是一个开源容器化平台，允许你在一致且隔离的环境中运行应用程序。使用 Docker 设置 PostgreSQL 服务器有几个优点，包括能够以标准化的方式安装应用程序，而不必担心与系统上其他配置冲突。

在我们的案例中，我们将直接在容器内部设置 Postgre 服务器。

我在这里假设你已经在计算机上安装了 Docker，因为安装方法因系统而异。

## 获取容器镜像

Docker 镜像可以看作是构建一个专用于特定任务的容器所需的所有规格。它本身不做任何事情，但用于构建容器（一个专用虚拟环境），你的应用程序将运行在其中。我们可以使用 Dockerfile 创建我们自己的自定义镜像（稍后会讨论），或者我们可以从社区共享的各种开源镜像中下载现成的镜像。

在我们的情况下，我们需要一个可以帮助我们创建运行 PostgreSQL 的容器的镜像，我们可以使用 [官方镜像](https://hub.docker.com/_/postgres) 来实现这一目的。

我们从在 Docker 上下载 PostgreSQL 镜像开始。这是在 Shell 中完成的：

```py
docker pull postgres
```

## 运行 Postgre 容器

一旦镜像在 Docker 中下载完成，我们可以使用以下命令基于它构建容器：

```py
docker run -itd -e POSTGRES_USER=jkaub -e POSTGRES_PASSWORD=jkaub -p 5432:5432 -v ~/db:/var/lib/postgresql/data --name station-db postgres
```

让我们解密它。

-itd 是三个参数的组合：

+   -d 表示我们以分离模式运行容器。在这种模式下，容器将在后台运行，我们可以继续使用终端进行其他操作。

+   -i 指定我们的容器将以交互模式运行。这将允许我们进入容器并与之交互。

+   -t 表示容器内部将提供一个伪终端，以便与容器进行交互，这将带来更无缝和直观的交互体验。

-e 用于在容器内部生成环境变量。在这种情况下，环境变量 POSTGRES_USER 和 POSTGRES_PASSWORD 还用于用给定的密码生成我们的 PostgreSQL 实例的新用户。如果没有这个，我们仍然可以使用默认用户/密码（postgre/postgre）访问 PostgreSQL 实例。

-p 用于将本地机器的端口映射到 Docker 容器中。PostgreSQL 的默认端口是 5432。如果它已经在你的本地机器上被使用，你可以使用这个参数将容器中的 5432 映射到你机器上的另一个端口。

-v 是一个在我们这种情况下非常重要的参数：它允许我们将一个卷从我们的机器（在我们这种情况下是文件夹 `~/db` ）映射到容器内部的卷，其中 SQL 数据默认存储在 `/var/lib/postgresql/data`。通过进行这种映射，我们创建了一个持久卷，即使容器停止后也会保留。因此，我们的数据库将持续存在，即使我们停止使用容器，之后也可以使用。

— name 只是一个标志，用于给容器命名，这将有助于我们以后访问它。

我们可以通过使用下面的命令检查容器是否处于活动状态，该命令将显示我们机器上运行的容器列表：

```py
docker ps 
```

返回：

```py
CONTAINER ID   IMAGE           COMMAND                  CREATED         STATUS        PORTS                    NAMES
cb0840806636   postgres        "docker-entrypoint.s…"   2 minutes ago   Up 2minutes   0.0.0.0:5432->5432/tcp   station-db
```

# 与 PostgreSQL 的初步交互

我们的 PostgreSQL 实例现在正在容器中运行，我们可以与它进行交互。

## 创建数据库

作为起点，让我们创建一个包含项目不同表格的第一个数据库。

为此，我们需要进入容器。记住，这可能是因为我们在初始化容器时指定了 -it 参数。下面的命令行将完成这项工作：

```py
docker exec -it station-db bash
```

命令提示符现在应该是：

```py
root@cb0840806636:/#
```

这意味着我们以 root 用户身份登录到容器中。我们可以使用用户 (-U)/密码 (-d) 连接到 PostgreSQL，如下所示：

```py
psql -U jkaub -d jkaub
```

一旦进入 PostgreSQL 实例，我们可以使用 SQL 查询与之交互，特别是创建一个新的数据库来托管我们未来的表。

```py
CREATE DATABASE stations;
```

我们可以通过运行来验证数据库是否已经创建

```py
\l
```

这将显示系统中的不同数据库。在一些在实例初始化时创建的默认数据库中，我们可以找到刚刚创建的那个：

```py
jkaub=# \l
                                             List of databases
   Name    | Owner | Encoding |  Collate   |   Ctype    | ICU Locale | Locale Provider | Access privileges 
-----------+-------+----------+------------+------------+------------+-----------------+-------------------
 stations  | jkaub | UTF8     | en_US.utf8 | en_US.utf8 |            | libc            |
```

现在我们已经设置好了 PostgreSQL 实例，我们可以通过手动在**psql**中编写 SQL 查询来创建表和从 .csv 文件导入数据。虽然这种方法适用于一次性使用，但如果我们需要频繁更新表，它可能会变得繁琐且容易出错。

因此，为了促进自动化，我们将使用 Python 框架与数据库及其表进行交互。这将允许我们通过代码轻松创建、更新和查询数据库，使过程更加高效且减少错误。

## 用 sqlalchemy 打开一个会话

SQLalchemy 是一个开源 SQL 工具包和对象关系映射（ORM）工具，供 Python 开发者使用。它提供了一组高级函数来与数据库交互，而不是编写 SQL 查询。

这特别方便，因为它允许我们使用 Python 类（在这里也称为“模型”）定义表的结构，并使用面向对象的范式。我们的 Python ORM，**sqlalchemy**，在下一部分构建后端 API 时将特别有用。

让我们开始安装项目所需的库。在 sqlalchemy 的基础上，我们还将使用**psycopg2**，这是一个 PostgreSQL 适配器，可被 sqlalchemy 用作连接器。

```py
pip install psycopg2 sqlalchemy
```

我们现在可以直接在 Python 中有效地创建一个会话来访问我们的数据库：

```py
from sqlalchemy import create_engine

engine = create_engine('postgresql://jkaub:jkaub@localhost/stations')

# test the connection by executing a simple query
with engine.connect() as conn:
    result = conn.execute('SELECT 1')
    print(result.fetchone())
```

一步步解释这个脚本：

```py
engine = create_engine('postgresql://jkaub:jkaub@localhost/stations')
```

create_engine 方法用于保持与数据库的连接。我们需要在这里指定一个数据库 URL，该 URL 包含连接到我们数据库所需的所有信息。

+   该 URL 的第一部分 postgresql**://** 是为了指定我们正在使用 PostgreSQL 连接，并且接下来将是该类型数据库连接的规格。如果你使用的是不同的数据库，如 SQLite，你将会有不同的基本 URL 和规格。

+   jkaub:jkaub 是连接到我们数据库的登录信息。

+   localhost 是运行数据库的服务器。服务器 IP 也可以用来连接远程服务器，或者，如我们稍后会看到的，在容器集群的情况下，我们在某些情况下也可以使用容器名称。

+   /stations 用于指定我们想要连接的数据库。在我们的例子中，我们连接到我们刚刚创建的“stations”。

```py
# test the connection by executing a simple query
with engine.connect() as conn:
    result = conn.execute('SELECT 1')
    print(result.fetchone())
```

这部分代码目前仅用于测试连接是否成功。我们的数据库还没有表可以查询，所以我们只是运行一个虚拟查询。它应该返回 (1,)，这意味着连接成功。

# 使用 FastAPI 构建 API

现在我们已经在 Docker 容器中设置了 PostgreSQL 数据库，并使用 SQLAlchemy 引擎访问了它，现在是时候开发与数据库交互的 API 了。

使用 API 有几个好处：

+   它提供了可重用性和平台/语言的独立性，允许多个服务使用相同的 API 端点。

+   它将数据库逻辑与应用逻辑分开，使得只要输入/输出被尊重，修改一个而不影响另一个变得更加容易。

+   它增加了一层安全性，因为你可以通过授权系统控制谁可以访问数据库。

+   最后，API 是可扩展的，可以在多个服务器上运行，使其在管理工作负载方面非常灵活。通过创建一组明确的 URL，我们将能够通过 API 从数据库中检索、修改、插入或删除数据。

## 关于 FastAPI

[FastAPI](https://fastapi.tiangolo.com/) 是一个现代的 Python 框架，在构建轻量级 API 方面特别高效，由 [Sebastián Ramírez](https://github.com/tiangolo) 开发。

结合**sqlalchemy**和[**pydantic**](https://docs.pydantic.dev/)这两个用于数据验证的 Python 库时，它特别高效（例如，它可以检查一个日期是否确实是日期，一个数字是否确实是数字，等等）。一起使用，它使我们能够通过框架有效地处理和查询表格。

更棒的是，[Sebastián Ramírez](https://github.com/tiangolo) 还设计了另一个库，[**sqlmodel**](https://sqlmodel.tiangolo.com/)，它结合了 pydantic 和 sqlalchemy，去除了一些冗余，并进一步简化了 API 的架构。

如果你还不熟悉 FastAPI，我建议你先看看[这个教程](https://fastapi.tiangolo.com/tutorial/)，这个教程做得非常好。

在开始项目之前，我们需要安装多个库。

```py
pip install uvicorn
pip install fastapi
pip install sqlmodel
pip install geoalchemy2
```

+   **uvicorn** 是运行 API 服务器的工具，并且非常适合与 FastAPI 一起使用。

+   **fastapi** 是 API 的核心引擎，我们将用它来创建不同的端点。

+   **sqlmodel** 将 sqlalchemy ORM 与 pydantic 的类型验证功能结合起来

+   **geolochemy2** 是 sqlalchemy 的一个扩展，用于执行地理查询。

## 初始化模型

让我们为我们的 API 项目创建一个新的仓库，从使用**sqlmodel**定义模型开始。 “模型”只是一个代表 SQL 中表格的 Python 类。

```py
api/
  |-- app/
    |-- __init__.py
    |-- models.py
```

我们的项目将包含 3 个表格，并遵循我们在 part I 中构建的初始设计。

+   一个包含与城市相关的信息（邮政编码、位置）的表格

+   一个包含关于燃气价格信息的表格

+   一个包含关于车站信息的表格

通过联接和地理过滤器组合这些表，将帮助我们构建前端请求的最终输出。

我们来看一下第一个表格，Cities 表：

```py
from sqlmodel import Field, SQLModel
from datetime import datetime
from typing import Optional

class Cities(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    postal_code: str
    name: str
    lat: float
    lon: float
```

类 “Cities” 继承自 SQLModel 类，结合了 sqlalchemy**’s** ORM 特性和 pydantic 的类型控制。

参数 table=True 表示如果数据库中尚不存在相应的表，则会自动创建该表，并匹配列名和列类型。

类的每个属性将定义每一列及其类型。特别是，“id”将作为我们的主键。使用 Optional 将指示 sqlalchemy 如果我们不提供 id，则自动生成 id。

我们还提供了另外两个表的模型：

```py
from datetime import datetime

...

class GasPrices(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    station_id: str
    oil_id: str
    nom: str
    valeur: float
    maj: datetime = Field(default_factory=datetime.utcnow)

class Stations(SQLModel, table=True):
    station_id: str = Field(primary_key=True)
    latitude: float
    longitude: float
    cp: str
    city: str
    adress: str
```

注意在 Stations 表中，我们使用 station_id 作为主键，与 GasPrices 不同，该字段是必填的。如果在发送到表格时该字段为空，将会生成错误信息。

## 初始化引擎

在另一个专用文件中，为了保持项目结构化，我们将初始化引擎。我们称之为 services.py。

```py
api/
  |-- app/
    |-- __init__.py
    |-- models.py
    |-- services.py
```

连接到数据库的方式与之前介绍的相同。

```py
from sqlmodel import SQLModel, create_engine
import models

DATABASE_URL = 'postgresql://jkaub:jkaub@localhost/stations'

engine = create_engine(DATABASE_URL)

def create_db_and_tables():
    SQLModel.metadata.create_all(engine)
```

注意函数 create_db_and_tables()：该函数将在 API 初始化期间被调用，查看 models.py 中定义的模型，并直接在 SQL 数据库中创建它们（如果它们尚不存在的话）。

## 实操 API

我们现在可以开始开发主要组件，我们将在其中放置端点（即允许我们与数据库交互的 URL）。

```py
api/
  |-- app/
    |-- __init__.py
    |-- main.py
    |-- models.py
    |-- services.py
```

我们要做的第一件事是配置 FastAPI 在启动时的设置，并处理 API 授权。

```py
from fastapi.middleware.cors import CORSMiddleware
from fastapi import FastAPI, HTTPException
from models import Cities, Stations, GasPrices
from services import engine, create_db_and_tables

#We create an instance of FastAPI
app = FastAPI()

#We define authorizations for middleware components
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

#We use a callback to trigger the creation of the table if they don't exist yet
#When the API is starting
@app.on_event("startup")
def on_startup():
    create_db_and_tables()
```

一个重要的点是：默认情况下，我们的前端没有访问 API 调用的权限，如果你忘记配置中间件部分，将会在前端出现错误。你可以决定通过使用以下方式允许所有来源：

```py
allow_origins=["*"],
```

但由于安全原因，我不推荐这样做，因为一旦上线，你基本上会将 API 向全世界开放。我们的前端目前在 localhost:3000 上本地运行，所以这是我们将允许的域名。

到那时，我们已经可以通过使用以下命令行来启动 API：

```py
uvicorn main:app --reload
```

— reload 只是意味着每次在 API 正在运行时保存修改，它将重新加载以包含这些更改。

一旦启动，你可以看到一些日志显示在终端中，特别是：

```py
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
```

表示 API 服务器正在 localhost（等同于 IP 127.0.0.1）上的 8000 端口运行。

如前所述，启动 API 还会触发数据库中空表的创建（如果它们尚不存在）。因此，从你第一次启动 API 的那一刻起，使用 table=True 创建的模型将在数据库中有一个专用表。

我们可以通过在 psql 中从 PostgresSQL 容器内部轻松检查这一点。连接为主用户后，我们首先连接到数据库 station：

```py
\c stations
```

我们现在可以检查我们的表是否已正确创建：

```py
\dt
```

这将返回：

```py
 List of relations
 Schema |      Name       | Type  | Owner 
--------+-----------------+-------+-------
 public | cities          | table | jkaub
 public | gasprices       | table | jkaub
 public | stations        | table | jkaub
```

我们还可以通过在 psql 中运行描述查询来验证列是否与我们的模型匹配，例如，对于 cities 表：

```py
\d cities
```

```py
 Column    |       Type        | Collation | Nullable |              Default  

-------------+-------------------+-----------+----------+-----------------------
-------------
 id          | integer           |           | not null | nextval('cities_id_seq
'::regclass)
 postal_code | character varying |           | not null | 
 name        | character varying |           | not null | 
 lat         | double precision  |           | not null | 
 lon         | double precision  |           | not null | 
Indexes:
    "cities_pkey" PRIMARY KEY, btree (id)
```

## 构建我们的第一个请求 — 使用 POST 请求向 Cities 添加行

Cities 表只会填充一次，并用于将邮政编码与城市的经纬度匹配，这在后续使用邮政编码查询这些位置时特别有帮助。

目前，数据存储在 .csv 文件中，我们想设计一个 POST 调用，用于更新表格，如果数据还不在数据库中，则一次添加一行。API 调用被放在 main.py 文件中。

```py
from sqlmodel import Session

...

@app.post("/add-city/")
def add_city(city: Cities):
    with Session(engine) as session:
        session.add(city)
        session.commit()
        session.refresh(city)
        return city
```

让我们逐行查看这段代码：

```py
@app.post("/add-city/")
```

每个 API 端点都是通过装饰器定义的。我们在这里定义了两件事：请求的类型（get、post、put、delete…）和关联的 URL 端点 (/add-city/)。

在这个特定的案例中，我们将能够在 http://127.0.0.1:8000/add-city/ 执行 POST 请求

```py
def add_city(city: Cities):
```

我们将不同的参数传递给函数以用于查询。在我们的案例中，post 请求将寻找 Cities 的实例，这将通过 JSON 传递在我们的请求中。此 JSON 将包含我们想要添加的新行的 Cities 表的每列的值。

```py
with Session(engine) as session:
```

要连接到数据库，我们打开一个 Session。每个查询需要自己的会话。使用这种方法在会话内部出现意外情况时特别有用：在会话初始化和 commit() 之间所做的所有更改将在出现问题时回滚。

```py
 session.add(city)
  session.commit()
  session.refresh(city)
```

在这里，对象被添加到数据库中，然后进行提交。从提交的那一刻起，操作无法回滚。refresh 用于更新 DB 进行的任何修改的“city”对象。在我们的案例中，例如，会自动添加递增的“id”。

```py
return city
```

我们通过以 JSON 格式发送 city 对象来结束请求。

我们现在可以在 python 中尝试请求（当然需要 API 正在运行）：

```py
import requests

url='http://127.0.0.1:8000/add-city/'

json = {
    'postal_code': '01400',
    'name':"L'Abergement-Clémenciat",
    'lat':46.1517,
    'lon':4.9306
}

req = requests.post(url, json=json)
```

请注意，我们在请求中发送的 JSON 键与我们要更新的表的列名称匹配。参数“id”是可选的，它将自动添加到操作中，我们不需要担心它。

这应该会在 API shell 中触发以下行：

```py
INFO:     127.0.0.1:33960 - "POST /add-gas-price/ HTTP/1.1" 200 OK
```

这意味着请求成功。我们可以进一步验证该行是否已正确添加。返回到我们 docker 中的 psql，我们可以尝试以下查询：

```py
SELECT * FROM cities LIMIT 1;
```

这将显示：

```py
 id | postal_code |          name           |   lat   |  lon   
----+-------------+-------------------------+---------+--------
  1 | 01400       | L'Abergement-Clémenciat | 46.1517 | 4.9306
```

演示该行已被 API 有效添加到我们的数据库中。

此外，我们不希望邮政编码被重复添加。为此，我们将查询 Cities 表，根据我们尝试发送的邮政编码过滤表，并在找到具有该邮政编码的行时返回 HTML 错误，从而避免邮政编码重复。

```py
from fastapi import FastAPI, HTTPException

...

@app.post("/add-city/")
async def add_city(city: Cities):
    with Session(engine) as session:

        #New code block
        exist = session.query(Cities).filter(
            Cities.postal_code == city.postal_code).first()
        if exist:
            raise HTTPException(
                status_code=400, detail="Postal code already exists")
        #New code block

        session.add(city)
        session.commit()
        session.refresh(city)
        return city
```

在这段新代码中，我们使用 sqlalchemy ORM 执行了第一次数据库查询：我们没有编写经典的 SQL（“SELECT FROM”），而是使用了一组函数来直接查询数据库。

```py
exist = session.query(Cities).filter(
            Cities.postal_code == city.postal_code).first()
```

+   `.query` 相当于 SELECT … FROM …，在我们的案例中，我们从 cities 表中选择所有内容。

+   `.filter` 相当于 WHERE 语句。特别地，我们要匹配与我们发送的对象的邮政编码相等的条目（由变量 city 表示）。

+   `.first()` 是自解释的，相当于 LIMIT 1。

+   如果没有找到行，则 `exist` 将为 None，并且不会引发异常，因此我们将把对象添加到数据库中。如果某行与邮政编码匹配，API 请求将返回状态码 400 的错误，且 POST 请求将被中断，元素不会被添加。

如果我们现在尝试发送完全相同的请求，将看到 API 返回错误消息：

```py
INFO:     127.0.0.1:49076 - "POST /add-city/ HTTP/1.1" 400 Bad Request
```

而且该行没有被添加到表中。

从那时起，我们可以简单地遍历 `.csv` 并逐一添加所有城市，以填充 cities 表。

## 使用 POST 请求向 Gasprices 和 Stations 表中添加行

我们将非常快速地跳过这些 API 调用的构建，因为它们与之前的非常相似。

```py
@app.post("/add-station/")
async def add_station(station: Stations):
    with Session(engine) as session:
        exist = session.query(Stations).filter(
            Stations.station_id == station.station_id).first()
        if exist:
            raise HTTPException(
                status_code=400, detail="Station already exists")

        session.add(station)
        session.commit()
        session.refresh(station)

        return station

@app.post("/add-gas-price/")
async def add_station(gasPrice: GasPrices):
    with Session(engine) as session:
        exist = session.query(GasPrices). \
            filter(GasPrices.oil_id == gasPrice.oil_id). \
            filter(GasPrices.maj == gasPrice.maj). \
            first()
        if exist:
            raise HTTPException(
                status_code=400, detail="Entry already exists")

        session.add(gasPrice)
        session.commit()
        session.refresh(gasPrice)
        return gasPrice
```

唯一需要注意的有趣之处在于，我们使用了双重过滤查询，以确保仅在 `oil_id` 有新更新时才添加一行。这样，我们确保了未来的更新不会在价格没有变化的情况下创建重复项，从而节省了数据库空间。

为了检索和处理油价，我们只是回收了来自 Part I 的解析代码，获取相应的数据集并循环遍历，为每个条目进行 POST 调用。

以下脚本在 API 范围之外执行，以将数据上传到数据库：

```py
import request
from data_parsing import get_data

BASE_API_URL = 'http://127.0.0.1:8000'

#get_data is the function designed in part I to pull the xml from the opendata
#source and convert them in Dataframes
stations, gas = get_data()

#Pushing stations data
to_push = stations[['latitude','longitude','cp','adress','city','station_id']].to_dict('records')

url=f'{BASE_API_URL}/add-station/'
for elmt in to_push:
    req = requests.post(url, json=elmt)

#Pushing gasprices data
to_push = gas.to_dict('records')

url=f'{BASE_API_URL}/add-gas-price/'
for elmt in to_push:
    req = requests.post(url, json=elmt)
```

*注意：为了简化起见，我在这里选择逐行推送数据。我们也可以设计端点以批量推送数据，并发送 JSON 列表。*

# 构建前端使用的 GET 查询

到这一点为止，我们的数据库已完全填充，以上脚本可以用于用更近期的数据更新数据库，我们可以开始构建前端用来查询特定城市周围加油站特定燃料价格的 GET 请求。

我决定为这个特定查询专门设置一个完整的章节，因为它的复杂性（我们将使用到目前为止定义的所有表，进行连接和地理过滤），同时因为我们需要在此时对数据库进行一些更改，以集成空间功能，安装附加组件和修改一些模型。虽然这可以从一开始就直接完成，但在实际项目中进行修改是常见的，我认为展示如何平滑地完成这一过程是很有趣的。

## 安装 PostGIS

PostGIS 是 PostgreSQL 的一个扩展，允许我们构建地理查询，这意味着需要一个空间组件。例如，在我们的情况下，我们能够选择距离某个兴趣点 30 公里半径内的所有车站数据行。

现在我们不想直接在运行的容器中安装 PostGIS，因为每次我们需要弹出一个新的容器时，这个安装会“丢失”，新容器基于仅安装 PostgreSQL 的镜像。

相反，我们将简单地更改用于构建容器的镜像，并将其替换为包含 PostgreSQL 和 PostGIS 的镜像。我们将提供相同的持久存储位置，以便新容器也可以访问它。

要构建包含 PostGIS 扩展的容器，我们首先从 docker 中拉取最新的 PostGIS 镜像，然后杀死并删除当前的 PostgreSQL 容器，并用新的镜像构建一个新的容器。

```py
docker pull postgis
docker kill stations
docker rm stations
docker run -itd -e POSTGRES_USER=jkaub -e POSTGRES_PASSWORD=jkaub -p 5432:5432 -v ~/db:/var/lib/postgresql/data --name station-db postgis/postgis:latest
```

我们可以像之前一样访问容器，但现在我们使用的是包括 PostGIS 的 PostgreSQL 版本。

我们现在需要将扩展添加到现有数据库中。我们首先重新连接到数据库：

```py
docker exec -it station-db bash
```

```py
psql -U jkaub -d jkaub
```

```py
\c stations
```

然后我们在其中包含 PostGIS 扩展：

```py
CREATE EXTENSION postgis;
```

## 修改我们的 Stations 模型

现在我们的数据库中已经运行了 PostGIS，我们需要修改我们的 Stations 表以便能够执行地理查询。更准确地说，我们需要添加一个“geometry”字段，该字段被理解并转换为地球上的实际位置。

建立地图或标示地球上的位置有多种方法，每种方法都有其自己的投影和参考坐标系统。为了确保一个系统能够与另一个系统对话，我们需要确保它们使用相同的语言，这可能包括单位的转换（就像我们可以将米转换为英尺，或将千克转换为磅）。

对于坐标，我们使用称为“地理参数数据集”（EPSG）的东西。纬度和经度（EPSG 4326）以角度表示，无法直接转换为距离（欧几里得几何，包括距离计算，不能直接应用于球面，因为本质上这不是一个欧几里得表面）。相反，需要将其投影到平面表示中，这在 PostGIS 中处理得很好，只要我们注意并应用适当的转换。

作为起点，我们需要在 Stations 数据库中添加一个可以被解释为“几何”坐标的新字段。在我们的数据库中：

```py
ALTER TABLE stations ADD COLUMN geom geometry(Point, 4326);
```

这一行将修改我们的 stations 表，添加一个新的字段 “geom”，它是类型为 “point” 的 PostGIS 几何图形，使用 EPSG 4326（经纬度系统的 EPSG）表示。该字段目前对所有行为空，但我们仍然可以在 SQL 中轻松填充它，以更新当前表（此时表并不为空）。

```py
UPDATE stations SET geom = ST_SetSRID(ST_MakePoint(longitude, latitude), 4326);
```

上面的 SQL 查询将为 Stations 表的每一行设置 geom 列，使用由经度/纬度构建的点。注意我们在这里使用了两个 PostGIS 函数，ST_MakePoint 和 ST_SetSRID，来帮助我们在 SQL 中定义几何图形。

我们可以检查这个新的几何图形在数据库中的存储方式

```py
SELECT * FROM stations LIMIT 1;
```

```py
 station_id | latitude | longitude |  cp   | city  |        adress         |                              geom                        
------------+----------+-----------+-------+-------+-----------------------+----------------------------------------------------
 26110004   |    44.36 |     5.127 | 26110 | NYONS | 31 Avenue de Venterol | 0101000020E6100000355EBA490C821440AE47E17A142E4640
```

你可以在这里看到，几何图形以字符串编码，这种格式是 Well-Known Binary (WKB)，它在存储几何图形时非常高效。我不会进一步展开这个内容，但如果你在数据集中看到这个格式不要感到惊讶，如果需要，你可能需要将其解码为更可读的格式。

现在，我们还需要更新 model.py 文件中的 Stations 类以包含这个新字段，为此我们使用 **geoalchemy** 的 “Geometry” 类型。

```py
from typing import Any
from geoalchemy2.types import Geometry

class Stations(SQLModel, table=True):
    station_id: str = Field(primary_key=True)
    latitude: float
    longitude: float
    cp: str
    city: str
    adress: str
    geom: Optional[Any] = Field(sa_column=Column(Geometry('GEOMETRY')))
```

最后的修改是：我们希望在 POST 调用（在 main.py 中）时，使用纬度和经度参数自动计算几何图形：

```py
from geoalchemy2.elements import WKTElement

@app.post("/add-station/")
async def add_station(station: Stations):
    with Session(engine) as session:
        exist = session.query(Stations).filter(
            Stations.station_id == station.station_id).first()
        if exist:
            raise HTTPException(
                status_code=400, detail="Station already exists")

        #New code block
        point = f"POINT({station.longitude} {station.latitude})"
        station.geom = WKTElement(point, srid=4326)
        #New code block

        session.add(station)
        session.commit()
        session.refresh(station)

        #This is only done to return a clean dictionnar with a proper json format
        to_return = {}
        to_return["station_id"] = station.station_id
        to_return["latitude"] = station.latitude
        to_return["longitude"] = station.longitude
        to_return["cp"] = station.cp
        to_return['city'] = station.city
        to_return["adress"] = station.adress

        return to_return
```

在这里，我们通过字符串创建一个点，使用另一种名为 WKTElement 的格式，这是一种使用人类可读的字符串编码几何图形的方式。我们的字符串随后通过 **geolalchemy** 函数 WKTElement 转换为几何图形，该函数隐式地将其转换为 WKB 格式以便编码到数据库中。

注意，“geom”不是 JSON 可序列化的，因此我们需要在通过 API 发送站点对象之前修改或删除它。

## 构建最终的 GET 查询

GET 查询的目标是从通过邮政编码识别的城市中检索 30 公里半径内的所有站点，并显示查询中提到的某种类型的所有站点的燃料最新价格，并附上一些美化的信息，如规范化的地址或 Google 地图链接。

```py
{  
  "lat": 49.1414,
  "lon": 2.5087,
  "city": "Orry-la-Ville",
  "station_infos": [
    {
      "address": "Zi Route de Crouy 60530 Neuilly-en-Thelle",
      "price_per_L": 1.58,
      "price_tank": 95,
      "delta_average": 25.1,
      "better_average": 1,
      "google_map_link": "https://www.google.com/maps/search/?api=1&query=Zi+Route+de+Crouy+60530+Neuilly-en-Thelle",
      "distance": 19.140224654602328,
      "latitude": 49.229,
      "longitude": 2.282
    }, ...
  ]
}
```

我们将分两步进行：

+   首先构建一个高效的 SQL 查询来执行连接和过滤操作

+   在通过 API 发送结果之前，使用 Python 函数修改查询的输出。

与其他参数通过请求体中的 JSON 传递的查询不同，我们在这里将使用另一种约定，即将查询参数直接传递在 URL 中，见下面的示例：

```py
http://localhost:8000/stations/?oil_type=SP98&postal_code=60560
```

在 FastAPI 中，这可以通过简单地向用于构建端点的函数中添加输入来自然完成：

```py
@app.get("/stations/")
async def get_prices(oil_type: str, postal_code: str):
  with Session(engine) as session:
    ...
```

现在我们要首先检索的是与邮政编码相关联的城市的纬度和经度。如果没有与邮政编码关联的城市，API 应该返回一个错误代码，说明未找到邮政编码。

```py
city = session.query(Cities).filter(
         Cities.postal_code == postal_code
       ).first()
if not city:
    raise HTTPException(
        status_code=404, detail="Postal Code not found")
```

接下来，我们将构建一系列子查询。每个子查询在最终查询完全执行之前不会被评估。这将帮助我们保持代码的可读性，并优化查询，因为 sqlalchemy ORM 会根据这些子查询动态优化查询。

我们要执行的第一个子查询是从 Stations 表中选择所有在已查询城市 30 公里半径范围内的车站。

```py
stations = session.query(
    Stations.station_id, Stations.adress,  Stations.cp, Stations.city,
    Stations.latitude, Stations.longitude,
).filter(
    ST_Distance(
        Stations.geom.ST_GeogFromWKB(),
        WKTElement(f"POINT({city.lon} {city.lat})",
                   srid=4326).ST_GeogFromWKB()
    ) < 30000).subquery()
```

这里有许多有趣的地方需要注意。

+   我们只在 session.query( … ) 中选择了少量列，并且不保留 geom 列，该列仅用于过滤。在标准 SQL 中，这可以通过 “SELECT station_id, adress, cp, city, latitude, longitude FROM stations” 来完成。

+   我们使用**ST_Distance**，这是 geoalchemy 的内置函数，用于计算两个地理位置之间的距离（另一种 geoalchemy 类型）。

+   ST_Distance 也可以与几何对象一起工作，但输出将变成角度距离（请记住，纬度/经度是以角度表示的），这不是我们想要的。

+   要将几何对象转换为地理对象，我们只需使用另一个内置函数 ST_GeoFromWKB，它会自动将我们的几何体投影到其参考系统中，以在地球上形成一个点。

接下来，我们根据所需的 oil_type（如 SP95、Gazole 等）过滤 Gasprices 表。

```py
 price_wanted_gas = session.query(GasPrices).filter(
      GasPrices.nom == oil_type
  ).subquery()
```

我们还需要根据数据集中最新的价格来过滤 Gasprices 表。这不是一项容易的任务，因为所有价格的更新不是同时完成的。我们将分两步构建子查询。

首先，我们通过从 price_wanted_gas 子表中提取 station_id 和最后更新时间来执行聚合。

```py
last_price = session.query(
    price_wanted_gas.c.station_id,
    func.max(price_wanted_gas.c.maj).label("max_maj")
    ).group_by(price_wanted_gas.c.station_id) \
     .subquery()
```

然后使用这些信息帮助我们通过连接过滤 price_wanted_gas，其中仅保留最新更新价格的行。“and_” 方法允许我们在连接操作中使用多个条件。

```py
last_price_full = session.query(price_wanted_gas).join(
    last_price,
    and_(
        price_wanted_gas.c.station_id == last_price.c.station_id,
        price_wanted_gas.c.maj == last_price.c.max_maj
    )
).subquery()
```

最后，我们在 last_price_full 子表（包含给定燃料的所有最新价格）和 stations 子表（包括所有在 30 公里半径内的车站）之间进行最终连接，并检索所有结果。

```py
stations_with_price = session.query(stations, last_price_full).join(
    last_price_full,
    stations.c.station_id == last_price_full.c.station_id
).all()
```

到达这一点时，我们检索了经过过滤的相关车站列表，并将其与 GasPrices 表中的相关信息（即：价格）合并，我们只需要对输出结果进行后处理，以符合前端的要求。由于此时表格已经被清理和过滤，因此最终的后处理步骤可以在原生 Python 中完成，而不会对性能产生太大影响。

我将稍微详细说明这个最终的后处理步骤，因为它不在文章的核心部分，但请随时查看 [GitHub](https://github.com/jkaub/station-prices-app-full) 仓库以获取更多信息。

```py
prices = [float(e["valeur"]) for e in stations_with_price]
avg_price = float(np.median(prices))

output = {
    "lat": city.lat,
    "lon": city.lon,
    "city": pretify_address(city.name),
    "station_infos": sorted([extend_dict(x, avg_price, city.lat, city.lon) for x in stations_with_price], key=lambda x: -(x['delta_average']))
}

return output
```

我们现在可以测试并验证查询是否返回了相关输出。我们可以使用 Python 请求进行检查，但 FastAPI 还提供了所有端点的内置文档，您可以在 [`localhost:8000/docs`](http://localhost:8000/docs) 测试您的 API。

![](img/d98f30901a0a542ae5e477d2d7662e46.png)

FastAPI 内置文档的截图，作者插图

# 容器化应用程序

现在我们有了一个运行中的 API，我们将通过将应用程序打包到容器中来完成本文。

这是我们项目的组织方式：

```py
stations-project/
  |-- db/
  |-- api/
    |-- app/
    |-- requirements.txt
    |-- Dockerfile
  |-- update_scripts/
  |-- front/
  |-- docker-compose.yml
```

我们将使用 api/ 中的 Dockerfile 来容器化 API，并使用 docker-compose 同时管理 API 和数据库。

文件夹 db/ 是 PostgreSQL 容器用来持久化数据库的卷。

## 打包我们的 API

为了打包我们的 API，我们将简单地构建一个 docker 镜像，该镜像将复制运行 API 所需的环境和依赖项。这个 docker 镜像将包含运行 API 所需的所有内容，包括代码、运行时、系统工具、库和配置。

为此，我们需要编写一个 Dockerfile，其中包含设置 FastAPI 环境的一系列指令。编写 Dockerfile 相对容易，只要理解了原理：它就像是从头开始配置一台新机器。在我们的案例中：

+   我们需要安装相关版本的 Python

+   设置工作目录

+   将相关文件复制到我们的工作目录中（包括 requirements.txt 文件，该文件是使用 pip install 安装项目所需所有库的强制要求）

+   使用 pip install 安装库

+   暴露 FastAPI 端口

+   运行初始化 API 的命令（uvicorn main:app — reload）

用 Docker 语言翻译过来，这变成了：

```py
FROM python:3.9

WORKDIR /code

COPY ./requirements.txt /code/requirements.txt

COPY ./app /code/app

RUN pip install --no-cache-dir -r requirements.txt

EXPOSE 80

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "80"]
```

我们还需要处理 requirements.txt 文件，其中明确列出所有使用的库及其版本。

```py
fastapi==0.94.0
GeoAlchemy2==0.13.1
numpy==1.24.2
SQLAlchemy==1.4.41
sqlmodel==0.0.8
uvicorn==0.20.0
psycopg2==2.9.5
```

在进行这些更新之后，我们现在可以构建容器镜像（在包含 Dockerfile 的文件夹内）：

```py
docker build -t fast-api-stations .
```

## 使用 docker-compose

**docker-compose** 是一个用于定义和运行多容器 Docker 应用程序的工具。在我们的案例中，我们希望同时运行 SQL 容器和 FastAPI 容器。我将假设您已经在计算机上安装了 docker-compose。如果没有，请按照[这些说明](https://docs.docker.com/compose/install/)进行操作。

为了使用 docker-compose，我们只需在项目的根目录中配置一个 `docker-compose.yml` 文件，该文件定义了构成应用程序的服务及其各自的配置。

`docker-compose.yml` 文件使用 YAML 语法定义了一组服务，每个服务代表一个将作为全球应用程序的一部分运行的容器。每个服务可以指定其镜像、构建上下文、环境变量、持久化卷、端口等……

这就是我们的 docker-compose.yml 文件的样子：

```py
version: "3"

services:
  fastapi:
    image: fast-api-stations
    ports:
      - "8000:80"

  stationdb:
    image: postgis/postgis
    environment:
      POSTGRES_USER: jkaub
      POSTGRES_PASSWORD: jkaub
      POSTGRES_DB: stations
    volumes:
      - ./db:/var/lib/postgresql/data
```

如您所见，我们定义了两个服务：

+   一个用于 API 的服务，现在名为 FastAPI，构建在我们在前一个小节中创建的 Docker 镜像 fast-api-station 上。对于这个服务，我们将容器的 80 端口暴露给本地的 8000 端口。

+   一个用于 DB，运行在 PostGIS 镜像上。我们指定了与之前相同的环境变量和相同的卷以持久化数据库。

## 最后一个小修改

我们曾经使用本地 IP 连接到 SQL 引擎。由于我们现在在两个不同的环境中运行 API 和 PostgreSQL，我们需要更改连接数据库的方式。

docker-compose 自行管理不同容器之间的网络，并使我们能够轻松地从一个服务连接到另一个服务。为了从 API 服务连接到 SQL 服务，我们可以在引擎创建时指定要连接的服务名称：

```py
DATABASE_URL = 'postgresql://jkaub:jkaub@stationdb/stations'
```

## 运行后端

现在我们已经配置好了所有内容，我们可以通过以下方式运行我们的后端应用程序：

```py
docker-compose up
```

API 将通过 8000 端口提供服务。

```py
http://localhost:8000/docs
```

# 结论

在这篇文章中，我们一直在处理我们 GasFinder 应用程序的后端。

我们决定将应用程序的所有相关数据存储在我们自己的存储解决方案中，以避免所有可能与依赖第三方连接相关的问题。

我们利用了 Docker 和 PostgreSQL+PostGIS 构建了一个数据库，使我们能够执行高效的地理查询，并使用 Python 框架 FastAPI + SQLModel 构建了一个高效的 API，可以用来与数据库交互，并向前端提供数据，这些前端是在之前的文章中开发的。

目前，我们有一个基于“生产标准”工具（React、PostgreSQL、FastAPI 等）的原型，可以在本地 100%运行。在本系列的最后部分，我们将看看如何使应用程序上线并自动更新我们的 SQL 表，以始终提供最新的信息。
