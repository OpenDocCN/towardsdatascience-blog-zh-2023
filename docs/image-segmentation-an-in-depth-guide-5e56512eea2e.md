# 图像分割：深入指南

> 原文：[https://towardsdatascience.com/image-segmentation-an-in-depth-guide-5e56512eea2e?source=collection_archive---------2-----------------------#2023-10-06](https://towardsdatascience.com/image-segmentation-an-in-depth-guide-5e56512eea2e?source=collection_archive---------2-----------------------#2023-10-06)

## 如何让计算机区分图像中的不同对象？一个逐步指南。

[](https://medium.com/@ed.izaguirre?source=post_page-----5e56512eea2e--------------------------------)[![Ed Izaguirre](../Images/c9eded1f06c47571baa662107428483f.png)](https://medium.com/@ed.izaguirre?source=post_page-----5e56512eea2e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5e56512eea2e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5e56512eea2e--------------------------------) [Ed Izaguirre](https://medium.com/@ed.izaguirre?source=post_page-----5e56512eea2e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F33a47cfa4187&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimage-segmentation-an-in-depth-guide-5e56512eea2e&user=Ed+Izaguirre&userId=33a47cfa4187&source=post_page-33a47cfa4187----5e56512eea2e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5e56512eea2e--------------------------------) ·21 分钟阅读·2023年10月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5e56512eea2e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimage-segmentation-an-in-depth-guide-5e56512eea2e&user=Ed+Izaguirre&userId=33a47cfa4187&source=-----5e56512eea2e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5e56512eea2e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimage-segmentation-an-in-depth-guide-5e56512eea2e&source=-----5e56512eea2e---------------------bookmark_footer-----------)![](../Images/18ea8243d356c00038d27bbaa2202afd.png)

一张猫咪在白色栅栏前的图像。来源于 [DALL·E 3](https://openai.com/dall-e-3)。

**目录**

1.  [介绍、动机](#4522)

1.  [提取数据](#6b76)

1.  [可视化图像](#af63)

1.  [构建简单的 U-Net 模型](#2142)

1.  [指标和损失函数](#c836)

1.  [构建完整的 U-Net 模型](#978d)

1.  [总结](#a780)

1.  [参考文献](#08e4)

**相关链接**

+   [工作中的 Kaggle 笔记本](https://www.kaggle.com/code/edizaguirre/carvana-image-segmentation) (**推荐；** 注册 Kaggle 竞赛，复制笔记本，并使用该免费的 GPU)

+   [Colab 笔记本](https://colab.research.google.com/drive/18sHAbbb4XNZlK0_rzH47DpIXukabiwe5?usp=sharing)（需要注册 Kaggle 比赛以获取数据集访问权限）

+   [Carvana 图像遮罩挑战 Kaggle 比赛](https://www.kaggle.com/competitions/carvana-image-masking-challenge/overview)

# 介绍，动机

**图像分割**是指计算机（更准确地说是存储在计算机上的模型）将图像中的每个像素分配到相应类别的能力。例如，可以将上面展示的猫的图像通过图像分割器处理，并得到如下的分割图像：

![](../Images/9385336d4aa4e3b361a97f77bff85580.png)

猫的图像，分割成‘猫’像素和‘背景’像素。修改后的图像来自 [DALL·E 3](https://openai.com/dall-e-3)。

在这个例子中，我手动分割了图像。这是一个繁琐的操作，我们希望能自动化。在本指南中，我将带你逐步了解训练算法进行图像分割的过程。互联网上和教科书中有许多指南在一定程度上是有帮助的，但它们都未能深入到实现的细节。在这里，我将尽可能不留任何遗漏，帮助你在自己数据集上实现图像分割时节省时间。

首先，让我们将任务放在**机器学习**的更广泛背景中。机器学习的定义不言而喻：我们在教机器如何解决我们希望自动化的问题。人类希望自动化的问题有很多；在本文中，我们关注的是**计算机视觉**中的一个子集。计算机视觉旨在教计算机*如何看见*。给一个六岁的小孩一张猫在白色栅栏前的图像，并让他们将图像分割成‘猫’像素和‘背景’像素（当然，在你向困惑的孩子解释‘分割’的意思之后）是很简单的。然而，几十年来，计算机在这个问题上却一直挣扎。

为什么计算机在做一个六岁小孩能做的事情时会挣扎？我们可以通过考虑一个人如何通过盲文学习阅读来设身处地为计算机着想。假设你拿到了一篇用盲文写的文章，并且假设你不知道如何阅读它。你会怎么做？你需要什么来将盲文解码成英文？

![](../Images/23797d6f1750c9e5b7acd23806fbec45.png)

一小段用盲文书写的文字。来源于 [Unsplash](https://unsplash.com/)。

你需要的是一种将输入转换为对你可读的输出的方法。在数学中，我们称之为**映射**。我们说我们希望学习一个函数 *f(x)*，它将我们无法读取的输入 *x* 映射到一个可读取的输出 *y*。

![](../Images/7732d26a356f0a39a3135c97f833928b.png)

通过长时间的练习和良好的导师，任何人都可以学会从盲文到英文的必要映射。类比而言，处理图像的计算机有点像初次接触盲文的人；它看起来像一堆无意义的东西。计算机需要学习必要的映射 *f(x)*，将与像素对应的一堆数字转换为可以用来分割图像的内容。不幸的是，计算机模型没有数千年的进化、生物学以及多年看世界的经验；它在您启动程序时基本上‘出生’。**这就是我们希望在计算机视觉中教给我们模型的内容。**

首先，我们为什么要进行图像分割呢？其中一个更明显的用例是Zoom。许多人在视频会议中喜欢使用虚拟背景，以避免同事看到他们的狗在客厅里做花式。*图像分割对于这项任务至关重要*。另一个强大的用例是医学成像。在对患者器官进行CT扫描时，自动分割图像中的器官可能有助于医疗专业人员确定诸如损伤、肿瘤存在等问题。[这里有一个很好的例子](https://www.kaggle.com/competitions/rsna-2023-abdominal-trauma-detection) 是一个专注于此任务的Kaggle竞赛。

图像分割有几种不同的类型，从简单到复杂不等。在本文中，我们将处理最简单的图像分割类型：*二进制分割*。这意味着只会有两类不同的对象，例如‘猫’和‘背景’。不多也不少。

请注意，我在此呈现的代码已经稍作整理和编辑以增加清晰度。要运行一些可工作的代码，请查看文章顶部的代码链接。我们将使用Kaggle的Carvana图像掩模挑战数据集。您需要注册此挑战以获取数据集，并将您的Kaggle API密钥插入Colab笔记本以使其正常工作（如果您不想使用Kaggle笔记本）。[请查看此讨论帖](https://www.kaggle.com/discussions/general/74235) 获取详细信息。

还有一件事；尽管我很想详细讨论代码中的每个想法，但我假设您对卷积神经网络、最大池化层、密集连接层、dropout层和残差连接器有一些工作知识。不幸的是，长时间讨论这些概念需要一篇新文章，超出了我们专注于实现细节的范围。

# 提取数据

本文的相关数据将存储在以下文件夹中：

+   **train_hq.zip**: 包含汽车高质量训练图像的文件夹

+   **test_hq.zip**: 包含汽车高质量测试图像的文件夹

+   **train_masks.zip:** 包含训练集掩模的文件夹

在图像分割的上下文中，**mask** 是分割后的图像。我们尝试让模型学习如何将输入图像映射到输出分割掩模。通常假设真实的掩模（即地面真相）是由人类专家手工绘制的。

![](../Images/535db816963d7b6d14f8c1a293d957d3.png)

一个图像及其相应的真实掩模的例子，是真实的人手工绘制的。来自 Carvana 图像掩模挑战数据集。

你的第一步是从你的 */kaggle/input* 源中解压文件夹：

```py
def getZippedFilePaths():
    zip_file_names = []
    for dirname, _, filenames in os.walk('/kaggle/input'):
        for filename in filenames:
            if filename.split('.')[-1] == 'zip':
                zip_file_names.append((os.path.join(dirname, filename)))

    return zip_file_names

zip_file_names = getZippedFilePaths()

items_to_remove = ['/kaggle/input/carvana-image-masking-challenge/train.zip', 
                   '/kaggle/input/carvana-image-masking-challenge/test.zip']

zip_file_names = [item for item in zip_file_names if item not in items_to_remove]

for zip_file_path in zip_file_names:
    with zipfile.ZipFile(zip_file_path, 'r') as zip_ref:
        zip_ref.extractall()
```

这段代码获取了输入中所有 .zip 文件的文件路径，并将它们提取到 */kaggle/output* 目录中。请注意，我故意不提取非高质量照片；Kaggle 存储库只能容纳 20 GB 的数据，这一步是为了防止超过此限制。

# 可视化图像

大多数计算机视觉问题中的第一步是检查数据集。我们究竟在处理什么？我们首先需要将图像组装成组织良好的数据集以进行查看。本指南将使用 TensorFlow；转换为 PyTorch 应该不会太困难。

```py
# Appending all path names to a sorted list
train_hq_dir = '/kaggle/working/train_hq/'
train_masks_dir = '/kaggle/working/train_masks/'
test_hq_dir = '/kaggle/working/test_hq/'

X_train_id = sorted([os.path.join(train_hq_dir, filename) for filename in os.listdir(train_hq_dir)], key=lambda x: x.split('/')[-1].split('.')[0])
y_train = sorted([os.path.join(train_masks_dir, filename) for filename in os.listdir(train_masks_dir)], key=lambda x: x.split('/')[-1].split('.')[0])
X_test_id = sorted([os.path.join(test_hq_dir, filename) for filename in os.listdir(test_hq_dir)], key=lambda x: x.split('/')[-1].split('.')[0])

X_train_id = X_train_id[:1000]
y_train = y_train[:1000]
X_train, X_val, y_train, y_val = train_test_split(X_train_id, y_train, test_size=0.2, random_state=42)

# Create Dataset objects from the list of file paths
X_train = tf.data.Dataset.from_tensor_slices(X_train)
y_train = tf.data.Dataset.from_tensor_slices(y_train)

X_val = tf.data.Dataset.from_tensor_slices(X_val)
y_val = tf.data.Dataset.from_tensor_slices(y_val)

X_test = tf.data.Dataset.from_tensor_slices(X_test_id)

img_height = 96
img_width = 128
num_channels = 3

img_size = (img_height, img_width)

# Apply preprocessing
X_train = X_train.map(preprocess_image)
y_train = y_train.map(preprocess_target)

X_val = X_val.map(preprocess_image)
y_val = y_val.map(preprocess_target)

X_test = X_test.map(preprocess_image)

# Add labels to dataframe objects (one-hot-encoded)
train_dataset = tf.data.Dataset.zip((X_train, y_train))
val_dataset = tf.data.Dataset.zip((X_val, y_val))

# Apply the batch size to the dataset
BATCH_SIZE = 32
batched_train_dataset = train_dataset.batch(BATCH_SIZE)
batched_val_dataset = val_dataset.batch(BATCH_SIZE)
batched_test_dataset = X_test.batch(BATCH_SIZE)

# Adding autotune for pre-fetching
AUTOTUNE = tf.data.experimental.AUTOTUNE
batched_train_dataset = batched_train_dataset.prefetch(buffer_size=AUTOTUNE)
batched_val_dataset = batched_val_dataset.prefetch(buffer_size=AUTOTUNE)
batched_test_dataset = batched_test_dataset.prefetch(buffer_size=AUTOTUNE)
```

让我们来拆解一下：

+   我们首先创建一个所有训练集、测试集和真实掩模图像的文件路径的排序列表。注意，这些*还不是*图像；我们目前只是在查看图像的文件路径。

+   然后，我们只取 Carvana 数据集中前 1000 个图像/掩模的文件路径。这是为了减少计算负载并加快训练速度。如果你有多个强大的 GPU（真幸运！），可以使用所有图像以获得更好的性能。我们还创建了 80/20 的训练/验证划分。包含的数据（图像）越多，这一划分就应该越倾向于训练集。在处理非常大的数据集时，训练/验证/测试的划分中看到 98/1/1 的情况并不少见。训练集中的数据越多，模型的表现通常会更好。

+   然后，我们使用 tf.data.Dataset.from_tensor_slices() 方法创建 TensorFlow (TF) **Dataset** 对象。使用 Dataset 对象是一种处理训练、验证和测试集的常见方法，而不是将它们保留为 Numpy 数组。根据我的经验，使用 Dataset 对象时数据预处理要快得多，也更容易。[参见此链接](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#from_tensor_slices) 获取文档。

+   接下来，我们指定输入图像的高度、宽度和通道数。实际的高质量图像远大于 96 像素乘 128 像素；对图像进行**降采样**是为了减少计算负载（更大的图像需要更多的训练时间）。如果你有必要的计算能力（GPU），我不推荐进行降采样。

+   然后，我们使用 Dataset 对象的 .map() 函数来预处理图像。这将文件路径转换为图像，并进行适当的预处理。稍后会详细介绍。

+   一旦我们预处理了原始训练图像和真实标注掩膜，我们需要一种方法将图像与其掩膜配对。为此，我们使用 Dataset 对象的 `.zip()` 函数。这将两个数据列表进行连接，将每个列表的第一个元素合并成一个元组。对第二个元素、第三个元素等进行相同操作。最终结果是一个包含 (image, mask) 形式的元组的单一列表。

+   然后，我们使用 `.batch()` 函数从我们的一千张图像中创建 32 张图像的批次。批量处理是机器学习管道的重要部分，因为它允许我们一次处理多张图像，而不是逐张处理。这加快了训练速度。

+   最后，我们使用 `.prefetch()` 函数。这是另一个帮助加速训练的步骤。加载和预处理数据可能成为训练管道中的瓶颈。这可能导致 GPU 或 CPU 空闲时间，这是没有人希望看到的。当你的模型进行前向和反向传播时，`.prefetch()` 函数可以准备好下一批数据。TensorFlow 中的 AUTOTUNE 变量会根据系统资源动态计算预取的批次数；这通常是推荐的做法。

让我们更深入地了解预处理步骤：

```py
def preprocess_image(file_path):
    # Load and decode the image
    img = tf.io.read_file(file_path)
    # You can adjust channels based on your images (3 for RGB)
    img = tf.image.decode_jpeg(img, channels=3) # Returned as uint8
    # Normalize the pixel values to [0, 1]
    img = tf.image.convert_image_dtype(img, tf.float32)
    # Resize the image to your desired dimensions
    img = tf.image.resize(img, [96, 128], method = 'nearest')
    return img

def preprocess_target(file_path):
    # Load and decode the image
    mask = tf.io.read_file(file_path)
    # Normalizing to between 0 and 1 (only two classes)
    mask = tf.image.decode_image(mask, expand_animations=False, dtype=tf.float32)
    # Get only one value for the 3rd channel
    mask = tf.math.reduce_max(mask, axis=-1, keepdims=True)
    # Resize the image to your desired dimensions
    mask = tf.image.resize(mask, [96, 128], method = 'nearest')
    return mask
```

这些函数完成以下操作：

+   首先，我们使用 tf.io.read_file() 将文件路径转换为数据类型为 'string' 的 **张量**。张量是 TensorFlow 中一种特殊的数据结构，类似于其他数学库中的多维数组，但具有对深度学习有用的特殊属性和方法。引用 TensorFlow 文档：tf.io.read_file() “不进行任何解析，它仅返回原始内容。” 本质上，这意味着它返回一个包含图像信息的二进制文件（1 和 0），以字符串类型表示。

+   其次，我们需要 *解码* 二进制数据。为此，我们需要使用 TensorFlow 中的适当方法。由于原始图像数据是 .jpeg 格式，我们使用 tf.image.decode_jpeg() 方法。由于掩膜是 GIF 格式，我们可以使用 tf.io.decode_gif()，或者使用更通用的 tf.image.decode_image()，它可以处理任何文件类型。你选择哪个并不重要。我们将 expand_animations 设置为 False，因为这些实际上不是动画，而只是图像。

+   然后我们使用 convert_image_dtype() 将我们的图像数据转换为 float32。**这仅适用于图像，不适用于掩膜，因为掩膜已经解码为 float32**。图像处理常用的两种数据类型是 float32 和 uint8。Float32 代表一个浮点数（小数），在计算机内存中占用 32 位。它们是有符号的（意味着数字可以是负数），值的范围从 0 到 2³² = 4294967296，尽管在图像处理的惯例中，我们将这些值归一化到 0 到 1 之间，其中 1 是颜色的最大值。Uint8 代表一个无符号（正数）整数，范围从 0 到 255，只占用 8 位内存。例如，我们可以将颜色 burnt orange 表示为 uint8 格式的 (Red: 204, Green: 85, Blue: 0) 或 float32 格式的 (Red: 0.8, Green: 0.33, Blue: 0)。Float32 通常是更好的选择，因为它提供了更多的精度，并且已经归一化，这有助于提高训练效果。然而，uint8 节省内存，根据你的内存限制，这可能是更好的选择。在 convert_image_dtype 中使用 float32 会自动归一化这些值。

+   在二分类分割中，我们期望我们的掩膜具有形状 (batch, height, width, channels)，其中 channels = 1。换句话说，我们希望一个类别（汽车）由数字 1 表示，另一个类别（背景）由数字 0 表示。没有理由将通道数设置为 3，就像 RGB 图像一样。不幸的是，解码后，它带有三个通道，其中类别编号重复三次。为了解决这个问题，我们使用 tf.math.reduce_max(mask, axis=-1, keepdims=True) 来取三个通道中的最大值，并去掉其他值。因此，通道值 (1,1,1) 会被减少为 (1)，通道值 (0,0,0) 会被减少为 (0)。

+   最后，我们将图像/掩膜调整为我们所需的尺寸（较小）。请注意，我之前展示的汽车图像与真实掩膜看起来模糊；这次缩小是故意进行的，以减少计算负荷并使训练相对较快。默认使用 method=‘nearest’ 是个好主意；否则函数将始终返回 float32，这对于你希望它是 uint8 格式的情况是不好的。

![](../Images/d9ea340e152cd50a2ed62d40fad7d400.png)

颜色 burnt orange 可以用 float32 或 uint8 格式表示。图片由作者提供。

一旦我们组织好数据集，我们现在可以查看我们的图像：

```py
# View images and associated labels
for images, masks in batched_val_dataset.take(1):
    car_number = 0
    for image_slot in range(16):
        ax = plt.subplot(4, 4, image_slot + 1)
        if image_slot % 2 == 0:
            plt.imshow((images[car_number])) 
            class_name = 'Image'
        else:
            plt.imshow(masks[car_number], cmap = 'gray')
            plt.colorbar()
            class_name = 'Mask'
            car_number += 1            
        plt.title(class_name)
        plt.axis("off")
```

![](../Images/4de9150e514f5730a3daf8f5bff40fd2.png)

我们的汽车图像与附带的掩膜配对显示。

在这里，我们使用 .take() 方法来查看我们批处理的 batched_val_dataset 中的第一批数据。由于我们正在进行二分类分割，我们希望我们的掩膜只包含两个值：0 和 1。绘制掩膜上的颜色条可以确认我们设置正确。请注意，我们在掩膜的 imshow() 中添加了参数 cmap = ‘gray’，以告知 plt 我们希望这些图像以灰度显示。

# 构建一个简单的 U-Net 模型

在1675年2月5日写给其对手罗伯特·胡克的信中，艾萨克·牛顿表示：

> *“如果我看得更远，那是因为我站在了巨人的肩膀上。”*

同样地，我们将站在之前机器学习研究者的肩膀上，他们已经发现了哪些架构最适合图像分割任务。实验自己设计的架构并非坏主意；然而，之前的研究者们走过许多弯路才发现了有效的模型。这些架构并不一定是终极解决方案，因为研究仍在进行中，可能会发现更好的架构。

![](../Images/d01011a3b09163f8eaf0dd41cc39c0f8.png)

U-Net的可视化，描述见[1]。图片由作者提供。

一个较为知名的架构称为**U-Net**，因为网络的下采样部分和上采样部分可以被可视化为一个U形（见上文）。在由Ronneberger、Fisher和Brox撰写的名为*U-Net: Convolutional Networks for Biomedical Image Segmentation*的论文[1]中，作者描述了如何创建一个**全卷积网络** **(FCN)**，该网络在图像分割中表现有效。全卷积意味着没有密集连接层；所有层都是卷积层。

有几点需要注意：

+   网络由一系列重复的两个卷积层块组成，使用padding = ‘same’和stride = 1，以确保卷积的输出在块内不会缩小。

+   每个块后面跟着一个最大池化层，该层将特征图的宽度和高度减半。

+   接下来的模块将滤波器的数量加倍。这个模式会继续。如果你研究过CNN，这种在减少特征空间的同时增加滤波器数量的模式应该很熟悉。这完成了作者所称的“收缩路径”。

+   “瓶颈”层位于‘U’的底部。该层捕捉高度抽象的特征（如线条、曲线、窗户、门等），但在空间分辨率上大幅降低。

+   接下来是他们所谓的“扩展路径”。简而言之，这个过程是反向的，每个模块再次由两个卷积层组成。每个模块后面跟着一个**上采样层**，在TensorFlow中我们称之为Conv2DTranspose层。它将较小的特征图的高度和宽度加倍。

+   接下来的模块将滤波器的数量减半。重复这个过程，直到得到与起始图像相同的高度和宽度。最后，用一个1x1卷积层来将通道数量减少到1。我们希望最后得到一个通道，因为这是二值分割，所以我们需要一个滤波器，像素值对应于我们的两个类别。我们使用sigmoid激活函数将像素值压缩在0到1之间。

+   U-Net架构中还有**跳跃连接**，允许网络在下采样和上采样后保留细粒度的空间信息。在这个过程中通常会丢失很多信息。通过将信息从收缩块传递到相应的扩展块，我们可以保留这些空间信息。这个架构有一个漂亮的对称性。

我们将首先做一个简单版本的U-Net。这将是一个FCN，但没有残差连接和最大池化层。

```py
data_augmentation = tf.keras.Sequential([
        tfl.RandomFlip(mode="horizontal", seed=42),
        tfl.RandomRotation(factor=0.01, seed=42),
        tfl.RandomContrast(factor=0.2, seed=42)
])

def get_model(img_size):
    inputs = Input(shape=img_size + (3,))
    x = data_augmentation(inputs)

    # Contracting path
    x = tfl.Conv2D(64, 3, strides=2, activation="relu", padding="same", kernel_initializer='he_normal')(x) 
    x = tfl.Conv2D(64, 3, activation="relu", padding="same", kernel_initializer='he_normal')(x) 
    x = tfl.Conv2D(128, 3, strides=2, activation="relu", padding="same", kernel_initializer='he_normal')(x) 
    x = tfl.Conv2D(128, 3, activation="relu", padding="same", kernel_initializer='he_normal')(x) 
    x = tfl.Conv2D(256, 3, strides=2, padding="same", activation="relu", kernel_initializer='he_normal')(x) 
    x = tfl.Conv2D(256, 3, activation="relu", padding="same", kernel_initializer='he_normal')(x)

    # Expanding path
    x = tfl.Conv2DTranspose(256, 3, activation="relu", padding="same", kernel_initializer='he_normal')(x)
    x = tfl.Conv2DTranspose(256, 3, activation="relu", padding="same", kernel_initializer='he_normal', strides=2)(x)
    x = tfl.Conv2DTranspose(128, 3, activation="relu", padding="same", kernel_initializer='he_normal')(x)
    x = tfl.Conv2DTranspose(128, 3, activation="relu", padding="same", kernel_initializer='he_normal', strides=2)(x)
    x = tfl.Conv2DTranspose(64, 3, activation="relu", padding="same", kernel_initializer='he_normal')(x)
    x = tfl.Conv2DTranspose(64, 3, activation="relu", padding="same", kernel_initializer='he_normal', strides=2)(x)

    outputs = tfl.Conv2D(1, 3, activation="sigmoid", padding="same")(x)
    model = keras.Model(inputs, outputs) 

    return model

custom_model = get_model(img_size=img_size)
```

这里我们有与U-Net相同的基本结构，包括一个收缩路径和一个扩展路径。一个有趣的观察是，与其使用最大池化层将特征空间切成两半，我们使用一个步幅为2的卷积层。根据Chollet [2]，这可以将特征空间切成两半，同时保留比最大池化层更多的空间信息。他指出，只要位置信息重要（如在图像分割中），最好避免破坏性的最大池化层，而改用步幅卷积（**这很有趣，因为著名的** **U-Net架构确实使用了最大池化**）。还要注意，我们正在进行一些数据增强，以帮助我们的模型泛化到未见的示例。

一些重要的细节：将kernel_initializer设置为‘he_normal’以用于ReLU激活会在训练稳定性方面产生意想不到的大差异。我最初低估了权重初始化的力量。与随机初始化权重不同，he_normalization将权重初始化为均值为0，标准差为（2 / 层的输入单元数）的平方根。在CNN的情况下，输入单元的数量指的是前一层特征图的通道数。已发现he_normal初始化能导致更快的收敛，减轻梯度消失，并改善学习。有关更多细节，请参见参考文献[3]。

# 指标和损失函数

二值分割可以使用几种常见的指标和损失函数。在这里，我们将使用**dice系数**作为指标，并使用相应的**dice损失**进行训练，因为这是比赛要求的。

让我们首先看看dice系数背后的数学原理：

![](../Images/bd23ec4488d38032c23f8f61c35f95b0.png)

一般形式的dice系数。

Dice系数被定义为两个集合（*X*和*Y*）的交集，除以每个集合的和，再乘以2。Dice系数的值范围在0（如果集合没有交集）到1（如果集合完全重叠）之间。现在我们可以理解为什么这成为图像分割的一个很好的度量标准。

![](../Images/cb83c98c2e92e3a20843999e79e16a57.png)

两个遮罩重叠的示例。橙色用于清晰度。图片来源于作者。

上述方程是 dice 系数的通用定义；当应用于 *向量* 量时（与集合不同），我们使用更具体的定义：

![](../Images/b147a9a8680ca375b9bc738af7aeb79e.png)

以向量形式表示的 dice 系数。

在这里，我们对每个掩膜中的每个元素（像素）进行迭代。*x* 表示预测掩膜中的第 *i* 个像素，而 *y* 表示真实掩膜中的对应像素。顶部是逐元素乘积，底部是分别对每个掩膜中的所有元素求和。*N* 表示像素的总数（预测掩膜和目标掩膜应相同）。记住，在我们的掩膜中，数字都为 0 或 1，因此真实掩膜中值为 1 的像素和预测掩膜中对应值为 0 的像素不会对 dice 分数产生贡献，这也是预期的（1 x 0 = 0）。

**dice 损失** 将简单地定义为 1 — Dice Score。由于 dice 分数在 0 和 1 之间，dice 损失也将在 0 和 1 之间。实际上，dice 分数和 dice 损失的和必须等于 1。它们是反向相关的。

让我们看看如何在代码中实现这一点：

```py
from tensorflow.keras import backend as K

def dice_coef(y_true, y_pred, smooth=10e-6):
    y_true_f = K.flatten(y_true)
    y_pred_f = K.flatten(y_pred)
    intersection = K.sum(y_true_f * y_pred_f)
    dice = (2\. * intersection + smooth) / (K.sum(y_true_f) + K.sum(y_pred_f) + smooth)
    return dice

def dice_loss(y_true, y_pred):
    return 1 - dice_coef(y_true, y_pred)
```

在这里，我们将两个 4-D 掩膜（批量，高度，宽度，通道=1）展平为 1-D 向量，并计算批次中所有图像的 dice 分数。请注意，我们向分子和分母都添加了平滑值，以防两个掩膜不重叠时出现 0/0 的问题。

最后，我们开始训练。我们进行早停以防止过拟合，并保存最佳模型。

```py
custom_model.compile(optimizer=tf.keras.optimizers.Adam(learning_rate=0.0001,
                                                        epsilon=1e-06), 
                                                        loss=[dice_loss], 
                                                        metrics=[dice_coef])
callbacks_list = [
    keras.callbacks.EarlyStopping(
        monitor="val_loss",
        patience=2,
    ),
    keras.callbacks.ModelCheckpoint(
        filepath="best-custom-model",
        monitor="val_loss",
        save_best_only=True,
    )
]

history = custom_model.fit(batched_train_dataset, epochs=20,
                    callbacks=callbacks_list,
                    validation_data=batched_val_dataset)
```

我们可以通过以下代码来确定我们训练的结果：

```py
def display(display_list):
    plt.figure(figsize=(15, 15))

    title = ['Input Image', 'True Mask', 'Predicted Mask']

    for i in range(len(display_list)):
        plt.subplot(1, len(display_list), i+1)
        plt.title(title[i])
        plt.imshow(tf.keras.preprocessing.image.array_to_img(display_list[i]))
        plt.axis('off')
    plt.show()

def create_mask(pred_mask):
    mask = pred_mask[..., -1] >= 0.5
    pred_mask[..., -1] = tf.where(mask, 1, 0)
    # Return only first mask of batch
    return pred_mask[0]

def show_predictions(model, dataset=None, num=1):
    """
    Displays the first image of each of the num batches
    """
    if dataset:
        for image, mask in dataset.take(num):
            pred_mask = model.predict(image)
            display([image[0], mask[0], create_mask(pred_mask)])
    else:
        display([sample_image, sample_mask,
             create_mask(model.predict(sample_image[tf.newaxis, ...]))])

custom_model = keras.models.load_model("/kaggle/working/best-custom-model", custom_objects={'dice_coef': dice_coef, 'dice_loss': dice_loss})

show_predictions(model = custom_model, dataset = batched_train_dataset, num = 6)
```

经过 10 个周期后，我们达到了最高验证 dice 分数 0.8788。还不错，但不是很好。在 P100 GPU 上，这大约花了我 20 分钟。这里是我们审查的样本掩膜：

![](../Images/7cbd52526f40648c35425e6eabfb9170.png)

输入图像、真实掩膜和预测掩膜的比较。作者提供。

突出几个有趣的点：

+   请注意，`create_mask` 是将像素值推送到 0 或 1 的函数。像素值小于 0.5 的将被裁剪为 0，我们会将该像素分配到“背景”类别。值 ≥ 0.5 的将增加到 1，我们会将该像素分配到“汽车”类别。

+   为什么掩码的颜色是黄色和紫色，而不是黑色和白色？我们使用了：tf.keras.preprocessing.image.array_to_img() 将掩码的输出从张量转换为 PIL 图像。然后我们将图像传递给 plt.imshow()。[来自文档](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.imshow.html) 我们看到单通道图像的默认颜色图是“viridis”（3 通道 RGB 图像保持原样）。viridis 颜色图将低值转化为深紫色，高值转化为黄色。这个颜色图显然可以帮助色盲人士准确查看图像中的颜色。我们本可以通过添加 cmap=“grayscale” 作为参数来解决这个问题，但这会搞砸我们的输入图像。更多信息见 [此链接](https://en.wikipedia.org/wiki/Color_blindness)。

![](../Images/82384d6f3ece15994a1d089ad40e0d75.png)

viridis 颜色图，从低值（紫色）到高值（黄色）。作者提供。

# 构建完整的 U-Net

现在我们转向使用完整的 U-Net 架构，包含残差连接、最大池化层，并包括用于正则化的丢弃层。注意收缩路径、瓶颈层和扩展路径。丢弃层可以在收缩路径中添加，在每个块的末尾。

```py
def conv_block(inputs=None, n_filters=64, dropout_prob=0, max_pooling=True):
    conv = Conv2D(n_filters,  
                  3,   
                  activation='relu',
                  padding='same',
                  kernel_initializer='he_normal')(inputs)
    conv = Conv2D(n_filters,  
                  3,   
                  activation='relu',
                  padding='same',
                  kernel_initializer='he_normal')(conv)

    if dropout_prob > 0:
        conv = Dropout(dropout_prob)(conv)

    if max_pooling:
        next_layer = MaxPooling2D(pool_size=(2, 2))(conv)

    else:
        next_layer = conv

    skip_connection = conv

    return next_layer, skip_connection

def upsampling_block(expansive_input, contractive_input, n_filters=64):
    up = Conv2DTranspose(
        n_filters,    
        3,    
        strides=(2, 2),
        padding='same',
        kernel_initializer='he_normal')(expansive_input)

    # Merge the previous output and the contractive_input
    merge = concatenate([up, contractive_input], axis=3)

    conv = Conv2D(n_filters,   
                  3,     
                  activation='relu',
                  padding='same',
                  kernel_initializer='he_normal')(merge)
    conv = Conv2D(n_filters,   
                  3,     
                  activation='relu',
                  padding='same',
                  kernel_initializer='he_normal')(conv)

    return conv

def unet_model(input_size=(96, 128, 3), n_filters=64, n_classes=1):
    inputs = Input(input_size)

    inputs = data_augmentation(inputs)

    # Contracting Path (encoding)
    cblock1 = conv_block(inputs, n_filters)
    cblock2 = conv_block(cblock1[0], n_filters*2)
    cblock3 = conv_block(cblock2[0], n_filters*4)
    cblock4 = conv_block(cblock3[0], n_filters*8, dropout_prob=0.3)

    # Bottleneck Layer
    cblock5 = conv_block(cblock4[0], n_filters*16, dropout_prob=0.3, max_pooling=False)

    # Expanding Path (decoding)
    ublock6 = upsampling_block(cblock5[0], cblock4[1],  n_filters*8)
    ublock7 = upsampling_block(ublock6, cblock3[1],  n_filters*4)
    ublock8 = upsampling_block(ublock7, cblock2[1],  n_filters*2)
    ublock9 = upsampling_block(ublock8, cblock1[1],  n_filters)

    conv9 = Conv2D(n_filters,
                   3,
                   activation='relu',
                   padding='same',
                   kernel_initializer='he_normal')(ublock9)

    conv10 = Conv2D(n_classes, 1, padding='same', activation="sigmoid")(conv9)

    model = tf.keras.Model(inputs=inputs, outputs=conv10)

    return model
```

然后我们编译 U-Net。我在第一个收缩块中使用了 64 个滤波器。这是一个超参数，你需要调整以获得最佳结果。

```py
unet = unet_model(input_size=(img_height, img_width, num_channels), n_filters=64, n_classes=1)
unet.compile(optimizer=tf.keras.optimizers.Adam(learning_rate=0.0001, epsilon=1e-06),
             loss=[dice_loss], 
             metrics=[dice_coef])

callbacks_list = [
    keras.callbacks.EarlyStopping(
        monitor="val_loss",
        patience=2,
    ),
    keras.callbacks.ModelCheckpoint(
        filepath="best-u_net-model",
        monitor="val_loss",
        save_best_only=True,
    )
]

history = unet.fit(batched_train_dataset, epochs=20,
                    callbacks=callbacks_list,
                    validation_data=batched_val_dataset)
```

经过 16 个训练周期，我得到了 0.9416 的验证骰子分数，比简单的 U-Net 要好得多。这不应该太令人惊讶；查看参数数量，我们从简单的 U-Net 到完整的 U-Net 增加了一个数量级。在 P100 GPU 上，这大约花费了 32 分钟。然后我们查看一下预测结果：

```py
unet = keras.models.load_model("/kaggle/working/best-u_net-model", custom_objects={'dice_coef': dice_coef, 'dice_loss': dice_loss})

show_predictions(model = unet, dataset = batched_train_dataset, num = 6)
```

![](../Images/49623a94ebb3eda2868cbb2909a1dd30.png)

完整 U-Net 的预测掩码。要好得多！作者提供。

这些预测结果要好得多。从多个预测中可以看到的一点是，车上的天线对网络来说很难处理。由于图像非常像素化，我不能责怪网络未能检测到这一点。

为了提高性能，需要调整包括以下在内的超参数：

+   下采样和上采样块的数量

+   滤波器数量

+   图像分辨率

+   训练集的大小

+   损失函数（也许将骰子损失与二进制交叉熵损失结合起来）

+   调整优化器参数。训练稳定性似乎是两个模型的问题。[来自文档](https://www.tensorflow.org/api_docs/python/tf/keras/optimizers/Adam)：“epsilon 的默认值 1e-7 可能并不是一个好的默认值。” 增加 epsilon 一个数量级或更多可能有助于提高训练稳定性。

我们已经可以看到在 Carvana 挑战中获得优秀分数的道路。真可惜比赛已经结束了！

# 总结

这篇文章深入探讨了图像分割的主题，特别是**二元分割**。如果你记住任何东西，请记住以下内容：

+   图像分割的目标是找到从图像中的输入像素值到模型可以用来为每个像素分配类别的输出数字的映射。

+   第一步是将你的图像组织成TensorFlow数据集对象，并查看你的图像和相应的掩码。

+   关于模型架构，不需要重新发明轮子：我们知道U-Net效果良好。

+   Dice得分是一个常用的指标，用于监控模型预测的成功。我们也可以从中获得损失函数。

未来的工作可能会将经典U-Net架构中的最大池化层转换为步幅卷积层。

祝你的图像分割问题好运！

# 参考文献

[1] O. Ronneberger, P. Fischer, 和 T. Brox, [U-Net：用于生物医学图像分割的卷积网络](https://arxiv.org/abs/1505.04597) (2015)，MICCAI 2015国际会议

[2] F. Chollet, 《用Python进行深度学习》（2021），Manning Publications Co.

[3] K. He, X. Zhang, S. Ren, J. Sun, [深入探讨整流器：在ImageNet分类中超越人类水平的表现](https://arxiv.org/abs/1502.01852) (2015)，国际计算机视觉大会（ICCV）
