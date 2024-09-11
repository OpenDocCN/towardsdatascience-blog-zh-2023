# 谷歌的 MusicLM：从文本描述到音乐

> 原文：[https://towardsdatascience.com/googles-musiclm-from-text-description-to-music-23794ab6955c?source=collection_archive---------6-----------------------#2023-02-01](https://towardsdatascience.com/googles-musiclm-from-text-description-to-music-23794ab6955c?source=collection_archive---------6-----------------------#2023-02-01)

## 一种新模型仅通过文本提示生成令人印象深刻的音乐

[](https://salvatore-raieli.medium.com/?source=post_page-----23794ab6955c--------------------------------)[![萨尔瓦托雷·赖利](../Images/6bb4520e2df40d20283e7283141b5e06.png)](https://salvatore-raieli.medium.com/?source=post_page-----23794ab6955c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----23794ab6955c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----23794ab6955c--------------------------------) [萨尔瓦托雷·赖利](https://salvatore-raieli.medium.com/?source=post_page-----23794ab6955c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff1a08d9452cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogles-musiclm-from-text-description-to-music-23794ab6955c&user=Salvatore+Raieli&userId=f1a08d9452cd&source=post_page-f1a08d9452cd----23794ab6955c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----23794ab6955c--------------------------------) ·8分钟阅读·2023年2月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F23794ab6955c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogles-musiclm-from-text-description-to-music-23794ab6955c&user=Salvatore+Raieli&userId=f1a08d9452cd&source=-----23794ab6955c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F23794ab6955c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogles-musiclm-from-text-description-to-music-23794ab6955c&source=-----23794ab6955c---------------------bookmark_footer-----------)![](../Images/effa592eb7e806938eac69eab439c2d5.png)

由作者使用 OpenAI DALL-E 生成的图像

谷歌发布了一款能够从[文本描述生成音乐](https://arxiv.org/pdf/2301.11325.pdf)的新模型。结果？令人印象深刻。

**实际上，将生成式 AI 应用于音乐并不是一个新概念。** 近年来已有几个尝试，包括 Riffusion、[Dance Diffusion](https://techcrunch.com/2022/10/07/ai-music-generator-dance-diffusion/)、微软的 Museformer 和 [OpenAI 的 Jukebox](https://openai.com/blog/jukebox/)。 Google 自己之前也发布了一个名为 [AudioML](https://ai.googleblog.com/2022/10/audiolm-language-modeling-approach-to.html) 的模型。 **那么，这个模型会有什么不同？**

[](https://medium.com/mlearning-ai/microsofts-museformer-ai-music-is-the-new-frontier-8dc5cb24459c?source=post_page-----23794ab6955c--------------------------------) [## 微软的 Museformer：AI 音乐是新的前沿领域

### AI 艺术正在爆炸，音乐可能是下一个领域。

[medium.com](https://medium.com/mlearning-ai/microsofts-museformer-ai-music-is-the-new-frontier-8dc5cb24459c?source=post_page-----23794ab6955c--------------------------------)

# 为什么用 AI 生成音乐如此困难？

![](../Images/bdab379e9038b789e6a153a8df2fdc7a.png)

图片来自 [Marius Masalar](https://unsplash.com/it/@marius) 于 unsplash.com

与此同时，之前的模型显示出明显的技术和质量问题。结果陈旧，歌曲不够复杂，常常重复，并且仍然不够高保真。
