# 使用 ChatGPT 编写生成图像的代码

> 原文：[https://towardsdatascience.com/image-generation-with-chatgpt-68c98a061bec?source=collection_archive---------2-----------------------#2023-02-10](https://towardsdatascience.com/image-generation-with-chatgpt-68c98a061bec?source=collection_archive---------2-----------------------#2023-02-10)

## 如果图像只是像素值的矩阵，ChatGPT 能否编写代码生成对应于有意义图像的矩阵？

[](https://medium.com/@jashahir?source=post_page-----68c98a061bec--------------------------------)[![Jamshaid Shahir, Ph.D](../Images/3f57e94cb3987f09667326df0cb66b5d.png)](https://medium.com/@jashahir?source=post_page-----68c98a061bec--------------------------------)[](https://towardsdatascience.com/?source=post_page-----68c98a061bec--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----68c98a061bec--------------------------------) [Jamshaid Shahir, Ph.D](https://medium.com/@jashahir?source=post_page-----68c98a061bec--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F56690d914eaf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimage-generation-with-chatgpt-68c98a061bec&user=Jamshaid+Shahir%2C+Ph.D&userId=56690d914eaf&source=post_page-56690d914eaf----68c98a061bec---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----68c98a061bec--------------------------------) · 8分钟阅读 · 2023年2月10日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F68c98a061bec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimage-generation-with-chatgpt-68c98a061bec&source=-----68c98a061bec---------------------bookmark_footer-----------)![](../Images/af469eaaa6a3cbf95c45358f7accd0c1.png)

图片由 [Jonathan Kemper](https://unsplash.com/@jupp?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，来源于 [Unsplash](https://unsplash.com/s/photos/chatgpt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。

像许多人一样，我对 OpenAI 的 ChatGPT 感到着迷，并将其用于各种活动。从像调试代码这样严肃的任务，到像写诗和短篇故事这样的创意应用，探索 ChatGPT 能做什么非常有趣。与此同时，审视它的不足之处也颇具启发性。作为一种语言模型，它无法像 Midjourney AI 或 DALL-E 那样生成直接的图像，因为它并未在大量图像上进行训练，而是基于文本。然而，考虑到从数学角度来看，图像只是 2D（或者在彩色图像的情况下，3D，即 RGB）数组，你可以让它编写代码来生成图像。因此，我让 ChatGPT 生成了与图像对应的数组。

![](../Images/a1e1882ec807c754036b3ca8d7123d83.png)

作者提供的图像

在没有任何修改的情况下，我将这些代码复制并粘贴到 Jupyter notebook 中运行，确实得到了一个白色方块（尽管不完全在中间）：
