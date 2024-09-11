# 使用 Python 创建惊艳的分形艺术：适合初学者和数学爱好者的教程

> 原文：[`towardsdatascience.com/create-stunning-fractal-art-with-python-a-tutorial-for-beginners-c83817fcb64b?source=collection_archive---------9-----------------------#2023-03-06`](https://towardsdatascience.com/create-stunning-fractal-art-with-python-a-tutorial-for-beginners-c83817fcb64b?source=collection_archive---------9-----------------------#2023-03-06)

## 只需一行代码或更少

[](https://ibexorigin.medium.com/?source=post_page-----c83817fcb64b--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----c83817fcb64b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c83817fcb64b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c83817fcb64b--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----c83817fcb64b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39db050c2ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-stunning-fractal-art-with-python-a-tutorial-for-beginners-c83817fcb64b&user=Bex+T.&userId=39db050c2ac2&source=post_page-39db050c2ac2----c83817fcb64b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c83817fcb64b--------------------------------) ·12 分钟阅读·2023 年 3 月 6 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc83817fcb64b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-stunning-fractal-art-with-python-a-tutorial-for-beginners-c83817fcb64b&source=-----c83817fcb64b---------------------bookmark_footer-----------)

## 介绍

“我从未见过如此美丽的事物”这个词语应只用于描述分形艺术。确实，有《蒙娜丽莎》，《星夜》，还有《维纳斯的诞生》（顺便提一下，这些作品都被 AI 生成的艺术所毁），但我认为没有任何艺术家或人类能创作出像分形那样**极其惊艳**的作品。

左边是标志性的分形艺术——曼德布罗特集合，它是在 1979 年发现的，当时没有 Python 或图形软件可用。

![](img/0804a1d9c64319ba5498269c00bda593.png)

作者使用[Fraqtive](https://github.com/mimecorg/fraqtive)制作的 GIF，这是一款开源应用程序。[GPL-3 许可证](https://github.com/mimecorg/fraqtive)。

曼德布罗集是一组复数，当在复平面上绘制时，形成我们看到的自我重复形状。集合中的每一个数字也可以是**朱利亚集**的种子，你可以看到在曼德布罗集边界内移动鼠标时出现的美丽图案。

但在我们能够绘制曼德布罗集或朱利亚集之前（相信我，我们会的），我们还有很多内容需要覆盖。如果你只是来看看酷炫的图片，我强烈推荐下载[开源 Fraqtive 软件](https://fraqtive.mimec.org/)（尽情享受！），我用它生成了上面的 GIF 和下面的那个：
