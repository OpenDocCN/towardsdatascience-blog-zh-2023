# 解密 Matplotlib

> 原文：[https://towardsdatascience.com/demystifying-matplotlib-3895ab229a63?source=collection_archive---------2-----------------------#2023-11-02](https://towardsdatascience.com/demystifying-matplotlib-3895ab229a63?source=collection_archive---------2-----------------------#2023-11-02)

## Quick Success 数据科学

## 你感到困惑是有原因的

[](https://medium.com/@lee_vaughan?source=post_page-----3895ab229a63--------------------------------)[![Lee Vaughan](../Images/9f6b90bb76102f438ab0b9a4a62ffa3f.png)](https://medium.com/@lee_vaughan?source=post_page-----3895ab229a63--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3895ab229a63--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3895ab229a63--------------------------------) [Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----3895ab229a63--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5d604015c08b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdemystifying-matplotlib-3895ab229a63&user=Lee+Vaughan&userId=5d604015c08b&source=post_page-5d604015c08b----3895ab229a63---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3895ab229a63--------------------------------) ·16分钟阅读·2023年11月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3895ab229a63&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdemystifying-matplotlib-3895ab229a63&user=Lee+Vaughan&userId=5d604015c08b&source=-----3895ab229a63---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3895ab229a63&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdemystifying-matplotlib-3895ab229a63&source=-----3895ab229a63---------------------bookmark_footer-----------)![](../Images/d556e988458efce5ec0801d4c6a1fca6.png)

图片来源：Cederic Vandenberghe, Unsplash

*你是否在使用 Matplotlib 时感到困惑？* 如果你是初学者，这可能是因为你还没有花时间去了解它的一些特性。如果你怀疑是这样，那么不妨继续阅读！这不会很痛苦，也不会花费太多时间。

# Matplotlib

开源的 Matplotlib 库主导了 Python 中的绘图。它允许你生成快速且简单的图表，以及详细复杂的图表，其中你可以控制显示的每一个方面。它的流行性和成熟度意味着你总能找到有用的建议和实用的代码示例。

像任何强大的软件一样，Matplotlib 有时会被某位作者形容为“**语法繁琐**”。最简单的图表很容易制作，但难度迅速增加。即使像[Matplotlib 画廊](https://matplotlib.org/stable/gallery/index.html)这样的资源提供了有用的代码示例，如果你需要一些稍微不同的东西，你可能会感到困惑。

实际上，许多人通过复制粘贴他人的代码并在边缘上进行修改来使用 Matplotlib，直到得到他们喜欢的效果。正如一位用户曾经告诉我的那样，“无论我使用 Matplotlib 几次，它总是感觉像是第一次使用！”
