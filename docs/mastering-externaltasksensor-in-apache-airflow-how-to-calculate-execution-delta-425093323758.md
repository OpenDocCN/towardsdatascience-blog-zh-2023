# 掌握 Apache Airflow 中的**ExternalTaskSensor**：如何计算执行增量

> 原文：[`towardsdatascience.com/mastering-externaltasksensor-in-apache-airflow-how-to-calculate-execution-delta-425093323758`](https://towardsdatascience.com/mastering-externaltasksensor-in-apache-airflow-how-to-calculate-execution-delta-425093323758)

## 外部任务传感器阻止坏数据在数据管道中流入下游。利用它们来创建可靠的数据基础设施。

[](https://casey-cheng.medium.com/?source=post_page-----425093323758--------------------------------)![Casey Cheng](https://casey-cheng.medium.com/?source=post_page-----425093323758--------------------------------)[](https://towardsdatascience.com/?source=post_page-----425093323758--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----425093323758--------------------------------) [Casey Cheng](https://casey-cheng.medium.com/?source=post_page-----425093323758--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----425093323758--------------------------------) ·15 分钟阅读·2023 年 5 月 8 日

--

![](img/6325cbd704ec069b44ecc8ece0402ada.png)

外部任务传感器就像守门员——它们阻止坏数据流入下游。[图片](https://www.freepik.com/free-photo/arrangement-financial-crisis-with-wooden-pieces_11433457.htm#query=domino%20effect%20stop&position=46&from_view=keyword&track=ais)由 Freepik 提供。

协调数据管道是一项微妙的工作。在数据管道中，我们可能会有数千个任务同时运行，并且这些任务通常相互依赖。如果不小心，单点故障可能会产生多米诺骨牌效应，流入下游并搞乱整个管道。

Apache Airflow 引入了外部任务传感器，以解决这些问题。虽然这是一个极其强大的功能，但也伴随着一定的复杂性。

在这篇介绍性文章中，我希望解开一些关于外部任务传感器的困惑，并展示我们如何使用它来增强数据管道的可靠性——让传感器变得有意义！

+   我们为什么需要外部任务传感器？

+   外部任务传感器有什么作用？

+   我们如何创建外部任务传感器？

+   什么是执行增量和执行日期函数？

    – 如何计算执行增量？

    – 如何计算执行日期函数？

+   我们如何将外部任务传感器融入到我们的 DAG 中？

+   附加信息：Airflow 中的日期概念

# 我们为什么需要外部任务传感器？

认识 Jamie，她是 Airflow Bakery 的一个新手厨师。她刚刚加入。她唯一的责任是每小时制作一批新的饼干面团。

![](img/13fdb464679bc75b3f7af3cccdc5793a.png)

杰米的职责以“DAG”格式展示。 [Chef (F)](https://www.flaticon.com/free-icon/chef_8840999) 图标由 Freepik 提供。

然后我们有戈登·丹姆斯，饼干大师。戈登从杰米那里拿到面团，并将其制作成获奖的饼干。

![](img/c5d53c21d5515b7fbdc85e71c416702c.png)

戈登的职责以“DAG”格式展示。 [Chef (M)](https://www.flaticon.com/free-icon/baker_817282?related_id=817282&origin=search) 图标由 Freepik 提供。

一天，戈登急忙去找最新鲜的面团来烘焙饼干。但当他咬一口时，哎呀！“坏”只是个轻描淡写的说法。戈登很快发现根本原因是面团陈旧，是一周前剩下的。

戈登显得很沮丧，把饼干扔进了垃圾桶。调整好情绪后，他慢慢转向杰米，问道：“为什么面团不新鲜？”

“我不得不停止制作它们，厨师。原料有问题，”杰米回答道，试图在戈登的愤怒面前保持冷静。不幸的是，坏饼干已经送给了客户，他们不再信任面包店的食品质量。

这次小小的偏离是关于验证数据源新鲜度重要性的警示故事。在故事中，戈登的成功依赖于杰米，但他们独立工作而没有互相沟通。他们“相信”对方会完美地完成自己的工作。但正如任何数据从业者所知道的那样，数据管道中一切可能出错的事情*都会*出错。

理想情况下，戈登应该检查杰米是否最近制作了面团。一旦确认，这意味着面团是新鲜的，他可以继续烘焙饼干。否则，停止烘焙并找出问题所在。

你看，戈登需要的是一个*外部任务传感器*。

# 外部任务传感器的作用是什么？

外部任务传感器检查其他人是否完成了分配的任务。它***感知***到***外部任务***的完成，因此得名。

在 Airflow 的背景下，杰米和戈登是 DAG。他们有具体的任务需要完成。

当我们添加一个外部任务传感器时，它会成为协调两个独立 DAG 之间的中介。传感器会在特定时间检查杰米是否完成了她的任务。

如果杰米成功完成了她的任务，传感器将通知戈登，以便他可以继续进行下游任务。

![](img/87d7f0dc1e3861d1c0cf946493ac62c0.png)

外部任务传感器 — check_dough() 在验证 make_dough() 成功运行后返回成功。 [Chef (F)](https://www.flaticon.com/free-icon/chef_8840999) 和 [Chef (M)](https://www.flaticon.com/free-icon/baker_817282?related_id=817282&origin=search) 图标由 Freepik 提供。

如果杰米未能完成她的任务，传感器会阻止戈登执行任何依赖于失败任务的任务。

![](img/47b6b5e65941e627db1483c61e3fe57f.png)

外部任务传感器 — check_dough() 在验证 make_dough() 未成功运行后返回失败。[厨师 (F)](https://www.flaticon.com/free-icon/chef_8840999) 和 [厨师 (M)](https://www.flaticon.com/free-icon/baker_817282?related_id=817282&origin=search) 图标由 Freepik 提供。

拥有这层额外的验证本质上可以防止陈旧数据进一步传递到下游，并污染我们管道中的其他部分，导致数据脏乱且不准确。

# 我们如何创建外部任务传感器？

Airflow 使创建外部任务传感器变得非常简单——只需导入它们即可。语法大致如下：

```py
from airflow.sensors.external_task import ExternalTaskSensor

ext_task_sensor = ExternalTaskSensor(
    dag=gordon_tasks,
    task_id='check_dough_freshness',
    external_dag_id='jamie_tasks',
    external_task_id='make_new_dough',
    email=['gordon.damnsie@gmail.com', 'jamie@gmail.com'],
    execution_delta=timedelta(minutes=30),
    # execution_date_fn=my_function,
    timeout=1800,
    poke_interval=300,
    mode='reschedule'
)
```

它们的含义如下：

1.  `**dag**` 是当前的 DAG 对象。由于 Gordon 是想检查 Jamie 是否做了面团的人，因此这应该指向 Gordon 的 DAG。

1.  `**task_id**` 是这个外部任务传感器的唯一名称。

1.  `**external_dag_id**` 是你要检查的 DAG 的名称。在这种情况下，是 Jamie 的 DAG。

1.  `**external_task_id**` 是你要检查的具体任务的名称。理想情况下，我们应始终指定这一点。否则，传感器将检查 *整个 DAG* 的完成情况，而不仅仅是一个特定任务。换句话说，Gordon 不会做任何事，直到 Jamie 完成切洋葱、洗碗和补充食品储备，即使我们只想知道她是否做了面团。更糟的是，如果这些无关任务中的任何一个失败，传感器将不必要地暂停整个管道。

1.  `**email**` 是你希望 Airflow 在外部任务传感器失败时通知的人员名单。请记住，要使其正常工作，你需要在 Airflow 配置文件中正确配置 SMTP 设置。

1.  `**execution_delta**` 可以说是外部任务传感器中最令人困惑但也是最重要的部分。因此，我为此专门设置了一个部分 如下。继续滚动！

1.  `**execution_date_fn**` 和执行增量非常相似。我们一次只能使用其中一个。有时候，使用这个比执行增量更方便。我也将其单独列出 如下。

1.  `**timeout**` 限制传感器能够存在的时间。当我们创建一个传感器时，它会占用一个工作槽，从而消耗资源。如果目标任务永远无法完成，这些传感器将无限期地继续检查，同时占用工作槽。随着时间的推移，我们可能会遇到一个 [传感器死锁](https://marclamberti.com/blog/airflow-sensors/#The_Deadlock)，所有工作槽都被无用的传感器占用，任务无法再运行。因此，设置检查的最大时间限制是最佳实践。

1.  `**poke_interval**` 是在传感器再次检查之前的持续时间，如果之前的检查失败。其理由是我们不希望传感器像疯子一样过度检查，因为这会给服务器增加不必要的负担。另一方面，检查过于频繁意味着传感器会比必要时等待更长时间，从而延迟管道。诀窍是根据外部任务的预期运行时间找到最佳点。

1.  `**mode**` 是我们希望传感器的行为方式。它可以设置为“poke”或“reschedule”。

    当设置为“poke”时，传感器在失败时进入休眠状态，并在下一个 poke 间隔时重新唤醒尝试。这就像处于待机模式。传感器将更具反应性，但由于它处于待机状态，工人插槽在整个过程中都会被占用。

    当设置为“reschedule”时，传感器会检查一次。如果检查失败，传感器会安排在稍后的时间进行另一次检查，但现在会终止自身，释放工人插槽。如果 poke 间隔大于 60 秒，Airflow 推荐使用“reschedule”。

好了，这就是我们需要了解的外部任务传感器的所有参数。虽然这个列表并不详尽，但了解这 10 个参数对于我们在几乎所有用例中正确设置外部任务传感器已经足够了。

为了完整性，我将包括 [Airflow 官方文档](https://airflow.apache.org/docs/apache-airflow/stable/_api/airflow/sensors/external_task/index.html) 供那些渴望更详细了解的人参考。

# 执行时间差和执行日期函数是什么？

在上面的部分中，我略过了这两个参数，因为它们可以说是外部任务传感器中最臭名昭著、最烦人且最令人困惑的部分。但我认为现在是时候解决它们了。

那么 `execution_delta` 和 `execution_date_fn` 是什么呢？

基于我们的类比，`external_task_id` 告诉传感器检查 Jamie 是否完成了 `make_dough()` 任务。但她做面团的频率很高——每小时一次。我们是在检查她在过去一小时、昨天还是上周是否做过面团？

这种模糊性使得外部任务传感器感到困惑，这也是 Airflow 提供了两种方式来传达这些信息的原因。`execution_delta` 和 `execution_date_fn` 都旨在告知传感器任务的具体时间。

1.  `execution_delta` 以 *相对* 时间为基础，举例来说：“Jamie 是否在 30 分钟前烘烤过？” 它接受一个 `datetime.timedelta` 对象作为参数，例如：`datetime.timedelta(minutes=30)`。

1.  `execution_date_fn` 以 *绝对* 时间为基础，举例来说：“Jamie 是否在 2023 年 5 月 3 日下午 4:30 烘烤过？” 它接受一个可调用的 Python 函数作为参数。这个函数应该返回我们想要检查的任务的 *执行日期*，例如：`datetime.datetime(year=2023,month=5,day=3,hour=4,minute=30)`。

由于这两者传达了相同的信息，**Airflow 仅允许我们使用其中一个，而不能同时使用两个**。

我通常使用`execution_delta`作为默认选择。但在某些情况下，计算`execution_delta`可能过于复杂。在这种情况下，我会使用`execution_date_fn`。

## 如何计算 execution_delta？

词语`execution_delta`是*delta*（即差异）和*execution dates*（即任务的上次运行时间）的缩写。

![](img/5f81154e7de3aecf803d0e49f60526b1.png)

execution_delta 的公式。图片由作者提供。

我想强调这里的关键词——“previous”。

有些人可能会想… 为什么 Airflow 需要之前运行的时间差，而不是当前运行的时间差？这在我刚开始使用 Airflow 时曾让我非常困惑。

原来有一个完全合理的原因。然而，我不想偏离当前话题，因此我会在后面的部分（这里）中包含它。现在，我们就按原样接受公式，并看看如何应用它。

假设 Jamie 每小时做一次面团（例如：13:00 pm, 14:00 pm, 15:00 pm，…）。Gordon 也每小时做一次饼干，但他在每小时的第 30 分钟做（例如：13:30 pm, 14:30 pm, 15:30 pm，…）。

在 14:30 pm 整点，Gordon 准备好烘烤饼干。在开始之前，他需要检查 Jamie 是否最近做了新鲜面团。`make_dough()`的最新运行时间将是 14:00 pm。

![](img/b636f935d6ebb4104aa91f6ac3e89276.png)

这条时间序列展示了 Jamie 和 Gordon 之间的任务依赖关系。Gordon 总是检查 Jamie 是否在半小时前完成了任务。[Chef (F)](https://www.flaticon.com/free-icon/chef_8840999) 和 [Chef (M)](https://www.flaticon.com/free-icon/baker_817282?related_id=817282&origin=search)图标由 Freepik 提供。

由于 Gordon 和 Jamie 的任务是按小时安排的，他们在 14:30 pm 运行时的执行日期（即之前的运行）将是…

+   Gordon 的执行时间 = 14:30 pm — 1 小时 = 13:30 pm

+   Jamie 的执行时间 = 14:00 pm — 1 小时 = 13:00 pm

我们可以将这些值代入公式中，瞧！

![](img/12d7bc1785b7cdc9a8a26a449c7a7bf2.png)

`execution_delta`的结果是`datetime.timedelta(minute=30)对于一次特定运行`。图片由作者提供。

你可以对任务的不同运行进行相同的计算，以获取各自的`execution_delta`。

![](img/737ce6ba54a829924e72fa42439efd98.png)

在计算 execution delta 时，将其以这种格式排列是有帮助的。我们想要计算多个运行的执行 delta，而不仅仅是一个，以确保它们都相同！图片由作者提供。

在这个（挑选的）示例中，所有的`execution_delta`都恰好相同。我们可以将其传递给我们的 External Task Sensor，一切都会正常工作。

```py
from airflow.sensors.external_task import ExternalTaskSensor

ext_task_sensor = ExternalTaskSensor(
    dag=gordon_tasks,
    task_id='check_dough_freshness',
    external_dag_id='jamie_tasks',
    external_task_id='make_new_dough',
    email=['gordon.damnsie@gmail.com', 'jamie@gmail.com'],
    execution_delta=timedelta(minutes=30),  # Pass the execution delta here
    timeout=1800,
    poke_interval=300,
    mode='reschedule'
)
```

但是-!

`execution_delta`*有时*可能不同。这通常发生在两个 dags 的调度间隔不同（例如：每日 vs 每周，每日 vs 每月，…）时。

例如，假设 Jamie 每周 *一次* 在星期天 14:00 制作面团，而 Gordon 每天 *一次* 在 14:30 制作饼干。

![](img/49521d0576017cd2b11926d4c783e8ae.png)

Jamie 的任务和 Gordon 的传感器之间的箭头表示执行增量。执行增量在一周内变得更长，直到在星期天再次重置。[Chef (F)](https://www.flaticon.com/free-icon/chef_8840999) 和 [Chef (M)](https://www.flaticon.com/free-icon/baker_817282?related_id=817282&origin=search) 图标由 Freepik 提供。

如果我们进行相同的计算，你会看到每次运行的执行增量都不同。

![](img/7017f1f0bc8e39608c9b438209c6172c.png)

请注意，不同运行的执行增量可能会有所不同。图片来源：作者。

这成为一个问题，因为 `execution_delta` 只接受单一的 `datetime` 对象作为其参数。我们不能为每次运行输入不同的 `execution_delta` 值。

在这种情况下，我们需要 `execution_date_fn`。

## 如何计算执行日期函数？

`execution_date_fn` 只是一个普通的 Python 函数。像所有 Python 函数一样，它接收一些参数并返回一些输出。但使用函数的优点在于能够根据函数的输入和逻辑返回不同的输出。

对于 `execution_date_fn`，Airflow 将 **当前任务的执行日期** 作为参数传递，并期望函数返回 **外部任务的** **执行日期**。请注意，这些执行日期需要 **以 UTC 时间表示**。

```py
def my_exec_date_fn(gordon_exec_date):
    # Add your logic here.
    return jamie_exec_date

ext_task_sensor = ExternalTaskSensor(
    dag=gordon_tasks,
    task_id='check_dough_freshness',
    external_dag_id='jamie_tasks',
    external_task_id='make_new_dough',
    email=['gordon.damnsie@gmail.com', 'jamie@gmail.com'],
    execution_date_fn=my_exec_date_fn,  # Pass the function here.
    timeout=1800,
    poke_interval=300,
    mode='reschedule'
)
```

根据我们之前的案例研究，我们的 `execution_date_fn` 需要执行以下操作…

![](img/165565eba9b9431153c3b7cecbc6b585.png)

我的 Airflow 配置为本地时间（GMT+8），所以我需要减去 8 小时来获得 UTC 时间。图片来源：作者。

一种简单的方法是硬编码每一个运行，直到时间的尽头。

```py
# The naive way (This is a bad practice. Don't do this.)
def my_exec_date_fn(gordon_exec_date):
    if gordon_exec_date == datetime(year=2023,month=3,day=14,hour=6,minute=30):
        jamie_exec_date = datetime(year=2023,month=3,day=5,hour=6,minute=0)
    elif gordon_exec_date == datetime(year=2023,month=3,day=15,hour=6,minute=30):
        jamie_exec_date = datetime(year=2023,month=3,day=5,hour=6,minute=0)
    elif gordon_exec_date == datetime(year=2023,month=3,day=16,hour=6,minute=30):
        jamie_exec_date = datetime(year=2023,month=3,day=5,hour=6,minute=0)
    elif gordon_exec_date == datetime(year=2023,month=3,day=17,hour=6,minute=30):
        jamie_exec_date = datetime(year=2023,month=3,day=5,hour=6,minute=0)
    ...

    return jamie_exec_date
```

这有效，但显然不是最有效的方法。

更好的方法是寻找一致的模式，并利用这些模式程序性地推导输出。通常，寻找模式的好地方是 `execution_delta`，因为它包含执行日期之间的关系（我们在这里讨论过这个问题）。

此外，我们还可以查看 `datetime` 属性，如星期几。如果我们仔细考虑一下，我们的 External Task Sensor 将总是指向星期天，因为 Jamie 只在星期天制作面团。随着我们度过一周，Gordon 的任务日期将越来越远离这个星期天，直到下一个星期天再次重置。然后，它会重复。

![](img/49521d0576017cd2b11926d4c783e8ae.png)

这显示了当前运行之间的时间差，为了简单起见。Execution_date_fn 查看之前的运行，但我们也会看到相同的模式。[Chef (F)](https://www.flaticon.com/free-icon/chef_8840999) 和 [Chef (M)](https://www.flaticon.com/free-icon/baker_817282?related_id=817282&origin=search) 图标由 Freepik 提供。

这表明 **星期几** 也可以帮助我们制定 `execution_date_fn`。所以我们将星期几添加到我们的表格中。我将星期一标记为 1，星期天标记为 7，按[ISO 8601](https://www.timeanddate.com/date/week-numbers.html#:~:text=The%20most%20common%20is%20the,and%20confusion%20when%20communicating%20internationally.) 标准。

![](img/ce44b091c9f31b9dac79fd58cb22956b.png)

括号中的数字表示星期几，其中星期一为 1，星期天为 7。图片由作者提供。

通过标记它们，立即可以清楚地看到……

+   `execution_delta` 从星期六的 6 开始。

+   `execution_delta` 每天增加 1，每周五最多增加到 12。

+   `execution_delta` 然后在星期六重置回 6。

我们可以在 Python 函数中重新创建这种关系，并将 `execution_date_fn` 分配给我们的外部任务传感器。

```py
def my_exec_date_fn(gordon_exec_date):
    day_of_week = gordon_exec_date.isoweekday()

    if day_of_week in (6, 7):
        time_diff = timedelta(days=day_of_week, minute=30)
        jamie_exec_date = gordon_exec_date - time_diff
    elif day_of_week in (1, 2, 3, 4, 5):
        time_diff = timedelta(days=day_of_week+7, minute=30)
        jamie_exec_date = gordon_exec_date - time_diff

    return jamie_exec_date

ext_task_sensor = ExternalTaskSensor(
    dag=gordon_tasks,
    task_id='check_dough_freshness',
    external_dag_id='jamie_tasks',
    external_task_id='make_new_dough',
    email=['gordon.damnsie@gmail.com', 'jamie@gmail.com'],
    execution_date_fn=my_exec_date_fn,
    timeout=1800,
    poke_interval=300,
    mode='reschedule'
)
```

这就是我们自己的 `execution_date_fn`。凭借一点创造力，`execution_date_fn` 可以满足 *任何* 场景。

# 我们如何将外部任务传感器融入到我们的 DAG 中？

到目前为止，我们已经涵盖了开始使用外部任务传感器所需了解的所有内容。在这一部分，我认为整理我们所学的内容，看看它们在数据管道中如何结合在一起，会是很好的。

首先，我们将在一个名为 `jamie_dag.py` 的文件中创建 Jamie DAG。

```py
from airflow.operators.dummy_operator import DummyOperator
from airflow.operators.python_operator import PythonOperator
from airflow.sensors.external_task import ExternalTaskSensor

# Define task 1
def make_dough():
    # include your secret recipe here!
    return cookies

# Create DAG
jamie_tasks = DAG(
    dag_id='jamie_tasks',
    description='Jamie to do list. (a.k.a making dough only)',
    schedule_interval='5 3 * * *',
    ...
)

# Include task 0 in DAG (as a starting point)
start = DummyOperator(
    dag=jamie_tasks,
    task_id='start'
)

# Include task 1 in DAG
make_dough = PythonOperator(
    dag=jamie_tasks,
    task_id='make_dough',
    python_callable=make_dough,
    ...
)

# Create dependencies (deciding the sequence of task to run)
start >> make_dough
```

然后，我们将在另一个名为 `gordon_dag.py` 的文件中创建 Gordon DAG。

```py
from airflow.operators.dummy_operator import DummyOperator
from airflow.operators.python_operator import PythonOperator
from airflow.sensors.external_task import ExternalTaskSensor

# Define task 1
def bake_cookies():
    # include your secret recipe here!
    return cookies

# Define task 2
def make_money():
    # include your money making technique step-by-step here.
    return money

# Define execution_date_fn for sensor 1
def my_exec_date_fn(gordon_exec_date):
    day_of_week = gordon_exec_date.isoweekday()

    if day_of_week in (6, 7):
        time_diff = timedelta(days=day_of_week, minute=30)
        jamie_exec_date = gordon_exec_date - time_diff
    elif day_of_week in (1, 2, 3, 4, 5):
        time_diff = timedelta(days=day_of_week+7, minute=30)
        jamie_exec_date = gordon_exec_date - time_diff

    return jamie_exec_date

# Create DAG
gordon_tasks = DAG(
    dag_id='gordon_tasks',
    description='List of things that Gordon needs to do.',
    schedule_interval='5 3 * * *',
    ...
)

# Include task 0 in DAG (as a starting point)
start = DummyOperator(
    dag=gordon_tasks,
    task_id='start'
)

# Include task 1 in DAG
bake_cookies = PythonOperator(
    dag=gordon_tasks,
    task_id='bake_cookies',
    python_callable=bake_cookies,
    ...
)

# Include task 2 in DAG
make_money = PythonOperator(
    dag=gordon_tasks,
    task_id='make_money',
    python_callable=make_money,
    ...
)

# Create sensor 1
check_dough_freshness = ExternalTaskSensor(
    dag=gordon_tasks,
    task_id='check_dough_freshness',
    external_dag_id='jamie_tasks',
    external_task_id='make_new_dough',
    email=['gordon.damnsie@gmail.com', 'jamie@gmail.com'],
    execution_date_fn=my_exec_date_fn,
    timeout=1800,
    poke_interval=300,
    mode='reschedule'
)

# Create dependencies (deciding the sequence of task to run)
(start
    >> check_dough_freshness
    >> bake_cookies
    >> make_money)
```

请注意，外部任务传感器在 `gordon_dag.py` 中，而不是 `jamie_dag.py` 中，因为我们希望 Gordon 来检查 Jamie，而不是反过来。Gordon 的 DAG 将是当前 DAG，而 Jamie 是外部 DAG。

然后……我们就完成了！

我们创建了第一个外部任务传感器 `check_dough_fresness`。这个传感器会检测 Jamie 的 `make_new_dough()` 是否返回成功或失败。如果失败，`bake_cookies()` 和 `make_money()` 将不会运行。

# 附加内容：Airflow 中的日期概念

在 Apache Airflow 中，日期令人困惑，因为有很多与日期相关的术语，如 `start_date`、`end_date`、`schedule_interval`、`execution_date` 等。真的很混乱。但让我们通过一个故事来尝试弄清楚。

假设我们的老板想了解他公司销售业绩。他希望这些数据在接下来的 *6 个月* 中 *每天* 的 *午夜 12 点* 刷新。

首先，我们编写一个复杂的 SQL 查询来生成销售业绩数据。运行查询需要 *6 小时*。

+   `task_start` 是任务的开始时间。

+   `task_end` 是任务的结束时间。

+   `task_duration` 是运行任务所需的时间。

![](img/77cc87def5c5b24335fa1b2be7b0b8bc.png)

单个任务。图片由作者提供。

每天，我们需要在午夜 12 点运行这个任务。

![](img/be480214c92910c701041c74585487e0.png)

一个单独的任务，安排在凌晨 12 点并运行 6 小时。图片由作者提供。

为了自动化此查询，我们创建一个 Airflow DAG 并指定 `start_date` 和 `end_date`。只要今天的日期在这个时间段内，Airflow 就会执行 DAG。

![](img/e8927e14501927829996d0a2e6730ac9.png)

一个 Airflow DAG。图片由作者提供。

然后，我们将任务放入 Airflow DAG 中。

我们需要这个数据每天凌晨 12 点刷新一次。因此，我们将 `schedule_interval` 设置为 `"0 0 * * *"`，这相当于 [CRON](https://crontab.guru/#0_0_*_*_*) 的每天午夜 12 点。

`schedule_interval` 实质上在每个连续调度之间添加了延迟，告知 Airflow 只在特定时间运行任务，因为我们不希望任务在完成后立即重新运行。

+   `interval_start` 指的是特定调度间隔的开始时间。

+   `interval_end` 指的是特定调度间隔的结束时间。

![](img/56b9d7e428899aabb7fe43f6896889b9.png)

请注意，`interval_start` 和 `interval_end` 可能会重叠。前一个调度间隔的 `interval_end` 将与下一个调度间隔的 `interval_start` 相同。图片由作者提供。

这里有最让人惊讶的部分——尽管看似直觉相悖，Airflow 调度器在调度间隔的***结束***时触发 DAG 运行，而不是在其开始时。

这意味着 Airflow 在第一次调度间隔内不会做任何事情。我们的查询将在 2023 年 1 月 2 日 12 点首次运行。

![](img/edad1a82094c5e1395626e6ac885a9a3.png)

这些彩色条像数据一样。所有“黄色”数据只有在 1 月 2 日才会被汇总。图片由作者提供。

这是因为 Airflow 最初是作为一个 ETL 工具创建的。它建立在一个理念上，即一段时间的数据在间隔的*结束*时进行汇总。

例如，如果我们想了解 1 月 1 日的饼干销售情况，我们不会在 1 月 1 日下午 1 点创建销售报告，因为一天还没有结束，销售数字会不完整。相反，我们只会在午夜 12 点处理数据。今天，我们将处理昨天的数据。

这为什么重要？

由于我们在总结上一个运行的数据，我们在 1 月 2 日生成的销售报告描述的是 1 月 1 日的销售情况，而不是 1 月 2 日的销售情况。

因此，尽管任务在 2 日执行，Airflow 仍将其称为 1 月 1 日的运行。为了更好地区分日期，Airflow 给调度间隔的开始时间赋予了一个特别的名称——`execution_date`。

![](img/f06b3ef75c125c2c44c27293b40ff896.png)

虽然我们在 1 月 2 日运行了“黄色”任务，但其执行日期实际上是 1 月 1 日。图片由作者提供。

这就是为什么我们在计算 `execution_delta` 时总是取“上一个”运行的差值，因为它是 `execution_dates` 的增量，本质上是“上一个”运行。

# 结论

外部任务传感器就像门卫。它们通过确保任务按照特定顺序执行，并且在继续执行后续任务之前满足必要的依赖关系，来阻止不良数据流入下游。

对于那些从未使用过外部任务传感器的人，我希望这篇文章能传达它的重要性并说服你开始使用它们。对于那些已经在使用它们的人，我希望这里的一些见解能够帮助加深你的理解。

感谢你的时间，祝你有美好的一天。

*喜欢这篇文章？考虑成为一个* [*Medium 会员*](https://casey-cheng.medium.com/membership) *以获得对每篇文章的完全访问权限，并支持像我这样的内容创作者。*

[](https://medium.com/@casey-cheng/membership?source=post_page-----425093323758--------------------------------) [## 使用我的推荐链接加入 Medium - Casey Cheng

### 阅读每一篇来自 Casey Cheng 的故事（以及 Medium 上成千上万的其他作者的文章）。你的会员费直接支持…

medium.com](https://medium.com/@casey-cheng/membership?source=post_page-----425093323758--------------------------------)
