# 仅在另一个 DAG 成功时运行 Airflow DAG

> 原文：[https://towardsdatascience.com/run-airflow-dag-if-another-dag-succeeds-233aaa4118c1?source=collection_archive---------3-----------------------#2023-12-19](https://towardsdatascience.com/run-airflow-dag-if-another-dag-succeeds-233aaa4118c1?source=collection_archive---------3-----------------------#2023-12-19)

## 使用 Airflow 传感器控制不同调度的 DAG 执行

[](https://gmyrianthous.medium.com/?source=post_page-----233aaa4118c1--------------------------------)[![Giorgos Myrianthous](../Images/ff4b116e4fb9a095ce45eb064fde5af3.png)](https://gmyrianthous.medium.com/?source=post_page-----233aaa4118c1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----233aaa4118c1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----233aaa4118c1--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----233aaa4118c1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frun-airflow-dag-if-another-dag-succeeds-233aaa4118c1&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----233aaa4118c1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----233aaa4118c1--------------------------------) · 11分钟阅读 · 2023年12月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F233aaa4118c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frun-airflow-dag-if-another-dag-succeeds-233aaa4118c1&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----233aaa4118c1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F233aaa4118c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frun-airflow-dag-if-another-dag-succeeds-233aaa4118c1&source=-----233aaa4118c1---------------------bookmark_footer-----------)![](../Images/3de6a6755eaa5db34e2a4adb3bcbeffe.png)

图像由 [DALL-E2](https://openai.com/dall-e-2) 生成

最近，我一直在尝试协调两个 Airflow DAG，以便一个 DAG 仅在另一个 DAG（每日运行）成功后按照自己的每小时调度运行。

在今天的教程中，我将引导你通过用例，并展示如何以三种不同的方式实现所需的行为；两种使用 `ExternalTaskSensor`，另一种使用 `PythonOperator` 的定制方法。

# 用例：仅在每日 DAG 成功后运行每小时的 DAG

现在让我们开始探索包含两个 Airflow DAG 的用例。

第一个DAG，`my_daily_dag`，每天在UTC时间凌晨5点运行。

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
) as dag…
```
