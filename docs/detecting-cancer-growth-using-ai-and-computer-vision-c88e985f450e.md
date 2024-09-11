# 使用 AI 和计算机视觉检测癌症生长

> 原文：[`towardsdatascience.com/detecting-cancer-growth-using-ai-and-computer-vision-c88e985f450e?source=collection_archive---------11-----------------------#2023-06-15`](https://towardsdatascience.com/detecting-cancer-growth-using-ai-and-computer-vision-c88e985f450e?source=collection_archive---------11-----------------------#2023-06-15)

## 社会公益中的 AI：医疗影像中的应用

[](https://medium.com/@saranggupta94?source=post_page-----c88e985f450e--------------------------------)![Sarang Gupta](https://medium.com/@saranggupta94?source=post_page-----c88e985f450e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c88e985f450e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c88e985f450e--------------------------------) [Sarang Gupta](https://medium.com/@saranggupta94?source=post_page-----c88e985f450e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb597879e5921&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdetecting-cancer-growth-using-ai-and-computer-vision-c88e985f450e&user=Sarang+Gupta&userId=b597879e5921&source=post_page-b597879e5921----c88e985f450e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c88e985f450e--------------------------------) ·9 分钟阅读·2023 年 6 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc88e985f450e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdetecting-cancer-growth-using-ai-and-computer-vision-c88e985f450e&user=Sarang+Gupta&userId=b597879e5921&source=-----c88e985f450e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc88e985f450e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdetecting-cancer-growth-using-ai-and-computer-vision-c88e985f450e&source=-----c88e985f450e---------------------bookmark_footer-----------)![](img/15f850a7ab92e533f3e3712165b1cb54.png)

封面图片来自 [unsplash.com](https://unsplash.com/photos/pwcKF7L4-no)

# 引言

乳腺癌是女性中最致命的癌症之一。根据[世界卫生组织 (WHO)](https://www.who.int/news-room/fact-sheets/detail/breast-cancer)的数据，仅 2020 年，全球就诊断出了约 230 万例侵袭性乳腺癌，并导致了 685,000 人死亡。

尽管发展中国家占所有乳腺癌病例的一半，但它们占乳腺癌导致的所有死亡人数的 62%。乳腺癌诊断后至少 5 年的生存率从[高收入国家的 90%到印度的 66%和南非的 40%](https://www.who.int/news-room/fact-sheets/detail/breast-cancer)不等。

![](img/e68a613d03dd048885efa5adc07cd125.png)

图 1：病理学家执行的乳腺癌转移检测的各个步骤 | 左上：来自[Camelyon17 挑战](https://camelyon17.grand-challenge.org/Background/)的图像 | 右上：来自[unsplash.com](https://unsplash.com/photos/tGYrlchfObE?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink)的图像 | 中间：来自[unsplash.com](https://unsplash.com/photos/bHNUueWud9c)的图像 | 左下和右下：作者提供的图像

确定癌症处于哪个阶段的一个关键步骤是通过显微镜检查靠近乳房的淋巴结，以了解癌症是否[已转移](https://en.wikipedia.org/wiki/Metastasis)（医学术语，意指扩散到身体其他部位）。这一过程不仅敏感、耗时且繁重，还需要高技能的医学病理学家。这影响到与治疗相关的决策，包括放射治疗、化疗以及可能的手术切除等方面的考虑…
