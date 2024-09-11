# 如何检测大型语言模型的幻觉

> 原文：[https://towardsdatascience.com/real-time-llm-hallucination-detection-9a68bb292698?source=collection_archive---------0-----------------------#2023-12-31](https://towardsdatascience.com/real-time-llm-hallucination-detection-9a68bb292698?source=collection_archive---------0-----------------------#2023-12-31)

## 教授聊天机器人说“我不知道”

[](https://medium.com/@brezeanu.iulia?source=post_page-----9a68bb292698--------------------------------)[![Iulia Brezeanu](../Images/f108eeec620ec9be40778dfaceca4e6c.png)](https://medium.com/@brezeanu.iulia?source=post_page-----9a68bb292698--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9a68bb292698--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9a68bb292698--------------------------------) [Iulia Brezeanu](https://medium.com/@brezeanu.iulia?source=post_page-----9a68bb292698--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5548b8f29f30&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freal-time-llm-hallucination-detection-9a68bb292698&user=Iulia+Brezeanu&userId=5548b8f29f30&source=post_page-5548b8f29f30----9a68bb292698---------------------post_header-----------) 发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----9a68bb292698--------------------------------) ·10分钟阅读·2023年12月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9a68bb292698&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freal-time-llm-hallucination-detection-9a68bb292698&user=Iulia+Brezeanu&userId=5548b8f29f30&source=-----9a68bb292698---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9a68bb292698&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freal-time-llm-hallucination-detection-9a68bb292698&source=-----9a68bb292698---------------------bookmark_footer-----------)![](../Images/a1e0d8fcdb97251e27161a08f711a2c3.png)

图片由[visuals](https://unsplash.com/@visuals?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> **Evelyn Hartwell 是谁？**
> 
> Evelyn Hartwell 是一位加拿大芭蕾舞演员以及创始艺术总监…
> 
> Evelyn Hartwell 是一位加拿大芭蕾舞演员，也是创始艺术总监…
> 
> Evelyn Hartwell 是一位美国演员，以其在…中的角色而闻名

不，Evelyn Hartwell 不是一个拥有多个虚假身份的诈骗犯，过着虚假的三重生活从事各种职业。事实上，她根本不存在，但这个模型却没有告诉我它不知道，而是开始编造事实。我们面对的是大型语言模型的幻觉。

长篇详细的输出看起来可能非常有说服力，即使是虚构的。这是否意味着我们不能信任聊天机器人，每次都需要手动验证输出的真实性？幸运的是，通过适当的保护措施，可能有方法使聊天机器人不容易说出虚假的内容。

![](../Images/b8715346e238a15d9203a36d4b79d5ab.png)

text-davinci-003 提示完成了一个虚构人物的描述。图片由作者提供。

对于上述输出，我设置了较高的温度为 0.7\。我允许语言模型改变句子的结构，以避免每次生成相同的文本。输出之间的差异…
