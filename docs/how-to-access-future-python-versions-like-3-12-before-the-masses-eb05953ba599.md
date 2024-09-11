# 如何在大众之前访问未来的 Python 版本，如 3.12

> 原文：[https://towardsdatascience.com/how-to-access-future-python-versions-like-3-12-before-the-masses-eb05953ba599?source=collection_archive---------10-----------------------#2023-06-22](https://towardsdatascience.com/how-to-access-future-python-versions-like-3-12-before-the-masses-eb05953ba599?source=collection_archive---------10-----------------------#2023-06-22)

## 并进行试用

[](https://ibexorigin.medium.com/?source=post_page-----eb05953ba599--------------------------------)[![Bex T.](../Images/516496f32596e8ad56bf07f178a643c6.png)](https://ibexorigin.medium.com/?source=post_page-----eb05953ba599--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eb05953ba599--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----eb05953ba599--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----eb05953ba599--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39db050c2ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-access-future-python-versions-like-3-12-before-the-masses-eb05953ba599&user=Bex+T.&userId=39db050c2ac2&source=post_page-39db050c2ac2----eb05953ba599---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb05953ba599--------------------------------) ·6 min read·2023年6月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Feb05953ba599&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-access-future-python-versions-like-3-12-before-the-masses-eb05953ba599&user=Bex+T.&userId=39db050c2ac2&source=-----eb05953ba599---------------------clap_footer-----------)

-- 

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Feb05953ba599&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-access-future-python-versions-like-3-12-before-the-masses-eb05953ba599&source=-----eb05953ba599---------------------bookmark_footer-----------)![](../Images/97b8818cf5623f3b04b622ac8e77d05a.png)

图片由我用 Midjourney 制作

新版本的 Python 总是带来显著改进的额外功能。以 3.11 为例 —— 它承诺提升高达 60% 的性能，并在去年十月兑现了这一承诺。

除了承诺的新功能外，测试未发布的 Python 版本帮助开发者更快地发现错误，并让其他人参与开发过程。通过在大众之前利用这些新功能，确实有可能获得**竞争优势**。

即使这些理由听起来不够吸引人，在社交媒体上炫耀你在大多数人之前查看了新版 Python 也总是很酷。

那么，我们开始吧。

## 步骤 0：安装 Docker

之所以不是每个人都能访问到新版 Python，是因为这些版本被很好地隐藏了。它们在 Python.org 上没有下载链接，而是托管在[官方 Python Docker 镜像](https://hub.docker.com/_/python)页面上：

![](../Images/5d6f6338bafa5bfda306718d4b6e0684.png)

截图由我提供

如你所见，该镜像的下载量超过 10 亿次。如果你向下滚动一点，你将会看到……
