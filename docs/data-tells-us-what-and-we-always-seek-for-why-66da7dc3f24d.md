# 数据告诉我们“是什么”，而我们总是寻求“为什么”

> 原文：[`towardsdatascience.com/data-tells-us-what-and-we-always-seek-for-why-66da7dc3f24d`](https://towardsdatascience.com/data-tells-us-what-and-we-always-seek-for-why-66da7dc3f24d)

![](img/3fc0370d74940fa3b1e5134b1b8d6768.png)

[照片由凯莉·西克马](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上拍摄

## 《为什么的书》第一章和第二章，“与我共读”系列

[](https://zzhu17.medium.com/?source=post_page-----66da7dc3f24d--------------------------------)![朱子静，博士](https://zzhu17.medium.com/?source=post_page-----66da7dc3f24d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----66da7dc3f24d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----66da7dc3f24d--------------------------------) [朱子静，博士](https://zzhu17.medium.com/?source=post_page-----66da7dc3f24d--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----66da7dc3f24d--------------------------------) ·9 分钟阅读·2023 年 11 月 8 日

--

在我的[上一篇文章](https://medium.com/towards-data-science/read-with-me-a-causality-book-club-edd7085d6ae6)中，我启动了“与我共读”书友会，探索犹太·珀尔的《为什么的书》。我想感谢每一位表现出兴趣并[报名参加](https://zzhu17.medium.com/subscribe)俱乐部的朋友。我希望我们能通过一起阅读和分享见解，深入理解因果关系。两周后，正如承诺的，我将分享我从前两章中获得的一些关键点。

在这两章中，犹太·珀尔首先解释了因果性阶梯，并回顾了因果理论的发展历史。我们将进一步深入探讨这三个层级。

![](img/5e1bc28afe3db4e14a91410405b05b03.png)

因果性阶梯源自于犹太·珀尔

# 第一层级：关联

回到 1800 年，从 Galton 到 Pearson，当他们试图理解人类如何继承遗传特征时，他们发现相关性在科学上是足够的。毕竟，“***数据即科学的一切***”。对他们来说，因果关系只是相关性的一种特殊情况，永远无法证明。另一方面，相关性足够强大，可以解释为什么较高父亲的儿子比人口平均值更高。基于相关性的预测模型通过识别最具预测性的变量来对目标进行预测，即使在许多情况下这可能没有意义。例如，一个国家的人均***巧克力消费量***与其***诺贝尔奖获奖者***人数之间有强烈的相关性。显然，吃更多巧克力不会增加你获得诺贝尔奖的机会，而一个国家的财富更可能是这里的混杂因素。我们可以找到很多类似的例子，这些例子并未提供有意义和科学的信息。当面对这些发现时，Pearson 将其视为“**虚假的**”相关性。

除了“虚假的”相关性外，还常常发现总体中的相关性在子群体中发生反转。例如，在测量颅骨长度和宽度之间的相关性时，单独在男性和女性群体中测量时，相关性微不足道。然而，合并性别组时，这种相关性是显著的。这种情况在存在混杂因素时很常见，我们称之为辛普森悖论。

[](https://zzhu17.medium.com/the-so-called-simpsons-paradox-6d0efdca6fdc?source=post_page-----66da7dc3f24d--------------------------------) [## 被称为“辛普森悖论”

### 我们因为失去而变得更好

zzhu17.medium.com](https://zzhu17.medium.com/the-so-called-simpsons-paradox-6d0efdca6fdc?source=post_page-----66da7dc3f24d--------------------------------)

尽管存在这些不足，基于相关性的预测模型在静态设置中仍能取得不错的准确度。统计学家们开发了许多复杂的模型来从数据中提取洞见。随着数据样本的增加和算法的复杂化，我们可以构建出不错的模型来预测销售、客户流失/保留等。然而，当面对新情况时，基于相关性的模型由于缺乏历史数据，无法做出可信的预测。为了保持良好的性能，模型开发者必须不断地向模型提供越来越多的数据，明确涵盖所有现有情况，这总是滞后于新的设置。正如 Pearl 所写：

> **任何在因果关系阶梯的第一级运作的系统都不可避免地缺乏灵活性和适应性。**

# 第 2 层：干预

超越“看到它是什么”，我们正在向“改变它是什么”迈进。如果我提高这个产品的价格，这个产品的销售会有什么变化？如果我停止给这位客户优惠，他还会继续订阅吗？第 2 层的问题不能再通过基于相关性的模型来回答。例如，当我们使用观察数据找出价格和销售之间的过去相关性时，这可能会受到宏观经济情况、其他产品的销售和价格等因素的偏倚。观察数据显示的是许多因素对销售的组合影响，而不是价格对销售的纯粹影响。这些因素中的一些可以被观察和量化，但有些很难量化，这使我们无法获得纯粹的影响。

第一个到达第 2 层的研究者是塞沃尔·赖特，他绘制了第一个路径图，展示了导致豚鼠毛色的因素。该图不仅列出了所有因素，还指导了如何估计每个因素的强度。要回答第 2 层的问题，除了第 1 层收集的数据，我们还需要因果假设，如赖特的路径图。下图展示了赖特提出的因果图，用于估计豚鼠的出生体重。

![](img/1f67f2e65d67af79dcf65de0840d1987.png)

作者提供的图像，参见《为什么的书》

因果图的好处在于我们可以隔离每个因素的影响。例如，孕期对出生体重的纯粹影响是什么？即使有些因素我们无法量化或未被观察到，我们可以提出与不可量化因素密切相关的其他因素，并在因果图中适当地估计它们的影响。工具变量的概念源于此。

![](img/88d49c17798caf0b990909761256e0de.png)

作者提供的图像

然而，尽管赖特在 1920 年代提出了这种因果图的想法，但这个理论从未被主流接受。朱迪亚将这种情况与伽利略声称地球围绕太阳旋转的情况进行了比较。反对意见来自于“***数据在所有情况下优于意见和解释***”这一规范。毕竟，数据是客观的，而意见是主观的。然而，正如朱迪亚所写：

> **关于因果关系，一点智慧的主观性比任何量的客观性更能揭示现实世界。**

事情直到 1960 年代才开始发生变化，当时经济学家和社会学家开始使用结构方程模型（SEM）。这些模型将因果关系嵌入线性代数中。在结构方程中，有些变量被指定为内生变量，这些变量与目标因果相关，而其他变量被称为外生变量。包括内生变量有助于解决绘制因果结论时的混淆问题。唯一的缺点是这些模型中存在的因果关系是非方向性的。因此，它仍然无法解决像“是否 y 导致 x 而不是 x 导致 y”这样的问题。

![](img/723d45d0e0d4e06a71d42ed9b374fc11.png)

图片由[马库斯·乌尔本茨](https://unsplash.com/@marcusurbenz?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

在突破数据主导的理论中的另一步进展是贝叶斯理论的出现，它提供了一种将观察数据与主观先验知识相结合的客观方法。贝叶斯统计以主观的先验信念开始，例如硬币不公平，然后通过收集新证据——多次抛掷硬币——不断修正信念。如果我们看到正面大约出现一半的时间，则修改信念认为硬币应该是公平的。由于主观性的包含，因果理论从贝叶斯概率开始，然后绕了一大圈，这个故事将在接下来的章节中分享。

# 第 3 层：反事实

信不信由你，回到我们的智人祖先，他们在计划猛犸象狩猎时，已经在心中有一个关于狩猎成功率的心理因果模型：

![](img/4b73d0b1edbb82427bb57bc821716853.png)

作者所指的图片参见《为什么的书》。

心理模型展示了成功率的不同原因，这就是***想象力***发生的地方。它帮助我们的祖先通过对模型进行局部调整来尝试不同的情境。例如，如果我们增加猎人的数量，成功率能提高多少？如果猛犸象的体型比平常大，我们需要增加多少猎人以保持相同的成功率？想象力的能力使我们达到了因果关系的最高层级，即第 3 层。

尤瓦尔·赫拉利在他的《人类简史》中定义了**认知革命**，它表现为描绘虚构生物的能力。[斯塔德尔洞穴的狮子人](https://www.britishmuseum.org/blog/lion-man-ice-age-masterpiece)雕像由考古学家发现，展示了智人新发展出的能力，想象一个不存在的“半人半狮”的生物。想象能力使人类与其他生物区别开来，就像制造反事实将人类智力与动物和人工智能区分开来一样。

![](img/b3b7df446f6b89a4a9f244c4e33633dd.png)

图片来源于 [Mohamed Nohassi](https://unsplash.com/@coopery?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

反事实与数据之间的关系尤其复杂，因为数据是事实，而反事实是想象。数据无法告诉我们在一个某些已观察事实被简单否定的虚拟世界中会发生什么。另一方面，因果模型可以通过帮助我们推理可能发生的事情来引导我们。在商业世界中，这些“如果”代表了最具盈利性的问题——如果我提高了价格，产品销售会发生什么？正确回答这些问题将指导我们自信地做出任何决策。

数据告诉我们“什么”，而我们找出“为什么”。我们大多数人都听说过《创世纪》中提到的伊甸园。当上帝发现亚当藏在园中时，发生了以下对话：

> **上帝：** 你吃了我禁戒的那棵树上的果子吗？
> 
> **亚当：** 你给我做伴侣的女人，她给了我树上的果子，我就吃了。
> 
> **上帝（对夏娃）：** 你做了什么？
> 
> **夏娃：** 蛇欺骗了我，我就吃了。

上帝问 ***什么***，但他们回答 ***为什么***。上帝要求 ***事实***，但他们回答 ***解释。*** 他们的直觉是，命名原因会缓解他们行为的后果。数据是客观的事实，人类主观地解释这些数据以得出结论并指导进一步的行动。**寻找“为什么”在“什么”背后的意义** 是人类智慧的本能，而这正是人工智能所缺乏的。没有因果结构——即人类主观思维过程的紧凑表示——人工智能无法具备人类水平的智慧。没有这些，人工智能模型仅仅是在获得类似动物的能力，就像我在我的 [上一篇文章](https://medium.com/towards-data-science/read-with-me-a-causality-book-club-edd7085d6ae6) 中给出的猫的例子。

这就是我想分享的《为何的书》第一个和第二个章节的内容。第一章和第二章通过介绍因果性阶梯和整个理论的发展历史奠定了书的基础。我预见到后续章节将包含更多技术细节和行业应用。别忘了 [订阅我的邮件列表](https://zzhu17.medium.com/subscribe) 来参与后续讨论：

+   [**第三章和第四章：因果图：面对观察数据中的阿基琉斯之踵**](https://medium.com/towards-data-science/causal-diagram-confronting-the-achilles-heel-in-observational-data-a69cdb1c4818)

+   [**第五章和第六章：为何理解数据生成过程比数据本身更重要**](https://medium.com/towards-data-science/why-understanding-the-data-generation-process-is-more-important-than-the-data-itself-f1b3b847e662)

+   [**第七章和第八章：你不能踏进同一条河流两次**](https://medium.com/towards-data-science/you-cant-step-in-the-same-river-twice-cfacf7cee305)

+   [**第九章和第十章：什么使人工智能强大**](https://medium.com/towards-data-science/what-makes-a-strong-ai-012315722793)

+   **附加内容：因果推断在学术界和工业界有什么不同？**

我分享的内容还有很多细节。像往常一样，我非常鼓励你阅读、思考并分享你在这里或在你的[个人博客文章](https://youtu.be/oFDl0-SKAL8?si=r1QiRIizhDBdvX-A)中的主要收获。

感谢阅读。如果你喜欢这篇文章，请不要忘记：

+   ***查看我最近的文章：*** [***数据讲故事中的 4D：将科学变成艺术***](https://medium.com/towards-data-science/the-4ds-in-data-storytelling-making-art-out-of-science-c4998ed7875e); ***数据科学中的持续学习******;*** ***我如何成为数据科学家******;***

+   ***查看我*** [***其他文章***](https://zzhu17.medium.com/my-blog-posts-gallery-ac6e01fe5cc3) ***，涵盖不同主题，如*** [***数据科学面试准备***](https://zzhu17.medium.com/list/data-science-interview-preparation-bfb0986a61a3)***;*** ***因果推断******;***

+   [***订阅***](https://zzhu17.medium.com/subscribe) ***我的邮件列表；***

+   [***注册 medium 会员***](https://zzhu17.medium.com/membership)***;***

+   ***或者关注我的*** [***YouTube***](https://youtube.com/channel/UCMs6go1pvY5OOy1DXVtMo5A) ***并观看我关于在家工作的数据科学家的最新 YouTube 视频：***

+   ***或其他我阅读的书籍：***

# 参考

[***《为什么的书》***](https://www.amazon.com/Book-Why-Science-Cause-Effect/dp/046509760X) ***由朱迪亚·珀尔撰写***

***因果性阶梯照片：***

*[1] 机器人照片由* [*Rock’n Roll Monkey*](https://unsplash.com/@rocknrollmonkey?utm_source=medium&utm_medium=referral) *提供，* [*Unsplash*](https://unsplash.com/?utm_source=medium&utm_medium=referral)*上*；

*[2] 猫照片由* [*Raoul Droog*](https://unsplash.com/@raouldroog?utm_source=medium&utm_medium=referral) *提供，* [*Unsplash*](https://unsplash.com/?utm_source=medium&utm_medium=referral)*上*；

*[3] 干预照片由* [*British Library*](https://unsplash.com/@britishlibrary?utm_source=medium&utm_medium=referral) *提供，* [*Unsplash*](https://unsplash.com/?utm_source=medium&utm_medium=referral)*上*；

*[4] 人类下棋照片由* [*JESHOOTS.COM*](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral) *提供，* [*Unsplash*](https://unsplash.com/?utm_source=medium&utm_medium=referral)*上*；
