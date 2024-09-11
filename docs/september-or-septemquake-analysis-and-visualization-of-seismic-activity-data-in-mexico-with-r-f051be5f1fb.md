# 9 月还是“九月地震”？使用 R 分析和可视化墨西哥的地震活动数据

> 原文：[`towardsdatascience.com/september-or-septemquake-analysis-and-visualization-of-seismic-activity-data-in-mexico-with-r-f051be5f1fb?source=collection_archive---------13-----------------------#2023-09-21`](https://towardsdatascience.com/september-or-septemquake-analysis-and-visualization-of-seismic-activity-data-in-mexico-with-r-f051be5f1fb?source=collection_archive---------13-----------------------#2023-09-21)

## 如何使用来自 SSN（国家地震服务）的数据分析和可视化墨西哥的地震历史

[](https://cosmoduende.medium.com/?source=post_page-----f051be5f1fb--------------------------------)![Saúl Buentello](https://cosmoduende.medium.com/?source=post_page-----f051be5f1fb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f051be5f1fb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f051be5f1fb--------------------------------) [Saúl Buentello](https://cosmoduende.medium.com/?source=post_page-----f051be5f1fb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F38f118393fa8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fseptember-or-septemquake-analysis-and-visualization-of-seismic-activity-data-in-mexico-with-r-f051be5f1fb&user=Sa%C3%BAl+Buentello&userId=38f118393fa8&source=post_page-38f118393fa8----f051be5f1fb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f051be5f1fb--------------------------------) · 13 分钟阅读 · 2023 年 9 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff051be5f1fb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fseptember-or-septemquake-analysis-and-visualization-of-seismic-activity-data-in-mexico-with-r-f051be5f1fb&user=Sa%C3%BAl+Buentello&userId=38f118393fa8&source=-----f051be5f1fb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff051be5f1fb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fseptember-or-septemquake-analysis-and-visualization-of-seismic-activity-data-in-mexico-with-r-f051be5f1fb&source=-----f051be5f1fb---------------------bookmark_footer-----------)

当我在去年的 9 月 30 日开始写这篇文章时，又一个月结束了，但这不是一个普通的月份。对于许多墨西哥人来说，它代表了持续的忧虑，因为我们都记得，这个月份常常见证了我们国家被地震袭击，通常震中震级较高。本文旨在通过数据分析和可视化向读者提供有关墨西哥地震历史的宝贵见解。虽然它不进行预测或制定政策，但提供了对地震趋势和模式的更深入理解。通过获取这些知识，读者可以更好地为地震事件做好准备，并在建筑和灾难准备方面做出明智的决策。

有一个特别的日期在这篇文章中显得尤为重要并激发了本文的写作：**9 月 19 日**。在 1985 年的这一天，墨西哥经历了有记录以来最具毁灭性的地震，震中震级为 8.1 级。约有 40,000 人遇难，近 4,000 人从无数因地壳运动而倒塌的建筑物和房屋的废墟中被救出。
