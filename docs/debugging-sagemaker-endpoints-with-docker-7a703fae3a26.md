# 使用 Docker 调试 SageMaker 终端节点

> 原文：[https://towardsdatascience.com/debugging-sagemaker-endpoints-with-docker-7a703fae3a26?source=collection_archive---------11-----------------------#2023-06-16](https://towardsdatascience.com/debugging-sagemaker-endpoints-with-docker-7a703fae3a26?source=collection_archive---------11-----------------------#2023-06-16)

## SageMaker 本地模式的替代方案

[](https://ram-vegiraju.medium.com/?source=post_page-----7a703fae3a26--------------------------------)[![Ram Vegiraju](../Images/07d9334e905f710d9f3c6187cf69a1a5.png)](https://ram-vegiraju.medium.com/?source=post_page-----7a703fae3a26--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7a703fae3a26--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7a703fae3a26--------------------------------) [Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----7a703fae3a26--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6e49569edd2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdebugging-sagemaker-endpoints-with-docker-7a703fae3a26&user=Ram+Vegiraju&userId=6e49569edd2b&source=post_page-6e49569edd2b----7a703fae3a26---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7a703fae3a26--------------------------------) ·6 分钟阅读·2023 年 6 月 16 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7a703fae3a26&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdebugging-sagemaker-endpoints-with-docker-7a703fae3a26&source=-----7a703fae3a26---------------------bookmark_footer-----------)![](../Images/0b957e3ecdec75d862d1a637b72d9fe3.png)

图片来自 [Unsplash](https://unsplash.com/photos/1VW6HLOQE5A) 由 [**Mohammad Rahmani**](https://unsplash.com/@afgprogrammer) 提供

开始使用 [SageMaker 实时推理](https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints.html) 的一个难点是，有时很难进行调试。当创建一个终端节点时，您需要确保有多个要素正确配置，以确保成功部署。

+   适当的模型工件文件结构取决于您使用的模型服务器和容器。基本上，您提供的 model.tar.gz 必须符合模型服务器的格式要求。

+   如果你有一个自定义的推理脚本用于实现模型的前后处理，你需要确保实现的处理程序与模型服务器兼容，并且代码层面没有脚本错误。

我们之前讨论过 [SageMaker 本地模式](/debugging-sagemaker-endpoints-quickly-with-local-mode-2975bd55f6f7)，但在这篇文章撰写时，本地模式并不支持 SageMaker 部署所有可用的托管选项和模型服务器。

为了克服这个限制，我们将使用 [Docker](https://www.docker.com/) 和一个示例模型，了解如何在 SageMaker 部署之前测试/调试我们的模型文档和推理脚本。在这个具体的例子中，我们将利用 [BART 模型](https://huggingface.co/facebook/bart-large)，这是我在我的 [上一篇文章](/deploying-llms-on-amazon-sagemaker-with-djl-serving-8220e3cfad0c) 中介绍过的，并且看看如何通过 Docker 托管它。
