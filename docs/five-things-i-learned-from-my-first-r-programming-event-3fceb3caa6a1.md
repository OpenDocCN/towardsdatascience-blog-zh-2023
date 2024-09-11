# 我从第一次 R 编程活动中学到的五件事

> 原文：[https://towardsdatascience.com/five-things-i-learned-from-my-first-r-programming-event-3fceb3caa6a1?source=collection_archive---------6-----------------------#2023-05-11](https://towardsdatascience.com/five-things-i-learned-from-my-first-r-programming-event-3fceb3caa6a1?source=collection_archive---------6-----------------------#2023-05-11)

## 从 SatRDays London 学到的关于 R、数据科学和吸引观众的经验

[](https://roryspanton.medium.com/?source=post_page-----3fceb3caa6a1--------------------------------)[![Rory Spanton](../Images/6c35a3de7cb516aac09bc5cf417a6c70.png)](https://roryspanton.medium.com/?source=post_page-----3fceb3caa6a1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3fceb3caa6a1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3fceb3caa6a1--------------------------------) [Rory Spanton](https://roryspanton.medium.com/?source=post_page-----3fceb3caa6a1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39501aa8ce39&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffive-things-i-learned-from-my-first-r-programming-event-3fceb3caa6a1&user=Rory+Spanton&userId=39501aa8ce39&source=post_page-39501aa8ce39----3fceb3caa6a1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3fceb3caa6a1--------------------------------) ·8分钟阅读·2023年5月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3fceb3caa6a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffive-things-i-learned-from-my-first-r-programming-event-3fceb3caa6a1&user=Rory+Spanton&userId=39501aa8ce39&source=-----3fceb3caa6a1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3fceb3caa6a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffive-things-i-learned-from-my-first-r-programming-event-3fceb3caa6a1&source=-----3fceb3caa6a1---------------------bookmark_footer-----------)![](../Images/20b8f1552ec8009b0748f6941c7fcf39.png)

图片由 [Teemu Paananen](https://unsplash.com/@xteemu?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

上个月，我做了一件以前从未做过的事情。我参加了一个关于数据科学的线下活动——具体来说，是在 R 编程语言中进行数据科学。

我对会议并不陌生。作为心理学研究员，我参加并在许多相关领域的会议上做过报告。但尽管我是一个长期的 R 爱好者，我从未有机会参加关于数据分析的非学术会议。因此，当我有机会参加 SatRDays 伦敦，这个为期一天的专注于 R 的活动时，我毫不犹豫地报名参加了。

这是一个很棒的决定。我学到了很多关于 R 及其在许多行业中的应用的知识，其中一些是我以前从未听说过的。我还与许多对 R 像我一样痴迷的人交流，这真的很令人愉快。

这是我从 SatRDays 学到的五个最强大的东西之一。

# 1. R 的用途比我想象的要多

以为你知道 R 能做所有事情？再想想。

我有时听到数据科学家将 R 贬低为一个只有少数用途的专业语言。他们说，如果你不从事生物信息学、学术研究或硬核统计学，你所在的领域里没有人使用 R。Python 是一个通用的、“万能”的语言，你应该改用它。

在某种程度上，这是正确的——像 Python 这样的其他语言比 R 更广泛使用，并且在自身领域中也很有价值。但这并不意味着 R 不能在各个领域完成许多重要的任务。

SatRDays 的讲座范围令人难以置信。每位讲者来自不同的公司，展示了他们在行业中如何使用 R 进行数据分析的新颖而有趣的方法。

我听了数据记者、金融审计师、互联网性能分析师、空气质量专家以及更多人分享他们与 R 的经验。作为一个学术人员，这些领域中的一半在会议之前甚至不在我的雷达上。听到这些人的讲述让我意识到 R 在各种商业环境中的广泛用途。

当然，R 语言的工作机会没有 Python 语言那么多。但不要让任何人告诉你学习 R 是把自己逼入绝境。越来越多的公司开始采纳 R，并且有许多新的应用场景值得期待。

# 2. 良好的 R 代码将计算与操作分开

这是我从 [Russ Hyde](http://russ-hyde.rbind.io/) 关于良好编码实践的讲座中学到的一个提示。对于经验丰富的开发者来说，这可能是熟悉的，但对我来说却是新颖而有用的。

在编程时，通常最好将你的代码分成用户定义的函数。这有助于避免脚本中的重复，并确保代码是可重用和易于维护的。但，有些将代码拆分成不同函数的方法比其他方法更好。

比如，考虑一个简短的脚本，它读取一些数据，清理这些数据，然后将其写入一个新文件。这个脚本包含许多步骤，但我们可以将每个步骤分为两类：计算和操作。

```py
read_csv("sales_data.csv") %>%
  select(date, transaction_id, category, item_price) %>%
  filter(date == "2023-05-10") %>%
  group_by(category) %>%
  summarise(day_turnover = sum(item_price)) %>%
  write_csv("sales_summaries/day_turnover_2023-05-10.csv")
```

计算是一步步的操作，给定一定的输入，每次都会返回相同的输出。过滤数据集和计算摘要统计量等操作就是很好的例子。因为它们返回可预测的结果且没有任何副作用，所以很容易编写自动化测试来确认它们的正常工作。

对比起来，动作要难测试得多。它们会产生副作用，比如将数据写入文件，这些副作用更难控制。它们还可能受到全局环境中的随机数生成器（RNG）或其他变量的影响。这使得测试它们变成一项挑战，通常需要使用虚拟文件或受控环境。

提示是：尽可能地将动作与计算分离。

这里有一个使用我之前介绍的代码的例子。与其在一个块中包含动作和计算，不如将它们分成两个函数。所有读写数据的动作都包含在一个函数中，而所有的计算都在另一个函数中。

```py
calculate_turnover <- function(sales_data, day) {
  sales_data %>%
    select(date, transaction_id, category, item_price) %>%
    filter(date == day) %>%
    group_by(category) %>%
    summarise(day_turnover = sum(item_price))
}

action_turnover <- function(day) {
  read_csv("sales_data.csv") %>%
    calculate_turnover(day) %>%
    write_csv(paste0("sales_summarise/day_turnover_", day, ".csv"))
}

action_turnover("2023-05-10")
```

这使得代码整洁而分隔明确。测试计算过程非常简单，因为它们与动作的任何副作用都被清晰分隔开来。这种分离也使我的动作更容易测试。

“哦，是的，他们在游戏开发中也经常这样做”，我向一位同事描述了这种方法后，他这样回答道。如果你有软件开发背景，你很可能已经知道这一点。但是数据行业的人员来自各行各业，有时可能从未学习过这些技巧。虽然我自学了一些良好的编码原则，但对我来说，这是一个新的领域，今后我会更多地加以利用。

# 3\. 严格的测试是必不可少且被期望的。

谈到测试，这是整个会议的一个常见主题。

Vyara Apostolova 和 Laura Cole 在国家审计办公室的一次讲话中重点讨论了测试和检查。她们的工作涉及接收重要的政府模型，并在 R 中复现这些模型。在此过程中，她们会细致地测试每个模型中的每个假设，检查错误和差异。

这可能需要数年时间才能完成。但是，这种细致的工作对于发现昂贵的错误至关重要。她们揭示了，由于在金融模型中的错误导致的不必要的费用浪费，他们的工作已经节省了数百万英镑。这一切都是因为他们在严格的框架内测试、检查和评估他们编码的一切。

在其他讲话中，观众的问题经常与测试相关。如果有人展示了使用 R 进行某种新的酷炫方法，人们想知道的第一件事就是如何测试它。

尽管 R 在我们的学术教学和实践中是内置的，但测试却被高度低估。从外部视角来看，SatRDays 让我意识到，对于数据专业人士来说，拥有健壮的自动化检查工作流程是多么重要。

无论你是已经从事数据工作还是试图进入这个行业，测试都可以巩固你工作的价值。离开 SatRDays 后，我计划在几个月内找工作之前提高我的软件测试技能。

# 4\. 学习 R 可能会很有趣。

我以前写过关于[学习 R 的乐趣](https://medium.com/towards-data-science/make-learning-r-fun-with-these-5-packages-3c3f6ca82c96)。但在 SatRDays 之前，我从未因数据科学演讲而大笑过。

如果我必须从整天的演讲中选择一个最喜欢的，那就是[Andrew Collier](https://www.datawookie.dev/blog/)的“整理世界的助手”。在这个演讲中，Andrew 介绍了一些不太为人知的 tidyverse 函数，以及它们如何补充更常用的函数。

在演讲中解释代码的工作原理是一个艰巨的任务。你需要在提供信息和避免让观众被技术细节困住之间找到平衡。SatRDays 的所有演讲都实现了这种平衡，但 Andrew 以突出的幽默和风格做到了这一点。

他的演讲充满了流行文化的引用和恰到好处的笑话。这些不仅有趣，还将新概念与观众已经知道的信息联系起来。

在演讲中使用幽默意味着让自己暴露在外。让观众笑一笑是一种特别温暖和令人满意的体验。当你的笑话在寂静的房间里回响时，那种感觉则完全相反。这就是演讲中的高潮和低谷。

话虽如此，幽默在技术演讲中是一个很好的工具，即使它并不总是有效。与其成为娱乐性的分心，它如果用得当，甚至能使你的解释更出色。往后，我会尝试在我的演讲中加入更多幽默，以使其更具特色。

# 5\. R 社区独一无二。

R 社区非常棒。

这一点对于了解我人品的任何人来说都不应该感到惊讶。我鼓励在[学习 R](/how-to-learn-r-for-data-science-fast-d47408d0becf)时积极参与社区互动，甚至在选择使用哪些软件包时也是如此。

[](/tidyverse-vs-base-r-how-to-choose-the-best-framework-for-you-29b702bdb384?source=post_page-----3fceb3caa6a1--------------------------------) [## Tidyverse 与 Base-R：如何为自己选择最佳框架

### 最受欢迎的 R 编程方法的优缺点。

towardsdatascience.com](/tidyverse-vs-base-r-how-to-choose-the-best-framework-for-you-29b702bdb384?source=post_page-----3fceb3caa6a1--------------------------------)

但我从未与这么多 R 用户和专业人士待在同一空间里过。我在一天开始时不认识任何人，所以我希望能找到至少几个友好的人聊聊天。说实话，我有点紧张。

我不必担心。我遇到的每个人都很友好、平易近人且有趣。我不是一个外向的人，也不擅长社交，但我在所有休息时间都站着和人交谈。我知道这在所有事件中并不是必然的——我曾经有几次孤独的午餐没有伴侣，所以在这里尤其感到高兴。

R 社区也在多样化。Ella Kaye 和 Heather Turner 讨论了她们在让代表性不足的群体参与 R 语言维护方面的持续工作。目前，大多数主要的语言贡献者是接近职业生涯末期的西方男性。为了使 R 语言保持运作并与时俱进，重要的是将接力棒交给来自全球的更多样化的贡献者。Ella 和 Heather 分享了她们正在设置的各种倡议和活动，所有这些都听起来是前进的有希望的步骤。

我很高兴看到这种多样性在观众中得到了体现。虽然我算得上是众多戴眼镜的白人男性中的一员，但也有很多其他性别、特征和背景的人。我感觉每个人都被包括在内，组织者也积极促进了这一点。

如果这里有一个教训，那就是你不应该害怕与人建立联系，特别是在 R 社区中。他们可以为你提供关于工作、公司和技术的宝贵见解，这些都能推动你的职业发展。而且，至少在我的经历中，他们真的很友善。

参加一个 R 事件是一次很棒的经历，我希望以后能再去一次。我不仅了解了我最喜欢的编程语言，还认识了使用这门语言的人和相关行业。可以说，我将从 SatRDays 中获得很多收获，并在未来几个月内运用我新获得的知识。

如果你在阅读完后做些什么，请找到一些以你喜欢的数据科学技术为中心的活动，看看你是否可以去参加其中一个。组织 SatRDays 的公司 Jumping Rivers 有一个[ R 事件列表](https://jumpingrivers.github.io/meetingsR/index.html)供你查看。像[RLadies](https://www.rladies.org/)这样的其他组织也经常在全球范围内举办面对面的聚会。

参加一个新的活动可能会让人感到紧张，但如果它与你及你的兴趣相关，通常是值得的。

**想要将我所有关于 R 编程、数据科学等的文章直接送到你的邮箱吗？** [**点击这里订阅**](https://roryspanton.medium.com/subscribe)**。**

要获取我在 Medium 上的所有故事的完整访问权限，请通过[此链接](https://medium.com/@roryspanton/membership)注册会员。
