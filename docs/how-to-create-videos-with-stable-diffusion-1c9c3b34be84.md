# 如何使用稳定扩散和 Deform 创建插值视频

> 原文：[`towardsdatascience.com/how-to-create-videos-with-stable-diffusion-1c9c3b34be84`](https://towardsdatascience.com/how-to-create-videos-with-stable-diffusion-1c9c3b34be84)

## 生成式人工智能

## 使用帧插值通过稳定扩散和 Deforum 创建视频

[](https://medium.com/@oscarleo?source=post_page-----1c9c3b34be84--------------------------------)![Oscar Leo](https://medium.com/@oscarleo?source=post_page-----1c9c3b34be84--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1c9c3b34be84--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c9c3b34be84--------------------------------) [Oscar Leo](https://medium.com/@oscarleo?source=post_page-----1c9c3b34be84--------------------------------)

·发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c9c3b34be84--------------------------------) ·6 分钟阅读·2023 年 2 月 17 日

--

![](img/10a84d5f8ec2a71a26f5a620cabc9241.png)

由作者使用[DeForum](https://github.com/deforum-art/deforum-stable-diffusion)生成的图像

# 介绍

无论你走到哪里，你都会看到由稳定扩散和 Midjourney 等算法生成的图像。然而，视频却是一个更加具有挑战性的前景。

我与一家媒体制作公司合作，人工智能视频在成为行业广泛使用的工具之前还有很长的路要走。质量尚远未达到要求。

## 挑战

存在一些挑战，比如帧之间的闪烁、分辨率和计算等。

我还应该提到有关使用互联网内容训练算法的版权问题的持续争论。很明显，目前的做法过于宽松。我将非常关注这一进展。

## 当前使用案例

然而，媒体制作完全是关于创造引人入胜和独特的内容，而人工智能无疑可以成为其中的一部分。

一个使用案例是给原始视频添加不同的风格，就像他们在[乌鸦](https://www.youtube.com/watch?v=5dvxY6vXHsA)中做的那样，他们首先录制了一位舞者，然后让算法调整每一帧。

我们将不断看到这一主题的进展，但在这篇文章中，我会展示一种简单的方法来让你入门。我们将通过使用[DeForum](https://github.com/deforum-art/deforum-stable-diffusion)对图像进行插值来创建一个短视频。

让我们开始吧！:)

# 步骤 1：设置 VastAI

要使用 Deforum，你需要一个 GPU。如果你的计算机上没有 GPU，最简单且最便宜的替代方案是[vast.ai](https://vast.ai/)。

## 实例配置

首先，你需要选择使用哪个图像。我会选择 pytorch/pytorch 版本“1.13.1-cuda11.6-cudnn8-devel”。这不是唯一正确的选择，但它有效。

![](img/17372a05e72c7d9ef587a1b2e5b1b0bd.png)

对于启动模式，我选择最简单的选项：标准 Jupyter 笔记本。

![](img/1a6e331741dbdfd171454fcd03854e0a.png)

最后一个需要更改的配置是增加磁盘空间，因为模型非常庞大。

## 选择一个 GPU

接下来，选择一个 GPU。虽然有成千上万的选项，但标准的 3090 就很好。确保上传和下载速度都不错。

![](img/a6039087b76c5121dac399762e26d1c6.png)

价格应该在每小时$0.4 左右，但如果你想要更好的 GPU，可以多花点钱。

# 步骤 2：安装 Deforum

Deforum 在他们的 GitHub 上解释了安装过程，但我也会在这里写下来，以使本教程更易于跟随。

## 2.1：打开实例

要打开你的实例，请前往左侧菜单中的“实例”，然后点击右侧的蓝色“打开按钮”。

![](img/e20274fb75bc0965c908b87fde9014a4.png)

## 2.2：打开终端

进入后，点击“新建”并选择终端选项以打开一个终端。

![](img/9c9dbf9c3df85a38a14f3c4992491941.png)

运行以下两个命令以安装 conda，并关闭终端以使更改生效。

```py
conda create -n dsd python=3.10 -y
conda init
```

打开一个新的终端，就像之前一样，并激活 conda 环境。

```py
conda activate dsd
```

## 2.3：安装所有内容

通过运行以下命令安装所有内容：

```py
git clone https://github.com/deforum-art/deforum-stable-diffusion.git
cd deforum-stable-diffusion
python install_requirements.py

# Add conda environment to jupyter
conda install -c anaconda ipykernel
python -m ipykernel install --user --name=firstEnv
```

几分钟后，你可以通过运行以下命令测试一切是否正常：

```py
python Deforum_Stable_Diffusion.py
```

# 步骤 3：创建视频

## 3.1 打开笔记本

当一切安装完毕并正常工作后，关闭终端，然后打开 deforum-stable-diffusion 文件夹中的“Deforum_Stable_Diffusion.ipynb”。

![](img/d9b3fbc64b012cdc98120857f405f233.png)

将内核更改为 dsd 并运行前三个单元。第三个单元可能需要一点时间来完成。

![](img/32ede2f8c1bfdb9eddb29a5011e2d75f.png)

## 3.2 选择一个起始图像 [可选]

如果你愿意，可以从原始图像开始。我将使用以下来自[Pexels](https://www.pexels.com/)的图像。

![](img/444df0de9a45529c29099c2fc1d4b907.png)

照片由 Nout Gons 拍摄：[`www.pexels.com/photo/city-street-photo-378570/`](https://www.pexels.com/photo/city-street-photo-378570/)

你需要确保图像的大小合适。否则，它将无法适应 RAM。因此，我将此图像的大小调整为 801x512（Deforum 会将边缘裁剪为 768x512）。

要上传图像，点击上传，并将其放置在合适的位置。例如，我将其放在/deforum-stable-diffusion 下。

![](img/2ba45648d68e246a783e95b8c641e41d.png)

## 3.3 Deforum 设置

一旦你打开 Notebook，你会看到可以调整的设置有很多。遗憾的是，由于许多设置仅与之前的选择相关，这个界面并不完美。

**注意：** 单独的设置可能很难找到，并且它们的顺序可能与我写的不一致。最好使用 ctrl+F 或 cmd+F 搜索位置。

首先，我们将**animation_mode**更改为**Interpolation**，并将**interpolate_x_frames**设置为一些合理的值。**interpolate_x_frames**决定算法应在你的关键帧之间生成多少帧。

```py
animation_mode = 'Interpolation'
interpolate_x_frames = 16
```

接下来，我们通过更改**animation_prompts**来创建我们的提示。你看到的左边的数字对于**Interpolation**并不重要。

对于其他使用场景，这个数字决定何时开始使用特定的提示，但对于插值（Interpolation），这些数字成为我们的关键帧，并且我们在每两个关键帧之间生成**interpolate_x_frames**帧。

这是我的提示，但你可以写你想要的任何内容。

```py
animation_prompts = {
    0: "a dystopian warzone with zombies, dark clouds in the sky, night",
    1: "a dystopian warzone, dark clouds in the sky",
    2: "a dystopian city, clouds in the sky",
    3: "a city",
    4: "a beautiful city",
    5: "a beautiful and modern city with blue sky",
    6: "a beautiful and modern city with blue sky and green trees"
}
```

如果你不使用原始图像，继续进行**3.4**；否则，请按照下面的说明操作。

现在，我们将**W**和**H**更改为我们选择的图像的尺寸。在我的例子中，这是 801x512。

```py
W = 801 #@param
H = 512 #@param
```

要使用图像，我们还需要更改以下设置：

```py
use_init = True #@param {type:"boolean"}
strength = 0.6 #@param {type:"number"}
init_image = "city-street.jpg" #@param {type:"string"}
```

**Strength**决定保留原始图像的程度。接近 1 的数字意味着我们将保留大部分原始图像。**Init_image**当然应该指向你的图像的位置。

## **3.4 生成图像**

运行单元格 3、4 和 5。首先，算法将根据你在**animation_prompts**中的提示生成关键帧。

当关键帧完成后，它将继续创建中间帧。如果你的**animation_prompts**中有很多提示和一个大的**interpolate_x_frames**，这可能需要一些时间。

## **3.5 创建视频**

在你的笔记本底部创建一个新单元，并粘贴以下代码。然后运行该单元以创建你的视频。

```py
import cv2

image_files = os.listdir(args.outdir)
image_files = ["{}/{}".format(args.outdir, i) for i in image_files if '.png' in i]
image_files = [i for i in image_files if args.timestring in i]

out = cv2.VideoWriter('video.avi',cv2.VideoWriter_fourcc(*'DIVX'), 15, (args.W, args.H))

for i in range(len(image_files)):
    out.write(cv2.imread(image_files[i]))
out.release()
```

这是我的视频的样子：

这是另一个例子：

这就是我的教程；祝你编码愉快！如果你创建了有趣的内容，请分享。

感谢阅读！:)
