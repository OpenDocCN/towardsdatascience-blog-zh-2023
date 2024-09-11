# KServe：高度可扩展的 Kubernetes 机器学习部署

> 原文：[https://towardsdatascience.com/kserve-highly-scalable-machine-learning-deployment-with-kubernetes-aa7af0b71202?source=collection_archive---------6-----------------------#2023-05-29](https://towardsdatascience.com/kserve-highly-scalable-machine-learning-deployment-with-kubernetes-aa7af0b71202?source=collection_archive---------6-----------------------#2023-05-29)

## Kubernetes 模型推理变得简单易行。

[](https://medium.com/@lloyd.hamilton?source=post_page-----aa7af0b71202--------------------------------)[![Lloyd Hamilton](../Images/0c516beb55ed3aac21978b3bc85ca9b1.png)](https://medium.com/@lloyd.hamilton?source=post_page-----aa7af0b71202--------------------------------)[](https://towardsdatascience.com/?source=post_page-----aa7af0b71202--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----aa7af0b71202--------------------------------) [Lloyd Hamilton](https://medium.com/@lloyd.hamilton?source=post_page-----aa7af0b71202--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd40c1f932caa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fkserve-highly-scalable-machine-learning-deployment-with-kubernetes-aa7af0b71202&user=Lloyd+Hamilton&userId=d40c1f932caa&source=post_page-d40c1f932caa----aa7af0b71202---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----aa7af0b71202--------------------------------) ·10 分钟阅读·2023年5月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faa7af0b71202&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fkserve-highly-scalable-machine-learning-deployment-with-kubernetes-aa7af0b71202&user=Lloyd+Hamilton&userId=d40c1f932caa&source=-----aa7af0b71202---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faa7af0b71202&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fkserve-highly-scalable-machine-learning-deployment-with-kubernetes-aa7af0b71202&source=-----aa7af0b71202---------------------bookmark_footer-----------)![](../Images/65c9c56369db8df360025d18d9c73a22.png)

图片来源于 [KServe](https://kserve.github.io/website/0.10/)

随着 chatGPT 的发布，越来越难以避免使用机器学习技术。从消息应用中的文本预测到智能门铃上的面部识别，机器学习（ML）几乎出现在我们今天使用的每一项技术中。

如何将机器学习技术交付给消费者是组织在开发过程中必须面对的众多挑战之一。机器学习产品的部署策略对最终用户有着重大影响。这可能意味着你在 iPhone 上的 Siri 或在网页浏览器中的 chatGPT。

超越 ChatGPT 那流畅的用户界面和过于自信的聊天对话，隐藏着部署大型语言机器学习模型所需的复杂机制。ChatGPT 建立在一个高度可扩展的框架上，旨在支持模型在其快速增长期间的交付和支持。实际上，实际的机器学习模型仅占整个项目的一小部分。这类项目通常是跨学科的，需具备数据工程、数据科学和软件开发方面的专业知识。因此，简化模型部署过程的框架变得越来越重要……
