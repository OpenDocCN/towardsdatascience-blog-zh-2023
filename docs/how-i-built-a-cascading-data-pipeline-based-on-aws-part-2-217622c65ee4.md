# 我是如何基于 AWS 构建级联数据管道的（第二部分）

> 原文：[`towardsdatascience.com/how-i-built-a-cascading-data-pipeline-based-on-aws-part-2-217622c65ee4?source=collection_archive---------12-----------------------#2023-08-25`](https://towardsdatascience.com/how-i-built-a-cascading-data-pipeline-based-on-aws-part-2-217622c65ee4?source=collection_archive---------12-----------------------#2023-08-25)

## 自动化、可扩展且强大

[](https://anzhemeng.medium.com/?source=post_page-----217622c65ee4--------------------------------)![Memphis Meng](https://anzhemeng.medium.com/?source=post_page-----217622c65ee4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----217622c65ee4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----217622c65ee4--------------------------------) [Memphis Meng](https://anzhemeng.medium.com/?source=post_page-----217622c65ee4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F85370dce2b14&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-built-a-cascading-data-pipeline-based-on-aws-part-2-217622c65ee4&user=Memphis+Meng&userId=85370dce2b14&source=post_page-85370dce2b14----217622c65ee4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----217622c65ee4--------------------------------) ·10 分钟阅读·2023 年 8 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F217622c65ee4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-built-a-cascading-data-pipeline-based-on-aws-part-2-217622c65ee4&user=Memphis+Meng&userId=85370dce2b14&source=-----217622c65ee4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F217622c65ee4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-built-a-cascading-data-pipeline-based-on-aws-part-2-217622c65ee4&source=-----217622c65ee4---------------------bookmark_footer-----------)![](img/527158d227cde2bf33b0286a233784ff.png)

照片由 [Mehmet Ali Peker](https://unsplash.com/@mrpeker?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，发布在 [Unsplash](https://unsplash.com/photos/hfiym43qBpk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

[之前](https://medium.com/p/997b212a84d2)我分享了使用 AWS CloudFormation 技术开发数据管道的经验。然而，这种方法并不理想，因为它留下了 3 个待解决的问题：

1.  部署必须手动进行，这可能增加错误的几率；

1.  所有资源都在一个单一的堆栈中创建，没有适当的边界和层次；随着开发周期的进行，资源堆栈将变得更加庞大，管理起来将是一场灾难；

1.  许多资源应当在其他项目中持续使用和重用。

简而言之，我们将以敏捷的方式提高这个项目的可管理性和可重用性。

# 解决方案

AWS 允许用户实现两种类型的 CloudFormation 结构模式：跨堆栈引用和嵌套堆叠。跨堆栈引用代表一种独立开发云堆栈的设计风格，而所有堆栈之间的资源可以根据引用关系相互关联。[嵌套堆叠](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-nested-stacks.html) 指的是由其他堆栈组成的 CloudFormation 堆栈。通过使用`[AWS::CloudFormation::Stack](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-stack.html)`资源来实现。
