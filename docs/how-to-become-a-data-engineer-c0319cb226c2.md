# 如何成为数据工程师

> 原文：[`towardsdatascience.com/how-to-become-a-data-engineer-c0319cb226c2`](https://towardsdatascience.com/how-to-become-a-data-engineer-c0319cb226c2)

## 2024 年初学者的捷径

[](https://mshakhomirov.medium.com/?source=post_page-----c0319cb226c2--------------------------------)![💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----c0319cb226c2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c0319cb226c2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c0319cb226c2--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----c0319cb226c2--------------------------------)

·发布在[数据科学前沿](https://towardsdatascience.com/?source=post_page-----c0319cb226c2--------------------------------) ·17 分钟阅读·2023 年 10 月 7 日

--

![](img/b49c1deff780a74750c4dfda9056b149.png)

图片由[Gabriel Vasiliu](https://unsplash.com/@gabimedia?utm_source=medium&utm_medium=referral)提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上发布

这个故事解释了一种加速进入数据工程角色的方法，通过学习所需的技能并熟悉数据工程工具和技术。这对初级 IT 从业者和希望转行的中级软件工程师将非常有用。通过我作为英国和中东地区最成功初创公司的数据工程主管的多年经验，我从职业生涯中学到了很多，我希望与您分享这些知识和经验。这是我在数据工程领域获得的个人经验的反映。我希望这对您有用。

## 数据工程师——角色

首先，为什么选择数据工程师？

数据工程是一个令人兴奋且非常有回报的领域。这是一份迷人的工作，我们有机会处理所有与数据相关的事物——API、数据连接器、数据平台、商业智能以及市场上数十种数据工具。数据工程与机器学习（ML）紧密相关。你将创建和部署各种数据和 ML 管道。

> 这份工作绝对不会无聊，并且薪资优厚。

这份工作回报丰厚，因为建立一个良好的数据平台并不容易。它从需求收集和设计开始，需要相当的经验。这不是一项简单的任务，也需要一些真正出色的编程技能。工作本身是安全的，因为只要企业产生数据，这份工作就会有很高的需求。

> 公司总是会聘用那些知道如何高效处理（ETL）数据的人。

数据工程在过去五年里成为了英国增长最快的职业之一，在 2023 年 LinkedIn 的最受欢迎职业榜单中排名第 13 [1]。另一个加入的理由是稀缺性。在 IT 领域，如今找到一个优秀的数据工程师是非常困难的。

> 作为“数据工程负责人”，我每周在 LinkedIn 上收到 4 个职位面试邀请。平均而言，初级数据工程角色的需求更高。

根据 DICE 的技术职位研究，数据工程师是增长最快的技术职业：

![](img/ee08b504be177749be9112bfa0f2dd1c.png)

来源：[DICE](http://marketing.dice.com/pdf/2020/Dice_2020_Tech_Job_Report.pdf)

## 现代数据栈

现代数据栈指的是一系列数据处理工具和数据平台类型。

> 你在这个领域吗？

“你在这个领域吗？”——这是我在一次面试中被问到的问题。你会希望能够回答这个问题，并了解相关新闻、首次公开募股、最近的发展、突破、工具和技术。

熟悉常见的数据平台架构类型，即数据湖、湖仓、数据仓库，并准备好回答它们使用哪些工具。查看这篇文章了解一些示例：

## 数据平台架构类型

### 它在多大程度上满足了你的业务需求？选择的困境。

## 数据平台架构类型

## 数据管道

作为一名数据工程师，你几乎每天都会被要求进行数据管道设计。你会希望熟悉数据管道设计模式，并能够解释何时使用它们。将这些知识应用于实践是至关重要的，因为它决定了使用哪些工具。正确的数据转换工具组合可以使数据管道极其高效。

因此，我们需要准确知道何时应用流数据处理，何时应用批处理。一种可能非常昂贵，而另一种则可能节省数千。然而，业务需求可能在每种情况下都不同。本文提供了全面的数据管道设计模式列表：

## 数据管道设计模式

### 选择合适的架构及示例

## 数据管道设计模式

## 数据建模

我认为数据建模是数据工程的一个关键部分。许多数据平台的设计方式是将数据“原样”加载到数据仓库解决方案中。这被称为 ELT 方法。数据工程师经常需要使用标准 SQL 方言创建数据转换管道。良好的 SQL 技能是必不可少的。确实，SQL 对于分析查询是自然的，现如今几乎已经成为标准。它有助于高效查询数据，并让所有业务利益相关者轻松使用分析功能。

数据工程师必须知道如何**清洗、丰富和更新数据集**。例如，使用 MERGE 执行增量更新。在你的工作台或数据仓库（DWH）中运行此 SQL。它解释了它的工作原理：

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

一些高级 SQL 提示和技巧可以在这里找到：

[## 面向初学者的高级 SQL 技巧](https://towardsdatascience.com/advanced-sql-techniques-for-beginners-211851a28488?source=post_page-----c0319cb226c2--------------------------------)

### 在 1 到 10 的尺度上，你的数据仓库技能有多好？

towardsdatascience.com

## 编码

这非常重要，因为数据工程不仅仅涉及数据建模和 SQL。可以将数据工程师视为软件工程师。他们必须具备 ETL/ELT 技术的良好知识，并且至少能够使用 Python 编码。是的，显然 Python 无疑是数据工程最方便的编程语言，但你用 Python 可以做的任何事情，都可以用其他语言轻松完成，比如 JavaScript 或 Java。不要限制自己，你会有时间学习公司选择作为其技术栈主要语言的任何语言。

我建议从**数据 API 和请求**开始。将这些知识与云服务结合，为未来可能需要的任何 ETL 过程提供了一个很好的基础。

> 我们不能知道一切，也不要求成为编码专家，但我们必须知道如何处理数据。

以将数据加载到 BigQuery 数据仓库为例。它将使用 BigQuery 客户端库 [5] 将行插入到表中：

```py
from google.cloud import bigquery
...
client = bigquery.Client(credentials=credentials, project=credentials.project_id)
...

def _load_table_from_csv(table_schema, table_name, dataset_id):
    '''Loads data into BigQuery table from a CSV file.
    ! source file must be comma delimited CSV:
    transaction_id,user_id,total_cost,dt
    1,1,10.99,2023-04-15
    blob = """transaction_id,user_id,total_cost,dt\n1,1,10.99,2023-04-15"""
    '''

    blob = """transaction_id,user_id,total_cost,dt
    1,1,10.99,2023-04-15
    2,2, 4.99,2023-04-12
    4,1, 4.99,2023-04-12
    5,1, 5.99,2023-04-14
    6,1,15.99,2023-04-14
    7,1,55.99,2023-04-14"""

    data_file = io.BytesIO(blob.encode())

    print(blob)
    print(data_file)

    table_id = client.dataset(dataset_id).table(table_name)
    job_config = bigquery.LoadJobConfig()
    schema = create_schema_from_yaml(table_schema)
    job_config.schema = schema

    job_config.source_format = bigquery.SourceFormat.CSV,
    job_config.write_disposition = 'WRITE_APPEND',
    job_config.field_delimiter =","
    job_config.null_marker ="null",
    job_config.skip_leading_rows = 1

    load_job = client.load_table_from_file(
        data_file,
        table_id,
        job_config=job_config,
        )

    load_job.result()
    print("Job finished.")
```

我们不能知道一切，也不要求成为编码专家，但我们必须知道如何处理数据。

我们可以在本地运行它，也可以将其部署到云端作为无服务器应用程序。它可以由我们选择的任何其他服务触发。例如，部署 AWS Lambda 或 GCP Cloud Function 会非常高效。它将轻松处理我们的数据管道事件，几乎不产生成本。我的博客中有很多文章解释了它的简便性和灵活性。

## Airflow、Airbyte、Luigi、Hudi……

使用帮助管理数据平台和编排数据管道的第三方框架和库进行实验。许多框架是开源的，如 Apache Hudi [6]，并且从不同角度帮助理解数据平台管理。它们中的许多在管理批处理和流处理工作负载方面表现出色。我通过使用它们学到了很多东西。例如，Apache Airflow 提供了很多现成的数据连接器。我们可以使用它们轻松地运行任何云供应商（AWS、GCP、Azure）的 ETL 任务。

使用这些框架创建批处理数据处理作业非常容易。如果我们深入了解，它确实可以使 ETL 的实际操作更加清晰。

例如，我使用 airflow 连接器构建了一个机器学习管道来训练推荐引擎：

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

它将创建一个简单的数据管道图，将数据导出到云存储桶中，然后使用 MLEngineTrainingOperator 训练 ML 模型。

![](img/e316a43ba542db4191a0a78ccfa00721.png)

使用 Airflow 进行 ML 模型训练。图片由作者提供。

## 编排数据管道

框架非常重要，但数据工程师必须知道如何创建自己的框架来编排数据管道。这将我们带回到原始的基础编码以及使用客户端库和 API。在这里，一个好的建议是熟悉数据工具及其 API 端点。通常，创建并部署我们自己的微服务来执行 ETL/ELT 任务更加直观和容易。

> 使用你自己的工具编排数据管道

例如，我们可以创建一个简单的无服务器应用程序，它将从消息代理（如 SNS）中获取数据。然后，它可以处理这些事件并编排我们创建的其他微服务来执行 ETL 任务。另一个例子是一个简单的 AWS Lambda，它由数据湖中创建的新文件触发，然后根据它从管道配置文件中读取的信息，它可以决定调用哪个服务或将数据加载到哪个表中。

请考虑下面的这个应用程序。它是一个非常简单的 AWS Lambda，可以在本地运行或在云中部署时运行。

```py
./stack
├── deploy.sh                 # Shell script to deploy the Lambda
├── stack.yaml                # Cloudformation template
├── pipeline_manager
|   ├── env.json              # enviroment variables                
│   └── app.py                # Application
├── response.json             # Lambda response when invoked locally
└── stack.zip                 # Lambda package
```

`app.py` 可以是任何 ETL 任务，我们只需添加一些逻辑，就像在前面的示例中使用几个客户端库将数据加载到 BigQuery 中一样：

```py
# ./pipeline_manager/app.py
def lambda_handler(event, context):
   message = 'Hello {} {}!'.format(event['first_name'], event['last_name']) 
   return {
       'message' : message
   }
```

现在我们可以在本地运行它或使用基础设施即代码进行部署。这个命令行脚本将在本地运行此服务：

```py
pip install python-lambda-local
cd stack
python-lambda-local -e pipeline_manager/env.json -f lambda_handler pipeline_manager/app.py event.json
```

另外，它也可以部署到云中，我们可以从那里调用它。这将我们带入了云环境。

## 云服务提供商

现在一切都在云端管理。这就是为什么至少学习一种供应商的服务至关重要。可以是 AWS、GCP 或 Azure。它们是领导者，我们希望专注于其中之一。获得云认证是理想的，比如“[Google Cloud Professional Data Engineer](https://cloud.google.com/learn/certification/data-engineer)” [7] 或类似的认证。这些考试很难，但值得获得，因为它提供了数据处理工具的良好概述，并使我们看起来非常可信。我考过一次，已在文章中记录了我的经验，你可以在我的故事中找到。

数据工程师使用云功能和/或 docker 创建的所有内容都可以部署在云中。考虑以下 AWS CloudFormation 堆栈模板。我们可以使用它来部署我们的简单 ETL 微服务：

```py
# stack.yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: AWS S3 data lake stack.
Parameters:

  ServiceName:
    Description: Data lake microservice to process data files and load them into __ BigQuery.
    Type: String
    Default: datalake-manager
  StackPackageS3Key:
    Type: String
    Default: pipeline_manager/stack.zip
  Testing:
    Type: String
    Default: 'false'
    AllowedValues: ['true','false']
  Environment:
    Type: String
    Default: 'staging'
    AllowedValues: ['staging','live','test']
  AppFolder:
    Description: app.py file location inside the package, i.e. ./stack/pipeline_manager/app.py.
    Type: String
    Default: pipeline_manager
  LambdaCodeLocation:
    Description: Lambda package file location.
    Type: String
    Default: datalake-lambdas.aws

Resources:

  PipelineManagerLambda:
    Type: AWS::Lambda::Function
    DeletionPolicy: Delete
    DependsOn: LambdaPolicy
    Properties:
      FunctionName: !Join ['-', [!Ref ServiceName, !Ref Environment] ] # pipeline-manager-staging if staging.
      Handler: !Sub '${AppFolder}/app.lambda_handler'
      Description: Microservice that orchestrates data loading into BigQuery from AWS to BigQuery project your-project-name.schema.
      Environment:
        Variables:
          DEBUG: true
          LAMBDA_PATH: !Sub '${AppFolder}/' # i.e. 'pipeline_manager/'
          TESTING: !Ref Testing
          ENV: !Ref Environment
      Role: !GetAtt LambdaRole.Arn
      Code:
        S3Bucket: !Sub '${LambdaCodeLocation}' #datalake-lambdas.aws
        S3Key:
          Ref: StackPackageS3Key
      Runtime: python3.8
      Timeout: 360
      MemorySize: 128
      Tags:
        -
          Key: Service
          Value: Datalake

  LambdaRole:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument:
        Version: "2012-10-17"
        Statement:
          -
            Effect: Allow
            Principal:
              Service:
                - "lambda.amazonaws.com"
            Action:
              - "sts:AssumeRole"

  LambdaPolicy:
    Type: AWS::IAM::Policy
    DependsOn: LambdaRole
    Properties:
      Roles:
        - !Ref LambdaRole
      PolicyName: 'pipeline-manager-lambda-policy'
      PolicyDocument:
        {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Action": [
                        "logs:CreateLogGroup",
                        "logs:CreateLogStream",
                        "logs:PutLogEvents"
                    ],
                    "Resource": "*"
                }
            ]
        }
```

如果我们在命令行中运行这个 shell 脚本，它将部署我们的服务和所有所需的资源，如 IAM 策略到云中：

```py
aws \
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

之后，我们可以使用 SDK 通过在命令行工具中运行此 CLI 命令来调用我们的服务：

```py
aws lambda invoke \
    --function-name pipeline-manager \
    --payload '{ "first_name": "something" }' \
    response.json
```

## 精通命令行工具

云供应商的命令行工具非常有用，有助于创建测试我们在云中部署的 ETL 服务的脚本。数据工程师经常使用它。与数据湖一起工作时，我们希望掌握帮助我们管理云存储的 CLI 命令，即上传、下载和复制文件和对象。为什么我们要这样做？因为云存储中的文件经常触发各种 ETL 服务。批处理是一个非常常见的数据转换模式，为了调查错误和问题，我们可能需要在桶之间下载或复制文件。

![](img/c59684bded8474031b90c752aed36ea9.png)

在数据湖对象上使用 AWS Lambda 进行 ETL。作者提供的图像。

在这个示例中，我们可以看到服务将数据输出到 Kinesis，然后存储在数据湖中。当 S3 中创建文件对象时，它们会触发由 AWS Lambda 处理的 ETL 过程。结果被保存到 S3 桶中，以供 AWS Athena 使用，从而生成一个使用 AWS Quicksight 的 BI 报告。

这是我们可能在某个时刻想要使用的一组 AWS CLI 命令：

## 复制和上传文件

```py
mkdir data
cd data
echo transaction_id,user_id,dt \\n101,777,2021-08-01\\n102,777,2021-08-01\\n103,777,2021-08-01\\n > simple_transaction.csv
aws  s3 cp ./data/simple_transaction.csv s3://your.bucket.aws/simple_transaction_from_data.csv
```

## 递归复制/上传/下载文件夹中的所有文件

```py
aws  s3 cp ./data s3://your.bucket.aws --recursive
```

## 递归删除所有内容

```py
aws  s3 rm s3://your.bucket.aws/ --recursive --exclude ""
```

## 删除一个桶

```py
aws  s3 rb s3://your.bucket.aws/
```

还有更多高级示例，但我认为这个概念已经很清楚了。

> 我们希望使用脚本有效地管理云存储。

我们可以将这些命令链接成 shell 脚本，这使得 CLI 成为一个非常强大的工具。

例如，考虑这个 shell 脚本。它将检查是否存在 lambda 包的存储桶，上传并部署我们的 ETL 服务作为 Lambda 函数：

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

更高级的示例可以在我之前的故事中找到。

## 数据质量

现在，当我们知道如何部署 ETL 服务、执行请求并从外部 API 拉取数据时，我们需要学习如何观察我们在数据平台上拥有的数据。理想情况下，我们希望在数据流入数据平台时实时检查数据质量。可以通过 ETL 或 ELT 方法来实现。使用 Kafka 或 Kinesis 构建的流应用程序具有分析数据质量的库，数据在数据管道中流动时可以进行分析。当数据工程师将数据可观察性和数据质量管理委派给其他在数据仓库中工作的利益相关者时，ELT 方法更为可取。就个人而言，我喜欢后者，因为它节省了时间。将数据仓库解决方案视为公司中每个人的单一真实来源。财务、市场营销和客户服务团队可以访问数据并检查**任何潜在问题**。这些中我们通常会看到以下情况：

+   缺失数据

+   数据源中断

+   数据源在模式字段更新时发生变化

+   各种数据异常，如异常值或不寻常的应用程序/用户行为。

数据工程师创建警报并安排通知，以便对潜在的数据问题保持关注。

考虑这个例子，当每天发送邮件通知利益相关者有关数据中断的情况：

![](img/c9b2a060d7071e8e6c95c403b0901690.png)

邮件警报。图片由作者提供。

在我的故事中，你可以找到一篇文章，解释如何使用 SQL 安排这样的数据监控工作流。

[](/automated-emails-and-data-quality-checks-for-your-data-1de86ed47cf0?source=post_page-----c0319cb226c2--------------------------------) ## 自动化邮件和数据质量检查

### 数据仓库指南以便更好、更干净的数据和定期邮件

towardsdatascience.com

考虑以下代码片段。它将检查昨天的数据是否有任何缺失字段，并在发现时发送通知警报：

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

## 数据环境

数据工程师测试数据管道。有多种方法来实现这一点。通常，它需要将数据环境分为生产和预发布管道。我们通常可能需要一个额外的沙盒用于测试目的，或者在我们的 ETL 服务触发 CI/CD 工作流时运行数据转换单元测试。

这是一种常见做法，面试官可能会问一些相关问题。刚开始可能有点棘手，但下面的图解解释了它是如何工作的。

![](img/081619c52103b0f55e7e9c6dca8bd0f8.png)

CI/CD 工作流示例，用于 ETL 服务。图片由作者提供。

例如，我们可以使用基础设施即代码和 GitHub Actions 来部署和测试任何来自开发分支的拉取请求中的预发布资源。当所有测试通过且我们对 ETL 服务满意时，我们可以通过合并到主分支将其提升到生产环境。

请看下面这个 GitHub Action 工作流。它将会在预生产环境中部署我们的 ETL 服务并进行测试。这样的做法有助于减少错误并更快地交付数据管道。

```py
# .github/workflows/deploy_staging.yaml
name: STAGING AND TESTS

on:
  #when there is a push to the master
  push:
    branches: [ master ]
  #when there is a pull to the master
  pull_request:
    branches: [ master ]

jobs:
 compile:
   runs-on: ubuntu-latest
   steps:
     - name: Checkout code into workspace directory
       uses: actions/checkout@v2
     - name: Install AWS CLI v2
       run:  |
             curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o /tmp/awscliv2.zip
             unzip -q /tmp/awscliv2.zip -d /tmp
             rm /tmp/awscliv2.zip
             sudo /tmp/aws/install --update
             rm -rf /tmp/aws/
     - name: test AWS connectivity
       run: aws s3 ls
       env:
         AWS_ACCESS_KEY_ID: ${{ secrets.MDS_AWS_ACCESS_KEY_ID }}
         AWS_SECRET_ACCESS_KEY: ${{ secrets.MDS_AWS_SECRET_ACCESS_KEY }}
         AWS_DEFAULT_REGION: "eu-west-1"

     - name: Deploy staging
       run: |
            cd stack
            date
            TIME=`date +"%Y%m%d%H%M%S"`
            base=${PWD##*/}
            zp=$base".zip"
            rm -f $zp
            pip install --target ./package pyyaml==6.0
            cd package
            zip -r ../${base}.zip .
            cd $OLDPWD
            zip -r $zp ./pipeline_manager
            # Upload Lambda code (replace with your S3 bucket):
            aws s3 cp ./${base}.zip s3://datalake-lambdas.aws/pipeline_manager/${base}${TIME}.zip
            STACK_NAME=SimpleCICDWithLambdaAndRole
            aws \
            cloudformation deploy \
            --template-file stack_cicd_service_and_role.yaml \
            --stack-name $STACK_NAME \
            --capabilities CAPABILITY_IAM \
            --parameter-overrides \
            "StackPackageS3Key"="pipeline_manager/${base}${TIME}.zip" \
            "Environment"="staging" \
            "Testing"="false"
       env:
         AWS_ACCESS_KEY_ID: ${{ secrets.MDS_AWS_ACCESS_KEY_ID }}
         AWS_SECRET_ACCESS_KEY: ${{ secrets.MDS_AWS_SECRET_ACCESS_KEY }}
         AWS_DEFAULT_REGION: "eu-west-1"
```

在我的一个故事中有一个完整的解决方案示例。

## 机器学习

添加一个机器学习组件将使我们成为机器学习工程师。数据工程与机器学习非常接近，因为数据工程师创建的数据管道通常被机器学习服务使用。

> 我们不需要了解每一种机器学习模型。

我们无法与像亚马逊和谷歌这样的云服务提供商在机器学习和数据科学领域竞争，但我们需要知道如何使用它。

云供应商提供了许多托管的机器学习服务，我们希望熟悉这些服务。数据工程师为这些服务准备数据集，做几个相关的教程肯定会很有用。

例如，考虑一个用户流失预测项目，以了解用户流失情况以及如何使用托管的机器学习服务为用户生成预测。这可以通过 BigQuery ML [9] 轻松完成，只需创建一个简单的逻辑回归模型，如下所示：

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

然后我们可以使用 SQL 生成预测：

```py
SELECT
  user_pseudo_id,
  churned,
  predicted_churned,
  predicted_churned_probs[OFFSET(0)].prob as probability_churned
FROM
  ML.PREDICT(MODEL sample_churn_model.churn_model,
  (SELECT * FROM sample_churn_model.churn)) #can be replaced with a proper test dataset
order by 3 desc
```

## 结论

我试图总结一组数据工程技能和技术，这些技能通常是入门级数据工程师角色所需的。根据我的经验，这些技能可以在两到三个月的积极学习中掌握。我建议从云服务提供商和 Python 开始，建立一个简单的 ETL 服务，并为预生产和生产环境设置 CI/CD 管道。这不需要花费任何费用，我们可以通过在本地和云中运行这些服务来快速学习。当前市场上对数据工程师的需求很高。我希望这篇文章能帮助你学习一些新知识，并为工作面试做准备。

## 推荐阅读

[1] [`www.linkedin.com/pulse/linkedin-jobs-rise-2023-25-uk-roles-growing-demand-linkedin-news-uk/`](https://www.linkedin.com/pulse/linkedin-jobs-rise-2023-25-uk-roles-growing-demand-linkedin-news-uk/)

[2] `towardsdatascience.com/data-platform-architecture-types-f255ac6e0b7`

[3] `towardsdatascience.com/data-pipeline-design-patterns-100afa4b93e3`

[4] [`medium.com/towards-data-science/advanced-sql-techniques-for-beginners-211851a28488`](https://medium.com/towards-data-science/advanced-sql-techniques-for-beginners-211851a28488)

[5] [`cloud.google.com/python/docs/reference/bigquery/latest`](https://cloud.google.com/python/docs/reference/bigquery/latest)

[6] [`hudi.apache.org/docs/overview/`](https://hudi.apache.org/docs/overview/)

[7] [`cloud.google.com/learn/certification/data-engineer`](https://cloud.google.com/learn/certification/data-engineer)

[8] `towardsdatascience.com/automated-emails-and-data-quality-checks-for-your-data-1de86ed47cf0`

[9] [`cloud.google.com/bigquery/docs/bqml-introduction`](https://cloud.google.com/bigquery/docs/bqml-introduction)
