# 径向树图：将树图扩展为圆形映射

> 原文：[https://towardsdatascience.com/radial-treemaps-extending-treemaps-to-circular-mappings-7b47785191da?source=collection_archive---------1-----------------------#2023-12-10](https://towardsdatascience.com/radial-treemaps-extending-treemaps-to-circular-mappings-7b47785191da?source=collection_archive---------1-----------------------#2023-12-10)

## 学习径向树图，并使用Python创建您自己的树图。

[](https://medium.com/@nickgerend?source=post_page-----7b47785191da--------------------------------)[![尼克·格伦德](../Images/716eb183008674ac46c6aee96093c4b3.png)](https://medium.com/@nickgerend?source=post_page-----7b47785191da--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7b47785191da--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7b47785191da--------------------------------) [尼克·格伦德](https://medium.com/@nickgerend?source=post_page-----7b47785191da--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffa23f7cc3eed&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fradial-treemaps-extending-treemaps-to-circular-mappings-7b47785191da&user=Nick+Gerend&userId=fa23f7cc3eed&source=post_page-fa23f7cc3eed----7b47785191da---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7b47785191da--------------------------------) ·16分钟阅读·2023年12月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7b47785191da&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fradial-treemaps-extending-treemaps-to-circular-mappings-7b47785191da&user=Nick+Gerend&userId=fa23f7cc3eed&source=-----7b47785191da---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7b47785191da&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fradial-treemaps-extending-treemaps-to-circular-mappings-7b47785191da&source=-----7b47785191da---------------------bookmark_footer-----------)![](../Images/6f01781b35b77fcfaf65ec239959c2a5.png)

尼克·格伦德的径向树图

# **背景**

## **树图概念**

“树图”是由本·施奈德曼（Ben Shneiderman）在1990年代初期于马里兰大学引入的¹。简单来说，它是一种将层次数据显示为一组嵌套矩形的高效方式。尽管概念简单，但矩形的排列取决于美学偏好，并且已开发了各种排列算法来增强最终布局的外观。

## **树图机制**

给定一个层级结构，Treemap 将层级中的每个分支表示为一个矩形，然后用表示子分支的较小矩形进行拼接。Treemap 中的空间是根据数据的特定属性（通常是大小或价值）进行划分的，每个矩形的面积对应于该属性的大小，使得比较层级中的不同部分变得容易。

![](../Images/b0de705dee566f5e999e0a30c843ec77.png)

组 a、b 和 c 的 Treemap，按此顺序 -> 每个层级的最大项：（a1）、（a1,b1）、（a1,b1,c1）

为了说明矩形的排列，这里列出了一些常见的算法来指导…
