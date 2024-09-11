# 五月刊：城市空间的数据

> 原文：[`towardsdatascience.com/may-edition-the-data-of-urban-spaces-815831aaf749?source=collection_archive---------8-----------------------#2023-05-03`](https://towardsdatascience.com/may-edition-the-data-of-urban-spaces-815831aaf749?source=collection_archive---------8-----------------------#2023-05-03)

## [月刊](https://towardsdatascience.com/tagged/monthly-edition)

## 数据如何帮助我们理解城市

[](https://towardsdatascience.medium.com/?source=post_page-----815831aaf749--------------------------------)![TDS 编辑](https://towardsdatascience.medium.com/?source=post_page-----815831aaf749--------------------------------)[](https://towardsdatascience.com/?source=post_page-----815831aaf749--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----815831aaf749--------------------------------) [TDS 编辑](https://towardsdatascience.medium.com/?source=post_page-----815831aaf749--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7e12c71dfa81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmay-edition-the-data-of-urban-spaces-815831aaf749&user=TDS+Editors&userId=7e12c71dfa81&source=post_page-7e12c71dfa81----815831aaf749---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----815831aaf749--------------------------------) ·4 分钟阅读·2023 年 5 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F815831aaf749&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmay-edition-the-data-of-urban-spaces-815831aaf749&user=TDS+Editors&userId=7e12c71dfa81&source=-----815831aaf749---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F815831aaf749&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmay-edition-the-data-of-urban-spaces-815831aaf749&source=-----815831aaf749---------------------bookmark_footer-----------)![](img/7df2a4ec132901503d39f4b29e8d62b5.png)

图片由 [Lennart Jönsson](https://unsplash.com/@lenjons?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

有时候很难不将城市视为我们需要解决的问题集合：住房、食品安全和环境影响都会浮现在脑海中。今年早些时候，我们强调了数据专业人士当前如何通过利用气候和地理空间数据的创新方法来应对这些挑战。

城市远不止其痛点的总和：它们反映了社会价值（善的、恶的和模糊的），激发创造力和联系，并作为不断演变的文化的活跃存储库。

本月，我们邀请您探索城市空间的丰富性和复杂性。从公交车等待时间到我们穿越的街道名称，我们选择的作者们将他们的数据技能应用于城市生活的具体片段，使我们能够更深入地理解城市环境的运作方式。

在我们深入探讨之前，我们想感谢您一如既往地支持我们发布的工作。如果您希望做出有意义的贡献，[考虑成为 Medium 会员](https://bit.ly/tds-membership)；如果您是符合条件国家的学生，现在可以[享受实质性折扣](https://blog.medium.com/new-student-discounts-cc10e964495b)。

[TDS 编辑](https://medium.com/u/7e12c71dfa81?source=post_page-----815831aaf749--------------------------------)

## TDS 编辑精选

+   [**街道名称中的隐藏模式：数据科学故事 [第一部分]**](/hidden-patterns-in-street-names-a-data-science-story-part-1-82c8dd130693) （2023 年 1 月，6 分钟）

    我们很少质疑谁应该以其名字命名街道，谁可能在这一过程中被排除。[Dea Bardhoshi](https://medium.com/u/d61c58ba988e?source=post_page-----815831aaf749--------------------------------)的迷人数据分析项目是对这种常见漠视形式的有效解药：它研究了阿尔巴尼亚地拉那街道名称的性别分布，并揭示了许多有趣的发现（有些比其他的更可预测）。

+   **德国住房租赁市场：使用 Python 进行探索性数据分析** （2023 年 4 月，27 分钟）

    随着全球许多城市的住房危机持续存在，围绕可靠数据构建解决方案导向的对话至关重要。[Dmitrii Eliuseev](https://medium.com/u/65c1f6ba75db?source=post_page-----815831aaf749--------------------------------)对德国租赁市场的深入分析提供了一条强有力的分析路线图，可以服务于租户和政策制定者。

+   **使用 GeoPy 和 Folium 绘制黑人拥有的企业地图** （2021 年 1 月，5 分钟）

    来自边缘化社区的城市居民经常面临长期的排斥和隔离历史。[Avonlea Fisher](https://medium.com/u/ca492a9277a8?source=post_page-----815831aaf749--------------------------------)的项目旨在解决 COVID-19 对波士顿黑人拥有企业产生的巨大经济影响；两年后，它仍然可以激励数据科学家寻找对超本地问题的超本地答案。

+   **公交车在哪里？GTFS 将告诉我们！** （2023 年 1 月，15 分钟）

    [Leo van der Meulen](https://medium.com/u/a051ea9a35df?source=post_page-----815831aaf749--------------------------------)的端到端教程的基本前提很难反驳：“公共交通与开放数据的结合具有巨大的潜力。” 将数据洪流转换为一个面向用户的界面，以告知乘客下一班公交车何时到达需要付出相当大的努力，但这个过程既有趣又可能扩展到其他背景和地点。

+   **数字文本分析：荷兰语地区的街头诗歌** （2021 年 11 月，5 分钟）

    地理空间数据、文本分析和诗歌在 [Emma-Sophia Nagels](https://medium.com/u/8e706ac53242?source=post_page-----815831aaf749--------------------------------)的帖子中结合在一起，该帖子追踪了在荷兰及荷兰语区比利时编制、绘制和分析公开展示的诗歌的过程。

+   **通过地理空间关联规则挖掘发现便利店位置模式** （2023 年 3 月，7 分钟）

    东京的便利店可谓传奇：无处不在，随时可达，且充满了世界上最好的一些小吃。[Elliot Humphrey](https://medium.com/u/13e1322246bb?source=post_page-----815831aaf749--------------------------------)超越了表面，试图检测商店位置中的模式，以推测出一个看似随意甚至混乱的现象背后的商业策略。

## 原创特色

探索我们最新的资源和阅读推荐。

+   **音频机器学习的新前沿**

    我们精心策划了一系列强大的文章，涵盖了新模型、AI 接口和应用程序的崛起，这些使得处理音频和音乐的工作变得更加高效。

+   **“最佳实践”究竟意味着什么？** 我们收集的关于工作流程优化的亮点帖子，从更好的绘图到更有效的实验。

## 热门帖子

如果你错过了，这里是上个月在 TDS 上最受欢迎的一些帖子。

+   **零 ETL、ChatGPT 与数据工程的未来** 由 [Barr Moses](https://medium.com/u/2818bac48708?source=post_page-----815831aaf749--------------------------------)

+   **时间序列预测：深度学习 vs 统计学 — 谁胜出？** by [Nikos Kafritsas](https://medium.com/u/bec849d9e1d2?source=post_page-----815831aaf749--------------------------------)

+   **6 Python 最佳实践，区分高级开发者和初学者的关键** by [Tomer Gabay](https://medium.com/u/c9c352dba00a?source=post_page-----815831aaf749--------------------------------)

+   **如何将公司文档转变为可搜索的数据库：使用 OpenAI** by [Jacob Marks, Ph.D.](https://medium.com/u/f7dc0c0eae92?source=post_page-----815831aaf749--------------------------------)

+   **你需要了解的 4 种自主 AI 代理** by [Sophia Yang](https://medium.com/u/ae9cae9cbcd2?source=post_page-----815831aaf749--------------------------------)

+   **Pandas 2.0 有哪些新特性？** by [Jeff Hale](https://medium.com/u/451599b1142a?source=post_page-----815831aaf749--------------------------------)

我们非常高兴在三月份迎来了新一批 TDS 作者——他们包括[Matt Collins](https://medium.com/u/d1970f1605f1?source=post_page-----815831aaf749--------------------------------)、[Krzysztof Pałczyński](https://medium.com/u/7c74555dd91c?source=post_page-----815831aaf749--------------------------------)、[Colin Horgan](https://medium.com/u/8d3875046cb?source=post_page-----815831aaf749--------------------------------)、[Victor Graff](https://medium.com/u/6802a7b0402e?source=post_page-----815831aaf749--------------------------------)、[Dr. Roi Yehoshua](https://medium.com/u/3886620c5cf9?source=post_page-----815831aaf749--------------------------------)、[Mark Chen](https://medium.com/u/377682c0f342?source=post_page-----815831aaf749--------------------------------)、[Bernardo Furtado](https://medium.com/u/220a6eda5891?source=post_page-----815831aaf749--------------------------------)、[Toon Beerten](https://medium.com/u/3aef462e13b5?source=post_page-----815831aaf749--------------------------------)、[Peng Qian](https://medium.com/u/8e2fe735546d?source=post_page-----815831aaf749--------------------------------)、[Edozie Onyearugbulem](https://medium.com/u/cbd576093dd8?source=post_page-----815831aaf749--------------------------------)、[Willem Koenders](https://medium.com/u/a754b81446b6?source=post_page-----815831aaf749--------------------------------)、[Aaron Master](https://medium.com/u/31905cfe67ce?source=post_page-----815831aaf749--------------------------------)和 Doron Bergman、[Lingjuan Lyu](https://medium.com/u/ca2f89d83dfb?source=post_page-----815831aaf749--------------------------------)、[Jacob Marks, Ph.D.](https://medium.com/u/f7dc0c0eae92?source=post_page-----815831aaf749--------------------------------)、[Anthony Mensier](https://medium.com/u/f8ca1fd30f6b?source=post_page-----815831aaf749--------------------------------)、[Francisco Caio Lima Paiva](https://medium.com/u/b20176e45fd4?source=post_page-----815831aaf749--------------------------------)、[Abhi Sawhney](https://medium.com/u/42ce11c2a627?source=post_page-----815831aaf749--------------------------------)、[Chris Mauck](https://medium.com/u/96a38f7ac238?source=post_page-----815831aaf749--------------------------------)、[Massimiliano Costacurta](https://medium.com/u/233cb43234c3?source=post_page-----815831aaf749--------------------------------)、[Lee Vaughan](https://medium.com/u/5d604015c08b?source=post_page-----815831aaf749--------------------------------)以及[Davide Caffagni](https://medium.com/u/b628be49d740?source=post_page-----815831aaf749--------------------------------)等人。如果你有有趣的项目或想法要与我们分享，[我们非常愿意听取你的意见](http://bit.ly/write-for-tds)!

下个月见。
