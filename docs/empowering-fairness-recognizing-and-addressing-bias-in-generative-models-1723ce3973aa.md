# 赋能公平性：识别和解决生成模型中的偏见

> 原文：[https://towardsdatascience.com/empowering-fairness-recognizing-and-addressing-bias-in-generative-models-1723ce3973aa?source=collection_archive---------11-----------------------#2023-07-06](https://towardsdatascience.com/empowering-fairness-recognizing-and-addressing-bias-in-generative-models-1723ce3973aa?source=collection_archive---------11-----------------------#2023-07-06)

## 随着人工智能融入我们的日常生活，一个有偏见的模型可能对用户产生严重后果

[](https://medium.com/@kevin.berlemont?source=post_page-----1723ce3973aa--------------------------------)[![Kevin Berlemont, PhD](../Images/18697f38b76f1fb04870f565cfb04b4c.png)](https://medium.com/@kevin.berlemont?source=post_page-----1723ce3973aa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1723ce3973aa--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1723ce3973aa--------------------------------) [Kevin Berlemont, PhD](https://medium.com/@kevin.berlemont?source=post_page-----1723ce3973aa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3dea771eb493&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fempowering-fairness-recognizing-and-addressing-bias-in-generative-models-1723ce3973aa&user=Kevin+Berlemont%2C+PhD&userId=3dea771eb493&source=post_page-3dea771eb493----1723ce3973aa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1723ce3973aa--------------------------------) ·6分钟阅读·2023年7月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1723ce3973aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fempowering-fairness-recognizing-and-addressing-bias-in-generative-models-1723ce3973aa&user=Kevin+Berlemont%2C+PhD&userId=3dea771eb493&source=-----1723ce3973aa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1723ce3973aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fempowering-fairness-recognizing-and-addressing-bias-in-generative-models-1723ce3973aa&source=-----1723ce3973aa---------------------bookmark_footer-----------)![](../Images/49c317d74bb76b8a63d74a410f64cb29.png)

照片由 [Dainis Graveris](https://unsplash.com/@dainisgraveris?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

2021年，普林斯顿大学信息技术政策中心发布了一份报告，其中发现机器学习算法会从训练数据中获取类似于人类的偏见。其中一个显著的例子是关于亚马逊AI招聘工具的研究**[1]**。该工具是在上一年提交给亚马逊的简历上进行训练的，并对不同的候选人进行排名。由于过去十年技术职位中的性别不平衡，算法学会了与女性相关的语言，例如女性运动队，并会降低这些简历的排名。这个例子突显了不仅需要公平和准确的模型，还有数据集，以消除训练过程中的偏见。在像ChatGPT这样的生成模型快速发展的当前背景下，模型的偏见可能会带来严重后果，侵蚀用户的信任和全球接受度。因此，从商业角度来看，解决这些偏见是必要的，数据科学家（广义上定义）必须意识到这些问题，以减轻它们并确保他们…
