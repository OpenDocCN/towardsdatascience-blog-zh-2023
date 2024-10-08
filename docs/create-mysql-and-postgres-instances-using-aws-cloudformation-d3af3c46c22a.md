# 使用 AWS Cloudformation 创建 MySQL 和 Postgres 实例

> 原文：[`towardsdatascience.com/create-mysql-and-postgres-instances-using-aws-cloudformation-d3af3c46c22a`](https://towardsdatascience.com/create-mysql-and-postgres-instances-using-aws-cloudformation-d3af3c46c22a)

## 面向数据库从业者的基础设施即代码

[](https://mshakhomirov.medium.com/?source=post_page-----d3af3c46c22a--------------------------------)![💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----d3af3c46c22a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d3af3c46c22a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d3af3c46c22a--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----d3af3c46c22a--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d3af3c46c22a--------------------------------) ·阅读时间 7 分钟·2023 年 3 月 20 日

--

![](img/21d6c29d19d4d9e6c31365437e7443f2.png)

照片由 [Yunqing Leo](https://unsplash.com/@leoo0oo?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

使用这两个简单的**Cloudformation 模板**，我们将学习如何通过一个命令行命令部署 Postgres 和 MySQL Aurora 数据库实例。我已优化模板文件，将属性数量减少到最低限度，以便更容易理解。

我知道基础设施即代码对于不熟悉这个概念的中级用户来说可能有点困难，但相信我，这是正确的方向。从职业发展的角度来看，这将带来很多好处。

## 这个教程适合谁？

这个故事是为了那些想进入数据工程并学习一些高级内容，如**基础设施即代码**的初学者和中级数据及软件工程师。

我希望这个教程对所有需要云数据库来存储应用数据的软件工程师有所帮助。

## **前提条件**

+   **Bash** 脚本的基本知识

    Bash/Shell 脚本对于初学者来说可能有点复杂，但它在部署 AWS 服务（即 RDS）时是必需的。

+   安装了 AWS 命令行工具（AWS CLI）。

    本文中提供的所有 shell 命令可以通过命令行或作为 `.sh` 脚本运行，即从命令行运行一个文件，如 `$ ./deploy.sh`

+   AWS 账户。

    注册是免费的，并且有免费层，所以除非你部署了一个非常大的数据库并且忘记删除，否则不会有重大费用。

## 为什么选择基础设施即代码（IAC）？

IAC（基础设施即代码）因其帮助以**模板文件（JSON 或 yaml 代码）**的一致且可重复的方式管理云平台资源而迅速获得流行。我们可以应用 CI/CD 管道，并利用 Github 集成来创建生产和预发布环境。例如。基础设施即代码现在是部署和资源管理的标准，我之前在这里写过相关内容：

[](https://levelup.gitconnected.com/infrastructure-as-code-for-beginners-a4e36c805316?source=post_page-----d3af3c46c22a--------------------------------) [## 初学者的基础设施即代码

### 使用这些模板像专业人士一样部署数据管道

levelup.gitconnected.com](https://levelup.gitconnected.com/infrastructure-as-code-for-beginners-a4e36c805316?source=post_page-----d3af3c46c22a--------------------------------)

那个故事解释了如何为我们的数据湖（即 AWS S3 和 AWS Lambda）部署和配置 AWS 资源。

从数据角度来看，基础设施即代码是一种出色的方式来分离数据环境并提供数据管道资源。利用它我们可以创建任何需要的数据管道。

## 我们在数据管道中为何以及何时使用关系型数据库？

通常，关系型数据库由行式表组成，表中的列使用规范化的模式连接相关的数据片段。

这种**数据架构类型旨在**快速捕获数据，并优化系统以快速检索应用程序所需的最新数据。

由于 RDS 数据库表和连接是“规范化的”，它们比典型的数据仓库设计更复杂。然而，这是功能需求所要求的权衡。

因此，“传统数据库”和“数据仓库”之间的主要区别在于前者是为了“记录”数据而创建和优化的，而后者则是为了“响应分析”而创建和构建的。我之前在这里写过相关内容：

[](/data-platform-architecture-types-f255ac6e0b7?source=post_page-----d3af3c46c22a--------------------------------) ## 数据平台架构类型

### 它如何满足你的业务需求？选择的困境。

towardsdatascience.com

流行的关系型数据库有 MySQL、Microsoft SQL Server、Oracle 和 PostgreSQL。

# 创建 MySQL Aurora 实例

要创建一个 Aurora 引擎的数据库实例，我们首先需要一个集群。问题在于 Aurora 实例必须通过**AWS::RDS::DBCluster**与**DBClusterIdentifier**相关联。如果在我们的栈文件中没有集群，我们将会遇到一些没有多大意义的通用 Cloudformation 错误。

你可以使用这个 Cloudformation 模板文件：

如果一切正确，那么在 AWS 控制台中你将看到如下内容：

![](img/5885f152c3638e461fed34b03b85ab66.png)

Cloudformation 事件。作者提供的图片

# 在 AWS 中创建 Postgres 数据库实例

要创建一个简单的 Postgres 实例，我们可以使用这个命令和下面的模板：

模板将是这样的：

> 如你所见，Postgres 要容易一些，因为它不需要集群。

![](img/4ab43d6bfdec947abd4883e8b4004cc9.png)

Postgres 实例已创建。图片由作者提供。

## 如果出现问题怎么办？

我们可以随时查看**events**部分。然后 Google 搜索。我知道，我知道…… AWS Cloudformation 文档并不完美。这就是生活，我们必须遵守。不幸的是。

例如，如果出现问题并且遇到像这样的错误：

> “DS 不支持使用以下组合创建 DB 实例：DBInstanceClass=db.t2.micro, Engine=postgres, EngineVersion=13.3…”

你需要找到你所在区域支持的引擎和版本 13.3。为此，你可以使用这个 bash 命令：

```py
aws rds describe-orderable-db-instance-options --engine postgres --engine-version 13.3     --query "*[].{DBInstanceClass:DBInstanceClass,StorageType:StorageType}|[?StorageType=='gp2']|[].{DBInstanceClass:DBInstanceClass}"  --output texts
```

# 部署 RDS 实例的最佳实践

我们上面使用的模板非常简单。这是故意的，以便我们可以轻松地成功部署它们，并从那里开始添加新功能。

> 我们还能做什么？

## 将密码保存在 Secrets Manager 中

这是一个非常好的主意，因此我们可能需要将模板中的密码属性更改为这个：

```py
 RootUser:
    Type: 'AWS::SecretsManager::Secret'
    Properties:
      Name: '/rds/EnvType/RootUser-secret'
      Description: 'Aurora database master user'
      SecretString: !Ref DBUser
```

然后我们可以在模板中这样使用它：

```py
MasterUsername: !Join ['', ['{{resolve:secretsmanager:', !Ref RootUser, '}}']]
```

我们可以对数据库密码做同样的事情：

```py
RootPassword:
    Type: 'AWS::SecretsManager::Secret'
    Properties:
      Name: '/rds/EnvType/RootPassword-secret'
      Description: 'Aurora database master password'
      GenerateSecretString:
        PasswordLength: 16
        ExcludePunctuation: true
```

然后从模板中引用它：

```py
RootUserPassword: !Join ['', ['{{resolve:secretsmanager:', !Ref RootPassword, '}}']]
```

## 什么是 RDSDBClusterParameterGroup？

它有一个类型：‘AWS::RDS::DBClusterParameterGroup’

在这个属性中，我们指定将连接到数据库集群的参数组的名称。通过这样做，我们将我们的数据库实例与 Aurora 数据库集群关联起来。

然后我们在**AWS::RDS::DBCluster**资源中引用它，即：

```py
Resources:
  RDSCluster: 
    Properties: 
      DBClusterParameterGroupName: 
        Ref: RDSDBClusterParameterGroup
```

这很重要，因为每次将来需要更新参数时，必须构建一个新的参数组并将其链接到集群。

要实施修改，必须重启集群中的主数据库实例。

## 删除保护

总是一个好主意为我们的数据库集群设置 DeletionProtection: true

这可以防止 RDS 实例的意外删除。它还可以防止我们更改某些模板文件属性后需要替换资源的情况。

## DBInstanceClass

这是一个重要的属性，因为它直接影响到我们数据库相关的费用。它在模板中属于类型：‘AWS::RDS::DBInstance’。

始终选择最小的实例进行测试。在模板中搜索`db.r6g.large`并更改为`db.t2.small`或`db.t3.micro`

```py
DBInstanceClass: db.t2.small
```

另外，如果我们在 `staging` 中不需要大存储，最好只配置所需的最小值：

```py
AllocatedStorage: '20'
DBInstanceClass: db.t2.small 
```

## 在单独的堆栈模板中配置 VPC 资源

我们可能希望在虚拟私有云 (VPC) 中部署我们的数据库集群。创建一个单独的堆栈来处理这些类型的资源，然后在我们的数据库堆栈中使用它是一个好主意，如下所示：

```py
Resources:
  RDSCluster: 
    Properties: 
      DBClusterParameterGroupName: 
        Ref: RDSDBClusterParameterGroup
      # DBSubnetGroupName: 
      #   Ref: DBSubnetGroup
      Engine: aurora
      MasterUserPassword: 
        Ref: DBPassword
      MasterUsername: 
        Ref: DBUser
    Type: "AWS::RDS::DBCluster"
```

## DBInstanceIdentifier

数据库实例由这个**名称**标识。

如果你没有提供名称，Amazon CloudFormation 将创建一个特殊的默认物理 ID 并将其用作名称。

## **AWS::RDS::DBParameterGroup**

我们应该出于与创建 `'AWS::RDS::DBClusterParameterGroup'` 相同的原因来创建这种资源类型。

> 对于生产数据库来说，稍后将新的数据库参数组关联起来可能会存在问题，因为这样做需要重新启动数据库。

我们也可能需要为写入和副本实例创建不同的 `AWS::RDS::DBParameterGroup`，即：

```py
 WriterRDSDBParameterGroup:
    Type: 'AWS::RDS::DBParameterGroup'
    Properties:
      Description: Custom db parameter group for writer
      Family: aurora5.6
      Parameters:
        sql_mode: STRICT_ALL_TABLES
        transaction_isolation: READ-COMMITTED
  RDSDBParameterGroup:
    Type: 'AWS::RDS::DBParameterGroup'
    Properties:
      Description: CloudFormation Sample Aurora Parameter Group
      Family: aurora5.6
      Parameters:
        sql_mode: IGNORE_SPACE
        max_allowed_packet: 1024
        innodb_buffer_pool_size: '{DBInstanceClassMemory*3/4}'
```

## 数据库监控

强烈建议在初始设置期间设置数据库监控。许多[指标](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.AuroraMySQL.Monitoring.Metrics.html)，无论是在集群还是数据库层级，默认情况下都会由**Aurora 发送到 CloudWatch**。因此，我们可以通过使用 [AWS::RDS::EventSubscription](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-rds-eventsubscription.html) 资源来订阅 RDS 事件。

```py
ClusterEventSubscription:
    Type: 'AWS::RDS::EventSubscription'
    Properties:
      EventCategories:
        - configuration change
        - creation
        - failure
        - deletion
        - failover
        - maintenance
        - notification
      SnsTopicArn: arn:aws:sns:us-east-1:09876543:example-topic 
      SourceIds:
        - !Ref myAuroraDBCluster
      SourceType: db-cluster
      Enabled: true
```

之后，我们可以创建 CloudWatch 指标过滤器，并像这样设置 CloudWatch 警报：

```py
DBCPUUtilizationHighAlarm:
    Type: 'AWS::CloudWatch::Alarm'
    Properties:
      AlarmDescription: 'CPU utilization over last 5 minutes higher than 95%'
      Namespace: 'AWS/RDS'
      MetricName: CPUUtilization
      Statistic: Average  
      Period: 60
      EvaluationPeriods: 5
      ComparisonOperator: GreaterThanThreshold
      Threshold: 95
      TreatMissingData: breaching
      AlarmActions: 
       - arn:aws:sns:us-east-1:9876543:example-topic
      OKActions:
       - arn:aws:sns:us-east-1:9876543:example-topic
      Dimensions:
      - Name: DBInstanceIdentifier
        Value: !Ref myDatabaseInstance
```

## EnablePerformanceInsights

这是一个有助于了解数据库性能的工具。

> 如果仅启用保留 7 天的数据，则不会产生额外费用。

```py
EnablePerformanceInsights: true
```

这个功能必须在主实例和副本实例上分别启用。

## 结论

在这个故事中，你可以找到两个简化的 CloudFormation 模板，用于在云中部署 Postgres 和 MySQL 数据库实例。我希望你会发现本文中涵盖的最佳实践有用，并且它将帮助你将这些模板发展成符合你需求的更大内容。

RDS 数据库对于任何数据平台设计至关重要，理解它们是如何创建的也很重要。它们在许多数据管道设计模式中被使用，即批处理和变更数据捕获：

[](/data-pipeline-design-patterns-100afa4b93e3?source=post_page-----d3af3c46c22a--------------------------------) ## 数据管道设计模式

### 选择合适的架构及其示例

towardsdatascience.com

应用一些在最佳实践中提到的功能也可能有助于获得数据库性能洞察。

## 推荐阅读：

[](/data-pipeline-design-patterns-100afa4b93e3?source=post_page-----d3af3c46c22a--------------------------------) ## 数据管道设计模式

### 选择合适的架构及其示例

towardsdatascience.com [## AWS::SecretsManager::Secret

### 创建一个新的密钥。密钥可以是一个密码、一组凭证（如用户名和密码）、OAuth…

[工作与参数组](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-secretsmanager-secret.html?source=post_page-----d3af3c46c22a--------------------------------)

### 数据库参数指定了数据库的配置方式。例如，数据库参数可以指定…

[AWS::RDS::EventSubscription](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/USER_WorkingWithParamGroups.html?source=post_page-----d3af3c46c22a--------------------------------)

### AWS::RDS::EventSubscription 资源允许您接收 Amazon 关系数据库服务的通知…

[Amazon RDS 性能洞察 | 云关系数据库 | 亚马逊网络服务](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-rds-eventsubscription.html?source=post_page-----d3af3c46c22a--------------------------------) [](https://aws.amazon.com/rds/performance-insights/?source=post_page-----d3af3c46c22a--------------------------------)

### 分析和调整 Amazon RDS 数据库性能。Amazon RDS 性能洞察是数据库性能调优和…

[AWS 性能洞察](https://aws.amazon.com/rds/performance-insights/?source=post_page-----d3af3c46c22a--------------------------------)
