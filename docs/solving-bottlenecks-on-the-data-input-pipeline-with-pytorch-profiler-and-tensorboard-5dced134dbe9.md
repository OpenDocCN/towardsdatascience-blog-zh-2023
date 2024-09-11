# 使用 PyTorch Profiler 和 TensorBoard 解决数据输入管道瓶颈

> 原文：[https://towardsdatascience.com/solving-bottlenecks-on-the-data-input-pipeline-with-pytorch-profiler-and-tensorboard-5dced134dbe9?source=collection_archive---------2-----------------------#2023-08-26](https://towardsdatascience.com/solving-bottlenecks-on-the-data-input-pipeline-with-pytorch-profiler-and-tensorboard-5dced134dbe9?source=collection_archive---------2-----------------------#2023-08-26)

## PyTorch 模型性能分析与优化——第四部分

[](https://chaimrand.medium.com/?source=post_page-----5dced134dbe9--------------------------------)[![Chaim Rand](../Images/c52659c389f167ad5d6dc139940e7955.png)](https://chaimrand.medium.com/?source=post_page-----5dced134dbe9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5dced134dbe9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5dced134dbe9--------------------------------) [Chaim Rand](https://chaimrand.medium.com/?source=post_page-----5dced134dbe9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9440b37e27fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-bottlenecks-on-the-data-input-pipeline-with-pytorch-profiler-and-tensorboard-5dced134dbe9&user=Chaim+Rand&userId=9440b37e27fe&source=post_page-9440b37e27fe----5dced134dbe9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5dced134dbe9--------------------------------) ·11分钟阅读·2023年8月26日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5dced134dbe9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-bottlenecks-on-the-data-input-pipeline-with-pytorch-profiler-and-tensorboard-5dced134dbe9&source=-----5dced134dbe9---------------------bookmark_footer-----------)![](../Images/6d1d0873c895da273752a2327c322b7c.png)

图片来源：[Alexander Grey](https://unsplash.com/@sharonmccutcheon?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这是我们关于 GPU 基于 PyTorch 工作负载的性能分析和优化系列文章中的第四篇。我们将在这篇文章中重点关注训练**数据输入管道**。在典型的训练应用中，主机的 CPU 加载、预处理和整理数据，然后将其送入 GPU 进行训练。当主机无法跟上 GPU 的速度时，输入管道中会出现瓶颈。这会导致 GPU —— 训练设置中*最昂贵*的资源——在等待来自*过度负荷*主机的数据输入时闲置。在之前的文章中（例如，[这里](/overcoming-data-preprocessing-bottlenecks-with-tensorflow-data-service-nvidia-dali-and-other-d6321917f851)），我们详细讨论了输入管道瓶颈，并回顾了应对这些瓶颈的不同方法，例如：

1.  选择一个 CPU 到 GPU 计算比率更适合你的工作负载的训练实例（例如，参见我们 [之前的帖子](/instance-selection-for-deep-learning-7463d774cff0) 关于选择最适合你的 ML 工作负载的实例类型的提示），

1.  通过将一些 CPU 预处理活动转移到 GPU 来改善 CPU 和 GPU 之间的工作负载平衡，并

1.  将部分 CPU 计算任务转移到辅助 CPU 工作设备（例如，参见 [这里](/overcoming-ml-data-preprocessing-bottlenecks-with-grpc-ca30fdc01bee)）。

当然，**解决数据输入管道中的性能瓶颈的第一步是识别并理解它**。在这篇文章中，我们将演示如何使用 [PyTorch Profiler](https://pytorch.org/tutorials/recipes/recipes/profiler_recipe.html) 及其相关的 [TensorBoard 插件](https://pytorch.org/tutorials/intermediate/tensorboard_profiler_tutorial.html) 来实现这一点。

正如我们之前的帖子中所述，我们将定义一个玩具 PyTorch 模型，然后*迭代地*分析其性能，识别瓶颈，并尝试解决它们。我们将在一个 [Amazon EC2 g5.2xlarge](https://aws.amazon.com/ec2/instance-types/g5/) 实例上运行我们的实验（该实例包含 NVIDIA A10G GPU 和 8 个 vCPUs），并使用官方的 [AWS PyTorch 2.0 Docker 镜像](https://github.com/aws/deep-learning-containers)。请记住，我们描述的某些行为可能因 PyTorch 版本不同而有所变化。

感谢 [Yitzhak Levi](https://www.linkedin.com/in/yitzhak-levi-49a217201/) 对这篇文章的贡献。

# 玩具模型

在接下来的部分中，我们介绍了我们将用于演示的玩具示例。我们首先定义了一个简单的图像分类模型。模型的输入是一个*256*x*256*的 YUV 图像批次，输出是其相关的语义类别预测批次。

```py
from math import log2
import torch
import torch.nn as nn
import torch.nn.functional as F

img_size = 256
num_classes = 10
hidden_size = 30

# toy CNN classification model
class Net(nn.Module):
    def __init__(self, img_size=img_size, num_classes=num_classes):
        super().__init__()
        self.conv_in = nn.Conv2d(3, hidden_size, 3, padding='same')
        num_hidden = int(log2(img_size))
        hidden = []
        for i in range(num_hidden):
            hidden.append(nn.Conv2d(hidden_size, hidden_size, 3, padding='same'))
            hidden.append(nn.ReLU())
            hidden.append(nn.MaxPool2d(2))
        self.hidden = nn.Sequential(*hidden)
        self.conv_out = nn.Conv2d(hidden_size, num_classes, 3, padding='same')

    def forward(self, x):
        x = F.relu(self.conv_in(x))
        x = self.hidden(x)
        x = self.conv_out(x)
        x = torch.flatten(x, 1)
        return x
```

下面的代码块包含我们的数据集定义。我们的数据集包含一万张 JPEG 图像的文件路径及其相关（随机生成的）语义标签。为了简化演示，我们将假设所有 JPEG 文件路径指向相同的图像——本文顶部的多彩“瓶颈”图片。

```py
import numpy as np
from PIL import Image
from torchvision.datasets.vision import VisionDataset
input_img_size = [533, 800]
class FakeDataset(VisionDataset):
    def __init__(self, transform):
        super().__init__(root=None, transform=transform)
        size = 10000
        self.img_files = [f'0.jpg' for i in range(size)]
        self.targets = np.random.randint(low=0,high=num_classes,
                                         size=(size),dtype=np.uint8).tolist()

    def __getitem__(self, index):
        img_file, target = self.img_files[index], self.targets[index]
        with torch.profiler.record_function('PIL open'):
            img = Image.open(img_file)
        if self.transform is not None:
            img = self.transform(img)
        return img, target

    def __len__(self):
        return len(self.img_files)
```

请注意，我们已经使用 [torch.profiler.record_function](https://pytorch.org/tutorials/beginner/profiler.html#performance-debugging-using-profiler) 上下文管理器包装了文件读取器。

我们的输入数据管道包括对图像进行以下转换：

1.  [PILToTensor](https://pytorch.org/vision/main/generated/torchvision.transforms.PILToTensor.html) 将 PIL 图像转换为 PyTorch Tensor。

1.  [RandomCrop](https://pytorch.org/vision/0.8/transforms.html#torchvision.transforms.RandomCrop) 在图像的随机偏移处返回一个 *256*x*256* 的裁剪区域。

1.  RandomMask 是一种自定义转换，创建一个随机的 *256*x*256* 布尔掩码并将其应用于图像。该转换包括对掩码的四邻域膨胀操作。

1.  ConvertColor 是一种自定义转换，将图像格式从 RGB 转换为 YUV。

1.  Scale 是一种自定义转换，将像素缩放到 [0,1] 范围内。

```py
class RandomMask(torch.nn.Module):
    def __init__(self, ratio=0.25):
        super().__init__()
        self.ratio=ratio

    def dilate_mask(self, mask):
        # perform 4 neighbor dilation on mask
        with torch.profiler.record_function('dilation'):
            from scipy.signal import convolve2d
            dilated = convolve2d(mask, [[0, 1, 0],
                                     [1, 1, 1],
                                     [0, 1, 0]], mode='same').astype(bool)
        return dilated

    def forward(self, img):
        with torch.profiler.record_function('random'):
            mask = np.random.uniform(size=(img_size, img_size)) < self.ratio
        dilated_mask = torch.unsqueeze(torch.tensor(self.dilate_mask(mask)),0)
        dilated_mask = dilated_mask.expand(3,-1,-1)
        img[dilated_mask] = 0.
        return img

    def __repr__(self):
        return f"{self.__class__.__name__}(ratio={self.ratio})"

class ConvertColor(torch.nn.Module):
    def __init__(self):
        super().__init__()
        self.A=torch.tensor(
            [[0.299, 0.587, 0.114],
             [-0.16874, -0.33126, 0.5],
             [0.5, -0.41869, -0.08131]]
        )
        self.b=torch.tensor([0.,128.,128.])

    def forward(self, img):
        img = img.to(dtype=torch.get_default_dtype())
        img = torch.matmul(self.A,img.view([3,-1])).view(img.shape)
        img = img + self.b[:,None,None]
        return img

    def __repr__(self):
        return f"{self.__class__.__name__}()"

class Scale(object):
    def __call__(self, img):
        return img.to(dtype=torch.get_default_dtype()).div(255)

    def __repr__(self):
        return f"{self.__class__.__name__}()"
```

我们使用 [Compose](https://pytorch.org/vision/0.8/transforms.html#torchvision.transforms.Compose) 类链接这些转换，我们稍微修改了它，以在每个转换调用周围包含一个 [torch.profiler.record_function](https://pytorch.org/tutorials/beginner/profiler.html#performance-debugging-using-profiler) 上下文管理器。

```py
import torchvision.transforms as T
class CustomCompose(T.Compose):
    def __call__(self, img):
        for t in self.transforms:
            with torch.profiler.record_function(t.__class__.__name__):
                img = t(img)
        return img

transform = CustomCompose(
    [T.PILToTensor(),
     T.RandomCrop(img_size),
     RandomMask(),
     ConvertColor(),
     Scale()])
```

在下面的代码块中，我们定义了数据集和数据加载器。我们将 [DataLoader](https://pytorch.org/docs/stable/data.html) 配置为使用 [自定义 collate 函数](https://pytorch.org/docs/stable/data.html#working-with-collate-fn)，在其中我们使用 [torch.profiler.record_function](https://pytorch.org/tutorials/beginner/profiler.html#performance-debugging-using-profiler) 上下文管理器包装 [默认 collate 函数](https://pytorch.org/docs/stable/data.html#torch.utils.data.default_collate)。

```py
train_set = FakeDataset(transform=transform)

def custom_collate(batch):
    from torch.utils.data._utils.collate import default_collate
    with torch.profiler.record_function('collate'):
        batch = default_collate(batch)
    image, label = batch
    return image, label

train_loader = torch.utils.data.DataLoader(train_set, batch_size=256,
                                           collate_fn=custom_collate,
                                           num_workers=4, pin_memory=True)
```

最后，我们定义了模型、损失函数、优化器和训练循环，并用 [profiler 上下文管理器](https://pytorch.org/docs/stable/profiler.html#torch.profiler.profile) 包装。

```py
from statistics import mean, variance
from time import time

device = torch.device("cuda:0")
model = Net().cuda(device)
criterion = nn.CrossEntropyLoss().cuda(device)
optimizer = torch.optim.SGD(model.parameters(), lr=0.001, momentum=0.9)
model.train()

t0 = time()
times = []

with torch.profiler.profile(
    schedule=torch.profiler.schedule(wait=10, warmup=2, active=10, repeat=1),
    on_trace_ready=torch.profiler.tensorboard_trace_handler('/tmp/prof'),
    record_shapes=True,
    profile_memory=True,
    with_stack=True
) as prof:
    for step, data in enumerate(train_loader):
        with torch.profiler.record_function('h2d copy'):
            inputs, labels = data[0].to(device=device, non_blocking=True), \
                             data[1].to(device=device, non_blocking=True)
        if step >= 40:
            break
        outputs = model(inputs)
        loss = criterion(outputs, labels)
        optimizer.zero_grad(set_to_none=True)
        loss.backward()
        optimizer.step()
        prof.step()
        times.append(time()-t0)
        t0 = time()

print(f'average time: {mean(times[1:])}, variance: {variance(times[1:])}')
```

在接下来的部分中，我们将使用 [PyTorch Profiler](https://pytorch.org/tutorials/recipes/recipes/profiler_recipe.html) 及其相关的 [TensorBoard 插件](https://pytorch.org/tutorials/intermediate/tensorboard_profiler_tutorial.html) 来评估我们模型的性能。我们的重点将是分析器报告的 *Trace View*。有关如何使用报告的其他部分，请参阅我们系列中的 [第一篇文章](/pytorch-model-performance-analysis-and-optimization-10c3c5822869)。

# 初步性能结果

我们定义的脚本报告的平均步长时间为 1.3 秒，平均 GPU 利用率非常低，为 18.21%。在下面的图像中，我们捕捉了在 [TensorBoard 插件](https://pytorch.org/tutorials/intermediate/tensorboard_profiler_tutorial.html) *跟踪视图* 中显示的性能结果。

![](../Images/d88379983c412418b10a9b62114465e9.png)

基线模型的跟踪视图（作者拍摄）

我们可以看到每隔四步训练就会出现一个较长的 (~5.5 秒) 数据加载期间，在此期间GPU完全空闲。这种现象发生在每隔四步的原因与我们选择的DataLoader工作线程数量—四个—直接相关。每隔四步，我们发现所有工作线程都在忙于生成下一批次的样本，而GPU则在等待。这清楚地表明数据输入管道中存在瓶颈。问题是我们该如何分析它？使问题复杂化的是，我们插入代码中的许多 [record_function](https://pytorch.org/tutorials/beginner/profiler.html#performance-debugging-using-profiler) 标记在性能分析轨迹中找不到。

在DataLoader中使用多个工作线程对优化性能**至关重要**。不幸的是，这也使得性能分析过程变得更加困难。虽然存在支持多进程分析的性能分析工具（例如，查看 [VizTracer](https://viztracer.readthedocs.io/en/latest/concurrency.html)），但我们在本文中采用的方法是以**单进程模式**（即，零DataLoader工作线程）运行、分析和优化我们的模型，然后将优化应用到**多工作线程模式**。诚然，优化一个独立函数的速度并不能保证多个（并行）调用相同函数也会有同样的效果。然而，正如我们在本文中将看到的，这一策略将帮助我们识别并解决一些我们*无法*通过其他方式识别的核心问题，并且，至少在这里讨论的问题方面，我们*将*发现这两种模式的性能影响之间有很强的关联。但在应用这一策略之前，让我们调整工作线程的数量选择。

# 优化 1：调整多进程策略

确定多进程/多线程应用程序（例如我们的应用程序）中的最佳线程或进程数量可能会很棘手。一方面，如果我们选择的数量太少，可能会导致CPU资源未被充分利用。另一方面，如果数量设置得过高，我们就有可能面临*thrashing*的风险，这是一个不希望出现的情况，操作系统花费大部分时间来管理多个线程/进程，而不是运行我们的代码。在PyTorch训练工作负载的情况下，建议尝试不同的DataLoader *num_workers*设置。一个好的起点是根据主机上的CPU数量来设置这个数字（例如，*num_workers*:=*num_cpus/num_gpus*）。在我们的案例中，[Amazon EC2 g5.2xlarge](https://aws.amazon.com/ec2/instance-types/g5/)有八个vCPUs，实际上，将DataLoader的工作线程数量增加到八个会导致稍微更好的平均步骤时间为1.17秒（提升11%）。

重要的是，要留意其他不太明显的配置设置，这些设置可能会影响数据输入管道使用的线程或进程数量。例如，[opencv-python](https://pypi.org/project/opencv-python/)，这是一个常用于计算机视觉工作负载中的图像预处理的库，其中包括用于控制线程数量的[cv2.setNumThreads(int)](https://medium.com/@rachittayal7/a-note-on-opencv-threads-performance-in-prod-d10180716fba)函数。

在下面的图像中，我们捕捉到在将*num_workers*设置为**零**时运行脚本的*Trace View*的一部分。

![](../Images/535011e9c86417421f028e452be9a593.png)

单进程模式下基线模型的跟踪视图（作者拍摄）

以这种方式运行脚本使我们能够看到我们设置的*record_function*标签，并识别出*RandomMask*变换，或者更具体地说，我们的*dilation*函数，作为检索每个单独样本时最耗时的操作。

# 优化2：优化膨胀函数

我们当前的膨胀函数实现使用2D卷积，通常使用矩阵乘法实现，并且在CPU上运行速度不是特别快。一个选项是将膨胀操作在GPU上运行（如[这篇文章](/overcoming-data-preprocessing-bottlenecks-with-tensorflow-data-service-nvidia-dali-and-other-d6321917f851)中所述）。然而，主机与设备之间的事务开销可能会超过这种解决方案带来的潜在性能提升，更不用说我们不愿增加GPU的负担。

在下面的代码块中，我们提出了一种更为CPU友好的膨胀函数实现，它使用布尔操作而不是卷积：

```py
 def dilate_mask(self, mask):
        # perform 4 neighbor dilation on mask
        with torch.profiler.record_function('dilation'):
            padded = np.pad(mask, [(1,1),(1,1)])
            dilated = padded[0:-2,1:-1] | padded[1:-1,1:-1] | padded[2:,1:-1] | padded[1:-1,0:-2]| padded[1:-1,2:]
        return dilated
```

经过这种修改后，我们的步骤时间降至0.78秒，相当于额外提高了50%。更新后的单进程*Trace-View*如下所示：

![](../Images/5ec0eeb29fc63b3c03de5b1c9fd86dd4.png)

单进程模式下膨胀优化后的跟踪视图（作者拍摄）

我们可以看到，膨胀操作已经显著缩小，现在最耗时的操作是[PILToTensor](https://pytorch.org/vision/main/generated/torchvision.transforms.PILToTensor.html)变换。

更详细地查看[PILToTensor](https://pytorch.org/vision/main/generated/torchvision.transforms.PILToTensor.html)函数（见[这里](https://pytorch.org/vision/stable/_modules/torchvision/transforms/functional.html#pil_to_tensor)）可以发现三个潜在的操作：

1.  由于[Image.open](https://pillow.readthedocs.io/en/latest/reference/Image.html#PIL.Image.open)的延迟加载特性，PIL图像在此处被加载。

1.  PIL图像被转换为numpy数组。

1.  numpy数组被转换为PyTorch Tensor。

尽管图像加载占用了大部分时间，但我们注意到对全尺寸图像进行随后的操作并立即裁剪的极端浪费。这引出了我们下一步的优化。

# 优化 3：重排变换

幸运的是，[RandomCrop](https://pytorch.org/vision/0.8/transforms.html#torchvision.transforms.RandomCrop)变换可以直接应用于PIL图像，使我们能够将图像尺寸缩减作为管道中的第一个操作：

```py
transform = CustomCompose(
    [T.RandomCrop(img_size),
     T.PILToTensor(),
     RandomMask(),
     ConvertColor(),
     Scale()])
```

在这一优化之后，我们的步骤时间降至0.72秒，额外的8%优化。下图中的*Trace View*捕获显示随机裁剪变换现在是主要操作：

![](../Images/f327927acfd400caac441940988a887a.png)

单进程模式下变换重排后的跟踪视图（作者拍摄）

实际上，与之前一样，瓶颈实际上是PIL图像（延迟）加载，而不是随机裁剪。

理想情况下，我们可以通过将读取操作限制为仅对感兴趣的裁剪区域进行优化。不幸的是，截止到本文撰写时，[torchvision](https://pytorch.org/vision/stable/index.html)不支持此选项。在未来的帖子中，我们将展示如何通过实现我们自己的[custom](https://pytorch.org/tutorials/advanced/cpp_extension.html) *decode_and_crop* PyTorch操作符来克服这一不足。

# 优化 4：应用批处理变换

在我们当前的实现中，每个图像变换都是对每个图像单独应用的。然而，当一次性对整个批量应用某些变换时，它们可能会运行得更优。下面的代码块中，我们修改了我们的管道，以便在我们的[custom collate function](https://pytorch.org/docs/stable/data.html#working-with-collate-fn)中对图像**批量**应用ColorTransformation和Scale变换：

```py
def batch_transform(img):
    img = img.to(dtype=torch.get_default_dtype())
    A = torch.tensor(
        [[0.299, 0.587, 0.114],
         [-0.16874, -0.33126, 0.5],
         [0.5, -0.41869, -0.08131]]
    )
    b = torch.tensor([0., 128., 128.])

    A = torch.broadcast_to(A, ([img.shape[0],3,3]))
    t_img = torch.bmm(A,img.view(img.shape[0],3,-1))
    t_img = t_img + b[None,:, None]
    return t_img.view(img.shape)/255

def custom_collate(batch):
    from torch.utils.data._utils.collate import default_collate
    with torch.profiler.record_function('collate'):
        batch = default_collate(batch)

    image, label = batch
    with torch.profiler.record_function('batch_transform'):
        image = batch_transform(image)
    return image, label
```

这个变化的结果实际上是步骤时间略微增加，达到0.75秒。虽然对于我们的玩具模型没有帮助，但将某些操作应用为批量变换而不是每个样本变换的能力，具有优化特定工作负载的潜力。

# 结果

我们在这篇文章中应用的连续优化使得运行时性能提高了80%。然而，尽管改进的效果不那么显著，输入管道仍存在瓶颈，而且GPU的利用率依然较低（约30%）。请参考我们之前的文章（例如，[这里](/overcoming-data-preprocessing-bottlenecks-with-tensorflow-data-service-nvidia-dali-and-other-d6321917f851)）以获取解决此类问题的额外方法。

# 总结

在这篇文章中，我们关注了训练数据输入管道中的性能问题。正如我们在本系列的之前文章中所做的，我们选择了PyTorch Profiler及其关联的TensorBoard插件作为我们首选的工具，并展示了它们在加速训练速度方面的使用。特别是，我们展示了如何通过使用零工作线程运行DataLoader，来提高识别、分析和优化数据输入管道瓶颈的能力。

正如我们在之前的文章中强调的，成功优化的路径将根据训练项目的细节，包括模型架构和训练环境，存在很大差异。实际上，实现你的目标可能比我们这里展示的例子更为困难。我们描述的一些技术可能对你的性能影响不大，甚至可能使其变得更差。我们还注意到，我们选择的具体优化及其应用顺序有些任意。我们强烈鼓励你根据项目的具体细节开发自己的工具和技术来实现优化目标。
