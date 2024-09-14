# 仅在另一个 DAG 成功时运行 Airflow DAG

> 原文：[`towardsdatascience.com/run-airflow-dag-if-another-dag-succeeds-233aaa4118c1`](https://towardsdatascience.com/run-airflow-dag-if-another-dag-succeeds-233aaa4118c1)

## 使用 Airflow 传感器来控制不同计划下 DAG 的执行

[](https://gmyrianthous.medium.com/?source=post_page-----233aaa4118c1--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----233aaa4118c1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----233aaa4118c1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----233aaa4118c1--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----233aaa4118c1--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----233aaa4118c1--------------------------------) ·阅读时间 11 分钟·2023 年 12 月 19 日

--

![](img/3de6a6755eaa5db34e2a4adb3bcbeffe.png)

图片由 [DALL-E2](https://openai.com/dall-e-2) 生成

最近，我一直在尝试协调两个 Airflow DAG，使得其中一个只会在另一个 DAG（每日运行）成功的情况下按自己的小时计划运行。

在今天的教程中，我将引导你了解这个用例，并演示如何通过三种不同的方法实现所需的行为；两种使用`ExternalTaskSensor`，另一种使用`PythonOperator`的自定义方法。

# 使用案例：仅在每日 DAG 成功时运行每小时 DAG

现在，让我们开始处理涉及两个 Airflow DAG 的用例。

第一个 DAG，`my_daily_dag`，每天在 UTC 时间早上 5 点运行。

```py
from datetime import datetime, timedelta
from pathlib import Path

from airflow.models import DAG
from airflow.operators.dummy import DummyOperator

with DAG(
    catchup=False,
    dag_id='my_daily_dag'
    start_date=datetime(2023, 7, 26),
    default_args={
        'owner': 'airflow',
        'retries': 1,
        'retry_delay': timedelta(minutes=2),
    },
    schedule_interval='0 5 * * *',
    max_active_runs=1,
) as dag:
   DummyOperator(task_id='dummy_task')
```

第二个 DAG，`my_hourly_dag`，每小时运行一次，时间在 UTC 的早上 6 点到晚上 8 点之间。

```py
from datetime import datetime, timedelta
from pathlib import Path

from airflow.models import DAG
from airflow.operators.dummy import DummyOperator

with DAG(
    catchup=False,
    dag_id='my_daily_dag'
    start_date=datetime(2023, 7, 26),
    default_args={
        'owner': 'airflow',
        'retries': 1,
        'retry_delay': timedelta(minutes=2),
    },
    schedule_interval='0 6-20 * * *',  # At :00 every hour between 6AM-8PM
    max_active_runs=1,
) as dag:
   DummyOperator(task_id='dummy_task')
```

在我们的使用案例中，我们希望`my_hourly_dag`仅在`my_daily_dag`在当天成功运行的情况下执行。如果没有，则`my_hourly_dag`应该被跳过。这里需要提到的是，我们不想在`my_daily_dag`成功后立刻触发`my_hourly_dag`。那可以通过`TriggerDagRun`操作符实现。相反，我们希望两个 DAG 各自按照自己的计划运行，但在`my_hourly_dag`上添加一个条件。

[](/airflow-skip-task-a5a6ab319378?source=post_page-----233aaa4118c1--------------------------------) ## 如何在 Airflow DAG 中跳过任务

### 基于特定条件跳过 Airflow DAG 中的任务

towardsdatascience.com

在接下来的两个部分中，我们将讨论并演示如何通过几种不同的方法实现这一点。

# 确定两个 DAG 的执行日期

在深入实现细节之前，首先了解两个 DAG 在各自 `execution_date` 方面的区别非常重要。这一点至关重要，因为我们将利用这一知识来确定所需行为的实现方式。

假设今天是 12 月 13 日。每日 DAG `my_daily_dag` 的 `execution_date` 为 `2023–12–12 00:00`，因为它涵盖了 `2023–12–12` 到 `2023–12–13` 之间的数据时间段。**请记住，Airflow DAG 运行从时间段结束时开始。**

与此同时，我们的每小时 `my_hourly_dag` DAG 具有 `execution_date` 为 `2023–12–13`（除了午夜运行，其 `execution_date` 将为 `2023–12–12`，因为该时间段的开始为 `2023–12–12 23:00` 到 `2023–12–13 00:00`）。

# 使用 ExternalTaskSensor

我们的第一个选择是内置的 `[ExternalTaskSensor](https://airflow.apache.org/docs/apache-airflow/stable/_api/airflow/sensors/external_task/index.html#airflow.sensors.external_task.ExternalTaskSensor)` 操作符。

> 等待不同的 DAG、任务组或任务完成特定的逻辑日期。
> 
> 默认情况下，`ExternalTaskSensor` 将等待外部任务成功，此时它也会成功。然而，默认情况下，如果外部任务失败，它将 *不会* 失败，而是继续检查状态，直到传感器超时（从而给你时间重试外部任务而无需同时清除传感器）。
> 
> — [Airflow 文档](https://airflow.apache.org/docs/apache-airflow/stable/_api/airflow/sensors/external_task/index.html#airflow.sensors.external_task.ExternalTaskSensor)

我们可以在 `my_hourly_dag` 中使用此传感器，该传感器将基本上检查 `my_daily_dag` 在指定时间段内是否成功。

`ExternalTaskSensor` 接受 `execution_delta` 或 `execution_date_fn` 之一。前者可用于指示与前一次执行的时间差。默认情况下，此值设置为与当前任务/DAG 相同的逻辑日期。后者接收一个可调用对象（即一个函数），该函数接受当前执行的逻辑日期作为第一个位置参数，并返回要查询的逻辑日期。

> **- execution_delta** (`[*datetime.timedelta*](https://docs.python.org/3/library/datetime.html#datetime.timedelta)` *|* `*None*`) — 与前一次执行的时间差，默认值是与当前任务或 DAG 相同的逻辑日期。对于昨天，请使用 [positive!] `datetime.timedelta(days=1)`。`ExternalTaskSensor` 可以传递 `execution_delta` 或 `execution_date_fn`，但不能同时传递两者。
> 
> **- execution_date_fn** (`*Callable*` *|* `*None*`) — 接收当前执行的逻辑日期作为第一个位置参数的函数，并可选地接收上下文字典中的任何数量的关键字参数，并返回要查询的逻辑日期。`ExternalTaskSensor` 可以传递 `execution_delta` 或 `execution_date_fn`，但不能同时传递两者。

由于两个 DAG 的运行时间表不同，传感器的默认行为对我们不起作用。在前面的部分中，我们澄清了两个 DAG 将具有不同的执行日期的原因。

因此，我们需要弄清楚如何使用 `execution_delta` 或 `execution_date_fn` 来使两个执行日期对齐。

## 使用 ExternalTaskSensor 和 execution_delta

在我看来，最简单的方法是使用 `execution_delta`。我们每日 DAG 的数据间隔开始日期是“昨天的 UTC 时间上午 5 点”。由于我们知道 `my_hourly_dag` 每小时运行一次，因此我们可以提出一个公式来计算小时级 DAG 的间隔开始时间与每日 DAG 的间隔开始时间之间的差异。

以下将创建一个累加的差异：

+   24 对应于两个 DAG 之间的 24 小时差异，前提是它们的运行时间表不同，如前所述。

+   小时级 DAG 的间隔开始时间与每天 DAG 运行的时间 5 之间的差异，即每天 DAG 每天运行的小时。

```py
24 + (hourly_dag_interval_start_hour - 5)
```

作为示例，考虑以下场景，当小时级 DAG 从早上 6 点开始运行（直到晚上 8 点）：

在早上 6 点：

+   小时数据间隔从上午 5 点开始（并于上午 6 点结束）

+   每日数据间隔从昨天的上午 5 点开始。

+   `execution_delta=24 + (5-5) = 24`

+   传感器将检查每日 DAG 的成功情况，其数据间隔开始日期设置为 24 小时之前。

在早上 7 点：

+   小时数据间隔从早上 6 点开始（并于早上 7 点结束）

+   每日数据间隔从昨天的上午 5 点开始。

+   `execution_delta=24 + (6-5) = 25`

+   传感器将检查每日 DAG 的成功情况，其数据间隔开始日期设置为 25 小时之前。

等等。

那么我们该如何实现呢？我们需要面对一个问题是（在本文撰写时），`execution_delta` 不是一个模板字段，这意味着我们不能使用提供有用信息的 [模板变量](https://airflow.apache.org/docs/apache-airflow/stable/templates-ref.html#variables)，包括 `data_interval_start`。

因此，我们将必须手动构造小时级 DAG 的 `data_interval_start`。鉴于 DAG 每小时运行一次，数据间隔开始小时对应于当前小时减去一小时。

```py
from datetime import datetime, timezone

datetime.now(timezone.utc).hour - 1
```

因此，`execution_delta` 作为参数提供给 `ExternalTaskSensor` 现在可以定义为：

```py
execution_delta=timedelta(hours=24 + datetime.now(timezone.utc).hour - 1 - 5)
```

这是我们小时级 DAG 的完整代码，该 DAG 将在 UTC 时间早上 6 点到晚上 8 点之间每小时运行一次，前提是每日 DAG 今天已经成功。

```py
from datetime import datetime, timedelta, timezone
from pathlib import Path

from airflow.models import DAG
from airflow.operators.dummy import DummyOperator
from airflow.sensors.external_task import ExternalTaskSensor

with DAG(
    catchup=False,
    dag_id='my_daily_dag'
    start_date=datetime(2023, 7, 26),
    default_args={
        'owner': 'airflow',
        'retries': 1,
        'retry_delay': timedelta(minutes=2),
    },
    schedule_interval='0 6-20 * * *',  # At :00 every hour between 6AM-8PM
    max_active_runs=1,
) as dag:
    sensor_task = ExternalTaskSensor(
        task_id='daily_dag_completed_successfully',
        external_dag_id='my_daily_dag',
        soft_fail=True,
        check_existence=True,
        execution_delta=timedelta(hours=24 + datetime.now(timezone.utc).hour - 1 - 5),
        poke_interval=30,
        timeout=120,
    )

    dummy_task = DummyOperator(task_id='dummy_task')

    sensor_task >> dummy_task
```

## 使用 ExternalTaskSensor 和 execution_date_fn

现在，除了 `execution_delta` 之外，传感器还可以配置为与 `execution_date_fn` 一起使用，该函数接受一个可调用对象，返回要查询的逻辑日期。

换句话说，我们需要创建一个函数，并获取每日 DAG 所需的逻辑日期，以便与传感器的条件相匹配，该条件默认会检查指定间隔的 DagRun 状态是否成功。

以下函数将获取日常 DAG 的 DagRuns，并仅在它发生在与每小时 DAG 相同的日期时返回 `DagRun` 的执行日期。如果未找到 DagRun（这意味着日常 DAG 在过去未执行），将引发 `AirflowSkipException`，以便跳过传感器任务（以及任何下游任务）。同样，如果没有找到与每小时 DAG 相同日期的日常 DAG 的 DagRun，将返回 `current_logical_dt`，这本质上是由 `ExternalTaskSensor` 检查的默认值（也是使用 `execution_date_fn` 参数时提供的函数定义中必须存在的参数）。

请记住，这两个 DAG 的调度不同，这意味着它们的 `execution_date` 不同。为了进行适当的比较并确定日常 DAG 是否在每小时 DAG 运行的同一天成功执行，我们需要从每小时 DAG 的执行日期中减去一天。请注意，我们只关心两个 DAG 之间的年份、月份和日期是否相同（在此上下文中我们不太关心时间信息）。

```py
import logging 

from airflow.exceptions import AirflowSkipException
from airflow.models import DagRun

def get_most_recent_dag_run(current_logical_dt):
    dag_id = 'my_daily_dag'
    # Get the historical DagRuns of the daily DAG
    dag_runs = DagRun.find(dag_id=dag_id)

    # Sort DagRuns on descending order such that the first element
    # in the list, corresponds to the latest DagRun of the daily DAG
    dag_runs.sort(key=lambda x: x.execution_date, reverse=True)

    # If the daily DAG was not executed ever before, simply raise an 
    # exception to skip. 
    if not dag_runs:
        logging.info(f'No DAG runs found for {dag_id}. Skipping..')
        raise AirflowSkipException

    # Get the latest DagRun of the daily DAG
    latest_daily_dag_run = dag_runs[0]

    # Subtract one day from hourly's DAG current execution_date in order to 
    # align with the daily DAG's scedule
    current_logical_dt_yesterday = current_logical_dt.subtract(hours=24)

    # if year/month/day of daily's DAG execution_date and hourly's DAG execution_date
    # (minus one day) are the same, it means the daily DAG was executed today. 
    # We therefore return the execution_date of the latest daily DagRun. 
    # It's state (i.e. if successful) will be handled by the sensor and the configuration 
    # we provide to it. 
    if (
        current_logical_dt_yesterday.day == latest_daily_dag_run.execution_date.day
        and current_logical_dt_yesterday.month == latest_daily_dag_run.execution_date.month
        and current_logical_dt_yesterday.year == latest_daily_dag_run.execution_date.year
    ):
        logging.info(f'DAG run was found for {dag_id} today.')
        return latest_daily_dag_run.execution_date

    # Alternatively, return the current execution_date of the hourly DAG
    # This is the default value the sensor would otherwise use, and essentially
    # it means that the sensor won't be triggered given that the intervals between 
    # the daily DAG and the sensor won't align. 
    return current_logical_dt
```

以下是我们使用 `execution_function_fn` 和 `ExternalTaskSensor` 的每小时 DAG 的完整代码。

```py
import logging
from datetime import datetime, timedelta
from pathlib import Path

from airflow.exceptions import AirflowSkipException
from airflow.models import DAG, DagRun
from airflow.operators.dummy import DummyOperator
from airflow.sensors.external_task import ExternalTaskSensor

def get_most_recent_dag_run(current_logical_dt):
    dag_id = 'my_daily_dag'
    # Get the historical DagRuns of the daily DAG
    dag_runs = DagRun.find(dag_id=dag_id)

    # Sort DagRuns on descending order such that the first element
    # in the list, corresponds to the latest DagRun of the daily DAG
    dag_runs.sort(key=lambda x: x.execution_date, reverse=True)

    # If the daily DAG was not executed ever before, simply raise an 
    # exception to skip. 
    if not dag_runs:
        logging.info(f'No DAG runs found for {dag_id}. Skipping..')
        raise AirflowSkipException

    # Get the latest DagRun of the daily DAG
    latest_daily_dag_run = dag_runs[0]

    # Subtract one day from hourly DAG's current execution_date in order to 
    # align with the daily DAG's scedule
    current_logical_dt_yesterday = current_logical_dt.subtract(hours=24)

    # if year/month/day of daily DAG's execution_date and hourly DAG's execution_date
    # (minus one day) are the same, it means the daily DAG was executed today. 
    # We therefore return the execution_date of the latest daily DagRun. 
    # It's state (i.e. if successful) will be handled by the sensor and the configuration 
    # we provide to it. 
    if (
        current_logical_dt_yesterday.day == latest_daily_dag_run.execution_date.day
        and current_logical_dt_yesterday.month == latest_daily_dag_run.execution_date.month
        and current_logical_dt_yesterday.year == latest_daily_dag_run.execution_date.year
    ):
        logging.info(f'DAG run was found for {dag_id} today.')
        return latest_daily_dag_run.execution_date

    # Alternatively, return the current execution_date of the hourly DAG
    # This is the default value the sensor would otherwise use, and essentially
    # it means that the sensor won't be triggered given that the intervals between 
    # the daily DAG and the sensor won't align. 
    return current_logical_dt

with DAG(
    catchup=False,
    dag_id='my_daily_dag'
    start_date=datetime(2023, 7, 26),
    default_args={
        'owner': 'airflow',
        'retries': 1,
        'retry_delay': timedelta(minutes=2),
    },
    schedule_interval='0 6-20 * * *',  # At :00 every hour between 6AM-8PM
    max_active_runs=1,
) as dag:
    sensor_task = ExternalTaskSensor(
        task_id='daily_dag_completed_successfully',
        external_dag_id='my_daily_dag',
        soft_fail=True,
        check_existence=True,
        execution_function_fn=get_most_recent_dag_run,
        poke_interval=30,
        timeout=120,
    )

    dummy_task = DummyOperator(task_id='dummy_task')

    sensor_task >> dummy_task
```

# 使用 PythonOperator

第二种方法涉及一个更为定制的解决方案。更具体地说，我们可以以编程方式找到我们日常 DAG 的最新成功 `DagRun`，并相应地处理操作符的行为。换句话说，如果日常 DAG 的最新成功 `DagRun` 与我们每小时 DAG 的执行日期不一致，则该任务将被跳过（以及下游任务）。

因此，我们可以编写一个函数——类似于我们在前一节中编写的，并作为 `ExternalTaskSensor` 的 `execution_date_fn` 参数使用。

更具体地说，我们需要获取日常 DAG 的 DagRuns，确定今天是否有人成功完成（即每小时 DAG 运行的同一天）。如果没有找到，我们将引发 `AirflowSkipException`，以便跳过每小时 DAG 的执行。在这种情况下，`PythonOperator` 支持模板变量，因此我们将充分利用这一点。

这就是我们的函数的样子：

```py
from airflow.exceptions import AirflowSkipException
from airflow.models import DagRun
from airflow.utils.state import DagRunState

def check_daily_dag_success_today(**kwargs):
    dag_id = 'my_daily_dag'
    # Get the historical DagRuns of the daily DAG
    dag_runs = DagRun.find(dag_id=dag_id)

    # Sort DagRuns on descending order such that the first element
    # in the list, corresponds to the latest DagRun of the daily DAG
    dag_runs.sort(key=lambda x: x.execution_date, reverse=True)

    # If the daily DAG was not executed ever before, simply raise an
    # exception to skip.
    if not dag_runs:
        logging.info(f'No DAG runs found for {dag_id}. Skipping..')
        raise AirflowSkipException

    # Get the latest DagRun of the daily DAG
    latest_daily_dag_run = dag_runs[0]

    # Subtract one day from hourly DAG's current execution_date in order to
    # align with the daily DAG's schedule
    data_interval_start = kwargs['data_interval_start']
    data_interval_start_yesterday = data_interval_start.subtract(hours=24)

    # Check the intervals and the success of the daily DAg's DagRun. If conditions are not met,
    # DAG run should be skipped.
    if not (
        latest_daily_dag_run.state == DagRunState.SUCCESS
        and data_interval_start_yesterday.day == latest_daily_dag_run.execution_date.day
        and data_interval_start_yesterday.month == latest_daily_dag_run.execution_date.month
        and data_interval_start_yesterday.year == latest_daily_dag_run.execution_date.year
    ):
        logging.info(f'No successful DAG run was found for {dag_id} today. Skipping..')
        raise AirflowSkipException

    logging.info(f'Successful DAG run was found for {dag_id} today.')
```

以下是 `my_hourly_dag` DAG 的完整代码，使用 `PythonOperator` 来检查 `my_daily_dag` 的状态：

```py
from datetime import datetime, timedelta
from pathlib import Path

from airflow.exceptions import AirflowSkipException
from airflow.models import DAG, DagRun
from airflow.operators.dummy import DummyOperator
from airflow.operators.python import PythonOperator

def check_daily_dag_success_today(**kwargs):
    dag_id = 'my_daily_dag'
    # Get the historical DagRuns of the daily DAG
    dag_runs = DagRun.find(dag_id=dag_id)

    # Sort DagRuns on descending order such that the first element
    # in the list, corresponds to the latest DagRun of the daily DAG
    dag_runs.sort(key=lambda x: x.execution_date, reverse=True)

    # If the daily DAG was not executed ever before, simply raise an
    # exception to skip.
    if not dag_runs:
        logging.info(f'No DAG runs found for {dag_id}. Skipping..')
        raise AirflowSkipException

    # Get the latest DagRun of the daily DAG
    latest_daily_dag_run = dag_runs[0]

    # Subtract one day from hourly DAG's current execution_date in order to
    # align with the daily DAG's schedule
    data_interval_start = kwargs['data_interval_start']
    data_interval_start_yesterday = data_interval_start.subtract(hours=24)

    # Check the intervals and the success of the daily DAg's DagRun. If conditions are not met,
    # DAG run should be skipped.
    if not (
        latest_daily_dag_run.state == DagRunState.SUCCESS
        and data_interval_start_yesterday.day == latest_daily_dag_run.execution_date.day
        and data_interval_start_yesterday.month == latest_daily_dag_run.execution_date.month
        and data_interval_start_yesterday.year == latest_daily_dag_run.execution_date.year
    ):
        logging.info(f'No successful DAG run was found for {dag_id} today. Skipping..')
        raise AirflowSkipException

    logging.info(f'Successful DAG run was found for {dag_id} today.')

with DAG(
    catchup=False,
    dag_id='my_daily_dag'
    start_date=datetime(2023, 7, 26),
    default_args={
        'owner': 'airflow',
        'retries': 1,
        'retry_delay': timedelta(minutes=2),
    },
    schedule_interval='0 6-20 * * *',  # At :00 every hour between 6AM-8PM
    max_active_runs=1,
) as dag:
   check_task = PythonOperator(
       task_id='check_daily_dag', 
       python_callable=check_daily_dag_success_today,
   )
   dummy_task = DummyOperator(task_id='dummy_task')

   check_task >> dummy_task
```

## 最后的想法..

在今天的教程中，我们讨论了如何处理使用 Airflow 时不同 DAG 之间的依赖关系。更具体地说，我们讨论了如何在一个 DAG 以每小时执行的情况下，仅在另一个按日计划的 DAG 在当天成功执行后运行它。

演示了三种不同的方法。根据你的用例的复杂性，你应该选择最合适且代码更优雅的方法。

[**订阅数据管道**](https://thedatapipeline.substack.com/welcome)**，这是一个专注于数据工程的新闻通讯**
