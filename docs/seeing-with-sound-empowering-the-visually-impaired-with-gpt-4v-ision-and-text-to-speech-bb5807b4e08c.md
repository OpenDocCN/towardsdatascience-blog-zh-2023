# 通过声音看见世界：利用 GPT-4V(ision)和文本转语音技术赋能视觉障碍者

> 原文：[`towardsdatascience.com/seeing-with-sound-empowering-the-visually-impaired-with-gpt-4v-ision-and-text-to-speech-bb5807b4e08c`](https://towardsdatascience.com/seeing-with-sound-empowering-the-visually-impaired-with-gpt-4v-ision-and-text-to-speech-bb5807b4e08c)

## 提升视觉障碍导航：将 GPT-4V(ision)和 TTS 集成以提供高级感官辅助

[](https://medium.com/@luisroque?source=post_page-----bb5807b4e08c--------------------------------)![Luís Roque](https://medium.com/@luisroque?source=post_page-----bb5807b4e08c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bb5807b4e08c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb5807b4e08c--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----bb5807b4e08c--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb5807b4e08c--------------------------------) ·阅读时间 12 分钟·2023 年 11 月 16 日

--

*本文由 Rafael Guedes 共同撰写。*

# 介绍

OpenAI 的最新发展通过发布 GPT-4V(ision)和文本转语音(TTS) API，将 AI 的可用性提升到了一个全新的水平。为什么？让我们通过一个使用案例来激发它们的实际效用。对大多数人来说，走在街上是一个简单的任务，但对视觉障碍者来说，每一步都可能是一个挑战。传统的辅助工具如导盲犬和手杖虽然有用，但 AI 技术的融合开启了一个改善盲人社区独立性和移动性的全新篇章。配备隐蔽摄像头的简单眼镜足以彻底改变视觉障碍者体验周围环境的方式。我们将解释如何利用 OpenAI 的最新发布来实现这一点。

另一个有趣的使用案例是改变我们在博物馆和其他类似场所的体验。设想一下，博物馆中常见的音频导览系统被别在衣服上的隐蔽摄像头所取代。假设你正在参观一家艺术博物馆。当你在博物馆中漫步时，这项技术可以为你提供有关每幅画作的信息，并且可以按照你选择的特定风格进行讲解。假设你有点疲倦，需要一些轻松有趣的内容，你可以提示它*‘给我一些关于这幅画的历史背景，但要有趣和引人入胜，甚至可以加些笑话’*。

增强现实（AR）呢？这种新技术能否改善甚至取代它？目前，AR 被视为我们可以叠加在对现实世界的视觉感知上的数字层。问题是，这可能很快变得嘈杂。这些新技术可能会在某些用例中取代 AR。在其他情况下，它可以使 AR 个性化，使我们能够以自己的节奏体验世界。

在这篇文章中，我们将探讨如何将 GPT-4V（视觉）和文本转语音（TTS）结合起来，使世界对视觉障碍者更具包容性和可导航性。我们将首先解释 GPT-4V（视觉）是如何工作的及其架构（我们将使用一些开源对应物来获取直觉，因为 OpenAI 不提供有关其模型的详细信息）。接下来，我们将解释什么是 TTS，以及用于将文本转化为音频信号的最常见模型架构。最后，我们将通过一步一步的实施，展示如何利用 GPT-4V（视觉）和 TTS 帮助视觉障碍者在葡萄牙波尔图的街道上导航。

![](img/1b79d260aa4ed403854e157a6dbcf3a9.png)

图 1：OpenAI 发布了有关其 API 服务的多个更新，并将多模态引入 GPT-4（[来源](https://unsplash.com/photos/a-computer-chip-with-the-word-gat-printed-on-it-Fc1GBkmV-Dw)）

一如既往，代码可以在我们的 [Github](https://github.com/zaai-ai/large-language-models) 上找到。

# 什么是 GPT-4V（视觉）？

GPT-4 像 GPT-3.5 一样，是一个大型多模态模型，能够处理文本输入并生成文本输出 [1]。在最新的 OpenAI 公告中，他们表示 GPT-4 已被扩展为一个多模态大型语言模型（LLM）。这意味着该模型现在能够接收额外的输入模态，在这种情况下是图像。多模态 LLM 扩展了仅语言系统的影响，通过新的接口和能力，为更复杂的用例开辟了可能性。你可以在下图中看到使用 GPT-4V（视觉）的示例，其中视觉和推理能力一起工作，以检测图片中的一些复杂细微之处。

![](img/c5e43fe1966fe9c01a26c44c95556e66.png)

图 2：GPT-4 解释对人类来说不寻常的能力（[来源](https://arxiv.org/pdf/2303.08774.pdf)）

尽管 OpenAI 在其论文中明确表示 …

‘*考虑到竞争环境以及像 GPT-4 这样的大规模模型的安全性，这份报告未包含有关架构（包括模型大小）、硬件、训练计算、数据集构建、训练方法或类似内容的更多细节。*’

…我们可以尝试估计 GPT-4V（视觉）的架构是什么样的。

我们知道 GPT-4V（视觉）接收文本和图像作为输入。因此，它很可能有三个主要组件：

1.  **编码器：** 用于处理文本和图像数据的独立编码器

1.  **注意力机制：** 它采用先进的注意力机制，使模型能够关注文本和图像输入中最相关的部分。

1.  **解码器：** 根据编码器的潜在空间结合注意力层生成输出文本。

![](img/d097929cd283ac88e910dd41d72197ee.png)

图 3：使用图像和文本作为输入的多模态模型的简单架构（图片由作者提供）

类似的架构可以在🦩 **Flamingo 模型 [2]**中找到，该模型由 DeepMind 创建。

Flamingo 旨在处理文本和视觉数据作为输入，以生成自由形式的文本作为输出。作者将其定义为视觉语言模型（VLM）。该模型有三个主要组件：输入处理、感知重采样器和整合两种数据类型并生成输出文本的层。

**输入处理**：Flamingo 接收视觉和文本数据。文本在进入语言模型（LM）之前经历常规的分词处理，而视觉输入由视觉编码器（VE）处理，将像素转换为更抽象的特征表示。

**感知重采样器**：此组件进一步处理视觉特征。它为图像添加时间感（在视频中尤为重要），并将数据压缩成更易于管理的格式。这对后续有效结合视觉和文本数据至关重要。

![](img/134ec5bcc06ef4040855ac8c79eb0cdf.png)

图 4：感知重采样器架构（[来源](https://arxiv.org/pdf/2204.14198.pdf)）

**集成和输出：** 处理过的视觉和文本数据随后被整合到 GATED XATTN-DENSE 层。该层采用了交叉注意力机制与门控函数结合，来有效地融合两种数据。该层的输出输入到 LM 层，最终由 Transformer 解码器生成自由形式的文本输出。

![](img/c2b106ab7885b471616e79d5ad1cf92d.png)

图 5：Flamingo 模型概述。Flamingo 模型是一系列视觉语言模型（VLM），可以同时接受视觉数据和文本作为输入，并生成自由形式的文本输出（[来源](https://arxiv.org/pdf/2204.14198.pdf)）。

# GPT-4V(ision) API

OpenAI 的 GPT-4V(ision) API 允许处理视觉和文本信息。我们在下面介绍使用该 API 的主要步骤。

**设置环境**：

+   在你的环境中安装 Python 依赖项，即 OpenAI 库。

```py
pip install openai
```

+   在你的 Python 脚本中导入必要的库。

```py
import openai 
import os
```

**配置 API 参数**：利用`**ChatCompletion**`类，结合特定参数处理多模态（文本和图像）数据。

+   **模型参数**：将其设置为`**gpt-4-vision-preview**`以启用对视觉和文本数据的处理。

```py
params = {
    "model": "gpt-4-vision-preview",
    "messages": PROMPT_MESSAGES,
    "api_key": os.environ['OPENAI_API_KEY'],
    "headers": {"Openai-Version": "2020-11-07"},
    "max_tokens": 400,
}
```

+   **消息参数**：这需要包括文本和图像。图像应以 base64 格式编码。

```py
PROMPT_MESSAGES = [{
    "role": "user",
    "content": [
        "<Your Prompt>",
        {"image": image_in_base64_format}
    ],
}]
```

**处理图像**：在将其包含在 API 调用中之前，图像必须转换为 base64 格式。

```py
import base64

def convert_image_to_base64(image_path):
    with open(image_path, "rb") as image_file:
        return base64.b64encode(image_file.read()).decode('utf-8')
```

**执行 API 调用**：设置好参数后，发起 API 调用以处理输入数据。

```py
response = openai.ChatCompletion.create(**params)
print(response)
```

# 什么是文本到语音？

TTS [4] 技术将书面文字转换为口语。这一复杂过程涉及多个阶段，每个阶段由不同的模型处理。首先，一个 **字形到音素** 模型将书面文字转换为音素单位。接下来，一个 **音素到 Mel** **谱图** 模型将这些音素转换为视觉频率表示。最后，一个 **Mel-谱图到音频** 模型或 **声码器** 从这种表示生成实际的口语音频。

![](img/2e1aeaf68c4df4d6984a6d73332edcdc.png)

图 6：TTS 架构（图片来源于作者）

1.  **字形到音素转换**：第一步是将书写的单词（字形）转换为音素，即发音的基本单位。例如，短语 `**"Swifts, flushed from chimneys"**` 将被转换为音标表示，如 `**"ˈswɪfts, ˈfɫəʃt ˈfɹəm ˈtʃɪmniz"**`。一个常用的模型是 G2P-Conformer [5]。

1.  **音素到 Mel 谱图**：接下来，这些音素会被处理以创建 mel-谱图，这是声音频率随时间变化的视觉表示。这通常通过编码器-解码器架构来实现，例如 Tacotron2 [6]。在这个模型中，卷积神经网络（CNN）嵌入音素，然后通过双向长短期记忆（LSTM）网络进行处理。生成的 mel-谱图是一个关键的中间步骤，充当音素与最终音频输出之间的桥梁。

1.  **Mel-谱图到音频转换**：最后阶段涉及将 mel-谱图转换为实际的音频。这时需要一个声码器，通常使用先进的生成模型。由 DeepMind 开发的 WaveNet [7] 是一个很好的例子。它使用带有扩张因果卷积的深度神经网络，以确保正确的序列处理。每个预测的音频样本会反馈到网络中以预测下一个，从而使模型能够捕捉音频信号中的长距离模式。

# 文本到语音 API

OpenAI 提供了一个通过 API 访问的 TTS 服务，提供两个质量等级和六种不同的声音，以满足不同的需求和偏好。

**质量选项**：

+   `**tts-1**`：适用于实时应用，这个选项提供较低的质量但具有减少延迟的优点。

+   `**tts-1-hd**`：适合于音频质量较高且延迟不是问题的场景。

**选择声音**：

+   OpenAI 的 TTS API 具有六种独特的声音：`**alloy**`、`**echo**`、`**fable**`、`**onyx**`、`**nova**` 和 `**shimmer**`。

+   每种声音都有其独特的特性；例如，`**Fable**` 类似于播客叙述者的声音。

+   你可以在 OpenAI 的 [文本到语音指南](https://platform.openai.com/docs/guides/text-to-speech) 上预览这些声音。

**发起 API 请求：**

要使用 OpenAI 的 TTS API，发送请求到 `**https://api.openai.com/v1/audio/speech**`。你需要以下内容：

1.  **模型规格**：根据你的需求选择 `**tts-1**`（低质量，低延迟）或 `**tts-1-hd**`（高质量，高延迟）。

1.  **输入文本**：你想要转换为语音的文本内容。

1.  **语音选择**：从可用的语音中选择一个最适合你的内容和观众的声音。

这是一个关于如何结构化 API 请求的基本示例：

```py
response = requests.post(
    "https://api.openai.com/v1/audio/speech",
    headers={
        "Authorization": f"Bearer {os.environ['OPENAI_API_KEY']}",
    },
    json={
        "model": "tts-1",
        "input": result.choices[0].message.content,
        "voice": "fable",
    },
)
```

# 如何实现一个帮助视障人士在街上行走时感到更安全的应用？

本节逐步描述了如何使用 OpenAI 的 GPT-4V(ision) 和 TTS 创建视频的音频描述。还涵盖了如何将生成的音频添加到视频中。

在我们的案例中，如介绍所述，我们创建了一个音频指南，帮助视障人士在街上行走，通过识别障碍物的位置并提供空间信息来辅助他们。

过程开始于导入必要的库并设置 Python 环境。我们使用如 cv2 进行视频处理和 openai 访问 GPT-4V(ision) 和 TTS 模型等库。

```py
import base64
import cv2
import openai
import os
import requests
import time

from IPython.display import display, Image, Audio
from moviepy.editor import VideoFileClip, AudioFileClip
from moviepy.video.io.ffmpeg_tools import ffmpeg_extract_subclip
```

接下来，我们加载并处理视频。视频被调整为可管理的分辨率，以确保不超过 OpenAI 的令牌限制。每一帧都被编码为 base64，这是 OpenAI API 所需的格式。

```py
video = cv2.VideoCapture("video.mp4")

base64Frames = []
while video.isOpened():
    success, frame = video.read()
    if not success:
        break
    frame = cv2.resize(frame, (512,512))
    _, buffer = cv2.imencode(".jpg", frame)
    base64Frames.append(base64.b64encode(buffer).decode("utf-8"))

video.release()
print(len(base64Frames), "frames read.")
```

一个重要的步骤是为 GPT-4V(ision) 制作合适的提示。精心设计的提示会显著影响我们从模型中获得的输出。从我们的实验中，影响输出的两个主要组件是：

+   动词如描述、叙述、讲述；

+   确定我们想要的语音风格。

我们尝试的第一个提示之一是：‘*这些是一个人在城市中行走的画面。描述周围的元素和障碍物，以帮助盲人成功导航。*’ 这种结构使模型变得极其冗长和描述性。我们的使用案例需要输出更少噪音。

对我们有效的提示是：*‘这些是一个人行走的画面。用 GPS 设备那样的简短句子简洁叙述周围的元素和障碍物，以帮助盲人成功导航。’* 这次模型给出了简短的句子，让我们获得了街道导航所需的基本信息。结果如下：

> *“在有纹理的路径上直行，将建筑物保持在右侧。继续前行，稍微向右转。保持直行，前方右侧有小悬挑。继续前进，经过悬挑，继续在平坦的路径上前行。直行，接近一个光线充足的区域。经过光线充足的区域后，过渡到有图案的人行道。跟随有引导线的瓷砖人行道直行。继续通过通道，保持柱子与自己平行。穿过通道，前方有小的下降。路径结束，准备在斑马线停下。站在斑马线前，等待听到过马路的信号。”*

```py
PROMPT_MESSAGES = [
    {
        "role": "user",
        "content": [
            "These are frames from a person walking. Narrate succinctly with short sentences like they do in GPS devices the elements and obstacles around you to help a blind person go through.",
            *map(lambda x: {"image": x}, base64Frames[0::100]),
        ],
    },
]

params = {
    "model": "gpt-4-vision-preview",
    "messages": PROMPT_MESSAGES,
    "api_key": os.environ['OPENAI_API_KEY'],
    "headers": {"Openai-Version": "2020-11-07"},
    "max_tokens": 350,
}

result = openai.ChatCompletion.create(**params)
print(result.choices[0].message.content)
```

一旦我们收到 GPT-4V(ision)的描述，下一步是将文本转换为音频。我们选择了寓言风格的声音，因为它的清晰度和叙述的相似性。使用 OpenAI 的 TTS API 将生成的文本转化为音频文件。

```py
response = requests.post(
    "https://api.openai.com/v1/audio/speech",
    headers={
        "Authorization": f"Bearer {os.environ['OPENAI_API_KEY']}",
    },
    json={
        "model": "tts-1",
        "input": result.choices[0].message.content,
        "voice": "fable",
    },
)

audio = b""
for chunk in response.iter_content(chunk_size=1024 * 1024):
    audio += chunk

with open('audio.mp3', 'wb') as file:
    file.write(audio)
```

最后一步是将音频与原始视频合并。

```py
# Open the video and audio
video_clip = VideoFileClip("video.mp4")
audio_clip = AudioFileClip("audio.mp3")

# Concatenate the video clip with the audio clip
final_clip = video_clip.set_audio(audio_clip)
final_clip.write_videofile("video_audio.mp4")
```

你可以在[这里](https://youtu.be/Erena-iLAY0)查看最终视频。

# 结论

OpenAI 的 GPT-4V(ision)和 TTS API 开辟了新的可能性，解决了可以改变许多人日常生活的重要用例。我们探讨了一个专注于视障人士的包容性的用例，但我们本可以选择其他许多用例。如在介绍中所述，它们还让我们能够提升人类体验（例如博物馆导览），并根据个人的偏好和情况进行更好的定制。

在实施过程中，我们发现提示工程对将解决方案量身定制到我们的特定用例有显著影响。未来，其他方法如微调或某种类型的检索增强生成（RAG）可能会适用于 VLMs。我们看到这些方法在某些任务和情境中使 LLMs 更为有效。尽管输出展示了这些新模型的潜力，但仍有待完善。如我们实验中所见，VLM *“说话”*的方式像是你可以看到它所说的*“跟随有引导线的瓷砖人行道直行。”* 它还难以准确区分左右，这是一个值得进一步探索的有趣事实。

尽管面临这些挑战，OpenAI 的最新进展向我们展示了一个更加包容的未来，AI 可以提升体验。

保持联系：[LinkedIn](https://www.linkedin.com/in/luisbrasroque/)，[X/Twitter](https://x.com/luisbrasroque)，[Medium](https://medium.com/@luisroque)。

# 参考文献

[1] OpenAI. (2023). GPT-4 技术报告。arXiv 预印本 arXiv:2303.08774。

[2] Alayrac, J.-B., Donahue, J., Luc, P., Miech, A., Barr, I., Hasson, Y., Lenc, K., Mensch, A., Millican, K., Reynolds, M., Ring, R., Rutherford, E., Cabi, S., Han, T., Gong, Z., Samangooei, S., Monteiro, M., Menick, J., Borgeaud, S., Brock, A., Nematzadeh, A., Sharifzadeh, S., Binkowski, M., Barreira, R., Vinyals, O., Zisserman, A., & Simonyan, K. (2022). Flamingo: 一种用于少样本学习的视觉语言模型。arXiv 预印本 arXiv:2204.14198。

[3] Brock, A., De, S., Smith, S. L., & Simonyan, K. (2021). 无需归一化的高性能大规模图像识别。arXiv 预印本 arXiv:2102.06171。

[4] Maheshwari, H. (2021 年 5 月 11 日). 基础文本到语音，解析。Towards Data Science。取自 `towardsdatascience.com/text-to-speech-explained-from-basic-498119aa38b5`。

[5] Gulati, A., Qin, J., Chiu, C.-C., Parmar, N., Zhang, Y., Yu, J., Han, W., Wang, S., Zhang, Z., Wu, Y., 等. (2020). Conformer: 用于语音识别的卷积增强变换器。arXiv 预印本 arXiv:2005.08100。

[6] Shen, J., Pang, R., Weiss, R. J., Schuster, M., Jaitly, N., Yang, Z., Chen, Z., Zhang, Y., Wang, Y., Skerry-Ryan, R. J., Saurous, R. A., Agiomyrgiannakis, Y., & Wu, Y. (2018). 通过将 WaveNet 条件化于 Mel 频谱预测进行自然 TTS 合成。arXiv 预印本 arXiv:1712.05884。

[7] van den Oord, A., Dieleman, S., Zen, H., Simonyan, K., Vinyals, O., Graves, A., Kalchbrenner, N., Senior, A., & Kavukcuoglu, K. (2016). WaveNet: 一种原始音频的生成模型。arXiv 预印本 arXiv:1609.03499。
