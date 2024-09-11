# 五个你应该警惕的数据泄漏隐患

> 原文：[https://towardsdatascience.com/five-hidden-causes-of-data-leakage-you-should-be-aware-of-e44df654f185?source=collection_archive---------13-----------------------#2023-04-11](https://towardsdatascience.com/five-hidden-causes-of-data-leakage-you-should-be-aware-of-e44df654f185?source=collection_archive---------13-----------------------#2023-04-11)

## 以及它们如何破坏机器学习模型

[](https://donatoriccio.medium.com/?source=post_page-----e44df654f185--------------------------------)[![Donato Riccio](../Images/0af2a026e72a023db4635522cbca50eb.png)](https://donatoriccio.medium.com/?source=post_page-----e44df654f185--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e44df654f185--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e44df654f185--------------------------------) [Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----e44df654f185--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe384fc71d292&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffive-hidden-causes-of-data-leakage-you-should-be-aware-of-e44df654f185&user=Donato+Riccio&userId=e384fc71d292&source=post_page-e384fc71d292----e44df654f185---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e44df654f185--------------------------------) ·8 min 阅读·2023年4月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe44df654f185&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffive-hidden-causes-of-data-leakage-you-should-be-aware-of-e44df654f185&user=Donato+Riccio&userId=e384fc71d292&source=-----e44df654f185---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe44df654f185&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffive-hidden-causes-of-data-leakage-you-should-be-aware-of-e44df654f185&source=-----e44df654f185---------------------bookmark_footer-----------)![](../Images/cd2be5f86610dde00f6fbf97f8484e3e.png)

图片由 [Linh Pham](https://unsplash.com/@linharex?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

数据泄露是一个隐秘的问题，常常困扰机器学习模型。泄露一词指的是测试数据*泄露*到训练集中。这发生在模型训练时接触到不应有的训练数据，导致过拟合，并在未见过的数据上表现不佳。这就像用测试答案来训练学生——他们在特定的测试中表现很好，但在其他测试中表现就不那么出色。机器学习的目标是创建能够泛化并对新、未见过的数据进行准确预测的模型。数据泄露破坏了这一目标，因此重要的是要意识到并做好准备。本文将详细探讨数据泄露是什么、其潜在原因，以及通过使用 Python 和 scikit-learn 的实际例子以及研究中的案例来防止数据泄露的方法。

# 数据泄露的后果

+   **过拟合**。数据泄露最显著的后果之一就是过拟合。过拟合发生在模型被训练得过于贴合训练数据，以至于无法对新数据进行泛化。当数据泄露发生时，模型在你开发过程中使用的训练集和测试集上的准确率都会很高。然而，当…
