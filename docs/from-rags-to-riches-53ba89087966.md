# 从 RAG 到财富

> 原文：[https://towardsdatascience.com/from-rags-to-riches-53ba89087966?source=collection_archive---------4-----------------------#2023-10-25](https://towardsdatascience.com/from-rags-to-riches-53ba89087966?source=collection_archive---------4-----------------------#2023-10-25)

## 向量搜索在深入理解您的数据和模型中的 10 种应用

[](https://medium.com/@jacob_marks?source=post_page-----53ba89087966--------------------------------)[![Jacob Marks, Ph.D.](../Images/94d9832b8706d1044e3195386613bfab.png)](https://medium.com/@jacob_marks?source=post_page-----53ba89087966--------------------------------)[](https://towardsdatascience.com/?source=post_page-----53ba89087966--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----53ba89087966--------------------------------) [Jacob Marks, Ph.D.](https://medium.com/@jacob_marks?source=post_page-----53ba89087966--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff7dc0c0eae92&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-rags-to-riches-53ba89087966&user=Jacob+Marks%2C+Ph.D.&userId=f7dc0c0eae92&source=post_page-f7dc0c0eae92----53ba89087966---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----53ba89087966--------------------------------) ·10 min read·2023年10月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F53ba89087966&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-rags-to-riches-53ba89087966&user=Jacob+Marks%2C+Ph.D.&userId=f7dc0c0eae92&source=-----53ba89087966---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F53ba89087966&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-rags-to-riches-53ba89087966&source=-----53ba89087966---------------------bookmark_footer-----------)![](../Images/bdbf65ccdee0abe12ca276688bfdacda.png)

向量搜索用于数据探索的艺术效果图。图像由 DALLE-3 生成。

随着大型语言模型（LLMs）席卷全球，向量搜索引擎也随之而来。向量数据库构成了 LLMs 长期记忆系统的基础。

通过高效地查找相关信息并将其作为上下文输入到语言模型中，向量搜索引擎可以提供超出训练截止日期的最新信息，并在不进行微调的情况下提高模型输出的质量。这个过程，通常称为检索增强生成（RAG），将曾经晦涩的近似最近邻（ANN）搜索算法挑战推向了聚光灯下！

在所有的喧嚣中，人们可能会误以为向量搜索引擎与大型语言模型不可分割。但事实远不止于此。向量搜索有许多强大的应用，远远超出了改善 RAG 对 LLMs 的应用！

在这篇文章中，我将展示我最喜欢的十种向量搜索的应用，用于数据理解、数据探索、模型解释等。

以下是我们将涵盖的应用，按复杂度大致递增的顺序：

+   [图像相似性搜索](#e595)

+   [反向图像搜索](#dd35)

+   [对象相似性搜索](#26bd)

+   [强健的 OCR 文档搜索](#c20f)

+   [语义搜索](#8c7d)

+   [跨模态检索](#23f9)

+   [探测感知相似性](#c565)

+   [比较模型表示](#e42e)

+   [概念插值](#391f)

+   [概念空间遍历](#6967)

# 图像相似性搜索

![](../Images/c65fcd3032940efee4d09558aface781.png)

*对来自* [*Oxford-IIIT Pet 数据集的图像进行相似性搜索*](https://www.robots.ox.ac.uk/~vgg/data/pets/) *(*[*LICENSE*](https://www.robots.ox.ac.uk/~vgg/data/pets/#:~:text=License)*). 图片由作者提供。*

也许最简单的起点是图像相似性搜索。在这个任务中，你有一个由图像组成的数据集——这可以是个人照片集，也可以是数以亿计的图像仓库，这些图像由数千个分布式相机拍摄，历时多年。

设置很简单：计算数据集中每张图像的嵌入向量，并从这些嵌入向量生成一个向量索引。在这初始批次计算之后，不需要进一步的推断。探索数据集结构的一个好方法是选择数据集中的一张图像，并查询向量索引获取 `k` 个最近邻——最相似的图像。这可以直观地了解查询图像周围图像空间的密度。

欲了解更多信息和示例代码，请见 [这里](https://docs.voxel51.com/user_guide/brain.html#image-similarity)。

# 反向图像搜索

![](../Images/799b67f1470b0370e9d9fda684c7ba8c.png)

*对来自* [*Unsplash 的图像进行反向图像搜索*](https://images.unsplash.com/photo-1568034097584-a3063e104ac3?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=3464&q=80) *(图片由 Mladen Šćekić 提供)*，与* [*Oxford-IIIT Pet 数据集*](https://www.robots.ox.ac.uk/~vgg/data/pets/)*进行对比。图片由作者提供。*

类似地，图像相似性搜索的自然延伸是找到数据集中与*外部*图像最相似的图像。这可以是来自本地文件系统的图像，也可以是来自互联网的图像！

要执行反向图像搜索，你需要像图像相似性搜索示例那样创建数据集的向量索引。不同之处在于运行时，你计算查询图像的嵌入向量，然后用这个向量查询向量数据库。

欲了解更多信息和示例代码，请参见 [这里](https://github.com/jacobmarks/reverse-image-search-plugin)。

# 对象相似性搜索

![](../Images/6b1842613c97550a31e177d205bec030.png)

*在* [*COCO-2017 数据集*](https://cocodataset.org/#home)*的验证分割中进行羊的对象相似性搜索 (*[*LICENSE*](https://viso.ai/computer-vision/coco-dataset/#:~:text=the%20MS%20COCO%20images%20dataset%20is%20licensed%20under%20a%20Creative%20Commons%20Attribution%204.0%20License)*）。图片由作者提供。*

如果你想深入研究图像*中的*内容，那么对象或“补丁”相似性搜索可能正是你所追求的。例如，人物再识别就是其中之一，在这种情况下，你有一张包含目标人物的单一图像，你想在数据集中找到所有该人的实例。

人物可能只占据每张图像的小部分，因此这些图像中包含的整个图像的嵌入可能会强烈依赖于这些图像中的其他内容。例如，图像中可能有*多个*人。

更好的解决方案是将每个对象检测补丁视为一个独立的实体，并为每个补丁计算嵌入。然后，使用这些补丁嵌入创建一个向量索引，并针对你想要重新识别的人的补丁进行相似性搜索。作为起点，你可能想尝试使用 ResNet 模型。

这里有两个细微之处：

1.  在向量索引中，你需要存储元数据，将每个补丁映射回数据集中的相应图像。

1.  你需要运行一个对象检测模型来生成这些检测补丁，然后再实例化索引。你可能还想仅为某些对象类别（如`person`）计算补丁嵌入，而不是其他类别——`chair`、`table`等。

欲了解更多信息和示例代码，请参见 [这里](https://docs.voxel51.com/user_guide/brain.html#object-similarity)。

# 强大的 OCR 文档搜索

![](../Images/2df3cf0f279735309d732e9f0d9ff25e.png)

*通过 Tesseract OCR 引擎生成的博士论文页面上的文本块进行模糊/语义搜索。使用 GTE-base 模型计算的嵌入。图片由作者提供。*

光学字符识别（OCR）是一种允许你数字化文档的技术，如手写笔记、旧期刊文章、医疗记录以及那些藏在你衣柜里的情书。像 [Tesseract](https://github.com/tesseract-ocr/tesseract) 和 [PaddleOCR](https://learnopencv.com/optical-character-recognition-using-paddleocr/) 这样的 OCR 引擎通过识别图像中的单个字符和符号并创建连续的“文本块”来工作——想象成段落。

一旦你有了这些文本，你可以对预测的文本块进行传统的自然语言关键词搜索，如[这里](https://github.com/jacobmarks/keyword-search-plugin)所示。然而，这种搜索方法容易受到单字符错误的影响。如果OCR引擎错误地将“l”识别为“1”，那么对“control”的关键词搜索将失败（这真是有趣！）。

我们可以通过使用向量搜索来克服这个挑战！使用像[GTE-base](https://huggingface.co/thenlper/gte-base)这样的文本嵌入模型将文本块嵌入，并创建一个向量索引。然后，我们可以通过嵌入搜索文本并查询该索引，对我们的数字化文档进行模糊和/或语义搜索。从高层次来看，这些文档中的文本块类似于目标相似性搜索中的目标检测补丁！

欲了解更多信息和示例代码，请见[这里](https://github.com/jacobmarks/semantic-document-search-plugin)。

# 语义搜索

![](../Images/8f8c589cf85a3bddb18ab437d7ffff8b.png)

*使用自然语言在COCO 2017验证数据集上进行语义图像搜索。图像由作者提供。*

使用多模态模型，我们可以将语义搜索的概念从文本扩展到图像。像[CLIP](https://github.com/openai/CLIP)、[OpenCLIP](https://github.com/mlfoundations/open_clip)和[MetaCLIP](https://github.com/facebookresearch/metaclip)这样的模型被训练以找到图像及其标题的共同表示，因此一张狗的图像的嵌入向量将与“狗的照片”这一文本提示的嵌入向量非常相似。

这意味着创建一个由CLIP嵌入生成的图像向量索引，并使用该向量数据库进行向量搜索是合理的（即我们“被允许”这样做），其中查询向量是*文本提示*的CLIP嵌入。

💡通过将视频中的每一帧视作图像，并将每帧的嵌入添加到向量索引中，你还可以[对视频进行语义搜索](https://medium.com/voxel51/a-google-search-experience-for-computer-vision-data-voxel51-a9ee41390986#:~:text=Find%20video%20frames%20with%20cars%20in%20an%20intersection)！

欲了解更多信息和示例代码，请见[这里](https://docs.voxel51.com/user_guide/brain.html#text-similarity)。

# 跨模态检索

*跨模态检索与输入音频文件匹配的图像。使用ImageBind和Qdrant向量索引实现，基于COCO 2017验证数据集。视频由作者提供。*

从某种意义上说，对图像数据集进行语义搜索是一种跨模态检索。一个概念化的方法是我们正在检索与文本查询对应的图像。使用像[ImageBind](https://github.com/facebookresearch/ImageBind)这样的模型，我们可以更进一步！

ImageBind 将来自六种不同模态的数据嵌入到同一嵌入空间中：图像、文本、音频、深度、热成像和惯性测量单元（IMU）。这意味着我们可以为*任何*这些模态的数据生成一个向量索引，并用这些模态中的任何其他样本来查询该索引。例如，我们可以用一段汽车鸣笛的音频片段来检索所有汽车的图像！

欲了解更多信息和工作代码，请查看[这里](https://github.com/jacobmarks/audio-retrieval-plugin)。

# 探测感知相似性

在向量搜索故事中，我们迄今为止只略微提及了一个非常重要的部分，那就是*模型*。我们向量索引中的元素是来自模型的嵌入。这些嵌入可以是定制嵌入模型的最终输出，也可以是从另一个任务（如分类）训练的模型中提取的隐藏或*潜在*表示。

无论如何，我们用来嵌入样本的模型可以对哪些样本被认为是最相似的样本产生重大影响。CLIP 模型捕捉语义概念，但在表示图像中的结构信息方面存在困难。另一方面，ResNet 模型非常擅长表示结构和布局的相似性，操作在像素和补丁的层面上。然后，还有像[DreamSim](https://dreamsim-nights.github.io/)这样的嵌入模型，旨在弥合差距，捕捉中级相似性 — 将模型的相似性概念与人类的感知对齐。

向量搜索为我们提供了一种探测模型如何“看待”世界的方法。通过为每个我们感兴趣的模型（在相同数据上）创建一个单独的向量索引，我们可以迅速培养对不同模型如何在背后表示数据的直觉。

下面是一个示例，展示了在 NIGHTS 数据集中，使用 CLIP、ResNet 和 DreamSim 模型嵌入进行相似性搜索的情况：

![](../Images/9def889010a979d55035d834539bb6fb.png)

使用 ResNet50 嵌入进行图像相似性搜索，在[NIGHTS数据集](https://dreamsim-nights.github.io/)中（图像由 Stable Diffusion 生成 — [MIT RAIL LICENSE](https://stability.ai/blog/stable-diffusion-public-release)）。ResNet 模型在像素和补丁的层面上操作。因此，检索到的图像在结构上类似于查询图像，但在语义上不一定相似。

![](../Images/f7acd455ddae7d434d94270b8d965e60.png)

使用 CLIP 嵌入进行相似性搜索，基于相同的查询图像。CLIP 模型尊重图像的基本语义，但不关注其布局。

![](../Images/0abe740139d3f32a9a563ab8da5dea5c.png)

使用 DreamSim 嵌入进行相似性搜索，基于相同的查询图像。DreamSim 弥合了语义和结构特征之间的差距，寻找最佳的中级相似性折衷。

欲了解更多信息和工作代码，请查看[这里](https://medium.com/voxel51/teaching-androids-to-dream-of-sheep-18d72f44f2b)。

# 比较模型表示

![](../Images/e115d4ca7f1bb4808737096b7c24fcea.png)

*对比 ResNet50 和 CLIP 模型在 NIGHTS 数据集上的表示。ResNet 嵌入通过 UMAP 降维到 2D。选择嵌入图中的一个点并突出显示附近的样本，我们可以看到 ResNet 如何捕捉组成和色彩相似性，而不是语义相似性。对选定样本使用 CLIP 嵌入进行向量搜索，我们可以看到根据 CLIP 的结果，大多数样本在 ResNet 中并没有局部化。*

我们可以通过结合向量搜索和降维技术，如均匀流形逼近 ([UMAP](https://umap-learn.readthedocs.io/en/latest/))，获得对两个模型差异的新见解。操作方法如下：

每个模型的嵌入包含了模型如何表示数据的信息。使用 UMAP（或 t-SNE 或 PCA），我们可以生成来自 model1 的低维（2D 或 3D）表示。这样做虽然牺牲了一些细节，但希望保留了样本之间的相似性信息。我们获得的是可视化这些数据的能力。

以 model1 的嵌入可视化为背景，我们可以在这个图中选择一个点，并对该样本执行一个针对 model2 嵌入的向量搜索查询。然后你可以看到检索到的点在 2D 可视化中的位置！

上述示例使用了与上一节相同的 NIGHTS 数据集，视觉化 ResNet 嵌入，捕捉更多的组成和结构相似性，并用 CLIP（语义）嵌入进行相似性搜索。

# 概念插值

![](../Images/e06bf1b4ad8310f8c3379048991f5cd2.png)

*使用 CLIP 嵌入在 Oxford-IIIT 宠物数据集上进行“哈士奇”和“吉娃娃”概念的插值*

我们接近这十个应用的尾声，但幸运的是，我把一些最佳的留到了最后。到目前为止，我们处理的唯一向量是嵌入—向量索引中填充了嵌入，查询向量也是嵌入。但有时在嵌入的*空间*中还有额外的结构，我们可以利用这些结构以更动态的方式与数据互动。

这种动态交互的一个例子是我喜欢称之为“概念插值”。操作方法如下：拿一个图像数据集，使用多模态模型（文本和图像）生成一个向量索引。选择两个文本提示词，比如“晴天”和“雨天”，这两个词代表了不同的概念，并设置一个在 `[0,1]` 范围内的值 `alpha`。我们可以为每个文本概念生成嵌入向量，并将这些向量按照 `alpha` 指定的线性组合方式相加。然后我们对这个向量进行归一化，并将其作为查询向量用于我们的图像嵌入向量索引。

因为我们在对两个文本提示（概念）的嵌入向量进行线性插值，从很大程度上说，我们在插值这些概念本身！我们可以动态改变`alpha`并在每次交互时查询我们的向量数据库。

💡这种概念插值的想法是实验性的（即：并不总是明确的操作）。我发现当文本提示在概念上相关且数据集足够多样化，以便在插值范围内产生不同结果时，它效果最佳。

欲了解更多信息和示例代码，请参见 [这里](https://github.com/jacobmarks/concept-interpolation)。

# 概念空间遍历

![](../Images/ac25af2786564b86cc7934e50f36eada.png)

*通过其嵌入在“概念”空间中移动，以各种文本提示的方向进行遍历，示例为COCO 2017数据集的测试拆分。图像和文本由CLIP模型嵌入。图像由作者提供。*

最后但同样重要的是，我喜欢称之为“概念空间遍历”。与概念插值类似，首先用像CLIP这样的多模态模型生成图像的嵌入。接着，从数据集中选择一张图像。这张图像将作为你的起点，你将“遍历”概念空间。

从那里，你可以通过提供一个文本字符串作为概念的替代来定义你想要移动的方向。设置你想在该方向上迈出的“步长”，并将该文本字符串的嵌入向量（带有乘法系数）加到初始图像的嵌入向量上。将使用“目标”向量来查询向量数据库。你可以添加任意数量的概念，并实时查看检索到的图像集的更新。

与“概念插值”一样，这并不总是一个严格定义的过程。然而，我发现它很迷人，当应用于文本嵌入的系数足够高时，它表现得相当好。

欲了解更多信息和示例代码，请参见 [这里](https://github.com/jacobmarks/concept-space-traversal-plugin)。

# 结论

向量搜索引擎是极其强大的工具。当然，它们是*RAG*时代最佳演出的明星。但向量数据库远比这更为多功能。它们使我们对数据有更深刻的理解，提供有关模型如何表示这些数据的见解，并为我们与数据互动提供了新的途径。

向量数据库并不局限于LLMs。只要涉及到嵌入，向量数据库就会显得非常有用，而嵌入恰好位于*模型*和*数据*的交汇处。我们对嵌入空间结构的理解越深入，我们的向量搜索驱动的数据和模型交互就会变得越动态和普遍。

如果你觉得这篇文章有趣，你可能还想看看这些由向量搜索驱动的文章：

+   🔗 [如何将我的公司文档转化为可搜索的数据库与 OpenAI](/how-i-turned-my-companys-docs-into-a-searchable-database-with-openai-4f2d34bd8736)

+   🔗 [如何将 ChatGPT 转变为类似 SQL 的图像和视频数据集翻译器](/how-i-turned-chatgpt-into-an-sql-like-translator-for-image-and-video-datasets-7b22b318400a)

+   🔗 [我在推动 Prompt Engineering 极限中学到的东西](/what-i-learned-pushing-prompt-engineering-to-the-limit-c40f0740641f)
