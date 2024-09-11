# CFXplorer: 反事实解释生成 Python 包

> 原文：[`towardsdatascience.com/cfxplorer-counterfactual-explanation-generation-python-package-483ca4221ab8?source=collection_archive---------8-----------------------#2023-08-17`](https://towardsdatascience.com/cfxplorer-counterfactual-explanation-generation-python-package-483ca4221ab8?source=collection_archive---------8-----------------------#2023-08-17)

## 介绍一个用于生成树基算法反事实解释的 Python 包

[](https://medium.com/@kyosuke1029?source=post_page-----483ca4221ab8--------------------------------)![Kyosuke Morita](https://medium.com/@kyosuke1029?source=post_page-----483ca4221ab8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----483ca4221ab8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----483ca4221ab8--------------------------------) [Kyosuke Morita](https://medium.com/@kyosuke1029?source=post_page-----483ca4221ab8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe98eeeb527fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcfxplorer-counterfactual-explanation-generation-python-package-483ca4221ab8&user=Kyosuke+Morita&userId=e98eeeb527fd&source=post_page-e98eeeb527fd----483ca4221ab8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----483ca4221ab8--------------------------------) ·9 分钟阅读·2023 年 8 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F483ca4221ab8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcfxplorer-counterfactual-explanation-generation-python-package-483ca4221ab8&user=Kyosuke+Morita&userId=e98eeeb527fd&source=-----483ca4221ab8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F483ca4221ab8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcfxplorer-counterfactual-explanation-generation-python-package-483ca4221ab8&source=-----483ca4221ab8---------------------bookmark_footer-----------)

随着机器学习模型在现实世界中的应用越来越广泛，解释性的重要性也在不断增加。了解模型如何做出决策不仅对模型的用户有益，也对那些受到模型决策影响的人有帮助。**反事实解释**应运而生，以应对这一问题，因为它们允许个人通过扰动原始数据来了解如何实现理想结果。在短期内，反事实解释可能会向受到机器学习模型决策影响的人提供可操作的建议。例如，一位贷款申请被拒绝的人可以了解到这次被拒绝的原因，并能改进下次申请时的表现，这将有助于提高他们的申请成功率。

Lucic 等人 [1] 提出了 FOCUS，该方法旨在为基于树的机器学习模型中的所有实例生成与原始数据的最优距离反事实解释。

CFXplorer 是一个 Python 包，使用 FOCUS 算法为给定的模型和数据生成反事实解释。本文介绍并展示了如何使用 CFXplorer 生成反事实解释。

# 链接
