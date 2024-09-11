# 如何优化特定领域的目标检测模型

> 原文：[https://towardsdatascience.com/how-to-optimize-object-detection-models-for-specific-domains-d8512c63a9c3?source=collection_archive---------3-----------------------#2023-10-19](https://towardsdatascience.com/how-to-optimize-object-detection-models-for-specific-domains-d8512c63a9c3?source=collection_archive---------3-----------------------#2023-10-19)

## 设计更好更快的模型以解决你的特定问题

[](https://alvaroleandrocavalcante.medium.com/?source=post_page-----d8512c63a9c3--------------------------------)[![Alvaro Leandro Cavalcante Carneiro](../Images/d23fc10840c906a43ae002185a100e40.png)](https://alvaroleandrocavalcante.medium.com/?source=post_page-----d8512c63a9c3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d8512c63a9c3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d8512c63a9c3--------------------------------) [Alvaro Leandro Cavalcante Carneiro](https://alvaroleandrocavalcante.medium.com/?source=post_page-----d8512c63a9c3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F948868831251&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-optimize-object-detection-models-for-specific-domains-d8512c63a9c3&user=Alvaro+Leandro+Cavalcante+Carneiro&userId=948868831251&source=post_page-948868831251----d8512c63a9c3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d8512c63a9c3--------------------------------) ·13 min read·2023年10月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd8512c63a9c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-optimize-object-detection-models-for-specific-domains-d8512c63a9c3&user=Alvaro+Leandro+Cavalcante+Carneiro&userId=948868831251&source=-----d8512c63a9c3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd8512c63a9c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-optimize-object-detection-models-for-specific-domains-d8512c63a9c3&source=-----d8512c63a9c3---------------------bookmark_footer-----------)![](../Images/2db005eef3f79d4f5f0e360dbbcd6ec6.png)

图片由 [paolo candelo](https://unsplash.com/@paolocandelo?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

目标检测在从学术界到工业领域的不同领域中被广泛使用，因为它能够以低计算成本提供良好的结果。然而，尽管有许多开源架构可公开获取，大多数模型设计用于解决通用问题，可能不适用于特定的上下文。

举个例子，我们可以提到[COCO数据集](https://arxiv.org/pdf/1405.0312v3.pdf)，这是在这一领域中通常作为研究基线的数据集，对模型的超参数和架构细节产生影响。该数据集包含90个不同的类别，涵盖了各种光照条件、背景和大小。事实证明，有时你面临的检测问题相对简单。你可能只需检测几个不同的对象，而没有太多的场景或大小变化。在这种情况下，如果你使用通用的超参数集来训练你的模型，你可能会得到一个产生不必要计算成本的模型。

从这个角度来看，本文的主要目标是提供优化各种目标检测模型以处理较简单任务的指导。我希望帮助你选择一个更高效的配置，以减少计算成本而不影响平均精度（mAP）。

# 提供一些背景信息

我的硕士学位目标之一是开发一个[手语识别系统](https://www.esann.org/sites/default/files/proceedings/2023/ES2023-185.pdf)，以最小的计算要求为前提。该系统的一个关键组成部分是预处理阶段，包括检测翻译员的手和脸，如下图所示：

![](../Images/0f1367786a2c8281270592926b136d9d.png)

这是本研究中创建的HFSL数据集的样本。图片作者提供。

如图所示，这个问题相对简单，只涉及两类不同的对象和图像中同时出现的三种对象。因此，我的目标是优化模型的超参数，以保持高的mAP，同时减少计算成本，从而使其能够在智能手机等边缘设备上高效执行。

# 目标检测架构和设置

在这个项目中，测试了以下目标检测架构：**EfficientDetD0、Faster-RCNN、SDD320、SDD640和YoloV7**。然而，这里提出的概念可以应用于适应各种其他架构。

在模型开发方面，我主要使用了Python 3.8和TensorFlow框架，除了YoloV7使用了PyTorch。虽然这里提供的大多数示例与TensorFlow相关，但你可以将这些原则适应于你所选择的框架。

在硬件方面，测试使用了RTX 3060 GPU和Intel Core i5–10400 CPU。所有源代码和模型都可以在[GitHub](https://github.com/AlvaroCavalcante/hand-face-detector)上找到。

# 目标检测器的微调

使用 TensorFlow 进行目标检测时，了解所有超参数都存储在一个名为“[**pipeline.config**](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/configuring_jobs.md)”的文件中是至关重要的。这个 protobuf 文件保存了用于训练和评估模型的配置，你可以在任何从[TF 模型库](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf2_detection_zoo.md)下载的预训练模型中找到它。在这个背景下，我将描述我在管道文件中实施的修改，以优化目标检测器。

> 需要注意的是，这里提供的超参数是专门为手部和面部检测（2 类，3 对象）设计的。务必根据你自己的问题领域进行调整。

## 一般简化

对所有模型可以应用的第一个变化是将每个类别的最大预测数量和生成的边界框数量从100分别减少到2和4。你可以通过调整“**max_number_of_boxes**”属性来实现这一点，该属性位于“**train_config**”对象中：

```py
...
train_config {
  batch_size: 128
  sync_replicas: true
  optimizer { ... }
  fine_tune_checkpoint: "PATH_TO_BE_CONFIGURED"
  num_steps: 50000
  startup_delay_steps: 0.0
  replicas_to_aggregate: 8
  max_number_of_boxes: 4 # <------------------ change this line
  unpad_groundtruth_tensors: false
  fine_tune_checkpoint_type: "classification"
  fine_tune_checkpoint_version: V2
}
...
```

之后，更改“**max_total_detections**” 和 “**max_detections_per_class**”，它们位于目标检测器的“**post_processing**”中：

```py
post_processing {
  batch_non_max_suppression {
    score_threshold: 9.99999993922529e-09
    iou_threshold: 0.6000000238418579
    max_detections_per_class: 2 # <------------------ change this line
    max_total_detections: 4     # <------------------ change this line
    use_static_shapes: false
  }
  score_converter: SIGMOID
}
```

这些变化很重要，特别是对我来说，因为图像中同时出现的对象只有三个且类别只有两个。通过减少预测次数，消除重叠边界框所需的迭代次数也减少了，这通过非极大值抑制 (NMS) 实现。因此，如果你需要检测的类别和场景中的对象数量有限，改变这个超参数可能是个好主意。

额外的调整是逐个应用的，考虑了每个目标检测模型的具体架构细节。

## 单次检测多框 (SSD)

在进行目标检测时，测试不同的分辨率总是一个好主意。在这个项目中，我使用了两个版本的模型，SSD320和SSD640，分别具有320x320和640x640像素的输入图像分辨率。

对于这两个模型，主要的修改之一是将[**特征金字塔网络**](https://jonathan-hui.medium.com/understanding-feature-pyramid-networks-for-object-detection-fpn-45b227b9106c) **(FPN)** 的深度从5减少到4，通过移除最表层。FPN是一个强大的特征提取机制，能够处理多种特征图尺寸。然而，对于较大的对象，最表层是为较高图像分辨率设计的，可能并不必要。也就是说，如果你要检测的对象不太小，移除这一层可能是个好主意。为了实现这一变化，将“**min_level**”属性从3调整到4，位于“**fpn**”对象中：

```py
...
feature_extractor {
  type: "ssd_mobilenet_v2_fpn_keras"
  depth_multiplier: 1.0
  min_depth: 16
  conv_hyperparams {
    regularizer { ... }
    initializer { ... }
    activation: RELU_6
    batch_norm {...}
  }
  use_depthwise: true
  override_base_feature_extractor_hyperparams: true
  fpn {
    min_level: 4 # <------------------ change this line
    max_level: 7
    additional_layer_depth: 108 # <------------------ change this line
  }
}
...
```

我还通过将“**additional_layer_depth**”从128减少到108简化了高分辨率模型（SSD640）。同样，我将两个模型的“**multiscale_anchor_generator**”深度从5调整到4层，如下所示：

```py
...
anchor_generator {
  multiscale_anchor_generator {
    min_level: 4 # <------------------ change this line
    max_level: 7
    anchor_scale: 4.0
    aspect_ratios: 1.0
    aspect_ratios: 2.0
    aspect_ratios: 0.5
    scales_per_octave: 2
  }
}
...
```

最后，生成边界框预测的网络（“**box_predictor**”）的层数从4减少到3。关于SSD640，边框预测的深度也从128减少到96，如下所示：

```py
...
box_predictor {
  weight_shared_convolutional_box_predictor {
    conv_hyperparams {
      regularizer { ... }
      initializer { ... }
      activation: RELU_6
      batch_norm { ... }
    }
    depth: 96 # <------------------ change this line
    num_layers_before_predictor: 3 # <------------------ change this line
    kernel_size: 3
    class_prediction_bias_init: -4.599999904632568
    share_prediction_tower: true
    use_depthwise: true
  }
}
...
```

这些简化是因为我们拥有的不同类别数量有限，且模式相对简单。因此，可以减少模型的层数和深度，因为即使特征图较少，我们仍能有效地从图像中提取所需的特征。

## EfficientDet-D0

关于EfficientDet-D0，我将[**双向特征金字塔网络**](https://paperswithcode.com/method/bifpn) **（Bi-FPN）**的深度从5减少到4。此外，我将Bi-FPN的迭代次数从3减少到2，将特征图内核从64减少到48。Bi-FPN是一种复杂的多尺度特征融合技术，可以产生出色的结果。然而，它也需要较高的计算资源，对于简单的问题，这可能会浪费资源。要实施上述调整，只需按如下方式更新“**bifpn**”对象的属性：

```py
...
bifpn {
      min_level: 4 # <------------------ change this line
      max_level: 7
      num_iterations: 2 # <------------------ change this line
      numyaml_filters: 48 # <------------------ change this line
    }
...
```

除此之外，还需要像处理SSD时一样，减少“**multiscale_anchor_generator**”的深度。最后，我将边框预测网络的层数从3减少到2：

```py
...
box_predictor {
  weight_shared_convolutional_box_predictor {
    conv_hyperparams {
      regularizer { ... }
      initializer { ... }
      activation: SWISH
      batch_norm { ... }
      force_use_bias: true
    }
    depth: 64
    num_layers_before_predictor: 2 # <------------------ change this line
    kernel_size: 3
    class_prediction_bias_init: -4.599999904632568
    use_depthwise: true
  }
}
...
```

## Faster R-CNN

Faster R-CNN模型依赖于[**区域建议网络**](https://paperswithcode.com/method/rpn#:~:text=A%20Region%20Proposal%20Network%2C%20or,generate%20high%2Dquality%20region%20proposals.)（RPN）和锚框作为其主要技术。锚框是一个滑动窗口的中心点，它在骨干CNN的最后特征图上迭代。每次迭代时，分类器会确定一个建议中是否包含物体的概率，而回归器则调整边界框坐标。为了确保检测器具有平移不变性，它使用三种不同的尺度和三种宽高比的锚框，这增加了每次迭代的建议数量。

虽然这是一个简单的解释，但显而易见，由于其两阶段检测过程，这个模型比其他模型复杂得多。然而，可以简化它并提高其速度，同时保持高精度。

为此，第一个重要的修改是将生成的建议数量从300减少到50。这一减少是可行的，因为图像中同时存在的物体数量较少。你可以通过调整“**first_stage_max_proposals**”属性来实现这一变化，如下所示：

```py
...
first_stage_box_predictor_conv_hyperparams {
  op: CONV
  regularizer { ... }
  initializer { ... }
}
first_stage_nms_score_threshold: 0.0
first_stage_nms_iou_threshold: 0.7
first_stage_max_proposals: 50 # <------------------ change this line
first_stage_localization_loss_weight: 2.0
first_stage_objectness_loss_weight: 1.0
initial_crop_size: 14
maxpool_kernel_size: 2
maxpool_stride: 2
...
```

之后，我从模型中移除了最大的锚框尺度 (**2.0**)。这个改变是因为手部和面部由于解释器与相机的固定距离而保持一致的大小，大锚框可能对提议生成没有用。此外，我移除了一个锚框的长宽比，因为我的物体在数据集中具有类似的形状，变化很小。下面的调整以视觉形式展示：

```py
first_stage_anchor_generator {
  grid_anchor_generator {
    scales: [0.25, 0.5, 1.0] # <------------------ change this line
    aspect_ratios: [0.5, 1.0] # <------------------ change this line
    height_stride: 16
    width_stride: 16
  }
}
```

也就是说，考虑目标物体的尺寸和长宽比至关重要。这种考虑可以帮助你排除不太有用的锚框，并显著减少模型的计算成本。

## YoloV7

相比之下，对 YoloV7 应用的更改很少，以保留架构的功能性。主要的修改是简化了负责特征提取的 CNN，包括骨干网和模型的头部。为此，我减少了几乎每个卷积层的内核/特征图数量，创建了以下模型：

```py
backbone:
  # [from, number, module, args]
  [[-1, 1, Conv, [22, 3, 1]],  # 0
   [-1, 1, Conv, [44, 3, 2]],  # 1-P1/2      
   [-1, 1, Conv, [44, 3, 1]],
   [-1, 1, Conv, [89, 3, 2]],  # 3-P2/4  
   [-1, 1, Conv, [44, 1, 1]],
   [-2, 1, Conv, [44, 1, 1]],
   [-1, 1, Conv, [44, 3, 1]],
   [-1, 1, Conv, [44, 3, 1]],
   [-1, 1, Conv, [44, 3, 1]],
   [-1, 1, Conv, [44, 3, 1]],
   [[-1, -3, -5, -6], 1, Concat, [1]],
   [-1, 1, Conv, [179, 1, 1]],  # 11
   [-1, 1, MP, []],
   [-1, 1, Conv, [89, 1, 1]],
   [-3, 1, Conv, [89, 1, 1]],
   [-1, 1, Conv, [89, 3, 2]],
   [[-1, -3], 1, Concat, [1]],  # 16-P3/8  
   [-1, 1, Conv, [89, 1, 1]],
   [-2, 1, Conv, [89, 1, 1]],
   [-1, 1, Conv, [89, 3, 1]],
   [-1, 1, Conv, [89, 3, 1]],
   [-1, 1, Conv, [89, 3, 1]],
   [-1, 1, Conv, [89, 3, 1]],
   [[-1, -3, -5, -6], 1, Concat, [1]],
   [-1, 1, Conv, [512, 1, 1]],  # 24
   [-1, 1, MP, []],
   [-1, 1, Conv, [89, 1, 1]],
   [-3, 1, Conv, [89, 1, 1]],
   [-1, 1, Conv, [89, 3, 2]],
   [[-1, -3], 1, Concat, [1]],  # 29-P4/16  
   [-1, 1, Conv, [89, 1, 1]],
   [-2, 1, Conv, [89, 1, 1]],
   [-1, 1, Conv, [89, 3, 1]],
   [-1, 1, Conv, [89, 3, 1]],
   [-1, 1, Conv, [89, 3, 1]],
   [-1, 1, Conv, [89, 3, 1]],
   [[-1, -3, -5, -6], 1, Concat, [1]],
   [-1, 1, Conv, [716, 1, 1]],  # 37
   [-1, 1, MP, []],
   [-1, 1, Conv, [256, 1, 1]],
   [-3, 1, Conv, [256, 1, 1]],
   [-1, 1, Conv, [256, 3, 2]],
   [[-1, -3], 1, Concat, [1]],  # 42-P5/32  
   [-1, 1, Conv, [128, 1, 1]],
   [-2, 1, Conv, [128, 1, 1]],
   [-1, 1, Conv, [128, 3, 1]],
   [-1, 1, Conv, [128, 3, 1]],
   [-1, 1, Conv, [128, 3, 1]],
   [-1, 1, Conv, [128, 3, 1]],
   [[-1, -3, -5, -6], 1, Concat, [1]],
   [-1, 1, Conv, [716, 1, 1]],  # 50
  ]

# yolov7 head
head:
  [[-1, 1, SPPCSPC, [358]], # 51
   [-1, 1, Conv, [179, 1, 1]],
   [-1, 1, nn.Upsample, [None, 2, 'nearest']],
   [37, 1, Conv, [179, 1, 1]], # route backbone P4
   [[-1, -2], 1, Concat, [1]],
   [-1, 1, Conv, [179, 1, 1]],
   [-2, 1, Conv, [179, 1, 1]],
   [-1, 1, Conv, [89, 3, 1]],
   [-1, 1, Conv, [89, 3, 1]],
   [-1, 1, Conv, [89, 3, 1]],
   [-1, 1, Conv, [89, 3, 1]],
   [[-1, -2, -3, -4, -5, -6], 1, Concat, [1]],
   [-1, 1, Conv, [179, 1, 1]], # 63
   [-1, 1, Conv, [89, 1, 1]],
   [-1, 1, nn.Upsample, [None, 2, 'nearest']],
   [24, 1, Conv, [89, 1, 1]], # route backbone P3
   [[-1, -2], 1, Concat, [1]],
   [-1, 1, Conv, [89, 1, 1]],
   [-2, 1, Conv, [89, 1, 1]],
   [-1, 1, Conv, [44, 3, 1]],
   [-1, 1, Conv, [44, 3, 1]],
   [-1, 1, Conv, [44, 3, 1]],
   [-1, 1, Conv, [44, 3, 1]],
   [[-1, -2, -3, -4, -5, -6], 1, Concat, [1]],
   [-1, 1, Conv, [89, 1, 1]], # 75
   [-1, 1, MP, []],
   [-1, 1, Conv, [89, 1, 1]],
   [-3, 1, Conv, [89, 1, 1]],
   [-1, 1, Conv, [89, 3, 2]],
   [[-1, -3, 63], 1, Concat, [1]],
   [-1, 1, Conv, [179, 1, 1]],
   [-2, 1, Conv, [179, 1, 1]],
   [-1, 1, Conv, [89, 3, 1]],
   [-1, 1, Conv, [89, 3, 1]],
   [-1, 1, Conv, [89, 3, 1]],
   [-1, 1, Conv, [89, 3, 1]],
   [[-1, -2, -3, -4, -5, -6], 1, Concat, [1]],
   [-1, 1, Conv, [179, 1, 1]], # 88
   [-1, 1, MP, []],
   [-1, 1, Conv, [179, 1, 1]],
   [-3, 1, Conv, [179, 1, 1]],
   [-1, 1, Conv, [179, 3, 2]],
   [[-1, -3, 51], 1, Concat, [1]],
   [-1, 1, Conv, [179, 1, 1]],
   [-2, 1, Conv, [179, 1, 1]],
   [-1, 1, Conv, [128, 3, 1]],
   [-1, 1, Conv, [128, 3, 1]],
   [-1, 1, Conv, [128, 3, 1]],
   [-1, 1, Conv, [128, 3, 1]],
   [[-1, -2, -3, -4, -5, -6], 1, Concat, [1]],
   [-1, 1, Conv, [358, 1, 1]], # 101
   [75, 1, RepConv, [179, 3, 1]],
   [88, 1, RepConv, [358, 3, 1]],
   [101, 1, RepConv, [716, 3, 1]],
   [[102,103,104], 1, IDetect, [nc, anchors]],   # Detect(P3, P4, P5)
  ]
```

如前所述，从检测器中移除一些层和特征图通常是处理简单问题的好方法，因为特征提取器最初是设计用于检测数十种甚至数百种不同类别，在多种场景中需要更强大的模型来应对这些复杂性并确保高准确性。

通过这些调整，我将参数数量从 3640 万减少到仅 1410 万，减少约 61%。此外，我使用了 512x512 像素的输入分辨率，而不是原论文中建议的 640x640 像素。

## 额外提示

另一个在目标检测训练中的宝贵提示是利用 [Kmeans 模型进行锚框比例的无监督调整](https://github.com/tensorflow/models/blob/master/research/object_detection/colab_tutorials/generate_ssd_anchor_box_aspect_ratios_using_k_means_clustering.ipynb)，将图形的宽度和高度调整到最大化训练集中的交并比 (IoU) 比例。通过这样做，我们可以更好地适应给定的问题领域，从而通过以合适的长宽比开始，提升模型收敛。下图展示了这个过程，将 SSD 算法中默认使用的三个锚框（红色）与用于手部和面部检测任务的三个优化比例的框（绿色）进行比较。

![](../Images/bec419cc78db42499e6ebebf3fcd54c3.png)

比较不同边界框的长宽比。图片由作者提供。

# 展示结果

我使用自己的数据集，即[手部和面部手语](https://github.com/AlvaroCavalcante/hand-face-detector)（HFSL）数据集，训练和评估了每个检测器，考虑了mAP和每秒帧数（FPS）作为主要指标。下表提供了结果的总结，表中的值（括号中的数字）表示在实施任何描述的优化之前检测器的FPS。

![](../Images/7b70a868c04ab549fc667045dddee8b1.png)

目标检测结果。

我们可以观察到，大多数模型在保持各个层次的[交并比](https://pyimagesearch.com/2016/11/07/intersection-over-union-iou-for-object-detection/)（IoU）高mAP的同时，推理时间显著减少。更复杂的架构，如Faster R-CNN和EfficientDet，分别将GPU上的FPS提高了200.80%和231.78%。即使是基于SSD的架构，其性能也有了巨大的提升，640版本和320版本的性能分别提高了280.23%和159.59%。考虑到YoloV7，尽管在CPU上的FPS差异最为明显，但优化后的模型参数减少了61%，降低了内存需求，使其更适合边缘设备。

# 结论

在计算资源有限或任务必须快速执行的情况下，我们可以进一步优化开源目标检测模型，以寻找能够减少计算需求而不影响结果的超参数组合，从而为各种问题领域提供合适的解决方案。

我希望这篇文章能帮助你在训练目标检测器时做出更好的选择，从而在付出最小努力的情况下实现显著的效率提升。如果你对某些解释的概念不太理解，我建议你深入了解你的目标检测架构是如何工作的。此外，考虑尝试不同的超参数值，以根据你所解决的具体问题进一步优化你的模型！
