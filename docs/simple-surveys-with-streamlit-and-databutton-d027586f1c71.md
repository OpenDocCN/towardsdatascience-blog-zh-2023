# 使用 Streamlit 进行简单调查

> 原文：[https://towardsdatascience.com/simple-surveys-with-streamlit-and-databutton-d027586f1c71?source=collection_archive---------11-----------------------#2023-06-19](https://towardsdatascience.com/simple-surveys-with-streamlit-and-databutton-d027586f1c71?source=collection_archive---------11-----------------------#2023-06-19)

## Streamlit 的用户界面组件使得构建简单调查变得容易

[](https://medium.com/@alan-jones?source=post_page-----d027586f1c71--------------------------------)[![Alan Jones](../Images/359379fab1d6685ff08080b98173e67c.png)](https://medium.com/@alan-jones?source=post_page-----d027586f1c71--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d027586f1c71--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d027586f1c71--------------------------------) [Alan Jones](https://medium.com/@alan-jones?source=post_page-----d027586f1c71--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7d3f5fb94faa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimple-surveys-with-streamlit-and-databutton-d027586f1c71&user=Alan+Jones&userId=7d3f5fb94faa&source=post_page-7d3f5fb94faa----d027586f1c71---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d027586f1c71--------------------------------) ·10 min read·2023年6月19日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd027586f1c71&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimple-surveys-with-streamlit-and-databutton-d027586f1c71&source=-----d027586f1c71---------------------bookmark_footer-----------)![](../Images/054e1e498d5035ba5a4e7c90024ebc1e.png)

图片来源：[Nguyen Dang Hoang Nhu](https://unsplash.com/@nguyendhn?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

+   *你认为人工智能的未来会怎样？它应该受到监管吗？它会创造新工作还是会毁灭现有工作？*

+   *你认为气候变化将如何影响你的生活方式？*

+   *你相信宇宙中存在外星生命吗？*

+   *你偏好的数据科学编程语言是什么？*

有时我们使用其他人的数据来创建一个故事——而有时我们需要创建自己的数据，因此我们需要收集数据。这可能是一个调查或实验结果的日志，但我们需要提出问题并记录所得数据。

当然，也有一些服务可以为你完成这些工作（有时需要付费，但通常也有免费的选项）。或者你可以坚持使用经过验证的剪贴板和铅笔方法。

但如果你是一个 Streamlit 用户，创建一个简单的调查是非常容易的。

## 数据存储

不过，还是有一点难点。Streamlit 的用户界面组件很棒且易于使用，但没有内置的数据存储方法。你可以简单地将数据存储在文本文件或 SQLite 数据库中，而这…
