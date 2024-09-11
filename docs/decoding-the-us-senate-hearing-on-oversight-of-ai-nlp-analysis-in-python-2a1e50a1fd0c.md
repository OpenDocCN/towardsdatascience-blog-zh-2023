# 解码美国参议院对AI的监督听证会：Python中的NLP分析

> 原文：[https://towardsdatascience.com/decoding-the-us-senate-hearing-on-oversight-of-ai-nlp-analysis-in-python-2a1e50a1fd0c?source=collection_archive---------7-----------------------#2023-06-02](https://towardsdatascience.com/decoding-the-us-senate-hearing-on-oversight-of-ai-nlp-analysis-in-python-2a1e50a1fd0c?source=collection_archive---------7-----------------------#2023-06-02)

![](../Images/53fe9cfa4140a8125d6870c45cd8085c.png)

摄影： [Harold Mendoza](https://unsplash.com/@haroldrmendoza?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 使用NLTK工具包进行词频分析、可视化和情感评分

[](https://medium.com/@raul.vizcarrach?source=post_page-----2a1e50a1fd0c--------------------------------)[![Raul Vizcarra Chirinos](../Images/9f507c6b9542809b9a32ab185e953ca1.png)](https://medium.com/@raul.vizcarrach?source=post_page-----2a1e50a1fd0c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2a1e50a1fd0c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2a1e50a1fd0c--------------------------------) [Raul Vizcarra Chirinos](https://medium.com/@raul.vizcarrach?source=post_page-----2a1e50a1fd0c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3da7d5d185b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecoding-the-us-senate-hearing-on-oversight-of-ai-nlp-analysis-in-python-2a1e50a1fd0c&user=Raul+Vizcarra+Chirinos&userId=3da7d5d185b5&source=post_page-3da7d5d185b5----2a1e50a1fd0c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2a1e50a1fd0c--------------------------------) ·21分钟阅读·2023年6月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2a1e50a1fd0c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecoding-the-us-senate-hearing-on-oversight-of-ai-nlp-analysis-in-python-2a1e50a1fd0c&user=Raul+Vizcarra+Chirinos&userId=3da7d5d185b5&source=-----2a1e50a1fd0c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2a1e50a1fd0c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecoding-the-us-senate-hearing-on-oversight-of-ai-nlp-analysis-in-python-2a1e50a1fd0c&source=-----2a1e50a1fd0c---------------------bookmark_footer-----------)

上周日早晨，当我在换频道找早餐时可以看的节目时，我偶然发现了参议院对 AI 监管听证会的重播。距离开始已经过去了 40 分钟，所以我决定观看剩下的部分（谈到度过一个有趣的周日早晨！）。

当像参议院司法委员会对 AI 监管的听证会这样的事件发生时，如果你想了解关键要点，你有四个选择：观看直播，寻找未来的录音（这两个选项都需要你花费三小时）；阅读书面版本（转录本），它们大约有 79 页，超过 29,000 个词；或者在网站或社交媒体上阅读评论以获取不同的观点并形成自己的看法（如果不是来自其他人）。

如今，随着一切变化如此迅速，我们的时间似乎总是过于短暂，人们很容易选择捷径，依赖评论而不是查阅原始来源（我也有过这样的经历）。如果你选择这个听证会的捷径，很可能你在网上或社交媒体上找到的大多数评论都会集中在 OpenAI CEO Sam Altman 呼吁监管 AI 上。然而，看过听证会后，我觉得还有更多内容值得探索，超越头条新闻。

所以，在我完成了周日的休闲早晨活动后，我决定下载参议院听证会的 transcript，并使用 NLTK 包（一个用于自然语言处理的 Python 包——NLP）来分析它，比较最常用的词汇，并对不同的兴趣群体（OpenAI、IBM、学术界、国会）应用一些情感评分，看看是否能发现其中的含义。剧透警告！在分析的 29,000 个词中，只有 70 个（0.24%）与“regulation”，“regulate”，“regulatory”或“legislation”等词相关。

需要注意的是，这篇文章并不是关于我对这次 AI 听证会或 ChatGPT 的 Sam Altman 的看法。相反，它关注的是在国会山这个屋檐下，各个社会部分（私人、学术界、政府）所代表的各方言辞背后的含义，以及我们从这些混杂的言辞中能学到什么。

鉴于未来几个月在人工智能监管方面的有趣时刻，因为 EU AI Act 的最终草案等待在欧洲议会辩论（预计在 6 月进行），探索大西洋这边围绕 AI 的讨论背后的内容是值得的。

## 步骤-01：获取数据

我使用了 Justin Hendrix 在 [Tech Policy Press](https://techpolicy.press/) 发布的 transcript（[可在这里访问](https://techpolicy.press/transcript-senate-judiciary-subcommittee-hearing-on-oversight-of-ai/)）。

![](../Images/5e6e0448ce258d146d6a59664c4d539e.png)

访问参议院听证会 transcript [这里](https://techpolicy.press/transcript-senate-judiciary-subcommittee-hearing-on-oversight-of-ai/)

虽然亨德里克斯提到这是一个快速转录，并建议通过观看参议院听证会视频来确认引述，但我仍然发现它对这次分析非常准确和有趣。如果你想观看参议院听证会或阅读萨姆·奥特曼（OpenAI）、克里斯蒂娜·蒙哥马利（IBM）和加里·马库斯（纽约大学教授）的证词，你可以在[这里](https://www.judiciary.senate.gov/committee-activity/hearings/oversight-of-ai-rules-for-artificial-intelligence)找到它们。

最初，我计划将转录文本复制到Word文档中，并在Excel中手动创建一个包含参与者姓名、他们代表的组织及其评论的表格。然而，这种方法既耗时又低效。所以，我转向了Python，并将Microsoft Word文件中的完整转录文本上传到数据框中。以下是我使用的代码：

```py
# STEP 01-Read the Word document
# remember to install  pip install python-docx

import docx
import pandas as pd

doc = docx.Document('D:\....your word file on microsoft word')

items = []
names = []
comments = []

# Iterate over paragraphs 
for paragraph in doc.paragraphs:
    text = paragraph.text.strip()

    if text.endswith(':'):
        name = text[:-1]  
    else:
        items.append(len(items))
        names.append(name)
        comments.append(text)

dfsenate = pd.DataFrame({'item': items, 'name': names, 'comment': comments})

# Remove rows with empty comments
dfsenate = dfsenate[dfsenate['comment'].str.strip().astype(bool)]

# Reset the index
dfsenate.reset_index(drop=True, inplace=True)
dfsenate['item'] = dfsenate.index + 1
print(dfsenate)
```

输出应如下所示：

```py
 item name comment
0 1 Sen. Richard Blumenthal (D-CT) Now for some introductory remarks.
1 2 Sen. Richard Blumenthal (D-CT) “Too often we have seen what happens when technology outpaces regulation, the unbridled exploitation of personal data, the proliferation of disinformation, and the deepening of societal inequalities. We have seen how algorithmic biases can perpetuate discrimination and prejudice, and how the lack of transparency can undermine public trust. This is not the future we want.”
2 3 Sen. Richard Blumenthal (D-CT) If you were listening from home, you might have thought that voice was mine and the words from me, but in fact, that voice was not mine. The words were not mine. And the audio was an AI voice cloning software trained on my floor speeches. The remarks were written by ChatGPT when it was asked how I would open this hearing. And you heard just now the result I asked ChatGPT, why did you pick those themes and that content? And it answered. And I’m quoting, Blumenthal has a strong record in advocating for consumer protection and civil rights. He has been vocal about issues such as data privacy and the potential for discrimination in algorithmic decision making. Therefore, the statement emphasizes these aspects.
3 4 Sen. Richard Blumenthal (D-CT) Mr. Altman, I appreciate ChatGPT’s endorsement. In all seriousness, this apparent reasoning is pretty impressive. I am sure that we’ll look back in a decade and view ChatGPT and GPT-4 like we do the first cell phone, those big clunky things that we used to carry around. But we recognize that we are on the verge, really, of a new era. The audio and my playing, it may strike you as curious or humorous, but what reverberated in my mind was what if I had asked it? And what if it had provided an endorsement of Ukraine, surrendering or Vladimir Putin’s leadership? That would’ve been really frightening. And the prospect is more than a little scary to use the word, Mr. Altman, you have used yourself, and I think you have been very constructive in calling attention to the pitfalls as well as the promise.
4 5 Sen. Richard Blumenthal (D-CT) And that’s the reason why we wanted you to be here today. And we thank you and our other witnesses for joining us for several months. Now, the public has been fascinated with GPT, dally and other AI tools. These examples like the homework done by ChatGPT or the articles and op-eds, that it can write feel like novelties. But the underlying advancement of this era are more than just research experiments. They are no longer fantasies of science fiction. They are real and present the promises of curing cancer or developing new understandings of physics and biology or modeling climate and weather. All very encouraging and hopeful. But we also know the potential harms and we’ve seen them already weaponized disinformation, housing discrimination, harassment of women and impersonation, fraud, voice cloning deep fakes. These are the potential risks despite the other rewards. And for me, perhaps the biggest nightmare is the looming new industrial revolution. The displacement of millions of workers, the loss of huge numbers of jobs, the need to prepare for this new industrial revolution in skill training and relocation that may be required. And already industry leaders are calling attention to those challenges.
5 6 Sen. Richard Blumenthal (D-CT) To quote ChatGPT, this is not necessarily the future that we want. We need to maximize the good over the bad. Congress has a choice. Now. We had the same choice when we face social media. We failed to seize that moment. The result is predators on the internet, toxic content exploiting children, creating dangers for them. And Senator Blackburn and I and others like Senator Durbin on the Judiciary Committee are trying to deal with it in the Kids Online Safety Act. But Congress failed to meet the moment on social media. Now we have the obligation to do it on AI before the threats and the risks become real. Sensible safeguards are not in opposition to innovation. Accountability is not a burden far from it. They are the foundation of how we can move ahead while protecting public trust. They are how we can lead the world in technology and science, but also in promoting our democratic values.
6 7 Sen. Richard Blumenthal (D-CT) Otherwise, in the absence of that trust, I think we may well lose both. These are sophisticated technologies, but there are basic expectations common in our law. We can start with transparency. AI companies ought to be required to test their systems, disclose known risks, and allow independent researcher access. We can establish scorecards and nutrition labels to encourage competition based on safety and trustworthiness, limitations on use. There are places where the risk of AI is so extreme that we ought to restrict or even ban their use, especially when it comes to commercial invasions of privacy for profit and decisions that affect people’s livelihoods. And of course, accountability, reliability. When AI companies and their clients cause harm, they should be held liable. We should not repeat our past mistakes, for example, Section 230, forcing companies to think ahead and be responsible for the ramifications of their business decisions can be the most powerful tool of all. Garbage in, garbage out. The principle still applies. We ought to beware of the garbage, whether it’s going into these platforms or coming out of them.
```

接下来，我考虑为未来的分析添加一些标签，通过所代表的社会群体来识别个人。

```py
 def assign_sector(name):
    if name in ['Sam Altman', 'Christina Montgomery']:
        return 'Private'
    elif name == 'Gary Marcus':
        return 'Academia'
    else:
        return 'Congress'

# Apply function 
dfsenate['sector'] = dfsenate['name'].apply(assign_sector)

# Assign organizations based on names
def assign_organization(name):
    if name == 'Sam Altman':
        return 'OpenAI'
    elif name == 'Christina Montgomery':
        return 'IBM'
    elif name == 'Gary Marcus':
        return 'Academia'
    else:
        return 'Congress'

# Apply function
dfsenate['Organization'] = dfsenate['name'].apply(assign_organization)

print(dfsenate)
```

最后，我决定添加一个列来统计每个声明的字数，这也有助于我们进一步分析。

```py
dfsenate['WordCount'] = dfsenate['comment'].apply(lambda x: len(x.split()))
```

此时，你的数据框应该如下所示：

```py
 item                            name  ... Organization WordCount
0       1  Sen. Richard Blumenthal (D-CT)  ...     Congress         5
1       2  Sen. Richard Blumenthal (D-CT)  ...     Congress        55
2       3  Sen. Richard Blumenthal (D-CT)  ...     Congress       125
3       4  Sen. Richard Blumenthal (D-CT)  ...     Congress       145
4       5  Sen. Richard Blumenthal (D-CT)  ...     Congress       197
..    ...                             ...  ...          ...       ...
399   400         Sen. Cory Booker (D-NJ)  ...     Congress       156
400   401                      Sam Altman  ...       OpenAI       180
401   402         Sen. Cory Booker (D-NJ)  ...     Congress        72
402   403  Sen. Richard Blumenthal (D-CT)  ...     Congress       154
403   404  Sen. Richard Blumenthal (D-CT)  ...     Congress        98
```

## STEP-02: 视觉化数据

让我们看看到目前为止的数据：404个问题或证词，几乎29,000字。这些数字为我们提供了启动所需的材料。重要的是要知道一些声明被分成了较小的部分。当有长声明并且包含不同的段落时，代码将它们分成了独立的声明，即使它们实际上是一个贡献的一部分。为了更好地理解每个参与者的参与程度，我还考虑了他们使用的字数。这提供了另一个角度来衡量他们的参与。

![](../Images/86bb115d760dec1f722d5d303d312ab0.png)

监督人工智能的听证会：图01

正如图01所示，国会议员的干预占所有听证会的一半以上，其次是萨姆·奥特曼的证词。然而，通过统计每一方的发言字数，另一种观点显示了国会（11名成员）与由奥特曼（OpenAI）、蒙哥马利（IBM）和马库斯（学界）组成的专家小组之间的更平衡的代表性。

值得注意的是参与参议院听证会的国会议员之间的不同参与程度（见下表）。正如预期的那样，作为分委员会主席的布卢门萨尔参议员参与度很高。但其他成员呢？表格显示所有十一位参与者的参与程度有显著差异。请记住，贡献的数量不一定表示其质量。我会让你在查看数字时自行判断。

最后，尽管萨姆·奥特曼受到了大量关注，但值得注意的是，尽管加里·马库斯可能看似参与机会较少，但他的发言量与奥特曼相当，说明他有很多要说。或者这可能是因为学术界往往提供详细解释，而商业世界则更倾向于实用和直接？

> 好的，马克斯教授，如果你能具体一点就好了。这是你的机会，伙计。用简单的英语告诉我，我们是否应该实施任何规则。请不要只是使用概念。我需要具体的内容。
> 
> 参议员约翰·肯尼迪（R-LA）。美国参议院关于AI监督的听证会（2023）

```py
#*****************************PIE CHARTS************************************
import pandas as pd
import matplotlib.pyplot as plt

# Pie chart - Grouping by 'Organization' Questions&Testimonies
org_colors = {'Congress': '#6BB6FF', 'OpenAI': 'green', 'IBM': 'lightblue', 'Academia': 'lightyellow'}
org_counts = dfsenate['Organization'].value_counts()

plt.figure(figsize=(8, 6))
patches, text, autotext = plt.pie(org_counts.values, labels=org_counts.index, 
                                  autopct=lambda p: f'{p:.1f}%\n({int(p * sum(org_counts.values) / 100)})', 
                                  startangle=90, colors=[org_colors.get(org, 'gray') for org in org_counts.index])
plt.title('Hearing on Oversight of AI: Questions or Testimonies')
plt.axis('equal')
plt.setp(text, fontsize=12)
plt.setp(autotext, fontsize=12)
plt.show()

# Pie chart - Grouping by 'Organization' (WordCount)
org_colors = {'Congress': '#6BB6FF', 'OpenAI': 'green', 'IBM': 'lightblue', 'Academia': 'lightyellow'}
org_wordcount = dfsenate.groupby('Organization')['WordCount'].sum()

plt.figure(figsize=(8, 6))
patches, text, autotext = plt.pie(org_wordcount.values, labels=org_wordcount.index, 
                                  autopct=lambda p: f'{p:.1f}%\n({int(p * sum(org_wordcount.values) / 100)})', 
                                  startangle=90, colors=[org_colors.get(org, 'gray') for org in org_wordcount.index])

plt.title('Hearing on Oversight of AI: WordCount ')
plt.axis('equal')
plt.setp(text, fontsize=12)
plt.setp(autotext, fontsize=12)
plt.show()

#************Engagement among the members of Congress**********************

# Group by name and count the rows
Summary_Name = dfsenate.groupby('name').agg(comment_count=('comment', 'size')).reset_index()

# WordCount column for each name
Summary_Name ['Total_Words'] = dfsenate.groupby('name')['WordCount'].sum().values

# Percentage distribution for comment_count
Summary_Name ['comment_count_%'] = Summary_Name['comment_count'] / Summary_Name['comment_count'].sum() * 100

# Percentage distribution for total_word_count
Summary_Name ['Word_count_%'] = Summary_Name['Total_Words'] / Summary_Name['Total_Words'].sum() * 100

Summary_Name  = Summary_Name.sort_values('Total_Words', ascending=False)

print (Summary_Name)
+-------+--------------------------------+---------------+-------------+-----------------+--------------+
| index |              name              | Interventions | Total_Words | Interv_%        | Word_count_% |
+-------+--------------------------------+---------------+-------------+-----------------+--------------+
|     2 | Sam Altman                     |            92 |        6355 |     22.77227723 |  22.32252626 |
|     1 | Gary Marcus                    |            47 |        5105 |     11.63366337 |  17.93178545 |
|    15 | Sen. Richard Blumenthal (D-CT) |            58 |        3283 |     14.35643564 |  11.53184165 |
|    10 | Sen. Josh Hawley (R-MO)        |            25 |        2283 |     6.188118812 |  8.019249008 |
|     0 | Christina Montgomery           |            36 |        2162 |     8.910891089 |  7.594225298 |
|     6 | Sen. Cory Booker (D-NJ)        |            20 |        1688 |      4.95049505 |  5.929256384 |
|     7 | Sen. Dick Durbin (D-IL)        |             8 |        1143 |      1.98019802 |  4.014893393 |
|    11 | Sen. Lindsey Graham (R-SC)     |            32 |         880 |     7.920792079 |  3.091081527 |
|     5 | Sen. Christopher Coons (D-CT)  |             6 |         869 |     1.485148515 |  3.052443008 |
|    12 | Sen. Marsha Blackburn (R-TN)   |            14 |         869 |     3.465346535 |  3.052443008 |
|     4 | Sen. Amy Klobuchar (D-MN)      |            11 |         769 |     2.722772277 |  2.701183744 |
|    13 | Sen. Mazie Hirono (D-HI)       |             7 |         755 |     1.732673267 |  2.652007447 |
|    14 | Sen. Peter Welch (D-VT)        |            11 |         704 |     2.722772277 |  2.472865222 |
|     3 | Sen. Alex Padilla (D-CA)       |             7 |         656 |     1.732673267 |  2.304260775 |
+-------+--------------------------------+---------------+-------------+-----------------+--------------+
```

## STEP-03: 标记化

现在是自然语言处理（NLP）有趣的开始阶段。为了分析文本，我们将使用 [NLTK Package](https://www.nltk.org/) 这个Python库。它提供了用于词频分析和可视化的有用工具。以下库和模块将提供进行词频分析和可视化所需的工具。

```py
 #pip install nltk
#pip install spacy
#pip install wordcloud
#pip install subprocess
#python -m spacy download en
```

首先，我们将开始**标记化**，这意味着将文本拆分成单独的词语，也称为“标记”。为此，我们将使用 [spaCy](https://spacy.io/)，一个开源的NLP库，能够处理缩写、标点和特殊字符。接下来，我们将使用来自NLTK库的[**停用词**](https://pythonspot.com/nltk-stop-words/)资源去除那些没有太多意义的常见词，如“a”，“an”，“the”，“is”和“and”。最后，我们将应用**词形还原**，将词语还原为其基本形式，称为词根。例如，“running”变成“run”，“happier”变成“happy”。这种技术帮助我们更有效地处理文本并理解其含义。

总结一下：

o 标记化文本。

o 去除常见词。

o 应用词形还原。

```py
#***************************WORD-FRECUENCY*******************************

import subprocess
import nltk
import spacy
from nltk.probability import FreqDist
from nltk.corpus import stopwords

# Download resources
subprocess.run('python -m spacy download en', shell=True)
nltk.download('punkt')

# Load spaCy model and set stopwords
nlp = spacy.load('en_core_web_sm')
stop_words = set(stopwords.words('english'))

def preprocess_text(text):
    words = nltk.word_tokenize(text)
    words = [word.lower() for word in words if word.isalpha()]
    words = [word for word in words if word not in stop_words]
    lemmas = [token.lemma_ for token in nlp(" ".join(words))]
    return lemmas

# Aggregate words and create Frecuency Distribution
all_comments = ' '.join(dfsenate['comment'])
processed_comments = preprocess_text(all_comments)
fdist = FreqDist(processed_comments)

#**********************HEARING TOP 30 COMMON WORDS*********************
import matplotlib.pyplot as plt
import numpy as np

# Most common words and their frequencies
top_words = fdist.most_common(30)
words = [word for word, freq in top_words]
frequencies = [freq for word, freq in top_words]

# Bar plot-Hearing on Oversight of AI:Top 30 Most Common Words
fig, ax = plt.subplots(figsize=(8, 10))
ax.barh(range(len(words)), frequencies, align='center', color='skyblue')

ax.invert_yaxis()
ax.set_xlabel('Frequency', fontsize=12)
ax.set_ylabel('Words', fontsize=12)
ax.set_title('Hearing on Oversight of AI:Top 30 Most Common Words', fontsize=14)
ax.set_yticks(range(len(words)))
ax.set_yticklabels(words, fontsize=10)

ax.spines['right'].set_visible(False)
ax.spines['top'].set_visible(False)
ax.spines['left'].set_linewidth(0.5)
ax.spines['bottom'].set_linewidth(0.5)
ax.tick_params(axis='x', labelsize=10)
plt.subplots_adjust(left=0.3)

for i, freq in enumerate(frequencies):
    ax.text(freq + 5, i, str(freq), va='center', fontsize=8)

plt.show()
```

![](../Images/0f009ef3df5ffcdcfbba2ace1be401df.png)

AI监督听证会：图02

正如在条形图（图02）中所见，“*思考*”的频率很高。也许前五个词语给我们提供了关于我们今天和未来在AI方面应该做些什么的有趣线索：

> “我们**需要****思考**并**了解****AI**应该**去哪里**。”

正如我在文章开头提到的，乍一看，“**监管**”在参议院AI听证会上并不是一个频繁使用的词。***然而，得出它不是主要关注话题的结论可能是不准确的***。关于AI是否应该受到监管的兴趣以不同的词汇表达，如**“监管”**、**“调控”**、**“机构”**或**“监管”**。因此，让我们对代码进行一些调整，汇总这些词，并重新运行条形图，以查看它如何影响分析。

```py
nlp = spacy.load('en_core_web_sm')
stop_words = set(stopwords.words('english'))

def preprocess_text(text):
    words = nltk.word_tokenize(text)
    words = [word.lower() for word in words if word.isalpha()]
    words = [word for word in words if word not in stop_words]
    lemmas = [token.lemma_ for token in nlp(" ".join(words))]
    return lemmas

# Aggregate words and create Frecuency Distribution
all_comments = ' '.join(dfsenate['comment'])
processed_comments = preprocess_text(all_comments)
fdist = FreqDist(processed_comments)
original_fdist = fdist.copy() # Save the original object

aggregate_words = ['regulation', 'regulate','agency', 'regulatory','legislation']
aggregate_freq = sum(fdist[word] for word in aggregate_words)
df_aggregatereg = pd.DataFrame({'Word': aggregate_words, 'Frequency': [fdist[word] for word in aggregate_words]})

# Remove individual words and add aggregation
for word in aggregate_words:
    del fdist[word]
fdist['regulation+agency'] = aggregate_freq

# Pie chart for Regulation+agency distribution
import matplotlib.pyplot as plt

labels = df_aggregatereg['Word']
values = df_aggregatereg['Frequency']

plt.figure(figsize=(8, 6))
plt.subplots_adjust(top=0.8, bottom=0.25)  

patches, text, autotext = plt.pie(values, labels=labels, 
                                  autopct=lambda p: f'{p:.1f}%\n({int(p * sum(values) / 100)})', 
                                  startangle=90, colors=['#6BB6FF', 'green', 'lightblue', 'lightyellow', 'gray'])

plt.title('Regulation+agency: Distribution', fontsize=14)
plt.axis('equal')
plt.setp(text, fontsize=8)  
plt.setp(autotext, fontsize=8)  
plt.show()
```

![](../Images/7859356dc46792a9a7a9e22f8528cb4b.png)

AI监督听证会：图03

如图03所示，监管的话题在参议院AI听证会上确实被提及了很多次。

## **STEP-04: 词语背后的含义**

单独的词汇可能给我们一些线索，但词汇的相互关系才真正提供了视角。因此，让我们采用词云的方法，探索是否可以发现简单的条形图和饼图无法显示的见解。

```py
# Word cloud-Senate Hearing on Oversight of AI
from wordcloud import WordCloud
wordcloud = WordCloud(width=800, height=400, background_color='white').generate_from_frequencies(fdist)
plt.figure(figsize=(10, 5))
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis('off')
plt.title('Word Cloud - Senate Hearing on Oversight of AI')
plt.show()
```

![](../Images/96535cbd8630dfa435a49aaeb091da40.png)

AI监督听证会：图04

让我们进一步探索，并比较AI听证会中不同利益集团（私营部门、国会、学术界）的词云，看看这些词汇是否揭示了对AI未来的不同看法。

```py
# Word clouds for each group of Interest
organizations = dfsenate['Organization'].unique()
for organization in organizations:
    comments = dfsenate[dfsenate['Organization'] == organization]['comment']
    all_comments = ' '.join(comments)
    processed_comments = preprocess_text(all_comments)
    fdist_organization = FreqDist(processed_comments)

    # Word clouds
    wordcloud = WordCloud(width=800, height=400, background_color='white').generate_from_frequencies(fdist_organization)
    plt.figure(figsize=(10, 5))
    plt.imshow(wordcloud, interpolation='bilinear')
    plt.axis('off')
    if organization == 'IBM':
        plt.title(f'Word Cloud: {organization} - Christina Montgomery')
    elif organization == 'OpenAI':
        plt.title(f'Word Cloud: {organization} - Sam Altman')
    elif organization == 'Academia':
        plt.title(f'Word Cloud: {organization} - Gary Marcus')
    else:
        plt.title(f'Word Cloud: {organization}')
    plt.show()
```

![](../Images/eddb57b199693932938ce6eb67da8aaf.png)

AI监督听证会：图05

有趣的是，不同利益集团在参议院AI听证会中讨论人工智能时，一些词汇会出现（或消失）。

关于大标题*“萨姆·奥特曼对监管AI的呼吁”*；嗯，不管他是否支持监管，我真的无法判断，但在我看来，他的话语中似乎没有太多监管的内容。相反，**萨姆·奥特曼**谈论AI时似乎更关注人本身，重复使用**“思考”、“人们”、“了解”、“重要”**和**“使用”**等词汇，更倾向于使用**“技术”、“系统”**或**“模型”**这样的词汇，而不是使用**“AI”**这个词。

说到**“风险”**和**“问题”**，**克里斯蒂娜·蒙哥马利（IBM）**在谈论**“技术”**、**“公司”**和**“AI”**时不断重复这些词。在她的证词中，一个有趣的事实是她提到的词汇最常见的有**“信任”**、**“治理”**和**“思考”**，以及在AI方面**“正确”**的看法。

> **“我们需要立即让公司对他们部署的AI负责，并承担责任……”**
> 
> 克里斯蒂娜·蒙哥马利。美国参议院AI监督听证会（2023年）

加里·马库斯在他的初步声明中提到，*“我以科学家的身份出现，曾创办AI公司，并且真正热爱AI……”* 因此，为了这次NLP分析，我们将他视为学术界声音的代表。**“需要”、“思考”、“了解”、“进行”**，**“人们”**等词汇在其中尤为突出。一个有趣的事实是，在他的证词中，**“系统”**这个词似乎比**“AI”**出现得更多。也许AI并不是一种单一的技术可以改变未来，未来的影响将来自于多种技术或系统的相互作用（物联网、机器人技术、生物技术等），而不是仅仅依赖于其中某一个。

最后，参议员约翰·肯尼迪提到的第一个假设似乎并非完全错误（不仅仅是对于国会，也对整个社会）。我们仍然处于试图理解AI发展方向的阶段。

> **“**请允许我向你们分享三个假设，我希望你们暂时接受这些假设为真。第一个假设，许多国会议员不了解人工智能。第二个假设，这种理解的缺乏可能不会阻止国会热情投入，并试图以可能对这一技术造成伤害的方式来监管它。第三个假设，我希望你们假设，人工智能社区中可能有一个失控的派别，无论是有意还是无意，都可能利用人工智能来杀死我们所有人，并在我们死去的整个过程中伤害我们……**”**
> 
> 参议员约翰·肯尼迪（R-LA）。美国参议院关于人工智能监管的听证会（2023年）

## STEP-05: 你话语背后的情感

我们将使用NLTK库中的**SentimentIntensityAnalyzer**类进行情感分析。这个预训练模型使用基于词典的方法，其中词典中的每个单词（VADER）都有一个预定义的情感极性值。将文本中单词的情感分数汇总以计算总体情感分数。数值范围从-1（负面情感）到+1（正面情感），0表示中立情感。正面情感反映了有利的情感、态度或热情，而负面情感传达了不利的情感或态度。

```py
#************SENTIMENT ANALYSIS************
from nltk.sentiment import SentimentIntensityAnalyzer
nltk.download('vader_lexicon')

sid = SentimentIntensityAnalyzer()
dfsenate['Sentiment'] = dfsenate['comment'].apply(lambda x: sid.polarity_scores(x)['compound'])

#************BOXPLOT-GROUP OF INTEREST************
import seaborn as sns
import matplotlib.pyplot as plt

sns.set_style('white')
plt.figure(figsize=(12, 7))
sns.boxplot(x='Sentiment', y='Organization', data=dfsenate, color='yellow', 
            width=0.6, showmeans=True, showfliers=True)

# Customize the axis 
def add_cosmetics(title='Sentiment Analysis Distribution by Group of Interest',
                  xlabel='Sentiment'):
    plt.title(title, fontsize=28)
    plt.xlabel(xlabel, fontsize=20)
    plt.xticks(fontsize=15)
    plt.yticks(fontsize=15)
    sns.despine()

def customize_labels(label):
    if "OpenAI" in label:
        return label + "-Sam Altman"
    elif "IBM" in label:
        return label + "-Christina Montgomery"
    elif "Academia" in label:
        return label + "-Gary Marcus"
    else:
        return label

# Apply customized labels to y-axis
yticks = plt.yticks()[1]
plt.yticks(ticks=plt.yticks()[0], labels=[customize_labels(label.get_text()) 
                                          for label in yticks])

add_cosmetics()
plt.show()
```

![](../Images/b183298e9c754313d0e513a3e27ebbc5.png)

关于人工智能监管的听证会：图 06

箱型图总是很有趣，因为它显示了最小值和最大值、中位数、第一四分位数（Q1）和第三四分位数（Q3）。此外，添加了一行代码以显示平均值。（感谢[Elena Kosourova](/violin-strip-swarm-and-raincloud-plots-in-python-as-better-sometimes-alternatives-to-a-boxplot-15019bdff8f8)设计了箱型图代码模板；我仅为我的数据集做了调整）。

总体而言，参议院听证会期间，大家似乎心情愉快，尤其是萨姆·奥特曼，他以最高的情感分数脱颖而出，其次是克里斯蒂娜·蒙哥马利。另一方面，加里·马库斯的体验似乎较为中立（中位数约为0.25），他可能有时感到不太舒服，值接近0或甚至为负。此外，国会整体上在情感分数上表现出左偏分布，显示出对中立或积极情感的倾向。有趣的是，如果我们进一步观察，某些干预措施的情感分数非常高或非常低。

![](../Images/99b955ef550b7a548e0e0986030ce144.png)

关于人工智能监管的听证会：图 07

也许我们不应将结果解读为参议院人工智能听证会上的人们感到快乐或不安。也许这表明参与听证会的人们对人工智能的未来并不持过于乐观的观点，但与此同时，他们也不悲观。评分可能表明存在一些担忧，并对人工智能的发展方向持谨慎态度。

那么时间线如何呢？*听证会期间的情绪是否一直保持不变？每个利益集团的情绪如何变化？* 为了分析时间线，我将陈述按捕获顺序进行整理，并进行了情感分析。由于有超过400个问题或证词，我定义了每个利益集团（国会、学术界、私人）情感评分的移动平均值，窗口大小为10。这意味着移动平均值是通过对每10个连续陈述的情感评分取平均值来计算的：

```py
#**************************TIMELINE US SENATE AI HEARING**************************************

import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np
from scipy.interpolate import make_interp_spline

# Moving average for each organization
window_size = 10  
organizations = dfsenate['Organization'].unique()

# Create the line plot
color_palette = sns.color_palette('Set2', len(organizations))

plt.figure(figsize=(12, 6))
for i, org in enumerate(organizations):
    df_org = dfsenate[dfsenate['Organization'] == org]

    # moving average
    df_org['Sentiment'].fillna(0, inplace=True) # missing values filled with 0
    df_org['Moving_Average'] = df_org['Sentiment'].rolling(window=window_size, min_periods=1).mean()

    x = np.linspace(df_org.index.min(), df_org.index.max(), 500)
    spl = make_interp_spline(df_org.index, df_org['Moving_Average'], k=3)
    y = spl(x)
    plt.plot(x, y, linewidth=2, label=f'{org} {window_size}-Point Moving Average', color=color_palette[i])

plt.xlabel('Statement Number', fontsize=12)
plt.ylabel('Sentiment Score', fontsize=12)
plt.title('Sentiment Score Evolution during the Hearing on Oversight of AI', fontsize=16)
plt.legend(fontsize=12)
plt.grid(color='lightgray', linestyle='--', linewidth=0.5)
plt.axhline(0, color='black', linewidth=0.5, alpha=0.5)

for org in organizations:
    df_org = dfsenate[dfsenate['Organization'] == org]
    plt.text(df_org.index[-1], df_org['Moving_Average'].iloc[-1], f'{df_org["Moving_Average"].iloc[-1]:.2f}', ha='right', va='top', fontsize=12, color='black')

plt.tight_layout()
plt.show()
```

![](../Images/c951823e89710cb97d462e7dfad04edc.png)

人工智能监管听证会：图08

起初，会议似乎友好而乐观，大家讨论人工智能的未来。但随着会议的进行，情绪开始发生变化。国会议员变得不那么乐观，问题也变得更加具有挑战性。这影响了讨论小组的评分，甚至有些评分较低（你可以在会议结束时看到这一点）。有趣的是，即使在与国会议员的紧张时刻，模型仍将Altman视为中立或略微积极。

重要的是要记住，模型有其局限性，可能带有一定主观性。虽然情感分析并不完美，但它为我们提供了对那一天国会山上情感强度的有趣窥视。

# 最后的想法

在我看来，这次美国参议院关于人工智能的听证会背后的教训在于五个最常出现的词汇：*“我们* ***需要*** *去* ***思考*** *和* ***知道*** *人工智能* ***应该*** *去* ***哪里****”*。值得注意的是，像**“人们”**和**“重要性”**这样的词在Sam Altman的词云中意外出现，超出了*“呼吁监管”*的标题范围。虽然我希望在Altman的NLP分析中看到更多**“透明度”**、**“问责制”**、**“信任”**、**“治理”**和**“公平”**等词，但发现这些词在Christina Montgomery的证词中经常出现，还是让人感到宽慰。这正是我们在讨论人工智能时期待听到的。

加里·马库斯强调了**“system”**与**“AI”**一样多，或许是在邀请我们从更广的视角来看待人工智能。目前有多种技术正在出现，它们对社会、工作和未来就业的综合影响将来自这些技术之间的冲突，而不仅仅是某一种技术。学术界在引导这一过程方面发挥着至关重要的作用，如果需要某种形式的监管的话。我说的是*“字面上的”* ***而不是*** *“精神上的”*（来自六个月停顿信的内部笑话）。

最后，**“Agency”** 这个词在不同形式中被重复使用的频率与**“Regulation”**相当。这表明**“Agency for AI”**的概念及其作用可能会在不久的将来成为讨论的话题。理查德·布卢门撒尔参议员在参议院人工智能听证会上提到对此挑战的有趣反思：

> **“…我职业生涯的大部分时间都在执法。我告诉你们，你们可以创建10个新机构，但如果不给他们资源，我说的不是仅仅是资金，还包括科学专业知识，你们将把他们绕圈子。而且不仅仅是模型或生成性人工智能会把他们绕圈子，而是你们公司里的科学家。对于政府监管中的每一个成功故事，你可以想到五个失败案例……我希望我们这里的经验会有所不同……”**
> 
> 理查德·布卢门撒尔（D-CT）参议员。美国参议院关于人工智能监督的听证会（2023年）

尽管对我来说，调和创新、意识和监管是具有挑战性的，我完全支持提升对人工智能在我们现在和未来角色的意识，但也要理解**“research”**和**“development”**是不同的。前者应该得到鼓励和推广，而不是限制，后者则是需要额外努力在**“thinking”**和**“knowing”**上的地方。

我希望你觉得这篇自然语言处理分析有趣，并且我想感谢 [贾斯廷·亨德里克斯](https://techpolicy.press/author/tppadmin/) 和 [Tech Policy Press](https://techpolicy.press/) 允许我在本文中使用他们的稿件。你可以在[这个GitHub库](https://github.com/rvizcarra15/NLTK_AIHEARING.git)中访问完整代码。*（同时感谢ChatGPT帮助我优化了一些代码，使其展示更佳）*。

*我有遗漏什么吗？* 欢迎提出建议，让对话持续进行。
