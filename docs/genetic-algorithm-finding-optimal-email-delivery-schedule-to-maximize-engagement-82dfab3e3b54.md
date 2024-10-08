# 遗传算法：寻找最佳电子邮件投递时间表以最大化互动

> 原文：[`towardsdatascience.com/genetic-algorithm-finding-optimal-email-delivery-schedule-to-maximize-engagement-82dfab3e3b54`](https://towardsdatascience.com/genetic-algorithm-finding-optimal-email-delivery-schedule-to-maximize-engagement-82dfab3e3b54)

## 使用进化算法优化消费者银行的 D2C 活动

[](https://abhijeetstalaulikar.medium.com/?source=post_page-----82dfab3e3b54--------------------------------)![Abhijeet Talaulikar](https://abhijeetstalaulikar.medium.com/?source=post_page-----82dfab3e3b54--------------------------------)[](https://towardsdatascience.com/?source=post_page-----82dfab3e3b54--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----82dfab3e3b54--------------------------------) [Abhijeet Talaulikar](https://abhijeetstalaulikar.medium.com/?source=post_page-----82dfab3e3b54--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----82dfab3e3b54--------------------------------) ·阅读时间 9 分钟·2023 年 11 月 1 日

--

![](img/0d31fcceb333d2b64b85bacbbed6f77f.png)

作者使用 Bing Image Creator 生成的图像

某些电子邮件投递时间是否会导致更高的互动？

电子邮件营销人员常面临的一个最常见问题是何时发送电子邮件以最大化打开率、点击率和转化率。这个问题没有明确的答案，因为不同的受众可能有不同的偏好和行为。他们处于哪个时区？他们使用什么设备查看电子邮件？他们的日常生活和时间表是什么？他们多频繁检查电子邮件？这些因素可能会影响他们何时最有可能打开和互动你的电子邮件。

你可以使用像 A/B 测试或分割测试这样的工具来比较在不同时间发送的电子邮件活动的表现。你还可以使用像 Google Analytics 或 Mailchimp 这样的分析工具来跟踪电子邮件活动的指标，例如打开率、点击率、跳出率和转化率。通过分析数据，你可以确定适合你受众和目标的最佳投递时间。

当你对客户在不同时间的点击率和打开率有了充分了解后，下一步是创建一个最佳的投递时间表，以最大化这些指标，同时避免负面影响，例如用户退订——我们在电子邮件营销中称之为“疲劳”现象。

在这篇文章中，我将尝试使用遗传算法来解决这个优化问题——这种方法在营销领域并不常见。

我理解遗传算法可能会让人感到困惑。我们将看到如何使用简单的 Pandas 操作来实现该算法的核心概念。

# 定义问题陈述

我在[下一步行动](https://medium.com/@abhijeetstalaulikar/next-best-action-learning-optimal-policy-for-maximizing-mortgage-collection-7160bb3d8e95)的文章中介绍了虚构但受欢迎的消费者银行 ULFC。在那次努力中，我们创建了一个强化模型，根据客户过去的响应，建议向其按揭客户推出下一个最佳优惠。现在，ULFC 希望数据科学团队推荐一个最优的时间表——日期、时间、数量——以作为季度电子邮件活动的一部分。**目标是最大化客户对电子邮件的参与，而不流失他们。**

我将使用[这份合成数据](https://github.com/abhijeet-talaulikar/Medium_Resources/blob/main/email_engagement_stats_data.zip)，这是我创建的。它包含了针对 1000 名客户的历史点击率、打开率和疲劳率数据，这些客户将作为本次活动的目标。

让我们先来看一下数据。

+   打开率通常较高，可达 30%。

+   点击率更具意图性，范围在 0-3%之间

+   疲劳率（客户取消电子邮件营销的概率）更具上下文性，并且可能因细分/活动而异。我们将假设在 0-5%之间。

```py
import numpy
import pandas

open_rate_df = pd.read_csv("open_rate.csv").set_index("Customer_ID")
click_rate_df = pd.read_csv("click_rate.csv").set_index("Customer_ID")
fatigue_rate_df = pd.read_csv("fatigue_rate.csv").set_index("Customer_ID")
```

![](img/310bc9a17fad2ffa1ed16a56c341f73c.png)

作者提供的图片 — 1000 名客户的每日打开率

![](img/278d8174e0a256a7855575d440b57521.png)

作者提供的图片 — 1000 名客户的每日点击率

![](img/ae446823ef69fa451e9082517709a0f0.png)

作者提供的图片 — 1000 名客户的每日疲劳率

# 遗传算法简介

遗传算法（GA）是一种通过模拟自然进化过程来寻找问题解决方案的方法。遗传算法通过创建一个可能解决方案的种群（称为个体），并对其应用选择、交叉和变异等生物学操作符来工作。这些个体通过一个适应度函数进行评估，该函数衡量它们解决问题的效果。最适应的个体更可能生存和繁殖，而较不适应的个体则被淘汰。通过这种方式，遗传算法可以在搜索空间中进行探索，并随着时间的推移趋向于最优或接近最优的解决方案。以下是任何遗传算法用例中涉及的核心步骤。

![](img/e1a508ce393268c8f308bebb75f50b68.png)

作者提供的图片 — 任何遗传算法中的核心步骤

# 设置参数

客户的数量 *K* 是 1000。在这次电子邮件营销活动中，我们希望将打开率和点击率最大化。它们的权重由 *w*₀ 和 *wc* 分别设定。

假设我们不希望在时间范围内向同一客户发送超过 2 封邮件（由 *E* 设置）。另一方面，我们希望将每个客户的预期疲劳率限制在 10%以下（由 *T* 设置）。

```py
K = 1000
w0 = 1
wc = 1
T = 0.1
E = 2
```

# 创建初始种群

在这里，我们基本上创建了初始候选者，这些候选者作为我们算法通过选择、交叉和变异来改进的起始种群。每个候选者是一组 0 和 1 的二进制决策变量，表示是否在该时间段发送邮件。

![](img/41bb458a6eb260d659db380463ca4726.png)

作者提供的图片 — 示例候选者

```py
def initialize_population(num_candidates):
    all_candidates = []
    for i in range(num_candidates):

        # Create random decision variables such that only 1 email is delivered to a person per day
        base = np.random.choice([-1,0,1,2], size=(K,5), p=[0.95, 0.05/3, 0.05/3, 0.05/3]).tolist()
        data = []
        for k in range(K):
            vals = []
            for t in base[k]:
                if t == -1:
                    vals.extend([0,0,0])
                elif t == 0:
                    vals.extend([1,0,0])
                elif t == 1:
                    vals.extend([0,1,0])
                elif t == 2:
                    vals.extend([0,0,1])

            data.append(vals)

        all_candidates.append(
            pd.DataFrame(
                data = np.array(data),
                index = customer_ids,
                columns = timeslots
            )
        )
    return all_candidates
```

# 定义适应度函数

目标是根据每个接收者的偏好和行为选择最佳的邮件发送日期。我们希望增加他们打开和点击邮件的可能性。我们假设每个接收者在每一天和时间段的打开率和点击率不同。我们为打开率和点击率添加权重，这可以根据是转换还是意识提升活动进行调整 [1]。因此，我们需要找到每个接收者得分最高的日期和时间段。

我们需要考虑两个限制：

1.  **疲劳限制：** 每个接收者在一定的频率下会感到疲倦，这种频率因人而异。如果我们发送过多的邮件，他们可能会退订。我们需要将每个接收者的总疲劳率保持在一定的限制内。

1.  **总投递限制：** 这控制了我们可以发送给一个接收者的最大邮件数量。

![](img/6f91839b52b9b421719875f58ee0b499.png)

作者提供的图片 — 目标函数

在下面的代码中，我已经将这一目标转换为一个适应度函数，该函数返回目标分数，并根据约束 1 和 2 是否满足返回布尔值。

```py
def fitness_function(candidate_df):
    obj = ((w0 * open_rate_df + wc * click_rate_df) * candidate_df).values.sum()
    const1 = ((fatigue_rate_df * candidate_df).sum(axis=1) > T).sum() == 0
    const2 = (candidate_df.sum(axis=1) > E).sum() == 0
    return obj, const1, const2

def evaluate_fitness(x):
    score, const1, const2 = fitness_function(x)
    if const1 and const2: return score
    else: return -1
```

现在让我们创建一个简单的函数来测量一些关键指标，以帮助我们了解投递时间表的质量。

```py
def print_candidate_metrics(candidate):
    open_rates = (open_rate_df * candidate).values.flatten()
    open_rate = round(open_rates[open_rates > 0].mean() * 100, 1)
    expected_opens = round(open_rates[open_rates > 0].sum(), 1)
    print(f"Open Rate = {open_rate}%")
    print(f"Expected number of opens = {expected_opens}")

    click_rates = (click_rate_df * candidate).values.flatten()
    click_rate = round(click_rates[click_rates > 0].mean() * 100, 1)
    expected_clicks = round(click_rates[click_rates > 0].sum(), 1)
    print(f"Click Rate = {click_rate}%")
    print(f"Expected number of clicks = {expected_clicks}")

    fatigue_rates = (fatigue_rate_df * candidate).values.flatten()
    fatigue_rate = round(fatigue_rates[fatigue_rates > 0].mean() * 100, 1)
    print(f"Fatigue Rate = {fatigue_rate}%")
```

首先，我们将检查一些初始随机选择的候选者的指标作为基准。

```py
initial_candidates = initialize_population(3)
for i in initial_candidates:
    print_candidate_metrics(i)
    print()
```

![](img/7ae2e93e1838bd32447b034161192ea3.png)

作者提供的图片 — 初始随机生成的候选者时间表的指标

我看到初始候选者在时间范围内平均生成 35–40 次打开。我们还看到大约 4 次预期点击。

让我们跟随这个过程，构建算法，看看它是否能改善这些指标。

# 自然选择中的过程

驱动代际过程的主要过程是 **选择**。在遗传算法的实践中，这有许多定义。最简单的一种是根据适应度分数对所有候选者进行排名，并选择前 N 名将其属性传递给下一代。这种方法被称为精英主义或选择精英候选者的过程。

另外两个过程是**交叉**和**变异**。这两个操作是遗传算法中的基本操作，有助于从现有的解决方案中创造新的解决方案。交叉将两个或多个父代候选者的特征结合起来，生成子代候选者，而变异则在候选者中引入随机变化，以创造多样性并避免局部最优解。交叉和变异的概率控制着种群中候选者的变化率。较高的交叉概率意味着将通过结合父代生成更多的子代，而较低的变异概率则意味着较少的子代会随机改变。这些概率可以是固定的，也可以是自适应的，具体取决于问题和适应度函数。

我打算实施一个简单的交叉操作，我将随机选择一个行号（即交叉点），并从第一个子代中提取该行号之前的所有行，从第二个子代中提取该行号之后的所有行。

```py
def crossover(candidate1, candidate2):
    crossover_point = random.randint(1,K-1)
    child1 = pd.concat([candidate1.iloc[:crossover_point,:], candidate2.iloc[crossover_point:,:]])
    child2 = pd.concat([candidate1.iloc[:crossover_point], candidate2.iloc[crossover_point:]])
    return child1, child2
```

变异有点有趣。我的实现是单点变异。我随机选择一个时间段中的一天（即变异点），并改变那一天的所有决策（0 和 1）。

```py
def mutate(candidate):
    mutation_point = random.randint(0,14)
    new_candidate = candidate.copy()
    new_candidate.iloc[:,mutation_point] = np.random.choice([0, 1], size=K, p=[.99, .01])
    return new_candidate
```

通常，变异对于探索搜索空间的新区域非常有用，而交叉则有助于利用局部最优解。要更好地理解探索和利用，查看一下[我的文章](https://medium.com/@abhijeetstalaulikar/next-best-action-learning-optimal-policy-for-maximizing-mortgage-collection-7160bb3d8e95)关于下一步行动模型。利用这些功能，让我们构建 ULFC 的算法，将所有部分拼接在一起。

# 最终算法

首先，我们有几个参数控制代数（把它看作是迭代）、每代结束时允许的种群大小、交叉和变异的概率，以及精英数量（用于选择过程）。

```py
generations = 100
population_size = 50
crossover_rate = 0.8
mutation_rate = 0.2
elitism_size = 2
```

如之前的流程图所示，我们的算法有 5 个步骤。

1.  创建随机初始种群。

1.  使用适应度函数评估候选者。

1.  选择精英。

1.  应用交叉以创建新的子代候选者。

1.  对新创建的候选者应用变异。

重新评估并重复多代。

```py
def optimal_schedule():

    # Create initial candidates
    population = initialize_population(population_size)

    for generation in range(generations):
        new_population = []

        # Elitism
        population.sort(key=lambda x: evaluate_fitness(x), reverse=True)
        new_population.extend(population[:elitism_size])

        while len(new_population) < population_size:
            parent1, parent2 = random.choices(population, k=2)
            if random.random() < crossover_rate:
                child1, child2 = crossover(parent1, parent2)
            else:
                child1, child2 = parent1.copy(), parent2.copy()

            if random.random() < mutation_rate:
                child1 = mutate(child1)
            if random.random() < mutation_rate:
                child2 = mutate(child2)

            new_population.extend([child1, child2])

        population = new_population

    return population[0]
```

最后，让我们运行刚刚构建的算法。

```py
result = optimal_schedule()
print_candidate_metrics(result)
```

![](img/c8fb1ceb5cb1d5c2554c1507fdc1ea1b.png)

作者提供的图片 — 最优时间表的指标

最优时间表的预期打开次数和点击次数比初始候选者更高。你可以在更多代中运行此算法以进一步优化。你甚至可以进行时间段分析，以跟踪推荐的时间表变化。

![](img/e65159811adea677772bc993ff8972ac.png)

作者提供的图片

# 总结

媒介如电子邮件和短信可以是非常具有成本效益和说服力的渠道来接触受众。但进一步优化电子邮件营销的时间安排可以确保企业从其电子邮件中获得最大互动。这些项目还解锁了许多后续分析。例如，流行的 CRM 软件 HubSpot 的团队每年发布关于“发送电子邮件的最佳时间”的报告 [2]。根据他们 2023 年的发现，电子邮件从晚上 9 点到 12 点获得的互动最多。然而，这些数据来自对他们客户的调查。如果你在公司内部进行时间安排优化，你可以以更高的准确性和细致度产生这样的见解，从而适应你的业务案例。

最后，作为一种警告，我建议谨慎使用遗传算法。只有当你知道如何在你的问题背景下解释和说明它时。它只是另一种优化算法，还有许多可能更容易解释和更快的算法。另一方面，遗传算法是稳健的，并且可以很好地达到全局最优。

感谢阅读 😁

**参考文献：**

1.  Zhang, L., He, J., Yan, Z., Dai, W., Pani, A. (2020). 遗传算法在多目标电子邮件营销投递问题中的应用

1.  发送电子邮件的最佳时间 [2023 研究] 由 [Flori Needle](https://blog.hubspot.com/marketing/author/flori-needle?hubs_content=blog.hubspot.com%2Fmarketing%2Fbest-time-to-send-email&hubs_content-cta=-medium) 撰写 — [`blog.hubspot.com/marketing/best-time-to-send-email`](https://blog.hubspot.com/marketing/best-time-to-send-email)
