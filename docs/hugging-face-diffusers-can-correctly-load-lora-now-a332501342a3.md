# Hugging Face Diffusers 可以正确加载 LoRA

> 原文：[`towardsdatascience.com/hugging-face-diffusers-can-correctly-load-lora-now-a332501342a3?source=collection_archive---------5-----------------------#2023-07-28`](https://towardsdatascience.com/hugging-face-diffusers-can-correctly-load-lora-now-a332501342a3?source=collection_archive---------5-----------------------#2023-07-28)

## 使用最新的 Diffusers Monkey Patching 函数加载 LoRA 会产生与 A1111 完全相同的结果

[](https://xhinker.medium.com/?source=post_page-----a332501342a3--------------------------------)![Andrew Zhu (Shudong Zhu)](https://xhinker.medium.com/?source=post_page-----a332501342a3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a332501342a3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a332501342a3--------------------------------) [Andrew Zhu (Shudong Zhu)](https://xhinker.medium.com/?source=post_page-----a332501342a3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbf0160c6bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhugging-face-diffusers-can-correctly-load-lora-now-a332501342a3&user=Andrew+Zhu+%28Shudong+Zhu%29&userId=bf0160c6bb&source=post_page-bf0160c6bb----a332501342a3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a332501342a3--------------------------------) ·5 分钟阅读·2023 年 7 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa332501342a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhugging-face-diffusers-can-correctly-load-lora-now-a332501342a3&user=Andrew+Zhu+%28Shudong+Zhu%29&userId=bf0160c6bb&source=-----a332501342a3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa332501342a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhugging-face-diffusers-can-correctly-load-lora-now-a332501342a3&source=-----a332501342a3---------------------bookmark_footer-----------)

从 Hugging Face 的 Diffusers 代码库中拉取最新代码，发现与 LoRA 加载相关的最新代码已更新，并且现在可以进行 Monkey-Patching LoRA 加载。

要安装最新的 Diffusers：

```py
pip install -U git+https://github.com/huggingface/diffusers.git@main
```

根据我的测试，LoRA 加载功能昨天生成了略微错误的结果。本文讨论了如何使用最新的 Diffusers 包中的 LoRA 加载器。

## 加载 LoRA 并更新稳定扩散模型权重

自从使用 Diffusers 的程序员无法轻松加载 LoRA 已经有一段时间了。要将 LoRA 加载到检查点模型中，并输出与 A1111 的 Stable Diffusion Webui 相同的结果，我们需要使用额外的自定义代码来加载权重，如本文所述。

[](/improving-diffusers-package-for-high-quality-image-generation-a50fff04bdd4?source=post_page-----a332501342a3--------------------------------) ## 改进 Diffusers 包以实现高质量图像生成

### 克服令牌大小限制、自定义模型加载、LoRa 支持、文本反演支持等问题

[towardsdatascience.com

本文提供的解决方案运行良好且速度快，但需要额外的…
