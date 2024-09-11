# Edge Emotion Recognition: 通过实时语音分析提升人机互动

> 原文：[https://towardsdatascience.com/edge-emotion-recognition-enhancing-human-machine-interaction-through-real-time-speech-analysis-235ee97cc5f6?source=collection_archive---------2-----------------------#2023-07-23](https://towardsdatascience.com/edge-emotion-recognition-enhancing-human-machine-interaction-through-real-time-speech-analysis-235ee97cc5f6?source=collection_archive---------2-----------------------#2023-07-23)

[](https://medium.com/@ruetsi?source=post_page-----235ee97cc5f6--------------------------------)[![Rüdiger Buchkremer, PhD](../Images/c116ab76808a3b5c892e2917f1a679d7.png)](https://medium.com/@ruetsi?source=post_page-----235ee97cc5f6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----235ee97cc5f6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----235ee97cc5f6--------------------------------) [Rüdiger Buchkremer, PhD](https://medium.com/@ruetsi?source=post_page-----235ee97cc5f6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F808330b1b301&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fedge-emotion-recognition-enhancing-human-machine-interaction-through-real-time-speech-analysis-235ee97cc5f6&user=R%C3%BCdiger+Buchkremer%2C+PhD&userId=808330b1b301&source=post_page-808330b1b301----235ee97cc5f6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----235ee97cc5f6--------------------------------) · 13分钟阅读 · 2023年7月23日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F235ee97cc5f6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fedge-emotion-recognition-enhancing-human-machine-interaction-through-real-time-speech-analysis-235ee97cc5f6&user=R%C3%BCdiger+Buchkremer%2C+PhD&userId=808330b1b301&source=-----235ee97cc5f6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F235ee97cc5f6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fedge-emotion-recognition-enhancing-human-machine-interaction-through-real-time-speech-analysis-235ee97cc5f6&source=-----235ee97cc5f6---------------------bookmark_footer-----------)

在现代社会，我们与计算机的对话急剧增加。然而，这些技术奇迹对我们的情感却毫无察觉，这可能很不方便。在这篇文章中，我尝试揭示通过先进技术手段检测情感的有趣方法。不仅如此，我还将讲述一个在我们创新大学研究所开发的突破性程序的故事，该程序可以在没有网络连接的情况下操作。所以，系好安全带，准备好被情感识别技术的奇迹所吸引吧！

![](../Images/79d8f4540c56db0ace654d9785a3fcfe.png)

来源：作者，图像由人工智能生成（leonardo.ai）

## **背景故事**

人们通过的不仅仅是他们说的话来表达感情。他们的语调、说话的速度，甚至是中间的沉默，都可以揭示**快乐、悲伤、愤怒、恐惧、厌恶和惊讶。**

*但是标准计算机对这些含义毫无概念。它们只是处理语音的基本声音。*

最近，我越来越多地需要与计算机进行交流，通常需要有一个人类中介提供指导或直接回应我的询问。**这让我感到困扰**，因为这些计算机似乎完全没有意识到这种互动对我产生的情感影响，它们始终……
