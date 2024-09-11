# 理解目标检测中的交并比（代码）

> 原文：[`towardsdatascience.com/understanding-intersection-over-union-for-object-detection-code-9a691c72d83a?source=collection_archive---------15-----------------------#2023-10-07`](https://towardsdatascience.com/understanding-intersection-over-union-for-object-detection-code-9a691c72d83a?source=collection_archive---------15-----------------------#2023-10-07)

## 对目标检测模型的评估归结为一件事：确定检测是否有效。

[](https://medium.com/@kiprono_65591?source=post_page-----9a691c72d83a--------------------------------)![Kiprono Elijah Koech](https://medium.com/@kiprono_65591?source=post_page-----9a691c72d83a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9a691c72d83a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9a691c72d83a--------------------------------) [Kiprono Elijah Koech](https://medium.com/@kiprono_65591?source=post_page-----9a691c72d83a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9872f324f100&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-intersection-over-union-for-object-detection-code-9a691c72d83a&user=Kiprono+Elijah+Koech&userId=9872f324f100&source=post_page-9872f324f100----9a691c72d83a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9a691c72d83a--------------------------------) ·7 分钟阅读·2023 年 10 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9a691c72d83a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-intersection-over-union-for-object-detection-code-9a691c72d83a&user=Kiprono+Elijah+Koech&userId=9872f324f100&source=-----9a691c72d83a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9a691c72d83a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-intersection-over-union-for-object-detection-code-9a691c72d83a&source=-----9a691c72d83a---------------------bookmark_footer-----------)![](img/96c9fc1ed064608c5a9f4def1073800c.png)

图片由 [Vardan Papikyan](https://unsplash.com/@varpap?utm_source=medium&utm_medium=referral) 提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

确定检测是否有效需要理解**交并比（IoU）**。

本文涵盖以下内容：

+   IoU 基础——**什么是 IoU**？

+   **如何计算（理论上和 Python 代码中）单对** 检测框和真实框的 IoU

+   **计算多组**预测和地面真实边界框的 IoU。

+   如何**解释 IoU 值**？

# 什么是交并比（IoU）？

IoU 是评估目标检测模型的核心指标。它通过评估检测框与地面真实框之间的重叠程度来衡量对象检测器的准确性。

+   **地面真实框**或**标签**是显示对象位置的注释框（注释通常是手工完成的，地面真实框被认为是对象的实际位置）。

+   **检测框**或**预测边界框**是对象检测器的预测结果。
