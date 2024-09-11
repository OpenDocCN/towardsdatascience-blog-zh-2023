# 如何从任何 PDF 和图像中提取文本以供大型语言模型使用

> 原文：[`towardsdatascience.com/how-to-extract-text-from-any-pdf-and-image-for-large-language-model-2d17f02875e6?source=collection_archive---------2-----------------------#2023-07-25`](https://towardsdatascience.com/how-to-extract-text-from-any-pdf-and-image-for-large-language-model-2d17f02875e6?source=collection_archive---------2-----------------------#2023-07-25)

## 使用这些文本提取技术来获得高质量的数据，以供您的 LLM 模型使用

[](https://zoumanakeita.medium.com/?source=post_page-----2d17f02875e6--------------------------------)![Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----2d17f02875e6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2d17f02875e6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2d17f02875e6--------------------------------) [Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----2d17f02875e6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe6ae785a30d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-extract-text-from-any-pdf-and-image-for-large-language-model-2d17f02875e6&user=Zoumana+Keita&userId=e6ae785a30d&source=post_page-e6ae785a30d----2d17f02875e6---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2d17f02875e6--------------------------------) ·7 分钟阅读·2023 年 7 月 25 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2d17f02875e6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-extract-text-from-any-pdf-and-image-for-large-language-model-2d17f02875e6&user=Zoumana+Keita&userId=e6ae785a30d&source=-----2d17f02875e6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2d17f02875e6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-extract-text-from-any-pdf-and-image-for-large-language-model-2d17f02875e6&source=-----2d17f02875e6---------------------bookmark_footer-----------)![](img/b75ec7ee88027665b818561aad71953d.png)

图片来自 [Patrick Tomasso](https://unsplash.com/@impatrickt) 于 [Unsplash](https://unsplash.com/photos/Oaqk7qqNh_c)

# 动机

大型语言模型已经席卷了互联网，这使得越来越多的人忽视了使用这些模型时最重要的部分：高质量的数据！

本文旨在提供几种高效提取各种文档中文本的技术。在完成本教程后，您将清楚知道根据具体使用场景选择哪种工具。

# Python 库

本文专注于 Pytesseract、easyOCR、PyPDF2 和 LangChain 库。实验数据是一个一页的 PDF 文件，且可以在我的 [GitHub](https://github.com/keitazoumana/Experimentation-Data/blob/main/Experimentation_file.pdf) 上免费获取。

Pytesseract 和 easyOCR 都处理图像，因此需要将 PDF 文件转换为图像后才能进行内容提取。
