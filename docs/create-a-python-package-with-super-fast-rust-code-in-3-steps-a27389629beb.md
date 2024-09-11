# 用超级快速的 Rust 代码在 3 步中创建一个 Python 包

> 原文：[`towardsdatascience.com/create-a-python-package-with-super-fast-rust-code-in-3-steps-a27389629beb?source=collection_archive---------6-----------------------#2023-02-24`](https://towardsdatascience.com/create-a-python-package-with-super-fast-rust-code-in-3-steps-a27389629beb?source=collection_archive---------6-----------------------#2023-02-24)

## *用包含 Rust 代码的包扩展你的 Python 代码，性能提升超过 150 倍！*

[](https://mikehuls.medium.com/?source=post_page-----a27389629beb--------------------------------)![Mike Huls](https://mikehuls.medium.com/?source=post_page-----a27389629beb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a27389629beb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a27389629beb--------------------------------) [Mike Huls](https://mikehuls.medium.com/?source=post_page-----a27389629beb--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7ffb62c607ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-a-python-package-with-super-fast-rust-code-in-3-steps-a27389629beb&user=Mike+Huls&userId=7ffb62c607ee&source=post_page-7ffb62c607ee----a27389629beb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a27389629beb--------------------------------) ·9 分钟阅读·2023 年 2 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa27389629beb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-a-python-package-with-super-fast-rust-code-in-3-steps-a27389629beb&user=Mike+Huls&userId=7ffb62c607ee&source=-----a27389629beb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa27389629beb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-a-python-package-with-super-fast-rust-code-in-3-steps-a27389629beb&source=-----a27389629beb---------------------bookmark_footer-----------)![](img/ed219a4972a58651e9682cc108017a96.png)

这个 Python 变得有点“生锈”了！（图片由 Dall-e 2 提供！）

Python 是一门相当容易上手的语言，相比于其他一些语言，它编写代码非常迅速。但这种易用性也有代价：速度会受到牺牲。有时候，Python 就是太慢了！

为了解决这个问题，我们将把部分 Python 代码用 Rust 重新编写，并将这段代码作为 Python 包导入到我们的原始项目中。最终，我们将得到一个超快的 Python 包，可以像其他包一样导入和使用。作为额外奖励，我们还将对 Rust 版的 Python 包进行多进程处理，使得函数速度提高约 150 倍。开始编码吧！

# 概述

这是我们在本文中要做的事情的快速总结。我们将通过 6 个步骤解决这个问题（其中第 2、3 和 4 步专注于实际编写包）：

1.  检查我们慢的函数；它为什么这么慢？

1.  准备我们的项目

1.  我们将这个函数用 Rust 重新编写

1.  编译 Rust 代码并将其放入 Python 包中

1.  将 Python 包导入到我们的项目中
