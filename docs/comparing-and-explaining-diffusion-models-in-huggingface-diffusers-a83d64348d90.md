# 比较和解释 HuggingFace 扩散器中的扩散模型

> 原文：[`towardsdatascience.com/comparing-and-explaining-diffusion-models-in-huggingface-diffusers-a83d64348d90?source=collection_archive---------2-----------------------#2023-08-24`](https://towardsdatascience.com/comparing-and-explaining-diffusion-models-in-huggingface-diffusers-a83d64348d90?source=collection_archive---------2-----------------------#2023-08-24)

## DDPM, Stable Diffusion, DALL·E-2, Imagen, Kandinsky 2, SDEdit, ControlNet, InstructPix2Pix 和更多

[](https://mnslarcher.medium.com/?source=post_page-----a83d64348d90--------------------------------)![Mario Larcher](https://mnslarcher.medium.com/?source=post_page-----a83d64348d90--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a83d64348d90--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a83d64348d90--------------------------------) [Mario Larcher](https://mnslarcher.medium.com/?source=post_page-----a83d64348d90--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcd2b72f39ad4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomparing-and-explaining-diffusion-models-in-huggingface-diffusers-a83d64348d90&user=Mario+Larcher&userId=cd2b72f39ad4&source=post_page-cd2b72f39ad4----a83d64348d90---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----a83d64348d90--------------------------------) ·33 min read·Aug 24, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa83d64348d90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomparing-and-explaining-diffusion-models-in-huggingface-diffusers-a83d64348d90&user=Mario+Larcher&userId=cd2b72f39ad4&source=-----a83d64348d90---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa83d64348d90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomparing-and-explaining-diffusion-models-in-huggingface-diffusers-a83d64348d90&source=-----a83d64348d90---------------------bookmark_footer-----------)![](img/62e4b9c064053a36fd3c580645994bbc.png)

使用扩散器生成的图像。继续阅读了解操作方法及其背后的理论。

# 目录

+   介绍

+   先决条件和建议材料

+   扩散器流水线

+   流水线：DDPM（扩散模型）

+   流水线：稳定扩散文本到图像

+   流水线：稳定扩散图像到图像（SDEdit）

+   流水线：稳定扩散图像变异

+   流水线：稳定扩散增强

+   流水线：稳定扩散潜在增强

+   流程图：unCLIP (Karlo/DALL·E-2)

+   流程图：DeepFloyd IF (Imagen)

+   流程图：Kandinsky

+   流程图：ControlNet

+   流程图：Instruct Pix2Pix

+   附录 — CLIP

+   附录 — VQGAN

+   附录 — Prompt-to-Prompt

+   结论

+   致谢

# 引言
