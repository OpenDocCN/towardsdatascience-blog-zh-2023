# 解码 NumPy 的点积：对维度魔法的简要探索

> 原文：[`towardsdatascience.com/decoding-numpys-dot-product-a-brief-exploration-of-dimensional-wizardry-63d80f21a315`](https://towardsdatascience.com/decoding-numpys-dot-product-a-brief-exploration-of-dimensional-wizardry-63d80f21a315)

## 一劳永逸地澄清对 NumPy 点积的困惑

[](https://mnslarcher.medium.com/?source=post_page-----63d80f21a315--------------------------------)![Mario Larcher](https://mnslarcher.medium.com/?source=post_page-----63d80f21a315--------------------------------)[](https://towardsdatascience.com/?source=post_page-----63d80f21a315--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----63d80f21a315--------------------------------) [Mario Larcher](https://mnslarcher.medium.com/?source=post_page-----63d80f21a315--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----63d80f21a315--------------------------------) ·5 分钟阅读·2023 年 7 月 24 日

--

![](img/3a4c3b065c8db38640141fa8b5164cca.png)

图片生成自 [DreamStudio](https://beta.dreamstudio.ai/generate)，提示词为“一片混乱、黑暗、阴沉、充满代码巫师的多维世界”。

# 介绍

我是唯一一个在处理 NumPy 中的维度时经常感到困惑的人吗？今天，在阅读 [Gradio 的文档页面](https://www.gradio.app/guides/quickstart#an-image-example)时，我遇到了以下代码片段：

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

嘿，嘿，嘿！为什么一个形状为 (W, H, 3) 的图像与一个形状为 (3, 3) 的滤镜的点积是合法的？我问了 ChatGPT，但它开始给我错误的答案（例如说这行不通）或忽略了我的问题，开始回答其他内容。因此，唯一的解决办法就是动脑筋（再加上阅读文档，唉）。

如果你对上面的代码也感到有些困惑，请继续阅读。

# 点积：一个通用示例

来自 [NumPy 点积文档](https://numpy.org/doc/stable/reference/generated/numpy.dot.html)（略有修改）：

> 如果 a.shape = (I, J, C) 且 b.shape = (K, C, L)，那么 dot(a, b)[i, j, k, l] = sum(a[i, j, :] * b[k, :, l])。请注意，“a”的最后一个维度等于“b”的倒数第二个维度。

或者，以代码的形式：

```py
I, J, K, L, C = 10, 20, 30, 40, 50
a = np.random.random((I, J, C))
b = np.random.random((K, C, L))
c = a.dot(b)
i, j, k, l = 3, 2, 4, 5
print(c[i, j, k, l])
print(sum(a[i, j, :] * b[k, :, l]))
```

输出（相同的结果）：

```py
13.125012901284713
13.125012901284713
```

# 理解 NumPy 点积形状

要提前确定点积的形状，请按照以下步骤操作：

**步骤 1**：考虑两个数组，“a”和“b”，以及它们各自的形状。

```py
# Example shapes for arrays a and b
a_shape = (4, 3, 2)
b_shape = (3, 2, 5)
# Create random arrays with the specified shapes
a = np.random.random(a_shape)
b = np.random.random(b_shape)
```

在这个例子中，数组“a”的形状是 (4, 3, 2)，而数组“b”的形状是 (3, 2, 5)。请注意，再次强调，“a”的最后一个维度和“b”的倒数第二个维度必须匹配。

**步骤 2**：取“a”的所有维度，除了最后一个维度，和“b”的所有维度，除了倒数第二个维度。

对于数组 “a”，我们排除最后一个维度（即 2），结果的形状为 (4, 3)。对于数组 “b”，我们排除倒数第二个维度（也是 2），结果的形状为 (3, 5)。

**步骤 3**：连接步骤 2 中获得的形状。

通过使用我们的规则连接形状，我们得到 (4, 3, 3, 5)。让我们验证一下这个结果是否正确：

```py
c = a.dot(b)
print(c.shape)
```

输出：

```py
(4, 3, 3, 5)
```

正如我们所看到的，点积的结果形状与我们计算的形状 (4, 3, 3, 5) 一致。因此，我们对点积形状的理解是正确的！

# 使用 sepia 滤镜澄清 RGB 像素的点积

让我们回到原始的例子，使用一个图像 (H, W, C) 和一个滤波器 (O, C)，在这个例子中是 (3, 3)。

记住，在原始示例中，点积是与 sepia_filter.T 进行的，后者的形状是 (C, O)。在这种情况下，C = O = 3，但如果它们不同，这将是重要的。

我需要从图像的维度中取出所有维度，除了最后一个，这里是 H 和 W，从滤波器的维度中取出所有维度，除了倒数第二个，这里是 O。因此，结果的维度是 (H, W, O)，在我们的例子中是 (H, W, 3)，仍然是“类似 RGB”的。

使用 NumPy 文档的表示法：

```py
sepia_filter_T = sepia_filter.T
dot(input_img, sepia_filter_T)[h, w, c] = sum(input_img[h, w, :] * sepia_filter_T[:, c])
```

请注意，这与（移除 sepia_filter 的转置）是一样的：

```py
dot(input_img, sepia_filter)[h, w, c] = sum(input_img[h, w, :] * sepia_filter[c, :])
```

但直观地说，如何计算新图像中的每个 RGB 像素？基本上，每个新像素的每个通道值（假设位置为 4, 2 的 R，即“红色”）是旧 RGB 像素值的线性组合，其中这一线性组合的权重是 sepia_filter 中相应行的值（R 的行索引为 0，G 为 1，B 为 2）。

**附加信息**：你还可以使用 [einsum](https://numpy.org/doc/stable/reference/generated/numpy.einsum.html) 来实现这一点！（更多的困惑哈哈，我知道，NumPy 确实很复杂）：

```py
sepia_img = np.einsum("HWC, OC -> HWO", input_img, sepia_filter)
sepia_img /= sepia_img.max()
```

```py
plt.imshow(sepia_img)
plt.axis("off")
```

输出：

![](img/7560752a9c5a0f8058c0160cd599ff12.png)

我的头像“怀旧色调”

尝试一下，并试图理解它是如何工作的作为练习。

# 结论

恭喜你！你已经成功地深入了解了 NumPy 的点积运算，并揭示了它的奥秘。通过遵循形状连接的简单规则，你现在可以轻松确定任何一对数组的点积结果形状。

理解维度如何相互作用，使你能够在各种图像操作中有效地使用点积。例如，我们探索了使用 sepia 滤镜对图像进行变换，通过 RGB 值的线性组合创造了美丽的效果。

现在掌握了这些知识，你可以自信地探索 NumPy 点积在数值计算和图像处理任务中的广泛应用。因此，勇敢地深入探索，进行实验，让点积发挥它的魔力吧！

感谢你花时间阅读这篇文章，欢迎随时留言或联系我，分享你的想法或提问。要获取我最新文章的更新，你可以关注我在[Medium](https://medium.com/@mnslarcher)、[LinkedIn](https://www.linkedin.com/in/mnslarcher/) 或 [Twitter](https://twitter.com/mnslarcher)上的动态。

[](https://medium.com/@mnslarcher/membership?source=post_page-----63d80f21a315--------------------------------) [## 使用我的推荐链接加入 Medium - Mario Namtao Shianti Larcher

### 作为 Medium 会员，你的部分会员费用将分配给你阅读的作者，你还可以全面访问每一个故事……

medium.com](https://medium.com/@mnslarcher/membership?source=post_page-----63d80f21a315--------------------------------)
