# CI/CD 在 AWS 的多模型端点

> 原文：[https://towardsdatascience.com/ci-cd-for-multi-model-endpoints-in-aws-18bf939e0a48?source=collection_archive---------7-----------------------#2023-06-22](https://towardsdatascience.com/ci-cd-for-multi-model-endpoints-in-aws-18bf939e0a48?source=collection_archive---------7-----------------------#2023-06-22)

## 一个简单、灵活的可持续机器学习解决方案的替代方案

[](https://medium.com/@andrewcharabin?source=post_page-----18bf939e0a48--------------------------------)[![Andrew Charabin](../Images/8cfe2657a9cd16c3ce30b98e3c9e9945.png)](https://medium.com/@andrewcharabin?source=post_page-----18bf939e0a48--------------------------------)[](https://towardsdatascience.com/?source=post_page-----18bf939e0a48--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----18bf939e0a48--------------------------------) [Andrew Charabin](https://medium.com/@andrewcharabin?source=post_page-----18bf939e0a48--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff282e085f18e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fci-cd-for-multi-model-endpoints-in-aws-18bf939e0a48&user=Andrew+Charabin&userId=f282e085f18e&source=post_page-f282e085f18e----18bf939e0a48---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----18bf939e0a48--------------------------------) ·14分钟阅读·2023年6月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F18bf939e0a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fci-cd-for-multi-model-endpoints-in-aws-18bf939e0a48&user=Andrew+Charabin&userId=f282e085f18e&source=-----18bf939e0a48---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F18bf939e0a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fci-cd-for-multi-model-endpoints-in-aws-18bf939e0a48&source=-----18bf939e0a48---------------------bookmark_footer-----------)![](../Images/c1424ad7b2588399204bb9dd6c2e504c.png)

图片来源于 VectorStock，授权给**Andrew Charabin**

自动化生产机器学习解决方案的再培训和部署是确保模型考虑到[covariate shift](https://www.seldon.io/what-is-covariate-shift#:~:text=It%20is%20when%20the%20distribution,issue%20encountered%20in%20machine%20learning.)的关键步骤，同时减少出错和不必要的人力投入。

对于使用 AWS 堆栈和特别是 SageMaker 部署的模型，AWS 提供了一种标准 CI/CD 解决方案，使用 [SageMaker Pipelines](https://aws.amazon.com/sagemaker/pipelines/) 来自动化重新训练/部署，以及 [SageMaker 模型注册表](https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry.html) 来跟踪模型的传承。

虽然标准解决方案在标准情况下效果良好，但在更复杂的情况下存在若干限制：

1.  输入数据需要从 AWS s3 获取。

1.  设置动态预热超参数调整的难度。

1.  需要额外的模型训练步骤来训练多个模型。

1.  执行管道的启动时间较长。

1.  调试工具有限。

幸运的是，AWS 推出了可以用来构建克服这些限制的 CI/CD 管道的新功能。以下功能可以在 [SageMaker Studio](https://aws.amazon.com/sagemaker/studio/) 中访问，这是 AWS 的集成开发环境，用于机器学习：

+   [使用自定义 SageMaker 图像](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-byoi-launch.html)

+   [Git/SageMaker Studio 集成](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-tasks-git.html)

+   [预热超参数调整](https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning-warm-start.html)

+   [SageMaker Studio 笔记本作业](https://aws.amazon.com/blogs/machine-learning/operationalize-your-amazon-sagemaker-studio-notebooks-as-scheduled-notebook-jobs/)

+   [跨账户模型注册表](https://aws.amazon.com/blogs/machine-learning/build-a-cross-account-mlops-workflow-using-the-amazon-sagemaker-model-registry/)

***本文的目的…***

目的是通过 AWS 云探讨一种替代 CI/CD 解决方案的关键细节，该方案提供了更多灵活性和更快的市场速度。

> ***解决方案组件概述：***
> 
> *1\. 自定义 SageMaker Studio 图像用于 PostgreSQL 查询*
> 
> *2\. 动态预热超参数调整*
> 
> *3\. 在单个交互式 Python 笔记本中注册多个模型到模型注册表*
> 
> *4\. 用新模型刷新多模型端点*
> 
> *5\. 计划重新训练/重新部署笔记本以在设定的周期上运行*

开始吧。

## 1\. 自定义 SageMaker Studio 图像 *用于 PostgreSQL 查询*

虽然 SageMaker 管道允许从 s3 获取输入数据，但如果新输入数据位于数据仓库中，如 AWS Redshift 或 Google BigQuery 呢？当然，可以使用 [ETL](https://www.ibm.com/topics/etl) 或类似过程将数据批量移动到 s3，但这与直接从数据仓库查询数据相比，增加了不必要的复杂性/僵化。

SageMaker Studio提供了几种默认镜像来初始化环境，其中一个例子是包含常用包如numpy和pandas的‘Data Science’镜像。然而，要在Python中连接到PostgreSQL数据库，需要一个驱动程序或适配器。[Psycopg2](https://pypi.org/project/psycopg2/)是Python编程语言中最流行的PostgreSQL数据库适配器。幸运的是，可以使用自定义镜像来初始化Studio环境，尽管有特定的要求。我已经预打包了一个满足这些要求的Docker镜像，并在Python Julia-1.5.2镜像基础上添加了psycopg2驱动程序。该镜像可以在[这个](https://github.com/acharabin/SageMaker-Studio-Image-With-Psycopg2)git仓库中找到。然后，可以使用[这里](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-byoi.html)概述的步骤使镜像在Studio域中可用。

## 2\. 动态预热启动超参数调优

模型重新训练在性质上与初始模型训练不同。在重新训练模型时，投资相同数量的资源来搜索最佳模型超参数以及相同的大范围搜索空间是不切实际的。特别是当仅期望对上一生产模型的最佳超参数进行微调时尤其如此。

因此，本文推荐的CI/CD超参数调优解决方案不会尝试通过K折交叉验证、预热池等方式快速重新调优。这些方法对于初始模型训练非常有效。然而，对于重新训练，我们希望从生产中已经有效的地方开始，并对新获取的数据进行小幅调整。因此，使用动态预热启动超参数调优是完美的解决方案。进一步地，可以创建一个动态预热启动调优系统，使用最新的生产调优作业作为父作业。以下是一个示例XGBoost贝叶斯调优作业的解决方案：

```py
# Set Run Parameters

testing=False
hyperparam_jobs=10

# Set Max Jobs

if testing==False: max_jobs=hyperparam_jobs
else: max_jobs=1

# Load Packages

from sagemaker.xgboost.estimator import XGBoost
from sagemaker.tuner import IntegerParameter
from sagemaker.tuner import ContinuousParameter
from sagemaker.tuner import HyperparameterTuner
from sagemaker.tuner import WarmStartConfig, WarmStartTypes

# Configure Warm Start
number_of_parent_jobs=1

# Can be up to 5, but currently only a value of 1 is supported in the code
# Note base_dir needs to be set, can also be set blank

try: eligible_parent_tuning_jobs=pd.read_csv(f"""{base_dir}logs/tuningjobhistory.csv""")
except: 
    eligible_parent_tuning_jobs=pd.DataFrame({'datetime':[],'tuningjob':[],'metric':[],'layer':[],'objective':[],'eval_metric':[],'eval_metric_value':[],'trainingjobcount':[]})
    eligible_parent_tuning_jobs.to_csv(f"""{base_dir}logs/tuningjobhistory.csv""",index=False)
eligible_parent_tuning_jobs=eligible_parent_tuning_jobs[(eligible_parent_tuning_jobs['layer']==prefix)&(eligible_parent_tuning_jobs['metric']==metric)&(eligible_parent_tuning_jobs['objective']==trainingobjective)&(eligible_parent_tuning_jobs['eval_metric']==objective_metric_name)&(eligible_parent_tuning_jobs['trainingjobcount']>1)].sort_values(by='datetime',ascending=True)
eligible_parent_tuning_jobs_count=len(eligible_parent_tuning_jobs)
if eligible_parent_tuning_jobs_count>0:
    parent_tuning_jobs=eligible_parent_tuning_jobs.iloc[(eligible_parent_tuning_jobs_count-(number_of_parent_jobs)):eligible_parent_tuning_jobs_count,1].iloc[0]
    warm_start_config = WarmStartConfig(
    WarmStartTypes.TRANSFER_LEARNING, parents={parent_tuning_jobs})
    # Note that WarmStartTypes.IDENTICAL_DATA_AND_ALGORITHM can be used when applicable

    print(f"""Warm starting using tuning job: {parent_tuning_jobs[0]}""")

else: warm_start_config = None

# Define exploration boundaries (default suggested values from Amazon SageMaker Documentation)

hyperparameter_ranges = {
    'eta': ContinuousParameter(0.1, 0.5, scaling_type='Logarithmic'),
    'max_depth': IntegerParameter(0,10,scaling_type='Auto'),
    'num_round': IntegerParameter(1,4000,scaling_type='Auto'),
    'subsample': ContinuousParameter(0.5,1,scaling_type='Logarithmic'),
    'colsample_bylevel': ContinuousParameter(0.1, 1,scaling_type="Logarithmic"),
    'colsample_bytree': ContinuousParameter(0.5, 1, scaling_type='Logarithmic'),
    'alpha': ContinuousParameter(0, 1000, scaling_type="Auto"),
    'lambda': ContinuousParameter(0,100,scaling_type='Auto'),
    'max_delta_step': IntegerParameter(0,10,scaling_type='Auto'),
    'min_child_weight': ContinuousParameter(0,10,scaling_type='Auto'),
    'gamma':ContinuousParameter(0, 5, scaling_type='Auto'),
}
tuner_log = HyperparameterTuner(
    estimator,
    objective_metric_name,
    hyperparameter_ranges,
    objective_type='Minimize', 
    max_jobs=max_jobs,
    max_parallel_jobs=10,
    strategy='Bayesian',
    base_tuning_job_name="transferlearning",
    warm_start_config=warm_start_config
)

# Note a SageMaker XGBoost estimater needs to be instantiated in advance
training_input_config = sagemaker.TrainingInput("s3://{}/{}/{}".format(bucket,prefix,filename), content_type='csv')
validation_input_config = sagemaker.TrainingInput("s3://{}/{}/{}".format(bucket,prefix,filename), content_type='csv')

# Note bucket, prefix, and filename objects/aliases need to be set

# Starts the hyperparameter tuning job

tuner_log.fit({'train': training_input_config, 'validation': validation_input_config})

# Prints the status of the latest hyperparameter tuning job

boto3.client('sagemaker').describe_hyper_parameter_tuning_job(
    HyperParameterTuningJobName=tuner_log.latest_tuning_job.job_name)['HyperParameterTuningJobStatus']
```

调优作业历史将保存在基础目录中的日志文件中，示例输出如下：

![](../Images/329069d473afac7188601c22592ee081.png)

作者提供的图表

日期/时间戳、调优作业名称以及元数据以.csv格式存储，新调优作业会追加到文件中。

系统将动态地使用满足要求条件的最新调优作业进行预热启动。在这个例子中，条件在以下代码行中注明：

```py
eligible_parent_tuning_jobs=eligible_parent_tuning_jobs[(eligible_parent_tuning_jobs['layer']==prefix)&(eligible_parent_tuning_jobs['metric']==metric)&(eligible_parent_tuning_jobs['objective']==trainingobjective)&(eligible_parent_tuning_jobs['eval_metric']==objective_metric_name)&(eligible_parent_tuning_jobs['trainingjobcount']>1)].sort_values(by='datetime',ascending=True)
```

因为我们需要测试管道的工作情况，所以提供了`testing=True`运行选项，这将强制仅进行一个超参数调优作业。添加了一个条件，只考虑具有多个调优模型作为父模型的作业，前提是这些是非测试的。此外，调优作业日志文件可以在不同模型间使用，因为理论上可以在模型间使用父作业。在这种情况下，模型通过‘metric’字段进行跟踪，符合条件的调优作业会过滤以匹配当前训练实例中的指标。

一旦重新训练完成，我们将把新的超参数调整作业追加到日志文件中，并将其写入本地以及s3，同时启用[版本控制](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html)。

```py
# Append Last Parent Job for Next Warm Start

eligible_parent_tuning_jobs=pd.read_csv(f"""{base_dir}logs/tuningjobhistory.csv""")
latest_tuning_job=boto3.client('sagemaker').describe_hyper_parameter_tuning_job(
    HyperParameterTuningJobName=tuner_log.latest_tuning_job.job_name)
updatetuningjobhistory=pd.concat([eligible_parent_tuning_jobs,pd.DataFrame({'datetime':[datetime.now().strftime("%Y/%m/%d %H:%M:%S")],'tuningjob':[latest_tuning_job['HyperParameterTuningJobName']],'metric':[metric],'layer':prefix,'objective':[trainingobjective],'eval_metric':[latest_tuning_job['BestTrainingJob']['FinalHyperParameterTuningJobObjectiveMetric']['MetricName']],'eval_metric_value':latest_tuning_job['BestTrainingJob']['FinalHyperParameterTuningJobObjectiveMetric']['Value'],'trainingjobcount':[latest_tuning_job['HyperParameterTuningJobConfig']['ResourceLimits']['MaxNumberOfTrainingJobs']]})],axis=0)
print(updatetuningjobhistory)

# Write locally

updatetuningjobhistory.to_csv(f"""{base_dir}logs/tuningjobhistory.csv""",index=False)

# Upload to s3

s3.upload_file(f"""{base_dir}logs/tuningjobhistory.csv""",bucket,'logs/tuningjobhistory.csv')
```

**3\. 在单个交互式Python笔记本中将多个模型注册到模型注册表**

通常，组织会有多个AWS账户用于不同的用例（即沙盒、QA和生产）。你需要确定在CI/CD解决方案的每个步骤中使用哪个账户，然后添加[本指南](https://aws.amazon.com/blogs/machine-learning/build-a-cross-account-mlops-workflow-using-the-amazon-sagemaker-model-registry/)中提到的跨账户权限。

推荐在同一账户中进行模型训练和模型注册，特别是沙盒或测试账户。因此，在下图中，‘数据科学’和‘共享服务’账户将是相同的。在该账户中，需要一个s3桶来存放模型工件并跟踪与流水线相关的其他文件的血统。模型/端点将在每个‘部署’账户（即沙盒、QA、生产）中分别部署，引用训练/注册账户中的模型工件和注册表。

![](../Images/62a4d64e3bf6d781dd23ffc4aeb9bfa0.png)

来自[AWS 文档](https://aws.amazon.com/blogs/machine-learning/build-a-cross-account-mlops-workflow-using-the-amazon-sagemaker-model-registry/)

现在我们已经决定了用于训练和存放模型注册表的AWS账户，我们可以构建初始模型并开发CI/CD解决方案。

使用SageMaker Pipelines时，为数据预处理、训练/调整、评估、注册以及任何后处理创建独立的管道步骤。虽然这对于单个模型管道是可以的，但当需要多个模型来解决机器学习方案时，会产生大量的管道代码重复。

因此，推荐的解决方案是构建并调度三个交互式Python笔记本在SageMaker Studio中。它们按顺序运行，并通过一个自动化的笔记本作业一起完成CI/CD流水线：

> ***A. 数据准备***
> 
> ***B. 模型训练、评估和注册***
> 
> ***C. 使用最新批准的模型刷新端点***

A. 数据准备

在这里，我们将从数据仓库查询并加载数据，然后将其写入本地和s3。我们可以使用当前日期设置动态的日期/时间条件，并将生成的日期下限和上限传递到SQL查询中。

```py
# Connect to Data Warehouse

dbname='<insert here>'
host='<insert here>'
password='<insert here>'
port='<insert here>'
search_path='<insert here>'
user='<insert here>'

import psycopg2
data_warehouse= psycopg2.connect(f"""host={host} port={port} dbname={dbname} user={user} password={password} options = '-c search_path={search_path}'""")

# Set Dataset Date Floor and Ceiling Applied to Pass in & Apply to Query

datestart=date(2000, 1, 1)
pushbackdays=30
dateend=date.today() - timedelta(days=pushbackdays)
print(datestart)
print(dateend)

# Query data warehouse

modelbuildingset=pd.read_sql_query(f"""<insert query>""",data_warehouse)

# Write .csv

modelbuildingset.to_csv(f"{base_dir}datasets/{filename}", index=False)
modelbuildingset

# Upload to s3 for Lineage Tracking

s3 = boto3.client('s3')
s3.upload_file(f"{base_dir}datasets/{filename}",bucket,f"datasets/{filename}")
```

这一步骤以将准备好的训练数据保存到本地以及s3以进行血统追踪结束。

B. 模型训练、评估和注册

通过在 Studio 中使用交互式 Python notebook，我们现在可以在一个 notebook 中完成模型训练、评估和注册。所有这些步骤都可以构建为一个函数，并适用于需要重新训练的其他模型。为了说明，代码未使用函数提供。

在继续之前，需要在注册表中为解决方案中的每个模型创建模型包组（可以在控制台中创建，也可以通过 Python 创建）。

```py
# Get the Best Training Job

best_overall_training_job_name = latest_tuning_job['BestTrainingJob']['TrainingJobName']

# latest_tuning_job was obtained from the hyperparameter tuning section
latest_tuning_job['BestTrainingJob']

# Install XGBoost

! pip install xgboost

# Download the Best Model

s3 = boto3.client('s3')
s3.download_file('<s3 bucket>', f"""output/{best_overall_training_job_name}/output/model.tar.gz""", f"""{base_dir}models/{metric}/model.tar.gz""")

# Open and Load the Downloaded Model Artifact in Memory

tar = tarfile.open(f"""{base_dir}models/{metric}/model.tar.gz""")
tar.extractall(f"""{base_dir}models/{metric}""")
tar.close()
model = pkl.load(open(f"""{base_dir}models/{layer}/{metric}/xgboost-model""", 'rb'))

# Perform Model Evaluation

import json
import pathlib
import joblib
from sklearn.metrics import mean_squared_error
from sklearn.metrics import mean_absolute_error
import math
evaluationset=pd.read_csv(f"""{base_dir}datasets/{layer}/{metric}/{metric}modelbuilding_test.csv""")
evaluationset['prediction']=model.predict(xgboost.DMatrix(evaluationset.drop(evaluationset.columns.values[0], axis=1), label=evaluationset[[evaluationset.columns.values[0]]]))

# In the Example a Regression Problem is Used with MAE & RMSE as Eval Metrics

mae = mean_absolute_error(evaluationset[evaluationset.columns.values[0]], evaluationset['prediction'])
rmse = math.sqrt(mean_squared_error(evaluationset[evaluationset.columns.values[0]], evaluationset['prediction']))
stdev_error = np.std(evaluationset[evaluationset.columns.values[0]] - evaluationset['prediction'])
evaluation_report=pd.DataFrame({'datetime':[datetime.now().strftime("%Y/%m/%d %H:%M:%S")], 'testing':[testing], 'trainingjob': [best_overall_training_job_name], 'objective':[trainingobjective], 'hyperparameter_tuning_metric':[objective_metric_name], 'mae':[mae], 'rmse':[rmse], 'stdev_error':[stdev_error]})

# Load Past Evaluation Reports

try: past_evaluation_reports=pd.read_csv(f"""{base_dir}models/{metric}/evaluationhistory.csv""")
except: past_evaluation_reports=pd.DataFrame({'datetime':[],'testing':[], 'trainingjob': [], 'objective':[], 'hyperparameter_tuning_metric':[], 'mae':[], 'rmse':[], 'stdev_error':[]})
evaluation_report=pd.concat([past_evaluation_reports,evaluation_report],axis=0)
print(evaluation_report)

# Write .csv

evaluation_report.to_csv(f"""{base_dir}models/{metric}/evaluationhistory.csv""",index=False)

# Write to s3

s3.upload_file(f"""{base_dir}models/{metric}/evaluationhistory.csv""",'<s3 bucket>',f"""{layer}/{metric}/evaluationhistory.csv""")

# Note Can Also Associate a Registered Model with Eval Metrics, But Will Skip it Here

report_dict = {}

# Register Model

model_package_group_name='<>'
modelpackage_inference_specification =  {
    "InferenceSpecification": {
      "Containers": [
         {
            "Image": xgboost_container,
     "ModelDataUrl": f"""s3://{s3 bucket}/output/{best_overall_training_job_name}/output/model.tar.gz"""
         }
      ],
      "SupportedContentTypes": [ "text/csv" ],
      "SupportedResponseMIMETypes": [ "text/csv" ],
   }
 }
create_model_package_input_dict = {
    "ModelPackageGroupName" : model_package_group_name,
    "ModelPackageDescription" : "<insert description here>",
    "ModelApprovalStatus" : "PendingManualApproval",
    "ModelMetrics" :report_dict
}
create_model_package_input_dict.update(modelpackage_inference_specification)
sm_client = boto3.client('sagemaker')
create_model_package_response = sm_client.create_model_package(**create_model_package_input_dict)
model_package_arn = create_model_package_response["ModelPackageArn"]
print('ModelPackage Version ARN : {}'.format(model_package_arn))
```

通过打开注册表中的模型包组，您可以查看所有已注册的模型版本、注册日期和批准状态。

![](../Images/0a582302948d0320e279dda5eb3f2bb5.png)

来自 [AWS 文档](https://docs.aws.amazon.com/sagemaker/latest/dg/modelregistryfaq.html) 的图表

管道的主管可以查看在前一步中本地保存的评估报告，其中包含所有过去模型评估的历史记录，并根据测试集评估指标确定是否批准或拒绝该模型。稍后，可以设置条件，仅在模型获得批准后更新生产（或 QA）端点。

**4\. 用新模型刷新多模型端点**

SageMaker 具有一个 [MultiDataModel](https://sagemaker.readthedocs.io/en/stable/api/inference/multi_data_model.html) 类，允许部署可以托管多个模型的 SageMaker 端点。其原理在于可以在同一个计算实例中加载多个模型，共享资源并节省成本。此外，它简化了模型的重新训练/管理，因为只需更新一个端点以反映新模型并进行管理，而不必在每个专用端点之间重复步骤（这也可以作为替代方案）。MultiDataModel 类也可以用于部署单个模型，如果计划在未来向解决方案中添加更多模型，这可能会很有意义。

在首次训练账户中，我们需要创建模型和端点。MultiDataModel 类需要一个位置来存储可以在调用时加载到端点中的模型工件；下面我们将使用正在使用的 s3 bucket 中的“models”目录。

```py
# Load Container

from sagemaker.xgboost.estimator import XGBoost
xgboost_container = sagemaker.image_uris.retrieve("xgboost", region, "1.2-2")

# One Time: Build Multi Model

estimator = sagemaker.estimator.Estimator.attach('sagemaker-xgboost-220611-1453-011-699894eb')
xgboost_container = sagemaker.image_uris.retrieve("xgboost", region, "1.2-2")
model = estimator.create_model(role=role, image_uri=xgboost_container)
from sagemaker.multidatamodel import MultiDataModel
sagemaker_session=sagemaker.Session()

# This is where our MME will read models from on S3.

model_data_prefix = f"s3://{bucket}/models/"
mme = MultiDataModel(
    name=model_name,
    model_data_prefix=model_data_prefix,
    model=model,  # passing our model - passes container image needed for the endpoint
    sagemaker_session=sagemaker_session,
)

# One Time: Deploy the MME

ENDPOINT_INSTANCE_TYPE = "ml.m4.xlarge"
ENDPOINT_NAME = "<insert here>"
predictor = mme.deploy(
    initial_instance_count=1, instance_type=ENDPOINT_INSTANCE_TYPE, endpoint_name=ENDPOINT_NAME,kms_key='<insert here if desired>'
)
```

之后，可以按照以下方式引用 MultiDataModel：

```py
model=sagemaker.model.Model(model_name)

from sagemaker.multidatamodel import MultiDataModel
sagemaker_session=sagemaker.Session()

# This is where our MME will read models from on S3.
model_data_prefix = f"s3://{bucket}/models/"

mme = MultiDataModel(
    name=model_name,
    model_data_prefix=model_data_prefix,
    model=model,  # passing our model - passes container image needed for the endpoint
    sagemaker_session=sagemaker_session,
)
```

可以通过将工件复制到端点将使用的 {s3 bucket}/models 目录，将模型添加到 MultiDataModel 中。我们所需的只是模型包组名称，模型注册表将提供相应的源工件位置和批准状态。

我们可以添加一个条件，仅在模型获得批准后才添加最新模型，如下所示。如果需要立即部署进行数据科学 QA 并最终批准模型，可以在沙箱账户中省略此条件。

```py
# Get the latest model version and associated artifact location for a given model package group

ModelPackageGroup = 'model_package_group'

list_model_packages_response = client.list_model_packages(ModelPackageGroupName=f"arn:aws:sagemaker:{region}:{aws_account_id}:model-package-group/{ModelPackageGroup}")
list_model_packages_response

latest_model_version_arn = list_model_packages_response["ModelPackageSummaryList"][0][
    "ModelPackageArn"
]
print(latest_model_version_arn)

modelpackage=client.describe_model_package(ModelPackageName=latest_model_version_arn)
modelpackage

artifact_path=modelpackage['InferenceSpecification']['Containers'][0]['ModelDataUrl']
artifact_path

# Add model if approved

if list_model_packages_response["ModelPackageSummaryList"][0]['ModelApprovalStatus']=="Approved":
    model_artifact_name='<model_name>.tar.gz'
    mme.add_model(model_data_source=artifact_path, model_data_path=model_artifact_name)
```

然后，我们可以使用以下函数列出已添加的模型：

```py
list(mme.list_models())

# Output we'd see if we added the following two models
['modela.tar.gz','modelb.tar.gz']
```

要删除模型，可以在控制台中导航到关联的 s3 目录，并删除其中的任何一个；它们会在重新列出可用模型时消失。

模型在部署的端点中可以通过以下代码进行调用：

```py
response = runtime_sagemaker_client.invoke_endpoint(
                        EndpointName = "<endpoint_name>",
                        ContentType  = "text/csv",
                        TargetModel  = "<model_name>.tar.gz",
                        Body         = body)
```

在首次调用模型时，端点将加载目标模型，从而导致额外的延迟。在模型已加载的未来调用中，推理将立即获得。在 [多模型端点开发指南](https://docs.aws.amazon.com/sagemaker/latest/dg/multi-model-endpoints.html) 中，AWS 指出，当端点达到内存使用阈值时，最近未调用的模型将被“卸载”。模型将在下次调用时重新加载。

当现有模型工件通过 mme.add_model() 或在 s3 控制台中被覆盖时，部署的端点不会立即反映变化。为了强制端点在下次调用时重新加载最新的模型工件，我们可以使用一个技巧，即通过任意新的端点配置更新端点。这将创建一个新端点，需要加载模型，并安全地管理旧端点和新端点之间的过渡。由于每个端点配置需要唯一的名称，我们可以添加带有日期/时间戳的后缀。

```py
# Get datetime for endpoint configuration

time=str(datetime.now())[0:10]+'--'+str(datetime.now())[11:13]+'-'+'00'
time

# Create new endpoint config in order to 'refresh' loaded models to account for new deployments
create_endpoint_config_api_response = client.create_endpoint_config(
                                            EndpointConfigName=f"""<endpoint name>-{time}""",
                                            ProductionVariants=[
                                                {
                                                    'VariantName': model_name,
                                                    'ModelName': model_name,
                                                    'InitialInstanceCount': 1,
                                                    'InstanceType': instance_type
                                                },
                                            ]
                                       )

# Update endpoint with new config

response = client.update_endpoint(
    EndpointName=endpoint_name,
    EndpointConfigName=f"""{model_name}-{time}""")
response
```

运行此代码后，你会看到关联的端点在控制台中将显示“更新中”状态。在此更新期间，之前的端点将可用，且在新端点准备好后将被交换，此时状态将调整为“服务中”。新添加的模型将在下次调用时加载。

我们现在已经构建了 CI/CD 解决方案所需的三个笔记本 —— 数据准备、训练/评估和端点更新。然而，这些文件目前仅存在于训练 AWS 账户中。我们需要调整第三个笔记本，以便在任何部署的 AWS 账户中工作，在这些账户中将创建/更新相应的端点。

为此，我们可以添加基于 AWS 账户 ID 的条件逻辑。新的 AWS 账户也需要 s3 存储桶来存放模型工件。由于 s3 存储桶名称在 AWS 中需要唯一，因此可以使用这种条件逻辑。此外，这也可以应用于调整端点实例类型以及添加新模型的条件（即审批状态）。

```py
# Get AWS Account ID

aws_account_id = boto3.client("sts").get_caller_identity()["Account"]
aws_account_id

# Set Bucket & Instance Type Across Accounts

if aws_account_id=='<insert AWS Account_ID 1>': 
    bucket='<insert s3 bucket name 1>'
    instance_type='ml.t2.medium'
elif aws_account_id=='<insert AWS Account_ID 2>': 
    bucket='<insert s3 bucket name 2>'
    instance_type='ml.t2.medium'
elif aws_account_id=='<insert AWS Account_ID 3>': 
    bucket='<insert s3 bucket name 3>'
    instance_type='ml.m5.large'
training_account_bucket='<insert training account bucket name>'
bucket_path = 'https://s3-{}.amazonaws.com/{}'.format(region,bucket)
```

最初创建和部署 MultiDataModel 的步骤需要在每个新的部署账户中重复进行。

现在我们有一个可以引用 AWS 账户 ID 并可以跨不同 AWS 账户运行的工作笔记本，我们将希望设置一个包含这个笔记本（以及可能包含其他两个用于传承追踪）的 git 仓库，然后在这些账户的 SageMaker Studio 域中克隆该仓库。幸运的是，借助 Studio/Git 集成，这些步骤非常直接/无缝，并在以下 [文档](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-tasks-git.html) 中进行了说明。根据我的经验，建议在 SageMaker Studio 外部创建仓库，并在每个 AWS 账户域中克隆它。

对于笔记本的任何未来更改可以在训练账户中进行，并推送到仓库。然后，通过拉取更改，可以在其他部署账户中反映这些更改。确保创建一个 `.gitignore` 文件，以便仅考虑这三个笔记本，而不是日志或其他文件；其传承将被追踪到 s3。进一步地，应当认识到每次运行笔记本时，控制台输出都会发生变化。为了避免在其他部署账户中拉取文件更改时发生冲突，必须在拉取最新更新之前恢复自上次拉取以来的文件更改。

## *安排重新训练/重新部署笔记本以按设定的节奏运行*

最后，我们可以安排在训练账户中同时运行所有三个笔记本。我们可以使用新的 [SageMaker Studio notebook jobs](https://aws.amazon.com/blogs/machine-learning/operationalize-your-amazon-sagemaker-studio-notebooks-as-scheduled-notebook-jobs/) 功能来实现这一点。调度应被视为环境/账户依赖的——即在部署账户中我们可以创建单独的笔记本作业，但现在只是为了用最新的模型更新端点，并在新批准的模型自动部署到沙盒、QA 和生产账户之间提供一些滞后时间。其优点是，解决方案发布后唯一的手动部分变成了在注册表中的模型审批/拒绝。如果新部署的模型出现问题，可以在注册表中拒绝/删除该模型，然后手动运行端点更新笔记本，以恢复到之前的生产模型版本，从而争取进一步调查的时间。在这种情况下，我们将管道设置为按设定时间间隔运行（例如每月/每季度），尽管此解决方案可以调整为基于条件工作（例如数据漂移或生产模型准确度下降）。

## 结束语

CI/CD 目前是机器学习运维领域的热门话题。这是合理的，因为很多时候对于机器学习解决方案在初次部署后的持续性考虑较少。为了确保生产机器学习解决方案对协变量漂移具有鲁棒性，并且能够在长时间内可持续，需要一个简单而灵活的 CI/CD 解决方案。幸运的是，AWS 在其 SageMaker 生态系统中发布了一系列新功能，使这种解决方案成为可能。本文展示了一种成功实现这一目标的路径，适用于各种量身定制的机器学习解决方案，只需进行一次手动模型验证步骤。

> 感谢阅读！如果你喜欢这篇文章，关注我以获取我新帖的通知。同时，欢迎随时分享你的评论/建议。
