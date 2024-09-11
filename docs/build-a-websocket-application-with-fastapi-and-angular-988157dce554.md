# 使用 FastAPI 和 Angular 构建 WebSocket 应用程序

> 原文：[`towardsdatascience.com/build-a-websocket-application-with-fastapi-and-angular-988157dce554?source=collection_archive---------3-----------------------#2023-01-30`](https://towardsdatascience.com/build-a-websocket-application-with-fastapi-and-angular-988157dce554?source=collection_archive---------3-----------------------#2023-01-30)

## 学习如何使用 WebSocket 协议构建双向互动通信应用程序

[](https://lynn-kwong.medium.com/?source=post_page-----988157dce554--------------------------------)![Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----988157dce554--------------------------------)[](https://towardsdatascience.com/?source=post_page-----988157dce554--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----988157dce554--------------------------------) [Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----988157dce554--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff649eccbbc3d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-websocket-application-with-fastapi-and-angular-988157dce554&user=Lynn+G.+Kwong&userId=f649eccbbc3d&source=post_page-f649eccbbc3d----988157dce554---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----988157dce554--------------------------------) ·7 min read·2023 年 1 月 30 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F988157dce554&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-websocket-application-with-fastapi-and-angular-988157dce554&user=Lynn+G.+Kwong&userId=f649eccbbc3d&source=-----988157dce554---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F988157dce554&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-websocket-application-with-fastapi-and-angular-988157dce554&source=-----988157dce554---------------------bookmark_footer-----------)![](img/862e87ceb50015d6686e582d408222c1.png)

图片由 geralt（手机智能手机股票交换）提供，来自 Pixabay

类似于 HTTP，WebSocket 也是一种用于客户端与服务器通信的协议。然而，与 HTTP 不同的是，WebSocket 是一种双向互动协议，它允许客户端向服务器发送消息，并且能够从服务器被动接收事件驱动的响应，而无需向服务器发起请求。

WebSocket 被广泛用于聊天和游戏中，这些场景中实时数据是必不可少的。它也可以用于其他需要实时或接近实时数据的领域。例如，一个使用历史价格预测未来价格的应用程序可以从 WebSocket 中获益。当来自数据流的客户端输入新数据时，使用某些机器学习或深度学习模型预测的数据可以自动发送给客户端。

在这篇文章中，我们将使用**FastAPI**和**Angular**构建一个简单的 WebSocket 应用程序，其中前者用于构建 WebSocket 服务器，后者用于构建客户端。这个概念可能对你来说是全新的，并且构建这样的应用程序可能看起来令人畏惧。然而，正如你将在本文中看到的，它实际上并没有那么复杂，我们可以构建一个 WebSocket 应用程序……
