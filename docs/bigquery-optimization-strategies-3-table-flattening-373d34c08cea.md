# BigQuery 优化策略 3：表格扁平化

> 原文：[`towardsdatascience.com/bigquery-optimization-strategies-3-table-flattening-373d34c08cea`](https://towardsdatascience.com/bigquery-optimization-strategies-3-table-flattening-373d34c08cea)

## 关于爆炸表格和收缩数组

[](https://medium.com/@martin.weitzmann?source=post_page-----373d34c08cea--------------------------------)![Martin Weitzmann](https://medium.com/@martin.weitzmann?source=post_page-----373d34c08cea--------------------------------)[](https://towardsdatascience.com/?source=post_page-----373d34c08cea--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----373d34c08cea--------------------------------) [Martin Weitzmann](https://medium.com/@martin.weitzmann?source=post_page-----373d34c08cea--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----373d34c08cea--------------------------------) ·9 分钟阅读·2023 年 2 月 1 日

--

数组是分析数据库中最酷的功能之一，因为它可以在事件发生的地方和时间存储额外的信息。让我们先探讨一些基本示例，然后看看 Google Analytics 4 中的数组。 

![](img/076ff3f2da5c8b766f6edf93ed5c02e2.png)

图片由 [Torsten Dederichs](https://unsplash.com/@tdederichs?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

比如说，在存储销售历史时，我们可以将购买的产品与购买事件一起存储在数组中，而不是单独的表中——这样可以避免在后续分析中处理 SQL 连接的麻烦。

尽管数组的直观性不强，但我认为 SQL 连接更糟糕（主要是因为人们[使用错误的思维模型来解释它们](https://medium.com/towards-data-science/explain-sql-joins-the-right-way-f6ea784b568b)）。

在查询优化方面，处理 SQL 中的数组有一个简单规则：

+   **如果你可以对数组进行汇总——那就做吧！**

让我解释一下——当你需要从数组中获取信息时，你有两个选择：

+   *汇总* 数组 / 将其减少为一个值

+   *横向连接* 数组到表 / “扁平化” 表格

如果你只需要 *一个值*，那么进行汇总——如果你真的需要 *多个* *值*，那么过滤、再过滤，然后再进行昂贵的横向连接。

顺便说一下——如果你想了解更详细的嵌套数据查询方法，请阅读这里：

[](/https-medium-com-martin-weitzmann-bigquery-sql-on-nested-data-cf9589c105f4?source=post_page-----373d34c08cea--------------------------------) ## BigQuery: SQL on Nested Data

### BigQuery 非常强大，因为嵌套数据意味着在预先连接的表上工作。但分析师在使用它时...

[towardsdatascience.com

由于我们想专注于优化，这里只是简短版本：

# 数组聚合

这意味着将你的数组减少/压缩为一个值，即

+   从数组中选择一个值： `my_array[OFFSET(2)]`

+   使用一个进行聚合的函数：`ARRAY_LENGTH(my_array)`

+   使用自查询进行自定义聚合 `(select sum(x) from unnest([3, 6, 7, 2]) as x)`

我们稍后会查看更多示例！

# 横向连接

这些连接是横向的，因为它们将一个表与每行唯一且变化的内容结合。在我们的例子中是一个数组——它通常在每行中包含不同的值。

对于展开数组，这只是意味着你将数组中的每个值添加到父行的副本中。因此，我们从两行 …

1.  [3, 6, 8]

1.  [2, 7]

到 5 行 …

1.  3, [3, 6, 8]

1.  6, [3, 6, 8]

1.  8, [3, 6, 8]

1.  2, [2, 7]

1.  7, [2, 7]

我们将元素从数组中提取出来，但从两行增加到五行，增加了 150%！

来源：[`giphy.com/channel/ThisIsAOK`](https://giphy.com/channel/ThisIsAOK)

现在想象一下在一个超过两行的表中会发生什么——它*在大小上爆炸*，而且有人必须为这种密集计算*付出代价*！

我们真的应该尽量避免*这种类型的*横向连接*如果可以的话*! 这就是你需要在你的用例中检查的：**我能否避免使用包含数组的*lateral join*？** 或者更好的是 **我能否避免*表爆炸*？**

将一个可避免的*lateral cross join* 实现与其*数组聚合*对应物进行比较只是意味着**将预聚合推送到数组级别**同时**完全取消连接操作**。这节省了计算和缓存所需的资源（这可能溢出到磁盘并进一步降低查询执行速度）。

# 最小示例

让我们从一个包含名称和成分的两种食品的表开始：

1.  **意大利番茄面**, [意大利面, 番茄, 橄榄油, 大蒜, 罗勒, 盐]

1.  **泡菜**, [大白菜, 韩式辣椒粉, 白萝卜, 大蒜, 姜, 盐]

这是我们的提示：

+   查找所有食品中的白萝卜数量！

+   查找包含字母‘a’和‘i’的所有成分

+   查找包含字母‘a’和‘i’的所有成分

三种情况都可以通过先展平表格，然后进行分组和/或计数来解决。但让我们实际看一下——这里是 CTE 中的表：

```py
WITH foods AS (
  SELECT 'Pasta al Pomodoro' as name, ['pasta', 'tomatoes', 'olive oil', 'garlic', 'basil', 'salt'] as ingredients
  UNION ALL
  SELECT 'Kimchi' as name, ['Napa cabbage', 'Korean pepper flakes', 'daikon radish', 'garlic', 'ginger', 'salt'] as ingredients
)

SELECT 
  * 
FROM 
  foods
```

![](img/0cf18e320e2da8181d2af2b843614de9.png)

美味查询结果（作者截图）

只需*将查询粘贴*到控制台中并跟随执行。

## 所有食品中的白萝卜数量

**我们可以进行聚合吗？**

当然——理论上我们只需要从数组中获取一个信息：*它包含多少个白萝卜？* 然后将结果汇总到所有行中。

让我们从*将信息添加到每一行*开始——我放置了评论编号以显示执行顺序：

```py
SELECT 
  *, 
  ( --3\. aggregate with count and feed to parent row using 'select'
    select count(1) 
    --1\. turn array into table format using unnest()
    from unnest(ingredients) as ingr 
    --2\. filter for right conditions
    where ingr like '%daikon%' 

  ) AS qty_daikon_ingredients
FROM 
  foods
```

![](img/1a1ab9888a8f54230c053f41827fc27f.png)

惊人的聚合数组！（作者截图）

现在我们在第三列中拥有这些信息，我们可以直接在父查询中对其进行汇总！所以，我们基本上只需*将子查询包装在* `*SUM()*` 中并移除通配符 `*` …

```py
SELECT 
  SUM( --4\. aggregate sub-query results over the whole table
    ( select count(1) --3\. aggregate with count and feed to parent row 
      from unnest(ingredients) as ingr --1\. turn array into table format using unnest
      where ingr like '%daikon%' --2\. filter for right conditions
    ) 
  ) AS qty_daikon_ingredients
FROM 
  foods
```

… 最终我们得到结果`1`。试试大蒜和其他成分！

是的，SQL 最令人困惑的地方在于读取查询的顺序与执行顺序完全不同。但至少它是一致的，你可以准确知道查询的输出是什么样的。

## 查找包含字母‘a’和‘i’的所有成分

我们能聚合吗？嗯，很难，因为我们可能在数组中有多个成分，并且可能需要对所有包含‘a’和‘i’的成分进行分组——所以我们需要先“扁平化”表 …

```py
SELECT 
  *
FROM 
  foods cross join unnest(ingredients) as ingr
  -- or if you want the same FROM even shorter:
  -- FROM foods, foods.ingredients as ingr
  -- comma = cross join; <table>.<array> = contextually implied unnest operation
```

查询简短而好，但表格条目从 2 增加到 12！

![](img/c9c0be8ba43af6d998ca7f83fc42bc62.png)

表格爆炸动作即横向交叉连接（作者截图）

幸运的是，BigQuery 是列式存储，我们不再需要数组。我们还可以过滤实际条件，从而得到：

```py
SELECT 
  name, ingr -- 3\. smooth output
FROM 
  foods cross join unnest(ingredients) as ingr -- 1\. nasty, nasty
WHERE
  ingr like '%a%' and ingr like '%i%' -- 2\. filter, filter!
```

![](img/e0e048f9603f03fd58a54f2ee8833e1d.png)

输出不错——讨厌的交叉连接隐藏在后台（作者截图）

现在我们可以使用这个输出进行分组和汇总，获得更多信息——但*没有*交叉连接，我们是做不到的！你可以看到**我们改变了表的含义**：

+   *之前*：每行代表一个食物

+   *之后*：每行代表一个成分

如果你对表进行“扁平化”，请注意*表的含义如何改变*！

## 查找同时包含字母‘a’和‘i’的成分数量

现在这和我们刚才看到的非常相似，对吧？你猜对了，我们不再需要交叉连接了！因为我们可以聚合数组，对吧？我们只需在数组内进行预计数，然后汇总结果！

```py
SELECT 
  name, 
  (select count(1) 
   from unnest(ingredients) as ingr 
   where ingr like '%a%' 
    and ingr like '%i%'
  ) as qty_ingredients_a_i
FROM 
  foods
```

![](img/f677e91c2e4f439a0537bfa69136de13.png)

输出不错——不需要讨厌的交叉连接！（作者截图）

当然，你可以再次将`SUM()`包装在子查询中，得到表格聚合。但让我们看看一些实际场景。

# Google Analytics 4

在我们这里面对的模式中，我们有两种数组：

+   存储**自定义变量**及其值（非层次结构）：*event_params*，*user_properties*

+   提供**层次结构**信息：*items*

自定义变量在语义上与父行处于同一层次，我们只是不能引入新的列，因此我们将它们存储在数组中。Items 在语义上*位于*事件之下。例如，在 items 视图事件中，我们想知道用户查看的具体项目。

对于**自定义变量**，你可能*几乎总是*会使用**子查询**，因为你只想要*一个变量*。第二种情况：items 就更具挑战性了。让我们看一下这个探索性查询：

让我们找到包含项目的事件，以便更好地理解数据在实际情况中的存储方式……

```py
SELECT 
  event_name,
  -- classic example to get one variable from a set of custom variables
  ( select value.string_value 
    from unnest(event_params) 
    where key='page_title'
  ) as page_title,
  items -- just show the whole plain array
FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_20210131` 
WHERE
  ARRAY_LENGTH(items)>0 -- simple but effective aggregation
LIMIT 1000
```

![](img/f95854fcc1f7b9420cc899eba8a66c54.png)

（截图由作者提供）

假设我们想要统计名称中包含颜色‘Charcoal’的项目的查看次数。

```py
SELECT 
  -- find parameter with name page_title and return its string value
  (select value.string_value from unnest(event_params) where key='page_title') as page_title,

  -- find count of products with 'Charcoal' in their name
  (select count(1) from unnest(items) where item_name like '%Charcoal%') as charcoal_items,

  items -- just to check if we're doing it right

FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_20210131` 
WHERE
  ARRAY_LENGTH(items)>0
  AND event_name='view_item' -- put the event name as a condition
LIMIT 1000
```

![](img/f7414c8e6959140ecf626bee48d741b8.png)

子查询计数和可视计数似乎匹配（截图由作者提供）

看起来不错！让我们将其汇总为计数总和，并按*page_title*分组！

```py
-- data preparation CTE
WITH prep AS (
  SELECT 
    -- find parameter with name page_title and return its string value
    (select value.string_value from unnest(event_params) where key='page_title') as page_title,

    -- find count of products with 'Charcoal' in their name
    (select count(1) from unnest(items) where item_name like '%Charcoal%') as charcoal_items,

    -- to build a share we need a count of all items
    ARRAY_LENGTH(items) as all_items

  FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_20210131` 
  WHERE
    ARRAY_LENGTH(items)>0
    AND event_name='view_item'
)

SELECT
  page_title,
  -- build the share
  ROUND( 
    SUM(charcoal_items) / SUM(all_items) *100
  , 2)as share_viewed_charcoal_items
FROM prep
GROUP BY 1
ORDER BY 2 DESC
```

![](img/a23b4435670cb84f6f23b307fe263508.png)

为孩子们准备的 Charcoal！（截图由作者提供）

儿童专区提供了最多的 Charcoal 项目查看次数……这对某些人来说一定是重要的信息！

不管怎样，我认为我们很高兴在将这项分析扩展到过去 6 个月的全球年度比较时，仅使用了子查询，而不是交叉连接，同时我们继续添加更多管理层迫切需要的非常重要的指标，以做出非常重要的决策。

我将留给你开发 GA4 中横向交叉连接的良好查询，但我能想到的唯一用例是你需要按某个项目维度分组——比如*item_category*。

希望你喜欢这次关于在 BigQuery 中使用 SQL 优化数组查询的小之旅！查看本系列中的其他文章：

[](/bigquery-sql-optimization-2-with-temp-tables-to-fast-results-41869b15fcff?source=post_page-----373d34c08cea--------------------------------) ## BigQuery SQL 优化 2：使用临时表实现快速结果

### 何时使用临时表而不是 WITH

towardsdatascience.com [](/bigquery-sql-optimization-1-filter-as-early-as-possible-60dfd65593ff?source=post_page-----373d34c08cea--------------------------------) ## BigQuery SQL 优化 1：尽早过滤

### 让我们讨论低效的 SQL 以及如何在平面和……中使用和优化 WHERE、HAVING 和 QUALIFY 的过滤器

towardsdatascience.com

*你喜欢阅读这篇文章吗？* [*成为会员*](https://medium.com/@martin.weitzmann/membership) *并加入一个不断增长的好奇心社区！*

[](https://medium.com/@martin.weitzmann/membership?source=post_page-----373d34c08cea--------------------------------) [## 使用我的推荐链接加入 Medium - Martin Weitzmann

### 阅读 Martin Weitzmann（以及 Medium 上的成千上万其他作者）的每个故事……

medium.com](https://medium.com/@martin.weitzmann/membership?source=post_page-----373d34c08cea--------------------------------)
