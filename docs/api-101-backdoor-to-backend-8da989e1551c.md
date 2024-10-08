# API 101：后端的秘密通道

> 原文：[`towardsdatascience.com/api-101-backdoor-to-backend-8da989e1551c?source=collection_archive---------10-----------------------#2023-01-23`](https://towardsdatascience.com/api-101-backdoor-to-backend-8da989e1551c?source=collection_archive---------10-----------------------#2023-01-23)

![](img/40b9a5b9f2c72bd1a2b7365a09ef5842.png)

图片由 [Lala Azizli](https://unsplash.com/@lazizli?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 快速深入 API 的世界，以及 API 如何驱动应用

[](https://amansharma2910.medium.com/?source=post_page-----8da989e1551c--------------------------------)![阿曼·夏尔马](https://amansharma2910.medium.com/?source=post_page-----8da989e1551c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8da989e1551c--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----8da989e1551c--------------------------------) [阿曼·夏尔马](https://amansharma2910.medium.com/?source=post_page-----8da989e1551c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F84e03a4d7261&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fapi-101-backdoor-to-backend-8da989e1551c&user=Aman+Sharma&userId=84e03a4d7261&source=post_page-84e03a4d7261----8da989e1551c---------------------post_header-----------) 发布在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----8da989e1551c--------------------------------) ·11 分钟阅读·2023 年 1 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8da989e1551c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fapi-101-backdoor-to-backend-8da989e1551c&user=Aman+Sharma&userId=84e03a4d7261&source=-----8da989e1551c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8da989e1551c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fapi-101-backdoor-to-backend-8da989e1551c&source=-----8da989e1551c---------------------bookmark_footer-----------)

> tl;dr — 这是对 API、应用开发以及后端开发理论的非常详细的介绍。可以作为这些主题的教育材料使用。

在这个数字时代，我们被各种应用包围，几乎依赖它们来满足我们几乎所有的日常需求。

你起床时最喜欢做的第一件事是阅读新闻，了解全球发生的事情吗？好吧，有像 *Google News* 和 *Inshorts* 这样的新闻应用程序。迟到了，想叫辆出租车去工作？你可以通过 *Uber* 应用程序快速在家门口叫到一辆！饿了但不想做饭？只需拿起手机，舒适地从家里订购食品，无需提着沉重的购物袋和在商店里徘徊。

那么，我们为什么要讨论这个？关键是今天我们有这些非常方便的应用程序，可以为我们做几乎所有事情。*但这是从用户的角度来看。* 对于软件开发者来说，情况要复杂得多。

虽然对于用户而言过于简化，但这样的应用程序对开发者来说可能相当复杂。因此，第一个出现的问题是—— **这些应用程序是如何制作的？**

为了解答这个问题，我们可以看看 **三层应用架构**。

# 三层应用架构

三层应用架构是一种成熟的软件开发架构，将软件分解为三个逻辑和物理计算层，即：

+   **展示层-***R* 指的是客户端应用程序，即用户实际看到和交互的应用程序。

+   **应用层-** 指的是处理所有业务逻辑的应用组件。

+   **数据层-** 指的是存储和处理应用程序中所有数据的组件。

现在，这三个独立的组件协同工作，构成一个可用的应用程序。

+   用户与客户端（也称为前端）应用程序进行交互，*发出某种请求*。

+   应用层 *处理这个请求*。

+   如果在处理此请求时 *生成或更改了任何数据*，这些数据将保存在数据层中。

+   请求在后端处理后，会将响应发送到前端应用程序。

> ***注意：*** *通常，应用层和数据层结合在一起被称为服务器端（或后端）应用程序。*

但现在，另一个问题出现了。我们将在本指南中专门回答的问题是—— **不同应用层（更具体地说，前端和后端）是如何相互通信的？**

嗯，这里就涉及到 **API** 的概念。

# 什么是 API？

API 是 ***应用程序编程接口*** 的缩写。顾名思义，它是一种接口。

更准确地说，API 允许跨平台和设备间的通信。

![](img/275bc843831971af80adda518226813f.png)

图片由作者提供

+   **软件到软件的通信：** 之前我们讨论了应用程序如何被分解为客户端和服务器端组件。这些组件交换请求和数据响应。这种通信接口由 API 在后台提供支持。客户端向服务器发送 API 请求，服务器处理请求，然后将 API 响应返回给客户端。

+   **平台无关的通信：** 由于 API 充当客户端和服务器之间的有效通信机制，这意味着客户端和服务器可以基于完全不同的技术栈，却能够协同工作。例如，你的前端应用程序可以用 JavaScript 编写，而你的后端可以用 Python 编写。API 通常具有一个标准化的请求和响应结构，几乎所有编程语言都支持。

+   **代码的重用性：** API 促进了代码的重用。我们可以这样理解：Instagram 既有移动应用程序，也有网站。在 Instagram 的情况下，我们有两种客户端，但只有一个处理来自网页客户端和移动客户端请求的后端。这两个客户端都通过 API 向同一个后端服务器发送请求。因此，不需要为移动端和网页端分别实现后端。

现在我们对 API 有了初步了解，让我们深入探讨一下不同种类的 API。

# API 的类型

基于提供的服务，API 主要有三种类型，分别如下：

+   **本地 API：** 这些是操作系统（OS）API，提供诸如麦克风访问、相机访问或从数据库服务请求数据等服务。

+   **程序 API：** 基于远程过程调用（RPC）技术，允许从另一服务器远程执行程序。简单来说，这些 API 只是允许在远程服务器或设备上执行脚本或程序。

+   **Web API：** Web API，也称为 Web 服务，允许应用程序或设备通过万维网进行通信，使用 HTTP 架构。

> **在构建涉及客户端-服务器架构的应用程序时，我们主要使用 Web API。**

既然本文的重点是熟悉应用程序的构建方式，我们将专注于 Web API。

> **注意：** Web API 还可以进一步细分为几个分类，如 SOAP（简单对象访问协议）、REST（表征状态转移）API 等。目前，世界上大多数应用程序主要使用 REST API 架构进行客户端-服务器交互，因为其实现简单。由于 REST 已成为行业标准，本文将讨论 REST API。

# 什么是 REST API？

REST 代表表现层状态转移，也称为 RESTful Web 服务。它是一种标准化的 API 架构，允许跨平台和设备间通过 HTTP 网络协议进行通信。

> 再简单一点，它允许两个应用程序组件互相交互。

典型的客户端-服务器交互主要包括四种操作：

1.  创建：通过 API 请求执行的创建操作涉及在后端数据层上创建资源。*例如，在 TODO 列表应用中，创建 API 请求可用于在数据库中创建一个 TODO 项。*

1.  读取：读取操作可用于从数据库中获取数据。根据客户端的请求和后端资源访问的实现，这些数据可以是某一类型的单个资源、一批单一类型的资源，或不同类型的资源集合。*以同样的 TODO 应用为例，读取请求可以用于获取用户创建的所有 TODO。*

1.  更新：可以执行更新操作以更新数据层中特定资源相关的数据。*在我们的 TODO 应用中，更新请求可用于更新 TODO 任务。*

1.  删除：顾名思义，删除操作用于删除资源。*对于我们的 TODO 应用，删除请求可用于从 TODO 列表中删除 TODO 对象。*

这些操作统称为**CRUD**（**C**reate，**R**ead，**U**pdate，**D**elete）**操作**。对于任何应用程序，这些都是用户可以执行的一些最基本的操作。

> 简单来说，当用户与应用程序交互时，他们要么在访问数据，要么数据正在被创建或操作。REST API 充当前端和后端之间的中介步骤，允许在应用程序中执行 CRUD 操作。

我们之前提到，由于 API 作为客户端和服务器之间的通信渠道实现，客户端和服务器可以使用完全不同的编程技术栈。然而，必须有一种标准化的，*语言无关*的机制，以便客户端和服务器可以通过 API 进行交互。为此，REST API 使用**JSON**基础的消息传递协议。

JSON 代表***JavaScript 对象表示法***。它是一种开放标准文件格式和数据交换格式，其具有可读性。数据通过 JSON 以键值对和数组格式传输。

下图演示了典型的客户端-服务器交互。

![](img/63b393a0897290d8365bcd00f1bade3b.png)

作者提供的图片

+   客户端通过 HTTP 协议向服务器发送 API 请求。*该请求采用 JSON 格式。*

+   服务器解析此 API 请求，处理请求，然后将 API 响应发送回客户端，*该响应再次以 JSON 格式呈现。*

+   客户端可以解析 JSON 响应对象以进行呈现。

正如你所观察到的，由于中介通信通道是标准化的，因此客户端和服务器可以在平台和实现方面独立操作。*也就是说，后端可以用一种编程语言（例如 Python）编写，前端可以用另一种（例如 JavaScript）编写，应用程序仍然可以正常工作。*

另一个 REST API 的重要特征是它们是***无状态的***。这意味着从客户端到服务器的每个请求必须包含理解和完成请求所需的所有信息。*后续请求不依赖于之前的请求。*

至此，我们已经完成了关于 API 的一些理论概念以及 REST API 的特征。这一切将我们带到了最重要的部分。

# API 请求是什么样的？

正如我们之前讨论的，*REST API 调用是无状态的*，这意味着每个 API 请求中，我们需要发送

+   处理请求所需的所有数据。这些数据涉及要执行的操作类型，

+   有关用户认证的信息，

+   通过 API 访问的资源，

+   以及可能需要处理请求的一些额外参数。

根据这些要求，REST API 请求可以分解为 4 个组件：

## 资源路径

这是通过 API 请求操作的资源路径。资源可以视为数据库中的对象或实体。资源路径是*映射到*数据库中的实体。继续以 TODO 应用为例。一个 API 端点可以用来获取用户所有 TODO 的列表。该端点的 URL 看起来像这样：`https://mytodolist.com/todos/`

这里，`https://mytodolist.com` 是网站的域名。`/todos/` 是 TODO 对象的资源路径。

## HTTP 动词

正如我们之前讨论的，API 请求必须指定打算执行的 CRUD 操作类型。为此，我们在 API 调用中指定一个 HTTP 动词。理解 HTTP 动词的最简单方法是它们是*指示*服务器对特定资源执行什么操作的“词”。

有许多预定义和标准化的 HTTP 动词，然而，最常用的如下所述。

+   **POST：** 用于创建资源对象

+   **GET：** 用于对资源执行读取操作

+   **PUT：** 用于执行更新操作

+   **DELETE：** 用于执行删除操作

## 请求体

请求体可以用来指定处理请求时可能需要的一些额外参数。例如，在我们的 TODO 应用中，为了创建一个 TODO 对象，我们必须指定任务。类似地，为了更新 TODO 对象，我们需要指定新的任务，这个任务将替换 TODO 列表中的原始任务，并提供要修改任务的 ID。在我们的 API 请求中，我们必须通过请求体发送这些信息。

*一般来说，我们需要为 POST 和 PUT 请求指定请求体。GET 请求没有请求体。*

> **注意：** 请求体使用 JSON 格式将请求数据以键值对对象的形式发送。

## 请求头

虽然上述 3 个组件通常足以发出 REST API 请求，但有时我们需要在 API 请求中指定一些额外的细节。*以用户认证为例。*

在我们的 TODO 应用中，我们可能会有一堆用户，每个用户都有自己的 TODO 列表。我们必须确保首先，只有授权用户才能创建、读取、更新和删除 TODO。然后，我们还需要确保一个用户不能访问另一个用户的 TODO。为了保证这些要求，我们需要在 API 端点上实现某种形式的授权，以确定是否是授权用户在进行 API 请求，以及资源是否真正属于该用户。

尽管我们不会深入讨论认证策略，但仅为了信息传递，我们在请求头中发送认证信息，通常以唯一令牌的形式。

所有这些组件一起构成了一个 API 调用。

让我们看看一些使用 cURL 发出的示例 API 调用：

```py
curl --request GET \
     --url https://realstonks.p.rapidapi.com/TSLA \
     --header 'X-RapidAPI-Host: realstonks.p.rapidapi.com' \
     --header 'X-RapidAPI-Key: 1ca406921amsh21f2341da9178c3p1a7dd9jsn5d319166bd10'
```

上述是一个 GET API 请求。我们使用 `--request` 标志来指定 HTTP 动词。`--url` 标志用于指定请求路径。然后使用 `--header` 标志来指定头部信息。

```py
curl --request POST \
     --url https://hotel-price-aggregator.p.rapidapi.com/rates \
     --header 'X-RapidAPI-Host: hotel-price-aggregator.p.rapidapi.com' \
     --header 'X-RapidAPI-Key: 1ca406921amsh21f2341da9178c3p1a7dd9jsn5d319166bd10' \
     --header 'content-type: application/json' \
     --data '{
    "hotelId": "102061485",
    "checkIn": "2022-07-01",
    "checkOut": "2022-07-02"
}'
```

上述是一个 POST 请求，我们可以看到使用 `--data` 标志指定请求体的示例。

API 请求的实现可能因编程语言或工具而异。然而，API 请求的关键组件将保持不变，即 HTTP 动词、请求 URL 或请求路径、请求体和请求头部。

现在我们知道 API 请求是什么样的，API 请求-响应周期中仅剩下一个组件需要理解——API 响应。

# API 响应是什么样的？

API 响应是后端在处理完 API 请求后返回给前端的响应。响应应告知客户端请求是否成功，如果成功，应返回请求的数据或适当的确认信息。如果请求不成功，客户端应接收到相关的错误信息。

根据这些要求，API 响应可以分为两个部分，如下所述。

## 响应体

响应体包含请求的数据（在 GET API 请求的情况下），或者在其他 API 请求类型中包含适当的确认消息。如果 API 请求失败，响应体将包含适当的失败消息，说明为什么 API 调用到服务器失败。

## 响应状态码

API 响应包含一个响应状态码，帮助客户端确定请求是成功还是失败。这些状态码已经为 HTTP 协议标准化，可以大致分为 5 类 — `1xx` → 信息性，`2xx` → 成功，`3xx` → 重定向，`4xx` → 客户端错误，`5xx` → 服务器错误

一些常见的状态码如下所示。

+   200 — *成功。请求成功。*

+   204 — *无内容。*

+   301 — *永久移动。*

+   400 — *错误请求。*

+   401 — *未经授权。*

+   403 — *禁止访问。*

+   404 — *未找到。*

+   500 — *内部服务器错误。*

例如，我们之前发出的 GET API 请求的响应如下：

```py
{
  "price": 141.98,
  "change_point": 8.56,
  "change_percentage": 6.42,
  "total_vol": "107.51M"
}
```

响应状态码是 200。

# 结论

随着这篇文章的结束，我们来总结一下。我们了解了什么是 API，以及它们如何在设备和应用程序之间实现通信。最后，我们深入研究了 REST API——API 请求响应周期是如何工作的。

显然，API 只是一个基础，或者说是进入广阔的软件开发（更准确地说是后端开发）世界的后门，正如标题所暗示的那样。

你可以关注我，以阅读更多有趣的概念。在本系列的后续博客中，我们将更深入地探讨后端开发。
