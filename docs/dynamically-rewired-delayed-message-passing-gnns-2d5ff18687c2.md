# 动态重连的延迟消息传递 GNNs

> 原文：[`towardsdatascience.com/dynamically-rewired-delayed-message-passing-gnns-2d5ff18687c2?source=collection_archive---------7-----------------------#2023-06-19`](https://towardsdatascience.com/dynamically-rewired-delayed-message-passing-gnns-2d5ff18687c2?source=collection_archive---------7-----------------------#2023-06-19)

## 延迟消息传递

## 消息传递图神经网络（MPNNs）往往会遭遇*过度压缩*的现象，这会导致依赖于长距离交互的任务性能下降。这主要归因于消息传递仅在*局部*范围内发生，即仅在节点的直接邻居之间。传统的静态图重连技术通常试图通过允许远程节点即时通信来对抗这一效果（在极端情况下，如 Transformers，通过使所有节点在每一层都可访问）。然而，这会带来计算成本，并且破坏了输入图结构所提供的归纳偏置。在这篇文章中，我们描述了两种新机制来克服过度压缩，同时抵消静态重连方法的副作用：*动态重连*和*延迟*消息传递。这些技术可以被纳入任何 MPNN 中，并带来更好的……

[](https://michael-bronstein.medium.com/?source=post_page-----2d5ff18687c2--------------------------------)![Michael Bronstein](https://michael-bronstein.medium.com/?source=post_page-----2d5ff18687c2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2d5ff18687c2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2d5ff18687c2--------------------------------) [Michael Bronstein](https://michael-bronstein.medium.com/?source=post_page-----2d5ff18687c2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7b1129ddd572&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamically-rewired-delayed-message-passing-gnns-2d5ff18687c2&user=Michael+Bronstein&userId=7b1129ddd572&source=post_page-7b1129ddd572----2d5ff18687c2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2d5ff18687c2--------------------------------) ·9 min read·Jun 19, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2d5ff18687c2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamically-rewired-delayed-message-passing-gnns-2d5ff18687c2&user=Michael+Bronstein&userId=7b1129ddd572&source=-----2d5ff18687c2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2d5ff18687c2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamically-rewired-delayed-message-passing-gnns-2d5ff18687c2&source=-----2d5ff18687c2---------------------bookmark_footer-----------)
