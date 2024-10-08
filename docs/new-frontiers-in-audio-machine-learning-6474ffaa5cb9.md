# 音频机器学习的新领域

> 原文：[`towardsdatascience.com/new-frontiers-in-audio-machine-learning-6474ffaa5cb9?source=collection_archive---------9-----------------------#2023-04-20`](https://towardsdatascience.com/new-frontiers-in-audio-machine-learning-6474ffaa5cb9?source=collection_archive---------9-----------------------#2023-04-20)

[](https://towardsdatascience.medium.com/?source=post_page-----6474ffaa5cb9--------------------------------)![TDS Editors](https://towardsdatascience.medium.com/?source=post_page-----6474ffaa5cb9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6474ffaa5cb9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6474ffaa5cb9--------------------------------) [TDS Editors](https://towardsdatascience.medium.com/?source=post_page-----6474ffaa5cb9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7e12c71dfa81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnew-frontiers-in-audio-machine-learning-6474ffaa5cb9&user=TDS+Editors&userId=7e12c71dfa81&source=post_page-7e12c71dfa81----6474ffaa5cb9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6474ffaa5cb9--------------------------------) · 发送 新闻简报 · 阅读时间 3 分钟 · Apr 20, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6474ffaa5cb9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnew-frontiers-in-audio-machine-learning-6474ffaa5cb9&user=TDS+Editors&userId=7e12c71dfa81&source=-----6474ffaa5cb9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6474ffaa5cb9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnew-frontiers-in-audio-machine-learning-6474ffaa5cb9&source=-----6474ffaa5cb9---------------------bookmark_footer-----------)

不久以前，任何涉及处理音频文件的工作流程——甚至是像转录播客剧集这样相对简单的任务——都伴随着一系列艰难的选择。你可以选择手动操作（在过程中浪费数小时甚至数天的时间），依赖于几款笨拙且最终令人失望的应用程序，或者拼凑出类似于弗兰肯斯坦怪物的工具和代码组合。

那些日子已经过去。强大的模型和易于访问的 AI 界面的兴起使得处理音频和音乐变得更加高效，新的视野每天都在不断开启。为了帮助您跟上音频聚焦机器学习的最新进展，我们从过去几周收集了一些突出的文章，涵盖了各种方法和用例。过滤掉噪音，深入了解吧！

+   **揭示音乐标签 AI 的黑箱**。随着每天在 Spotify 和 Apple Music 等平台上添加数千首歌曲，您是否曾想过这些服务如何知道为每首歌分配哪种音乐流派？[Max Hilsdorf](https://medium.com/u/d0c085a74ae8?source=post_page-----6474ffaa5cb9--------------------------------)的项目利用 Shapley 值确定特定乐器的存在如何影响 AI 系统标记新曲目的方式。

+   **探索基于深度学习的鸟鸣识别方法**。[Leonie Monigatti](https://medium.com/u/3a38da70d8dc?source=post_page-----6474ffaa5cb9--------------------------------)最近的贡献涵盖了去年的 BirdCLEF2022 Kaggle 竞赛，参赛者需创建鸟鸣录音的分类器。Leonie 向我们展示了一种巧妙的方法，将音频波形转换为梅尔频谱图，使深度学习模型可以像处理图像一样处理它们。

![](img/bd4b9d5ac6e5d107fb089b0520914472.png)

图片由[Oskars Sylwan](https://unsplash.com/@oskarssylwan?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

+   **从长 YouTube 视频中自动生成摘要**。如果您是一个完美主义者，您会欣赏[Bildea Ana](https://medium.com/u/c57d3db39a47?source=post_page-----6474ffaa5cb9--------------------------------)使用 OpenAI 的 Whisper 模型和 Hugging Face 进行音频转录的简化流程，然后使用开源的 BART 编码器进行总结。您可以将此方法应用于自己的录音和语音备忘录，或者任何其他音频文件（前提是其所有者允许，当然——*始终*仔细检查您希望使用的数据的版权和许可状态）。

+   **将转录提升到一个新水平**。[Luís Roque](https://medium.com/u/2195f049db86?source=post_page-----6474ffaa5cb9--------------------------------)的最新项目与 Ana 的项目有相似之处，但有所不同。它也依赖于 Whisper 来转录音频文件，但随后通过部署 PyAnnotate 进行说话者分离，“即识别和区分不同说话者的语音的过程”。

你说“请不要停止音乐”？我们很乐意满足——这里是我们最近一些最喜欢的关于非音频相关主题的文章。请享用！

+   *“*学习神经网络不应该是解读误导性图示的练习，*”* [Aaron Master](https://medium.com/u/31905cfe67ce?source=post_page-----6474ffaa5cb9--------------------------------)和 Doron Bergman 表示，他们提出了一种建设性的新方法来创建更好、更准确的神经网络。

+   从推广设计到库存分析，[Idil Ismiguzel](https://medium.com/u/6d965c736f2?source=post_page-----6474ffaa5cb9--------------------------------) 展示了关联规则挖掘的力量：一种赋能数据专业人士发现数据集中的频繁模式的技术。

+   对于无监督学习和 K-means 聚类的动手实践，不要错过[Nabanita Roy](https://medium.com/u/d36a8b28c928?source=post_page-----6474ffaa5cb9--------------------------------)的最新教程，该教程专注于按颜色分组图像像素的使用案例。

+   如果你发现人工智能、政府监管和加拿大官僚主义的交集很吸引人（谁会不感兴趣呢？），[Mathieu Lemay](https://medium.com/u/f84a70d8f74?source=post_page-----6474ffaa5cb9--------------------------------)的深度剖析是你本周绝对不容错过的一篇文章。

+   随着合成数据在多个领域的作用不断发展（和增长），[Miriam Santos](https://medium.com/u/243289394aaa?source=post_page-----6474ffaa5cb9--------------------------------)的实用 CTGAN 生成合成数据指南依然时效性和实用性十足。

+   我们绝对不能在一整周内没有一个以 GPT 为主题的推荐；如果你还没读过，我们强烈推荐[Henry Lai](https://medium.com/u/d5548707b59?source=post_page-----6474ffaa5cb9--------------------------------)对这些备受欢迎的模型背后的数据驱动 AI 概念的概述。

感谢您本周收听《Variable》！如果您喜欢在 TDS 上阅读的文章，请考虑[成为 Medium 会员](https://bit.ly/tds-membership)——如果您是符合条件国家的学生，不要错过[享受会员大幅折扣](https://blog.medium.com/new-student-discounts-cc10e964495b)的机会。

下期《Variable》见，

TDS 编辑团队
