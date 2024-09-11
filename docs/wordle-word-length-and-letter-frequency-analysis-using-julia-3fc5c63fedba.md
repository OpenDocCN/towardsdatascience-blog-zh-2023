# 使用 Julia 进行 Wordle 单词长度和字母频率分析

> 原文：[https://towardsdatascience.com/wordle-word-length-and-letter-frequency-analysis-using-julia-3fc5c63fedba?source=collection_archive---------12-----------------------#2023-02-10](https://towardsdatascience.com/wordle-word-length-and-letter-frequency-analysis-using-julia-3fc5c63fedba?source=collection_archive---------12-----------------------#2023-02-10)

## 探索英语单词数据集以提升我们的游戏

[](https://medium.com/@naresh-ram?source=post_page-----3fc5c63fedba--------------------------------)[![Naresh Ram](../Images/4a20b5646f75fb6cc2a5684fd3daf6db.png)](https://medium.com/@naresh-ram?source=post_page-----3fc5c63fedba--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3fc5c63fedba--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3fc5c63fedba--------------------------------) [Naresh Ram](https://medium.com/@naresh-ram?source=post_page-----3fc5c63fedba--------------------------------)

·

[Follow](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa5665b0dac5f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwordle-word-length-and-letter-frequency-analysis-using-julia-3fc5c63fedba&user=Naresh+Ram&userId=a5665b0dac5f&source=post_page-a5665b0dac5f----3fc5c63fedba---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3fc5c63fedba--------------------------------) ·19分钟阅读·2023年2月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3fc5c63fedba&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwordle-word-length-and-letter-frequency-analysis-using-julia-3fc5c63fedba&user=Naresh+Ram&userId=a5665b0dac5f&source=-----3fc5c63fedba---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3fc5c63fedba&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwordle-word-length-and-letter-frequency-analysis-using-julia-3fc5c63fedba&source=-----3fc5c63fedba---------------------bookmark_footer-----------)![](../Images/626ad3edde1f12cd388d3eeb5dc4f5e5.png)

受 [Wordle](https://www.nytimes.com/games/wordle/index.html) 启发。图像由作者提供。

[Wordle](https://www.nytimes.com/games/wordle/index.html)™ 是一款无需介绍的绝佳游戏。但保持连胜并非易事。为了更好地理解游戏的运作方式，我们将加载英语单词到 Julia 中，并对它们进行一点分析。我们将关注字母频率以及特别是5字母单词，以帮助在游戏中做出更有根据的猜测。同时，我们还将探讨为何选择五个字母作为游戏的单词长度。

虽然我不能保证它会减少你的猜测次数，但我可以保证使用 [Julia](https://julialang.org/) 和 [Julia Plots](https://docs.juliaplots.org/) 进行数据分析会很有趣。

在我工作的 [AAXIS](https://www.aaxisdigital.com/) 公司，我们实施了面向企业的数字商务解决方案以及面向消费者的数字商务解决方案。这包括将大量数据从现有的旧系统迁移到具有不同数据结构的新系统。我们使用各种数据工具来分析源数据，以确保一致性。我通常会使用本文中概述的技巧。熟悉这些技巧的好方法是经常使用它们，而 Wordle 单词列表听起来是一个有趣的练习方式。

让我们开始吧。

# 设置

![](../Images/d988ce41b881f02bb5d44abf69e1cb32.png)

图片由 [Linus Mimietz](https://unsplash.com/@linusmimietz?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/gvptKmonylk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果你计划跟我一起学习 Julia，我强烈建议你阅读这一部分。如果不计划，你可以安全地跳过。

## 安装 Julia

请从 [Julia Language](https://julialang.org/downloads/) 网站安装 Julia。该网站提供适用于 Windows、Mac 和 Linux 操作系统的安装程序。我还为 [VSCode](https://code.visualstudio.com/download) 安装了 *Julia Language Support* 扩展，并在 VSCode 中执行这些命令的 [Julia REPL](https://docs.julialang.org/en/v1/stdlib/REPL/)。这样，我的图形会显示在窗口中，使数据集分析变得极为方便。

![](../Images/db73c709c922b58b1a4b2a0c04b99df5.png)

Visual Studio Code 中的 Julia REPL。图片由作者提供。

现在，让我们加载图形和其他所需的库。安装完成后，你可以通过输入 CNTRL-SHIFT-P 并选择 *Julia: Start REPL* 来打开 Julia REPL 窗口。

在 Julia REPL 中输入这些命令来检查一切是否正常：

```py
julia> println("Hello world")
Hello World

julia> f(x) = x^2 + 3x + 2
f (generic function with 1 method)

julia> f(2)
12
```

在 VSCode 中，当代码被选中时，按 SHIFT-ENTER 可以执行它，并在下方的 Julia REPL 中输出结果。你可以直接从本文中剪切代码，粘贴到 VSCode 中，选择代码，然后按 SHIFT-ENTER 执行它。为了方便这种方法，我将跳过代码中的 `julia>` 提示。请注意，你可以在 REPL 中输入所有这些命令，即使是包含 for-loops 或函数的长命令。

接下来，让我们加载 Plots。你的用户界面可能会有所不同，因为 Julia 可能会下载和安装一些包。我们还需要 DelimitedFiles 和 Statistics 包：

```py
import Pkg
Pkg.add("Plots")
using Plots
using DelimitedFiles
using Statistics

plot(f,-10,10)
```

最后一行应该会生成一个如上图所示的图形。

## 词汇数据集

在线有多个单词数据集，洁净程度不一。不幸的是，我没有权限在这篇文章中使用大多数数据集。我选择了SCOWL（Spell Checker Oriented Word Lists）及Friends，这是一个关于英语单词的数据库，适用于创建高质量的单词列表，适合大多数英语方言的拼写检查器，并可供我们[使用](http://wordlist.aspell.net/scowl-readme/)。如果你决定探索，其他可供公众用于教育目的的单词列表如下：

+   [Infochimp的简单英语单词列表](https://github.com/dwyl/english-words)是Infochimp提供的英语单词列表。GitHub网站提供了该列表，但声明版权归Infochimp所有。Infochimp的链接指向一个不存在的页面。

+   [Collins Official Scrabble™ Words](https://www.collinsdictionary.com/games/scrabble/tools)列表得到[WESPA](https://www.wespa.org/)的认可，用于全球锦标赛和俱乐部比赛，不包括美国和加拿大。该软件[仅限](https://github.com/scrabblewords?tab=repositories)私人、非商业用途。这是一个精心策划的英语单词列表。

+   [The Wordle Guess List](https://gist.github.com/dracos/dd0668f281e685bad51479e5acaadb93)是游戏中允许的13K单词的完整列表，由Josh Wardle版权所有，现在可能由纽约时报版权所有。该列表与上述Scrabble列表中的5字母单词子集非常匹配。答案列表，即Wordle的2.5K可能答案，可以在[这里](https://www.wordunscrambler.net/word-list/wordle-word-list)找到。

现在，让我们继续获取SCOWL单词列表，并为分析做好准备。前往[http://app.aspell.net/create](http://app.aspell.net/create)下载单词列表。我选择的选项如图所示。我使用了80字母列表进行下一部分，并将文件命名为scowl_american_words_80.txt。

![](../Images/163d525c2cfb7657b8aec3078969dc06.png)

生成单词列表所选择的选项的图像。截图由作者提供。

我还下载了35字母选项，并将其命名为scowl_american_words_35.txt。我将用于稍后在文章中分析更常见的单词。

对于单词使用，我使用了在[Miller Center](https://millercenter.org/the-presidency/presidential-speeches)¹提供的总统演讲。这些演讲属于公共领域，因此没有使用限制。只需选择总统的名字，点击演讲，然后点击抄本。我将文本复制并粘贴到文本编辑器（VSCode）中进行分析。

# **为什么选择5字母单词？**

![](../Images/95acbaadb23fee399a80f37672431646.png)

图片由[Glen Carrie](https://unsplash.com/@glencarrie?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/photos/oHoBIbDj7lo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我的好朋友和邻居Milan提出了这个问题。他认为5字母词汇可能是英语中最常见的词汇。通过加载我们在设置中讨论的SCOWL词汇表数据集并计算词汇长度的频率，这确实值得检查。

```py
rawwords = readdlm("data/scowl_american_words_80.txt", header=false,String)
343681×1 Matrix{String}:
 "A"
 "A'asia"
 "A's"
 "AA"
 "AA's"
 "AAA"
 "AAM"
 ⋮
 "zymurgies"
 "zymurgy"
 "zymurgy's"
 "zythum"
 "zyzzyva"
 "zyzzyvas"
 "zzz"

totalWords = length(rawwords)
343681
```

aspell词汇表包含了像`zymurgy's`这样的所有格词和像`Albert.`这样的专有名词。让我们把它们去掉。

```py
dictwords = String[];
for word in rawwords
      # remove words with ', -' space and capital letter start
      if (length(word)>0 && !(occursin("\'",word) 
            || occursin("-",word) 
            || occursin(" ",word)
            || (word[1]<'a' || word[1]>'z')
            ))
          push!(dictwords,word)
      end
end

# get count
totalWords = length(dictwords)
244274
```

接下来，我们应该为词汇长度构建一个直方图。我知道有三种方法可以做到这一点：列表推导、map函数和for循环。考虑到性能，在Julia中它们都应该差不多。让我们来看看：

```py
# use list comprehension
wordlengths = zeros(Int64,0)
@time wordlengths = [length(x) for x in dictwords];
0.021251 seconds (45.30 k allocations: 4.246 MiB, 84.02% compilation time)

# use map
wordlengths = zeros(Int64,0);
@time wordlengths = map(word->length(word), dictwords);
0.024202 seconds (49.09 k allocations: 4.473 MiB, 85.97% compilation time)

# use for loop
wordlengths = zeros(Int64,0);
@time for word in dictwords
  push!(wordlengths, length(word))
end
0.027516 seconds (488.05 k allocations: 14.131 MiB) 
```

一般来说，虽然列表推导在性能上略好，但它们都非常接近，我会选择for循环，因为它使逻辑清晰易懂。现在，*词汇长度*数组应该包含每个词的长度。最小值和最大值是：

```py
lrange = minimum(wordlengths),maximum(wordlengths)
(1, 45)
```

最小值为1，最大值为45，但为了直方图的目的，我决定将所有大于20的词汇放入“20”箱中（`len>20 ? 20 : len`）。现在，让我们来看看每个词汇长度的分布。Julia Plots使这一过程变得简单。

```py
julia>  histogram(wordlengths,bins=20)
```

![](../Images/6a27f2e0991e8315cf4f2a3d5792c67e.png)

英语词汇数据集中的词汇长度。图像作者提供

我实际上用了一些选项运行了上述命令，以使图表看起来更具信息性。

```py
histogram(wordlengths,
    bins=20,
    xaxis=("WORD LENGTH"),
    yaxis=("COUNT"),
    xticks=([1:1:20;]),
    yticks=([0:5e3:4.5e4;],["$(x)k" for x=0:5:45]),
    label=("Word count"),
    xguidefontsize=8, yguidefontsize=8,
    margin=5mm, ylims = (0,4e4),
    framestyle = :box,
    fill = (0,0.5,:green),
    size=(800,420))

savefig("pics/2-english-word-lengths-v2.png")
```

令人惊讶的是，5字母词汇甚至不在前五个最常见的词汇长度中！它们仅占我们词汇数据集的4.6%。Ravi Parikh²和Reginald Smith³也报告了类似的发现（5.2%）。差异可能是因为我选择了一个不包含俚语、连字符词等的词汇数据集。

首先，图表看起来像经典的钟形曲线分布。这种钟形曲线是自然界和心理学中常见的特征⁴，例如婴儿出生体重、男性身高、血压测量等。我不是语言学家，但在我看来，这表明英语语言似乎像是自然基础和随机演化的。

```py
julia> mean(wordlengths)
9.250849455938823

julia> median(wordlengths)
9.0
```

最常见的词汇长度是8和9，但我似乎很难想到任何一个。因此，让我们列出几个以确保：

```py
julia> filter(x->(length(x)==9), dictwords)[1:5]
5-element Vector{String}:
 "aardvarks"
 "aasvogels"
 "abactinal"
 "abamperes"
 "abandoned"

julia> filter(x->(length(x)==9), dictwords)[27001:27005]
5-element Vector{Any}:
 "sassabies"
 "sassafras"
 "sassarara"
 "sassiness"
 "sassolite"
```

AASVOGELS？显然，这是南非的一种秃鹫。SASSAFRAS？难怪我在拼字游戏中不太行。

那么，有多少5字母词汇呢？

```py
julia> length(filter(x->(length(x)==5), dictwords))
11210
```

相较于36K个9字母词汇，5字母词汇只有11K个。嗯……

## **那么为什么Wordle使用5字母词汇？**

也许我们使用它们更多。

我的编辑Megan，那个帮助我写这些文章的好女士，认为普通人最适应5字母词汇。这意味着5字母词汇应该占据我们交流的大部分，并且出现频率较高。为了验证这个理论，我查看了总统演讲。

总统演讲通常是为不同背景和观点的广泛受众专业撰写的，因此写作必须具有包容性、文化敏感性和可及性。演讲稿撰写者还必须对当前事件和政治问题有深刻的理解，以有效传达总统的立场和目标。总统演讲对公众舆论和政治话语的形成有着重要影响，因此它们成为了那个时代语言的极佳代表文本。

这些演讲属于公有领域，且易于[访问](https://millercenter.org/the-presidency/presidential-speeches)进行分析¹。

对于这项练习，我抓取了美国过去四位总统的演讲。我分析了单词长度并进行了图示。

为了简化图表绘制，我编写了一个函数来返回直方图数据。长度超过18个字母的单词，会被归入18字母以下的桶中。

```py
function wordLengthHistoFromFile(filename, doUnique=true)
    set_tokenizer(poormans_tokenize)

    # split the words in the file into tokens
    words = collect(tokenize(read(filename, String)))

    if (doUnique) 
        words = unique(words)
    end
    totalWords = length(words)

    # this array holds the count. Index is the word length``
    local wordLengths = zeros(Int64,18)

    # Now iterate through the words
    for word in words
        l = length(word)

        # there should be very few words more than 18
        l = l<=18 ? l : 18;
        wordLengths[l] += 1
    end

    # Return % of total word lengths used
    return wordLengths./(totalWords/100.0)
end
```

我使用了穷人版的分词器。分词器本质上是将文本拆分为单词或子单词，以便我们可以逐个处理它们。穷人版分词器删除所有标点符号，并按空格拆分，虽然不够复杂，但足够满足我们的目的。

Julia的分词器接口接受一个`text`作为输入，并返回一个单词数组。然后，我们简单地遍历数组中的所有单词，计算长度，并增加表示该长度的桶。

现在函数已经准备好，我们可以处理文本格式的演讲文件。首先，让我们看看[这里](https://millercenter.org/the-presidency/presidential-speeches)的就职演讲。

```py
hBushJrInaugural = wordLengthHistoFromFile("data/speech-bushjr-inaugural.txt")
hObamaInaugural = wordLengthHistoFromFile("data/speech-obama-inaugural.txt")
hTrumpInaugural = wordLengthHistoFromFile("data/speech-trump-inaugural.txt")
hBidenInaugural = wordLengthHistoFromFile("data/speech-biden-inaugural.txt")
```

因此，我们处理每篇就职演讲，并创建一个桶数组。我们将这些数组水平拼接（`hcat`），得到一个可以绘制的单一18x5数组。

```py
plot(hcat(hBushJrInaugural,hObamaInaugural,hTrumpInaugural,hBidenInaugural ), 
        xticks=([1:18;]),
        yticks=([0:2:100;],["$(x)%" for x=0:2:100]),
        label=["Bush" "Obama" "Trump" "Biden"],
        yaxis=("% OF APPEARANCE"),
        xaxis=("WORD LENGTH"),
        xguidefontsize=8, yguidefontsize=8,
        margin=5mm,
        framestyle = :box,
        lw=2, size=(800,420), marker=(:circle),
        lc=[:red :blue :lightcoral :dodgerblue],
        mc=[:white :white :black :black]
)

savefig("pics/3-presidential-speeches.png")
```

上面的第一行绘制了图表，然后我将图表保存为图像文件。

![](../Images/0ef5fa66d7a0e7996f0fd73b9c2228df.png)

总统演讲中的单词长度。图片来源：作者。

这里的图表显示了每篇演讲的分布。例如，奥巴马总统的就职演讲中不到3%的独特单词由2个字母的单词组成。

5个字母的单词似乎很受欢迎，同时4个和6个字母的单词也不少。注意，虽然9个字母的单词占字典的15%，但在演讲中只占所有独特单词的不到8%。

现在，让我们看一下更为即兴的环境。这应该能给我们一个日常用词的感觉，而不是经过深思熟虑的脚本化工作。我选择了过去四位总统中的每位的一个新闻发布会进行分析。

```py
hBushJrPC = wordLengthHistoFromFile("data/pc_bushjr_final.txt")
hObamaPC = wordLengthHistoFromFile("data/pc_obama_final.txt")
hTrumpPC = wordLengthHistoFromFile("data/pc_trump_laborday.txt")
hBidenPC = wordLengthHistoFromFile("data/pc_biden_first.txt")
```

然后绘制结果：

```py
plot(hcat(hBushJrPC,hObamaPC,hTrumpPC,hBidenPC ), 
        xticks=([1:18;]),
        yticks=([0:2:20;],["$(x)%" for x=0:2:20]),
         label=["Bush" "Obama" "Trump" "Biden"],
         xaxis=("WORD LENGTH"),yaxis="% OF APPEARANCE",
         lw=2, size=(800,420), marker=(:circle),
         xguidefontsize=8, yguidefontsize=8,
         framestyle = :box,
         margin=5mm, ylims = (0,20),
         lc=[:red :blue :lightcoral :dodgerblue],
         mc=[:white :white :black :black]
       )
savefig("pics/4-presidential-press-conf.png")
```

![](../Images/68650cd1995cfa74a7f9fb1fdeaf8117.png)

总统新闻发布会中的单词长度。图片来源：作者。

这里的模式更加清晰。除了拜登总统（他似乎偏爱4个字母的单词）之外，5个字母的单词明显更受欢迎。

在文学和网络上，我们可以找到更多的证据。纽约时报文章的平均单词长度显然是4.9⁵。根据[WolframAlpha](https://www.wolframalpha.com/input?i=average+english+word+length)，英语中的平均单词长度是5.1个字符。该网站还声称，《大英百科全书在线》的平均单词长度为5.3，而维基百科为5.2。本文的单词平均长度为4.75个字母，计算[这里](https://countwordsworth.com/)。这似乎不算太差。

那么，为什么是5个字母呢？在英语中，我们的词汇似乎由5个字母的单词组成，尽管有更多更长的单词可供选择。也许Josh Wardle尝试了四个、五个和六个字母，决定五个字母最适合目标受众和解决时间。游戏应该有一点挑战性，这样你会觉得自己完成了一些事情，但又不会太难。或者，虽然可能性较小，他可能经过了这种分析并决定了五个字母。我们可能永远不会知道。

# 字母频率

![](../Images/057867e72a40485bfab375742041096e.png)

图片由[Isaac Smith](https://unsplash.com/es/@isaacmsmith?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/photos/6EnTPvPPL6I?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

既然我们已经把为什么是五个字母的问题搞清楚了，就让我们专注于改进我们的游戏。

玩Wordle的一种策略是最大化命中率（黄色或绿色），以减少总猜测次数。显而易见的方法是选择包含最常见字母的单词⁶。都柏林大学的模拟发现，根据一个好的首字母选择，平均猜测次数可以从5减少到4⁷。这就是字母频率分析可以帮助的地方。

首先，让我们处理一下我们之前下载的整个单词数据集。这些将包括所有长度的单词。一旦我们弄清楚了所有英语单词中的频率，我们可以将其与5个字母单词中的字母频率进行比较，看看是否有显著差异。

我暂时想不到用列表推导的方式来做这个。不过，“map”和“for-loop”相当简单。由于map稍微快一些，我们先用它。

```py
charfrequency = zeros(Int64,26)
@time map(word->(
map(
  char-> ((char>='a' && char<='z') ? charfrequency[char-'a'+1]+=1 : 1), collect(word))
), dictwords);

0.386160 seconds (5.85 M allocations: 142.942 MiB, 7.98% gc time, 26.68% compilation time)

# convert to percentage
charfrequency = charfrequency ./ (totalWords/100.0) 
```

我不知道你怎么样，但如果这不是我的代码，我可能会对它的作用感到有些迷惑。让我们看看for循环是否更好：

```py
charfrequency = zeros(Int64,26)
@time for word in dictwords
  for char in collect(word)
     if (char>='a' && char<='z')
        charfrequency[char-'a'+1] += 1
     end
  end
end
0.437091 seconds (7.49 M allocations: 171.794 MiB, 6.45% gc time, 1.16% compilation time)

# convert to percentage
charfrequency = charfrequency ./ (totalWords/100.0)
```

这个我可以读懂：一个针对单词的for循环，然后是一个针对字符的for循环。之后，我只是简单地统计字母。性能也不差，增加可读性带来的百分之十的下降似乎是值得的。

让我们绘制这个数组：

```py
bar(charfrequency,
         orientation=:h,
         yticks=(1:26, 'A':'Z'),
         xlabel=("% of APPREARANCE"),
         ylabel=("ALPHABET"),
         yflip=true,
         legend = :none, framestyle = :box,
         xticks=([0:10:110;],["$(x)%" for x=0:10:110]),
         xguidefontsize=8, yguidefontsize=8,
         margin=5mm, xlims = (0,110),
         ytickfont = font(6,"Arial"),
         fill = (0,0.5,:green),
         size=(800,420))

savefig("pics/5-dict-char-freq.png")
```

这就是结果。

![](../Images/c7a86e3c0827cb89d4ef8a8bb21573d8.png)

字母在整个单词数据集中的出现百分比。图片由作者提供。

由于代码的工作方式，我们最终对 ‘SLEEP’ 中的‘E’进行了 2 次计数。这就是为什么我们在数据集中看到 E 的出现率超过 100%。我们应该只计数 1 次，即使字母在单词中出现多次。我们可以通过在计数之前简单地去除单词中的重复字符来解决这个问题。

```py
charfrequencyu = zeros(Int64,26)
@time for word in dictwords
    for char in unique(word)
        if (char>='a' && char<='z')
        charfrequencyu[char-'a'+1] += 1
        end
    end
end

 0.491082 seconds (7.41 M allocations: 241.289 MiB, 7.55% gc time)
```

我将第二个 for 循环中的 `collect` 更改为 `unique`。现在，让我们绘制两个图表，看看是否有变化：

```py
bar(hcat(charfrequency, charfrequencyu),
         orientation=:h,
         yticks=(1:26, 'A':'Z'),
         xlabel=("% of APPREARANCE"),
         ylabel=("ALPHABET"),
         yflip=true, framestyle = :box,
         xticks=([0:10:110;],["$(x)%" for x=0:10:110]),
         xguidefontsize=8, yguidefontsize=8,
         margin=5mm, xlims = (0,110),
         ytickfont = font(6,"Arial"),
         lc=[:black :black], mc=[:black :black],
         fill=[:green :blue], fillalpha=[0.5 0.5],
         label=["frequency" "unique"],
         size=(800,420))
savefig("pics/6-compare-vs-unique.png")
```

这是图表：

![](../Images/a8a15e3bb31ef34fd45d8ad903d33058.png)

有无独特字母的出现次数比较。图片由作者提供。

我没有看到模式上有什么显著的不同。最常出现的字母也是出现次数最多的。从现在起，我们可以使用独特的字母来推动我们在 Wordle 中的词汇选择决策。

## 猜测词和答案词集合

上述分析适用于整个英语词汇数据集，不过，仅考虑 5 个字母的词汇会有所不同吗？让我们找出答案。使用 `filter` Julia 关键字，我们来制作一个只包含 5 个字母的词汇的新数组。

```py
julia> guesswords = filter(x->(length(x)==5), dictwords)
julia> length(guesswords)

11210
```

官方 Wordle [猜测词汇表](https://github.com/tabatkins/wordle-list/blob/main/words) 包含略低于 13K 个词汇。所以，我们已经足够接近了。这些是你可以在游戏框中输入的词汇。[Wordle 答案词汇表](https://www.wordunscrambler.net/word-list/wordle-word-list) 是一个子集，包含 2.3K 个词汇，只包含可能作为隐藏答案出现的词汇。那么，为什么会有差异呢？

Josh Wardle 和他的伙伴 Palak Shah 创建了一个经过精心筛选的 [答案词汇表](https://www.wordunscrambler.net/word-list/wordle-word-list)，避免了晦涩和难解的词汇，以便让游戏对每个人都充满乐趣⁸。虽然你会发现 ZOPPA 是一个有效的猜测词，但不要指望它很快会出现作为答案。

为了查看两者之间的字母频率模式是否存在差异，让我们也生成那个列表。我使用了 size-35 的 [aspell](http://app.aspell.net/create) 词汇表作为起点，因为它应该包含更多常见的英语词汇。

```py
rawwords35 = readdlm("data/scowl_american_words_35.txt",header=false,String)
length(rawwords35)
50043

dictwords35 = String[];
for word in rawwords35
    # println(word)
    if (length(word)>0 && !(occursin("\'",word) 
          || occursin("-",word) 
          || occursin(" ",word)
          || (word[1]<'a' || word[1]>'z')
          ))
        push!(dictwords35,word)
    end
end
totalWords = length(dictwords35)
39142
```

这个列表包含 39K 个词汇，而 85 大小的词汇表则包含 343K 个词汇。让我们只筛选出 5 个字母的词汇。

```py
answerlist = filter(x->(length(x)==5), dictwords)
length(answerlist)
3467
```

Wardle 和 Shah 还从他们的答案词汇表中删除了所有的复数词汇。像 CROPS、ROCKS、DROPS 等复数形式的词汇已经从答案列表中删除，而它们仍然存在于猜测列表中。让我们大致看看有多少复数词汇。

```py
answerlist = filter(x->(x[5]!='s' ||  x[4]=='s'), answerlist)

length(answerlist)
 2282

3467-2282
 1185
```

在猜测词汇表中，35% 或 1185 个词汇是复数！CROSS、CLASS 等是答案列表中的一些代表性词汇，它们没有 3 或 4 个字母的单数变体（这些词汇已经在 Wordle 中出现过，因此这里没有剧透）。

这解释了为什么最近我的好朋友 Sirisha 在那天的单词上表现得与平时不同，最终用 6 次尝试解决了它。她是我们中学小组的活跃成员，该小组每天解决 Wordle，并且通常表现很好。然而，她觉得选择太多，不知道答案列表中已经去除了 30% 的复数形式⁸。现在掌握了这些信息，她不太可能在下次 S 在第 5 位标记为绿色时被难住，我们也是如此。

在我们开始分析答案列表的模式之前，让我们看看目前拥有的三组字母频率；完整的 85 个单词列表、5 个字母的猜测单词列表和 5 个字母的答案单词列表。

让我们编写一个辅助函数来加载单词列表，

```py
function letterFrequency(words)

    # count total words in the file
    totalWords = length(words);

    # initialize freq vector
    charFreq = zeros(Int64,26)
    for word in words
        for char in unique(word)
            if (char>='a' && char<='z')
                charFreq[char-'a'+1] += 1
            end
        end
    end

    # element wise devide by totalWords and return
    return charFreq ./ (totalWords/100.0) ;
end
```

函数准备好了，我们加载它吧。

```py
cfDictwords = letterFrequency(dictwords)
cfGuesslist  = letterFrequency(guesslist)
cfAnswerlist = letterFrequency(answerlist)
```

现在是比较图，

```py
plot((1:26, 'A':'Z'), hcat(cfDictwords, cfGuesslist, cfAnswerlist), 
    label=["Dict Words" "Guess list" "Answer list"],
    yticks=([0:5:70;],["$(x)%" for x=0:5:70]),
    framestyle = :box,
    xlabel="ALPHABET",ylabel="% OF APPEARANCE",
    xguidefontsize=8, yguidefontsize=8,
    margin=5mm, ylims = (0,75),
    xticks=(1:26, 'A':'Z'),
    size=(800,420)
)

savefig("pics/7-compare-guess-answer.png")
```

我将频率串联起来，然后在折线图上绘制它们。

![](../Images/89699e816906cd739c58fc68d0d27b41.png)

来自整个数据集、猜测和答案列表的字母频率。图片由作者提供。

在上面的图表中，每个数据点表示包含特定字母的单词百分比。例如，对于 A，47% 的所有英语单词中至少有一个 A。相比之下，猜测和答案单词列表中的单词中只有 37% 包含 A。频率百分比之间的差异可能是由于英语单词列表中的长单词，因为像 A、E、I、R 等字母在长单词中出现的频率较高。我们已经知道，答案单词列表中的 S 频率低得多，这是因为去除了复数形式。

查看猜测列表可能会给出不正确的 S 模式。因此，让我们专注于答案列表，并更详细地分析其趋势。

## 字母频率

我们现在将按顺序绘制字母频率，以便在图表的顶部显示出现频率最高的字母。

```py
listOrder = sortperm(cfAnswerlist, rev=true)

bar(cfAnswerlist[listOrder],
      orientation=:h,
      yticks=(1:26, ('A':'Z')[listOrder]),
      xlabel=("% of APPREARANCE"),
      ylabel=("ALPHABET"),
      yflip=true, framestyle = :box,
      xticks=([0:5:55;],["$(x)%" for x=0:5:50]),
      xguidefontsize=8, yguidefontsize=8,
      margin=5mm, xlims = (0,55),
      legend=:none,
      ytickfont = font(6,"Arial"),
      lc=(:black), mc=(:black),
      fill=(:orange), fillalpha=(0.5),
      size=(800,420))

savefig("pics/8-letterfrequency.png")
```

Julia 的 sortperm 返回一个排列向量 `I`，将 `cfAnswerlist[I]` 按排序顺序排列。条形图获取 `cfAnswerlist[listOrder]`，这是一个频率值的排序列表。`bar` 函数的 yticks 参数被分配为 `('A':'Z')[listOrder]`，这将数组从 A 到 Z 按频率递减的顺序排序。

![](../Images/d1dbb827ad99c82791bc0c8112ee1fca.png)

答案列表中字母的出现频率。图片由作者提供。

E 是最频繁的字母，至少出现在 52% 的答案单词中。A、R、O、I 和 T 是接下来的五个。ORATE 似乎是一个好的起始词。

V、X、Z、Q 和 J 是出现最少的字母，其中 J 出现在答案列表中的比例稍微超过 1%。我还没有验证这一点，但分析指出 XYLYL 是最差的起始词⁹ 看起来是对的。

## 如何使用分布

答案列表中字母的分布可以非常有用进行研究和记忆。假设你使用了第一个猜测单词 ‘ORATE’ 或者（我另一个喜欢的）‘AROSE’。对于 ORATE，你得到了 3 个黄色字母 R、A 和 E。下一个选择可以是 LASER、WAVER 等。我假设你是在使用“困难模式”进行游戏。上面的图表告诉我们，L 和 S 比 W 和 V 更可能出现在答案单词中。实际上，可能性大约是 W 和 V 的 3 倍。因此，我的猜测是 LASER。

作为另一个例子，如果你在 AROSE 上没有得到任何匹配，我通常会尝试 ‘UNWIT’，然后是 GLYPH。如果 ORATE 没有匹配，SULCI 可以是一个不错的第二个猜测，然后是 NYMPH。

作为一种策略，你可以记住频率的填充顺序；

EAROI TLSDN CUHYP GMBKW FVXZQ J

或者，记住这三个单词 ORATE、SULCI 和 NYMPH（以及 D）。当你在 ORATE 中得到一个匹配项，比如 E，你可以开始从 SULCI 和 NYMPH 中挑选字母作为你的下一个单词（这里可以想到 CLUES）。

其他研究者也推荐了类似的策略，即选择三个字母互不重叠的单词。Nisansa de Silva¹⁰ 推荐了 RAISE、CLOUT 和 NYMPH。我个人也曾使用过 AROSE、UNWIT 和 GLYPH。

# 结论

![](../Images/0d7486ff6e274a639a5c0715c07b8a6a.png)

照片由 [Sigmund](https://unsplash.com/@sigmund?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，发布于 [Unsplash](https://unsplash.com/photos/By-tZImt0Ms?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我们已经从两个方面分析了游戏 Wordle，使用 Julia 来分析单词。首先，我试图证明游戏中选择五个字母的合理性。结果发现，我们的口语和书面表达中，五个字母的单词使用频率高于其他长度的单词。因此，选择五个字母的单词对于游戏是很有意义的。

其次，我们首先查看了整个英语单词列表，然后查看了五个字母猜测和浓缩答案列表中字母的频率。字母的分布图可以用来更有效地猜测单词。我注意到记住 ORATE、SULCI 和 NYMPH 可以帮助我们猜测更可能返回更多匹配的单词。使用这些字母作为指南将有助于提高你的游戏水平。

记住这些技巧，祝你明天的 Wordle 游戏一切顺利！

# 参考文献

1.  弗吉尼亚大学公共事务米勒中心。“总统演讲：可下载数据。”访问于 2022 年 3 月 17 日。 [https://millercenter.org/presidential-speeches-downloadable-data](https://millercenter.org/presidential-speeches-downloadable-data)。

1.  Ravi Parikh，《各种语言中的单词长度分布》，网站 [http://www.ravi.io/language-word-lengths](http://www.ravi.io/language-word-lengths)

1.  Reginald Smith，《不同单词长度频率：分布与符号熵》，[https://arxiv.org/ftp/arxiv/papers/1207/1207.2334.pdf](https://arxiv.org/ftp/arxiv/papers/1207/1207.2334.pdf)

1.  Liston Tellis，“正态分布——钟形曲线”，Medium，2020年7月5日，[https://medium.com/analytics-vidhya/normal-distribution-the-bell-curve-4f4a5fc2caaa](https://medium.com/analytics-vidhya/normal-distribution-the-bell-curve-4f4a5fc2caaa)

1.  Ann Wylie，“在线词语的最佳长度是什么？”，Ann Wylie的写作技巧博客，[https://www.wyliecomm.com/2021/11/whats-the-best-length-of-a-word-online/](https://www.wyliecomm.com/2021/11/whats-the-best-length-of-a-word-online/)

1.  Ste Knight，“提高你得分的12个Wordle技巧和窍门”，Make Use Of，2022年9月，[https://www.makeuseof.com/wordle-tips-hints-tricks/](https://www.makeuseof.com/wordle-tips-hints-tricks/)

1.  Barry Smyth，“Wordle中的大数据”，2022年6月，Towards Data Science，[https://towardsdatascience.com/big-data-in-little-wordle-306d5502c4d9](https://towardsdatascience.com/big-data-in-little-wordle-306d5502c4d9)

1.  Jonathan Lee，“纽约时报终于对Wordle做出改变”，华盛顿邮报，2022年11月，[https://www.washingtonpost.com/video-games/2022/11/07/wordle-new-answers-new-york-times-update/](https://www.washingtonpost.com/video-games/2022/11/07/wordle-new-answers-new-york-times-update/)

1.  Brittany Alva，“根据科学，Wordle中最糟糕的起始词”，SVG，2022年3月，[https://www.svg.com/751712/the-worst-starting-word-in-wordle-according-to-science/](https://www.svg.com/751712/the-worst-starting-word-in-wordle-according-to-science/)

1.  Nisansa de Silva，“使用字符统计选择Wordle种子词”，2022年2月，*arXiv:2202.03457*，[https://arxiv.org/pdf/2202.03457.pdf](https://arxiv.org/pdf/2202.03457.pdf)
