# 在您的数据上训练 YOLOv8 实例分割

> 原文：[https://towardsdatascience.com/trian-yolov8-instance-segmentation-on-your-data-6ffa04b2debd?source=collection_archive---------0-----------------------#2023-02-15](https://towardsdatascience.com/trian-yolov8-instance-segmentation-on-your-data-6ffa04b2debd?source=collection_archive---------0-----------------------#2023-02-15)

## 如何基于最新的 YOLOv8 模型在您的数据上训练一个实例分割模型

[](https://alon-lek.medium.com/?source=post_page-----6ffa04b2debd--------------------------------)[![Alon Lekhtman](../Images/1451bacf9a127f7fe596cf32249035be.png)](https://alon-lek.medium.com/?source=post_page-----6ffa04b2debd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6ffa04b2debd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6ffa04b2debd--------------------------------) [Alon Lekhtman](https://alon-lek.medium.com/?source=post_page-----6ffa04b2debd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F931822b64e54&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftrian-yolov8-instance-segmentation-on-your-data-6ffa04b2debd&user=Alon+Lekhtman&userId=931822b64e54&source=post_page-931822b64e54----6ffa04b2debd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6ffa04b2debd--------------------------------) ·10 分钟阅读·2023年2月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6ffa04b2debd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftrian-yolov8-instance-segmentation-on-your-data-6ffa04b2debd&user=Alon+Lekhtman&userId=931822b64e54&source=-----6ffa04b2debd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6ffa04b2debd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftrian-yolov8-instance-segmentation-on-your-data-6ffa04b2debd&source=-----6ffa04b2debd---------------------bookmark_footer-----------)

YOLOv8 于 2023 年 1 月 10 日发布。到目前为止，这是计算机视觉领域用于分类、检测和分割任务的最先进模型。该模型在准确性和执行时间方面均优于所有已知模型。

![](../Images/846d796782d2742599ba66f04d7a424c.png)

YOLOv8 与其他 YOLO 模型的比较（来自 [ultralytics](https://github.com/ultralytics/ultralytics)）

ultralytics 团队在使这个模型比所有之前的 YOLO 模型更易于使用方面做得非常出色——你甚至不再需要克隆 git 仓库了！

# 创建图像数据集

在这篇文章中，我创建了一个非常简单的示例，展示了训练 YOLOv8 所需的所有步骤，特别是用于分割任务。数据集很小，对模型来说“易于学习”，故意如此，以便我们能够在简单的 CPU 上训练几秒钟后获得令人满意的结果。

我们将创建一个白色圆圈与黑色背景的数据集。圆圈将有不同的尺寸。我们将训练一个模型，能够在图像中分割出这些圆圈。

这就是数据集的样子：

![](../Images/7a79eee703fe9676934e9eef85f99867.png)

数据集是使用以下代码生成的：

```py
import numpy as np
from PIL import Image
from skimage import draw
import random
from pathlib import Path

def create_image(path, img_size, min_radius):
    path.parent.mkdir( parents=True, exist_ok=True )

    arr = np.zeros((img_size, img_size)).astype(np.uint8)
    center_x = random.randint(min_radius, (img_size-min_radius))
    center_y = random.randint(min_radius, (img_size-min_radius))
    max_radius = min(center_x, center_y, img_size - center_x, img_size - center_y)
    radius = random.randint(min_radius, max_radius)

    row_indxs, column_idxs = draw.ellipse(center_x, center_y, radius, radius, shape=arr.shape)

    arr[row_indxs, column_idxs] = 255

    im = Image.fromarray(arr)
    im.save(path)

def create_images(data_root_path, train_num, val_num, test_num, img_size=640, min_radius=10):
    data_root_path = Path(data_root_path)

    for i in range(train_num):
        create_image(data_root_path / 'train' / 'images' / f'img_{i}.png', img_size, min_radius)

    for i in range(val_num):
        create_image(data_root_path / 'val' / 'images' / f'img_{i}.png', img_size, min_radius)

    for i in range(test_num):
        create_image(data_root_path / 'test' / 'images' / f'img_{i}.png', img_size, min_radius)

create_images('datasets', train_num=120, val_num=40, test_num=40, img_size=120, min_radius=10)
```

# 创建标签

现在我们有了图像数据集，我们需要为图像创建标签。通常，我们需要进行一些手动操作，但由于我们创建的数据集非常简单，因此很容易编写代码来生成标签：

```py
from rasterio import features

def create_label(image_path, label_path):
    arr = np.asarray(Image.open(image_path))

    # There may be a better way to do it, but this is what I have found so far
    cords = list(features.shapes(arr, mask=(arr >0)))[0][0]['coordinates'][0]
    label_line = '0 ' + ' '.join([f'{int(cord[0])/arr.shape[0]} {int(cord[1])/arr.shape[1]}' for cord in cords])

    label_path.parent.mkdir( parents=True, exist_ok=True )
    with label_path.open('w') as f:
        f.write(label_line)

for images_dir_path in [Path(f'datasets/{x}/images') for x in ['train', 'val', 'test']]:
    for img_path in images_dir_path.iterdir():
        label_path = img_path.parent.parent / 'labels' / f'{img_path.stem}.txt'
        label_line = create_label(img_path, label_path)
```

这里是标签文件内容的示例：

```py
0 0.0767 0.08433 0.1417 0.08433 0.1417 0.0917 0.15843 0.0917 0.15843 0.1 0.1766 0.1 0.1766 0.10844 0.175 0.10844 0.175 0.1177 0.18432 0.1177 0.18432 0.14333 0.1918 0.14333 0.1918 0.20844 0.18432 0.20844 0.18432 0.225 0.175 0.225 0.175 0.24334 0.1766 0.24334 0.1766 0.2417 0.15843 0.2417 0.15843 0.25 0.1417 0.25 0.1417 0.25846 0.0767 0.25846 0.0767 0.25 0.05 0.25 0.05 0.2417 0.04174 0.2417 0.04174 0.24334 0.04333 0.24334 0.04333 0.225 0.025 0.225 0.025 0.20844 0.01766 0.20844 0.01766 0.14333 0.025 0.14333 0.025 0.1177 0.04333 0.1177 0.04333 0.10844 0.04174 0.10844 0.04174 0.1 0.05 0.1 0.05 0.0917 0.0767 0.0917 0.0767 0.08433
```

标签对应于这张图像：

![](../Images/41ddab79fd8e2339d49bf12eec44bba4.png)

对应于标签示例的图像

标签内容仅为一行文本。每张图像中只有一个物体（圆圈），每个物体由文件中的一行表示。如果每张图像中有多个物体，你应该为每个标记物体创建一行。

第一个 0 表示标签的类别类型。因为我们只有一个类别（圆圈），所以我们总是使用 0。如果你的数据中有多个类别，你应该将每个类别映射到一个数字（0, 1, 2…），并在标签文件中使用这个数字。

所有其他数字表示标记物体的边界多边形的坐标。格式是 <**x1 y1 x2 y2 x3 y3…>**，这些坐标相对于图像的大小——你应该将坐标标准化为 1x1 的图像大小。例如，如果有一个点 (15, 75) 而图像大小是 120x120，则标准化后的点是 (15/120, 75/120) = (0.125, 0.625)。

在处理图像库时，处理坐标的正确方向总是令人困惑。因此，为了使其清晰，对于 YOLO，X 坐标从左到右，而 Y 坐标从上到下。

# YAML 配置

我们已经有了图像和标签。现在我们需要创建一个包含数据集配置的 YAML 文件：

```py
yaml_content = f'''
train: train/images
val: val/images
test: test/images

names: ['circle']
    '''

with Path('data.yaml').open('w') as f:
    f.write(yaml_content)
```

请注意，如果你有更多的物体类别类型，你需要在名称数组中添加它们，顺序与标签文件中的顺序一致。第一个是 0，第二个是 1，依此类推…

# 数据集文件结构

让我们看看我们创建的文件结构，使用 Linux [**tree**](https://pimylifeup.com/tree-command-linux/) 命令：

```py
tree .
```

```py
data.yaml
datasets/
├── test
│   ├── images
│   │   ├── img_0.png
│   │   ├── img_1.png
│   │   ├── img_2.png
│   │   ├── ...
│   └── labels
│       ├── img_0.txt
│       ├── img_1.txt
│       ├── img_2.txt
│       ├── ...
├── train
│   ├── images
│   │   ├── img_0.png
│   │   ├── img_1.png
│   │   ├── img_2.png
│   │   ├── ...
│   └── labels
│       ├── img_0.txt
│       ├── img_1.txt
│       ├── img_2.txt
│       ├── ...
|── val
|   ├── images
│   │   ├── img_0.png
│   │   ├── img_1.png
│   │   ├── img_2.png
│   │   ├── ...
|   └── labels
│       ├── img_0.txt
│       ├── img_1.txt
│       ├── img_2.txt
│       ├── ...
```

# 训练模型

现在我们已经有了图像和标签，我们可以开始训练模型。因此首先安装包：

```py
pip install ultralytics==8.0.38
```

ultralytics 库更新非常快，有时会导致 API 中断，因此我更倾向于使用一个版本。以下代码依赖于版本 8.0.38（这是我写这些内容时的最新版本）。如果你升级到较新的版本，可能需要对代码进行一些调整以使其正常工作。

并开始训练：

```py
from ultralytics import YOLO

model = YOLO("yolov8n-seg.pt")

results = model.train(
        batch=8,
        device="cpu",
        data="data.yaml",
        epochs=7,
        imgsz=120,
    )
```

为了简化这篇文章，我使用了 nano 模型（yolov8n-seg），仅在 CPU 上训练，仅用了 7 个周期。训练在我的笔记本电脑上只花了几秒钟。

关于可用于训练模型的参数的更多信息，你可以查看[这个链接](https://github.com/ultralytics/ultralytics/blob/main/ultralytics/yolo/engine/trainer.py#L42)。

## 理解结果

训练完成后，你会在输出的末尾看到一行，类似于这样：

```py
Results saved to runs/segment/train60
```

让我们来看一下这里的一些结果：

## 验证标签

```py
from IPython.display import Image as show_image
show_image(filename="runs/segment/train60/val_batch0_labels.jpg")
```

![](../Images/567dec0b45f36610ab6970aaa544e672.png)

部分验证集标签

在这里我们可以看到部分验证集的真实标签。这应该几乎完全对齐。如果你看到这些标签没有很好地覆盖对象，很可能是你的标签不正确。

**预测验证标签**

```py
show_image(filename="runs/segment/train60/val_batch0_pred.jpg")
```

![](../Images/2192471bc68c59dcf54fcdecafb638d0.png)

验证集预测

在这里我们可以看到训练模型在部分验证集上的预测（与上面看到的部分相同）。这可以让你感受模型的表现如何。请注意，为了创建此图像，应选择一个置信度阈值，这里使用的阈值是**0.5**，这并不总是最佳的（我们稍后会讨论）。

## 精度曲线

要理解这些以及接下来的图表，你需要对精度和召回率的概念有所了解。[这里](https://medium.com/@shrutisaxena0617/precision-vs-recall-386cf9f89488)对它们的工作原理做了很好的解释。

```py
show_image(filename="runs/segment/train60/MaskP_curve.png")
```

![](../Images/6d5e98b3d6235facea1866bfba43ad9b.png)

精度/置信度阈值曲线

模型检测到的每个对象都有一定的置信度，通常，如果你希望在声明“这是一个圆形”时尽可能确定，你会只使用高置信度值（高置信度阈值）。当然，这会有一个权衡——你可能会错过一些“圆形”。另一方面，如果你想“捕捉”尽可能多的“圆形”，即使有些并不真正是“圆形”，你会使用低置信度值和高置信度值（低置信度阈值）。

上面的图表（以及下面的图表）帮助你决定使用哪个置信度阈值。在我们的例子中，我们可以看到对于高于**0.128**的阈值，我们获得了**100%**的精度，这意味着所有对象都被正确预测。

请注意，由于我们实际上是在进行分割任务，还有一个重要的阈值需要关注——IoU（交并比），如果你不熟悉它，可以在[这里](https://pyimagesearch.com/2016/11/07/intersection-over-union-iou-for-object-detection/)阅读相关信息。对于这张图表，使用了**0.5**的IoU。

**召回率曲线**

```py
show_image(filename="runs/segment/train60/MaskR_curve.png")
```

![](../Images/e9898f596c3f8de76b7f97ec15de9546.png)

召回率/置信度阈值曲线

这里你可以看到召回率图表，随着置信度阈值的提高，召回率下降。这意味着你“捕捉到”的“圆圈”变少了。

这里你可以看到为什么在这种情况下使用**0.5**的置信度阈值是一个糟糕的主意。对于**0.5**的阈值，你得到大约**90%**的召回率。然而，在精度曲线中，我们看到对于高于**0.128**的阈值，我们可以获得**100%**的精度，因此我们不需要达到**0.5**，可以安全地使用**0.128**的阈值，获得**100%**的精度和接近**100%**的召回率 :)

## 精度-召回曲线

[这里](https://medium.com/@douglaspsteen/precision-recall-curves-d32e5b290248)是对精度-召回曲线的很好解释

```py
show_image(filename="runs/segment/train60/MaskPR_curve.png")
```

![](../Images/ae03968630d242ece8e4631990d2805a.png)

精度-召回曲线

我们可以清楚地看到我们之前得出的结论，对于这个模型，我们可以达到接近**100%**的精度和**100%**的召回率。

这个图表的缺点是我们无法看到应该使用哪个阈值，这就是为什么我们仍然需要上述图表的原因。

**随时间变化的损失**

```py
show_image(filename="runs/segment/train60/results.png")
```

![](../Images/94602d3dd0c79a4c321ba0f545bbd9d7.png)

随时间变化的损失

这里你可以看到不同损失在训练过程中的变化，以及每个时期之后它们在验证集上的表现。

关于损失和从这些图表中得出的结论有很多可以说的，但这超出了本文的范围。我只是想说明你可以在这里找到相关信息 :)

## 使用训练好的模型

结果目录中还可以找到模型本身。这是如何在新图像上使用模型：

```py
my_model = YOLO('runs/segment/train60/weights/best.pt')
results = list(my_model('datasets/test/images/img_5.png', conf=0.128))
result = results[0]
```

结果列表可能包含多个值，每个值对应一个检测到的对象。由于在这个例子中每张图片中只有一个对象，我们取第一个列表项。

你可以看到我传递了我们之前找到的最佳置信度阈值（**0.128**）。

有两种方法可以获取检测到的对象在图像中的实际位置。选择正确的方法取决于你打算如何使用结果。我将展示这两种方法。

```py
result.masks.segments
```

```py
[array([[    0.10156,     0.34375],
        [    0.09375,     0.35156],
        [    0.09375,     0.35937],
        [   0.078125,       0.375],
        [   0.070312,       0.375],
        [     0.0625,     0.38281],
        [    0.38281,     0.71094],
        [    0.39062,     0.71094],
        [    0.39844,     0.70312],
        [    0.39844,     0.69531],
        [    0.41406,     0.67969],
        [    0.42187,     0.67969],
        [    0.44531,     0.46875],
        [    0.42969,     0.45312],
        [    0.42969,     0.41406],
        [    0.42187,     0.40625],
        [    0.41406,     0.40625],
        [    0.39844,     0.39062],
        [    0.39844,     0.38281],
        [    0.39062,       0.375],
        [    0.38281,       0.375],
        [    0.35156,     0.34375]], dtype=float32)]
```

这会返回对象的边界多边形，类似于我们传递标记数据的格式。

第二种方法是：

```py
result.masks.masks
```

```py
tensor([[[0., 0., 0.,  ..., 0., 0., 0.],
         [0., 0., 0.,  ..., 0., 0., 0.],
         [0., 0., 0.,  ..., 0., 0., 0.],
         ...,
         [0., 0., 0.,  ..., 0., 0., 0.],
         [0., 0., 0.,  ..., 0., 0., 0.],
         [0., 0., 0.,  ..., 0., 0., 0.]]])
```

这会返回一个形状为（1, 128, 128）的张量，表示图像中的所有像素。属于对象的像素接收1，背景像素接收0。

让我们看看掩模的样子：

```py
import torchvision.transforms as T
T.ToPILImage()(result.masks.masks).show()
```

![](../Images/188f74a8e99b400d049abcf6fa0ffb6a.png)

预测图像的分割

这就是原始图像：

![](../Images/787981982a3eb511bfc09f1f297c9c6e.png)

原始图像

虽然不完美，但对于许多应用来说已经足够好，并且 IoU 确实高于**0.5**。

总之，与之前的 Yolo 版本相比，新版的 ultralytics 库更容易使用，特别是在分割任务上，现在它已经成为一个一流的功能。你可以在 ultralytics 新包中找到 Yolov5，因此如果你不想使用仍然有些新的和实验性的 Yolo 版本，你可以选择使用广为人知的 yolov5：

![](../Images/2d6bb0b746deda291cf6d79aa89155af.png)

[yolov8 和 yolov5 的 Google 趋势比较](https://trends.google.com/trends/explore?date=today+5-y&q=yolov5%2Cyolov8)

有些话题我没有涵盖，比如用于模型的不同损失函数、创建 yolov8 时进行的架构更改等等。如果你想了解更多这些话题，欢迎在这篇文章下评论。如果有兴趣，我可能会写另一篇关于这些内容的文章。

感谢你抽时间阅读这篇文章，希望它能帮助你理解 Yolov8 模型的训练过程。

*除非另有说明，所有图像均由作者创建。*
