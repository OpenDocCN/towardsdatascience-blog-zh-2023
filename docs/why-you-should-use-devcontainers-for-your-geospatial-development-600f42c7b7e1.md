# 为什么你应该使用Devcontainers进行你的地理空间开发

> 原文：[https://towardsdatascience.com/why-you-should-use-devcontainers-for-your-geospatial-development-600f42c7b7e1?source=collection_archive---------7-----------------------#2023-03-31](https://towardsdatascience.com/why-you-should-use-devcontainers-for-your-geospatial-development-600f42c7b7e1?source=collection_archive---------7-----------------------#2023-03-31)

## 探索使用DevContainers和Codespaces进行无缝地理空间开发的优势

[](https://cordmaur.medium.com/?source=post_page-----600f42c7b7e1--------------------------------)[![Maurício Cordeiro](../Images/1ec750bf68bbaa0331fabdebebf28eb5.png)](https://cordmaur.medium.com/?source=post_page-----600f42c7b7e1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----600f42c7b7e1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----600f42c7b7e1--------------------------------) [Maurício Cordeiro](https://cordmaur.medium.com/?source=post_page-----600f42c7b7e1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8878c77fe1a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-you-should-use-devcontainers-for-your-geospatial-development-600f42c7b7e1&user=Maur%C3%ADcio+Cordeiro&userId=8878c77fe1a3&source=post_page-8878c77fe1a3----600f42c7b7e1---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----600f42c7b7e1--------------------------------) ·5 min阅读·2023年3月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F600f42c7b7e1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-you-should-use-devcontainers-for-your-geospatial-development-600f42c7b7e1&user=Maur%C3%ADcio+Cordeiro&userId=8878c77fe1a3&source=-----600f42c7b7e1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F600f42c7b7e1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-you-should-use-devcontainers-for-your-geospatial-development-600f42c7b7e1&source=-----600f42c7b7e1---------------------bookmark_footer-----------)![](../Images/11a36ccf930dd6cff3a8e6e8247fd8ab.png)

图像由Midjourney创建。说明：“世界被电缆包围，在一个凌乱的环境中，赛博朋克，现实主义3D”

> 由于Medium.com政策变更，非会员阅读的新规定已于2023年9月实施，该帖子现在可以在**geocorner.net**上免费阅读：[https://www.geocorner.net/post/why-you-should-use-devcontainers-for-your-geospatial-development](https://www.geocorner.net/post/why-you-should-use-devcontainers-for-your-geospatial-development)

# 介绍

在我最近的一篇文章中，发布在 TDS 上（[使用 Python 配置最小化 Docker 镜像进行空间分析](https://medium.com/towards-data-science/configuring-a-minimal-docker-image-for-spatial-analysis-with-python-dc9970ca8a8a)），我演示了如何为 Python 配置一个包含 GDAL 和 XArray 等必要地理空间分析工具的 Docker 镜像。管理软件包依赖性可能是具有挑战性的，特别是当涉及到专门的地理空间库时。在这方面，Docker 容器提供了一种优雅的解决方案，用于将系统部署到云服务器上。但开发过程本身呢？在这篇文章中，我将讨论那些改变了我对开发环境管理看法的关键因素，以及 DevContainers 和 Codespaces 如何改变地理空间开发工作流程。

# 理由

## 1- 一致环境的必要性…
