# 查看你使用 SAM 的分割效果

> 原文：[https://towardsdatascience.com/see-what-you-sam-4eea9ad9a5de?source=collection_archive---------1-----------------------#2023-05-03](https://towardsdatascience.com/see-what-you-sam-4eea9ad9a5de?source=collection_archive---------1-----------------------#2023-05-03)

## 如何生成和可视化 Segment Anything Model 的预测结果

[](https://medium.com/@jacob_marks?source=post_page-----4eea9ad9a5de--------------------------------)[![Jacob Marks, Ph.D.](../Images/94d9832b8706d1044e3195386613bfab.png)](https://medium.com/@jacob_marks?source=post_page-----4eea9ad9a5de--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4eea9ad9a5de--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4eea9ad9a5de--------------------------------) [Jacob Marks, Ph.D.](https://medium.com/@jacob_marks?source=post_page-----4eea9ad9a5de--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff7dc0c0eae92&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsee-what-you-sam-4eea9ad9a5de&user=Jacob+Marks%2C+Ph.D.&userId=f7dc0c0eae92&source=post_page-f7dc0c0eae92----4eea9ad9a5de---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4eea9ad9a5de--------------------------------) ·10 分钟阅读·2023年5月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4eea9ad9a5de&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsee-what-you-sam-4eea9ad9a5de&user=Jacob+Marks%2C+Ph.D.&userId=f7dc0c0eae92&source=-----4eea9ad9a5de---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4eea9ad9a5de&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsee-what-you-sam-4eea9ad9a5de&source=-----4eea9ad9a5de---------------------bookmark_footer-----------)![](../Images/69d09f042372384691aca27cf4873995.png)

*使用 Meta AI 的 Segment Anything Model (*[*许可证*](https://github.com/openimages/dataset/blob/main/LICENSE)*) 对 Open Images V7 的样本进行分割 (*[*许可证*](https://github.com/facebookresearch/segment-anything/blob/main/LICENSE)*)。图片由作者提供。*

在过去的几周里，Meta AI Research 的通用图像分割模型吸引了大量关注。这个模型，名为 [Segment Anything Model (SAM)](https://github.com/facebookresearch/segment-anything) ([Apache 许可证 2.0](https://github.com/facebookresearch/segment-anything/blob/main/LICENSE))，是在一个包含 1100 万张图像和超过十亿个分割掩码的数据集上进行训练的。

SAM功能强大。但像往常一样，在将模型投入生产之前，你需要了解模型在你的数据集上的表现。在计算机视觉的背景下，关键因素之一是可视化模型预测。

本博文旨在帮助你快速上手SAM：我们将引导你如何使用SAM将分割掩码添加到你的数据集中，以及如何系统地可视化整个数据集中的这些分割掩码。通过可视化（和评估）这些预测，我们可以更好地了解SAM在我们数据集上的表现、其局限性以及将模型集成到我们管道中的潜在影响。

SAM提供了多种生成分割掩码的途径：

1.  自动：它*自动工作*，无需任何提示或提示

1.  从边界框：给定一个边界框，SAM对边界内的对象进行分割

1.  从点：给定点标签，可能是正数或负数，SAM推断需要分割的区域。

1.  从点和框：你可以提供点*和*边界框来提高性能

接下来，我们将明确介绍前三个。本文将按以下结构组织：

+   [设置](#dc9a)

+   [使用SAM进行自动分割](#58c1)

+   [使用SAM进行语义分割](#23ee)

+   [使用SAM进行实例分割](#37d5)

# 设置

## 安装

本教程要求`python≥3.8`，`pytorch≥1.7`和`torchvision≥0.8`。如果你没有安装Torch或Torchvision，请运行：

```py
pip install torch torchvision
```

此外，我们将使用开源计算机视觉库[FiftyOne](https://github.com/voxel51/fiftyone)来加载数据集和可视化预测。如果你没有安装FiftyOne，可以运行：

```py
pip install fiftyone
```

为了使用SAM，你可以从源代码安装[Segment Anything库](https://github.com/facebookresearch/segment-anything)，使用：

```py
pip install git+https://github.com/facebookresearch/segment-anything.git
```

然后，你将能够将库导入为`segment_anything`。

然后，下载一个[模型检查点](https://github.com/facebookresearch/segment-anything#model-checkpoints)。在这次演练中，我们将使用默认的[ViT-H SAM模型](https://huggingface.co/facebook/sam-vit-huge)，即“huge”视觉变换器分割模型。如果你愿意，你也可以使用大型（ViT-L SAM）或基础（ViT-B SAM）模型。

## 导入模块

这是我们需要导入所有模块的头部代码：

```py
import numpy as np
import PIL
import torch

import fiftyone as fo
import fiftyone.zoo as foz # for loading/downloading datasets

from segment_anything import sam_model_registry, SamAutomaticMaskGenerator, SamPredictor
```

## 定义常量

我们还可以定义一些在所有分割应用中不会改变的元素：

```py
sam_checkpoint = "path/to/ckpt.pth"
model_type = "vit_h"
device = "cuda"

sam = sam_model_registry[model_type](checkpoint=sam_checkpoint)
sam.to(device)
```

## 加载数据集

对于本教程，我们将使用来自Google的[Open Images V7](https://storage.googleapis.com/openimages/web/index.html)（[Apache许可证2.0](https://github.com/openimages/dataset/blob/main/LICENSE)）数据集的图像。数据集已经为许多图像准备了实例分割掩码，但为了说明，我们只会加载点标签和目标检测边界框。关于如何处理Open Images V7中的点标签的全面教程，请查看[此Medium文章](https://medium.com/voxel51/exploring-googles-open-images-v7-a6218d0098cb)。

让我们从验证集中加载100个随机图像：

```py
dataset = foz.load_zoo_dataset(
    "open-images-v7", 
    split="validation", 
    max_samples=100,
    label_types=["detections", "points"],
    shuffle=True,
)
```

我们将命名数据集并使其持久化。此外，我们还将通过运行`compute_metadata()`将图像的宽度和高度存储为像素，以便我们可以使用这些信息在绝对坐标和相对坐标之间进行转换：

```py
dataset.name = "openimages_sam"
dataset.persistent = True
dataset.compute_metadata()

## visualize the dataset
session = fo.launch_app(dataset)
```

在我们开始添加SAM预测之前，数据集的外观如下所示：

![](../Images/d906fa04c0f8dac283c99ab15cc42fdc.png)

*Open Images V7中的图像在FiftyOne App中可视化。图像由作者提供。*

# 使用SAM进行自动分割

如果您没有任何现有的关键点或边界框来指导Segment Anything模型，您可以使用“自动分割”功能为图像中的任何[对象和物品](https://arxiv.org/abs/1612.03716)生成分割掩码。这是通过`SamAutomaticMaskGenerator`类完成的。请注意，这不是全景分割，因为掩码没有标注。

您可以实例化一个`SamAutomaticMaskGenerator`对象，并设置交并比（IoU）阈值、返回的掩码的最小面积和其他参数：

```py
mask_generator = SamAutomaticMaskGenerator(
    model=sam,
    points_per_side=32,
    pred_iou_thresh=0.9,
    stability_score_thresh=0.92,
    crop_n_layers=1,
    crop_n_points_downscale_factor=2,
    min_mask_region_area=400
)
```

有关可接受参数的完整列表，请参阅[此SAM笔记本](https://github.com/facebookresearch/segment-anything/blob/main/notebooks/automatic_mask_generator_example.ipynb)。

给定一个样本（位于`sample.filepath`的图像），我们可以使用Pillow读取图像，并调用我们的`SamAutomaticMaskGenerator`对象的`generate()`方法生成掩码：

```py
image = np.array(PIL.Image.open(sample.filepath))
masks = mask_generator.generate(image)
```

这些掩码包含2D的“分割”数组，但没有标签。如果我们还想要标签，我们可以使用类似[语义分割工具](https://github.com/fudan-zvg/Semantic-Segment-Anything)的库。为简单起见，我们将向您展示如何将所有这些组合成一个完整的图像掩码，并为我们的掩码生成器返回的每个单独的掩码分配一个不同的颜色。

要为单个样本添加“自动”分割掩码，我们可以将与该样本关联的图像传递给我们的掩码生成器。然后对于返回的每个掩码，我们可以将该掩码添加到完整的图像掩码中，乘以一个唯一的数字，使得显示颜色对应于该子掩码。然后，我们可以将这个完整的图像掩码作为`Segmentation`标签对象存储在我们的数据集中。

这包含在以下函数中：

```py
def add_SAM_auto_segmentation(sample):
    image  = np.array(PIL.Image.open(sample.filepath))
    masks = mask_generator.generate(image)

    full_mask = np.zeros_like(masks[0]["segmentation"]).astype(int)
    for i in range(len(masks)):
        x, y = np.where(masks[i]['segmentation'])
        full_mask[x,y] = i + 1

    sample["auto_SAM"] = fo.Segmentation(mask=full_mask.astype(np.uint8))
```

只要在定义掩码生成器时设置`crop_n_layers=1`，添加步骤就是有效的。此代码可以处理最多256个唯一子掩码。

我们将遍历数据集中的样本，并在此过程中保存每个样本：

```py
def add_SAM_auto_segmentations(dataset):
    for sample in dataset.iter_samples(autosave=True, progress=True):
        add_SAM_auto_segmentation(sample)
```

当我们在FiftyOne App中可视化结果时，我们看到的是：

![](../Images/69d09f042372384691aca27cf4873995.png)

*Meta AI的Segment Anything Model对Open Images V7样本的自动分割。图片由作者提供。*

观察这些自动生成的掩码，我们可以看到有一些很小的斑点对我们并没有特别的意义。当我们定义掩码生成器时，我们将最小掩码区域面积设置为400像素。如果我们将此方法用作更大管道的一部分，我们可能需要考虑增加这一最小要求，或者根据图像中的像素数量为某些图像使用不同的最小值。

# 使用SAM进行语义分割

如果你的数据集中图像上有点标签（关键点），你可以使用这些点标签来提示SAM模型。这对于正负点标签都是适用的！本节将向你展示如何做到这一点。

在FiftyOne中，点标签表示为`Keypoint`对象。在Open Images V7中，图像上显示的每个单独点都存储在“points”字段中的自己的`Keypoint`对象中，因为它携带了额外的信息。

我们可以通过`keypoints`属性访问给定样本的点标签内容。例如，要获取数据集中第一个样本的第一个点标签，我们可以使用：

```py
dataset.first().points.keypoints[0]
```

```py
<Keypoint: {
    'id': '644c260d753fe20b7f60f9de',
    'attributes': {},
    'tags': [],
    'label': 'Rope',
    'points': [[0.11230469, 0.7114094]],
    'confidence': None,
    'index': None,
    'estimated_yes_no': 'no',
    'source': 'ih',
    'yes_votes': 0,
    'no_votes': 3,
    'unsure_votes': 0,
}>
```

这个点是类别`Rope`的负标签（`estimated_yes_no`字段），其结果由单个`yes`和`no`投票数决定。在Open Images V7数据集中，点标签有`estimated_yes_no`值`("yes", "no", "unsure")`。我们将忽略`unsure`点（这仅占总点标签的一小部分），并关注高确定性的点。

让我们实例化一个SAM预测器模型，用于语义和实例分割：

```py
predictor = SamPredictor(sam)
```

为了初始化预测器，我们将通过`point_coords`和`point_labels`参数传递图像中的点标签信息。

`SamPredictor`期望`point_coords`使用绝对坐标，而FiftyOne存储相对坐标。此外，`point_labels`接受`0`和`1`的数组，所以我们将从`[yes, no]`转换过来。以下函数接受给定图像的点标签列表、标签类别、图像宽度和高度，并返回所有相关点的`point_coords`和`point_labels`：

```py
def generate_sam_points(keypoints, label, w, h):
    def scale_keypoint(p):
        return [p[0] * w, p[1] * h]

    sam_points, sam_labels = [], []
    for kp in keypoints:
        if kp.label == label and kp.estimated_yes_no != "unsure":
            sam_points.append(scale_keypoint(kp.points[0]))
            sam_labels.append(bool(kp.estimated_yes_no == "yes"))

    return np.array(sam_points), np.array(sam_labels)
```

对于单个样本，我们可以使用以下函数添加SAM语义分割掩码：

```py
def add_SAM_semantic_segmentation(sample, n2i):
    image = np.array(PIL.Image.open(sample.filepath))
    predictor.set_image(image)

    if sample.points is None:
        return

    points = sample.points.keypoints
    labels = list(set([point.label for point in points]))

    w, h = sample.metadata.width, sample.metadata.height
    semantic_mask = np.zeros((h, w))
    for label in labels:
        sam_points, sam_labels = generate_sam_points(points, label, w, h)
        if not np.any(sam_labels):
            continue

        masks, scores, _ = predictor.predict(
            point_coords=sam_points,
            point_labels=sam_labels,
            multimask_output=True,
        )
        mask = masks[np.argmax(scores)].astype(int) ## get best guess

        semantic_mask *= (1 - mask)
        semantic_mask += mask * n2i[label]

    sample["semantic_SAM"] = fo.Segmentation(
        mask=semantic_mask.astype(np.uint8)
    )
```

这里，`n2i` 是一个字典，将类名映射到整数值，用于填充分割掩码。值得注意的是，当`multimask_output=True`时，预测器会为每个输入返回多个分割掩码的猜测。我们选择置信度最高的预测（最大 `score`）。

遍历数据集中的样本：

```py
def add_SAM_semantic_segmentations(dataset):
    point_classes = dataset.distinct("points.keypoints.label")
    dataset.default_mask_targets = {i+1:n for i, n in enumerate(point_classes)}
    dataset.default_mask_targets[0] = "other"  # reserve 0 for background
    NAME_TO_INT = {n:i+1 for i, n in enumerate(point_classes)}
    dataset.save()

    for sample in dataset.iter_samples(autosave=True, progress=True):
        add_SAM_semantic_segmentation(sample, NAME_TO_INT)
```

我们可以为数据集生成分割掩码：

![](../Images/3f2d7f7a665af436ad38e6fdc04ed93a.png)

*Meta AI 的 Segment Anything 模型对 Open Images V7 样本的语义分割。图片由作者提供。*

当然，并不是*所有*的内容都进行了语义分割，因为图像中包含一些稀疏的点标签。向初始数据中添加更多点将导致数据集中图像的语义分割掩码更加密集。

我们还可以看到，虽然SAM在整个数据集上的表现相当不错，但在适当地分割摩托车轮子方面表现得比较困难。

# 使用SAM进行实例分割

如果你已经有了数据集中对象的边界框，你可以用这些边界框来提示SAM模型，并生成这些对象的分割掩码！方法如下：

与点标签一样，我们需要将边界框从相对坐标转换为绝对坐标。在 FiftyOne 中，边界框的存储格式为 `[<top-left-x>, <top-left-y>, <width>, <height>]`，坐标在 `[0,1]` 范围内。而 SAM 边界框的格式为 `[<top-left-x>, <top-left-y>, <top-right-x>, <top-right-y>]`，使用绝对坐标。以下函数将为我们执行转换：

```py
def fo_to_sam(box, img_width, img_height):
    new_box = np.copy(np.array(box))
    new_box[0] *= img_width
    new_box[2] *= img_width
    new_box[1] *= img_height
    new_box[3] *= img_height
    new_box[2] += new_box[0]
    new_box[3] += new_box[1]
    return np.round(new_box).astype(int)
```

一旦我们为给定的对象检测生成了实例分割掩码，我们可以使用以下方式将掩码添加到检测对象中：

```py
def add_SAM_mask_to_detection(detection, mask, img_width, img_height):
    y0, x0, y1, x1 = fo_to_sam(detection.bounding_box, img_width, img_height)    
    mask_trimmed = mask[x0:x1+1, y0:y1+1]
    detection["mask"] = np.array(mask_trimmed)
    return detection
```

要将实例分割掩码添加到图像中，我们需要遍历所有对象检测，使用 `SamPredictor` 对象与每个检测的边界框，并将生成的掩码添加到 FiftyOne `Detection` 对象中：

```py
def add_SAM_instance_segmentation(sample):
    w, h = sample.metadata.width, sample.metadata.height
    image = np.array(PIL.Image.open(sample.filepath))
    predictor.set_image(image)

    if sample.detections is None:
        return

    dets = sample.detections.detections
    boxes = [d.bounding_box for d in dets]
    sam_boxes = np.array([fo_to_sam(box, w, h) for box in boxes])

    input_boxes = torch.tensor(sam_boxes, device=predictor.device)
    transformed_boxes = predictor.transform.apply_boxes_torch(input_boxes, image.shape[:2])

    masks, _, _ = predictor.predict_torch(
            point_coords=None,
            point_labels=None,
            boxes=transformed_boxes,
            multimask_output=False,
        )

    new_dets = []
    for i, det in enumerate(dets):
        mask = masks[i, 0]
        new_dets.append(add_SAM_mask_to_detection(det, mask, w, h))

    sample.detections = fo.Detections(detections = new_dets)
```

对于实例分割，将其扩展到整个数据集是很简单的：

```py
def add_SAM_instance_segmentations(dataset):
    for sample in dataset.iter_samples(autosave=True, progress=True):
        add_SAM_instance_segmentation(sample)
```

按标签着色，我们得到的效果如下：

![](../Images/ec63cc352ddba57be6d72b4cdba891c2.png)

*Meta AI 的 Segment Anything 模型对 Open Images V7 样本的实例分割。图片由作者提供。*

*注意：为了提高效率，你还可以批量处理这些预测！*

# 全景分割

如果你希望使用SAM对数据集进行全景分割，你可以通过以下方式结合关键点和边界框的方法：

**对于每一个边界对象，或者*事物*，如汽车或桌子：**

1.  生成对象周围的边界框，可以通过传统注释，或使用[Grounding DINO](https://github.com/IDEA-Research/GroundingDINO)，或者其他方法。

1.  选择边界框的中心点作为该对象的默认关键点。如果发现这个点不在对象内部，请相应调整。

1.  使用这些关键点和边界框来计算实例分割掩码

**对于每个连续的*物体*区域（例如天空或草地）：**

1.  添加一个或多个标记的关键点。

1.  使用这些关键点计算语义分割掩码

**填补空白：**

1.  在所有实例和语义分割掩码的基础上，识别重叠区域和没有任何掩码的区域。

1.  使用适合你应用的策略处理这些区域。

# 结论

Meta AI的Segment Anything Model功能强大且多才多艺。尽管如此，SAM只是分割和提示/引导计算机视觉领域众多令人兴奋的进展中的一个。这个领域发展非常迅速！如果你对了解更多感兴趣，我建议你查看以下相关项目：

+   [Track Anything](https://github.com/gaomingqi/track-anything)：视频分割——基于SAM构建

+   [MedSAM](https://github.com/bowang-lab/medsam)：用于医学图像的全方位分割

+   [Inpaint Anything](https://github.com/geekyutao/inpaint-anything)：Segment Anything + 填充

+   [Semantic Segment Anything](https://github.com/fudan-zvg/Semantic-Segment-Anything)：SAM + 语义标注引擎

+   [Segment Everything Everywhere All at Once](https://github.com/ux-decoder/segment-everything-everywhere-all-at-once)：类似于SAM，并支持多模态提示

# 来源

+   [Segment Anything Model](https://github.com/facebookresearch/segment-anything/blob/main/LICENSE)（[许可证](https://github.com/facebookresearch/segment-anything/blob/main/LICENSE)）

+   [Open Images V7](https://github.com/openimages/dataset/blob/main/LICENSE)（[许可证](https://github.com/openimages/dataset/blob/main/LICENSE)）
