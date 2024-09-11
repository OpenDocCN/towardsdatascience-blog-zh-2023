# 使用量子计算机寻找暗物质

> 原文：[`towardsdatascience.com/finding-dark-matter-using-a-quantum-computer-a99f4bff4685?source=collection_archive---------9-----------------------#2023-11-03`](https://towardsdatascience.com/finding-dark-matter-using-a-quantum-computer-a99f4bff4685?source=collection_archive---------9-----------------------#2023-11-03)

## QML — 量子机器学习在高能和粒子物理的有趣用例中的应用

[](https://prashantmdgl9.medium.com/?source=post_page-----a99f4bff4685--------------------------------)![Prashant Mudgal](https://prashantmdgl9.medium.com/?source=post_page-----a99f4bff4685--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a99f4bff4685--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a99f4bff4685--------------------------------) [Prashant Mudgal](https://prashantmdgl9.medium.com/?source=post_page-----a99f4bff4685--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8fa3af9ed30d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-dark-matter-using-a-quantum-computer-a99f4bff4685&user=Prashant+Mudgal&userId=8fa3af9ed30d&source=post_page-8fa3af9ed30d----a99f4bff4685---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----a99f4bff4685--------------------------------) ·9 分钟阅读·2023 年 11 月 3 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa99f4bff4685&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-dark-matter-using-a-quantum-computer-a99f4bff4685&source=-----a99f4bff4685---------------------bookmark_footer-----------)

# 背景

今年的八月专门为 IBM 的全球量子暑期学校所用，在那里我不仅在紧凑的时间表和安排下学到了基础知识，还了解了一些量子计算的应用。经过 4 周的艰苦学习后获得的[徽章](https://www.credly.com/go/ZlukKqHe)本身就是一种“*量子体验*”，因为你以为自己理解了自己在做什么，但同时你对实际情况一无所知。这个月从量子电路基础过渡到变分算法的速度很快，这仅留下了有限的时间来‘进行自主研究’并亲自操作应用部分。

![](img/5ec7d8ce72d318eaaaf6c7221fe5e741.png)

照片由 [Dynamic Wang](https://unsplash.com/@dynamicwang?utm_source=medium&utm_medium=referral) 拍摄，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

就应用而言，量子化学、量子模拟以及一些非常复杂的建模任务都适合用量子计算机解决。话虽如此，还有一个正在蓬勃发展的领域，吸引了许多用户和研究者的兴趣，那就是量子机器学习——简称 QML。

我认为 QML 应该是传统 ML 的逻辑继承者，因此我开始了相应的工作。现在，我想要一个不那么直接的问题……
