# 促销预测：与零售巨头的案例研究

> 原文：[https://towardsdatascience.com/promotion-forecasting-case-study-with-a-retail-giant-4d8dcbae6b5c?source=collection_archive---------4-----------------------#2023-12-30](https://towardsdatascience.com/promotion-forecasting-case-study-with-a-retail-giant-4d8dcbae6b5c?source=collection_archive---------4-----------------------#2023-12-30)

## 发现利用机器学习、自然语言处理（NLP）和领域专业知识的需求预测算法如何在短短一年内实现了全国运营中的缺货和短缺情况减少了令人印象深刻的18%。

[](https://medium.com/@AlexandreWarembourg?source=post_page-----4d8dcbae6b5c--------------------------------)[![Alexandre Warembourg](../Images/cdc21c2c337c6dcdacdaaea60c49c832.png)](https://medium.com/@AlexandreWarembourg?source=post_page-----4d8dcbae6b5c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4d8dcbae6b5c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4d8dcbae6b5c--------------------------------) [Alexandre Warembourg](https://medium.com/@AlexandreWarembourg?source=post_page-----4d8dcbae6b5c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa5f80e627a47&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpromotion-forecasting-case-study-with-a-retail-giant-4d8dcbae6b5c&user=Alexandre+Warembourg&userId=a5f80e627a47&source=post_page-a5f80e627a47----4d8dcbae6b5c---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4d8dcbae6b5c--------------------------------) ·7分钟阅读·2023年12月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4d8dcbae6b5c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpromotion-forecasting-case-study-with-a-retail-giant-4d8dcbae6b5c&user=Alexandre+Warembourg&userId=a5f80e627a47&source=-----4d8dcbae6b5c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4d8dcbae6b5c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpromotion-forecasting-case-study-with-a-retail-giant-4d8dcbae6b5c&source=-----4d8dcbae6b5c---------------------bookmark_footer-----------)![](../Images/af1f61cdde4f34a147d5a5a767580379.png)

来源：图像由Dall-E根据作者提示创建。

# 引言：面临的挑战

在零售领域，全球领先的欧尚面临着一个关键挑战：掌握促销预测的艺术。这个故事讲述了如何利用机器学习、自然语言处理（NLP）和深厚的领域知识，我们取得了突破——在短短一年内将缺货和库存过剩减少了18%。

# 背景介绍

在每个进行促销的商店中，准确预测这些促销商品的需求是一个关键挑战。目标明确但复杂：准确地将供应与不断变化的客户需求对接，以避免过剩库存并保证客户满意度。

在欧尚零售国际的数据预测团队中，我开始了一项任务。我们的目标？制定一个可以在多样化的国家中适应的预测模型，改动最小。这个模型最初为欧尚乌克兰开发，后来…
