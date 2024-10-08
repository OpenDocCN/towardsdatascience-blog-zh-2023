# 如何使用 Python、NLTK 和一些简单的统计进行语言检测

> 原文：[`towardsdatascience.com/how-to-do-language-detection-using-python-nltk-and-some-easy-statistics-6cec9a02148`](https://towardsdatascience.com/how-to-do-language-detection-using-python-nltk-and-some-easy-statistics-6cec9a02148)

## 一个关于你每天使用的技术的实用介绍。

[](https://katherineamunro.medium.com/?source=post_page-----6cec9a02148--------------------------------)![Katherine Munro](https://katherineamunro.medium.com/?source=post_page-----6cec9a02148--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6cec9a02148--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6cec9a02148--------------------------------) [Katherine Munro](https://katherineamunro.medium.com/?source=post_page-----6cec9a02148--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----6cec9a02148--------------------------------) ·12 分钟阅读·2023 年 1 月 11 日

--

![](img/645352f0855da593110591ce2a9202a4.png)

图片由[Etienne Girardet](https://unsplash.com/@etiennegirardet)提供，来源于[Unsplash](https://unsplash.com)

有没有想过 Google Translate 的“检测语言”功能是如何工作的？当然你没想过，你有更重要的事情要做。但我去寻找答案时却没有找到（尽管[我确实写了一本关于自然语言处理（NLP）的书](https://www.amazon.com/Handbook-Data-Science-AI-Analytics/dp/1569908869)）。这是 Google 的秘密调料。所以今天，我将向你展示一种超级简单的方法，使用一个被低估的 NLP 工具和一些非常简单的数学，你自己进行语言检测。你很快就能将它添加到你的 GitHub 作品集中。

## **什么是语言检测，为什么要使用它？**

语言检测仅仅意味着识别一段输入文本的语言。这是自然语言处理中的许多任务的第一步，包括许多你每天使用的任务：

+   拼写和语法修正（例如 MS Word、Google Docs 等）

+   下一个词预测（你的手机一直在做这个！）

+   机器翻译（例如 Google Translate 的“检测语言”选项）

![](img/e7bd41ad34bcc968094da7111a953d1a.png)

来源：作者

## **我们如何检测语言？**

进行语言识别的一种简单方法是：为不同的语言建立词汇表（词汇列表），然后计算每种语言的词汇在文本中出现的次数。因此，如果测试文本包含五个日语单词和两个英语单词，我们可能会得出它是日语的结论。我们甚至可以专注于所谓的“停用词”，这些词在语言中出现频率很高，虽然往往没有什么实际意义，但对语法非常重要，例如“the”，“a”和“and”。

问题是，许多词汇在多种语言中都有出现，即使它们有不同的含义。例如，‘gift’在英语中表示‘礼物’，但在德语中表示‘毒药’。所以短语‘Das Gift’可能会带来问题，特别是如果有拼写错误的话。假设我们想说‘毒药很强’：Das Gift ist stark。如果我们忘记了‘ist’中的‘t’，我们就会得到‘Das Gift is stark’。由于‘stark’在两种语言中都出现，我们现在确实遇到问题了。而且，集中关注停用词可能会使问题更严重。例如，法语和德语都经常使用‘des’和‘du’，所以如果我们只关注这些，我们就会陷入困境。

*有趣的事实：在自然语言处理领域，*总是*会有拼写错误。*

另一种方法是集中于字母的分布，而不是单词。例如，与英语相比，德语使用变音符号（ä、ö、ü），法语使用很多特殊字符（ç、â/ê/î/ô/û、à/è/ì/ò/ù、ë/ï/ü）。2 个和 3 个字母的组合，称为 bi-和 trigrams，效果更好。这是因为不同的语言有一些在其他语言中不存在的字母组合。

## **在 Python 中构建语言检测模型**

我们的语言检测方法将使用 uni-、bi-和 tri-grams：即，单个字母以及两个和三个字母的组合。这些组合的通用术语是‘n-grams’。我们将通过计算它们的 n-gram 频率来创建不同语言的统计模型。然后我们将这些与测试文本中的 n-gram 频率进行比较。n-gram 频率与测试句子最匹配的语言将是我们的赢家。

这种方法基于[1]。

## **可视化 N-Grams**

让我们开始可视化一些不同长度的 n-gram：

```py
text = "This is a test text"
n = 3 # 1 = Unigram, 2 = bigram, 3 = trigram
text_len = len(text)
num_ngrams = text_len - n + 1 # How many ngrams of length n will fit in this text
print(f"The text is {text_len} characters long and will fit {num_ngrams} n-grams of length {n}.")

for p in range(num_ngrams) :
            print(f"{p}: {text[p:p+n]}")
```

## **构建一个 N-Gram 提取器**

让我们定义一个函数`extract_xgrams()`。它将接收一个文本和一个数字列表`n_vals`，并从文本中提取这些长度的 n-grams：

```py
import typing

def extract_xgrams(text: str, n_vals: typing.List[int]) -> typing.List[str]:
    """
    Extract a list of n-grams of different sizes from a text.
    Params:
        text: the test from which to extract ngrams
        n_vals: the sizes of n-grams to extract
        (e.g. [1, 2, 3] will produce uni-, bi- and tri-grams)
    """
    xgrams = []

    for n in n_vals:
        # if n > len(text) then no ngrams will fit, and we would return an empty list
        if n < len(text):
            for i in range(len(text) - n + 1) :
                ng = text[i:i+n]
                xgrams.append(ng)

    return xgrams

text = "I was taught that the way of progress was neither swift nor easy.".lower()
# Quote from Marie Curie, the first woman to win a Nobel Prize, the only woman to win it twice, and the only human to win it in two different sciences.

# Extract all ngrams of size 1 to 3.
xgrams = extract_xgrams(text, n_vals=range(1,4))

print(xgrams)
```

注意，我们将测试文本转为小写。这将减少返回的 n-gram 数量，同时不会丢失关于语言本身的信息（想想看：如果我说‘i went to new york’，即使没有大写，你仍然能理解我）。

## **定义一个构建语言模型的函数**

我们的`build_model()`函数使用了 collections.Counter。Counter 接收一个列表，计算列表中每个项目的出现次数，并返回一个包含每个项目及其频率的字典。

因此，对于任何语言，我们可以通过创建一个 n-gram 字典及其在该语言中出现的概率来对其建模。n-gram 的概率是多少？它仅仅是其频率，除以提取的 n-gram 总数。让我们运行代码并打印语言模型，排序使得最频繁的 n-gram 排在前面：

```py
def build_model(text: str, n_vals: typing.List[int]) -> typing.Dict[str, int]:
    """
    Build a simple model of probabilities of xgrams of various lengths in a text
    Parms:
        text: the text from which to extract the n_grams
        n_vals: a list of n_gram sizes to extract
    Returns:
        A dictionary of ngrams and their probabilities given the input text
    """
    model = collections.Counter(extract_xgrams(text, n_vals))  
    num_ngrams = sum(model.values())

    for ng in model:
        model[ng] = model[ng] / num_ngrams

    return model

test_model = build_model(text, n_vals=range(1,4))
print({k: v for k, v in sorted(test_model.items(), key=lambda item: item[1], reverse=True)})
```

## **安装 NLTK 并下载我们的文本数据**

自然语言工具包（Natural Language ToolKit）是自然语言处理的一个隐藏宝石。它包含用于文本处理的类和方法，以及大量的文本语料库（准备好的文本数据集合）供你练习。如果你还没有安装 NLTK，可以使用你喜欢的方法安装，例如 `pip install nltk` ([查看指南](https://www.nltk.org/install.html))

为了测试我们的语言识别器，我们将使用《世界人权宣言》（UDHR），它在 [NLTK 中包含](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&cd=&cad=rja&uact=8&ved=2ahUKEwi5p7avpMf8AhWI3KQKHbn7DmcQFnoECAgQAw&url=https%3A%2F%2Fwww.nltk.org%2Fbook%2Fch02.html&usg=AOvVaw1czTHzjPxNOk1JLrT_zeOS) 300 种语言。实际上，这样的数据集太小，文本也太干净（联合国大概进行了校对，而且他们可能不使用标签和表情符号，那些扫兴的东西）。但这个数据集足以演示我们在这里尝试做的概念。此外，它将使你了解如何使用 NLTK：

```py
import nltk
nltk.download('udhr') # udhr = Universal Declaration of Human Rights

# Now import corpus and print number of files and the fileids (as these reveal the languages)
from nltk.corpus import udhr 
print(f"There are {len(udhr.fileids())} files with the following ids: {udhr.fileids()}")
```

为了简化，我将选择几个语言进行处理。它们都使用类似的字符，因此对我们的检测器来说会是一个更艰难的测试。可以随意添加更多语言：代码注释将告诉你如何操作：

```py
languages = ['english', 'german', 'dutch', 'french', 'italian', 'spanish']
language_ids = ['English-Latin1', 'German_Deutsch-Latin1', 'Dutch_Nederlands-Latin1', 'French_Francais-Latin1', 'Italian_Italiano-Latin1', 'Spanish_Espanol-Latin1']# I chose the above sample of languages as they all use similar characters. 

### Optional: If you want to add more languages:

# First use this function to find the language file id
def retrieve_fileid_by_first_letter(fileids, letter):
    return [id for id in fileids if id.lower().startswith(letter.lower())]

# Example usage
print(f"Fileids beginning with 'R': {retrieve_fileid_by_first_letter(udhr.fileids(), letter='R')}")

# Then copy-paste the language name and language id into the relevant list:
languages += []
language_ids += []
```

命令 `udhr.raw(fileids)` 返回指定 `fileid` 的完整文本。我们将使用它来建立一个包含每种语言名称及其文本的字典，然后从这个字典中构建每种语言的模型：

```py
raw_texts = {language: udhr.raw(language_id) for language, language_id in zip(languages, language_ids)}
print(raw_texts['english'][:1000]) # Just print the first 1000 characters

# Build a model of each language
models = {language: build_model(text=raw_texts[language], n_vals=range(1,4)) for language in languages}
print(models['german'])
```

## **确定给定文本的语言**

我们现在可以使用测试文本，将其 n-gram 频率与各种语言模型的频率进行比较。目的是查看哪种语言的频率与我们的测试文本最接近。

我们通过计算 [余弦相似度](https://en.wikipedia.org/wiki/Cosine_similarity)，如下面的公式所示：

![](img/be95b3e7d99c3eac83df4a11cfcd6645.png)

余弦相似度公式

它看起来很吓人，但我们不会深入数学。基本上，余弦相似度用于比较两个数值向量。结果范围从−1，表示完全相反，到 1，表示完全相同。我们的 `calculate_cosine()` 公式实现了这些数学计算：

```py
import math

def calculate_cosine(a: typing.Dict[str, float], b: typing.Dict[str, float]) -> float:
    """
    Calculate the cosine between two numeric vectors
    Params:
        a, b: two dictionaries containing items and their corresponding numeric values
        (e.g. ngrams and their corresponding probabilities)
    """
    numerator = sum([a[k]*b[k] for k in a if k in b])
    denominator = (math.sqrt(sum([a[k]**2 for k in a])) * math.sqrt(sum([b[k]**2 for k in b])))
    return numerator / denominator
```

现在是时候构建一个 `identify_language()` 函数了。这个函数将接受一个测试文本，使用不同大小的 n-gram（由 `n_vals` 指定）为其构建一个模型，并将其与语言模型字典进行比较。输出将是与测试文本最相似的语言名称。

出于演示目的，我在函数中添加了一条打印语句以显示每种语言与测试文本的相似度。你可以在你对余弦值有了感觉后删除它。

对原始测试文本运行此函数时，最高的相似度正确地出现在英语中：

```py
def identify_language(
    text: str,
    language_models: typing.Dict[str, typing.Dict[str, float]],
    n_vals: typing.List[int]
    ) -> str:
    """
    Given a text and a dictionary of language models, return the language model 
    whose ngram probabilities best match those of the test text
    Params:
        text: the text whose language we want to identify
        language_models: a Dict of Dicts, where each key is a language name and 
        each value is a dictionary of ngram: probability pairs
        n_vals: a list of n_gram sizes to extract to build a model of the test 
        text; ideally reflect the n_gram sizes used in 'language_models'
    """
    text_model = build_model(text, n_vals)
    language = ""
    max_c = 0
    for m in language_models:
        c = calculate_cosine(language_models[m], text_model)
        # The following line is just for demonstration, and can be deleted
        print(f'Language: {m}; similarity with test text: {c}')
        if c > max_c:
            max_c = c
            language = m
    return language

print(f"Test text: {text}")
print(f"Identified language: {identify_language(text, models, n_vals=range(1,4))}")

# Prints
# Test text: i was taught that the way of progress was neither swift nor easy.
# Language: english; similarity with test text: 0.7812347488239613
# Language: german; similarity with test text: 0.6638235631734796
# Language: dutch; similarity with test text: 0.6495872103674768
# Language: french; similarity with test text: 0.7073331083503462
# Language: italian; similarity with test text: 0.6635204671187273
# Language: spanish; similarity with test text: 0.6811923819801172
# Identified language: english
```

在你删除那一行打印语句之前，看看当我们用一个截然不同的文本测试这个函数时会发生什么；相似度值通常会降低：

```py
# An example text in Slovenian
tricky_text = "učili so me, da pot napredka ni ne hitra ne lahka."
print(f"Identified language: {identify_language(tricky_text, models, n_vals=range(1,4))}")

# Prints
# Language: english; similarity with test text: 0.7287873650203188
# Language: german; similarity with test text: 0.6721847143945305
# Language: dutch; similarity with test text: 0.6794130641102911
# Language: french; similarity with test text: 0.7395592659566902
# Language: italian; similarity with test text: 0.7673665450525412
# Language: spanish; similarity with test text: 0.7588017776235897
# Identified language: italian
```

## **测试我们的语言检测器在不同语言上的效果**

让我们看看荷兰语、法语和西班牙语文本的效果：

```py
t = "mij werd geleerd dat de weg van vooruitgang noch snel noch gemakkelijk is."  
print(identify_language(t, models, n_vals=range(1,4)))

t = "on m'a appris que la voie du progrès n'était ni rapide ni facile."  
print(identify_language(t, models, n_vals=range(1,4)))

t = "me enseñaron que el camino hacia el progreso no es ni rápido ni fácil."
print(identify_language(t, models, n_vals=range(1,4)))
```

结果是正确的，除了第二个例子输出的是意大利语而不是法语。天哪！

## **改进模型**

显然，我们的模型并不完美，但有很多方法可以改进它们：

**使用更大、更具代表性的数据：** 我之前承认我们的训练文本太短且过于干净，无法真实反映实际语言识别情况。事实上，它们只是《宣言》的*样本*，每个文本被截断到大约 1000 个字符。你可以通过探索每种语言中文本的单词和字符数量来查看这一点：

```py
 from nltk.tokenize import word_tokenize  # A function from nltk for splitting strings into individual words
nltk.download('punkt') # Required for word_tokenize to function

print("Number of characters and words per text per language:")
for language in raw_texts.keys():
    print(f"\n{language}: {len(raw_texts[language])} characters, {len(nltk.word_tokenize(raw_texts[language]))} words")
```

训练文本足够让你了解 NLTK 和这种简单的语言检测方法，但为了提高我们的泛化能力，我们需要使用更长、更具多样性的真实世界文本数据：错别字、标签、表情符号等等。

![](img/bacf8c31a69c0a5ea9d2aa476900758c.png)

我向 ChatGPT 询问了“Twitter 语言风格”的《世界人权宣言》。即便这是相当干净和清晰的，它也可能变得更糟。来源：作者提供的“ChatGPT”对话截图，来自 OpenAI。

为什么我们需要更长、更具多样性的文本？很简单：这是捕捉每种语言及其与其他语言不同之处的唯一方法。

以“gnome”这个词为例。除非将接触园艺侏儒视为普世人权，否则三字母组‘ gn’（空格，g，n）可能不在我们的英语样本数据中。你可能会觉得没关系，因为没有很多英语单词以‘gn’开头。（虽然有一些，但很少用）。问题是，如果这是其他语言中的*常见模式*呢？实际上，确实有[许多这样的德语常见词](https://www.wordmine.info/Search?slang=de&stype=words-that-starts-with&sword=GN)，但其中没有一个出现在《世界人权宣言》中（我查过了）。因此，如果我们看到含有三字母组‘ gn’的测试文本，它不会对我们总结的 x-gram 概率产生贡献，无论是哪种语言。这意味着它无法帮助我们区分它们。

**添加更多特征：** 我们本可以添加额外长度的字符 x-grams，例如四字母组（quadgrams）。基于单词的 x-grams 也可能有帮助。这两种情况的好处与使用更长文本相同：更多特征有助于捕捉语言之间的区别。例如，尽管我讨厌果酱，但我不太可能说“die Marmelade!”。但一些德国人每天早餐时都会这么说（‘die’只是‘the’的一种变体）。因此，使用单词 x-grams 可能捕捉到这种差异。

不过，单词 x-grams 有一些问题。大多数语言的单词数量远远超过其字母表中的字符数量，因此仅仅添加单词二元组将会爆炸性地增加我们语言模型中的项数。三元组及更大的项只会使问题变得更严重。更大的模型会使整个过程变慢，而且收效甚微：这些 x-gram 单词组合中的大多数几乎不会出现，因此它们甚至不会在测试时对区分语言贡献多少。

一个更好的方法是使用停用词，因为每种语言都有自己频繁出现的停用词，这些停用词是很好的指示词。我之前提到过，单独使用停用词来建模语言是有风险的，因为它们可能出现在多种语言中。但将它们作为我们字符 x-grams 的附加特征，或作为单词 x-grams 的一部分来使用，可以解决这个问题。

同样，我们可以在每种语言中添加前 1000 或 10,000 个单词（或在单词 x-grams 中使用它们）。其背后的理论是，单词往往遵循‘[Zipf 定律](https://en.wikipedia.org/wiki/Zipf%27s_law)’，即最常见的单词出现的频率大约是第二常见单词的两倍，第三常见单词的三倍，以此类推。因此，通过仅取前 n 个单词，你可以捕捉到输入数据和 —— 关键是 —— 测试数据中大多数单词的概率。这些概率将是我们语言检测决策的依据。

**使用机器学习：** 你不能谈论‘特征’而不考虑机器学习。我们可以尝试许多算法，包括一些令人惊讶的简单但有效的选项，如朴素贝叶斯。

理解这些算法需要整篇新博客，但好奇的读者可以在[这里](https://dbs.cs.uni-duesseldorf.de/lehre/bmarbeit/barbeiten/ba_panich.pdf)阅读关于语言检测的朴素贝叶斯分类器。

**添加更多语言：** 我只用了几种语言，但世界上有成千上万种语言，它们都值得关注。所以这是给你的挑战：添加更多语言，看看你能做到什么（我甚至已经给了你代码）。

## **将这一概念应用于其他 NLP 任务**

本文中涉及的概念可以很容易地应用于其他挑战，而 NLTK 内置的语料库可以帮助你实现。因此，请关注我的后续帖子，我们将使用 —— 你猜对了 —— Python、NLTK 和那些非常简单的统计学来讨论文档分类和说话人识别。

## **感谢阅读！**

首先，感谢[教程](https://textmining.wp.hs-hannover.de/Sprachbestimmung.html)的创作者，它们启发了这篇文章。

如果这篇文章对你有所帮助 —— 太棒了！—— 请订阅获取更多关于自然语言处理和其他数据科学基础的内容。你还可以在[Twitter](https://twitter.com/KatherineAMunro)上与我联系（我会发布大量有趣的内容，涉及 AI、技术、伦理等）。

[1] Cavnar, W.B., Trenkle, J.M.：[基于 N-gram 的文本分类](https://dsacl3-2019.github.io/materials/CavnarTrenkle.pdf) (1994)，SDAIR-94 文档分析与信息检索第三届年会论文集。
