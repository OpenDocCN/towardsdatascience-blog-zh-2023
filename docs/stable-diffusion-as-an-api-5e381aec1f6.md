# Stable Diffusion 作为 API：创建一个去除人物的微服务

> 原文：[`towardsdatascience.com/stable-diffusion-as-an-api-5e381aec1f6`](https://towardsdatascience.com/stable-diffusion-as-an-api-5e381aec1f6)

![](img/a964e08268efee5aaf0f29c6237487db.png)

使用 Stable Diffusion 2 生成的风景图像（作者提供）。

## 使用 Stable Diffusion 微服务从照片中去除人物

[](https://medium.com/@masonmcgough?source=post_page-----5e381aec1f6--------------------------------)![Mason McGough](https://medium.com/@masonmcgough?source=post_page-----5e381aec1f6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5e381aec1f6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5e381aec1f6--------------------------------) [Mason McGough](https://medium.com/@masonmcgough?source=post_page-----5e381aec1f6--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5e381aec1f6--------------------------------) ·阅读时间 12 分钟·2023 年 2 月 4 日

--

# 概述

Stable Diffusion 是一个前沿的开源工具，用于从文本生成图像。Stable Diffusion Web UI 通过 API 以及交互式用户界面提供了许多这些功能。我们将首先介绍如何使用这个 API，然后设置一个示例，将其作为一个隐私保护微服务来从图像中去除人物。

# 生成 AI 介绍

去年，基于机器学习的数据生成器出现了许多创新，2022 年或许可以称为“生成 AI 的年度”。我们迎来了 [DALL-E 2](https://openai.com/dall-e-2/)，这是 OpenAI 的文本到图像生成模型，生成了令人惊叹的现实主义图像，如宇航员骑马和穿着人类衣物的狗。 [GitHub Copilot](https://github.blog/2022-06-21-github-copilot-is-generally-available-to-all-developers/)，这个强大的代码补全工具可以自动完成语句、编写文档，并根据单个评论实现完整的功能，已经作为订阅服务公开发布。我们还看到了 [Dream Fields](https://www.ajayjain.net/dreamfields/)、[Dream Fusion](https://dreamfusion3d.github.io/) 和 [Magic3D](https://research.nvidia.com/labs/dir/magic3d/)，一系列能够仅通过文本生成纹理化 3D 模型的突破性模型。最后但同样重要的是我们迎来了 [ChatGPT](https://openai.com/blog/chatgpt/)，这个前沿的 AI 聊天机器人如今无需介绍。

这个列表几乎只是触及了表面。在像 DALL-E 2 这样的生成图像模型世界中，我们还有[Midjourney](https://www.midjourney.com/)、[Google Imagen](https://imagen.research.google/)、[StarryAI](https://starryai.com/)、[WOMBO Dream](https://dream.ai/create)、[NightCafe](https://nightcafe.studio/)、[InvokeAI](https://invoke-ai.github.io/InvokeAI/)、[Lexica Aperture](https://lexica.art/aperture)、[Dream Studio](https://beta.dreamstudio.ai/)、[Deforum](https://deforum.github.io/)……我想你已经明白了*画面*。😉 📷 说生成 AI 已经捕捉了整个世界的想象力，似乎并不夸张。

# 稳定扩散

尽管许多流行的生成 AI 工具如 ChatGPT、GitHub Copilot 和 DALL-E 2 是专有的且需付费的，但开源社区并没有停下脚步。去年，LMU 慕尼黑大学、Runway 和 Stability AI 合作公开分享了[Stable Diffusion](https://github.com/CompVis/stable-diffusion)，这是一个强大但高效的文本到图像模型，足够高效以在消费级硬件上运行。这意味着任何拥有一台不错 GPU 和互联网连接的人都可以下载 Stable Diffusion 代码和模型权重，将低成本图像生成带给世界。

# 稳定扩散网络界面

[Stable Diffusion Web UI](https://github.com/AUTOMATIC1111/stable-diffusion-webui)是利用 Stable Diffusion 的最受欢迎工具之一，它在基于浏览器的用户界面中暴露了 Stable Diffusion 的广泛设置和功能。该项目一个鲜为人知的功能是，你可以将其作为 HTTP API 使用，允许你从自己的应用程序中请求图像。

![](img/b361ece7b8424e89b5c7e22e9230c880.png)

Stable Diffusion Web UI 的示例生成（作者提供的照片）。

它具有大量功能，如修补、扩展、调整大小、放大、变体等。 [项目 wiki](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features) 提供了所有功能的详细概述。此外，它还提供了扩展性脚本。

## 设置

在开始之前，请确保你的系统中有一个 GPU（最好是 NVIDIA，但 AMD 也支持），且至少有 8GB 的显存。这将确保你可以将模型加载到内存中。接下来，你需要将代码库克隆到你的系统中（例如通过 HTTPS）：

```py
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git
```

根据你的系统遵循[安装说明](https://github.com/AUTOMATIC1111/stable-diffusion-webui#installation-and-running)，因为这些可能与你的有所不同。我使用的是 Ubuntu 18.04 进行设置，但它也应该适用于 Windows 和 Apple Silicon。这些说明将包括设置 Python 环境，因此请确保在稍后启动服务器时，所设置的环境处于活动状态。

完成后，我们需要一份模型权重的副本。我使用的是 [Stable Diffusion 2.0](https://huggingface.co/stabilityai/stable-diffusion-2)，但现在也有 [Stable Diffusion 2.1](https://huggingface.co/stabilityai/stable-diffusion-2-1) 可用。无论你选择哪个选项，确保下载 [stablediffusion](https://github.com/Stability-AI/stablediffusion) 仓库的权重。最后，将这些权重复制到 `models/Stable-diffusion` 文件夹中，如下所示：

```py
cp 768-v-ema.ckpt models/Stable-diffusion
```

现在你应该准备好开始生成图像了！要启动服务器，请在根目录下执行以下操作（确保你设置的环境已激活）：

```py
python launch.py
```

服务器需要一些时间来设置，因为它可能需要安装要求、将模型权重加载到内存中、检查嵌入等。准备好后，你应该会在终端中看到类似下面的消息：

```py
Running on local URL:  http://127.0.0.1:7860
```

这个用户界面是基于浏览器的，因此在你喜欢的网页浏览器中导航到 “[127.0.0.1:7860](http://127.0.0.1:7860)” 。如果它正常工作，它应该看起来像这样：

![](img/3ea02f9b2002eee7e0ce6eedd81f3629.png)

稳定扩散 Web 用户界面首次打开时（作者拍摄）。

## 使用

现在你应该准备好生成一些图像了！进入“提示”字段中输入文本并点击“生成”以生成一些内容。如果这是你第一次使用这个用户界面，花点时间探索并了解其一些功能和设置。如果有任何问题，请参考 [维基](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki)。这些知识在设计你的 API 时会很有用。

我不会过于深入地讲解如何使用网页用户界面，因为之前有很多人已经做了。不过，我会提供以下基本设置的备忘单以供参考。

+   **采样方法**：采样算法。这会极大地影响生成图像的内容和整体外观。不同方法的执行时间和结果可能会有很大差异。理想情况下，首先尝试这个选项。

+   **采样步骤**：图像生成过程中的去噪步骤数。某些结果会随着步骤数量的变化而剧烈改变，而其他结果则会很快出现边际效益递减。大多数采样器理想的步骤数为 20–50。

+   **宽度**，**高度**：输出图像的尺寸。对于 SD 2.0，768x768 是首选分辨率。分辨率会影响生成的内容。

+   **CFG 比例**： [无分类器引导](https://arxiv.org/abs/2207.12598) (CFG) 比例。增加这个比例会增加图像受到提示的影响。较低的值会产生更具创意的结果。

+   **去噪强度**：确定允许原始图像的变化程度。值为 0.0 时没有变化。值为 1.0 时完全忽略原始图像。一般来说，从 0.4–0.6 的值开始是一个安全的选择。

+   **种子**：随机种子值。当你想比较设置效果而尽可能减少变化时，这很有用。如果你喜欢某个特定生成但想稍作修改，请复制种子。

# 将 Stable Diffusion 作为 API 使用

网络用户界面旨在供单个用户使用，并且作为一个互动艺术工具制作自己的创作非常好。然而，如果我们想要使用它作为引擎来构建应用程序，我们将需要一个 API。stable-diffusion-webui 项目的一个鲜为人知（且文档较少）的功能是它还具有内置的 API。网络用户界面是用 [Gradio](https://gradio.app/) 构建的，但也有一个可以用以下命令启动的 FastAPI 应用：

```py
python launch.py --api
```

这为我们提供了一个 API，暴露了我们在网络用户界面中拥有的许多功能。我们可以发送带有提示和参数的 POST 请求，并接收包含输出图像的响应。

# 创建一个微服务

作为示例，我们现在将设置一个简单的微服务，用于从照片中移除人物。这有很多应用，例如保护个人隐私。我们可以使用稳定扩散作为一种原始的隐私保护过滤器，它可以从照片中去除人物而没有任何难看的马赛克或像素块。

请注意，这只是一个基本设置；它不包括加密、负载均衡、多租户、RBAC 或任何其他功能。这个设置可能不适合生产环境，但可以用于在家庭或私人服务器上设置应用程序。

## 以 API 模式启动应用程序

以下说明将使用 API 模式运行服务器，因此请先通过 CTRL+C 停止网络用户界面（web UI）。然后使用 `--api` 选项以 API 模式重新启动它：

```py
python launch.py --api
```

当服务器准备好时，它应该会打印出类似以下内容的信息：

```py
INFO:     Uvicorn running on http://127.0.0.1:7860 (Press CTRL+C to quit)
```

运行服务器而不带用户界面的无头状态也可能很有用。要仅启用 API 而不使用 Gradio 应用：

```py
python launch.py --nowebui
```

## 向 API 发送请求

我们首先要做的是演示如何向 API 发出请求。我们希望向应用程序的 `txt2img`（即“文本到图像”）API 发送 POST 请求，以简单地生成一张图像。

我们将使用 `requests` 包，因此如果你还没有安装，请先安装它：

```py
pip install requests
```

我们可以发送一个包含简单字符串的提示的请求。服务器将返回一个 [base64](https://en.wikipedia.org/wiki/Base64) 编码的 PNG 文件，我们需要对其进行解码。要解码 base64 图像，我们只需使用 `base64.b64decode(b64_image)`。以下脚本应该可以测试这个功能：

```py
import json
import base64

import requests

def submit_post(url: str, data: dict):
    """
    Submit a POST request to the given URL with the given data.
    """
    return requests.post(url, data=json.dumps(data))

def save_encoded_image(b64_image: str, output_path: str):
    """
    Save the given image to the given output path.
    """
    with open(output_path, "wb") as image_file:
        image_file.write(base64.b64decode(b64_image))

if __name__ == '__main__':
    txt2img_url = 'http://127.0.0.1:7860/sdapi/v1/txt2img'
    data = {'prompt': 'a dog wearing a hat'}
    response = submit_post(txt2img_url, data)
    save_encoded_image(response.json()['images'][0], 'dog.png')
```

将内容复制到一个文件中，并将其命名为 `sample-request.py`。现在使用以下命令执行它：

```py
python sample-request.py
```

如果一切正常，它应该会将图像保存为文件 `dog.png`。我的图像看起来像这样这位衣着光鲜的家伙：

![](img/f4ee5a19b1d19b4de18ac8f09681aebb.png)

使用‘sample-request.py’创建的图像（作者拍摄的照片）。

请记住，您的结果可能与我的不同。如果遇到问题，请仔细检查运行稳定扩散应用的终端输出。可能是服务器尚未完成设置。如果遇到“404 Not Found”的问题，请仔细检查 URL 是否正确输入，并指向正确的地址（例如 127.0.0.1）。

## 图像掩膜。

如果到目前为止一切正常，那就太好了！但是我们如何使用它来修改已有的图像呢？为此，我们将使用 `img2img`（即“图像到图像”）API。该 API 使用稳定扩散来修改您提交的图像。我们将使用 [修复功能](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#inpainting)：给定图像和掩膜，修复技术将尝试用稳定扩散生成的内容替换图像中被掩膜遮挡的部分。掩膜作为权重，平滑地插值于原始图像和生成内容之间，将两者融合在一起。

我们将尝试使用许多可用的预训练计算机视觉模型之一生成掩膜，而不是手动制作掩膜。我们将使用模型输出中的“person”类别来生成掩膜。虽然对象检测模型也可以，但我选择使用分割模型，以便您可以尝试使用密集掩膜或边界框。

我们需要一个示例图像进行测试。我们可以从互联网上下载一个，但为了保护隐私（和版权），为什么不使用稳定扩散生成一个呢？以下是我使用提示词“美丽的山地风景，一个女人背对镜头走开”生成的图像。

![](img/a2a07a10abe92d5e95f51db818854298.png)

稳定扩散生成的图像（照片作者提供）。

您可以下载这个，但我鼓励您尝试自己生成一个。当然，您也可以使用真实照片。以下是将 `torchvision` 的库存分割模型应用于此图像作为掩膜的最小代码。

```py
import torch
from torchvision.models.segmentation import fcn_resnet50, FCN_ResNet50_Weights
from torchvision.io.image import read_image
from torchvision.utils import draw_segmentation_masks
import matplotlib.pyplot as plt

if __name__ == '__main__':
    img_path = 'woman-on-trail.png'

    # Load model
    weights = FCN_ResNet50_Weights.DEFAULT
    model = fcn_resnet50(weights=weights, progress=False)
    model = model.eval()

    # Load image
    img = read_image(img_path)

    # Run model
    input_tform = weights.transforms(resize_size=None)
    batch = torch.stack([input_tform(img)])
    output = model(batch)['out']

    # Apply softmax to outputs
    sem_class_to_idx = {cls: idx for (idx, cls) in enumerate(weights.meta['categories'])}
    normalized_mask = torch.nn.functional.softmax(output, dim=1)

    # Show results
    class_idx = 1
    binary_masks = (normalized_mask.argmax(class_idx) == sem_class_to_idx['person'])
    img_masked = draw_segmentation_masks(img, masks=binary_masks, alpha=0.7)
    plt.imshow(img_masked.permute(1, 2, 0).numpy())
    plt.show()
```

像之前一样，将其复制到名为 `segment-person.py` 的文件中。使用以下命令执行代码：

```py
python segment-person.py
```

结果预测应该类似于这个：

![](img/042dc0bd35ef951cfb12daf3ecb788ff.png)

应用到图像上的分割掩膜结果（照片作者提供）。

现在我们有了向 API 发出请求和预测边界框的工具。现在可以开始构建我们的微服务了。

## 人物移除微服务。

现在让我们转到实际示例：从图像中移除人物。微服务应执行以下操作：

1.  读取一些输入参数。

1.  从文件中加载图像。

1.  将“person”类别的分割模型应用于图像以创建掩膜。

1.  将图像和掩膜转换为 base64 编码。

1.  发送一个请求，包含 base64 编码的图像、base64 编码的掩膜、提示词和任何参数到本地服务器的 `img2img` API。

1.  解码并将输出图像保存为文件。

由于我们已经单独涵盖了所有这些步骤，微服务已经为您在 [这个 GitHub Gist](https://gist.github.com/Mason-McGough/9733aff5bc9d04faecfbb81074617315) 中实现了。现在下载脚本并使用以下命令在“woman-on-trail.png”（或您喜欢的任何图片）上执行：

```py
python inpaint-person.py woman-on-trail.png -W 1152 -H 768
```

`-W` 和 `-H` 分别表示所需的输出宽度和高度。它将生成的图像保存为 `inpaint-person.png`，对应的掩模保存为 `mask_inpaint-person.png`。您的输出会有所不同，但这是我收到的结果：

![](img/c2b2c2a0fdc22772c274443559b753a3.png)

使用原始分割掩模的 API 调用结果（图像由作者提供）。

嗯，这不是我们想要的。似乎很多人的轮廓仍然存在，特别是剪影。我们可能需要掩盖更大的区域。为此，我们可以尝试将掩模转换为边界框。我们可以使用`-B`标志来完成这一操作。

```py
python inpaint-person.py woman-on-trail.png -W 1152 -H 768 -B
```

我收到的输出是：

![](img/13239ae2bb31628762428c05fb379093.png)

使用边界框作为掩模的 API 调用结果（照片由作者提供）。

这也不完全正确！混凝土柱子不是我们期望在小径中间找到的东西。也许引入一个提示会有助于引导方向。我们使用`-p`标志将提示“山地风景，景观，小径”添加到请求中。我们还使用`-D 32`扩展边界框，以去除一些边缘效果，并使用`-b 16`模糊边界框，使掩模与背景稍微融合。

```py
python inpaint-person.py woman-on-trail.png \
    -W 1152 -H 768 \
    -b 16 -B -D 32 \
    -p "mountain scenery, landscape, trail"
```

使用这个我得到了以下输出：

![](img/af78bf46b1810c8eef81bd6d66fd9685.png)

最终 API 调用的结果（照片由作者提供）。

现在看起来很有可能！继续尝试不同的图片、设置和提示，以便适应您的用例。要查看该脚本可用的完整参数和提示列表，请输入 `python inpaint-person.py -h`。

# 讨论

您的图片很可能与上述图片差别很大。因为这是一个本质上随机的过程，即使使用相同设置的稳定扩散也会产生截然不同的结果。理解所有特性和正确的提示设计有相当陡峭的学习曲线，即便如此，结果也可能很挑剔。让一张图片完全符合您的喜好极其困难，需要经过大量的试验和错误。

为了帮助您，请记住以下提示：

+   在转到 API 之前，使用 Web UI 找到适合您用例的正确参数。

+   在将图像微调到您喜欢的状态时，依赖于提示矩阵和 X/Y 图特性。这些将帮助您快速探索参数搜索空间。

+   请注意种子。如果您喜欢特定的输出但想要对其进行迭代，请复制种子。

+   尝试使用不同的生成器，如 Midjourney！每个工具略有不同。

+   使用互联网资源，如 [Lexica](https://lexica.art/) 来获取灵感并寻找好的提示。

+   在设置菜单中使用“在每张图片旁边创建一个文本文件，并记录生成参数”选项，以跟踪你用来制作每张图片的提示和设置。

最重要的是，要玩得开心！
