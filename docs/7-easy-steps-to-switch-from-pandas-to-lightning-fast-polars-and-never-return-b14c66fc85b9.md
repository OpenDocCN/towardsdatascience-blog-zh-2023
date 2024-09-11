# 7 个简单步骤，将 Pandas 切换到 lightning fast Polars 并且永不返回

> 原文：[`towardsdatascience.com/7-easy-steps-to-switch-from-pandas-to-lightning-fast-polars-and-never-return-b14c66fc85b9?source=collection_archive---------3-----------------------#2023-04-03`](https://towardsdatascience.com/7-easy-steps-to-switch-from-pandas-to-lightning-fast-polars-and-never-return-b14c66fc85b9?source=collection_archive---------3-----------------------#2023-04-03)

## 一个将最常见 Pandas 操作转换为 Polars 的备忘单

[](https://ibexorigin.medium.com/?source=post_page-----b14c66fc85b9--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----b14c66fc85b9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b14c66fc85b9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b14c66fc85b9--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----b14c66fc85b9--------------------------------)

·

[阅读](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39db050c2ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-easy-steps-to-switch-from-pandas-to-lightning-fast-polars-and-never-return-b14c66fc85b9&user=Bex+T.&userId=39db050c2ac2&source=post_page-39db050c2ac2----b14c66fc85b9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b14c66fc85b9--------------------------------) ·9 分钟阅读·2023 年 4 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb14c66fc85b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-easy-steps-to-switch-from-pandas-to-lightning-fast-polars-and-never-return-b14c66fc85b9&user=Bex+T.&userId=39db050c2ac2&source=-----b14c66fc85b9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb14c66fc85b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-easy-steps-to-switch-from-pandas-to-lightning-fast-polars-and-never-return-b14c66fc85b9&source=-----b14c66fc85b9---------------------bookmark_footer-----------)![](img/c3c3d6087a84b3a47f4c643d748b0d43.png)

图片由作者通过 Midjourney 提供

## 时候告别了！

Pandas 可以做任何事情，几乎无所不能。但是（而且这是我*无数次希望能有其他选择*的情况），它缺乏速度。Pandas 无法跟上当今数据集规模和复杂性增长的速度。

Pandas 的作者 Wes McKinney 表示，当他编写 Pandas 时，他心中有一个针对自己库的经验法则：

> 拥有比数据集大小多 5 到 10 倍的 RAM。

也许在 Iris 数据集首次引入时你可以忽视这个规则，但今天情况不同了。你的 RAM 如果*固守*在 64 GB 时，你根本无法加载一个 100GB 的数据集（这在现代已相当常见）。

当然，像 Dask 这样的优秀替代方案是存在的。但 Dask 并没有实现新的功能。它将现有的 Pandas 语法扩展到多个进程（线程）上，并忽略了底层的性能和内存问题。

它将 Pandas 视为一个**黑匣子**。请原谅我这么说，但这几乎就像是在给猪涂口红。
