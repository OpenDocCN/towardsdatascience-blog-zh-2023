# 使用深度学习进行音频分类的 Python 实现

> 原文：[https://towardsdatascience.com/audio-classification-with-deep-learning-in-python-cf752b22ba07?source=collection_archive---------5-----------------------#2023-04-04](https://towardsdatascience.com/audio-classification-with-deep-learning-in-python-cf752b22ba07?source=collection_archive---------5-----------------------#2023-04-04)

## [Kaggle 蓝图](/the-kaggle-blueprints-unlocking-winning-approaches-to-data-science-competitions-24d7416ef5fd)

## 使用 PyTorch 和 torchaudio 调整图像模型，以应对领域转移和类别不平衡问题

[](https://medium.com/@iamleonie?source=post_page-----cf752b22ba07--------------------------------)[![Leonie Monigatti](../Images/4044b1685ada53a30160b03dc78f9626.png)](https://medium.com/@iamleonie?source=post_page-----cf752b22ba07--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cf752b22ba07--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cf752b22ba07--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----cf752b22ba07--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faudio-classification-with-deep-learning-in-python-cf752b22ba07&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----cf752b22ba07---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cf752b22ba07--------------------------------) ·10分钟阅读·2023年4月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcf752b22ba07&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faudio-classification-with-deep-learning-in-python-cf752b22ba07&user=Leonie+Monigatti&userId=3a38da70d8dc&source=-----cf752b22ba07---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcf752b22ba07&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faudio-classification-with-deep-learning-in-python-cf752b22ba07&source=-----cf752b22ba07---------------------bookmark_footer-----------)![](../Images/5658cabbf5faa49cd94151fa8d262288.png)

使用机器学习对声音景观中的鸟鸣进行分类（图像由作者绘制）

欢迎来到另一期的“[Kaggle 蓝图](/the-kaggle-blueprints-unlocking-winning-approaches-to-data-science-competitions-24d7416ef5fd)”，在这里我们将分析[Kaggle](https://www.kaggle.com/)竞赛的获胜解决方案，以便从中获得可以应用于我们自己数据科学项目的经验。

本届比赛将回顾[“BirdCLEF 2022”](https://www.kaggle.com/competitions/birdclef-2022)比赛的技术和方法，该比赛于2022年5月结束。

# 问题陈述：具有领域转移的音频分类

[“BirdCLEF 2022”](https://www.kaggle.com/competitions/birdclef-2022)比赛的目标是通过声音识别夏威夷鸟类。参赛者被提供了单只鸟叫的短音频文件，并被要求预测在更长的录音中是否存在特定的鸟类。

[## BirdCLEF 2022](https://www.kaggle.com/competitions/birdclef-2022/?source=post_page-----cf752b22ba07--------------------------------)

### 在声音景观中识别鸟叫

[www.kaggle.com](https://www.kaggle.com/competitions/birdclef-2022/?source=post_page-----cf752b22ba07--------------------------------)

与普通音频分类问题相比，此次比赛增加了以下挑战：
