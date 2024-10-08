# TimeGPT: 第一个用于时间序列预测的基础模型

> 原文：[`towardsdatascience.com/timegpt-the-first-foundation-model-for-time-series-forecasting-bf0a75e63b3a`](https://towardsdatascience.com/timegpt-the-first-foundation-model-for-time-series-forecasting-bf0a75e63b3a)

## 探索第一个生成式预训练预测模型，并在 Python 项目中应用它

[](https://medium.com/@marcopeixeiro?source=post_page-----bf0a75e63b3a--------------------------------)![Marco Peixeiro](https://medium.com/@marcopeixeiro?source=post_page-----bf0a75e63b3a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bf0a75e63b3a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bf0a75e63b3a--------------------------------) [Marco Peixeiro](https://medium.com/@marcopeixeiro?source=post_page-----bf0a75e63b3a--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bf0a75e63b3a--------------------------------) ·阅读时间 12 分钟·2023 年 10 月 24 日

--

![](img/69c006f4feeba3f9fe392aff3b2a35f0.png)

图片由 [Boris Smokrovic](https://unsplash.com/@borisworkshop?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

时间序列预测领域正经历一个非常激动人心的时期。在过去三年里，我们见证了许多重要的贡献，比如 [N-BEATS](https://medium.com/towards-data-science/the-easiest-way-to-forecast-time-series-using-n-beats-d778fcc2ba60)、[N-HiTS](https://medium.com/towards-data-science/all-about-n-hits-the-latest-breakthrough-in-time-series-forecasting-a8ddcb27b0d5)、[PatchTST](https://medium.com/towards-data-science/patchtst-a-breakthrough-in-time-series-forecasting-e02d48869ccc) 和 [TimesNet](https://medium.com/towards-data-science/timesnet-the-latest-advance-in-time-series-forecasting-745b69068c9c)。

与此同时，[大型语言模型（LLMs）](https://medium.com/towards-data-science/catch-up-on-large-language-models-8daf784f46f8) 近年来获得了极大的关注，例如 ChatGPT，因为它们可以在没有进一步训练的情况下适应各种任务。

这引出了一个问题：基础模型是否可以像自然语言处理一样存在于时间序列中？一个在大量时间序列数据上预训练的大模型是否可以对未见数据做出准确预测？

通过 [TimeGPT-1](https://arxiv.org/pdf/2310.03589.pdf)，由 Azul Garza 和 Max Mergenthaler-Canseco 提出，作者将 LLM 的技术和架构适配到预测领域，成功构建了第一个能够进行零样本推理的时间序列基础模型。

在这篇文章中，我们首先探讨了 TimeGPT 背后的架构以及模型的训练方式。然后，我们将其应用于一个预测项目，以评估其与其他先进方法（如 N-BEATS、N-HiTS 和 PatchTST）的表现。

欲了解更多细节，请务必阅读 [原始论文](https://arxiv.org/pdf/2310.03589.pdf)。

> ***使用我的*** [***免费时间序列速查表***](https://www.datasciencewithmarco.com/pl/2147608294) ***学习最新的时间序列分析技术，全部用 Python 实现！获得统计和深度学习技术的实现，全都用 Python 和 TensorFlow！***

开始吧！

# 探索 TimeGPT

如前所述，TimeGPT 是创建时间序列预测基础模型的首次尝试。

![](img/5a18b137f0b737e2e4df634cbc8111b2.png)

TimeGPT 如何被训练以对未见数据进行推断的示意图。图片来自 [TimeGPT-1](https://arxiv.org/pdf/2310.03589.pdf) 的 Azul Garza 和 Max Mergenthaler-Canseco

从上图可以看出，TimeGPT 的总体思路是通过在来自不同领域的大量数据上训练模型，从而对未见数据进行零样本推断。

当然，这种方法依赖于 **迁移学习**，即模型利用在训练过程中获得的知识来解决新任务的能力。

现在，这只有在模型足够大且在大量数据上训练时才有效。

## 训练 TimeGPT

为此，作者在超过 1000 亿个数据点上训练了 TimeGPT，这些数据点都来自开源时间序列数据。数据集涵盖了广泛的领域，包括金融、经济和天气、网页流量、能源和销售。

> 请注意，作者未披露用于整理 1000 亿数据点的公共数据来源。

这种多样性对于基础模型的成功至关重要，因为它可以学习不同的时间模式，从而实现更好的泛化。

例如，我们可以预期天气数据会有日周期性（白天比夜晚更热）和年度季节性，而汽车交通数据则可能具有日周期性（白天路上的车更多）和周周期性（工作日路上的车更多）。

为了确保模型的稳健性和泛化能力，预处理保持在最低限度。实际上，仅填充了缺失值，其余数据保持原始状态。虽然作者没有具体说明数据填补的方法，但我怀疑使用了一些插值技术，如线性插值、样条插值或移动平均插值。

模型随后在多天的时间里进行了训练，在此期间对超参数和学习率进行了优化。虽然作者未透露训练所需的天数和 GPU 数量，但我们知道该模型是用 PyTorch 实现的，并使用了 Adam 优化器和学习率衰减策略。

## TimeGPT 的架构

TimeGPT 利用基于 Google 和多伦多大学 2017 年开创性工作的自注意力机制的 Transformer 架构。

![](img/cf13280001ad8725c402657bb8fe31eb.png)

TimeGPT 的架构。输入序列以及外生变量被送入 Transformer 的编码器，解码器随后生成预测。图像来自 [TimeGPT-1](https://arxiv.org/pdf/2310.03589.pdf) 的 Azul Garza 和 Max Mergenthaler-Canseco。

从上图中，我们可以看到 TimeGPT 使用了完整的编码器-解码器 Transformer 架构。

输入可以包括一段历史数据窗口以及外生数据，如特定事件或其他序列。

输入被送入模型的编码器部分。编码器内部的注意力机制然后学习来自输入的不同特性。这些信息随后被送入解码器，解码器利用学到的信息来生成预测。当然，预测序列会在达到用户设置的预测范围长度时结束。

重要的是要注意，作者在 TimeGPT 中实现了符合预测功能，使模型能够根据历史误差估计预测区间。

## TimeGPT 的能力

考虑到 TimeGPT 是首次尝试构建时间序列的基础模型，它具备了广泛的功能。

首先，由于 TimeGPT 是一个预训练模型，这意味着我们可以在不专门在我们的数据上训练它的情况下生成预测。不过，也可以对模型进行微调以适应我们的数据。

其次，该模型支持外生变量来预测我们的目标，并且可以处理多变量预测任务。

最终，通过使用符合预测，TimeGPT 可以估计预测区间。这反过来允许模型执行异常检测。基本上，如果一个数据点落在 99% 置信区间之外，模型就会将其标记为异常。

请记住，所有这些任务都可以通过零样本推理或一些微调来实现，这对时间序列预测领域来说是一次根本性的范式转变。

现在我们对 TimeGPT 有了更扎实的理解，了解了它的工作原理和训练方式，让我们看看模型的实际表现。

# 使用 TimeGPT 进行预测

现在，让我们将 TimeGPT 应用于一个预测任务，并将其表现与其他模型进行比较。

请注意，在撰写本文时，TimeGPT 仅通过 API 访问，并处于封闭测试阶段。我提交了申请，并获得了为期两周的免费访问模型权限。要获取令牌并访问模型，你必须访问他们的 [网站](https://www.nixtla.io/)。

如前所述，该模型在来自公开数据的 1000 亿数据点上进行训练。由于作者没有指定实际使用的数据集，我认为在已知的基准数据集上测试模型是不合理的，例如 [ETT](https://paperswithcode.com/dataset/ett) 或 [weather](https://www.kaggle.com/datasets/mnassrib/jena-climate)，因为模型在训练过程中可能已经见过这些数据。

因此，我编制并开源了我自己的数据集用于本文。

具体来说，我整理了从 2020 年 1 月 1 日到 2023 年 10 月 12 日我博客的每日浏览量。我还添加了两个外生变量：一个是标记新文章发布的日子，另一个是标记美国的假期日子，因为我的大多数观众都在美国。

数据集现已在 [GitHub](https://github.com/marcopeix/time-series-analysis/blob/master/data/medium_views_published_holidays.csv) 上公开，并且最重要的是，我们确定 TimeGPT 没有在此数据上进行训练。

像往常一样，你可以在 [GitHub](https://github.com/marcopeix/time-series-analysis/blob/master/TimeGPT.ipynb) 上访问完整的笔记本。

## 导入库并读取数据

自然的第一步是导入实验所需的库。

```py
import pandas as pd
import numpy as np
import datetime
import matplotlib.pyplot as plt

from neuralforecast.core import NeuralForecast
from neuralforecast.models import NHITS, NBEATS, PatchTST

from neuralforecast.losses.numpy import mae, mse

from nixtlats import TimeGPT

%matplotlib inline
```

然后，为了访问 TimeGPT 模型，我们从文件中读取 API 密钥。请注意，我没有将 API 密钥分配给环境变量，因为访问仅限于两周。

```py
with open("data/timegpt_api_key.txt", 'r') as file:
        API_KEY = file.read()
```

然后，我们可以读取数据。

```py
df = pd.read_csv('data/medium_views_published_holidays.csv')
df['ds'] = pd.to_datetime(df['ds'])

df.head()
```

![](img/dfd146f8cdfc8f777aac50ca4a8472e6.png)

我们数据集的前五行。图片由作者提供。

从上图中，我们可以看到数据集的格式与我们使用其他来自 Nixtla 的开源库时相同。

我们有一个 *unique_id* 列来标记不同的时间序列，但在我们的情况下，我们只有一个序列。

列 *y* 代表我博客上的每日浏览量，*published* 是一个简单的标志，用于标记一天是否有新文章发布（1）或没有文章发布（0）。直观上，我们知道当发布新内容时，浏览量通常会在一段时间内增加。

最后，列 *is_holiday* 指示是否为美国假期。直观上，假期期间访问我博客的人较少。

现在，让我们可视化数据并寻找明显的模式。

```py
published_dates = df[df['published'] == 1]

fig, ax = plt.subplots(figsize=(12,8))

ax.plot(df['ds'], df['y'])
ax.scatter(published_dates['ds'], published_dates['y'], marker='o', color='red', label='New article')
ax.set_xlabel('Day')
ax.set_ylabel('Total views')
ax.legend(loc='best')

fig.autofmt_xdate()

plt.tight_layout()
```

![](img/2bcfd0a41d8a036f517aecf0be949731.png)

我博客上的每日浏览量。图片由作者提供。

从上图中，我们已经可以看到一些有趣的行为。首先，注意红点表示新发布的文章，它们几乎立即伴随着访问量的峰值。

我们还注意到 2021 年的活动较少，这在我博客的每日浏览量减少中体现出来。最后，在 2023 年，我们注意到在文章发布后访问量出现一些异常峰值。

放大数据后，我们还发现了明显的每周季节性。

![](img/2a9cb296a60b240f8c5df3f584f588dc.png)

我的博客每日浏览量。在这里，我们看到明显的每周季节性，周末访问人数较少。图片由作者提供。

从上图中，我们现在可以看到周末来博客的访客比工作日少。

了解了这些之后，让我们看看如何利用 TimeGPT 进行预测。

## 使用 TimeGPT 进行预测

首先，将数据集拆分为训练集和测试集。在这里，我会保留 168 个时间步作为测试集，这对应于 24 周的每日数据。

```py
train = df[:-168]
test = df[-168:]
```

然后，我们使用七天的预测范围，因为我有兴趣预测一整周的每日浏览量。

目前，API 没有交叉验证的实现。因此，我们创建了自己的循环，每次生成七个预测，直到我们对整个测试集都有预测。

```py
future_exog = test[['unique_id', 'ds', 'published', 'is_holiday']]

timegpt = TimeGPT(token=API_KEY)

timegpt_preds = []

for i in range(0, 162, 7):

    timegpt_preds_df = timegpt.forecast(
        df=df.iloc[:1213+i],
        X_df = future_exog[i:i+7],
        h=7,
        finetune_steps=10,
        id_col='unique_id',
        time_col='ds',
        target_col='y'
    )

    preds = timegpt_preds_df['TimeGPT']

    timegpt_preds.extend(preds)
```

在上面的代码块中，请注意我们必须传递外生变量的未来值。这是可以的，因为它们是静态变量。我们知道假期的未来日期，博客作者也知道他计划发布文章的时间。

同样注意，我们使用*finetune_steps*参数对 TimeGPT 进行微调以适应我们的数据。

一旦循环完成，我们可以将预测添加到测试集中。再次，TimeGPT 每次生成七个预测，直到获得 168 个预测，以便我们评估其预测下周每日浏览量的能力。

```py
test['TimeGPT'] = timegpt_preds

test.head()
```

![](img/7568508f0602da67a14bc017d7b8893c.png)

TimeGPT 的预测。图片由作者提供。

## 使用 N-BEATS、N-HiTS 和 PatchTST 进行预测

现在，让我们应用其他方法，看看是否专门在我们的数据集上训练这些模型可以产生更好的预测。

对于这个实验，如前所述，我们使用了 N-BEATS、N-HiTS 和 PatchTST。

```py
horizon = 7

models = [NHITS(h=horizon,
               input_size=5*horizon,
               max_steps=50),
         NBEATS(h=horizon,
               input_size=5*horizon,
               max_steps=50),
         PatchTST(h=horizon,
                 input_size=5*horizon,
                 max_steps=50)]
```

然后，我们初始化*NeuralForecast*对象，并指定数据的频率，在这种情况下是每日。

```py
nf = NeuralForecast(models=models, freq='D')
```

接着，我们在 24 个 7 时间步的窗口上进行交叉验证，以获得与 TimeGPT 使用的测试集对齐的预测。

```py
preds_df = nf.cross_validation(
    df=df, 
    static_df=future_exog , 
    step_size=7, 
    n_windows=24
)
```

然后，我们可以将 TimeGPT 的预测简单地添加到这个新的*preds_df* DataFrame 中，以便拥有一个包含所有模型预测的单一 DataFrame。

```py
preds_df['TimeGPT'] = test['TimeGPT']
```

![](img/88ac6201d63fccff2ba447acfcdfb5e3.png)

包含所有模型预测的 DataFrame。图片由作者提供。

太好了！我们现在准备评估每个模型的性能。

## 评估

在测量性能指标之前，让我们可视化每个模型在测试集上的预测。

![](img/1dae3bf9da71a92143bbb581ee80e438.png)

可视化每个模型的预测。图片由作者提供。

首先，我们看到每个模型之间有很多重叠。然而，我们确实注意到 N-HiTS 预测了两个在现实中未实现的峰值。此外，PatchTST 似乎经常低估预测。然而，TimeGPT 似乎总体上与实际数据重叠得相当好。

当然，评估每个模型性能的唯一方法是测量性能指标。在这里，我们使用均值绝对误差（MAE）和均方误差（MSE）。此外，我们将预测结果四舍五入为整数，因为在博客的日常访问者上下文中，小数没有意义。

```py
preds_df = preds_df.round({
    'NHITS': 0,
    'NBEATS': 0,
    'PatchTST': 0,
    'TimeGPT': 0
})

data = {'N-HiTS': [mae(preds_df['NHITS'], preds_df['y']), mse(preds_df['NHITS'], preds_df['y'])],
       'N-BEATS': [mae(preds_df['NBEATS'], preds_df['y']), mse(preds_df['NBEATS'], preds_df['y'])],
       'PatchTST': [mae(preds_df['PatchTST'], preds_df['y']), mse(preds_df['PatchTST'], preds_df['y'])],
       'TimeGPT': [mae(preds_df['TimeGPT'], preds_df['y']), mse(preds_df['TimeGPT'], preds_df['y'])]}

metrics_df = pd.DataFrame(data=data)
metrics_df.index = ['mae', 'mse']

metrics_df.style.highlight_min(color='lightgreen', axis=1)
```

![](img/b5f5589624a057c73f2c8b3685b85464.png)

每个模型的性能指标。在这里，TimeGPT 是冠军模型，因为它实现了最低的 MAE 和 MSE。图片由作者提供。

从上图中，我们可以看到 TimeGPT 是冠军模型，因为它实现了最低的 MAE 和 MSE，紧随其后的是 N-BEATS、PatchTST 和 N-HiTS。

这是一个令人兴奋的结果，因为 TimeGPT 从未见过这个数据集，只进行了少量的微调。虽然这不是一个详尽的实验，但我相信它确实展示了基础模型在预测领域的潜力。

# 我对 TimeGPT 的个人看法

尽管我对 TimeGPT 的短期实验令人兴奋，但我必须指出，[原始论文](https://arxiv.org/pdf/2310.03589.pdf) 在许多重要领域仍然模糊。

再次强调，我们不知道用于训练和测试模型的数据集，因此无法真正验证 TimeGPT 的性能结果，如下所示。

![](img/3d0b4e177853479b015c216be4d57815.png)

TimeGPT 的性能结果如 [原始论文](https://arxiv.org/pdf/2310.03589.pdf) 所述，由 Azul Garza 和 Max Mergenthaler-Canseco 提供。

从上表中可以看出，TimeGPT 在月度和每周频率下表现最佳，N-HiTS 和 Temporal Fusion Transformer (TFT) 通常排名第 2 或第 3。再者，由于我们不知道使用了什么数据，我们无法验证这些指标。

关于模型的训练方式以及如何适应时间序列数据，也缺乏透明度。

我相信该模型旨在商业使用，这解释了为什么论文缺乏重现 TimeGPT 的细节。这并没有错，但论文缺乏可重现性对科学界来说是个问题。

尽管如此，我希望这能激发在时间序列领域基础模型的新工作和研究，并且我们最终能看到这些模型的开源版本，就像我们看到 LLM 的情况一样。

# 结论

TimeGPT 是第一个用于时间序列预测的基础模型。

它利用了 Transformer 架构，并在 1000 亿数据点上进行了预训练，以对新的未见数据进行零样本推理。

结合符合预测技术，该模型可以生成预测区间并执行异常检测，而无需在特定数据集上进行训练。

我仍然相信每个预测问题需要独特的方法，因此请确保测试 TimeGPT 以及其他模型。

感谢阅读！希望你喜欢这篇文章，并学到了新东西！

想要掌握时间序列预测吗？那就来看看我的课程[Python 中的应用时间序列预测](https://www.datasciencewithmarco.com/offers/zTAs2hi6/checkout?coupon_code=ATSFP10)。这是唯一一个通过 Python 实现统计学、深度学习和最先进模型的课程，包含 16 个指导性的实践项目。

干杯 🍻

# 支持我

喜欢我的工作吗？通过[请我喝咖啡](http://buymeacoffee.com/dswm)来支持我，这是一种简单的鼓励方式，我可以享受一杯咖啡！如果你愿意，只需点击下面的按钮 👇

![](img/a90a701107c4ea11414ef27bd59465af.png)

# 参考文献

[TimeGPT-1](https://arxiv.org/pdf/2310.03589.pdf) 由 Azul Garza 和 Max Mergenthaler-Canseco 编写
