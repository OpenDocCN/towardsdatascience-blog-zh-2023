# AutoML — 让机器学习为你的模型选择提供起步

> 原文：[https://towardsdatascience.com/automl-let-machine-learning-give-your-model-selection-a-jump-start-a318de373890?source=collection_archive---------5-----------------------#2023-02-13](https://towardsdatascience.com/automl-let-machine-learning-give-your-model-selection-a-jump-start-a318de373890?source=collection_archive---------5-----------------------#2023-02-13)

## 利用 AutoML 提高生产力

[](https://medium.com/@fmnobar?source=post_page-----a318de373890--------------------------------)[![Farzad Mahmoodinobar](../Images/2d75209693b712300e6f0796bd2487d0.png)](https://medium.com/@fmnobar?source=post_page-----a318de373890--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a318de373890--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a318de373890--------------------------------) [Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----a318de373890--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3c56b7d4893e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fautoml-let-machine-learning-give-your-model-selection-a-jump-start-a318de373890&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=post_page-3c56b7d4893e----a318de373890---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a318de373890--------------------------------) ·6 分钟阅读·2023年2月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa318de373890&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fautoml-let-machine-learning-give-your-model-selection-a-jump-start-a318de373890&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=-----a318de373890---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa318de373890&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fautoml-let-machine-learning-give-your-model-selection-a-jump-start-a318de373890&source=-----a318de373890---------------------bookmark_footer-----------)![](../Images/70f880341c0148024182fe0ad6b008b3.png)

人类与机器，[DALL.E 2](https://openai.com/dall-e-2/)

我们每天都使用机器学习（ML）来寻找问题的解决方案和进行预测，这通常涉及通过探索性分析来了解数据，接着进行数据清洗，根据我们最佳的判断决定使用哪些ML模型来解决问题，然后进行超参数优化和迭代。但如果我们可以利用ML来解决这些步骤以及选择最佳模型的更高层次的问题，而不是我们手动经历这些重复且繁琐的步骤呢？AutoML可以为我们解决这个问题！

在这篇文章中，我将演示如何仅用3行代码，AutoML在不到14秒的时间里超越了我个人开发的预测ML模型（用于之前的一篇文章）。

我在这篇文章中的目标不是提议我们不再需要科学家和ML从业者，因为我们有了AutoML，而是希望展示如何利用AutoML使我们的模型选择过程更加高效，从而提高整体生产力。一旦AutoML为我们提供了各种ML模型系列的性能比较，我们可以继续进行任务，并进一步微调模型以取得更好的结果。
