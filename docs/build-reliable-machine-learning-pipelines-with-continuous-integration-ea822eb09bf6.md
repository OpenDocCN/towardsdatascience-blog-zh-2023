# 使用持续集成构建可靠的机器学习管道

> 原文：[`towardsdatascience.com/build-reliable-machine-learning-pipelines-with-continuous-integration-ea822eb09bf6?source=collection_archive---------2-----------------------#2023-04-06`](https://towardsdatascience.com/build-reliable-machine-learning-pipelines-with-continuous-integration-ea822eb09bf6?source=collection_archive---------2-----------------------#2023-04-06)

## 自动化机器学习工作流程与持续集成

[](https://khuyentran1476.medium.com/?source=post_page-----ea822eb09bf6--------------------------------)![Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----ea822eb09bf6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ea822eb09bf6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ea822eb09bf6--------------------------------) [Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----ea822eb09bf6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F84a02493194a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-reliable-machine-learning-pipelines-with-continuous-integration-ea822eb09bf6&user=Khuyen+Tran&userId=84a02493194a&source=post_page-84a02493194a----ea822eb09bf6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ea822eb09bf6--------------------------------) ·8 分钟阅读·2023 年 4 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fea822eb09bf6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-reliable-machine-learning-pipelines-with-continuous-integration-ea822eb09bf6&user=Khuyen+Tran&userId=84a02493194a&source=-----ea822eb09bf6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fea822eb09bf6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-reliable-machine-learning-pipelines-with-continuous-integration-ea822eb09bf6&source=-----ea822eb09bf6---------------------bookmark_footer-----------)

# 场景

作为数据科学家，你负责改进当前生产中的模型。在花费数月时间对模型进行微调后，你发现了一个比原始模型具有更高准确度的模型。

对你的突破感到兴奋时，你创建了一个拉取请求，将你的模型合并到主分支中。

![](img/480d636e81974aa7e60cbc4909caa2af.png)

作者提供的图片

不幸的是，由于变化众多，你的团队需要一周多的时间来评估和分析这些变化，这最终阻碍了项目进展。

此外，在部署模型后，你发现了由于代码错误导致的意外行为，使公司蒙受了损失。

![](img/0ee3976e144e82d05820476edb13c49d.png)

图片来源于作者

回顾过去，**在提交拉取请求后自动化代码和模型测试**将会…
