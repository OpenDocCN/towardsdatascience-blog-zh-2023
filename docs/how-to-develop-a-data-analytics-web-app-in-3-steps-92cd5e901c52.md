# 如何在 3 步中开发一个 Streamlit 数据分析 Web 应用

> 原文：[`towardsdatascience.com/how-to-develop-a-data-analytics-web-app-in-3-steps-92cd5e901c52?source=collection_archive---------4-----------------------#2023-02-25`](https://towardsdatascience.com/how-to-develop-a-data-analytics-web-app-in-3-steps-92cd5e901c52?source=collection_archive---------4-----------------------#2023-02-25)

## 创建您的第一个 YouTube 分析应用的逐步指南

[](https://destingong.medium.com/?source=post_page-----92cd5e901c52--------------------------------)![Destin Gong](https://destingong.medium.com/?source=post_page-----92cd5e901c52--------------------------------)[](https://towardsdatascience.com/?source=post_page-----92cd5e901c52--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----92cd5e901c52--------------------------------) [Destin Gong](https://destingong.medium.com/?source=post_page-----92cd5e901c52--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffa1913854e95&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-develop-a-data-analytics-web-app-in-3-steps-92cd5e901c52&user=Destin+Gong&userId=fa1913854e95&source=post_page-fa1913854e95----92cd5e901c52---------------------post_header-----------) 发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----92cd5e901c52--------------------------------) · 8 分钟阅读 · 2023 年 2 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F92cd5e901c52&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-develop-a-data-analytics-web-app-in-3-steps-92cd5e901c52&user=Destin+Gong&userId=fa1913854e95&source=-----92cd5e901c52---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F92cd5e901c52&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-develop-a-data-analytics-web-app-in-3-steps-92cd5e901c52&source=-----92cd5e901c52---------------------bookmark_footer-----------)![](img/2033b5a816b904b2e5d44e11dcbc8159.png)

图片由[Tran Mau Tri Tam](https://unsplash.com/@tranmautritam?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

大多数时候，数据科学/数据分析项目最终只交付一个静态报告或仪表板，这极大地降低了在过程中的努力和思考。另一方面，网页应用是展示数据分析工作的绝佳方式，可以进一步扩展为自助和互动平台上的服务。然而，作为数据科学家或数据分析师，我们并未接受开发软件或网站的培训。在这篇文章中，我将介绍像 Streamlit 和 Plotly 这样的工具，它们允许我们通过网页应用轻松开发和提供数据分析项目，具体分为以下三个步骤：

![](img/4a8bbd1139b13a7381239bbf0f585642.png)

通过 3 个步骤开发数据分析网页应用（图片来源于作者的[网站](https://www.visual-design.net/)）

1.  提取数据并建立数据库

1.  将数据分析过程定义为函数

1.  构建网页应用界面

然后，我们将能够创建一个像这样的简单网页应用：
