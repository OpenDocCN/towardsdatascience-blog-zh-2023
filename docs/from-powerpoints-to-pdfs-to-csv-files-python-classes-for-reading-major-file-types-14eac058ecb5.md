# 从 Powerpoints 到 PDFs 再到 CSV 文件：用于读取主要文件类型的 Python 类

> 原文：[`towardsdatascience.com/from-powerpoints-to-pdfs-to-csv-files-python-classes-for-reading-major-file-types-14eac058ecb5`](https://towardsdatascience.com/from-powerpoints-to-pdfs-to-csv-files-python-classes-for-reading-major-file-types-14eac058ecb5)

## 在下一个数据科学项目中能够从多种不同的文件类型中提取和比较信息！

[](https://ben-mccloskey20.medium.com/?source=post_page-----14eac058ecb5--------------------------------)![本杰明·麦克洛斯基](https://ben-mccloskey20.medium.com/?source=post_page-----14eac058ecb5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----14eac058ecb5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----14eac058ecb5--------------------------------) [本杰明·麦克洛斯基](https://ben-mccloskey20.medium.com/?source=post_page-----14eac058ecb5--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----14eac058ecb5--------------------------------) ·阅读时间 6 分钟·2023 年 1 月 6 日

--

![](img/ae884e2e91a90681e265bf02b77cd8e0.png)

图片来源于 [Glen Carrie](https://unsplash.com/@glencarrie?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

数据科学家可能需要读取多种不同类型的文件以完成他们的项目。虽然在 Python 中读取 *文本* 文件特别容易，但其他文件类型可能需要来自不同 Python API 的额外支持，以确保它们在 Python 中可读和可用。今天的代码提供了多种 Python 类，可用于读取许多不同的文件类型。每个类的输出是一个文本字符串，数据科学家可以利用这些字符串进行信息提取以及各种文档的相似性分析。

# PDF 文件

我在过去多次讨论了 PDF 文件的重要性及其在 Python 中的处理方法。

[](/pdf-parsing-dashboard-with-plotly-dash-256bf944f536?source=post_page-----14eac058ecb5--------------------------------) ## PDF 解析仪表板与 Plotly Dash

### 介绍如何在下一个仪表板中读取和展示 PDF 文件。

towardsdatascience.com [](/natural-language-processing-pdf-processing-function-for-obtaining-a-general-overview-6fa63e81fbf1?source=post_page-----14eac058ecb5--------------------------------) ## 自然语言处理：用于获取概述的 PDF 处理功能

### 今天用于自然语言处理 (NLP) 的许多文档都是 .pdf 格式的。读取 pdf 文件到…

[towardsdatascience.com

我想包括下面的类，因为我无法强调在下一个项目中使用 PDF 文件的重要性。今天，许多人使用 PDF 执行各种任务，里面的信息太多，不应轻易丢弃。*pdfReader* 类有两个功能：一个是使 PDF 可以被 Python 读取，另一个是将 PDF 转换为 Python 中的一个文本字符串。**注意：最近 PyPDF2 有了新的更新，所以在使用未更新的旧教程时要小心。PyPDF2 3.0.0 中许多方法名称已被更改。**

```py
!pip install PyPDF2
import PyPDF2
import re
class pdfReader:

    def __init__(self, file_path: str) -> None:
        self.file_path = file_path

    def pdf_reader(self) -> None:
            """A function that can read .pdf formatted files 
                and returns a python readable pdf.

                Returns:
                read_pdf: A python readable .pdf file.
            """
            opener = open(self.file_path,'rb')
            read_pdf = PyPDF2.PdfReader(opener)
            return read_pdf

    def PDF_one_pager(self) -> str:
        """A function that returns a one line string of the 
            pdf.

            Returns:
            one_page_pdf (str): A one line string of the pdf.

        """
        p = pdfReader(self.file_path)
        pdf = p.pdf_reader()
        content= ''
        num_pages = len(pdf.pages)

        for i in range(0, num_pages):
            content += pdf.pages[i].extract_text() + "\n"
        content = " ".join(content.replace(u"\xa0", " ").strip().split())
        page_number_removal = r"\d{1,3} of \d{1,3}"
        page_number_removal_pattern = re.compile(page_number_removal, re.IGNORECASE)
        content = re.sub(page_number_removal_pattern, '',content)
        return content
```

# Microsoft Powerpoint 文件

有趣的事实：有一种方法可以将 Microsoft Powerpoint 文件读入 Python 并查看它们包含的信息。下面的 *pptReader* 类将从你 Powerpoint 演示文稿中的每一张幻灯片读取文本，并将其添加到一个文本字符串中。不要担心 Powerpoint 中的图片，**ppt_text()** 函数会处理这些内容！

```py
class pptReader:

    def __init__(self, file_path: str) -> None:
        self.file_path = file_path

    def ppt_text(self) -> str:
    """A function that returns a string of text from all 
       of the slides in a pptReader object.

      Returns:
      text (str): A single string containing the text 
      within each slide of the pptReader object.
   """
      prs = Presentation(self.file_path)
      text = str()

      for slide in prs.slides:
        for shape in slide.shapes:
          if not shape.has_text_frame:
              continue
          for paragraph in shape.text_frame.paragraphs:
            for run in paragraph.runs:
              text += ' ' + run.text
      return text
```

# Microsoft Excel 文件

出乎意料的是，Excel 文件比我最初预期的更难以读入 Python。我必须使用 *openpyxl* 库首先将 Excel 文件读入 Python。我没有得到想要的输出，于是将读入的文件保存为标准的 *csv* 文件，然后继续处理。这个技巧有效！

```py
 class xlsxReader:

    def __init__(self, file_path: str) -> None:
        self.file_path = file_path

    def xlsx_text(self) -> str:
      """A function that will return a string of text from 
         the information contained within an xlsxReader object.

         Returns:
         text (str): A string of text containing the information
         within the xlsxReader object.
     """
      inputExcelFile = self.file_path
      text = str()
      wb = openpyxl.load_workbook(inputExcelFile)
      for ws in wb.worksheets:
        for val in ws.values:
          print(val)

      for sn in wb.sheetnames:
        print(sn)
        excelFile = pd.read_excel(inputExcelFile, engine = 'openpyxl', sheet_name = sn)
        excelFile.to_csv("ResultCsvFile.csv", index = None, header=True)

        with open("ResultCsvFile.csv", "r") as csvFile: 
          lines = csvFile.read().split(",") # "\r\n" if needed
          for val in lines:
            if val != '':
              text += val + ' '
          text = text.replace('\ufeff', '')
          text = text.replace('\n', ' ')
      return text
```

# CSV 文件

你可能会想知道为什么我没有使用 Pandas 库来读取我的 CSV 文件。我使用传统的 **open()** 方法，因为我想要获取给定 *csv* 文件中的所有信息，而且许多 *csv* 文件的格式不同。此外，我倾向于使用标准 Python 库，而不是导入外部 API，*当内置函数与 API 同样有效或可以快速完成我的任务时*。

```py
class csvReader:

    def __init__(self, file_path: str) -> str:
        self.file_path = file_path

    def csv_text(self):
    """A function that returns a string of text containing
       the information within a csvReader object.

      Returns:
      text (str): A string of text containing information
      within a csvReader object.
     """ 
      text = str()
      with open(self.file_path, "r") as csvFile: 
        lines = csvFile.read().split(",") # "\r\n" if needed

        for val in lines:
          text += val + ' '
        text = text.replace('\ufeff', '')
        text = text.replace('\n', ' ')
      return text
```

# 额外：自然语言预处理类

在一个项目中处理所有上述类时，通过将每个类的输出格式化为文本字符串来进行归一化处理。我最近在处理各种文档时，还需要对每个字符串进行预处理，以增加更多的归一化以及移除不必要的信息。在将所有文件读入 Python 后，使用 *data processor* 类清理每个字符串，并在所有样本中标准化它们。

```py
class dataprocessor:

  def __init__(self):
    return

  @staticmethod
  def get_wordnet_pos(text: str) -> str:
    """Map POS tag to first character lemmatize() accepts.

       Parameters:
       text (str): A string of text

       Returns:
       tag: The tag of the word
    """
    tag = nltk.pos_tag([text])[0][1][0].upper()
    tag_dict = {"J": wordnet.ADJ,
                "N": wordnet.NOUN,
                "V": wordnet.VERB,
                "R": wordnet.ADV}
    return tag_dict.get(tag, wordnet.NOUN)

  @staticmethod
  def preprocess(input_text: str):
    """A function that accepts a string of text and conducts various
       NLP preprocessing steps on said text including puncation removal, 
      stopword removal and lemmanization.

       Parameters:
       input_text (str): A string of text

       Returns:
       output_text (str): A processed string of text.
    """
    #lowercase
    input_text = input_text.lower()

    #punctuation removal
    input_text = "".join([i for i in input_text if i not in string.punctuation])

    #Stopword removal
    stopwords = nltk.corpus.stopwords.words('english')
    custom_stopwords = ['\n','\n\n', '&', ' ', '.', '-', '$', '@']
    stopwords.extend(custom_stopwords)
    input_text = [i for i in input_text if i not in stopwords]
    input_text = ' '.join(word for word in input_text)

    #lemmanization
    lm = WordNetLemmatizer()
    input_text = [lm.lemmatize(word, dataprocessor.get_wordnet_pos(word)) for word in input_text.split(' ')]
    text = ' '.join(word for word in input_text)

    output_text = re.sub(' +', ' ',input_text)

    return output_text
```

# 结论

今天我们讨论了数据科学家在下一个项目中可以使用的不同 Python 类，以便读取各种文件类型。虽然还有其他可以读取到 Python 中的文件类型，但今天讨论的这些种类非常重要，有时可能会被忽视。使用今天介绍的文件读取类和最终的数据清理类，你可以比较和对比完全不同文件类型中的信息。例如，也许你想使用自然语言处理来查看不同的研究文献，找出哪些相似，然后推荐给学生作为当前课程的学习资料。这只是从今天提供的类中可以创建的众多不同项目之一，祝你享受！

**如果你喜欢今天的阅读内容，请给我关注，并告诉我是否有其他话题你希望我探索！如果你没有 Medium 账户，可以通过我的链接** [**这里**](https://ben-mccloskey20.medium.com/membership)**注册！另外，欢迎添加我的** [**LinkedIn**](https://www.linkedin.com/in/benjamin-mccloskey-169975a8/)**，或者随时联系我！感谢阅读！**
