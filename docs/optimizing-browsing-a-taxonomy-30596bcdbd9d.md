# 优化浏览分类法

> 原文：[`towardsdatascience.com/optimizing-browsing-a-taxonomy-30596bcdbd9d`](https://towardsdatascience.com/optimizing-browsing-a-taxonomy-30596bcdbd9d)

## **问题、模型和实施问题**

[](https://jagota-arun.medium.com/?source=post_page-----30596bcdbd9d--------------------------------)![Arun Jagota](https://jagota-arun.medium.com/?source=post_page-----30596bcdbd9d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----30596bcdbd9d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----30596bcdbd9d--------------------------------) [Arun Jagota](https://jagota-arun.medium.com/?source=post_page-----30596bcdbd9d--------------------------------)

·发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----30596bcdbd9d--------------------------------) ·阅读时间 20 分钟·2023 年 1 月 4 日

--

![](img/701afca47c30a4c877941cba92f3156f.png)

图片来自[mcmurryjulie](https://pixabay.com/users/mcmurryjulie-2375405/)，[pixabay](https://pixabay.com/)

这里的主要用例是[`jagota-arun.medium.com/interactive-and-adaptive-menu-ordering-agent-bb447c58b3af`](https://jagota-arun.medium.com/interactive-and-adaptive-menu-ordering-agent-bb447c58b3af)（不是必读材料。）

我们专注于建模。它是什么？它如何与推荐系统相关？然后是实施。就像面向对象设计一样。类等。

面向对象的阐述风格将序列化、模块化、可组合性和可扩展性的好处扩展到写作中。这里非常合适。

本文最重要的概念是分类法。一切都发生在它上面。用户浏览、用户行为（在叶子上）、建模和评分，以动态重新排列分类法，优化用户的浏览体验。

那么让我们从定义它开始。分类法是一个选择树，具体的感兴趣项目位于其叶子上。我们最终假设用户关心的是到达感兴趣的叶子。内部节点主要用于导航。我们通过用户是否在叶子上采取行动来学习。

一个例子是餐厅菜单。菜单项位于叶子上。内部节点是菜单上的各种类别：饮料、开胃菜、主菜等。用户的一个动作是点选特定的菜单项。如果用户想点饮料，她会进入*饮料*子树。

在视频观看场景中，用户的行为可能是观看视频。

我们将用户与分类法的互动建模为一个会话，其中用户浏览树的某些部分。当用户到达一个合适的叶子时，她可以选择行动或不行动。

我们的主要兴趣是从用户在特定叶子上的行为中学习偏好，以便在树中的某些节点上表现出偏好。我们希望在叶子节点和内部节点上都能学习到偏好。

**这与常规推荐系统有何关系？**

通常，我们期望推荐系统主要推荐新的项目。我们在这里的重点是简化用户对感兴趣项目的导航。这可能包括用户之前已操作过的项目。例如餐厅设置。

另一个需要注意的是，推荐系统通常基于用户-物品矩阵操作，而不是基于分类法。

**面向对象的阐述**

我们最重要的类将是 *UserInteractionsWithTaxonomy*。其角色应从其名称和我们之前的讨论中明确。

这个类会被实例化以建模特定用户与特定分类法的互动。实例化时需要提供分类法。调用者负责将实例化的模型与适当的用户关联。如下面所示。这个类会为分类法中的每个节点维护一个计数器。这些计数器在新实例中会被初始化为零。

```py
john_doe_interactions_with_specific_restaurant =
UserInteractionsWithTaxonomy(taxonomy_of_restaurant)
```

“用户”可以是模型创建者想要的任何人。一个人或一整个群体。

**关键方法**

*UserInteractionsWithTaxonomy* 将支持一个方法 *act*。这个方法将建模用户在特定叶子上的一次行为实例。例如，点餐时选择某个特定的菜品。

在我们版本的 *act* 方法中，每次调用都会递增叶子及其所有祖先的计数。为了高效地做到这一点，这个类可能会维护分类法中所有节点的父节点弧。

请注意，我们选择仅从 *act* 行为中学习用户偏好。我们可以将此推广到包括用户点击树的内部节点的反馈。但在这里我们不会追求这一点，因为我们认为 *act* 行为比内部节点点击更能预测用户偏好。读者可以自行尝试。

第二个方法将模拟用户在树上的某个节点上点击。这将让我们追踪用户当前在树上的位置，以便我们可以在下面展示适当的选择。

我们将在一个方法 *choices()* 中表达这些选择，该方法将返回当前用户在树上所在节点的子节点的得分。更精确地说，它将返回一个 [*child*, *score*] 对列表，其中 *child* 是 *self*.*current_node* 的子节点，*score* 是其得分。这个列表按得分的非递增顺序排列。

这是一个示例流程。

```py
john_doe_interactions_with_specific_restaurant.click("Drinks")
choices = john_doe_interactions_with_specific_restaurant.choices()
# User picks 'Coca Cola' from choices and acts on it. 
# ('act' is 'order here'.)
john_doe_interactions_with_specific_restaurant.act('Coca Cola')
```

让我们一起来看看这个类的内部细节。

```py
class UserInteractionsWithTaxonomy:

  def _init_(self, taxonomy):
    self.taxonomy = taxonomy
    self.current_node = None

  def click(self, node):
    self.current_node = node

  def act(self, leaf, pc = 1):
    # increment the leaf's counter by pc
    # increment the counters of all of the leaf's ancestors, each by pc

  def choices(self):
    res = {}
    for each child of self.current_node:
      res[child] = self.score(child, self.current_node)
    return res

  def score(self,child,parent):
    return self.counter[child]/self.counter[parent]
```

请注意 *act* 方法中的参数 *pc*。我们稍后会解释它。目前，假设 *pc* 等于 1，即其默认值。因此，“递增 … by *pc*” 意味着 “递增 … by 1” 或简称 “递增 …”。

方法*score*(*.*)返回*P*(*child*|*parent*)，即用户在*parent*位置时访问*child*的概率。这只是子项的计数除以父项的计数。

**冷启动问题**

假设用户访问了一家新餐馆。由于用户在该餐馆没有订单历史记录，因此不能使用评分方法。因此，我们必须等用户点几个项目后才能评分。这个场景被称为*用户冷启动问题*。

类似地，还有一个*分类冷启动问题*。在餐馆的场景中，这对应于一个全新的餐馆开张。尚未与任何用户互动。

实际上，我们当前评分中隐藏了更细致的冷启动问题。我们的评分函数是

```py
self.counter[child]/self.counter[parent]
```

想象一下一个餐馆，用户有订单历史记录，但在特定类别下没有，例如*甜点*。*父类别* = *甜点*的计数将为 0，因此其子类别的得分也无法计算。

现在想象一下，用户在某个类别中下单，但未在特定子类别中下单。例如，在饮料类别中点了一种特定的饮料。子项的得分将为 0。虽然 0 是一个有效的得分，但可能不是我们所希望的。

总结来说，未曾被点过的项目将无法评分或得分为 0。

冷启动问题在推荐系统的环境中经常出现，见[4]。在这种环境下，分类冷启动问题对应于项目冷启动问题。

**缓解冷启动问题**

缓解所有冷启动问题的一个简单方法是将所有节点上的计数器初始化为一个称为*pc*的正数。虽然它需要是正数，但可以小于 1。

在我们的设置中，我们将支持一个类似但更强大的机制。无论是从建模还是编码的角度来看，都不会更复杂。

我们将为类*UserInteractionsWithTaxonomy*添加一个方法*act_randomly*(*n*, *pc*)。

```py
class UserInteractionsWithTaxonomy:

  …

  def act_randomly(self, n. pc):
    for i in range(n):
      leaf = leaves.sample_randomly()
      self.act(leaf, pc)
```

现在回到用户第一次访问餐馆的场景。假设我们调用*act_randomly*(*n* = 100, *pc* = 0.01)。这样做的效果就像用户在这家餐馆点了 100 次，每次随机选择菜单上的一项。为了考虑*pc*，我们应该将这些订单称为“分数订单”。

为什么要使用*pc*？也就是说，为什么不设置为 1？因为在某些环境中，我们可能希望单个*实际*订单比单个虚拟订单更重要。在这种情况下，我们会将*pc*设置为远小于 1 的值。在其他环境中，我们可能希望相反的行为。只有当用户订单达到一定的最小次数时，我们才会认为它是重要的。在这种情况下，我们会将*pc*设置为大于 1 的值。

*act_randomly*方法可以缓解所有上述冷启动问题。不过，我们通常可以在用户冷启动问题上做得更好。

**使用分类模型加热用户冷启动**

考虑一个有很多用户互动历史的餐厅。这些历史互动可以用来为该餐厅形成一个模型。这个模型，我们称之为*餐厅模型*，实际上是*UserInteractionsWithTaxonomy*的一个实例，其中用户是一个集体而非个人。

为了在特定用户的交互中利用分类模型，我们将重构*UserInteractionsWithTaxonomy*类如下。

```py
class UserInteractionsWithTaxonomy:

  def __init__(self, taxonomy, taxonomy_model = None):
    …
    self.taxonomy_model = taxonomy_model
    …

  def act_on_this(self, leaf, pc = 1):
    # Previous version of act(.) renamed

  def act(self, leaf):
    self.act_on_this(leaf)
    if self.taxonomy_model is not None:
      self.taxonomy_model.act_on_this(leaf)

  def act_randomly(self, n. pc):
      for i in range(n):
        …
        self.act_on_this(leaf, pc)

  def score(self,child,parent, pcm = 1):
    if self.taxonomy_model is None:
      return self.counter[child]/self.counter[parent]
    else:
      s = self.taxonomy_model.score(child, parent)
      pc = pcm*s
      child_adjusted_count   = self.counter[child] + pc
      parent_adjusted_count  = self.counter[parent] + pcm
    return child_adjusted_count/parent_adjusted_count
```

让我们解释一下。在实例化*UserInteractionsWithTaxonomy*时，我们提供另一个*UserInteractionsWithTaxonomy*的实例，它将用作分类模型，如下所示。

```py
restaurant_model = UserInteractionsWithTaxonomy(taxonomy_of_restaurant)
user_restaurant_models = {}
for user in users_of_this_restaurant:
  user_restaurant_models[user] = UserInteractionsWithTaxonomy(
    taxonomy_of_restaurant, restaurant_model)
```

在上面的示例中，所有用户餐厅模型共享相同的（通用）餐厅模型。

对任何用户餐厅模型上的*act*，即使是未来的模型，也会对共享餐厅模型执行相同的操作。

为了干净地实现这一点，我们将之前的*act*版本重命名为*act_on_this*，以便可以在其上构建新的*act*。由于这一名称更改，我们还不得不对*act_randomly*(.)进行更改。

然后我们重构了方法*score*(.)，使得如果实例附加了分类模型，它也会在评分中使用该模型。

在*score*(.)的重构版本中发生了很多事情，所以我们来详细说明一下。考虑在用户特定模型中对某个孩子与某个父母进行评分。首先，我们调用

```py
self.taxonomy_model.score(child, parent)
```

这给我们提供了*P*(*child*|*parent*, *generic taxonomy model*)。接下来，我们假设对父母进行*pcm*次访问，其中*pcm***P*(*child*|*parent*, *generic* *taxonomy model*)预计会以*child*结束。因此，我们将这些想象中的访问次数加到用户的实际访问次数上。

在上面的段落中，术语“访问节点”仅用于说明。在实际情况中，对一个节点的访问对应于在该节点下的叶子上的*act*实例。

让我们看一个数字示例。假设*pcm*是 10。假设*P*(*child*|*parent*, *generic taxonomy model*)等于⅕。对于这个孩子，*pc*将是 2。如果特定用户从未访问过父母，*P*(*child*|*parent*, *user model for this taxonomy*)将等于⅕，即*P*(*child*|*parent*, *generic taxonomy model*)的值。随着用户开始访问*parent*，用户随后访问的孩子将开始影响*P*(*child*|*parent*, *user model for this taxonomy*)，如果用户的行为偏离，可能会将其从*P*(*child*|*parent*, *generic taxonomy* *model*)中移开。

**建模用户偏好**

考虑这种情况。一个全新的餐厅刚刚开张。由于没有历史记录，因此没有餐厅模型。

一个用户走进来。当然，我们可以使用*act_randomly*从菜单中随机采样，如前所述。我们能做得更好吗？

我们应该将餐馆模型实例与每一个访问餐馆的新用户关联起来。这样，至少餐馆模型会比任何一个个人学习得更快。任何突出显示的菜单项，即在用户中被频繁点餐的项，会更快地被呈现出来。我们还能做得更好吗？

假设用户过去访问过其他餐馆。如果我们能够对这些访问建模并学习用户的偏好，我们就可以将这些偏好应用到用户首次访问这家新餐馆时。例如，某些词汇如*curry*的偏好，或某些价格范围的偏好。

虽然在前一段中我们使用了餐馆冷启动问题来激励建模用户偏好，但解决这个问题涉及到复杂的知识转移问题。这是因为用户之前访问的餐馆的分类法可能与当前餐馆的分类法不同。所以我们将推迟讨论这个用例到另外一篇文章中。

也就是说，将用户偏好纳入我们的模型可以帮助未来用户与相同分类法的互动。

比如，当有很多菜品时。用户可能会错过一些好的菜品。一些餐馆的菜单上有超过四十种菜品。在其他情况下，分类法甚至可能更大。例如，Netflix、Amazon Prime 或其他各种流媒体视频服务上的视频。

或者，当相同的词出现在多个项目中时。这甚至适用于小型分类法。想象一下访问一家泰国餐馆，该餐馆有几个主菜中包含了词汇 *curry*。假设用户点了其中一道菜。下次，用户会看到其他咖喱菜品被显著展示。这很有道理，对吧？

**词袋模型**

既然我们决定对用户偏好进行建模，就让我们开始吧。我们将把它们建模为词袋。

词袋模型是一个词频对的集合。一个例子是 { *beer* :10, *wine* :2 }。我们可以将这个例子解释为用户偏好啤酒而非葡萄酒。

虽然我们在这里使用了“词”这个术语，但我们可以根据需要定义它。例如，我们可以把术语“wheat beer”表示为“wheat-bear”。

我们将把*BagOfWords*建模为一个类。我们将用一个哈希来表示词袋，即 Python 的*Dict*。键是单词，值是它们的频率。

我们将如何具体使用*BagOfWords*？在实例化*UserInteractionsWithTaxonomy*时，我们还将创建一个*BagOfWords*实例来建模用户的关键词偏好。当用户对某个叶子进行操作时，我们将把该叶子名称的词袋添加到代表用户偏好的词袋中。为此，*BagOfWords* 类需要支持一个“add”方法，用于将提供的词袋添加到实例的词袋中。

这是我们的初始版本的 BagOfWords。

```py
class BagOfWords:

  def _init_(self):
    # Create self.bow

  def add(self, bow):
    for word in bow:
      self.bow[word] += bow[word]
```

这是*用户与分类法交互*的版本，重构以建模她在分类法中的交互中的用户偏好。

```py
class UserInteractionsWithTaxonomy():

  def _init_(self, taxonomy):
    …
    self.user_likes = BagOfWords()

  def act(self, leaf):
    …
    self.user_likes.add(leaf.name.bow)
```

**使用用户偏好**

现在我们完成了首个版本的用户偏好建模和训练，让我们讨论一下如何使用它。

我们从回顾*UserInteractionsWithTaxonomy*中的*score*方法开始。它对特定子项进行评分，以区分给定父项的各种子项，用于导航目的。

现在假设我们想要增强评分功能，以便除了订单数量外，还使用用户偏好。

在我们当前的设计中，用户偏好被建模为在*用户与分类法交互*实例中维护的单一词袋。鉴于我们打算如何使用评分函数，即区分一个父项的各种子项，如果每个父项拥有自己的用户偏好会更自然。这意味着分类法中的每个内部节点都应该有自己的用户偏好。

这将使我们可以将 UserInteractionsWithTaxonomy.score 重构如下。

```py
class UserInteractionsWithTaxonomy:

  …

  def score(self,child,parent, pcm = 1):
    …
    if it makes sense to score off user likes and not order frequencies:
      return self.user_likes[parent].score(self.user_likes[child])
    …
```

我们不会详细说明“是否有意义基于用户喜欢而非订单频率进行评分”这一点。这需要一些讨论。

从单一用户偏好模型转向节点特定的用户偏好模型乍看之下可能令人望而生畏。经过进一步考虑，首先，这种变化并不像最初看起来那样令人生畏；其次，它实际上可以使评分更加准确。

详细说明一下，实际中许多分类法会很小。例如，即使是一个全方位服务的餐厅的分类法，通常也只有四个内部节点，不包括根节点：*饮料*、*开胃菜*、*主菜*和*甜点*。

详细说明一下，某些词可能跨越节点边界。在某个餐厅中，例如，*三文鱼*这个词出现在开胃菜的名称中，也出现在主菜的名称中。假设从某个用户的订单历史中，我们发现她喜欢三文鱼开胃菜但不喜欢三文鱼主菜。当她寻找开胃菜时，我们应该显著展示三文鱼开胃菜。当她寻找主菜时，我们则不应显著展示三文鱼主菜。

既然我们决定继续进行这种建模增强，我们需要重构*__init__*()以构造节点特定的*BagOfWords*实例，并适当地更新*act*(.)。以下是重构后的版本。

```py
class UserInteractionsWithTaxonomy:

  def _init_(self, taxonomy):
    …
    self.user_likes = {}
    for each internal node in the taxonomy:
      self.user_likes[node] = BagOfWords()

  def act(self, leaf):
    …
    for each node that is an ancestor of leaf:
      self.user_likes[node].add(leaf.name.bow)
```

**何时基于用户偏好而非订单频率进行评分**

现在让我们讨论一下

```py
if it makes sense to score off user likes and not order frequencies:
  return self.user_likes[parent].score(self.user_likes[child])
```

部分。

当*自我*.*计数器*[*父项*]的值足够大时，基于订单频率来进行评分比基于用户偏好更好。这是因为实际订单通常应该比用户偏好具有更高的权重。某个特定项目中可能有些东西未被其名称捕捉到，从而解释了用户为何频繁订购它。或者避免它。也许是口味。

当*self*.*counter*[*parent*]的值不够大时，我们有几个选择。我们可以基于分类模型和用户的订单历史来打分。或者我们可以根据用户的偏好来打分。

用简单的语言来说，上一段的选项可以理解为以下内容。

对某个项目或类别与其父类别进行评分，依据

+   其他人在这家餐厅的体验。

+   用户自己在这个父类下的体验，可能结合其他用户的体验，点了与这个词有一些相同的词的项目。

让我们用一个例子来说明这两者。假设一个用户第一次访问某个泰国餐馆。如果用户知道某道主菜非常受欢迎，她可能会有兴趣点这道菜。如果用户知道餐馆中的主菜中含有*咖喱*这个词很受欢迎，那么所有这些主菜都应该为这个用户打高分。

**BagOfWords 中的评分方法**

首先，让我们提醒自己我们在*UserInteractionsWithTaxonomy*.*score*(.)中添加的相关部分：

```py
if it makes sense to score off user likes and not order frequencies:
  return self.user_likes[parent].score(self.user_likes[child])
```

我们看到我们需要向*BagOfWords*添加一个*score*()方法。调用*BagOfWords*.*score*(*bag_of_words*)将把被调用的对象解释为父对象，把传递给它的词袋解释为某个子对象。

我们之所以提到这一点，是因为这里存在固有的不对称性。我们不是试图以对称的方式对两个词袋进行评分。相反，我们寻求评分一个特定子词袋在其父词袋中的适配程度。

让我们看一个例子来解释这一点，同时给出一些对我们评分函数的具体要求。

考虑一个有*啤酒*类别并且有多个项目的餐厅。假设一个用户访问过餐厅几次，每次都点了淡啤酒。假设*淡*和*啤酒*这两个词在这些点过的项目中明确出现。用户在*啤酒*类别下的词袋现在将包含这两个词。

假设在啤酒菜单中有一个名为*Coors Light*的项目，但用户在之前的访问中没有点过这个项目。这个项目应该得高分，因为它是一种淡啤酒。即使用户还没有点过包含*Coors*这个词的啤酒。

现在考虑啤酒类别下的第二个项目，名为*Coors Beer*。它的评分不应该像*Coors Light*那么高。为什么呢？因为*啤酒*这个词在*啤酒*类别中的信息量不如*淡*。假设这两个词在用户的*啤酒*类别词袋中的频率相同，那么*淡*应该胜出。

现在，让我们朝着一个能满足我们需求的评分函数努力。

我们的第一个决定是仅对那些在子词袋和父词袋中都存在的词进行评分。对于我们的两个例子，如下所示。

```py
{ coors, light } and { light, beer } ⇒ light
{ coors, beer } and { light, beer}   ⇒ beer
```

我们的第二个决定是独立并且加法地评分这个交集中的词。

我们的第三个决定是对交集中的任何单词 *w* 进行如下评分

*f*(*w*, *child*)*log *P*(*w*|*parent*, *user*)/*P*(*w*|*parent*, *uniform user*)

这里 *f*(*w*, *child*, *user*) 是用户的子词袋中 *w* 的频率。

*P*(*w*|*parent*, *user*) 是 *f*(*w*, *parent*, *user*)/sum_{*w*’ in *parent*} *f*(*w*’, *parent*, *user*)

*P*(*w*|*parent*, *uniform user*) 类似，只是用户是我们所说的“均匀用户”。均匀用户被假定为对 *child* 下的任何项都有相等的订购概率。

在关联规则挖掘中，*P*(*w*|*parent*, *user*)/*P*(*w*|*parent*, *uniform user*) 是所谓的提升 [3] 的一种版本。

让我们看看这些在我们的示例中是如何工作的。下图展示了这个用户对单词 *light* 和 *beer* 的评分。为了简化表达，省略了术语 *parent*。

log *P*(*light*|*user*)/*P*(*light*|*uniform user*)

log *P*(*beer*|*user*)/*P*(*beer*|*uniform user*)

*P*(*light*|*user*) 和 *P*(*beer*|*user*) 各为 ½。假设菜单上包含单词 *beer* 的啤酒比包含单词 *light* 的啤酒多。*P*(*light*|*uniform user*) 可能会显著小于 *P*(*beer*|*uniform user*)。因此，log *P*(*light*|*user*)/*P*(*light*|*uniform user*) 可能会显著大于 log *P*(*beer*|*user*)/*P*(*beer*|*uniform user*)。这是预期中的结果。

让我们在伪代码中查看完整的得分函数。实现中的一个重要细节是评分将分布在两个类之间：*BagOfWords* 和 *UserInteractionsWithTaxonomy*。首先，让我们查看两个类中的实际更改，然后再解释原因。

让我们从重构后的 *BagOfWords* 开始。由于更改并不是局部的，我们将在这里全面展示类的新版本。

```py
class BagOfWords:

  def _init_(self):
    # Create self.bow
    n = 0

  def add(self, bow):
    for word in bow:
      self.bow[word] += bow[word]
      n += bow[word]

  def p(self, w):
    return float(self.bow[w])/n

  def score(self, user_bow):
    words_in_common = set(user_bow.keys()) and set(self.bow.keys())
    score = 0
    for w in words_in_common:
      score += user_bow[w]*log(self.p(w))
    return score
```

现在让我们引入重构后的 UserInteractionsWithTaxonomy 类。

```py
class UserInteractionsWithTaxonomy:

  def __init__(self, taxonomy, taxonomy_model = None, uniform_acts = None):
    …
    self.taxonomy_model = taxonomy_model
    self.uniform_acts   = uniform_acts
    …

  def score_using_user_preferences(self, child, parent):
    user_score = self.user_likes[parent].score(self.user_likes[child])
    uniform_score = self.uniform_acts.user_likes[parent].
      score(self.user_likes[child])
    return user_score - uniform_score

  def score(self,child,parent, pcm = 1):
    …
    if it makes sense to score off user likes and not order frequencies:
      return self.score_using_user_preferences(child, parent)
    …
```

**扩展用户喜好模型**

到目前为止，我们已经将用户喜好建模为词袋。在许多用例中，分类法的叶子上可能还有其他属性，这些属性可能会影响用户的喜好。例如 *price*。

为了超越词袋的模型，将用户喜好建模为一个独立的类 *UserLikes* 是合理的。我们还需要重构 *UserInteractionsWithTaxonomy* 以使用 *UserLikes*。

首先，让我们回顾一下 *BagOfWords* 在 *UserInteractionsWithTaxonomy* 中的使用情况，因为这是我们需要推广的内容。

```py
class UserInteractionsWithTaxonomy:

  def _init_(self, taxonomy):
    …
    …
    self.user_likes[node] = BagOfWords()

  def act(self, leaf):
    …
    for each node that is an ancestor of leaf:
      self.user_likes[node].add(leaf.name.bow)

  def score_using_user_preferences(self, child, parent):
    user_score = self.user_likes[parent].score(self.user_likes[child])
    uniform_score = self.uniform_acts.user_likes[parent].
      score(self.user_likes[child])
    return user_score - uniform_score 
```

很明显，我们需要进行以下更改：

```py
self.user_likes[node] = BagOfWords() ⇒
self.user_likes[node] = UserLikes()

self.user_likes[node].add(leaf.name.bow) ⇒
self.user_likes[node].add(leaf)
```

假设这些更改已经完成。

现在让我们讨论一下对 *UserLikes* 类的影响。

它需要支持一个方法 *add*(*leaf*)，该方法从 *leaf* 中提取所需的所有属性，并将它们添加到当前的用户喜好模型中。

它需要支持一个方法 *score*，以根据 *self*（即父节点）对任何给定子节点的用户喜好进行评分。

我们几乎准备好查看*UserLikes*类了。在此之前，让我们建模一个额外的属性，因为这就是我们首先构建此类的原因。

让我们从价格开始。首先，我们需要讨论其建模方面。

**价格基础建模**

想象一下访问一家餐馆，其菜单如下所示。这里只显示了部分。对于这些项目，仅显示价格。

*饮料*：$2, $3, $2, $**8**, $**9**, … *主菜*：$8, $9, $**22**, $**28**, $9, $8, …

“…”是为了表示每个类别中还有许多其他项目。

加粗的项目是特定用户在这家餐馆过去订购的项目。由此，我们可以推断出至少在这家餐馆中，这位用户偏好每个类别中相对较贵的项目。（当然，可能还有其他因素，但这就是我们从这些数据中可以得出的结论。）

这些信息可以用来在用户下次访问这家餐馆时更显著地展示某些项目。也许以不同的顺序展示，如下所示。

*饮料*：$**8**, $**9**, $2, $3, $2, … *主菜*：$**22**, $**28**, $8, $9, $9, $8, …

**价格详细建模**

我们可以像处理项目名称中的单词一样处理价格。这很好地融入了我们当前的设计。每个内部节点都可以有自己的价格包，就像它有自己的单词包一样。

节点特定的价格包比节点特定的单词包更有动机。这是因为价格在预测的类别上比单词更不具体。用户可能不会在饮料上花费$8，但可能会在主菜上花费这么多。类别特定的价格包将允许我们建模这种区分。

现在，我们将价格包建模为*BagOfWords*的一个实例。这将满足我们基本的需求。即，对于每个类别，我们记录用户在该类别中订购的项目价格及其频率。

**UserLikes**

现在是伪代码部分。

```py
class UserLikes:

  def __init__(self):
    self.bow = BagOfWords()
    # Additional attributes
    self.prices = BagOfWords()

  def add(self, leaf):
    self.bow.add(leaf.name.bow)
    self.prices.add(leaf.price.as_bow())

  def score(self, user_likes):
    # This needs some modeling discussion first.
```

注意*leaf*.*price*.*as_bow*。 “as_bow”是指价格（一个标量）已被表示为一个包含单一键（价格）及其频率为 1 的单词包。

**UserLikes.score(.)**

考虑一下这个。

```py
class UserLikes:

  …

  def score(self, user_likes):
    return self.bow.score(user_likes.bow) + 
           self.prices.score(user_likes.prices)
```

注意到*自我*是父级的*UserLikes*，在*score*(.)的参数中提供的*user_likes*是特定子级的，通过观察两次调用*score*(.)中的发生情况，我们可以将其表示为

```py
sum_{w appears in both parent.user_likes.bow and in child.user_likes.bow}
child.user_likes.bow[w]*log(parent.user_likes.bow.p(w)
+
sum_{p appears in both parent.user_likes.prices and in child.user_likes.prices}
child.user_likes.prices[p]*log(parent.user_likes.prices.p(p)
```

现在考虑到

*UserInteractionsWithTaxonomy*.*score_using_user_preferences*(*child*, *parent*)

它调用两次，一次如上所述，一次对不同的用户进行均匀交互。

因此，整体评分函数可以表示为

```py
(
  sum_{w appears in both parent.user_likes.bow and in 
    child.user_likes.bow}
    child.user_likes.bow[w]*log(parent.user_likes.bow.p(w)

+

  sum_{p appears in both parent.user_likes.prices and in 
    child.user_likes.prices}
    child.user_likes.prices[p]*log(parent.user_likes.prices.p(p)
)

-

(
  sum_{w appears in both parent.uniform_user.user_likes.bow and in 
    child.uniform_user.user_likes.bow}
    child.uniform_user.user_likes.bow[w]*log(parent.uniform_user.user_likes.bow.p(w)

+

  sum_{p appears in both parent.uniform_user.user_likes.prices and in 
    child.uniform_user.user_likes.prices}
    child.uniform_user.user_likes.prices[p]*log(parent.uniform_user_likes.prices.p(p)
)
```

让我们在一个稍作改进的场景中尝试理解这一点。假设有一家餐厅的分类为*啤酒*。一个特定的用户已经两次光顾这家餐厅，每次点的都是一些淡色啤酒。（两次点的啤酒不同。）假设“*淡色*”和“*啤酒*”这两个词显式出现在所点啤酒的名称中。

现在进行改进。假设两款点的啤酒分别定价为$5 和$6。现在考虑菜单上另外两款啤酒。两者均不包括在用户之前点过的两款啤酒中。

```py
Coors Light $5
? Light     $8
```

上述评分函数会给*Coors Light*更高的分数。这是因为虽然两者都包含“light”这个词，但第二款的价格显著高于用户之前点的啤酒。

我们可以通过替换进一步改进这一点

```py
p appears in both parent.user_likes.prices and in child.user_likes.prices
```

由

```py
p is in the price ranges of both parent.user_likes.prices and of child.user_likes.prices
```

我们不会在这篇文章中再多说了。

**总结**

在这篇文章中，我们深入探讨了优化用户在分类体系中的浏览，以便在叶子节点中找到感兴趣的项目。我们介绍了一种从用户操作的项目反馈中学习的算法。（在餐厅环境中，即用户实际点的项目。）

反馈被推送到分类体系的上层，以便内部节点可以从中受益。这使得这些节点可以被更（或更少）优先显示，以便用户在未来更容易找到感兴趣的项目。

“感兴趣的项目”包括用户过去操作过的项目，以及足够相似且值得推荐的项目。

初始模型基于按分类体系聚合的订单频率。通过增强模型以模拟随机行为并使用从用户集体参与分类体系中不断学习的分类模型，也解决了各种冷启动问题。

初始模型随后得到增强，以模拟和使用用户偏好，这些偏好可以从用户操作的项目名称中的词汇和其价格中辨别出来。像初始模型一样，用户偏好被映射到分类体系中。这适应了在特定分类体系中的细致学习。例如

+   用户愿意为这家餐厅的主菜支付$8，但不愿为饮料付费。

+   用户喜欢这家餐厅的三文鱼开胃菜，但不喜欢三文鱼主菜。

在这篇文章中，建模和评分是通过 Python 伪代码构建的。在各个阶段，当需要时，伪代码会进行重构。在这篇文章中，Python 伪代码作为沟通和讨论想法的语言。它也可能对那些希望练习实现的读者有用。伪代码的模块化程度足够高，可以逐步实现。

**参考文献**

1.  [向量空间模型 — 维基百科](https://en.wikipedia.org/wiki/Vector_space_model#:~:text=Vector%20space%20model%20or%20term,retrieval%2C%20indexing%20and%20relevancy%20rankings)

1.  [杰卡德指数 — 维基百科](https://en.wikipedia.org/wiki/Jaccard_index)

1.  [关联规则学习 — 维基百科](https://en.wikipedia.org/wiki/Association_rule_learning#Lift)

1.  [冷启动（推荐系统） — 维基百科](https://en.wikipedia.org/wiki/Cold_start_(recommender_systems)#:~:text=Cold%20start%20is%20a%20potential,not%20yet%20gathered%20sufficient%20information)
