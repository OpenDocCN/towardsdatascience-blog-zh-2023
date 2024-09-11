# 创建并部署一个 REST API 以提取图像中的主要颜色

> 原文：[https://towardsdatascience.com/create-and-deploy-a-rest-api-extracting-predominant-colors-from-images-a44b94cc3d46?source=collection_archive---------7-----------------------#2023-09-26](https://towardsdatascience.com/create-and-deploy-a-rest-api-extracting-predominant-colors-from-images-a44b94cc3d46?source=collection_archive---------7-----------------------#2023-09-26)

## 使用无监督机器学习、FastAPI 和 Docker

[](https://nicolo-albanese.medium.com/?source=post_page-----a44b94cc3d46--------------------------------)[![Nicolo Cosimo Albanese](../Images/9a2c26207146741b58c3742927d09450.png)](https://nicolo-albanese.medium.com/?source=post_page-----a44b94cc3d46--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a44b94cc3d46--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a44b94cc3d46--------------------------------) [Nicolo Cosimo Albanese](https://nicolo-albanese.medium.com/?source=post_page-----a44b94cc3d46--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7430df412ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-and-deploy-a-rest-api-extracting-predominant-colors-from-images-a44b94cc3d46&user=Nicolo+Cosimo+Albanese&userId=7430df412ec&source=post_page-7430df412ec----a44b94cc3d46---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----a44b94cc3d46--------------------------------) ·15 分钟阅读·2023年9月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa44b94cc3d46&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-and-deploy-a-rest-api-extracting-predominant-colors-from-images-a44b94cc3d46&user=Nicolo+Cosimo+Albanese&userId=7430df412ec&source=-----a44b94cc3d46---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa44b94cc3d46&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-and-deploy-a-rest-api-extracting-predominant-colors-from-images-a44b94cc3d46&source=-----a44b94cc3d46---------------------bookmark_footer-----------)![](../Images/2c1fc392c4d210706b63a714d397b644.png)

图片由作者提供。

# 目录

1.  [问题陈述](#c5a9)

1.  [从图像中提取颜色](#dfe8)

1.  [项目结构](#dfe8)

1.  [代码](#50f4)

1.  [部署 Docker 容器](#d547)

1.  [让我们试试吧！](#aeea)

1.  [API 文档](#1e07)

1.  [结论](#17f9)

1.  [许可免责声明](#01e6)

# 1. 问题陈述

让我们设想一个**制造设施**的控制室，其中制造出的产品需要自动分类。例如，根据颜色，货物可能会被重新引导到滚筒输送机的不同分支，以便进一步处理或包装。

另外，我们也可以设想一个**在线零售商**，试图通过添加*按颜色搜索*功能来提升用户体验。客户可以更容易地找到特定颜色的服装，从而简化他们对感兴趣产品的访问。

或者，就像作者一样，你可以把自己想象成一名**IT顾问**，实现一个简单、快速且可重复使用的工具来生成用于演示文稿、图表等的颜色调色板。
