# 如何测试你的单元测试

> 原文：[`towardsdatascience.com/how-to-test-your-unit-tests-afe18d6e19d8?source=collection_archive---------19-----------------------#2023-01-16`](https://towardsdatascience.com/how-to-test-your-unit-tests-afe18d6e19d8?source=collection_archive---------19-----------------------#2023-01-16)

## [单元测试](https://medium.com/tag/unit-testing)

## 关于变异测试的简要介绍

[](https://dr-robert-kuebler.medium.com/?source=post_page-----afe18d6e19d8--------------------------------)![Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----afe18d6e19d8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----afe18d6e19d8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----afe18d6e19d8--------------------------------) [Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----afe18d6e19d8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6d6b5fb431bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-test-your-unit-tests-afe18d6e19d8&user=Dr.+Robert+K%C3%BCbler&userId=6d6b5fb431bf&source=post_page-6d6b5fb431bf----afe18d6e19d8---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----afe18d6e19d8--------------------------------) ·5 分钟阅读·2023 年 1 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fafe18d6e19d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-test-your-unit-tests-afe18d6e19d8&user=Dr.+Robert+K%C3%BCbler&userId=6d6b5fb431bf&source=-----afe18d6e19d8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fafe18d6e19d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-test-your-unit-tests-afe18d6e19d8&source=-----afe18d6e19d8---------------------bookmark_footer-----------)![](img/fbcb034d926169de17728b3dae68786b.png)

照片由[Erik Mclean](https://unsplash.com/@introspectivedsgn?utm_source=medium&utm_medium=referral)拍摄，刊登在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在我上一篇文章中，我讨论了单元测试是数据科学工作的重要方面，因为错误的代码会导致分析结果出错。提高单元测试的一种方法是将**属性基础测试**与**示例基础测试**结合使用，即*正常*的单元测试方式。你可以在这里查看我的上一篇文章：

[](/stop-hardcoding-your-unit-tests-e6643dfd254b?source=post_page-----afe18d6e19d8--------------------------------) ## 停止硬编码你的单元测试

### 使用 Hypothesis 进行 Python 的基于属性的测试指南

towardsdatascience.com

在发布这篇文章后，我 [收到了回应](https://medium.com/@dotcom.software/mutation-testing-is-also-an-amazing-technique-to-verify-if-weve-missed-any-edge-cases-in-our-unit-e3c63cae1a06) 来自 **.com software**，指出了 **变异测试**，今天我想给你讲解一下。长话短说：

> 变异测试可以帮助你发现薄弱的测试。

薄弱的测试是那些给你虚假安全感的测试，因为它们会通过**即使你知道它们不应该通过**，其中最典型的就是 `assert True`。替换这些测试会使你的整个测试套件更具意义和可信度。

现在，让我们深入了解一下吧！

# 薄弱的测试
