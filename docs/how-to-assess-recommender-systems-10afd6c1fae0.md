# 如何评估推荐系统

> 原文：[https://towardsdatascience.com/how-to-assess-recommender-systems-10afd6c1fae0?source=collection_archive---------2-----------------------#2023-04-16](https://towardsdatascience.com/how-to-assess-recommender-systems-10afd6c1fae0?source=collection_archive---------2-----------------------#2023-04-16)

## 对评估指标公式的深入探讨

[](https://medium.com/@amine.dadoun?source=post_page-----10afd6c1fae0--------------------------------)[![Amine Dadoun](../Images/ebde3241f8071b9c6dbcd2e498ab10c0.png)](https://medium.com/@amine.dadoun?source=post_page-----10afd6c1fae0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----10afd6c1fae0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----10afd6c1fae0--------------------------------) [Amine Dadoun](https://medium.com/@amine.dadoun?source=post_page-----10afd6c1fae0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F338cc4ecc244&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-assess-recommender-systems-10afd6c1fae0&user=Amine+Dadoun&userId=338cc4ecc244&source=post_page-338cc4ecc244----10afd6c1fae0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----10afd6c1fae0--------------------------------) ·6 min read·Apr 16, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F10afd6c1fae0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-assess-recommender-systems-10afd6c1fae0&user=Amine+Dadoun&userId=338cc4ecc244&source=-----10afd6c1fae0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F10afd6c1fae0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-assess-recommender-systems-10afd6c1fae0&source=-----10afd6c1fae0---------------------bookmark_footer-----------)![](../Images/ebbfe1bf0b7b2b8574e920a21e72d9d1.png)

[图片来源](https://pixabay.com/images/search/)。该图片在Pixabay的内容许可下可免费使用。

在最近的文章中，我们介绍了一些在工业界和文献中广泛应用的显著推荐系统算法，[通过不同的视角使用图](https://medium.com/@amine.dadoun/addressing-the-recommendation-task-through-a-different-lens-3226998bad58)。此外，我们还阐述了包括[基于知识图谱的推荐系统](https://medium.com/@amine.dadoun/introduction-to-knowledge-graph-based-recommender-systems-34254efd1960)在内的新兴方法，这些方法在推荐研究领域越来越受到关注。

在本文中，我们将深入探讨评估推荐系统的过程。我们将探索评估其性能的关键标准和指标，包括准确性、多样性、覆盖率、新颖性和意外惊喜。

# 准确性指标

近年来，大多数推荐系统领域的研究工作部分通过来自信息检索领域的准确性指标进行评估，例如精确度 (P@K) 和召回率 (R@K)，其公式如下：

![](../Images/19e4d4c77a82174d7961940eaac28d3d.png)![](../Images/c8537fe1e259cc39c2cbe6289d334939.png)

其中 n 和 m 分别表示用户数和项目数，i₁,i₂,…,iₖ 是从 1 到 K 排名的项目，如果推荐的项目 iⱼ 对用户 u 相关，则 hit 的值为 1，否则为 0。Rel(u) 表示测试集中用户 u 的相关项目集合。

在产品推荐的背景下，这些评估指标的目的是识别给定用户的 K 个最相关项目，并衡量检索精确相关信息的质量。在仅向用户推荐一个项目的特殊情况下，如会话式推荐系统中，我们希望测量即时下一个项目的正确性，即 ***hitrate@K***：

![](../Images/85b76d0b324a9197a93763f0a3dfa280.png)

类似于 R@K，hitrate@K 衡量推荐系统的正确性或准确性。

![](../Images/20da25746f1e764aa4a568bb0b72c1d4.png)

推荐系统向用户建议一份按排名排列的电影列表。如果用户决定观看前 K 列表中的一部电影（例如 *Gatsby*），hitrate@K 将等于 1，而 MRR@K 将等于 0.5，因为该电影在列表中排名第二。注意：该图表由作者创建。

文献中广泛使用的其他三个指标，用于评估推荐系统的准确性，特别是捕捉命中在列表中的排名情况：

+   ***均值平均精度*** (MAP@K)：该指标衡量推荐系统提供的相关项目的顺序：

![](../Images/574b3bd0b6be2c99f5498f46c110e6ba.png)

+   ***Top-K 均值倒数排名*** (MRR@K)：该指标是 MAP@K 的特例，其中只有一个相关项目。它衡量推荐系统如何将相关项目与不相关项目进行良好排名：

![](../Images/344a7a17de72024d6e476cca624f8d82.png)

其中 iⱼ 是前 K 个推荐项目中的相关推荐项目。

+   ***归一化折扣累积增益*** (NDCG@K)：MAP@K 和 NDCG@K 都重视排序，但主要区别在于均值平均精度测量的是二元相关性（项目要么感兴趣，要么不感兴趣），而 NDCG@K 允许以实数形式的相关性分数：

![](../Images/9c308d77a3b17279069967fd3a4a375c.png)

在这里，IDCG@K 是理想折扣累积增益，定义如下（Rel(u) 仅包含用户 u 的相关项目）：

![](../Images/3937aeae46494e5639c5f42b2cb3afc7.png)

# 超越准确性

尽管这些指标在评估推荐系统中的相关性，但推荐相同类型的产品有时可能适得其反，并且在实际应用中可能不够充分（如 Netflix、YouTube 等）。例如，在 Netflix 上，用户可能会被新类型的电影和系列吸引；在 YouTube 上，用户通常希望观看新视频。用户必须感到惊喜，一个好的推荐系统应该具有推荐意外且吸引人的项目的能力。业界也支持不完全依赖于基于精度的度量（例如 [Spotify 的每周发现功能](https://open.spotify.com/playlist/37i9dQZEVXcNuIGtTaIMH8)）。在研究中 [1]，作者指出评估协议的目的是评估推荐项目的质量，而不仅仅是其准确性或实用性。在这种情况下，只有在线实验中系统的用户能够评判推荐质量，才能可靠地评估推荐。因此，在离线评估时，必须考虑除单一准确性之外的其他指标。

为了对推荐质量得出可靠结论，推荐系统不仅需要提供准确的推荐，还必须提供***有用的建议***。确实，一个极其流行的项目可能是一个准确的建议，但对用户来说可能并不有趣。意外性、新颖性以及多样性是与准确性指标相对的替代度量标准。

推荐系统中的***意外性***概念指的是系统向用户推荐意外且吸引人的产品的能力。在 [3] 中，提出了一种度量来通过在过滤掉那些过于明显的项目后评估推荐项目的精度来衡量意外性。下面的公式概述了该度量的计算方法。变量 hit_non_pop 类似于 hit，但它将前 k 个最流行的项目视为不相关的，即使它们被包含在用户 u 的测试集中。这是因为流行项目被认为是显而易见的，因为大多数用户都很熟悉它们。

![](../Images/edeeb069d471b678734e36e048ab4a1a.png)

在 [4] 中，提出了一种***新颖性***度量来评估推荐系统建议用户不太可能知道的项目的能力。该度量的目的是帮助推荐系统促进用户发现新项目。下面的公式在 [5] 中定义，概述了这一度量的计算方法。需要注意的是，与之前的指标不同，新颖性指标仅关注推荐项目的新颖性，而不考虑其正确性或相关性。

![](../Images/fd3a57982c890f83df740bfd1392679e.png)

函数 Pₜᵣₐᵢₙ : I → [0, 1] 返回在训练集中分配给项目 i 的反馈比例。这个值表示在训练集中观察到特定项目的概率，即与该项目相关的评分数量除以可用评分的总数。

# 下一步是什么？评估协议

在大多数推荐系统的研究工作中，评估协议是在离线环境中进行的，其中上述指标仅基于过去的互动进行测量。然而，这在现实中被证明是不够的。

在下一个博客文章中，我们将突出不同的评估方法，包括离线评估、在线评估和用户研究，并讨论它们的优缺点。

# 参考文献

[1] Mouzhi Ge, Carla Delgado-Battenfeld, 和 Dietmar Jannach. 超越准确性：通过覆盖度和偶然性评估推荐系统。在第四届ACM推荐系统会议论文集中，RecSys ’10，第257–260页，美国纽约，2010年。计算机协会。

[2] Jonathan L. Herlocker, Joseph A. Konstan, Loren G. Terveen, 和 John T. Riedl. 评估协同过滤推荐系统。22(1):5–53，2004年1月。

[3] Marco de Gemmis, Pasquale Lops, Giovanni Semeraro, 和 Cataldo Musto. 关于推荐系统中偶然性问题的研究。信息处理与管理，51(5):695–717，2015年9月。

[4] Saúl Vargas 和 Pablo Castells. 推荐系统中新颖性和多样性度量中的排名与相关性。在第五届ACM推荐系统会议论文集中，RecSys ’11，第109–116页，美国纽约，2011年。计算机协会。

[5] Enrico Palumbo, Diego Monti, Giuseppe Rizzo, Raphaël Troncy, 和 Elena Baralis. entity2rec: 针对项目推荐的属性特定知识图谱嵌入。专家系统应用，151:113235，2020年。
