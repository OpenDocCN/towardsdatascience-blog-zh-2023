# 增强型目标检测：如何有效实现 YOLOv8

> 原文：[`towardsdatascience.com/enhanced-object-detection-how-to-effectively-implement-yolov8-afd1bf6132ae`](https://towardsdatascience.com/enhanced-object-detection-how-to-effectively-implement-yolov8-afd1bf6132ae)

## 一本关于在图像、视频和实时摄像头画面中使用 CLI 和 Python 进行目标检测的实用指南。

[](https://thomasdorfer.medium.com/?source=post_page-----afd1bf6132ae--------------------------------)![Thomas A Dorfer](https://thomasdorfer.medium.com/?source=post_page-----afd1bf6132ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----afd1bf6132ae--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----afd1bf6132ae--------------------------------) [Thomas A Dorfer](https://thomasdorfer.medium.com/?source=post_page-----afd1bf6132ae--------------------------------)

· 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----afd1bf6132ae--------------------------------) · 阅读时间：7 分钟 · 2023 年 3 月 23 日

--

![](img/7648a5890412b71ee953a2a079374602.png)

视频由 [Camilo Calderón](https://www.pexels.com/@camilo-calderon-3343529/) 制作，发布在 [Pexels](https://www.pexels.com/video/a-video-footage-of-busy-street-4997787/)。由作者转换为 GIF 格式。

# 介绍

目标检测是计算机视觉的一个子领域，主要关注于以一定的置信度识别和定位图像或视频中的对象。被识别的对象通常会用一个边界框进行标注，这为观众提供有关对象性质和在场景中位置的信息。

2015 年，[**YOLO** 的首次发布](https://arxiv.org/abs/1506.02640)，即 **You Only Look Once**，震撼了计算机视觉界，因为它的系统能够以惊人的准确性和速度进行实时目标检测。从那时起，YOLO 经过了几次改进，提高了预测准确性和效率，最终推出了其最新的家族成员：由 [Ultralytics](https://github.com/ultralytics/ultralytics) 推出的 **YOLOv8**。

YOLOv8 有五个版本：nano (n)、small (s)、medium (m)、large (l) 和 extra large (x)。它们各自的改进可以通过它们的平均精度均值（mAP）和延迟来展示，这些是通过 [COCO val2017](http://cocodataset.org/) 数据集进行评估的。

![](img/1d7acc0bee95ad83fd133a136f70ce50.png)

图片由 [Ultralytics](https://github.com/ultralytics/ultralytics) 提供。许可证： [GNU 通用公共许可证](https://github.com/ultralytics/ultralytics/blob/main/LICENSE)。

与之前的版本相比，YOLOv8 不仅更快更准确，还需要更少的参数来实现其性能，而且，如果这还不够，它还配备了一个直观且易于使用的命令行接口（CLI）以及一个[Python 包](https://pypi.org/project/ultralytics/)，为用户和开发者提供了更无缝的体验。

在本文中，我将演示如何使用 CLI 和 Python 将 YOLOv8 应用于静态图像、视频和实时摄像头中的对象检测。

不再赘述，让我们开始吧！

# 安装

要开始使用 YOLOv8，你只需在终端中运行以下命令：

```py
pip install ultralytics
```

这将通过`ultralytics` pip 包安装 YOLOv8。

# 图像检测

静态图像中的对象检测在许多领域中都被证明是有用的，如监控、医学成像或零售分析。无论你选择将检测系统应用于哪个领域，YOLOv8 都使这一过程变得极其简单。

下面是我们将进行对象检测的原始图像。

![](img/72f307a8e29880c10ac55e9b51f8d2ea.png)

[詹姆斯·科尔曼](https://unsplash.com/@jhc)拍摄，发布于[Unsplash](https://unsplash.com/photos/jViepQKI01Q)。

为了运行 YOLOv8，我们将探讨 CLI 和 Python 实现。虽然在这个具体的例子中我们将使用一张`jpg`图像，但 YOLOv8 支持多种不同的[图像格式](https://docs.ultralytics.com/modes/predict/#image-formats)。

## CLI

假设我们想在图像（我们称之为`img.jpg`）上运行超大 YOLOv8x，可以将以下命令输入 CLI：

```py
yolo detect predict model=yolov8x.pt source="img.jpg" save=True
```

在这里，我们指定以下参数：`detect`用于对象检测，`predict`用于执行预测任务，`model`用于选择模型版本，`source`用于提供图像的文件路径，以及`save`用于保存带有对象边界框及其预测类别和类别概率的处理图像。

## Python

在 Python 中，完全相同的任务可以通过以下直观且低代码的解决方案来完成：

```py
from ultralytics import YOLO

model = YOLO('yolov8x.pt')
results = model('img.jpg', save=True)
```

无论你使用 CLI 还是 Python，在任何情况下，保存的处理后图像如下所示：

![](img/31f70aa8124121a131a552d4f565fc2c.png)

[詹姆斯·科尔曼](https://unsplash.com/@jhc)拍摄，发布于[Unsplash](https://unsplash.com/photos/jViepQKI01Q)。经作者使用 YOLOv8 处理。

我们可以清楚地看到它检测到的每个对象周围的边界框，以及它们对应的类别标签和概率。

# 视频检测

对视频文件进行对象检测与图像文件几乎相同，唯一的区别是源文件格式。与图像一样，YOLOv8 支持多种不同的[视频格式](https://docs.ultralytics.com/modes/predict/#video-formats)作为模型的输入。在我们的案例中，我们将使用一个`mp4`文件。

让我们再次查看 CLI 和 Python 实现。为了加快计算速度，我们现在使用 YOLOv8m 模型，而不是超大版本。

## CLI

```py
yolo detect predict model=yolov8m.pt source="vid.mp4" save=True
```

## Python

```py
from ultralytics import YOLO

model = YOLO('yolov8m.pt')
results = model('vid.mp4', save=True)
```

首先，在对`vid.mp4`文件进行物体检测之前，让我们检查一下原始文件：

![](img/69a4afce1a59ac810aa7ccb5fa3bed97.png)

视频由[Camilo Calderón](https://www.pexels.com/@camilo-calderon-3343529/)提供，发布在[Pexels](https://www.pexels.com/video/a-video-footage-of-busy-street-4997787/)上。作者转换为 GIF 格式。

视频展示了一个繁忙城市的场景，交通繁忙，包括汽车、公交车、卡车和骑自行车的人，以及右侧的一些人显然在等公交车。

使用 YOLOv8 的中等版本处理此文件后，我们得到了以下结果：

![](img/7648a5890412b71ee953a2a079374602.png)

视频由[Camilo Calderón](https://www.pexels.com/@camilo-calderon-3343529/)提供，发布在[Pexels](https://www.pexels.com/video/a-video-footage-of-busy-street-4997787/)上。使用 YOLOv8m 处理并由作者转换为 GIF 格式。

再次看到，YOLOv8m 在准确捕捉场景中的物体方面做得非常好。它甚至能够检测到大整体中的小物体，例如骑自行车背着背包的人。

# 实时检测

最后，让我们看看在实时网络摄像头喂送中检测物体需要什么。为此，我将使用我的个人网络摄像头，并像之前一样，采用 CLI 和 Python 方法。

为了减少延迟和视频中的滞后，我将使用 YOLOv8 的轻量级 nano 版本。

## CLI

```py
yolo detect predict model=yolov8n.pt source=0 show=True
```

这些参数大多与我们之前在图像和视频文件中看到的一致，不同之处在于`source`，它允许我们指定使用的视频源。在我的情况下，它是内置的网络摄像头（0）。

## Python

```py
from ultralytics import YOLO

model = YOLO('yolov8n.pt')
model.predict(source="0", show=True)
```

再次，我们可以通过超低代码解决方案在 Python 中执行相同的任务。

这是 YOLOv8n 在实时网络摄像头上显示的插图：

![](img/1eaea037e2dd5c0a26e7956db86e34c8.png)

GIF 由作者制作。使用 YOLOv8n 从网络摄像头录制。

令人印象深刻！尽管视频质量较低且光线条件较差，它仍然相当好地捕捉了物体，甚至检测到了一些背景中的物体，比如左侧的橄榄油和醋瓶以及右侧的水槽。

值得注意的是，尽管这些直观的 CLI 命令和低代码 Python 解决方案是快速开始物体检测任务的好方法，但在自定义配置方面确实存在局限性。例如，如果我们想要配置边框的美观或执行一些简单任务，比如统计和显示在任何给定时间检测到的物体数量，我们就必须使用[cv2](https://pypi.org/project/opencv-python/)或[supervision](https://pypi.org/project/supervision/)等包自行编写自定义实现。

实际上，上面的摄像头视频是使用以下 Python 代码录制的，以调整摄像头的分辨率和自定义定义边界框及其注释。（注意：这主要是为了使上面的 GIF 更具可观赏性。上述的 CLI 和 Python 实现已经足以产生类似的效果。）

```py
import cv2
import supervision as sv
from ultralytics import YOLO

def main():

    # to save the video
    writer= cv2.VideoWriter('webcam_yolo.mp4', 
                            cv2.VideoWriter_fourcc(*'DIVX'), 
                            7, 
                            (1280, 720))

    # define resolution
    cap = cv2.VideoCapture(0)
    cap.set(cv2.CAP_PROP_FRAME_WIDTH, 1280)
    cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 720)

    # specify the model
    model = YOLO("yolov8n.pt")

    # customize the bounding box
    box_annotator = sv.BoxAnnotator(
        thickness=2,
        text_thickness=2,
        text_scale=1
    )

    while True:
        ret, frame = cap.read()
        result = model(frame, agnostic_nms=True)[0]
        detections = sv.Detections.from_yolov8(result)
        labels = [
            f"{model.model.names[class_id]} {confidence:0.2f}"
            for _, confidence, class_id, _
            in detections
        ]
        frame = box_annotator.annotate(
            scene=frame, 
            detections=detections, 
            labels=labels
        ) 

        writer.write(frame)

        cv2.imshow("yolov8", frame)

        if (cv2.waitKey(30) == 27): # break with escape key
            break

    cap.release()
    writer.release()
    cv2.destroyAllWindows()

if __name__ == "__main__":
    main()
```

尽管本文中的代码细节超出了讨论范围，但这里有一个很好的参考，采用了类似的方法，如果您有兴趣提升您的目标检测技能：

# 结论

YOLOv8 不仅在准确性和速度上超越了其前辈，而且通过极易使用的 CLI 和低代码 Python 解决方案显著提升了用户体验。它还提供了五种不同的模型版本，用户可以根据个人需求和对延迟及准确性的容忍度选择合适的版本。

无论您的目标是对静态图像、视频还是实时摄像头进行目标检测，YOLOv8 都可以无缝完成。然而，如果您的应用需要自定义配置，您可能需要依赖其他计算机视觉包，例如 `cv2` 和 `supervision`。

## 更多资源

+   [ultralytics/ultralytics: NEW — YOLOv8 🚀 in PyTorch > ONNX > CoreML > TFLite (github.com)](https://github.com/ultralytics/ultralytics)

+   [YOLOv8 文档 (ultralytics.com)](https://docs.ultralytics.com/)

+   [YOLOv8 实时物体计数，使用摄像头、OpenCV 和 Supervision — YouTube](https://www.youtube.com/watch?v=QV85eYOb7gk)

## 喜欢这篇文章吗？

让我们联系吧！您可以在 [Twitter](https://twitter.com/ThomasADorfer) 和 [LinkedIn](https://www.linkedin.com/in/thomasdorfer/) 上找到我。

如果您希望支持我的写作，您可以通过 [Medium 会员](https://thomasdorfer.medium.com/membership) 来实现，这样您可以访问我所有的故事，以及 Medium 上其他成千上万作家的文章。

[## 通过我的推荐链接加入 Medium - Thomas A Dorfer](https://medium.com/@thomasdorfer/membership?source=post_page-----afd1bf6132ae--------------------------------)

### 阅读 Thomas A Dorfer 的每一篇文章（以及 Medium 上成千上万的其他作家的文章）。您的会员费用直接支持……

[medium.com](https://medium.com/@thomasdorfer/membership?source=post_page-----afd1bf6132ae--------------------------------)
