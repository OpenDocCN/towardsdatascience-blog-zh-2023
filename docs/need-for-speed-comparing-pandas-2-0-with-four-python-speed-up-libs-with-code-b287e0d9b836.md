# 需求速度：比较 Pandas 2.0 与四种 Python 加速库（附带代码）

> 原文：[`towardsdatascience.com/need-for-speed-comparing-pandas-2-0-with-four-python-speed-up-libs-with-code-b287e0d9b836?source=collection_archive---------0-----------------------#2023-05-19`](https://towardsdatascience.com/need-for-speed-comparing-pandas-2-0-with-four-python-speed-up-libs-with-code-b287e0d9b836?source=collection_archive---------0-----------------------#2023-05-19)

## *Polars*、*Dask*、*RAPIDS.ai cuDF*和*Numba*与后端的*Pandas 2.0*和*pyarrow*进行比较，涵盖向量化和*itertuples()*、*apply()*方法。

[](https://theomitsa.medium.com/?source=post_page-----b287e0d9b836--------------------------------)![Dr. Theophano Mitsa](https://theomitsa.medium.com/?source=post_page-----b287e0d9b836--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b287e0d9b836--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b287e0d9b836--------------------------------) [Dr. Theophano Mitsa](https://theomitsa.medium.com/?source=post_page-----b287e0d9b836--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7709c007f0ca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fneed-for-speed-comparing-pandas-2-0-with-four-python-speed-up-libs-with-code-b287e0d9b836&user=Dr.+Theophano+Mitsa&userId=7709c007f0ca&source=post_page-7709c007f0ca----b287e0d9b836---------------------post_header-----------) 发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----b287e0d9b836--------------------------------) ·9 分钟阅读·2023 年 5 月 19 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb287e0d9b836&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fneed-for-speed-comparing-pandas-2-0-with-four-python-speed-up-libs-with-code-b287e0d9b836&user=Dr.+Theophano+Mitsa&userId=7709c007f0ca&source=-----b287e0d9b836---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb287e0d9b836&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fneed-for-speed-comparing-pandas-2-0-with-four-python-speed-up-libs-with-code-b287e0d9b836&source=-----b287e0d9b836---------------------bookmark_footer-----------)![](img/3e9e1405f4a9150be078fd1dfff4beda.png)

图片由 Pixabay 上的 alan9187 提供

到目前为止，2023 年在机器学习领域带来了显著的进展，特别是在先进的*大型语言模型*（*LLMs*）的开发和全球分发方面。然而，机器学习的专业知识不仅限于微调*LLMs*和提示工程。一个机器学习专家了解系统的内部工作原理，能够解释/优化各种系统行为，并最终负责机器学习解决方案的整体质量、业务需求适配性和性能。

> 除了性能比较外，本文还旨在通过提供代码示例来教育读者如何实现各种 Python 加速操作，以阐明关键点。

说到了解系统的内部工作原理和性能，本文将比较新发布的*Pandas 2.0*与其他四个加速 Python 库的性能，并…
