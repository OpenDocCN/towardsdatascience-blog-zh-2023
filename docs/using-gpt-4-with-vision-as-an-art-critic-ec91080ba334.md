# 使用 GPT-4 与视觉作为艺术评论家

> 原文：[https://towardsdatascience.com/using-gpt-4-with-vision-as-an-art-critic-ec91080ba334?source=collection_archive---------7-----------------------#2023-11-02](https://towardsdatascience.com/using-gpt-4-with-vision-as-an-art-critic-ec91080ba334?source=collection_archive---------7-----------------------#2023-11-02)

## OpenAI 最新模型如何提供对视觉艺术的见解

[](https://robgon.medium.com/?source=post_page-----ec91080ba334--------------------------------)[![Robert A. Gonsalves](../Images/96b4da0f602a1cd9d1e1d2917868cbee.png)](https://robgon.medium.com/?source=post_page-----ec91080ba334--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ec91080ba334--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ec91080ba334--------------------------------) [Robert A. Gonsalves](https://robgon.medium.com/?source=post_page-----ec91080ba334--------------------------------)

·

[阅读更多](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc97e6c73c13c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-gpt-4-with-vision-as-an-art-critic-ec91080ba334&user=Robert+A.+Gonsalves&userId=c97e6c73c13c&source=post_page-c97e6c73c13c----ec91080ba334---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----ec91080ba334--------------------------------) ·14 分钟阅读·2023年11月2日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fec91080ba334&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-gpt-4-with-vision-as-an-art-critic-ec91080ba334&source=-----ec91080ba334---------------------bookmark_footer-----------)![](../Images/e59bc437e9a44ecd8cf8a02dfd1b3920.png)

**一个 AI 艺术评论家的极简主义画作**，*图像由 AI 图像生成程序 Midjourney 创建，并由作者编辑*

大约两周前，OpenAI 开始推出其 GPT-4 视觉模型（GPT-4V）的访问权限 [1]。作为 ChatGPT Plus 的订阅者，每月费用为 20 美元，我获得了新模型的早期访问权限。系统可在这里访问：[chat.openai.com](https://chat.openai.com/).

使用 GPT-4V，我可以提交一个包含最多四张图片的文本提示。系统将分析提示和图片，并作出相应的回应。由于我一直在撰写关于 AI 和机器学习的创意应用的文章，我的第一反应是查看 GPT-4V 是否能够有效地充当艺术评论家。

这是我的第一次实验。我让系统审阅了一幅我在2002年创作的新英格兰风景丙烯画，这幅画是借助参考图像、一些自定义软件和投影仪完成的。我拍了我的画作的照片，并将其发送给了GPT-4V进行审阅。

![](../Images/c91704fa86310ccc95850e5c9302dfe4.png)![](../Images/46ac8ce44e66aed8e70d6b3c24cc5daa.png)

***ChatGPT 用户界面*** *(左)，* ***秋天的沃伦公园*** *(右)，截图和画作均由作者提供*

这是我的提示，接下来是GPT-4V的审阅。

> ***RobG****：根据这幅画的照片，写一篇带有建设性批评的评论。
> 
> 秋天的沃伦公园
> 
> 罗伯特·A·贡萨尔维斯
> 
> 布面丙烯画
> 
> 24” x 32”*
