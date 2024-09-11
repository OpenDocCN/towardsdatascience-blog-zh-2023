# 我如何通过第一个 #30DayChartChallenge 学习 Observable Plot

> 原文：[https://towardsdatascience.com/how-i-used-my-first-30daychartchallenge-to-learn-observable-plot-78141929e3e2?source=collection_archive---------6-----------------------#2023-10-13](https://towardsdatascience.com/how-i-used-my-first-30daychartchallenge-to-learn-observable-plot-78141929e3e2?source=collection_archive---------6-----------------------#2023-10-13)

[](https://medium.com/@menghani.deepsha?source=post_page-----78141929e3e2--------------------------------)[![Deepsha Menghani](../Images/56a6ed8597c36e8c76d8a29a449325a4.png)](https://medium.com/@menghani.deepsha?source=post_page-----78141929e3e2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----78141929e3e2--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----78141929e3e2--------------------------------) [Deepsha Menghani](https://medium.com/@menghani.deepsha?source=post_page-----78141929e3e2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb0c00845bcfa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-used-my-first-30daychartchallenge-to-learn-observable-plot-78141929e3e2&user=Deepsha+Menghani&userId=b0c00845bcfa&source=post_page-b0c00845bcfa----78141929e3e2---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----78141929e3e2--------------------------------) ·6分钟阅读·2023年10月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F78141929e3e2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-used-my-first-30daychartchallenge-to-learn-observable-plot-78141929e3e2&user=Deepsha+Menghani&userId=b0c00845bcfa&source=-----78141929e3e2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F78141929e3e2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-used-my-first-30daychartchallenge-to-learn-observable-plot-78141929e3e2&source=-----78141929e3e2---------------------bookmark_footer-----------)![](../Images/1633eb05d0bfe3a67a845e1b4f3f0d67.png)

图片由 [Isaac Smith](https://unsplash.com/@isaacmsmith?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，出处 [Unsplash](https://unsplash.com/photos/6EnTPvPPL6I?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)。

如果你在数据领域工作，你可能熟悉那种有一长串工具列表等待学习的感觉，希望有一天能实现。对我而言，我的列表上有很长时间的一项是[Observable Plot](https://observablehq.com/plot/)，这是一个用于探索性数据可视化的JavaScript库。终于在今年，我决定利用[#30DayChartChallenge](https://30daychartchallenge.org/about/)来深入了解这个令人惊叹的库。作为第一次参与这个挑战的人，我想分享我的经验和所获得的见解，并为那些考虑参与类似即将到来的挑战的人提供建议。

# 什么是 #30DayChartChallenge

#30DayChartChallenge 是一个由社区驱动的挑战，每年四月举行。每天有一个主题提示，为当天的可视化提供灵感，数据可视化创作者可以根据这个主题进行解释。对数据来源或工具的使用没有限制。对于数据爱好者、设计师和有志的数据可视化艺术家来说，这是一个迷人的活动。

# 为什么我参与了 #30DayChartChallenge。

我决定参加挑战是由于几个因素驱动的：

+   **探索 Observable Plot 库：** 这是我的主要目标。我想通过深入了解所有JS基础的[Observable Plot](https://observablehq.com/plot/)库来扩展我的技能并探索新的交互式数据可视化工具。你可以在我的[#30DayChartChallenge collection](https://observablehq.com/collection/@deepsha/30-day-chart-challenge)中找到所有我的可视化的代码和数据来源，包括这里提到的那些。

+   **提升数据可视化技能：** 我迫切希望提高将原始数据转化为视觉上吸引人且信息丰富的图表和图形的能力。

+   **激发创造力：** 这个挑战提供了一个独特的机会，可以尝试各种数据可视化技术和创意，推动我跳出固有思维。它提供了一种有趣的方式来发挥我的创造力。

+   **建立联系：** #30DayChartChallenge 社区由来自不同背景的数据爱好者组成，为网络交流和分享创意提供了一个很好的平台。

# 从我第一次 #30DayChartChallenge 学习新工具的经验中获得的建议

## 1\. 规划 — 你值得信赖的盟友：

由于提示在月份开始前很久就已提供，所以可以提前分配时间进行头脑风暴，寻找数据来源，并草拟粗略设计。挑战开始前的规划允许你专注于可视化过程和学习，而不会每天感到不堪重负。我浏览过的一些最喜欢的数据来源慷慨地来自[Our World in Data](https://ourworldindata.org/)和[Kaggle](https://www.kaggle.com/datasets/)。

这是我使用 [US tornadoes dataset](https://www.kaggle.com/datasets/michaelbryantds/tornadoes) 从 Kaggle 创建的可视化，用于第7天的“危险”提示。

![](../Images/330d0e7fbcca0e659888426b95431c23.png)

第7天：危险，来源：[#30DayChartChallenge collection](https://observablehq.com/collection/@deepsha/30-day-chart-challenge)

## 2\. 数据重用的艺术：

每天寻找新的数据来激励你，持续30天会很快让人感到疲惫。重复使用数据，尝试用新的方式讲述相同的故事。

以下第一个图是第6天的“数据日：OWID”提示。我使用了[累积土地使用变化数据](https://raw.githubusercontent.com/owid/co2-data/master/owid-co2-data.csv)，涵盖了六个国家一个世纪的时间。在第28天的提示是“趋势”，我认为这些数据也很适合。因此，我没有讲述一个新的故事，而是专注于通过动画讲述故事，这样我既可以在使用相同数据的同时学习新东西，又能节省寻找数据的时间。

![](../Images/a88aea2e1ae7c1f730070f4d63e952b3.png)

第6天：数据日 — OWID，来源：[#30DayChartChallenge collection](https://observablehq.com/collection/@deepsha/30-day-chart-challenge)

![](../Images/50ad5438fe85889b8f5054d57bed7d92.png)

第28天：趋势，来源：[#30DayChartChallenge collection](https://observablehq.com/collection/@deepsha/30-day-chart-challenge)

## 3\. 目标的清晰度。

确定你参加挑战的目标。对我来说，学习 Observable Plot 库是主要的重点。这种清晰度帮助我在整个过程中保持专注和积极。有时这意味着我不得不接受一个花费时间较长的“足够好”的图，即使有其他我已经熟悉的工具如 ggplot2，我可以用它们在更短的时间内制作更好的图。

这是我为第5天的“斜率”提示创建的图，使用了[动物园动物寿命数据](https://data.world/animals/zoo-animal-lifespans)。这个简单的折线图和点图花了我太多时间去弄清楚，因为我在学习一个新工具，但完成它时感觉很值得。

![](../Images/37f24d9992452e7b8aad4137aee7f46d.png)

第5天：斜率，来源：[#30DayChartChallenge collection](https://observablehq.com/collection/@deepsha/30-day-chart-challenge)

## 4\. 追求成长，而非完美！

这很困难，因为我们都希望每天都能发布出最好的可视化。拥抱学习过程，并接受不是每个可视化都会完美，这是避免在挑战中感到疲惫的关键。专注于提高你的技能和对数据可视化概念的理解。

这是第13天的一个示例，提示是“流行文化”。在一天长时间的工作之后，当我坐下来进行挑战时，我想学习创建一个径向图，以查看[超级碗广告类别分布](https://www.kaggle.com/datasets/ulrikthygepedersen/superbowl-ads)如何随着时间变化。我曾设想为所有类别创建它，我从最感兴趣的4个类别开始。虽然这不是最终计划，也很难停在这里，但我决定在4个类别时结束，以免疲惫并为第二天的可视化保留精力。

![](../Images/2dd81165ec937b9106630d6fddfef9cd.png)

第13天：流行文化，来源：[#30DayChartChallenge collection](https://observablehq.com/collection/@deepsha/30-day-chart-challenge)

## 5\. 庆祝这段旅程！

庆祝你的成就，无论多小。认可你的进步和成长，最重要的是不要陷入与他人比较的陷阱，而要以此为灵感。记住，这个挑战是关于个人成长和学习的。我发布的前几个图表远不如我看到的其他人的精彩，但在最初的几天里，提醒自己在变得更好之前会很糟糕，这是很重要的。

不必完成所有三十天，参与其中可能会很累，但也很有影响力，所以只要你能参与就足够了，每次出现都应该庆祝。

这是我在最后一周为了好玩制作的一个图表，使用了[大脚怪数据](https://data.world/timothyrenner/bfro-sightings-data)，因为谁不喜欢大脚怪呢，而在最后一周，我已经逼自己很辛苦了，需要一些乐趣来继续前进！

![](../Images/4f30afe47b27dad81dcafcca94f6ad8e.png)

来源：[#30DayChartChallenge collection](https://observablehq.com/collection/@deepsha/30-day-chart-challenge)

# 向前迈进，持续进步！

参加#30DayChartChallenge对我来说是一次变革性的经历。我提高了数据可视化技能，掌握了一个新库，并与许多了不起的创作者进行了交流和学习。现在，我的[代码库](https://observablehq.com/collection/@deepsha/30-day-chart-challenge)以及所有数据源充当了个人的Observable Plot备忘单，我经常参考。最近，我在与两位非常有才华的数据可视化艺术家Tanya Shapiro和Allison Horst的[小组讨论](https://www.youtube.com/watch?v=Bn2-MLHtBJ8&t=14s)中分享了我通过#30DayChartChallenge学习Observable Plot的经历。

我希望这篇文章鼓励你参与你的第一次#30DayChartChallenge，也许还可以用它来学习一个新工具。今年11月还会有#30DayMapChallenge，届时的每日提示已经在[这里](https://30daymapchallenge.com/)提供了。所以，现在是时候开始为假期季节做计划了。

你可以在我的[Observable collection](https://observablehq.com/collection/@deepsha/30-day-chart-challenge)中找到重现本文所有可视化的代码和数据。

如果你愿意，可以在[Linkedin](https://www.linkedin.com/in/deepshamenghani/)上找到我。
