# 使用 Python 分析地理空间数据

> 原文：[`towardsdatascience.com/analyzing-geospatial-data-with-python-7244c1b9e302?source=collection_archive---------1-----------------------#2023-08-19`](https://towardsdatascience.com/analyzing-geospatial-data-with-python-7244c1b9e302?source=collection_archive---------1-----------------------#2023-08-19)

## 一篇带有 Python 代码的实用数据分析文章。

[](https://gustavorsantos.medium.com/?source=post_page-----7244c1b9e302--------------------------------)![Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----7244c1b9e302--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7244c1b9e302--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7244c1b9e302--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----7244c1b9e302--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4429d99b1245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyzing-geospatial-data-with-python-7244c1b9e302&user=Gustavo+Santos&userId=4429d99b1245&source=post_page-4429d99b1245----7244c1b9e302---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7244c1b9e302--------------------------------) ·8 分钟阅读·2023 年 8 月 19 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7244c1b9e302&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyzing-geospatial-data-with-python-7244c1b9e302&user=Gustavo+Santos&userId=4429d99b1245&source=-----7244c1b9e302---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7244c1b9e302&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyzing-geospatial-data-with-python-7244c1b9e302&source=-----7244c1b9e302---------------------bookmark_footer-----------)

# 介绍

地理空间数据科学是我感兴趣的领域之一。我觉得它非常有趣，因为我们可以在地图上可视化数据，并且——很多时候——数据点之间的关系可以快速提供重要见解。

我相信数据科学的这一子领域在任何业务中都是非常有用的，比如杂货店、汽车租赁、物流、房地产*等*。在这篇文章中，我们将讨论一个来自*AirBnb*的关于美国北卡罗来纳州阿什维尔市的数据集。

旁注：在那座城市中有美国最惊人的地产之一，*我敢说是全世界*。这块地产属于范德堡家族，在很长一段时间里，它是美国最大的私人地产。那么，它绝对值得一次参观，但这不是本文的核心内容。

![](img/bd00be96cce8bdcd4578ea74975fb8ad.png)

位于北卡罗来纳州阿什维尔的 Biltmore 庄园建筑。照片由[Stephanie Klepacki](https://unsplash.com/@sklepacki?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，来自[Unsplash](https://unsplash.com/photos/60IeQ4lmDGs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。

这次练习要使用的数据集是 Asheville 市的 AirBnb 租赁数据。它们可以直接从这个网站[`insideairbnb.com/get-the-data`](http://insideairbnb.com/get-the-data)下载，在[知识共享署名 4.0 国际许可证](http://creativecommons.org/licenses/by/4.0/)下使用。

让我们开始工作。

# 地理空间数据科学
