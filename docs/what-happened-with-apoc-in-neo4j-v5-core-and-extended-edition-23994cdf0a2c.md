# Neo4j v5 中 APOC 发生了什么：核心版和扩展版

> 原文：[https://towardsdatascience.com/what-happened-with-apoc-in-neo4j-v5-core-and-extended-edition-23994cdf0a2c?source=collection_archive---------9-----------------------#2023-04-04](https://towardsdatascience.com/what-happened-with-apoc-in-neo4j-v5-core-and-extended-edition-23994cdf0a2c?source=collection_archive---------9-----------------------#2023-04-04)

## Neo4j 的 APOC 插件在 v5 版本中被分为两个版本

[](https://bratanic-tomaz.medium.com/?source=post_page-----23994cdf0a2c--------------------------------)[![Tomaz Bratanic](../Images/d5821aa70918fcb3fc1ff0013497b3d5.png)](https://bratanic-tomaz.medium.com/?source=post_page-----23994cdf0a2c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----23994cdf0a2c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----23994cdf0a2c--------------------------------) [Tomaz Bratanic](https://bratanic-tomaz.medium.com/?source=post_page-----23994cdf0a2c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F57f13c0ea39a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-happened-with-apoc-in-neo4j-v5-core-and-extended-edition-23994cdf0a2c&user=Tomaz+Bratanic&userId=57f13c0ea39a&source=post_page-57f13c0ea39a----23994cdf0a2c---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----23994cdf0a2c--------------------------------) ·5分钟阅读·2023年4月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F23994cdf0a2c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-happened-with-apoc-in-neo4j-v5-core-and-extended-edition-23994cdf0a2c&user=Tomaz+Bratanic&userId=57f13c0ea39a&source=-----23994cdf0a2c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F23994cdf0a2c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-happened-with-apoc-in-neo4j-v5-core-and-extended-edition-23994cdf0a2c&source=-----23994cdf0a2c---------------------bookmark_footer-----------)![](../Images/7dd50831ee195dfdd4ad1943452dea29.png)

图片由 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) 提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

你是否升级或下载了 Neo4j v5，现在一些你喜欢的 APOC 程序不再工作了？你并不孤单。

从Neo4j v5开始，APOC已被拆分为两个版本。主要区别在于，一些过程在核心版中得到了官方支持并可用。APOC Core包含经过严酷测试的过程和函数，没有外部依赖项。另一方面，扩展版包含可能需要外部依赖项的附加过程，并且没有官方支持。

我写这篇博客文章是为了帮助你理解两个版本之间的新区别。首先，现在有两个独立的文档网站：

+   [APOC Core文档](https://neo4j.com/docs/apoc/current/)

+   [APOC Extended文档](https://neo4j.com/labs/apoc/5/)

此外，如果一个过程不是核心版的一部分，它在文档中会被标记为扩展标签。

![](../Images/a7cf6be0bdb6895f87a1f62e99351fac.png)

APOC扩展标签。图片由作者提供。

例如，`apoc.load.json`过程是核心版的一部分，因为它没有扩展标签。另一方面，`apoc.load.csv`过程是扩展版的一部分，如扩展标签所示。

现在让我们来看看安装过程。

## Neo4j Desktop

我喜欢使用[Neo4j Desktop](https://neo4j.com/download/)在本地环境中进行任何原型设计。这是一个很好的应用程序，可以让你通过几次点击来设置和安装APOC插件。

![](../Images/7c76765e785545920aeda1af1aee4588.png)

在Neo4j Desktop中安装APOC。图片由作者提供。

但是，目前并没有明确提到单击安装仅安装核心版。如果我们想添加扩展版，需要手动下载并将其复制到插件文件夹中。

扩展版的发布可以在[GitHub](https://github.com/neo4j-contrib/neo4j-apoc-procedures/releases)上找到。我们需要确保APOC版本与Neo4j版本兼容。APOC版本遵循Neo4j版本，例如，APOC v5.5.0与Neo4j v5.5.0兼容。确保Neo4j和APOC之间的前两个版本号匹配。

![](../Images/e3660f4328bdc0af7b7e2c91ce603385.png)

APOC Extended v5.5.0。图片由作者提供。

我们需要下载适当版本的扩展jar文件。一些过程需要额外的依赖项，这些依赖项也可以下载。然而，大多数情况下，仅下载扩展jar文件就足够了。下载完成后，将jar文件复制到插件文件夹中。

![](../Images/50108a3aa9dc3380c249d5f4d5174f68.png)

选择插件文件夹。图片由作者提供。

你可以通过点击**三个点**选项，选择**打开文件夹**，最后点击**插件**来打开`plugins`文件夹。

文件夹中应已包含你在Desktop应用程序中安装的APOC插件核心版本。因此，最终应该有两个APOC Jar文件，一个用于核心版本，另一个用于扩展版Docker版本。

*确保首先安装核心版，因为它总是设置适当的配置值，以允许我们执行APOC过程！*

另一个变化是，我们不能将APOC配置设置添加到`neo4j.conf`文件中。相反，我们需要创建一个`apoc.conf`文件，并在其中设置任何APOC配置值。例如，我们需要设置以下配置，以允许APOC过程从本地磁盘读取文件。

```py
apoc.import.file.enabled=true
```

你可以通过以下流程在Neo4j Desktop中设置此配置：

![](../Images/e4c6de07c7b56b0703d6662504a94b51.png)

设置APOC配置值。图像由作者提供。

按照说明打开**Configuration**文件夹。接下来，创建一个新的`apoc.conf`文件，然后在新创建的文件中设置适当的配置值。

## Neo4j Docker

在生产环境中，我喜欢在docker容器中运行Neo4j。Neo4j的docker容器提供了一个方便的环境变量，帮助我们安装适当版本的APOC插件。

```py
docker run \
    -p 7474:7474 -p 7687:7687 \
    -e NEO4J_PLUGINS=\[\"apoc\"\] \
    neo4j:5.6.0
```

然而，使用环境变量在Neo4j v5中安装APOC插件将只安装核心版。不幸的是，如果我们想使用扩展版，我们需要手动下载并将其复制到插件文件夹中。

我更喜欢在与docker容器交互时使用`docker-compose`。因此，我将使用以下`docker-compose`设置来运行Neo4j，并同时使用APOC核心版和扩展版。

```py
version: '3.7'
services:
  neo4j:
    image: neo4j:5.5.0
    restart: always
    hostname: neo4j
    container_name: neo4j
    ports:
      - 7474:7474
      - 7687:7687
    volumes:
      - ./neo4j/data:/data
      - ./neo4j/plugins:/plugins
    environment:
      - NEO4J_AUTH=neo4j/pleaseletmein
      - NEO4J_PLUGINS=["apoc"]
      - NEO4J_apoc_import_file_enabled=true
```

在这个例子中，我们使用的是Neo4j v5.5.0。我们还使用了环境变量来安装APOC核心库，并允许APOC过程从磁盘读取文件。然而，我们需要下载[APOC Extended v.5.5.0](https://github.com/neo4j-contrib/neo4j-apoc-procedures/releases/download/5.5.0/apoc-5.5.0-extended.jar)并将其复制到**neo4j/plugins**文件夹中。所以，我们只需要手动复制扩展版，而核心版是通过**NEO4J_PLUGINS**变量安装的。此外，在大多数情况下，你还希望持久化数据库文件。因此，我们还挂载了**data**文件夹。

## 总结

在Neo4j v5中，APOC被分为核心版和扩展版。如果你使用的是核心版的过程，安装过程没有变化。然而，如果你使用的是扩展版的过程，你需要手动下载并从GitHub发布页面复制扩展版。希望这篇博客文章能帮助解决你在Neo4j v5中遇到的任何APOC问题。
