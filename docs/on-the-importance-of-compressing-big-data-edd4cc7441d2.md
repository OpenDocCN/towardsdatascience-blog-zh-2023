# 关于压缩大数据的重要性

> 原文：[https://towardsdatascience.com/on-the-importance-of-compressing-big-data-edd4cc7441d2?source=collection_archive---------18-----------------------#2023-01-24](https://towardsdatascience.com/on-the-importance-of-compressing-big-data-edd4cc7441d2?source=collection_archive---------18-----------------------#2023-01-24)

## 为什么以及如何最小化你的数据存储占用

[](https://chaimrand.medium.com/?source=post_page-----edd4cc7441d2--------------------------------)[![Chaim Rand](../Images/c52659c389f167ad5d6dc139940e7955.png)](https://chaimrand.medium.com/?source=post_page-----edd4cc7441d2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----edd4cc7441d2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----edd4cc7441d2--------------------------------) [Chaim Rand](https://chaimrand.medium.com/?source=post_page-----edd4cc7441d2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9440b37e27fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fon-the-importance-of-compressing-big-data-edd4cc7441d2&user=Chaim+Rand&userId=9440b37e27fe&source=post_page-9440b37e27fe----edd4cc7441d2---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----edd4cc7441d2--------------------------------) ·15分钟阅读·2023年1月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fedd4cc7441d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fon-the-importance-of-compressing-big-data-edd4cc7441d2&user=Chaim+Rand&userId=9440b37e27fe&source=-----edd4cc7441d2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fedd4cc7441d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fon-the-importance-of-compressing-big-data-edd4cc7441d2&source=-----edd4cc7441d2---------------------bookmark_footer-----------)![](../Images/3ef46726724121d8bb2393d48a773ef6.png)

图片来源：[Joshua Sortino](https://unsplash.com/@sortino?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

“数据是新的石油”，这是[Clive Humby](https://en.wikipedia.org/wiki/Clive_Humby)提出的一句话，用来描述现代许多公司在其发展和成功中对数据日益增长的依赖。公司们正收集大量数据，以至于像拍字节（petabyte）、艾字节（exabyte）和泽字节（zettabyte）这样的计量单位，已经在日常对话中取代了兆字节（megabyte）、千兆字节（gigabyte）和太字节（terabyte）。然而，无目的的数据收集是无用且浪费的。这个事实可以通过对Humby名言的以下扩展来最准确地总结：

> 数据就像原油。它有价值，但如果未经精炼，它实际上不能被有效使用。
> 
> — [迈克尔·帕尔默](https://ana.blogs.com/maestros/2006/11/data_is_the_new.html)

为了使数据具有价值，收集数据的方式、使用目标及如何实现这些目标需要精心设计。这种设计的一个重要元素是如何以及在哪里存储收集的数据。鉴于今天收集的数据规模巨大，存储需要专门的解决方案。为了满足不断增长的需求，数据中心存储设施的规模不断扩大，基于云的对象存储服务如[Amazon S3](https://aws.amazon.com/s3/)、[Google Cloud Storage](https://cloud.google.com/storage)和[Azure Blob Storage](https://azure.microsoft.com/en-us/products/category/storage/)也越来越受欢迎。

## 数据存储的成本

数据存储涉及许多成本，有些成本比其他成本更为明显。如果管理不当，存储成本很容易成为你每月研发费用中的主导因素。以下是一些成本考虑因素。

**直接存储成本**：

尽管单位存储空间的成本多年来一直稳步下降（由于技术进步），但这种下降已被需求的增加所盖过。不论你使用的是本地数据中心还是云存储服务，**直接存储成本可能迅速上升**。

**环境成本**：

在过去的几年里，我们见证了对数据中心碳足迹意识的提高，以及对计算科学中增加可持续性的呼吁（例如，见[这里](https://www.green-algorithms.org/)）。虽然数据中心服务器的计算密集型工作负荷占据了碳足迹的最大部分，但**存储也需要在电力、冷却和生命周期更换上进行大量投资**。预计大约10%的数据中心碳足迹——相当于一个中型西方国家的碳足迹——来自数据存储（例如，见[这里](https://journals.plos.org/ploscompbiol/article?id=10.1371%2Fjournal.pcbi.1009324)和[这里](https://www.osti.gov/servlets/purl/1372902)）。

**数据流成本**：

虽然不是直接存储成本，但必须关注与数据推送和拉取相关的成本。这些可能是与云存储服务相关的显性数据传输成本，或是与设计支持数据应用所需通信带宽的基础设施相关的成本。

**对数据应用的影响：** 还需要注意数据存储的方式，特别是其大小，对数据应用的影响。许多数据应用依赖于从存储中持续流动的数据。在理想情况下，数据应以足够快的速度流经系统，以使应用主机的所有计算资源得到充分利用。然而，如果您的存储解决方案设计不当，您的应用可能会在等待输入数据时处于空闲状态。这将增加运行数据应用所需的整体时间，并且**增加计算成本**。我们在本文的附录中更详细地描述了这一情况。

## 降低数据存储成本

我们的数据收集和存储策略解决方案必须考虑到数据存储的潜在高成本。一些良好的做法包括以下几点：

1.  将数据的收集和存储限制在实际需要的范围内。

1.  从存储中删除不再需要的数据。

1.  许多云服务提供商提供多种*存储类别*—以应对不同类型的数据访问模式（例如，请参见 [Amazon S3 Intelligent-Tiering](https://aws.amazon.com/s3/storage-classes/intelligent-tiering/)、[Google Storage Classes](https://cloud.google.com/storage/docs/storage-classes)和 [Azure Storage Access Tiers](https://learn.microsoft.com/en-us/azure/storage/blobs/access-tiers-overview)）。虽然标准存储选项（例如，[Amazon S3](https://aws.amazon.com/s3/)、[Google Cloud Storage](https://cloud.google.com/storage)和 [Azure Blob Storage](https://azure.microsoft.com/en-us/products/category/storage/)）推荐用于频繁访问或需要低延迟的数据，将其他数据分配给更具归档性质的存储类别，可以降低成本。

1.  以**压缩格式**存储数据。

在本文中，我们将重点讨论**数据压缩**作为降低存储成本的一种手段。我们将讨论几种不同的压缩技术，并通过示例展示应用这些技术的潜在节省。

## **免责声明**

在深入讨论之前，需要说明几点免责声明：

+   在我们的讨论中，我们将提到几种压缩技术和工具。这些仅作为示例。我们的意图是**不**要推广这些技术或工具，而是与许多其他替代技术或工具进行比较。适合您的最佳解决方案将极大地依赖于您的具体需求。

+   在决定使用某种压缩算法（或*任何*已发布的算法）之前，请确保您已阅读并理解相关的使用条款。

+   虽然我们的示例将重点放在图像数据上，但这些基本原则同样适用于其他领域。

# 数据压缩

在本节中，我们将回顾几种压缩技术并通过示例展示它们。我们将得出的主要见解是，通过将压缩算法适应于数据的特定属性，可以获得最佳结果。一个典型的例子就是***有损***和***无损***压缩方案之间的区别。

## 有损与无损压缩

在无损压缩方案中不会丢失数据。解压缩数据时，所有信息都会被恢复：***X=uncompress(compress(X))***。在有损压缩方案中，由于压缩的原因会丢失一些信息：***X≠uncompress(compress(X))***。虽然这种数据丢失一开始可能让人担忧，但在许多情况下，这种影响会很小。一个经典的例子就是图像压缩。许多图像压缩方法，如流行的JPEG压缩，涉及到一定程度的数据丢失，但（前提是使用合理的压缩配置）得到的图像与原始图像几乎无法区分。毫无疑问，尽管视觉上相似，但某些算法可能对这种数据丢失特别敏感。然而，更多情况下，这种影响不会显著。而且潜在的存储空间节省可以非常可观。我们将在下面进一步扩展图像压缩的话题。

## 如何衡量压缩质量

评估压缩方案质量的方法有很多种，包括：

1.  **压缩比**：这衡量了压缩过程中数据大小的减少。

1.  **信息丢失**：仅在有损压缩的情况下相关，这衡量了压缩导致的信息丢失对数据质量的影响程度。根据数据类型、领域、数据的用途等，有许多不同的方法来衡量这种质量损失。

1.  **压缩开销**：使用压缩方案意味着需要在管道的不同阶段进行**压缩**和**解压缩**。这两项活动都需要一定的计算资源，并可能意味着一定程度的延迟。所需的计算量和延迟可以根据选择的压缩策略有所不同。

1.  **基础设施依赖**：不同的压缩方案会因其基础设施依赖而有所不同。这些依赖可能是硬件依赖和/或软件依赖。

在接下来的示例中，我们将仅测量**压缩比**和**质量损失**。在实际操作中，应应用其他指标，以便对压缩策略做出全面的决策。

## 示例

为了方便讨论，我们将考虑一个玩具示例，其中每个数据样本包括：一个800x534 RGB图像、两个具有16个类别的像素级分类图，以及一个像素级深度图，包含从图像平面到场景中每个像素捕获的3D位置的距离（以米为单位）。如果您想跟随示例，可以将下面的代码片段应用于[Unsplash](https://unsplash.com/)上这篇博客文章顶部的图像。

```py
from PIL import Image
import numpy as np
np.random.seed(0)

im = Image.open('image.jpeg', mode='r')
image = np.array(im)
H,W,C = image.shape

# create artificial labels from image color channels
label1 = image[:,:,0].astype(np.int32)//16
label2 = image[:,:,1].astype(np.int32)//16
depth = (image[:,:,2]+np.random.normal(size=(H,W))).astype(np.float32)

# write all data sample elements to file
with open('image.bin','wb') as f:
    f.write(image.tobytes())
with open('label2.bin','wb') as f:
    f.write(label2.tobytes())
with open('label1.bin','wb') as f:
    f.write(label1.tobytes())
with open('depth.bin','wb') as f:
    f.write(depth.tobytes())
```

测量原始数据文件的存储足迹（例如，通过在Linux中运行*ls -l*），我们发现图像需要1.3 MB的存储空间，而3个标签图需要每个1.7 MB。

## 选择文件格式

设计数据存储策略的一个重要步骤是选择存储数据的格式。这个决定可能会对数据应用程序的访问简便性和速度产生重大影响。特别是，您的设计应考虑不同应用程序的不同访问模式。在[上一篇文章](/data-formats-for-training-in-tensorflow-parquet-petastorm-feather-and-more-e55179eeeb72)中，我们讨论了选择文件格式对机器学习训练的一些潜在影响。为了简化起见，我们将数据样本存储在标准tarball中。

```py
import tarfile
with tarfile.open("base.tar", "w") as tar:
    for name in ["image.bin", "label1.bin", "label2.bin", "depth.bin"]:
        tar.add(name)
```

结果文件为6.2 MB。

## 使用ZIP变体进行压缩

所谓“ZIP变体”，是指各种流行的通用文件格式及其相关压缩方案，包括[ZIP](https://en.wikipedia.org/wiki/ZIP_(file_format))、[gzip](https://en.wikipedia.org/wiki/Gzip)、[7-zip](https://en.wikipedia.org/wiki/7-Zip)、[bzip2](https://en.wikipedia.org/wiki/Bzip2)、[Brotli](https://en.wikipedia.org/wiki/Brotli)等。请注意，虽然我们将这些格式归为一类，但底层算法可能存在显著差异。许多文件格式包含用于自动压缩数据样本的ZIP变体标志。例如，使用[bzip2](https://en.wikipedia.org/wiki/Bzip2)压缩，我们可以将tarball的大小减少到2.7 MB，减少了2.3倍。

```py
import tarfile
with tarfile.open("base.bz2", "w:bz2") as tar:
    for name in ["image.bin", "label1.bin", "label2.bin", "depth.bin"]:
        tar.add(name)
```

使用ZIP变体进行压缩特别吸引人，因为它的通用性。它可以普遍应用，而无需了解底层数据的具体类型或领域。在接下来的章节中，我们将查看是否可以使用考虑到原始数据类型详细信息的压缩方案来改进这一结果。

## 使用低精度数据类型

通过将数据转换为使用低位精度数据类型，可以节省大量存储空间。在我们的例子中有两个优化机会。

1.  **将int32替换为uint8**：通过使用最小的整数类型来满足需求，可以节省大量的存储空间。在我们的例子中，32位整数表示显然对于表示我们的16类标签图来说是过多的。这些可以很容易地适应uint8矩阵而不会丢失任何信息。

1.  **将 float32 替换为 float16**：与整数精度减少相反，此操作*会*导致信息丢失（即，它是*有损的*）。此更改应仅在评估其对数据算法的潜在影响后进行。在下面的代码块中，我们演示了两种衡量数据质量变化的指标。这些可以用来预测对数据算法的影响。理想情况下，我们会发现这些指标与算法性能之间的某种关联，但这并不总是那么简单。

```py
label1 = label1.astype(np.uint8)
label2 = label2.astype(np.uint8)
depth_new = depth.astype(np.float16)

# measure loss of quality
from numpy import linalg as LA
l_max = LA.norm((depth-depth_new.astype(np.float32)).flatten(),np.inf) # 0.12
l_2 = LA.norm((depth-depth_new.astype(np.float32)).flatten(),2) # 10.47

with open('label1.bin','wb') as f:
    f.write(label1.tobytes())
with open('label2.bin','wb') as f:
    f.write(label2.tobytes())
with open('depth.bin','wb') as f:
    f.write(depth_new.tobytes())
```

仅这些优化就产生了 2.9 MB 的 tarball，减少了 2.14 倍。

## 使用按位操作合并元素

目前，每个分类图都存储在8位`uint8`缓冲区中。然而，由于这些图像只包含16个类别，它们实际上每个只使用了四位。我们可以通过将两个图像合并成一个数据图来进一步压缩数据。

```py
# compress
combined_label = (label2 * 16 + label1).astype(np.uint8)

# restore
label1 = combined_label % 16
label2 = combined_label // 16
```

结果的 tarball 大小为 2.5 MB，总体减少了 2.48 倍。

注意，我们也可以考虑将每个单独标签图中的相邻元素对合并，从而将其分辨率降至 400x534。在实践中，我们发现将单独的标签图合并有利于在后续处理阶段更好地压缩（如下一节讨论）。借用信息论中的术语，结果具有更低的熵。

## 图像压缩

各种图像压缩算法（有时称为*编解码器*）利用图像数据的独特统计特性来提高压缩率。虽然对图像压缩的全面概述超出了本文的范围，但我们将触及一些与当前问题相关的要点。

对图像编解码器进行初步搜索将返回各种选项，包括 [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics)、[JPEG](https://en.wikipedia.org/wiki/JPEG)、[WebP](https://en.wikipedia.org/wiki/WebP)、[JPEG XL](https://en.wikipedia.org/wiki/JPEG_XL) 等。这些编解码器在几个属性上有所不同，包括以下几点：

1.  **无损与有损压缩支持**：一些编解码器支持无损压缩，一些支持有损压缩，还有一些支持两者。在大多数情况下，有损压缩的压缩率比无损压缩更好。

1.  **支持的输入格式**：不同的算法支持不同类型的输入。典型的限制包括支持的颜色通道数量、支持的每像素位数等。

1.  **压缩质量控制**：编解码器在允许控制结果压缩质量的程度和性质上有所不同。例如，通过调整质量控制，我们可以管理压缩率与编码/解码速度和/或信息丢失之间的权衡。

1.  **底层压缩算法**：编解码器底层的算法行为各异，优化的功能不同，表现出的伪影也不同。例如，一些算法可能比其他算法更容易去除你所依赖的特定图像频率。

[**结构相似性**指数测量（**SSIM**）](https://en.wikipedia.org/wiki/Structural_similarity) 是一种常用的度量标准，用于评估图像压缩方案带来的图像质量退化。SSIM值范围从0到1，其中1表示与原始图像完全匹配。其他测量图像退化的指标包括 [MSE](https://en.wikipedia.org/wiki/Mean_squared_error) 和 [PSNR](https://en.wikipedia.org/wiki/Peak_signal-to-noise_ratio)。如前所述，你的目标应是选择能够预测信息丧失对数据算法性能影响的指标。

关于使用有损图像压缩方案，有几点需要注意：

+   尽管大多数编解码器优化了视觉感知，你的算法可能对图像中那些不明显的元素较为敏感。特别是，依赖于高度视觉相似性来评估压缩方案**不**应替代深入评估。

+   当心那些给人以某个编解码器总是优于其他编解码器印象的网站。实际上，不同编解码器的相对性能在图像领域（例如，深空图像与医学图像）之间差异很大，甚至同一领域内的图像样本之间也会有所不同。强烈建议你对自己的图像数据集进行分析。

+   对于视频序列数据，你可能会觉得有必要采用视频压缩格式。视频压缩利用相邻帧之间的相似性来进一步提高压缩率。然而，这通常会导致与图像压缩相比显著降低的质量（例如，通过SSIM测量）。在某些情况下，你可能会发现独立压缩每一帧效果更好。

我们的示例包括两个应用图像压缩的机会。首先，我们使用经典的有损JPEG编解码器压缩图像图，并将压缩*质量*设置为95（有关设置*质量*值的详细信息，请参见 [这里](https://pillow.readthedocs.io/en/stable/handbook/image-file-formats.html#jpeg)）。接下来，我们压缩标签图。由于我们期望机器学习算法高度依赖数据标签的准确性，因此我们选择无损PNG压缩格式，以避免丢失任何标签信息。请注意，尽管JPEG和PNG都是极受欢迎的格式，但它们并不以提供最佳压缩率而闻名。虽然足够用于我们的演示目的，但你可能会发现使用更现代的图像压缩算法能获得更好的结果。

下面的代码块演示了使用[Pillow](https://pypi.org/project/Pillow/) 包（版本 9.2.0）进行图像压缩。我们使用[scikit-image](https://pypi.org/project/scikit-image/) 包（版本 0.19.3）应用 SSIM 评分。

```py
from PIL import Image
from skimage.metrics import structural_similarity as SSIM

Image.fromarray(combined_label).save('label.png')
Image.fromarray(image).save('image.jpg',quality=95)

decoded = np.array(Image.open('image.jpg'))

ssim = SSIM(image, decoded, channel_axis=2)} # 0.996
```

我们示例中的 SSIM 分数为 0.996，表明 JPEG 编码导致的信息质量损失相对较低。自然，这一水平的图像降级是否可接受将取决于消耗数据的 ML 算法的敏感性。请注意，我们选择的压缩质量相对较高。较低的质量率可能会导致更好的压缩，但以更大的图像细节损失为代价。

下图展示了原始输入、解码输出以及它们之间的绝对差异（为了增强效果，差异被缩放了 20 倍）。

![](../Images/ce4b9a2ffdadd6d8d95aacc4b9fc14ae.png)

JPEG 对[Joshua Sortino](https://unsplash.com/@sortino?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上的照片的影响

## 结果

在这个阶段，图像、标签和深度图分别占用 341 KB、205 KB 和 835 KB 的存储空间。tarball 的大小为 1.2 MB。通过对 tarball 应用通用的 ZIP 算法，这一大小降至 1.1 MB。这比我们最初开始时的简单压缩结果小了一半。最终的压缩序列总结在下面的代码块中：

```py
import tarfile
from PIL import Image

combined_label = (label2 * 16 + label1).astype(np.uint8)
Image.fromarray(combined_label).save('label.png')
Image.fromarray(image).save('image.jpg',quality=95)

depth_new = depth.astype(np.float16)
with open('depth.bin','wb') as f:
    f.write(depth_new.tobytes())

with tarfile.open("final.tar.bz2", "w:bz2") as tar:
    for name in ["image.jpg", "label.png", "depth.bin"]:
        tar.add(name)
```

通过这个相对简单的序列，我们成功地将数据的大小减少了 5.64 倍。这意味着存储空间节省了超过**80%**。将其应用于你的完整数据集可能对**存储成本产生深远影响**。

我们可能还会发现额外的压缩机会。然而，每次额外操作的压缩率可能会降低。此外，使用不同的压缩序列可能会获得更好的压缩率。根据你现有的存储成本和潜在的节省，可能值得继续探索。

# 摘要

在这篇文章中，我们讨论了数据压缩对数据科学尤其是机器学习的重要性。我们演示了几个简单的压缩技术，并测量了它们对数据存储大小的影响。如上所述，我们选择的方法不一定适合你。找到一个好的解决方案的关键包括：对原始数据类型的深入了解、对数据消耗方式的深刻理解，以及对相关压缩方案的良好掌握。

随着2023年的开始，我们发现自己深陷于通常被称为大数据革命的过程中。正确的数据管理，包括使用数据压缩方案，仅仅是这一革命中许多重要组成部分之一。新年快乐。

# 附录：压缩对数据应用的影响

许多数据应用可以描述为在不同设备和设备组件之间连续流动的大量数据。例如，在深度学习训练负载中，原始训练数据从存储中流向CPU工作节点进行预处理和批处理，训练批次从CPU送入训练加速器，然后在前向计算图的不同阶段流动，梯度通过反向传播计算，而在分布式训练的情况下，数据在参与的加速器之间进行通信。

![](../Images/ea40e2462fdf4ff84624888c19ee2440.png)

数据在典型深度学习训练步骤中的流动（作者提供）

在理想情况下，数据会在系统中足够快地流动，以保持所有计算资源的充分利用。然而，有时你可能会发现数据流受到通信通道带宽限制的约束。这可能导致应用中存在性能瓶颈，并导致计算资源的未充分利用。在这种不理想的情况下，昂贵的资源处于闲置状态，等待数据输入。这类问题可以通过多种方式解决，包括：增加通信带宽（例如，使用不同规格的实例类型）、改变应用架构和/或**减少数据的大小**。

如果数据的大小很大，并且存储与应用主机之间的通信带宽有限，你可能特别容易遇到数据流瓶颈。**将数据存储在压缩格式中可以减少存储位置与应用之间接口的瓶颈潜力**。

压缩的一种潜在权衡是压缩和/或解压数据所需的额外计算资源。如果你的数据应用已经计算密集，你可能会发现将数据存储在压缩格式中虽然在应用的某一部分释放了*数据流*瓶颈，却在其他地方引入了*计算*瓶颈。因此，**数据应用管道中的数据压缩可能成为在不同资源的利用之间取得微妙平衡的艺术**。
