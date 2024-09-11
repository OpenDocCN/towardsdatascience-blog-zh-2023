# 拆解 DINOv2，Meta 的新型全能计算机视觉骨干网

> 原文：[`towardsdatascience.com/unboxing-dinov2-metas-new-all-purpose-computer-vision-backbone-d8e22c059040?source=collection_archive---------2-----------------------#2023-05-07`](https://towardsdatascience.com/unboxing-dinov2-metas-new-all-purpose-computer-vision-backbone-d8e22c059040?source=collection_archive---------2-----------------------#2023-05-07)

## 人工智能

## 视觉基础模型是否正在追赶大型语言模型？

[](https://michaloleszak.medium.com/?source=post_page-----d8e22c059040--------------------------------)![Michał Oleszak](https://michaloleszak.medium.com/?source=post_page-----d8e22c059040--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d8e22c059040--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d8e22c059040--------------------------------) [Michał Oleszak](https://michaloleszak.medium.com/?source=post_page-----d8e22c059040--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc58320fab2a8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funboxing-dinov2-metas-new-all-purpose-computer-vision-backbone-d8e22c059040&user=Micha%C5%82+Oleszak&userId=c58320fab2a8&source=post_page-c58320fab2a8----d8e22c059040---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d8e22c059040--------------------------------) · 8 分钟阅读 · 2023 年 5 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd8e22c059040&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funboxing-dinov2-metas-new-all-purpose-computer-vision-backbone-d8e22c059040&user=Micha%C5%82+Oleszak&userId=c58320fab2a8&source=-----d8e22c059040---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd8e22c059040&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funboxing-dinov2-metas-new-all-purpose-computer-vision-backbone-d8e22c059040&source=-----d8e22c059040---------------------bookmark_footer-----------)![](img/a82bce14167223d4394f5857aa29d74f.png)

自监督训练方法不断取得突破。上周，Meta AI 发布了第二版的自我蒸馏模型，无需标签或 DINO 模型。该模型据说可以作为解决几乎任何计算机视觉任务的主干骨架，而无需微调！计算机视觉中的基础模型是否已经赶上了大型语言模型所持有的多功能性水平？让我们来探索一下 DINO 的能力吧！

*如果你主要对玩新的 DINO 感兴趣，可以随意滚动到“测试 DINOv2”部分。在此之前，我们将更详细地查看模型的架构和训练过程。*

# 🦖 计算机视觉中的自监督学习

自监督学习在计算机视觉应用中已经越来越受欢迎，这并不令人惊讶：没有标注示例的训练模型的可能性允许使用更大范围的训练数据，在一些标签难以获取或成本高昂的应用中，它甚至可能使得之前无法进行的训练成为可能……
