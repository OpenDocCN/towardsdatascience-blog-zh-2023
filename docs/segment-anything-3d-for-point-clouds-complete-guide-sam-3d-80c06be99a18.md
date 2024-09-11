# 点云的 Segment Anything 3D：完整指南（SAM 3D）

> 原文：[`towardsdatascience.com/segment-anything-3d-for-point-clouds-complete-guide-sam-3d-80c06be99a18?source=collection_archive---------1-----------------------#2023-12-13`](https://towardsdatascience.com/segment-anything-3d-for-point-clouds-complete-guide-sam-3d-80c06be99a18?source=collection_archive---------1-----------------------#2023-12-13)

## 3D Python

## 如何构建一个利用 SAM 和 Python 进行 3D 点云语义分割的应用程序。附加内容：关于 3D 点和 2D 像素之间投影和关系的代码。

[](https://medium.com/@florentpoux?source=post_page-----80c06be99a18--------------------------------)![Florent Poux, Ph.D.](https://medium.com/@florentpoux?source=post_page-----80c06be99a18--------------------------------)[](https://towardsdatascience.com/?source=post_page-----80c06be99a18--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----80c06be99a18--------------------------------) [Florent Poux, Ph.D.](https://medium.com/@florentpoux?source=post_page-----80c06be99a18--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8ba7bf4ad784&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsegment-anything-3d-for-point-clouds-complete-guide-sam-3d-80c06be99a18&user=Florent+Poux%2C+Ph.D.&userId=8ba7bf4ad784&source=post_page-8ba7bf4ad784----80c06be99a18---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----80c06be99a18--------------------------------) ·27 分钟阅读·2023 年 12 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F80c06be99a18&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsegment-anything-3d-for-point-clouds-complete-guide-sam-3d-80c06be99a18&user=Florent+Poux%2C+Ph.D.&userId=8ba7bf4ad784&source=-----80c06be99a18---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F80c06be99a18&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsegment-anything-3d-for-point-clouds-complete-guide-sam-3d-80c06be99a18&source=-----80c06be99a18---------------------bookmark_footer-----------)![](img/cbaf694227e7e6b933f01064d27a96d6.png)

3D 环境的 Segment Anything 模型。我们将使用 3D 点云数据集在室内空间中检测物体。感谢 [Mimatelier](https://linktr.ee/mimatelier)，这位才华横溢的插画师创作了这张图像。

技术飞跃简直令人疯狂，尤其是看到人工智能（AI）应用于三维挑战时。能够利用最新的前沿研究来推动高级三维应用是非常令人振奋的。特别是在将人类级别的推理能力带入计算机时，我们清楚地需要从我们观察到的三维实体中提取形式化的意义。

> 在本教程中，我们的目标是确保将令人惊叹的 AI 进展与使用三维点云的三维应用结合起来。 — *🐲* **Florent & Ville**

这不是一件容易的事，但一旦掌握，三维点云与深度学习的融合将开辟理解和解读我们视觉世界的新维度。

在这些进展中，Segment Anything Model 是最近的创新亮点，尤其是用于全自动化的场景，无需监督。
