# 精通 Apache Airflow 中的 ExternalTaskSensor：如何计算执行时间差

> 原文：[https://towardsdatascience.com/mastering-externaltasksensor-in-apache-airflow-how-to-calculate-execution-delta-425093323758?source=collection_archive---------3-----------------------#2023-05-08](https://towardsdatascience.com/mastering-externaltasksensor-in-apache-airflow-how-to-calculate-execution-delta-425093323758?source=collection_archive---------3-----------------------#2023-05-08)

## External Task Sensors 阻止数据管道中糟糕的数据泄露。利用它们来创建可靠的数据基础设施。

[](https://casey-cheng.medium.com/?source=post_page-----425093323758--------------------------------)[![Casey Cheng](../Images/92174e223d1436b326ec42622ceefdd6.png)](https://casey-cheng.medium.com/?source=post_page-----425093323758--------------------------------)[](https://towardsdatascience.com/?source=post_page-----425093323758--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----425093323758--------------------------------) [Casey Cheng](https://casey-cheng.medium.com/?source=post_page-----425093323758--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F514ba843cfe4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-externaltasksensor-in-apache-airflow-how-to-calculate-execution-delta-425093323758&user=Casey+Cheng&userId=514ba843cfe4&source=post_page-514ba843cfe4----425093323758---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----425093323758--------------------------------) ·阅读时长 15 分钟·2023年5月8日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F425093323758&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-externaltasksensor-in-apache-airflow-how-to-calculate-execution-delta-425093323758&source=-----425093323758---------------------bookmark_footer-----------)![](../Images/6325cbd704ec069b44ecc8ece0402ada.png)

External Task Sensors 就像是守门人 — 它们阻止糟糕的数据向下游渗透。[图片](https://www.freepik.com/free-photo/arrangement-financial-crisis-with-wooden-pieces_11433457.htm#query=domino%20effect%20stop&position=46&from_view=keyword&track=ais) 由 Freepik 提供。

协调数据管道是一项精细的工作。在数据管道中，我们可能会有成千上万的任务同时运行，并且它们通常相互依赖。如果我们不小心，单点故障可能会产生连锁反应，向下传递并搞乱整个管道。

Apache Airflow 引入了外部任务传感器，以解决这些问题。虽然这是一个非常强大的功能，但它也带来了一定程度的复杂性。

在这篇介绍性文章中，我希望解开有关外部任务传感器的一些混乱，并展示我们如何利用它来增强数据管道的可靠性——理解传感器的作用！

+   [我们为什么需要外部任务传感器？](#6513)

+   [外部任务传感器的作用是什么？](#a2d1)

+   [我们如何创建外部任务传感器？](#a476)

+   [执行增量和执行日期函数是什么？](#bdab)

    – [如何计算执行增量？](#6c71)

    – [如何计算执行日期函数？](#cb9b)
