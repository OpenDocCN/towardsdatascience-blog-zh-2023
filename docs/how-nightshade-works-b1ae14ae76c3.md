# Nightshade 的工作原理

> 原文：[https://towardsdatascience.com/how-nightshade-works-b1ae14ae76c3?source=collection_archive---------1-----------------------#2023-11-03](https://towardsdatascience.com/how-nightshade-works-b1ae14ae76c3?source=collection_archive---------1-----------------------#2023-11-03)

## 将生成图像的人工智能与被污染的数据混淆

[](https://medium.com/@doriandrost?source=post_page-----b1ae14ae76c3--------------------------------)[![Dorian Drost](../Images/1795395ad0586eafd83d3e2f7b975ca8.png)](https://medium.com/@doriandrost?source=post_page-----b1ae14ae76c3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b1ae14ae76c3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b1ae14ae76c3--------------------------------) [Dorian Drost](https://medium.com/@doriandrost?source=post_page-----b1ae14ae76c3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1d49ea537d1c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-nightshade-works-b1ae14ae76c3&user=Dorian+Drost&userId=1d49ea537d1c&source=post_page-1d49ea537d1c----b1ae14ae76c3---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b1ae14ae76c3--------------------------------) ·10 分钟阅读·2023年11月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb1ae14ae76c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-nightshade-works-b1ae14ae76c3&user=Dorian+Drost&userId=1d49ea537d1c&source=-----b1ae14ae76c3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb1ae14ae76c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-nightshade-works-b1ae14ae76c3&source=-----b1ae14ae76c3---------------------bookmark_footer-----------)![](../Images/06b719db201217808f3920a3b86de4c9.png)

就像城堡的高墙一样，*Nightshade* 可以成为保护知识产权不受非法使用的一种方式。照片由 [Nabih El Boustani](https://unsplash.com/@nounouis?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

最近出现的 [*Nightshade*](https://arxiv.org/abs/2310.13828) 算法能够创建用于混淆图像生成 AI 模型的污染数据，给有关此类模型的对抗攻击讨论注入了新的活力。这一讨论也受到伦理和社会考量的影响，因为这些攻击可能为艺术家、内容创作者和其他人提供了反击的方式，如果他们觉得 AI 模型未经许可使用了他们的内容，但也可能被用于不良意图。

在这篇文章中，我想解释 Nightshade 的核心概念。为此，我将首先解释数据中毒的一般思路，并突出其不足之处。然后，我将向你介绍 Nightshade，一种克服幼稚方法一些缺点的算法。最后，我将简要讨论一些来自其使用的伦理考虑。

## 数据中毒

![](../Images/af49aea3e5964e2f1d180c6f498af741.png)

毒性还是无毒？图片由 [Fiona Smallwood](https://unsplash.com/@thepeoplesdigital?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

让我们从数据中毒的概念开始说起。假设你想影响图像生成 AI，使其无法生成某些类型的图像，或者无法理解某些提示。你为什么要这样做？最可能的非破坏性原因可能是，你是一个艺术家，想避免图像生成模型能够生成你的风格的图像，或者你创造了一个新的漫画角色，该角色在未经你许可的情况下不应由图像生成模型再现。

那么，你该怎么做呢？让我们从了解生成 AI 学习的基本概念开始。显然，图像生成 AI 依赖于其训练数据。具体来说，它依赖于存在展示某个概念（比如 *狗*）的图像，并且这些图像与描述其内容的文本（例如像 *带眼镜的可爱狗* 这样的图像说明）相关联。基于此，它学习提取图像共享的某些视觉属性，这些图像的说明中也包含某些关键词。也就是说，模型通过学习提到 *狗* 的所有图像的属性来学习 *狗的样子*。

现在，如果你引入显示狗的图像，但这些图像的说明总是提到 *猫*，会发生什么呢？最终，*狗* 和 *猫* 只是图像中可以看到的东西的符号。如果显示狗的图像被标记为猫，模型将只是学到符号 *猫* 代表我们称之为 *狗* 的东西。如果模型没有任何英语语言的先验知识，如何知道这些标签是错误的，即使它们是如此一致？如果你不懂德语，我给你展示一百张狗的图像，并告诉你它们的标签是 *Katze*，你会假设 *Katze* 是德语中的 *狗*。你不会知道实际的德语单词 *狗* 是 *Hund*，而 *Katze* 意思是 *猫*，因为你只是学习了标签与图像属性之间的关联。

刚才描述的过程被称为 *数据中毒*，源于你引入对模型训练具有恶意影响的数据实例的想法（就像毒药对你的健康有恶意影响一样）。

## 幼稚的数据中毒攻击

![](../Images/8632fa35f988b5aed9475c9f689dd709.png)

一只戴着眼镜的可爱狗狗，想着如何攻击图像生成模型。照片由[Jamie Street](https://unsplash.com/@jamie452?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

作为一种简单的方法，你可以使用上述想法来混淆像Stable Diffusion这样的机器学习模型。假设你想让Stable Diffusion在被提示为狗时生成猫的图片。为此，你需要创建许多猫的图片，将它们标记为*狗*，并将它们上传到互联网。然后你希望这些图片被抓取用于Stable Diffusion模型的下次训练。

如果你许多图片成为下次训练的一部分，这确实可能导致猫和狗之间的混淆。然而，这种方法也有一些缺点：

+   你需要很多图片。由于有许多其他未中毒的猫的图片，你需要大量的图片才能产生影响。如果你只提供10张中毒图片，而另一边有1000张未中毒的猫的图片，你对训练几乎没有影响。通常，你需要对所有图片中20%或更多的图片进行中毒才能产生效果。

+   请注意，你不知道哪些图片会成为训练的一部分。因此，如果你想将500张中毒图片引入训练中，你可能需要创建5000张并将它们散布在整个互联网，因为只有其中的一些可能会实际被抓取用于训练。

+   如果你上传标记为*狗*的猫的图片，人类可以很容易地识别出来。在使用你的图片进行训练之前，它们可能会被质量门槛过滤掉（无论是人类还是专门的AI）。

## Nightshade

![](../Images/89ec939d3c8c67ec4065cc2af1df3fee.png)

Nightshade算法的名字源自一种非常毒性的植物。照片由[Georg Eiermann](https://unsplash.com/@georgeiermann?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

现在让我们来看看*Nightshade*，这是一种旨在克服这些缺点的算法。为此，Nightshade使用两个关键概念：它创建对模型产生最大影响的图片（这意味着总共需要更少的图片），以及这些图片在人类眼中无法与非中毒图片区分开来。

首先，如何从图像中获得最大效果？理论上，你会希望使用那些在训练过程中导致梯度变化最大的图像。然而，要找出这些图像，你需要观察训练过程，这在一般情况下是做不到的。不过，Nightshade 的作者提出了一种不同的解决方案：你可以使用一个由你想要破坏的模型生成的图像。也就是说，如果你想让猫的图像被标记为狗，你可以用一个简单的提示词来提示模型，比如*一只猫的图像*。模型生成的图像将是它理解为猫的一个非常典型的表现。如果这个图像在训练中出现，它将对猫的概念理解产生很高的影响（比那些不典型的猫图像要高得多）。因此，如果你破坏这个图像，你将对模型的训练产生非常大的影响。

其次，我们说过，Nightshade 的图像应该与未被破坏的图像难以区分。为了达到这个目标，Nightshade 采用自然图像并施加扰动（即像素值的小变化），直到图像被模型以不同方式感知。继续使用我们之前的狗与猫的例子，我们会取一个由模型生成的显示猫的图像。这个图像我们称之为*锚图像*或接下来的公式中的 xᵃ。接下来，我们取一张非常典型的狗图像，我们称之为 xₜ。现在，我们对这张图像 xₜ 添加扰动 δ，使其优化以下目标：

![](../Images/9a43af97d7fafd49aa3a4825c482c826.png)

其中 *F()* 是模型使用的图像特征提取器，*Dist* 是距离函数，*p* 是 δ 的上界，以避免图像变化过大。这意味着我们希望找到 δ，使得扰动后的狗图像（F(xₜ + δ)）和锚图像（显示猫的 F(xᵃ)）的特征之间的距离尽可能小。换句话说，我们希望从模型的角度来看，这两个图像是相似的。请注意，F(x) 是特征提取器的结果，它是模型在特征空间中*看见*的图像，这与你在像素空间中看到的图像不同。

在接下来的图像中，你将无法发现原始图像和被破坏图像之间的任何差异（至少我看不到）。然而，在它们的特征空间中，它们的差异很大。例如，被破坏的狗图像的特征非常接近猫图像的特征，因此对于模型来说，它几乎看起来像一只猫。

![](../Images/3ab1ef415b193785dc81a66adfde6204.png)

两个被破坏图像的例子。下排的图像是上排图像的扰动版本。尽管人眼无法看出差异，但从模型的角度看，原始图像和被破坏的图像非常不同。图像取自 Nightshade 论文[1]。

使用这种技术，我们能够生成对模型训练产生巨大影响且无法被检测为被毒化的图像。如果你将这些图像上传到互联网上，没有人会对其产生怀疑，因此这些图像很不可能被任何质量门槛过滤掉。此外，由于这些图像的威力如此之大，你不需要像采用简单方法时那样毒化20%的狗图像。使用Nightshade，通常50到100张图像就足以破坏模型在特定概念上的表现。

## 泛化能力

除了我们刚才看到的要点外，Nightshade还有另一个有趣的优势，即其多种方式的概括能力。

首先，毒化某个关键词也会影响那些在语言或语义上相关的概念。例如，毒化*狗*的图像也会影响像*小狗*或*哈士奇*这样的关键词，这些概念与*狗*相关。在以下示例中，*狗*概念已被毒化，这也阻碍了*小狗*和*哈士奇*的生成。

![](../Images/d7ee38ee05cb93705b036d29642c55f7.png)

这是一个示例，说明了如何通过毒化一个概念（狗）也会阻碍相关概念（小狗、哈士奇、狼）的生成。图像摘自Nightshade论文[1]。

同样地，毒化一个如*幻想*的概念也会影响语义上相关的概念，但不会影响其他概念，如下例所示。你可以看到，像*龙*这样的概念，它与被毒化的*幻想*相近，受到了影响，而像*椅子*这样的概念则没有。

![](../Images/cacb3836b0f65edcff5ece31a9a62e37.png)

这是一个示例，说明了如何通过毒化一个概念（幻想）也会阻碍相关概念（例如龙）的生成。注意，非相关概念（例如椅子）没有受到影响。图像摘自Nightshade论文[1]。

此外，当毒化多个概念时，生成图像的能力可能会完全崩溃。在以下示例中，100、250或500个概念已被毒化。随着毒化概念的增加，即使是那些完全未被毒化的其他概念（如*人*或*绘画*），其生成也会受到严重阻碍。

![](../Images/00245ac22b1a1cb87635c2fa244f56b9.png)

一个例子是如何毒化多个概念会阻碍生成图像的能力。注意，*人*、绘画和*海贝*这些概念并没有特别被毒化。图像摘自Nightshade论文[1]。

此外，Nightshade 的效果也会对不同的目标模型产生普遍影响。请记住，我们使用了我们想攻击的模型来生成锚定图像，这帮助我们构建了被污染的图像。其背后的想法是，这些图像具有很强的原型特征，因此会对训练产生强烈影响。我们还需要访问特征提取器来优化扰动。自然，Nightshade 的影响在这些锚定图像由被攻击的模型生成，并且该模型的特征提取器可以用于优化时最强。然而，即使锚定图像和特征提取器来自其他模型，中毒效果也相当好。也就是说，你可以使用例如 Stable Diffusion 2 来生成你的中毒图像，即使你想攻击的是 Stable Diffusion XL。如果你没有访问实际想攻击的模型，这可能会很有趣。

## 伦理问题

到目前为止，我介绍了 Nightshade 作为一种可以被内容创作者用来防御其知识产权不被非法使用的方法。然而，正如硬币有两面一样，数据中毒也可以被用来有害地，可能是有意的，也可能是无意的。无需多言，数据中毒可以用来故意扰乱生成式 AI 模型，给其创建者造成财务损失，并妨碍科学研究。一家 AI 公司为了提升自己的模型而摧毁竞争对手的训练数据，只是数据中毒恶意使用的无数例子之一。然而，即使你只是想保护自己的内容，我们刚刚看到，大量的概念中毒会妨碍 AI 生成图像的能力。因此，如果很多人使用 Nightshade，这可能会摧毁图像生成 AI 对那些本应合法使用的概念的生成能力。因此，即使是为了保护自己的内容，使用 Nightshade 的内容创作者也可能造成不必要的损害。是否以及在多大程度上接受这种附带损害，是一个值得热烈讨论的问题。

更重要的是，正如你可以想象的那样，攻击生成式 AI 的能力是一场不断起伏的战斗。每当新的攻击手段被发明出来，另一方便会提出新的防御机制。虽然作者声称 Nightshade 对常见的防御机制（例如，使用专门分类器检测被污染的图像或其他属性）相当强健，但只不过是时间问题，直到新的防御措施被发现来对抗 Nightshade。从这个角度来看，Nightshade 可能暂时允许创作者保护他们的内容，但最终可能会变得过时。

## 总结

正如我们刚刚看到的，Nightshade 是一种创建中毒数据集的算法，它超越了用错误标签标记数据的幼稚方法。它创建的图像不会被人类识别为中毒图像，即使示例数量较少，也能严重影响图像生成 AI。这大大增加了中毒图像成为训练数据的一部分并产生影响的可能性。此外，它承诺在多种方式上进行泛化，这使得攻击更加强大且不易防御。因此，Nightshade 提供了一种新的方式来反击未经其创作者许可用于模型训练的不正当使用，但也包括了潜在的破坏性使用，因此需要对其伦理影响进行讨论。若出于高尚意图使用，Nightshade 可以帮助保护知识产权，例如艺术家的风格或发明。

## 来源

这是介绍 Nightshade 的原始论文：

+   [1] Shan, S., Ding, W., Passananti, J., Zheng, H., & Zhao, B. Y. (2023). Prompt-Specific Poisoning Attacks on Text-to-Image Generative Models. *arXiv preprint arXiv:2310.13828*.

*喜欢这篇文章？* [*关注我*](https://medium.com/@doriandrost) *以便获取我未来的帖子通知。*
