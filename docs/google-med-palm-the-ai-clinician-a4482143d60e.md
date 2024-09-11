# Google Med-PaLM: AI 临床医生

> 原文：[https://towardsdatascience.com/google-med-palm-the-ai-clinician-a4482143d60e?source=collection_archive---------1-----------------------#2023-03-17](https://towardsdatascience.com/google-med-palm-the-ai-clinician-a4482143d60e?source=collection_archive---------1-----------------------#2023-03-17)

## 人工智能 | 医学 | 自然语言处理 |

## Google 的新模型经过训练以回答医学问题。如何做到的？

[](https://salvatore-raieli.medium.com/?source=post_page-----a4482143d60e--------------------------------)[![Salvatore Raieli](../Images/6bb4520e2df40d20283e7283141b5e06.png)](https://salvatore-raieli.medium.com/?source=post_page-----a4482143d60e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a4482143d60e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a4482143d60e--------------------------------) [Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----a4482143d60e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff1a08d9452cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogle-med-palm-the-ai-clinician-a4482143d60e&user=Salvatore+Raieli&userId=f1a08d9452cd&source=post_page-f1a08d9452cd----a4482143d60e---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4482143d60e--------------------------------) ·14 min read·2023年3月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa4482143d60e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogle-med-palm-the-ai-clinician-a4482143d60e&user=Salvatore+Raieli&userId=f1a08d9452cd&source=-----a4482143d60e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa4482143d60e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogle-med-palm-the-ai-clinician-a4482143d60e&source=-----a4482143d60e---------------------bookmark_footer-----------)![](../Images/343cb55205bd7b26fac1907a4976f5de.png)

图片由作者使用 OpenAI DALL-E 创建

医学也基于患者和医生之间的互动。此外，尽管患者会接受不同的测试和影像技术，但总会有书面报告。那么，为什么用于医学和医疗保健的 AI 模型未能充分利用语言呢？

# 医学的基础模型？

![](../Images/9568bb410aa816f7d631aa907308b6bc.png)

图片由 [Myriam Zilles](https://unsplash.com/it/@myriamzilles) 在 Unsplash 提供

近年来的趋势是试图约束大型[语言模型](https://en.wikipedia.org/wiki/Language_model)（LMs），然后对其进行[微调](https://en.wikipedia.org/wiki/Fine-tuning_(machine_learning))以满足所需的应用。类似的方法也可以应用于医学文本的大型语料库，以便模型能够学习到有用的表示。一个对医学主题有良好理解的模型可能在无数应用中发挥作用（患者分诊、知识检索、关键发现总结、诊断辅助等）。

问题在于医学领域是一个特殊领域。与其他领域相比，这里存在不同的问题，甚至更为复杂……
