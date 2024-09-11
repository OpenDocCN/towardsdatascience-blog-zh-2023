# 摩托车共享系统前往爱斯托尼亚塔尔图的**金属乐队**音乐会

> 原文：[https://towardsdatascience.com/bike-sharing-system-movements-to-the-metallica-concert-in-tartu-estonia-1af8361bc6f?source=collection_archive---------21-----------------------#2023-01-04](https://towardsdatascience.com/bike-sharing-system-movements-to-the-metallica-concert-in-tartu-estonia-1af8361bc6f?source=collection_archive---------21-----------------------#2023-01-04)

## 使用Movingpandas和KeplerGl进行GPS跟踪可视化 — 教程

[](https://bryanvallejo16.medium.com/?source=post_page-----1af8361bc6f--------------------------------)[![Bryan R. Vallejo](../Images/fd92974f57c72875cc133a2c959d64ca.png)](https://bryanvallejo16.medium.com/?source=post_page-----1af8361bc6f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1af8361bc6f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1af8361bc6f--------------------------------) [Bryan R. Vallejo](https://bryanvallejo16.medium.com/?source=post_page-----1af8361bc6f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcbd681aaa725&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbike-sharing-system-movements-to-the-metallica-concert-in-tartu-estonia-1af8361bc6f&user=Bryan+R.+Vallejo&userId=cbd681aaa725&source=post_page-cbd681aaa725----1af8361bc6f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1af8361bc6f--------------------------------) ·9分钟阅读·2023年1月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1af8361bc6f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbike-sharing-system-movements-to-the-metallica-concert-in-tartu-estonia-1af8361bc6f&user=Bryan+R.+Vallejo&userId=cbd681aaa725&source=-----1af8361bc6f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1af8361bc6f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbike-sharing-system-movements-to-the-metallica-concert-in-tartu-estonia-1af8361bc6f&source=-----1af8361bc6f---------------------bookmark_footer-----------)![](../Images/6fea2e94047d72eff0d6ae35bdf5d6d3.png)

图片由作者提供。摩托车共享系统前往金属乐队音乐会的自行车站。2019年7月18日，塔尔图-爱斯托尼亚

# 引言

[塔尔图的金属乐队音乐会](https://www.metallica.com/tour/2019-07-18-tartu-estonia.html)（爱沙尼亚）于2019年7月18日在爱沙尼亚国家博物馆（ERM）后面的拉迪空军基地举行。该活动的门票全部售罄，吸引了60,000人（[ERR, 2019](https://news.err.ee/963069/gallery-metallica-gives-sold-out-show-in-tartu)）。市政府建议访客使用公共交通，包括自行车共享系统，并在活动期间通过名为“Metallica parkla”的虚拟停靠站来改善城市交通，访客可以在场馆附近停放自行车。

> [自行车共享移动的网页地图在此](https://bryanvallejo16.github.io/bike-moves-metallica/root/metallica_moves.html)
> 
> [代码仓库](https://github.com/bryanvallejo16/bike-moves-metallica)

塔尔图的自行车共享系统在智能出行方面取得了成功。自行车高效且使城市交通变得便捷。订阅费用合理，500/750辆自行车是电动的。车站分布在城市各处，方便你到达任何需要的地方。该系统于2019年6月启动，经过一个月的使用，它在60,000人次的活动中被用来改善出行。系统提供用户锁车和解锁的实时数据。毫无疑问，塔尔图的自行车共享系统取得了成功。如果你愿意了解…
