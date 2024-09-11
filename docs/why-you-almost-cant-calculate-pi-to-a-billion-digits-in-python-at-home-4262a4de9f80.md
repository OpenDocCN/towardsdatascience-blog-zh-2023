# 为什么你（几乎）无法在家用 Python 计算到十亿位的 π

> 原文：[`towardsdatascience.com/why-you-almost-cant-calculate-pi-to-a-billion-digits-in-python-at-home-4262a4de9f80?source=collection_archive---------6-----------------------#2023-10-09`](https://towardsdatascience.com/why-you-almost-cant-calculate-pi-to-a-billion-digits-in-python-at-home-4262a4de9f80?source=collection_archive---------6-----------------------#2023-10-09)

## 比你想象的要困难

[](https://ibexorigin.medium.com/?source=post_page-----4262a4de9f80--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----4262a4de9f80--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4262a4de9f80--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4262a4de9f80--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----4262a4de9f80--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39db050c2ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-you-almost-cant-calculate-pi-to-a-billion-digits-in-python-at-home-4262a4de9f80&user=Bex+T.&userId=39db050c2ac2&source=post_page-39db050c2ac2----4262a4de9f80---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4262a4de9f80--------------------------------) ·9 分钟阅读·2023 年 10 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4262a4de9f80&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-you-almost-cant-calculate-pi-to-a-billion-digits-in-python-at-home-4262a4de9f80&user=Bex+T.&userId=39db050c2ac2&source=-----4262a4de9f80---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4262a4de9f80&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-you-almost-cant-calculate-pi-to-a-billion-digits-in-python-at-home-4262a4de9f80&source=-----4262a4de9f80---------------------bookmark_footer-----------)![](img/470ca271edba8912cae6b51f3a61c164.png)

图片由我使用 Midjourney 创建。

## 介绍

2022 年 6 月 9 日，谷歌创下了计算 π 最多位数的新世界纪录——100 万亿位！这一宏伟成就得益于在谷歌云上运行的 y-cruncher 程序。它计算了整整 157 天、23 小时、31 分钟和 7.651 秒。

如果十亿比十万亿小 100 万倍，那么运行时间会相应减少吗？换句话说，它只需要 136 秒吗？

但 136 秒过于雄心勃勃。家用电脑远不如谷歌云最强大的环境。那么，24 小时这样的运行时间如何？

结果是，即便是计算十亿位的圆周率也在 24 小时内是一个巨大的空想。本文通过 Python 中的证据解释了原因。

## 首先，`math.pi` 有什么问题？

```py
import math

print(math.pi)
```

```py
3.141592653589793
```

`math.pi` 的精度为 15 位数字。虽然不多，但足以满足科学中的最高精度计算。
