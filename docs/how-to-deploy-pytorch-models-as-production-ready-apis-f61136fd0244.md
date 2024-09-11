# 如何将 PyTorch 模型部署为生产就绪的 API

> 原文：[`towardsdatascience.com/how-to-deploy-pytorch-models-as-production-ready-apis-f61136fd0244?source=collection_archive---------7-----------------------#2023-04-03`](https://towardsdatascience.com/how-to-deploy-pytorch-models-as-production-ready-apis-f61136fd0244?source=collection_archive---------7-----------------------#2023-04-03)

## 一个结合 PyTorch Lightning 和 BentoML 的端到端用例

[](https://ahmedbesbes.medium.com/?source=post_page-----f61136fd0244--------------------------------)![Ahmed Besbes](https://ahmedbesbes.medium.com/?source=post_page-----f61136fd0244--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f61136fd0244--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f61136fd0244--------------------------------) [Ahmed Besbes](https://ahmedbesbes.medium.com/?source=post_page-----f61136fd0244--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fadc8ea174c69&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-deploy-pytorch-models-as-production-ready-apis-f61136fd0244&user=Ahmed+Besbes&userId=adc8ea174c69&source=post_page-adc8ea174c69----f61136fd0244---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f61136fd0244--------------------------------) · 12 分钟阅读 · 2023 年 4 月 3 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff61136fd0244&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-deploy-pytorch-models-as-production-ready-apis-f61136fd0244&user=Ahmed+Besbes&userId=adc8ea174c69&source=-----f61136fd0244---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff61136fd0244&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-deploy-pytorch-models-as-production-ready-apis-f61136fd0244&source=-----f61136fd0244---------------------bookmark_footer-----------)![](img/3d420812acc306b94ced6405638c8222.png)

图片由 [SpaceX](https://unsplash.com/@spacex?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

作为一名 ML 工程师，我在处理生产环境中的深度学习模型时经常遇到两个主要挑战。

首先是每个项目都需要重写样板代码来处理训练循环、数据加载或度量计算等任务。由于这些工作通常使代码库变得更复杂，添加了不必要的抽象，因此减慢了迭代过程。

第二个挑战涉及到将训练好的模型有效部署到云端所需的广泛技能或工具。这包括 Docker、API 或云服务。更不用说与 GPU 支持、多线程或可扩展性相关的知识。

如果你是数据科学家，你显然不需要知道这些所有的内容。

> *幸运的是，有两个 Python 框架旨在缓解这些问题：* [***Pytorch Lightning***](https://github.com/Lightning-AI/lightning) *和* [***BentoML***](https://github.com/bentoml/BentoML)*。
> 
> 在这篇文章中，我们将结合这些框架来构建* ***一个生产就绪的图像分类服务*** *并部署到一个原生 Kubernetes 环境中。*

让我们来看看 🔍
