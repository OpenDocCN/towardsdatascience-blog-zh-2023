# 如何基于 AWS 构建级联数据管道（第一部分）

> 原文：[`towardsdatascience.com/how-i-built-a-cascading-data-pipeline-based-on-aws-997b212a84d2`](https://towardsdatascience.com/how-i-built-a-cascading-data-pipeline-based-on-aws-997b212a84d2)

## 自动化、可扩展且强大

[](https://anzhemeng.medium.com/?source=post_page-----997b212a84d2--------------------------------)![Memphis Meng](https://anzhemeng.medium.com/?source=post_page-----997b212a84d2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----997b212a84d2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----997b212a84d2--------------------------------) [Memphis Meng](https://anzhemeng.medium.com/?source=post_page-----997b212a84d2--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----997b212a84d2--------------------------------) ·14 分钟阅读·2023 年 7 月 31 日

--

今天我要分享一些我引以为豪的数据工程项目的经验。你将了解到我为何使用这些工具和 AWS 组件，以及我如何设计架构。

![](img/4f95b8b2918bb62a2337706df55ff6e3.png)

作者提供的图片

**免责声明**：本文本内容受到我与一个未命名实体的经验启发。然而，某些关键商业利益和细节已故意用虚构数据/代码替代或省略，以保持机密性和隐私。因此，实际涉及的商业利益的完整和准确程度保留。

# 前提条件

1.  对 Python 的了解

1.  对 AWS 组件的了解，如 DynamoDB、Lambda 无服务器、SQS 和 CloudWatch

1.  对 [YAML](https://en.wikipedia.org/wiki/YAML) 和 [SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html) 的舒适编码体验

# 背景

假设你是一个数据工程师，需要不断更新数据仓库中的数据。例如，你需要定期与 [Dunder Mifflin Paper Co.](https://dundermifflinpaper.com) 的销售记录同步。（我知道这不是一个现实的场景，但请尽情享受 :)！）数据通过供应商的 API 发送给你，你需要确保分支、员工（实际上只考虑销售人员）和销售的信息是最新的。提供的 API 有以下 3 个路径：

1.  `/branches` ，接受分支名称作为查询参数以检索指定分支的元数据；

1.  `/employees` ，接受分支 ID 作为查询参数以检索某个分支所有员工的信息，响应中包括一个表示员工职业的键值对；

1.  `/sales`，接受员工 ID 作为查询参数来检索销售人员的所有销售记录，响应包括一个指示交易完成时间的键值对。

总的来说，API 的返回结果如下：

`/branches` 路径：

```py
{
  "result": [
   {
     "id": 1,
     "branch_name": "Scranton",
     "employees": 50,
     "location": "Scranton, PA",
     ...
   }
 ]
}
```

`/employees` 路径：

```py
{
"result": {
  "branch_id": 1,
  "employees": [
   {
     "id": 1234,
     "occupation": "data engineer",
     "name": "John Doe",
     ...
   },
   {
     "id": 1235,
     "occupation": "salesperson",
     "name": "Jim Doe",
     ...
   },
   ...
  ],
  ...
 }
}
```

`/sales` 路径：

```py
{
  "result": {
  "employee_id": 1235,
  "sales: [
   {
     "id": 3972,
     "transaction_timestamp": "2023-01-01 23:43:23",
     ...
   },
   {
     "id": 4002,
     "transaction_timestamp": "2023-01-05 12:23:31",
     ...
   },
   ...
  ],
  ...
 }
}
```

期望你的最终工作将使数据分析师能够仅通过 SQL 查询来检索他们分析所需的数据。

# 思路

最终，我们将有 3 个不同的地方分别存储分支、销售人员和销售的数据，这一点说起来很简单。数据将通过访问特定的 API 路径来导入。然而，由于所有这些实体的标识符大多是自动生成的，实践者不大可能提前获得这些 ID。相反，由于我们通常可以找到分支名称，因此可以使用第一个路径来获取分支的元数据及其员工的 ID。然后，我们可以使用员工 ID 访问`/employees`路径，`/sales`路径也是如此。***这就是我称这个流程为级联的原因。***

为了确保我们的数据库大部分时间是最新的，必须足够频繁地执行这些操作。但另一方面，我们也需要考虑成本和潜在的 API 访问配额。因此，每小时运行一次是适当的，尽管可以说尚未达到最佳。

最后但同样重要的是，让我们讨论 AWS。首先，执行这些操作的代码将由[**AWS Lambda**](https://aws.amazon.com/lambda/)运行，因为它能够使用 200 多个 AWS 服务和应用程序作为触发器，包括[SQS](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-pipes-sqs.html)和[EventBridge](https://docs.aws.amazon.com/eventbridge/index.html)。数据将通过**SQS**传递，作为 AWS 提供的最成熟的消息服务之一。最后，从 API 抓取的信息将存储在**DynamoDB**中。对于一些有经验的读者来说，利用**DynamoDB**作为数据仓库工具可能会令人困惑，因为这是一个 NoSQL 数据库服务，而数据仓库通常与 NoSQL 数据库不兼容。我确实意识到这一点，DynamoDB 表在这里将仅作为临时存储表，因为我可以利用其在键值/文档数据模型模式中的灵活性，然后最终将 JSON 格式的 API 检索数据转换为数据仓库记录。如果你对我实现 DynamoDB-S3 加载的细节感兴趣，可以查看[**这篇文章**](https://medium.com/plumbersofdatascience/a-real-time-streaming-channel-c8bbae187c83)。

# 实现

这是我最终工作的结构。

```py
Cascading-ETL-pipeline
├── LICENSE
├── README.md
├── branches
│   ├── Pipfile
│   ├── Pipfile.lock
│   ├── lambda_function.py
│   ├── requirements.txt
│   └── service
│       ├── config.py
│       └── service.py
├── sales
│   ├── Pipfile
│   ├── Pipfile.lock
│   ├── lambda_function.py
│   ├── requirements.txt
│   └── service
│       ├── config.py
│       └── service.py
├── salespersons
│   ├── Pipfile
│   ├── Pipfile.lock
│   ├── lambda_function.py
│   ├── requirements.txt
│   └── service
│       ├── config.py
│       └── service.py
├── template.yml
└── utils.py
```

有 3 个文件夹（*/branches*，*/salespersons*，*/sales*），分别包含每个 lambda 函数的代码。*Utils.py* 是一个多功能文件，其中的函数、变量和类被全局应用。模板.yml 是我们将用来声明和部署 AWS 资源以建立数据管道的 AWS CloudFormation 模板。

每个文件夹中的`Lambda_function.py`是代码执行的入口函数：

```py
import json
import logging
from pythonjsonlogger import jsonlogger
from service import service, config

# Load environment
ENV = config.load_env()

LOGGER = logging.getLogger()
# Replace the LambdaLoggerHandler formatter :
LOGGER.handlers[0].setFormatter(jsonlogger.JsonFormatter())
# Set default logging level
LOGGING_LEVEL = getattr(logging, ENV["LOGGING_LEVEL"])
LOGGER.setLevel(LOGGING_LEVEL)

def _lambda_context(context):
    """
    Extract information relevant from context object.

    Args:
        context: The context object provided by the Lambda runtime.

    Returns:
        dict: A dictionary containing relevant information from the context object.

    """
    return {
        "function_name": context.function_name,
        "function_version": context.function_version,
    }

# @datadog_lambda_wrapper
def lambda_handler(event, context):
    """
    Handle the Lambda event.

    Args:
        event(dict): The event object containing input data for the Lambda function.
        context(dict): The context object provided by the Lambda runtime.

    Returns:
        dict: A dictionary containing the response for the Lambda function.

    """
    LOGGER.info("Starting lambda executing.", extra=_lambda_context(context))
    service.main(event, ENV)
    LOGGER.info("Successful lambda execution.", extra=_lambda_context(context))
    return {"statusCode": 200}
```

/Service/config.py 返回*template.yml*中输入的环境变量：

```py
import os
import sys
import logging

LOGGER = logging.getLogger(__name__)

def load_env():
    """Load environment variables.

    Returns:
       dict: A dictionary containing the loaded environment variables.

    Raises:
        KeyError: If any required environment variable is missing.

    Notes:
        - The function attempts to load several environment variables including:
        - If any of the required environment variables are missing, a KeyError is raised.
        - The function logs an exception message indicating the missing environment variable and exits the program with a status code of 1.
    """
    try:
        return {
            "LOGGING_LEVEL": os.environ["LOGGING_LEVEL"],
            "APP_ENV": os.environ["APP_ENV"],
            "SQS": os.environ["SQS"],
            "DB": os.environ["DB"],
        }
    except KeyError as error:
        LOGGER.exception("Enviroment variable %s is required.", error)
        sys.exit(1)
```

`/Service/service.py` 是我们实际处理数据的地方。基本上，该功能由一个或两个触发器（时间调度器）调用，在从数据源（API 或 SQS 队列）中检索数据之前。数据将以键值对的形式打包，如有需要，更新到相应的 DynamoDB 表中，然后该功能分发其成员的标识符（即，分支中的所有员工，销售人员的所有销售记录）。

以 `/branches/service/service.py` 为例。它的功能包括：

1.  一旦被唤醒，立即从 API `/branches` 获取所有数据；

1.  检查 DynamoDB 数据表中每个销售人员的个人信息是否存在和准确，如果不更新，则插入数据记录；

1.  获取所有员工的 ID，并将其连同分支 ID 作为消息通过 SQS 队列传递给尾部函数（*/salespersons*）。

实际操作中，实施方式如下：

```py
import logging, requests, sys
from utils import *
from boto3.dynamodb.conditions import Key

LOGGER = logging.getLogger(__name__)

def main(event, environment):
    """Process invoking event data and update the DynamoDB table based on specified branches.

    Args:
        event (dict): A JSON-formatted document that contains data for a Lambda function to process.
        environment (dict): A context object that provides methods and properties about the invocation, function and runtime environment.

    Returns:
        None

    Raises:
        SystemExit: If an exception occurs during the execution.

    Notes:
        - If `event` does not contain the 'branches' key, the function will default to processing information for all branches.
        - The function retrieves branch-specific information from a URL and updates the DynamoDB table accordingly.
        - The updated information is then delivered to an SQS queue for further processing.

    """
    LOGGER.info(event)

    if not event.get("branches"):
        # default to look up all branches if the value is an empty list
        branches = [
            "Scranton",
            "Akron",
            "Buffalo",
            "Rochester",
            "Syracuse",
            "Utica",
            "Binghamton",
            "Albany",
            "Nashua",
            "Pittsfield",
            "Stamford",
            "Yonkers",
            "New York",
        ]
    else:
        branches = event["branches"]  # should be an array

    queue = environment["SQS"]
    table = environment["DB"]

    try:
        for branch in branches:
            # go to a path that allows users to retrieve all information of the specified branch(es) based on input date range
            response = requests.get(
                url=f"www.dundermifflinpaper.com/branches/?branch={branch}"
            )
            response = response.json()
            branches = response.get("result")
            for result in branches:
                if not upToDate(
                    table,
                    Key("branch_id").eq(str(result["id"])),
                    result,
                    "branch_",
                ):
                    # only update DynamoDB table when it's NOT complete ingesting
                    update_info(table, result)

            deliver_message(queue, str({"branch": result["branch_id"]}))
            LOGGER.info(f"sending branch {result['branch_id']} for the next stage")

    except Exception as e:
        LOGGER.error(str(e), exc_info=True)
        sys.exit(1)
```

最后，我们需要为构建和部署做准备：

```py
AWSTemplateFormatVersion: '2010-09-09'
Transform: 'AWS::Serverless-2016-10-31'
Parameters:  #   Type: String
  Environment:
    Type: String
Resources:
  # =========================================================================================
  # IAM ROLES, POLICIES, PERMISSIONS
  # =========================================================================================
  LambdaRole:
    Type: AWS::IAM::Role
    Properties:
      RoleName: !Sub '${AWS::StackName}-lambda-role'
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
        - Effect: Allow
          Principal:
            Service:
            - lambda.amazonaws.com
            - events.amazonaws.com
          Action:
          - sts:AssumeRole
      ManagedPolicyArns:
      - arn:aws:iam::aws:policy/AWSLambdaExecute
      - arn:aws:iam::aws:policy/AmazonSQSFullAccess
      - arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess
      Path: '/'
  LambdaPolicy:
    Type: AWS::IAM::Policy
    Properties:
      PolicyName: !Sub '${AWS::StackName}-lambda-policy'
      PolicyDocument:
        Version: '2012-10-17'
        Statement:
        - Sid: EventBusAccess
          Effect: Allow
          Action:
          - events:PutEvents
          Resource: '*'
        - Sid: LambdaInvokeAccess
          Effect: Allow
          Action:
          - lambda:InvokeFunction
          Resource: "*"
        - Sid: LogAccess
          Effect: Allow
          Action:
          - logs:CreateLogGroup
          - logs:CreateLogStream
          - logs:PutLogEvents
          Resource: arn:aws:logs:*:*:*
      Roles:
      - !Ref LambdaRole
  # =========================================================================================
  # AWS LAMBDA FUNCTIONS
  # ========================================================================================= 
  BranchCollector:
    Type: AWS::Serverless::Function
    Properties:
      FunctionName: !Sub branch-collector-${Environment}
      Handler: lambda_function.lambda_handler
      Runtime: python3.9
      CodeUri: branches/
      Description: updating branch info in our DynamoDB table
      MemorySize: 128
      Timeout: 900
      Role: !GetAtt LambdaRole.Arn
      Environment:
        Variables:
          LOGGING_LEVEL: INFO
          APP_ENV: !Ref Environment
          SQS: !Ref EmployeeQueue
          DB: !Sub branches-${Environment}
      DeadLetterQueue:
        Type: SQS
        TargetArn: 
          Fn::GetAtt: BranchFunctionDeadLetterQueue.Arn
      Events:
        StartScheduledEvent:
          Type: Schedule
          Properties:
            Schedule: rate(1 hour)

  SalespersonCollector:
    Type: AWS::Serverless::Function
    Properties:
      FunctionName: !Sub salesperson-collector-${Environment}
      Handler: lambda_function.lambda_handler
      Runtime: python3.9
      CodeUri: salespersons/
      Description: updating salesperson info in our DynamoDB table
      MemorySize: 128
      Timeout: 900
      Role: !GetAtt LambdaRole.Arn
      ReservedConcurrentExecutions: 5
      Environment:
        Variables:
          LOGGING_LEVEL: INFO
          APP_ENV: !Ref Environment
          SOURCE_SQS: !Ref EmployeeQueue
          TARGET_SQS: !Ref SaleQueue
          DB: !Sub salespersons-${Environment}
      DeadLetterQueue:
        Type: SQS
        TargetArn: 
          Fn::GetAtt: EmployeeFunctionDeadLetterQueue.Arn
      Events:
        StartScheduledEvent:
          Type: Schedule
          Properties:
            # every minute
            Schedule: rate(1 minute)

  SaleCollector:
    Type: AWS::Serverless::Function
    Properties:
      FunctionName: !Sub sale-collector-${Environment}
      Handler: lambda_function.lambda_handler
      Runtime: python3.9
      CodeUri: sales/
      Description: updating sales info in our DynamoDB table
      MemorySize: 128
      Timeout: 900
      ReservedConcurrentExecutions: 3
      Role:
        Fn::GetAtt:
        - LambdaRole
        - Arn
      Environment:
        Variables:
          LOGGING_LEVEL: INFO
          APP_ENV: !Ref Environment
          SQS: !Ref SaleQueue
          DB: !Sub sales-${Environment}
      DeadLetterQueue:
        Type: SQS
        TargetArn: 
          Fn::GetAtt: SaleFunctionDeadLetterQueue.Arn
      Events:
        StartScheduledEvent:
          Type: Schedule
          Properties:
            # every minute
            Schedule: rate(1 minute)

  # =========================================================================================
  # AWS DynamoDB TABLES
  # ========================================================================================= 
  BranchDynamoDBTable: 
    Type: AWS::DynamoDB::Table
    DeletionPolicy: Delete
    Properties:
      BillingMode: PAY_PER_REQUEST 
      AttributeDefinitions: 
        - 
          AttributeName: "branch_id"
          AttributeType: "S"
      KeySchema: 
        - 
          AttributeName: "branch_id"
          KeyType: "HASH"
      StreamSpecification:
        StreamViewType: NEW_IMAGE
      TableName: !Sub branch-${Environment}

  SalespersonDynamoDBTable: 
    Type: AWS::DynamoDB::Table
    DeletionPolicy: Delete
    Properties:
      BillingMode: PAY_PER_REQUEST 
      AttributeDefinitions: 
        - 
          AttributeName: "employee_id"
          AttributeType: "S"
        - 
          AttributeName: "branch_id"
          AttributeType: "S"
      KeySchema: 
        - 
          AttributeName: "employee_id"
          KeyType: "HASH"
        - 
          AttributeName: "branch_id"
          KeyType: "RANGE"
      StreamSpecification:
        StreamViewType: NEW_IMAGE
      TableName: !Sub salesperson-${Environment}

  SaleDynamoDBTable:
    Type: AWS::DynamoDB::Table
    DeletionPolicy: Delete
    Properties:
      BillingMode: PAY_PER_REQUEST 
      AttributeDefinitions: 
        - 
          AttributeName: "sale_id"
          AttributeType: "S"
        - 
          AttributeName: "employee_id"
          AttributeType: "S"
      KeySchema: 
        - 
          AttributeName: "sale_id"
          KeyType: "HASH"
        - 
          AttributeName: "employee_id"
          KeyType: "RANGE"
      StreamSpecification:
        StreamViewType: NEW_IMAGE
      TableName: !Sub sale-${Environment}

  # =========================================================================================
  # AWS SQS QUEUES
  # ========================================================================================= 
  EmployeeQueue:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: !Sub employee-queue-${Environment}
      VisibilityTimeout: 900
      RedrivePolicy:
        deadLetterTargetArn: 
          Fn::GetAtt: EmployeeWorkloadDeadLetterQueue.Arn
        maxReceiveCount: 10

  EmployeeWorkloadDeadLetterQueue: 
    Type: AWS::SQS::Queue
    Properties:
      QueueName: !Sub employee-workload-dead-letter-queue-${Environment}
      MessageRetentionPeriod: 1209600

  BranchFunctionDeadLetterQueue: 
    Type: AWS::SQS::Queue
    Properties:
      QueueName: !Sub branch-function-dead-letter-queue-${Environment}
      MessageRetentionPeriod: 1209600

  SaleQueue:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: !Sub sale-queue-${Environment}
      VisibilityTimeout: 900
      RedrivePolicy:
        deadLetterTargetArn: 
          Fn::GetAtt: SaleWorkloadDeadLetterQueue.Arn
        maxReceiveCount: 10

  SaleWorkloadDeadLetterQueue: 
    Type: AWS::SQS::Queue
    Properties:
      QueueName: !Sub sale-workload-dead-letter-queue-${Environment}
      MessageRetentionPeriod: 1209600

  EmployeeFunctionDeadLetterQueue: 
    Type: AWS::SQS::Queue
    Properties:
      QueueName: !Sub employee-function-dead-letter-queue-${Environment}
      MessageRetentionPeriod: 1209600

  SaleFunctionDeadLetterQueue:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: !Sub sale-function-dead-letter-queue-${Environment}
      MessageRetentionPeriod: 1209600

  # =========================================================================================
  # AWS CLOUDWATCH ALARMS
  # =========================================================================================
  BranchErrorAlarm:
    Type: AWS::CloudWatch::Alarm
    Properties:
      ComparisonOperator: GreaterThanOrEqualToThreshold
      Dimensions:
        - Name: FunctionName
          Value: !Ref BranchCollector
      EvaluationPeriods: 1
      MetricName: Errors
      Namespace: AWS/Lambda
      Period: 300
      Statistic: Sum
      Threshold: '1'
      AlarmActions: 
        - arn:aws:sns:us-east-1:{id}:{alarm-action-name}
  BranchDurationAlarm:
    Type: AWS::CloudWatch::Alarm
    Properties:
      ComparisonOperator: GreaterThanOrEqualToThreshold
      Dimensions:
        - Name: FunctionName
          Value: !Ref BranchCollector
      EvaluationPeriods: 1
      MetricName: Duration
      Namespace: AWS/Lambda
      Period: 60
      Statistic: Maximum
      Threshold: '750000'
      AlarmActions:
        - arn:aws:sns:us-east-1:{id}:{alarm-action-name}
  BranchThrottleAlarm:
    Type: AWS::CloudWatch::Alarm
    Properties:
      ComparisonOperator: GreaterThanOrEqualToThreshold
      Dimensions:
        - Name: FunctionName
          Value: !Ref BranchCollector
      EvaluationPeriods: 1
      MetricName: Throttles
      Namespace: AWS/Lambda
      Period: 300
      Statistic: Sum
      Threshold: '1'
      AlarmActions:
        - arn:aws:sns:us-east-1:{id}:{alarm-action-name}

  SalespersonErrorAlarm:
    Type: AWS::CloudWatch::Alarm
    Properties:
      ComparisonOperator: GreaterThanOrEqualToThreshold
      Dimensions:
        - Name: FunctionName
          Value: !Ref SalespersonCollector
      EvaluationPeriods: 1
      MetricName: Errors
      Namespace: AWS/Lambda
      Period: 300
      Statistic: Sum
      Threshold: '1'
      AlarmActions: 
        - arn:aws:sns:us-east-1:{id}:{alarm-action-name}
  SalespersonDurationAlarm:
    Type: AWS::CloudWatch::Alarm
    Properties:
      ComparisonOperator: GreaterThanOrEqualToThreshold
      Dimensions:
        - Name: FunctionName
          Value: !Ref SalespersonCollector
      EvaluationPeriods: 1
      MetricName: Duration
      Namespace: AWS/Lambda
      Period: 60
      Statistic: Maximum
      Threshold: '750000'
      AlarmActions:
        - arn:aws:sns:us-east-1:{id}:{alarm-action-name}
  SalespersonThrottleAlarm:
    Type: AWS::CloudWatch::Alarm
    Properties:
      ComparisonOperator: GreaterThanOrEqualToThreshold
      Dimensions:
        - Name: FunctionName
          Value: !Ref SalespersonCollector
      EvaluationPeriods: 1
      MetricName: Throttles
      Namespace: AWS/Lambda
      Period: 300
      Statistic: Sum
      Threshold: '1'
      AlarmActions:
        - arn:aws:sns:us-east-1:{id}:{alarm-action-name}

  SaleErrorAlarm:
    Type: AWS::CloudWatch::Alarm
    Properties:
      ComparisonOperator: GreaterThanOrEqualToThreshold
      Dimensions:
        - Name: FunctionName
          Value: !Ref SaleCollector
      EvaluationPeriods: 1
      MetricName: Errors
      Namespace: AWS/Lambda
      Period: 300
      Statistic: Sum
      Threshold: '1'
      AlarmActions: 
        - arn:aws:sns:us-east-1:{id}:{alarm-action-name}
  SaleDurationAlarm:
    Type: AWS::CloudWatch::Alarm
    Properties:
      ComparisonOperator: GreaterThanOrEqualToThreshold
      Dimensions:
        - Name: FunctionName
          Value: !Ref SaleCollector
      EvaluationPeriods: 1
      MetricName: Duration
      Namespace: AWS/Lambda
      Period: 60
      Statistic: Maximum
      Threshold: '750000'
      AlarmActions:
        - arn:aws:sns:us-east-1:{id}:{alarm-action-name}
  SaleThrottleAlarm:
    Type: AWS::CloudWatch::Alarm
    Properties:
      ComparisonOperator: GreaterThanOrEqualToThreshold
      Dimensions:
        - Name: FunctionName
          Value: !Ref SaleCollector
      EvaluationPeriods: 1
      MetricName: Throttles
      Namespace: AWS/Lambda
      Period: 300
      Statistic: Sum
      Threshold: '1'
      AlarmActions:
        - arn:aws:sns:us-east-1:{id}:{alarm-action-name}
```

# 问答

是的，我正在进行自问自答的环节。在编码时，挑战自己“为什么？”或“怎么做？”通常是有帮助的，这样我会对自己所做的每个决定有更多信心，也能证明我使用的每个工具。

## a. 我如何监控这些功能？

我使用[CloudWatch 警报](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)。CloudWatch 警报监视任何可用的指标或 AWS CloudWatch 支持的指标计算。它们可以根据给定阈值在指定时间内对指标或计算进行自定义操作。

对我来说，最关键的是在发生错误时立即学习和解决。因此，我为所有 3 个功能设置了以 1 个错误为阈值的警报，也就是说，只要出现错误就会触发警报。我希望在不不断盯着 CloudWatch 仪表板的情况下识别错误，因此警报的操作是将通知推送到 SNS 主题，该主题将警报转发到我的电子邮件收件箱。

如果你在一个协作环境中工作，我建议你通过将其发送到 Slack 频道，分发到分发列表中的所有地址，或将其包含在共享邮箱中，以扩展可见性。

## b. 你为什么按现在的方式定义表的键？

这是基于现实情况的。显然，分支之间有所不同，因此它们的 ID 足以作为分支表中的唯一**哈希键**，符合[1NF 约束](https://en.wikipedia.org/wiki/Database_normalization)。相比之下，销售人员和销售表则需要额外的键进行规范化。

因为在现实中，一个分支可能在记录中有多个员工，而员工允许从一个分支转移到另一个分支，从数据模式的角度来看，分支与员工之间的关系是多对多的。而且正因为如此，只有销售记录 ID + 销售人员 ID + 分支 ID（交易发生时的分支）组合才能指向销售表中的一个确切记录。瓶颈在于像 DynamoDB 这样的文档数据库允许最多两个属性作为键，我选择了销售人员 ID 作为**排序键**，而不是分支 ID，以确保销售记录与销售人员之间的关系。销售和分支之间的差异将在接下来的问题中解释。

## c. 我如何建立销售和分支之间的联系？为什么？

数据供应商在销售记录中未能包含分支信息。处理这个问题的万灵药是从最上层（分支收集器函数）到最底层附加分支 ID。然而，这种方式忽略了一些极端情况。例如，[吉姆·哈尔普特](https://en.wikipedia.org/wiki/Jim_Halpert) 在他在斯克兰顿分支的最后一天进行了销售。由于一些技术问题，这条记录没有被添加到他的销售记录列表中，也没有发布到 API 上，直到第二个工作日他被预设为转移到斯坦福德的员工。

没有上下文很难发现标签错误，尤其是当根本原因来自供应商时。从我的经验来看，此阶段的调试非常依赖于我们的反馈。这就是为什么我让销售表中的分支 ID 成为一个宽松的键值对；否则，需要额外的工作来删除和重写该项。

## d. 我如何触发销售人员和销售收集器函数？

SQS 队列是 Lambda 函数官方允许的触发操作之一，因此这是唤醒这两个函数的自然选择，因为它们已经设置为监听队列。我绕了一下路，绕过了 API 所有者施加的最大访问限制。如果我让我的函数在消息到达时立即从队列中获取并访问 API，将会有多个函数几乎同时处理消息，这使得管道架构失效，因为它可能很容易超过 API 配额。通过设置每分钟的时间调度器（我为每个函数创建了两个调度器），处理频率从毫秒级下降到秒级。通过这种方式，数据管道中的消息流量得到了缓解。

## e. 如何避免重复操作？

几乎不可能在不实际访问 API（真相来源）的情况下判断收集的数据是否是最新的。因此，我们不能减少 API 访问次数，而是可以像我在上一个问题中所做的那样降低超出 API 访问配额的风险。

如果数据流的目标是 DynamoDB，每次从 API 接收时都会完全更新每条记录。令人担忧的是，我们从 DynamoDB 到 S3 的火 hose 流带宽不足，导致运输偶尔中断。鉴于这一情况，我在更新之前插入了一个合理性检查。此检查将记录的每个属性值与最近从 API 提取的值进行比较。除非记录完全没有变化，否则现有记录将被覆盖。

附件是合理性检查函数：

```py
def upToDate(table_name, condition, result, prefix):
    """
    Check if a record in a given specified DynamoDB table is up-to-date, which means that it's no different from the API retrieval.

    Args:
        table_name (str): The name of the DynamoDB table to check.
        condition (boto3.dynamodb.conditions.Key): The key condition expression for querying the table.
        result (dict): The record to check for ingestion completion.
        prefix (str): The prefix used for key matching.

    Returns:
        bool: True if the ingestion is completed, False otherwise.

    Notes:
        - The function queries the specified DynamoDB table using the provided condition.
        - It retrieves the items matching the condition.
        - The function compares the key-value pairs of the result with the retrieved items, accounting for the provided prefix if applicable.
        - If all key-value pairs match between the result and the retrieved items, the ingestion is considered completed.
        - The function returns True if the ingestion is completed, and False otherwise.

    """
    table = dynamodb.Table(table_name)
    retrieval = table.query(KeyConditionExpression=condition)["Items"]

    existing_items = 0
    if len(retrieval) > 0:
        for key in retrieval.keys():
            if key.upper() not in reserved_words:
                if result[key] == retrieval[0].get(key):
                    existing_items += 1
            elif result["key"] == retrieval[0].get(prefix + key):
                existing_items += 1

    completed = len(retrieval) and existing_items == len(result.items())
    # len(retrieval) == 0: the item doesn't exist in DynamoDB at all
    # existing_items == len(result.items()): the item exists and all its key-value pairs
    # are synced up with API
    return completed
```

# 部署

在每个文件夹中，执行

```py
pip install -r requirements.txt
```

然后返回到上一级文件夹：

```py
# copy utils.py to each folder
for d in */; do cp utils.py "$d"; done
# build the cloudformation
sam build -u
# invoke the functions locally for local testing
# event.json should be like:
# {
#    "branches": ["Scranton"]
# }
# env.json should be like:
# {
#    "Parameters": {
#        "Environment": "local"
#    }
# }
sam local invoke "BranchCollector" -e branch.json --env-vars env.json
sam local invoke "SalespersonCollector" -e branch.json --env-vars env.json
sam local invoke "SalesCollector" -e branch.json --env-vars env.json
# deploy it onto AWS
sam deploy --parameter-overrides Environment=dev
```

# 最终工作

[我的 GitHub 仓库](https://github.com/MemphisMeng/Cascading-ETL-pipeline/tree/dev)

# 版权信息

+   [作为代码的图表](https://diagrams.mingrammer.com/)提供了所有支持简洁可视化的工具

+   [《办公室》（The Office](https://www.imdb.com/title/tt0386676/)是一个精彩的节目，带来的欢笑和灵感。
