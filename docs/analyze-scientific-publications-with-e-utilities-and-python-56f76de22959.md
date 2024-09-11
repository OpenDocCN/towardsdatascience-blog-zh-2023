# 用 E-utilities 和 Python 分析科学出版物

> 原文：[https://towardsdatascience.com/analyze-scientific-publications-with-e-utilities-and-python-56f76de22959?source=collection_archive---------3-----------------------#2023-05-19](https://towardsdatascience.com/analyze-scientific-publications-with-e-utilities-and-python-56f76de22959?source=collection_archive---------3-----------------------#2023-05-19)

## 如何收集科学文献数据并发现趋势

[](https://neuraljojo.medium.com/?source=post_page-----56f76de22959--------------------------------)[![Jozsef Meszaros](../Images/6a2bd76b1ccc00305c19cb6d41d1771e.png)](https://neuraljojo.medium.com/?source=post_page-----56f76de22959--------------------------------)[](https://towardsdatascience.com/?source=post_page-----56f76de22959--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----56f76de22959--------------------------------) [Jozsef Meszaros](https://neuraljojo.medium.com/?source=post_page-----56f76de22959--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F71417ba6ebf5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyze-scientific-publications-with-e-utilities-and-python-56f76de22959&user=Jozsef+Meszaros&userId=71417ba6ebf5&source=post_page-71417ba6ebf5----56f76de22959---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----56f76de22959--------------------------------) · 15分钟阅读 · 2023年5月19日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F56f76de22959&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyze-scientific-publications-with-e-utilities-and-python-56f76de22959&user=Jozsef+Meszaros&userId=71417ba6ebf5&source=-----56f76de22959---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F56f76de22959&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyze-scientific-publications-with-e-utilities-and-python-56f76de22959&source=-----56f76de22959---------------------bookmark_footer-----------)![](../Images/a7ae68b0dc6ab28cdb570c4867252f35.png)

图片来源：[Alex Block](https://unsplash.com/@alexblock?utm_source=medium&utm_medium=referral) 摄影于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

跟上科学研究趋势对于任何科学家、科学作家或有志于创办初创企业的人都至关重要。谈到搜索生物医学科学文献，大多数人会转向Google Scholar、PubMed或他们喜欢的参考管理工具。正如你可能预期的，这些面向公众的搜索工具提供了易用性，但牺牲了效率、控制和可扩展性。因此，它们通常在数据科学中并不实用。相反，你将需要使用由国家生物技术信息中心（NCBI）提供的E-utilities。[1] NCBI提供的科学文章存储在PubMed数据库中，该数据库主要涵盖生命科学研究，但也包括一些化学和物理相关的期刊。[2]

> 对于数据科学应用，基于网页的搜索引擎和流行的参考管理工具无法满足需求。

关于如何高效使用NCBI进行数据科学的信息散布在各种政府和学术网站上。许多资源上传于1990年代，集中于遗传序列的搜索而非原始文献出版物，并且代码示例仅用Perl编写。曾经有一门关于使用NCBI E-Utilities的课程在2000年代初由NIH主办，Powerpoint“幻灯片集”以及必要的剪贴画插图可在[此处](https://ftp.ncbi.nlm.nih.gov/pub/PowerTools/eutils/)获得。在本文中，我将传达我从这些各种来源中编纂的信息以及常规的反复试验中学到的经验。

> **注意：** NCBI中可搜索的数据类型很多，包括遗传序列、系统发育树、3-D结构及其他信息。本文将专注于搜索**原始** **文献**。

## NCBI可以回答哪些数据科学问题？

有许多种科学问题可以通过NCBI回答。例如，你可以创建一个作者提供的关键词的聚类图，以检查随时间变化的出版趋势，正如我在本文末尾演示的那样。此外，你还可以使用摘要和出版物中的文本构建LLMs和NLP模型，以帮助在文献之间建立联系。以下是我将在文章中覆盖的三个步骤：

(1) 查询数据库，

(2) 返回多个出版物的结果，以及

(3) 检索相似文章和文章的全文版本。

最后，我将提供一个完整的文档代码示例，展示如何对一个相对较大的语料库（约10,000篇文章）进行这三个步骤，并附上数据可视化。

# 查询NCBI数据库

要有效地查询NCBI数据库，你需要了解某些E-utilities，定义你的搜索字段，并选择你的搜索参数——这些参数控制结果如何返回到你的浏览器中，或者在我们的案例中，我们将使用Python来查询数据库。

## 四个最有用的E-utilities

NCBI提供了**九个E-utilities**，它们都实现为服务器端的快速CGI程序。这意味着你将通过创建以**.cgi**结尾的URL来访问它们，并在问号后指定查询参数，参数之间用&符号分隔。除了**EFetch**之外，它们都会提供**XML或JSON**输出。

+   `**ESearch**`生成符合你搜索查询的ID号码列表

以下E-Utilities可以与一个或多个ID号码一起使用：

+   `**ESummary**`期刊、作者列表、资助、日期、参考文献、出版类型

+   `**EFetch**`**仅XML**，提供`**ESummary**`的所有内容，以及摘要、用于研究的资助列表、作者机构和MeSH关键词

+   `**ELink**`提供与相关引文的链接列表，使用计算出的相似度分数***同时提供已发布项目的链接*** [你通向文章全文的门户]

NCBI在其服务器上托管了38个数据库，这些数据库涉及各种数据，超出了文献引用的范围。要获取当前数据库的完整列表，你可以使用**EInfo**而无需搜索词：

`[https://eutils.ncbi.nlm.nih.gov/entrez/eutils/einfo.fcgi](https://eutils.ncbi.nlm.nih.gov/entrez/eutils/einfo.fcgi)`

每个数据库在如何访问及返回的信息方面会有所不同。为了我们的目的，我们将重点关注*pubmed*和*pmc*数据库，因为这些是搜索和检索科学文献的地方。

了解搜索NCBI的两个最重要的方面是**搜索字段**和**输出**。**搜索字段**种类繁多，将取决于数据库。输出则更为直接，学习如何使用输出是至关重要的，特别是进行大规模搜索时。

## 搜索字段

你将无法真正利用E-utilities的潜力，如果你不了解可用的搜索字段。你可以在[NLM网站](https://www.nlm.nih.gov/bsd/mms/medlineelements.html)上找到这些搜索字段的完整列表及其描述，但要获取***特定数据库的最准确搜索词列表***，你需要使用这个链接解析自己的XML列表：

`[https://eutils.ncbi.nlm.nih.gov/entrez/eutils/einfo.fcgi?db=pubmed](https://eutils.ncbi.nlm.nih.gov/entrez/eutils/einfo.fcgi?db=pubmed)`

**db**标志设置为数据库（本文将使用*pubmed*，但文献也可以通过*pmc*获取）。

![](../Images/f468c29de103dbfc12170620a136f19f.png)

用于查询PubMed MEDLINE记录的搜索字段列表。（来源：[https://www.nlm.nih.gov/bsd/mms/medlineelements.html](https://www.nlm.nih.gov/bsd/mms/medlineelements.html)）`

一个特别有用的搜索字段是 Medline 主题词 (**MeSH**)。索引专家维护 PubMed 数据库，并使用 MeSH 术语来反映期刊文章的主题。每篇被索引的出版物通常由 10 到 12 个由索引专家精心挑选的 MeSH 术语描述。如果未指定搜索词，则查询将针对数据库中所有可用的搜索词执行。

## 查询参数

每个 E-utility 都通过 URL 行接受多个查询参数，你可以用来控制从查询返回的输出类型和数量。这是你可以设置检索结果数量或搜索日期的地方。以下是一些更重要的参数列表：

**数据库参数：**

+   `db` 应设置为你感兴趣的数据库 — *pubmed* 或 *pmc* 用于科学文献

**日期参数：** 你可以通过使用搜索字段（例如 [pdat] 代表出版日期）来获得对日期的更多控制，但日期参数提供了一种更方便的方式来限制结果。

+   `reldate` 相对于当前日期的天数，设置 reldate=1 以查询最近的一天

+   `mindate` 和 `maxdate` 根据格式 YYYY/MM/DD、YYYY 或 YYYY/MM 指定日期 **（查询必须同时包含 `mindate` 和 `maxdate` 参数）**

+   `datetype` 设置按日期查询时的日期类型 — 选项包括 ‘mdat’（修改日期）、‘pdat’（出版日期）和 ‘edat’（Entrez 日期）

**检索参数：**

+   `rettype` 返回的信息类型（对于文献搜索，使用默认设置）

+   `retmode` 输出格式（XML 为默认格式，虽然除 fetch 外的所有 E-utilities 都支持 JSON）

+   `retmax` 是返回记录的最大数量 — ***默认值为 20，最大值为 10,000***（一万）

+   `retstart` 给定查询的命中列表，`retstart` 指定索引（当搜索超出一万最大值时很有用）

+   `cmd` 仅与 `**ELink**` 相关，用于指定是否返回类似文章的 IDs 或全文的 URLs

# 使用 Python 执行查询并存储结果

一旦我们了解了 E-Utilities，选择了搜索字段，并决定了查询参数，我们就准备好执行查询并存储结果 — 甚至是多个页面的结果。

虽然你不需要专门使用 Python 来使用 E-utilities，但它确实使解析、存储和分析查询结果变得更加容易。下面是如何开始你的数据科学项目。

假设你要在 2022 年和 2023 年之间搜索“肌红蛋白”的 MeSH 术语。你现在将 **retmax** 设置为 50，但请记住 ***最大值为 10,000，查询速率为 3/s。***

```py
import urllib.request
search_url = f'http://eutils.ncbi.nlm.nih.gov/entrez//eutils/esearch.fcgi/' + \
              f'?db=pubmed' + \
              f'&term=myoglobin[mesh]' + \
              f'&mindate=2022' + \
              f'&maxdate=2023' + \
              f'&retmode=json' + \
              f'&retmax=50'

link_list = urllib.request.urlopen(search_url).read().decode('utf-8')
link_list
```

![](../Images/fcd81481c9d922509e560a15b150f2a0.png)

上述 **esearch** 查询的输出。

结果以ID列表的形式返回，可以在你查询的数据库中进行后续搜索。注意，**“count”**显示这个查询有154个结果，你可以用来获取某组搜索词的总出版物数。如果你想返回所有出版物的ID，你需要将**retmax**参数设置为计数值或154。一般来说，我将其设置为一个非常大的数字，以便能够检索所有结果并进行存储。

在PubMed中，**布尔搜索**很简单，只需在搜索词之间添加`+OR+`、`+NOT+`或`+AND+`到网址中即可。例如：

`[http://eutils.ncbi.nlm.nih.gov/entrez//eutils/esearch.fcgi/?db=pubmed&term=**CEO[cois]+OR+CTO[cois]+OR+CSO[cois]**&mindate=2022&maxdate=2023&retmax=10000](http://eutils.ncbi.nlm.nih.gov/entrez//eutils/esearch.fcgi/?db=pubmed&term=CEO%5Bcois%5D+OR+CTO%5Bcois%5D+OR+CSO%5Bcois%5D&mindate=2022&maxdate=2023&retmax=10000)`

这些搜索字符串可以使用Python构造。在接下来的步骤中，我们将使用Python的*json包*解析结果，以获取每篇返回出版物的ID。这些ID可以用于创建一个字符串——这个ID字符串可以被其他E-Utilities使用，以返回关于出版物的信息。

## 使用ESummary来返回关于出版物的信息。

**ESummary**的目的是返回你可能期望在论文引用中看到的数据（出版日期、页码、作者等）。一旦你获得了**ESearch**形式的ID列表（在上一步中），你可以将这个列表拼接成一个长网址。

> URL的限制是2048个字符，每篇出版物的ID长度为8个字符，因此，为了安全起见，如果你的ID列表超过250个，你应该将链接列表分成250个一批。有关示例，请参见文章底部的笔记本。

**ESummary**的结果以JSON格式返回，并可能包括链接到论文全文的链接。

```py
import json
result = json.loads( link_list )
id_list = ','.join( result['esearchresult']['idlist'] )

summary_url = f'http://eutils.ncbi.nlm.nih.gov/entrez//eutils/esummary.fcgi?db=pubmed&id={id_list}&retmode=json'

summary_list = urllib.request.urlopen(summary_url).read().decode('utf-8')
```

我们可以再次使用json来解析*summary_list*。使用json包时，可以通过summary[‘result’][id as string]来浏览每篇文章的字段，如下例所示：

```py
summary = json.loads( summary_list )
summary['result']['37047528']
```

我们可以创建一个数据框架来捕捉每篇文章的ID，以及期刊名称、出版日期、文章标题、检索全文的网址以及第一作者和最后一作者。

```py
uid = [ x for x in summary['result'] if x != 'uids' ]
journals = [ summary['result'][x]['fulljournalname'] for x in summary['result'] if x != 'uids' ]
titles = [ summary['result'][x]['title'] for x in summary['result'] if x != 'uids' ]
first_authors = [ summary['result'][x]['sortfirstauthor'] for x in summary['result'] if x != 'uids' ]
last_authors = [ summary['result'][x]['lastauthor'] for x in summary['result'] if x != 'uids' ]
links = [ summary['result'][x]['elocationid'] for x in summary['result'] if x != 'uids' ]
pubdates = [ summary['result'][x]['pubdate'] for x in summary['result'] if x != 'uids' ]

links = [ re.sub('doi:\s','http://dx.doi.org/',x) for x in links ]
results_df = pd.DataFrame( {'ID':uid,'Journal':journals,'PublicationDate':pubdates,'Title':titles,'URL':links,'FirstAuthor':first_authors,'LastAuthor':last_authors} )
```

以下是**ESummary**返回的所有不同字段的列表，以便你可以创建自己的数据库：

```py
'**uid**','**pubdate**','**epubdate**','**source**','**authors**','**lastauthor**','**title**',
'**sorttitle**','**volume**','**issue**','**pages**','**lang**','**nlmuniqueid**','**issn**',
'**essn**','**pubtype**','**recordstatus**','**pubstatus**','**articleids**','**history**',
'**references**','**attributes**','**pmcrefcount**','**fulljournalname**','**elocationid**',
'**doctype**','**srccontriblist**','**booktitle**','**medium**','**edition**',
'**publisherlocation**','**publishername**','**srcdate**','**reportnumber**',
'**availablefromurl**','**locationlabel**','**doccontriblist**','**docdate**',
'**bookname**','**chapter**','**sortpubdate**','**sortfirstauthor**','**vernaculartitle**'
```

## 当你需要摘要、关键词和其他细节（仅限XML输出）时，请使用EFetch。

我们可以使用**EFetch**返回类似于**ESummary**的字段，但有个警告是***结果仅以XML格式返回***。**EFetch**中还有几个有趣的附加字段，包括：摘要、作者选择的关键词、Medline子标题（MeSH术语）、资助研究的拨款、冲突声明、研究中使用的化学品列表，以及论文引用的所有参考文献的完整列表。以下是如何使用BeautifulSoup获取这些项目的一部分：

```py
from bs4 import BeautifulSoup
import lxml
import pandas as pd

abstract_url = f'http://eutils.ncbi.nlm.nih.gov/entrez//eutils/efetch.fcgi?db=pubmed&id={id_list}'
abstract_ = urllib.request.urlopen(abstract_url).read().decode('utf-8')
abstract_bs = BeautifulSoup(abstract_,features="xml")

articles_iterable = abstract_bs.find_all('PubmedArticle')

# Abstracts
abstract_texts = [ x.find('AbstractText').text for x in articles_iterable ]

# Conflict of Interest statements
coi_texts = [ x.find('CoiStatement').text if x.find('CoiStatement') is not None else '' for x in articles_iterable ]

# MeSH terms
meshheadings_all = list()
for article in articles_iterable:
  result = article.find('MeshHeadingList').find_all('MeshHeading')
  meshheadings_all.append( [ x.text for x in result ] )

# ReferenceList
references_all = list()
for article in articles_:
  if article.find('ReferenceList') is not None:
    result = article.find('ReferenceList').find_all('Citation')
    references_all.append( [ x.text for x in result ] )
  else:
    references_all.append( [] )

results_table = pd.DataFrame( {'COI':coi_texts, 'Abstract':abstract_texts, 'MeSH_Terms':meshheadings_all, 'References':references_all} )
```

现在我们可以使用这个表格来搜索摘要、冲突声明，或使用MeSH主题和参考列表制作连接不同研究领域的可视化图表。还有许多其他标签你可以探索，由**EFetch**返回，以下是如何使用BeautifulSoup查看所有这些标签：

```py
efetch_url = f'http://eutils.ncbi.nlm.nih.gov/entrez//eutils/efetch.fcgi?db=pubmed&id={id_list}'
efetch_result = urllib.request.urlopen( efetch_url ).read().decode('utf-8')
efetch_bs = BeautifulSoup(efetch_result,features="xml")

tags = efetch_bs.find_all()

for tag in tags:
  print(tag)
```

## **使用ELink检索相似出版物和全文链接**

你可能想找一些与你的搜索查询返回的文章相似的文章。这些文章根据使用概率主题模型的相似度评分进行分组。[5] 要检索给定ID的相似度评分，你必须在调用**ELink**时传递**cmd=neighbor_score**。以下是一个文章的示例：

```py
import urllib.request
import json

id_ = '37055458'
elink_url = f'http://eutils.ncbi.nlm.nih.gov/entrez//eutils/elink.fcgi?db=pubmed&id={id_}&retmode=json&cmd=neighbor_score'
elinks = urllib.request.urlopen(elink_url).read().decode('utf-8')

elinks_json = json.loads( elinks )

ids_=[];score_=[];
all_links = elinks_json['linksets'][0]['linksetdbs'][0]['links']
for link in all_links:
  [ (ids_.append( link['id'] ),score_.append( link['score'] )) for id,s in link.items() ]

pd.DataFrame( {'id':ids_,'score':score_} ).drop_duplicates(['id','score'])
```

**ELink**的另一个功能是根据文章的ID提供全文链接，如果你将**cmd=prlinks**传递给**ELink**，则可以返回这些链接。

> 如果你只希望访问那些对公众免费的全文链接，你需要使用包含“pmc”（PubMed Central）的链接。访问付费墙后的文章可能需要通过大学订阅——**在通过付费墙下载大量全文文章之前，你应该咨询你所在组织的图书管理员。**

下面是一个如何检索出版物链接的代码片段：

```py
id_ = '37055458'
elink_url = f'http://eutils.ncbi.nlm.nih.gov/entrez//eutils/elink.fcgi?db=pubmed&id={id_}&retmode=json&cmd=prlinks'
elinks = urllib.request.urlopen(elink_url).read().decode('utf-8')

elinks_json = json.loads( elinks )

[ x['url']['value'] for x in elinks_json['linksets'][0]['idurllist'][0]['objurls'] ]
```

你还可以在一次调用**ELink**时检索多个出版物的链接，如下所示：

```py
id_list = '37055458,574140'
elink_url = f'http://eutils.ncbi.nlm.nih.gov/entrez//eutils/elink.fcgi?db=pubmed&id={id_list}&retmode=json&cmd=prlinks'
elinks = urllib.request.urlopen(elink_url).read().decode('utf-8')

elinks_json = json.loads( elinks )

elinks_json
urls_ = elinks_json['linksets'][0]['idurllist']
for url_ in urls_:
  [ print( url_['id'], x['url']['value'] ) for x in url_['objurls'] ]
```

# 示例数据可视化：来自C-suite作者的科学出版物

有时，科学出版物的作者可能是公司的CEO、CSO或CTO。利用PubMed，我们可以分析最新的生命科学行业趋势。冲突声明（在2017年作为搜索词引入PubMed）提供了一个视角，了解哪些作者提供的关键词出现在声明了C-suite执行官身份的出版物中。换句话说，就是作者用来描述其发现的关键词。要实现这个功能，只需在URL中包含**CEO[cois]+OR+CSO[cois]+OR+CTO[cois]**作为搜索词，检索所有返回的结果，然后从每个出版物的XML输出中提取关键词。每个出版物包含4到8个关键词。一旦生成语料库，你可以按***每年指定关键词的出版物数量除以该年的出版物总数***来量化关键词的频率。

例如，如果10篇出版物列出了关键词“癌症”，而该年共有1000篇出版物，那么关键词频率将是0.001。使用seaborn clustermap模块和关键词频率，你可以生成一个可视化图，其中较深的色带表示关键词频率/年值较大（我已经从可视化中去掉了COVID-19和SARS-COV-2，因为它们的频率远大于0.05，正如预期的那样）。每年，大约有1000到1500篇论文被检索出来。

![](../Images/4c46cca20553c590a80efa73168b43ff.png)

使用Seaborn的clustermap模块生成的，列出C-suite作者的出版物的作者指定关键词频率的热图。

从这个可视化中，几个关于列出C-suite作者的出版物的数据洞察变得清晰。首先，一个最为明显的簇（位于底部）包含了在过去五年中在数据集中强烈出现的关键词：癌症、机器学习、生物标志物、人工智能——仅举几个例子。显然，行业在这些领域非常活跃并进行大量出版。第二个簇，位于图中部附近，展示了自2018年后从数据集中消失的关键词，包括身体活动、公共卫生、儿童、质谱和mhealth（或移动健康）。这并不是说这些领域在行业中没有发展，只是出版活动有所减缓。查看图的右下角，你可以提取出最近在数据集中出现的术语，包括液体活检和精准医学——这些确实是目前医学领域的两个非常“热门”的领域。通过进一步检查出版物，你可以提取出公司的名称和其他感兴趣的信息。下面是我编写的生成此可视化的代码：

```py
import pandas as pd
import time
from bs4 import BeautifulSoup
import seaborn as sns
from matplotlib import pyplot as plt
import itertools
from collections import Counter
from numpy import array_split
from urllib.request import urlopen

class Searcher:
    # Any instance of searcher will search for the terms and return the number of results on a per year basis #
    def __init__(self, start_, end_, term_, **kwargs):
        self.raw_ = input
        self.name_ = 'searcher'
        self.description_ = 'searcher'
        self.duration_ = end_ - start_
        self.start_ = start_
        self.end_ = end_
        self.term_ = term_
        self.search_results = list()
        self.count_by_year = list()
        self.options = list()

        # Parse keyword arguments

        if 'count' in kwargs and kwargs['count'] == 1:
            self.options = 'rettype=count'

        if 'retmax' in kwargs:
            self.options = f'retmax={kwargs["retmax"]}'

        if 'run' in kwargs and kwargs['run'] == 1:
            self.do_search()
            self.parse_results()

    def do_search(self):
        datestr_ = [self.start_ + x for x in range(self.duration_)]
        options = "".join(self.options)
        for year in datestr_:
            this_url = f'http://eutils.ncbi.nlm.nih.gov/entrez//eutils/esearch.fcgi/' + \
                       f'?db=pubmed&term={self.term_}' + \
                       f'&mindate={year}&maxdate={year + 1}&{options}'
            print(this_url)
            self.search_results.append(
                urlopen(this_url).read().decode('utf-8'))
            time.sleep(.33)

    def parse_results(self):
        for result in self.search_results:
            xml_ = BeautifulSoup(result, features="xml")
            self.count_by_year.append(xml_.find('Count').text)
            self.ids = [id.text for id in xml_.find_all('Id')]

    def __repr__(self):
        return repr(f'Search PubMed from {self.start_} to {self.end_} with search terms {self.term_}')

    def __str__(self):
        return self.description

# Create a list which will contain searchers, that retrieve results for each of the search queries
searchers = list()
searchers.append(Searcher(2022, 2023, 'CEO[cois]+OR+CTO[cois]+OR+CSO[cois]', run=1, retmax=10000))
searchers.append(Searcher(2021, 2022, 'CEO[cois]+OR+CTO[cois]+OR+CSO[cois]', run=1, retmax=10000))
searchers.append(Searcher(2020, 2021, 'CEO[cois]+OR+CTO[cois]+OR+CSO[cois]', run=1, retmax=10000))
searchers.append(Searcher(2019, 2020, 'CEO[cois]+OR+CTO[cois]+OR+CSO[cois]', run=1, retmax=10000))
searchers.append(Searcher(2018, 2019, 'CEO[cois]+OR+CTO[cois]+OR+CSO[cois]', run=1, retmax=10000))

# Create a dictionary to store keywords for all articles from a particular year
keywords_dict = dict()

# Each searcher obtained results for a particular start and end year
# Iterate over searchers
for this_search in searchers:

    # Split the results from one search into batches for URL formatting
    chunk_size = 200
    batches = array_split(this_search.ids, len(this_search.ids) // chunk_size + 1)

    # Create a dict key for this searcher object based on the years of coverage
    this_dict_key = f'{this_search.start_}to{this_search.end_}'

    # Each value in the dictionary will be a list that gets appended with keywords for each article
    keywords_all = list()

    for this_batch in batches:
        ids_ = ','.join(this_batch)

        # Pull down the website containing XML for all the results in a batch
        abstract_url = f'http://eutils.ncbi.nlm.nih.gov/entrez//eutils/efetch.fcgi?db=pubmed&id={ids_}'

        abstract_ = urlopen(abstract_url).read().decode('utf-8')
        abstract_bs = BeautifulSoup(abstract_, features="xml")
        articles_iterable = abstract_bs.find_all('PubmedArticle')

        # Iterate over all of the articles from the website
        for article in articles_iterable:
            result = article.find_all('Keyword')
            if result is not None:
                keywords_all.append([x.text for x in result])
            else:
                keywords_all.append([])

        # Take a break between batches!
        time.sleep(1)

    # Once all the keywords are assembled for a searcher, add them to the dictionary
    keywords_dict[this_dict_key] = keywords_all

    # Print the key once it's been dumped to the pickle
    print(this_dict_key)

# Limit to words that appeared approx five times or more in any given year

mapping_ = {'2018to2019':2018,'2019to2020':2019,'2020to2021':2020,'2021to2022':2021,'2022to2023':2022}
global_word_list = list()

for key_,value_ in keywords_dict.items():
  Ntitles = len( value_ )
  flattened_list = list( itertools.chain(*value_) )

  flattened_list = [ x.lower() for x in flattened_list ]
  counter_ = Counter( flattened_list )
  words_this_year = [ ( item , frequency/Ntitles , mapping_[key_] ) for item, frequency in counter_.items() if frequency/Ntitles >= .005 ]
  global_word_list.extend(words_this_year)

# Plot results as clustermap

global_word_df = pd.DataFrame(global_word_list)
global_word_df.columns = ['word', 'frequency', 'year']
pivot_df = global_word_df.loc[:, ['word', 'year', 'frequency']].pivot(index="word", columns="year",
                                                                    values="frequency").fillna(0)

pivot_df.drop('covid-19', axis=0, inplace=True)
pivot_df.drop('sars-cov-2', axis=0, inplace=True)

sns.set(font_scale=0.7)
plt.figure(figsize=(22, 2))
res = sns.clustermap(pivot_df, col_cluster=False, yticklabels=True, cbar=True)
```

# 结论

阅读完这篇文章后，你应该能够从制定高度定制的科学文献搜索查询，到生成数据可视化以进行更深入的审查。虽然有其他更复杂的方法可以使用各种E-utilities的附加功能来访问和存储文章，但我尝试展示了最简单的一组操作，应该适用于对科学出版趋势感兴趣的数据科学家。通过熟悉我所呈现的E-utilities，你将能更好地理解科学文献中的趋势和联系。如前所述，通过掌握E-utilities及其在NCBI数据库广阔宇宙中的操作，还有许多其他项目可以解锁。

要了解更多关于访问NCBI的信息，[你可以下载2008年之前举办的一系列NIH课程的材料](https://ftp.ncbi.nlm.nih.gov/pub/PowerTools/eutils/)或查看下面的参考资料。要保持对E-Utilities的更新，包括可能的新API，你可能想要订阅NCBI那看起来非常90年代的邮件列表，[请访问这个网站](https://www.ncbi.nlm.nih.gov/mailman/listinfo/utilities-announce/)。最后，在arxiv.org上搜索“PubMed”将返回一些结果，这些结果可以激发利用这些数据的研究项目。

## 参考文献

[1] [https://www.nlm.nih.gov/bsd/difference.html#:~:text=MEDLINE%20is%20the%20largest%20subset,Journal%20Categories%20filter%20called%20MEDLINE](https://www.nlm.nih.gov/bsd/difference.html#:~:text=MEDLINE%20is%20the%20largest%20subset,Journal%20Categories%20filter%20called%20MEDLINE)

[2] Chapman D. PubMed的高级搜索功能。J Can Acad Child Adolesc Psychiatry. 2009年2月；18(1):58–9\. PMID: 19270851; PMCID: PMC2651214.

[3] Sayers E. E-utilities深入探讨：参数、语法及更多内容。2009年5月29日 [更新于2022年11月30日]。见：Entrez编程实用工具帮助 [互联网]。Bethesda (MD): 国家生物技术信息中心 (US); 2010-. 可从 [https://www.ncbi.nlm.nih.gov/books/NBK25499/](https://www.ncbi.nlm.nih.gov/books/NBK25499/) 获得

[4] [https://ftp.ncbi.nlm.nih.gov/pub/PowerTools/eutils/Oct.2007/slides/Lecture1.pdf](https://ftp.ncbi.nlm.nih.gov/pub/PowerTools/eutils/Oct.2007/slides/Lecture1.pdf)

[5] Lin J, Wilbur WJ. PubMed相关文献：基于主题的内容相似性概率模型。BMC Bioinformatics. 2007年10月30日；8:423\. doi: 10.1186/1471–2105–8–423\. PMID: 17971238; PMCID: PMC2212667\. 可从 [https://pubmed.ncbi.nlm.nih.gov/17971238/](https://pubmed.ncbi.nlm.nih.gov/17971238/) 获得

[6] [https://library.mskcc.org/blog/2019/07/conflict-of-interest-statement-field-in-pubmed/](https://library.mskcc.org/blog/2019/07/conflict-of-interest-statement-field-in-pubmed/)
