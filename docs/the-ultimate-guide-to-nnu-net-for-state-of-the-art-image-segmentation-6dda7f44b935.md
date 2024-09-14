# nnU-Net 终极指南

> 原文：[`towardsdatascience.com/the-ultimate-guide-to-nnu-net-for-state-of-the-art-image-segmentation-6dda7f44b935`](https://towardsdatascience.com/the-ultimate-guide-to-nnu-net-for-state-of-the-art-image-segmentation-6dda7f44b935)

## 理解最先进的 nnU-Net 及其如何应用于你自己的数据集所需了解的一切。

[](https://medium.com/@francoisporcher?source=post_page-----6dda7f44b935--------------------------------)![François Porcher](https://medium.com/@francoisporcher?source=post_page-----6dda7f44b935--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6dda7f44b935--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6dda7f44b935--------------------------------) [François Porcher](https://medium.com/@francoisporcher?source=post_page-----6dda7f44b935--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6dda7f44b935--------------------------------) ·阅读时长 13 分钟·2023 年 8 月 2 日

--

![](img/e93dc02adab7ff7d7eafaebe00a09554.png)

Neuroimaging，作者：Milak Fakurian，图片来源于 Unsplash，[链接](https://unsplash.com/photos/58Z17lnVS4U)

在剑桥大学的深度学习与神经科学研究实习期间，我使用了大量的 nnU-Net，它在语义图像分割中是一个极其强大的基准。

不过，我在完全理解模型及其训练方法时遇到了一些困难，并且在互联网上没有找到太多帮助。现在我对它已经很熟悉了，因此我创建了这个教程，以帮助你，无论是更好地理解这个模型的内涵，还是如何在你自己的数据集中使用它。

在本指南中，你将：

1.  开发 nnU-Net 关键贡献的简要概述。

1.  学会如何将 nnU-Net 应用到你自己的数据集中。

所有代码均可在这个 [Google Collab notebook](https://colab.research.google.com/drive/1h6scef1i258x0abxT_FcSI_QBN9Eh_9e?usp=sharing) 中找到

> 这项工作花费了我大量的时间和精力。如果你觉得这个内容有价值，请考虑关注我，以增加它的可见度并支持更多类似教程的创作！

# nnU-Net 的简要历史

作为**图像分割领域的最先进模型**，nnU-Net 在 2D 和 3D 图像处理方面都具有强大的实力。它的表现非常稳健，成为了新的计算机视觉架构的强基准。如果你打算进入开发新型计算机视觉模型的领域，可以把 nnU-Net 作为你的**“超越目标”**。

这个强大的工具基于 U-Net 模型（你可以在这里找到我的一个教程: [Cook your first U-Net](https://medium.com/@foporcher/cooking-your-first-u-net-for-image-segmentation-e812e37e9cd0)），它首次亮相于 2015 年。名称“nnU-Net”代表“无新 U-Net”，意味着它的**设计并未引入革命性的架构变化。** 相反，它利用一系列巧妙的优化策略充分挖掘现有 U-Net 结构的潜力。

与许多现代神经网络不同，nnU-Net 并不依赖残差连接、密集连接或注意力机制。它的强项在于其细致的优化策略，包括重采样、归一化、谨慎选择损失函数、优化器设置、数据增强、基于补丁的推断和模型集成。这种整体方法使 nnU-Net 能够推动原始 U-Net 架构的极限。

# 探索 nnU-Net 中的多样化架构

尽管它可能看起来像一个单一的实体，nnU-Net 实际上是对三种不同类型的 U-Net 的总称：

![](img/5d874d9c4bc9ca3cb7356906001aafea.png)

2D、3D 和级联，图片来源于 [nnU-Net 文章](https://arxiv.org/abs/1809.10486)

1.  **2D U-Net:** 可以说是最知名的变体，它直接处理 2D 图像。

1.  **3D U-Net:** 这是 2D U-Net 的扩展，能够通过应用 3D 卷积直接处理 3D 图像。

1.  **U-Net Cascade:** 该模型生成低分辨率分割结果，并随后进行精细化处理。

每种架构都有其独特的优点，并且不可避免地有一定的局限性。

例如，使用 2D U-Net 进行 3D 图像分割可能看起来违反直觉，但实际上，它仍然可以非常有效。这是通过将 3D 体积切片为 2D 平面来实现的。

尽管 3D U-Net 由于其更高的参数数量看起来更复杂，但它并不总是最有效的解决方案。特别是，3D U-Net 通常在处理各轴空间分辨率不一致时表现不佳（例如，x 轴为 1mm，而 z 轴为 1.2mm）。

当处理大型图像时，U-Net Cascade 变体尤其有用。它采用初步模型来压缩图像，然后使用标准的 3D U-Net 生成低分辨率分割结果。生成的预测结果随后被放大，从而得到精细化的全面输出。

![](img/a664858b9bbc6fc6c909880bdc09e685.png)

图片来源于 [nnU-Net 文章](https://arxiv.org/abs/1809.10486)

通常，这一方法包括在 nnU-Net 框架内训练所有三个模型变体。接下来的步骤可能是从这三者中选择表现最佳的一个，或者使用集成技术。其中一种技术可能涉及整合 2D 和 3D U-Net 的预测结果。

然而，值得注意的是，这一过程可能非常耗时（并且也需要资金，因为你需要 GPU 计算资源）。如果你的限制只允许训练一个模型，不必担心。你可以选择只训练一个模型，因为集成模型仅带来微小的收益。

此表格展示了与特定数据集相关的最佳性能模型变体：

![](img/47f06af7a3d5b2e7a3c57dde766c77aa.png)

图片来自 [nnU-Net 文章](https://arxiv.org/abs/1809.10486)

## 网络拓扑的动态适应

鉴于图像大小差异显著（考虑肝脏图像的中位形状为 482 × 512 × 512，而海马体图像为 36 × 50 × 35），nnU-Net 智能地调整了输入补丁的大小以及每个轴的池化操作数量。这本质上意味着对每个数据集卷积层数量的自动调整，从而促进了空间信息的有效汇聚。除了适应不同的图像几何形状外，该模型还考虑了技术约束，例如可用内存。

需要注意的是，该模型并不是直接在整个图像上进行分割，而是在精心提取的重叠区域的补丁上进行。对这些补丁的预测结果随后会进行平均，从而得到最终的分割输出。

但是，较大的补丁意味着更多的内存使用，同时批量大小也会消耗内存。所做的权衡是始终优先考虑补丁大小（模型的容量）而非批量大小（仅对优化有用）。

这是计算最佳补丁大小和批量大小的启发式算法：

![](img/7048bc42072c03bd4ca7efa8edaac8c8.png)

批量和补丁大小的启发式规则，图片来自 [nnU-Net 文章](https://arxiv.org/abs/1809.10486)

这就是在不同数据集和输入维度下的表现：

![](img/a092fe248ec185074a2cd72b71c96018.png)

根据输入图像分辨率的架构，图片来自 [nnU-Net 文章](https://arxiv.org/abs/1809.10486)

很好！现在让我们快速回顾一下 nnU-Net 中使用的所有技术：

## 训练

所有模型都是从头开始训练的，并使用五折交叉验证进行评估，这意味着原始训练数据集被随机分为五个相等的部分或“折”。在这个交叉验证过程中，四个折用于模型的训练，剩下的一个折用于评估或测试。然后这个过程重复五次，每个折都被用作评估集一次。

对于损失，我们使用 Dice 损失和交叉熵损失的组合。这是图像分割中非常常见的损失。有关 Dice 损失的更多细节，请参阅[V-Net，U-Net 的大兄弟](https://medium.com/ai-mind-labs/v-net-u-nets-big-brother-in-image-segmentation-906e393968f7)

## 数据增强技术

nnU-Net 具有非常强大的数据增强管道。作者使用了随机旋转、随机缩放、随机弹性变形、伽马校正和镜像。

NB：你可以通过修改源代码添加自己的变换

![](img/56fa89a9ffb590f5d2d10c2f0d621694.png)

弹性变形，来自这篇[文章](https://www.researchgate.net/figure/Grid-distortion-and-elastic-transform-applied-to-a-medical-image_fig4_327742409)

![](img/d5d84805c71d600cd88c93a8d63a7aee.png)

图片来自[OpenCV 库](https://www.google.com/url?sa=i&url=https%3A%2F%2Fpyimagesearch.com%2F2015%2F10%2F05%2Fopencv-gamma-correction%2F&psig=AOvVaw2LDHscTrcGOq-tR4Ki2AbS&ust=1691168921010000&cd=vfe&opi=89978449&ved=0CBAQjRxqFwoTCKi4i6j9wIADFQAAAAAdAAAAABAE)

## 基于补丁的推断

正如我们所说，模型不会直接在全分辨率图像上进行预测，而是在提取的补丁上进行预测，然后汇总预测结果。

这就是它的样子：

![](img/cb25522983df31b26778b3f89f8ad728.png)

基于补丁的推断，作者提供的图片

NB：图像中心的补丁比边缘的补丁赋予更高的权重，因为它们包含更多信息，模型在这些区域表现更好

## 成对模型集成

![](img/6da3d92b7f49ba1c22aaf9b0e0a7ad5c.png)

模型集成，作者提供的图片

如果你记得的话，我们可以训练多达 3 个不同的模型，2D、3D 和级联。但在推断时，我们只能一次使用一个模型，对吗？

实际情况是，不同模型具有不同的优缺点。因此，我们可以结合多个模型的预测，以便当一个模型非常自信时，我们优先考虑它的预测。

nnU-Net 测试所有 3 个可用模型中的 2 个模型组合，并选择最佳的一个。

实际上，有 2 种方法可以做到这一点：

**硬投票：** 对于每个像素，我们查看 2 个模型输出的所有概率，然后选择概率最高的类别。

**软投票：** 对于每个像素，我们计算模型概率的平均值，然后选择概率最大的类别。

# 实际实施

> 在我们开始之前，你可以从[这里](https://drive.google.com/drive/folders/1rQeujQZs5rPH1AeI-KoUICOAzs4u-Eie?usp=sharing)下载数据集，并跟随[Google Colab 笔记本](https://colab.research.google.com/drive/1h6scef1i258x0abxT_FcSI_QBN9Eh_9e?usp=sharing)。

如果你没有理解第一部分的内容，不用担心，这是实践部分，你只需跟随我，你仍然会获得最佳结果。

你需要一个 GPU 来训练模型，否则无法运行。你可以在本地或者在 Google Collab 上进行训练，**不要忘记将运行时更改为 GPU**

所以，首先，你需要准备一个包含输入图像及其对应分割的数据集。你可以按照我的教程下载这个用于 3D 大脑分割的准备好数据集，然后你可以用自己的数据集替换它。

## 下载数据

首先，你应该下载数据并将其放置在数据文件夹中，将两个文件夹命名为“input”和“ground_truth”，其中包含分割数据。

在接下来的教程中，我将使用 MindBoggle 数据集进行图像分割。你可以在这个[Google Drive](https://drive.google.com/drive/folders/1rQeujQZs5rPH1AeI-KoUICOAzs4u-Eie?usp=sharing)上下载：

我们得到的是 3D 大脑 MRI 扫描，我们想要分割白质和灰质：

![](img/105b1c3c613a1f97eca045e11b3bac52.png)

作者提供的图片

它应该是这样的：

![](img/08fad5b409f9fd75a6221ba5f8318385.png)

树，作者提供的图片

## 设置主目录

如果你在 Google Colab 上运行此代码，将 collab 设置为 True，否则 collab 设置为 False

```py
collab = True

import os
import shutil
#libraries
from collections import OrderedDict
import json
import numpy as np

#visualization of the dataset
import matplotlib.pyplot as plt
import nibabel as nib

if collab:
    from google.colab import drive
    drive.flush_and_unmount()
    drive.mount('/content/drive', force_remount=True)
    # Change "neurosciences-segmentation" to the name of your project folder
    root_dir = "/content/drive/MyDrive/neurosciences-segmentation"

else:
    # get the dir of the parent dir
    root_dir = os.getcwd()

input_dir = os.path.join(root_dir, 'data/input')
segmentation_dir = os.path.join(root_dir, 'data/ground_truth')

my_nnunet_dir = os.path.join(root_dir,'my_nnunet')
print(my_nnunet_dir)
```

现在我们将定义一个函数来为我们创建文件夹：

```py
def make_if_dont_exist(folder_path,overwrite=False):
    """
    creates a folder if it does not exists
    input:
    folder_path : relative path of the folder which needs to be created
    over_write :(default: False) if True overwrite the existing folder
    """
    if os.path.exists(folder_path):

        if not overwrite:
            print(f'{folder_path} exists.')
        else:
            print(f"{folder_path} overwritten")
            shutil.rmtree(folder_path)
            os.makedirs(folder_path)

    else:
      os.makedirs(folder_path)
      print(f"{folder_path} created!")
```

并且我们使用这个函数来创建我们的“my_nnunet”文件夹，在那里一切都将被保存

```py
os.chdir(root_dir)
make_if_dont_exist('my_nnunet', overwrite=False)
os.chdir('my_nnunet')
print(f"Current working directory: {os.getcwd()}")
```

## 安装库

现在我们将安装所有的依赖项。首先，让我们安装 nnunet 库。如果你在笔记本中，请在单元格中运行以下命令：

```py
!pip install nnunet
```

否则你可以直接通过终端安装 nnunet，命令如下：

```py
pip install nnunet
```

现在我们将克隆 nnUnet git 仓库和 NVIDIA apex。这些包含了训练脚本以及 GPU 加速器。

```py
!git clone https://github.com/MIC-DKFZ/nnUNet.git
!git clone https://github.com/NVIDIA/apex

# repository dir is the path of the github folder
respository_dir = os.path.join(my_nnunet_dir,'nnUNet')
os.chdir(respository_dir)
!pip install -e
!pip install --upgrade git+https://github.com/nanohanno/hiddenlayer.git@bugfix/get_trace_graph#egg=hiddenlayer
```

## 创建文件夹

nnUnet 对文件夹结构有非常具体的要求。

```py
task_name = 'Task001' #change here for different task name

# We define all the necessary paths
nnunet_dir = "nnUNet/nnunet/nnUNet_raw_data_base/nnUNet_raw_data"
task_folder_name = os.path.join(nnunet_dir,task_name) 
train_image_dir = os.path.join(task_folder_name,'imagesTr') # path to training images
train_label_dir = os.path.join(task_folder_name,'labelsTr') # path to training labels
test_dir = os.path.join(task_folder_name,'imagesTs') # path to test images
main_dir = os.path.join(my_nnunet_dir,'nnUNet/nnunet') # path to main directory
trained_model_dir = os.path.join(main_dir, 'nnUNet_trained_models') # path to trained models
```

最初，nnU-Net 是为具有不同任务的十项全能挑战设计的。如果你有不同的任务，请为所有任务运行此单元格。

```py
# Creation of all the folders
overwrite = False # Set this to True if you want to overwrite the folders
make_if_dont_exist(task_folder_name,overwrite = overwrite)
make_if_dont_exist(train_image_dir, overwrite = overwrite)
make_if_dont_exist(train_label_dir, overwrite = overwrite)
make_if_dont_exist(test_dir,overwrite= overwrite)
make_if_dont_exist(trained_model_dir, overwrite=overwrite)
```

现在你应该有如下的结构：

![](img/90cb0cd8c344fe8b30fade3ab7cdc037.png)

作者提供的图片

## 设置环境变量

脚本需要知道你将 raw_data 放在哪里，在哪里可以找到预处理的数据，以及需要将结果保存到哪里。

```py
os.environ['nnUNet_raw_data_base'] = os.path.join(main_dir,'nnUNet_raw_data_base')
os.environ['nnUNet_preprocessed'] = os.path.join(main_dir,'preprocessed')
os.environ['RESULTS_FOLDER'] = trained_model_dir
```

## 将文件移动到正确的仓库中：

我们定义了一个函数，将我们的图像移动到 nnunet 文件夹中的正确仓库：

```py
def copy_and_rename(old_location,old_file_name,new_location,new_filename,delete_original = False):
    shutil.copy(os.path.join(old_location,old_file_name),new_location)
    os.rename(os.path.join(new_location,old_file_name),os.path.join(new_location,new_filename))
    if delete_original:
        os.remove(os.path.join(old_location,old_file_name))
```

现在让我们运行这个函数来处理输入和真实值图像：

```py
list_of_all_files = os.listdir(segmentation_dir)
list_of_all_files = [file_name for file_name in list_of_all_files if file_name.endswith('.nii.gz')]

for file_name in list_of_all_files:
    copy_and_rename(input_dir,file_name,train_image_dir,file_name)
    copy_and_rename(segmentation_dir,file_name,train_label_dir,file_name)
```

现在我们需要将文件重命名为 nnUnet 格式接受的名称，例如 subject.nii.gz 将变为 subject_0000.nii.gz

```py
def check_modality(filename):
    """
    check for the existence of modality
    return False if modality is not found else True
    """
    end = filename.find('.nii.gz')
    modality = filename[end-4:end]
    for mod in modality:
        if not(ord(mod)>=48 and ord(mod)<=57): #if not in 0 to 9 digits
            return False
    return True

def rename_for_single_modality(directory):

    for file in os.listdir(directory):

        if check_modality(file)==False:
            new_name = file[:file.find('.nii.gz')]+"_0000.nii.gz"
            os.rename(os.path.join(directory,file),os.path.join(directory,new_name))
            print(f"Renamed to {new_name}")
        else:
            print(f"Modality present: {file}")

rename_for_single_modality(train_image_dir)
# rename_for_single_modality(test_dir)
```

## 设置 JSON 文件

我们快完成了！

你主要需要修改 2 件事：

1.  模态（如果是 CT 或 MRI，这会改变归一化）

1.  标签：输入你自己的类别

```py
overwrite_json_file = True #make it True if you want to overwrite the dataset.json file in Task_folder
json_file_exist = False

if os.path.exists(os.path.join(task_folder_name,'dataset.json')):
    print('dataset.json already exist!')
    json_file_exist = True

if json_file_exist==False or overwrite_json_file:

    json_dict = OrderedDict()
    json_dict['name'] = task_name
    json_dict['description'] = "Segmentation of T1 Scans from MindBoggle"
    json_dict['tensorImageSize'] = "3D"
    json_dict['reference'] = "see challenge website"
    json_dict['licence'] = "see challenge website"
    json_dict['release'] = "0.0"

    ######################## MODIFY THIS ########################

    #you may mention more than one modality
    json_dict['modality'] = {
        "0": "MRI"
    }
    #labels+1 should be mentioned for all the labels in the dataset
    json_dict['labels'] = {
        "0": "Non Brain",
        "1": "Cortical gray matter",
        "2": "Cortical White matter",
        "3" : "Cerebellum gray ",
        "4" : "Cerebellum white"
    }

    #############################################################

    train_ids = os.listdir(train_label_dir)
    test_ids = os.listdir(test_dir)
    json_dict['numTraining'] = len(train_ids)
    json_dict['numTest'] = len(test_ids)

    #no modality in train image and labels in dataset.json
    json_dict['training'] = [{'image': "./imagesTr/%s" % i, "label": "./labelsTr/%s" % i} for i in train_ids]

    #removing the modality from test image name to be saved in dataset.json
    json_dict['test'] = ["./imagesTs/%s" % (i[:i.find("_0000")]+'.nii.gz') for i in test_ids]

    with open(os.path.join(task_folder_name,"dataset.json"), 'w') as f:
        json.dump(json_dict, f, indent=4, sort_keys=True)

    if os.path.exists(os.path.join(task_folder_name,'dataset.json')):
        if json_file_exist==False:
            print('dataset.json created!')
        else:
            print('dataset.json overwritten!')
```

## 将数据预处理为 nnU-Net 格式

这将创建 nnU-Net 格式的数据集

```py
# -t 1 means "Task001", if you have a different task change it
!nnUNet_plan_and_preprocess -t 1 --verify_dataset_integrity
```

## 训练模型

我们现在准备好训练模型了！

训练 3D U-Net：

```py
#train 3D full resolution U net
!nnUNet_train 3d_fullres nnUNetTrainerV2 1 0 --npz 
```

训练 2D U-Net：

```py
# train 2D U net
!nnUNet_train 2d nnUNetTrainerV2 1 0 --npz
```

训练级联模型：

```py
# train 3D U-net cascade
!nnUNet_train 3d_lowres nnUNetTrainerV2CascadeFullRes 1 0 --npz
!nnUNet_train 3d_fullres nnUNetTrainerV2CascadeFullRes 1 0 --npz
```

注意：如果你暂停训练并想继续，末尾添加“-c”表示“继续”。

例如：

```py
#train 3D full resolution U net
!nnUNet_train 3d_fullres nnUNetTrainerV2 1 0 --npz 
```

## 推断

现在我们可以进行推断：

```py
result_dir = os.path.join(task_folder_name, 'nnUNet_Prediction_Results')
make_if_dont_exist(result_dir, overwrite=True)

# -i is the input folder
# -o is where you want to save the predictions
# -t 1 means task 1, change it if you have a different task number
# Use -m 2d, or -m 3d_fullres, or -m 3d_cascade_fullres
!nnUNet_predict -i /content/drive/MyDrive/neurosciences-segmentation/my_nnunet/nnUNet/nnunet/nnUNet_raw_data_base/nnUNet_raw_data/Task001/imagesTs -o /content/drive/MyDrive/neurosciences-segmentation/my_nnunet/nnUNet/nnunet/nnUNet_raw_data_base/nnUNet_raw_data/Task001/nnUNet_Prediction_Results -t 1 -tr nnUNetTrainerV2 -m 2d -f 0  --num_threads_preprocessing 1
```

## 预测的可视化

首先让我们检查训练损失。这看起来非常健康，我们有一个 Dice 分数 > 0.9（绿色曲线）。

对于这么少的工作量和一个 3D 神经影像分割任务，这确实非常出色。

![](img/c211daa238ad5b5ee4c518148ee9fb4b.png)

训练损失、测试损失、验证 Dice，图像由作者提供

让我们看一个样本：

![](img/c6a69656aceafc62f4945052409777de.png)

对 MindBoggle 数据集的预测，图像由作者提供

结果确实令人印象深刻！显然模型已经有效地学会了如何高精度地对脑部图像进行分割。虽然可能存在一些小的瑕疵，但要记住，图像分割领域正在快速发展，我们正朝着完美迈出重要步伐。

未来有机会进一步优化 nnU-Net 的性能，但这将是另一篇文章的内容。

感谢阅读！在你离开之前：

+   查看我在 GitHub 上的[AI 教程合集](https://github.com/FrancoisPorcher/awesome-ai-tutorials)

[](https://github.com/FrancoisPorcher/awesome-ai-tutorials?source=post_page-----6dda7f44b935--------------------------------) [## GitHub - FrancoisPorcher/awesome-ai-tutorials: 最佳 AI 教程合集，让你成为…

### 最佳 AI 教程合集，让你成为数据科学的高手！ - GitHub …

github.com](https://github.com/FrancoisPorcher/awesome-ai-tutorials?source=post_page-----6dda7f44b935--------------------------------)

*你应该在收件箱中收到我的文章。* [***订阅这里。***](https://medium.com/@francoisporcher/subscribe)

*如果你想访问 Medium 上的高级文章，只需每月$5 的会员资格。如果你通过* [***我的链接***](https://medium.com/@francoisporcher/membership)*注册，你将以没有额外费用的方式支持我。*

> 如果你觉得这篇文章有见地且有益，请考虑关注我并点赞以获取更多深入内容！你的支持帮助我继续制作有助于我们共同理解的内容。

# 参考文献

1.  Ronneberger, O., Fischer, P., & Brox, T. (2015). U-net：用于生物医学图像分割的卷积网络。在医学图像计算与计算机辅助手术国际会议（第 234–241 页）。Springer, Cham。

1.  Isensee, F., Jaeger, P. F., Kohl, S. A., Petersen, J., & Maier-Hein, K. H. (2021). nnU-Net：一种自配置的深度学习生物医学图像分割方法。Nature Methods, 18(2), 203–211。

1.  Ioffe, S., & Szegedy, C. (2015). 批量归一化：通过减少内部协变量偏移加速深度网络训练。arXiv 预印本 arXiv:1502.03167。

1.  Ulyanov, D., Vedaldi, A., & Lempitsky, V. (2016). 实例归一化：快速风格化的缺失成分。arXiv 预印本 arXiv:1607.08022。

1.  [MindBoggle 数据集](https://mindboggle.info)
