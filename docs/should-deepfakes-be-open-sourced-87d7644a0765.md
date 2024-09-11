# 深度伪造技术是否应该开源？

> 原文：[`towardsdatascience.com/should-deepfakes-be-open-sourced-87d7644a0765?source=collection_archive---------9-----------------------#2023-05-25`](https://towardsdatascience.com/should-deepfakes-be-open-sourced-87d7644a0765?source=collection_archive---------9-----------------------#2023-05-25)

## 意见

## 讨论了开放深度伪造技术的利弊

[](https://medium.com/@jacksaunders909?source=post_page-----87d7644a0765--------------------------------)![Jack Saunders](https://medium.com/@jacksaunders909?source=post_page-----87d7644a0765--------------------------------)[](https://towardsdatascience.com/?source=post_page-----87d7644a0765--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----87d7644a0765--------------------------------) [Jack Saunders](https://medium.com/@jacksaunders909?source=post_page-----87d7644a0765--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F24c6b2ceeccc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshould-deepfakes-be-open-sourced-87d7644a0765&user=Jack+Saunders&userId=24c6b2ceeccc&source=post_page-24c6b2ceeccc----87d7644a0765---------------------post_header-----------) 发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----87d7644a0765--------------------------------) ·5 分钟阅读·2023 年 5 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F87d7644a0765&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshould-deepfakes-be-open-sourced-87d7644a0765&user=Jack+Saunders&userId=24c6b2ceeccc&source=-----87d7644a0765---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F87d7644a0765&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshould-deepfakes-be-open-sourced-87d7644a0765&source=-----87d7644a0765---------------------bookmark_footer-----------)![](img/304a644260e00d49a236cba29efb0cc7.png)

图片生成使用了[DreamStudio](https://beta.dreamstudio.ai/generate)

我是一名博士研究人员，创建可以被认为是深度伪造的技术用于我的研究。我对创建逼真的数字双胞胎和提升娱乐水平的能力感到着迷。在开始我的研究之前，我曾认为这些模型可能会造成太多伤害，不适合向公众发布。在过去几个月里，我注意到越来越多的顶尖声音主张将开源软件作为人工智能领域的核心原则。虽然这次讨论几乎完全集中在大规模语言模型（LLMs）上，但我认为这一理念在整个领域中普遍存在。我完全支持几乎所有人工智能模型的开源，但对于我自己的研究领域，我不太确定。

> 几乎没有哪个领域的误用潜力像深度伪造技术那样高。

到目前为止，我的方法是走中间道路。尽量以一种不需要博士学位才能理解的水平来传达深度伪造技术的工作原理。然而，我经常怀疑这是否是正确的方法。**本文的目的是尝试启动关于我们深度伪造研究人员应采取方向的讨论。** 有鉴于此，本文讨论了开源的一些优缺点。

# 开源的优点

我们常常听到关于深度伪造技术的负面信息。有人可能会问，像我这样的研究人员为什么还要考虑创建开源模型。然而，实际上有很多合理的理由这样做：

+   **透明性**：对我来说，这是最重要的一点。如果大多数主要的深度伪造模型完全开源，那么它们就会变得透明。这样，监管者就可以理解他们正在处理的内容，其他研究人员也可以开发更好的检测算法。对于深度伪造技术来说，**那些希望利用它们进行恶意行为的“坏演员”和那些试图防止这种伤害的“好演员”之间将会有一场军备竞赛**。你可以确定“坏演员”无论我们是否发布我们的模型都会开发他们的深度伪造技术。在开源中，我们可以给“好演员”提供更多的数据，以构建他们的防害模型。

+   **公平性**：如果我们选择不进行开源，只有那些拥有人才和计算资源的机构才能创建深度伪造技术。根据经验，开发这些模型需要很长时间，没有开源软件的话，很少有人能做到这一点。这可能进一步将权力集中在已经强大的手中。深度伪造技术可以在多个市场中使用，并且可能具有数十亿美元的潜在价值。例如，[仅配音市场就预计超过 35 亿美元](https://www.marketwatch.com/press-release/2023-film-dubbing-market-research-report-2030-2023-05-24#:~:text=The%20global%20Film%20Dubbing%20market,USD%205339.61%20million%20by%202028.)。如果只有像谷歌这样的公司能够创建深度伪造技术，那么只有谷歌这样的公司才能从中获益。

+   **意识：** 深度伪造技术正在迅速发展。我们很可能很快会达到这样一个阶段，即你无法相信在线上看到的任何视频，除非它以其他方式经过验证。虽然很多人对此有模糊的认识，但我认为很少有人真正理解其含义。作为深度伪造研究人员，我们有责任帮助教育公众。我们需要真正鼓励每个人保持良好的数字怀疑态度，检查他们在线上看到的任何媒体的来源，并质疑其真实性。开源软件有所帮助。当模型可以在网上自由获取时，教育变得更加容易。如果你能自己创建深度伪造，你自然会留意其他人的伪造。

# 缺点

当然，将深度伪造模型开源存在许多潜在的缺点，从明显的到更微妙的都有。

+   **人们将滥用它们：** 无论我们如何监管或检测模型有多么有效，总会有一小部分人会出于最糟糕的理由使用深度伪造。从[报复色情](https://www.technologyreview.com/2021/02/12/1018222/deepfake-revenge-porn-coming-ban/)到[虚假信息](https://www.nytimes.com/2023/02/07/technology/artificial-intelligence-training-deepfake.html)，这项技术有一些非常恶劣的应用。如果我们开源模型，就会使所有人更容易访问它们，这无疑会造成伤害。确实，一些坏人无论如何都会做到这一点，尤其是大型犯罪或国家组织，但大多数寻求伤害的人可能本来无法做到，如果没有开源模型的话。

+   **保护措施可能被移除：** 保护深度伪造技术不被滥用的较好方法之一是引入保护措施。特别是，大多数创建深度伪造的团队都使用水印技术。水印涉及将数据添加到创建的视频中，以一种对人类和大多数软件都不可见但可以被拥有“密钥”的人轻松识别的方式。这意味着，例如，YouTube 或 Twitter 可以快速检测到视频是否由深度伪造平台创建，并将其移除。由于水印只能被那些获得了这个秘密密钥的人看到，坏人无法移除它们。如果我们开源深度伪造生成，那么**坏人将可以简单地跳过添加水印。** 这使得深度伪造变得不可检测。

+   **单向共享：** 如果我们再次考虑所谓的好人和坏人之间的军备竞赛，那么我们可以看到开源的另一个缺点。如果我们这些好人开源了我们所有的软件，那么坏人可以在此基础上进行构建。另一方面，坏人不会开源他们的模型，这意味着信息只是在一个方向上共享。这给坏人带来了显著的优势。

# 总结

正如所见，这不是一个容易回答的问题。利弊权衡很多，无论哪种情况，潜在的危害都很大。在写这篇文章的过程中，我进行了许多对话。让我感到惊讶的一点是开源绝对主义者的数量。我经常听到的一个论点是，深伪技术已经存在，不可能把魔鬼收回瓶子。许多人，包括我自己在内，都认为，我们将迎来一个深伪技术与现实难以区分的时代。如果到那时我们都没有足够的意识去质疑这些技术，我们可能会面临很大麻烦，因为坏分子可能在未被察觉的情况下行动。这是一个我认为[最近对 AI 发展暂停的呼吁忽视的点](https://www.bbc.co.uk/news/uk-65746524)。然而，虽然开源可能会减少长期的危害，但它也为那些可能想要滥用深伪技术但没有技术能力的人打开了短期危害的大门。

尽管我对开源问题仍然未作决定，但我比以往任何时候都更自信地认为，这个讨论是必须进行的，至少深伪技术的研究需要在公开的环境下进行，并且要向公众传达。***我强烈鼓励每个人发表意见。如果你有我没有涉及到的看法或任何问题，请留下评论或直接联系我，我真的希望听到尽可能多的人反馈。***
