# 多变量时间序列的主成分分析：动态高维数据的预测

> 原文：[`towardsdatascience.com/pca-for-multivariate-time-series-forecasting-dynamic-high-dimensional-data-ab050a19e8db`](https://towardsdatascience.com/pca-for-multivariate-time-series-forecasting-dynamic-high-dimensional-data-ab050a19e8db)

## 噪声和序列相关性存在下的系统预测

[](https://medium.com/@cerlymarco?source=post_page-----ab050a19e8db--------------------------------)![Marco Cerliani](https://medium.com/@cerlymarco?source=post_page-----ab050a19e8db--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ab050a19e8db--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----ab050a19e8db--------------------------------) [Marco Cerliani](https://medium.com/@cerlymarco?source=post_page-----ab050a19e8db--------------------------------)

·发表在[数据科学前沿](https://towardsdatascience.com/?source=post_page-----ab050a19e8db--------------------------------) ·5 分钟阅读·2023 年 1 月 31 日

--

![](img/d24175ab02c0431b2c94255d1da09b50.png)

图片由[Viva Luna Studios](https://unsplash.com/@vivalunastudios?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

多步骤的多变量时间序列预测被认为是一个复杂的预测任务。我们必须考虑到输入和输出的高维度；必须充分处理横截面和时间依赖关系；最后但同样重要的是，我们必须确保长时间准确性的可接受水平。

如今，处理时间和数量维度巨大数据的分析应用非常普遍。因此，所有基于这些系统构建的解决方案必须能够操作大数据集。**在物联网（IoT）时代，处理大量时间序列是很常见的，这些时间序列在大多数情况下展示了强相关模式**。这些动态在电信、工业制造、金融、电网等领域非常常见。

假设你是一个数据科学家。我们负责开发一个预测分析应用，提供多步骤的预测，针对由相关和噪声传感器组成的物联网系统。**多变量预测在文献中广泛讨论**。从 VAR（向量自回归模型）等统计技术到更复杂的基于深度学习的方法，存在许多可用于预测任务的解决方案。然而，现实世界比预期的更复杂和残酷。**实时管理大量高频传感器需要开发兼具适当准确度和合理响应延迟的解决方案**。

在这篇文章中，我们尝试开发一个用于多变量和多步骤传感器预测的预测应用程序，该应用程序可以在接近实时模式下使用。这是通过**将降维与适合多变量上下文的预测技术相结合**来实现的。所提出的方法在经济预测文献中很受欢迎，被称为动态因子建模[1]。换句话说，**我们将最喜欢的预测算法叠加在降维技术（如 PCA）的结果之上**，以预测未来的系统动态。

# 实验设置

在本文范围内，我们生成了多个合成时间序列。这些序列可以根据其正弦动态分成两组。一切都很完美，只是信号被噪声掩盖了。

![](img/6e422fba63700901728baa73339c911f.png)

数据中的合成动态 [image by the author]

我们在时间序列中加入了大量噪声，以复制现实世界系统的混沌行为，使预测任务变得更加困难。

![](img/9c0d9ba27682035af1cbad7abfd8714d.png)

带有噪声的模拟序列预测 [image by the author]

拥有多个时间序列可供使用，我们的目标是预测它们未来的多个步骤。**为了以接近实时的模式进行多变量预测任务，我们应该在预测准确性和推理过程的持续时间之间找到一个折衷**。

让我们看看如何解决这个问题。

# 多变量动态预测

**动态因子建模**（DFM）是一种源自经济文献的多变量预测技术[1]。DFM 的基本思想是**少量系列可以解释大量变量的时间行为**。如果我们可以获得这些因素的准确估计，整个预测任务可以通过使用估计的动态因素来简化，而不是使用所有系列。

![](img/9536d14c090c2e31ea973c7ca37c2b88.png)

动态因子建模估计流程 [image by the author]

使用 DFM 获得的预测质量取决于两个主要方面：因素估计的优良性和因素预测的准确性。估计动态因素的方式有很多种。在机器学习生态系统中最常见的方法是通过正交旋转（PCA）获得一组数据的主成分[2]。

**DFM 也是一种与模型无关的技术**。换句话说，任何降维和任何预测策略都可以用于进行预测。对于我们的实验，我们使用标准 PCA 和朴素直接预测。下面是进行 DFM 估计和预测的代码快照。

```py
scaler_pca = make_pipeline(StandardScaler(), PCA(n_components=2))

X_train_factors = scaler_pca.fit_transform(X_train)

forecaster = ForecastingChain(
    Ridge(),
    n_estimators=test_size,
    lags=range(1,25),
    use_exog=False,
    n_jobs=-1,
).fit(None, X_train_factors)

y_pred_factors = forecaster.predict(np.arange(test_size))

y_pred = scaler_pca.steps[0][-1].inverse_transform(
    scaler_pca.steps[-1][-1].inverse_transform(y_pred_factors)
)
```

最后的步骤是通过简单地对我们所有可用的系列采用多变量直接预测来解决相同的任务。无论结果如何，这种方法都不可持续，因为它需要对所有可用系列进行滞后特征计算。这可能需要**处理大量的滞后变量，这使得大多数系统由于物理（内存）和时间限制而无法进行预测**。

我们采用了时间交叉验证策略来验证这两种方法的结果并存储性能数据。

![](img/36eb20be5b3330c36bfe59bf1615068d.png)

两种方法预测结果的视觉比较 [作者提供的图像]

DFM 超越了幼稚的多变量直接预测。在较短时间内（推断/估计时间依赖于系统变量的数量）实现了更高的准确性。检查生成的预测，我们可以观察到 DFM 能够正确区分并再现原始系列中存在的双正弦波动态。

![](img/14a3fea925c119f465c26663666b4120.png)

多变量预测性能比较 [作者提供的图像]

# 总结

在这篇文章中，我们提出了动态因子建模的实际应用。它被证明是一种有效的多变量时间序列预测建模方法。它特别适用于预测高维数据，同时也显示出可能的高噪声水平。正如往常一样，**不存在适用于所有情况的完美预测技术**。作为数据科学家，我们有责任尝试以前未曾了解的技术。**只有通过持续的自我学习，我们才能选择和区分适用于日常任务的最佳解决方案**。

[**查看我的 GitHub 仓库**](https://github.com/cerlymarco/MEDIUM_NoteBook)

保持联系: [Linkedin](https://www.linkedin.com/in/marco-cerliani-b0bba714b/)

## 参考文献

[1] M. Forni, M. Hallin, M. Lippi 和 L. Reichlin, “[*广义动态因子模型*](https://www.jstor.org/stable/27590616)”，《美国统计学会杂志》，第 100 卷，第 471 期，第 830–840 页。

[2] G. Bontempi, Y. -A. Le Borgne 和 J. de Stefani, “[*动态因子机器学习方法用于多变量和多步预测*](https://ieeexplore.ieee.org/document/8259781)”，2017 年 IEEE 国际数据科学与高级分析会议 (DSAA)，东京，日本，2017 年，第 222–231 页。
