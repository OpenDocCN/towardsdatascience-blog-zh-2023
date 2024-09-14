# 数据工程面试问题

> 原文：[`towardsdatascience.com/data-engineering-interview-questions-fdef62e46505`](https://towardsdatascience.com/data-engineering-interview-questions-fdef62e46505)

## 准备面试的技巧

[](https://mshakhomirov.medium.com/?source=post_page-----fdef62e46505--------------------------------)![💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----fdef62e46505--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fdef62e46505--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fdef62e46505--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----fdef62e46505--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fdef62e46505--------------------------------) ·20 min 阅读·2023 年 11 月 30 日

--

![](img/a7bc9856e987821675426d58096e4630.png)

照片由 [Ignacio Amenábar](https://unsplash.com/@amenabarladrondeguevara?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这个故事旨在揭示各种数据工程面试场景和典型讨论。它涵盖了你可能会被问到的几乎所有问题，希望对初学者和中级数据从业者在求职面试准备期间有所帮助。在我将近十五年的分析和数据工程职业生涯中，我面试了很多人，现在我想与大家分享我的观察。

数据工程面试难吗？不，如果你了解你在处理什么的话，并不难。许多公司都有技术博客，描述他们的技术栈和使用的技术。我建议提前做一些研究。

> 数据工程面试本身其实很简单，这项工作也非常有回报。

面试确实很简单，因为问题通常遵循相同的模式。**数据平台类型** [1] 的数量仅限于四种，这将定义答案，帮助你通过面试。所以，如果我们知道我们在工程什么，那么正确回答面试问题就不是一项很大的任务。

[](/data-platform-architecture-types-f255ac6e0b7?source=post_page-----fdef62e46505--------------------------------) ## 数据平台架构类型

### 它如何满足你的业务需求？选择的两难困境。

towardsdatascience.com

数据工程（DE）面试很容易通过，除非你被要求编写代码。这通常是面试过程的第二部分。以下是我收集的数据工程面试问题及答案。希望你喜欢！

## 你日常的 DE 工作是怎样的？

通常招聘经理会用这个简单的问题开始谈话。在这里，我们希望展示对各种 DE 工具和框架的丰富热情和经验。提供一些数据管道的示例来装饰你的回答。可以是你构建的几个数据管道，或者以数据仓库为中心的全生命周期项目。不要称其为教程。最好说点像这样的话…

> “… 一个从需求收集到数据管道设计和上线的全生命周期项目。”

这看起来更专业，这也是你希望传达的印象。尽量简洁，但也要流利地描述你的日常工作。例如，你可以说你是一个学生，目前主要关注数据质量，并且你设计并构建了数据管道，以便在将数据加载到数据平台之前，首先使用行条件检查数据。或者，你可以提到你知道如何使用 SDK 将数据加载到数据仓库等。你可以在这篇文章中找到一些好的示例[2]：

[](/python-for-data-engineers-f3d5db59b6dd?source=post_page-----fdef62e46505--------------------------------) ## 数据工程师的 Python

### 面向初学者的高级 ETL 技术

towardsdatascience.com

这并不太困难。你可以说，你在左侧有各种数据源，可以按照下面的模式创建数据管道，将它们集成到你的数据仓库（DWH）解决方案中。

![](img/a533b28c5e039871007b44ee56caff97.png)

数据仓库示例。图片由作者提供

请考虑这个将数据加载到 BigQuery 数据仓库的示例，使用了 Pandas 和 google.cloud 库[3]：

```py
from google.cloud import bigquery
from google.oauth2 import service_account
...
# Authenticate BigQuery client:
service_acount_str = config.get('BigQuery') # Use config
credentials = service_account.Credentials.from_service_account_info(service_acount_str)
client = bigquery.Client(credentials=credentials, project=credentials.project_id)

...
def load_table_from_dataframe(table_schema, table_name, dataset_id):
    #! source data file format must be outer array JSON:
    """
    [
    {"id":"1"},
    {"id":"2"}
    ]
    """
    blob = """
            [
    {"id":"1","first_name":"John","last_name":"Doe","dob":"1968-01-22","addresses":[{"status":"current","address":"123 First Avenue","city":"Seattle","state":"WA","zip":"11111","numberOfYears":"1"},{"status":"previous","address":"456 Main Street","city":"Portland","state":"OR","zip":"22222","numberOfYears":"5"}]},
    {"id":"2","first_name":"John","last_name":"Doe","dob":"1968-01-22","addresses":[{"status":"current","address":"123 First Avenue","city":"Seattle","state":"WA","zip":"11111","numberOfYears":"1"},{"status":"previous","address":"456 Main Street","city":"Portland","state":"OR","zip":"22222","numberOfYears":"5"}]}
    ]
    """
    body = json.loads(blob)
    print(pandas.__version__)

    table_id = client.dataset(dataset_id).table(table_name)
    job_config = bigquery.LoadJobConfig()
    schema = create_schema_from_yaml(table_schema) 
    job_config.schema = schema

    df = pandas.DataFrame(
    body,
    # In the loaded table, the column order reflects the order of the
    # columns in the DataFrame.
    columns=["id", "first_name","last_name","dob","addresses"],

    )
    df['addresses'] = df.addresses.astype(str)
    df = df[['id','first_name','last_name','dob','addresses']]

    print(df)

    load_job = client.load_table_from_dataframe(
        df,
        table_id,
        job_config=job_config,
    )

    load_job.result()
    print("Job finished.")
```

## 你是如何创建数据管道的？

你需要明确表示你对使用第三方 ETL 工具（如 Fivetran、Stitch 等）和自己编写的定制数据连接器都很有信心。数据管道是指将数据从 A 点提取、转换和/或加载到 B 点的过程[4]。

[](/data-pipeline-design-patterns-100afa4b93e3?source=post_page-----fdef62e46505--------------------------------) ## 数据管道设计模式

### 选择合适的架构及示例

towardsdatascience.com

所以你需要做的就是展示你知道如何按照三种主要数据管道设计模式来操作——**批处理**（分块汇总和处理）、**流处理**（逐条处理和加载记录）、**变更数据捕获**（CDC，在 A 点识别和捕获变化，以处理和加载到 B 点）。

> CDC 和流处理是紧密相关的。

例如，我们可以使用 MySQL 二进制日志文件实时将数据移动到我们的 DWH 解决方案中。它必须谨慎使用，并不总是数据管道中最具成本效益的工具，但值得一提。按照概念设计图保持一切有序。这有助于解释许多 ETL 相关的事项。

![](img/f319a0d445798a12e7ff1aa554120f2a.png)

概念数据管道设计。图像由作者提供

## 你对数据平台设计了解多少？

简而言之，有四种数据平台架构类型，这将定义你在构建管道时可能想要使用的工具。这是问题的关键——它帮助选择合适的数据工程工具和技术。数据湖、数据仓库和数据湖屋各有其优点并服务于不同的目的。第四种架构类型是**数据网格**，其中数据管理是分散的。**数据网格**定义了当我们拥有不同的数据领域（公司部门）及其自身团队和共享数据资源时的状态。它可能看起来有些混乱，但许多公司选择这种模型来减少数据官僚主义。

通常情况下，与数据湖相比，数据仓库提供更好的数据治理。由于内置 ANSI-SQL 能力，它使数据栈看起来现代且灵活。迁移到数据湖或数据仓库主要取决于用户的技能水平。数据仓库解决方案将提供更多的互动性，并将我们的选择缩小到 SQL 优先的产品（如 Snowflake、BigQuery 等）。

> 数据湖适用于具备编程技能的用户，我们倾向于选择以 Python 为优先的产品，如 Databricks、Galaxy、Dataproc、EMR。

## 什么是数据建模？

数据建模是数据工程的一个重要部分，因为数据是通过实体（表、视图、孤岛、数据湖）之间的关系进行转化的。你需要展示你对**概念**和**物理**设计过程的理解。我们总是从为我们的业务流程或数据转化任务创建一个模型的概念开始。然后是功能模型，即原型，旨在证明我们的概念模型适用于该任务。最后，我们将创建一个物理模型，其中包含最终的基础设施，包括所有所需的物理实体和对象。值得一提的是，这并不总是 SQL 实体。概念数据建模可能包括所有类型的数据平台以及云存储中的半结构化数据文件。一个好的例子是，当我们需要先在数据湖中准备数据，然后用它来训练机器学习（ML）模型时。我之前在这篇文章中写过：

[](https://pub.towardsai.net/orchestrate-machine-learning-pipelines-with-aws-step-functions-d8216a899bd5?source=post_page-----fdef62e46505--------------------------------) [## 使用 AWS Step Functions 编排机器学习管道]

### 高级数据工程和基础设施即代码的机器学习操作

[pub.towardsai.net](https://pub.towardsai.net/orchestrate-machine-learning-pipelines-with-aws-step-functions-d8216a899bd5?source=post_page-----fdef62e46505--------------------------------)

提到你熟悉**模板引擎**如**DBT**和**Dataform**是很好的，这些工具可以用于这项任务。为什么？它们对于数据转换**单元测试**[4]和数据环境[5]非常有帮助，防止人为错误，并提供更好的部署工作流。我之前在这里写过：

[](/continuous-integration-and-deployment-for-data-platforms-817bf1b6bed1?source=post_page-----fdef62e46505--------------------------------) ## 数据平台的持续集成和部署

### 数据工程师和机器学习操作的 CI/CD

[towardsdatascience.com

## 星型模式和雪花模式有什么区别？

求职面试官很常测试你的数据工程设计模式知识。尽量简明扼要地说明星型模式是我们可以利用超级大型非规范化数据集与一个事实表连接的地方。这就是为什么它被称为星型数据库设计模式，因为它看起来像一颗星。这更适用于数据仓库的 OLAP 风格分析管道。这些数据集中的数据并不总是最新的，但没关系，因为我们需要以这种方式物化数据，如果需要，我们可以更新所需的字段。

与星型模式相对，雪花模式设计的中心是相同的事实表，但它与许多其他事实表和维度表连接，这些表通常是**非规范化的**。这种模式设计更适合于 OLTP 数据处理，当数据需要始终保持最新状态时，能够快速提取单独的行以供应用程序使用。

## 在 1 到 10 的范围内，你的 SQL 技能有多好？

确保你能解释你的答案。SQL 是一种自然的方言，用于建模数据转换和创建分析数据集。自信地处理增量表更新可以直接给你 6 分。请考虑下面的例子。它使用 MERGE 创建一个**增量**表：

```py
create temp table last_online as (
    select 1 as user_id
    , timestamp('2000-10-01 00:00:01') as last_online
)
;
create temp table connection_data  (
  user_id int64
  ,timestamp timestamp
)
PARTITION BY DATE(_PARTITIONTIME)
;
insert connection_data (user_id, timestamp)
    select 2 as user_id
    , timestamp_sub(current_timestamp(),interval 28 hour) as timestamp
union all
    select 1 as user_id
        , timestamp_sub(current_timestamp(),interval 28 hour) as timestamp
union all
    select 1 as user_id
        , timestamp_sub(current_timestamp(),interval 20 hour) as timestamp
union all
    select 1 as user_id
    , timestamp_sub(current_timestamp(),interval 1 hour) as timestamp
;

merge last_online t
using (
  select
      user_id
    , last_online
  from
    (
        select
            user_id
        ,   max(timestamp) as last_online

        from 
            connection_data
        where
            date(_partitiontime) >= date_sub(current_date(), interval 1 day)
        group by
            user_id

    ) y

) s
on t.user_id = s.user_id
when matched then
  update set last_online = s.last_online, user_id = s.user_id
when not matched then
  insert (last_online, user_id) values (last_online, user_id)
;
select * from last_online
;
```

我之前写过高级技巧。我认为这是开始准备的好地方[6]：

[](/advanced-sql-techniques-for-beginners-211851a28488?source=post_page-----fdef62e46505--------------------------------) ## 高级 SQL 技巧入门

### 在 1 到 10 的范围内，你的数据仓库技能有多好？

[towardsdatascience.com

对于数据转换脚本运行 SQL 单元测试和处理自定义用户定义函数（UDF）[7]将使你获得 9 分。

## 如何在 SQL 中获得 10 分？

这将是非常棘手的，显然与对特定工具的专业知识相关，即将表转换为**结构体数组**并将其传递给 UDF。

> 当你需要对每一行或表应用具有复杂逻辑的用户定义函数（UDF）时，这很有用。

你可以将你的表视为一组 TYPE STRUCT 对象，然后将每个对象传递给 UDF。这取决于你的逻辑。例如，我在购买堆叠中使用它来计算过期时间：

```py
select 
     target_id
    ,product_id
    ,product_type_id
    ,production.purchase_summary_udf()(
        ARRAY_AGG(
            STRUCT(
                target_id
                , user_id
                , product_type_id
                , product_id
                , item_count
                , days
                , expire_time_after_purchase
                , transaction_id 
                , purchase_created_at 
                , updated_at
            ) 
            order by purchase_created_at
        )
    ) AS processed

from new_batch
;
```

## OLAP 和 OLTP 的区别是什么？

在线分析处理（OLAP）和在线事务处理（OLTP）是为完全不同目的设计的数据处理系统。OLAP 旨在聚合和存储数据，以便用于分析，如报告和大规模数据处理，这也是为什么在这里经常可以看到非规范化的超大表。OLTP 处理则不同，它专注于单个事务，并要求数据处理速度非常快。好的例子包括应用内购买、管理用户账户和更新商店内容。OLTP 的数据存储在使用雪花模式连接的索引表中，其中维度表大多是规范化的。

## 你知道哪些数据工程框架？

我们不能知道所有事情。我面试了很多人，拥有所有数据工程工具和框架的经验并不是必要的。你可以列举一些：**Python ETL (PETL)、Bonobo、Apache Airflow、Bubbles、Kestra、Luigi**，我之前写过关于过去几年我们见证的 ETL 框架爆炸的文章。

[](/modern-data-engineering-e202776fb9a9?source=post_page-----fdef62e46505--------------------------------) ## 现代数据工程

### 平台特定工具和高级技术

towardsdatascience.com

> 我们不需要对所有框架都非常熟悉，但展示自信是必须的。

为了展示对各种数据工具的自信，我们应该至少学习一两个，然后使用基本原则（数据工程原则）。采用这种方法，我们可以回答几乎所有数据工程问题：

> 你为什么这样做？——我从基本原则中得到了这个。

说到这，学习 Apache Airflow 的一些内容并通过简单的管道示例来展示它是完全可以的。例如，我们可以在将数据导出到云存储（bq_export_op）后运行 ml_engine_training_op，并使这个工作流每天或每周运行一次。

![](img/9d08e679e0eaf09488813ce519b4a635.png)

使用 Airflow 进行 ML 模型训练。图片由作者提供。

请看下面的这个例子。

> *它创建一个简单的数据管道图，将数据导出到云存储桶中，然后使用 MLEngineTrainingOperator 训练 ML 模型。*

```py
"""DAG definition for recommendation_bespoke model training."""

import airflow
from airflow import DAG
from airflow.contrib.operators.bigquery_operator import BigQueryOperator
from airflow.contrib.operators.bigquery_to_gcs import BigQueryToCloudStorageOperator
from airflow.hooks.base_hook import BaseHook
from airflow.operators.app_engine_admin_plugin import AppEngineVersionOperator
from airflow.operators.ml_engine_plugin import MLEngineTrainingOperator

import datetime

def _get_project_id():
  """Get project ID from default GCP connection."""

  extras = BaseHook.get_connection('google_cloud_default').extra_dejson
  key = 'extra__google_cloud_platform__project'
  if key in extras:
    project_id = extras[key]
  else:
    raise ('Must configure project_id in google_cloud_default '
           'connection from Airflow Console')
  return project_id

PROJECT_ID = _get_project_id()

# Data set constants, used in BigQuery tasks.  You can change these
# to conform to your data.
DATASET = 'staging' #'analytics'
TABLE_NAME = 'recommendation_bespoke'

# GCS bucket names and region, can also be changed.
BUCKET = 'gs://rec_wals_eu'
REGION = 'us-central1' #'europe-west2' #'us-east1'
JOB_DIR = BUCKET + '/jobs'

default_args = {
    'owner': 'airflow',
    'depends_on_past': False,
    'start_date': airflow.utils.dates.days_ago(2),
    'email': ['mike.shakhomirov@gmail.com'],
    'email_on_failure': True,
    'email_on_retry': False,
    'retries': 5,
    'retry_delay': datetime.timedelta(minutes=5)
}

# Default schedule interval using cronjob syntax - can be customized here
# or in the Airflow console.
schedule_interval = '00 21 * * *'

dag = DAG('recommendations_training_v6', default_args=default_args,
          schedule_interval=schedule_interval)

dag.doc_md = __doc__

#
#
# Task Definition
#
#

# BigQuery training data export to GCS

training_file = BUCKET + '/data/recommendations_small.csv' # just a few records for staging

t1 = BigQueryToCloudStorageOperator(
    task_id='bq_export_op',
    source_project_dataset_table='%s.recommendation_bespoke' % DATASET,
    destination_cloud_storage_uris=[training_file],
    export_format='CSV',
    dag=dag
)

# ML Engine training job
training_file = BUCKET + '/data/recommendations_small.csv'
job_id = 'recserve_{0}'.format(datetime.datetime.now().strftime('%Y%m%d%H%M'))
job_dir = BUCKET + '/jobs/' + job_id
output_dir = BUCKET
delimiter=','
data_type='user_groups'
master_image_uri='gcr.io/my-project/recommendation_bespoke_container:tf_rec_latest'

training_args = ['--job-dir', job_dir,
                 '--train-file', training_file,
                 '--output-dir', output_dir,
                 '--data-type', data_type]

master_config = {"imageUri": master_image_uri,}

t3 = MLEngineTrainingOperator(
    task_id='ml_engine_training_op',
    project_id=PROJECT_ID,
    job_id=job_id,
    training_args=training_args,
    region=REGION,
    scale_tier='CUSTOM',
    master_type='complex_model_m_gpu',
    master_config=master_config,
    dag=dag
)

t3.set_upstream(t1)
```

## 你会使用什么来协调你的数据管道？

区分用于数据转换的 ETL 框架和用于编排数据管道的框架非常重要。你可以提到一些：**Airflow、Prefect、Dagster、Kestra、Argo、Luigi**。这些是目前最受欢迎的开源项目，可以免费使用。然而，一个好的回答应该表明你能够使用自己的定制工具进行数据管道编排。如果你喜欢 AWS，你可以使用 CloudFormation（基础设施即代码）和 Step Functions 部署和编排数据管道。我之前在这里写过关于它的内容[9]：

[](/data-pipeline-orchestration-9887e1b5eb7a?source=post_page-----fdef62e46505--------------------------------) ## 数据管道编排

### 正确进行数据管道管理可以简化部署，并提高数据的可用性和可访问性...

[towardsdatascience.com

实际上，我们在这里甚至不需要 Step Functions，因为这将是一个非常平台特定的选择。我们可以使用平台无关的 Terraform（基础设施即代码）和 Serverless 部署带有所需数据管道编排逻辑的微服务。

## 你的编程语言是什么？

对这个问题的回答取决于公司技术栈。长话短说，如果你回答 Python，你不会错。这在 DE 和数据科学中是绝对必要的，因为它的简单性以及市场上众多的库和开源数据工具。

> 不过，不要仅仅局限于 Python。

熟悉其他语言总是有益的，例如 JAVA、JavaScript、Scala、Spark 和 R。例如，R 在数据科学中表现优异，并且在学术界和大学中非常受欢迎。提到 Spark 也是有益的。它不是一种语言（框架），但由于其卓越的可扩展性和处理大规模数据的能力，它变得非常流行[8]。

> 你可能不知道 Spark，但如果你知道 Python，那么你可以使用 Spark API 连接器（PySpark）。

## 什么是 *args 和 **kwargs？

通常，如果你在前一个问题中提到 Python，下一步就是这个。回答关于函数参数的问题是我在面试中最常问的问题之一。你需要准备好回答它，也许还会用几行代码给面试官留下深刻印象：

```py
def sum_example(*args):
    result = 0
    for x in args:
        result += x
    return result

print(sum_example(1, 2, 3))

def concat(**kwargs):
    result = ""
    for arg in kwargs.values():
        result += arg
    return result

print(concat(a="Data", b="Engineering", c="is", d="Great", e="!"))
```

## 你对 CLI 工具和 shell 脚本的掌握如何？

云供应商的命令行工具基于 REST API，使数据工程师能够通过强大的命令行界面与云服务端点进行通信，以描述和修改资源。数据工程师使用 CLI 工具与 bash 脚本链式命令。这有助于创建强大的脚本，并轻松与云服务进行交互。考虑下面的示例。它将调用名为 pipeline-manager 的 AWS Lambda 函数：

```py
aws lambda invoke \
    --function-name pipeline-manager \
    --payload '{ "key": "something" }' \
    response.json 
```

我们可以创建一些更强大的工具来部署我们的无服务器微服务。考虑下面这个例子。它将检查 lambda 包的存储桶是否存在，上传并部署我们的 ETL 服务作为一个 Lambda 函数[10]：

```py
# ./deploy.sh
# Run ./deploy.sh
LAMBDA_BUCKET=$1 # your-lambda-packages.aws
STACK_NAME=SimpleETLService
APP_FOLDER=pipeline_manager
# Get date and time to create unique s3-key for deployment package:
date
TIME=`date +"%Y%m%d%H%M%S"`
# Get the name of the base application folder, i.e. pipeline_manager.
base=${PWD##*/}
# Use this name to name zip:
zp=$base".zip"
echo $zp
# Remove old package if exists:
rm -f $zp
# Package Lambda
zip -r $zp "./${APP_FOLDER}" -x deploy.sh

# Check if Lambda bucket exists:
LAMBDA_BUCKET_EXISTS=$(aws  s3 ls ${LAMBDA_BUCKET} --output text)
#  If NOT:
if [[ $? -eq 254 ]]; then
    # create a bucket to keep Lambdas packaged files:
    echo  "Creating Lambda code bucket ${LAMBDA_BUCKET} "
    CREATE_BUCKET=$(aws  s3 mb s3://${LAMBDA_BUCKET} --output text)
    echo ${CREATE_BUCKET}
fi

# Upload the package to S3:
aws s3 cp ./${base}.zip s3://${LAMBDA_BUCKET}/${APP_FOLDER}/${base}${TIME}.zip

# Deploy / Update:
aws --profile $PROFILE \
cloudformation deploy \
--template-file stack.yaml \
--stack-name $STACK_NAME \
--capabilities CAPABILITY_IAM \
--parameter-overrides \
"StackPackageS3Key"="${APP_FOLDER}/${base}${TIME}.zip" \
"AppFolder"=$APP_FOLDER \
"LambdaCodeLocation"=$LAMBDA_BUCKET \
"Environment"="staging" \
"Testing"="false"
```

## 你如何部署你的数据管道？

没有对错之分，但如果你说“我手动创建管道步骤，然后在云中使用供应商的控制台进行部署……”那就不是最佳答案。

现在，好的答案是提到脚本。这告诉面试官你是一个中级用户，至少熟悉 Shell 脚本。你会想说，无论你部署什么，都可以使用 bash 脚本和 CLI 工具进行部署。所有主要的云供应商都有他们的命令行工具，你至少要对其中一种工具有所了解。

最优的方法，通常被认为是最佳实践，是使用基础设施即代码和 CI/CD 工具来部署你的管道[11]。

[](/continuous-integration-and-deployment-for-data-platforms-817bf1b6bed1?source=post_page-----fdef62e46505--------------------------------) ## 数据平台的持续集成和部署

### 数据工程师和机器学习运维的 CI/CD

[towardsdatascience.com

## 你在数据科学方面的水平如何？

作为数据工程师，你不需要了解数据科学模型训练和超参数调优的所有细节，但记住一个好的数据科学家必须是一个好的数据工程师。不必完全相反，但展示至少一些基本的数据科学算法知识总是有益的。例如，你可以提到你知道如何创建线性回归和逻辑回归模型。线性回归生成定量输出（预测的数字），而逻辑回归则返回简单的答案——“是”或“否”（1/0）。事实上，所有主要的数据科学模型都可以在你的数据仓库解决方案中使用 SQL 轻松训练。

> 让我们假设我们的用例是客户流失预测。

以**BigQuery ML**为例，我们可以像这样创建一个逻辑回归模型：

```py
CREATE OR REPLACE MODEL sample_churn_model.churn_model

OPTIONS(
  MODEL_TYPE="LOGISTIC_REG",
  INPUT_LABEL_COLS=["churned"]
) AS

SELECT
  * except (
     user_pseudo_id
    ,first_seen_ts    
    ,last_seen_ts     
  )
FROM
  sample_churn_model.churn
```

## 你对数据质量和数据可靠性了解多少？

这是一个很好的问题，因为你可能会被问及在数据平台中确保数据质量的可能方法。这是数据工程师日常工作的一部分，涉及提高数据管道的数据准确性。数据工程师连接数据源并部署管道，其中数据必须被提取，然后根据业务需求经常需要进行转换。

> 我们需要确保所有必需的字段存在（数据质量）且没有数据丢失（可靠性）。

我们如何做到这一点？总是提到自修复管道是好的，并且你知道如何部署它们。数据工程师可以以类似于部署 ETL 管道的方式部署数据质量管道。简单来说，你需要为一个数据集使用行条件，并根据结果部署修复步骤，即提取缺失的数据并加载它。

> 使用行条件来确保数据集的数据质量。

所有的数据质量检查都可以作为脚本进行调度，如果任何一个检查未能满足特定条件，我们可以发送电子邮件通知。值得一提的是，现代数据仓库解决方案允许使用 SQL 脚本进行这些检查，但这不一定要局限于 SQL。任何数据检查脚本都可以在数据湖或其他地方的数据上运行。这取决于我们数据平台的类型。在这种情况下，良好的编码技能是必需的，因此我们需要展示我们知道如何创建一个简单的巡检应用程序，可以根据数据的实际位置扫描数据。

基于 SQL 的答案也很好，但它更适合数据开发人员角色，因为 SQL 通常被认为是分析中的主要数据查询方言。考虑下面的例子。它将使用 SQL 和行条件来检查是否有 NULL `payment_date`的记录。它还会检查重复记录。

```py
with checks as (
    select
      count( transaction_id )                                                           as t_cnt
    , count(distinct transaction_id)                                                    as t_cntd
    , count(distinct (case when payment_date is null then transaction_id end))          as pmnt_date_null
    from
        production.user_transaction
)
, row_conditions as (

    select if(t_cnt = 0,'Data for yesterday missing; ', NULL)  as alert from checks
union all
    select if(t_cnt != t_cntd,'Duplicate transactions found; ', NULL) from checks
union all
    select if(pmnt_date_null != 0, cast(pmnt_date_null as string )||' NULL payment_date found', NULL) from checks
)

, alerts as (
select
        array_to_string(
            array_agg(alert IGNORE NULLS) 
        ,'.; ')                                         as stringify_alert_list

    ,   array_length(array_agg(alert IGNORE NULLS))     as issues_found
from
    row_conditions
)

select
    alerts.issues_found,
    if(alerts.issues_found is null, 'all good'
        , ERROR(FORMAT('ATTENTION: production.user_transaction has potential data quality issues for yesterday: %t. Check dataChecks.check_user_transaction_failed_v for more info.'
        , stringify_alert_list)))
from
    alerts
;
```

结果是 BigQuery 将发送包含警报的自动电子邮件：

![](img/333076df44ae7cc144559f8d8999a013.png)

邮件警报。图像由作者提供。

## 你会使用什么算法来提取或处理非常大的数据集？

如果你之前有关于使用 Python 进行数据转换的问题，这个问题可能是个陷阱。如果你喜欢 Python，那么你可能是 Pandas 库的忠实粉丝，并且你可能已经在面试中提到过。实际上，这种问题中你不希望使用 Pandas。问题在于 Pandas 处理大数据集时效果不好，尤其是在数据转换方面。你在 Pandas 数据框中运行数据转换时，总是会受到机器内存的限制。

正确的答案应该提到，如果内存有限，你会找到一个可扩展的解决方案。这可以是一个简单的 Python **生成器**，是的，它可能需要很长时间，但至少不会失败。

```py
# Create a file first: ./very_big_file.csv as:
# transaction_id,user_id,total_cost,dt
# 1,John,10.99,2023-04-15
# 2,Mary, 4.99,2023-04-12

# Example.py
def etl(item):
    # Do some etl here
    return item.replace("John", '****') 

# Create a generator 
def batch_read_file(file_object, batch_size=19):
    """Lazy function (generator) can read a file in chunks.
    Default chunk: 1024 bytes."""
    while True:
        data = file_object.read(batch_size)
        if not data:
            break
        yield data
# and read in chunks
with open('very_big_file.csv') as f:
    for batch in batch_read_file(f):
        print(etl(batch))

# In command line run
# Python example.py
```

最优的答案应该包括使用分布式计算来转换数据，并且理想情况下，使用一些快速且扩展性好的工具。Spark 或基于 HIVE 的工具可能是一个不错的选择。

## 你会在管道中何时使用 Hadoop？

你应该提到 Hadoop 是一个由 Apache 基金会开发的开源大数据处理框架，它带来了分布式数据处理的所有好处。这就是为什么它在处理大量数据的数据管道中变得如此受欢迎。它有自己的内在组件，旨在确保数据质量（HDFS——Hadoop 分布式数据系统）和可扩展性（MapReduce）。即使你没有 Hadoop 的经验，仅仅提到这些内容也足够，因为有许多工具是建立在 Apache Hadoop 之上的，例如**Apache Pig**（一个在 MapReduce 中执行 Hadoop 作业的编程平台）或**Apache Hive**——一个数据仓库项目，我们可以使用标准 SQL 方言处理存储在数据库和与 Hadoop 集成的文件系统中的数据。

## 你会如何处理一个大数据迁移项目？

在面试中，你可能会被问到这个问题，因为面试官希望了解你在数据迁移方面的经验以及迁移完成后的数据验证方法。在这里，我建议从业务需求开始。这可能是成本效益、数据治理或整体数据库性能。根据这些需求，我们可以选择最优的解决方案作为我们迁移项目的目标。例如，如果你当前的数据平台建立在数据湖上，并且有许多业务利益相关者希望访问数据，那么你的选择应该是 ANSI-SQL 数据仓库解决方案，在这些解决方案中我们可以提供更好的数据治理和细粒度访问控制。相反，如果我们的数据仓库解决方案在数据存储方面存在成本效益问题，那么迁移或归档到数据湖可能是一个好的选择。我之前在这里写过关于它的内容：

[## 数据仓库 DBA 的日常任务](https://medium.com/geekculture/data-warehouse-dba-tasks-i-do-daily-32d9f823d799?source=post_page-----fdef62e46505--------------------------------)

### 监控活动并像专家一样管理资源，或者“我的表……它去哪儿了？”

[medium.com](https://medium.com/geekculture/data-warehouse-dba-tasks-i-do-daily-32d9f823d799?source=post_page-----fdef62e46505--------------------------------)

一旦迁移完成，我们需要验证数据。数据一致性是数据工程师的首要任务，你需要展示你知道如何验证在迁移完成时没有数据丢失。例如，我们可以计算数据仓库中每个分区的总记录数，然后将其与数据湖分区中的记录数进行比较。`count(*)`是最便宜的操作，但它对数据验证非常有效且运行速度很快。实际上，在许多数据仓库解决方案中，`count(*)`是免费的。

## 你知道哪些 ETL 工具，它们与 ELT 有何不同？

回答这个问题时，我们想要展示的是我们不仅知道如何使用第三方工具提取、转换和加载数据，还能通过编写自己的定制数据连接器和加载器来完成。

你可以先简单提到一些托管解决方案，比如 Fivetran、Stitch 等，它们有助于 ETL。别忘了提到它们的定价模型，通常是基于处理的记录数量。

> 当你知道如何编码时，你不需要第三方 ETL 工具。

不要害怕说出这句话。创建自己的 ETL 工具然后将数据加载到你选择的 DWH 解决方案中是相当简单的。考虑我之前的一篇文章，其中我从 MySQL 或 Postgres 数据库中提取了数百万行数据作为例子。它解释了如何创建一个强大的数据连接器并以内存高效的方式分块提取数据 [12]。这类东西设计成无服务器的，可以轻松地在云中部署和调度。

[](/building-a-batch-data-pipeline-with-athena-and-mysql-7e60575ff39c?source=post_page-----fdef62e46505--------------------------------) ## 使用 Athena 和 MySQL 构建批量数据管道

### 初学者的端到端教程

towardsdatascience.com

如果我们需要在将数据加载到 DWH 目标之前进行准备和转换，我们甚至可以创建自己的定制数据加载管理器。这是一个相当复杂的应用，但值得学习。这里是教程。

## 结论

“你对……的处理方法是什么？”这类问题在数据工程面试中非常常见。准备好回答这些场景问题。在面试中，你可能会被要求设计一个管道或数据平台。如果你从更广泛的角度看待问题，就是这样。每个数据平台都有其业务和功能需求——总是好在回答中提到这一点，然后提到你会根据这些需求选择数据工具。用数据管道的例子来装饰你的答案，你一定会通过面试。

## 推荐阅读

[1] [`medium.com/towards-data-science/data-platform-architecture-types-f255ac6e0b7`](https://medium.com/towards-data-science/data-platform-architecture-types-f255ac6e0b7)

[2] [`medium.com/towards-data-science/python-for-data-engineers-f3d5db59b6dd`](https://medium.com/towards-data-science/python-for-data-engineers-f3d5db59b6dd)

[3] `towardsdatascience.com/modern-data-engineering-e202776fb9a9`

[4] [`medium.com/towards-data-science/unit-tests-for-sql-scripts-with-dependencies-in-dataform-847133b803b7`](https://medium.com/towards-data-science/unit-tests-for-sql-scripts-with-dependencies-in-dataform-847133b803b7)

[5] [`medium.com/towards-data-science/continuous-integration-and-deployment-for-data-platforms-817bf1b6bed1`](https://medium.com/towards-data-science/continuous-integration-and-deployment-for-data-platforms-817bf1b6bed1)

[6] `towardsdatascience.com/advanced-sql-techniques-for-beginners-211851a28488`

[7] [`cloud.google.com/bigquery/docs/user-defined-functions`](https://cloud.google.com/bigquery/docs/user-defined-functions)

[8] [`spark.apache.org/`](https://spark.apache.org/)

[9] [`medium.com/towards-data-science/data-pipeline-orchestration-9887e1b5eb7a`](https://medium.com/towards-data-science/data-pipeline-orchestration-9887e1b5eb7a)

[10] `towardsdatascience.com/how-to-become-a-data-engineer-c0319cb226c2`

[11] [`medium.com/towards-data-science/continuous-integration-and-deployment-for-data-platforms-817bf1b6bed1`](https://medium.com/towards-data-science/continuous-integration-and-deployment-for-data-platforms-817bf1b6bed1)

[12] [`medium.com/towards-data-science/mysql-data-connector-for-your-data-warehouse-solution-db0d338b782d`](https://medium.com/towards-data-science/mysql-data-connector-for-your-data-warehouse-solution-db0d338b782d)
