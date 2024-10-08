# 梯度提升：预测中的银弹

> 原文：[`towardsdatascience.com/gradient-boosting-a-silver-bullet-in-forecasting-5820ba7182fd`](https://towardsdatascience.com/gradient-boosting-a-silver-bullet-in-forecasting-5820ba7182fd)

## 我们展示了梯度提升在时间序列预测中非常强大，并尝试解释原因

[](https://medium.com/@davide.burba?source=post_page-----5820ba7182fd--------------------------------)![Davide Burba](https://medium.com/@davide.burba?source=post_page-----5820ba7182fd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5820ba7182fd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5820ba7182fd--------------------------------) [Davide Burba](https://medium.com/@davide.burba?source=post_page-----5820ba7182fd--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5820ba7182fd--------------------------------) ·6 分钟阅读·2023 年 7 月 20 日

--

![](img/6df1867e40446de2efa71a90ee5550c2.png)

“梯度提升”，由 [Giulia Roggia](https://www.instagram.com/giulia_roggia__/)。经许可使用。

+   什么是梯度提升？

+   梯度提升作为银弹

+   为什么梯度提升如此出色？

+   注意事项

+   附录：竞赛和发布解决方案列表

时间序列预测在许多领域中都非常重要，包括金融、销售和天气预测。尽管经典的时间序列模型和深度学习技术已被广泛使用，但有越来越多的证据表明，梯度提升通常优于其他方法。

# 什么是梯度提升？

[梯度提升](https://en.wikipedia.org/wiki/Gradient_boosting) 是一种机器学习技术，通过按顺序组合一组弱学习者来构建预测模型。它旨在通过迭代最小化前一个模型的误差来创建一个强学习者。其核心思想是将后续模型拟合到前一个模型的残差上，通过每次迭代逐步提高预测精度。

![](img/0574a91103d1b19408d9912ebd17c86b.png)

图片由作者提供。

[LightGBM](https://lightgbm.readthedocs.io/en/v3.3.2/) 和 [XGBoost](https://xgboost.readthedocs.io/en/stable/) 是两个著名的实现梯度提升算法的库。它们因高效、可扩展和卓越的性能而受到欢迎。

尽管梯度提升并非专门为时间序列数据设计，但我们可以通过特征工程步骤将其用于预测。你可以查看[这篇文章](https://medium.com/towards-data-science/why-backtesting-matters-and-how-to-do-it-right-731fb9624a)以获取具体的示例。

# 梯度提升作为银弹

我们可以通过检视竞赛的获胜解决方案来评估某一领域内最强的模型。获胜的解决方案有时会因为过于复杂且难以在生产环境中复制而受到批评。然而，当某个特定模型在不同竞赛中 consistently 出现于获胜解决方案中时，它展示了应对复杂挑战的能力。

最著名的数据科学竞赛平台是 [Kaggle](https://www.kaggle.com/)。让我们来看看过去 10 年中进行的货币预测竞赛（见下图）。

![](img/dba08f3ecb10bee2654931cad88b6179.png)

这是从 [Kaggle](https://www.kaggle.com/) 截取的屏幕截图，拍摄于 2023 年 5 月。

顶级解决方案通常会在竞赛结束后发布在讨论区。在本分析中，我们考虑了那些进入前 10 的发布解决方案（你可以在附录中查看详情）。在这些解决方案中，我们看到：

+   **所有竞赛都有使用梯度提升的顶级解决方案**（通常是 LightGBM）

+   如果我们只考虑每个竞赛的单一顶级发布解决方案，我们会发现**它们** **在 8 个案例中有 6 个是基于梯度提升的**（另外两个基于深度学习模型，即 [*MLB*](https://www.kaggle.com/competitions/mlb-player-digital-engagement-forecasting/discussion/274255) 和 [*Wikipedia*](https://www.kaggle.com/competitions/web-traffic-time-series-forecasting/discussion/43795)） 

我们不得不放弃最早的两个竞赛，因为没有发布解决方案，以及“*M5 不确定性*”竞赛，因为一些解决方案基于“*M5 准确性*”（这些解决方案通常基于 LightGBM，包括[获胜者](https://www.kaggle.com/competitions/m5-forecasting-accuracy/discussion/163684)）。

# 为什么梯度提升如此出色？

有几个因素使得梯度提升成为一个强大的模型：

1.  **处理非线性关系**：传统的时间序列模型通常难以捕捉数据中的非线性模式。另一方面，梯度提升可以自动学习变量之间的复杂关系。特别是，基于树的结构能够学习突变，这在表格数据中很常见，并且对于深度学习模型而言更难以学习。

1.  **全局模型**：梯度提升允许训练全局模型，即在多个时间序列上训练单个模型。这通常比使用每个时间序列一个模型的方式（传统时间序列模型使用的方式）能得到更好的预测效果。有关局部模型与全局模型的更多信息，你可以查看[这篇文章](https://medium.com/towards-data-science/local-vs-global-forecasting-what-you-need-to-know-1cc29e66cae0)。

1.  **分类特征**：LightGBM 的一个优点是能够处理分类特征，而无需进行 one-hot 编码，这通常效率不高，或者由于内存限制甚至不可行。

1.  **微调**：除了特征工程外，我们可以微调的选项范围很广，以改善基于梯度提升的模型，包括选择自定义损失函数和参数，以提高对无信息特征的鲁棒性。

1.  **快速且可扩展**：LightGBM 和 XGBoost 在设计时考虑了效率和可扩展性。这些库采用了先进的优化技术和并行处理，使得处理大规模数据成为可能。

有多个资源对梯度提升进行基准测试，并尝试解释为何梯度提升在表格数据上表现优异，这些资源通常也适用于时间序列预测。以下是一些这些资源：

+   [为什么基于树的模型在表格数据上仍优于深度学习？](https://arxiv.org/abs/2207.08815)

+   [深度学习 vs. 梯度提升：信用评分的前沿机器学习算法基准测试](https://arxiv.org/abs/2205.10535#:~:text=The%20experiment%20has%20shown%20that,and%20choice%20for%20credit%20scoring.)

+   [表格数据：深度学习并非唯一选择](https://arxiv.org/abs/2106.03253)

# 警告

我们看到梯度提升是时间序列预测的强大模型。然而，有些情况下使用其他模型可能更为合适：

+   **小而规则的数据**：如果数据非常小（例如仅有少数样本的单一时间序列）且平滑，传统方法如指数平滑或 ARIMA 可能更为合适。原因在于梯度提升仍需要最少量的数据来拟合和调整超参数。另一方面，传统时间序列模型对生成数据的随机过程有较强的假设，这使得参数估计更为容易。在实践中，这种数据在实际问题中很少见。

+   **大规模数据**：梯度提升相当可扩展。然而，如果我们谈论的是数十亿的数据样本，可能更倾向于使用深度学习方法。除了深度学习模型在大数据上表现优越外，它们还可以不断训练，而梯度提升模型通常需要从头开始训练。此外，它们在分类特征方面通过嵌入提供了更好的可扩展性。

+   **极低的信噪比**：虽然梯度提升对噪声相当鲁棒，但在一些极端情况下（例如股票价格预测），可能会导致过拟合。如果是这样，可能值得尝试一些较不强大但更为鲁棒的模型（例如线性模型）。

# 附录：竞赛和已发布解决方案列表

下面你可以找到为本文考虑的竞赛列表和顶级公开解决方案。

[](https://www.kaggle.com/competitions/march-machine-learning-mania-2023?source=post_page-----5820ba7182fd--------------------------------) [## 2023 年三月机器学习狂欢]

### 预测 2023 年 NCAA 篮球锦标赛

[www.kaggle.com](https://www.kaggle.com/competitions/march-machine-learning-mania-2023?source=post_page-----5820ba7182fd--------------------------------)

+   [第 1 名，XGBoost](https://www.kaggle.com/competitions/march-machine-learning-mania-2023/discussion/399553)

+   [第 2 名，包含 XGBoost 的集成方法](https://www.kaggle.com/competitions/march-machine-learning-mania-2023/discussion/401578)

+   [第 3 名 XGBoost](https://www.kaggle.com/competitions/march-machine-learning-mania-2023/discussion/401641)

+   [第 5 名，XGBoost](https://www.kaggle.com/competitions/march-machine-learning-mania-2023/discussion/401382)

+   [第 7 名，包含 XGBoost 的集成方法](https://www.kaggle.com/competitions/march-machine-learning-mania-2023/discussion/400116)

+   [第 8 名，包含 LightGBM 的集成方法](https://www.kaggle.com/competitions/march-machine-learning-mania-2023/discussion/400834)

+   [第 9 名，包含 XGBoost 的集成方法](https://www.kaggle.com/competitions/march-machine-learning-mania-2023/discussion/400151)

[](https://www.kaggle.com/competitions/g-research-crypto-forecasting?source=post_page-----5820ba7182fd--------------------------------) [## G-Research 加密货币预测

### 运用你的机器学习专业知识预测真实的加密货币市场数据

[www.kaggle.com](https://www.kaggle.com/competitions/g-research-crypto-forecasting?source=post_page-----5820ba7182fd--------------------------------)

+   [第 2 名，LightGBM](https://www.kaggle.com/competitions/g-research-crypto-forecasting/discussion/323098)

+   [第 3 名，LightGBM](https://www.kaggle.com/competitions/g-research-crypto-forecasting/discussion/323703)

+   [第 7 名，Transformers](https://www.kaggle.com/competitions/g-research-crypto-forecasting/discussion/323250)

[](https://www.kaggle.com/competitions/mlb-player-digital-engagement-forecasting?source=post_page-----5820ba7182fd--------------------------------) [## MLB 球员数字互动预测

### 预测棒球选手数字内容的粉丝互动

[www.kaggle.com](https://www.kaggle.com/competitions/mlb-player-digital-engagement-forecasting?source=post_page-----5820ba7182fd--------------------------------)

+   [第 1 名，GRU](https://www.kaggle.com/competitions/mlb-player-digital-engagement-forecasting/discussion/274255)

+   [第 2 名，LightGBM 和 XGBoost](https://www.kaggle.com/competitions/mlb-player-digital-engagement-forecasting/discussion/274661)

+   [第 3 名，包含 LightGBM 的集成方法](https://www.kaggle.com/competitions/mlb-player-digital-engagement-forecasting/discussion/256620)

+   [第 5 名，包含 LightGBM 和 CatBoost 的集成方法](https://www.kaggle.com/competitions/mlb-player-digital-engagement-forecasting/discussion/271345)

+   [第 6 名，包含 LightGBM 和 CatBoost 的集成方法](https://www.kaggle.com/competitions/mlb-player-digital-engagement-forecasting/discussion/271890)

+   [第 8 名，LSTM](https://www.kaggle.com/competitions/mlb-player-digital-engagement-forecasting/discussion/271683)

[](https://www.kaggle.com/competitions/m5-forecasting-accuracy?source=post_page-----5820ba7182fd--------------------------------) [## M5 预测 — 准确性

### 估计沃尔玛零售商品的单位销售

www.kaggle.com](https://www.kaggle.com/competitions/m5-forecasting-accuracy?source=post_page-----5820ba7182fd--------------------------------)

+   [第 1 名，LightGBM](https://www.kaggle.com/competitions/m5-forecasting-accuracy/discussion/163684)

+   [第 2 名，LightGBM](https://www.kaggle.com/competitions/m5-forecasting-accuracy/discussion/164599)

+   [第 3 名，修改版 DeepAR](https://www.kaggle.com/competitions/m5-forecasting-accuracy/discussion/164374)

+   [第 4 名，LightGBM](https://www.kaggle.com/competitions/m5-forecasting-accuracy/discussion/163216)

[](https://www.kaggle.com/competitions/m5-forecasting-uncertainty?source=post_page-----5820ba7182fd--------------------------------) [## M5 预测 — 不确定性

### 估计沃尔玛单位销售的不确定性分布。

www.kaggle.com](https://www.kaggle.com/competitions/m5-forecasting-uncertainty?source=post_page-----5820ba7182fd--------------------------------)

跳过：基于 M5 准确性的解决方案。

[](https://www.kaggle.com/competitions/recruit-restaurant-visitor-forecasting?source=post_page-----5820ba7182fd--------------------------------) [## Recruit 餐厅访客预测

### 预测餐厅未来将接待多少访客

www.kaggle.com](https://www.kaggle.com/competitions/recruit-restaurant-visitor-forecasting?source=post_page-----5820ba7182fd--------------------------------)

+   [第 7 名，LightGBM](https://www.kaggle.com/competitions/recruit-restaurant-visitor-forecasting/discussion/49166)

[](https://www.kaggle.com/competitions/favorita-grocery-sales-forecasting?source=post_page-----5820ba7182fd--------------------------------) [## Corporación Favorita 超市销售预测

### 你能准确预测大型超市链的销售吗？

www.kaggle.com](https://www.kaggle.com/competitions/favorita-grocery-sales-forecasting?source=post_page-----5820ba7182fd--------------------------------)

+   [第 1 名，包含 LightGBM 的集成方法](https://www.kaggle.com/c/favorita-grocery-sales-forecasting/discussion/47582)

+   [第 2 名，Wavenet](https://www.kaggle.com/c/favorita-grocery-sales-forecasting/discussion/47568)

+   [第 3 名，包含 LightGBM 的集成方法](https://www.kaggle.com/c/favorita-grocery-sales-forecasting/discussion/47560)

+   [第 4 名，Seq2Seq](https://www.kaggle.com/c/favorita-grocery-sales-forecasting/discussion/47529)

+   [第 5 名，包含 LightGBM 的集成方法](https://www.kaggle.com/c/favorita-grocery-sales-forecasting/discussion/47556)

+   [第 6 名，Lightgbm](https://www.kaggle.com/c/favorita-grocery-sales-forecasting/discussion/47575)

+   [第 8 名，包含 LightGBM 的集成方法](https://www.kaggle.com/c/favorita-grocery-sales-forecasting/discussion/47564)

[](https://www.kaggle.com/competitions/web-traffic-time-series-forecasting?source=post_page-----5820ba7182fd--------------------------------) [## 网络流量时间序列预测

### 预测维基百科页面的未来流量

www.kaggle.com](https://www.kaggle.com/competitions/web-traffic-time-series-forecasting?source=post_page-----5820ba7182fd--------------------------------)

+   [第 1 名，Seq2Seq](https://www.kaggle.com/competitions/web-traffic-time-series-forecasting/discussion/43795)

+   [第 2 名，包含 XGBoost 的集成](https://www.kaggle.com/competitions/web-traffic-time-series-forecasting/discussion/39370)

+   [第 5 名，多项式回归](https://www.kaggle.com/competitions/web-traffic-time-series-forecasting/discussion/43603)

+   [第 6 名，CNN](https://www.kaggle.com/competitions/web-traffic-time-series-forecasting/discussion/39370)

+   [第 7 名，神经网络](https://www.kaggle.com/competitions/web-traffic-time-series-forecasting/discussion/43621)

+   [第 8 名，卡尔曼滤波](https://www.kaggle.com/competitions/web-traffic-time-series-forecasting/discussion/43727)

[](https://www.kaggle.com/competitions/rossmann-store-sales?source=post_page-----5820ba7182fd--------------------------------) [## Rossmann Store Sales

### 使用商店、促销和竞争对手数据进行销售预测

www.kaggle.com](https://www.kaggle.com/competitions/rossmann-store-sales?source=post_page-----5820ba7182fd--------------------------------)

+   [第 1 名，XGBoost](https://www.kaggle.com/competitions/rossmann-store-sales/discussion/18024)

+   [第 3 名，神经网络](https://www.kaggle.com/competitions/rossmann-store-sales/discussion/17974)

[](https://www.kaggle.com/competitions/genentech-flu-forecasting?source=post_page-----5820ba7182fd--------------------------------) [## 流感预测

### 预测流感的发生时间、地点和强度

www.kaggle.com](https://www.kaggle.com/competitions/genentech-flu-forecasting?source=post_page-----5820ba7182fd--------------------------------)

讨论页面无法加载。

[](https://www.kaggle.com/competitions/ams-2014-solar-energy-prediction-contest/overview?source=post_page-----5820ba7182fd--------------------------------) [## AMS 2013–2014 太阳能预测竞赛

### 使用天气模型的集成预测每日太阳能

www.kaggle.com](https://www.kaggle.com/competitions/ams-2014-solar-energy-prediction-contest/overview?source=post_page-----5820ba7182fd--------------------------------)

无已发布的解决方案。

*喜欢这篇文章？* [*看看我的其他文章*](https://medium.com/@davide.burba) *并关注我获取更多内容！* [*点击这里*](https://medium.com/@davide.burba/membership) *阅读无限文章，并在不增加额外费用的情况下支持我*❤️
