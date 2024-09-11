# 现代数据科学家的数据版本控制：7 个你不能忽视的 DVC 概念

> 原文：[https://towardsdatascience.com/data-version-control-for-the-modern-data-scientist-7-dvc-concepts-you-cant-ignore-bb2433ccec88?source=collection_archive---------2-----------------------#2023-05-25](https://towardsdatascience.com/data-version-control-for-the-modern-data-scientist-7-dvc-concepts-you-cant-ignore-bb2433ccec88?source=collection_archive---------2-----------------------#2023-05-25)

## 一份深入插图的指南，讲解数据科学中的关键实践

[](https://ibexorigin.medium.com/?source=post_page-----bb2433ccec88--------------------------------)[![Bex T.](../Images/516496f32596e8ad56bf07f178a643c6.png)](https://ibexorigin.medium.com/?source=post_page-----bb2433ccec88--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bb2433ccec88--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bb2433ccec88--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----bb2433ccec88--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39db050c2ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-version-control-for-the-modern-data-scientist-7-dvc-concepts-you-cant-ignore-bb2433ccec88&user=Bex+T.&userId=39db050c2ac2&source=post_page-39db050c2ac2----bb2433ccec88---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb2433ccec88--------------------------------) · 11 分钟阅读 · 2023 年 5 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbb2433ccec88&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-version-control-for-the-modern-data-scientist-7-dvc-concepts-you-cant-ignore-bb2433ccec88&user=Bex+T.&userId=39db050c2ac2&source=-----bb2433ccec88---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbb2433ccec88&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-version-control-for-the-modern-data-scientist-7-dvc-concepts-you-cant-ignore-bb2433ccec88&source=-----bb2433ccec88---------------------bookmark_footer-----------)![](../Images/d9bfb2bb06fd41ecd7191c98085578d9.png)

图片由我用 Midjourney 制作。

2020 年 5 月 31 日

多么美好的一天！我正坐在车里听着**Data Beats FM**，突然被一个广告吸引了注意。

> 数据科学家们已经羡慕软件工程师很久了。虽然纯粹的软件工程师——我们可以戏称他们为邪恶的编程巫师——可以依靠 Git 轻松地处理代码提交和分支，我们却常常不得不与庞大的数据集和 ML 模型作斗争，让 Git 气喘吁吁。但不要担心，DVC 以闪亮的盔甲来了……

我把音量调高了，脸上带着好奇的表情。

> DVC 以其卓越的能力，解放了成千上万的数据科学家，摆脱了 Git 在处理那些庞大文件时的挣扎，这些文件似乎无论怎样尝试高效版本控制都显得徒劳（Git-LFS 就是完全糟糕）。

我在这一刻露出了微笑。

> 现在是告别疯狂删除和重新上传数据以避免 Git 大小限制的日子的时候了。借助 DVC，我们终于可以恢复理智，自信地在数据驱动的工作中前进。
