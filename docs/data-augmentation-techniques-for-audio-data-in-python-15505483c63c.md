# Python 中的音频数据增强技术

> 原文：[`towardsdatascience.com/data-augmentation-techniques-for-audio-data-in-python-15505483c63c`](https://towardsdatascience.com/data-augmentation-techniques-for-audio-data-in-python-15505483c63c)

## 如何使用 librosa、numpy 和 PyTorch 在波形（时域）和频谱图（频域）中增强音频

[](https://medium.com/@iamleonie?source=post_page-----15505483c63c--------------------------------)![Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----15505483c63c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----15505483c63c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----15505483c63c--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----15505483c63c--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----15505483c63c--------------------------------) ·7 分钟阅读·2023 年 3 月 28 日

--

![](img/09de7dc6b33b345123ccb71209ef5b2b.png)

（图片由作者绘制）

深度学习模型需要大量数据。如果你没有足够的数据，生成合成数据可以帮助提高深度学习模型的泛化能力。尽管你可能已经熟悉图像的数据增强技术（例如，水平翻转图像），但音频数据的数据增强技术往往鲜为人知。

[](/audio-classification-with-deep-learning-in-python-cf752b22ba07?source=post_page-----15505483c63c--------------------------------) ## 用深度学习进行音频分类

### 微调图像模型以应对领域转移和类别不平衡，使用 PyTorch 和 torchaudio 处理音频数据

towardsdatascience.com

本文将回顾流行的音频数据增强技术。你可以在波形和频谱图中应用音频数据增强：

+   音频数据增强（波形，时域）

    ∘ 噪声注入

    ∘ 时间偏移

    ∘ 改变速度

    ∘ 改变音调

    ∘ 改变音量（不推荐）

+   频谱图的音频数据增强（频域）

    ∘ 混合数据

    ∘ SpecAugment

对于数据增强，我们将使用`[librosa](https://librosa.org/doc/main/index.html)`，这是一个流行的音频处理库，以及`numpy`。

```py
import numpy as np
import librosa
```

如果你已经在使用 PyTorch，你还可以使用`torchaudio`作为替代方案。

# 音频数据增强（波形，时域）

本节将讨论可以应用于波形音频数据的流行数据增强技术。你可以使用`[librosa](https://librosa.org/doc/main/index.html)`库中的`load()`方法将音频文件加载为波形。

```py
PATH = "audio_example.wav" # Replace with your file here
original_audio, sample_rate = librosa.load(PATH)
```

![](img/cf21435e81ab505cacb8d357ddf21a01.png)

“Speech Commands”数据集中单词“stop”的原始音频数据（图片来源：作者）

以下代码实现参考自 Kaggle 笔记本，作者为[kaerururu](https://www.kaggle.com/kaerunantoka) [7]和[CVxTz](https://www.kaggle.com/CVxTz) [5]。

## 噪音注入

一种流行的数据增强技术是将某种噪音注入到原始音频数据中。

你可以选择多种不同类型的噪音：

+   白噪音

```py
# Code copied and edited from https://www.kaggle.com/code/kaerunantoka/birdclef2022-use-2nd-label-f0
noise_factor = 0.005
white_noise = np.random.randn(len(original_audio)) * noise_factor
```

+   彩色噪音（例如粉色噪音、棕色噪音等）

```py
# Code copied and edited from https://www.kaggle.com/code/kaerunantoka/birdclef2022-use-2nd-label-f0
import colorednoise as cn

pink_noise = cn.powerlaw_psd_gaussian(1, len(original_audio))
```

+   背景噪音

```py
# Load background noise from another audio file
background_noise, sample_rate = librosa.load("background_noise.wav")
```

一旦定义了你想注入的噪音类型，你可以将噪音添加到原始波形音频中。当然，你可以使用各种不同类型的噪音进行数据增强。下方可以看到白噪音注入的示例。

```py
augmented_audio = original_audio + noise
```

![](img/9c47320d4a1db694faa565c3ef2b6611.png)

音频数据增强：白噪音（图片来源：作者）

## 时间平移

使用`numpy`库中的`roll()`函数，你可以在时间上平移音频。

```py
# Code copied and edited from https://www.kaggle.com/code/CVxTz/audio-data-augmentation/notebook
augmented_audio = np.roll(original_audio, 3000)
```

![](img/082e112ad5cfff1f02568331ff1d125a.png)

音频数据增强：时间平移（图片来源：作者）

请注意，如果没有足够的尾部静音，音频将会环绕。根据你的声音类型，这种数据增强在某些情况下（例如人声）可能不推荐使用。

## 改变速度

你还可以使用`[librosa](https://librosa.org/doc/main/index.html)`库中的`time_stretch()`方法来增加（`rate>1`）或减少（`rate<1`）音频的速度。

```py
# Code copied and edited from https://www.kaggle.com/code/CVxTz/audio-data-augmentation/notebook
rate = 0.9
augmented_audio = librosa.effects.time_stretch(original_audio, rate = rate)
```

![](img/f17f337abe6296ca32c3d88c2c677696.png)

音频数据增强：时间拉伸/改变速度（图片来源：作者）

## 改变音高

或者你可以使用`[librosa](https://librosa.org/doc/main/index.html)`库中的`pitch_shift()`方法来修改音频的音高。

```py
# Code copied and edited from https://www.kaggle.com/code/CVxTz/audio-data-augmentation/notebook
augmented_audio = librosa.effects.pitch_shift(original_audio, sampling_rate, pitch_factor)
```

![](img/70f8b751d84e8f408be6f6692e0903be.png)

音频数据增强：音高变换（图片来源：作者）

## 改变音量（不推荐）

你也可以在音量方面增强波形。然而，如果你打算将波形转换为谱图，**音量增强将无效**，因为幅度在频域中不被考虑。

```py
# Increase volume by 5 dB
augmented_audio = original_audio + 5

# Decrease volume by 5 dB
augmented_audio = original_audio - 5
```

# 频域的音频数据增强（谱图）

在用深度学习模型建模音频数据时，将音频分类问题转化为图像分类问题是一种流行的方法。为此，波形音频数据被转换为 Mel 谱图。如果你需要对 Mel 谱图有进一步了解，推荐阅读以下文章：

[](/audio-deep-learning-made-simple-part-2-why-mel-spectrograms-perform-better-aad889a93505?source=post_page-----15505483c63c--------------------------------) [## 音频深度学习简明教程（第二部分）：为何 Mel 频谱图表现更佳]

### 《Python 音频处理指南》：用简单的英语解释什么是 Mel 频谱图以及如何生成它们。

towardsdatascience.com

你可以使用`melspectrogram()`和`power_to_db()`方法从`[librosa](https://librosa.org/doc/main/index.html)`库将波形音频转换为 Mel 频谱图。

```py
original_melspec = librosa.feature.melspectrogram(y = original_audio,
                                                  sr = sample_rate, 
                                                  n_fft = 512, 
                                                  hop_length = 256, 
                                                  n_mels = 40).T

original_melspec = librosa.power_to_db(original_melspec)
```

![](img/cf21435e81ab505cacb8d357ddf21a01.png)

“Speech Commands” 数据集中“stop”一词的原始音频数据（波形） [1]（图像作者提供）

![](img/5e6dbf4ece82dbf01e814922b47b5c9e.png)

“Speech Commands” 数据集中“stop”一词的原始音频数据作为 Mel 频谱图 [1]（图像作者提供）

尽管你现在面临的是图像分类问题，但在选择频谱图的图像增强技术时，你必须小心。例如，水平翻转频谱图会实质性改变频谱图中包含的信息，因此不推荐这样做。

本节将讨论你可以应用于 Mel 频谱图的流行数据增强技术。

以下代码实现参考自 Kaggle Notebooks，由 [kaerururu](https://www.kaggle.com/kaerunantoka) [7] 和 [DavidS](https://www.kaggle.com/davids1992) [6] 提供。

## Mixup

简单来说，Mixup [4] 通过叠加两个样本并给新样本两个标签来实现样本的合成。

```py
# Code copied and edited from https://www.kaggle.com/code/kaerunantoka/birdclef2022-use-2nd-label-f0
def mixup(original_melspecs, original_labels, alpha=1.0):
    indices = torch.randperm(original_melspecs.size(0))

    lam = np.random.beta(alpha, alpha)

    augmented_melspecs = original_melspecs * lam + original_melspecs[indices] * (1 - lam)
    augmented_labels = [(original_labels * lam), (original_labels[indices] * (1 - lam))]

    return augmented_melspec, augmented_labels
```

![](img/3761f4efb8f2f79422bcd29acd222255.png)

频谱图的数据增强：Mixup [4]（图像作者提供）

另外，你也可以尝试计算机视觉中使用的其他数据增强技术，比如 cutmix [3]。

## SpecAugment

SpecAugment [2] 对频谱图的作用类似于 cutout 对常规图像的作用。虽然 cutout 会遮挡图像中的随机区域，SpecAugment [2] 会遮罩随机频率和时间段。

```py
# Code copied and edited from https://www.kaggle.com/code/davids1992/specaugment-quick-implementation
def spec_augment(original_melspec,
                 freq_masking_max_percentage = 0.15, 
                 time_masking_max_percentage = 0.3):

    augmented_melspec = original_melspec.copy()
    all_frames_num, all_freqs_num = augmented_melspec.shape

    # Frequency masking
    freq_percentage = random.uniform(0.0, freq_masking_max_percentage)
    num_freqs_to_mask = int(freq_percentage * all_freqs_num)
    f0 = int(np.random.uniform(low = 0.0, high = (all_freqs_num - num_freqs_to_mask)))

    augmented_melspec[:, f0:(f0 + num_freqs_to_mask)] = 0

    # Time masking
    time_percentage = random.uniform(0.0, time_masking_max_percentage)
    num_frames_to_mask = int(time_percentage * all_frames_num)
    t0 = int(np.random.uniform(low = 0.0, high = (all_frames_num - num_frames_to_mask)))

    augmented_melspec[t0:(t0 + num_frames_to_mask), :] = 0

    return augmented_melspec
```

![](img/cf7b42d71e651da842088d30b328d256.png)

频谱图的数据增强：SpecAugment [2]（图像作者提供）

另外，如果你使用的是 Pytorch，你还可以使用 `torchaudio` 中的 `TimeMasking` 和 `FrequencyMasking` 增强方法：

```py
import torch
import torchaudio
import torchaudio.transforms as T

time_masking = T.TimeMasking(time_mask_param = 80)
freq_masking = T.FrequencyMasking(freq_mask_param=80)

augmented_melspec = time_masking(original_melspec)
augmented_melspec = freq_masking(augmented_melspec)
```

# 总结

音频数据的增强可以应用于时间域（波形）以及频率域（Mel 频谱图）。为了在深度学习环境中成功应用数据增强，你需要考虑以下处理步骤：

1.  将音频文件加载为波形（时间域）

1.  将数据增强应用于波形

1.  将音频从波形转换为频谱图（频率域）

1.  将数据增强应用于频谱图

本文介绍了波形数据的不同数据增强技术。需要注意的是，某些数据增强技术，如增强音量，在将波形转换为谱图时效果不佳，因为频域中不考虑振幅。

虽然理论上你可以将所有图像增强技术应用于谱图，但并非所有技术都是合理的。例如，垂直或水平翻转会改变谱图的意义。此外，一种专为谱图量身定制的流行 cutout 图像增强变体会遮蔽整个时间戳和频率。

# 喜欢这个故事吗？

[*免费订阅*](https://medium.com/subscribe/@iamleonie) *以便在我发布新故事时收到通知。*

[](https://medium.com/@iamleonie/subscribe?source=post_page-----15505483c63c--------------------------------) [## 每当 Leonie Monigatti 发布新内容时获取电子邮件。

### 每当 Leonie Monigatti 发布新内容时获取电子邮件。通过注册，如果你还没有，你将创建一个 Medium 账户……

medium.com](https://medium.com/@iamleonie/subscribe?source=post_page-----15505483c63c--------------------------------)

*在* [*LinkedIn*](https://www.linkedin.com/in/804250ab/)、[*Twitter*](https://twitter.com/helloiamleonie)*和* [*Kaggle*](https://www.kaggle.com/iamleonie)*上找到我！*

# 参考文献

## 数据集

[1] Warden P. Speech Commands: 用于单词语音识别的公开数据集，2017 年。可从 [`download.tensorflow.org/data/speech_commands_v0.01.tar.gz`](http://download.tensorflow.org/data/speech_commands_v0.01.tar.gz) 获得

许可：CC-BY-4.0

## 文献

[2] Park, D. S., Chan, W., Zhang, Y., Chiu, C. C., Zoph, B., Cubuk, E. D., & Le, Q. V. (2019). Specaugment: 一种用于自动语音识别的简单数据增强方法。 *arXiv 预印本 arXiv:1904.08779*。

[3] Yun, S., Han, D., Oh, S. J., Chun, S., Choe, J., & Yoo, Y. (2019). Cutmix: 正则化策略以训练具有可定位特征的强分类器。 *IEEE/CVF 国际计算机视觉会议论文集*（第 6023–6032 页）。

[4] Zhang, H., Cisse, M., Dauphin, Y. N., & Lopez-Paz, D. (2017) mixup: 超越经验风险最小化。arXiv 预印本 arXiv:1710.09412。

## 代码

[5] [CVxTz](https://www.kaggle.com/CVxTz) (2018). [音频数据增强](https://www.kaggle.com/code/CVxTz/audio-data-augmentation/notebook) 在 Kaggle 笔记本中（访问日期：2023 年 3 月 24 日）。

[6] [DavidS](https://www.kaggle.com/davids1992) (2019). [SpecAugment 快速实现](https://www.kaggle.com/code/davids1992/specaugment-quick-implementation) 在 Kaggle 笔记本中（访问日期：2023 年 3 月 24 日）。

[7] [kaerururu](https://www.kaggle.com/kaerunantoka) (2022). [BirdCLEF2022：使用第二标签 f0](https://www.kaggle.com/code/kaerunantoka/birdclef2022-use-2nd-label-f0) 在 Kaggle 笔记本中（访问日期：2023 年 3 月 24 日）。
