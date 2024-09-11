# 如何构建一个完全自动化的数据漂移检测管道

> 原文：[https://towardsdatascience.com/how-to-build-a-fully-automated-data-drift-detection-pipeline-e9278584e58d?source=collection_archive---------2-----------------------#2023-08-01](https://towardsdatascience.com/how-to-build-a-fully-automated-data-drift-detection-pipeline-e9278584e58d?source=collection_archive---------2-----------------------#2023-08-01)

## 自动化指南：检测和处理数据漂移

[](https://khuyentran1476.medium.com/?source=post_page-----e9278584e58d--------------------------------)[![Khuyen Tran](../Images/98aa66025ad29b618e875c75f1c400a5.png)](https://khuyentran1476.medium.com/?source=post_page-----e9278584e58d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e9278584e58d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e9278584e58d--------------------------------) [Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----e9278584e58d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F84a02493194a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-a-fully-automated-data-drift-detection-pipeline-e9278584e58d&user=Khuyen+Tran&userId=84a02493194a&source=post_page-84a02493194a----e9278584e58d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e9278584e58d--------------------------------) · 10分钟阅读 · 2023年8月1日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe9278584e58d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-a-fully-automated-data-drift-detection-pipeline-e9278584e58d&user=Khuyen+Tran&userId=84a02493194a&source=-----e9278584e58d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe9278584e58d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-a-fully-automated-data-drift-detection-pipeline-e9278584e58d&source=-----e9278584e58d---------------------bookmark_footer-----------)![](../Images/c433a55c93060b21965daf484c047144.png)

作者提供的图片

# 动机

数据漂移发生在生产环境中的输入特征分布与训练数据不同，从而可能导致不准确性和模型性能下降。

![](../Images/1a58af829c62bf069cfd2e99d4203f0d.png)

作者提供的图片

为了减轻数据漂移对模型性能的影响，我们可以设计一个工作流，检测漂移，通知数据团队，并触发模型重训练。

![](../Images/677b5b6931730a825eff3e9288beab99.png)

作者提供的图片

# 工作流

工作流包括以下任务：

1.  从 Postgres 数据库中获取参考数据。

1.  从网络获取当前生产数据。

1.  通过比较参考数据和当前数据来检测数据漂移。

1.  将当前数据附加到现有的 Postgres 数据库中。

1.  当出现数据漂移时，采取以下措施：

+   发送 Slack 消息以提醒数据团队。

+   重新训练模型以更新其性能。

+   将更新后的模型推送到 S3 存储。

这个工作流程被安排在特定时间运行，例如每周一上午 11:00。

![](../Images/b5b8d48a2b308df0226e2b5963b320e1.png)

图片由作者提供

总体而言，工作流程包括两种类型的任务：数据科学任务和数据工程任务。

数据科学任务，用粉色框表示，由数据科学家执行，涉及数据漂移检测、数据处理和模型训练。

数据工程任务，用蓝色和紫色框表示，由数据工程师执行，涉及数据移动和发送通知的任务。

# 数据科学任务

## 检测数据漂移

为了检测数据漂移，我们将创建一个 Python 脚本，该脚本接受两个 CSV 文件“data/reference.csv”（参考数据）和“data/current.csv”（当前数据）。

![](../Images/12ed1960bcc3d5295dacaa4c1ba12e6c.png)

图片由作者提供

我们将使用[Evidently](https://www.evidentlyai.com/)，一个开源的机器学习可观测性平台，来将参考数据（作为基线）与当前生产数据进行比较。

如果检测到数据漂移，则“drift_detected”输出为 True；否则为 False。

```py
from evidently.metric_preset import DataDriftPreset
from evidently.report import Report
from kestra import Kestra

data_drift_report = Report(metrics=[DataDriftPreset()])
data_drift_report.run(reference_data=reference, current_data=current)
report = data_drift_report.as_dict()
drift_detected = report["metrics"][0]["result"]["dataset_drift"]

if drift_detected:
    print("Detect dataset drift")
else:
    print("Detect no dataset drift")

Kestra.outputs({"drift_detected": drift_detected})
```

[完整代码。](https://github.com/khuyentran1401/detect-data-drift-pipeline/blob/main/src/detect/detect_data_drift.py)

## 重新训练模型

接下来，我们将创建一个负责模型训练的 Python 脚本。该脚本以过去和当前数据的结合作为输入，并将训练好的模型保存为“model.pkl”文件。

![](../Images/d7a01c96325ffca329b924fe15b782c4.png)

图片由作者提供

```py
def train_model(X_train: pd.DataFrame, y_train: pd.Series, model_params: DictConfig):
    y_train_log = np.log1p(y_train)
    model = Ridge()
    scorer = metrics.make_scorer(rmsle, greater_is_better=True)
    params = dict(model_params)

    grid = GridSearchCV(model, params, scoring=scorer, cv=3, verbose=3)
    grid.fit(X_train, y_train_log)
    return grid

model = train_model(X_train, y_train, config.model.params)
joblib.dump(model, "model.pkl")
```

[完整代码。](https://github.com/khuyentran1401/detect-data-drift-pipeline/blob/main/src/train/train_model.py)

## 推送到 GitHub

在开发完这两个脚本后，数据科学家可以将它们推送到 GitHub，从而允许数据工程师在创建工作流时使用它们。

在这里查看这些文件的 GitHub 仓库：

[](https://github.com/khuyentran1401/detect-data-drift-pipeline?source=post_page-----e9278584e58d--------------------------------) [## GitHub - khuyentran1401/detect-data-drift-pipeline: 一个检测数据漂移并重新训练模型的流水线]

### 一个检测数据漂移并在出现漂移时重新训练模型的流水线 - GitHub …

github.com](https://github.com/khuyentran1401/detect-data-drift-pipeline?source=post_page-----e9278584e58d--------------------------------)

# 数据工程任务

流行的协调库如 Airflow、Prefect 和 Dagster 需要修改 Python 代码以使用其功能。

当Python脚本与数据工作流紧密集成时，整体代码库可能会变得更复杂，维护也更困难。如果没有独立的Python脚本开发，数据工程师可能需要修改数据科学代码以添加编排逻辑。

![](../Images/64c035d84a1ea8d5f62045eacc58bb41.png)

图片来源：作者

另一方面，[Kestra](https://kestra.io/) 是一个开源库，它允许你独立开发Python脚本，然后通过YAML文件无缝地将它们融入数据工作流。

这样，数据科学家可以专注于模型处理和训练，而数据工程师则可以专注于处理编排。

因此，我们将使用Kestra设计一个更模块化和高效的工作流。

![](../Images/7723cb5f1e607561113267c99407f63a.png)

图片来源：作者

克隆 [detect-data-drift-pipeline 仓库](https://github.com/khuyentran1401/detect-data-drift-pipeline) 以获取Kestra的docker-compose文件，然后运行：

```py
docker compose up -d
```

访问 [localhost:8080](http://localhost:8080/) 以访问和探索Kestra UI。

![](../Images/1a838ca283c40d4d82b9376507f305fd.png)

图片来源：作者

按照这些 [说明](https://github.com/khuyentran1401/detect-data-drift-pipeline) 配置本教程所需的环境。

在开发目标流程之前，让我们通过创建一些简单的流程来熟悉Kestra。

## 从Python脚本访问Postgres表

我们将创建一个包含以下任务的流程：

+   `getReferenceTable`：从Postgres表中导出CSV文件。

+   `saveReferenceToCSV`：创建一个本地CSV文件，可以被Python任务访问。

+   `runPythonScript`：使用Python读取本地CSV文件。

为了在`saveReferenceToCSV`和`runPythonScript`任务之间传递数据，我们将通过将这两个任务放置在相同的工作目录中，并将它们封装在`wdir`任务内部来实现。

```py
id: get-reference-table
namespace: dev
tasks:
  - id: getReferenceTable
    type: io.kestra.plugin.jdbc.postgresql.CopyOut
    url: jdbc:postgresql://host.docker.internal:5432/
    username: "{{secret('POSTGRES_USERNAME')}}"
    password: "{{secret('POSTGRES_PASSWORD')}}"
    format: CSV
    sql: SELECT * FROM reference
    header: true
  - id: wdir
    type: io.kestra.core.tasks.flows.WorkingDirectory
    tasks:
    - id: saveReferenceToCSV
      type: io.kestra.core.tasks.storages.LocalFiles
      inputs:
        data/reference.csv: "{{outputs.getReferenceTable.uri}}"
    - id: runPythonScript
      type: io.kestra.plugin.scripts.python.Script
      beforeCommands:
        - pip install pandas
      script: | 
        import pandas as pd
        df = pd.read_csv("data/reference.csv")
        print(df.head(10))
```

![](../Images/44e62914c8c5b0c999d474d9d9666370.png)

图片来源：作者

执行流程将显示以下日志：

![](../Images/0229ba03295229c369b01a8935459d49.png)

图片来源：作者

## 使用输入参数化流程

让我们创建另一个可以用输入参数化的流程。这个流程将包含以下输入：`startDate`、`endDate` 和 `dataURL`。

`getCurrentCSV`任务可以使用`{{inputs.name}}`符号访问这些输入。

```py
id: get-current-table
namespace: dev
inputs:
  - name: startDate
    type: STRING
    defaults: "2011-03-01"
  - name: endDate
    type: STRING
    defaults: "2011-03-31"
  - name: dataURL
    type: STRING 
    defaults: "https://raw.githubusercontent.com/khuyentran1401/detect-data-drift-pipeline/main/data/bikeride.csv"
tasks:
  - id: getCurrentCSV
    type: io.kestra.plugin.scripts.python.Script
    beforeCommands:
      - pip install pandas
    script: |
      import pandas as pd
      df = pd.read_csv("{{inputs.dataURL}}", parse_dates=["dteday"])
      print(f"Getting data from {{inputs.startDate}} to {{inputs.endDate}}")
      df = df.loc[df.dteday.between("{{inputs.startDate}}", "{{inputs.endDate}}")]
      df.to_csv("current.csv", index=False)
```

![](../Images/87d1ffd8c99d5157d0a0150b8d72fe34.png)

图片来源：作者

这些输入的值可以在每次执行流程时指定。

![](../Images/d1b49a9a4e18af13b7733b23d54cbc33.png)

图片来源：作者

## 将CSV文件加载到Postgres表中

以下流程执行以下任务：

+   `getCurrentCSV`：运行Python脚本以在工作目录中创建CSV文件。

+   `saveFiles`：将工作目录中的CSV文件发送到Kestra的内部存储。

+   `saveToCurrentTable`：将CSV文件加载到Postgres表中。

```py
iid: save-current-table
namespace: dev
tasks:
  - id: wdir
    type: io.kestra.core.tasks.flows.WorkingDirectory
    tasks:
    - id: getCurrentCSV
      type: io.kestra.plugin.scripts.python.Script
      beforeCommands:
        - pip install pandas
      script: |
        import pandas as pd
        data_url = "https://raw.githubusercontent.com/khuyentran1401/detect-data-drift-pipeline/main/data/bikeride.csv"
        df = pd.read_csv(data_url, parse_dates=["dteday"])
        df.to_csv("current.csv", index=False)
    - id: saveFiles
      type: io.kestra.core.tasks.storages.LocalFiles
      outputs:
        - current.csv
  - id: saveToCurrentTable
    type: io.kestra.plugin.jdbc.postgresql.CopyIn
    url: jdbc:postgresql://host.docker.internal:5432/
    username: "{{secret('POSTGRES_USERNAME')}}"
    password: "{{secret('POSTGRES_PASSWORD')}}"
    from: "{{outputs.saveFiles.uris['current.csv']}}"
    table: current
    format: CSV
    header: true
    delimiter: ","
```

![](../Images/1de966b034e4885dacb6ac0314a3e300.png)

图片来源：作者

运行此流程后，你将在 Postgres 数据库中的“current”表中看到结果数据。

![](../Images/fbc70170398295680a9ac5bbb5719cd4.png)

图片由作者提供

## 从 GitHub 仓库中运行文件

此流程包括以下任务：

+   `cloneRepository`：克隆一个公共 GitHub 仓库

+   `runPythonCommand`：从 CLI 执行一个 Python 脚本

这两个任务将在同一工作目录中操作。

```py
id: clone-repository
namespace: dev
tasks:
  - id: wdir
    type: io.kestra.core.tasks.flows.WorkingDirectory
    tasks:
      - id: cloneRepository
        type: io.kestra.plugin.git.Clone
        url: https://github.com/khuyentran1401/detect-data-drift-pipeline
        branch: main
      - id: runPythonCommand
        type: io.kestra.plugin.scripts.python.Commands
        commands:
          - python src/example.py
```

![](../Images/6b2586cf15d9c1f6c82c384c0c489220.png)

图片由作者提供

运行流程后，你将看到以下日志：

![](../Images/0c4413d165a8d794188b80e3da871b71.png)

图片由作者提供

## 按计划运行一个流程

我们将创建另一个基于特定时间表运行的流程。以下流程在每周一上午 11:00 运行。

```py
id: triggered-flow
namespace: dev
tasks:
  - id: hello
    type: io.kestra.core.tasks.log.Log
    message: Hello world
triggers:
  - id: schedule
    type: io.kestra.core.models.triggers.types.Schedule
    cron: "0 11 * * MON"
```

## 上传到 S3

此流程包括以下任务：

+   `createPickle`：在 Python 中生成一个 pickle 文件

+   `savetoPickle`：将 pickle 文件转移到 Kestra 的内部存储

+   `upload`：将 pickle 文件上传到 S3 桶

```py
id: upload-to-S3
namespace: dev
tasks:
  - id: wdir
    type: io.kestra.core.tasks.flows.WorkingDirectory
    tasks:
    - id: createPickle
      type: io.kestra.plugin.scripts.python.Script
      script: |
        import pickle
        data = [1, 2, 3]
        with open('data.pkl', 'wb') as f:
          pickle.dump(data, f)
    - id: saveToPickle
      type: io.kestra.core.tasks.storages.LocalFiles
      outputs:
        - data.pkl  
  - id: upload
    type: io.kestra.plugin.aws.s3.Upload
    accessKeyId: "{{secret('AWS_ACCESS_KEY_ID')}}"
    secretKeyId: "{{secret('AWS_SECRET_ACCESS_KEY_ID')}}"
    region: us-east-2
    from: '{{outputs.saveToPickle.uris["data.pkl"]}}'
    bucket: bike-sharing
    key: data.pkl
```

![](../Images/fd9fb0b08e7002ba19882ac87c1d13f4.png)

图片由作者提供

运行此流程后，`data.pkl` 文件将上传到“bike-sharing”桶中。

![](../Images/fb126a6249d2ca4bcaa71dd87ca96432.png)

图片由作者提供

# 将一切整合在一起

## 构建一个流程来检测数据漂移

现在，让我们结合所学创建一个检测数据漂移的流程。每周一上午 11:00，这个流程执行以下任务：

+   从 Postgres 数据库中获取参考数据。

+   运行一个 Python 脚本以从网络获取当前生产数据。

+   克隆包含漂移检测代码的 GitHub 仓库

+   运行一个 Python 脚本，通过比较参考数据和当前数据来检测数据漂移。

+   将当前数据附加到现有的 Postgres 数据库中。

![](../Images/47a206ff95c59d1fcf51066f79f58875.png)

图片由作者提供

```py
id: detect-data-drift
namespace: dev
inputs:
  - name: startDate
    type: STRING
    defaults: "2011-03-01"
  - name: endDate
    type: STRING
    defaults: "2011-03-31"
  - name: data_url
    type: STRING 
    defaults: "https://raw.githubusercontent.com/khuyentran1401/detect-data-drift-pipeline/main/data/bikeride.csv"
tasks:
  - id: getReferenceTable
    type: io.kestra.plugin.jdbc.postgresql.CopyOut
    url: jdbc:postgresql://host.docker.internal:5432/
    username: "{{secret('POSTGRES_USERNAME')}}"
    password: "{{secret('POSTGRES_PASSWORD')}}"
    format: CSV
    sql: SELECT * FROM reference
    header: true
  - id: wdir
    type: io.kestra.core.tasks.flows.WorkingDirectory
    tasks:
      - id: cloneRepository
        type: io.kestra.plugin.git.Clone
        url: https://github.com/khuyentran1401/detect-data-drift-pipeline
        branch: main
      - id: saveReferenceToCSV
        type: io.kestra.core.tasks.storages.LocalFiles
        inputs:
          data/reference.csv: "{{outputs.getReferenceTable.uri}}"
      - id: getCurrentCSV
        type: io.kestra.plugin.scripts.python.Script
        beforeCommands:
          - pip install pandas
        script: |
          import pandas as pd
          df = pd.read_csv("{{inputs.data_url}}", parse_dates=["dteday"])
          print(f"Getting data from {{inputs.startDate}} to {{inputs.endDate}}")
          df = df.loc[df.dteday.between("{{inputs.startDate}}", "{{inputs.endDate}}")]
          df.to_csv("data/current.csv", index=False)
      - id: detectDataDrift
        type: io.kestra.plugin.scripts.python.Commands
        beforeCommands:
          - pip install -r src/detect/requirements.txt
        commands:
          - python src/detect/detect_data_drift.py
      - id: saveFileInStorage
        type: io.kestra.core.tasks.storages.LocalFiles
        outputs:
          - data/current.csv
  - id: saveToCurrentTable
    type: io.kestra.plugin.jdbc.postgresql.CopyIn
    url: jdbc:postgresql://host.docker.internal:5432/
    username: "{{secret('POSTGRES_USERNAME')}}"
    password: "{{secret('POSTGRES_PASSWORD')}}"
    from: "{{outputs.saveFileInStorage.uris['data/current.csv']}}"
    table: current
    format: CSV
    header: true
    delimiter: ","
triggers:
  - id: schedule
    type: io.kestra.core.models.triggers.types.Schedule
    cron: "0 11 * * MON"
```

## 构建一个流程来发送 Slack 消息

接下来，我们将创建一个流程，通过一个 [Slack Webhook URL](https://api.slack.com/messaging/webhooks) 在 `detect-data-drift` 流程中的 `detectDataDrift` 任务返回 `drift_detected=true` 时发送 Slack 消息。

![](../Images/def45e005820f105f401b936d94c72c7.png)

图片由作者提供

```py
id: send-slack-message
namespace: dev
tasks:
  - id: send
    type: io.kestra.plugin.notifications.slack.SlackExecution
    url: "{{secret('SLACK_WEBHOOK')}}"
    customMessage: Detect data drift

triggers:
  - id: listen
    type: io.kestra.core.models.triggers.types.Flow
    conditions:
    - type: io.kestra.core.models.conditions.types.ExecutionFlowCondition
      namespace: dev
      flowId: detect-data-drift
    - type: io.kestra.core.models.conditions.types.VariableCondition
      expression: "{{outputs.detectDataDrift.vars.drift_detected}} == true"
```

运行 `detect-data-drift` 流程后，`send-slack-message` 流程将发送一条 Slack 消息。

![](../Images/354ca0f5c2948f71f2d0d3d6e697e04d.png)

图片由作者提供

## 构建一个流程以重新训练模型

最后，我们将创建一个流程来重新训练模型。这个流程执行以下任务：

+   从 Postgres 数据库的当前表导出一个 CSV 文件

+   克隆包含模型训练代码的 GitHub 仓库

+   运行一个 Python 脚本来训练模型并生成一个 pickle 文件

+   上传 pickle 文件到 S3

![](../Images/253b848b5ef7fa84acbb3d646ff324ac.png)

图片由作者提供

```py
id: train-model
namespace: dev
tasks:
  - id: getCurrentTable
    type: io.kestra.plugin.jdbc.postgresql.CopyOut
    url: jdbc:postgresql://host.docker.internal:5432/
    username: "{{secret('POSTGRES_USERNAME')}}"
    password: "{{secret('POSTGRES_PASSWORD')}}"
    format: CSV
    sql: SELECT * FROM current
    header: true
  - id: wdir
    type: io.kestra.core.tasks.flows.WorkingDirectory
    tasks:
      - id: cloneRepository
        type: io.kestra.plugin.git.Clone
        url: https://github.com/khuyentran1401/detect-data-drift-pipeline
        branch: main
      - id: saveCurrentToCSV
        type: io.kestra.core.tasks.storages.LocalFiles
        inputs:
          data/current.csv: "{{outputs.getCurrentTable.uri}}"
      - id: trainModel
        type: io.kestra.plugin.scripts.python.Commands
        beforeCommands:
          - pip install -r src/train/requirements.txt
        commands:
          - python src/train/train_model.py
      - id: saveToPickle
        type: io.kestra.core.tasks.storages.LocalFiles
        outputs:
          - model.pkl
  - id: upload
    type: io.kestra.plugin.aws.s3.Upload
    accessKeyId: "{{secret('AWS_ACCESS_KEY_ID')}}"
    secretKeyId: "{{secret('AWS_SECRET_ACCESS_KEY_ID')}}"
    region: us-east-2
    from: '{{outputs.saveToPickle.uris["model.pkl"]}}'
    bucket: bike-sharing
    key: model.pkl
triggers:
  - id: listenFlow
    type: io.kestra.core.models.triggers.types.Flow
    conditions:
      - type: io.kestra.core.models.conditions.types.ExecutionFlowCondition
        namespace: dev
        flowId: detect-data-drift
      - type: io.kestra.core.models.conditions.types.VariableCondition
        expression: "{{outputs.detectDataDrift.vars.drift_detected}} == true"After running this flow, the model.pkl file will be uploaded to the "bike-sharing" bucket.
```

运行此流程后，`model.pkl` 文件将上传到“bike-sharing”桶中。

![](../Images/032f439354c4a375bd0af16db6b4a192.png)

图片由作者提供

# 扩展此工作流程的想法

与其依赖计划的数据提取来识别数据漂移，我们可以利用[Grafana的外发Webhook](https://grafana.com/docs/oncall/latest/outgoing-webhooks/)和[Kestra的入站Webhook](https://kestra.io/plugins/core/triggers/io.kestra.core.models.triggers.types.webhook)来建立实时数据监控，并在数据漂移发生时立即触发流程。这种方法能够在数据漂移发生时立即检测，避免了等待计划脚本运行的需要。

![](../Images/4f595292d9b246c072b5f1a5e60c0e39.png)

作者提供的图片

请在评论中告诉我你认为这个工作流程可以如何扩展，以及你希望在未来的内容中看到其他什么用例。

我喜欢撰写关于数据科学概念的文章，并玩弄不同的数据科学工具。你可以通过以下方式保持更新我的最新帖子：

+   订阅我在[数据科学简化](https://mathdatasimplified.com/)上的新闻通讯。

+   在[LinkedIn](https://www.linkedin.com/in/khuyen-tran-1401/)和[Twitter](https://twitter.com/KhuyenTran16)上与我联系。
