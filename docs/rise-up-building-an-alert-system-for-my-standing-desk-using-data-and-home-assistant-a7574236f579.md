# 崛起！利用数据和 Home Assistant 为我的站立式办公桌构建警报系统

> 原文：[`towardsdatascience.com/rise-up-building-an-alert-system-for-my-standing-desk-using-data-and-home-assistant-a7574236f579?source=collection_archive---------12-----------------------#2023-05-25`](https://towardsdatascience.com/rise-up-building-an-alert-system-for-my-standing-desk-using-data-and-home-assistant-a7574236f579?source=collection_archive---------12-----------------------#2023-05-25)

## 集成微处理器、Home Assistant、Grafana、InfluxDB 和 Telegram，打造更智能的桌面和更健康的工作环境

[](https://medium.com/@juandes?source=post_page-----a7574236f579--------------------------------)![胡安·德·迪奥斯·圣安东尼奥](https://medium.com/@juandes?source=post_page-----a7574236f579--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a7574236f579--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a7574236f579--------------------------------) [胡安·德·迪奥斯·圣安东尼奥](https://medium.com/@juandes?source=post_page-----a7574236f579--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9b9998a144da&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frise-up-building-an-alert-system-for-my-standing-desk-using-data-and-home-assistant-a7574236f579&user=Juan+De+Dios+Santos&userId=9b9998a144da&source=post_page-9b9998a144da----a7574236f579---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a7574236f579--------------------------------) ·11 分钟阅读·2023 年 5 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa7574236f579&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frise-up-building-an-alert-system-for-my-standing-desk-using-data-and-home-assistant-a7574236f579&user=Juan+De+Dios+Santos&userId=9b9998a144da&source=-----a7574236f579---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa7574236f579&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frise-up-building-an-alert-system-for-my-standing-desk-using-data-and-home-assistant-a7574236f579&source=-----a7574236f579---------------------bookmark_footer-----------)![](img/0fc8679dadbc1e23eb5c4424087070cb.png)

图片由 DALL-E 生成。提示词：“一个带有笔记本电脑的站立式办公桌。笔记本电脑的图像需要是折线图。”

我们都很清楚长期坐着对健康的风险。这可能导致肌肉退化、背部问题、糖尿病风险增加等等（[source](https://www.betterhealth.vic.gov.au/health/healthyliving/the-dangers-of-sitting)）。是的，情况确实很糟。尽管有这些有害影响，但我们中的许多人——包括我自己——仍然长时间坐着。我们这样做是因为我们喜欢这样，或者因为我们的工作需要这样，就像我一样。

为了应对这些健康风险，我买了一个立式办公桌。我喜欢这张桌子。它看起来很酷，并允许我配置高度预设，我在一天中会在这些预设之间切换。然而，我必须承认，有长时间桌子处于最低设置时——这反映了我缺乏运动。为了解决这个问题（也作为一个有趣的借口来开始一个新项目），我在桌子下安装了一个微处理器。这个微处理器监控桌子的高度，并且它是一个流程的入口，最终会通过 Telegram 通知提醒我站起来，如果桌子高度处于我定义的“坐着”预设下太久。
