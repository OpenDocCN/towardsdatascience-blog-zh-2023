# 将 Apache Zeppelin 和 Apache Spark 容器化以便于部署

> 原文：[`towardsdatascience.com/dockerizing-apache-zeppelin-and-apache-spark-for-easy-deployment-1814d0c1c245`](https://towardsdatascience.com/dockerizing-apache-zeppelin-and-apache-spark-for-easy-deployment-1814d0c1c245)

## 学习如何使用 Docker-Compose 和卷构建一个可移植和可扩展的数据分析环境。

[](https://anbento4.medium.com/?source=post_page-----1814d0c1c245--------------------------------)![Antonello Benedetto](https://anbento4.medium.com/?source=post_page-----1814d0c1c245--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1814d0c1c245--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1814d0c1c245--------------------------------) [Antonello Benedetto](https://anbento4.medium.com/?source=post_page-----1814d0c1c245--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1814d0c1c245--------------------------------) ·阅读时长 9 分钟·2023 年 1 月 24 日

--

![](img/dcd662fabf5ce0f263fe35015b17886d.png)

照片由 [Tom Fisk](https://www.pexels.com/@tomfisk/) 提供，刊登在 [Pexels](https://www.pexels.com/photo/birds-eye-view-photo-of-freight-containers-2226458/) 上

## 按需课程 | 推荐

*我的一些读者联系我，询问是否有按需课程可以学习更多关于* ***Apache Spark 与 Python*** 的内容。这是我推荐的三个很棒的资源：*

+   [**Apache Kafka 和 Apache Spark 数据流处理纳米学位 (UDACITY)**](https://imp.i115008.net/zaX10r)

+   [**数据工程纳米学位 (UDACITY)**](https://imp.i115008.net/zaX10r)

+   [**PySpark 大数据与 Python (UDEMY)**](https://click.linksynergy.com/deeplink?id=533LxfDBSaM&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fspark-and-python-for-big-data-with-pyspark%2F)

*还不是 Medium 会员？考虑使用我的* [*推荐链接*](https://anbento4.medium.com/membership) *注册，您只需每月支付 5 美元即可访问 Medium 提供的所有内容！*

# 介绍

Docker 革新了我们部署和管理应用程序及数据分析工具的方式。它允许轻松扩展，并且可以根据特定需求轻松定制服务。

在本教程中，我将向你展示如何使用`docker-compose.yaml`文件快速部署 Apache Zeppelin 和 Apache Spark，并利用卷来管理服务之间的数据依赖。

> **下面展示的解决方案的优势在于，它不需要将任何额外的指令作为** `**Dockerfile**`**的一部分。**

即使是经验有限的 Docker 用户，也会发现这个模板是部署*易于维护*容器的宝贵起点。

使用这种方法，你将能够创建一个便携且可扩展的数据分析环境，减少维护需求，并且可以自定义使用你首选的 Spark 镜像。

这使得它特别适合数据工程师、数据科学家和机器学习工程师，他们需要处理大数据集，进行探索性分析和训练模型。

****注意：** 在接下来的部分中，我假设你已经在机器上安装了 Docker（*否则你可能会发现* [*官方文档*](https://docs.docker.com/engine/install/) *很有用*），并且熟悉与容器交互的 Docker 基本命令（*否则请参考* *这篇文章*）。**

# 1\. Docker-Compose 文件

以下代码（*也可以在* [*GitHub*](https://github.com/anbento0490/docker/blob/master/zeppelin_spark/docker-compose.yaml) *找到），是一个 `docker-compose.yaml` 文件的示例，可以用来在 **Apache Spark** 独立模式下运行 **Apache Zeppelin**：

```py
version: '3'

services:
  spark-master:
    image: bde2020/spark-master:3.1.1-hadoop3.2
    container_name: spark-master
    ports:
      - "8080:8080"
      - "7077:7077"
    environment:
      - INIT_DAEMON_STEP=setup_spark
    volumes:
      - spark_volume:/spark
  spark-worker-1:
    image: bde2020/spark-worker:3.1.1-hadoop3.2
    container_name: spark-worker-1
    depends_on:
      - spark-master
    ports:
      - "8081:8081"
    environment:
      - "SPARK_MASTER=spark://spark-master:7077"
      - "SPARK_WORKER_CORES=2"
      - "SPARK_WORKER_MEMORY=4g"
  zeppelin:
    image: apache/zeppelin:0.10.1
    container_name: zeppelin
    depends_on:
      - spark-master
    ports:
      - "8082:8080"
    volumes:
      - ./zeppelin/notebook:/opt/zeppelin/notebook
      - ./zeppelin/conf:/opt/zeppelin/conf
      - spark_volume:/opt/zeppelin/spark
    environment:
      - "SPARK_HOME=/opt/zeppelin/spark"
      - "SPARK_MASTER=spark://spark-master:7077"
volumes:
    spark_volume:
```

通过运行命令 `docker-compose up -d`，这个文件将启动三个服务：

+   一个 `SPARK-MASTER`（`version=3.1.1`），负责管理 Spark 集群。这个服务将在 `port 7077` 运行，用户界面可在 [`localhost:8080/`](http://localhost:8080/) 访问。

+   一个 `SPARK-WORKER-1`，实际上是一个分配了 2 个核心和 4GB 内存的工作节点，通过端口 `7077` 连接到 `SPARK-MASTER`，其用户界面可在 [`localhost:8081/`](http://localhost:8080/) 访问。

用于 `SPARK-MASTER` 和 `SPARK-WORKER` 的镜像由 [bde2020](https://github.com/big-data-europe/docker-spark) 管理，并可从 [DockerHub](https://hub.docker.com/r/bde2020/spark-master/tags) 下载。

+   一个`ZEPPELIN` 基于 web 的笔记本（`version=10.1`），设置为在独立的 `SPARK-MASTER` 上运行代码，通过 `SPARK-WORKER-1`，其用户界面可在 [`localhost:8082/`](http://localhost:8080/) 访问。

要验证所有服务是否正常运行，请执行：

```py
docker ps --format "{{.ID}}\t{{.Names}}\t{{.Status}}"
```

这个命令应该返回 `container ID`、`container name` 和 `current status`：

```py
02a84456d38d zeppelin        Up 18 minutes
1f9b683dd027 spark-worker-1  Up 18 minutes
0a482bfc43f3 spark-master    Up 18 minutes
```

# 2\. 访问 Docker 容器

在此阶段，假设 `docker-compose.yaml` 文件中的所有服务都在运行，它们的用户界面应当可以在端口映射配置中指定的主机地址上访问。

不过，在检查用户界面样式之前，让我们访问 `SPARK-MASTER` 并验证它确实在 `port 7077` 上可用，并且 PySpark 命令可以在其集群上执行。

要在运行中的`SPARK-MASTER`容器内执行命令，通过 bash shell，你可以使用：

```py
docker exec -it spark-master /bin/bash
```

> 这是一个极其有用的命令，用于排查容器问题，检查日志，和对容器的文件系统或运行进程进行更改。

在这个阶段，你应该能够启动一个 PySpark shell，连接到 `port 7077` 上的 `spark-master` 服务，通过运行：

```py
/spark/bin/pyspark --master spark://spark-master:7077
```

如果一切正常，你应该会看到一个 Python shell 提示符，并且可以尝试在集群上运行一些命令（*例如创建一个简单的 DF*），如下所示：

```py
data = [(1, “John”, “Doe”), (2, “Jane”, “Doe”), (3, “Bob”, “Smith”), \ 
        (4, “Alice”, “Johnson”), (5, “Charlie”, “Brown”), (6, “David”, “Jones”),\
        (7, “Eve”, “White”), (8, “Fred”, “Garcia”), (9, “Gina”, “Green”),\
        (10, “Harry”, “Harris”)]

df = spark.createDataFrame(data, ["id", "first_name", "last_name"])

df.show()
```

![](img/f512a3ebf623676a56ab68306f74e3a7.png)

成功！通过上一步，你验证了 `spark-master` 服务可以访问并能够在独立模式下运行 Spark 应用程序（要退出交互式 Python 提示符，请输入 `exit()` 然后按 `CTRL + D`）。

实际上，如果你现在访问主机 [`localhost:8080/`](http://localhost:8080/)，你会注意到在 `Running Applications` 部分下出现了一个 `PySparkShell` 应用程序：

![](img/f785bea04ef0e015f7733eb3b34eff75.png)

刚刚执行的创建 DF 的 PySpark 任务的详细信息，也由主机上的 `spark-worker` 汇总，主机地址为 [`localhost:8081/`](http://localhost:8080/)：

![](img/67a5ce824f27a639070f9aa363e50d14.png)

好极了！Spark 正在工作！

然而，下一步是确保 `ZEPPELIN` 服务也能与 `SPARK-MASTER` 进行交互，并在基于 `version=3.1.1` 的相同独立集群上运行命令。

通过访问主机 [`localhost:8082/`](http://localhost:8080/)，你可以验证 `ZEPPELIN` 是否确实在运行，并且可以创建新的笔记本（*例如，我创建了一个名为* `PySpark Project 1` 的笔记本）并运行 Spark 命令。

![](img/c3bce0c64f1d2fc68b98c7da6bc0b269.png)

默认情况下，代码将 *在本地* 执行，而不是在独立集群上，使用 Zeppelin 的标准解释器。

要改变这种行为，你需要熟悉 Docker 的 *卷* 和 *环境变量* 的概念。

****请注意：** 与 `SPARK-MASTER` 一样，`ZEPPELIN` 容器也可以通过运行 `docker exec -it zeppelin /bin/bash` 进行访问。你可能想要探索的主要路径是 `/opt/zeppelin/`，对于 `version=10.1`。

# 3\. Docker 卷

Docker 卷是一种将数据存储在容器文件系统之外的方式。这允许数据在容器被删除或重建时依然得以保留。

返回到 `docker-compose.yaml` 文件，你可以验证在容器运行时是否创建了以下卷：

```py
#SPARK-MASTER SERVICE
volumes:
  - spark_volume:/spark
#ZEPPELIN SERVICE
 volumes:
  - ./zeppelin/notebook:/opt/zeppelin/notebook
  - ./zeppelin/conf:/opt/zeppelin/conf
  - spark_volume:/opt/zeppelin/spark
#NAMED VOLUME - MANAGED BY DOCKER
volumes:
    spark_volume:
```

+   首先，我创建了一个所谓的 *“命名卷”* `spark_volume` 在 `volumes` 部分，并将其映射到 `SPARK-MASTER` 容器文件系统中的 `/spark` 文件夹。

+   相同的 `spark_volume` 已经映射到 `ZEPPELIN` 文件系统中的 `/opt/zeppelin/spark` 目录。这实际上创建了一个新的 `/spark` 文件夹，包含了原始 `SPARK-MASTER` 文件系统中的所有内容。这个文件夹中，包括其他文件，还包含运行 Spark 应用程序所需的 `SPARK-SUBMIT` 可执行文件。

+   ` /opt/zeppelin/notebook`目录已映射到本地的`./zeppelin/notebook`文件夹。这允许你在本地创建和保存笔记本，以避免在使用`docker-compose down`删除容器或使用`docker restart <service-name>`重新启动容器时丢失它们。

+   最后，`/opt/zeppelin/conf`目录已经映射到本地的`./zeppelin/conf`。这是必要的，因为在下一步中，你需要在这个位置设置一些环境变量。

****请注意**：这四个基本卷是必需的，以避免 Zeppelin 笔记本中的应用程序/解释器错误。不过你可以根据需要创建更多的卷。******

现在，让我们深入了解一下所需的环境变量及其设置方法。

# 4\. Docker 环境变量

环境变量用于将配置选项传递给容器。查看`docker-compose.yaml`文件，最相关的环境变量如下：

```py
#SPARK-WORKER SERVICE
environment:
  - "SPARK_MASTER=spark://spark-master:7077"
  - "SPARK_WORKER_CORES=2"
  - "SPARK_WORKER_MEMORY=4g"
#ZEPPELIN SERVICE
environment:
  - "SPARK_HOME=/opt/zeppelin/spark"
  - "SPARK_MASTER=spark://spark-master:7077"
```

+   首先，对于`SPARK-WORKER`和`ZEPPELIN`服务，`SPARK_MASTER`变量已设置为`spark://spark-master:7077`，这确实是`SPARK-MASTER`服务的 URL。这使得这两个服务可以连接到独立的 Spark 集群。

+   同样，在`ZEPPELIN`服务中，`SPARK_HOME`变量已设置为`/opt/zeppelin/spark`，正如你在前一部分所记得的，这就是通过`spark_volume`创建的文件夹的路径（*包含* `SPARK-SUBMIT` *可执行文件*）。 ***正确设置这个变量是关键，如果你希望* `zeppelin` *应用程序在 Spark 独立模式下运行的话。***

+   对于`SPARK-WORKER`服务，我设置了`SPARK_WORKER_CORES=2`，这意味着 Spark Worker 将使用 2 个核心，并且`SPARK_WORKER_MEMORY=4g`，这意味着 Spark Worker 将有 4GB 的内存可用。你当然可以根据具体需求调整这些参数。

至于服务配置，这基本上就是你需要做的全部。事实上，你可以立即使用 Zeppelin 执行代码（*用 Scala 编写*），对 Spark 集群进行操作。

但是，为了避免在 Zeppelin 中出现与 PySpark 解释器相关的错误，你还应该在`zeppelin-env.sh`文件中导出两个额外的变量，`version=10.1`的文件必须在`/opt/zeppelin/conf`目录下创建和保留。

要设置环境变量，首先访问容器：

```py
docker exec -it zeppelin /bin/bash
```

然后运行这个 bash 脚本：

```py
cd /opt/zeppelin/conf 
&& echo "export PYSPARK_PYTHON=python3" >> zeppelin-env.sh 
&& echo "export PYSPARK_DRIVER_PYTHON=python3" >> zeppelin-env.sh
```

一旦创建并填充了`PYSPARK_PYTHON`和`PYSPARK_DRIVER_PYTHON`变量，`zeppelin-env.sh`的存在将确保 Zeppelin 中的 PySpark 解释器使用`python3`，在执行 PySpark 代码时避免与默认版本冲突。

# 5\. 在 Zeppelin 中执行代码

在卷和环境变量设置完成后，你现在可以测试 Scala 和 Python 命令在 Zeppelin 笔记本中是否按预期执行。

在保存了最近对`docker-compose`文件的更改后，重启`ZEPPELIN`容器：

```py
docker restart zeppelin
```

访问主机 [`localhost:8082/`](http://localhost:8080/) 并创建一个新笔记本或打开一个现有笔记本（*我将使用* `PySpark Project 1`）。现在，尝试执行以下代码：

```py
spark.version #this should return 3.1.1 like for bde2020 image

!python --version #this should return a version >= 3.7.0
```

到此，你可以尝试使用 PySpark 创建一个简单的数据框，就像我在第 2 步的 shell 中做的那样。输出应类似于此：

![](img/147e28d3b40e2b87978210e7157d4d3e.png)

同时，请注意`ZEPPELIN`应用在 Spark 集群上是如何分配资源的：

![](img/8bf796da98e77dc8ac79ece4992c7b6d.png)

做得很好！

# 结论

在本教程中，你已经学习了如何通过`docker-compose`文件部署**Apache Zeppelin**和**Apache Spark**的最新版本，而不需要任何额外的说明，通常这些说明是通过`Dockerfile`提供的。

相反，你利用了**卷**来管理和持久化数据依赖项，并使用**环境变量**来定制服务。这使得创建一个便于移植和可扩展的数据分析环境变得简单，几乎无需维护。

这个解决方案允许你快速启动一个**独立的 Spark 集群**，与一个或多个 Spark 工作节点互动以处理大型数据集，并使用 Apache Zeppelin 作为基于网页的笔记本来进行数据探索和可视化。

此外，你了解了如何利用环境变量来配置服务，以及如何在 Zeppelin 中设置**PySpark 解释器**，以避免错误。

总体而言，使用`docker-compose`来部署 Apache Zeppelin 和 Apache Spark 是管理数据分析环境的高效便捷方式。它允许轻松扩展，并能够根据具体需求定制服务。

这使得它成为**数据工程师**、**数据科学家**和**机器学习工程师**的绝佳选择，他们需要处理大型数据集、执行探索性分析并训练他们的模型。

# 资源

1.  [Docker Engine 安装概述](https://docs.docker.com/engine/install/)

1.  [Docker 文档 | 卷](https://docs.docker.com/storage/volumes/)

1.  [Docker 文档 | Compose 中的环境变量](https://docs.docker.com/compose/environment-variables/)

1.  [DockerHub | bde2020 镜像](https://hub.docker.com/r/bde2020/spark-master/tags)

1.  [BDE Docker-Spark GitHub 仓库](https://github.com/big-data-europe/docker-spark)

1.  [Apache Zeppelin 的 Spark 解释器（版本 10.1）](https://zeppelin.apache.org/docs/latest/interpreter/spark.html)

1.  在 Zeppelin 中使用 PySpark 与 python3

1.  你应该知道的 12 个必备 Docker 命令
