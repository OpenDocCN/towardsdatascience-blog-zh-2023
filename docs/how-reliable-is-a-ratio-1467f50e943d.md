# 比率的可靠性如何？

> 原文：[`towardsdatascience.com/how-reliable-is-a-ratio-1467f50e943d?source=collection_archive---------7-----------------------#2023-12-08`](https://towardsdatascience.com/how-reliable-is-a-ratio-1467f50e943d?source=collection_archive---------7-----------------------#2023-12-08)

## 了解如何使用经验贝叶斯分析在 Python 中评估比率的可靠性

[](https://gustavorsantos.medium.com/?source=post_page-----1467f50e943d--------------------------------)![Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----1467f50e943d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1467f50e943d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1467f50e943d--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----1467f50e943d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4429d99b1245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-reliable-is-a-ratio-1467f50e943d&user=Gustavo+Santos&userId=4429d99b1245&source=post_page-4429d99b1245----1467f50e943d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1467f50e943d--------------------------------) ·9 分钟阅读·2023 年 12 月 8 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1467f50e943d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-reliable-is-a-ratio-1467f50e943d&user=Gustavo+Santos&userId=4429d99b1245&source=-----1467f50e943d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1467f50e943d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-reliable-is-a-ratio-1467f50e943d&source=-----1467f50e943d---------------------bookmark_footer-----------)![](img/101cda4d83b516311142c34ced48fec5.png)

照片由 [rupixen.com](https://unsplash.com/@rupixen?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，来源于 [Unsplash](https://unsplash.com/photos/person-using-laptop-computer-holding-card-Q59HmzK38eQ?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

# 介绍

我在数据科学领域的参考之一是 [Julia Silge](https://juliasilge.com/)。在她的 *Tidy Tuesday* 视频中，她总是制作代码跟随类的视频教授/展示某个技术，帮助其他分析师提升技能并将其纳入自己的工具箱。

上周二的话题是经验贝叶斯（她的 [博客文章](https://juliasilge.com/blog/doctor-who-bayes/)），引起了我的注意。

但是，这是什么？

# 经验贝叶斯

经验贝叶斯是一种统计方法，当我们处理像 *[成功]/[总尝试]* 这样的比例时会用到。当我们处理这些变量时，很多时候我们会遇到 1/2 的成功率，这意味着 50% 的成功率，或者 3/4（75%），0/1（0%）。

那些极端的百分比并不能代表长期的现实，因为尝试次数非常少，这使得很难判断是否存在趋势，而且这些情况大多数时候会被忽略或删除。需要更多的尝试才能确定真实的成功率，例如 30/60、500/100 或其他对业务有意义的比例。

使用经验贝叶斯方法，我们能够利用当前的数据分布进行计算……
