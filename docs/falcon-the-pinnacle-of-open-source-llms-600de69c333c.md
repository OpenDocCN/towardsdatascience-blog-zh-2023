# Falcon: 开源LLMs的巅峰

> 原文：[https://towardsdatascience.com/falcon-the-pinnacle-of-open-source-llms-600de69c333c?source=collection_archive---------1-----------------------#2023-10-24](https://towardsdatascience.com/falcon-the-pinnacle-of-open-source-llms-600de69c333c?source=collection_archive---------1-----------------------#2023-10-24)

## 开源LLMs与专有LLMs之间的差距持续缩小……

[](https://wolfecameron.medium.com/?source=post_page-----600de69c333c--------------------------------)[![Cameron R. Wolfe, Ph.D.](../Images/52bb88d7cf1105501be2fae5ccbe7a03.png)](https://wolfecameron.medium.com/?source=post_page-----600de69c333c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----600de69c333c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----600de69c333c--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----600de69c333c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffalcon-the-pinnacle-of-open-source-llms-600de69c333c&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----600de69c333c---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----600de69c333c--------------------------------) ·14 min read·2023年10月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F600de69c333c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffalcon-the-pinnacle-of-open-source-llms-600de69c333c&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----600de69c333c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F600de69c333c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffalcon-the-pinnacle-of-open-source-llms-600de69c333c&source=-----600de69c333c---------------------bookmark_footer-----------)![](../Images/4a5506c6e87e01bb8145fcb5b5d29b13.png)

（图片由[艾伦·梅尔索姆](https://unsplash.com/@merse?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)拍摄，发布在[Unsplash](https://unsplash.com/photos/aerial-photography-of-bird-NzkjR1pw0Lc?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)）

最近对开源大型语言模型（LLMs）的研究主要集中在两个领域：模仿学习和预训练开源基础模型。虽然这两种方法都很可行，但创建高质量的开源基础模型尤为诱人，因为这些模型可以在更低的成本下进行进一步微调，并用于各种不同的下游应用。最初创建这些模型的尝试失败了。尽管后来的模型（例如LLaMA和MPT-7B）表现得更好，但这些模型直到最近才在质量上与它们的专有对手（例如GPT-3.5或GPT-4）相匹敌。

随着Falcon-7B和Falcon-40B LLMs的发布[1]，我们首次看到——*开源基础LLMs*开始在质量上与最受欢迎的付费模型相抗衡。通过一种新颖的数据管道训练于大量文本语料库，这些模型在开源LLMs中达到了（大幅度超越）新的最先进性能，并且可以在商业应用中免费使用。更好的是，Falcon模型采用了对其基础变压器架构的若干修改，这些修改显著加快了推理速度，甚至可以提高预训练的效率。
