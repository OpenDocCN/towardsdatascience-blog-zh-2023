# 如何通过 Cloud SQL Auth Proxy 在 Docker 中连接到 GCP Cloud SQL

> 原文：[`towardsdatascience.com/how-to-connect-to-gcp-cloud-sql-with-cloud-sql-auth-proxy-in-docker-99bdf810c498`](https://towardsdatascience.com/how-to-connect-to-gcp-cloud-sql-with-cloud-sql-auth-proxy-in-docker-99bdf810c498)

## 学习一种标准的方法将 Docker 化应用程序连接到 GCP Cloud SQL 实例

[](https://lynn-kwong.medium.com/?source=post_page-----99bdf810c498--------------------------------)![Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----99bdf810c498--------------------------------)[](https://towardsdatascience.com/?source=post_page-----99bdf810c498--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----99bdf810c498--------------------------------) [Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----99bdf810c498--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----99bdf810c498--------------------------------) ·9 分钟阅读·2023 年 2 月 20 日

--

![](img/ab898b9635bf96960353af4de7d4cb3b.png)

图片来源于 Pixabay 上的 WilliamsCreativity (Servers Data)

Google Cloud Platform (GCP) 上的 Cloud SQL 是一个很棒的服务，如果你想在云中托管你的关系型数据库。连接到 Cloud SQL 的标准方法有一些，比如从你的 GCP 资源（如 Cloud Run、Compute engine 等）连接。然而，关于如何将 Docker 化应用程序连接到 Cloud SQL 的文档并不多。

Cloud SQL Auth proxy 是连接你的 Docker 化应用程序到 Cloud SQL 的推荐方法。它提供了安全访问你的 Cloud SQL 实例，而无需授权网络或配置 SSL。

在本文中，我们将介绍如何以各种方式使用 Cloud SQL Auth proxy，重点讲解如何编写 `docker-compose.yaml` 文件以连接 Docker 化应用程序到 Cloud SQL 实例。

## 准备工作

在这一点上，我假设你已经有了一个 GCP Cloud SQL 实例。Cloud SQL 实例的创建通常由系统或 DevOps 工程师完成。然而，如果你在做自己的项目或者只是为了学习目的，你可以自己创建一个。前往 GCP 控制台并按照说明操作，创建一个应该相当简单。记得创建一个数据库用户，以便后续使用。

如果你想在个人笔记本电脑或台式机上运行本文中的代码，建议设置本地环境以便与 GCP 配合使用。

确保你的 Google 帐号或服务帐号至少具有“*Cloud SQL Client*”角色。如果适用，也可以分配“Cloud SQL Editor”或“Cloud SQL Admin”角色。

确保已启用 Cloud SQL Admin API。

## 直接使用 Cloud SQL Auth proxy 命令。

有些情况下，直接使用 Cloud SQL Auth 代理命令（而不是使用 Docker）是更可取的。例如，当你想将命令作为启动脚本运行，或在裸金属机器或云虚拟机（VM）上将其作为服务时。

学习 Cloud SQL Auth 代理命令对在 Docker 中使用也很有帮助，因为命令实际上是相同的，只不过后者是在 Docker 容器中运行。

首先，我们需要下载 Cloud SQL Auth 代理的二进制文件：

```py
sudo curl -o /usr/local/bin/cloud-sql-proxy \
  https://storage.googleapis.com/cloud-sql-connectors/cloud-sql-proxy/v2.0.0/cloud-sql-proxy.linux.amd64

sudo chmod +x /usr/local/bin/cloud-sql-proxy
```

你可以在其 GitHub 仓库中找到 Cloud SQL Auth 代理的最新版本。

然后我们可以使用 Cloud SQL Auth 代理连接到我们的 Cloud SQL 实例，请查看下面的评论，这些评论包含每个命令的上下文解释：

```py
# 1\. Find the instance name to be used.
#    It should have a format of myproject:myregion:myinstance.
gcloud sql instances describe <INSTANCE_NAME> --format='value(connectionName)'

# 2\. Use the long instance connection name returned by 1 for cloud-sql-proxy.
cloud-sql-proxy --port 13306 INSTANCE_CONNECTION_NAME &
# Note that a high port is used to avoid potential port conflicts.
# It may not be needed for your use case.

# 3\. When not running on GCP resources and GOOGLE_APPLICATION_CREDENTIALS is
#    not set locally, we need to specify the key file directly.
cloud-sql-proxy --port 13306 INSTANCE_CONNECTION_NAME \
    --credentials-file /local/path/to/service-account-key.json &

# 4\. On GCP compute engine, you can connect by a private IP if the compute
#    engine and SQL instance are in the same VPC network.
cloud-sql-proxy --port 13306 INSTANCE_CONNECTION_NAME --private-ip &

# 5\. You can connect to multiple Cloud SQL instances at same time, which can
#    be handy for local development as you may need to access multiple
#    databases at the same time.
cloud-sql-proxy "INSTANCE_CONNECTION_NAME_1?port=13306" \
    "INSTANCE_CONNECTION_NAME_2?port=13307" &
# Remember to specify differnt ports for different instances.
```

请注意，在`cloud-sql-proxy`命令行的末尾放置了一个和号（&），这样我们可以在后台的另一个进程中运行 Cloud SQL Auth 代理。这样，我们不会意外关闭连接，也不需要打开一个新的标签页，如果我们想立即使用 SQL 客户端连接到我们的 SQL 实例。

我们可以使用`jobs`命令查看后台运行的进程。我们还可以使用`fg %N`或`kill %N`（其中`N`是`jobs`返回的数字）将进程切换到前台或终止它。

当你看到：

```py
The proxy has started successfully and is ready for new connections!
```

然后你可以使用 SQL 客户端或在你的应用程序中使用一些库（如 SQLAlchemy）连接到你的 SQL 实例。

对于 MySQL 客户端，命令是：

```py
mysql -h 127.0.0.1 -P 13306 -u DB_USERNAME -p
```

请注意，主机是`127.0.0.1`，即我们的本地主机，因为我们使用代理连接到我们的数据库。

如果你需要编写复杂的 SQL 查询，强烈推荐使用图形化工具，如 DBeaver，它是一个免费的通用数据库管理工具。它对语法高亮、自动补全以及许多其他出色功能有很好的支持。它就像 SQLAlchemy 的图形化版本。如果你还没试过，值得一试，你会发现它的表现优于大多数其他工具。

## 使用 Docker 启动 Cloud SQL Auth 代理

我们也可以使用 Docker 启动 Cloud SQL Auth 代理，这通常适用于你不想或不能安装`cloud-sql-proxy`时。

使用 Docker 启动 Cloud SQL Auth 代理非常简单，你只需按照这些命令操作：

```py
docker pull gcr.io/cloud-sql-connectors/cloud-sql-proxy:2.0.0

docker run -d \
  -v /local/path/to/service-account-key.json:/container/path/to/service-account-key.json \
  -p 127.0.0.1:3306:3306 \
  gcr.io/cloud-sql-connectors/cloud-sql-proxy:2.0.0 \
  --address 0.0.0.0 --port 3306 \
  --credentials-file /container/path/to/service-account-key.json INSTANCE_CONNECTION_NAME
```

请注意，`-p 127.0.0.1:3306`指定了 Cloud SQL Auth 代理不会暴露在本地主机之外，而`--address 0.0.0.0`用于使端口在 Docker 容器外部可访问。

直接使用 Docker 启动 Cloud SQL Auth 代理并不常见。更有用的方法是使用 Docker Compose，如我们将在下一节中看到的那样。

## 使用 Docker Compose 启动 Cloud SQL Auth 代理

通常我们不会使用 Docker 启动一个独立的 Cloud SQL Auth 代理容器。相反，我们将其与应用程序一起在 `docker-compose.yaml` 文件中指定。这样，我们的应用程序可以得到适当的 docker 化，并且所有开发人员可以在他们的计算机上与所需的数据库依赖项一起安装。

我们将看到如何在同一个 `docker-compose.yaml` 文件中放置 FastAPI 微服务和 Cloud SQL Auth 代理。

但首先，让我们看看如何仅为 Cloud SQL Auth 代理创建 `docker-compose.yaml` 文件，因为有一些细节需要先弄清楚。我们初始的 `docker-compose.yaml` 文件内容如下：

```py
version: "3.9"

services:
  cloudsql:
    image: gcr.io/cloud-sql-connectors/cloud-sql-proxy:2.0.0
    volumes:
      - type: bind
        source: ./service-account-key.json
        target: /service-account-key.json
        read_only: true
    ports:
      - target: 3306
        published: 13306
    command: --address 0.0.0.0 --port 3306 --credentials-file /service-account-key.json glass-core-xxxxxx:europe-west1:gs-mysql
```

如我们所见，设置与直接使用 Docker 时的设置相同。请注意，对于 `command` 键，我们不应指定 `cloud-sql-proxy` 命令，因为它会在 Docker 容器内自动指定。

然后，我们可以通过以下命令启动 `cloudsql` 服务：

```py
docker-compose up -d
```

通常作为开发人员，我们会有权限使用个人 Google 帐户访问我们的数据库。如果你已经通过 `gcloud auth login` 登录到 GCP，你只需将 `~/.config/gcloud` 文件夹绑定到容器中，不需要提供服务帐号密钥文件：

```py
version: "3.9"

services:
  cloudsql:
    image: gcr.io/cloud-sql-connectors/cloud-sql-proxy:2.0.0
    volumes:
      - type: bind
        source: ~/.config/gcloud
        target: /home/nonroot/.config/gcloud
        read_only: true
    ports:
      - target: 3306
        published: 13306
    command: --address 0.0.0.0 --port 3306 glass-core-xxxxxx:europe-west1:gs-mysql
```

这是本地开发的推荐方式，因为使用服务帐号密钥文件有泄露的风险，并可能导致安全问题。

然而，如果你直接使用这个 `docker-compose.yaml` 文件启动 `cloudsql` 服务，通常会看到这个错误：

```py
open /home/nonroot/.config/gcloud/application_default_credentials.json: permission denied
```

原因是我们将本地主机上的 `~/.config/gcloud` 文件夹绑定到容器中的一个文件夹，该文件夹将被 `nonroot` 用户使用。因此，我们需要更改 `~/.config/gcloud` 文件夹的权限，以便容器中的 `nonroot` 用户可以访问：

```py
find ~/.config/gcloud/ -type d | xargs -I {} chmod 755 {}
find ~/.config/gcloud/ -type f | xargs -I {} chmod 644 {}
```

这两个命令使 `~/.config/gcloud/` 文件夹及其子文件夹以及所有内容对其他用户（非拥有者或组用户）可访问。如果你想了解更多关于这些命令的信息，请查看 [这篇文章](https://levelup.gitconnected.com/some-linux-commands-that-can-boost-your-work-efficiency-dramatically-9dc802a10618)。

如果你重新启动服务，一切应该能正常工作。

现在让我们将我们的 FastAPI 应用程序添加到 `docker-compose.yaml` 文件中：

```py
version: "3.9"

services:
  my-app:
    build:
      context: ./app
    image: my-app:latest
    ports:
      - target: 80
        published: 8080
    networks:
      - my-app
    volumes:
      - type: bind
        source: ./app
        target: /app
    env_file:
      - ./secrets.env
    environment:
      - PYTHONPATH=/app:.:..
      - DB_NAME=app
      - DB_HOST=cloudsql
      - DB_PORT=3306
    depends_on:
      - cloudsql

  cloudsql:
    image: gcr.io/cloud-sql-connectors/cloud-sql-proxy:2.0.0
    volumes:
      - type: bind
        source: ~/.config/gcloud
        target: /home/nonroot/.config/gcloud
        read_only: true
    ports:
      - target: 3306
        published: 13306
    networks:
      - my-app
    command: --address 0.0.0.0 --port 3306 glass-core-xxsxxx:europe-west1:gs-mysql

networks:
  my-app:
    name: my-app
    driver: bridge
```

`docker-compose.yaml` 文件的关键点：

1.  应用程序和 Cloud 代理服务必须在同一网络中。否则，应用程序无法访问数据库。

1.  一些非敏感的环境变量通过 `environment` 键以明文设置，而敏感的环境变量则通过秘密文件 `secrets.env` 设置。此文件不应添加到版本控制的代码库中。

1.  应用程序被设置为依赖于 Cloud 代理，以便在应用程序运行时数据库始终可访问。

1.  一个特殊的环境变量 `PYTHONPATH` 被设置为 `/app:.:..`，以便更容易通过相对或绝对路径导入模块。

我们可以使用 Pydantic 来读取应用程序中的环境变量，这非常方便：

```py
# app/db/db.py
from pydantic import BaseSettings, Field, SecretStr
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import scoped_session, sessionmaker, Session
from sqlalchemy.schema import MetaData

# Create a base for SQLAlchemy mappers.
Base = declarative_base(metadata=MetaData(schema="app"))
metadata = Base.metadata

# A Pydantic model to get environment variables.
class DbSettings(BaseSettings):
    """Settings for SQL."""

    host: str = Field(..., env="DB_HOST")
    user: str = Field(..., env="DB_USERNAME")
    password: SecretStr = Field(..., env="DB_PASSWORD")
    port: int = Field(env="DB_PORT", default=3306)
    db_name: str = Field(env="DB_NAME", default="app")

# We need to create an instance of the Pydantic model to access the
# environment variables.
db_settings = DbSettings()

db_conn_url = (
    "mysql+pymysql://"
    f"{db_settings.user}:{db_settings.password.get_secret_value()}"
    f"@{db_settings.host}:{db_settings.port}/{db_settings.db_name}"
)

# Create SQLAlchemy SQL engine and session factory.
engine = create_engine(db_conn_url)
session_factory = sessionmaker(bind=engine)
scoped_session_factory = scoped_session(session_factory)

def get_db_sess():
    """Get a SQLAlchemy ORM Session instance.

    Yields:
        A SQLAlchemy ORM Session instance.
    """
    db_session: Session = scoped_session_factory()
    try:
        yield db_session
    except Exception as exc:
        db_session.rollback()
        raise exc
    finally:
        db_session.close()
```

上述代码可以作为任何 Python 微服务的模板。

在我们的简单应用中，我们可以通过[FastAPI](https://levelup.gitconnected.com/build-apis-with-fastapi-in-python-all-essentials-you-need-to-get-started-6bf9fa90c6b8)依赖注入来读取用户数据：

```py
# app/main.py
from fastapi import Depends, FastAPI
from sqlalchemy.orm import Session

from app.db.db import get_db_sess
from app.db.models.users import User as UserModel
from app.schema.users import User as UserSchema

app = FastAPI()

@app.get("/users")
async def get_users(
    db_sess: Session = Depends(get_db_sess),
) -> list[UserSchema]:
    users = db_sess.query(UserModel).all()

    return users
```

[这个 GitHub 仓库](https://github.com/lynnkwong/cloudsql-proxy-demo)包含了本篇文章的所有代码。如果你想深入了解代码，建议查看相关的文章，比如[Pydantic](https://lynn-kwong.medium.com/how-to-use-pydantic-to-read-environment-variables-and-secret-files-in-python-8a6b8c56381c)、[SQLAlchemy](https://levelup.gitconnected.com/learn-the-basics-and-get-started-with-sqlalchemy-orm-from-scratch-66c8624b069)和[FastAPI](https://levelup.gitconnected.com/build-apis-with-fastapi-in-python-all-essentials-you-need-to-get-started-6bf9fa90c6b8)。这些都是非常受欢迎的 Python 开发者库，非常值得加入你的工具集。

本文介绍了如何在裸机或虚拟机上直接使用 Cloud SQL Auth 代理。介绍了不同的认证方法，包括使用默认认证和服务账号密钥文件。

我们还介绍了如何使用 Docker 直接启动 Cloud SQL Auth 代理。然而，由于其不可移植，不推荐使用这种方法，因此难以与其他开发者共享设置。更好的方式是使用`docker-compose.yaml`文件将我们的 Docker 化应用程序连接到 Cloud SQL 实例。

提供了一个简单但实用的 Cloud SQL Auth 代理在实际项目中使用的例子，可以作为你自己项目的模板。

## 相关文章：

+   如何在 Cloud Run 服务中连接到 GCP Cloud SQL 实例

+   [使用 FastAPI 构建 Python API — 所有启动所需的基本知识](https://levelup.gitconnected.com/build-apis-with-fastapi-in-python-all-essentials-you-need-to-get-started-6bf9fa90c6b8)
