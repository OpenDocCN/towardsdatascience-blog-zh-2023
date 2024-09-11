# 使用 PDF 文档训练自定义 Detectron2 模型进行目标检测（第 1 部分）

> 原文：[https://towardsdatascience.com/training-and-deploying-a-custom-detectron2-model-for-object-detection-using-pdf-documents-part-1-c724f61d8b4b?source=collection_archive---------1-----------------------#2023-11-29](https://towardsdatascience.com/training-and-deploying-a-custom-detectron2-model-for-object-detection-using-pdf-documents-part-1-c724f61d8b4b?source=collection_archive---------1-----------------------#2023-11-29)

## 让你的机器学会像人类一样读取 PDF 文档

[](https://medium.com/@noahhaglund?source=post_page-----c724f61d8b4b--------------------------------)[![Noah Haglund](../Images/edfcc90677444ebced16549a1524d7fe.png)](https://medium.com/@noahhaglund?source=post_page-----c724f61d8b4b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c724f61d8b4b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c724f61d8b4b--------------------------------) [Noah Haglund](https://medium.com/@noahhaglund?source=post_page-----c724f61d8b4b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe93013369dfc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-and-deploying-a-custom-detectron2-model-for-object-detection-using-pdf-documents-part-1-c724f61d8b4b&user=Noah+Haglund&userId=e93013369dfc&source=post_page-e93013369dfc----c724f61d8b4b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c724f61d8b4b--------------------------------) · 11 分钟阅读 · 2023 年 11 月 29 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc724f61d8b4b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-and-deploying-a-custom-detectron2-model-for-object-detection-using-pdf-documents-part-1-c724f61d8b4b&user=Noah+Haglund&userId=e93013369dfc&source=-----c724f61d8b4b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc724f61d8b4b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-and-deploying-a-custom-detectron2-model-for-object-detection-using-pdf-documents-part-1-c724f61d8b4b&source=-----c724f61d8b4b---------------------bookmark_footer-----------)![](../Images/ae9cddf5c27f7be9a2175d7ecdc2f571.png)

我尝试了大半年的时间解决一个商业案例，通过使 PDF 文档机器可读，至少在头部/标题（指定一个部分的文本）可以从文档中提取出来，以及它们相关的内容，从而形成一些类似关系的数据结构。最初，我采用了卷积神经网络（CNN），以及 CNN 和递归神经网络（RNN）的结合，来使用文本和文本特征（字体、字体大小、粗细等）对文档结构进行分类。这个框架由 [Rahman & Finin](https://arxiv.org/pdf/1910.03678.pdf) 实现，并通过使用长短期记忆（LSTM）进行语义分类超越了推断文档结构的能力。我面临的问题是没有足够的时间和精力来准备和标注足够的数据，以便使用类似的框架制作准确的模型。

为了作为一个人团队让这个过程变得更简单，我转向了计算机视觉！与其尝试从 PDF 中提取文本和文本特征并推断文档结构，我决定将 PDF 页面转换为图像，通过目标检测可视化地推断文档结构，然后对相应的推断（例如：页面标题）及其相关内容（例如：标题的内容）进行 OCR。在这篇文章中，我将展示如何使用 Detectron2 来实现相同的目标！

Detectron2 是 Facebook AI 研究的下一代库，简化了构建计算机视觉应用程序的过程。在计算机视觉的各种可能用例（图像识别、语义分割、目标检测和实例分割）中，本文将训练一个自定义的 Detectron2 模型用于目标检测，其特点是使用边界框，易于与其他用例区分。训练完成后，我们将对应用进行 docker 化，并将其部署到 Heroku/AWS，同时探索内存管理和批处理推断等其他主题，以帮助你定制你的脚本和模型以适应你的用例。

为了跟随本文，你最好提前准备：

+   扎实的 Python 知识

+   熟悉 AWS

# **Detectron2 安装**

如果你是 Mac 或 Linux 用户，你真幸运！通过运行以下命令，这个过程将相对简单：

```py
pip install torchvision && pip install "detectron2@git+https://github.com/facebookresearch/detectron2.git@v0.5#egg=detectron2"
```

请注意，这个命令将编译库，所以你需要等待一会儿。如果你想安装支持 GPU 的 Detectron2，请参考官方 Detectron2 [安装说明](https://detectron2.readthedocs.io/en/latest/tutorials/install.html)获取详细信息。

然而，如果你是 Windows 用户，这个过程会有点麻烦，但我自己在 Windows 上也成功完成了。

紧密按照 [这里](https://layout-parser.readthedocs.io/en/latest/notes/installation.html#for-windows-users) Layout Parser 包提供的说明进行操作（这是一个很有用的包，如果你不想训练自己的 Detectron2 模型用于 PDF 结构/内容推断而希望依赖预标注数据！这无疑更省时间，但你会发现针对特定用例，你可以在自己的数据上训练一个更准确且更小的模型，这对于部署中的内存管理非常有利，稍后我会讨论）。确保安装 pycocotools，以及 Detectron2，因为这个包将帮助加载、解析和可视化 [COCO](https://cocodataset.org/#home) 数据，这是我们训练 Detectron2 模型所需的数据格式。

本文系列的第2部分将使用本地 Detectron2 安装，我们将在文章的后续部分使用 AWS EC2 实例进行 Detectron2 训练。

# **Detectron2 自定义训练 — 使用 LabelMe 进行标注**

对于图像标注，我们需要两个东西：（1）我们将要标注的图像，以及（2）一个标注工具。将所有要标注的图像整理到一个目录中，如果你跟随我的用例并希望使用 PDF 图像，则整理一个 PDF 目录，并安装 pdftoimage 包：

```py
pip install pdf2image
```

然后使用以下脚本将每个 PDF 页面转换为图像：

```py
import os
from pdf2image import convert_from_path

# Assign input_dir to PDF dir, ex: "C://Users//user//Desktop//pdfs"
input_dir = "##"
# Assign output_dir to the dir you’d like the images to be saved"
output_dir = "##"
dir_list = os.listdir(input_dir)

index = 0
while index < len(dir_list):

    images = convert_from_path(f"{input_dir}//" + dir_list[index])
    for i in range(len(images)):
        images[i].save(f'{output_dir}//doc' + str(index) +'_page'+ str(i) +'.jpg', 'JPEG')
    index += 1
```

一旦你有了一个图像目录，我们将使用 LabelMe 工具，安装说明请见 [这里](https://github.com/wkentaro/labelme#installation)。安装完成后，只需在命令行或终端中运行 labelme 命令。这将打开一个具有以下布局的窗口：

![](../Images/602961b6c75732530f4b0da3152bc93d.png)

点击左侧的“打开目录”选项，打开保存图像的目录（我们也称这个目录为“train”）。LabelMe 将打开目录中的第一张图像，并允许你对每张图像进行标注。右键单击图像以找到各种标注选项，例如创建多边形以点击图像中给定对象周围的每个点，或创建矩形以捕捉对象，同时确保90度角。

一旦放置了边界框/多边形，LabelMe 会要求输入标签。在下面的示例中，我为页面上的每个标题实例提供了标签头。你可以使用多个标签来识别图像中的各种对象（对于 PDF 示例，这可能是标题/头部、表格、段落、列表等），但出于我的目的，我将仅识别标题/头部，然后在模型推断后通过算法将每个标题与其相应的内容关联起来（见第2部分）。

![](../Images/e10b5f7e2ac5c5de670c867b7fde0d81.png)

注释完成后，点击保存按钮，然后点击下一张图片以注释给定目录中的下一张图片。Detectron2 在使用最少的数据进行推断时表现出色，因此可以随意注释大约 100 张图片进行初步训练和测试，然后再进一步注释和训练以提高模型的准确性（请记住，对多个标签类别进行训练会降低一点准确性，需要更大的数据集来提高准确性）。

一旦训练目录中的每张图片都被注释过，我们就可以将这些图片/注释对的约 20% 移动到一个单独的名为 test 的目录中。

如果你熟悉机器学习，一个简单的经验法则是需要有测试/训练/验证的拆分（60–80% 的训练数据，10–20% 的验证数据，以及 10–20% 的测试数据）。在这里，我们只是做一个测试/训练的拆分，即 20% 的测试和 80% 的训练。

# **Detectron2 自定义训练 — COCO 格式**

现在我们有了注释的文件夹，我们需要将 labelme 注释转换为 COCO 格式。你可以通过 [我这里的这个仓库](https://github.com/nzh2534/detectron2tutorial/tree/main) 中的 labelme2coco.py 文件来简单完成这个操作。我从 [Tony607](https://github.com/Tony607/labelme2coco/tree/master) 重构了这个脚本，它将转换多边形注释 *和* 任何矩形注释（因为初始脚本没有正确转换矩形注释到 COCO 格式）。

一旦下载了 labelme2coco.py 文件，通过在终端运行以下命令来执行它：

```py
python labelme2coco.py path/to/train/folder
```

它会输出一个 train.json 文件。对于测试文件夹，再次运行命令，并编辑 labelme2coco.py 的第 172 行，将默认输出名称更改为 test.json（否则它会覆盖 train.json 文件）。

# **Detectron2 自定义训练 — EC2**

既然繁琐的注释过程已经结束，我们可以进入有趣的部分——训练！

如果你的计算机没有 Nvidia GPU 功能，我们需要使用 AWS 启动一个 EC2 实例。Detectron2 模型可以在 CPU 上进行训练，但如果你这样做，你会发现这将花费非常长的时间，而使用 [Nvidia CUDA](https://developer.nvidia.com/cuda-toolkit) 的 GPU 实例则可以在几分钟内完成模型训练。

首先，登录到 [AWS 控制台](https://aws.amazon.com/console/)。登录后，在搜索栏中搜索 EC2 以进入 EC2 仪表板。在这里，点击屏幕左侧的实例，然后点击启动实例按钮。

![](../Images/ad3c3b76c2e8bfcd9367144dafe96ac0.png)

你需要为实例提供的最低详细信息是：

+   **一个名字**

+   **Amazon 机器镜像（AMI）** 指定了软件配置。确保使用一个具有 GPU 和 PyTorch 功能的 AMI，因为它将包含 CUDA 所需的包和 Detectron2 额外依赖项，如 Torch。为了跟随本教程，也使用 Ubuntu AMI。我使用了 AMI —— Deep Learning AMI GPU PyTorch 2.1.0（Ubuntu 20.04）。

+   **实例类型** 指定了硬件配置。请参考 [这里](https://aws.amazon.com/ec2/instance-types/) 的指南了解各种实例类型。我们希望使用性能优化的实例，例如 P 或 G 实例系列中的一个。我使用了 p3.2xlarge，它提供了我们所需的所有计算能力，特别是 GPU 能力。

> 请注意：P 系列的实例需要您联系 AWS 客户服务以申请配额增加（因为由于相关费用，它们不会立即允许基本用户访问性能更高的实例）。如果您使用 p3.2xlarge 实例，您需要申请将配额增加到 8 vCPU。

+   指定一个 **密钥对（登录）。** 如果您尚未创建此密钥对，请创建一个，并可以随意将其命名为 p3key，正如我所做的那样。

![](../Images/e2389169d19bea071a94db6806fc3986.png)

+   最后，**配置存储。** 如果您使用了与我相同的 AMI 和实例类型，您将看到起始默认存储为 45gb。根据您的训练数据集大小，可以将其增加到约 60gb 或更多，以确保实例有足够的空间存储您的图像。

启动您的实例并点击实例 ID 超链接以在 EC2 仪表板中查看它。当实例运行时，打开命令提示符窗口，我们将使用以下命令通过 SSH 连接到 EC2 实例（并确保将粗体文本替换为 (1) 您的 .pem 密钥对的路径和 (2) 您的 EC2 实例的地址）：

```py
ssh -L 8000:localhost:8888 -i **C:\path\to\p3key.pem** ubuntu@**ec2id.ec2region.compute.amazonaws.com**
```

由于这是一个新主机，请对以下消息回答“是”：

![](../Images/1bf5b52c248ee53ee2c20a84d7135356.png)

然后 Ubuntu 将启动，并带有一个名为 PyTorch（来自 AWS AMI）的预打包虚拟环境。激活 venv 并使用以下两个命令启动预安装的 Jupyter Notebook：

![](../Images/8302201c5df6ed23bbf6e20081fabdfe.png)

这将返回供您复制并粘贴到浏览器中的 URL。将包含 localhost 的 URL 复制到浏览器中，并将 8888 改为 8000。这样会带您到一个看起来类似于此的 Jupyter Notebook：

![](../Images/ffe5c78684dddb1228cf7479ae0717ea.png)

从 [我的 GitHub 仓库](https://github.com/nzh2534/detectron2tutorial/blob/main/Detectron2__Tutorial.ipynb) 中，将 Detectron2_Tutorial.ipynb 文件上传到笔记本中。从这里，运行安装头下的命令以完全安装 Detectron2。然后，重启运行时以确保安装生效。

返回到重启的笔记本中后，我们需要上传一些额外的文件，然后才能开始训练过程：

+   来自 [github repo](https://github.com/nzh2534/detectron2tutorial/blob/main/utils.py) 的 utils.py 文件。此文件为 Detectron2 提供了包含配置详细信息的 .ipynb 文件（如果你对配置细节感兴趣，请参阅文档 [这里](https://detectron2.readthedocs.io/en/latest/modules/config.html)）。该文件中还包含一个 *plot_samples* 函数，在 .ipynb 文件中有引用，但**在两个文件中都被注释掉了**。如果你希望在训练过程中查看样本的可视化，可以取消注释并使用此函数。请注意，你需要进一步安装 cv2 才能使用 plot_samples 功能。

+   使用 labelme2coco.py 脚本生成的 train.json 和 test.json 文件。

+   一个包含 Train 图像目录和 Test 图像目录的 zip 文件（压缩目录允许你只上传一个项目到笔记本中；你可以将 labelme 注释文件保留在目录中，这不会影响训练）。上传这两个 zip 文件后，通过点击笔记本右上角的 (1) New 和 (2) Terminal 打开一个终端，并使用以下命令解压每个文件，在笔记本中创建一个单独的 Train 和 Test 图像目录：

```py
! unzip ~/train.zip -d ~/
! unzip ~/test.zip -d ~/
```

最后，运行 .ipynb 文件中 Training 部分的笔记本单元。最后一个单元将输出类似于以下的响应：

![](../Images/309393f75f12959eba0a6bbc4f9434e5.png)

这将显示用于训练的图像数量，以及你在训练数据集中标注的实例计数（这里，在训练之前找到“title”类别的 470 个实例）。然后，Detectron2 将数据序列化，并按配置中指定的批次加载数据（utils.py）。

一旦训练开始，你将看到 Detectron2 打印事件：

![](../Images/8b88dbef3bd6f86760a4aac08ddea6f4.png)

这让你了解以下信息：估计剩余的训练时间、Detectron2 已执行的迭代次数，以及最重要的监控准确度的 total_loss，它是其他损失计算的指标，表示模型在单个示例上的预测有多差。如果模型的预测是完美的，则损失为零；否则，损失会更大。如果模型不完美也不要担心！我们可以随时添加更多标注数据来提高模型的准确度，或者在我们的应用中使用最终训练好的模型的高分推断（表示模型对推断准确性的信心）。

完成后，在笔记本中会创建一个名为 output 的目录，其中包含一个名为 object detection 的子目录，该子目录包含与训练事件和指标相关的文件、记录模型检查点的文件，以及一个名为 model_final.pth 的 .pth 文件。这是已经保存和训练好的 Detectron2 模型，现在可以用于在部署的应用程序中进行推断！在关闭或终止 AWS EC2 实例之前，请确保下载此文件。

现在我们已经有了 model_final.pth，请继续阅读 *第2部分：设计与部署* 文章，该文章将涵盖使用机器学习的应用程序的部署过程，并提供一些关键提示，帮助提高这一过程的效率。

*除非另有说明，本文中使用的所有图片均为作者所用*
