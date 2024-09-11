# 使用 Google AI 的 TCAV 解释可解释的人工智能

> 原文：[`towardsdatascience.com/explainable-ai-with-tcav-from-google-ai-5408adf905e?source=collection_archive---------7-----------------------#2023-02-18`](https://towardsdatascience.com/explainable-ai-with-tcav-from-google-ai-5408adf905e?source=collection_archive---------7-----------------------#2023-02-18)

## 使用基于概念的解释来解释深度神经网络

[](https://adib0073.medium.com/?source=post_page-----5408adf905e--------------------------------)![Aditya Bhattacharya](https://adib0073.medium.com/?source=post_page-----5408adf905e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5408adf905e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5408adf905e--------------------------------) [Aditya Bhattacharya](https://adib0073.medium.com/?source=post_page-----5408adf905e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4bb294d0fe6b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexplainable-ai-with-tcav-from-google-ai-5408adf905e&user=Aditya+Bhattacharya&userId=4bb294d0fe6b&source=post_page-4bb294d0fe6b----5408adf905e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5408adf905e--------------------------------) · 13 分钟阅读 · 2023 年 2 月 18 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5408adf905e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexplainable-ai-with-tcav-from-google-ai-5408adf905e&user=Aditya+Bhattacharya&userId=4bb294d0fe6b&source=-----5408adf905e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5408adf905e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexplainable-ai-with-tcav-from-google-ai-5408adf905e&source=-----5408adf905e---------------------bookmark_footer-----------)![](img/08876ebee32ea4c27b44a794161d9bab.png)

图片来源：[Pixabay](https://pixabay.com/illustrations/tick-tock-tiktok-network-computer-7730760/)

[**可解释的人工智能 (XAI)**](https://amzn.to/3cY4c2h) 是人工智能（AI）的一个子领域，旨在开发能够向人类提供清晰且易于理解的决策过程解释的 AI 系统。XAI 的目标是使 AI 变得更加透明、可信、负责任和伦理。在医疗保健、金融和执法等高风险领域，XAI 是提高 AI 采纳率的重要因素。在这些领域，了解 AI 系统如何得出特定决策或建议至关重要。

XAI 中使用了各种技术，包括模型透明性、基于规则的系统以及模型无关的方法，如 LIME 和 SHAP。XAI 的方法可以根据 AI 系统的类型、应用领域和所需的解释性水平而有所不同。总体而言，XAI 是开发可被信任并在实际应用中以道德和有效方式使用的 AI 系统的一个重要领域。

如果你想在短短 45 分钟的视频中获得对 XAI 的简要介绍，你可以观看我在**AI Accelerator Festival APAC, 2021**上关于 XAI 的其中一个过去的演讲：
