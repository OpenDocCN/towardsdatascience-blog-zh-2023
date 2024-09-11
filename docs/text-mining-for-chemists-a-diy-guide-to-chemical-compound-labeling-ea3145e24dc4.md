# 自动化化学实体识别：创建您的 ChemNER 模型

> 原文：[`towardsdatascience.com/text-mining-for-chemists-a-diy-guide-to-chemical-compound-labeling-ea3145e24dc4?source=collection_archive---------4-----------------------#2023-11-16`](https://towardsdatascience.com/text-mining-for-chemists-a-diy-guide-to-chemical-compound-labeling-ea3145e24dc4?source=collection_archive---------4-----------------------#2023-11-16)

[](https://victormurcia-53351.medium.com/?source=post_page-----ea3145e24dc4--------------------------------)![Victor Murcia](https://victormurcia-53351.medium.com/?source=post_page-----ea3145e24dc4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ea3145e24dc4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ea3145e24dc4--------------------------------) [Victor Murcia](https://victormurcia-53351.medium.com/?source=post_page-----ea3145e24dc4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5a3b921bcf52&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftext-mining-for-chemists-a-diy-guide-to-chemical-compound-labeling-ea3145e24dc4&user=Victor+Murcia&userId=5a3b921bcf52&source=post_page-5a3b921bcf52----ea3145e24dc4---------------------post_header-----------) 发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----ea3145e24dc4--------------------------------) ·15 min read·Nov 16, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fea3145e24dc4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftext-mining-for-chemists-a-diy-guide-to-chemical-compound-labeling-ea3145e24dc4&user=Victor+Murcia&userId=5a3b921bcf52&source=-----ea3145e24dc4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fea3145e24dc4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftext-mining-for-chemists-a-diy-guide-to-chemical-compound-labeling-ea3145e24dc4&source=-----ea3145e24dc4---------------------bookmark_footer-----------)![](img/8f85d501be5dff1e61e9629b62c49954.png)

照片由[Aakash Dhage](https://unsplash.com/@aakashdhage?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)在[Unsplash](https://unsplash.com/photos/a-group-of-gold-and-silver-spheres-uV5n4TrFs8M?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)上拍摄。

我一直对化学有着浓厚的兴趣，它在塑造我学术和职业道路上发挥了重要作用。作为一名具有化学背景的数据专业人士，我找到了许多将科学和研究技能如创造力、好奇心、耐心、敏锐的观察力和数据分析应用于项目的方法。在本文中，我将带领你了解一个我命名为 ChemNER 的简单命名实体识别（NER）模型的开发过程。该模型可以识别文本中的化学化合物，并将其分类为烷烃、烯烃、炔烃、醇、醛、酮或羧酸等类别。

## TL;DR（简短概述）

如果你只是想试玩 ChemNER 模型和/或使用我制作的 Streamlit 应用程序，你可以通过下面的链接访问它们：

***HuggingFace 链接：*** [`huggingface.co/victormurcia/en_chemner`](https://huggingface.co/victormurcia/en_chemner)

***Streamlit 应用***：[ChemNER 链接](https://chemner-5i7mrvyelw79tzasxwy96x.streamlit.app/)

# 简介

NER 方法通常可以归类为以下 3 类之一：

+   基于词典：定义类别和术语的词典

+   基于规则：定义与每个类别对应的术语规则

+   基于机器学习（ML）的……
