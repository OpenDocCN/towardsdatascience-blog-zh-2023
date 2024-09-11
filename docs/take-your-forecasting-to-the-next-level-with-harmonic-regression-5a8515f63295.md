# 什么是用于时间序列预测的谐波回归？

> 原文：[`towardsdatascience.com/take-your-forecasting-to-the-next-level-with-harmonic-regression-5a8515f63295?source=collection_archive---------7-----------------------#2023-03-29`](https://towardsdatascience.com/take-your-forecasting-to-the-next-level-with-harmonic-regression-5a8515f63295?source=collection_archive---------7-----------------------#2023-03-29)

## 揭示傅里叶级数与时间序列之间的迷人关系

[](https://medium.com/@egorhowell?source=post_page-----5a8515f63295--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----5a8515f63295--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5a8515f63295--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5a8515f63295--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----5a8515f63295--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftake-your-forecasting-to-the-next-level-with-harmonic-regression-5a8515f63295&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----5a8515f63295---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5a8515f63295--------------------------------) ·6 min read·2023 年 3 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5a8515f63295&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftake-your-forecasting-to-the-next-level-with-harmonic-regression-5a8515f63295&user=Egor+Howell&userId=1cac491223b2&source=-----5a8515f63295---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5a8515f63295&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftake-your-forecasting-to-the-next-level-with-harmonic-regression-5a8515f63295&source=-----5a8515f63295---------------------bookmark_footer-----------)![](img/22e584d1ffdab7db6faabde247cdda72.png)

照片来源：[Pawel Czerwinski](https://unsplash.com/@pawel_czerwinski?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 背景和问题

当我们想要对时间序列中的[***季节性***](https://medium.com/towards-data-science/seasonality-of-time-series-5b45b4809acd)进行建模时，我们通常会转向[***SARIMA***](https://medium.com/towards-artificial-intelligence/how-to-forecast-with-sarima-d4b4c98fca7b)模型。这个模型通过在特定的滞后索引找到[***自回归项***](https://medium.com/towards-data-science/how-to-forecast-time-series-using-autoregression-1d45db71683)和[***移动平均项***](https://medium.com/towards-data-science/how-to-forecast-with-moving-average-models-6f3c9cbba60d)来为[***ARIMA***](https://medium.com/towards-data-science/how-to-forecast-with-arima-96b3d4db111a)模型添加季节性成分。例如，具有年度季节性的月度数据将适配在***12***的倍数下的自回归项和移动平均项。你可以在我之前的文章中详细阅读这个过程：

[](https://pub.towardsai.net/how-to-forecast-with-sarima-d4b4c98fca7b?source=post_page-----5a8515f63295--------------------------------) [## 如何使用 SARIMA 进行预测

### 深入了解 SARIMA 模型及其应用

pub.towardsai.net](https://pub.towardsai.net/how-to-forecast-with-sarima-d4b4c98fca7b?source=post_page-----5a8515f63295--------------------------------)

然而，如果我们有日数据，其年度季节性为***365.25 天***？或者甚至是具有***52.14***季节性的周数据呢？

不幸的是，SARIMA 不能处理这种情况，因为它是*非整数*的，并且由于需要处理每个季节的***365***个数据点以寻找模式，计算上也[*很吃力*](https://otexts.com/fpp2/dhr.html)。

*那么，我们该怎么办？*

收入[***傅里叶级数***](https://en.wikipedia.org/wiki/Fourier_series)来拯救一天！
