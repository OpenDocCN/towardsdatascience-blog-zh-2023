# 解码 NumPy 的点积：维度魔法的简要探索

> 原文：[`towardsdatascience.com/decoding-numpys-dot-product-a-brief-exploration-of-dimensional-wizardry-63d80f21a315?source=collection_archive---------15-----------------------#2023-07-24`](https://towardsdatascience.com/decoding-numpys-dot-product-a-brief-exploration-of-dimensional-wizardry-63d80f21a315?source=collection_archive---------15-----------------------#2023-07-24)

## 一劳永逸地澄清 NumPy 点积的困惑

[](https://mnslarcher.medium.com/?source=post_page-----63d80f21a315--------------------------------)![Mario Larcher](https://mnslarcher.medium.com/?source=post_page-----63d80f21a315--------------------------------)[](https://towardsdatascience.com/?source=post_page-----63d80f21a315--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----63d80f21a315--------------------------------) [Mario Larcher](https://mnslarcher.medium.com/?source=post_page-----63d80f21a315--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcd2b72f39ad4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecoding-numpys-dot-product-a-brief-exploration-of-dimensional-wizardry-63d80f21a315&user=Mario+Larcher&userId=cd2b72f39ad4&source=post_page-cd2b72f39ad4----63d80f21a315---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----63d80f21a315--------------------------------) ·5 分钟阅读·2023 年 7 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F63d80f21a315&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecoding-numpys-dot-product-a-brief-exploration-of-dimensional-wizardry-63d80f21a315&user=Mario+Larcher&userId=cd2b72f39ad4&source=-----63d80f21a315---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F63d80f21a315&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecoding-numpys-dot-product-a-brief-exploration-of-dimensional-wizardry-63d80f21a315&source=-----63d80f21a315---------------------bookmark_footer-----------)![](img/3a4c3b065c8db38640141fa8b5164cca.png)

使用 [DreamStudio](https://beta.dreamstudio.ai/generate) 生成的图像，提示词为“一个混乱、黑暗、阴郁的多维世界，充满了代码魔法师”。

# 介绍

我是唯一一个在处理 NumPy 的维度时偶尔感到困惑的人吗？今天，在阅读 [Gradio 的文档页面](https://www.gradio.app/guides/quickstart#an-image-example)时，我遇到了以下代码片段：

```py
sepia_filter = np.array([
    [0.393, 0.769, 0.189], 
    [0.349, 0.686, 0.168],
    [0.272, 0.534, 0.131],
])
# input_img shape (H, W, 3)
# sepia_filter shape (3, 3)
sepia_img = input_img.dot(sepia_filter.T)  # <- why this is legal??
sepia_img /= sepia_img.max()
```

嘿，嘿，嘿！为什么一个图像（W, H, 3）与一个滤波器（3, 3）的点积是合法的？我问了 ChatGPT 来解释，但它开始给我错误的答案（比如说这不行）或者忽略我的问题，开始回答其他问题。因此，别无选择，只能用脑子（加上阅读文档，唉）。

如果你对上面的代码还有点困惑，请继续阅读。

# 点积：一个通用示例

来自[NumPy 点积文档](https://numpy.org/doc/stable/reference/generated/numpy.dot.html)（有少许修改）：

> 如果`a.shape = (I, J, C)`且…
