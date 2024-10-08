# 如何在 AWS EMR 上使用 Airflow 自动化 PySpark 管道

> 原文：[`towardsdatascience.com/how-to-automate-pyspark-pipelines-on-aws-emr-with-airflow-a0cbd94c4516`](https://towardsdatascience.com/how-to-automate-pyspark-pipelines-on-aws-emr-with-airflow-a0cbd94c4516)

## 优化大数据工作流的编排。

[](https://anbento4.medium.com/?source=post_page-----a0cbd94c4516--------------------------------)![Antonello Benedetto](https://anbento4.medium.com/?source=post_page-----a0cbd94c4516--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a0cbd94c4516--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a0cbd94c4516--------------------------------) [Antonello Benedetto](https://anbento4.medium.com/?source=post_page-----a0cbd94c4516--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a0cbd94c4516--------------------------------) ·8 分钟阅读·2023 年 8 月 23 日

--

![](img/8d92446cdf5a73adbbd00d940c246dee.png)

图片由 [Tom Fisk](https://www.pexels.com/photo/trees-near-ocean-2246950/) 提供，来自 [Pexels](https://www.pexels.com/@tomfisk/)

# 按需课程 | 推荐

*我的一些读者联系过我，询问是否有按需课程来帮助你* ***成为*** *一名扎实的* ***数据工程师***。这些是我推荐的 3 个优秀资源：

+   [**数据工程微学位 (UDACITY)**](https://imp.i115008.net/zaX10r)

+   [**使用 Apache Kafka 和 Apache Spark 的数据流处理微学位** **(UDACITY)**](https://imp.i115008.net/zaX10r)

+   [**Spark 和 Python 大数据与 PySpark (UDEMY)**](https://click.linksynergy.com/deeplink?id=533LxfDBSaM&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fspark-and-python-for-big-data-with-pyspark%2F)

***还不是 Medium 会员？*** *考虑使用我的* [*推荐链接*](https://anbento4.medium.com/membership) *注册，以每月仅需 $5 的费用访问 Medium 的所有内容！*

# **简介**

在数据工程和分析的动态领域中，构建可扩展和自动化的管道至关重要。

对于已经在使用 Airflow 的 Spark 爱好者来说，可能会好奇：

> 如何使用 Airflow 在远程集群上执行 Spark 作业？
> 
> 如何使用 AWS EMR 和 Airflow 自动化 Spark 管道？

在本教程中，我们将通过展示如何集成这两种技术来进行演示：

1.  配置并从 Airflow UI 中获取必要的参数。

1.  创建辅助函数以自动生成首选的 `spark-submit` 命令。

1.  使用 Airflow 的 `EmrAddStepsOperator()` 方法构建一个任务，该任务提交并执行一个 PySpark 作业到 EMR。

1.  使用 Airflow 的 `EmrStepSensor()` 方法监控脚本执行。

本教程中使用的代码可在 [GitHub](https://github.com/anbento0490/projects/tree/main/airflow_emr_spark) 上获取。

# 先决条件

+   一个 [AWS 账户](https://aws.amazon.com/) 配置了一个 S3 存储桶和 EMR 集群 **在同一区域（** 在这种情况下是 `eu-north-1`**）。** EMR 集群应处于 `WAITING` 状态。在我们的案例中，它被命名为 `emr-cluster-tutorial`：

![](img/fa41a5326a1368fdd721eab8322a0987.png)

作者提供的照片（个人 EMR 集群）

+   一些模拟 `balances` 数据已经在 `S3` 存储桶的 `src/balances` 文件夹中。数据可以使用 [数据生成器](https://github.com/anbento0490/projects/blob/main/airflow_emr_spark/dags/assets/data_producer.ipynb) 脚本生成并写入该位置。

+   所需的`JARs`应已经从 Maven 下载并保存在 `S3` 存储桶中。

+   Docker 已安装并在本地计算机上运行，分配了 4-6 GB 的内存。

# 架构

目标是将一些模拟数据以 `parquet` 格式写入 `S3` 存储桶，然后构建一个 `DAG`，该 `DAG`：

+   从 Airflow UI 获取所需配置；

+   将 `pyspark` 脚本上传到同一个 `S3` 存储桶；

+   提交一个 Spark 作业，执行刚刚上传的脚本；

+   通过传感器监控执行状态，直到成功或失败。

![](img/cb0937a803dbb8aab7666d9aba107508.png)

作者提供的照片 ([`app.diagrams.net/`](https://app.diagrams.net/))

# 配置

## 克隆仓库

第一步是将 [仓库](https://github.com/anbento0490/projects/tree/main) 克隆到您选择的本地文件夹，并显示文件夹结构：

```py
git clone git@github.com:anbento0490/projects.git repo_local_name

cd repo_local_name/airflow_emr_spark

tree
```

`airflow_emr_spark` 文件夹结构如下：

+   `docker-compose.yml` 和 `Dockerfile.airflow` 文件是运行本地 Airflow 服务所需的，然后可视化/执行 `DAG`；

+   `orchestration` 文件夹包含名为 `submit_spark_job_to_emr.py` 的 `DAG` 本身；

+   `computation` 文件夹包含将提交到 `EMR` 集群的 `.py` 可执行文件；

+   最后，`assets` 文件夹包括了所需配置的 `JSON` 格式副本以及 `data_producer` 笔记本。

![](img/576805ef306cc6b546746d8e8c2bee6c.png)

文件夹结构（树状图）

## 在 Docker 上运行 Airflow

确保本地运行 `docker`，然后在 `airflow_emr_spark` 文件夹中执行 `docker compose up -d`。此命令将在我们的容器中启动 Apache Airflow：

![](img/b4fa8d3976b70f3852f763ee7307e9c9.png)

作者终端的截图

现在我们导航到 [**localhost:8091**](http://localhost:8091/home)，当请求凭证时，我们只需输入 `admin` 两次。此时，`DAG` 应该已经解析完毕，样子如下：

![](img/7ae030d73168254a63c6f1a7ff40d444.png)

从运行在 Docker 上的 Airflow

在探索 `DAG` 代码之前，在 Airflow 中还有两个剩余操作：

+   导航到 `Variables` 部分，创建一个名为 `dag_params` 的新变量，将 `dags/assets/dag_params.json` 文件的内容复制粘贴到其中。当然，`bucket_name` 应更新以匹配我们 AWS 账户中的 `S3` 存储桶。

![](img/85d32219e5d74342494bb1e9941f6eba.png)

AF UI 中的 **dag_params** 变量

+   导航到 `Connections` 部分，并创建一个名为 `aws_default` 的新 `Amazon Web Services` 连接，用于通过 Airflow 与 `S3` 和 `EMR` 集群进行交互。连接参数应以如下形式粘贴到 `Extra` 部分：

```py
{"aws_access_key_id": "xxxxxx",
 "aws_secret_access_key": "xxxxxx",
 "region_name": "eu-north-1"}
```

![](img/15e40305b44c74a5a85d438ea97a35b1.png)

AF UI 中的 **aws_default** 连接

# 任务

初步步骤作为 `submit_spark_job_to_emr.py` DAG 的一部分是从 Airflow UI 中之前保存的 `dag_params` 变量中检索配置。

如果变量不存在，`DAG` 将默认为文件夹中可用的 `dag_params.json`（`default_json`）。

由于本教程还展示了如何将特定的 `JARS` 提供给 `EMR` 集群，因此一开始我们还定义了一个 `jar_string` 变量以便在稍后添加步骤时作为参数提交。然而，如果不需要 `JARS`，则可以省略此变量（*以及* `spark_jars_conf` *和* `spark_jars_conf_value` *在* `dag_params` 中）：

![](img/2b0d75dca561f72f9bc037f4760af39e.png)

我们应该认识到，这不是在 Airflow 中导入变量的高效方法。

这是因为 `Variable.get()` 方法在任务之外被调用，这意味着变量在每次 `airflow scheduler` 解析 `dags` 文件夹时都会被导入 *每次*。

然而，为了学习目的，这种实现方式更简单，当只有一个管道涉及时并不算大问题。然而，在生产环境中，我们应确保遵循 [这些](https://airflow.apache.org/docs/apache-airflow/stable/best-practices.html#top-level-python-code) 最佳实践，总结来说建议：

+   *将变量作为任务的一部分并仅在需要时导入*；

+   *使用* `XCOMS` *在任务之间交换变量*。

`DAG` 包含四个任务。让我们逐一描述它们的功能。

## 获取集群 ID

第一个任务是 `fetch_cluster_id`：

```py
fetch_cluster_id = PythonOperator(
    dag=dag,
    task_id="fetch_cluster_id",
    python_callable=fetch_cluster_id,
)
```

这个任务的功能是运行 `fetch_cluster_id()` python 函数，该函数：

+   按名称获取并返回 `CLUSTER_ID`（*后续添加 EMR 步骤所需*），并期望 `cluster_state` 为 `WAITING` 或 `RUNNING`。在我们的例子中，我们将 `"emr-cluster-tutorial"` 传递给 `get_cluster_id_by_name` 方法 → 为了正常工作，连接中指定的 `region_name` 必须与创建 `EMR` 集群的区域匹配。

+   显示导入变量的值以便于查看（***建议**：*在生产中，此函数应修改为首先获取所有属性，以避免低效的顶级导入*）。

![](img/2f67301ca715bc5e062f1ac41ec9c873.png)

## 上传脚本到 S3

我们使用以下任务将 `read_and_write_back_to_s3.py` 可执行文件从本地 `computation/src/python_scripts` 文件夹上传到指定的 `s3://bucket_name/src/` 文件夹，然后提交到 `EMR` 集群：

```py
upload_script_to_s3 = PythonOperator(
    dag=dag,
    task_id="upload_script_to_s3",
    python_callable=upload_script_to_s3,
    op_kwargs={"filename": local_script, "key": s3_script}
)
```

函数 `upload_script_to_s3` 创建了一个 `S3Hook`，在 Airflow 和 AWS 之间使用 `aws_default` 连接，并使脚本在首选的 `S3` 路径下可用：

![](img/378ac533bd4a22047e1ff124f78c4c71.png)

## 执行 PySpark 脚本

现在 python 脚本已经存在于 `s3` 存储桶中，是时候使用 `EmrAddStepsOperator()` 向 `EMR` 添加一个步骤了。

实际上，`execute_pyspark_script` 利用下面的辅助函数，将一个 spark 应用程序以 `client` 模式部署到 `EMR`（*但在生产环境中* `cluster` *模式* [*应当更为推荐*](https://stackoverflow.com/questions/41124428/spark-yarn-cluster-vs-client-how-to-choose-which-one-to-use) *），同时使用从 `dag_params` 中获取的其他（**可选**）`spark_conf` 参数：

![](img/c118f04a777da9afce44131d98e05109.png)

实际上，`execute_pyspark_script` 任务指示 `EMR` 执行 `read_and_write_back_to_s3.py` 脚本（*在* `CLUSTER_ID` *通过第一个任务获取并通过* `XCOMS` *拉取*），并将多个参数提供给集群，顺序如下：

+   全局参数如 `jar_strings` 或 `packages` → 需要作为参数传递在 python 脚本之前（*尽管是可选的*）；

+   函数参数 `s3_input` → 指定了传递给 python 脚本的输入 `balances` 数据集 `S3` 路径；

+   函数参数 `s3_output` → 指定了存储输出数据集的 `S3` 路径。

```py
execute_pyspark_script = EmrAddStepsOperator(
    dag=dag,
    task_id='execute_pyspark_script',
    job_flow_id= "{{ ti.xcom_pull(task_ids='fetch_cluster_id', key='return_value') }}",
    aws_conn_id='aws_default',
    steps=[{
        'Name': 'execute_pyspark_script',
        'ActionOnFailure': 'CONTINUE',
        'HadoopJarStep': {
            'Jar': 'command-runner.jar',
            'Args': [
                *generate_spark_submit_command(spark_submit_cmd, spark_conf_map),
                "--jars",
                '{{ params.jars_string }}',
                's3://{{ params.bucket_name }}/{{ params.s3_script }}',
                '--s3_output',
                '{{ params.s3_output }}',
                '--s3_input',
                '{{ params.s3_input }}'
            ]
           }
        }],
    params={
        "bucket_name": BUCKET_NAME,
        "s3_script": s3_script,
        "s3_input": s3_input,
        "s3_output": s3_output,
        "jars_string": jars_string
    }
)
```

## 使用传感器监控执行

最后但同样重要的是，`execute_pyspark_script_sensor` 任务运行一个 `EmrStepSensor()`，该传感器定期 *探测* 集群本身，以验证应用程序的健康状况和执行状态。

它特别要求两个参数：`JOB_FLOW_ID == CLUSTER_ID` 和 `STEP_ID`，我们可以通过 `XCOMS` 获取这两个参数：

```py
execute_pyspark_script_sensor = EmrStepSensor(
    dag=dag,
    task_id="execute_pyspark_script_sensor",
    job_flow_id="{{ ti.xcom_pull(task_ids='fetch_cluster_id', key='return_value') }}",
    aws_conn_id="aws_default",
    step_id="{{ ti.xcom_pull(task_ids='execute_pyspark_script', key='return_value')[0] }}",
    sla=timedelta(minutes=5)
)
```

# 代码

python 可执行文件包括一个简单的 `process_data_and_write_to_s3()` 函数，该函数：

+   从 `s3_input` 路径读取 `parquet` 格式的数据到一个 PySpark SQL 视图中；

+   利用 SQL API 过滤初始数据集，并将结果保存到 `df_filtered` DF；

+   最终将过滤后的数据集（*再次以* `parquet` *格式*）写入 `s3_output` 路径。

![](img/07b7b60f2b0750373b8ae1d852a7d89e.png)

我们现在可以触发 `DAG` 并等待其成功执行：

![](img/1e6ea7f4f0a449da8829baec7e319586.png)

Airflow DAG 图

导航到 `EMR` 集群的 `Steps` 部分，我们应该能够验证脚本执行确实已完成：

![](img/526d4ca4746a172fc2a744cf13aaf920.png)

AWS EMR **步骤** 部分

实际上，通过选择 `stout` 日志，我们应该能看到类似于以下的消息，它告诉我们一个包含 `~2.5M` 行（*从初始的* `10M` *中筛选出来*）的 DF 已经被写入 `tgt/balances/` 文件夹中：

```py
Start process...
Reading data from s3://emr-data-947775527574/src/balances directory and creating Temp view
Creating DF with only balances for Company_A
Writing DF to s3://emr-data-947775527574/tgt/balances/ directory...
Data written to target folder...
DF rows count is 2499654
PROCESS COMPLETED!
```

实际上，检查 `bucket_name/tgt/balances/` 文件夹时，我们应该找到一个或多个包含与 `Company_A` 相关的数据的 `parquet` 文件（*如筛选条件中指定的*）：

![](img/4e8798a2a8a45f1426bf90917afc18b2.png)

**tgt/balances** 文件夹的内容

# 结论

在本教程中，我们学习了如何使用 Apache Airflow 和 AWS EMR 结合来自动化 PySpark 流水线。

这里的假设是**EMR 集群始终处于闲置状态，等待添加步骤**。在专业环境中这种情况并不罕见，或者通过设置个人 AWS 账户也能轻松实现。

另一种常见的做法是直接从 AF UI 中获取配置，并使用辅助功能生成更复杂的提交命令，这些命令可以通过按需编辑配置来更新。

我们使用的是一个简单的 PySpark 脚本，但我们当然可以设想扩展这个示例，以涵盖更复杂的使用案例。

一旦完成工作，请记得使用以下命令停止 docker 上的 Airflow 服务：

```py
docker compose down 
```

# 来源

+   [Amazon EMR 入门](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-gs.html)

+   [如何从 Airflow 提交 Spark 作业到 EMR 集群](https://www.startdataengineering.com/post/how-to-submit-spark-jobs-to-emr-cluster-from-airflow/)

+   [创建 DAG 时的 Airflow 最佳实践](https://airflow.apache.org/docs/apache-airflow/stable/best-practices.html#top-level-python-code)

+   [Spark Yarn 集群与客户端 — 如何选择使用哪一个？](https://stackoverflow.com/questions/41124428/spark-yarn-cluster-vs-client-how-to-choose-which-one-to-use)
