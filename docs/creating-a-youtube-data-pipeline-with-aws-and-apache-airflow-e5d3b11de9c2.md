# 使用 AWS 和 Apache Airflow 创建 YouTube 数据管道

> 原文：[https://towardsdatascience.com/creating-a-youtube-data-pipeline-with-aws-and-apache-airflow-e5d3b11de9c2?source=collection_archive---------5-----------------------#2023-04-20](https://towardsdatascience.com/creating-a-youtube-data-pipeline-with-aws-and-apache-airflow-e5d3b11de9c2?source=collection_archive---------5-----------------------#2023-04-20)

## 一个有效管理 YouTube 数据的解决方案，利用了云服务和作业调度器

[](https://medium.com/@aashishnair?source=post_page-----e5d3b11de9c2--------------------------------)[![Aashish Nair](../Images/23f4b3839e464419332b690a4098d824.png)](https://medium.com/@aashishnair?source=post_page-----e5d3b11de9c2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e5d3b11de9c2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e5d3b11de9c2--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----e5d3b11de9c2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3087ba81e065&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-a-youtube-data-pipeline-with-aws-and-apache-airflow-e5d3b11de9c2&user=Aashish+Nair&userId=3087ba81e065&source=post_page-3087ba81e065----e5d3b11de9c2---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e5d3b11de9c2--------------------------------) ·12分钟阅读·2023年4月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe5d3b11de9c2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-a-youtube-data-pipeline-with-aws-and-apache-airflow-e5d3b11de9c2&user=Aashish+Nair&userId=3087ba81e065&source=-----e5d3b11de9c2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe5d3b11de9c2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-a-youtube-data-pipeline-with-aws-and-apache-airflow-e5d3b11de9c2&source=-----e5d3b11de9c2---------------------bookmark_footer-----------)![](../Images/e59f2354c5c004a43224decca7d5b946.png)

图片由 Artem Podrez 提供: [https://www.pexels.com/photo/thoughtful-woman-with-earbuds-using-laptop-4492161/](https://www.pexels.com/photo/thoughtful-woman-with-earbuds-using-laptop-4492161/)

∘ [介绍](#d514)

∘ [问题陈述](#615d)

∘ [理解数据管道](#c674)

∘ [理解数据](#c793)

∘ [第一阶段——设置 AWS 环境](#d201)

∘ [第二阶段——使用 Boto3 促进 AWS 操作](#bd0e)

∘ [第三阶段——设置 Airflow 管道](#0497)

∘ [第四阶段——运行管道](#2a4f)

∘ [检查结果](#66c0)

∘ [此数据管道的优势](#1a83)

∘ [此数据管道的劣势](#b332)

∘ [结论](#62b5)

## 介绍

YouTube 已成为信息、思想和创意的主要交流平台，每天平均上传 300 万个视频。这个视频流媒体平台始终为观众准备了新的讨论话题，其内容涵盖从严肃的新闻故事到轻松的音乐视频。

话虽如此，随着视频内容的不断涌入，很难评估哪种类型的内容能够吸引观众的注意。
