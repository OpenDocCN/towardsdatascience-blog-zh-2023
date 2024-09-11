# 图像分类入门

> 原文：[`towardsdatascience.com/image-classification-for-beginners-8546aa75f331?source=collection_archive---------5-----------------------#2023-10-17`](https://towardsdatascience.com/image-classification-for-beginners-8546aa75f331?source=collection_archive---------5-----------------------#2023-10-17)

## 2014 年的 VGG 和 ResNet 架构

[](https://medium.com/@mina.ghashami?source=post_page-----8546aa75f331--------------------------------)![Mina Ghashami](https://medium.com/@mina.ghashami?source=post_page-----8546aa75f331--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8546aa75f331--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8546aa75f331--------------------------------) [Mina Ghashami](https://medium.com/@mina.ghashami?source=post_page-----8546aa75f331--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc99ed9ed7b9a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimage-classification-for-beginners-8546aa75f331&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=post_page-c99ed9ed7b9a----8546aa75f331---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8546aa75f331--------------------------------) · 10 分钟阅读 · 2023 年 10 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8546aa75f331&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimage-classification-for-beginners-8546aa75f331&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=-----8546aa75f331---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8546aa75f331&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimage-classification-for-beginners-8546aa75f331&source=-----8546aa75f331---------------------bookmark_footer-----------)![](img/f097fdb7f59ef0feba34382bfc8b5311.png)

图片来自 [unsplash](https://unsplash.com/photos/Sg3XwuEpybU) — 作者修改

图像分类是我在[*Interview Kickstart*](https://www.interviewkickstart.com/)教授的第一个主题，旨在帮助专业人士准备进入顶尖科技公司。我在准备一次讲座时写了这篇文章。所以如果你对这个主题不太熟悉，这个直观的解释也许对你有帮助。

在这篇文章中，我们探讨了 VGG 和 ResNet 模型；这两者都是卷积神经网络（CNN）在计算机视觉领域发展的开创性和影响力的工作。VGG[2]是 2014 年由牛津大学的研究小组提出的，ResNet[3]则是 2015 年由微软研究人员提出的。

让我们开始吧。

# 什么是 VGG？

> **VGG** 代表 **视觉几何组**，这是牛津大学的一个研究小组。2014 年，他们设计了一个用于图像分类任务的深度卷积神经网络架构，并以他们的名字命名，即 VGG。[2].

## VGG 网络架构

该网络有几种配置，所有配置的架构相同，只是层数不同。最著名的配置是 VGG16 和 VGG19。VGG19 更深且性能优于 VGG16。为了简单起见，我们将重点关注 VGG16。
