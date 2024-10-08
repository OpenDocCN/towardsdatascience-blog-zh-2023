# 获取你下一个数据项目的有趣数据集的 5 种方法（非 Kaggle）

> 原文：[`towardsdatascience.com/5-ways-to-get-interesting-datasets-for-your-next-data-project-not-kaggle-71cf76eef64b`](https://towardsdatascience.com/5-ways-to-get-interesting-datasets-for-your-next-data-project-not-kaggle-71cf76eef64b)

## 对 Kaggle 和 FiveThirtyEight 感到厌倦了吗？这里是我用来获取高质量和独特数据集的替代策略。

[](https://medium.com/@mattchapmanmsc?source=post_page-----71cf76eef64b--------------------------------)![Matt Chapman](https://medium.com/@mattchapmanmsc?source=post_page-----71cf76eef64b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----71cf76eef64b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----71cf76eef64b--------------------------------) [Matt Chapman](https://medium.com/@mattchapmanmsc?source=post_page-----71cf76eef64b--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----71cf76eef64b--------------------------------) ·7 分钟阅读·2023 年 6 月 24 日

--

![](img/e876fa791ac0f463d9091464b6651ba9.png)

图片来源 [Efe Kurnaz](https://unsplash.com/@efekurnaz) 在 [Unsplash](https://unsplash.com/photos/RnCPiXixooY)

一个伟大的数据科学项目的关键是一个优秀的数据集，但找到优质数据远比说起来容易。

我记得在我读数据科学硕士学位时，一年多前。整个课程中，我发现想出项目想法很简单——真正让我困扰的是*找到好的数据集*。我会花几个小时在互联网上搜索，拼命想找到有用的数据源，但始终无功而返。

从那时起，我在方法上取得了很大进展，在这篇文章中，我想与您分享我用来寻找数据集的 5 种策略。如果你对 Kaggle 和 FiveThirtyEight 这样的标准来源感到厌倦，这些策略将使你能够获得独特且更加贴合你特定用例的数据。

# 1\. 制作自己的数据

没错，无论你信不信，这确实是一个合法的策略。它甚至有一个花哨的技术名称（“合成数据生成”）。

如果你正在尝试一个新想法或有非常具体的数据需求，制作合成数据是获取原创且量身定制的数据集的绝佳方式。

比如，假设你正在尝试建立一个客户流失预测模型——一个可以预测客户离开公司的可能性模型。流失是许多公司面临的一个相当常见的“运营问题”，解决这样的问题是展示你可以利用机器学习解决商业相关问题的绝佳方式，就像我之前所说的：

[](/how-to-find-unique-data-science-project-ideas-that-make-your-portfolio-stand-out-1c2ddfdbefa6?source=post_page-----71cf76eef64b--------------------------------) ## 如何找到独特的数据科学项目创意，使你的作品集脱颖而出

### 忘掉 Titanic 和 MNIST：选择一个独特的项目来提升你的技能，帮助你在众人中脱颖而出

towardsdatascience.com

然而，如果你在网上搜索“客户流失数据集”，你会发现（截至写作时）公开可用的主要数据集只有两个： [银行客户流失数据集](https://www.kaggle.com/datasets/gauravtopre/bank-customer-churn-dataset) 和 [电信流失数据集](https://www.kaggle.com/mnassrib/telecom-churn-datasets)。这些数据集是一个很好的起点，但可能无法反映其他行业所需的数据类型。

相反，你可以尝试创建更符合你要求的合成数据。

如果这听起来太美好而不真实，这里有一个我用一个简短提示创建的示例数据集，这个提示是发给 ChatGPT 的老方法：

![](img/38fca05145d88471423f4ed5fb19d78b.png)

作者提供的图片

当然，ChatGPT 在创建数据集的速度和规模上有一定的限制，所以如果你想扩展这项技术，我建议使用 Python 库 `faker` 或 scikit-learn 的 `sklearn.datasets.make_classification` 和 `sklearn.datasets.make_regression` 函数。这些工具是以编程方式快速生成巨大数据集的绝佳方法，非常适合构建概念验证模型，而不必花费大量时间寻找完美的数据集。

实际上，我很少需要使用合成数据创建技术来生成*完整*数据集（而且，正如我稍后将解释的那样，如果你打算这样做，你应该谨慎）。相反，我发现这是生成对抗性示例或向数据集中添加噪声的一个非常好的技术，使我能够测试模型的弱点并构建更强健的版本。但是，无论你如何使用这项技术，它都是一个非常有用的工具。

# 向公司请求他们的数据（礼貌地）

创建合成数据是当你找不到所需数据类型时的一个不错的解决办法，但明显的问题是，你无法保证这些数据能很好地代表真实生活中的人群。

如果你想确保你的数据是现实的，最好的方法是，出乎意料的是……

… 实际上去寻找一些*真实*数据。

一种方法是联系可能持有这些数据的公司，询问他们是否有兴趣与您分享一些数据。风险提示，没有公司会给你高度敏感的数据，尤其是如果你计划将其用于商业或不道德的目的。这会显得非常愚蠢。

然而，如果你打算将数据用于研究（例如，用于大学项目），你可能会发现公司愿意提供数据，只要这是在*互惠互利*的联合研究协议的背景下。

我所说的是什么意思？实际上很简单：我指的是一种安排，即他们向你提供一些（匿名/去敏感化的）数据，而你利用这些数据进行对他们有一定益处的研究。例如，如果你有兴趣研究客户流失建模，你可以提出一个比较不同流失预测技术的提案。然后，将提案与一些公司分享，并询问是否有可能合作。如果你足够坚持并广泛接触，你可能会找到愿意提供数据的公司*只要你与他们分享你的发现*，这样他们就可以从研究中获得益处。

如果这听起来太好了以至于难以置信，你可能会惊讶地发现[这正是我在硕士学位期间所做的](https://medium.com/towards-data-science/4-ways-to-get-the-most-out-of-your-data-science-degree-40815f6a311d)。我联系了几家公司，提出了一个如何利用他们的数据进行对他们有益的研究的提案，签署了一些文件以确认我不会将数据用于其他目的，并使用一些真实世界的数据进行了一个非常有趣的项目。确实可以做到这一点。

我特别喜欢这种策略的另一点是，它提供了一种锻炼和发展一套广泛技能的方式，这些技能在数据科学中非常重要。你必须善于沟通，展示商业意识，并成为管理利益相关者期望的高手——这些都是数据科学家日常生活中必不可少的技能。

![](img/2f5396a4ba057dbee6ac686705a4b161.png)

请让我获取你的数据。我会表现良好的，我保证！图片由[Nayeli Rosales](https://unsplash.com/@nrosales)提供，来源于[Unsplash](https://unsplash.com/photos/BbGxyxb2O3U)

# 查阅学术界存储其期刊文章代码的代码库

很多用于学术研究的数据集并没有发布在像 Kaggle 这样的平台上，但仍然对其他研究人员公开可用。

寻找类似数据集的最佳方法之一是查看与学术期刊文章相关的资料库。为什么？因为很多期刊要求其投稿者公开基础数据。例如，我在攻读硕士学位期间使用的两个数据源（[Fragile Families](https://www.fragilefamilieschallenge.org/) 数据集和 [Hate Speech Data](https://github.com/leondz/hatespeechdata) 网站）在 Kaggle 上没有提供；我通过学术论文及其相关代码库找到了这些数据源。

如何找到这些资料库？实际上非常简单——我从打开 [paperswithcode.com](https://paperswithcode.com/) 开始，搜索我感兴趣领域的论文，并查看可用的数据集，直到找到看起来有趣的内容。根据我的经验，这是一种非常巧妙的方式来找到那些没有被 Kaggle 群众反复使用的数据集。

# BigQuery 公共数据集

老实说，我不知道为什么更多人不利用 BigQuery 公共数据集。这里真的有*数百*个数据集，涵盖从 Google 搜索趋势到伦敦自行车租赁，再到大麻基因组测序的所有内容。

我特别喜欢这个来源的一个原因是，这些数据集在商业上非常相关。你可以告别像花卉分类和数字预测这样的小众学术话题；在 BigQuery 中，有关于广告效果、网站访问和经济预测等现实世界商业问题的数据集。

很多人避开这些数据集，因为它们需要 SQL 技能来加载。但即使你不知道 SQL，只会 Python 或 R 等语言，我仍然建议你花一两个小时学习一些基本的 SQL，然后开始查询这些数据集。上手并不需要很长时间，这确实是一个高价值数据资产的宝库。

要使用 BigQuery 公共数据集中的数据集，你可以按照 [这里](https://cloud.google.com/bigquery/docs/sandbox) 的说明注册一个完全免费的账户并创建一个沙箱项目。你无需输入信用卡信息或类似的东西——只需提供你的名字、电子邮件、项目的基本信息即可。如果以后需要更多计算能力，你可以将项目升级为付费项目，访问 GCP 的计算资源和高级 BigQuery 功能，但我个人从未需要这样做，发现沙箱环境已经足够了。

# 尝试使用数据集搜索引擎

我的最后一个建议是尝试使用数据集搜索引擎。这些工具在过去几年才刚刚出现，它们使得快速查看现有数据变得非常容易。我最喜欢的三个是：

+   [哈佛数据集](https://dataverse.harvard.edu/)

+   [Google 数据集搜索](https://datasetsearch.research.google.com/)

+   [Papers with Code](https://paperswithcode.com/datasets)

根据我的经验，使用这些工具进行搜索往往比使用通用搜索引擎更有效，因为你通常可以获得有关数据集的元数据，并且可以根据使用频率和发布日期对其进行排名。如果你问我，这是一种相当巧妙的方法。

感谢阅读！希望你觉得这 5 种策略有帮助，如果你有任何反馈或问题，请随时联系我 :-)

# 还有一件事——你能成为我那 1% 的一员吗？

在 Medium 上不到 1% 的读者点击我的“关注”按钮，因此无论是在 Medium、[Twitter](https://twitter.com/matt_chapma) 还是 [LinkedIn](https://www.linkedin.com/in/matt-chapman-ba8488118/)，你做出这样的举动对我而言意义重大。

如果你希望获得对我所有故事（以及 Medium.com 上其他内容）的无限访问权限，你可以通过我的 [推荐链接](https://medium.com/@mattchapmanmsc/membership) 以每月 $5 注册。相比于通过普通注册页面注册，这不会额外增加你的费用，并且有助于支持我的写作，因为我会获得一小部分佣金。
