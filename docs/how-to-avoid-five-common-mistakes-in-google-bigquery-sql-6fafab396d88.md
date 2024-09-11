# 如何避免 Google BigQuery / SQL 中的五个常见错误

> 原文：[https://towardsdatascience.com/how-to-avoid-five-common-mistakes-in-google-bigquery-sql-6fafab396d88?source=collection_archive---------6-----------------------#2023-10-25](https://towardsdatascience.com/how-to-avoid-five-common-mistakes-in-google-bigquery-sql-6fafab396d88?source=collection_archive---------6-----------------------#2023-10-25)

## 在多年来使用 BigQuery 的过程中，我观察到即使是经验丰富的数据科学家也常犯的 5 个问题

[](https://medium.com/@benjamin.thuerer?source=post_page-----6fafab396d88--------------------------------)[![Benjamin Thürer](../Images/b4c49698c7270c592bf992fc47f75765.png)](https://medium.com/@benjamin.thuerer?source=post_page-----6fafab396d88--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6fafab396d88--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6fafab396d88--------------------------------) [Benjamin Thürer](https://medium.com/@benjamin.thuerer?source=post_page-----6fafab396d88--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcd27eb9661fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-avoid-five-common-mistakes-in-google-bigquery-sql-6fafab396d88&user=Benjamin+Th%C3%BCrer&userId=cd27eb9661fd&source=post_page-cd27eb9661fd----6fafab396d88---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6fafab396d88--------------------------------) · 8分钟阅读 · 2023年10月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6fafab396d88&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-avoid-five-common-mistakes-in-google-bigquery-sql-6fafab396d88&user=Benjamin+Th%C3%BCrer&userId=cd27eb9661fd&source=-----6fafab396d88---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6fafab396d88&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-avoid-five-common-mistakes-in-google-bigquery-sql-6fafab396d88&source=-----6fafab396d88---------------------bookmark_footer-----------)![](../Images/6b6e9ac6ded4356c801ecb5e2396fdfb.png)

照片由 [James Harrison](https://unsplash.com/@jstrippa?utm_source=medium&utm_medium=referral) 拍摄，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Google BigQuery 受到许多原因的青睐。它极其快速，易于使用，提供了完整的 GCP 套件，处理你的数据，并确保能早期发现错误。更重要的是，你可以使用标准 SQL 和一些非常棒的内置函数。简而言之，它几乎是一个完整的解决方案！

> 始终假设存在错误和重复项，永远如此！

然而，与其他网络服务和编程语言类似，在使用 BigQuery 时，有一些需要知道的事项，以避免陷入陷阱。多年来，我自己犯了很多错误，并意识到几乎每一个我认识的人在某个时候也遇到过相同的问题。我在这里想要指出其中一些问题，因为我在职业生涯中发现这些问题比较晚，也看到其他非常有经验的数据科学家遇到同样的问题。

因此，我将提供我的前五大潜在错误列表，这些错误几乎每个人在使用 BigQuery 时都会犯，而且有些人可能甚至不知道。所以务必避免这些错误，因为每一点都可能导致严重的…
