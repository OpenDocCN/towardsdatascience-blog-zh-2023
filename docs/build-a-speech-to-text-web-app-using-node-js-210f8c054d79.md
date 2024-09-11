# 使用 Node.js 构建语音转文本的网页应用程序

> 原文：[`towardsdatascience.com/build-a-speech-to-text-web-app-using-node-js-210f8c054d79?source=collection_archive---------1-----------------------#2023-03-27`](https://towardsdatascience.com/build-a-speech-to-text-web-app-using-node-js-210f8c054d79?source=collection_archive---------1-----------------------#2023-03-27)

## 让我们构建一个使用 OpenAI 的 Whisper 模型进行音频转录和翻译的网页应用程序

[](https://shubhamstudent5.medium.com/?source=post_page-----210f8c054d79--------------------------------)![Kumar Shubham](https://shubhamstudent5.medium.com/?source=post_page-----210f8c054d79--------------------------------)[](https://towardsdatascience.com/?source=post_page-----210f8c054d79--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----210f8c054d79--------------------------------) [Kumar Shubham](https://shubhamstudent5.medium.com/?source=post_page-----210f8c054d79--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F65525b35f68&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-speech-to-text-web-app-using-node-js-210f8c054d79&user=Kumar+Shubham&userId=65525b35f68&source=post_page-65525b35f68----210f8c054d79---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----210f8c054d79--------------------------------) · 11 分钟阅读 · 2023 年 3 月 27 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F210f8c054d79&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-speech-to-text-web-app-using-node-js-210f8c054d79&user=Kumar+Shubham&userId=65525b35f68&source=-----210f8c054d79---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F210f8c054d79&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-speech-to-text-web-app-using-node-js-210f8c054d79&source=-----210f8c054d79---------------------bookmark_footer-----------)![](img/25b5d8abf919e8d8c304f803d5948166.png)

图片来源：[AltumCode](https://unsplash.com/@altumcode?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

大家好！希望你们一切顺利。今天，我们将使用 Node.js 和 OpenAI 的 API 构建一个语音转文本的网页应用程序。我们将使用 OpenAI 的 API 来利用其 Whisper 模型，这个模型允许我们上传 mp3 格式的音频文件，并为我们提供其转录文本。它甚至可以将其他语言的音频翻译成英语文本，这真是太不可思议了。

首先，我们会设置一个新的 Node.js 项目，以便我们可以开始构建我们的应用程序。所以，我们会创建一个文件夹，选择我们要构建项目的地方，并通过命令行进入那个文件夹，然后我们可以使用以下命令设置一个新的 Node.js 项目：

```py
npm init
```

运行此命令后，它会询问几个问题，比如应用程序的名称、入口点等。我们现在可以保持默认设置。之后，你会看到它创建了一个 `package.json` 文件。这个文件会包含关于我们应用程序的信息以及我们为这个应用程序安装了哪些包。

所以，下一步是将必要的节点模块，也就是包，安装到我们的应用程序中，以便我们准备好开始构建应用程序。我们可以通过运行…
