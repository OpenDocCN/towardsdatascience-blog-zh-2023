# 自定义 YOLOv7 对象检测与 TensorFlow.js

> 原文：[https://towardsdatascience.com/training-a-custom-yolov7-in-pytorch-and-running-it-directly-in-the-browser-with-tensorflow-js-96a5ecd7a530?source=collection_archive---------6-----------------------#2023-03-28](https://towardsdatascience.com/training-a-custom-yolov7-in-pytorch-and-running-it-directly-in-the-browser-with-tensorflow-js-96a5ecd7a530?source=collection_archive---------6-----------------------#2023-03-28)

![](../Images/9b4d0e99afd60aaf9cf2530e000da490.png)

照片由 [Martijn Baudoin](https://unsplash.com/@martijnbaudoin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，来源于 [Unsplash](https://unsplash.com/photos/4z0-2mQE7io?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 在 PyTorch 中训练自定义 YOLOv7 模型，并将其转换为 TensorFlow.js 以实现浏览器上的实时离线检测

[](https://medium.com/@zaninihugo?source=post_page-----96a5ecd7a530--------------------------------)[![Hugo Zanini](../Images/cbda793f1cf82f34b29c6e136556361d.png)](https://medium.com/@zaninihugo?source=post_page-----96a5ecd7a530--------------------------------)[](https://towardsdatascience.com/?source=post_page-----96a5ecd7a530--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----96a5ecd7a530--------------------------------) [Hugo Zanini](https://medium.com/@zaninihugo?source=post_page-----96a5ecd7a530--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8f4c63386bac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-a-custom-yolov7-in-pytorch-and-running-it-directly-in-the-browser-with-tensorflow-js-96a5ecd7a530&user=Hugo+Zanini&userId=8f4c63386bac&source=post_page-8f4c63386bac----96a5ecd7a530---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----96a5ecd7a530--------------------------------) ·6 分钟阅读·2023年3月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F96a5ecd7a530&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-a-custom-yolov7-in-pytorch-and-running-it-directly-in-the-browser-with-tensorflow-js-96a5ecd7a530&user=Hugo+Zanini&userId=8f4c63386bac&source=-----96a5ecd7a530---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F96a5ecd7a530&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-a-custom-yolov7-in-pytorch-and-running-it-directly-in-the-browser-with-tensorflow-js-96a5ecd7a530&source=-----96a5ecd7a530---------------------bookmark_footer-----------)

最近，我开源了一个[**YOLOv7在Tensorflow.js中的实现**](https://github.com/hugozanini/yolov7-tfjs?linkId=8607585)，我收到的最常见问题是：

> 你是如何将模型从PyTorch**转换**为Tensorflow**.js**的？

本文将通过使用自定义**YOLOv7**模型在**浏览器和离线环境下直接运行**来解决实际问题。

我们将解决的行业是**实体零售**。除了最近发生的数字化转型——主要是在疫情期间—— [实体店仍然是客户最偏爱的购物目的地](https://www.forbes.com/sites/sap/2022/02/07/retail-trends-2022-in-search-of-the-ultimate-customer-experience/?sh=13a0809c52ad)。

商店中的一切都关乎体验。 [零售反馈组](https://feedbackgroup.com/)（RFG）已经跟踪了大约15年的杂货购物体验，[一贯发现](https://www.winsightgrocerybusiness.com/retailers/supermarket-experience-its-time-integrate) 影响客户满意度的最关键因素是顾客是否能够**在访问期间找到他们所需的一切，无论是在店内还是在线**。

所以零售商们不断关注**产品可用性**和**适当的商品组合**以满足客户需求。

在上一篇文章中，我展示了[如何创建一个TensorFlow.js模型来识别SKU](https://blog.tensorflow.org/2022/05/real-time-sku-detection-in-browser.html)。在这篇文章中，我们将探讨如何使用自定义YOLOv7模型来**识别空货架**——一切都在**实时、离线和智能手机的浏览器中运行**。

本文将涵盖的内容：

+   配置环境；

+   收集数据；

+   准备模型进行训练；

+   训练和重新参数化模型；

+   评估模型；

+   转换为TensorFlow.js；

+   在网页浏览器上部署模型。

## 配置环境

整个训练流程将使用Google Colab提供的免费GPU执行。如果你想访问包含所有整合步骤的笔记本，[点击这里](https://colab.research.google.com/drive/185pYr5D2us666KxGim3INaJNAx8vIkcS?usp=sharing)。

## 收集数据；

我们将用来训练模型的数据集是 [零售空货架——缺货](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi%3A10.7910%2FDVN%2F8RET7B&version=DRAFT)（[CC0 1.0许可证](https://creativecommons.org/publicdomain/zero/1.0/)）。它有1155张图像中的3608个标注和一个独特的类别：*缺货*。

![](../Images/70b73d9895d065c5c32565ee121080d5.png)

数据集样本来自 [哈佛数据中心](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi%3A10.7910%2FDVN%2F8RET7B&version=DRAFT)

注释应采用YOLOv7格式，每张图像都有相应的`txt`文件。每行的第一个值表示类别——对于Stockout数据集，所有类别都相同，等于`0`。行中的其余四个数字表示边界框的坐标。

> 如果你想创建自己的数据集，可以使用像[CVAT](https://github.com/opencv/cvat)这样的工具。

![](../Images/d5c36d8d43a31dd9c75c83234e7adae3.png)

aYOLOv7注释示例 | 图片作者提供

要下载并解压[库存数据集](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi%3A10.7910%2FDVN%2F8RET7B)，请使用以下代码：

## 为训练准备模型

第一步是克隆YOLOv7的代码库并安装依赖：

然后，下载在[COCO 2017 数据集](https://cocodataset.org/#home)上预训练的权重。这些权重将用于初始化模型并加速训练——这种技术被称为[迁移学习](https://www.tensorflow.org/tutorials/images/transfer_learning)。

由于我们处理的是单类问题，我们选择了YOLOv7-tiny，这是YOLOv7的轻量级变体。

在开始训练之前，我们需要配置一个.yaml文件，设置我们想使用的参数。

## 训练和重新参数化模型

训练过程直观且可定制，允许你调整诸如epoch数量和批次等参数，以适应数据集的需求。

执行结束时，你将得到保存在`yolov7/runs/train/your-model-name/weights/best.pt`的权重。

要以可视化格式查看训练指标，请启动TensorBoard或打开图像`yolov7/runs/train/your-model-name/results.png`。

![](../Images/cd8431647b1ccbc7dc39022a64a6d612.png)

运行tensorboard | 图片作者提供

![](../Images/66f74eb7cfef6723a38067d512565204.png)

results.png | 图片作者提供

现在你已经训练好了模型，是时候**重新参数化权重以进行推理**。

除了实时目标检测的架构优化，YOLOv7还引入了额外的模块和方法，这些模块可以提高训练效率和目标检测精度。这些模块被称为**免费礼包**，必须优化以实现高效推理。有关更多信息，请参阅[模型论文](https://arxiv.org/abs/2207.02696)。

检查下面代码中的权重路径并执行，以生成重新参数化的模型。

## 评估模型

现在模型已针对推理进行了优化，接下来是运行一些测试图像，以查看它是否检测到货架上的空白区域。

## 转换为TensorFlow.js

转换模型可能会很具挑战性，因为它需要经过几个转换：PyTorch到ONNX，ONNX到TensorFlow，最后是TensorFlow到TensorFlow.js。

以下代码将为你处理所有事情。只需确保**模型路径**配置正确，然后运行它！

完成后，生成的TensorFlow.js模型将由Google Colab自动下载。

假设一切顺利，模型现在将被转换为***TensorFlow.js layers format***。

下载到本地的文件夹应包含一个*model.json*文件和一组二进制格式的分片权重文件。*model.json*包含模型拓扑（即“架构”或“图”：层及其连接方式的描述）和权重文件的清单。

```py
└ stockout_web_model
  ├── group1-shard1of6.bin
  ├── group1-shard2of6.bin
  ├── group1-shard3of6.bin
  ├── group1-shard4of6.bin
  ├── group1-shard5of6.bin
  ├── group1-shard6of6.bin
  └── model.json
```

## 部署模型

现在模型已经准备好可以加载到JavaScript中。如前所述，我开源了一个YOLOv7的JavaScript代码。因此，我们可以利用相同的代码库，并用我们刚刚训练的模型替换原有模型。

克隆[代码库](https://github.com/hugozanini/yolov7-tfjs.git)：

```py
git clone https://github.com/hugozanini/yolov7-tfjs.git
```

安装这些包：

```py
npm install
```

进入`[public](https://github.com/hugozanini/yolov7-tfjs/tree/master/public)`文件夹并粘贴训练好的模型。你应该有如下结构：

```py
├── git-media
├── index.html
├── LICENSE
├── node_modules
├── package.json
├── public
│    ├── stockout_web_model
│    └── yolov7_web_model
├── README.MD
└── src
```

前往`[src/App.jsx](https://github.com/hugozanini/yolov7-tfjs/blob/master/src/App.jsx)`并将`[line 29](https://github.com/hugozanini/yolov7-tfjs/blob/4efa4cba39d1168d4bcf7bfe79274e54e87d2eaa/src/App.jsx#L29)`上的模型名称更改为stockout：

```py
 const modelName = "stockout";
```

要执行应用程序，请前往根文件夹并运行以下命令：

```py
npm start
```

将启动本地服务器，你应该会看到类似于这样的内容：

![](../Images/741d69af65df9bc8058117a62bbad714.png)

运行示例 | 作者提供的图像

这个模型也部署在CodesSandbox上。访问下面的链接查看其运行情况。

[](https://codesandbox.io/p/github/hugozanini/yolov7-tfjs/stockout?file=%2FREADME.md&workspace=%257B%2522activeFileId%2522%253A%2522clffv5qby000yg1fw2ridgthd%2522%252C%2522openFiles%2522%253A%255B%2522%252FREADME.md%2522%255D%252C%2522sidebarPanel%2522%253A%2522EXPLORER%2522%252C%2522gitSidebarPanel%2522%253A%2522COMMIT%2522%252C%2522spaces%2522%253A%257B%2522clfjm1t8i00733b6idq6lqztm%2522%253A%257B%2522key%2522%253A%2522clfjm1t8i00733b6idq6lqztm%2522%252C%2522name%2522%253A%2522Default%2522%252C%2522devtools%2522%253A%255B%257B%2522type%2522%253A%2522PREVIEW%2522%252C%2522taskId%2522%253A%2522start%2522%252C%2522port%2522%253A3000%252C%2522key%2522%253A%2522clfjm1t8i00743b6iw255o0qk%2522%252C%2522isMinimized%2522%253Afalse%257D%255D%257D%257D%252C%2522currentSpace%2522%253A%2522clfjm1t8i00733b6idq6lqztm%2522%252C%2522spacesOrder%2522%253A%255B%2522clfjm1t8i00733b6idq6lqztm%2522%255D%252C%2522hideCodeEditor%2522%253Afalse%257D&source=post_page-----96a5ecd7a530--------------------------------) [## CondeSandbox

[缺货检测器](https://codesandbox.io/p/github/hugozanini/yolov7-tfjs/stockout?file=%2FREADME.md&workspace=%257B%2522activeFileId%2522%253A%2522clffv5qby000yg1fw2ridgthd%2522%252C%2522openFiles%2522%253A%255B%2522%252FREADME.md%2522%255D%252C%2522sidebarPanel%2522%253A%2522EXPLORER%2522%252C%2522gitSidebarPanel%2522%253A%2522COMMIT%2522%252C%2522spaces%2522%253A%257B%2522clfjm1t8i00733b6idq6lqztm%2522%253A%257B%2522key%2522%253A%2522clfjm1t8i00733b6idq6lqztm%2522%252C%2522name%2522%253A%2522Default%2522%252C%2522devtools%2522%253A%255B%257B%2522type%2522%253A%2522PREVIEW%2522%252C%2522taskId%2522%253A%2522start%2522%252C%2522port%2522%253A3000%252C%2522key%2522%253A%2522clfjm1t8i00743b6iw255o0qk%2522%252C%2522isMinimized%2522%253Afalse%257D%255D%257D%257D%252C%2522currentSpace%2522%253A%2522clfjm1t8i00733b6idq6lqztm%2522%252C%2522spacesOrder%2522%253A%255B%2522clfjm1t8i00733b6idq6lqztm%2522%255D%252C%2522hideCodeEditor%2522%253Afalse%257D&source=post_page-----96a5ecd7a530--------------------------------)

利用YOLOv7，可以检测到多达80种不同的类别。对于零售行业的企业而言，这是一个提升店内产品执行的绝佳机会。

通过训练模型识别公司所有产品，零售商可以确保他们的**产品正确摆放在货架上，以便顾客轻松找到所需的商品**。

为验证训练模型的有效性，我带着手机去了一家杂货店和药店，并记录了一些实时运行检测器的示例。

在下面的视频中，您可以验证解决方案在真实环境中准确检测空货架的能力。

与[SKU识别模型](https://blog.tensorflow.org/2022/05/real-time-sku-detection-in-browser.html)结合使用的缺货检测器可以极大地提升零售业务的效率和效果。

虽然存在基于云的解决方案，但有时它们可能很慢，需要长达24小时才能处理一次检测。相比之下，使用TensorFlow.js模型在智能手机浏览器上进行离线实时识别，可以让企业做出更即时的决策，并更快速地响应缺货情况。

总的来说，将缺货检测器与SKU识别模型结合使用，可以为优化零售操作和提升顾客购物体验提供强大的途径。通过实时分析和离线识别能力，企业可以做出明智的决策，并快速响应变化的情况。

如果您有任何问题或想分享一个用户案例，请通过[Linkedin](https://www.linkedin.com/in/hugozanini/?locale=en_US)或[Twitter](https://twitter.com/hugoznn)联系我。本文中使用的所有源代码均可在[项目仓库](https://github.com/hugozanini/yolov7-tfjs/tree/stockout)中找到。

谢谢阅读 :)
