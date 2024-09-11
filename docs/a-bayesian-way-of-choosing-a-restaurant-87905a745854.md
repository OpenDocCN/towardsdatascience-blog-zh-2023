# 贝叶斯选择餐厅的方法

> 原文：[https://towardsdatascience.com/a-bayesian-way-of-choosing-a-restaurant-87905a745854?source=collection_archive---------5-----------------------#2023-10-12](https://towardsdatascience.com/a-bayesian-way-of-choosing-a-restaurant-87905a745854?source=collection_archive---------5-----------------------#2023-10-12)

[](https://5why.medium.com/?source=post_page-----87905a745854--------------------------------)[![Kirill Tsyganov](../Images/376bcac6eae9741114ff6cc385883d62.png)](https://5why.medium.com/?source=post_page-----87905a745854--------------------------------)[](https://towardsdatascience.com/?source=post_page-----87905a745854--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----87905a745854--------------------------------) [Kirill Tsyganov](https://5why.medium.com/?source=post_page-----87905a745854--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fad57bdbd9754&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-bayesian-way-of-choosing-a-restaurant-87905a745854&user=Kirill+Tsyganov&userId=ad57bdbd9754&source=post_page-ad57bdbd9754----87905a745854---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----87905a745854--------------------------------) ·3分钟阅读·2023年10月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F87905a745854&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-bayesian-way-of-choosing-a-restaurant-87905a745854&user=Kirill+Tsyganov&userId=ad57bdbd9754&source=-----87905a745854---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F87905a745854&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-bayesian-way-of-choosing-a-restaurant-87905a745854&source=-----87905a745854---------------------bookmark_footer-----------)

最近我在寻找一家新的好餐厅。Google Maps 给了我两个选项：餐厅 A 有 10 条评论全是 5 星，而餐厅 B 有 200 条评论，平均评分为 4 星。我倾向于选择餐厅 A，但评论数量少让我感到担忧。另一方面，餐厅 B 的许多评论让我对其 4 星评级有信心，但没有承诺任何卓越。因此，我想比较这些餐厅，并选择最好的，考虑评论或缺少评论的情况。感谢贝叶斯，我们有办法做到这一点。

![](../Images/e306eaf4ab6d73bf22c1c73a73023f0b.png)

图片由作者制作。

贝叶斯框架允许我们对评分的初始分布做出假设，然后根据观察到的数据更新最初的信念。

## **设定初始信念 / 先验**

+   起初，我们对每个评分（从 1 到 5 星）的概率一无所知。因此，在任何评论之前，所有评分的可能性是相等的。这意味着我们从均匀分布开始，这可以表示为**Dirichlet** 分布（Beta 分布的推广）。

+   我们的平均评分将是 (1+2+3+4+5)/5 = 3，这是概率集中最多的地方。

```py
# prior prob. estimates sampling from uniform
sample_size = 10000
p_a = np.random.dirichlet(np.ones(5), size=sample_size)
p_b = np.random.dirichlet(np.ones(5), size=sample_size)

# prior ratings' means based on sampled probs
ratings_support = np.array([1, 2, 3, 4, 5])
prior_reviews_mean_a = np.dot(p_a, ratings_support)
prior_reviews_mean_b = np.dot(p_b, ratings_support)
```

![](../Images/29d7c20cc46584189adb11920e1c115f.png)

图片由作者制作。

## 更新信念

+   要更新初始信念，我们需要将**先验信念**乘以**观察数据的似然性**，再乘以先验信念。

+   观察到的数据自然地用**Multinomial** 分布（Binomial 的推广）来描述。

+   事实证明，Dirichlet 是 Multinomial 似然的**共轭先验**。换句话说，我们的**后验分布也是 Dirichlet** 分布，参数包含了观测数据。

![](../Images/3efe17e0895746f8924ac4546b80868f.png)

图片由作者制作。

```py
# observed data
reviews_a = np.array([0, 0, 0, 0, 10])
reviews_b= np.array([21, 5, 10, 79, 85])

# posterior estimates of ratings probabilities based on observed
sample_size = 10000
p_a = np.random.dirichlet(reviews_a+1, size=sample_size)
p_b = np.random.dirichlet(reviews_b+1, size=sample_size)

# calculate posterior ratings' means
posterior_reviews_mean_a = np.dot(p_a, ratings_support)
posterior_reviews_mean_b = np.dot(p_b, ratings_support)
```

+   现在，A 的后验平均评分介于**先验 3 和观察到的 5 之间**。但 B 的平均评分变化不大，因为大量的评论超出了初始信念的影响。

![](../Images/54add7a0439835424879234e30891ed8.png)

图片由作者制作。

## 那么，哪个更好呢？

+   回到我们最初的问题，“更好”意味着**A 的平均评分大于 B 的平均评分**的概率，即 P(E(A|data)>E(B|data))。

+   在我的情况下，我得到了85%的概率，即餐厅 A 比餐厅 B 更好。

```py
# P(E(A)-E(B)>0)
posterior_rating_diff = posterior_reviews_mean_a-posterior_reviews_mean_b
p_posterior_better = sum(posterior_rating_diff>0)/len(posterior_rating_diff)
```

![](../Images/a1bb98cbb307aad650137ec4267dd2d1.png)

图片由作者制作。

贝叶斯更新允许我们结合先验信念，这在评论数量较少的情况下特别有价值。然而，当评论数量很大时，初始信念对后验信念的影响不大。

代码可在[我的 GitHub](https://github.com/uselessskills/back-of-the-envelope/blob/eb487cfd1a577518e24af478081af68f95c6db30/rating_uncertainty.ipynb)上找到，我打算去餐厅 A。
