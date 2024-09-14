# 一个学术研究（以及其他数据类型）的推荐系统！

> 原文：[`towardsdatascience.com/a-recommendation-system-for-academic-research-and-other-data-types-db5c5a68f1f5`](https://towardsdatascience.com/a-recommendation-system-for-academic-research-and-other-data-types-db5c5a68f1f5)

## 实施自然语言处理和图论来比较和推荐不同类型的文档

[](https://ben-mccloskey20.medium.com/?source=post_page-----db5c5a68f1f5--------------------------------)![本杰明·麦克洛斯基](https://ben-mccloskey20.medium.com/?source=post_page-----db5c5a68f1f5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----db5c5a68f1f5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----db5c5a68f1f5--------------------------------) [本杰明·麦克洛斯基](https://ben-mccloskey20.medium.com/?source=post_page-----db5c5a68f1f5--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----db5c5a68f1f5--------------------------------) ·阅读时间 15 分钟·2023 年 3 月 29 日

--

![](img/0dd61847b82293c61235cf84a6d1f226.png)

照片由 [Shubham Dhage](https://unsplash.com/@theshubhamdhage?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

# 介绍

许多人现在开发的项目通常都从第一个关键步骤开始：*积极研究*。投资于别人所做的工作并在此基础上构建对你的项目增加价值至关重要。你不仅要从他人的结论中学习，还要找出在你的项目中*不应该做*的事情，以确保成功。

在我完成论文的过程中，我开始收集各种类型的研究文件。例如，我有不同的学术出版物的合集，以及包含不同实验结果的 Excel 表格。当我完成论文的研究时，我在想：**是否有办法创建一个推荐系统，比较我档案中的所有研究，并帮助指导我下一个项目？**

事实上，确实有！

*注意：这不仅适用于你可能从各种搜索引擎收集的所有研究资料的存储库，也适用于你拥有的包含各种不同文档的目录。*

# 设置

我和我的团队使用 Python 3 开发了这个推荐系统。也要向他们致敬！我们在这里取得了很棒的成就。

有很多 API 支持这个推荐系统，研究每个具体的 API 可以执行的操作可能对你的学习有益。

```py
import string 
import csv
from io import StringIO
from pptx import Presentation
import docx2txt
import PyPDF2
import spacy
import pandas as pd 
import numpy as np
import nltk 
import re
import openpyxl
from nltk.stem import WordNetLemmatizer
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity
from gensim.parsing.preprocessing import STOPWORDS as SW
nltk.download('stopwords')
nltk.download('wordnet')
nltk.download('omw-1.4')
nltk.download('averaged_perceptron_tagger')
from nltk.corpus import wordnet
import networkx as nx
from networkx.algorithms.shortest_paths import weighted
import glob
```

## 障碍

我必须克服的一大难题是推荐机器比较不同类型文件的能力。例如，我想看看一个 Excel 电子表格中的信息是否与 PowerPoint 和学术 PDF 期刊中的信息相似或相关。做到这一点的诀窍是将每种文件类型读取到 Python 中，并将每个对象转换为一个单一的字符串。这*规范化*了所有数据，并允许计算相似性度量。

## PDF 阅读类

我们将首先关注的课程是**pdfReader**类，它可以将 PDF 格式化为可在 Python 中读取的格式。在所有文件格式中，我认为 PDF 是最重要的之一，因为许多从研究库（如 Google Scholar）下载的期刊文章都是 PDF 格式的。

```py
class pdfReader:

    def __init__(self, file_path: str) -> str:
        self.file_path = file_path

    def PDF_one_pager(self) -> str:
        """A function which returns a one line string of the 
            pdf.

            Returns:
            one_page_pdf (str): A one line string of the pdf.

        """
        content = ""
        p = open(self.file_path, "rb")
        pdf = PyPDF2.PdfReader(p)
        num_pages = len(pdf.pages)
        for i in range(0, num_pages):
            content += pdf.pages[i].extract_text() + "\n"
        content = " ".join(content.replace(u"\xa0", " ").strip().split())
        page_number_removal = r"\d{1,3} of \d{1,3}"
        page_number_removal_pattern = re.compile(page_number_removal, re.IGNORECASE)
        content = re.sub(page_number_removal_pattern, '',content)

        return content

    def pdf_reader(self) -> str:
        """A function which can read .pdf formatted files 
            and returns a python readable pdf.

            Returns:
            read_pdf: A python readable .pdf file.
        """
        opener = open(self.file_path,'rb')
        read_pdf = PyPDF2.PdfFileReader(opener)

        return read_pdf

    def pdf_info(self) -> dict:
        """A function which returns an information dictionary of a 
        pdf.

        Returns:
        dict(pdf_info_dict): A dictionary containing the meta
        data of the object.
        """
        opener = open(self.file_path,'rb')
        read_pdf = PyPDF2.PdfFileReader(opener)
        pdf_info_dict = {}
        for key,value in read_pdf.documentInfo.items():
            pdf_info_dict[re.sub('/',"",key)] = value
        return pdf_info_dict

    def pdf_dictionary(self) -> dict:
        """A function which returns a dictionary of 
            the object where the keys are the pages
            and the text within the pages are the 
            values.

            Returns:
            dict(pdf_dict): A dictionary pages and text.
        """
        opener = open(self.file_path,'rb')

        read_pdf = PyPDF2.PdfReader(opener)
        length = read_pdf.pages
        pdf_dict = {}
        for i in range(length):
            page = read_pdf.getPage(i)
            text = page.extract_text()
            pdf_dict[i] = text
            return pdf_dict
```

## Microsoft PowerPoint 阅读器

**pptReader** 类能够将 Microsoft PowerPoint 文件读取到 Python 中。

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

## Microsoft Word 文档阅读器

**wordDocReader** 类可以用于在 Python 中读取 Microsoft Word 文档。它利用了[*doc2txt*](https://pypi.org/project/doc2text/) API，并返回给定 Word 文档中位于的文本/信息的字符串。

```py
class wordDocReader:
  def __init__(self, file_path: str) -> str:
    self.file_path = file_path

  def word_reader(self):
  """A function that transforms a wordDocReader object into a Python readable
     word document."""

    text = docx2txt.process(self.file_path)
    text = text.replace('\n', ' ')
    text = text.replace('\xa0', ' ')
    text = text.replace('\t', ' ')
    return text 
```

## Microsoft Excel 阅读器

有时研究人员会将他们的结果以 Excel 表格的形式随出版物一起提供。能够读取列名，甚至是数值，可能有助于推荐与你搜索内容相似的结果。例如，如果你在研究某只股票的过去表现？也许你会在历史表现的 Excel 表格中搜索名称和符号。这种推荐系统会向你推荐这个 Excel 表格，以帮助你的研究。

```py
 class xlsxReader:

    def __init__(self, file_path: str) -> str:
        self.file_path = file_path

    def xlsx_text(self):
      """A function which returns the string of an 
         excel document.

          Returns:
          text(str): String of text of a document.
      """
      inputExcelFile = self.file_path
      text = str()
      wb = openpyxl.load_workbook(inputExcelFile)
      #This will save the excel sheet as a CSV file
      for sn in wb.sheetnames:
        excelFile = pd.read_excel(inputExcelFile, engine = 'openpyxl', sheet_name = sn)
        excelFile.to_csv("ResultCsvFile.csv", index = None, header=True)

        with open("ResultCsvFile.csv", "r") as csvFile: 
          lines = csvFile.read().split(",") # "\r\n" if needed
          for val in lines:
            if val != '':
              text += val + ' '
          text = text.replace('\ufeff', '')
          text = text.replace('\n', ' ')
      return textCSV File Reader
```

**csvReader** 类将允许将 CSV 文件包含在你的数据库中，并在系统的推荐中使用。

```py
 class csvReader:

    def __init__(self, file_path: str) -> str:
        self.file_path = file_path

    def csv_text(self):
      """A function which returns the string of a
         csv document.

          Returns:
          text(str): String of text of a document.
      """
      text = str()
      with open(self.file_path, "r") as csvFile: 
        lines = csvFile.read().split(",") # "\r\n" if needed
        for val in lines:
          text += val + ' '
        text = text.replace('\ufeff', '')
        text = text.replace('\n', ' ')
      return textMicrosoft PowerPoint Reader
```

这是一个有用的类。不是很多人会考虑到 PowerPoint 演示文稿中存储着有价值的信息。这些演示文稿主要是为了将关键观点和信息可视化展示给观众。以下课程将帮助你将数据库中的任何 PowerPoint 文件与其他信息体关联，希望能引导你找到相关的工作。

```py
class pptReader:

    def __init__(self, file_path: str) -> str:
        self.file_path = file_path

    def ppt_text(self):
      """A function which returns the string of a 
        Mirocsoft PowerPoint document.

        Returns:
        text(str): String of text of a document.
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

      return textMicrosoft Word Document Reader
```

本系统的最终课程是一个 Microsoft Word 文档阅读器。Word 文档是另一个宝贵的信息来源。许多人会撰写报告，以 Word 文档格式表示他们的发现和想法。

```py
class wordDocReader:
  def __init__(self, file_path: str) -> str:
    self.file_path = file_path

  def word_reader(self):
    """A function which returns the string of a 
          Microsoft Word document.

          Returns:
          text(str): String of text of a document.
      """
    text = docx2txt.process(self.file_path)
    text = text.replace('\n', ' ')
    text = text.replace('\xa0', ' ')
    text = text.replace('\t', ' ')
    return text 
```

今天项目中使用的课程就到这里了。**请注意：** 还有许多其他文件类型可以用来增强你的推荐系统。当前版本的代码正在开发中，将接受图像并尝试将其与数据库中的其他文档关联！

# 数据处理

## 预处理

让我们看看如何预处理这些数据。这个推荐系统是为一个学术研究库构建的，因此使用自然语言处理（NLP）指导的预处理步骤来分解文本是非常重要的。

数据处理类被简单地称为*datapreprocessor*，类中的第一个函数是一个词性标记器。

```py
class dataprocessor:
  def __init__(self):
    return

  @staticmethod
  def get_wordnet_pos(text: str) -> str:
    """Map POS tag to first character lemmatize() accepts
    Inputs:
    text(str): A string of text

    Returns:
    tag_dict(dict): A dictionary of tags
    """
    tag = nltk.pos_tag([text])[0][1][0].upper()
    tag_dict = {"J": wordnet.ADJ,
                "N": wordnet.NOUN,
                "V": wordnet.VERB,
                "R": wordnet.ADV}

    return tag_dict.get(tag, wordnet.NOUN)
```

这个函数为单词标记词性，这在项目后续阶段会非常有用。

第二步，有一个函数执行许多我们已经见过的标准 NLP 步骤。这些步骤包括：

1.  将每个单词都转为小写

1.  去掉标点符号

1.  去掉数字（*我只想查看非数字信息。如果需要，可以省略此步骤*）

1.  停用词移除。

1.  词形还原。**在这一过程中，get_wordnet_pos()函数对于包含词性非常有用！**

```py
@staticmethod
  def preprocess(text: str):
    """A function that prepoccesses text through the
    steps of Natural Language Processing (NLP).
      Inputs:
      text(str): A string of text

      Returns:
      text(str): A processed string of text
      """
    #lowercase
    text = text.lower()

    #punctuation removal
    text = "".join([i for i in text if i not in string.punctuation])

    #Digit removal (Only for ALL numeric numbers)
    text = [x for x in text.split(' ') if x.isnumeric() == False]

    #Stop removal
    stopwords = nltk.corpus.stopwords.words('english')
    custom_stopwords = ['\n','\n\n', '&', ' ', '.', '-', '$', '@']
    stopwords.extend(custom_stopwords)

    text = [i for i in text if i not in stopwords]
    text = ' '.join(word for word in text)

    #lemmanization
    lm = WordNetLemmatizer()
    text = [lm.lemmatize(word, dataprocessor.get_wordnet_pos(word)) for word in text.split(' ')]
    text = ' '.join(word for word in text)

    text = re.sub(' +', ' ',text)

    return text
```

接下来，有一个函数可以将所有文件读入系统中。

```py
@staticmethod
  def data_reader(list_file_names):
    """A function that reads in the data from a directory of files.

    Inputs:
    list_file_names(list): List of the filepaths in a directory.

    Returns:
    text_list (list): A list where each value is a string of text
    for each file in the directory
    file_dict(dict): Dictionary where the keys are the filename and the values
    are the information found within each given file
    """

    text_list = []
    reader = dataprocessor()
    for file in list_file_names:
      temp = file.split('.')
      filetype = temp[-1]
      if filetype == "pdf":
        file_pdf = pdfReader(file)
        text = file_pdf.PDF_one_pager()

      elif filetype == "docx":
        word_doc_reader = wordDocReader(file)
        text = word_doc_reader.word_reader()

      elif filetype == "pptx" or filetype == 'ppt':
        ppt_reader = pptReader(file)
        text = ppt_reader.ppt_text()

      elif filetype == "csv":
        csv_reader = csvReader(file)
        text = csv_reader.csv_text()

      elif filetype == 'xlsx':
        xl_reader = xlsxReader(file)
        text = xl_reader.xlsx_text()
      else:
        print('File type {} not supported!'.format(filetype))
        continue

      text = reader.preprocess(text)
      text_list.append(text)
      file_dict = dict()
      for i,file in enumerate(list_file_names):
        file_dict[i] = (file, file.split('/')[-1])
    return text_list, file_dict
```

**由于这是该系统的第一个版本，我想强调一下代码可以调整以包含许多其他文件类型！**

下一个函数被称为**database_preprocess()**，用于处理给定数据库中的所有文件。输入是一个文件列表，每个文件都有其关联的文本字符串（已处理）。这些文本字符串随后使用**sklearn 的 tfidfVectorizer**进行*向量化*。这到底是什么？基本上，它会根据每个给定单词的频率，将所有文本转换为不同的特征向量。我们这样做是为了通过与向量算术相关的相似度公式查看文档的相关性。

```py
@staticmethod
@staticmethod
  def database_processor(file_dict,text_list: list):
    """A function that transforms the text of each file within the 
    database into a vector.

    Inputs:
    file_dixt(dict): Dictionary where the keys are the filename and the values
    are the information found within each given file
    text_list (list): A list where each value is a string of the text
    for each file in the directory

    Returns:
    list_dense(list): A list of the files' text turned into vectors.
    vectorizer: The vectorizor used to transform the strings of text
    file_vector_dict(dict): A dictionary where the file names are the keys
    and the vectors of each files' text are the values.
    """
    file_vector_dict = dict()
    vectorizer = TfidfVectorizer()
    vectors = vectorizer.fit_transform(text_list)
    feature_names = vectorizer.get_feature_names_out()
    matrix = vectors.todense()
    list_dense = matrix.tolist()
    for i in range(len(list_dense)):
      file_vector_dict[file_dict[i][1]] = list_dense[i]

    return list_dense, vectorizer, file_vector_dict
```

创建一个基于数据库的向量化器的原因是，当用户给出一个需要在数据库中搜索的术语列表时，这些单词将根据其在数据库中的频率被向量化。**这是当前系统的最大弱点。随着数据库规模的增加，计算相似度所需的时间和计算资源将增加，从而减慢系统速度**。在质量控制会议上提出的一个建议是使用强化学习来推荐不同的数据文章。

接下来，我们可以使用一个输入处理器，将任何提供的单词处理成向量。这类似于你在搜索引擎中输入请求的过程。

```py
 @staticmethod
  def input_processor(text, TDIF_vectorizor):
    """A function which accepts a string of text and vectorizes the text using a 
     TDIF vectorizoer.

    Inputs:
    text(str): A string of text
    TDIF_vectorizor: A pretrained vectorizor

    Returns:
    words(list): A list of the input text in vectored form.
    """
    words = ''
    total_words = len(text.split(' '))
    for word in text.split(' '):
      words += (word + ' ') * total_words
      total_words -= 1

    words = [words[:-1]]
    words = TDIF_vectorizor.transform(words)
    words = words.todense()
    words = words.tolist()
    return words
```

由于数据库中的所有信息及给定的信息都将是向量，我们可以使用**余弦相似度**来计算向量之间的角度。角度越接近 0，两个向量的相似度就越低。

```py
@staticmethod
  def similarity_checker(vector_1, vector_2):
    """A function which accepts two vectors and computes their cosine similarity.

    Inputs:
    vector_1(int): A numerical vector
    vector_2(int): A numerical vector

    Returns:
    cosine_similarity([vector_1], vector_2) (int): Cosine similarity score
    """
    vectors = [vector_1, vector_2]
    for vec in vectors:
      if np.ndim(vec) == 1:
        vec = np.expand_dims(vec, axis=0)
    return cosine_similarity([vector_1], vector_2)
```

一旦找到两个向量之间的相似度分数，便可以创建搜索词与数据库中文档之间的排名。

```py
@staticmethod
  def recommender(vector_file_list,query_vector, file_dict):
    """A function which accepts a list of vectors, query vectors, and a dictionary
    pertaining to the list of vectors with their original values and file names.

    Inputs:
    vector_file_list(list): A list of vectors
    query_vector(int): A numerical vector
    file_dict(dict): A dictionary of filenames and text relating to the list
    of vectors

    Returns:
    final_recommendation (list): A list of the final recommended files
    similarity_list[:len(final_recommendation)] (list): A list of the similarity
    scores of the final recommendations.
    """
    similarity_list = []
    score_dict = dict()
    for i,file_vector in enumerate(vector_file_list):
      x = dataprocessor.similarity_checker(file_vector, query_vector)
      score_dict[file_dict[i][1]] = (x[0][0])
      similarity_list.append(x)
    similarity_list = sorted(similarity_list, reverse = True)
    #Recommends the top 20%
    recommended = sorted(score_dict.items(), 
                  key=lambda x:-x[1])[:int(np.round(.5*len(similarity_list)))]

    final_recommendation = []
    for i in range(len(recommended)):
      final_recommendation.append(recommended[i][0])
    #add in graph for greater than 3 recommendationa
    return final_recommendation, similarity_list[:len(final_recommendation)]
```

向量文件列表是我们之前从文件中创建的向量列表。查询向量是正在搜索的词的向量。之前创建了文件字典，它使用文件名作为键，文件文本作为值。计算相似度后，会创建一个排名，优先推荐与查询词最相似的信息。注意，如果推荐结果超过 3 个怎么办？结合**网络与图论**的元素将为系统增加额外的计算优势，并生成更有信心的推荐。

## 页面排名理论

让我们稍作绕道，回顾一下页面排名理论。别误会，余弦相似度是测量向量之间相似性的强大计算方法，但将页面排名融入你的推荐算法，可以进行多个向量（你数据库中的数据）之间的相似性比较。

页面排名最初由拉里·佩奇（Larry Page）设计，用于对网站进行排名并衡量其重要性[1]。基本思想是，如果更多的网站链接到一个网站，那么这个网站可以被认为是“更重要的”。借鉴这一思想，如果图中的一个节点到其他节点的边距离减少，该节点可以被认为更重要。节点相对于图中其他节点的整体距离越短，该节点就越重要。

今天我们将使用页面排名的一种变体，称为*特征向量中心性*。*特征向量中心性*类似于页面排名，它衡量图中节点之间的连接，为更强的连接分配更高的分数。最大不同点？*特征向量中心性*会考虑与给定节点连接的节点的重要性，以估计该节点的相对重要性。这就好比说，认识许多重要人物的人，可能自己也通过这些强大的关系变得非常重要。总体而言，这两个算法在实现方式上非常相似。

对于这个数据库，在计算出向量之后，可以将它们放入图中，其中它们的边距离由它们与其他向量的相似度决定。

```py
@staticmethod
  def ranker(recommendation_val, file_vec_dict):
    """A function which accepts a list of recommendaton values and a dictionary
    files wihin the databse and their vectors.

    Inputs:
    reccomendation_val(list): A list of recommendations found through cosine
    similarity
    file_vec_dic(dict): A dictionary of the filenames as keys and their
    text in vectors as the values.

    Returns:
    ec_recommended(list): A list of the top 20% recommendations found using the 
    eigenvector centrality algorithm.
    """
    my_graph = nx.Graph()
    for i in range(len(recommendation_val)):
      file_1 = recommendation_val[i]
      for j in range(len(recommendation_val)):
        file_2 = recommendation_val[j]

        if i != j:
          #Calculate sim_score between two values (weight)
          edge_dist = cosine_similarity([file_vec_dict[recommendation_val[i]]],[file_vec_dict[recommendation_val[j]]])
          #add an edge from file 1 to file 2 with the weight 
          my_graph.add_edge(file_1, file_2, weight=edge_dist)

    #Pagerank the graph  ]    
    rec = nx.eigenvector_centrality(my_graph)
    #Takes 20% of the values
    ec_recommended = sorted(rec.items(), key=lambda x:-x[1])[:int(np.round(len(rec)))]

    return ec_recommended
```

好了，接下来呢？我们有使用数据库中每个数据点之间的余弦相似度创建的推荐，以及通过特征向量中心性算法计算的推荐。我们应该输出哪些推荐？**两者都输出！**

```py
@staticmethod
  def weighted_final_rank(sim_list,ec_recommended,final_recommendation):
    """A function which accepts a list of similiarity values found through 
      cosine similairty, recommendations found through eigenvector centrality,
      and the final recommendations produced by cosine similarity.

        Inputs:
        sim_list(list): A list of all of the similarity values for the files
        within the database.
        ec_recommended(list): A list of the top 20% recommendations found using the 
        eigenvector centrality algorithm.
        final_recommendation (list): A list of the final recommendations found
        by using cosine similarity.

        Returns:
        weighted_final_recommend(list): A list of the final recommendations for 
        the files in the database.
        """
    final_dict = dict()

    for i in range(len(sim_list)):
      val = (.8*sim_list[final_recommendation.index(ec_recommendation[i][0])].squeeze()) + (.2 * ec_recommendation[i][1])
      final_dict[ec_recommendation[i][0]] = val

    weighted_final_recommend = sorted(final_dict.items(), key=lambda x:-x[1])[:int(np.round(len(final_dict)))]

    return weighted_final_recommend
```

这个脚本的最终功能将根据余弦相似度和特征向量中心性加权不同的推荐。目前，80% 的权重将分配给余弦相似度生成的推荐，20% 的权重将分配给特征向量中心性生成的推荐。最终的推荐可以根据这些权重进行计算，并汇总生成代表系统中所有相似度计算的推荐。开发者可以轻松调整权重，以反映他们认为更重要的推荐批次。

# 示例

让我们用这段代码做一个快速示例。我的数据库中的文档都以之前讨论的格式存在，并涉及机器学习的不同领域。数据库中更多的文档与*生成对抗网络（GANs）*相关，因此我预计当查询词为“生成对抗网络”时，这些文档会被优先推荐。

```py
path = '/content/drive/MyDrive/database/'
db = [f for f in glob.glob(path + '*')]

research_documents, file_dictionary = dataprocessor.data_reader(db)
list_files, vectorizer, file_vec_dict = dataprocessor.database_processor(file_dictionary,research_documents)
query = 'Generative Adversarial Networks'
query = dataprocessor.preprocess(query)
query = dataprocessor.input_processor(query, vectorizer)
recommendation, sim_list = dataprocessor.recommender(list_files,query, file_dictionary)
ec_recommendation = dataprocessor.ranker(recommendation, file_vec_dict)
final_weighted_recommended = dataprocessor.weighted_final_rank(sim_list,ec_recommendation,  recommendation)
print(final_weighted_recommended)
```

运行这段代码会生成以下推荐，并附上每个推荐的权重值。

**[(‘GAN_presentation.pptx’, 0.3411272882084124), (‘Using GANs to Augment UAV Data_V2.docx’, 0.16293615818015078), (‘GANS_DAY_1.docx’, 0.12546058188955278), (‘ml_pdf.pdf’, 0.10864164490536887)]**

再试一次。如果我查询“机器学习”会怎么样？

**[(‘ml_pdf.pdf’, 0.31244922151487337), (‘GAN_presentation.pptx’, 0.18170070184645432), (‘GANS_DAY_1.docx’, 0.14825501243059303), (‘Using GANs to Augment UAV Data_V2.docx’, 0.1309153863914564)]**

啊哈！正如预期的那样，第一个推荐文档是机器学习的介绍性简报！我仅使用了 7 个文档作为示例，添加的文档越多，收到的推荐也会越多！

# 结论

今天我们探讨了如何为你收集的文件创建推荐系统（尤其是当你收集研究材料用于项目时）。该系统的主要特点是，通过采用特征向量中心性算法计算向量的余弦相似度，从而提供更简洁、更好的推荐。今天试试看，希望这能帮助你更好地理解你拥有的数据之间的相关性。

**如果你喜欢今天的阅读内容，请关注我，并告诉我是否有其他你希望我探讨的主题！如果你没有 Medium 账号，可以通过我的链接** [**这里**](https://ben-mccloskey20.medium.com/membership) **注册（这样我会收到少量佣金）！此外，你还可以在** [**LinkedIn**](https://www.linkedin.com/in/benjamin-mccloskey-169975a8/) **上添加我，或随时联系我！感谢你的阅读！**

## 来源

1.  [`www.geeksforgeeks.org/page-rank-algorithm-implementation/`](https://www.geeksforgeeks.org/page-rank-algorithm-implementation/)

1.  完整代码：[`github.com/benmccloskey/Research_recommend_model/blob/main/app.py`](https://github.com/benmccloskey/Research_recommend_model/blob/main/app.py)
