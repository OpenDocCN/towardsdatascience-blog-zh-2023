# 创建你自己的生成 AI 文本到图像 API

> 原文：[`towardsdatascience.com/create-your-own-generative-ai-text-to-image-api-548c07a4d839`](https://towardsdatascience.com/create-your-own-generative-ai-text-to-image-api-548c07a4d839)

## 将你的随想转化为杰作，按需制作

[](https://medium.com/@omermx?source=post_page-----548c07a4d839--------------------------------)![奥默尔·马赫穆德](https://medium.com/@omermx?source=post_page-----548c07a4d839--------------------------------)[](https://towardsdatascience.com/?source=post_page-----548c07a4d839--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----548c07a4d839--------------------------------) [奥默尔·马赫穆德](https://medium.com/@omermx?source=post_page-----548c07a4d839--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----548c07a4d839--------------------------------) ·17 分钟阅读·2023 年 4 月 12 日

--

![](img/738244045935fe00f21f0a7c31c3f546.png)

图片由作者使用 Midjourney 根据一般商业条款生成。

# TL;DR

+   生成 AI 的最新进展导致了一系列服务的推出，如 DALL-E 2、Midjourney 和 Stability AI，它们有潜力彻底改变我们对内容创作的方式。

+   在这篇文章中，我将展示如何通过 API 构建并提供你自己高性能的文本到图像服务。基于 [稳定扩散](https://github.com/CompVis/stable-diffusion) 通过 [HuggingFace](https://medium.com/towards-data-science/whats-hugging-face-122f4e7eb11a)，使用 Vertex AI Workbench 和 Endpoints。

# 🚣🏼 我们是如何到达这里的

正如乔治·劳顿在他的 [文章](https://www.techtarget.com/searchenterpriseai/definition/generative-AI) 中提到的：“生成 AI 是一种人工智能技术，能够生成各种类型的内容，包括文本、图像、音频和合成数据。最近围绕生成 AI 的热议是由于新用户界面的简便性，这些界面可以在几秒钟内创建高质量的文本、图形和视频。”[2]

机器学习并不是什么新鲜事，事实上，自 1960 年代以来，它以某种形式存在。“但直到 2014 年，随着 [生成对抗网络](https://en.wikipedia.org/wiki/Generative_adversarial_network)（GANs）的引入，一种机器学习算法，生成 AI 才得以创造令人信服的真实人物图像、视频和音频。”[2]

结合能够接收自然语言提示并生成照片级真实图像的大型语言模型（LLMs）的能力，我们在短时间内取得了巨大的进步。第一个做到这一点的是[OpenAI 的 DALL·E](https://openai.com/product/dall-e-2)，于 2022 年 4 月推出，随后是 2022 年 8 月的 Disco Diffusion，最终被稳定扩散所取代。与这些产品并行的是一家名为[Midjourney](https://www.midjourney.com/home/?callbackUrl=%2Fapp%2F)的公司，它开发了一个非常受欢迎的模型，通过与 Discord 机器人互动使用。

从那时起，艺术状态的进展令人惊叹。下面的截图展示了仅在几个月的时间内所取得的成就——两个不同的模型提供了相同的提示，但早期模型和后期模型之间的对比非常明显！

![](img/c0322ec3ed14c73845d0dc9300b08599.png)

*图 1：生成性人工智能艺术的进展，由 Disco Diffusion 和 Midjourney 生成的图像。*

快进到 2023 年，Midjourney 于 3 月 15 日刚刚发布了第 5 版。这个模型具有非常高的连贯性，擅长解释自然语言提示，分辨率更高，并支持像重复图案这样的高级功能[4]。Stability AI 也发布了稳定扩散 2.1。这些模型的发展速度非常迅猛！

正如德勤最近发布的[白皮书](https://www2.deloitte.com/us/en/pages/consulting/articles/generative-artificial-intelligence.html)中描述的：“我们感觉可能只是刚刚开始看到生成性人工智能模型所能带来的影响。虽然早期的吸引力主要来自消费者发布，这可能会成为定义时代的里程碑，但生成性人工智能也有潜力为几乎所有生活领域增加情境意识和类人决策能力。

因此，生成性人工智能吸引了来自传统（例如风险投资（VC），并购（M&A））和新兴（例如生态系统合作伙伴）来源的兴趣。仅在 2022 年，风险投资公司就投资了超过 20 亿美元，科技领袖也进行了重要投资，如微软对 OpenAI 的 100 亿美元投资和谷歌对 Anthropic 的 3 亿美元投资。”[3]

# 🧐 什么是稳定扩散？

稳定扩散是一种用于人工智能文本生成图像的方法，它利用扩散模型从文本描述中创建图像。扩散模型从一张随机图像开始，然后逐步向其中添加噪声。噪声以受控的方式添加，使得图像仍然可以辨认。

在每个扩散步骤中，图像变得更加精细，细节变得更加清晰。这个过程会持续几个扩散步骤，直到生成的图像被认为“稳定”，这意味着我们已经达到了一个进一步迭代不会改善图像的点。过程如下所示：

![](img/ce6279d504e7f24b30e95752aabbdd18.png)

*图 2：Stable Diffusion 过程，图像来源：* [*Benlisquare*](https://en.wikipedia.org/wiki/Stable_Diffusion#/media/File:X-Y_plot_of_algorithmically-generated_AI_art_of_European-style_castle_in_Japan_demonstrating_DDIM_diffusion_steps.png)*，根据许可共享：* [*CC BY-SA 4.0*](http://creativecommons.org/licenses/by-nc/4.0/)

扩散模型在一个图像和文本描述的数据集上进行训练，它学会将文本描述与匹配的图像关联起来。当你给扩散模型一个新的文本描述时，它会利用其知识生成一个匹配描述的图像。

Stable Diffusion 的主要优势在于它非常快速。它可以在几秒钟内从文本描述生成图像。这比其他方法（如 GANs）快得多。我们的现代竞争者（例如 Midjourney、DALL E 等）都使用某种变体的 Stable Diffusion 技术。

但 Stable Diffusion 并不完美。虽然在过去一年中它的应用有了飞跃式的进步，但有时生成的图像可能会失真或缺乏细节。这可能是因为像这样的模型会根据训练数据来“猜测”图像应该是什么样子。随着训练数据集和模型算法的改进，这个问题将会减少。

# 👷🏾‍♀️ 开始吧！

在这篇文章中，我将向你展示如何使用一些代码来：

1.  **互动地实验 Stable Diffusion** 模型，生成一些酷炫的艺术作品！

1.  **通过端点服务模型** 使用 Vertex AI。

1.  **使用 FLASK 创建一个简单的 RESTful API**。

当然，你可以直接使用我们在前面章节中提到的现有的面向消费者的模型，但那样会没有乐趣啊？😜

➡️ 随意跳到你感兴趣的部分！

👾 我会把任何代码片段（如果没有链接的话）放在一个 github 仓库中，链接见最后的有用资源部分

# 👩🏻‍💻 1. 实验 Stable Diffusion

当你开始处理一个新的机器学习问题时，笔记本提供了一种非常灵活的方式来测试模型和快速迭代。也许你喜欢在本地环境中运行 [Jupyter](https://jupyter.org/)，使用 [Kaggle Kernel](https://www.kaggle.com/code)，或者我个人最喜欢的 [Colab](https://colab.research.google.com/)。有了这些工具，创建和实验机器学习变得越来越可及。

我们将创建一个快速的笔记本，向你介绍 Stable Diffusion 模型

💨 如果你没有时间（或不想）一步步构建，可以查看资源部分下载笔记本文件！

**前提条件：**

+   用于登录 Colab 和保存笔记本的 Google 账户

+   HuggingFace 账户和 API 令牌（免费）

1. 访问 [`colab.research.google.com/`](https://colab.research.google.com/)。这将为你创建一个新的 Python 笔记本，并将其保存到你的 Google Drive 中。

2. Colab 免费提供，但如果你需要更多的内存和计算资源来运行你的笔记本，你需要付费。我们将只使用免费的资源，首先你需要配置笔记本运行时以包含 GPU 加速器。使用 GPU 会加快模型生成图像的时间，也就是执行推理。免费层提供了 12GB 内存的 NVIDIA Tesla T4 GPU（这对我们要做的事情正好足够）。从文件菜单中选择 *运行时 -> 更改运行时类型*，然后选择硬件加速器 = GPU。

![](img/f221b1b327a28f16c52af8d6322c8787.png)

*图 3：Colab 笔记本，更改笔记本运行时。*

然后你需要连接到你的运行时，以便我们可以执行笔记本中编写的任何代码。点击 *连接/重新连接 -> 连接到托管运行时*：

![](img/ba22e38c451cbb8a3098582d01982b16.png)

*图 4：Colab 笔记本，“+ 代码”按钮添加单元格，以及连接到托管运行时。*

3. 一旦你连接到你的运行时，就可以输入一些代码了！你可以在单个笔记本“单元格”中输入代码，或者如果你想单独执行步骤，可以使用“+ 代码”按钮创建一个新的“单元格”。我发现将代码分解成单元格很有用，这样更容易识别和调试问题。每个单元格在你悬停时都有一个“播放”按钮，点击它以运行代码。输入或复制粘贴代码到新的单元格中后，你可以运行以下步骤：

a. 由于我们需要从 HuggingFace 拉取模型，因此需要安装一些 Python 库并使用 API 令牌进行认证：

```py
!pip install --upgrade huggingface_hub
```

b. 接下来，我们需要 [使用你 HuggingFace 账户的令牌进行认证](https://huggingface.co/docs/hub/security-tokens#how-to-manage-user-access-tokens)，当你执行以下代码时，你会在笔记本中被提示粘贴你的 HuggingFace API 令牌：

```py
from huggingface_hub import notebook_login
notebook_login()
```

c. 成功登录你的 HuggingFace 账户后，我们将下载 diffusers 和 transformers Python 库：

```py
!pip install -qq -U diffusers transformers
```

d. 我们需要创建一个 StableDiffusion 模型管道，以便我们可以将模型传递一些文本，并根据该提示生成图像。你可能会注意到我们传递的参数之一是 HuggingFace 上托管的 Stable Diffusion 模型的路径。此帖子中的示例使用的是 v1.5，你可以尝试在写作时交换为最新的（v2.1）：

```py
from diffusers import StableDiffusionPipeline
pipe = StableDiffusionPipeline.from_pretrained('runwayml/stable-diffusion-v1–5')
.to('cuda')
```

e. 现在进入有趣的部分，我们将生成一些艺术作品！加载 torch Python 库，然后添加一个单元格，将 prompt 变量的字符串值替换为你想要的任何内容！我提供了一个简单的示例以帮助你入门：

```py
import torch

# Initialize a prompt
prompt = “polar bear on an iceberg”

# Pass the prompt in the pipeline
pipe(prompt).images[0]
```

你应该得到如下所示的结果：

![](img/e98a0eeada649dd901d490466eea6b7c.png)

*图 5：Colab 笔记本，Stable Diffusion 模型生成的图像输出。*

**💡提示：** 如果你遇到‘内存不足’的错误，你可以通过在文本提示上方添加以下代码行定期清除 GPU 的缓存：

```py
torch.cuda.empty_cache()
```

只需在一个笔记本中写几行代码，你就可以从基于文本的提示生成艺术作品！

# 🍦 2\. 通过 Vertex AI 端点服务模型

制作生产应用程序或训练大型模型需要额外的工具来帮助你超越笔记本中的代码，并且使用云服务提供商可以提供帮助。

我们的目标是将 Stable Diffusion 模型打包，并托管在一个可以处理来自应用代码的预测请求的端点上。

你可以使用其他公共云服务提供商来实现大致相同的结果，但我将使用 Google Cloud Platform (GCP)，特别是它的 Vertex AI 工具集，因为我对此最为熟悉。

**前提条件：**

+   启用计费的 GCP 账户/免费起始积分

+   具备一些基本的 GCP 管理知识，例如如何创建项目、虚拟机、存储桶和其他资源，

+   通过 IAM 给服务账户赋予权限，

+   下载服务账户密钥，

+   使用 Google Cloud SDK + 一些 Python 代码。

1\. 我们将使用一个预构建的笔记本示例：[`github.com/GoogleCloudPlatform/vertex-ai-samples/blob/main/notebooks/community/vertex_endpoints/torchserve/dreambooth_stablediffusion.ipynb`](https://github.com/GoogleCloudPlatform/vertex-ai-samples/blob/main/notebooks/community/vertex_endpoints/torchserve/dreambooth_stablediffusion.ipynb)，该示例来自 GCP Vertex AI 在 Github 上的官方仓库。**我不会在这里详细复述整个内容**，只会解释最相关的部分以及我在实际操作中遇到的问题……代码示例通常没有看起来那么简单！

⚠️ **在这个示例中创建用户管理的笔记本并将模型部署到端点将产生费用**。当你第一次创建 GCP 账户时，可以获得 $300 的免费积分，但推荐的硬件配置（以及模型部署的端点的任何后续调用）可能会很快消耗这些积分——你已经被警告了。我将在“结语”部分分享我实际产生的费用。

2\. 如果这是你第一次使用 Vertex AI，请登录到你的 GCP 控制台，并确保从 Vertex AI 仪表板启用所有必要的 API：

![](img/f5477d0fd316762a0569855edf4e1316.png)

*图 6: GCP 中的 Vertex AI 仪表板，启用所有推荐的 API 按钮。*

3\. 打开 [笔记本](https://github.com/GoogleCloudPlatform/vertex-ai-samples/blob/main/notebooks/community/vertex_endpoints/torchserve/dreambooth_stablediffusion.ipynb)，在标题下方，你应该能看到一些按钮，包括“在 Colab 中运行”和“**在 Vertex AI Workbench 中打开**”。你需要点击后者。

4\. 然后你需要配置一个 VM 实例来托管用户管理的笔记本。示例建议使用带有 85GB RAM 的 NVIDIA A100 实例，也就是“a2-highgpu-1g”。推理（从笔记本中的示例创建图像）很快，大约 11 秒钟就能从 Stable Diffusion 模型中返回图像。

5\. 一旦你的实例创建完成，你可以通过 *Workbench -> 用户管理的笔记本* 访问它。

![](img/9f903e4a5efbd63cfc2ec27b1d02f917.png)

*图 7：Vertex AI 工作台，用户管理的笔记本列表。*

在你深入并点击 *OPEN JUPYTERLAB* 之前，确保给你的笔记本所在 VM 的服务账户授予权限，以便进行诸如创建存储桶和端点等操作。**由于它是“用户管理”的，这不会自动为你完成。**

6\. 点击笔记本名称，在笔记本详细信息中你将看到所有者或服务账户别名。

![](img/63111df16dbce5738f2d20cba3cb731a.png)

*图 8：Vertex AI，笔记本详细信息。*

7\. 转到 *IAM & Admin -> IAM*，然后点击 *授予访问权限*，粘贴或输入你的笔记本实例的服务账户别名，为了简化操作，我授予了“编辑者”访问权限。如果你很挑剔，你可以只授予 `dreambooth_diffusion` 示例步骤所需的特定权限。

![](img/a28ceee2caddfcc72ef7537fb5b7a97d.png)

*图 9：GCP IAM & Admin, IAM, 编辑访问/分配角色。*

8\. 现在你应该准备好在 Jupyterlab 中运行 `dreambooth_diffusion.ipynb` 笔记本代码了，从工作台中打开它（如步骤 5 所示）。由于某种原因，示例的 git 仓库中的代码没有被复制到我的笔记本实例中，所以我只是打开了终端并快速克隆了 GitHub 仓库：

```py
$ git clone https://github.com/GoogleCloudPlatform/vertex-ai-samples
```

9\. 一旦你在 Jupyterlab 中打开了 `dreambooth_diffusion.ipynb`，你应该能够运行笔记本单元而不会遇到重大问题。笔记本的第一部分是通过步骤下载 Stable Diffusion 模型并从提示中创建图像。下一步是创建 Vertex AI 端点并将模型部署到那里进行服务。

10\. 按照笔记本中的步骤进行：

a. 创建自定义 TorchServe 处理程序。

b. 将模型工件上传到 Google Cloud Storage。

c. 使用模型工件和预构建的 PyTorch 容器镜像创建 Vertex AI 模型。

d. 将 Vertex AI 模型部署到端点上。

只要你在之前启用了 Vertex API（参见步骤 2。如果你忘记了！），这一切应该都会顺利进行。对我而言，在创建端点之后，模型部署大约花费了 30 分钟。当它准备好提供服务时，你会在 *端点* 下看到类似的内容：

![](img/c7daecac9d9e3b07f85e43677b8db55e.png)

*图 10：Vertex AI 端点。*

你现在准备好从你的 Stable Diffusion 模型中处理请求了！！此时，如果你想节省成本，可以停止笔记本实例 VM 并删除你创建的存储桶。请参见 `dreambooth_diffusion.ipynb` 最后部分的“清理”步骤。

## 🧪 测试你的端点

要向 Vertex AI 端点发送请求，你需要使用支持发送具有适当 HTTP 方法和请求参数的请求的 HTTP 客户端库或命令行工具。

我在我的笔记本电脑上进行了本地测试。为此，你需要下载并安装 [Vertex AI Python SDK](https://cloud.google.com/python/docs/reference/aiplatform/latest/index.html#endpoints)，然后创建并下载一个服务密钥用于认证。

如果你保留了你的笔记本实例虚拟机，你可以使用之前的服务帐户别名，或者只需创建一个新的服务帐户，其权限至少包括[从端点获取预测](https://cloud.google.com/vertex-ai/docs/general/iam-permissions)。

1\. 转到 *IAM & Admin -> 服务帐户*。然后点击服务帐户别名右侧操作下的三个点，然后点击 *管理密钥*：

![](img/855a1d349360219d1836d9c84000eb22.png)

*图 11：GCP IAM & Admin，服务帐户，管理服务帐户的密钥。*

然后点击*添加密钥 -> 创建新密钥*，并按推荐的 .JSON 格式下载：

![](img/658dd6110b2eb15cc1c7f0f98d6f40c5.png)

*图 12：GCP IAM & Admin，服务帐户，为服务帐户创建密钥。*

⚠️ **请记住，服务帐户密钥文件授予与你的 GCP 项目中服务帐户相同的权限。** 始终非常小心处理该文件，删除它时不要再使用，并且永远不要将其上传到 Github 仓库，我见过太多这种情况！

2\. 在终端窗口中，或在你运行代码以测试端点的地方，你需要设置一个变量以指向你下载的服务密钥。这将对端点请求进行认证：

```py
$ export GOOGLE_APPLICATION_CREDENTIALS=”path/to/your.json file”
```

3\. 现在只需用你的项目名称、区域和端点 ID 修改这个 Python 代码片段。它将向你的端点传递一个提示，并将响应（即由 Stable Diffusion 生成的图像）存储在 JPEG 文件中：

```py
import base64
import logging
from io import BytesIO
from google.cloud import aiplatform as aip

PROJECT_NAME = “YOUR-PROJECT-ID”
REGION = “us-central1”
ENDPOINT_ID = “YOUR-ENDPOINT-ID”

aip.init(project=PROJECT_NAME, location=REGION)
endpoint = aip.Endpoint(endpoint_name=ENDPOINT_ID)
text_input = “””Polar bear on an iceberg”””

# Invoke the Vertex AI endpoint 
def query_endpoint(endpoint, text_input):
  payload = {“prompt”: text_input}
  response = endpoint.predict(instances=[payload])
  return response

response = query_endpoint(endpoint, text_input)

with open(“generated_imgage.jpg”, “wb”) as g:
    g.write(base64.b64decode(response.predictions[0]))
```

如果你安装了 Vertex AI Python SDK、进行了 GCP 认证，并且端点处于活动状态，几秒钟后你应该会看到生成的图像文件出现在你的文件系统中！

# 🎁 3\. 使用 FLASK 创建一个简单的 RESTful API

此时，你可以很容易地将调用生成 AI 模型端点的代码集成到使用 Vertex AI Python SDK 的现有应用程序中。

但我确实承诺了一个 API，所以我将在这一部分的最后部分详细介绍它。

**前提条件：**

+   安装 [Flask](https://pypi.org/project/Flask/) 和 [Pillow](https://pypi.org/project/Pillow/) Python 库。

+   [下载 Postman](https://www.postman.com/downloads/?utm_source=postman-home) 并安装，它是免费的！我们将用它来模拟对我们的 API 的调用。

1\. 与前一节一样，确保你有一个环境变量，指向你计划运行代码的地方，这个变量指向你的服务帐户密钥。

2\. 这是你需要的代码，用于使用 Flask Web 应用程序框架创建一个简单的 RESTful API。请参见代码中的注释，了解正在发生的事情。我们基本上使用了之前的代码来查询端点，并将其封装在 API 调用中：

```py
import base64
from google.cloud import aiplatform as aip
from flask import Flask, jsonify, request, send_file
from PIL import Image
from io import BytesIO

app = Flask(__name__)

@app.route(‘/predict’, methods=[‘POST’])
def predict():
  PROJECT_NAME = ‘YOUR-PROJECT-ID’
  REGION = ‘us-central1’
  ENDPOINT_ID = ‘YOUR-ENDPOINT-ID’

  # Get the input data from the HTTP request
  input_data = request.get_json()

  # Extract the text parameter from the input data
  prompt = input_data.get(‘prompt’, ‘’)

  aip.init(project=PROJECT_NAME, location=REGION)
  endpoint = aip.Endpoint(endpoint_name=ENDPOINT_ID)
  text_input = prompt

  # Invoke the Vertex AI
  payload = {“prompt”: text_input}
  response = endpoint.predict(instances=[payload])

  # Decode the image data from base64 format
  image_data = response.predictions[0]
  image_bytes = base64.b64decode(image_data)

  # Create a PIL Image object from the decoded image data
  image = Image.open(BytesIO(image_bytes))

  # Save the image to a BytesIO buffer
  buffer = BytesIO()
  image.save(buffer, format=’JPEG’)
  buffer.seek(0)

  # Return the image file in the response
  return send_file(buffer, mimetype=’image/jpeg’)

if __name__ == ‘__main__’:
    app.run(debug=True)
```

3\. 运行生成的文件，你应该会有一个托管你 API 的 Flask 服务器，准备接收请求：

![](img/7a83c0c778babc2f9b0881ebd22530da.png)

*图 13：MacOS 终端，本地运行 Flask 服务器。*

4\. 接下来我们将启动 Postman，进入 *文件 -> 导入*，粘贴以下 cURL 命令（如果你的服务器地址配置与默认不同，你可能需要修改它）：

```py
curl -X POST \
-H “Content-Type: application/json” \
-d ‘{“prompt”: “Astronauts in the ocean”}’ \
http://127.0.0.1:5000/predict \
-- output generated_image.jpg
```

我们可以从终端运行这个，API 的响应会被保存为本地文件系统中的 JPEG 图像。但为了模拟一个应用程序，而且因为我没有时间编写 Discord 机器人或 HTML 前端… 😅

![](img/1ab9d69c5b3463030296060bc70e88b2.png)

*图 14：Postman，API 请求示例，Stable Diffusion 模型生成的图像响应。*

如果一切顺利，你将得到一个漂亮生成的图像，通过你自己的 API 提供服务。你可以在“Body”标签下修改提示，创造力是唯一的限制！

🗑️ **实验完成了吗？** 你可以返回到 *Vertex AI -> 端点*，选择端点，撤销模型部署，返回上一层删除它（提示：每行末尾的三点菜单）。仔细检查你是否关闭或删除了与这篇文章相关的任何其他内容，以避免消耗资源。你也可以撤销你创建的服务账户密钥，以确保安全。

⚠️ **最后警告 —** 这显然不是生产就绪的代码。如果你打算让你的 API 对外公开，还有很多工作需要做，比如认证等，这些超出了本文的范围！相对而言，我们的端点每次请求的成本（或计算时间）也比 Midjourney 或 DALL-E 2 高得多，因此可能不适合作为服务上线。

# 🏆 总结和结束思考

我在写这篇文章时**非常**开心，并且学到了很多关于生成式 AI、Stable Diffusion 以及将其打包成类似今天流行的消费者服务的内容。我向那些站在这项技术最前沿的开发团队致敬，这确实是一个令人兴奋的领域！

# 💸 成本

我有点担心运行大型 GPU 附加实例。如果你是长期使用公共云的用户，你会知道没有适当的检查和制衡机制，很容易超支。

在写完这篇文章的最后，我查看了 GCP 控制台中的账单部分，看看花了多少钱…

![](img/f0e6619fbe9fabd43cfb448f83477b81.png)

*图 15：GCP 账单，报告。*

在使用 GCP 的两天里，专门为这篇文章创建的项目中，我花费了大约 40 美元。你可以在上面的截图中看到详细分解。

大部分费用与推理相关（参见 Vertex AI 服务条目）。这是使我的端点能够使用我部署的 Stable Diffusion 模型生成图像的“计算小时”费用。

端点使用了“Vertex AI:在线/批处理预测 NVIDIA Tesla P100 GPU 运行于美洲的 AI 平台 SKU”，还包括运行预定义实例的一些其他费用。我们本可以通过选择更便宜的 GPU 家族来减少开支，但这将导致生成图像的时间延长。

与商业上可用的解决方案相比，Midjourney 的 [基础计划](https://docs.midjourney.com/docs/plans) 每月订阅费用为 10 美元。这个计划包括每月 3.3 小时的快速 GPU 计算，超出部分费用为每小时 4 美元。在我们的案例中，我让端点运行了大约 24 小时，花费了 30 美元，没有并发或作业等待的限制。再次说明，与 Midjourney 这样的完全托管服务相比，工程师们始终在改进模型，而选择在可扩展的云基础设施上迭代和部署自己的模型之间存在权衡。

> **这里需要注意的重要一点是，你支付的不是每次预测请求的费用，而是你的端点运行的时间以及它所运行的实例的大小/类型。**

测试模型和通过用户管理的笔记本实际部署端点的费用不到 10 美元（参见计算引擎和笔记本条目）。

所以，本文简要概述了生成式人工智能，特别是使用 Stable Diffusion 技术创建艺术的应用。接着，我们深入探讨了一些代码示例，以展示使用 Stable Diffusion 模型生成艺术的步骤，以及如何将其部署到端点并通过 API 使用。**希望你喜欢这篇文章，我们下次见！** 👋🏼

本文中的数据由作者创建，除非另有说明。

# 📇 参考资料

[1] 机器学习，历史及其与其他领域的关系: [`en.wikipedia.org/wiki/Machine_learning#History_and_relationships_to_other_fields`](https://en.wikipedia.org/wiki/Machine_learning#History_and_relationships_to_other_fields)

[2] 什么是生成式人工智能？你需要知道的一切，George Lawton: [`www.techtarget.com/searchenterpriseai/definition/generative-AI`](https://www.techtarget.com/searchenterpriseai/definition/generative-AI)

[3] 生成式人工智能对企业的影响——人工智能的新前沿，Deloitte LLP: [`www2.deloitte.com/us/en/pages/consulting/articles/generative-artificial-intelligence.html`](https://www2.deloitte.com/us/en/pages/consulting/articles/generative-artificial-intelligence.html)

[4] Midjourney 文档，版本: [`docs.midjourney.com/docs/model-versions#:~:text=Current%20Model,places%2C%20objects%2C%20and%20more`](https://docs.midjourney.com/docs/model-versions#:~:text=Current%20Model,places%2C%20objects%2C%20and%20more)。

# 📚 有用资源

+   你可以从“1. 实验稳定扩散”和“2. 通过 API 提供模型 -> 端点”中找到完整的笔记本（StableDiffusion_Hugging_Face.ipynb）和 Python 代码，链接如下：[`github.com/omermx/medium_text_to_image_api`](https://github.com/omermx/medium_text_to_image_api)

+   你可以将我在示例中使用的稳定扩散版本（1.5）替换为写作时的最新版本（2.1），请访问 Hugging Face：[`huggingface.co/stabilityai/stable-diffusion-2-1`](https://huggingface.co/stabilityai/stable-diffusion-2-1)
