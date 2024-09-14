# 测试支持 1162 种语言的大规模多语言语音（MMS）模型

> 原文：[`towardsdatascience.com/testing-the-massively-multilingual-speech-mms-model-that-supports-1162-languages-5db957ee1602`](https://towardsdatascience.com/testing-the-massively-multilingual-speech-mms-model-that-supports-1162-languages-5db957ee1602)

## 探索 Meta 最新的自动语音识别（ASR）模型的前沿多语言功能

[](https://medium.com/@luisroque?source=post_page-----5db957ee1602--------------------------------)![Luís Roque](https://medium.com/@luisroque?source=post_page-----5db957ee1602--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5db957ee1602--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5db957ee1602--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----5db957ee1602--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5db957ee1602--------------------------------) ·阅读时间 8 分钟·2023 年 5 月 26 日

--

# 介绍

大规模多语言语音（MMS）¹ 是 Meta AI 最近发布的产品（就在几天前）。它通过将其覆盖范围从大约 100 种语言扩展到超过 1,000 种语言，推动了语音技术的边界。这是通过构建一个单一的多语言语音识别模型实现的。该模型还可以识别超过 4,000 种语言，比以前的能力提高了 40 倍。

MMS 项目旨在让人们更容易以他们首选的语言获取信息和使用设备。它将文本转语音和语音转文本技术扩展到服务不足的语言，继续减少我们全球世界中的语言障碍。现有的应用程序现在可以包括更多种类的语言，例如虚拟助手或语音激活设备。与此同时，跨文化沟通中出现了新的应用场景，例如在消息服务或虚拟与增强现实中。

在本文中，我们将介绍 MMS 在英语和葡萄牙语的 ASR 应用，并提供逐步指南，帮助设置环境以运行该模型。

![](img/a2b17ee13d06f5681126543d65c4f142.png)

图 1：大规模多语言语音（MMS）能够识别超过 4,000 种语言，并支持 1162 种语言（[source](https://unsplash.com/photos/ZzWsHbu2y80)）

本文属于“**大语言模型编年史：导航 NLP 前沿**”系列文章中的一篇，这是一个新的每周系列文章，旨在探索如何利用大型模型的力量来完成各种 NLP 任务。通过深入了解这些前沿技术，我们旨在赋能开发者、研究人员和爱好者，利用 NLP 的潜力并开启新的可能性。

迄今为止发布的文章：

1.  [使用 ChatGPT 总结最新的 Spotify 发布内容](https://medium.com/towards-data-science/summarizing-the-latest-spotify-releases-with-chatgpt-553245a6df88)

1.  [掌握大规模语义搜索：使用 FAISS 和 Sentence Transformers 索引数百万文档，实现超快推理时间](https://medium.com/towards-data-science/master-semantic-search-at-scale-index-millions-of-documents-with-lightning-fast-inference-times-fa395e4efd88)

1.  [解锁音频数据的力量：使用 Whisper、WhisperX 和 PyAnnotate 进行高级转录和分段](https://medium.com/towards-data-science/unlock-the-power-of-audio-data-advanced-transcription-and-diarization-with-whisper-whisperx-and-ed9424307281)

1.  [Whisper JAX 与 PyTorch：揭示 ASR 在 GPU 上的性能真相](https://medium.com/towards-data-science/whisper-jax-vs-pytorch-uncovering-the-truth-about-asr-performance-on-gpus-8794ba7a42f5)

1.  [Vosk：高效企业级语音识别的评估和实施指南](https://medium.com/towards-data-science/vosk-for-efficient-enterprise-grade-speech-recognition-an-evaluation-and-implementation-guide-87a599217a6c)

与往常一样，代码可以在我的 [Github](https://github.com/luisroque/large_laguage_models) 上找到。

# 构建大规模多语言语音模型的方法

Meta 使用了宗教文本，例如《圣经》，来构建一个覆盖广泛语言的模型。这些文本有几个有趣的特点：首先，它们被翻译成多种语言，其次，有公开的音频录音，记录了不同语言的人朗读这些文本。因此，训练该模型的主要数据集是《新约》，研究团队能够收集到超过 1,100 种语言的文本，并提供了每种语言超过 32 小时的数据。他们进一步拓展了识别 4,000 种语言的能力。这是通过使用各种其他基督教宗教阅读的未标注录音实现的。从实验结果来看，尽管数据来自特定领域，但它具有良好的泛化能力。

这些并不是工作的唯一贡献。他们创建了一个新的预处理和对齐模型，可以处理长时间的录音。这被用来处理音频，错位的数据通过最终的交叉验证过滤步骤被移除。回忆一下我们之前的文章，我们看到 Whisper 的一个挑战是无法正确对齐转录。该方法的另一个重要步骤是使用 wav2vec 2.0，一个自监督学习模型，来在超过 1,400 种语言的海量语音数据（约 500,000 小时）上训练他们的系统。我们之前讨论的标注数据集不足以训练 MMS 这样规模的模型，因此使用了 wav2vec 2.0 来减少对标注数据的需求。最后，结果模型会被微调以执行特定的语音任务，例如多语言语音识别或语言识别。

MMS 模型在几天前由 Meta 开源，并在 Fairseq 仓库中提供。在下一节中，我们将深入了解 Fairseq 及如何测试这些来自 Meta 的新模型。

# Fairseq 仓库概览：一个强大的序列到序列学习工具包

Fairseq 是一个开源的序列到序列工具包，由 Facebook AI Research（也称为 FAIR）开发。它提供了各种序列建模算法的参考实现，包括卷积神经网络、递归神经网络、变压器及其他架构。

Fairseq 仓库基于 PyTorch，这是另一个开源项目，最初由 Meta 开发，现在归 Linux 基金会旗下。它是一个非常强大的机器学习框架，提供了高灵活性和速度，特别是在深度学习方面。

Fairseq 实现旨在帮助研究人员和开发者训练自定义模型，支持翻译、摘要、语言建模和其他文本生成任务。Fairseq 的一个关键特性是它支持分布式训练，这意味着它可以有效利用多个 GPU，无论是在单台机器上还是跨多台机器。这使其非常适合大规模机器学习任务。

# 大规模多语言语音模型

Fairseq 提供了两个可下载的预训练模型：MMS-300M 和 MMS-1B。你还可以访问针对不同语言和数据集的微调模型。为了我们的目的，我们测试了在 FLEURS 数据集中针对 102 种语言微调的 MMS-1B 模型，以及针对 1162 种语言（！）微调的 MMS-1B-all，微调使用了多个不同的数据集。

# 实现自动语音识别与大规模多语言语音

请记住，这些模型仍处于研究阶段，使得测试变得更具挑战性。与生产就绪的软件相比，还需要额外的步骤。

首先，你需要在项目根目录下设置一个`.env`文件，以配置你的环境变量。它应如下所示：

```py
CURRENT_DIR=/path/to/current/dir
AUDIO_SAMPLES_DIR=/path/to/audio_samples
FAIRSEQ_DIR=/path/to/fairseq
VIDEO_FILE=/path/to/video/file
AUDIO_FILE=/path/to/audio/file
RESAMPLED_AUDIO_FILE=/path/to/resampled/audio/file
TMPDIR=/path/to/tmp
PYTHONPATH=.
PREFIX=INFER
HYDRA_FULL_ERROR=1
USER=micro
MODEL=/path/to/fairseq/models_new/mms1b_all.pt
LANG=eng
```

接下来，你需要配置位于`fairseq/examples/mms/asr/config/infer_common.yaml`的 YAML 文件。该文件包含脚本使用的重要设置和参数。

在 YAML 文件中，为`checkpoint`字段使用完整路径，如下所示（除非你使用的是容器化应用程序来运行脚本）：

```py
checkpoint: /path/to/checkpoint/${env:USER}/${env:PREFIX}/${common_eval.results_path}
```

这个完整路径是必要的，以避免潜在的权限问题，除非你在容器中运行应用程序。

如果你计划使用 CPU 进行计算而不是 GPU，则需要在 YAML 文件的顶部添加以下指令：

```py
common:
  cpu: true
```

此设置指示脚本使用 CPU 进行计算。

我们使用`dotevn` Python 库在 Python 脚本中加载这些环境变量。由于我们正在覆盖一些系统变量，我们需要使用一个技巧以确保加载正确的变量。我们使用`dotevn_values`方法并将输出存储在一个变量中。这确保了我们获取的是存储在`.env`文件中的变量，而不是即使名称相同的随机系统变量。

```py
config = dotenv_values(".env")

current_dir = config['CURRENT_DIR']
tmp_dir = config['TMPDIR']
fairseq_dir = config['FAIRSEQ_DIR']
video_file = config['VIDEO_FILE']
audio_file = config['AUDIO_FILE']
audio_file_resampled = config['RESAMPLED_AUDIO_FILE']
model_path = config['MODEL']
model_new_dir = config['MODELS_NEW']
lang = config['LANG']
```

然后，我们可以克隆 fairseq GitHub 仓库并在我们的机器上安装它。

```py
def git_clone(url, path):
    """
    Clones a git repository

    Parameters:
    url (str): The URL of the git repository
    path (str): The local path where the git repository will be cloned
    """
    if not os.path.exists(path):
        Repo.clone_from(url, path)

def install_requirements(requirements):
    """
    Installs pip packages

    Parameters:
    requirements (list): List of packages to install
    """
    subprocess.check_call(["pip", "install"] + requirements)

git_clone('https://github.com/facebookresearch/fairseq', 'fairseq')
install_requirements(['--editable', './'])
```

我们已经讨论了本文中使用的模型，因此让我们将它们下载到本地环境中。

```py
def download_file(url, path):
    """
    Downloads a file

    Parameters:
    url (str): URL of the file to be downloaded
    path (str): The path where the file will be saved
    """
    subprocess.check_call(["wget", "-P", path, url])

download_file('https://dl.fbaipublicfiles.com/mms/asr/mms1b_fl102.pt', model_new_dir)
```

关于 MMS 模型的输入，还有一个额外的限制，即音频数据的采样率需要为 16000 Hz。在我们的情况下，我们定义了两种生成这些文件的方法：一种是将视频转换为音频，另一种是重新采样音频文件以获得正确的采样率。

```py
def convert_video_to_audio(video_path, audio_path):
    """
    Converts a video file to an audio file

    Parameters:
    video_path (str): Path to the video file
    audio_path (str): Path to the output audio file
    """
    subprocess.check_call(["ffmpeg", "-i", video_path, "-ar", "16000", audio_path])

def resample_audio(audio_path, new_audio_path, new_sample_rate):
    """
    Resamples an audio file

    Parameters:
    audio_path (str): Path to the current audio file
    new_audio_path (str): Path to the output audio file
    new_sample_rate (int): New sample rate in Hz
    """
    audio = AudioSegment.from_file(audio_path)
    audio = audio.set_frame_rate(new_sample_rate)
    audio.export(new_audio_path, format='wav')
```

我们现在准备使用支持 1162 种语言的 MMS-1B-all 模型运行推理过程。

```py
def run_inference(model, lang, audio):
    """
    Runs the MMS ASR inference

    Parameters:
    model (str): Path to the model file
    lang (str): Language of the audio file
    audio (str): Path to the audio file
    """
    subprocess.check_call(
        [
            "python",
            "examples/mms/asr/infer/mms_infer.py",
            "--model",
            model,
            "--lang",
            lang,
            "--audio",
            audio,
        ]
    )

run_inference(model_path, lang, audio_file_resampled)
```

# 使用 Fairseq 的自动语音识别结果

在这一部分，我们描述了我们的实验设置并讨论了结果。我们使用 Fairseq 的两种不同模型（MMS-1B-all 和 MMS-1B-FL102）在英语和葡萄牙语中进行了 ASR。你可以在我的 GitHub 库中找到音频文件。这些文件是我自己生成的，仅用于测试目的。

让我们从 MMS-1B-all 模型开始。以下是英语和葡萄牙语音频样本的输入和输出：

> **Eng**：只需一个小片段即可了解新的 Facebook Research 模型在语音识别任务中的实际表现
> 
> **Por**：现在只需在这里提供一个示例，以便尝试了解 Facebook Research 的新模型是否真正有效，我们将进行测试。

使用 MMS-1B-FL102 时，生成的语音质量显著下降。我们来看一下英语的相同示例：

> **Eng**：只需回顾一下小片段，以了解新的 Facebook Research 模型在速度识别任务中的实际表现，让我们看看

尽管生成的语音对于我们今天拥有的模型标准来说并不特别令人印象深刻，但我们需要从这些模型可以使 ASR 服务于更广泛全球人群的角度来考虑这些结果。

# 结论

Meta 开发的 Massively Multilingual Speech 模型代表了推动全球沟通和扩大语言技术应用范围的又一步。它能够理解超过 4000 种语言，并在其中 1162 种语言中有效运行，提高了许多传统上被忽视语言的可及性。

我们对 MMS 模型的测试展示了该技术在当前阶段的可能性和局限性。尽管 MMS-1B-FL102 模型生成的语音不如预期令人印象深刻，但 MMS-1B-all 模型提供了有希望的结果，展示了其在英语和葡萄牙语中的转录能力。葡萄牙语一直是那些服务不足的语言之一，特别是当我们考虑到来自葡萄牙的葡萄牙语时。

欢迎尝试你喜欢的语言，并在评论区分享转录结果和反馈。

保持联系：[LinkedIn](https://www.linkedin.com/in/luisbrasroque/)

# 参考文献

[1] — [Pratap, V., Tjandra, A., Shi, B., Tomasello, P., Babu, A., Kundu, S., Elkahky, A., Ni, Z., Vyas, A., Fazel-Zarandi, M., Baevski, A., Adi, Y., Zhang, X., Hsu, W.-N., Conneau, A., & Auli, M. (2023). 将语音技术扩展到 1,000 多种语言。arXiv.](https://scontent.fopo6-1.fna.fbcdn.net/v/t39.8562-6/348827959_6967534189927933_6819186233244071998_n.pdf?_nc_cat=104&ccb=1-7&_nc_sid=ad8a9d&_nc_ohc=nLkMCxk5EFMAX_VcRa0&_nc_ht=scontent.fopo6-1.fna&oh=00_AfDVqBOzIgYWtX65XMtxu0wXy8DPWJWl-Z_xwk0eZxUDsw&oe=64764402)
