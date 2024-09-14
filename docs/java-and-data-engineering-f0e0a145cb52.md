# Java 和数据工程

> 原文：[`towardsdatascience.com/java-and-data-engineering-f0e0a145cb52`](https://towardsdatascience.com/java-and-data-engineering-f0e0a145cb52)

## 数据工程

## Java Juggernaut: 数据工程掌握的关键

[](https://tamimi-naser.medium.com/?source=post_page-----f0e0a145cb52--------------------------------)![Naser Tamimi](https://tamimi-naser.medium.com/?source=post_page-----f0e0a145cb52--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f0e0a145cb52--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0e0a145cb52--------------------------------) [Naser Tamimi](https://tamimi-naser.medium.com/?source=post_page-----f0e0a145cb52--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0e0a145cb52--------------------------------) ·阅读时间 4 分钟·2023 年 11 月 11 日

--

![](img/beb492e0841f67c958a6cbd9ba1cd4b5.png)

图片由 [Zhen H](https://unsplash.com/@zhenh2424?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 数据工程和编程技能

当我们想到数据工程时，首先想到的编程技能通常是 SQL 和 Python。SQL 是一种用于查询数据的著名语言，深深根植于数据和管道的世界。另一方面，Python 在数据科学中变得相当强大，并且现在在不断发展的数据工程领域中也开始展现其影响力。但是，这种普遍的看法是否准确？SQL 和 Python 真的就是数据工程师最重要的编程技能吗？在本文中，我将分享我在这一领域的经验，旨在帮助年轻专业人士找出最佳技能，以充分利用他们的时间和精力。

# 为什么选择 Java 和 Scala？

在今天的数据工程中，我们处理大量的数据。主要的工作是搞清楚如何每天、每小时甚至实时地收集、改变和存储这些海量数据。更棘手的是，要确保不同的数据服务能够在各种系统上顺利运行，而不用担心底层发生了什么。

在过去的 15 年里，聪明的人们提出了分布式计算框架来应对数据过载问题。Hadoop 和 Spark 是这个领域的两个大名鼎鼎的名字。由于这两个框架主要使用 JVM（Java 虚拟机）语言构建（Hadoop 使用 Java，而 Spark 使用 Scala），许多数据和软件专家认为 Java 和 Scala 是数据工程的未来方向。

此外，JVM 应用程序的可移植性使它们成为跨各种系统和环境操作的数据应用的优秀选择。你可以开发无缝运行于各种云端和本地环境的数据管道，从而可以在不担心底层基础设施的情况下，按需扩展系统。

# 在基于 JVM 的应用程序中，数据管道是什么样的？

现在我们已经探讨了 Java 和 Scala，或者更广泛地说，基于 JVM 的数据应用在处理大数据方面的好处，接下来的逻辑问题是：这些应用程序，或者简单的数据管道，看起来是什么样的？本节旨在概述这些应用程序的架构。

首先，必须在 Java 或 Scala 中开发一个数据管道。通常，多个相关的数据管道可以共存于同一个 Java 或 Scala 项目中。为了有效的项目管理，可以使用像**Apache Maven**这样的工具。Maven 简化了 Java 应用程序的创建、管理和构建过程，使这一过程更加高效和可靠。

在这些项目中，数据管道通常包括一个或多个 Java 或 Scala 类。Spark 通常集成到这些类中，用于读取（或提取）、转换和写入（或加载）数据。尽管数据可以从各种来源读取和写入，但 Hive 表通常是自然选择。标准转换被封装在常用类中，使其可以在不同管道之间重用。

这段代码展示了一个基本的 Spark Scala 数据管道。

```py
import org.apache.spark.sql.{SparkSession, DataFrame}
import org.apache.spark.sql.functions._

class MyExampleDataPipeline {

  val spark: SparkSession = SparkSession.builder
    .appName("DataFrame Transformation Example")
    .master("local[*]")
    .enableHiveSupport()
    .getOrCreate()

  def main(inputTableName: String, outputTableName: String): Unit = {
    val inputDataFrame: DataFrame = spark.table(inputTableName)

    val transformedDataFrame: DataFrame = inputDataFrame.SOME_TRANSFORMATIONS

    transformedDataFrame.write.mode("overwrite").format("parquet").saveAsTable(outputTableName)
  }
}
```

*最终*，目标是构建一个通常以 jar 文件形式存在的 Java 应用程序。这个 jar 文件以及适当的参数，可以通过像**Apache Airflow**这样的作业和工作流管理系统进行调用。这使得在计划的时间间隔执行特定的数据管道成为可能，从而促进有序和自动化的数据处理工作流。

这是一个简单的示例，展示如何从命令行执行一个作为类表示的数据管道。

```py
spark-submit --class MyExampleDataPipeline --master local[*] yourJarFile.jar your_input_hive_table_name your_output_hive_table_name
```

# 更高级的实践

如前所述，数据管道现在被封装在 jar 文件中，通常需要定时执行，特别是用于批处理，或者基于触发事件激活，通常用于实时处理。**Apache Airflow** 作为一个强大的解决方案，用于编排这些 jar 文件和任务类，促进定期执行作业。或者，可以使用像 AWS Lambda 这样的工具触发 jar 文件，以应对不规则的时间表和实时处理。

这是一个 Apache Airflow DAG 的示例，旨在每天执行一个 Java 类。

```py
from datetime import datetime, timedelta
from airflow import DAG
from airflow.operators.bash_operator import BashOperator
from airflow.operators.dummy_operator import DummyOperator

jar_file_path = "/path/to/yourJarFile.jar"

input_table_name = "your_input_hive_table_name"
output_table_name = "your_output_hive_table_name"

default_args = {
    'owner': 'airflow',
    'start_date': datetime(2023, 1, 1),
    'depends_on_past': False,
    'retries': 1,
    'retry_delay': timedelta(minutes=5),
}

dag = DAG(
    'my_data_pipeline_dag',
    default_args=default_args,
    description='DAG to run the MyExampleDataPipeline class',
    schedule_interval='@daily',  # Adjust the schedule as needed
)

start_task = DummyOperator(task_id='start', dag=dag)
end_task = DummyOperator(task_id='end', dag=dag)

spark_submit_command = f"spark-submit --class MyExampleDataPipeline --master local[*] {jar_file_path} {input_table_name} {output_table_name}"

run_data_pipeline_task = BashOperator(
    task_id='run_data_pipeline',
    bash_command=spark_submit_command,
    dag=dag,
)

start_task >> run_data_pipeline_task >> end_task
```

此外，持续集成/持续部署（CI/CD）工具，包括 Jenkins、GitHub Actions、Spinnaker 等，提供了一种无缝的方式来开发和部署跨各种环境的数据管道，从开发到测试和生产环境。这确保了管道在整个开发生命周期中的平滑和自动化过渡。

# 在最后...

我们探索了数据工程不断变化的格局以及该领域所需的基本编程技能。虽然 SQL 和 Python 传统上与数据工程相关，但重点正转向 Java 和 Scala，特别是在通过像 Hadoop 和 Spark 这样的分布式计算框架处理大量数据的背景下。

我们再次强调了 JVM（Java 虚拟机）语言的重要性，因为它们的可移植性使其适合开发可以在各种系统和环境中无缝运行的数据应用程序。它深入探讨了基于 JVM 的数据应用程序的架构，展示了如何使用 Java 或 Scala 开发数据管道，同时 Apache Maven 帮助项目管理。
