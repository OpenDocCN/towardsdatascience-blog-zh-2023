# 使用人工智能、OpenAI 和探索性数据分析来探讨情感

> 原文：[`towardsdatascience.com/exploring-emotions-with-artificial-intelligence-openai-and-exploratory-data-analysis-36a3882d3f11?source=collection_archive---------11-----------------------#2023-12-12`](https://towardsdatascience.com/exploring-emotions-with-artificial-intelligence-openai-and-exploratory-data-analysis-36a3882d3f11?source=collection_archive---------11-----------------------#2023-12-12)

## 下面是如何使用 Python 和 OpenAI 及探索性数据分析来可视化文本中的情感

[](https://piero-paialunga.medium.com/?source=post_page-----36a3882d3f11--------------------------------)![Piero Paialunga](https://piero-paialunga.medium.com/?source=post_page-----36a3882d3f11--------------------------------)[](https://towardsdatascience.com/?source=post_page-----36a3882d3f11--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----36a3882d3f11--------------------------------) [Piero Paialunga](https://piero-paialunga.medium.com/?source=post_page-----36a3882d3f11--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F254e653181d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-emotions-with-artificial-intelligence-openai-and-exploratory-data-analysis-36a3882d3f11&user=Piero+Paialunga&userId=254e653181d2&source=post_page-254e653181d2----36a3882d3f11---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----36a3882d3f11--------------------------------) · 9 分钟阅读 · 2023 年 12 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F36a3882d3f11&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-emotions-with-artificial-intelligence-openai-and-exploratory-data-analysis-36a3882d3f11&user=Piero+Paialunga&userId=254e653181d2&source=-----36a3882d3f11---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F36a3882d3f11&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-emotions-with-artificial-intelligence-openai-and-exploratory-data-analysis-36a3882d3f11&source=-----36a3882d3f11---------------------bookmark_footer-----------)![](img/417d5e40aed1306a713601d76c61dcc4.png)

作者使用 [Midjourney](https://www.midjourney.com/home?callbackUrl=%2Fexplore) 制作的图像

我想先说的是，我比起新的迪士尼电影，更喜欢旧版的。

我认为这与我小时候看老版迪士尼电影的经历有关，那时我对那个时刻充满了怀旧感。即使我不是电影专家，我也觉得老版迪士尼电影的情节是最精彩的。

不过有一个显著的例外，那就是[**头脑特工队**](https://movies.disney.com/inside-out)。我在电影院看了那部电影，对它爱不释手。我不想剧透任何内容，只想说这部电影探讨了我们内心有一系列情感的想法。

+   **愤怒**

+   **厌恶**

+   **快乐**

+   **恐惧**

+   **悲伤**

这些情感有时会像真实的人一样在我们内心对话。这是一部极其甜美的电影，我认为情节非常精彩。当我听说新片《头脑特工队 2》即将上映时，我感到非常兴奋，并在数着日子。:)
