# 如何在3步中开发一个Streamlit数据分析Web应用

> 原文：[https://towardsdatascience.com/how-to-develop-a-data-analytics-web-app-in-3-steps-92cd5e901c52?source=collection_archive---------4-----------------------#2023-02-25](https://towardsdatascience.com/how-to-develop-a-data-analytics-web-app-in-3-steps-92cd5e901c52?source=collection_archive---------4-----------------------#2023-02-25)

## 创建您的第一个YouTube分析应用的逐步指南

[](https://destingong.medium.com/?source=post_page-----92cd5e901c52--------------------------------)[![Destin Gong](../Images/c93d4341a928c36bc47031f02e23ebbf.png)](https://destingong.medium.com/?source=post_page-----92cd5e901c52--------------------------------)[](https://towardsdatascience.com/?source=post_page-----92cd5e901c52--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----92cd5e901c52--------------------------------) [Destin Gong](https://destingong.medium.com/?source=post_page-----92cd5e901c52--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffa1913854e95&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-develop-a-data-analytics-web-app-in-3-steps-92cd5e901c52&user=Destin+Gong&userId=fa1913854e95&source=post_page-fa1913854e95----92cd5e901c52---------------------post_header-----------) 发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----92cd5e901c52--------------------------------) · 8分钟阅读 · 2023年2月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F92cd5e901c52&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-develop-a-data-analytics-web-app-in-3-steps-92cd5e901c52&user=Destin+Gong&userId=fa1913854e95&source=-----92cd5e901c52---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F92cd5e901c52&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-develop-a-data-analytics-web-app-in-3-steps-92cd5e901c52&source=-----92cd5e901c52---------------------bookmark_footer-----------)![](../Images/2033b5a816b904b2e5d44e11dcbc8159.png)

图片由[Tran Mau Tri Tam](https://unsplash.com/@tranmautritam?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

大多数时候，数据科学/数据分析项目最终只交付一个静态报告或仪表板，这极大地降低了在过程中的努力和思考。另一方面，网页应用是展示数据分析工作的绝佳方式，可以进一步扩展为自助和互动平台上的服务。然而，作为数据科学家或数据分析师，我们并未接受开发软件或网站的培训。在这篇文章中，我将介绍像Streamlit和Plotly这样的工具，它们允许我们通过网页应用轻松开发和提供数据分析项目，具体分为以下三个步骤：

![](../Images/4a8bbd1139b13a7381239bbf0f585642.png)

通过3个步骤开发数据分析网页应用（图片来源于作者的[网站](https://www.visual-design.net/)）

1.  提取数据并建立数据库

1.  将数据分析过程定义为函数

1.  构建网页应用界面

然后，我们将能够创建一个像这样的简单网页应用：
