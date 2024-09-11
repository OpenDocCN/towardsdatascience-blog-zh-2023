# 多模态思维链：在多模态世界中解决问题

> 原文：[`towardsdatascience.com/multimodal-chain-of-thoughts-solving-problems-in-a-multimodal-world-961a8ab9d0fa?source=collection_archive---------8-----------------------#2023-03-13`](https://towardsdatascience.com/multimodal-chain-of-thoughts-solving-problems-in-a-multimodal-world-961a8ab9d0fa?source=collection_archive---------8-----------------------#2023-03-13)

## 自然语言处理 | 多模态性 | 思维链 |

## 世界不仅仅是文字：如何将思维链扩展到图像和文字？

[](https://salvatore-raieli.medium.com/?source=post_page-----961a8ab9d0fa--------------------------------)![Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----961a8ab9d0fa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----961a8ab9d0fa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----961a8ab9d0fa--------------------------------) [Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----961a8ab9d0fa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff1a08d9452cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultimodal-chain-of-thoughts-solving-problems-in-a-multimodal-world-961a8ab9d0fa&user=Salvatore+Raieli&userId=f1a08d9452cd&source=post_page-f1a08d9452cd----961a8ab9d0fa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----961a8ab9d0fa--------------------------------) ·14 分钟阅读·2023 年 3 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F961a8ab9d0fa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultimodal-chain-of-thoughts-solving-problems-in-a-multimodal-world-961a8ab9d0fa&user=Salvatore+Raieli&userId=f1a08d9452cd&source=-----961a8ab9d0fa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F961a8ab9d0fa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultimodal-chain-of-thoughts-solving-problems-in-a-multimodal-world-961a8ab9d0fa&source=-----961a8ab9d0fa---------------------bookmark_footer-----------)![](img/868bf8d7fafd276e2175a475fa6cc822.png)

图片由 [Giulio Magnifico](https://unsplash.com/it/@giuliomagnifico) 提供，来源于 Unsplash

有时候，找到答案并不容易，特别是当问题需要推理时。一个模型并不总是能在其参数中隐藏答案，但可以通过正确的上下文和方法找到答案。思维链是什么？为什么这种方法使解决多步骤推理任务成为可能？它能扩展到多模态问题（即包含图像和文本的问题）吗？只有大型模型才能做到这一点吗？

**本文讨论了如何回答这些问题。**

# 思维链（CoT）：它是什么？

![](img/118e0e262bad7b95096ebc2f40b9c948.png)

照片由[Todd Cravens](https://unsplash.com/it/@toddcravens)拍摄，来源于[Unsplash](https://unsplash.com/)

近年来，我们看到模型参数的数量增长（超过 100 亿个参数）。这是由扩展法则推动的：随着参数数量的增加，误差减少。
