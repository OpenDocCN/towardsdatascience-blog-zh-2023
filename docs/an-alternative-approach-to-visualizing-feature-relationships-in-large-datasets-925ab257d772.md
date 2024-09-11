# 大型数据集中可视化特征关系的替代方法

> 原文：[`towardsdatascience.com/an-alternative-approach-to-visualizing-feature-relationships-in-large-datasets-925ab257d772?source=collection_archive---------5-----------------------#2023-09-28`](https://towardsdatascience.com/an-alternative-approach-to-visualizing-feature-relationships-in-large-datasets-925ab257d772?source=collection_archive---------5-----------------------#2023-09-28)

## 如何让那些拥挤的散点图更具信息量

[](https://medium.com/@zvonimir.boban.mef?source=post_page-----925ab257d772--------------------------------)![Zvonimir Boban](https://medium.com/@zvonimir.boban.mef?source=post_page-----925ab257d772--------------------------------)[](https://towardsdatascience.com/?source=post_page-----925ab257d772--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----925ab257d772--------------------------------) [Zvonimir Boban](https://medium.com/@zvonimir.boban.mef?source=post_page-----925ab257d772--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe31978768a4e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-alternative-approach-to-visualizing-feature-relationships-in-large-datasets-925ab257d772&user=Zvonimir+Boban&userId=e31978768a4e&source=post_page-e31978768a4e----925ab257d772---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----925ab257d772--------------------------------) ·5 分钟阅读·2023 年 9 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F925ab257d772&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-alternative-approach-to-visualizing-feature-relationships-in-large-datasets-925ab257d772&user=Zvonimir+Boban&userId=e31978768a4e&source=-----925ab257d772---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F925ab257d772&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-alternative-approach-to-visualizing-feature-relationships-in-large-datasets-925ab257d772&source=-----925ab257d772---------------------bookmark_footer-----------)![](img/fdaba01469448b7502bd62cc1324c477.png)

图片来源：作者

理解数据特征之间关系的最简单方法是通过可视化它们。对于数值特征，这通常意味着制作散点图。

如果点的数量很少，这样的方式是可以的，但对于大型数据集，观察值重叠的问题会出现。对于中型数据集，可以通过使点半透明来部分缓解这个问题，但对于非常大的数据集，这种方法甚至无济于事。

那接下来该怎么办呢？我将向你展示一种使用公开的[来自 Kaggle 的 Spotify 数据集](https://www.kaggle.com/datasets/maharshipandya/-spotify-tracks-dataset)的替代方法。

该数据集包含了 114000 首 Spotify 曲目的音频特征，如舞蹈感、节奏、时长、语音特征等。作为本帖的示例，我将考察舞蹈感与其他所有特征之间的关系。

首先导入数据集并稍微整理一下。

```py
#load the required packages
library(dplyr)
library(tidyr)
library(ggplot2)
library(ggthemes)
library(readr)
library(stringr)

#load and tidy the data
spotify <- readr::read_csv('spotify_songs.csv') %>% 
  select(-1) %>%
  mutate(duration_min = duration_ms/60000, 
         track_genre…
```
