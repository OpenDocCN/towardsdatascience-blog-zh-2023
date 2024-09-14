# 使用信用卡交易数据掌握客户细分

> 原文：[`towardsdatascience.com/mastering-customer-segmentation-using-credit-card-transaction-data-dc39a8465766`](https://towardsdatascience.com/mastering-customer-segmentation-using-credit-card-transaction-data-dc39a8465766)

## 使用 RFM 评分建立客户细分

[](https://spierre91.medium.com/?source=post_page-----dc39a8465766--------------------------------)![Sadrach Pierre, Ph.D.](https://spierre91.medium.com/?source=post_page-----dc39a8465766--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dc39a8465766--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dc39a8465766--------------------------------) [Sadrach Pierre, Ph.D.](https://spierre91.medium.com/?source=post_page-----dc39a8465766--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dc39a8465766--------------------------------) ·阅读时间 7 分钟 ·2023 年 7 月 6 日

--

![](img/34a0d2a0111e285112b79a5f8f727038.png)

图片由 [Andrea Piacquadio](https://www.pexels.com/@olly/) 提供，来源于 [Pexels](https://www.pexels.com/photo/happy-woman-shopping-online-at-home-3769747/)

客户细分是根据历史购买模式识别客户群体的过程。例如，它可以包括识别重复/忠实客户、高消费客户、一次性或不频繁购买的客户等。细分可以使用购买频率、交易金额、购买日期等信息来创建。所有这些特征都可以用来生成具有易于解释特征的明确集群。

这些细分的特征可以为公司提供大量信息和商业价值。例如，公司可以利用这些细分群体通过有针对性的促销信息、客户保留活动、忠诚度计划、产品交叉销售等来增加收入。公司可以对客户细分群体进行有针对性的消息传递，以适应每个细分群体的需求。此外，这些细分群体还可以提供有关客户最敏感的渠道的信息，无论是电子邮件、社交媒体还是外部应用程序。公司还可以利用消费者细分进行升级销售或交叉销售。例如，他们可以为经常购买的商品提供高价选项，或为之前购买的商品提供补充产品。所有这些策略都可以用于增加收入和客户保留。

有多种技术可以用于客户细分。其中一种流行的技术是近期性、频率和货币价值（RFM）评分。

## 近期性、频率和货币价值

1.  **最近一次购买** 是指客户最后一次购买与参考日期（通常是当前日期或数据中可用的最大日期）之间的天数。

1.  **购买频率** 是指从最后一次购买到当前或数据中最大日期之间的购买次数。

1.  **货币金额** 是指从第一次购买到最后一次购买之间的总花费金额。

你可以使用这些数值来构建 RFM 分数，这些分数可以用来对客户进行分段，并识别高价值和低价值客户。这些分数可以用于多种商业用例，包括定制营销、流失分析、价格优化等。

在这里，我们将学习如何使用信用卡交易数据集计算 RFM 分数。为了我们的目的，我们将使用 [Synthetic Credit Card Transaction](https://datafabrica.info/products/customer-segmentation-credit-card-transaction-data-dining-small) 数据，这些数据可以在 [DataFabrica](http://datafabrica.info/) 上获得。数据包含合成的信用卡交易金额、信用卡信息、交易 ID 等。 [免费版](https://datafabrica.info/products/synthetic-e-commerce-data) 可以免费下载、修改和分享，使用 [Apache 2.0 许可证](https://www.apache.org/licenses/LICENSE-2.0)。

在这项工作中，我将使用 [Deepnote](https://deepnote.com/) 编写代码，这是一款协作的数据科学笔记本，可以非常方便地进行可重复实验。

## 数据探索

首先，让我们导航到 Deepnote 并创建一个新项目（如果你还没有账户，可以免费注册）。

让我们安装必要的包：

作者创建的嵌入

并导入我们将使用的包：

作者创建的嵌入

接下来，让我们将数据读入 pandas 数据框，并显示前五行数据：

作者创建的嵌入

接下来，我们将过滤数据框，只包含购买了 Chick-fil-A 的客户：

作者创建的嵌入

接下来，我们可以查看各州客户的数量。为此，我们需要将 merchant_state 映射到州缩写，这样我们就可以在 Plotly 中绘制每个州的客户数量：

作者创建的嵌入

接下来，我们将映射州缩写，并定义我们的 state_count 表。我们通过对每个州的每个持卡人执行 groupby `nunique()` 操作来完成：

```py
df['merchant_state_abbr'] = df['merchant_state'].map(state_abbreviations)
state_counts = df.groupby('merchant_state_abbr')['cardholder_name'].nunique().reset_index()
state_counts.columns = ['State', 'Customer_Count']
```

接下来，我们可以使用 Plotly Express 的 `chloropleth` 方法生成每个州客户数量的地理图：

```py
fig = px.choropleth(state_counts, locations='State', locationmode='USA-states',
                    color='Customer_Count', scope='usa',
                    color_continuous_scale='Blues',
                    title='Number of Customers by State')

fig.show()
```

完整的逻辑是：

作者创建的嵌入

## 生成 RFM 分数

现在让我们定义创建 RFM 分数的逻辑。首先将我们的 `transaction_date` 转换为 pandas datetime 对象，并定义一个 `NOW` 变量，它是最大的 `transaction_date`：

```py
df['transaction_date'] = pd.to_datetime(df['transaction_date'])
NOW = df['transaction_date'].max()
```

接下来，让我们执行一个 groupby 聚合操作，以计算最近一次购买、购买频率和货币金额。

1.  最近一次购买 — 总最大日期减去客户最大日期：`df.groupby('cardholder_name').agg({'transaction_date': lambda x: (NOW — x.max()).days})`

1.  频率 — 每个客户的交易 ID 数量：`df.groupby('cardholder_name').agg({transaction_id': lambda x: len(x)})`

1.  货币价值 — 每个客户的交易金额总和：`df.groupby('cardholder_name').agg({transaction_amount': lambda x: x.sum()})`

我们还将 `transaction_date` 转换为整数：

```py
rfmTable = df.groupby('cardholder_name').agg({'transaction_date': lambda x: (NOW - x.max()).days, 'transaction_id': lambda x: len(x), 'transaction_amount': lambda x: x.sum()})
rfmTable['transaction_date'] = rfmTable['transaction_date'].astype(int)
```

接下来，让我们适当地重命名我们的列。

1.  `transaction_date` 变为 `recency`

1.  `transaction_id` 变为 `frequency`

1.  `transaction_amount` 变为 `monetary_value`

```py
rfmTable.rename(columns={'transaction_date': 'recency', 
                         'transaction_id': 'frequency',
                         'transaction_amount': 'monetary_value'}, inplace=True)
rfmTable = rfmTable.reset_index()
```

完整逻辑是：

嵌入由作者创建

我们可以查看最近性的分布：

嵌入由作者创建

频率：

嵌入由作者创建

以及货币价值：

嵌入由作者创建

接下来，我们可以使用 Pandas `qcut` 方法计算最近性、频率和货币价值的四分位数：

```py
rfmTable['r_quartile'] = pd.qcut(rfmTable['recency'], q=4, labels=range(1,5), duplicates='raise')
rfmTable['f_quartile'] = pd.qcut(rfmTable['frequency'], q=4, labels=range(1,5), duplicates='drop')
rfmTable['m_quartile'] = pd.qcut(rfmTable['monetary_value'], q=4, labels=range(1,5), duplicates='drop')
rfm_data = rfmTable.reset_index()
```

接下来，我们可以可视化最近性/频率热图，其中每个单元格显示具有相应最近性和频率值的客户百分比。首先，让我们计算百分比：

```py
heatmap_data = rfm_data.groupby(['r_quartile', 'f_quartile']).size().reset_index(name='Percentage')
heatmap_data['Percentage'] = heatmap_data['Percentage'] / heatmap_data['Percentage'].sum() * 100
```

接下来，让我们生成我们的热图矩阵：

```py
heatmap_matrix = heatmap_data.pivot('r_quartile', 'f_quartile', 'Percentage')
```

使用 Seaborn 和 Matplotlib 生成地图并标记/标题我们的热图：

```py
sns.set()
sns.heatmap(heatmap_matrix, annot=True, fmt=".2f", cmap="YlGnBu")

plt.title("Customer Segmentation Heatmap")
plt.xlabel("Frequency Quartile")
plt.ylabel("Recency Quartile")
plt.show()
```

完整逻辑是：

嵌入由作者创建

我们可以从热图中看到以下洞察：

1.  16.21% 的客户最近购买但购买不频繁。

1.  3.45% 的客户是频繁且最近的客户。

1.  10% 的客户频繁购买但时间不长。

1.  5.86% 的客户最近没有购买，也没有频繁购买。

我们仅考虑了单一商户 Chick-fil-A，但我鼓励你对其他一些休闲和高档餐饮商户重复此分析。

接下来，我们可以通过连接最近性、频率和货币价值的四分位数来生成我们的 RFM 分数：

嵌入由作者创建

我们可以可视化我们的 RFM 分数的分布：

嵌入由作者创建

在这里我们看到，最常见的 RFM 分数是‘411’，这对应于最近的客户，花费不频繁且金额很少。

## 使用 RFM 分数生成和可视化客户分段

接下来，我们可以生成我们的客户分段。这一步有点主观，但我们定义的分段如下：

1.  高价值客户：r、f 和 m 全部 ≥ 3

1.  重复客户：f ≥ 3 且 r 或 m ≥ 3

1.  顶级花费者：m ≥ 3 且 f 或 r ≥ 3

1.  高风险客户：r、f 和 m 中两个或更多 ≤ 2

1.  非活跃客户：两个或更多 = 1

1.  其他：其他任何东西

嵌入由作者创建

接下来，我们可以查看各个分段的分布：

嵌入由作者创建

在这里我们看到，最大的客户分段是非活跃客户，其次是高风险客户、顶级花费者、重复客户和高价值客户。

还可以可视化分布：

嵌入由作者创建

我们还可以查看每个客户分段的平均货币价值：

嵌入由作者创建

我们看到高价值客户、重复客户和顶级花费者的平均货币价值最高。此外，非活跃和高风险客户的货币价值较低。

各客户细分的平均频率值：

嵌入由作者创建

我们看到优质客户、重复客户和高消费客户的频率最高，而风险客户和非活跃客户的频率较低。

最后，各客户细分的平均最近性值：

嵌入由作者创建

我们看到“其他”类别的购买最近性最高。这可能是一个适合“新”客户的细分，这些客户的购买模式尚不明确。

## 使用客户细分进行定制营销

你可以利用这些客户细分生成与每个细分市场相关的定制/个性化营销信息。例如，你可以通过特别促销和忠诚度优惠奖励优质客户。还可以根据他们的购买历史推广他们可能感兴趣的其他产品。对于重复/忠诚客户，你可以开发自动化电子邮件活动以保持他们的忠诚度。风险客户通常处于脱离状态。我们可以基于这些客户开发活动，以重新吸引他们并促使他们重新购买。对于非活跃客户，可以开发类似的活动。最后，对于高消费客户，我们可以提供特别促销和针对他们可能再次购买的高价产品的优惠。

利用这些数据，我们可以进一步分析，并使用`items`和`prices`字段为这些细分市场制定定制的营销活动。

我鼓励你从[GitHub](https://github.com/spierre91/medium_code/blob/master/datafabrica/rfm_scoring.ipynb)下载代码，并在其他餐厅商户上重复此分析。

## 结论

在这里，我们讨论了如何对合成信用卡交易数据进行客户细分。我们首先进行了一些简单的数据探索，查看了商户 Chick-fil-A 每个州的客户数量。接着，我们计算了生成 RFM 得分所需的列，包括最近性、频率和货币价值。然后，我们展示了如何为每个客户生成最近性、频率和货币价值的四分位数，并利用这些数据构建 RFM 得分。接下来，我们使用 RFM 得分生成了客户细分“优质客户”、“重复客户”、“高消费客户”、“风险客户”和“非活跃客户”。我们还生成了一些可视化图表，以便分析客户细分在数据中的分布情况。

使用 RFM 得分生成有洞察力的客户细分是任何希望创造商业价值的数据科学家的宝贵技能。构建可解释的细分并从这些细分中提取洞察力可以帮助企业设计营销活动策略，从而增加收入和客户留存。了解客户的购买行为使企业能够针对适当的客户群体量身定制促销优惠。本文提供了开始所需的基本知识！

免费版的合成信用卡数据可以在[这里](https://datafabrica.info/products/synthetic-e-commerce-data)找到。完整的数据集可以在[这里](https://datafabrica.info/products/customer-segmentation-credit-card-transaction-data-dining-small)找到。
