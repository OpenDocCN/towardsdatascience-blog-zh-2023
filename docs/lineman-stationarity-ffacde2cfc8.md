# 线员稳定性

> 原文：[https://towardsdatascience.com/lineman-stationarity-ffacde2cfc8?source=collection_archive---------14-----------------------#2023-02-06](https://towardsdatascience.com/lineman-stationarity-ffacde2cfc8?source=collection_archive---------14-----------------------#2023-02-06)

## 对进攻线员的数据驱动指标

[](https://harrisonfhoffman.medium.com/?source=post_page-----ffacde2cfc8--------------------------------)[![哈里森·霍夫曼](../Images/5eaa3e2bd0507297eb6c4a7efcf06324.png)](https://harrisonfhoffman.medium.com/?source=post_page-----ffacde2cfc8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ffacde2cfc8--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ffacde2cfc8--------------------------------) [哈里森·霍夫曼](https://harrisonfhoffman.medium.com/?source=post_page-----ffacde2cfc8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F38889d0801d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flineman-stationarity-ffacde2cfc8&user=Harrison+Hoffman&userId=38889d0801d0&source=post_page-38889d0801d0----ffacde2cfc8---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----ffacde2cfc8--------------------------------) · 6 分钟阅读 · 2023年2月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fffacde2cfc8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flineman-stationarity-ffacde2cfc8&user=Harrison+Hoffman&userId=38889d0801d0&source=-----ffacde2cfc8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fffacde2cfc8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flineman-stationarity-ffacde2cfc8&source=-----ffacde2cfc8---------------------bookmark_footer-----------)

进攻线创造的口袋是传球游戏中的关键元素，因为四分卫需要一个干净的口袋来准确和及时地传球。一个好的口袋为四分卫提供了充足的空间和时间来移动和传球，而一个差的口袋则会迅速塌陷，迫使四分卫 scramble 或被擒杀（除非四分卫是帕特里克·马霍姆斯）。虽然很容易可视化一个好的或差的口袋，但量化其质量，以及更广泛地量化线员的表现，并不简单。

本文提出了一种新的指标称为“球员稳定性”，它测量了进攻线员从球被发球到下一个事件发生之间的移动距离。球员稳定性可以在每一次进攻中计算，并且对评估进攻线员的表现和疲劳状态非常有用。通过跟踪线员在每次进攻中的运动和承受的压力，教练和分析师可以利用这个指标来深入了解进攻线在比赛中的整体效果。所有的指标和可视化都是使用[2023 NFL 大数据碗](https://www.kaggle.com/competitions/nfl-big-data-bowl-2023)的数据生成的。

# 什么是球员稳定性？

球员稳定性是衡量进攻线员在球被发球后，从起始位置移动的远近的一个指标。它通过计算球被发球时和第一次事件发生之间的码线位置差来得出。
