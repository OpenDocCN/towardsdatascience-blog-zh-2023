# 钢琴需要多少个琴键才够用？

> 原文：[https://towardsdatascience.com/how-many-keys-are-enough-to-play-the-piano-639916f47a22?source=collection_archive---------5-----------------------#2023-12-23](https://towardsdatascience.com/how-many-keys-are-enough-to-play-the-piano-639916f47a22?source=collection_archive---------5-----------------------#2023-12-23)

## 使用Python、MIDI和Matplotlib寻找答案

[](https://dmitryelj.medium.com/?source=post_page-----639916f47a22--------------------------------)[![Dmitrii Eliuseev](../Images/7c48f0c016930ead59ddb785eaf3e0e6.png)](https://dmitryelj.medium.com/?source=post_page-----639916f47a22--------------------------------)[](https://towardsdatascience.com/?source=post_page-----639916f47a22--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----639916f47a22--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----639916f47a22--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F65c1f6ba75db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-many-keys-are-enough-to-play-the-piano-639916f47a22&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=post_page-65c1f6ba75db----639916f47a22---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----639916f47a22--------------------------------) ·9分钟阅读·2023年12月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F639916f47a22&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-many-keys-are-enough-to-play-the-piano-639916f47a22&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=-----639916f47a22---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F639916f47a22&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-many-keys-are-enough-to-play-the-piano-639916f47a22&source=-----639916f47a22---------------------bookmark_footer-----------)![](../Images/c44ccfaa64fbed9efdb75a2dc66e3bf7.png)

由Cedrik Malabanan拍摄，[Unsplash](https://unsplash.com/@ohboyced)

学习弹钢琴是一个具有挑战性且有趣的过程，但每个初学者都会面临一个困境：应该买什么样的乐器？市场上的选择非常广泛，从上图中那种只有两个八度的小型乐器到重达70公斤的全尺寸木质键盘。一方面，更多的琴键总是有益的；另一方面，大小、重量和价格之间总是存在权衡。

显然，一种方法是直接询问老师他或她的推荐，但也许大多数音乐家从未从那个角度考虑过音乐。是否有更量化的方法来找到答案？实际上，答案是肯定的；我们可以轻松地使用Python制作音乐音符分布。

这个教程对数据科学初学者非常有用；它不需要复杂的数学或库，结果也很容易解读。它也对那些想学习音乐但尚未决定购买什么乐器的人有用。

让我们开始吧！

## 数据源

对于数据分析，我将使用[MIDI](https://en.wikipedia.org/wiki/MIDI) 文件。这是一个相当古老的格式；第一个MIDI（音乐仪器数字接口）规范是……
