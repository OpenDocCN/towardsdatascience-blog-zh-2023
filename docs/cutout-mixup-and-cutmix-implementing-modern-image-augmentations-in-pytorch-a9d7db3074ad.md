# Cutout、Mixup 和 Cutmix：在 PyTorch 中实现现代图像增强

> 原文：[`towardsdatascience.com/cutout-mixup-and-cutmix-implementing-modern-image-augmentations-in-pytorch-a9d7db3074ad`](https://towardsdatascience.com/cutout-mixup-and-cutmix-implementing-modern-image-augmentations-in-pytorch-a9d7db3074ad)

## 用 Python 实现的计算机视觉数据增强技术

[](https://medium.com/@iamleonie?source=post_page-----a9d7db3074ad--------------------------------)![Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----a9d7db3074ad--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a9d7db3074ad--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a9d7db3074ad--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----a9d7db3074ad--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----a9d7db3074ad--------------------------------) ·阅读时间 9 分钟·2023 年 4 月 14 日

--

![](img/9c2cd9a7e9619a0232564ae5df5fe0b3.png)

Cutmix 图像增强（背景图像由作者绘制，人工生成的雕像照片使用 DALLE）

几乎可以保证，应用数据增强会提升你的神经网络性能。增强是一种正则化技术，可以人工扩展你的训练数据，帮助你的深度学习模型更好地泛化。因此，图像增强可以提升模型性能。

> 图像增强可以提升模型性能

经典的图像增强技术用于计算机视觉中的卷积神经网络，包括缩放、裁剪、翻转或旋转图像。

在最近的一篇关于中级深度学习技术的文章中，我们了解到除了经典技术之外，最有效的图像增强技术是：

+   Cutout [2]

+   Mixup [4]

+   Cutmix [3]

![](img/ce29c82e0d40ffe0118468fcd443604c.png)

数据增强技术：Mixup、Cutout、Cutmix

本文将简要描述上述图像增强及其在 PyTorch 深度学习框架中的 Python 实现。

# 设置

本教程将使用一个“基础”的图像分类问题作为示例。任务是分类郁金香和玫瑰的图像：

![](img/24714a2fbc65e76b46ab0b87b1a2b4cf.png)

用于图像分类的玩具数据集[1]：玫瑰还是郁金香。

*在此插入你的数据！* — 为了跟随本文，你的数据集应该类似于这样：

![](img/b43b29fe87f67bc2974bd7044861e9b4.png)

用于图像分类的玩具数据集[1]。在此插入你的数据。

PyTorch（版本 1.11.0）、OpenCV（版本 4.5.4）和 `albumentations`（版本 1.3.0）。

```py
import torch
from torch.utils.data import DataLoader, Dataset
import torch.utils.data as data_utils

import cv2
import numpy as np

import albumentations as A
from albumentations.pytorch import ToTensorV2
```

PyTorch `Dataset`使用 OpenCV 加载图像。

```py
class ExampleDataset(Dataset):
    def __init__(self, 
                data, 
                transform = None):
        self.file_paths = data['file_paths'].values
        self.labels = data['labels'].values
        self.transform = transform

    def __len__(self):
        return len(self.file_paths)

    def __getitem__(self, idx):
        # Get file_path and label for index
        label = self.labels[idx]
        file_path = self.file_paths[idx]

        # Read an image with OpenCV
        image = cv2.imread(file_path)

        # Convert the image to RGB color space.
        image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)

        # Apply augmentations
        if self.transform:
            augmented = self.transform(image=image)
            image = augmented['image']

        return image, label 
```

PyTorch `DataLoader`随后将数据集划分为每批 8 张图像。这些基本图像变换将图像调整为 256x256 像素。

```py
transforms = A.Compose([
    A.Resize(256, 256), # Resize images
    ToTensorV2()])

example_dataset = ExampleDataset(train_df,
                                 transform = transforms)

train_dataloader = DataLoader(example_dataset, 
                              batch_size = 8, 
                              shuffle = True, 
                              num_workers = 0)
```

下面你可以看到一个普通图像分类问题的训练代码。所有分散注意力的代码部分已被省略，以简化代码并只关注相关部分。

由于我们有一个二分类问题，我们将使用`nn.CrossEntropyLoss()`。这是值得注意的，因为我们稍后将实现一个自定义损失函数。

```py
# Define device, model, optimizer, learning rate scheduler
device = ...
model = ...
criterion = nn.CrossEntropyLoss()
optimizer = ... 
scheduler = ...

for epoch in range(NUM_EPOCHS):        
    # Train
    model.train()

    # Define any variables for metrics
    ...

    # Iterate over data
    for samples, labels in (train_dataloader):

        samples, labels = samples.to(device), labels.to(device)

        # Normalize
        samples = samples/255

        # Zero the parameter gradients
        ...

        with torch.set_grad_enabled(True):
            # Forward: Get model outputs and calculate loss
            output = model(samples)

            loss = criterion(output, labels)

            # Backward: Optimize, step optimizer and calculate predictions
            ...

        # Step scheduler
        ...

        # Calculate any statistics
        ...

    # Validate 
    ...
```

# Cutout

Cutout [2] 在 2017 年由 DeVries & Taylor 在论文“Improved regularization of convolutional neural networks with cutout.”中提出。

## 简要描述

Cutout 图像增强的核心思想是在训练期间随机去除输入图像中的一个方形像素区域。

![](img/ccd607604f875ab08a0e3e444605ff77.png)

Cutout 图像增强

Cutout 可以通过迫使模型学习更强健的特征来防止过拟合。

优势：

+   实现简单（见 Cutout 的实现）

+   可以去除噪声，例如背景

缺点：

+   可以移除重要特征，特别是在稀疏图像中

## 使用 PyTorch 的 Python 实现

幸运的是，Cutout 在[Albumentations](https://albumentations.ai/docs/)图像增强库中可用。你可以使用`CoarseDropout`类（早期库版本中有`Cutout`类，但已被弃用）。

```py
transforms_cutout = A.Compose([
    A.Resize(256, 256), 
    A.CoarseDropout(max_holes = 1, # Maximum number of regions to zero out. (default: 8)
                    max_height = 128, # Maximum height of the hole. (default: 8) 
                    max_width = 128, # Maximum width of the hole. (default: 8) 
                    min_holes=None, # Maximum number of regions to zero out. (default: None, which equals max_holes)
                    min_height=None, # Maximum height of the hole. (default: None, which equals max_height)
                    min_width=None, # Maximum width of the hole. (default: None, which equals max_width)
                    fill_value=0, # value for dropped pixels.
                    mask_fill_value=None, # fill value for dropped pixels in mask. 
                    always_apply=False, 
                    p=0.5
                   ),
    ToTensorV2(),
])
```

返回的样本批次如下所示：

![](img/4d24c9c5d9a7dbbdbe30b9407018014a.png)

对样本批次应用了 Cutout 图像增强。

# Mixup

Mixup [4] 在 2017 年由 Zhang, Cisse, Dauphin & Lopez-Paz 在论文“mixup: Beyond empirical risk minimization”中提出。

## 简要描述

Mixup 图像增强的核心思想是在训练期间混合一对随机的输入图像及其标签。

![](img/05b9840f200b24f205e3c1a8f480ac0b.png)

Mixup 图像增强

Mixup 可以通过创建更多**多样化**的训练样本来防止过拟合，从而迫使模型学习对图像中的小变化不变的更具通用性的特征。

优势：

+   实现相对简单（见 Mixup 的实现）

+   增加训练数据的多样性

缺点：

+   可以创建模糊图像，特别是对于复杂纹理的图像

## 使用 PyTorch 的 Python 实现

你必须实现一个`mixup()`函数，以将 Mixup 图像增强应用到你的深度学习训练管道中。以下代码最初来自于[Riad 的这个 Kaggle Notebook](https://www.kaggle.com/code/riadalmadani/fastai-effb0-base-model-birdclef2023)，并为本文进行了修改。

`mixup()`函数将 Mixup 应用于整个批次。对会生成配对的操作通过打乱批次并从原始批次中选择一张图像和从打乱的批次中选择一张图像来实现。

```py
# Copied and edited from https://www.kaggle.com/code/riadalmadani/fastai-effb0-base-model-birdclef2023
def mixup(data, targets, alpha):
    indices = torch.randperm(data.size(0))
    shuffled_data = data[indices]
    shuffled_targets = targets[indices]

    lam = np.random.beta(alpha, alpha)
    new_data = data * lam + shuffled_data * (1 - lam)
    new_targets = [targets, shuffled_targets, lam]
    return new_data, new_targets
```

除了增强图像和标签的函数，我们还必须使用自定义 `mixup_criterion()` 函数修改损失函数。该函数根据 `lam` 返回两个标签的损失。

```py
# Copied and edited from https://www.kaggle.com/code/riadalmadani/fastai-effb0-base-model-birdclef2023
def mixup_criterion(preds, targets):
    targets1, targets2, lam = targets[0], targets[1], targets[2]
    criterion = nn.CrossEntropyLoss()
    return lam * criterion(preds, targets1) + (1 - lam) * criterion(preds, targets2)
```

`mixup()` 和 `mixup_criterion()` 函数不会应用于 PyTorch `Dataset`，而是在训练代码中应用，如下所示。

由于增强应用于整个批次，我们还将添加一个变量 `p_mixup` 来控制将被增强的批次比例。例如，`p_mixup = 0.5` 将对一个 epoch 中的 50% 批次应用 Mixup 增强。

```py
for epoch in range(NUM_EPOCHS):        
    # Train
    model.train()

    # Define any variables for metrics
    ...

    for samples, labels in (train_dataloader):

        samples, labels = samples.to(device), labels.to(device)

        # Normalize
        samples = samples/255

        ############################
        # Apply Mixup augmentation #
        ############################
        p = np.random.rand()
        if p < p_mixup:
            samples, labels = mixup(samples, labels, 0.8)

        # Zero the parameter gradients
        ...

        with torch.set_grad_enabled(True):
            # Forward: Get model outputs and calculate loss
            output = model(samples)

            ############################
            # Apply Mixup criterion    #
            ############################      
            if p < p_mixup:
                loss = mixup_criterion(output, labels)
            else:
                loss = criterion(output, labels) 

            # Backward: Optimize, step optimizer and calculate predictions
            ...

        # Step scheduler, Calculate any statistics, validate
        ...
```

返回的样本批次如下所示：

![](img/4436f8f8b5fb5384fb5d05486c1ee739.png)

应用于样本批次的 Mixup 图像增强。

# Cutmix

Cutmix [3] 在 2019 年的一篇名为“Cutmix: Regularization strategy to train strong classifiers with localizable features.”的论文中由 Yun、Han、Oh、Chun、Choe 和 Yoo 提出。

## 简要描述

Cutmix 图像增强的核心思想是在训练过程中随机选择一对输入图像，从第一张图像中剪切一个随机的像素块并将其粘贴到第二张图像上，然后按像素块的面积成比例混合它们的标签。

![](img/15012b33bde276cdcc559a2ba44032a8.png)

Cutmix 图像增强

Cutmix 可以通过迫使模型学习更强健和具有区分性的特征来防止过拟合。

Cutmix 结合了 Cutout 和 Mixup 的优缺点：

优点：

+   相对容易实现（参见 Cutmix 的实现）

+   增加训练数据的多样性

缺点：

+   由于不自然的组合，可能会生成不真实的图像

+   可能会移除重要特征，尤其是在稀疏图像中

## 使用 PyTorch 的 Python 实现

Cutmix 的实现类似于 Mixup 的实现。

首先，你还需要一个自定义函数 `cutmix()` 来应用图像增强。以下代码最初取自 [Riad 的 Kaggle Notebook](https://www.kaggle.com/code/riadalmadani/fastai-effb0-base-model-birdclef2023) 并为本文做了修改。

```py
# Copied and edited from https://www.kaggle.com/code/riadalmadani/fastai-effb0-base-model-birdclef2023
def cutmix(data, targets, alpha):
    indices = torch.randperm(data.size(0))
    shuffled_data = data[indices]
    shuffled_targets = targets[indices]

    lam = np.random.beta(alpha, alpha)
    bbx1, bby1, bbx2, bby2 = rand_bbox(data.size(), lam)
    data[:, :, bbx1:bbx2, bby1:bby2] = data[indices, :, bbx1:bbx2, bby1:bby2]
    # adjust lambda to exactly match pixel ratio
    lam = 1 - ((bbx2 - bbx1) * (bby2 - bby1) / (data.size()[-1] * data.size()[-2]))

    new_targets = [targets, shuffled_targets, lam]
    return data, new_targets

def rand_bbox(size, lam):
    W = size[2]
    H = size[3]
    cut_rat = np.sqrt(1\. - lam)
    cut_w = int(W * cut_rat)
    cut_h = int(H * cut_rat)

    # uniform
    cx = np.random.randint(W)
    cy = np.random.randint(H)

    bbx1 = np.clip(cx - cut_w // 2, 0, W)
    bby1 = np.clip(cy - cut_h // 2, 0, H)
    bbx2 = np.clip(cx + cut_w // 2, 0, W)
    bby2 = np.clip(cy + cut_h // 2, 0, H)

    return bbx1, bby1, bbx2, bby2
```

剩下的与 Mixup 相同：

1.  定义 `cutmix_criterion()` 函数来处理自定义损失（参见 `mixup_criterion()` 的实现）

1.  定义变量 `p_cutmix` 来控制将被增强的批次比例（参见 `p_mixup`）

1.  在训练代码中根据 `p_cutmix` 应用 `cutmix()` 和 `cutmix_criterion()`（参见 Mixup 的实现）

返回的样本批次如下所示：

![](img/9537f35e4ece1973dca502b3c16f9de2.png)

应用于样本批次的 Cutmix 图像增强。

# 总结

本文总结了三种现代有效的数据增强技术，用于计算机视觉：

+   Cutout [2]：随机去除输入图像中的一个方形区域的像素

+   Mixup [4]：混合一对随机的输入图像及其标签

+   Cutmix [3]: 随机选择一对输入图像，从第一张图像中剪切出一个随机像素块，将其粘贴到第二张图像上，然后按像素块的面积比例混合它们的标签。

![](img/ce29c82e0d40ffe0118468fcd443604c.png)

数据增强技术：Mixup, Cutout, Cutmix（图片由作者提供）

虽然 Cutout 将增强应用于单张图像，但 Mixup 和 Cutmix 则从一对输入图像中创建新图像。

所有讨论的图像增强技术都相对容易实现：对于 Cutout，[Albumentations](https://albumentations.ai/docs/)库已经提供了现成的实现。对于 Mixup 和 Cutmix，实施相对简单，需要实现一个增强函数和一个自定义损失函数。

# 享受这个故事吗？

[*免费订阅*](https://medium.com/subscribe/@iamleonie) *以便在我发布新故事时收到通知。*

[](https://medium.com/@iamleonie/subscribe?source=post_page-----a9d7db3074ad--------------------------------) [## 每当 Leonie Monigatti 发布新内容时，通过电子邮件收到通知。

### 每当 Leonie Monigatti 发布新内容时，您将通过电子邮件收到通知。通过注册，您将创建一个 Medium 帐户（如果您尚未拥有的话）……

medium.com](https://medium.com/@iamleonie/subscribe?source=post_page-----a9d7db3074ad--------------------------------)

*在* [*LinkedIn*](https://www.linkedin.com/in/804250ab/)、[*Twitter*](https://twitter.com/helloiamleonie)*和* [*Kaggle*](https://www.kaggle.com/iamleonie)*上找到我！*

# 参考文献

## 数据集

[1] 使用的图像数据集由作者从[Unsplash.com](https://unsplash.com/de)收集的八张图片创建。该数据集在[Kaggle](https://www.kaggle.com/datasets/iamleonie/roses-or-tulips)上公开提供，数据集描述中注明了图像来源。由于 Unsplash 上的图片可以用于商业用途，并且这八张照片的收集并不重复类似或竞争的 Unsplash 服务，因此该数据集在 CC BY-SA 4.0 许可证下使用。

[](https://www.kaggle.com/datasets/iamleonie/roses-or-tulips?source=post_page-----a9d7db3074ad--------------------------------) [## 玫瑰还是郁金香

### 玩具数据集仅包含 8 张图片（4 张郁金香和 4 张玫瑰），均来自 Unsplash。

[www.kaggle.com](https://www.kaggle.com/datasets/iamleonie/roses-or-tulips?source=post_page-----a9d7db3074ad--------------------------------)

## 图像参考

除非另有说明，所有图像均由作者创建。

## 文献

[2] DeVries, T., & Taylor, G. W. (2017). Improved regularization of convolutional neural networks with cutout. *arXiv 预印本 arXiv:1708.04552*。

[3] Yun, S., Han, D., Oh, S. J., Chun, S., Choe, J., & Yoo, Y. (2019). Cutmix: Regularization strategy to train strong classifiers with localizable features. In *IEEE/CVF 国际计算机视觉会议论文集* (第 6023–6032 页)。

[4] 张华、Cisse M.、Dauphin Y. N. 和 Lopez-Paz D. (2017) mixup: 超越经验风险最小化。arXiv 预印本 arXiv:1710.09412。
