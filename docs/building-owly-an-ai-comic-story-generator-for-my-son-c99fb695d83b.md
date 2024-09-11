# 为我儿子打造 Owly，一个 AI 漫画视频生成器

> 原文：[https://towardsdatascience.com/building-owly-an-ai-comic-story-generator-for-my-son-c99fb695d83b?source=collection_archive---------5-----------------------#2023-04-11](https://towardsdatascience.com/building-owly-an-ai-comic-story-generator-for-my-son-c99fb695d83b?source=collection_archive---------5-----------------------#2023-04-11)

## 利用在 Amazon SageMaker JumpStart 上微调的 Stable Diffusion 2.1，我开发了一种名为 Owly 的 AI 技术，它可以制作个性化的漫画视频，并配有音乐，以我儿子的玩具作为主要角色。

[](https://agustinus-nalwan.medium.com/?source=post_page-----c99fb695d83b--------------------------------)[![Agustinus Nalwan](../Images/7c5ade9ab8bca1d27a317b5c09d1b734.png)](https://agustinus-nalwan.medium.com/?source=post_page-----c99fb695d83b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c99fb695d83b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c99fb695d83b--------------------------------) [Agustinus Nalwan](https://agustinus-nalwan.medium.com/?source=post_page-----c99fb695d83b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8b7ab157b0a4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-owly-an-ai-comic-story-generator-for-my-son-c99fb695d83b&user=Agustinus+Nalwan&userId=8b7ab157b0a4&source=post_page-8b7ab157b0a4----c99fb695d83b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c99fb695d83b--------------------------------) ·24 分钟阅读·2023年4月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc99fb695d83b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-owly-an-ai-comic-story-generator-for-my-son-c99fb695d83b&user=Agustinus+Nalwan&userId=8b7ab157b0a4&source=-----c99fb695d83b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc99fb695d83b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-owly-an-ai-comic-story-generator-for-my-son-c99fb695d83b&source=-----c99fb695d83b---------------------bookmark_footer-----------)![](../Images/e337e899f2f9184b93e905364049f799.png)

Owly，AI 漫画讲述者 [AI 生成图像]

每晚，和我4岁的儿子Dexie分享睡前故事已经成为了一个珍贵的习惯，他非常喜欢这些故事。他的书籍收藏相当可观，但他特别着迷于我创作的原创故事。这种创作方式也让我可以融入我希望他学习的道德价值观，这在商店购买的书籍中很难找到。随着时间的推移，我磨练了创作个性化故事的技巧，这些故事能点燃他的想象力——从有破裂墙壁的龙到寻找陪伴的孤独天灯。最近，我一直在编织关于虚构超级英雄的故事，如慢动作先生和放屁侠，这些已经成为他的最爱。

虽然对我来说这段旅程非常愉快，但经过半年的夜间讲故事，我的创意储备正在经受考验。为了让他保持对新鲜有趣故事的兴趣而不至于使自己精疲力竭，我需要一个更可持续的解决方案——一种能够自动生成吸引人故事的AI技术！我给它取了个名字……
