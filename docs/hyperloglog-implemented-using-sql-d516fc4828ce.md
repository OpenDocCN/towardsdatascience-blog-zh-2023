# 使用 SQL 实现的 HyperLogLog

> 原文：[https://towardsdatascience.com/hyperloglog-implemented-using-sql-d516fc4828ce?source=collection_archive---------16-----------------------#2023-01-23](https://towardsdatascience.com/hyperloglog-implemented-using-sql-d516fc4828ce?source=collection_archive---------16-----------------------#2023-01-23)

## 我们查看一个完全使用声明式 SQL 编写的 HyperLogLog 基数估计算法的实现

[](https://medium.com/@dhruvbird?source=post_page-----d516fc4828ce--------------------------------)[![Dhruv Matani](../Images/d63bf7776c28a29c02b985b1f64abdd3.png)](https://medium.com/@dhruvbird?source=post_page-----d516fc4828ce--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d516fc4828ce--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d516fc4828ce--------------------------------) [Dhruv Matani](https://medium.com/@dhruvbird?source=post_page-----d516fc4828ce--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F63f5d5495279&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhyperloglog-implemented-using-sql-d516fc4828ce&user=Dhruv+Matani&userId=63f5d5495279&source=post_page-63f5d5495279----d516fc4828ce---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d516fc4828ce--------------------------------) ·6 min read·2023年1月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd516fc4828ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhyperloglog-implemented-using-sql-d516fc4828ce&user=Dhruv+Matani&userId=63f5d5495279&source=-----d516fc4828ce---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd516fc4828ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhyperloglog-implemented-using-sql-d516fc4828ce&source=-----d516fc4828ce---------------------bookmark_footer-----------)![](../Images/31dfa44e59ebefd0b15bd9d0057b2f7b.png)

照片由 [Vidar Smits](https://unsplash.com/es/@vidarsmits?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

[HyperLogLog](https://en.wikipedia.org/wiki/HyperLogLog) 算法是一种极为流行的算法，用于估算（近似）给定数据集中的唯一元素数量。这与 SQL 中的精确唯一计数有所不同。

## COUNT(DISTINCT x)

1.  执行*准确*唯一计数。

1.  使用*O(唯一元素)* 额外内存。

## HyperLogLog

1.  执行*近似*唯一计数。

1.  使用*O(1)*额外内存。当你需要跟踪数百万或数十亿个唯一值时，这大大减少了内存需求。

如果你想了解关于LogLog和HyperLogLog算法的详细信息，可以在[这里](/hyperloglog-a-simple-but-powerful-algorithm-for-data-scientists-aed50fe47869)找到。

# HyperLogLog工作原理的基本直觉

问：你需要多少次抛硬币才能得到3次连续的正面？

我们期望在抛一次硬币时看到以下结果。

```py
H
T
```

我们期望在抛两次硬币时看到以下结果。

```py
HH
HT
TH
TT
```

我们期望在抛三次硬币时看到以下结果。

```py
HHH
HHT
HTH
HTT
THH
THT
TTH
TTT
```

期望你需要大约8次（2³）抛硬币才能看到3次连续的正面。

问：你需要多少次抛硬币才能得到4次连续的正面？

同样，期望你需要大约16次（2⁴）抛硬币才能看到4次连续的正面。

## 输入预处理

对你的每个输入元素使用一个生成64位哈希的哈希算法进行哈希，确保哈希结果是随机且均匀的。也就是说，任何位上的0（或1）的概率是1/2。

现在，从这个比特串的最低有效位（BIT）开始，计数连续0位的数量。这应该让你想起我们上面讨论的连续正面问题。运用相同的直觉，我们期望每次看到2^K个唯一哈希时都会看到一串K个0。我们假设每个元素哈希到一个唯一的哈希值，并且哈希值具有随机位流分布。

## 第一版

![](../Images/17898d4a0ef823838822266e3d7efe0b.png)

示例比特串（作者提供的图片）

我们只跟踪最长的比特位置（从位置1开始），即一串连续0结束的位置。也就是说，如果我们看到一些数字在比特位置3、2、2、2、1、4有1，那么我们只跟踪4。

看到的唯一值数量的近似值是8。

## 缺点

上述方法的主要缺点是：

1.  我们只能估算2的幂的数量。也就是说，一旦看到一串零，我们估计数量为2^K。我们不能估算2个连续的2的幂之间的任何数量。这对于较大的2的幂来说范围相当广泛。

1.  我们可能会非常不幸地看到一串（比如）6个零，仅仅代表1个唯一元素。这意味着我们会估计看到2⁶ = 256个唯一元素，而实际上我们只看到1个。

## 第二版（LogLog估算器）

为了解决上述两个缺点，我们可以进行2个更改。

1.  维护2^B个计数器，而不仅仅是一个计数器。每个计数器由对相同输入应用的不同哈希更新。如果假设输入中共有N个元素，那么每个计数器大致计数N/(2^B)个输入元素中的唯一元素数。

1.  计算2^K作为唯一元素的数量时，取所有2^B个计数器的平均值，然后计算2^avg(K)。将结果乘以2^B（桶的数量）。

作为优化，我们可以取哈希的最后B位，并用它来计算桶，然后使用其余的位（右移B位）如上面所述。

![](../Images/03693667883b3f13622b3c1e3e0694df.png)

将位串分成桶和随机哈希位串（作者图像）

![](../Images/5976fa0fcd0bbb639d25f3ff90757be6.png)

每个桶包含一个LogLog估计器（作者图像）

这基本上是[LogLog 估计器](https://www.ic.unicamp.br/~celio/peer2peer/math/bitmap-algorithms/durand03loglog.pdf)。

小知识：这叫做LogLog，因为它需要*log2(log2(n))* 位来存储表示数字“n”所需的位数。例如，如果n == 2^b，则“n”有“b”位，而b == log2(n)。我们需要log2(b) == log2(log2(n))位来存储值“b”。“b”也表示在其后的所有位都为0的情况下，可以有1位的最大位置（这是我们要测量的每个哈希的情况）。

## 第三版（HyperLogLog）

我们不是取值的算术均值（平均值）再平方，而是取值的[调和均值](https://en.wikipedia.org/wiki/Harmonic_mean)。

> 由于一组数字的调和均值强烈趋向于列表中的最小元素，它比算术均值更能减轻大异常值的影响，而加重小异常值的影响。

就是这样！

# SQL输入模式

输入表非常简单。基本上是一个包含单一整数列（输入整数）的表。

```py
CREATE TABLE input_data(n INT NOT NULL);
```

# SQL中的HyperLogLog

```py
INSERT INTO input_data

WITH RECURSIVE fill_n(ctr, n) AS (
  SELECT 4000, (RANDOM() * 2000)::INT

  UNION ALL

  SELECT ctr-1, (RANDOM() * 2000)::INT
  FROM fill_n
  WHERE ctr > 0
)

SELECT n FROM fill_n;

SELECT COUNT(DISTINCT n) AS num_unique_actual FROM input_data;

WITH hashed_list AS (
  SELECT
    ('x' || SUBSTR(md5(n::TEXT), 1, 16))::BIT(64)::BIGINT AS h
  FROM input_data
),

bucketed AS (
  SELECT
    -- Keep 64 buckets
    h & (63::BIGINT) AS bucket_id,
    (h >> 6) & 2147483647::BIGINT AS h_key
  FROM hashed_list
),

loglog_mapped AS (
  SELECT
    bucket_id,
    -- The following computes the position of the first 1 bit
    -- when looking at the number from right (least significant
    -- bit) to left.
    LOG(h_key & -h_key) / LOG(2.0) AS fsb,
    h_Key
  FROM bucketed
),

max_in_bucket AS (
  SELECT
    bucket_id,
    -- Retain the largest bit position in any bucket.
    MAX(fsb) AS fsb
  FROM loglog_mapped
  GROUP BY 1
),

harmonic_mean_mapped AS (
  SELECT
    AVG(fsb) AS avg_fsb,
    COUNT(1) / SUM(1.0 / (CASE WHEN fsb = 0 THEN 1 ELSE fsb END)) hm_fsb,
    -- Number of unique elements using the LogLog estimator.
    COUNT(1) * POW(2, AVG(fsb)) / 0.77351 AS num_unique_log_log,
    -- Number of unique elements using the HyperLogLog estimator.
    COUNT(1) * POW(
      2, COUNT(1) / SUM(1.0 / (CASE WHEN fsb = 0 THEN 1 ELSE fsb END))
    ) / 0.77351 AS num_unique_hll
  FROM max_in_bucket
  WHERE fsb > 0
)

SELECT * FROM harmonic_mean_mapped;
```

这是运行上述查询的结果。

![](../Images/944c1314fe01669cc96b64dee979d10e.png)

实际与估计的唯一数字计数（作者图像）

**误差率：** 输入表中唯一元素的实际数量是1729。LogLog估计器在这种情况下相差很大，估计为2795个唯一元素。[HyperLogLog](https://en.wikipedia.org/wiki/HyperLogLog)估计器的误差为3.6%，估计为1792个唯一元素。

通过使用更多的桶，我们可以得到更好的估计（更小的误差）。

**桶：** 上述实现使用了64个桶。由于我们可以在一个8位（单字节）数字中实际存储0到63之间的任何数字，因此我们为上面显示的实现使用了64个字节。

根据上述维基百科文章，

> HyperLogLog算法能够估计> 109的基数，典型准确度（标准误差）为2%，使用1.5 kB的内存。

**空间：** 上述实现由于查询编写方式使用了O(n)的额外空间。实际（非玩具）实现不会存储计算出的哈希，而只是更新内存中的桶。

**哈希计算：** 我们在上述查询中使用了md5，但你可以使用任何生成均匀随机64位数字的哈希。

关于校正因子常数0.77351的一些细节我为了简洁起见省略了。你可以阅读论文以了解有关此常数的详细信息。

# SQL Fiddle

上述查询的 SQL Fiddle 可以在[这里](http://sqlfiddle.com/#!17/e783f4/103)找到。

# 结论

HyperLogLog 算法既简单又强大，同时每个存储位能存储大量信息！

之前的文章: [使用 SQL 验证字符串是否为 HTML](https://medium.com/towards-data-science/validate-a-string-as-html-using-sql-d70e81149a2)
