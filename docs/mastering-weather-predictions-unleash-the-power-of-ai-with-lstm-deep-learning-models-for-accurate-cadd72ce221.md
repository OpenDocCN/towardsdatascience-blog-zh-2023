# 掌握天气预报：利用 LSTM 深度学习模型释放 AI 的力量以实现准确的温度预测

> 原文：[`towardsdatascience.com/mastering-weather-predictions-unleash-the-power-of-ai-with-lstm-deep-learning-models-for-accurate-cadd72ce221`](https://towardsdatascience.com/mastering-weather-predictions-unleash-the-power-of-ai-with-lstm-deep-learning-models-for-accurate-cadd72ce221)

## 使用 LSTM 的先进深度学习技术预测温度趋势

[](https://octaviobomfim.medium.com/?source=post_page-----cadd72ce221--------------------------------)![Octavio Santiago](https://octaviobomfim.medium.com/?source=post_page-----cadd72ce221--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cadd72ce221--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cadd72ce221--------------------------------) [Octavio Santiago](https://octaviobomfim.medium.com/?source=post_page-----cadd72ce221--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----cadd72ce221--------------------------------) ·7 分钟阅读·2023 年 3 月 27 日

--

![](img/54483c9868dcecdd790798f64147f9da.png)

作者提供的图片：使用 DALLE-2 生成的天气预测

天气预报是现代世界中最重要的工具之一，开发一个良好的温度预测模型可以为许多企业带来巨大的竞争优势。环境温度测量与农业、能源部门、交易、航空等多个业务领域直接相关。

测量温度是天气预报的重要方面，是气候条件中最基本且广泛使用的指标之一，因为它影响大气气体的行为以及空气和水的循环，这些都是气候系统的关键组成部分。

天气温度测量对许多企业的重要性体现在以下几个关键方面：

+   农业：温度测量用于监测生长条件并预测潜在的作物产量，这有助于农民做出关于种植、收获和储存作物的明智决策。

+   能源和公用事业：温度测量用于预测热浪、寒潮及其他温度极端情况，这有助于公用事业公司计划并应对能源需求的变化。一些公司进行能源交易，而能源需求预测对于获取良好的利润至关重要。

+   运输：温度测量用于预测和监控极端温度，如热浪或寒潮，这些会影响交通系统（如道路和机场）的安全性和效率。

总体而言，温度测量是理解和预测天气及气候条件的基本工具，一个能够准确预测温度的模型对于许多行业的运作至关重要，并且对其他许多行业非常有利。

在这篇文章中，我们将学习如何构建 LSTM 深度学习模型来精确预测温度。

# 数据集

用于训练的数据集来自 INMET 网站，包含来自巴西圣保罗市 SAO PAULO — INTERLAGOS（A771）气象站的气象数据。温度数据的采样频率为每小时，训练数据从 2022 年 3 月 23 日 01:00 到 2023 年 3 月 23 日 12:00，共 1 年。

来源：[`mapas.inmet.gov.br/#`](https://mapas.inmet.gov.br/#)

![](img/bdfe7b6134a652e4698010339aebda00.png)

作者提供的图像：圣保罗地图

+   目标变量：最高温度（ºC）

![](img/79bf5832ecc03e61914d56d2353d4138.png)

作者提供的图像：INMET 温度（最高、平均、最低）

# LSTM

LSTM（长短期记忆）是一种递归神经网络（RNN）架构，特别适合处理序列数据，如时间序列、语音或文本。

与传统的前馈神经网络不同，LSTM 模型的基本构建块是 LSTM 单元，该单元设计用于长期记住和更新信息。每个 LSTM 单元具有一组“门”，控制信息的流入和流出，使网络在处理输入数据时能够选择性地存储或忘记信息。

LSTM 网络通常由多个 LSTM 单元按序列排列组成。输入数据一次通过一个时间步的单元，前一个单元的输出用作下一个单元的输入。这使得网络能够学习和建模数据中的复杂序列模式。

![](img/319085d9a268b6674082d5b7c8110f93.png)

作者提供的图像：LSTM 架构

# 训练和基准

训练时间序列模型的主要结果是如何正确地将数据集拆分为训练集和测试集。由于序列很重要，我们不能随意拆分数据集，为了正确拆分数据集，使用了 sklearn 函数“TimeSeriesSplit”。

# **LSTM Vanilla**

LSTM vanilla（或“vanilla LSTM”）指的是一种长短期记忆（LSTM）神经网络架构，是 LSTM 模型的基本或标准版本。

## **训练**

+   18 天的采样时间

+   数据集按小时分隔

+   80% 训练数据集

![](img/29c59fc9d96c249e345ad70c12984c85.png)

作者提供的图像：训练 LSTM Vanilla

**训练的回归指标：**

+   MAE（平均绝对误差）：0.53

+   MSE（均方误差）：0.60

+   MAPE（平均绝对百分比误差）：2.9%

**问题：**

+   温度上升的延迟和峰值识别

## **测试**

+   数据集的 20%

![](img/ba0edd3b4cd8352826225a9031078574.png)

作者提供的图像：测试 LSTM vanilla

**训练的回归指标：**

+   MAE（平均绝对误差）：0.59

+   MSE（均方误差）：0.82

+   MAPE（均绝对百分比误差）：2.5%

## **预测**

+   48 小时的预测

![](img/35975a572b3ebc3b7e7ad5741dff9212.png)

作者提供的图像：预测 LSTM Vanilla

# **LSTM 堆叠**

堆叠 LSTM（长短期记忆）是一种流行 LSTM 循环神经网络架构的变体。在标准 LSTM 中，使用单层 LSTM 单元来处理序列数据。在堆叠 LSTM 中，使用多个 LSTM 单元层，其中一层的输出作为下一层的输入。

堆叠 LSTM 架构用于提高网络学习和建模数据中复杂序列模式的能力。堆叠中的每一层可以学习不同层次的抽象，并从输入数据中提取特征，这使得网络能够捕捉数据中的更复杂关系和依赖性。

## **训练**

+   18 天的样本时间

+   数据集按小时分隔

+   80% 训练数据集

![](img/8912f8845eb5a97aa7fef3270e6b3b60.png)

作者提供的图像：训练 LSTM 堆叠

**训练的回归指标：**

+   MAE（均绝对误差）：0.52

+   MSE（均方误差）：0.57

+   MAPE（均绝对百分比误差）：2.8%

**问题：**

+   温度上升和峰值识别的延迟

+   对未来预测存在问题

## **测试**

+   数据集的 20%

![](img/7025f5a2f623e1afb3e5788423322a95.png)

作者提供的图像：测试 LSTM 堆叠

**训练的回归指标：**

+   MAE（均绝对误差）：0.57

+   MSE（均方误差）：0.82

+   MAPE（均绝对百分比误差）：2.4%

## **预测**

+   48 小时的预测

![](img/7be57654a5922ece79cb57f54a63534d.png)

作者提供的图像：预测 LSTM 堆叠

# 评估预测 — LSTM Vanilla

在对每个模型进行训练和测试后，比较它们效率的最一致方法是与新的实际数据进行对比。对 LSTM Vanilla 和堆叠模型进行了未来 48 小时的预测，并与实际数据（黄色）进行比较，如下所示：

![](img/93fbcce8eeb2bf07064f309392b60dcb.png)

作者提供的图像：LSTM Vanilla、LSTM 堆叠和实际温度对比

**LSTM Vanilla**

+   平均误差：2.04 ºC

+   最大误差：4.44ºC

**LSTM 堆叠**

+   平均误差：1.36 ºC

+   最大误差：3.16ºC

![](img/299d79057c1752c06422ca5b17c0997e.png)

作者提供的图像：MAE 比较

# 结论

在 48 小时的预测中，堆叠 LSTM 模型显示出较低的平均绝对误差和较低的最大温度误差，表明对这些 LSTM 模型及拟合数据来说，增加模型复杂性是有益的。

在 48 小时预测中，Stacked LSTM 模型显示出相较于 Vanilla 模型的平均绝对误差和最大温度误差均有所减少。这些发现表明，模型中加入的额外复杂性对 LSTM 架构是有利的。这些额外层的引入使得模型能够捕捉数据中的更复杂的模式和依赖性，从而提升其在预测任务中的性能。观察到的误差减少表明，增加的复杂性使得模型能够捕捉到更多的基础模式和温度数据的变异性，从而实现更准确的预测。

两个模型在预测未来 48 小时的温度值时均表现出较高的精确度，平均误差率约为 2%和 2ºC。然而，两个模型都遇到了准确识别与预期结果不同的次级温度谷的挑战。这一问题突显了模型在捕捉和解释温度数据中的复杂模式时的潜在限制，尤其是在出现意外波动或异常时。可能需要进一步研究以提高模型对这些不规则性的敏感性，并增强其在温度预测中的整体表现。

非常感谢您的阅读！如有任何问题或建议，请通过 LinkedIn 联系我：[`www.linkedin.com/in/octavio-b-santiago/`](https://www.linkedin.com/in/octavio-b-santiago/)

如果您想实现此解决方案或了解更多关于 LSTM 算法的内容，可以在我的 GitHub 库中找到完整的 Python 代码，链接如下：

# 代码

[](https://github.com/octavio-santiago/temperature-forecasting?source=post_page-----cadd72ce221--------------------------------) [## GitHub - octavio-santiago/temperature-forecasting: 温度预测模型与 AI - Python

### 温度预测模型与 AI - Python 本库包含了用于训练和评估 LSTM 深度学习的代码…

[github.com](https://github.com/octavio-santiago/temperature-forecasting?source=post_page-----cadd72ce221--------------------------------)

## 参考文献

数据来源：[`mapas.inmet.gov.br/#`](https://mapas.inmet.gov.br/#) — INMET（国家气象研究所）公开数据集：CC0 1.0 通用许可证 ([`portal.inmet.gov.br/sobre`](https://portal.inmet.gov.br/sobre))
