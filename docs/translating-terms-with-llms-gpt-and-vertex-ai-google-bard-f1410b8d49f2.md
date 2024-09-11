# 翻译术语与 LLM（GPT 和 Vertex AI/Google Bard）

> 原文：[https://towardsdatascience.com/translating-terms-with-llms-gpt-and-vertex-ai-google-bard-f1410b8d49f2?source=collection_archive---------12-----------------------#2023-09-21](https://towardsdatascience.com/translating-terms-with-llms-gpt-and-vertex-ai-google-bard-f1410b8d49f2?source=collection_archive---------12-----------------------#2023-09-21)

[](https://medium.com/@shafquat?source=post_page-----f1410b8d49f2--------------------------------)[![Shafquat Arefeen](../Images/a52f40380207c523acd2feff7d2c4d4a.png)](https://medium.com/@shafquat?source=post_page-----f1410b8d49f2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f1410b8d49f2--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f1410b8d49f2--------------------------------) [Shafquat Arefeen](https://medium.com/@shafquat?source=post_page-----f1410b8d49f2--------------------------------)

·

[阅读](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F73abef6f209b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftranslating-terms-with-llms-gpt-and-vertex-ai-google-bard-f1410b8d49f2&user=Shafquat+Arefeen&userId=73abef6f209b&source=post_page-73abef6f209b----f1410b8d49f2---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----f1410b8d49f2--------------------------------) ·6分钟阅读·2023年9月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff1410b8d49f2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftranslating-terms-with-llms-gpt-and-vertex-ai-google-bard-f1410b8d49f2&user=Shafquat+Arefeen&userId=73abef6f209b&source=-----f1410b8d49f2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff1410b8d49f2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftranslating-terms-with-llms-gpt-and-vertex-ai-google-bard-f1410b8d49f2&source=-----f1410b8d49f2---------------------bookmark_footer-----------)![](../Images/3d6348c7aafe453c0c1aca222ec00686.png)

照片由 [Mojahid Mottakin](https://unsplash.com/@iammottakin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/qkAUuWW_YHk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供

像 ChatGPT 这样的 LLM 能否比人类更准确地进行翻译？我们可以使用哪些 LLM 选项？了解更多关于使用生成式 AI 进行多语言翻译的信息。

# 背景

在撰写此帖子时，我已经在数据处理领域工作了近十年，并在本地化领域工作了两年。我有各种形式的人工智能经验，包括但不限于聚类、分类和情感分析。机器翻译（MT）在本地化中被广泛使用。可以将其视为将一些文本输入 Google 翻译并请求将其翻译成另一种语言。根据我的经验，机器翻译通常在 80% 的时间内是正确的，但仍需人工审查/修正误译。

随着像 ChatGPT 和 Google Bard 这样的语言模型（LLMs）的兴起，我们可能通过为 LLM 提供额外的上下文（如定义和词性）来更接近人类翻译的准确性。

![](../Images/557569e79fc2091cf7292e696eabcb5b.png)

# 假设

LLMs 通过基于提示的输入工作。这意味着你在提示中提供的信息和上下文越多，LLM 的输出就会越好。给定一组英语术语、它们的定义和词性，我们想看看 LLM 是否能在将术语翻译成不同语言时产生更好的结果。我们将使用的两个 LLM 是 GPT（通过 Jupyter Notebook 和 OpenAI API）和 Vertex AI（通过 Google BigQuery 的 ML.GENERATE_TEXT 函数）。后者需要更多的设置，但可以直接在你的查询控制台中通过 SQL 运行。

# 使用 LLMs 进行翻译

# GPT

我们首先在 Jupyter Notebook 中安装 OpenAI python 库

```py
import sys !{sys.executable} 
-m pip install openai
```

导入 pandas 以处理数据框。导入之前安装的 openai 库并设置你的 API 密钥。将数据读取到数据框中。如果你想跟着操作，我将使用的数据可以在[这里](https://shafquatarefeen.com/wp-content/uploads/2023/09/terms_sample.csv)找到。

```py
import pandas
import openai openai.api_key = "YOUR_API_KEY" 
# read in your data 
df = pd.read_csv('mydata/terms_sample.csv')
```

在列表中设置你希望将单词翻译成的语言。

```py
languages = ['Spanish (Spain)', 'Spanish (LatAm)', 'German', 'Italian', 'Japanese', 'Chinese (S)', 'French'
```

创建一个函数，遍历数据框中的行和语言列表，以分别翻译术语。提示将输入到“消息”部分。我们将使用 GPT 3.5，并将温度设置为一个非常小的数字，以确保我们从 LLM 得到精准/较少创造性的回应。

```py
def translate_terms(row, target_language):
    """Translate the terms"""

    user_message = (
        f"Translate the following english term with the into {target_language}: '" + row['Term'] +
        "'.\n Here is the definition of the term for more context: \n" + row['Definition'] +
        "'.\n Here is the part of speech of the term for more context: \n" + row['Part of speech'] +
        ".\n Please only translate the term and not the definition. Do not provide any additional text besides the translated term." 
    )

    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "system", "content": "You are a helpful translation assistant that translate terms into other languages given an english term and defintion."},
            {"role": "user", "content": user_message},
        ],
        max_tokens=50,
        n=1,
        temperature=0.1
    )

    return response['choices'][0]['message']['content']
```

最后的步骤是对列表中每种语言的数据框运行翻译函数，并为这些语言的术语创建新列。请查阅完整的[代码](https://shafquatarefeen.com/wp-content/uploads/2023/09/gpt-terms.pdf)作为参考。

```py
# Apply the function to the DataFrame
for language in languages:
    df[language+'_terms'] = df.apply(translate_terms, axis=1, args=(language,))
```

![](../Images/0ca63057418f35de4d924084a9458fb2.png)

带有翻译术语的数据框

# Google Bard / text-bisonm / Vertex AI

我们将使用[ML.GENERATE_TEXT](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-generate-text#arguments)函数，在谷歌的 text-bison 模型中翻译相同的术语。这个过程确实需要更多的资源来设置，但一旦运行起来，我们将能够在 SQL 查询中直接调用 LLM。

## 设置

我不会提供详细的设置指南，因为每个人的 Google Cloud 基础设施都是根据他们的需求量身定制的。我将提供一个高层次的概述和一些启动的链接。

1.  确保启用了[Vertex AI 用户角色](https://cloud.google.com/vertex-ai/docs/general/access-control)以访问您的服务账户。

1.  按照[Google Cloud](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-create-remote-model)上的说明设置远程云连接。

1.  [创建一个 LLM 模型](https://cloud.google.com/bigquery/docs/generate-text-tutorial#create_the_remote_model)并进行远程云连接

1.  现在你应该能够使用 ML.GENERATE_TEXT 函数运行你的 LLM 模型。我建议查看[函数的参数](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-generate-text#arguments)以了解所需的参数。

1.  将您的数据上传到计费项目中，以便进行查询。

## 代码

生成翻译所用的代码如下所示。由于我自身的限制和查询引擎的限制，我决定分别为每种语言单独运行代码（手动替换加粗的文本），而不是像在之前的 jupyter notebook 中那样循环遍历语言数组。

```py
DECLARE USER_MESSAGE_PREFIX STRING DEFAULT (
  'You are a helpful translation assistant that translate terms into other languages given an english term and defintion.'|| '\n\n' ||
  'Translate the following english term into French. Please do not translate the definition as well.'|| '\n\n' ||
  'Term: '
);

DECLARE USER_MESSAGE_SUFFIX DEFAULT (
  '\n\n' || 'Here is the definiton of the term for more context: '|| '\n\n'
);

DECLARE USER_MESSAGE_SUFFIX2 DEFAULT (
  '\n\n' || 'Here is the part of speech of the term for more context: '|| '\n\n'
);

DECLARE USER_MESSAGE_SUFFIX3 DEFAULT (
  '\n\n' || 'French translation of term:'
);

DECLARE languages ARRAY<string>;
SET languages = ['Spanish (Spain)', 'Spanish (LatAm)', 'German', 'Italian', 'Japanese', 'Chinese (S)', 'French'];

SELECT
  ml_generate_text_result['predictions'][0]['content'] AS generated_text,
  ml_generate_text_result['predictions'][0]['safetyAttributes']
    AS safety_attributes,
  * EXCEPT (ml_generate_text_result)
FROM
  ML.GENERATE_TEXT(
    MODEL `YOUR_BILLING_PROJECT.YOUR_DATASET.YOUR_LLM`,
    (
      SELECT
        Term,
        Definition,
        USER_MESSAGE_PREFIX || SUBSTRING(TERM, 1, 8192) || USER_MESSAGE_SUFFIX || SUBSTRING(Definition, 1, 8192) || USER_MESSAGE_SUFFIX2 || SUBSTRING(Part_of_speech, 1, 8192) || USER_MESSAGE_SUFFIX3 AS prompt
      FROM
        `YOUR_BILLING_PROJECT.YOUR_DATASET.terms_sample`
    ),
    STRUCT(
      0 AS temperature,
      100 AS max_output_tokens
    )
  );
```

# 结果解释

*说明：除了英语，我不懂这些语言，因此请对我的结论保持一些保留。*

![](../Images/8b4548e8734b00ebeefcbfe9f2348079.png)

GPT 翻译术语

![](../Images/49c75449feeb3da3bbab802a36bdbd96.png)

Vertex AI 翻译术语

结果可以在[这里](https://shafquatarefeen.com/wp-content/uploads/2023/09/ML.GENERATE_TEXT-translations.pdf)找到。我注意到的一些发现：

+   47 / 84（56%）的翻译在两个 LLM 中完全相同。

    - GPT 经常在词尾加上句点 (.)。通过去除这些，匹配百分比增加到**63%**。

+   看起来日语和法语在两个 LLM 之间的翻译不一致。

+   GPT 将术语“make up”理解为化妆品，这令人担忧，因为它似乎在翻译之前没有利用该术语的定义和词性。

    - 这可能是因为我的提示结构不是最优的。例如，我本可以在术语前提供定义，以便 LLM 先处理这些信息。

+   “Heavy metal”（专有名词）似乎被GPT字面翻译，特别是在德语中，它被翻译成一个与音乐流派不相符的术语。

我最终会说这两种LLM各有优缺点。GPT易于在Python中设置和运行，而Vertex AI对提示的理解更清晰，在进行翻译之前会考虑所有上下文。我认为可以公平地说，LLM比普通机器翻译做得更好，因为它们能够处理提示中的额外上下文。告诉我你的想法。我是否能做得更好？

*原文发表于* [*https://shafquatarefeen.com*](https://shafquatarefeen.com/gpt/)*。*
