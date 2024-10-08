# 如何从任何 PDF 和图像中提取文本以用于大型语言模型

> 原文：[`towardsdatascience.com/how-to-extract-text-from-any-pdf-and-image-for-large-language-model-2d17f02875e6`](https://towardsdatascience.com/how-to-extract-text-from-any-pdf-and-image-for-large-language-model-2d17f02875e6)

## 使用这些文本提取技术为您的 LLM 模型获取高质量数据

[](https://zoumanakeita.medium.com/?source=post_page-----2d17f02875e6--------------------------------)![Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----2d17f02875e6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2d17f02875e6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2d17f02875e6--------------------------------) [Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----2d17f02875e6--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----2d17f02875e6--------------------------------) ·阅读时间 7 分钟·2023 年 7 月 25 日

--

![](img/b75ec7ee88027665b818561aad71953d.png)

图片来自[Patrick Tomasso](https://unsplash.com/@impatrickt)的[Unsplash](https://unsplash.com/photos/Oaqk7qqNh_c)

# 动机

大型语言模型已在互联网引起轰动，导致更多的人忽视使用这些模型最重要的部分：高质量的数据！

本文旨在提供一些从任何类型的文档中高效提取文本的技术。完成本教程后，您将对根据使用情况选择工具有一个清晰的了解。

YouTube 上的完整视频讲解

# Python 库

本文重点介绍了 Pytesseract、easyOCR、PyPDF2 和 LangChain 库。实验数据是一个单页 PDF 文件，可以在我的[GitHub](https://github.com/keitazoumana/Experimentation-Data/blob/main/Experimentation_file.pdf)上自由获取。

Pytesseract 和 easyOCR 都处理图像，因此需要在执行内容提取之前将 PDF 文件转换为图像。

转换可以使用`pypdfium2`完成，这是一种强大的 PDF 文件处理库，下面给出了实现方法：

```py
pip install pypdfium2
```

该函数以 PDF 作为输入，并返回 PDF 每一页的图像列表。

```py
def convert_pdf_to_images(file_path, scale=300/72):

    pdf_file = pdfium.PdfDocument(file_path)

    page_indices = [i for i in range(len(pdf_file))]

    renderer = pdf_file.render(
        pdfium.PdfBitmap.to_pil,
        page_indices = page_indices, 
        scale = scale,
    )

    final_images = [] 

    for i, image in zip(page_indices, renderer):

        image_byte_array = BytesIO()
        image.save(image_byte_array, format='jpeg', optimize=True)
        image_byte_array = image_byte_array.getvalue()
        final_images.append(dict({i:image_byte_array}))

    return final_images
```

现在，我们可以使用`display_images`函数来可视化 PDF 文件的所有页面。

```py
def display_images(list_dict_final_images):

    all_images = [list(data.values())[0] for data in list_dict_final_images]

    for index, image_bytes in enumerate(all_images):

        image = Image.open(BytesIO(image_bytes))
        figure = plt.figure(figsize = (image.width / 100, image.height / 100))

        plt.title(f"----- Page Number {index+1} -----")
        plt.imshow(image)
        plt.axis("off")
        plt.show()
```

通过结合上述两个函数，我们可以得到以下结果：

```py
convert_pdf_to_images = convert_pdf_to_images('Experimentation_file.pdf')
display_images(convert_pdf_to_images)
```

![](img/a403ee31eaf0a83985388989519bd1fb.png)

以图像格式可视化 PDF（图片来自作者）

现在是深入探索文本提取过程的时候了！

## Pytesseract

Pytesseract（Python-tesseract）是一个用于从图像中提取文本信息的 Python OCR 工具，安装可以使用`pip`命令：

```py
pip install pytesseract
```

以下辅助函数使用`Pytesseract`的`image_to_string()`函数从输入图像中提取文本。

```py
from pytesseract import image_to_string  

def extract_text_with_pytesseract(list_dict_final_images):

    image_list = [list(data.values())[0] for data in list_dict_final_images]
    image_content = []

    for index, image_bytes in enumerate(image_list):

        image = Image.open(BytesIO(image_bytes))
        raw_text = str(image_to_string(image))
        image_content.append(raw_text)

    return "\n".join(image_content) 
```

文字可以使用`extract_text_with_pytesseract`函数提取，如下所示：

```py
text_with_pytesseract = extract_text_with_pytesseract(convert_pdf_to_images)
print(text_with_pytesseract)
```

上述代码的成功执行会生成以下结果：

```py
This document provides a quick summary of some of Zoumana’s article on Medium.
It can be considered as the compilation of his 80+ articles about Data Science, Machine Learning and

Machine Learning Operations.

Whether you are just getting started or you're an experienced professional looking to upskill, these

materials can be helpful.

Data Science section covers basic to advanced
concepts such as statistics, model evaluation
metrics, SQL queries, NoSQL courses, data
visualization using Tableau and #powerbi, and
many more.

Link: httos://Inkd.in/g8zcS_vE

MLOps chapter explains how to build and
deploy models using different strategies such as
Docker containers, and GitHub actions on AWS
EC2 instances, Azure. Also, it covers how to build
REST APIs to serve your models.

Link: httos://Inkd.in/gyiUsdgz

Natural Language Processing Covers simple NLP
concepts to more advanced ones such as
Transformers and their applications in Finance,
Science, etc.

Link: httos://Inkd.in/gBdZbHty

Computer Vision section covers SOTA models
(e.g. YOLO) and different technics to mitigate

overfitting when training computer vision
models.

Link: httos://Inkd.in/gDY8ZqVs

Python section showcases multiple libraries to
facilitate one's daily life, especially when dealing
with PDF, and Word files when scraping data
from the web, and even benchmarking analysis
to help choose the right data processing tool.
Link: https://Inkd.in/gH HUMM9

Pandas & Python Tricks Covers my daily tips and
tricks on LinkedIn. And, there are plenty of those,
especially on my

website https://Inkd.in/gPbichB5
https://Inkd.in/QgUs8inuZ

Machine Learning part is about Fexplainable Al,
clustering, classification tasks, etc.

Link: httos://Inkd.in/gJdSvQns
```

`Pytesseract`能够提取图像的内容。

> 下面是它如何完成这个任务的！

`Pytesseract`通过识别输入图像中的矩形形状，从右上角到右下角开始。然后提取各个图像的内容，最终结果是这些提取内容的串联。这种方法在处理基于列的 PDF 和图像文档时效果很好。

## easyOCR

这也是一个用于光学字符识别的开源 Python 库，目前支持提取 80 多种语言的文本。`easyocr`需要同时安装`Pytorch`和`OpenCV`，可以使用以下说明进行安装。

```py
!pip install opencv-python-headless==4.1.2.30
```

根据你的操作系统，Pytorch 模块的安装可能会有所不同。但所有说明都可以在[官方页面](https://pytorch.org/)找到。

现在进行`easyocr`库的安装。

```py
!pip install easyocr
```

在使用`easyocr`时，指定我们正在处理的文档语言是很重要的，因为它支持多种语言。通过其`Reader`模块指定语言列表来设置语言。

例如，`fr`表示法语，`en`表示英语。完整的语言列表可在[这里](https://www.jaided.ai/easyocr/)查看。

考虑到这一点，让我们开始这个过程吧！

```py
from easyocr import Reader

# Load model for the English language
language_reader = Reader(["en"])
```

文本提取过程在`extract_text_with_easyocr`函数中实现：

```py
def extract_text_with_easyocr(list_dict_final_images):

    image_list = [list(data.values())[0] for data in list_dict_final_images]
    image_content = []

    for index, image_bytes in enumerate(image_list):

        image = Image.open(BytesIO(image_bytes))
        raw_text = language_reader.readtext(image)
        raw_text = " ".join([res[1] for res in raw_text])

        image_content.append(raw_text)

    return "\n".join(image_content)
```

我们可以按如下方式执行上述函数：

```py
text_with_easy_ocr = extract_text_with_easyocr(convert_pdf_to_images)
print(text_with_easy_ocr)
```

![](img/79c085ae0f30e61fdd3369d3316ffee8.png)

EasyOCR 结果（作者提供的图像）

相比于`Pytesseract`，`easyocr`的结果似乎效率较低。例如，它能够高效地读取前两段文字。然而，它没有将每一块文本视为独立的文本，而是使用基于行的方法进行读取。例如，来自第一块的字符串***Data Science section covers basic to advanced***与来自第二块的***overfitting when training computer vision***结合在一起，这种组合完全打乱了文本结构，并偏离了最终结果。

## PyPDF

`PyPDF2`也是一个专门用于 PDF 处理任务的 Python 库，例如文本和元数据检索、合并、裁剪等。

```py
!pip install PyPDF2
```

提取逻辑在`extract_text_with_pyPDF`函数中实现：

```py
def extract_text_with_pyPDF(PDF_File):

    pdf_reader = PdfReader(PDF_File)

    raw_text = ''

    for i, page in enumerate(pdf_reader.pages):

        text = page.extract_text()
        if text:
            raw_text += text

    return raw_text
```

```py
text_with_pyPDF = extract_text_with_pyPDF("Experimentation_file.pdf")
print(text_with_pyPDF)
```

![](img/11cf6ca1c6763b3f336c2f365c177b1f.png)

使用 PyPDF 库的文本提取（作者提供的图像）

提取过程快速且准确，并且甚至保留了原始字体大小。PyPDF 的主要问题是无法高效地从图像中提取文本。

## LangChain

来自 [langchain](https://python.langchain.com/docs/get_started/introduction.html) 的 `UnstructuredImageLoader` 和 `UnstructuredFileLoader` 模块可用于分别从图像和文本/pdf 文件中提取文本，这两个选项将在本节中进行探讨。

但是，首先，我们需要按照以下步骤安装 `langchain` 库：

```py
!pip install langchain
```

> 从图像中提取文本

```py
from langchain.document_loaders.image import UnstructuredImageLoader.
```

文本提取函数如下所示：

```py
def extract_text_with_langchain_image(list_dict_final_images):

    image_list = [list(data.values())[0] for data in list_dict_final_images]
    image_content = []

    for index, image_bytes in enumerate(image_list):

        image = Image.open(BytesIO(image_bytes))
        loader = UnstructuredImageLoader(image)
        data = loader.load()
        raw_text = data[index].page_content

        image_content.append(raw_text)

    return "\n".join(image_content)
```

现在，我们可以提取内容了：

```py
text_with_langchain_image = extract_text_with_langchain_image(convert_pdf_to_images)
print(text_with_langchain_image)
```

![](img/1371ef3704faac52caa42c8463480576.png)

从 langchain UnstructuredImageLoader 提取文本（图像来源：作者）

该库成功地从图像中提取了内容。

> 从 PDF 中提取文本

以下是从 PDF 中提取内容的实现。

```py
from langchain.document_loaders import UnstructuredFileLoader

def extract_text_with_langchain_pdf(pdf_file):

    loader = UnstructuredFileLoader(pdf_file)
    documents = loader.load()
    pdf_pages_content = '\n'.join(doc.page_content for doc in documents)

    return pdf_pages_content
```

```py
text_with_langchain_files = extract_text_with_langchain_pdf("Experimentation_file.pdf")
print(text_with_langchain_files)
```

与 `PyPDF` 模块类似，`langchain` 模块能够在保持原始字体大小的同时生成准确的结果。

![](img/b3b54f052fe3aa01c4fe997a8ce5e481.png)

从 langchain UnstructuredFileLoader 提取文本（图像来源：作者）

# 结论

这个简短的教程概述了一些知名库。每个库都有其自身的优点和缺点，应该根据具体用例明智地应用。完整的代码可以在我的 GitHub 上找到 [这里](https://github.com/keitazoumana/Medium-Articles-Notebooks/blob/main/OCR_Content_Extraction.ipynb)。

希望这个简短的教程能帮助你获得新的技能。

此外，如果你喜欢阅读我的故事并希望支持我的写作，可以考虑 [成为 Medium 会员](https://zoumanakeita.medium.com/membership)。只需每月 $5 的承诺，你即可解锁 Medium 上的无限制访问权限。

想请我喝咖啡 ☕️ 吗？→ [这里是链接](http://www.buymeacoffee.com/zoumanakeig)!

随时可以在 [Medium](https://zoumanakeita.medium.com/)、[Twitter](https://twitter.com/zoumana_keita_) 和 [YouTube](https://www.youtube.com/channel/UC9xKdy8cz6ZuJU5FTNtM_pQ) 上关注我，或者在 [LinkedIn](https://www.linkedin.com/in/zoumana-keita/) 上打招呼。讨论人工智能、机器学习、数据科学、自然语言处理和 MLOps 相关话题总是很愉快的！
