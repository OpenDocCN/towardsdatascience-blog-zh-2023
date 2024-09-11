# 使用Ray加速时间序列预测的训练，第3部分，共3部分

> 原文：[https://towardsdatascience.com/faster-time-series-forecasting-with-ray-air-distributed-computing-part-3-of-3-632c96974774?source=collection_archive---------10-----------------------#2023-01-24](https://towardsdatascience.com/faster-time-series-forecasting-with-ray-air-distributed-computing-part-3-of-3-632c96974774?source=collection_archive---------10-----------------------#2023-01-24)

## 使用Ray和Ray AIR通过分布式计算更快地训练多个模型

[](https://medium.com/@christybergman?source=post_page-----632c96974774--------------------------------)[![Christy Bergman](../Images/b8431b925cfe7bd0d3a035761fd1e7f8.png)](https://medium.com/@christybergman?source=post_page-----632c96974774--------------------------------)[](https://towardsdatascience.com/?source=post_page-----632c96974774--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----632c96974774--------------------------------) [Christy Bergman](https://medium.com/@christybergman?source=post_page-----632c96974774--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff18ab4254b46&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffaster-time-series-forecasting-with-ray-air-distributed-computing-part-3-of-3-632c96974774&user=Christy+Bergman&userId=f18ab4254b46&source=post_page-f18ab4254b46----632c96974774---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----632c96974774--------------------------------) ·11分钟阅读·2023年1月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F632c96974774&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffaster-time-series-forecasting-with-ray-air-distributed-computing-part-3-of-3-632c96974774&user=Christy+Bergman&userId=f18ab4254b46&source=-----632c96974774---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F632c96974774&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffaster-time-series-forecasting-with-ray-air-distributed-computing-part-3-of-3-632c96974774&source=-----632c96974774---------------------bookmark_footer-----------)![](../Images/0b4cdf6bfa4d1843765d0442ef94d0b7.png)

图片由 [StableDiffusion](https://stablediffusionweb.com/#demo) 绘制，日期为2022年1月5日，查询内容为“绘制一幅图像，展示多种不同的深度神经网络时间序列模型同时训练的样子，风格类似Cy Twombly”。

## 介绍 / 动机

即使在当前生成式 AI（如稳定扩散、ChatGPT）和大语言模型（LLM）时代，**时间序列预测仍然是任何依赖供应链或资源的业务的基础部分。** 例如，它可以用于：

+   [根据地理位置进行库存管理的履行预测](https://www.youtube.com/watch?v=3t26ucTy0Rs)。

+   [不同产品类别的需求预测](https://www.anyscale.com/blog/how-anastasia-implements-ray-and-anyscale-to-speed-up-ml-processes-9x)。

+   [资源利用预测](https://medium.com/kocdigital/using-ray-for-time-series-forecasting-at-scale-3355c9e4e28c) 用于数据中心资源配置。

**所有这些用例的共同点是** [**在不同数据片段上训练多个模型**](https://www.anyscale.com/blog/training-one-million-machine-learning-models-in-record-time-with-ray)**。** 使用分布式计算并行训练、调优和部署成千上万的机器学习模型可能是一项挑战！ 典型的时间序列建模软件本身并不具备分布式功能。

**本博客将展示我开始将预测工作负载转换为分布式计算的技巧。** 我将使用最新的 [Ray v2 API](https://docs.ray.io/en/latest/)，结合 ARIMA 使用 [statsforecast](https://github.com/Nixtla/statsforecast)、[Prophet](https://facebook.github.io/prophet/) 和 [PyTorch Forecasting](https://pytorch-forecasting.readthedocs.io/en/stable/index.html) 库。数据方面，我将使用流行的 [NYC Taxi 数据集](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)，该数据集包含按时间戳和地点记录的历史出租车接送信息。

Ray 是一个开源框架，用于通过分布式计算扩展 AI 工作负载。有关 Ray 的概述，请查看 [Ray 文档](https://docs.ray.io/en/latest/) 或这篇 [介绍博客文章](/modern-parallel-and-distributed-python-a-quick-tutorial-on-ray-99f8d70369b8)。

![](../Images/936ddd06d94b1c8950dcf87f360c7154.png)

[Ray “AIR](https://docs.ray.io/en/latest/ray-air/getting-started.html)”（AI Runtime），自 Ray 2.0 起提供，包括 [Ray Data](https://docs.ray.io/en/latest/data/dataset.html)、[Ray Train](https://docs.ray.io/en/latest/train/train.html)、[Ray Tune](https://docs.ray.io/en/latest/tune/index.html)、[RLlib](https://docs.ray.io/en/latest/rllib/index.html) 和 [Ray Serve](https://docs.ray.io/en/latest/serve/index.html)。图片来源于作者。

本博客分为 4 个部分：

+   使用 Ray Core 多进程进行多模型分布式训练。

+   使用 Ray AIR 进行多模型分布式调优。

+   使用 Ray AIR 进行多模型分布式调优，针对更少但更大的模型。

+   使用 Ray AIR 和 Ray Serve 进行多模型分布式部署。

## 第一部分：使用 Ray Core 多进程进行多模型分布式训练

回到2021年11月，我写了一篇 [博客文章](https://medium.com/towards-data-science/scaling-time-series-forecasting-with-ray-arima-and-prophet-e6c856e605ee) 演示如何在 AWS 云上使用 Ray Core 并行训练多个预测模型（无论是 ARIMA 还是 Prophet）。**从那时起，** [**Ray 多进程**](https://docs.ray.io/en/latest/ray-more-libs/multiprocessing.html) **是一个巨大的改进，使事情比 Ray Core API 更加简单。**

下面是代码大纲。完整的更新代码在 [我的 GitHub](https://github.com/christy/AnyscaleDemos/blob/main/forecasting_demos/Ray_v2/train_prophet_blog.ipynb) 上。首先，让我们从几个导入开始：

```py
import time, dateutil
from typing import Tuple, List
import numpy as np
import pandas as pd
# Import libraries for reading from partitioned parquet.
import pyarrow.parquet as pq
import pyarrow.dataset as pds
# Import forecasting libraries.
import prophet
from prophet import Prophet
# Import Ray's multiprocessing library.
import ray
from ray.util.multiprocessing import Pool 
import tqdm
```

接下来，让我们定义 Python 函数来预处理数据、训练和评估模型。为了更快地了解分布式计算概念，我们将假设时间序列数据已经准备好并根据所需模型拆分为不同的文件。

```py
##########
# STEP 1\. Define a Python function to read and preprocess a file of data. 
##########
def preprocess_data(file_path: str) -> Tuple[pd.DataFrame, np.int32]:

    # Read a single pyarrow parquet S3 file.
    data = pq.read_table(file_path,
           filters=[ ("pickup_location_id", "=", SAMPLE_UNIQUE_ID) ],
           columns=[ "pickup_at", "pickup_location_id", TARGET ])\
          .to_pandas()

    # Transform data.
    data["ds"] = data["pickup_at"].dt.to_period("D").dt.to_timestamp()
    data.rename(columns={TARGET: "y"}, inplace=True)
    data.rename(columns={"pickup_location_id": "unique_id"}, inplace=True)
    data.drop("pickup_at", inplace=True, axis=1)
    unique_id = data["unique_id"][0]
    return data, unique_id

##########
# STEP 2\. Define Python functions to train and evaluate a model on a file of data.
##########
def train_model(file_path: str) -> Tuple[pd.DataFrame, pd.DataFrame, 
    'prophet.forecaster.Prophet', np.int32]:

    # Prepare data from a single S3 file.
    data, unique_id = preprocess_data(file_path)

    # Split data into train, test.
    train_end = data.ds.max() - relativedelta(days=FORECAST_LENGTH - 1)
    train_df = data.loc[(data.ds <= train_end), :].copy()
    test_df = data.iloc[-(FORECAST_LENGTH):, :].copy()

    # Define Prophet model with 75% confidence interval.
    model = Prophet(interval_width=0.75, seasonality_mode="multiplicative")      

    # Train and fit Prophet model.
    model = model.fit(train_df[["ds", "y"]])
    return train_df, test_df, model, unique_id

def evaluate_model(model: 'prophet.forecaster.Prophet', train: pd.DataFrame, 
    valid: pd.DataFrame, input_value: np.int32) -> Tuple[float, pd.DataFrame]:

    # Inference model using FORECAST_LENGTH.
    future_dates = model.make_future_dataframe(
        periods=FORECAST_LENGTH, freq="D")
    future = model.predict(future_dates)

    # Merge in the actual y-values.
    future = pd.merge(future, train[['ds', 'y']], on=['ds'], how='left')
    future = pd.merge(future, valid[['ds', 'y']], on=['ds'], how='left')
    future['y'] = future.y_x.combine_first(future.y_y)
    future.drop(['y_x', 'y_y'], inplace=True, axis=1)
    future['unique_id'] = input_value

    # Calculate mean absolute forecast error.
    temp = future.copy()
    temp["forecast_error"] = np.abs(temp["yhat"] - temp["y"])
    temp.dropna(inplace=True)
    error = np.mean(temp["forecast_error"])
    return error, future

############
# STEP 3\.  Define a calling function which calls all the above functions,
#          and will be called in parallel for every data file.
############
def train__and_evaluate(file_path: str) -> Tuple[pd.DataFrame, 
    'prophet.forecaster.Prophet', pd.DataFrame, float, np.int16]:

    # Read S3 file and train a Prophet model.
    train_df, valid_df, model, unique_id = train_model(file_path)

    # Inference model and evaluate error.
    error, future = evaluate_model(model, train_df, valid_df, unique_id)
    return valid_df, model, future, error, unique_id
```

我们可以直接使用 Ray 核心 API 调用 `[ray.remote](https://docs.ray.io/en/latest/ray-core/walkthrough.html)` 进行并行化，但 Ray 的 [多进程库](https://docs.ray.io/en/latest/ray-more-libs/multiprocessing.html) 作为 Ray 的分布式库之一，使这变得更简单。

下面，将对 `pool` 的调用包装在 `tqdm` 中，可以获得一个很好的进度条来监控进度。在内部，Ray 将任务分配给 Ray 集群中的工作节点，这自动处理如容错和批处理优化等问题。

```py
start = time.time()
# Create a pool, where each worker is assigned 1 CPU by Ray.
pool = Pool(ray_remote_args={"num_cpus": 1})

# Use the pool to run `train_model` on the data, in batches of 1.
iterator = pool.imap_unordered(train__and_evaluate, models_to_train, chunksize=1)

# Track the progress using tqdm and retrieve the results into a list.
results = list(tqdm.tqdm(iterator, total=len(models_to_train)))

# Print some training stats.
time_ray_multiprocessing = time.time() - start
print(f"Total number of models: {len(results)}")
print(f"TOTAL TIME TAKEN: {time_ray_multiprocessing/60:.2f} minutes")
print(type(results[0][0]), type(results[0][1]), type(results[0][2]), 
      type(results[0][3]), type(results[0][4]))
```

![](../Images/fdca6909d17c7ae9f3020f481ada53d7.png)

在我的 MacBook Pro 笔记本电脑（配备 8 个 CPU）上运行上述示例时的截图。Ray 多进程的运行时间大约为半分钟，比串行 Python 快 *3.5 倍，即 300.0% 的加速。使用更大的集群和/或更大的数据可以获得更多加速。图片由作者提供。*

上面，我们可以看到 Ray 作业在不到 1 分钟的时间内训练了 12 个模型。

## 第 2 部分：使用 Ray AIR 进行多模型分布式调优

精明的读者可能已经注意到，在上述部分中，Ray 多进程要求数据已经按模型组织成一个文件。**但如果你的数据尚未按模型组织怎么办？** 使用 Ray AIR，你可以在训练不同模型的同时在同一管道中预处理数据。

另一个问题是，**如果你想同时混合使用来自多个库的算法怎么办？** Ray Tune，作为 Ray AIR 的一部分，允许你运行并行试验，从任何 Python AI/ML 库和超参数中找到最佳算法选择，每个数据段。

以下是预处理数据和自动调整模型的步骤。虽然这些步骤特定于 [Ray AIR](https://docs.ray.io/en/latest/ray-air/getting-started.html) 及其 [API](https://docs.ray.io/en/latest/ray-air/package-ref.html)，但这些步骤通常适用于将串行 Python 转换为分布式 Python：

1.  定义 Python 函数来 `**preprocess**` 一个数据段。

1.  定义 Python 函数来 `**train**` 和 `**evaluate**` 一个数据段上的模型。

1.  定义一个调用函数`**train_models**`，该函数调用所有上述函数，并将为Tune搜索空间中的每个排列并行调用！

    在这个[**train_models**](https://docs.ray.io/en/latest/tune/api_docs/trainable.html#trainable-docs)函数中：

    📖 输入参数必须包括一个配置字典参数。

    📈 调优指标（模型的损失或错误）必须使用`session.report()`计算并报告。

    ✔️ 推荐`Checkpoint`（保存）模型以便于容错和后续部署。

1.  **配置分布式计算缩放**。

1.  **定义Tune搜索空间**的所有训练参数。

1.  （可选）指定超参数搜索策略。

1.  **运行实验**。

下面是我们将添加的附加代码；完整代码在[我的GitHub](https://github.com/christy/AnyscaleDemos/blob/6b1cea50a8c3b75bf9b680e77216f6927bdd2f85/forecasting_demos/Ray_v2/train_prophet_blog.ipynb)上。

+   下面的`preprocess_data`和`train_model`函数与之前完全相同，只是它们接受一个文件列表而不是单个文件。

+   `train_models`函数与`train_and_evaluate`完全相同，只是它接受一个文件列表而不是单个文件。它还训练配置中传递的算法，而不是固定的算法，并进行检查点保存。

```py
import os
num_cpu = os.cpu_count()
# Import another forecasting library.
import statsforecast
from statsforecast import StatsForecast
from statsforecast.models import AutoARIMA
# Import Ray AIR libraries.
from ray import air, tune
from ray.air import session, ScalingConfig
from ray.air.checkpoint import Checkpoint

##########
# STEP 1\. Define Python functions to read and prepare a segment of data.
##########
def preprocess_per_uniqueid(
    s3_files: List[str], sample_location_id: np.int32) -> pd.DataFrame:

    # Load data.
    df_list = [read_data(f, sample_location_id) for f in s3_files]
    df_raw = pd.concat(df_list, ignore_index=True)

    # Transform data.
    df = transform_df(df_raw)
    df.sort_values(by="ds", inplace=True)
    return df

##########
# STEP 2\. Define Python functions to train and evaluate a model on a segment of data.
##########
def train_prophet(s3_files: List[str], sample_unique_id: np.int32,
    model_type: str) -> Tuple[pd.DataFrame, pd.DataFrame, 
                        'prophet.forecaster.Prophet', np.int32]:

    # Prepare data from a list of S3 files.
    data = preprocess_per_uniqueid(s3_files, sample_unique_id)

    # Split data into train, test.
    train_end = data.ds.max() - relativedelta(days=FORECAST_LENGTH - 1)
    train_df = data.loc[(data.ds <= train_end), :].copy()
    test_df = data.iloc[-(FORECAST_LENGTH):, :].copy()

    # Define Prophet model with 75% confidence interval.
    if model_type == "prophet_additive":
        model = Prophet(interval_width=0.75, seasonality_mode="additive")
    elif model_type == "prophet_multiplicative":
        model = Prophet(interval_width=0.75, seasonality_mode="multiplicative")     

    # Train and fit Prophet model.
    model = model.fit(train_df[["ds", "y"]])
    return train_df, test_df, model

# Train an ARIMA model. Full code not shown here.
def train_arima(): 

# Evaluate an ARIMA model. Full code not shown here.
def evaluate_arima(): 

############
# STEP 3\.  Define a calling function `train_models`, which calls all 
#          the above functions, and will be called in parallel for every 
#          permutation in the Tune search space.
############
def train_models(config: dict) -> None:

    # Get Tune parameters
    file_list = config['params']['file_list']
    model_type = config['params']['algorithm']
    sample_unique_id = config['params']['location']

    # Train model.
    if model_type == "arima":
        # Train and fit the Prophet model.
        train_df, valid_df, model = \
            train_arima(file_list, sample_unique_id)
        # Inference model and evaluate error.
        error, future = \
            evaluate_arima(model, valid_df)
    else:
        # Train and fit the Prophet model.
        train_df, valid_df, model = \
            train_prophet(file_list, sample_unique_id, model_type)
        # Inference model and evaluate error.
        error, future = evaluate_model(model, train_df, valid_df, sample_unique_id)

    # Define a model checkpoint using AIR API.
    checkpoint = ray.air.checkpoint.Checkpoint.from_dict({
          "model": model,
          "valid_df": valid_df,
          "forecast_df": future,
          "location_id": sample_unique_id,
        })
    metrics = dict(error=error)
    session.report(metrics, checkpoint=checkpoint)

############
# STEP 4\. Customize distributed compute scaling.
############
num_training_workers = min(num_cpu - 2, 32)
scaling_config = ScalingConfig(
    # Number of distributed workers.
    num_workers=num_training_workers,
    # Turn on/off GPU.
    use_gpu=False,
    # Specify resources used for trainer.
    trainer_resources={"CPU": 1},
    # Try to schedule workers on different nodes.
    placement_strategy="SPREAD")

############
# STEP 5\. Define a search space dict of all config parameters.
############
search_space = {
    "scaling_config": scaling_config,
    "params": {
        "file_list": tune.grid_search([files_to_use]),
        "algorithm": tune.grid_search(algorithms_to_use),
        "location": tune.grid_search(models_to_train),
    },
}

# Optional STEP 6\. Specify the hyperparameter tuning search strategy.

##########
# STEP 7\. Run the experiment with Ray AIR APIs.
##########
# Define a tuner object.
tuner = tune.Tuner(
        train_models,
        param_space=search_space,
        tune_config=tune.TuneConfig(
            metric="error",
            mode="min",
        ),
        run_config=air.RunConfig(
            # Redirect logs to relative path instead of default ~/ray_results/.
            local_dir="my_Tune_logs",
            # Specify name to make logs easier to find in log path.
            name="tune_nyc",
        ),
    )
# Fit the tuner object.
results = tuner.fit()
```

![](../Images/0256c490f5411ce2ac7c0261144e0f9f.png)

**顶部**：Ray Tune试验状态的截图，显示了768个候选模型的错误（每256个NYC出租车接送位置有3种算法选择）。在不到45分钟内完成训练。**左下**：Prophet模型推断+预测图（实际值为黑点，带置信区间的预测为蓝色）用于NYC出租车接送位置=165。**右下**：ARIMA模型推断+预测图（实际值为蓝色，预测为橙色）用于NYC出租车接送位置=237。所有模型使用Anyscale在10节点的AWS集群上训练，集群包括[ m5.4xlarges](https://aws.amazon.com/ec2/instance-types/m5/)工作节点和一个m5.2xlarge头节点。图像由作者提供。

在上述截图中，自2018年1月以来的数据被分组并汇总到日级别。我曾尝试在SageMaker上完成这项工作，仅数据处理就花费了太长时间，更不用说同时调优如此多的模型了。

## 第三节：多模型分布式调优（更大的PyTorch模型）

目标通常是创建一些更大的模型，例如按地理区域划分的模型，其中只有少数几个这样的区域。一年前，即2021年12月，我写了一篇[博客文章](https://medium.com/towards-data-science/how-to-train-time-series-forecasting-faster-using-ray-part-2-of-2-aacba89ca49a)，展示了如何使用Ray Lightning训练更大的PyTorch预测模型。**自那时以来，一个重大改进是，感谢** [**Anyscale Workspaces**](https://docs.anyscale.com/user-guide/develop-and-debug/workspaces)**，笔记本电脑和云之间的代码开发切换更加无缝。**

这些较大的模型有时被称为“全球模型”，因为只有一个深度神经网络模型在多个不同时间序列上进行训练，而不是每个时间序列一个模型（Prophet 或 ARIMA）。

请查看 [我的 GitHub](https://github.com/christy/AnyscaleDemos/blob/main/forecasting_demos/Ray_v2/ray_air/pytorch_forecasting.ipynb) 以获取完整的 [PyTorch Forecasting](https://pytorch-forecasting.readthedocs.io/en/stable/index.html) 代码，其中展示了最新的 Ray AIR API 与 [Ray Lightning](https://docs.ray.io/en/latest/tune/examples/tune-pytorch-lightning.html)。你需要将集群 ID 添加到数据中，然后调优步骤与第 2 节中看到的相同：

```py
# Import forecasting libraries.
import torch
import pytorch_lightning as pl
import pytorch_forecasting as ptf 
import tensorboard as tb  
# Import ray libraries.
import ray_lightning
from ray_lightning import RayStrategy
from ray_lightning.tune import get_tune_resources, TuneReportCheckpointCallback
from ray import air, tune
from ray.tune.schedulers import ASHAScheduler

# Define a tuner object.
tuner = tune.Tuner(
        tune.with_resources(
            train_with_parameters,
            resources=get_tune_resources(num_workers=num_training_workers),
        ),
        tune_config=tune.TuneConfig(
            metric="loss",
            mode="min",
            scheduler=scheduler,
        ),
        run_config=air.RunConfig(
            # Redirect logs to relative path instead of default ~/ray_results/.
            local_dir="my_Tune_logs",
            # Specify name to make logs easier to find in log path.
            name="ptf_nyc",
        ),
        param_space=FORECAST_CONFIG,
    )

# Fit the tuner object.
results = tuner.fit()

# Get checkpoint for best model from results object, code not shown.

# Plot inference forecasts for some unique_ids.
some_unique_ids = [25, 41, 14, 24, 4]
for idx in some_unique_ids:
    best_model.plot_prediction(x, raw_predictions, idx=idx)
```

![](../Images/dc844e0d7e1c866b57556638403bbad8.png)

**顶部**。在调优六个 PyTorch Forecasting TemporalFusionTransformer 模型时截图 Ray Tune Trial 状态。（3 个学习率，2 个纽约市出租车位置簇）。总运行时间少于 2 分钟。在 2 节点的 AWS 集群（m5.4xlarge 工作节点和一个 m5.2xlarge 主节点）上运行，时间不超过 2 分钟。**底部**：使用单个模型对多个出租车接客地点进行的推断预测图。

请注意，与 ARIMA 和 Prophet 模型（每个 unique_id 一个模型）不同，这些较大的模型每次包含对多个 unique_ids 的推断。

## 第 4 节：使用 Ray AIR 和 Ray Serve 进行多模型分布式部署

在部署之前，你必须决定你的部署是需要一个在线、始终运行的 http 服务，还是离线的（按需调用的 Python 服务）。下面，我演示了如何使用新的 [Ray AIR Predictors](https://docs.ray.io/en/latest/ray-air/predictors.html) 和 [Ray Serve](https://docs.ray.io/en/latest/serve/index.html) 进行离线部署。

离线部署的步骤是：

**步骤 1**。使用 Ray AIR 检查点实例化一个批量预测器。

**步骤 2**。创建一些测试数据。

**步骤 3**。运行 `batch_predictor.predict(test_data)`。

将上述步骤 3 替换为自定义预测器的以下步骤：

**步骤 3**。通过使用 Ray 装饰器 `@serve.deployment` 定义一个 Ray Serve 部署类。

**步骤 4**。部署预测器。

**步骤 5**。查询部署并获取结果。

上述步骤 3–5 仅在使用自定义预测器（如 ARIMA、Prophet 或 PyTorch Forecasting）时需要。

否则，对于集成了 Ray AIR 的 ML 库（HuggingFace transformers、PyTorch、TensorFlow、Scikit-learn、XGBoost 或 LightGBM），你只需调用 `batch_predictor.predict(test_data)` 即可。

继续上一节中关于 PyTorch Forecasting 的示例，下面是部署代码。完整代码在 [我的 GitHub](https://github.com/christy/AnyscaleDemos/blob/main/forecasting_demos/Ray_v2/ray_air/pytorch_forecasting.ipynb) 上。

```py
import pickle
import numpy as np
import pandas as pd
import pyarrow
import pyarrow.parquet as pq
# Import forecasting libraries. 
import torch
import pytorch_lightning as pl
import pytorch_forecasting as ptf
# Import ray libraries.
import ray
from ray import serve

##########
# STEP 1\. Instantiate a batch predictor from checkpoint.
##########
batch_predictor = ptf.models.TemporalFusionTransformer.load_from_checkpoint(model_path)

##########
# STEP 2\. Create some test data. 
##########
# Being lazy, pretend the last test data is our out-of-sample test data.
max_prediction_length = FORECAST_CONFIG['forecast_horizon']
new_prediction_data = df.copy()
new_prediction_data["time_idx"] = new_prediction_data["time_idx"] + max_prediction_length
# Convert data from pandas to PyTorch tensors.
_, _, test_loader = convert_pandas_pytorch_timeseriesdata(
    new_prediction_data, FORECAST_CONFIG)

##########
# STEP 3\. Define a Ray Serve deployment class.
##########
@serve.deployment
class ForecastPredictor:
    def __init__(self, predictor, test_data):
        self.predictor = predictor
        self.test_data = test_data

    def predict(self):
        raw_predictions, x = \
          self.predictor.predict(self.test_data, mode="raw", return_x=True)
        return x, raw_predictions

    def __call__(self):
        x, raw_predictions = self.predict()
        return [x, raw_predictions]

##########
# STEP 4\. Deploy the predictor.
##########
# Bind arguments to the Class constructor.
my_first_deployment = ForecastPredictor.bind(
    predictor=batch_predictor,
    test_data=test_loader)

##########
# STEP 5\. Query the deployment and get the result.
##########
# Get handle from serve.run().
handle = serve.run(my_first_deployment)

# ray.get() the results from the handle.
ray_return = ray.get(handle.remote())
new_x = ray_return[0]
new_pred = ray_return[1]
```

![](../Images/352284bfab1b2d33c66eb92fe5f433e5.png)

**左侧**：Ray 仪表板的截图（默认在`localhost:8265`的主节点上访问）在服务期间的情况。**右侧**：Ray 仪表板在运行上述示例时的截图。你可以看到在自动缩放中有 5 个峰值，因为在部署最终训练模型之前，我对训练代码进行了 5 次不同的迭代。Anyscale 运行在 3 节点 AWS 集群上，其中包括[ m5.4xlarges](https://aws.amazon.com/ec2/instance-types/m5/) 工作节点和 1 个 m5.2xlarge 主节点。图片由作者提供。

上述右侧的截图展示了在训练和服务期间 Ray 集群的可观察性。如果你需要对预测结果进行后处理，我在[这个笔记本的末尾有一个示例](https://github.com/christy/AnyscaleDemos/blob/main/forecasting_demos/Ray_v2/train_prophet_blog.ipynb)。

## 结论

总之，本博客展示了如何使用开源 Ray 在分布式计算中并行训练和调整多个模型。模型不必都是相同类型的，可以从任何 AI/ML Python 库中混合匹配。

Ray AIR API 清晰、直观，并隐藏了许多分布式计算的复杂性，因此可以轻松完成许多复杂的任务，如早期停止、ASHA 调度、检查点和部署。

进一步学习：

+   阅读[Ray 文档](https://docs.ray.io/en/latest/)，获取详细的解释和示例。

+   在[Slack](https://docs.google.com/forms/d/e/1FAIpQLSfAcoiLCHOguOm8e7Jnn-JJdZaCxPGjgVCvFijHB5PLaQLeig/viewform)和[Discuss](https://discuss.ray.io/)上提问。

+   使用[Anyscale](https://docs.anyscale.com/user-guide/develop-and-debug/workspaces#workspaces-tutorial)，这使得启动集群并在云上运行你的代码变得简单（获取邀请码[这里](https://www.anyscale.com/signup)）。
