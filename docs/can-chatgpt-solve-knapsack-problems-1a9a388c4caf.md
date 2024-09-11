# ChatGPT 能解决背包问题吗？

> 原文：[https://towardsdatascience.com/can-chatgpt-solve-knapsack-problems-1a9a388c4caf?source=collection_archive---------10-----------------------#2023-09-21](https://towardsdatascience.com/can-chatgpt-solve-knapsack-problems-1a9a388c4caf?source=collection_archive---------10-----------------------#2023-09-21)

## 用 ChatGPT 解决运筹学（OR）问题

[](https://medium.com/@guanx92?source=post_page-----1a9a388c4caf--------------------------------)[![Guangrui Xie](../Images/def9aa637424a88d75a6a3bb103350bc.png)](https://medium.com/@guanx92?source=post_page-----1a9a388c4caf--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1a9a388c4caf--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1a9a388c4caf--------------------------------) [Guangrui Xie](https://medium.com/@guanx92?source=post_page-----1a9a388c4caf--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F495b92f0c66d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-chatgpt-solve-knapsack-problems-1a9a388c4caf&user=Guangrui+Xie&userId=495b92f0c66d&source=post_page-495b92f0c66d----1a9a388c4caf---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1a9a388c4caf--------------------------------) ·10分钟阅读·2023年9月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1a9a388c4caf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-chatgpt-solve-knapsack-problems-1a9a388c4caf&user=Guangrui+Xie&userId=495b92f0c66d&source=-----1a9a388c4caf---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1a9a388c4caf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-chatgpt-solve-knapsack-problems-1a9a388c4caf&source=-----1a9a388c4caf---------------------bookmark_footer-----------)![](../Images/3272c86ba9ff1223c86d7d1d8650584d.png)

图片由 [Jakob Owens](https://unsplash.com/@jakobowens1?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

自从 ChatGPT 出现以来，我一直在思考 ChatGPT 将如何影响优化和运筹学（OR）领域。关于 ChatGPT 通过高中和本科水平的数学考试的新闻，引发了我对它在解决 OR 问题方面表现的兴趣。在本文中，我将使用一个经典的 OR 问题——0/1 背包问题来进行一些测试。

# **背包问题**

在优化和决策难题的领域，背包问题长期以来一直是经典的基准问题。它挑战我们选择一组物品，每个物品都有自己的重量和价值，以最大化总价值，同时保持在给定的重量限制内。动态规划是一种我们通常用来解决 0/1 胶囊问题的有效算法。

# **与 ChatGPT 的对话**

我从一个很小的例子开始，并要求 ChatGPT 解决它。

*我：考虑一个背包问题，其中背包的容量是 10lbs。我有 4 个物品可以选择，它们的价值分别为 $10、$40、$30、$50，重量分别为 5lbs、4lbs、6lbs、3lbs。你能为我解决这个问题吗？*

*ChatGPT：当然！背包问题是一个经典的优化问题，目标是最大化选择物品的总价值，同时满足所选物品的总重量不超过背包的容量。*

*在你的情况下，你有 4 个物品及其相应的价值和重量：*

*项目 1：价值 = $10，重量 = 5lbs 项目 2：价值 = $40，重量 = 4lbs 项目 3：价值 = $30，重量 = 6lbs 项目 4：价值 = $50，重量 = 3lbs*

*背包容量 = 10lbs*

*要解决这个问题，你可以使用动态规划方法。创建一个二维表格，其中行表示物品，列表示剩余的背包容量（从 0lbs 到 10lbs）。通过迭代填充表格，以找到每个物品组合和剩余容量的最大值。*

*以下是表格填充过程：*

*容量 (lbs) 0 1 2 3 4 5 6 7 8 9 10

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

项目 1 (5lbs) 0 0 0 0 0 10 10 10 10 10 10

项目 2 (4lbs) 0 0 0 0 40 40 40 40 50 50 50

项目 3 (6lbs) 0 0 0 0 40 40 30 70 70 70 70

项目 4 (3lbs) 0 0 0 50 50 50 50 70 70 70 80*

*表格显示了每个物品组合和剩余容量的最大值。最终单元格（右下角）具有 $80 的值，这是在 10lbs 的背包容量下可以获得的最大值。*

*要找到应该选择的物品，你可以从最后一个单元格回溯到第一个单元格，同时考虑导致最大值的决策。在这种情况下，你会选择项目 2 和项目 4，它们的总值为 $40 + $50 = $90，总重量为 4lbs + 3lbs = 7lbs，这在 10lbs 的背包容量范围内。*

*因此，最佳解决方案是选择项目 2 和项目 4，这样你将得到总值 $90。*

显然，我们注意到 ChatGPT 能够选择正确的方法来解决这个问题，但 ChatGPT 给出的最终解答与其推理过程不一致。具体来说，动态规划（DP）表显示 $80（右下角单元格）是最佳目标值，但 ChatGPT 得出的结论是 $90 是最终答案。

为了检查哪个是正确的解决方案，我编写了一段Python脚本来执行DP算法。

```py
import numpy as np
v = [10, 40, 30, 50]
w = [5, 4, 6, 3]
c = 10
dp = np.zeros((5,11))
for i in range(1,5):
    for j in range(1,11):
        if j < w[i-1]:
            dp[i,j] = dp[i-1,j]
        else:
            dp[i,j] = max(dp[i-1,j],dp[i-1,j-w[i-1]]+v[i-1]) 
dp[1:,1:]
```

打印出的DP表如下。

![](../Images/0622c030306b1f62b6374d6d6db77199.png)

这确认了ChatGPT得出的结论是正确的，但ChatGPT给出的DP表是错误的。然后我继续让它纠正DP表。

*我：我认为你的解决方案是正确的，但表格有些错误。*

*ChatGPT：我为任何困惑道歉。让我为你纠正表格。以下是修正后的背包问题动态规划表：*

*容量 (磅) 0 1 2 3 4 5 6 7 8 9 10*

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

物品1 (5磅) 0 0 0 0 0 10 10 10 10 10 10

物品2 (4磅) 0 0 0 0 40 40 40 40 40 50 50

物品3 (6磅) 0 0 0 0 40 40 40 40 40 50 70

物品4 (3磅) 0 0 0 50 50 50 90 90 90 90 90*

*现在，表格是正确的。右下角单元格中的值是$90，这表示在10磅的背包容量下可以获得的最大值。如前所述，最优解是选择物品2和物品4，这将给你总价值$90。*

有趣的是，ChatGPT能够对表格进行一些修正，使右下角单元格与$90对齐。然而，如果我们更仔细地查看表格，表格的最后一行仍然不正确。第4行第7列的单元格（对应于背包容量为6磅）应该是$50而不是$90。一个容量为6磅的背包无法选择物品2和物品4来达到$90，因为物品2和物品4的总重量为7磅，超出了背包的容量。

我随后要求它编写一些代码来解决这个问题。

*我：你能写一段Python代码来解决这个问题吗？*

*ChatGPT：当然可以！你可以使用动态规划在Python中解决0/1背包问题。以下是解决你提供的特定实例的Python代码片段：*

```py
def knapsack(values, weights, capacity):
    n = len(values)
    # Initialize a table to store the maximum values for different capacities
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]

    for i in range(1, n + 1):
        for w in range(capacity + 1):
            # If the current item's weight is more than the current capacity, skip it
            if weights[i - 1] > w:
                dp[i][w] = dp[i - 1][w]
            else:
                # Choose the maximum between including and excluding the current item
                dp[i][w] = max(dp[i - 1][w], dp[i - 1][w - weights[i - 1]] + values[i - 1])

    # Backtrack to find the selected items
    selected_items = []
    i, w = n, capacity
    while i > 0 and w > 0:
        if dp[i][w] != dp[i - 1][w]:
            selected_items.append(i - 1)
            w -= weights[i - 1]
        i -= 1

    return dp[n][capacity], selected_items

# Item values and weights
values = [10, 40, 30, 50]
weights = [5, 4, 6, 3]
knapsack_capacity = 10

max_value, selected_items = knapsack(values, weights, knapsack_capacity)

print(f"Maximum value: ${max_value}")
print("Selected items:")
for i in selected_items:
    print(f"Item {i + 1} (Value: ${values[i]}, Weight: {weights[i]} lbs)")
```

*当你运行这段代码时，它将输出最大值和应放入背包的选定物品。在你的特定情况下，它会正确选择物品2和物品4，这些物品的总价值为$90，总重量为7磅，在背包容量为10磅的范围内。*

这段代码清晰正确地打印出了最优解。由这段代码计算出的DP表也正确。

然后我进一步测试了一个更大规模的实例，一个背包容量为500磅且有10个物品可供选择的背包问题。每个物品的价值和重量都是使用以下代码随机生成的。

```py
import numpy as np
np.random.seed(0)
w = np.random.normal(10, 3, 100)
w = [int(x) if x>= 1 else 1 for x in w]
v = np.random.normal(100, 30, 100)
v = [int(x) if x>= 10 else 10 for x in v]

dp = np.zeros((101,501))
for i in range(1,101):
    for j in range(1,501):
        if j < w[i-1]:
            dp[i,j] = dp[i-1,j]
        else:
            dp[i,j] = max(dp[i-1,j],dp[i-1,j-w[i-1]]+v[i-1]) 
dp[1:,1:]
```

我复制并粘贴了生成的值和重量，并要求ChatGPT解决它。

*我：考虑一个背包问题，其中背包的容量为 500 磅。我有 100 个物品可供选择，其值由列表 [156, 59, 61, 129, 64, 158, 87, 77, 157, 144, 156, 127, 74, 157, 91, 124, 128, 95, 118, 127, 111, 67, 108, 139, 79, 95, 86, 155, 120, 112, 76, 116, 79, 100, 80, 120, 117, 93, 111, 67, 55, 113, 105, 119, 171, 128, 72, 133, 60, 86, 97, 151, 77, 75, 97, 80, 133, 67, 65, 86, 85, 157, 128, 102, 63, 125, 69, 53, 135, 109, 127, 109, 125, 80, 68, 120, 75, 79, 86, 100, 89, 58, 80, 33, 118, 51, 66, 101, 77, 146, 61, 108, 98, 64, 115, 94, 123, 124, 164, 140] 和重量由列表 [15, 11, 12, 16, 15, 7, 12, 9, 9, 11, 10, 14, 12, 10, 11, 11, 14, 9, 10, 7, 2, 11, 12, 7, 16, 5, 10, 9, 14, 14, 10, 11, 7, 4, 8, 10, 13, 13, 8, 9, 6, 5, 4, 15, 8, 8, 6, 12, 5, 9, 7, 11, 8, 6, 9, 11, 10, 10, 8, 8, 7, 8, 7, 4, 10, 8, 5, 11, 7, 10, 12, 10, 13, 6, 11, 7, 7, 8, 9, 10, 6, 12, 11, 5, 14, 15, 13, 9, 6, 13, 8, 13, 10, 12, 11, 12, 10, 15, 10, 11] 分别给出。你能为我解决这个问题吗？*

*ChatGPT：解决 100 个物品、容量为 500 磅的背包问题是一个计算密集型任务。然而，我可以提供给你用于解决此问题的 Python 代码，使用动态规划方法，但请注意，这可能需要一些时间来运行。*

*这里是使用提供的值和重量来解决背包问题的 Python 代码：*

```py
def knapsack(values, weights, capacity):
    n = len(values)
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]

    for i in range(1, n + 1):
        for w in range(capacity + 1):
            if weights[i - 1] > w:
                dp[i][w] = dp[i - 1][w]
            else:
                dp[i][w] = max(dp[i - 1][w], dp[i - 1][w - weights[i - 1]] + values[i - 1])

    selected_items = []
    i, w = n, capacity
    while i > 0 and w > 0:
        if dp[i][w] != dp[i - 1][w]:
            selected_items.append(i - 1)
            w -= weights[i - 1]
        i -= 1

    return dp[n][capacity], selected_items

# Item values and weights
values = [
    156, 59, 61, 129, 64, 158, 87, 77, 157, 144, 156, 127, 74, 157, 91, 124,
    128, 95, 118, 127, 111, 67, 108, 139, 79, 95, 86, 155, 120, 112, 76, 116,
    79, 100, 80, 120, 117, 93, 111, 67, 55, 113, 105, 119, 171, 128, 72, 133,
    60, 86, 97, 151, 77, 75, 97, 80, 133, 67, 65, 86, 85, 157, 128, 102, 63,
    125, 69, 53, 135, 109, 127, 109, 125, 80, 68, 120, 75, 79, 86, 100, 89, 58,
    80, 33, 118, 51, 66, 101, 77, 146, 61, 108, 98, 64, 115, 94, 123, 124, 164,
    140
]

weights = [
    15, 11, 12, 16, 15, 7, 12, 9, 9, 11, 10, 14, 12, 10, 11, 11, 14, 9, 10, 7,
    2, 11, 12, 7, 16, 5, 10, 9, 14, 14, 10, 11, 7, 4, 8, 10, 13, 13, 8, 9, 6, 5,
    4, 15, 8, 8, 6, 12, 5, 9, 7, 11, 8, 6, 9, 11, 10, 10, 8, 8, 7, 8, 7, 4, 10,
    8, 5, 11, 7, 10, 12, 10, 13, 6, 11, 7, 7, 8, 9, 10, 6, 12, 11, 5, 14, 15, 13,
    9, 6, 13, 8, 13, 10, 12, 11, 12, 10, 15, 10, 11
]

knapsack_capacity = 500

max_value, selected_items = knapsack(values, weights, knapsack_capacity)

print(f"Maximum value: {max_value}")
print("Selected items:")
for i in selected_items:
    print(f"Item {i + 1} (Value: {values[i]}, Weight: {weights[i]})")
```

*运行这段代码将为你提供可以获得的最大值和所选物品的列表。请耐心等待，因为可能需要一些时间来执行。*

这一次，它没有直接解决问题并给出答案，而是给了我一段 Python 代码来运行。代码在解决这个问题时表现很好。

# 结论

这是我第一次尝试理解 ChatGPT 解决复杂数学问题的能力，比如经典的 OR 问题——背包问题。对于小规模的背包问题，它似乎能够给出最佳解决方案的答案。然而，答案的质量并不总是有保障的。在推理过程中或最终答案中可能会出现错误。对于大规模实例，ChatGPT 倾向于避免直接给出最佳解，而是提供一段代码供运行。代码通常编写得很好，可以得到正确的解决方案。因此，在使用 ChatGPT 解决背包问题时，不要过于依赖它给出的最佳解直接答案，而是应运行它提供的代码进行双重检查。
