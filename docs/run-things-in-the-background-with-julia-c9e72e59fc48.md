# 在 Julia 中后台运行任务

> 原文：[`towardsdatascience.com/run-things-in-the-background-with-julia-c9e72e59fc48?source=collection_archive---------9-----------------------#2023-05-26`](https://towardsdatascience.com/run-things-in-the-background-with-julia-c9e72e59fc48?source=collection_archive---------9-----------------------#2023-05-26)

## 停止等待，开始多线程

[](https://niczky12.medium.com/?source=post_page-----c9e72e59fc48--------------------------------)![Bence Komarniczky](https://niczky12.medium.com/?source=post_page-----c9e72e59fc48--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c9e72e59fc48--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c9e72e59fc48--------------------------------) [Bence Komarniczky](https://niczky12.medium.com/?source=post_page-----c9e72e59fc48--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F29f55325f60b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frun-things-in-the-background-with-julia-c9e72e59fc48&user=Bence+Komarniczky&userId=29f55325f60b&source=post_page-29f55325f60b----c9e72e59fc48---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c9e72e59fc48--------------------------------) ·4 min read·2023 年 5 月 26 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc9e72e59fc48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frun-things-in-the-background-with-julia-c9e72e59fc48&user=Bence+Komarniczky&userId=29f55325f60b&source=-----c9e72e59fc48---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc9e72e59fc48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frun-things-in-the-background-with-julia-c9e72e59fc48&source=-----c9e72e59fc48---------------------bookmark_footer-----------)![](img/b6d8943bd405300d84e92529392e8559.png)

图片来自 [Max Wolfs](https://unsplash.com/ko/@yesterdazed?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

尽管 Julia 是最快的语言之一，但有时任务执行可能需要时间。如果你是使用 Julia 的数据科学家或分析师，也许你会想把计算任务交给服务器，等待完成后再处理结果。

> 但等待是无聊的。

当你忙于工作、充满创意和热情想要交付一些有趣的东西时，你会想要**继续敲打键盘寻找其他东西**。

让我给你展示一个在 Julia 中的简单技巧，如何**将计算任务分配到另一个线程**并继续你的工作。

# 设置工作环境

如我之前所说，Julia 很快。作为一种现代语言，它也**考虑到了多进程处理**。所以，如果你知道怎么做，利用你机器上的额外核心是很简单的。

首先，我们必须确保启动一个具有多个线程的 Julia 实例：

```py
julia -t 4
```

这将使用 4 个线程启动 Julia。我们可以通过询问线程数量来确认这一点：

```py
julia> using Base.Threads

julia> Threads.nthreads()
4
```

## 使其变慢…
