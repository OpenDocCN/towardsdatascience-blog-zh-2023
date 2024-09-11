# 推荐系统：基于矩阵分解的协同过滤

> 原文：[`towardsdatascience.com/recommendation-system-with-matrix-factorization-ebc4736869e4?source=collection_archive---------9-----------------------#2023-04-26`](https://towardsdatascience.com/recommendation-system-with-matrix-factorization-ebc4736869e4?source=collection_archive---------9-----------------------#2023-04-26)

## 通过矩阵分解进行推荐解释

[](https://medium.com/@christienatashia?source=post_page-----ebc4736869e4--------------------------------)![Christie Natashia](https://medium.com/@christienatashia?source=post_page-----ebc4736869e4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ebc4736869e4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ebc4736869e4--------------------------------) [Christie Natashia](https://medium.com/@christienatashia?source=post_page-----ebc4736869e4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb867d9015fd1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frecommendation-system-with-matrix-factorization-ebc4736869e4&user=Christie+Natashia&userId=b867d9015fd1&source=post_page-b867d9015fd1----ebc4736869e4---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ebc4736869e4--------------------------------) · 8 min read · Apr 26, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Febc4736869e4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frecommendation-system-with-matrix-factorization-ebc4736869e4&user=Christie+Natashia&userId=b867d9015fd1&source=-----ebc4736869e4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Febc4736869e4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frecommendation-system-with-matrix-factorization-ebc4736869e4&source=-----ebc4736869e4---------------------bookmark_footer-----------)![](img/59da938334bfc2021061170bbcb4c9df.png)

图片由 [freestocks](https://unsplash.com/es/@freestocks?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Netflix 是一个受欢迎的在线流媒体平台，提供广泛的电影、纪录片和电视节目。为了提升用户体验，Netflix 开发了一个复杂的推荐系统，根据您的观看历史、评分和偏好建议电影。

推荐系统使用复杂的算法来分析大量数据，以预测用户最可能喜欢的内容。拥有超过 2 亿全球订阅者的 Netflix 推荐系统是其成功的关键因素，并且为流媒体行业树立了标准。以下是 Netflix 如何通过个性化实现 80%观看时间的来源[链接](https://www.youtube.com/watch?v=f8OK1HBEgn0)。

# 那么，什么是推荐系统？

**推荐**系统是一种无监督学习系统，利用信息过滤技术根据用户的偏好、兴趣和行为来建议产品或内容。这些系统在电子商务和在线流媒体环境中被广泛使用，并且在其他应用中也有助于发现可能感兴趣的新产品和内容。
