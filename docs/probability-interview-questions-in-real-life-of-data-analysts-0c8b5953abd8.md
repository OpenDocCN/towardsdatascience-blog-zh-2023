# 数据分析师实际生活中的概率面试问题

> 原文：[https://towardsdatascience.com/probability-interview-questions-in-real-life-of-data-analysts-0c8b5953abd8?source=collection_archive---------3-----------------------#2023-10-22](https://towardsdatascience.com/probability-interview-questions-in-real-life-of-data-analysts-0c8b5953abd8?source=collection_archive---------3-----------------------#2023-10-22)

## 将概率面试问题与数据分析师的日常任务联系起来

[](https://medium.com/@kseniia.baidina?source=post_page-----0c8b5953abd8--------------------------------)[![Kseniia Baidina](../Images/a6ee80021fb9b319d463006ce5952634.png)](https://medium.com/@kseniia.baidina?source=post_page-----0c8b5953abd8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----0c8b5953abd8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----0c8b5953abd8--------------------------------) [Kseniia Baidina](https://medium.com/@kseniia.baidina?source=post_page-----0c8b5953abd8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F74b02464564b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprobability-interview-questions-in-real-life-of-data-analysts-0c8b5953abd8&user=Kseniia+Baidina&userId=74b02464564b&source=post_page-74b02464564b----0c8b5953abd8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----0c8b5953abd8--------------------------------) · 5分钟阅读 · 2023年10月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F0c8b5953abd8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprobability-interview-questions-in-real-life-of-data-analysts-0c8b5953abd8&user=Kseniia+Baidina&userId=74b02464564b&source=-----0c8b5953abd8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F0c8b5953abd8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprobability-interview-questions-in-real-life-of-data-analysts-0c8b5953abd8&source=-----0c8b5953abd8---------------------bookmark_footer-----------)![](../Images/088890fd4b59e6ee8c392f33701bb03c.png)

[Thomas](https://unsplash.com/@tee833)在[Unsplash](https://unsplash.com/photos/a-close-up-of-a-bunch-of-buttons-on-a-table-0n7_eiAQZwA)上的照片

如果你申请数据分析师或数据科学家的职位，在面试中你会经常遇到概率问题。但问题是：有些人确信这些问题与实际工作关系不大。像“为什么我们要计算掷骰子5次都掷出6的概率？”这样的问题经常出现。在这篇文章中，我将分享一些真实的例子来解释为什么理解概率比你想象的更重要。为此，我们来看看一些面试任务，并了解它们在现实世界中的应用。

***Q1\. 你连续抛掷10次硬币，所有硬币都是正面朝上的概率是多少？***

想象你是一个食品配送服务的数据分析师。每次订单完成后，客户可以评分食物的质量。团队的主要目标是提供顶级服务，如果餐厅收到差评，你需要检查。所以，关键问题是——多少条差评应该触发对餐厅的检查？

有时候，一个餐厅偶尔会收到一些不太好的反馈，这并不是他们的错。如果一个餐厅处理了1000个订单，他们可能会因为偶然原因收到几条差评。

这样考虑：大约5%的订单偶然会收到负面评价。然后，每个餐厅的差评数量遵循二项分布*Bin(n, p)*，其中“n”是订单数量，“p”是负面评价的可能性（在我们这里是5%）。

所以，如果一个餐厅有100个订单，他们收到至少7条差评的概率大约是23.4%，而收到至少10条差评的概率则小得多，只有2.8%。你可以通过计算器[这里](https://homepage.divms.uiowa.edu/~mbognar/applets/bin.html)来检查，参数是*n=100*、*x=10*、*p=0.05*，别忘了选择选项*x>=X*。

![](../Images/a67179d740a927145606ef2ad58d8b5b.png)

作者提供的图片。

结论是：如果你将检查阈值设定为100个订单中的7条差评，你可能会过于频繁地检查餐厅，这意味着你会增加额外成本，并对餐厅施加更多压力。

***Q2\. 你从52张标准扑克牌中抽取10次。抽到没有红色牌的概率是多少？***

现在，想象你处在电子商务网站的世界里。你和你的团队刚刚引入了一种新的支付方式，你想知道客户使用这个新功能的频率。但问题是——由于一个小bug，大约2%的新支付请求会失败。换句话说，客户在98%的会话中看到这个新支付选项。为了弄清楚客户选择这种支付方式的频率，你想关注那些始终可以使用它的用户。但这就有点棘手了。

设想一个只有一个会话的用户——你以2%的概率将他们排除在分析之外。现在，考虑一个有25个会话的用户。对于他们来说，*至少一个会话*中没有该功能的机会是1–0.98²⁵ = 39.7%。所以，你可能会无意中遗漏一些最忠诚的客户，这可能会扭曲你的分析。

![](../Images/9626b75b67f57412d03af33747441658.png)

图片由作者提供。

***Q3\. 如果你掷骰子三次，得到两个连续的三的概率是多少？***

想象你在一家像Uber这样的打车公司工作。在某些国家，人们仍然用现金支付车费，这对司机来说可能是个麻烦。他们需要携带零钱，处理现金交易等等。

你的团队担心如果司机连续接到三个现金订单，他们可能会感到沮丧并且零钱用完。因此，你考虑在这种情况下限制现金订单。但在此之前，你想了解这种情况发生的频率。

假设每个司机每天的平均行程数是10，其中10%的行程以现金支付。

因此，得到三个连续现金订单的概率是0.1*0.1*0.1 = 0.001\. 但它可以是第1、第2、第3次订单；第2、第3、第4次订单，等等。这意味着连续三个现金订单的机会仅为8*0.1*0.1*0.1 = 0.008\. 看起来相当低，你可能要考虑暂时不实现这个功能。

![](../Images/cec314d785898023edd61f539d47ec6c.png)

***Q4, 一项HIV测试的准确率是99%（双向）。只有0.3%的人口是HIV阳性。如果一个随机人检测结果为阳性，这个人HIV阳性的概率是多少？***

原文文章见[这里](/14-probability-problems-for-acing-data-science-interviews-3735025a6425)。

你在银行或信用行业，建立模型来预测客户是否会归还贷款。总体来说，85%的贷款通常会被偿还。在你最新的模型中，对于那些还款的客户，预测正确率为92%。然而，当客户没有还款时，预测的正确率仅为60%。现在，你有一个担忧：**如果你的模型表示客户不会还款，那么他们实际还款的真实概率是多少？**

首先，让我们计算模型预测“客户不会还款”的可能性。这涉及两个部分：

+   从*不会*还款的客户那里得到这种预测的概率：0.6*(1–0.85) = 0.09

+   从*会*还款的客户那里得到这种预测的概率：（1–0.92）*0.85 = 0.068

+   如果我们的模型认为客户不会还款，那么客户实际还款的概率是：0.068/(0.068+0.09) = 0.43

![](../Images/27af7e3a574742fdb10fad0cac857b15.png)

因此，如果你认为客户不会归还贷款，实际上他们有相当高的概率会归还。

这篇文章的全部意义是什么？它强调了理解概率和组合数学对数据科学家和分析师至关重要。在你的日常生活中，你会遇到需要掌握概率的情况，否则你可能会得出错误的结论。然而，从雇主的角度来看，面试问题应更具实际性，以帮助未来的分析师认识到这些知识在工作中的实际应用。

*感谢你花时间阅读这篇文章。我非常希望听到你的想法，请随时分享你的评论或问题。*
