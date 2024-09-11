# 音乐发现的未来：搜索与生成

> 原文：[https://towardsdatascience.com/the-future-of-music-discovery-search-vs-generation-4d565a08515e?source=collection_archive---------6-----------------------#2023-08-22](https://towardsdatascience.com/the-future-of-music-discovery-search-vs-generation-4d565a08515e?source=collection_archive---------6-----------------------#2023-08-22)

## AI时代的功能性音乐

[](https://medium.com/@maxhilsdorf?source=post_page-----4d565a08515e--------------------------------)[![Max Hilsdorf](../Images/01da76c553e43d5ed6b6849bdbfd00da.png)](https://medium.com/@maxhilsdorf?source=post_page-----4d565a08515e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4d565a08515e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4d565a08515e--------------------------------) [Max Hilsdorf](https://medium.com/@maxhilsdorf?source=post_page-----4d565a08515e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd0c085a74ae8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-future-of-music-discovery-search-vs-generation-4d565a08515e&user=Max+Hilsdorf&userId=d0c085a74ae8&source=post_page-d0c085a74ae8----4d565a08515e---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4d565a08515e--------------------------------) ·11分钟阅读·2023年8月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4d565a08515e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-future-of-music-discovery-search-vs-generation-4d565a08515e&user=Max+Hilsdorf&userId=d0c085a74ae8&source=-----4d565a08515e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4d565a08515e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-future-of-music-discovery-search-vs-generation-4d565a08515e&source=-----4d565a08515e---------------------bookmark_footer-----------)![](../Images/a191d73272b4e33bb2b1fdedf24393a7.png)

使用 DALL-E 2 创建。提示：“大脑和放大镜，背景为乐谱，数字艺术”

# 为什么这个问题很重要

大约十年前，音乐流媒体服务公司在争夺最佳音乐推荐系统。显然，一个完美的推荐系统会每次都为用户提供准确满足其需求的音乐。然而，有些人将推荐系统视为过渡性技术。最终，无论你的音乐目录多大，也不可能为每个可能的用户请求提供完美的匹配。

现代生成性人工智能系统可能通过生成针对每个请求（机器人）量身定制的音乐来解决这个问题。另一方面，这些生成系统仍然无法生产高质量的音乐，具有巨大的计算成本，并且面临复杂的伦理和法律问题。

因此，本文旨在比较基于搜索的音乐检索和音乐生成的优缺点，以了解我们是否应该期待生成系统完全替代、增强，或甚至不影响当前的解决方案。在开始之前，让我们定义一下“搜索算法”和“生成模型”的含义。

## 搜索算法
