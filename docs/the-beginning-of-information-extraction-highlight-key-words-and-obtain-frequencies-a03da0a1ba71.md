# 信息提取的起始：突出关键字并获取频率

> 原文：[`towardsdatascience.com/the-beginning-of-information-extraction-highlight-key-words-and-obtain-frequencies-a03da0a1ba71`](https://towardsdatascience.com/the-beginning-of-information-extraction-highlight-key-words-and-obtain-frequencies-a03da0a1ba71)

## 一种快速的方法，用于在 PDF 文档中突出显示感兴趣的关键字并计算其频率。

[](https://ben-mccloskey20.medium.com/?source=post_page-----a03da0a1ba71--------------------------------)![本杰明·麦克洛斯基](https://ben-mccloskey20.medium.com/?source=post_page-----a03da0a1ba71--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a03da0a1ba71--------------------------------)![数据科学](https://towardsdatascience.com/?source=post_page-----a03da0a1ba71--------------------------------) [本杰明·麦克洛斯基](https://ben-mccloskey20.medium.com/?source=post_page-----a03da0a1ba71--------------------------------)

·发表于 [数据科学](https://towardsdatascience.com/?source=post_page-----a03da0a1ba71--------------------------------) ·10 分钟阅读·2023 年 8 月 28 日

--

![](img/47bf25395e329662fb5f9381919d3942.png)

图片由 [朱迪·维拉斯克斯](https://unsplash.com/@roses_n_basil?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

随着每天可用信息量的增加，能够快速收集相关统计数据对于关系映射和获取对重复数据的新视角变得至关重要。今天我们将深入探讨 PDF 文本提取，也称为信息提取，以及对不同语料库进行快速事实和想法的形成。今天的文章深入探讨了自然语言处理（NLP）的领域，这是计算机理解人类语言的能力。

# 信息提取

**信息提取（IE）**，正如 Jurafsky 等人所定义的，是*“将嵌入文本中的非结构化信息转化为结构化数据的过程。”*[1]。一种非常快速的信息提取方法不仅是搜索以查找某个词是否出现在文本中，还包括计算该词提到的*频率*。这一点的支持是基于**一个词在文本中出现得越多，它就越重要，并且与语料库的主题关系越大。** 重要的是要注意，停用词的去除对于这个过程非常重要。为什么？因为如果你简单地计算语料库中所有单词的频率，像*the*这样的词会被提到很多次。这是否意味着这个词在传达文本信息方面很重要？不，因此你需要确保你关注的是那些有助于语料库语义意义的词汇频率。

信息提取（IE）可以导致在文档中使用其他自然语言处理技术。这些技术超出了本文的代码范围，但我认为它们既有趣又重要，因此值得分享。

第一种技术是**命名实体识别（NER）**。正如 Jurafsky 等人详细说明的那样，*“命名实体识别（NER）的任务是找到文本中每个命名实体的提及并标注其类型。”*[1] 这类似于在文本中搜索单词频率的想法，但 NER 更进一步，通过使用单词的位置以及其他文献规则来寻找文本中的不同实体。

信息提取（IE）还可以支持另一种技术，即**关系抽取**，这就是“*在文本实体之间找到并分类语义关系抽取*。”[1]。目标不仅是提取文本中不同词汇出现的频率以及它们所属的实体标签，还要如何将这些不同的实体联系在一起，以形成语料库的潜在语义意义、模式或摘要。

之前的三个任务都与**事件抽取**的目标相关。Jurafsky 等人进一步指出，“*事件抽取是寻找这些实体参与的事件*。”[1]。这意味着，通过从文本中寻找和提取不同的统计数据和标签，我们可以开始阐述关于这些事件发生的假设，并且通过这些实体了解不同事件之间可能存在的关系。

# 过程

![](img/8159545b1b4c139fe00c8330cdfd9684.png)

过程（图像来源于作者）

该过程的目标是快速了解一个文本语料库是否不仅包含与您所寻找的信息相关的内容，而且还提供频率数据，以便判断是否有足够的信息来进行不同文档之间的比较。最后，它提供了查询词在文本中高亮显示的可视化，这有助于根据周围的词语得出关于关系和文本内容的结论。整个过程是用 Python 在 Google Colab 中实现的，可以轻松地转移到您选择的任何 IDE 中。让我们看看代码吧！

## 代码

这个代码所需的 Python 库是[**PyMuPDF**](https://pypi.org/project/PyMuPDF/)和[**Counter**](https://pypi.org/project/Counter/)。

```py
 import fitz  # PyMuPDF
from collections import Counter
```

接下来，我们将创建一个名为“highlight_terms”的函数，该函数接受一个输入 PDF，一个输出 PDF 的路径，一个输出文本文件的路径，以及我们希望高亮显示的术语。

```py
def highlight_terms_and_count(input_pdf_path, output_pdf_path, terms_to_highlight, output_text_file):
   """
A function which accepts a PDF file and a sting of words as input 
and outputs a highlighted PDF file of the queried words and a text file 
with the query word frequences.

Arguments:
input_pdf_path (str): Path to a PDF file
output_pdf_file (str): Path to the output pdf file
terms_to_highlight (list): List of terms (str) to highlight
output_text_file (str): Path to output text file.

Returns
output_pd_file : A PDF highlighted with the queried words.
output_text_file: A text file containing the frequency of each queried word.
"""

# Open the PDF file
    pdf_document = fitz.open(input_pdf_path)
    term_counter = Counter()

    for page_number in range(len(pdf_document)):
        page = pdf_document[page_number]
        # Get the text on the page
        text = page.get_text()

        for term in terms_to_highlight:
            term_instances = page.search_for(term)
            term_counter[term] += len(term_instances)  # Count term instances on this page

            for term_rect in term_instances:
                # Create a highlight annotation
                highlight = page.add_highlight_annot(term_rect)
                # Set the color of the highlight (e.g., yellow)
                highlight.set_colors(stroke=(1, 1, 0))
                # Set the opacity of the highlight (0 to 1)
                highlight.set_opacity(0.5)

    # Save the modified PDF
    pdf_document.save(output_pdf_path)
    pdf_document.close()

    # Save term frequencies to a text file
    with open(output_text_file, 'w') as text_file:
        for term, frequency in term_counter.items():
            text_file.write(f"{term}: {frequency}\n")
```

一旦我们创建了函数，就可以设置文件路径到我们的不同目录。

```py
if __name__ == "__main__":
    input_pdf_path = "/content/AlexNet Paper.pdf"  # Replace with your input PDF file
    output_pdf_path = "/content/output.pdf"  # Replace with your output PDF file
    terms_to_highlight = ["neural", "networks"]  # Add the terms you want to highlight
    output_text_file = "/content/term_frequencies.txt"  # Text file to store term frequencies

    highlight_terms_and_count(input_pdf_path, output_pdf_path, terms_to_highlight, output_text_file)
```

就这样，简单如斯！这对于分析时快速了解某个词（或词组）在文档中的相关性非常有帮助。此外，当查看高亮显示的输出 PDF 时，标出词语的位置可以帮助引起您的注意，并与查询词周围的其他词语建立关系。

# 示例

在今天的示例中，让我们看看来自 Google Scholar 的一篇学术文章。如果我想知道一篇文章是否有足够的信息关于*Neural Networks*，该怎么办呢？我们可以查询文档，然后使用查询到的频率来评估文档是否集中于神经网络。我们将查看的第一篇文档标题为*ImageNet Classification with Deep Convolutional Neural Networks*[2]。

![](img/90c65f51feba69d4bbcabeb2de382308.png)

比如，我们对研究神经网络感兴趣，并想查看这些词在文本中出现的次数和位置。通过查询*Neural*和*Networks*，我们将得到一个高亮显示的 PDF，如下所示。

![](img/fdf3a14fdd05475fd47ceac46e7036a8.png)

此外，还将创建一个文本文件，记录*Neural*和*Network*这两个词的出现频率，分别为 21 和 23。词频的多少确实取决于用户是否认为某个词在文档中提及的次数足够，以至于被认为是相关的。由于这两个词在文档中出现超过 20 次，我认为它们与文档的上下文相关。

这个过程可以轻松扩展到多个文档中，然后可以比较不同文档之间某些词的频率。通过这样做，我们可以比较文档，并得出哪些文档与我们寻求的信息更相关的结论。

# 限制

虽然这种方法简单且能够完成所需的任务，但还是有一些值得讨论的限制。

1.  词汇在语料库中出现的频率越高就越能支持其潜在含义的假设并不总是成立。

1.  查询错误的词汇可能导致你无法揭示文本的正确含义或找到你所寻找的信息。

1.  该方法论更多地是对信息提取的总结，并为更深入的分析提供了一个起点。

关于假设的第一个局限性认为，词汇使用得越频繁，就越能支持文本的潜在含义，这取决于作者的发表方式和他们如何表达自己的观点，并不总是成立。此外，不同的词汇可以用来描述相同的含义，而仅关注几个高频词汇，你可能会发现自己对文本语料库附加了错误的含义。

第二个局限性来自于第一个关于词汇查询的限制。如果你没有查询正确的词汇，可能会错过文本语料库的重要潜在含义。克服这一点的一种方法是使用 PyMuPDF 或 PyPDF2 将 PDF 转换为文本文档，然后计算出现频率最高的 N 个词。从这些频率中，你可以找出这些词在文本中的位置，这有助于你找到最重要的信息所在。

今日方法论的第三个局限性是它没有深入到信息提取的层面。不过，这段代码确实很好地为你开始文本提取项目奠定了基础，并可以衍生出几个不同的方向。以下是一些你可以从这个过程继续发展的可能方式：

+   开始比较文档，选择某个词及其频率较高的文档。

+   评估文档中突出显示的区域，并创建一个脚本来提取这些区域。然后，对每个句子进行情感分析。

+   与刚才提到的方法相同，只不过现在要执行主题建模分析。任何被突出显示的句子都将用于支持文本中的不同主题。

# 优势

尽管我提到这个过程存在一些局限性，但它确实有一些好处，我相信你会发现它的使用有些实用之处。首先，它可以展示一个词在不同文本中的相关性。这在学术研究中尤其有帮助，特别是当你在研究某个主题时。如果我在研究机器学习，而一篇学术文章提到它的频率比另一篇高，我可能更倾向于先查看那篇文章。一旦我选择了一篇文章，这个过程将突出显示词汇在该文章中的位置。与其盲目地筛选文献，这个过程将我们的注意力集中在文本中的信息所在之处。说到节省时间！

这个过程的另一个好处是你可以开始标记某些句子的位置，这可能对你引用不同文档中的信息时非常有用。能够追溯到你找到这些信息的地方不仅对提供证据给你的观众很重要，也是一种确保你不会抄袭的好方法。我辅导了许多学生，并且使用这个过程为他们标记不同的文章，以便他们看到我从哪里获得我的工作（根据我们研究的主题），并将标记的文档作为学习的参考。

最后，这个过程是你文本提取项目的绝佳起点。它收集初步统计数据，以开始塑造项目方向，并提供一个可视化的文本理解工具，并为客户提供内在含义的证据。一个真实的例子是，我为一位主要在食品行业工作的客户开发了一个项目。他们想知道他们收到的这份巨大文档（数百页）是否提到他们寻求销售的商品，并且还想知道这些商品与谁相关。我能够查询他们销售的不同商品，并将文档中的产品高亮显示，以便快速参考。他们非常喜欢！我的唯一建议是开始注释页码，这将进一步缩短分析时间！

# 结论

今天我们讨论了如何分析 PDF 文档，找到并定位文档中的感兴趣词汇。此外，我们还可以收集查询词汇的频率，以更好地理解文本语料的可能含义，以及是否与我们想要深入了解的内容相关。信息提取至关重要，因为它不仅帮助我们发现不同实体之间的关系，还能提供关于这些相关实体和不同文档中可能出现的行为模式和事件的洞察。试试这个代码，希望它对你下一个自然语言处理项目有所帮助！

**如果你喜欢今天的阅读，请关注我，并告诉我是否有其他话题你希望我深入探讨！如果你没有 Medium 账户，可以通过我的链接** [**这里**](https://ben-mccloskey20.medium.com/membership) **注册（这样我会获得小额佣金）！此外，可以在** [**LinkedIn**](https://www.linkedin.com/in/benjamin-mccloskey-169975a8/) **上添加我，或者随时联系我！感谢阅读！**

# 来源

1.  [`web.stanford.edu/~jurafsky/slp3/old_oct19/17.pdf`](https://web.stanford.edu/~jurafsky/slp3/old_oct19/17.pdf)

1.  [`dl.acm.org/doi/10.5555/2999134.2999257`](https://dl.acm.org/doi/10.5555/2999134.2999257)
