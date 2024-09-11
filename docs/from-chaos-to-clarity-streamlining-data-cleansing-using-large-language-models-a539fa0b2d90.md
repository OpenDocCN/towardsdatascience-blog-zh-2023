# 从混乱到清晰：利用大型语言模型简化数据清洗

> 原文：[https://towardsdatascience.com/from-chaos-to-clarity-streamlining-data-cleansing-using-large-language-models-a539fa0b2d90?source=collection_archive---------5-----------------------#2023-06-07](https://towardsdatascience.com/from-chaos-to-clarity-streamlining-data-cleansing-using-large-language-models-a539fa0b2d90?source=collection_archive---------5-----------------------#2023-06-07)

## 使用 OpenAI 的 GPT 模型清理调查回应。包含完整代码和 Github 链接。

[](https://medium.com/@naresh-ram?source=post_page-----a539fa0b2d90--------------------------------)[![Naresh Ram](../Images/4a20b5646f75fb6cc2a5684fd3daf6db.png)](https://medium.com/@naresh-ram?source=post_page-----a539fa0b2d90--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a539fa0b2d90--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a539fa0b2d90--------------------------------) [Naresh Ram](https://medium.com/@naresh-ram?source=post_page-----a539fa0b2d90--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa5665b0dac5f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-chaos-to-clarity-streamlining-data-cleansing-using-large-language-models-a539fa0b2d90&user=Naresh+Ram&userId=a5665b0dac5f&source=post_page-a5665b0dac5f----a539fa0b2d90---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a539fa0b2d90--------------------------------) ·17 min 阅读 ·2023年6月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa539fa0b2d90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-chaos-to-clarity-streamlining-data-cleansing-using-large-language-models-a539fa0b2d90&user=Naresh+Ram&userId=a5665b0dac5f&source=-----a539fa0b2d90---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa539fa0b2d90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-chaos-to-clarity-streamlining-data-cleansing-using-large-language-models-a539fa0b2d90&source=-----a539fa0b2d90---------------------bookmark_footer-----------)![](../Images/629cba145be86d7a78351b5798f1bb46.png)

图片由 Dall-E 2 提供，生成并由作者修改。

在数字时代，准确可靠的数据对致力于提供个性化体验和做出明智决策的企业至关重要[1]。然而，数据的庞大数量和复杂性常常带来重大挑战，需要大量繁琐的手工工作。此时，改变游戏规则的大型语言模型（LLMs）技术登场。这些先进的AI工具凭借其自然语言处理能力和模式识别，有可能彻底改变数据清洗过程，使其更加可用。

在数据科学家的工具箱中，LLMs就像是扳手和螺丝刀，重新塑造活动，利用其强大能力提高数据质量。隐喻中的锤子将揭示可操作的见解，并*最终*为更好的客户体验铺平道路。

话虽如此，让我们直接深入到今天将用作示例的用例中。

![](../Images/97f979d0821efb5af6e1b55a5fd222fc.png)

图片由 [Scott Graham](https://unsplash.com/@homajob?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/OQMZwNd3ThU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 用例

在对学生进行调查时，最糟糕的事情就是把一个事实字段留作自由文本！你可以想象我们收到了哪些回应。

开玩笑的话题，我们的一位客户，[Study Fetch](https://www.studyfetch.com/)，一个利用课程材料为学生创建个性化全能学习集的AI驱动平台，进行了一项针对大学生的调查。在收到超过10K的回应后，他们的CEO兼联合创始人Esan Durrani遇到了一点小麻烦。原来，调查中的“主要”字段是一个自由文本框，意味着受访者可以随意输入内容。作为数据科学家，我们知道如果你想进行统计分析，这不是最明智的做法。因此，调查的原始数据看起来就是这样的……

```py
Anthropology 
Chem E 
Computer Science
Business and Law
Drama
cs
IMB
```

哦天哪！准备好拿起你的Excel，开始一个排序冒险，这可能仅需一个小时，或者，谁知道，也许需要三小时。只有这样，这些数据异端才会被彻底清除。

然而，不用担心，因为我们有大型语言模型（LLM）的锤子。

正如一位智者曾说的，如果你只有一把锤子，那么一切看起来都像钉子。而且，数据清洗工作似乎就是最完美的钉子了。

我们可以简单地让我们友好的 LLM 将这些分类到已知的专业中。具体来说，OpenAI 的生成预训练变换器（GPT），一个支持流行的聊天机器人应用 ChatGPT 的 LLM，将适用于这种情况。GPT 模型使用超过 1750 亿个参数，并且已经在从 Common Crawl 提取的 26 亿个存储网页上进行了训练。此外，通过一种称为人类反馈的强化学习（RLHF）的技术，训练师可以引导和促使模型提供更准确和有用的响应。[2]

我认为对于我们的目的来说，1750 亿+ 参数应该足够了。只要我们能够提出正确的提示。

![](../Images/3010dd28b52df0da8ba791e7211a3cca.png)

[Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 的照片，来源于 [Unsplash](https://unsplash.com/photos/BoAbPMRKLS0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 一切都在提示中

来自 AI 公司 Ryan 和 Esan，他们的专长是编写出色的提示，提出了我们提示的第一个版本。这个版本非常好，并且在使用语言推断[3]时效果很好，但还有两个方面可以改进：

+   这篇文章是为了一个记录而写的

+   它是作为‘完成’使用 Da Vinci 模型编写的（提到 IT 让我银行账户都吓了一跳）

这会花费我们太多，而且这显然是不行的。因此，Ryan 和我分别将提示重写为一个使用‘gpt-3.5-turbo’进行批量操作的聊天提示。OpenAI 的 [提示最佳实践](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-openai-api) 和课程 [ChatGPT Prompt Engineering for Developers](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/) 对我非常有帮助。我们经过了几次迭代的构思、实施、分析和改进，最终得到了一个好的工作版本。

不再废话，以下是修订版 2 后的提示：

```py
1\. Arts and Humanities: Literature and arts.
2\. Social Sciences: psychology, sociology, and political science
3\. Business and Management: marketing, finance, economics, MBA
4\. Life Sciences: Biology, environmental,agriculture-related
5\. Engineering and Technology: Electrical, mechanical, etc.
6\. Medicine Health and Allied Sciences: Nursing, medical school
7\. All Other Programs: Everything else.

The above information pertains to categories of undergraduate programs. 
Categorize the below data into one of the program categories above.

Desired Format: CSV of data row, program name

Data
###
Drama
Literature
Physics
Health sciences
Mechanical Engineering
###
```

对于这个提示，LLM 的响应是

```py
Drama, Arts and Humanities: Literature and arts
IMB, All Other Programs: Everything else
Health sciences, Medicine Health and Allied Sciences: Nursing, medical school
MBA, Business and Management: marketing, finance, economics, MBA
Mechanical Engineering, Engineering and Technology: Electrical, mechanical, etc.
```

这将有效，虽然有点。 但我不太喜欢冗长的程序名称重复的冗余文本。对于 LLM，文本是标记，而标记是要花钱的。你看，我的编程技能是在互联网泡沫的熊熊烈火中锻造出来的。让我告诉你，我从不放过任何节省成本的机会。

所以，我在‘期望格式’部分稍微更改了提示。我要求模型仅输出调查响应的序号（例如上面的 1 代表戏剧）和程序的序号（例如 1 代表文学）。然后 Ryan 建议我请求 JSON 输出而不是 CSV，以便更容易解析。他还建议我添加一个‘示例输出’部分，这个建议非常好。

最终的提示如下（为了清晰简化）：

```py
1\. Arts and Humanities: Literature and arts.
2\. Social Sciences: psychology, sociology, and political science
3\. Business and Management: marketing, finance, economics, MBA
4\. Life Sciences: Biology, environmental,agriculture-related
5\. Engineering and Technology: Electrical, mechanical, etc.
6\. Medicine Health and Allied Sciences: Nursing, medical school
7\. All Other Programs: Everything else.

The above information pertains to categories of undergraduate programs. 
Categorize the below data into one of the program categories above.

Desired Format: JSON format of an array with response number, program number

Example Example output:  [[1,7],[2,1],[3,5],[4,8],[5,7]]

Data
###
1\. Drama
2\. IMB
3\. Health sciences
4\. MBA
5\. Mechanical Engineering
###
```

我们使用的完整提示可以在GitHub链接[这里](https://github.com/aaxis-nram/data-cleanser-llm-node)查看。

**模型的输出**：

```py
[
  [1, 1]
  [2, 3]
  [3, 6]
  [4, 3]
  [5, 5]
]
```

正如之前讨论的那样，模型的输出是调查响应的序数与我们定义的类别之间的映射。以第一行举例：1,1。那意味着1是响应号，1是对应的映射程序号。调查响应1是“戏剧”，映射程序1是“艺术与人文学科”。这似乎是对的！戏剧在其正确的#1位置，所有的目光都集中在它上面。

尽管输出乍看起来像是嵌入（用于聚类和降维）的结果，但它们只是带有序数位置的相同映射信息。除了在令牌使用上提供一些成本优势外，这些数字也更容易解析。

我们现在可以将文件中的原始调查响应翻译为有意义的专业，进行汇总，并获得有价值的可操作见解。

但等等，我不打算坐在电脑前，将每一块调查响应输入到浏览器中并计算映射。除了令人厌烦外，错误率也完全无法接受。

我们需要一些老式的自动化。进入API…

![](../Images/3fa72dd277d6faa436ff71e5527e30fe.png)

图片来源于[劳拉·奥克尔](https://unsplash.com/@viazavier?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄于[Unsplash](https://unsplash.com/photos/UQ2Fw_9oApU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# API的救援

如你所知，应用程序编程接口（API）允许我们的程序高效地与第三方服务互动。尽管许多人在使用ChatGPT时取得了令人印象深刻的成就，但语言模型的真正潜力在于利用API将自然语言能力无缝集成到应用程序中，使其对用户不可察觉。这就像你用来阅读这篇文章的手机或电脑中所蕴含的不可思议的科技。

如果你还没有，可以在这里申请API访问，[https://openai.com/blog/openai-api](https://openai.com/blog/openai-api) [4]。一旦注册并获得API密钥，可以在[这里](https://platform.openai.com/docs/api-reference/chat)找到规格说明。包含代码示例的一些非常有用的示例可以在[这里](https://platform.openai.com/examples)找到。[playground](https://platform.openai.com/playground)是一个很好的功能，可以在将其投入使用前测试各种设置。

我们将使用REST的聊天完成API。调用的示例有效负载如下：

```py
{ 
  “model”: “gpt-3.5-turbo”, 
  “temperature”: 0,
  "n": 1,
  “messages”: [
     {“role”: “user”, “content”: “Hello, I'm a nail.”}
  ]
}
```

让我们快速查看一下参数及其效果

**模型**

目前唯一对公众开放的聊天完成模型是 gpt-3.5-turbo。Esan 有访问 GPT 4 模型的权限，这让我非常嫉妒。虽然 gpt-4 更加准确，并且幻觉更少[2]，但它的费用大约是 gpt-3.5-turbo 的 20 倍。对于我们的需求来说，Turbo 先生已经相当合适了，谢谢。

**温度**

在提示旁边，温度是我们可以传递给模型的最重要设置之一。根据 API 文档，它可以设置为 0 到 2 之间的值。它有着显著的影响[6]，因为它控制输出的随机性，类似于你在开始写作前体内的咖啡因含量。关于每个应用程序可使用的值的指南在这里给出[7]。

对于我们的使用案例，我们仅仅希望没有变化。我们希望引擎每次都给出完全一样的映射。因此，我们使用了 0 的值。

**n**

要生成多少个聊天完成选项？如果我们是在进行创意写作，并且希望选择的选项超过 1 个，我们可以使用 2 甚至 3 个。对于我们的情况，n=1（默认）效果很好。

**消息**

角色可以是系统、用户或助手。系统角色提供指令并设定上下文。用户角色代表最终用户的提示。助手角色基于对话历史进行回应。这些角色有助于构建对话，并实现用户与 AI 助手之间的有效互动。

**模型最大令牌**

这不一定是我们在请求中传递的参数，不过另一个名为 max_tokens 的参数限制了聊天响应的总长度。

首先，令牌可以被认为是一个单词的一部分。一个令牌大约是 4 个英文字符。例如，引用“预见未来的最佳方式是创造未来”，这个引言被归因于亚伯拉罕·林肯等人，包含了 11 个令牌。

![](../Images/69813147518fbb984cbb356dc463a64a.png)

图片来源于 Open AI Tokenizer。由作者生成。

如果你认为一个令牌就是一个单词，这里有另外一个 64 个令牌的例子，来说明这并不是那么简单。

![](../Images/45ebcfd49afaab54bb19814bf769cbc0.png)

图片来源于 Open AI Tokenizer。由作者生成。

准备好接受震撼的揭示：你在消息中包含的每一个表情符号都增加了最多 6 的费用。没错，你喜爱的笑脸和眨眼符号都是狡猾的小令牌窃贼！😉💸

模型的最大令牌窗口是一个技术限制。你的提示（包括你放入的任何额外数据）和答案必须都在模型最大限制内，详细信息请查看[这里](https://platform.openai.com/docs/models/model-endpoint-compatibility)。在聊天完成的情况下，内容、角色和所有之前的消息都消耗令牌。如果你从输入或输出（助手消息）中删除一条消息，模型将失去对其的所有记忆[8]。就像多莉在寻找奇科时一样，没有法比奥，没有宾戈，没有哈波，没有艾尔莫？… 尼莫！

对于gpt-3.5-turbo，模型的最大限制是4096个token，或者大约16K字符。对于我们的用例，提示大约是2000个字符，每个调查回复大约是20个字符（平均），映射回复是7个字符。因此，如果我们在每个提示中放入N个调查回复，最大字符数将是：

2000 + 20*N + 7*N 应小于16,000。

解算后，我们得到一个小于518或大约500的N值。从技术上讲，我们可以在每个请求中放入500个调查回复，并处理数据20次。相反，我们选择每个请求中放入50个调查回复，处理200次，因为如果我们在单个请求中放入超过50个调查回复，偶尔会收到异常回复。有时，服务会发脾气！我们不确定这是系统性乖戾的慢性病，还是刚好碰上了运气不好的一面。

那么，我们如何使用这个API呢？让我们进入重点，代码部分。

![](../Images/b6ca9b8296d4852b235147598e4088ad.png)

[Markus Spiske](https://unsplash.com/ko/@markusspiske?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄于[Unsplash](https://unsplash.com/photos/iar-afB0QQw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 代码的方式

Node.js是一个JavaScript运行环境[9]。我们将编写一个Node.js/JavaScript程序，该程序将执行在这个流程图中描述的操作：

![](../Images/822cff75f20673c7821f6388fd4629d3.png)

程序的流程图。图片由作者提供。

我的JavaScript技能不是特别出色。我可以写更好的Java、PHP、Julia、Go、C#，甚至Python。但Esan坚持使用Node，所以就用JavaScript吧。

所有代码、提示和示例输入可以在这个[GitHub链接](https://github.com/aaxis-nram/data-cleanser-llm-node)中找到。不过，让我们来看看最重要的部分：

首先，让我们看看如何使用“csv-parser”Node库读取CSV文件。

```py
fs.createReadStream(fileName)
  .pipe(csv())
  .on('data', (data) => rows.push(data))
  .on('end', async () => {
   // Reading is done. Call classifier here
     . . .      
}
```

接下来，我们调用分类器生成映射。

```py
for(index = 0; index < totalDataPoints; ++index) {
        dataText += (index+1).toString() + ". " 
                        + uniqueData[index] + "\n";
        requestCount++;
        if (requestCount>batchSize||index==totalDataPoints-1){
           dataText += "###\n";
           // We have loaded batchSize(50) response. 
           // Now construct the prompt 
           ...
      }
}
```

然后，提示由类别、主要提示文本和CSV中的数据构成。我们将提示通过他们的OpenAI Node库发送到服务。

```py
let prompt = categoriesText + mainPrompt + dataText;
let payload = {
    model: "gpt-3.5-turbo",
    temperature: 0,
    messages: [ 
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user","content": prompt }
    ]
};

try {
   const response = await openai.createChatCompletion(payload);
   let mapping = JSON.parse(response.data.choices[0].message.content); 
   // Here save the mappings
   ...

} catch (error) {
   console.error('API Error:',error);
}
```

最后，当所有迭代完成后，我们可以将srcCol文本（调查回复）翻译为targetCol（标准化的程序名称），并写出CSV文件。

```py
for (let row of rows) {
    srcVal = row[srcCol].trim();
    if (!row[targetCol] && dataMapping[srcVal])
        row[targetCol] = dataMapping[srcVal];
}

stringify(rows, {
    header: true
}, function (err, output) {
   fs.writeFile(__dirname+'/'+destFileName, output,
       function(err, result) {
          if(err) console.log('error', err);
       });
});
```

那段JavaScript代码没有我想象中的那么复杂，2到3小时内完成了。我想，直到开始做之前总是看起来很令人生畏。

现在，代码已经准备好，是时候进行最终执行了……

![](../Images/c1dccfd7b15920a59038fb6d2723ee56.png)

[Alexander Grey](https://unsplash.com/@sharonmccutcheon?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄于[Unsplash](https://unsplash.com/photos/8lnbXtxFGZw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 执行

现在，我们需要一个地方来运行代码。在争论是否应该获得一个云实例来运行负载后，我做了一些快速计算，发现我可以在不到一小时内在我的笔记本电脑上运行它。这也不算太糟糕。

我们开始了测试轮次，并注意到服务有1/10的几率会返回提供给它的数据，而不是映射。因此，我们只会收到调查回应的列表。由于没有找到映射，这些CSV文件中的回应会被映射为空字符串。

与其在代码中检测并重试，我决定重新运行脚本，但只处理目标列为空的记录。

脚本将从所有行中目标列为空开始，并填写标准化的程序名称。由于响应中的错误，一些行的目标列没有被映射并保持为空。当脚本第二次运行时，它只会为第一次运行中未处理的回应构造提示。我们重跑了几次程序，最终将所有内容映射完成。

多次运行大约花了30分钟，并且不需要太多监督。这里是模型的一些更有趣的映射的选择：

![](../Images/99e85c0374f04c76b2cc3ad622f97dfc.png)

输入和程序名称之间的示例映射。图片由作者提供。

大多数看起来对。组织行为学是社会科学还是商业学科呢？我想两者都可以。

每个约50条记录的请求总共使用了大约800个tokens。整个过程的费用是40美分。我们可能花了10美分进行测试、重跑等。所以，总费用约50美分，大约2 ½小时的编码/测试时间和½小时的运行时间，我们完成了工作。

**总费用：** 约1美元以内

**总时间：** 大约3小时

也许使用Excel、排序、正则表达式以及拖拽复制的手动转换，我们可以在同样的时间内完成并节省一点费用。但是，这要有趣得多，我们学到了东西，我们有了一个可重复的脚本/过程，并且还写了一篇文章。而且，我觉得[StudyFetch](https://www.studyfetch.com/)可以承担这50美分。

这是我们高效且经济地实现的一个好用例，但LLMs还能用于什么其他用途呢？

![](../Images/16d14ddbac51d8d5b5a9d9c84159a6af.png)

照片由[Marcel Strauß](https://unsplash.com/@martzzl?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来自[Unsplash](https://unsplash.com/photos/NMGFl05r728?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 寻找更多的钉子

为你的应用程序添加语言能力可能会有比我上面所展示的更多的用途。这里是一些仅与我们查看的审核数据相关的更多用例：

**数据解析和标准化**：LLMs 可以通过识别和提取来自非结构化或半结构化数据源的相关信息，来帮助解析和标准化数据，例如我们刚刚查看的数据。

**数据去重**：LLMs 可以通过比较各种数据点来帮助识别重复记录。例如，我们可以比较评论数据中的姓名、专业和大学，并标记潜在的重复项。

**数据总结**：LLMs 可以总结不同的记录，以了解响应情况。例如，对于“你在学习过程中面临的最大挑战是什么？”这个问题，大型语言模型可以总结来自相同专业和大学的多个回答，以查看是否存在任何模式。然后我们可以将所有总结放入一个请求中，获取总体列表。但我怀疑对每个客户细分市场的总结会更有用。

**情感分析**：LLMs 可以分析评论以确定情感并提取有价值的见解。对于“你会为帮助你学习的服务付费吗？”这个问题，LLMs 可以将情感分类为 0（非常负面）到 5（非常积极）。我们可以利用这一点按细分市场分析学生对付费服务的兴趣。

尽管学生评价是一个较小的缩影，但这个技术在更广泛的领域也有许多用途。在我工作的[AAXIS](https://www.aaxisdigital.com)公司，我们实施了企业对企业和企业对消费者的数字商务解决方案。这包括将大量数据从现有的旧系统迁移到具有不同数据结构的新系统。我们使用各种数据工具来分析源数据，以确保一致性。本文中概述的技术可能会对这个目的有很大帮助。

其他一些数字商务用例包括检查产品目录中的错误、编写产品文案、扫描评论回复和产品评论总结等等。比起当询问他们的专业时的本科生创意，编程要简单得多。

尽管大型语言模型（LLMs）在数据清洗中可能是强大的工具，但仍需注意，它们应该与其他技术和人工监督结合使用。数据清洗过程通常需要领域专业知识、上下文理解和手动审查，以做出明智的决策并保持数据完整性。LLMs 也不是推理引擎[10]。它们是下一个词预测器。它们往往非常自信且令人信服地提供错误信息（幻觉）[2][11]。幸运的是，在我们的测试过程中，我们没有遇到任何幻觉，因为我们的用例主要涉及分类。

如果谨慎使用并了解潜在陷阱，LLMs 可以成为你工具箱中的一个很好的工具。

![](../Images/14ea7f5c68fc1c141a45404c249fc973.png)

图片由 [Paul Szewczyk](https://unsplash.com/fr/@allphotobangkok?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/photos/xTPiSriowpA?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 最终的关键

在这篇文章中，我们首先探讨了数据清理的具体用例：将调查回应标准化为一组特定值。这将使我们能够对回应进行分组，并获得有价值的见解。我们使用了大型语言模型（LLM），Open AI 的 GPT 3.5 Turbo，来帮助分类这些回应。我们回顾了使用的提示、如何利用提示进行 API 调用以及所需的代码来自动化所有操作。最后，我们将所有内容整合在一起，总共花费的 OpenAI 工具成本不到一美元。

我们是否拥有一个典型的大型语言模型工具，并找到完美的解决方案？也许。但更可能的是，我们拥有了一把瑞士军刀，并用它来剥皮和吃鱼。虽然不是特别专业，但仍然非常合适。至于Esan，真的很喜欢寿司。

你的用例是什么？我们很想听听你的意见！

# 同谋者

本文的主要工作由我 **Esan Durrani** 和 Ryan Trattner 执行，我们是 [StudyFetch](https://www.linkedin.com/company/studyfetch/about/) 的联合创始人，该平台利用课程材料为学生创建个性化的全能学习集。

我还要感谢 Prashant Mishra、Rajeev Hans、Israel Moura 和 Andy Wagner，我在 [AAXIS Digital](https://www.aaxisdigital.com/) 的同事们，感谢他们对本文的审阅和建议。

我还要感谢我三十年的朋友 **基兰·邦达拉帕提**，TRM Labs 的工程副总裁，感谢他在生成 AI 领域的初步指导和对本文的审阅。

另外，感谢我的编辑 [梅根·波尔斯特拉](https://www.linkedin.com/in/megan-polstra-3b6561171/)，一如既往地使文章看起来和感觉专业。

# 参考文献

1\. **泰穆·赖塔卢奥托**，《数字时代个性化营销的重要性》，MaketTailor Blog，2023年5月，[https://www.markettailor.io/blog/importance-of-personalized-marketing-in-digital-age](https://www.markettailor.io/blog/importance-of-personalized-marketing-in-digital-age)

2\. **安库尔·A·帕特尔**、**布赖恩特·林顿** 和 **迪娜·索斯塔雷克**，《GPT-4、GPT-3 和 GPT-3.5 Turbo：OpenAI 大型语言模型综述》，2023年4月，Ankur’s Newsletter，[https://www.ankursnewsletter.com/p/gpt-4-gpt-3-and-gpt-35-turbo-a-review](https://www.ankursnewsletter.com/p/gpt-4-gpt-3-and-gpt-35-turbo-a-review)

3\. **亚历山德拉·门德斯**，《终极ChatGPT提示工程指南：面向普通用户和开发者》，2023年6月，Imaginary Cloud Blog，[https://www.imaginarycloud.com/blog/chatgpt-prompt-engineering/](https://www.imaginarycloud.com/blog/chatgpt-prompt-engineering/)

4\. **塞巴斯蒂安**，《如何在 Node.js 中使用 OpenAI 的 ChatGPT API》，2023年3月，Medium — **智能编程方式**，[https://medium.com/codingthesmartway-com-blog/how-to-use-openais-chatgpt-api-in-node-js-3f01c1f8d473](https://medium.com/codingthesmartway-com-blog/how-to-use-openais-chatgpt-api-in-node-js-3f01c1f8d473)

5\. **特里斯坦·沃尔夫**，《利用 OpenAI API Playground 解放你的 ChatGPT 提示》，2023年2月，Medium — **明日故事**，[https://medium.com/tales-of-tomorrow/liberate-your-prompts-from-chatgpt-restrictions-with-the-openai-api-playground-a0ac92644c6f](https://medium.com/tales-of-tomorrow/liberate-your-prompts-from-chatgpt-restrictions-with-the-openai-api-playground-a0ac92644c6f)

6\. **AlgoWriting**，《简单指南：设置 GPT-3 温度》，2020年11月，Medium，[https://algowriting.medium.com/gpt-3-temperature-setting-101-41200ff0d0be](https://algowriting.medium.com/gpt-3-temperature-setting-101-41200ff0d0be)

7\. **凯恩·胡珀**，《用 Ruby 掌握 GPT-3 温度参数》，2023年1月，Plain English，[https://plainenglish.io/blog/mastering-the-gpt-3-temperature-parameter-with-ruby](https://plainenglish.io/blog/mastering-the-gpt-3-temperature-parameter-with-ruby)

8\. **OpenAI 作者**，《GPT 指南 — 管理令牌》，2023年，OpenAI 文档，[https://platform.openai.com/docs/guides/gpt/managing-tokens](https://platform.openai.com/docs/guides/gpt/managing-tokens)

9\. **普里耶什·帕特尔**，《Node.js 究竟是什么？》，2018年4月，Medium — **自由代码营**，[https://medium.com/free-code-camp/what-exactly-is-node-js-ae36e97449f5](https://medium.com/free-code-camp/what-exactly-is-node-js-ae36e97449f5)

10\. **本·迪克森**，《大型语言模型存在推理问题》，2022年6月，Tech Talks Blog，[https://bdtechtalks.com/2022/06/27/large-language-models-logical-reasoning/](https://bdtechtalks.com/2022/06/27/large-language-models-logical-reasoning/)

11\. **弗兰克·诺伊根鲍尔**，《理解 LLM 幻觉》，2023年5月，Towards Data Science，[https://towardsdatascience.com/llm-hallucinations-ec831dcd7786](/llm-hallucinations-ec831dcd7786)
