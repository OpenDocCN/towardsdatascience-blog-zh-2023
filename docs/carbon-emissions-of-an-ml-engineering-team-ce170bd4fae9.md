# 机器学习工程团队的碳排放

> 原文：[https://towardsdatascience.com/carbon-emissions-of-an-ml-engineering-team-ce170bd4fae9?source=collection_archive---------5-----------------------#2023-10-16](https://towardsdatascience.com/carbon-emissions-of-an-ml-engineering-team-ce170bd4fae9?source=collection_archive---------5-----------------------#2023-10-16)

## 发展中的隐性成本真正重要

[](https://medium.com/@teosiyang?source=post_page-----ce170bd4fae9--------------------------------)[![Jake Teo](../Images/9687f43822fab69befb750a8ec58516d.png)](https://medium.com/@teosiyang?source=post_page-----ce170bd4fae9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ce170bd4fae9--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ce170bd4fae9--------------------------------) [Jake Teo](https://medium.com/@teosiyang?source=post_page-----ce170bd4fae9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F52b0d82d5bf5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcarbon-emissions-of-an-ml-engineering-team-ce170bd4fae9&user=Jake+Teo&userId=52b0d82d5bf5&source=post_page-52b0d82d5bf5----ce170bd4fae9---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----ce170bd4fae9--------------------------------) · 9分钟阅读 · 2023年10月16日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fce170bd4fae9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcarbon-emissions-of-an-ml-engineering-team-ce170bd4fae9&source=-----ce170bd4fae9---------------------bookmark_footer-----------)

每个人都知道全球变暖导致的气候危机。为了防止其灾难性后果 [1]，世界需要大幅减少温室气体排放，许多国家设定了到2050年实现净零排放的目标。

近年来AI的技术繁荣也引发了对其环境成本的关注。如果我们只看它的直接贡献，那就是通过电力来训练和运行模型。例如，训练具有1750亿参数的ChatGPT-3产生了高达502吨的碳当量排放（tCO2e）[2]。新晋的Llama2训练其四个模型家族也产生了类似的539吨tCO2e排放[3]。作为参考，每一个这些排放量相当于乘客从纽约飞往旧金山单程飞行500次的排放量。

我在一个机器学习工程团队工作，这个问题也不断地刺激着我。我们通过电力消耗贡献了多少碳排放？有没有减少的办法？这也是我们自己进行碳核算的第一次尝试。

![](../Images/a849f3ef32d0a93e1c7acdfa9bb2bc17.png)

图片由[Chris LeBoutillier](https://unsplash.com/@chrisleboutillier?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# **方法**
