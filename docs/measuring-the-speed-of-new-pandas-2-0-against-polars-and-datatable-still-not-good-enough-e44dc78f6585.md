# 测量新 Pandas 2.0 与 Polars 和 Datatable 的速度——仍然不够好

> 原文：[`towardsdatascience.com/measuring-the-speed-of-new-pandas-2-0-against-polars-and-datatable-still-not-good-enough-e44dc78f6585?source=collection_archive---------4-----------------------#2023-03-29`](https://towardsdatascience.com/measuring-the-speed-of-new-pandas-2-0-against-polars-and-datatable-still-not-good-enough-e44dc78f6585?source=collection_archive---------4-----------------------#2023-03-29)

## 尽管新的 PyArrow 后端为 Pandas 带来了令人兴奋的功能，但在速度方面仍然令人失望。

[](https://ibexorigin.medium.com/?source=post_page-----e44dc78f6585--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----e44dc78f6585--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e44dc78f6585--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e44dc78f6585--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----e44dc78f6585--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39db050c2ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmeasuring-the-speed-of-new-pandas-2-0-against-polars-and-datatable-still-not-good-enough-e44dc78f6585&user=Bex+T.&userId=39db050c2ac2&source=post_page-39db050c2ac2----e44dc78f6585---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e44dc78f6585--------------------------------) ·7 分钟阅读·2023 年 3 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe44dc78f6585&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmeasuring-the-speed-of-new-pandas-2-0-against-polars-and-datatable-still-not-good-enough-e44dc78f6585&user=Bex+T.&userId=39db050c2ac2&source=-----e44dc78f6585---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe44dc78f6585&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmeasuring-the-speed-of-new-pandas-2-0-against-polars-and-datatable-still-not-good-enough-e44dc78f6585&source=-----e44dc78f6585---------------------bookmark_footer-----------)![](img/741230d22d003ff7576c9b047ca76d76.png)

作者提供的图片，来自 Midjourney

自从人们尝试使用 `read_csv` 读取第一个大小为一吉字节的数据集时，就开始抱怨 Pandas 的速度，因为他们意识到需要等待 - *哎呀* - 五秒钟。没错，我也是那些抱怨者之一。

五秒钟可能听起来不多，但当加载数据集本身需要那么长时间时，这通常意味着随后的操作也会花费相同的时间。由于速度是在快速、粗略数据探索中至关重要的因素，你可能会感到*非常*沮丧。

出于这个原因，PyData 的团队最近宣布了计划发布的 Pandas 2.0，并将使用新推出的 PyArrow 后端。对于那些完全不了解的人来说，PyArrow 本身是一个设计用于高性能、内存高效操作数组的小巧库。

人们真诚地希望新后端能够比原版 Pandas 带来显著的速度提升。本文将通过将 PyArrow 后端与两个最快的 DataFrame 库：Datatable 和 Polars 进行比较，以检验这一丝希望。
