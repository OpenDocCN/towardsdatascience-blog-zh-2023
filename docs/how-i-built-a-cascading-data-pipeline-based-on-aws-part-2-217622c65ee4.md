# 如何基于 AWS 构建级联数据管道（第二部分）

> 原文：[`towardsdatascience.com/how-i-built-a-cascading-data-pipeline-based-on-aws-part-2-217622c65ee4`](https://towardsdatascience.com/how-i-built-a-cascading-data-pipeline-based-on-aws-part-2-217622c65ee4)

## 自动化、可扩展和强大

[](https://anzhemeng.medium.com/?source=post_page-----217622c65ee4--------------------------------)![Memphis Meng](https://anzhemeng.medium.com/?source=post_page-----217622c65ee4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----217622c65ee4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----217622c65ee4--------------------------------) [Memphis Meng](https://anzhemeng.medium.com/?source=post_page-----217622c65ee4--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----217622c65ee4--------------------------------) ·10 分钟阅读·2023 年 8 月 25 日

--

![](img/527158d227cde2bf33b0286a233784ff.png)

照片由[Mehmet Ali Peker](https://unsplash.com/@mrpeker?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，发布在[Unsplash](https://unsplash.com/photos/hfiym43qBpk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

[之前](https://medium.com/p/997b212a84d2)，我分享了使用 AWS CloudFormation 技术开发数据管道的经验。然而，这不是一个最优的方法，因为它留下了 3 个待解决的问题：

1.  部署必须手动进行，这可能增加错误的机会；

1.  所有资源都创建在一个单一的堆栈中，没有适当的边界和层次；随着开发周期的进行，资源堆栈会变得越来越重，管理它将会是一场灾难；

1.  许多资源应当在其他项目中持续使用和重用。

简而言之，我们将以敏捷的方式增加该项目的可管理性和重用性。

# 解决方案

AWS 使用户能够实现 2 种 CloudFormation 结构模式：跨堆栈引用和嵌套堆叠。跨堆栈引用代表了一种单独且通常独立地开发云堆栈的设计风格，而所有堆栈之间的资源可以根据引用关系相互关联。[嵌套堆叠](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-nested-stacks.html)指的是由其他堆栈组成的 CloudFormation 堆栈。这是通过使用`[AWS::CloudFormation::Stack](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-stack.html)`资源实现的。

![](img/2ff043977326ddfcf62a296a186bd9f0.png)

现实生活中的嵌套堆栈：一个充满巢穴/蛋的巢（照片由 [Giorgi Iremadze](https://unsplash.com/@apollofotografie?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，来源于 [Unsplash](https://unsplash.com/photos/lbCsrVgJ0z8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)）

因为我们旨在实现更好的项目管理，所以项目将通过分层分离来拆分，而嵌套堆栈将对此提供帮助。然而，鉴于现有堆栈工件之间的内在关系，我们还需要进行跨堆栈引用。

# 实现

我们创建了 3 个 Lambda 函数，3 个 DynamoDB 表，1 个 IAM 角色及其附加策略，几个 SQS 队列和几个 Cloudwatch 警报。由于这些功能本身的复杂性，在这个版本中，它们将被定义在不同的模板中，服务仅供自己使用，包括警报和死信队列。除此之外，IAM 资源将是另一个嵌套堆栈，DynamoDB 表也是，为了保持其可重用性。用于在 Lambda 函数之间传递消息的 SQS 队列也将是一个不同的堆栈。这些嵌套堆栈将被放在一个新的目录中，称为 `/templates`。

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
├── templates
│   ├── BranchCollector.yml
│   ├── SaleCollector.yml
│   ├── SalespersonCollector.yml
│   ├── iam.yml
│   ├── queues.yml
│   └── tables.yml
├── template.yml
└── utils.py
```

与之前相比，这一部分的编码工作相对轻松，因为我们主要只需将这些原始代码从一个文件移动到另一个文件。例如，在配置 DynamoDB 表堆栈时，我所做的仅是复制代码片段并粘贴到新文件中：

```py
AWSTemplateFormatVersion: '2010-09-09'
Transform: 'AWS::Serverless-2016-10-31'
Parameters:  #   Type: String
  Environment:
    Type: String
Resources:
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
```

## 跨嵌套堆栈引用

值得注意的是，还有一些修改是必要的。如果我们需要确保堆栈内定义的资源可以被堆栈外部的其他资源引用，那么那些导出堆栈需要一个新的部分，称为 `Outputs`，以输出引用值。在我们的案例中，IAM 角色将被全球使用，因此在其定义之后，我们会输出其 ARN，以便在一定范围内可见。

```py
AWSTemplateFormatVersion: '2010-09-09'
Transform: 'AWS::Serverless-2016-10-31'
Parameters:  #   Type: String
  Environment:
    Type: String
Resources:
  LambdaRole:
    Type: AWS::IAM::Role
    Properties:
      RoleName: !Sub '${Environment}-lambda-role'
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
      PolicyName: !Sub '${AWS::StackName}-${Environment}-lambda-policy'
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

Outputs:
  Role:
    Description: The role to be used across the stacks
    Value: !GetAtt LambdaRole.Arn
    Export:
      Name: !Sub ${Environment}-Role
```

同样，消息队列也以相同的方式导出：

```py
AWSTemplateFormatVersion: '2010-09-09'
Transform: 'AWS::Serverless-2016-10-31'
Parameters:  #   Type: String
  Environment:
    Type: String
Resources:
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

Outputs:
  EmployeeQueue:
    Description: The SQS queue that delivers the payloads from branch collector to salesperson collector
    Value: !Ref EmployeeQueue
    Export:
      Name: !Sub ${Environment}-EmployeeQueue
  SaleQueue:
    Description: The SQS queue that delivers the payloads from salesperson collector to sale collector
    Value: !Ref SaleQueue
    Export:
      Name: !Sub ${Environment}-SaleQueue
```

与此同时，从其他地方导入资源的堆栈也需要做些许调整。AWS 通过提供一个内置函数 `[Fn::ImportValue](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-importvalue.html)` 来支持此功能。以“分支收集器”为例。它涉及到 IAM 角色和一个消息队列，这些都不是在同一堆栈中创建的。因此，每当出现上述资源时，我将它们替换为 `Fn::ImportValue` 函数的值。

```py
AWSTemplateFormatVersion: '2010-09-09'
Transform: 'AWS::Serverless-2016-10-31'
Parameters:  #   Type: String
  Environment:
    Type: String
Resources:
  BranchCollector:
    Type: AWS::Serverless::Function
    Properties:
      FunctionName: !Sub branch-collector-${Environment}
      Handler: lambda_function.lambda_handler
      Runtime: python3.8
      CodeUri: ./../branches/
      Description: updating branch info in our DynamoDB table
      MemorySize: 128
      Timeout: 900
      Role: 
        Fn::ImportValue:
          !Sub ${Environment}-Role
      Environment:
        Variables:
          LOGGING_LEVEL: INFO
          APP_ENV: !Ref Environment
          SQS: 
            Fn::ImportValue:
              !Sub ${Environment}-EmployeeQueue
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

  # dead letter queue
  BranchFunctionDeadLetterQueue: 
    Type: AWS::SQS::Queue
    Properties:
      QueueName: !Sub branch-function-dead-letter-queue-${Environment}
      MessageRetentionPeriod: 1209600

  # alarms
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
      Threshold: 1
      AlarmActions: 
        - arn:aws:sns:us-east-1:{id}:{alarm-name}
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
      Threshold: 750000
      AlarmActions:
        - arn:aws:sns:us-east-1:{id}:{alarm-name}
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
      Threshold: 1
      AlarmActions:
        - arn:aws:sns:us-east-1:{id}:{alarm-name}
```

## 根堆栈

现在所有嵌套堆栈都已定义并创建，应该有一个根堆栈将整个基础设施集成在一起。考虑到使用嵌套堆栈风格的基础设施是一个层次结构，根堆栈是所有嵌套堆栈所归属的父级（尽管嵌套堆栈也可以作为其他堆栈的父级）。在我们的案例中，替换定义单个资源的现有片段为对预定义的 `template.yml` 中嵌套堆栈 CloudFormation 模板的引用非常简单。

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
  IAM:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: ./templates/iam.yml
      Parameters: 
        Environment: !Ref Environment
  # =========================================================================================
  # AWS LAMBDA FUNCTIONS
  # ========================================================================================= 
  BranchCollector:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: ./templates/BranchCollector.yml
      Parameters: 
        Environment: !Ref Environment
    DependsOn: 
      - IAM
      - Queues

  SalespersonCollector:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: ./templates/SalespersonCollector.yml
      Parameters: 
        Environment: !Ref Environment
    DependsOn: 
      - IAM
      - Queues

  SaleCollector:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: ./templates/SaleCollector.yml
      Parameters: 
        Environment: !Ref Environment
    DependsOn: 
      - IAM
      - Queues

  # =========================================================================================
  # AWS DynamoDB TABLES
  # ========================================================================================= 
  Tables:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: ./templates/tables.yml
      Parameters: 
        Environment: !Ref Environment

  # =========================================================================================
  # AWS SQS QUEUES
  # ========================================================================================= 
  Queues:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: ./templates/queues.yml
      Parameters: 
        Environment: !Ref Environment
```

从这里开始，可以放心地说 CloudFormation 升级已经完成！

等等，这是否意味着我们需要重建和重新部署？遗憾的是，是的，这正是下一节将要解决的问题。

## 自动部署

由于我总是将我编写的所有代码存储在 GitHub 仓库中，我打算利用 [GitHub Actions](https://github.com/features/actions)。GitHub Actions 是 GitHub 的一个功能，用于自动化存储在任何 GitHub 仓库中的工作流，从而无缝支持构建、测试和部署。尽管有预定义的工作流，但由于我们任务的独特性和复杂性，我们需要自定义一个满足我们需求的工作流。

一般来说，工作流应该模拟我们在软件构建和部署阶段在云环境中手动执行的操作。即包括以下步骤：

1.  为参数分配值，例如 `environment`

1.  设置 AWS 配置，例如输入默认的 AWS 凭证和服务区域代码

1.  [安装 SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html)

1.  授予 AWS 角色访问将要使用的服务/资源的权限

1.  创建一个 S3 桶作为 CloudFormation 存储位置

1.  将通用代码库克隆到每个独立目录

1.  构建和部署无服务器应用程序模型（SAM）

为了在工作流文件中将它们全部包装起来，格式如下：

```py
name: A workflow that automates the data pipeline deployment
on: 
  workflow_dispatch:
  push:
    branches:
      - main
    paths-ignore: 
      - '.gitignore'
      - '*.png'
      - 'README.md'
  pull_request:
    paths-ignore:
      - '.gitignore'
      - '*.png'
      - 'README.md'

jobs:
  deploy:
    container: 
      image: lambci/lambda:build-python3.8
    runs-on: ubuntu-latest
    env:
      BUCKET_NAME: your-bucket-name
    steps:
      - name: Set Environment
        id: setenv
        run: |
          echo "Running on branch ${{ github.ref }}"
          if [ "${{ github.ref }}" = "refs/heads/main" ]; then
            echo "::set-output name=env_name::prod"
          else
             echo "::set-output name=env_name::dev"
          fi
      - name: Set Repo
        id: setrepo
        run: |
          echo "::set-output name=repo_name::${{ github.event.repository.name }}"
      - name: Set Branch
        id: setbranch
        run: |
          echo "::set-output name=branch_name::${{ github.head_ref}}"
      - name: Checkout
        uses: actions/checkout@v2
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{secrets.AWS_ACCESS_KEY_ID}}
          aws-secret-access-key: ${{secrets.AWS_SECRET_ACCESS_KEY}}
          aws-region: us-east-1
          # role-to-assume: arn:aws:iam::807324965916:role/cdk-hnb659fds-deploy-role-807324965916-us-east-1
          role-duration-seconds: 900
      - name: Install sam cli
        run: 'pip3 install aws-sam-cli'
      - name: Complete policies
        run: |
          aws iam attach-user-policy \
          --policy-arn arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess \
          --policy-arn arn:aws:iam::aws:policy/CloudWatchEventsFullAccess \
          --policy-arn arn:aws:iam::aws:policy/AWSLambda_FullAccess \
          --policy-arn arn:aws:iam::aws:policy/IAMFullAccess \
          --policy-arn arn:aws:iam::aws:policy/AWSCloudFormationFullAccess \
          --policy-arn arn:aws:iam::aws:policy/AmazonSQSFullAccess \
          --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess \
          --user-name Memphis
      - name: Create S3 Bucket
        run: |
          if ! aws s3api head-bucket --bucket "${{env.BUCKET_NAME}}" 2>/dev/null; then
            aws s3api create-bucket --bucket "${{env.BUCKET_NAME}}"
          else
            echo "Bucket ${{env.BUCKET_NAME}} already exists"
          fi

      - name: Copy utils.py
        run: 'for d in */; do cp utils.py "$d"; done'
      - name: build
        run: sam build && sam package --s3-bucket ${{env.BUCKET_NAME}} --s3-prefix "${{steps.setrepo.outputs.repo_name}}/${{steps.setbranch.outputs.branch_name}}/${{steps.setenv.outputs.env_name}}" --output-template-file packaged.yaml --region us-east-1 || { echo 'my_command failed' ; exit 1; }
      - name: deploy
        run: sam deploy --template-file packaged.yaml --s3-bucket ${{env.BUCKET_NAME}} --s3-prefix "${{steps.setrepo.outputs.repo_name}}/${{steps.setbranch.outputs.branch_name}}/${{steps.setenv.outputs.env_name}}" --stack-name "${{steps.setrepo.outputs.repo_name}}-${{steps.setenv.outputs.env_name}}-stack" --capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND --region us-east-1 --no-fail-on-empty-changeset --parameter-overrides Environment=${{steps.setenv.outputs.env_name}} || { echo 'my_command failed' ; exit 1; }
```

在这里我包括了一个名为“deploy”的任务。它通过 `lambci/lambda:build-python3.8` 镜像进行容器化，并在 Ubuntu 系统上运行，这在此任务中进行了定义。创建了一个名为 `BUCKET_NAME` 的变量，用于存储我们将要命名的 S3 桶的字符串值。

第 1 步是创建一个新的参数 `env_name`，用于存储工作环境名称的值。它取决于触发工作流的分支：除非工作流在主分支上运行，否则称为 `prod`；否则为 `dev`。

第 2 步在另一个输出值中记录仓库名称。这将成为 S3 桶中模板文件位置前缀的一部分。

第 3 步类似于第 2 步，获取将来作为前缀另一部分使用的分支名称。

第 4 步简单地使用 [actions/checkout](https://github.com/actions/checkout)，这是一个软件包，用于在不同的工作区中检出当前的仓库，以便工作流能够访问它。

第 5 步还使用了一个包。在[aws-actions/configure-aws-credentials@v1](https://github.com/aws-actions/configure-aws-credentials)的帮助下，我们可以轻松配置 AWS 工作环境，只需提供 AWS 访问密钥 ID、AWS 秘密访问密钥和区域即可。请注意，建议将你的敏感凭证（AWS 访问密钥 ID 和 AWS 秘密访问密钥）保存在秘密中。

第 6~11 步在 bash 命令行中执行。按照这个顺序，工作流的其余部分安装 SAM CLI；然后将所有必要的策略 ARN 附加到 IAM 用户；如果 S3 桶不存在，则使用作为环境参数提供的桶名称创建一个 S3 桶；最后但同样重要的是，将通过 SAM 命令构建、打包和部署 CloudFormation 堆栈。

使用工作流，开发人员将避免不断重新运行部分步骤（如果不是全部步骤的话）。相反，只要创建了拉取请求，任何新的提交都会触发一个新的工作流运行，代码在非主分支上；当拉取请求合并时，最近的工作流也会被触发，同时处理主分支上的资源。

# 离开之前

感谢你读到这里。在这篇文章中，我讨论了一个主要的升级，以提高代码的可管理性和重用性，同时介绍了一个自动化堆栈构建和部署的解决方案。如果你对细节感兴趣，随时查看[我的仓库](https://github.com/MemphisMeng/Cascading-ETL-pipeline/tree/main)!

顺便说一下，我组织资源和工件的方式也可以是启动[微服务](https://en.wikipedia.org/wiki/Microservices)的一个很好的开端。如果你对微服务感兴趣，别忘了订阅以跟踪我的下一篇文章！
