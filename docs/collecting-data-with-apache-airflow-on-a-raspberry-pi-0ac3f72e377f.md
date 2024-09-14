# 在 Raspberry Pi 上使用 Apache Airflow 收集数据

> 原文：[`towardsdatascience.com/collecting-data-with-apache-airflow-on-a-raspberry-pi-0ac3f72e377f`](https://towardsdatascience.com/collecting-data-with-apache-airflow-on-a-raspberry-pi-0ac3f72e377f)

## 一个 Raspberry Pi 就能满足你的需求

[](https://dmitryelj.medium.com/?source=post_page-----0ac3f72e377f--------------------------------)![Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----0ac3f72e377f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----0ac3f72e377f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----0ac3f72e377f--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----0ac3f72e377f--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----0ac3f72e377f--------------------------------) ·8 分钟阅读·2023 年 10 月 21 日

--

![](img/1fdf15ccbedadf88e443d6b5a1fc8a14.png)

Raspberry Pi Zero（2021 型号），图片来源 [维基百科](https://en.wikipedia.org/wiki/Raspberry_Pi)

通常，我们需要在一定时间内收集一些数据。这些数据可以来自物联网传感器、社交网络的统计数据或其他来源。举个例子，[YouTube Data API](https://developers.google.com/youtube/v3) 允许我们获取任何频道当前的观看次数和订阅者数量，但分析和历史数据仅对频道所有者可用。因此，如果我们想要获取这些频道的每周或每月总结数据，我们需要自己收集这些数据。对于物联网传感器，可能根本没有 API，我们也需要自己收集和保存数据。在这篇文章中，我将展示如何在 Raspberry Pi 上配置 Apache Airflow，这样可以在长时间内运行任务，而无需依赖任何云服务提供商。

显然，如果你在一家大公司工作，你可能不需要 Raspberry Pi。在这种情况下，如果你需要额外的云实例，只需为你的 MLOps 部门创建一个 Jira 工单即可 ;) 但对于一个小项目或低预算的创业公司，它可能是一个有趣的解决方案。

让我们看看它是如何工作的。

## Raspberry Pi

那么 Raspberry Pi 到底是什么？对于那些在过去 10 年里从未对硬件感兴趣的读者（第一款 Raspberry Pi 模型是在 2012 年推出的），我可以简要地解释一下，这是一款运行完整 Linux 系统的单板计算机。通常，Raspberry Pi 配备有 1GHz 的 2–4 核 ARM CPU 和 1–8 MB 的 RAM。它小巧、便宜且安静；没有风扇，也没有磁盘驱动器（操作系统从 Micro SD 卡运行）。Raspberry Pi 只需要一个标准的 USB 电源；它可以通过 Wi-Fi 或以太网连接到网络，并可以在几个月甚至几年内运行各种任务。

对于我的数据科学小项目，我想在 2 周内收集 YouTube 频道统计信息。对于一个每天只需 30-60 秒的任务，无服务器架构可以是一个完美的解决方案，我们可以使用类似[Google Cloud Function](https://cloud.google.com/functions?hl=en)的服务。但 Google 的每个教程都以“启用项目计费”开始。Google 提供了免费首信用和免费配额，但我不想再有另一个监控花费和请求量的麻烦，因此决定使用[Raspberry Pi](https://en.wikipedia.org/wiki/Raspberry_Pi)。结果发现 Raspberry Pi 是一个出色的数据科学工具，用于收集数据。这台$50 的单板计算机仅消耗 2W 的功率；它体积小，安静，可以放在任何地方。最后但同样重要的是，我家里已经有了一台 Raspberry Pi，因此我的成本几乎为零。我只需将电源插入插座，云计算问题就解决了 ;)

市场上有不同的 Raspberry Pi 型号，在撰写本文时，Raspberry Pi 4 和 Raspberry Pi 5 是最强大的。但对于不需要大量请求或“重”后处理的任务，最早的型号也能完成工作。

## Apache Airflow

对于我的小项目，我有一个包含 3000 个 YouTube 频道的列表，而 YouTube 数据 API 限制为每天 10000 个请求。因此，我决定每天收集数据两次。如果我们想要每天运行 Python 代码两次，可以使用 CRON 作业或仅在主应用循环中添加`time.sleep(12*60*60)`。但一种更有效和有趣的方法是使用[Apache Airflow](https://airflow.apache.org)。Apache Airflow 是一个专业的工作流管理平台，它也是开源的，且免费使用。

使用*pip*命令在 Raspberry Pi 上安装 Airflow 很简单（这里，我使用了 Apache Airflow 2.7.1 和 Python 3.9）：

```py
sudo pip3 install "apache-airflow==2.7.1" --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.7.1/constraints-3.9.txt"
```

一般不建议使用*sudo*与 pip，但在我的 Raspberry Pi 上，当我将 airflow 作为服务启动时（服务以 root 身份运行），找不到 Python airflow 库，使用`sudo pip3`是最简单的解决方法。

安装 Apache Airflow 后，我们需要初始化它并创建一个用户：

```py
cd ~
mkdir airflow && cd airflow
export AIRFLOW_HOME=/home/pi/airflow
airflow db init
airflow users create --role Admin --username airflow --password airflow --email admin --firstname admin --lastname admin
mkdir dags
```

现在，Airflow 已安装，但我希望它在启动后自动作为服务运行。

首先，我创建了一个`/etc/systemd/system/airflow-webserver.service`文件，将 Apache Airflow 网络服务器作为服务运行：

```py
[Unit]
Description=Airflow webserver daemon
After=network.target postgresql.service mysql.service redis.service rabbitmq-server.service
Wants=postgresql.service mysql.service redis.service rabbitmq-server.service

[Service]
EnvironmentFile=/home/pi/airflow/env
User=pi
Group=pi
Type=simple
ExecStart=/bin/bash -c 'airflow webserver --pid /home/pi/airflow/webserver.pid'
Restart=on-failure
RestartSec=5s
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

以相同的方式，我为 Airflow 调度器创建了一个`/etc/systemd/system/airflow-scheduler.service`文件：

```py
[Unit]
Description=Airflow scheduler daemon
After=network.target postgresql.service mysql.service redis.service rabbitmq-server.service
Wants=postgresql.service mysql.service redis.service rabbitmq-server.service

[Service]
EnvironmentFile=/home/pi/airflow/env
User=pi
Group=pi
Type=simple
ExecStart=/usr/bin/bash -c 'airflow scheduler'
Restart=always
RestartSec=5s

[Install]
WantedBy=multi-user.target
```

我们还需要一个`/home/pi/airflow/env`文件：

```py
AIRFLOW_CONFIG=/home/pi/airflow/airflow.cfg
AIRFLOW_HOME=/home/pi/airflow/
```

现在，我们可以启动新服务，Apache Airflow 已准备好使用：

```py
sudo systemctl daemon-reload
sudo systemctl enable airflow-webserver.service
sudo systemctl enable airflow-scheduler.service
sudo systemctl start airflow-webserver.service
sudo systemctl start airflow-scheduler.service
```

如果一切操作正确，我们可以登录到 Apache Airflow 网页面（凭据为“airflow”，“airflow”）：

![](img/80ebada6c41b3cba46938cbe614a3b1c.png)

Apache Airflow 登录页面，图像由作者提供

## Apache Airflow DAG

DAG（有向无环图）是 Apache Airflow 的核心概念。将不同的任务组合成一个图形可以让我们组织相当复杂的数据处理管道。DAG 本身是以 Python 文件的形式创建的，必须放置在 Apache Airflow 的“dags”文件夹中（该路径在“airflow.cfg”文件的 `dags_folder` 参数中指定）。在启动时或按下“刷新”按钮后，Apache Airflow 会导入这些 Python 文件，并从中获取所有所需的信息。

在我的例子中，我创建了一个位于 `get_statistics.py` 文件中的 `process_channels` 方法：

```py
from pyyoutube import Api

def process_channels(requests_limit: int,
                     data_path: str):
    """ Get data for YouTube channels and save it in CSV file """
    ...
```

（获取 YouTube 数据本身超出了本文的范围；它可以是我们想要定期运行的任何方法）

用于在 Apache Airflow 中运行我们代码的 DAG 文件非常简单：

```py
from airflow import DAG
from airflow.decorators import task
from airflow.models import Variable
from datetime import datetime, timedelta

data_path = "/home/pi/airflow/data/"

default_args={
        "depends_on_past": False,
        "email": [],
        "email_on_failure": False,
        "email_on_retry": False,
        "retries": 1,
        "retry_delay": timedelta(minutes=60),
}

def create_dag():
    """ Create a DAG object """
    return DAG(
        "dag_youtube",
        default_args=default_args,
        description="YouTube Retreive",
        schedule_interval=timedelta(hours=12),
        start_date=datetime(2021, 1, 1),
        catchup=False,
        tags=["youtube"]
    )

@task(task_id="collect_channels_stats_gr1")
def get_channels_stats_gr1():
    import get_statistics as gs
    limit = int(Variable.get("RequestLimit"))
    ret = gs.process_channels(limit, data_path)
    return f"GR1: {ret} channels saved"

@task(task_id="collect_channels_stats_gr2")
def get_channels_stats_gr2():
    import get_statistics as gs
    limit = int(Variable.get("RequestLimit"))
    ret = gs.process_channels(limit, data_path)
    return f"GR2: {ret} channels saved"

@task(task_id="collect_channels_stats_gr3")
def get_channels_stats_gr3():
    import get_statistics as gs
    limit = int(Variable.get("RequestLimit"))
    ret = gs.process_channels(limit, data_path)
    return f"GR3: {ret} channels saved"

# Create the DAG
with create_dag() as dag:    
    get_channels_stats_gr1()
    get_channels_stats_gr2()
    get_channels_stats_gr3()
```

如我们所见，我在这里创建了 `create_dag` 方法，其中最重要的部分是 `schedule_interval` 参数，它等于 12 小时。总的来说，我的 DAG 中有 3 个任务；它们由三个几乎相同的 `get_channels_stats_gr1..3` 方法表示。每个任务都是隔离的，将由 Apache Airflow 单独执行。我还创建了一个变量 `RequestLimit`。YouTube API 每天的请求限制为 10,000 次，在调试过程中，将此参数设置为较低的值是有意义的。稍后，可以通过使用 Apache Airflow 的“变量”控制面板随时更改此值。

## 运行 DAG

我们的任务已经准备好了。我们可以按“刷新”按钮，一个新的 DAG 将出现在列表中，并按照我们编程的时间表执行。如我们所见，安装 Apache Airflow 和创建 DAG 并不是火箭科学，但仍然需要一些努力。这是为了什么？我们能否仅仅在 CRON 作业中添加一行呢？即使是像这样的简单任务，Apache Airflow 也提供了大量功能。

+   我们可以看到任务状态、完成和失败任务的数量、下次运行的时间以及其他参数：

![](img/7bc63c5d369514c89ce799db9c8924c6.png)

Apache Airflow DAGs 列表，图片来源于作者

+   如果任务失败，可以轻松点击它，查看发生了什么以及事件发生的时间：

![](img/cddd1aa7a71d6aa8acb0c1d1c622850b.png)

Apache Airflow 图形视图，图片来源于作者

+   我甚至可以点击失败的任务，查看其崩溃日志。在我的例子中，崩溃发生在检索 YouTube 频道数据时。一个频道可能已经被所有者删除或禁用，因此不再有数据可用：

![](img/c2ff793efdaf431bf50a62a2c6827648.png)

Apache Airflow 崩溃日志，图片来源于作者

+   我可以看到一个日历，详细记录了之前和未来任务的日志：

![](img/7d41ddef5e1420f8432cd094348f5677.png)

Apache Airflow 日历视图，图片来源于作者

+   我还可以看到一个持续时间日志，这可以提供有关任务执行时间的一些见解：

![](img/f015abdd10f476e8d8454940b90f1b11.png)

Apache Airflow 持续时间日志，图片来源于作者

因此，与添加简单的 CRON 作业相比，使用 Apache Airflow 在功能上要好得多。最后但同样重要的是，掌握 Airflow 也是一个在行业中常常需要的不错技能 ;)

## 结论

在这篇文章中，我安装并配置了 Apache Airflow，并成功在 Raspberry Pi 上运行它。Apache Airflow 是一个专业的工作流管理平台；根据 [6sense.com](https://6sense.com/tech/workflow-automation/apache-airflow-market-share)，它拥有 29% 的市场份额，被 Ubisoft、SEB 或 Hitachi 等大型公司使用。但正如我们所见，即使在“nano”规模上，Apache Airflow 也可以成功地在像 Raspberry Pi 这样的微型计算机上使用。

对于有兴趣在 Raspberry Pi 上进行数据科学相关项目的读者，欢迎阅读我的其他 TDS 文章：

+   MEMS 传感器数据的探索性分析

+   在 Raspberry Pi 上的 YOLO 目标检测

如果你喜欢这个故事，可以随时 [订阅](https://medium.com/@dmitryelj/membership) Medium，你将收到我的新文章发布通知，并能完全访问其他作者的成千上万的故事。如果你想获取本文及我下一篇文章的完整源代码，可以访问我的 [Patreon 页面](https://www.patreon.com/deliuseev)。

感谢阅读。
