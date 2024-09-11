# 掌握定价优化的艺术 — 一种数据科学解决方案

> 原文：[https://towardsdatascience.com/mastering-the-art-of-pricing-optimization-a-data-science-solution-eb8befb79425?source=collection_archive---------0-----------------------#2023-08-28](https://towardsdatascience.com/mastering-the-art-of-pricing-optimization-a-data-science-solution-eb8befb79425?source=collection_archive---------0-----------------------#2023-08-28)

## 解锁零售定价优化中的真实世界数据科学解决方案的秘密

[](https://rhydhamgupta.medium.com/?source=post_page-----eb8befb79425--------------------------------)[![Rhydham Gupta](../Images/cd49324ddce6d950f2c7803b53b3741f.png)](https://rhydhamgupta.medium.com/?source=post_page-----eb8befb79425--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eb8befb79425--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----eb8befb79425--------------------------------) [Rhydham Gupta](https://rhydhamgupta.medium.com/?source=post_page-----eb8befb79425--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F62e949c6123b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-the-art-of-pricing-optimization-a-data-science-solution-eb8befb79425&user=Rhydham+Gupta&userId=62e949c6123b&source=post_page-62e949c6123b----eb8befb79425---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb8befb79425--------------------------------) ·16 min 阅读·2023年8月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Feb8befb79425&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-the-art-of-pricing-optimization-a-data-science-solution-eb8befb79425&user=Rhydham+Gupta&userId=62e949c6123b&source=-----eb8befb79425---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Feb8befb79425&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-the-art-of-pricing-optimization-a-data-science-solution-eb8befb79425&source=-----eb8befb79425---------------------bookmark_footer-----------)![](../Images/8041d022ac7a7b13705f33a4e5d6592d.png)

作者提供的图片

目录：

[**1\. 概述**](#df73)[**2\. 弹性建模**](#ff4f)[**3\. 优化**](#03a7)

# 1\. 概述

定价在商业世界中扮演着至关重要的角色。平衡销售和利润对于任何业务的成功至关重要。我们如何以数据科学的方式来实现这一目标？在本节中，我们将建立一个有效的数据科学解决方案的直觉，然后深入了解每个组件的细节和代码。

注意 — 尽管有不同类型的定价策略，但在本文中，我们将重点关注为拥有足够价格变动历史数据的传统业务/成熟品牌建立定价策略。在详细讨论之前，让我们先看看我们尝试遵循的基本方法 —

![](../Images/123637b88ef1ee7bcc0d90641ecedeac.png)

图片由作者提供

我们已经为商品1绘制了销售和价格图。在过去的9个月里，价格发生了2次变动，我们可以清楚地看到销售的影响。当价格较低时，销售额较高。现在的问题是如何**量化过去价格变动对销售的影响，并预测未来商品的最佳价格。**

从1月至4月的一个有趣观察是，价格固定在$5，但我们仍然观察到销售波动。这是非常正常的，因为在实际世界中，有许多外部因素会影响销售，如季节性、节假日、促销活动、市场营销支出等。**因此，我们不对实际销售进行建模，而是使用不同模型推导出的基线销售。**

![](../Images/9fd3ee50e2685162b78495491512f47e.png)

图片由作者提供

你可以观察到，我们正在查看基线销售系列中的平滑销售趋势。它是100%准确的吗？当然不是！数据科学的关键在于我们能接近现实的程度。现在让我们进入过程 —

假设我们被聘请并要求为Retailmart集团的数千种商品提供价格。这些商品在不同商店的价格可能不同。公司提供了过去5年的数据。我们应该采用什么方法来解决这个问题？

让我们通过一个价格计量器的例子来理解这一点。假设我们有一个价格计量器，我们已经固定了最低和最高值，并且拨盘可以在这两个极端之间移动。目前，拨盘指向当前价格。我们的目标是将拨盘停在一个可以最大化利润的点上。

现在，当我们将调整拨盘向右移动（即我们在提高价格）时，销售将开始下降，利润率将增加，但

1.  我们可以量化这种下降吗？是的，我们可以，这被称为项目的价格弹性。简单来说，**价格弹性**表示价格变动1%时，销售额的百分比变化。

1.  在现实世界中，销售往往受到促销活动、假期、额外折扣等因素的驱动，但为了优化价格，我们需要排除所有这些外部因素的影响，并计算**基线销售**。

1.  一旦我们量化了销售变化与价格变化的关系，**我们需要的答案是，我应该在哪儿停下调整？** 为此，我们需要一个目标，通常是最大化利润。利润 = 销售额 * 利润率，因此我们需要在利润值达到最大的位置停下。从数学上讲，这是一种非线性优化的概念，其中值可以在范围内移动。

1.  商业规则很重要，我们必须确保最终推荐的价格符合这些规则。

所以这些是我们将遵循的主要步骤，以为给定商店中的每件商品确定正确的价格。让我们更详细地了解这些步骤 —

## 1\. 基线销售/基本单位

这一步是后续步骤的前提步骤。如前所述，我们希望建模价格变化对销售的影响。理想情况下，销售只会受到价格变化的影响，但在实际情况下，情况从未如此 —

所以我们希望模拟我们理想情况下的销售，并通过下面的方程式使用时间序列模型来实现 —

> 销售 ~ 函数[基线销售 + （促销效果） + （假期效果） + （其他任何效果）]

请注意，有时我们没有实际的数据来反映影响销售的外部因素。在这种情况下，我们可以使用虚拟变量来考虑所有这些因素。例如，如果在某个月我们看到销售突然增加但价格保持不变，我们可以引入一个简单的虚拟变量，在该月标记为1，其余月份标记为0。

## 2\. 价格弹性

价格弹性是指销售量相对于某商品价格变化的百分比变化。

举个例子，考虑两种产品牛奶和ABC绿茶。你认为哪一个会有较高的价格弹性？

牛奶作为一种必需的日常商品，竞争激烈，显示出较高的价格弹性。即使是价格的微小变化也能显著影响销售，因为其需求广泛。另一方面，ABC绿茶可能仅在少数商店有售，价格弹性较低。由于其市场定位，小幅度的价格变化不太可能对销售产生重大影响。

我们将如何建模这个？

> 基线销售 ~ 函数[（价格） + 趋势]

价格变量的系数将用作价格弹性。趋势变量用于考虑由于长期趋势而导致的销售增长，而不一定是由于价格变化。我们将在下面的价格弹性部分详细讨论如何计算弹性。

## 3\. 在范围内的非线性优化

在这一步中，我们将得到旋钮应该停在哪儿的答案。

我们首先定义我们的目标函数——**最大化利润**

然后我们定义价格计量器的起点和终点，这定义了价格的下界（LB）和上界（UB）

我们已经计算了基线销售和价格弹性，这些量化了销售对价格的敏感性。我们将把所有这些输入放入我们的非线性优化函数中，得到优化后的价格。

用非常简单的话来说，算法将在范围内尝试不同的价格点，并检查目标函数的值，在我们的案例中是利润。它会返回到能够为我们的目标函数获得最大值的价格点。（在线性优化中，可以想象一下梯度下降的工作方式）。我们将在下面的优化部分中讨论计算优化价格的更多细节。

## 4\. 商业规则

那么我们可以直接在我们的商店里实施优化后的价格吗？

不能，但现在还剩下什么？遵循商业规则是任何业务最重要的要求之一。

但我们讨论的定价规则是什么——

1.  结束数字规则——通常我们会将产品定价为$999或$995，而不是$1000。这样做有几个心理学原因，因此我们需要确保最终推荐的价格遵循这些规则（如适用）。

1.  产品差距规则——你能否以单个包装的Maggi价格高于4包装的Maggi？不能，对吧。通常情况下，如果包装的数量增加，每单位的成本应该下降，或至少保持不变。

这些是业务希望应用的一些商业规则示例。我们将对优化价格进行一些后处理步骤，以获得最终推荐价格。

现在你了解了整体过程，是时候深入探讨更多细节和编码了。

# 2\. 弹性建模

在本节中，我们将了解如何利用这个概念来推导出在多个商店中1000多件商品的优化价格。假设我们需要确定过去3年在加州商店销售的零食Yochips的价格弹性。让我们首先看看**价格弹性**的定义：

> 价格弹性定义为价格变化1%时，销售额的百分比变化。

现在你可能在想，我可以使用哪个算法来计算像Yochips这样的商品的价格弹性？

让我们深入探讨经济学书中的**常数价格弹性模型**，看看是否能将其与一些数据科学算法相关联。

需求函数的乘法形式将是：-

> Yi = α*Xi^β（其中 y 为销售/需求，x 为价格，β 为价格弹性）

对两边取对数

> log(Yᵢ) = log(α*Xᵢ^β)
> 
> log(Yᵢ) = log(α) + β*log(Xᵢ) ……….Eq(1)

log(α) 可以视为截距，类似于 β₀

> log(Yᵢ) = β₀ + β₁*log(Xᵢ) ………….Eq(2)

现在对两边进行微分，我们将得到

> δY/Y = β₁*δX/X

左侧的项表示Y的百分比变化，即销售额的百分比变化，而右侧的项表示价格的百分比变化。现在当

%价格变化 = 1%；则 δX/X = 1

**δY/Y = β₁**

这意味着销售额的百分比变化将是**β₁，这就是我们的弹性。**

现在，如果你注意到的话，方程2是一个**回归方程，其中对数（销售）对数（价格）进行回归，对数（价格）的系数将是我们的价格弹性。**

太棒了！现在我们知道计算弹性就像训练一个回归模型一样简单。

但还有一个问题。需求函数方程有一些假设，其中之一是销售仅受价格影响，但在现实世界中通常并非如此，因为销售通常受促销、假期、活动等多个因素的影响。那么解决方案是什么？我们需要计算销售组件，从而去除所有这些额外事件的影响。

有一点需要澄清的是，基础销售指的是单位销售，而不是美元销售。因此，在方程2中，我们需要将价格对基础单位进行回归，而不是实际单位销售。那么问题是我们如何从实际销售单位中推导出基础单位？

我们通过一个例子来理解。下面你可以看到销售单位和销售价格的时间序列的每周图：

![](../Images/f39c43935b6492d28c4309658055a0b3.png)

作者提供的图片

你能在上面的图中看到任何模式吗？这很难判断，因为销售单位系列中有太多波动。这些波动可能由多个因素造成，如假期、促销、活动、FIFA世界杯等。为了隔离价格变化的影响，我们需要计算排除这些额外因素影响的销售数据。

使用 prophet 模型，我们可以分解时间序列并提取代表基础销售的趋势组件。通过应用这一技术，我们将价格变化的长期影响与其他短期影响分开。让我们看看我们在说什么：

![](../Images/1e1908852be7d6620f9a864793c7613a.png)

作者提供的图片

在上面的图中，我们将原始对数销售单位（灰色）分解为对数基础单位（黄色线图）

这里有一个代码，你可以用它来分解时间序列并获取将成为基础销售的趋势组件：

```py
# Defining the inputs
timestamp_var = "week_ending_sunday"
baseline_dep_var = "ln_sales"
changepoint_prior_scale_value = 0.3
list_ind_vars_baseline = ['event_type_1_Cultural','event_type_1_National','event_type_1_Religious','event_type_1_Sporting']
```

```py
# Preparing the datasecloset
df_item_store = df_item_store.rename(columns={timestamp_var: 'ds', baseline_dep_var: 'y'})
df_item_store['ds'] = pd.to_datetime(df_item_store['ds'])

# Initializing and fitting the model
model = Prophet(changepoint_prior_scale= changepoint_prior_scale_value) #Default changepoint_prior_scale = 0.05

# Add the regressor variables to the model

for regressor in list_ind_vars_baseline:
    model.add_regressor(regressor)
model.fit(df_item_store)

# Since we are only decomposing current time series, we will use same data is forecasting that was used for modelling
# Making predictions and extracting the level component
forecast = model.predict(df_item_store)
level_component = forecast['trend']
```

以下是我们需要定义的输入：

+   changepoint_prior_scale_value — 这控制趋势的平滑性。你可以在 prophet 模型文档中阅读更多关于它的信息。

+   list_ind_vars_baseline — 这些包括所有影响销售的额外事件，如节日、体育赛事、文化活动等。

changepoint_prior_scale_value 如何影响趋势：当值较小时，它会导致几乎是一条直线；当值较高时，趋势就不那么平滑了。

![](../Images/fdfab52f476276fa257fd3cd9a158b74.png)

代码很简单，首先，我们将“ln_sales”变量重命名为“y”，将“week”变量重命名为“ds”，以满足使用 Prophet 模型的前提条件。接下来，我们初始化 Prophet 模型，指定“changepoint_prior_scale”参数。随后，我们将额外的事件和假期变量纳入模型。最后，我们使用训练模型的同一数据集生成预测，并提取趋势成分。

很好。我们现在有了基础单位系列，我们可以在基础单位（因为我们分解了 log_base_units 系列，已经是对数尺度）和 log(price) 之间拟合线性回归模型。下面是方程：-

> log(base units) = 截距 + 弹性*log(price)

使用上述方程，我们可以计算弹性值。在实际操作中，并不是所有的系列都适合建模，因此你可能会遇到一些不同于预期的弹性值。那解决方案是什么呢？如果我们能以某种方式在弹性值上施加约束进行回归该怎么办？答案是使用优化函数。

对于任何优化，基本要求如下：-

1.  目标函数 —— 这是我们尝试最小化/最大化的方程。在我们的案例中，它将是我们在线性回归中使用的损失函数 MSE (pred-actual => [**截距** + **弹性** *ln_price — 实际值]²)

1.  我们尝试优化的参数的初始值，在我们的案例中是截距和弹性。这些初始值可以是任何随机值。

1.  参数的边界，即截距和弹性的最小值和最大值

1.  优化算法 —— 这取决于库，但你可以使用默认设置，这应该能给你正确的结果

现在让我们来看一下代码：-

```py
# Preparing the matrix to feed into optimization algorithm
x = df_item_store_model
x["intercept"] = 1
x = x[["intercept","ln_sell_price","ln_base_sales"]].values.T
# x_t = x.T

actuals = x[2]
```

```py
from scipy.optimize import minimize

# Define the objective function to be minimized
def objective(x0):
    return sum(((x[0]*x0[0] + x[1]*x0[1]) - actuals)**2) # (intercept*1 + elasticity*(ln_sell_price) -ln_base_sales)^2

# Define the initial guess
x0 = [1, -1]

# Define the bounds for the variables
bounds = ((None, None), (-3,-0.5))

# Use the SLSQP optimization algorithm to minimize the objective function
result = minimize(objective, x0, bounds=bounds, method='L-BFGS-B')

# Print the optimization result
print(result)

# Saving the price elastitcity of an item in the dataframe
price_elasticity = result.x[1]
df_item_store_model["price_elasticity"] = result.x[1]
```

注意，我们为截距定义了初始参数值为 1，为弹性定义了初始参数值为 -1\。截距没有定义边界，而弹性则定义了 (-3,-0.5) 的边界。这就是我们通过优化函数进行回归的主要原因。在运行优化后，我们保存了优化后的价格弹性参数值。太棒了！我们已经计算出价格弹性！

**因此，我们在加利福尼亚商店的 Yochips 价格弹性为 -1.28。**

让我们看看其他系列的价格弹性：-

**低价格弹性**：价格上涨时销售额没有显著变化。下面是一个价格弹性为 -0.5 的项目的图表

![](../Images/0c8e9f123b595309d43427a87f59afc7.png)

作者提供的图片

**中等价格弹性**：价格上涨时销售额有适度下降。下面是一个价格弹性为 -1.28 的项目的图表

![](../Images/186e10cd8a574a963eb6ff10ed3e248e.png)

作者提供的图片

**高价格弹性**：价格上涨时销售额有很大下降。下面是一个价格弹性为 -2.5 的项目的图表

![](../Images/f1ecc0907db96f1b9c2125c147be5c64.png)

作者提供的图片

使用相同的方法，我们可以计算所有物品的价格弹性。在下一篇文章中，我们将探讨如何利用这些弹性值来确定每个物品的优化价格。

# 3\. 优化

在前一部分中，我们已经确定了Yochips在加州商店的价格弹性。但这对商店经理并没有帮助，他想知道如何调整Yochips的价格以最大化收益。在这篇文章中，我们将了解优化价格的方法。

但在此之前，我们必须向商店经理提出一些问题。

问：在优化价格时，是否应考虑最低和最高价格变动的限制？

根据与商店经理的讨论，我们确定价格下降不得超过20%，价格上涨也应限制在20%以内。

目前Yochips的价格是$3.23，现在我们知道优化价格必须在$2.58 — $3.876之间。但我们如何得出优化价格呢？

但是我们如何得出一个最大化收益的优化价格呢？让我们做些数学运算：-

> 优化收益 = 总售出单位 * (**优化价格**)

我们需要优化价格以最大化收益。但总售出单位也会随着价格变化而变化。让我们重新写一下上述方程，并将总售出单位称为优化单位：-

> 优化收益 = 优化单位* (**优化价格**)……………(eq1)

我们已经知道 —

> 弹性 = 售出单位的百分比变化/价格的百分比变化

因此：-

> 优化单位 = 基准单位 + 优化价格下的单位变化

这里，基准单位指的是当前价格为$2.58时的总销售单位

> 优化单位 = (基准单位 + (基准单位 * 价格弹性 * (优化价格与常规价格的百分比变化) ………. (eq2)

让我们将eq2代入eq1中

> 优化收益 = (基准单位 + (基准单位 * 价格弹性 * (**优化价格与常规价格的百分比变化**) * (**优化价格**) …….. (eq3)
> 
> 优化收益 = (基准单位 + (基准单位 * 价格弹性 * [(**优化价格** — 当前价格)/ 当前价格]* (**优化价格)**…………..eq(4)

以下是优化方程（eq4）中的关键参数：

**基准单位** = 当前价格下的平均销售单位。

**价格弹性** = 计算得出的物品价格弹性值

**当前价格** = 最新销售价格

很好！在我们的方程式中，除了优化价格外，我们还有所有其他变量的数据。那么我们可以使用哪种算法来计算一个最大化收益的优化价格？**我们可以简单地使用优化算法。**

优化所需的关键组件是什么：-

1.  **需要最小化/最大化的目标函数**:- 我们已经定义了目标函数，即最大化优化收益，如方程(eq4)所定义。

1.  **边界**：根据店铺经理的定义，我们需要优化价格变化不超过20%。因此，下限 = 当前价格（1–0.2），上限 = 当前价格（1+0.2）

1.  **优化算法**：我们将使用Python中的Scipy.optimize库来实现优化。

让我们看看代码：-

```py
# Taking latest 6 weeks average of the base sales
#--------------------------------------------------

# Ranking the date colume
df_item_store_optimization["rank"] = df_item_store_optimization["ds"].rank(ascending=False)

# Subset latest 6 weeks of data
base_sales_df = df_item_store_optimization.loc[df_item_store_optimization["rank"] <= 6].groupby("id")["base_sales"].mean().reset_index()

df_item_store_optimization_input.rename(columns = {"base_sales":"base_units"}, inplace=True)

# Deriving the min and max bound for the sell_price
#--------------------------------------------------

# Creating UB and LB as with the range of 20%
df_item_store_optimization_input["LB_price"] = df_item_store_optimization_input["sell_price"] - (0.2*df_item_store_optimization_input["sell_price"])
df_item_store_optimization_input["UB_price"] = df_item_store_optimization_input["sell_price"] + (0.2*df_item_store_optimization_input["sell_price"])
```

上述代码帮助我们准备数据以进行优化。首先，我们将计算基准单位，即最近6周**base_sales**（分解系列的趋势成分）的平均值。我们已经在 [t](https://medium.com/p/98f4f0514bfc/edit)节中讨论了计算**base_sales**的方法。

接下来，我们通过分别从当前销售价格减少和增加20%来定义**LB_price**和**UB_price**。

让我们定义执行优化的代码。

```py
from scipy.optimize import minimize

# Define the objective function to be minimized
def objective(opti_price):

    df_item_store_optimization_input["opti_price"] = opti_price
    df_item_store_optimization_input["optimized_units"] = df_item_store_optimization_input["base_units"] + (df_item_store_optimization_input["base_units"]*\
                                                                                                        ((df_item_store_optimization_input["opti_price"]/df_item_store_optimization_input["sell_price"]) - 1)*\
                                                                                                       (df_item_store_optimization_input["price_elasticity"]))

    df_item_store_optimization_input["optimized_revenue"] = df_item_store_optimization_input["optimized_units"]*df_item_store_optimization_input["opti_price"]

    return -sum(df_item_store_optimization_input["optimized_revenue"])

# Define the initial guess
opti_price = df_item_store_optimization_input["sell_price"][0]

# Define the bounds for the variables
bounds = ((df_item_store_optimization_input["LB_price"][0], df_item_store_optimization_input["UB_price"][0]),)

# # Use the optimization algorithm to minimize the objective function
result = minimize(objective, opti_price, bounds=bounds)

# Print the optimization result
print(result)
```

上述代码将为我们提供优化价格。你能猜到在目标函数中为什么定义了负优化收益吗？-(-1)是什么？是1。我们在最小化目标函数，使用负号表示优化收益将导致最大化优化收益。

此外，我们可以用任何随机值初始化**opti_price**变量，仅仅为了促进快速收敛，我们将其初始化为当前的**sell_price**。在边界中，我们定义了在上述代码中创建的**LB**和**UB**。

万岁！我们已经找到了Yochips的优化价格，并准备向加州的店铺经理提议。

![](../Images/3f7bfed7f50f4c6a3e4483ff3ef0308a.png)

图片由作者提供

我们的建议是将Yochips的价格降低10.2%至$2.9。这将带来最大的收益。

这是价格优化方法的最后一步，这种整体方法非常强大，可以帮助我们为每个店铺的每个商品返回优化价格。

上述方法的一个局限性是对于我们没有足够价格变动历史的商品。在这种情况下，我们使用其他技术，但如果这种商品的比例较少，则可以使用类别级别的平均价格弹性。

希望你喜欢这篇文章！现在你知道如何进行价格优化了。

![](../Images/27449dfeab75a88bfc44f6ea5dbf12e7.png)

图片由作者提供（字体由 [onlinewebfonts](http://www.onlinewebfonts.com) 制作）
