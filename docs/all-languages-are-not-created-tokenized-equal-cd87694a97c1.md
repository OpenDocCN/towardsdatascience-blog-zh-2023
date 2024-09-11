# 所有语言的创建（分词）并不相等

> 原文：[https://towardsdatascience.com/all-languages-are-not-created-tokenized-equal-cd87694a97c1?source=collection_archive---------5-----------------------#2023-05-03](https://towardsdatascience.com/all-languages-are-not-created-tokenized-equal-cd87694a97c1?source=collection_archive---------5-----------------------#2023-05-03)

## 语言模型在某些语言中的成本远高于其他语言

[](https://medium.com/@artfish?source=post_page-----cd87694a97c1--------------------------------)[![Yennie Jun](../Images/b635e965f21c3d55833269e12e861322.png)](https://medium.com/@artfish?source=post_page-----cd87694a97c1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cd87694a97c1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cd87694a97c1--------------------------------) [Yennie Jun](https://medium.com/@artfish?source=post_page-----cd87694a97c1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F12ca1ab81192&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fall-languages-are-not-created-tokenized-equal-cd87694a97c1&user=Yennie+Jun&userId=12ca1ab81192&source=post_page-12ca1ab81192----cd87694a97c1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cd87694a97c1--------------------------------) ·12分钟阅读·2023年5月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcd87694a97c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fall-languages-are-not-created-tokenized-equal-cd87694a97c1&user=Yennie+Jun&userId=12ca1ab81192&source=-----cd87694a97c1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcd87694a97c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fall-languages-are-not-created-tokenized-equal-cd87694a97c1&source=-----cd87694a97c1---------------------bookmark_footer-----------)![](../Images/d5d82c8ce5ba774594d82455e802916d.png)

“hey” 翻译成了52种不同的语言。文本的大小按比例调整，以对应于表示消息所需的令牌数量。图片由作者创建。

*原文文章发布在我的* [*博客*](https://www.artfish.ai/p/all-languages-are-not-created-tokenized)*上*。

大型语言模型如ChatGPT通过首先将文本拆分成称为**标记**的较小单元来处理和生成文本序列。在下图中，每个彩色块代表一个独特的标记。像“you”、“say”、“loud”和“always”这样的短或常见词是自己的标记，而像“atrocious”、“precocious”和“supercalifragilisticexpialidocious”这样的较长或不常见的词则被拆分成更小的子词。

![](../Images/17d355ca37bbda6bda1137a1956d5f38.png)

使用[OpenAI的标记器网站](https://platform.openai.com/tokenizer)可视化短文本的标记化。截图由作者提供。

这种**标记化**过程在不同语言中并不统一，导致在不同语言中产生的标记数量存在差异。例如，**缅甸语或阿姆哈拉语中的一个句子可能需要比英语中的类似信息多出10倍的标记**。

![](../Images/77a9be17c8b6729603e33808832bd519.png)

将同一信息翻译成五种语言的示例以及所需的相应标记数量…
