# 使用 Azure ML 实现实用的 MLOps

> 原文：[`towardsdatascience.com/practical-mlops-using-azure-ml-c6a3cb201d2b?source=collection_archive---------6-----------------------#2023-02-20`](https://towardsdatascience.com/practical-mlops-using-azure-ml-c6a3cb201d2b?source=collection_archive---------6-----------------------#2023-02-20)

![](img/d17a49bdf391f5a3923daaeab5cddd78.png)

图片由 [Luca Bravo](https://unsplash.com/@lucabravo?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 使用 Azure ML CLI(v2) 和 github actions 自动化 ML 流程

[](https://shiftcoder-95.medium.com/?source=post_page-----c6a3cb201d2b--------------------------------)![Anupam Misra](https://shiftcoder-95.medium.com/?source=post_page-----c6a3cb201d2b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c6a3cb201d2b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c6a3cb201d2b--------------------------------) [Anupam Misra](https://shiftcoder-95.medium.com/?source=post_page-----c6a3cb201d2b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2ebdcaf49014&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpractical-mlops-using-azure-ml-c6a3cb201d2b&user=Anupam+Misra&userId=2ebdcaf49014&source=post_page-2ebdcaf49014----c6a3cb201d2b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c6a3cb201d2b--------------------------------) · 10 分钟阅读 · 2023 年 2 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc6a3cb201d2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpractical-mlops-using-azure-ml-c6a3cb201d2b&user=Anupam+Misra&userId=2ebdcaf49014&source=-----c6a3cb201d2b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc6a3cb201d2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpractical-mlops-using-azure-ml-c6a3cb201d2b&source=-----c6a3cb201d2b---------------------bookmark_footer-----------)

# 介绍

机器学习模型像我们日常使用的软件产品一样，影响着我们与世界的互动。正如 DevOps 对于无缝的 CI/CD 是必要的，MLOps 也变得至关重要，以不断构建最新的模型并利用其预测结果。

在本文中，我们将使用 Azure ML CLI(v2) 和 Github Actions 构建端到端的 MLOps。本文希望能成为你下一个 MLOps 项目的起点！

## 本文将帮助你模拟以下场景：

> 数据经常发生漂移并通过 API 提供。因此需要在规定的频率下重新训练模型并重新部署到在线端点。

通过以下步骤实现每周的定时任务：

1.  使用 API 下载数据并将其注册为 Azure 数据集。

1.  计算管理及触发 Azure ML Studio 中的模型训练任务。

1.  注册在最新任务中创建的模型。

1.  将新模型部署到在线端点。

在 Azure ML 中自动化 ML 流水线有两个选项：

1\. Azure DevOps

2\. Github Actions

你可以在[这里](https://medium.com/objectsharp/azure-pipelines-vs-github-actions-key-differences-45390ab132ee)阅读相关内容。我选择了 GitHub Actions 因为它的使用方便。

## 关于 MLOps 的快速回顾

MLOps 的需求及实现步骤：

1.  通过一致性和变更跟踪进行质量控制：

    a. 整个项目的初始设置 — IDE、工作区、权限

    b. 环境版本控制

    c. 数据版本控制

    d. 代码版本控制

    e. 其他组件的版本控制

1.  模型的快速实验：

    a. 跟踪模型超参数

    b. 跟踪不同数据切片上的模型指标、偏差、公平性和可解释性

    c. 维护 ML 流水线中变更部分之间的链接

1.  无缝的模型部署和全面的模型监控：

    a. 快速将模型部署到生产环境

    b. 分阶段推出、蓝绿或其他部署策略

    c. 跟踪模型效果以触发重新训练

    d. 跟踪数据漂移以触发重新训练

# 范围

我将整个 ML 项目结构化为三个不同的流水线，以实现上述 MLOps 目标：

1.  构建流水线

1.  训练流水线

1.  部署流水线

> 在讨论这些流水线后，我们将深入探讨代码实现。

# 1\. 构建流水线

## 1.1 初始设置

**步骤 1：设置 Azure ML Studio：**

我们将使用 MS Azure 进行数据版本控制、模型训练和部署。请按照以下步骤创建你的 Azure ML 工作区：

1.  登录到你的 Azure 帐户，或从[这里](https://azure.microsoft.com/en-in/free/search/?ef_id=Cj0KCQiAorKfBhC0ARIsAHDzsluF2Bwn4cqowJoVMFYFEUDP8Q3p-ZbXASpPhkARnhfUKPErWNlomJ0aAv-KEALw_wcB%3AG%3As&OCID=AIDcmmf1elj9v5_SEM_Cj0KCQiAorKfBhC0ARIsAHDzsluF2Bwn4cqowJoVMFYFEUDP8Q3p-ZbXASpPhkARnhfUKPErWNlomJ0aAv-KEALw_wcB%3AG%3As)获取免费的 Azure 订阅。

1.  创建资源组（[参考](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal)）。

1.  创建 ML 工作区 - 访问 [ml.azure.com](http://ml.azure.com)，点击 *创建工作区* 并按照屏幕上的指示操作。

当你的 Azure ML 工作区创建完成后，你应该能看到如下界面：

![](img/6392191e004d7a69a14a455dfa06e963.png)

通过 ml.azure.com 访问 Azure ML 工作区

> 你暂时无法看到这些任务，不用担心！

**步骤 2：将 GitHub Actions 与 Azure ML Studio 链接：**

在 GitHub 中初始化一个仓库并转到设置 > 秘密和变量 > Actions > 新建仓库秘密

打开一个新的浏览器标签页以创建一个服务主体以访问你的 ML 工作区。([ref](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-setup-authentication?tabs=sdk#configure-a-service-principal))

将生成的 JSON 保存为 AZURE_CREDENTIALS 并存储在你的仓库秘密中。

![](img/37f1b9f485e8ae45987d542cc91275e1.png)

仓库中的 GitHub Actions 密钥

**第 3 步：生成个人访问令牌 (PAT)**

使用普通的 GITHUB_TOKEN，你无法编辑工作流文件（.github/workflows 中的文件），你需要添加 PAT。你需要编辑工作流文件以自动化版本控制训练运行、模型版本等。

在 GitHub 中，从设置 > 开发者设置 > 个人访问令牌 生成 PAT。

![](img/197d96360ceb08cba34ef221581e0f36.png)

用于编辑工作流的个人访问令牌

将 PAT 保存到你的仓库秘密中，并设置以下仓库权限：

![](img/1e2f3d02c89776335ef0750c36dc2165.png)

PAT 的仓库权限

## 1.2 环境版本控制

需要两个环境：

1.  训练环境 — 用于模型训练依赖

1.  部署环境 — 用于模型服务依赖

对于较小的项目，你也可以将它们保存在相同的环境中。

## 1.3 数据版本控制

在我们的例子中，数据每周下载一次，并在 Azure 数据存储中注册为数据集。数据版本控制对于跟踪模型的血统非常重要。在我们的例子中，这是通过*jobs/data_download.save_to_data_upload(…)*

## 1.4 代码版本控制

代码版本控制通过 GitHub 完成。

## 1.5 其他组件的版本控制

你还需要版本控制 *runs, models* 和其他组件。为了自动化它们的命名，你需要在之前的 cron 作业中编辑它们。在我们的例子中，这是通过 *jobs/update_training_yamls.py* 完成的。

## 1.6 自动化测试

为了自动测试 Python 文件，我们可以使用 pytest 并跟踪代码覆盖率。

# 2. 训练管道

## 2.1 模型超参数跟踪

在我们的例子中，我们使用了 [mlflow for pytorch](https://mlflow.org/docs/latest/python_api/mlflow.pytorch.html) 来记录以下详细信息：

指标：

![](img/9f0df5264e9bdee328897fbd9f7bccf8.png)

每次训练任务中捕获的指标

运行时模型参数：

![](img/fd264a1c2d1677d117e8372deea0d1db.png)

每次运行时版本控制的参数

## 2.2 在不同数据切片上跟踪模型指标、偏差、公平性和解释性

由于我使用了股票数据来模拟不断变化的数据，我跳过了这部分。然而，在大多数机器学习用例中，这些是判断模型性能的重要指标。

## 2.3 维护机器学习管道中变化部分之间的链接

Azure ML studio 自动链接所有内容。

训练模型时的示例血统跟踪：

![](img/d0de23fe0891284a2a28c6e9964ebb11.png)

训练任务快照

同样，数据和模型也会自动版本化并跟踪它们的历史。

# 3\. 部署流水线

## 3.1 快速模型部署到生产环境

最初创建端点并部署第一个模型需要一些时间。之后，新注册的模型可以在更短的时间内用于更新端点。

## 3.2 分阶段推出、蓝绿部署或不同的部署策略

可以使用不同的部署策略。在我们的案例中，我们覆盖了之前部署的模型。然而，通过 Azure ML，蓝绿部署在部署过程中可以非常轻松地实现。

## 3.3 追踪模型效果以触发重新训练

模型部署后，我们需要跟踪模型对已知标签的性能。这将帮助我们识别模型在哪些数据层次上表现不佳。这将帮助我们确定是否需要收集更多数据或在下一次模型重新训练期间采取其他措施。

## 3.4 追踪数据漂移以触发重新训练

对于模型重新训练，有两种思路：

1\. 基于计划

2\. 基于漂移

两种选项各有优缺点。在这个例子中，我遵循基于计划的模型重新训练。因此，我没有实现任何数据漂移监控。然而，Azure 提供了监控数据漂移的工具。

# 代码

作为工程师，从头开始编写代码时会变得更加清晰。所以，尽情使用吧！

**代码库：** [**coderkol95/MLOps_stock_prediction**](https://github.com/coderkol95/MLOps_stock_prediction)

项目文件夹结构：

![](img/b16aca6ac49230e3ab74f5503205421c.png)

## 文件夹和文件的简要信息：

## **.github/workflows/**

控制流水线的 yml 文件放在这里。这些文件通过 cron 作业触发数据下载/上传、模型训练、注册和部署。

data_pipeline.yml：频率 - 每周一凌晨 1:01

- 下载股票数据并更新 yml 文件

通过 API 下载数据到 csv 文件，并更新 data-upload.yml 文件，添加数据集标签、版本和路径。

- 编辑 yaml 文件

更新 yml 文件中其他组件的版本，如 job_name、模型版本等，这些将在运行期间使用。

- 将文件推送到 github

将更新的 yml 文件和下载的 csv 文件推送到代码库

- 上传到 azure

在 Azure 数据存储中注册数据集

```py
name: data upload to azure

env:
  ticker: WIPRO.NS
  start: 366
  end: 1

on:
  schedule:
    - cron: "1 1 * * 1"

jobs:
  datawork:
    runs-on: ubuntu-latest
    steps:
    - name: checkout repository
      uses: actions/checkout@v2
      with:
        token: ${{ secrets.PAT }}
        repository: 'coderkol95/MLOps_stock_prediction'
    - name: setup python 3.9
      uses: actions/setup-python@v4
      with: 
        python-version: "3.9"
    - name: install python packages
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
    - name: download ticker data and update yml file
      run: python data_download.py --ticker $ticker --start $start --end $end
      id: data
      working-directory: jobs
    - name: edit yaml files
      run: python update_training_yamls.py
      working-directory: jobs
    - name: push files to github
      run: |
        git config --local user.email "action@github.com"
        git config --local user.name "GitHub Action"
        git add -A
        git commit -m "Ticker data for $ticker downloaded and YAML file updated." || exit 0
        git push @github.com/${GITHUB_REPOSITORY}.git">https://x-access-token:${GITHUB_TOKEN}@github.com/${GITHUB_REPOSITORY}.git HEAD
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    - name: azure login
      uses: azure/login@v1
      with:
        creds: ${{secrets.AZURE_CREDENTIALS}}
    - name: setup
      run: bash setup.sh
      working-directory: cli
      continue-on-error: true
    - name: upload to azure
      run: az ml data create -f jobs/data_upload.yml
```

model_pipeline.yml：频率 - 每周一凌晨 2:01

* 训练作业

在最新的数据集上计算创建和模型训练。

数据集准备和模型训练通过 pytorch lightning 完成。所有日志记录通过 MLFlow 进行。有关代码的详细信息，请参阅我的 [代码库](https://github.com/coderkol95/MLOps_stock_prediction)。

* 注册作业

从最新运行中注册模型。

* 删除计算

训练完成后计算删除。

```py
name: training and registering model

env:
  job_name: ga-run-10
  compute_name: computer456
  registered_model_name: GA_model

on:
    schedule:
    - cron: "1 2 * * 1"

jobs:
  train-job:
    runs-on: ubuntu-latest
    steps:
    - name: check out repo
      uses: actions/checkout@v2
    - name: azure login
      uses: azure/login@v1
      with:
        creds: ${{secrets.AZURE_CREDENTIALS}}
    - name: setup
      run: bash setup.sh
      working-directory: cli
      continue-on-error: true
    - name: create-compute
      run: az ml compute create --name $compute_name --size STANDARD_DS11_v2 --min-instances 1 --max-instances 2 --type AmlCompute
    - name: train-job
      working-directory: jobs
      run: az ml job create --file train.yml --debug --stream # --stream causes the step to go on, as long as the model trains.
# If training is expected to take a long time, registration can be scheduled in a separate cron job, triggered later.
  register-job:
    needs: [train-job]
    runs-on: ubuntu-latest
    steps:
    - name: check out repo
      uses: actions/checkout@v2
    - name: azure login
      uses: azure/login@v1
      with:
        creds: ${{secrets.AZURE_CREDENTIALS}}
    - name: setup
      run: bash setup.sh
      working-directory: cli
      continue-on-error: true
    - name: register-model
      run: az ml model create
        --name $registered_model_name 
        --version 10
        --path azureml://jobs/ga-run-10/outputs/artifacts/paths/outputs/
        --type custom_model

  delete-compute:
    needs: [train-job]
    runs-on: ubuntu-latest
    steps:
    - name: check out repo
      uses: actions/checkout@v2
    - name: azure login
      uses: azure/login@v1
      with:
        creds: ${{secrets.AZURE_CREDENTIALS}}
    - name: setup
      run: bash setup.sh
      working-directory: cli
      continue-on-error: true
    - name: delete-step
      run: az ml compute delete --name $compute_name --yes
```

deployment_pipeline.yml：频率 - 每周一凌晨 3:01

* 端点和部署创建（如果是第一次）

* 使用最新模型更新在线部署（如下所示）

```py
name: model deployment
on:
  schedule:
    - cron: "1 3 * * 1"
jobs:
  # compare-job:
  #  Compare if the model is good enough
  #  Profile the model
  #  If it is good enough, proceed to next step 
  deployment-job:
    runs-on: ubuntu-latest
    steps:
    - name: check out repo
      uses: actions/checkout@v2
    - name: azure login
      uses: azure/login@v1
      with:
        creds: ${{secrets.AZURE_CREDENTIALS}}
    - name: setup
      run: bash setup.sh
      working-directory: cli
      continue-on-error: true
    # Commenting out as endpoint creation is only needed during the first run
    # - name: create-endpoint
    #   run: az ml online-endpoint create --name ga-deployment
    - name: deployment-step
      run: az ml online-deployment update -f deploy.yml #--all-traffic # First time it'll be az ml ... create --all-traffic
      working-directory: jobs
```

我将 cron 作业的执行时间间隔设置为 1 小时，因为它在 1 小时内完成，你可以根据需要设置更长的间隔。你也可以设置标志来捕捉作业完成状态。

## **cli/**

setup.sh: 配置运行代码的虚拟机，适用于 Azure ML。

```py
GROUP="RG"
LOCATION="eastus"
WORKSPACE="AzureMLWorkspace"

az configure --defaults group=$GROUP workspace=$WORKSPACE location=$LOCATION

az extension remove -n ml
az extension add -n ml
```

## **jobs/**

这里保存了特定于 Azure 的 YAML 文件以及用于单个管道步骤执行的 Python 脚本。

**data_download.py** get_ticker_data(…)

通过 API 调用下载数据并保存到 CSV 文件。我使用 Yahoo Finance API 下载了数据。

get_dataset_tags(…)

版本控制数据并添加标签。

![](img/287918fd613e7b674d00d7e943155647.png)

在上传到 Azure 之前生成的数据集标签。

save_to_data_upload(…)

将数据集规格写入 Azure yml 文件，以便上传到 Azure 数据存储。

对于代码，你可以参考我的[代码库](https://github.com/coderkol95/MLOps_stock_prediction)。

**data_upload.yml**

这个 yml 文件由*jobs/data_download.save_to_data_upload(…)* 更新。这个 yml 文件将数据集上传到 Azure。

```py
$schema: https://azuremlschemas.azureedge.net/latest/data.schema.json

type: uri_file
name: 'WIPRO'
description: Stock data for WIPRO.NS during 2022-02-14:2023-02-13 in 1d interval.
path: '../data/WIPRO.NS.csv'
tags: {'Length': 249, 'Start': '2022-02-14', 'End': '2023-02-13', 'Median': 413.7, 'SD': 69.09}
version: 20230215
```

**deploy.yml**

这个 yml 文件在每次 cron 作业前由*jobs/update_training_yamls.py* 更新。

```py
$schema: https://azuremlschemas.azureedge.net/latest/managedOnlineDeployment.schema.json
name: green
endpoint_name: ga-deployment
model: azureml:GA_model:10
code_configuration:
  code: ../jobs
  scoring_script: deployment.py
environment: azureml:stock-pricing:5
instance_type: Standard_DS1_v2
instance_count: 1
```

**deployment.py**

在端点使用的脚本来生成在线预测。有关代码，你可以参考我的[代码库](https://github.com/coderkol95/MLOps_stock_prediction)。

init()

初始化模型和 PyTorch Lightning 使用的数据模块。

run()

用于提供来自模型的在线预测。

**train.py**

使用 PyTorch Lightning 和 MLflow 的模型训练脚本。有关代码，你可以参考我的[代码库](https://github.com/coderkol95/MLOps_stock_prediction)。

**train.yml** 这个 yml 文件在每次 cron 作业前由*jobs/update_training_yamls.py* 更新。

```py
$schema: https://azuremlschemas.azureedge.net/latest/commandJob.schema.json
name: ga-run-10
tags:
    modeltype: pytorch
code: ../jobs
command: >-
  python train.py 
  --data ${{inputs.data}}
inputs:
  data:
    type: uri_file
    path: azureml:WIPRO@latest
environment: azureml:stock-pricing:4
compute: azureml:computer456
display_name: stock
experiment_name: ga_train_job
description: Training job via Github actions
```

**update_training_yamls.py**

更新在每次运行前需要更新的多个组件的版本，如运行 ID、待注册的模型版本、待部署的模型。有关代码，你可以参考我的[代码库](https://github.com/coderkol95/MLOps_stock_prediction)。

# 结论

我希望你现在对如何使用 MS Azure 实现自动化端到端的 MLOps 项目有了了解。有关详细的代码实现，你可以参考我的[代码库](https://github.com/coderkol95/MLOps_stock_prediction)。
