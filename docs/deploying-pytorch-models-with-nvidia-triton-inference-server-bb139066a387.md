# 使用 Nvidia Triton 推理服务器部署 PyTorch 模型

> 原文：[`towardsdatascience.com/deploying-pytorch-models-with-nvidia-triton-inference-server-bb139066a387?source=collection_archive---------4-----------------------#2023-09-14`](https://towardsdatascience.com/deploying-pytorch-models-with-nvidia-triton-inference-server-bb139066a387?source=collection_archive---------4-----------------------#2023-09-14)

## 一种灵活的高性能模型服务解决方案

[](https://ram-vegiraju.medium.com/?source=post_page-----bb139066a387--------------------------------)![Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----bb139066a387--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bb139066a387--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb139066a387--------------------------------) [Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----bb139066a387--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6e49569edd2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-pytorch-models-with-nvidia-triton-inference-server-bb139066a387&user=Ram+Vegiraju&userId=6e49569edd2b&source=post_page-6e49569edd2b----bb139066a387---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb139066a387--------------------------------) ·7 分钟阅读·2023 年 9 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbb139066a387&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-pytorch-models-with-nvidia-triton-inference-server-bb139066a387&user=Ram+Vegiraju&userId=6e49569edd2b&source=-----bb139066a387---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbb139066a387&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-pytorch-models-with-nvidia-triton-inference-server-bb139066a387&source=-----bb139066a387---------------------bookmark_footer-----------)![](img/eb2afba9c81fb0c85c927e7f318bc8e1.png)

图片来自于 [Unsplash](https://unsplash.com/photos/8fVK5TFKaSE)

机器学习（ML）的价值在实际应用中真正体现，当我们进入到[模型托管和推理](https://cloud.google.com/bigquery/docs/inference-overview#:~:text=Machine%20learning%20inference%20is%20the,machine%20learning%20model%20into%20production.%22)。如果没有高性能的模型服务解决方案，帮助你的模型进行扩展和收缩，就很难将 ML 工作负载投入生产。

**什么是模型服务器/什么是模型服务？** 可以把模型服务器看作是机器学习领域中类似于 web 服务器的东西。仅仅依靠大量的硬件是不够的，你还需要一个通信层来处理客户端的请求，同时有效地分配硬件，以应对应用程序所遇到的流量。模型服务器是一个可调节的功能：我们可以通过控制诸如 gRPC 与 REST 等方面，从延迟的角度挤出性能。常见的模型服务器示例包括以下几种：

+   [TensorFlow Serving](https://www.tensorflow.org/tfx/guide/serving)

+   [TorchServe](https://pytorch.org/serve/)

+   [Multi-Model Server (MMS)](https://github.com/awslabs/multi-model-server)

+   [Deep Java Library (DJL)](https://github.com/deepjavalibrary/djl-serving)

我们今天探索的是[Nvidia Triton Inference Server](https://github.com/triton-inference-server/server)，这是一种高度灵活且高性能的模型服务解决方案。每个模型服务器都需要模型工件和推理脚本来…
