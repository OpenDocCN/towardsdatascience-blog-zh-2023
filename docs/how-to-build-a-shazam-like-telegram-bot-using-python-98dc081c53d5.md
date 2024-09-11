# 如何使用 Python 构建一个类似 Shazam 的 Telegram 机器人

> 原文：[`towardsdatascience.com/how-to-build-a-shazam-like-telegram-bot-using-python-98dc081c53d5?source=collection_archive---------11-----------------------#2023-01-24`](https://towardsdatascience.com/how-to-build-a-shazam-like-telegram-bot-using-python-98dc081c53d5?source=collection_archive---------11-----------------------#2023-01-24)

## 一个创建和部署可以实时识别音乐的 Telegram 机器人，并帮助你查找歌曲标题和歌手的教程。

[](https://eugenia-anello.medium.com/?source=post_page-----98dc081c53d5--------------------------------)![Eugenia Anello](https://eugenia-anello.medium.com/?source=post_page-----98dc081c53d5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----98dc081c53d5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----98dc081c53d5--------------------------------) [Eugenia Anello](https://eugenia-anello.medium.com/?source=post_page-----98dc081c53d5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F86fdc517c278&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-a-shazam-like-telegram-bot-using-python-98dc081c53d5&user=Eugenia+Anello&userId=86fdc517c278&source=post_page-86fdc517c278----98dc081c53d5---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----98dc081c53d5--------------------------------) ·9 min read·2023 年 1 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F98dc081c53d5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-a-shazam-like-telegram-bot-using-python-98dc081c53d5&user=Eugenia+Anello&userId=86fdc517c278&source=-----98dc081c53d5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F98dc081c53d5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-a-shazam-like-telegram-bot-using-python-98dc081c53d5&source=-----98dc081c53d5---------------------bookmark_footer-----------)![](img/42e32cdfef8d602899d368abc141af42.png)

照片来源于 [Austin Neill](https://unsplash.com/@arstyy) 于 [Unsplash](https://unsplash.com/photos/hgO1wFPXl3I)

当你在外面听音乐，比如在酒吧或商店里，如果你真的喜欢这首歌，你是否会想知道这首歌的标题和歌手的名字？

我遇到过很多次，唯一的解决办法就是 Shazam。下载它并点击弹出按钮来识别音乐。

在这篇文章中，我们将构建一个类似的应用来识别歌曲和歌手。开发这个数据科学项目的一个快速且简便的方法是使用 Python 创建一个 Telegram 机器人。

当你听音乐的时候，你可以录制声音。这个机器人将把音频转录成文本，并为你找到歌曲标题和歌手。让我们开始吧！

## 目录：

+   **第一部分：使用 Python 创建 Telegram 机器人**

+   **第二部分：将 Telegram 机器人部署到 Fly.io**

# 第一部分：使用 Python 创建 Telegram 机器人
