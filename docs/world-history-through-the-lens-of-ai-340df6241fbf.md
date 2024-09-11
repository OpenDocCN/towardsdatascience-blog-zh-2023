# 通过人工智能看世界历史

> 原文：[https://towardsdatascience.com/world-history-through-the-lens-of-ai-340df6241fbf?source=collection_archive---------10-----------------------#2023-07-07](https://towardsdatascience.com/world-history-through-the-lens-of-ai-340df6241fbf?source=collection_archive---------10-----------------------#2023-07-07)

## 语言模型编码了哪些历史知识？

[](https://medium.com/@artfish?source=post_page-----340df6241fbf--------------------------------)[![Yennie Jun](../Images/b635e965f21c3d55833269e12e861322.png)](https://medium.com/@artfish?source=post_page-----340df6241fbf--------------------------------)[](https://towardsdatascience.com/?source=post_page-----340df6241fbf--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----340df6241fbf--------------------------------) [Yennie Jun](https://medium.com/@artfish?source=post_page-----340df6241fbf--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F12ca1ab81192&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fworld-history-through-the-lens-of-ai-340df6241fbf&user=Yennie+Jun&userId=12ca1ab81192&source=post_page-12ca1ab81192----340df6241fbf---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----340df6241fbf--------------------------------) ·11 min read·2023年7月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F340df6241fbf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fworld-history-through-the-lens-of-ai-340df6241fbf&user=Yennie+Jun&userId=12ca1ab81192&source=-----340df6241fbf---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F340df6241fbf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fworld-history-through-the-lens-of-ai-340df6241fbf&source=-----340df6241fbf---------------------bookmark_footer-----------)![](../Images/d14490fb1ce7bc05910c5287b7e27954.png)

探讨OpenAI的GPT-4、Anthropic的Claude和TII的Falcon 40B Instruct对1910年以来的重大历史事件的理解（以6种不同语言进行提示）。由作者创作。

*本文最初* [*发表在我的博客上*](https://www.artfish.ai/p/world-history-through-ai)

人工智能的进步，特别是大型语言模型，为[历史研究](https://ts2.space/en/chatgpt-4-a-valuable-tool-for-historical-research-and-analysis/#:~:text=ChatGPT%2D4%20works%20by%20taking,information%20available%20in%20its%20database.)和[教育](https://www.history4humans.com/blogs/history/7-ways-history-teachers-can-use-chat-gpt-in-the-classroom)开辟了令人兴奋的可能性。然而，重要的是要审视这些模型解读和回忆过去的方式。它们是否反映了其对历史理解中的固有偏见？

我非常清楚历史的主观性（我在本科时主修历史！）。我们记住的事件和我们形成的关于过去的叙述，受到编撰这些叙述的历史学家和我们所处社会的重大影响。例如，我的高中世界历史课程将超过75%的课程内容用于欧洲历史，这使得我对世界事件的理解产生了偏差。

在这篇文章中，我探讨了人类历史如何通过人工智能的视角被记忆和解读。我审视了几种大型语言模型对关键历史事件的解释，以揭示：

+   这些模型是否表现出对事件的西方或美国偏见？

+   这些模型的历史解释是否因使用的语言而异，例如，韩语或法语的提示是否更多强调韩国或法国…
