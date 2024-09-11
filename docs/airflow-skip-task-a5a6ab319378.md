# 如何跳过 Airflow DAG 中的任务

> 原文：[https://towardsdatascience.com/airflow-skip-task-a5a6ab319378?source=collection_archive---------0-----------------------#2023-02-08](https://towardsdatascience.com/airflow-skip-task-a5a6ab319378?source=collection_archive---------0-----------------------#2023-02-08)

## 根据特定条件跳过 Airflow DAG 中的任务

[](https://gmyrianthous.medium.com/?source=post_page-----a5a6ab319378--------------------------------)[![Giorgos Myrianthous](../Images/ff4b116e4fb9a095ce45eb064fde5af3.png)](https://gmyrianthous.medium.com/?source=post_page-----a5a6ab319378--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a5a6ab319378--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a5a6ab319378--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----a5a6ab319378--------------------------------)

·

[Follow](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fairflow-skip-task-a5a6ab319378&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----a5a6ab319378---------------------post_header-----------) Published in [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a5a6ab319378--------------------------------) ·9 min read·Feb 8, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa5a6ab319378&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fairflow-skip-task-a5a6ab319378&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----a5a6ab319378---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa5a6ab319378&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fairflow-skip-task-a5a6ab319378&source=-----a5a6ab319378---------------------bookmark_footer-----------)![](../Images/881b8b4f1e7bf014f76b61f5f78e7900.png)

照片由 [Hello I’m Nik](https://unsplash.com/@helloimnik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，发布在 [Unsplash](https://unsplash.com/photos/l4ADb9OVqTY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

最近，我试图在现有的 Airflow DAG 中添加一个新任务，该任务仅在特定的星期几运行。然而，我惊讶地发现，跳过 Airflow 中的任务并不像我预期的那样简单。

在本文中，我将演示如何在 Airflow DAG 中跳过任务，特别是关注在使用 `PythonOperator` 或继承自内置操作符（如 `TriggerDagRunOperator`）的操作符时使用 `AirflowSkipException`。

最后，我将讨论 `BranchPythonOperator` 和 `ShortCircuitOperator` 的使用，以及它们如何用来决定任务何时需要跳过。

[](/run-airflow-docker-1b83a57616fb?source=post_page-----a5a6ab319378--------------------------------) [## 如何在本地使用 Docker 运行 Airflow

### 运行 Airflow 和 Docker 的逐步指南

[towardsdatascience.com](/run-airflow-docker-1b83a57616fb?source=post_page-----a5a6ab319378--------------------------------)

现在假设我们有一个由三个任务组成的 Airflow DAG

```py
from datetime import datetime

from airflow import DAG
from airflow.operators.python import PythonOperator

with DAG(
    dag_id='test_dag',
    start_date=datetime(2021, 1, 1),
    catchup=False,
    tags=['example'],
) as dag…
```
