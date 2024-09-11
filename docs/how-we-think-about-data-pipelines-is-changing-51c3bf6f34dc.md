# 我们对数据管道的看法正在改变

> 原文：[https://towardsdatascience.com/how-we-think-about-data-pipelines-is-changing-51c3bf6f34dc?source=collection_archive---------2-----------------------#2023-11-08](https://towardsdatascience.com/how-we-think-about-data-pipelines-is-changing-51c3bf6f34dc?source=collection_archive---------2-----------------------#2023-11-08)

![](../Images/7d79fb389159e7b29999cbc1ea81691a.png)

图片由 [Ali Kazal](https://unsplash.com/@lureofadventure?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，来源于 [Unsplash](https://unsplash.com/photos/a-mountain-range-with-trees-in-the-foreground-and-a-field-in-the-foreground-walahB6h_sU?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

## 目标是可靠且高效地将数据发布到生产环境中

[](https://medium.com/@hugolu87?source=post_page-----51c3bf6f34dc--------------------------------)[![Hugo Lu](../Images/045de11463bb16ea70a816ba89118a9e.png)](https://medium.com/@hugolu87?source=post_page-----51c3bf6f34dc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----51c3bf6f34dc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----51c3bf6f34dc--------------------------------) [Hugo Lu](https://medium.com/@hugolu87?source=post_page-----51c3bf6f34dc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F202d118f4cb9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-we-think-about-data-pipelines-is-changing-51c3bf6f34dc&user=Hugo+Lu&userId=202d118f4cb9&source=post_page-202d118f4cb9----51c3bf6f34dc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----51c3bf6f34dc--------------------------------) · 6 分钟阅读·2023年11月8日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F51c3bf6f34dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-we-think-about-data-pipelines-is-changing-51c3bf6f34dc&user=Hugo+Lu&userId=202d118f4cb9&source=-----51c3bf6f34dc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F51c3bf6f34dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-we-think-about-data-pipelines-is-changing-51c3bf6f34dc&source=-----51c3bf6f34dc---------------------bookmark_footer-----------)

数据管道是组织在一个[有向无环图](https://en.wikipedia.org/wiki/Directed_acyclic_graph)或“DAG”中的一系列任务。历史上，这些任务通常在像[Airflow](https://airflow.apache.org/)或[Prefect](https://www.prefect.io/?gclid=Cj0KCQjwqP2pBhDMARIsAJQ0CzoV5DrzqjyDqDJonPcBPT5lE2ih47H2LMSKBst2jh6mR6h3azCcRnwaAhOJEALw_wcB)这样的开源工作流编排包上运行，并且需要由数据工程师或平台团队管理的[i[nfrastructure](https://www.bhavaniravi.com/apache-airflow/deploying-airflow-on-kubernetes)]。这些数据管道通常按照[计划](https://airflow.apache.org/docs/apache-airflow/1.10.1/scheduler.html)运行，并允许数据工程师在数据仓库或数据湖等位置更新数据。

现在这一点正在发生变化。正在发生一个[巨大的思维转变](/what-data-engineers-can-learn-from-software-engineers-and-vice-versa-643cade3ef23)。随着数据工程行业的发展，思维方式正在从“无论如何将数据移动以服务业务”转变为“可靠性和效率”/“软件工程”思维方式。

## 持续数据集成与交付

我之前写过关于[数据团队如何发布*数据*](https://medium.com/orchestras-data-release-pipeline-blog/a-new-paradigm-for-data-continuous-data-integration-and-delivery-miniseries-part-5-a3338b3ffd03)的内容，而软件团队发布*代码*。

这是一个叫做“持续数据集成与交付”的过程，是将数据可靠、高效地发布到生产环境中的过程。与软件工程中使用的“[CI/CD](https://aws.amazon.com/solutions/app-development/ci-cd/#:~:text=An%20integral%20part%20of%20development,with%20collaborative%20and%20automated%20processes.)”定义存在微妙的差别，见下图。
