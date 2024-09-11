# 如何解决 Windows 上 Anaconda 的 Python 依赖问题

> 原文：[`towardsdatascience.com/how-to-solve-python-dependency-issues-with-anaconda-on-windows-d5033c9d2f9a?source=collection_archive---------16-----------------------#2023-07-13`](https://towardsdatascience.com/how-to-solve-python-dependency-issues-with-anaconda-on-windows-d5033c9d2f9a?source=collection_archive---------16-----------------------#2023-07-13)

## 不改变绝对路径

[](https://federicotrotta.medium.com/?source=post_page-----d5033c9d2f9a--------------------------------)![Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----d5033c9d2f9a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d5033c9d2f9a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d5033c9d2f9a--------------------------------) [Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----d5033c9d2f9a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F654cd4bbe899&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-solve-python-dependency-issues-with-anaconda-on-windows-d5033c9d2f9a&user=Federico+Trotta&userId=654cd4bbe899&source=post_page-654cd4bbe899----d5033c9d2f9a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d5033c9d2f9a--------------------------------) · 5 分钟阅读 · 2023 年 7 月 13 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd5033c9d2f9a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-solve-python-dependency-issues-with-anaconda-on-windows-d5033c9d2f9a&user=Federico+Trotta&userId=654cd4bbe899&source=-----d5033c9d2f9a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd5033c9d2f9a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-solve-python-dependency-issues-with-anaconda-on-windows-d5033c9d2f9a&source=-----d5033c9d2f9a---------------------bookmark_footer-----------)![](img/989f9fa2f85067dbcbe42832ec66a93a.png)

图片来源于 [kirill_makes_pics](https://pixabay.com/it/users/kirill_makes_pics-5203613/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2261021) 在 [Pixabay](https://pixabay.com/it//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2261021)

在过去几天，我在 Windows 机器上遇到了 Python 依赖的问题。

我尝试安装新的包进行测试。实际上，它们已经安装了，我可以用`$ pip show [library_name]`查看所有细节，但当我尝试导入一分钟前安装的库时却出现了`import`问题。

我在使用 Jupyter Notebooks 时遇到了这些问题，因此我认为这可能是与 Anaconda 相关的问题。这是我尝试解决问题的步骤：

+   我尝试更改所有 Python（和 Anaconda）包安装的绝对路径，但这导致了 Anaconda 崩溃。因此，我不得不恢复到之前的绝对路径。

+   我尝试了“暴力破解”：我打开了 VS CODE，并在虚拟环境中安装了我想使用的库。你知道吗？我遇到了不同的依赖问题……

所以，在经过 2 到 3 小时的折腾之后，我决定使用最高的暴力破解手段，并且有三个选择：

+   卸载并重新安装 Python 和 Anaconda，但这不会解决实际问题，因为一些文件可能仍保留在当前文件夹中，这样路径可能保持不变，并导致…
