# 填空自我监督在 NLP 中

> 原文：[https://towardsdatascience.com/fill-in-the-blanks-self-supervision-in-nlp-f0afb16dc7fd?source=collection_archive---------16-----------------------#2023-05-10](https://towardsdatascience.com/fill-in-the-blanks-self-supervision-in-nlp-f0afb16dc7fd?source=collection_archive---------16-----------------------#2023-05-10)

## 其强大之处以及如何解决

[](https://jagota-arun.medium.com/?source=post_page-----f0afb16dc7fd--------------------------------)[![Arun Jagota](../Images/3c3eb142f671b5fb933c2826d8ed78d9.png)](https://jagota-arun.medium.com/?source=post_page-----f0afb16dc7fd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f0afb16dc7fd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f0afb16dc7fd--------------------------------) [Arun Jagota](https://jagota-arun.medium.com/?source=post_page-----f0afb16dc7fd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fef9ed921edad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffill-in-the-blanks-self-supervision-in-nlp-f0afb16dc7fd&user=Arun+Jagota&userId=ef9ed921edad&source=post_page-ef9ed921edad----f0afb16dc7fd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0afb16dc7fd--------------------------------) · 14 分钟阅读 · 2023年5月10日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff0afb16dc7fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffill-in-the-blanks-self-supervision-in-nlp-f0afb16dc7fd&user=Arun+Jagota&userId=ef9ed921edad&source=-----f0afb16dc7fd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff0afb16dc7fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffill-in-the-blanks-self-supervision-in-nlp-f0afb16dc7fd&source=-----f0afb16dc7fd---------------------bookmark_footer-----------)![](../Images/03be7b5832a9d134961e51b08dd7a5a7.png)

由 [Patrick Tomasso](https://unsplash.com/@impatrickt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 摄影，来源于 [Unsplash](https://unsplash.com/photos/Oaqk7qqNh_c?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

预测下一个词在语言建模中有着悠久而成功的历史，尤其是在大型语言模型中。它利用了大量的文本语料库：维基百科、全球各地的公共网页等。它比这些语料库上的其他类型的无监督学习更为强大，因为这种学习是有监督的。此外，由于它是自我监督的，无需人工努力来创建标注数据。

考虑

> 暴露在阳光下会导致 __

从足够丰富的英语句子语料库中，机器学习应该能够学习出一个可以合理填补空白的语言模型，在这种情况下，填入的术语是“皮肤癌”。

我们为什么称之为监督？因为我们从一个完整的标记序列开始，模糊掉最后一个词，要求我们的模型预测它是什么，如果预测正确则给予奖励，如果预测错误则进行惩罚。所以这就是监督学习。

最近，这种自我监督的方法以一种简单而强大的方式得到了扩展。具体来说，待填入的空白可以出现在文本中的任何位置。因此，我们可能将其称为文本重构任务，而不仅仅是文本填空任务。
