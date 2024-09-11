# 我在不到10分钟内用LlamaIndex构建了一个定制的GPT聊天机器人

> 原文：[https://towardsdatascience.com/how-i-built-a-custom-gpt-based-chatbot-in-under-10-minutes-with-llamaindex-2102f0173420?source=collection_archive---------2-----------------------#2023-04-12](https://towardsdatascience.com/how-i-built-a-custom-gpt-based-chatbot-in-under-10-minutes-with-llamaindex-2102f0173420?source=collection_archive---------2-----------------------#2023-04-12)

## 使用Python的概述和实现

[](https://medium.com/@aashishnair?source=post_page-----2102f0173420--------------------------------)[![Aashish Nair](../Images/23f4b3839e464419332b690a4098d824.png)](https://medium.com/@aashishnair?source=post_page-----2102f0173420--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2102f0173420--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2102f0173420--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----2102f0173420--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3087ba81e065&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-built-a-custom-gpt-based-chatbot-in-under-10-minutes-with-llamaindex-2102f0173420&user=Aashish+Nair&userId=3087ba81e065&source=post_page-3087ba81e065----2102f0173420---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2102f0173420--------------------------------) ·6 min read·2023年4月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2102f0173420&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-built-a-custom-gpt-based-chatbot-in-under-10-minutes-with-llamaindex-2102f0173420&user=Aashish+Nair&userId=3087ba81e065&source=-----2102f0173420---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2102f0173420&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-built-a-custom-gpt-based-chatbot-in-under-10-minutes-with-llamaindex-2102f0173420&source=-----2102f0173420---------------------bookmark_footer-----------)![](../Images/1ae789d4b6cc2e0d3b71df8d0b8fd123.png)

图片来源于Miguel Á. Padriñán: [https://www.pexels.com/photo/two-white-message-balloons-1111368/](https://www.pexels.com/photo/two-white-message-balloons-1111368/)

不久前，我读了一篇Jerry Liu的文章，介绍了LlamaIndex，一个利用GPT根据用户提供的信息合成查询响应的接口。

[## 如何利用Unstructured和LlamaIndex将LLMs的力量带到你的数据中](https://medium.com/@jerryjliu98/how-unstructured-and-llamaindex-can-help-bring-the-power-of-llms-to-your-own-data-3657d063e30d?source=post_page-----2102f0173420--------------------------------)

### （由LlamaIndex的创始人Jerry Liu和Unstructured的CEO Brian Raymond合著）

[medium.com](https://medium.com/@jerryjliu98/how-unstructured-and-llamaindex-can-help-bring-the-power-of-llms-to-your-own-data-3657d063e30d?source=post_page-----2102f0173420--------------------------------)

这立刻引起了我的注意，我知道我必须亲自尝试一下。

毕竟，许多引起世界轰动的大型语言模型（LLMs）由于没有用用户可用的数据进行训练，因此其应用场景有限。

因此，仍然需要根据用户需求定制的LLMs。简单来说，企业需要能够处理文本摘要、问答（Q&A）和使用自身信息生成文本的LLMs。

为了看看LlamaIndex是否有潜力满足这种需求，我尝试了它的功能，并对我能够做到的事情感到真正惊讶。我甚至在大约10分钟内构建了一个问答聊天机器人…
