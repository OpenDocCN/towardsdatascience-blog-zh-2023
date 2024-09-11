# 使用 PyMC-Marketing 进行客户生命周期价值预测

> 原文：[https://towardsdatascience.com/pymc-marketing-the-key-to-advanced-clv-customer-lifetime-value-forecasting-bc0730973c0a?source=collection_archive---------1-----------------------#2023-11-10](https://towardsdatascience.com/pymc-marketing-the-key-to-advanced-clv-customer-lifetime-value-forecasting-bc0730973c0a?source=collection_archive---------1-----------------------#2023-11-10)

## 探索**买到死（BTYD）**建模的深度及实际编码技巧

[](https://medium.com/@hajime.takeda?source=post_page-----bc0730973c0a--------------------------------)[![Hajime Takeda](../Images/42ffd8c9416240fa43773817e76a55fa.png)](https://medium.com/@hajime.takeda?source=post_page-----bc0730973c0a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bc0730973c0a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bc0730973c0a--------------------------------) [Hajime Takeda](https://medium.com/@hajime.takeda?source=post_page-----bc0730973c0a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6d7012b72e49&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpymc-marketing-the-key-to-advanced-clv-customer-lifetime-value-forecasting-bc0730973c0a&user=Hajime+Takeda&userId=6d7012b72e49&source=post_page-6d7012b72e49----bc0730973c0a---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bc0730973c0a--------------------------------) · 15 min 阅读 · 2023年11月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbc0730973c0a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpymc-marketing-the-key-to-advanced-clv-customer-lifetime-value-forecasting-bc0730973c0a&user=Hajime+Takeda&userId=6d7012b72e49&source=-----bc0730973c0a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbc0730973c0a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpymc-marketing-the-key-to-advanced-clv-customer-lifetime-value-forecasting-bc0730973c0a&source=-----bc0730973c0a---------------------bookmark_footer-----------)![](../Images/911c8fd8f93c5e4448f5294fe42fabe9.png)

图片由 [Boxed Water Is Better](https://unsplash.com/@boxedwater?utm_source=medium&utm_medium=referral) 提供，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**TL; DR：** 客户生命周期价值（CLV）模型是客户分析中的关键技术，有助于公司识别出哪些是有价值的客户。忽视CLV可能导致对短期客户过度投资，这些客户可能只会进行一次购买。‘Buy Till You Die’建模，利用BG/NBD和Gamma-Gamma模型，可以估算CLV。尽管最佳实践因数据规模和建模优先级而异，[PyMC-Marketing](https://github.com/pymc-labs/pymc-marketing) 是推荐的Python库，适合那些希望快速实现CLV建模的人。

对于那些急于直接查看示例代码的人，请参考我的[GitHub仓库](https://github.com/takechanman1228/Effective-CLV-Modeling/blob/main/PyMC_Marketing_CLV_demo.ipynb)。如果你能留下一个星标，我将非常高兴！

[](https://github.com/takechanman1228/Effective-CLV-Modeling/tree/main?source=post_page-----bc0730973c0a--------------------------------) [## GitHub - takechanman1228/Effective-CLV-Modeling

### 通过在GitHub上创建账户来为takechanman1228/Effective-CLV-Modeling的发展做贡献。

github.com](https://github.com/takechanman1228/Effective-CLV-Modeling/tree/main?source=post_page-----bc0730973c0a--------------------------------)

# 1\. 什么是CLV？

**CLV的定义是公司在客户整个关系期间可以期望获得的总净收入。** 你们中的一些人可能更熟悉‘LTV’（生命周期价值）这个术语。是的，CLV和LTV是可以互换的。

![](../Images/a087a3d1f6f110149bfe94b51f0a7e31.png)

图片由作者提供

+   第一个目标是计算和预测未来的CLV，这将帮助你了解每个客户能带来多少预期收入。

+   第二个目标是识别盈利客户。模型将通过分析高CLV客户的特征来告诉你这些有价值的客户是谁。

+   第三个目标是根据分析采取营销行动，从而优化你的营销预算分配。

![](../Images/d7a027532b930fc16aecb49dc06a9865.png)

图片由作者提供

# 2\. 业务背景

以像Nike这样的时尚品牌的电子商务网站为例，该网站可能会利用广告和优惠券来吸引新客户。现在，假设大学生和在职专业人士是两个主要的重要客户群体。对于首次购买，公司在大学生身上花费了$10的广告费，在在职专业人士身上花费了$20。两者的购买金额都在$100左右。

如果你负责营销，你会更愿意在哪个细分市场上投入更多？你可能会自然地认为，考虑到大学生的较低成本和较高的投资回报率，将更多资金投入大学生市场是更有逻辑的。

![](../Images/b2d5573d2b3a26a22b53a494f2efcbbe.png)

图片由作者提供，照片来自Pixabay

那么，如果你知道了这些信息会怎样呢？

大学生细分市场往往有较高的流失率，这意味着他们在首次购买后不会再购买，导致平均每位顾客花费 $100。另一方面，工作专业人士细分市场的重复购买率较高，每位顾客平均花费 $400。

在这种情况下，你可能更愿意将更多资源投入到商务专业人士的细分市场，因为它承诺更高的投资回报率。这看起来是一个简单的概念，任何人都可以理解。然而，令人惊讶的是，大多数营销人员专注于实现每次获取成本（CPA），但他们没有考虑到长期内盈利的顾客是谁。

![](../Images/ff1f97ad3e16294afb7a08e9b97d555c.png)

图片由作者提供，照片来自Pixabay

通过调整“每次获取成本”（CPA），我们可以吸引更多高价值顾客并改善投资回报率。左侧的图表表示没有考虑 CLV 的方法。红线表示 CPA，即我们可以花费的最大成本来获取新顾客。对每位顾客使用相同的营销预算会导致对低价值顾客的过度投资和对高价值顾客的投资不足。

现在，右侧的图表显示了利用 CLV 时的理想支出分配。我们为高价值顾客设置了更高的 CPA，为低价值顾客设置了更低的 CPA。

![](../Images/1ce5265ad118b6a8775d95eb1bd56cb7.png)

图片由作者提供，照片来自Pixabay

这类似于招聘过程。如果你打算招聘前谷歌员工，提供具有竞争力的薪水是必要的，对吧？通过这样做，我们可以在不改变总营销预算的情况下获得更多高价值顾客。

# 3\. 所需数据

我介绍的 CLV 模型仅使用销售交易数据。正如你所见，我们有三列数据：customer_id、交易日期和交易金额。在数据量方面，CLV 通常需要两到三年的交易数据。

![](../Images/e21fa6fa3331c5c080bc6f6dad3dce88.png)

图片由作者提供

# 4\. 传统 CLV 公式

**4.1 CLV 建模方法**

让我们从了解计算 CLV 的两种主要方法开始：历史方法和预测方法。在预测方法下，有两种模型。即概率模型和机器学习模型。

![](../Images/a6ff97d19373dc0e4c83be5a02e70694.png)

图片由作者提供

**4.2 传统 CLV 公式**

首先，让我们考虑一个传统的 CLV 公式。在这里，CLV 可以分解为三个组成部分：平均订单价值、购买频率和顾客生命周期。

![](../Images/c53d565bec6b3c835317311da0d8e329.png)

图片由作者提供

以一家时尚公司为例，平均来看：

+   顾客每次订单消费 $100

+   他们每年购物 4 次

+   他们保持忠诚度达 3 年

在这种情况下，CLV 的计算公式为 100 乘以 4 乘以 3，即每位顾客 $1,200。这个公式非常简单，看起来很直接，对吧？然而，它也有一些局限性。

**4.3 传统 CLV 公式的限制**

![](../Images/748db88585c29f5c086b9aaaaee0899b.png)

作者提供的图片

**限制 #1: 并非所有顾客都一样**

这个传统公式假设所有顾客都是同质的，通过分配一个平均数来表示。当一些顾客进行异常大的购买时，平均数并不能代表所有顾客的特点。因此，我们需要在个体层面上进行建模，而不是将所有人聚合在一起。

**限制 #2 : 首次购买时间的差异**

假设我们使用过去12个月作为数据收集周期。

![](../Images/cd1e21953f9a99f1c7d44fa6d4b6ebb8.png)

作者提供的图片，图片来源于 Pixabay

这个人大约在一年前进行了第一次购买。在这种情况下，我们可以准确计算他每年的购买频率。是8次。

那两个顾客怎么样？一个人六个月前开始购买，另一个人三个月前开始购买。他们的购买节奏相同。然而，当我们查看过去一年的总购买次数时，它们有所不同。关键是我们需要考虑顾客的任期，即自第一次购买以来的时间长度。

**限制 #3 : 活着还是死亡？**

确定顾客何时被视为“流失”是很棘手的。对于像 Netflix 这样的订阅服务，我们可以认为顾客一旦取消订阅就已经流失。然而，对于零售或电子商务来说，顾客是否“活着”或“死亡”是模糊的。

顾客的“存活概率”取决于他们过去的购买模式。例如，如果一个通常每月购买的人在接下来的三个月内没有购买，他们可能会转向其他品牌。然而，如果一个通常每六个月购买一次的人在接下来的三个月内没有购买，也不必担心。

![](../Images/3dba306eb464405b6803a778c9576f3a.png)

作者提供的图片，图片来源于 Pixabay

# 5\. **买到死（BTYD）模型**

为了应对这些挑战，我们通常转向[“买到死”（BTYD）建模](https://en.wikipedia.org/wiki/Buy_Till_you_Die)。这种方法包括两个子模型：

1.  BG-NBD 模型：预测顾客活跃的可能性及其交易频率。

1.  Gamma-Gamma 模型：估计平均订单值。

通过结合这些子模型的结果，我们可以有效预测顾客终身价值（CLV）。

![](../Images/4d74137dd9e592c1ab0a3af3c37ea317.png)

作者提供的图片

**5.1 BG/NBD 模型**

我们认为顾客的状态有两个过程：“购买过程”，即顾客正在积极购买，以及“流失过程”，即顾客停止了购买。

在活跃购买阶段，模型通过“泊松过程”预测顾客的购买频率。

顾客在每次购买后总有可能流失。BG/NBD 模型为这种可能性分配一个概率‘p’。

请参考下图进行说明。数据表明这位客户进行了五次购买。然而，在假设下，模型认为如果客户保持活跃，他们总共会进行八次购买。但是，由于在某些时候“活跃”的概率下降，我们只看到了五次实际购买。

![](../Images/589a40c0d68cf1d19f03d9f03f27359e.png)

作者提供的图像

在被视为“活跃”时，购买频率遵循泊松过程。泊松分布通常表示随机发生事件的计数。这里，“λ”象征每位客户的购买频率。然而，客户的购买频率可能会波动。泊松分布考虑了购买频率的这种变异性。

![](../Images/fd0ddbecf6cef9adccef94e7042f4c00.png)

作者提供的图像；图表来源于维基百科

下图展示了“p”随时间的变化。随着自上次购买以来的时间增加（T=31），客户“活跃”的概率减少。当再次购买发生（大约在 T=36）时，你会注意到“p”再次上升。

![](../Images/8e1c21c76bf8e21d0d251380c7610b25.png)

作者提供的图像

这是图形模型。如前所述，它包括 λ 和 p。在这里，λ 和 p 在不同人之间有所变化。为了考虑这种多样性，我们假设 λ 的异质性遵循 Gamma 分布，而 p 的异质性遵循“Beta 分布”。换句话说，该模型使用了一个基于贝叶斯定理的分层方法，也称为贝叶斯层次建模。

![](../Images/e28328bbafd56a1495c9c256fa0a12de.png)

作者提供的图像

有关公式的详细推导，请参阅此论文：[https://www.brucehardie.com/papers/bgnbd_2004-04-20.pdf](https://www.brucehardie.com/papers/bgnbd_2004-04-20.pdf)

**5.2 Gamma-Gamma 模型**

Gamma 分布非常适合建模平均订单值，因为它自然地考虑了消费数据的连续性和严格正值特性，这些数据通常呈现右偏态。Gamma 分布由两个参数决定：形状参数和尺度参数。正如这个图表所示，通过改变这两个参数，Gamma 分布的形态可以发生相当大的变化。

![](../Images/c35786954801dafebc941d8e0903a27e.png)

作者提供的图像；图表来源于维基百科

这个图解说明了正在使用的图形模型。该模型在贝叶斯层次方法中使用了两个 Gamma 分布。第一个 Gamma 分布表示每位客户的“平均订单值”。由于这个值在客户之间有所不同，第二个 Gamma 分布捕捉了整个客户群体中平均订单值的变化。先验分布的参数 p、q 和 γ（gamma）通过使用半平坦先验确定。

![](../Images/1f4c2584fe48a9f2541d99d7c575af08.png)

作者提供的图像

有关公式的详细推导，请参考这篇论文：[https://www.brucehardie.com/notes/025/gamma_gamma.pdf](https://www.brucehardie.com/notes/025/gamma_gamma.pdf)

# 6\. 示例代码

**有用的 CLV 库**

这里，我想介绍两个用于 CLV 建模的优秀 OSS 库。第一个是 PyMC-Marketing，第二个是 CLVTools。这两个库都包含 Buy-till-you-die 模型。最显著的区别在于，PyMC-Marketing 是一个基于 Python 的库，而 CLVTools 是基于 R 的。PyMC-Marketing 是建立在流行的贝叶斯库 PyMC 上的。之前有一个知名的库叫做 ‘Lifetimes’，但 ‘Lifetimes’ 现在已经进入维护模式，因此它已经转变为 PyMC-Marketing。

**完整代码**

完整代码可以在我的 Github 上找到。我的示例代码基于 yMC-Marketing 的官方快速入门指南。

[## GitHub - takechanman1228/Effective-CLV-Modeling](https://github.com/takechanman1228/Effective-CLV-Modeling?source=post_page-----bc0730973c0a--------------------------------)

### 通过在 GitHub 上创建帐户来为 takechanman1228/Effective-CLV-Modeling 开发做出贡献。

[github.com](https://github.com/takechanman1228/Effective-CLV-Modeling?source=post_page-----bc0730973c0a--------------------------------)

**代码讲解**

首先，你需要导入 pymc_marketing 和其他库。

```py
import arviz as az
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import pymc as pm
from arviz.labels import MapLabeller

from IPython.display import Image
from pymc_marketing import clv
```

你需要下载 [“在线零售数据集”来自 “UCI 机器学习库”](https://archive.ics.uci.edu/dataset/352/online+retail)。该数据集包含来自一家总部位于英国的在线零售商的交易数据，并且是根据创作共享署名 4.0 国际（CC BY 4.0）许可授权的。

```py
import requests
import zipfile
import os

# Download the zip file
url = "https://archive.ics.uci.edu/static/public/352/online+retail.zip"
response = requests.get(url)
filename = "online_retail.zip"

with open(filename, 'wb') as file:
    file.write(response.content)

# Unzip the file
with zipfile.ZipFile(filename, 'r') as zip_ref:
    zip_ref.extractall("online_retail_data")

# Finding the Excel file name
for file in os.listdir("online_retail_data"):
    if file.endswith(".xlsx"):
        excel_file = os.path.join("online_retail_data", file)
        break

# Convert from Excel to CSV
data_raw = pd.read_excel(excel_file)

data_raw.head()
```

![](../Images/805019ed21336f546a08b99f8f7463a1.png)

**数据清理**

需要进行快速数据清理。例如，我们需要处理退货订单，筛选出没有客户 ID 的记录，并通过将数量和单价相乘来创建一个 ‘总销售额’ 列。

```py
# Handling Return Orders
# Extracting rows where InvoiceNo starts with "C"
cancelled_orders = data_raw[data_raw['InvoiceNo'].astype(str).str.startswith("C")]

# Create a temporary DataFrame with the columns we want to match on, and also negate the 'Quantity' column
cancelled_orders['Quantity'] = -cancelled_orders['Quantity']

# Merge the original DataFrame with the temporary DataFrame on the columns we want to match
merged_data = pd.merge(data_raw, cancelled_orders[['CustomerID', 'StockCode', 'Quantity', 'UnitPrice']], 
                       on=['CustomerID', 'StockCode', 'Quantity', 'UnitPrice'], 
                       how='left', indicator=True)

# Filter out rows where the merge found a match, and also filter out the original return orders
data_raw = merged_data[(merged_data['_merge'] == 'left_only') & (~merged_data['InvoiceNo'].astype(str).str.startswith("C"))]

# Drop the indicator column
data_raw = data_raw.drop(columns=['_merge'])

# Selecting relevant features and calculating total sales
features = ['CustomerID', 'InvoiceNo', 'InvoiceDate', 'Quantity', 'UnitPrice', 'Country']
data = data_raw[features]
data['TotalSales'] = data['Quantity'].multiply(data['UnitPrice'])

# Removing transactions with missing customer IDs as they don't contribute to individual customer behavior
data = data[data['CustomerID'].notna()]
data['CustomerID'] = data['CustomerID'].astype(int).astype(str)
data.head()
```

![](../Images/83830ea13d626064fdf07893f32a7b64.png)

作者提供的图片

然后，我们需要使用 ‘clv_summary’ 函数创建一个摘要表。该函数返回一个 RFM-T 格式的数据框。RFM-T 代表每个客户的近期性、频率、货币量和在职时间。这些指标在购物者分析中很受欢迎。

```py
data_summary_rfm = clv.utils.clv_summary(data, 'CustomerID', 'InvoiceDate', 'TotalSales')
data_summary_rfm = data_summary_rfm.rename(columns={'CustomerID': 'customer_id'})
data_summary_rfm.index = data_summary_rfm['customer_id']
data_summary_rfm.head()
```

![](../Images/5f4aa39e85182f00572b99583822652b.png)

**BG/NBD 模型**

BG/NBD 模型在这个库中作为 BetaGeoModel 函数提供。当你执行 `bgm.fit()` 时，你的模型开始训练。

当你执行 `bgm.fit_summary()` 时，系统会提供学习过程的统计摘要。例如，这个表格显示了参数的均值、标准差、高密度区间（HDI）等。我们还可以检查 r_hat 值，这有助于评估 Markov Chain Monte Carlo (MCMC) 模拟是否已经收敛。如果 r-hat 小于或等于 1.1，则认为是可接受的。

```py
bgm = clv.BetaGeoModel(
    data = data_summary_rfm,
)
bgm.build_model()

bgm.fit()
bgm.fit_summary()
```

![](../Images/afce79c5a5316b732e87c915fd29ae3d.png)

下面的矩阵被称为概率存活矩阵。通过这个矩阵，我们可以推断哪些用户可能会返回，哪些用户可能不会返回。X轴表示客户的历史购买频率，Y轴表示客户的最近一次购买时间。颜色表示存活的概率。我们的新客户位于左下角：低频率和高最近性。这些客户的存活概率较高。我们的忠实客户位于右下角：高频率和高最近性客户。如果他们长时间没有购买，忠实客户将成为高风险客户，其存活概率较低。

```py
clv.plot_probability_alive_matrix(bgm);
```

![](../Images/b3e29e13012ac77d73a7d1a12c31e6a4.png)

作者提供的图片

下一步，我们可以预测每个客户的未来交易。你可以使用`expected_num_purchases`函数。在模型拟合之后，我们可以询问下一期间预期的购买次数。

```py
num_purchases = bgm.expected_num_purchases(
    customer_id=data_summary_rfm["customer_id"],
    t=365,
    frequency=data_summary_rfm["frequency"],
    recency=data_summary_rfm["recency"],
    T=data_summary_rfm["T"]
)

sdata = data_summary_rfm.copy()
sdata["expected_purchases"] = num_purchases.mean(("chain", "draw")).values
sdata.sort_values(by="expected_purchases").tail(4)
```

![](../Images/7ba8489861c0c8c150165770b839dc5a.png)

**Gamma-Gamma模型**

接下来，我们将转到Gamma-Gamma模型来预测平均订单值。我们可以通过`Expected_customer_spend`函数预测预期的“平均订单值”。

```py
nonzero_data = data_summary_rfm.query("frequency>0")
dataset = pd.DataFrame({
    'customer_id': nonzero_data.customer_id,
    'mean_transaction_value': nonzero_data["monetary_value"],
    'frequency': nonzero_data["frequency"],
})
gg = clv.GammaGammaModel(
    data = dataset
)
gg.build_model()
gg.fit();

expected_spend = gg.expected_customer_spend(
    customer_id=data_summary_rfm["customer_id"],
    mean_transaction_value=data_summary_rfm["monetary_value"],
    frequency=data_summary_rfm["frequency"],
)
```

下面的图表显示了5个客户的预期平均订单值。这两位客户的平均订单值超过$500，而这三位客户的平均订单值约为$350。

```py
labeller = MapLabeller(var_name_map={"x": "customer"})
az.plot_forest(expected_spend.isel(customer_id=(range(5))), combined=True, labeller=labeller)
plt.xlabel("Expected average order value");
```

![](../Images/05bd826e68482b82c88a33692f70105a.png)

作者提供的图片

**结果**

最后，我们可以结合两个子模型来估算每个客户的CLV。我想提到的一点是参数：**Discount_rate**。该函数使用折现现金流（DCF）方法。当月折现率为1%时，一个月后的$100在今天的价值是$99。

```py
clv_estimate = gg.expected_customer_lifetime_value(
    transaction_model=bgm,
    customer_id=data_summary_rfm['customer_id'],
    mean_transaction_value=data_summary_rfm["monetary_value"],
    frequency=data_summary_rfm["frequency"],
    recency=data_summary_rfm["recency"],
    T=data_summary_rfm["T"],
    time=120, # 120 months = 10 years
    discount_rate=0.01,
    freq="D",
)

clv_df = az.summary(clv_estimate, kind="stats").reset_index()

clv_df['customer_id'] = clv_df['index'].str.extract('(\d+)')[0]

clv_df = clv_df[['customer_id', 'mean', 'hdi_3%', 'hdi_97%']]
clv_df.rename(columns={'mean' : 'clv_estimate', 'hdi_3%': 'clv_estimate_hdi_3%', 'hdi_97%': 'clv_estimate_hdi_97%'}, inplace=True)

# monetary_values = data_summary_rfm.loc[clv_df['customer_id'], 'monetary_value']
monetary_values = data_summary_rfm.set_index('customer_id').loc[clv_df['customer_id'], 'monetary_value']
clv_df['monetary_value'] = monetary_values.values
clv_df.to_csv('clv_estimates_output.csv', index=False)
```

现在，我将向你展示如何改进我们的营销行动。下面的图表显示了按国家估算的CLV。

```py
# Calculating total sales per transaction
data['TotalSales'] = data['Quantity'] * data['UnitPrice']
customer_sales = data.groupby('CustomerID').agg({
    'TotalSales': sum,
    'Country': 'first'  # Assuming a customer is associated with only one country
})

customer_countries = customer_sales.reset_index()[['CustomerID', 'Country']]

clv_with_country = pd.merge(clv_df, customer_countries, left_on='customer_id', right_on='CustomerID', how='left')

average_clv_by_country = clv_with_country.groupby('Country')['clv_estimate'].mean()

customer_count_by_country = data.groupby('Country')['CustomerID'].nunique()

country_clv_summary = pd.DataFrame({
    'AverageCLV': average_clv_by_country,
    'CustomerCount': customer_count_by_country,
})
# Calculate the average lower and upper bounds of the CLV estimates by country
average_clv_lower_by_country = clv_with_country.groupby('Country')['clv_estimate_hdi_3%'].mean()
average_clv_upper_by_country = clv_with_country.groupby('Country')['clv_estimate_hdi_97%'].mean()

# Add these averages to the country_clv_summary dataframe
country_clv_summary['AverageCLVLower'] = average_clv_lower_by_country
country_clv_summary['AverageCLVUpper'] = average_clv_upper_by_country

# Filtering countries with more than 20 customers
filtered_countries = country_clv_summary[country_clv_summary['CustomerCount'] >= 20]

# Sorting in descending order by CustomerCount
sorted_countries = filtered_countries.sort_values(by='AverageCLV', ascending=False)

# Prepare the data for error bars
lower_error = sorted_countries['AverageCLV'] - sorted_countries['AverageCLVLower']
upper_error = sorted_countries['AverageCLVUpper'] - sorted_countries['AverageCLV']
asymmetric_error = [lower_error, upper_error]

# Create a new figure with a specified size
plt.figure(figsize=(12,8))

# Create a plot representing the average CLV with error bars indicating the confidence intervals
# We convert the index to a regular list to avoid issues with matplotlib's handling of pandas Index objects
plt.errorbar(x=sorted_countries['AverageCLV'], y=sorted_countries.index.tolist(), 
             xerr=asymmetric_error, fmt='o', color='black', ecolor='lightgray', capsize=5, markeredgewidth=2)

# Set labels and title
plt.xlabel('Average CLV')  # x-axis label
plt.ylabel('Country')  # y-axis label
plt.title('Average Customer Lifetime Value (CLV) by Country with Confidence Intervals')  # chart title

# Adjust the y-axis to display countries from top down
plt.gca().invert_yaxis()

# Show the grid lines
plt.grid(True, linestyle='--', alpha=0.7)

# Display the plot
plt.show()
```

![](../Images/ab11faf31ba96af8fa215c012d66f245.png)

作者提供的图片

法国的客户通常拥有较高的CLV。另一方面，比利时的客户通常拥有较低的CLV。根据这个结果，我建议增加在法国获取客户的营销预算，并减少在比利时获取客户的营销预算。当我们用基于美国的数据进行建模时，我们会使用各州而不是国家。

# 7\. 你如何提高模型的准确性？

你可能会想知道：

+   我们能否利用其他类型的数据，例如访问日志？

+   是否可以将更多特征，如人口统计信息或营销活动，纳入模型中？

基本上，BTYD模型只需要交易数据。如果你想使用其他数据或其他特征，机器学习（ML）方法可能是一个选择。之后，你可以评估贝叶斯和ML模型的性能，选择准确性和解释性更好的模型。

下面的流程图展示了更好的CLV建模指南。

![](../Images/2be43fbfbc79f5cb3b79b28dc9baa14a.png)

图片来源：作者

首先，考虑你的数据规模。如果数据量不够大或仅有交易数据，使用 PyMC Marketing 的 BTYD 模型可能是最佳选择。即使数据量足够大，我认为一个好的方法是先从 BTYD 模型开始，如果效果不佳，可以尝试不同的方法。具体来说，如果你的首要任务是准确性而不是解释性，神经网络、XGboost、LightGBM 或集成技术可能会有帮助。如果解释性对你仍然重要，可以考虑随机森林或可解释 AI 方法。

总结来说，我建议无论如何，从 PyMC Marketing 开始是一个好的第一步！

# 8. 结论

以下是一些关键要点。

+   客户生命周期价值 (CLV) 是公司可以预期从单个客户在其整个关系期间获得的总净利润。

+   我们可以使用 BG/NBD 模型和 Gamma-Gamma 模型建立一个概率模型 (BTYD)。

+   如果你熟悉 Python，可以从 PyMC-Marketing 开始。

感谢阅读！如果你有任何问题/建议，请随时通过 [Linkedin](https://www.linkedin.com/in/hajime-takeda/) 联系我！同时，如果你能关注我在 Towards Data Science 上，我会非常开心。

# 9. 参考文献

+   **BG/NBD 模型 :** [https://www.brucehardie.com/papers/bgnbd_2004-04-20.pdf](https://www.brucehardie.com/papers/bgnbd_2004-04-20.pdf)

+   **Gamma-Gamma 模型 :** [https://www.brucehardie.com/notes/025/gamma_gamma.pdf](https://www.brucehardie.com/notes/025/gamma_gamma.pdf)

+   **PyMC Marketing :** [https://github.com/pymc-labs/pymc-marketing](https://github.com/pymc-labs/pymc-marketing)

+   **在线零售数据集 (**许可证CC BY 4.0**)**[: https://archive.ics.uci.edu/dataset/352/online+retail](https://archive.ics.uci.edu/dataset/352/online+retail)
