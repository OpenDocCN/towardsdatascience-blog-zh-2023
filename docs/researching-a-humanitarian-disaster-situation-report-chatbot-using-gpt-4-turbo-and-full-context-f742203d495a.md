# 研究人道主义灾难情况报告聊天机器人 — 使用GPT-4-Turbo和完整上下文提示

> 原文：[https://towardsdatascience.com/researching-a-humanitarian-disaster-situation-report-chatbot-using-gpt-4-turbo-and-full-context-f742203d495a?source=collection_archive---------7-----------------------#2023-11-15](https://towardsdatascience.com/researching-a-humanitarian-disaster-situation-report-chatbot-using-gpt-4-turbo-and-full-context-f742203d495a?source=collection_archive---------7-----------------------#2023-11-15)

[](https://medium.com/@astrobagel?source=post_page-----f742203d495a--------------------------------)[![Matthew Harris](../Images/4fa3264bb8a028633cd8d37093c16214.png)](https://medium.com/@astrobagel?source=post_page-----f742203d495a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f742203d495a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f742203d495a--------------------------------) [Matthew Harris](https://medium.com/@astrobagel?source=post_page-----f742203d495a--------------------------------)

·

[跟进](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4a2cd25b8ff9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fresearching-a-humanitarian-disaster-situation-report-chatbot-using-gpt-4-turbo-and-full-context-f742203d495a&user=Matthew+Harris&userId=4a2cd25b8ff9&source=post_page-4a2cd25b8ff9----f742203d495a---------------------post_header-----------) 发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----f742203d495a--------------------------------) ·16 分钟阅读·Nov 15, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff742203d495a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fresearching-a-humanitarian-disaster-situation-report-chatbot-using-gpt-4-turbo-and-full-context-f742203d495a&user=Matthew+Harris&userId=4a2cd25b8ff9&source=-----f742203d495a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff742203d495a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fresearching-a-humanitarian-disaster-situation-report-chatbot-using-gpt-4-turbo-and-full-context-f742203d495a&source=-----f742203d495a---------------------bookmark_footer-----------)

TL;DR

*在本文中，我们探索了 OpenAI 全新的 GPT-4-Turbo 模型，利用其增加的 128k 令牌上下文窗口来传递完整文档语料库以进行信息检索。这种粗略的蛮力方法——仅在更大的上下文窗口下才可能——实现简单，不需要文档嵌入和语义搜索，如检索增强生成（RAG）中使用的那样。应用于发布在令人惊叹的 ReliefWeb 平台上的人道主义灾难情况报告——使用稀疏引导表示（SPR）进行压缩——我们展示了 GPT-4-Turbo 能够回答有关近期灾难的基本问题。然而，即使在 OpenAI 最近降低了令牌成本的情况下，这种方法仍然代价高昂，提示预览版 GPT-4-Turbo 模型响应非常缓慢，有时需要一分钟才能回复。与所有 LLM 信息检索模式一样，实施验证框架以确保控制虚假信息和信息遗漏当然至关重要。也就是说，GPT-4-Turbo 在能力上迈出了重要一步，尤其是随着性能的提升和成本的下降，它将加入快速扩展的 LLM 工具包。*

![](../Images/b9ce334d1988ebcccc7aa01a3ad3d2d1.png)

新的 GPT-4-Turbo 具有 128k 令牌的更大上下文窗口。图像由 GPT-4 + Dall-E-3 生成。

鉴于过去几个月中，[autogen](https://github.com/microsoft/autogen)、[memgpt](https://memgpt.ai/)、[Semantic Kern](https://github.com/microsoft/semantic-kernel)el，以及[OpenAI 的 GPTs 和 GPT-4-Turbo](https://openai.com/blog/new-models-and-developer-products-announced-at-devday)的发布，发展步伐非常迅速，我想做一系列文章，比较这些新工具提供的一些对话信息检索技术。每种技术都有其优缺点，有些可能会在我们使用大型语言模型（LLMs）的方式上带来潜在的范式转变。这是一个非常令人惊叹的时刻，但将这些新技术应用于实际场景中，并不像初步探索时所建议的那样简单。

# OpenAI 发布 GPT-4-Turbo（预览）

OpenAI 在[DevDay 的近期公告](https://openai.com/blog/new-models-and-developer-products-announced-at-devday)中包括了一些可能改变生成 AI 领域的新功能。其中之一是发布了具有 128k 令牌上下文窗口限制的[GPT-4-Turbo](https://help.openai.com/en/articles/8555510-gpt-4-turbo)（预览），相比之前的 32k 令牌。此前，[Claude AI](https://www.anthropic.com/index/claude-2) 提供了 100k 令牌的最大商业上下文限制，因此 GPT-4-Turbo 在这方面更进一步。此外，OpenAI 还表示，他们的新模型在遵循指令方面更为高效，成本将比 GPT-4 低 3 倍。作为许多 LLM 基准测试的领先者，GPT-4 的任何进展都很重要。

# 增加模型令牌限制以扩展上下文

那么为什么增加的令牌限制很重要呢？在提示 LLM 时，你可以包括过去的对话，所以一个直接的好处是，如果你在每次提示中提供对话历史，它会记住你之前谈论的内容。这对于引用在早期对话中可能现在重要的事实非常有用。更大的上下文窗口意味着你还可以‘预加载’聊天，加入支持信息，比如文档内容和数据。

但也有一个缺点。

更多的令牌意味着更高的成本和更慢的性能，因为使用变换器架构时，内存和计算需求呈平方增加（远快于简单的直线）。另外，有研究表明较长的上下文窗口可能会降低 LLM 的准确性（[刘等，2023](https://arxiv.org/abs/2307.03172)）。

# 使用 GPT-4-Turbo

在写作时，GPT-4-Turbo 仅处于预览模式，仅作为模型 ‘gpt-4–1106-preview’ 提供。调用时我们将使用 openai Python 包，如下 …

```py
import openai

openai.api_key = '<YOUR KEY GOES KEY>'

def run_llm(query, system_prompt, reference_content):

    llm_query = {
        "temperature": 1.0,
        "max_tokens": 2000,
        "top_p": 0.95,
        "frequency_penalty": 0,
        "presence_penalty": 0,
    }

    response = openai.ChatCompletion.create(
        model=model,
        messages=[  {
                "role":"system",
                "content": system_prompt
            },
            {
                "role":"user",
                "content": query
            }
        ],
        temperature=llm_query['temperature'],
        max_tokens=llm_query['max_tokens'],
        top_p=llm_query['top_p'],
        frequency_penalty=llm_query['frequency_penalty'],
        presence_penalty=llm_query['presence_penalty'],
        stop=None
    )  

    answer = response['choices'][0]['message']['content']
    return answer
```

# 分析 ReliefWeb 灾害情况报告

我们将通过使用 GPT-4-Turbo 来分析 [令人惊叹的 ReliefWeb 平台上的人道主义灾害情况报告](https://reliefweb.int/disasters) 来探索其增加的令牌限制。这些报告（称为‘Sitreps’）对于监测和应对全球的人道主义灾害至关重要。它们还提供了一个文本数据语料库，可以被压缩（总结）以适应 GPT-4-Turbo 的上下文窗口，以供我们的分析使用。

# 建立灾害报告语料库

ReliefWeb 有一个 [非常棒的 API](https://reliefweb.int/help/api) 用于访问内容，因此我们将使用它来提取灾害和情况报告的列表 …

```py
import requests  
import os  
from bs4 import BeautifulSoup 
import re
import pandas as pd
import PyPDF2 
import traceback
import json
import ast
from langchain.text_splitter import RecursiveCharacterTextSplitter
import tiktoken

def auto_translate(text):
    """
    This function automatically detects language and translates to english 

    Parameters:
        text(str): The text to be translated

    Returns:
        text (str): Translated text if in another language, otherwise 
                    input text
    """
    try:
        lang = translator.detect(text)
        lang = lang.lang
        print(f"Language detected: {lang}")
        q = translator.translate(text, dest='en')
        text = q.text
    except Exception as e:
        print("An exception occurred trying to translate")
    return text

def get_safe_name(name):
    """
    This function takes a string and returns a version of it that is 
    safe to use as a filename.

    Parameters:
        name (str): The string to be converted to a safe filename.

    Returns:
        name (str): The safe filename.
    """
    name = str(name)
    name = re.sub("[^0-9a-zA-Z]+", "_", name)
    name = re.sub(r"_$","", name)
    if len(name) == 0:
        name = 'Unknown' 
    return name

def download_pdf(url, download_path):  
    """
    Function to download a PDF from a URL and save locally

    Parameters:
        url (str): Location of online PDF file
        download_path (str): Folder where to save PDF

    """
    response = requests.get(url)  
    with open(download_path, 'wb') as f:  
        f.write(response.content)  

def save_text(content, file_path):  
    """
    Function to save text to local file

    Parameters:
        content (str): Text to save
        file_path (str): Folder where to save 
    """
    with open(file_path, 'w') as file:  
        print(f'Saving {file_path}')
        file.write(content)  

def extract_text_from_pdf(pdf_path):  
    """
    Function to extract text from PDF file

    Parameters:
        pdf_path (str): Path to PDF file

    Returns:
        text (str): Text extracted from PDF file
    """
    print(pdf_path)
    pdf_reader = PyPDF2.PdfReader(pdf_path)  
    text = ''  
    for page_num in range(len(pdf_reader.pages)):  
        page_obj = pdf_reader.pages[page_num]
        text += page_obj.extract_text()  
    return text  

def get_rw_data(keyword, filter, sort, fields, endpoint, limit=10, \
                save_body_to_text=False):  
    """
    Function to extract data from ReliefWeb API. For API details see:

    https://apidoc.rwlabs.org/?utm_medium=blog&utm_source=reliefweb+website&utm_campaign=api+doc+launching+2016_06

    Parameters:
        keyword (str): Search string
        filter (dict): ReliefWeb filter json
        sort (dict): ReliefWeb sort json
        fields (list): List of fields to return
        endpoint (str): API Endpoint, eg reports, disasters
        limit (int): Maximum records to return
        save_body_to_text (bool) : Flag to save body to text file, including any PDFs on page

    Returns:
        all_data (pandas dataframe): Dataframe of data from API
    """
    query = {  
        "appname": "myapp",  
        "query": {  
            "value": keyword
        },  
        "filter":filter,
        "sort": sort,
        "limit": limit,  
        "fields": fields
    }  
    endpoint = f"{reliefweb_api_url}/{endpoint}?appname=apidoc&query[value]="
    print(f"Getting {endpoint} ...")

    all_data =[]
    response = requests.post(endpoint, json=query)  
    if response.status_code == 200:  
        data = response.json()  
        for article in data["data"]: 
            article_url = article['fields']['url']   
            try:
                r = article['fields']
                print(article_url)
                article_response = requests.get(article_url)  
                if save_body_to_text:
                    soup = BeautifulSoup(article_response.text, 'html.parser')  
                    main_content = [p.text for p in soup.find_all('p')]  
                    article_text = ' '.join(main_content)
                    save_text(article_text, docs_folder + '/{}.txt'.format(get_safe_name(article['fields']['title'])))  
                    for link in soup.find_all('a'):  
                        href = link.get('href')  
                        if href.endswith('.pdf'):  
                            download_path = os.path.join(docs_folder, href.split('/')[-1])  
                            if href.startswith('/attachments'):
                                pdf_url = f'{reliefweb_pdf_url}{href}'
                            else:
                                pdf_url = href
                            download_pdf(pdf_url, download_path)  
                            print(f".    Downloaded PDF {download_path} from {pdf_url}")
                            article_text = extract_text_from_pdf(download_path)
                    r['article_text'] = article_text
                    r['reliefweb_query'] = keyword
                all_data.append(r)
            except Exception as e:
                print(f"An exception occurred trying to extract {article_url}")
                tb_str = ''.join(traceback.format_exception(None, e, e.__traceback__))
                print(tb_str)

        all_data = pd.DataFrame(all_data)
        for f in ['disaster','theme']:
            if f in list(all_data.columns):
                all_data[f] = all_data[f].astype(str)
        return all_data  
    else:  
        print(f"Request failed with status {response.status_code} {response.text}")  
        return None 
```

在上述函数中，一些关注点 …

1.  如果 ReliefWeb 内容提到 PDF，我们会从中提取文本

1.  任何文本都会通过 Google Translate API 自动翻译成英语

1.  我们总是捕捉来源以便归属

1.  对于这次快速分析，我们没有处理 API 响应的分页

这是我们调用函数以获取自 2023 年 11 月 1 日以来的灾害情况报告的方法 …

```py
filter = {
    "operator": "AND",
    "conditions": [
        {
            "field": "disaster.status",
            "value": "ongoing"
        },
        {
            "field": "format.name",
            "value": "Situation Report"
        },
        {
            "field": "date.created",
            "value": {
                "from": "2023-11-01T00:00:00+00:00",
                "to": "2023-11-30T23:59:59+00:00"
            }
        }
    ]
}
sort = ["date.created:desc"]
endpoint = "reports"
fields = {  
    "include": ["title", "body", "url", "source", "date", "format", "theme", 
    "country", "status", "primary_country", "disaster", "language", "id"] 
}  
reliefweb_query = ""
articles = get_rw_data(reliefweb_query, filter, sort, fields, endpoint, 1000, True)
```

情况报告被保存到文件系统作为文本文件 …

```py
for index, row in articles.iterrows():
    date = row['date']['created']
    source = row['source'][0]['name']
    title = row['title']
    id = row['id']
    filename = f'{get_safe_name(title)}__{id}_{get_safe_name(date)}.txt'

    text = f'''
        title: {title}
        source: {source}
        date: {date}
        id: {id}

        {row['article_text']}
    '''
    text = text.encode('utf-8','ignore').decode('utf-8','ignore')
    print(text)

    save_text(text, docs_folder + '/' + filename) 
```

# 使用稀疏引导表示 (SPR) 压缩内容

我们可以直接用原始文本从情况报告中提示 GPT-4-Turbo。然而，那些文本中很多都是不相关的——如停用词、标题和页脚——因此包含所有这些内容会很快超过 GPT-4-Turbo 增加后的令牌限制。我们将使用一种叫做 [稀疏引导表示 (SPR)](https://github.com/daveshap/SparsePrimingRepresentations) 的技术来压缩文档到其关键事实，并稍作修改以试图保留定量信息。

这是我们将提供给 GPT-4-Turbo 的系统提示，用于压缩我们的文档 …

```py
# MISSION
You are a Sparse Priming Representation (SPR) writer. An SPR is a 
particular kind of use of language for advanced NLP, NLU, and NLG 
tasks, particularly useful for the latest generation of Large Language 
Models (LLMs). You will be given information by the USER which you 
are to render as an SPR.

# THEORY
LLMs are a kind of deep neural network. They have been demonstrated 
to embed knowledge, abilities, and concepts, ranging from reasoning 
to planning, and even to theory of mind. These are called latent abilities and latent content, collectively referred to as latent space. The latent space of an LLM can be activated with the correct series of words as inputs, which will create a useful internal state of the neural network. This is not unlike how the right shorthand cues can prime a human mind to think in a certain way. Like human minds, LLMs are associative, meaning you only need to use the correct associations to "prime" another model to think in the same way. The exception are quantitative metrics, which you MUST preserve.

# METHODOLOGY
Render the input as a distilled list of succinct statements, 
assertions, associations, concepts, analogies, and metaphors. 
The idea is to capture as much, conceptually, as possible but with 
as few words as possible while preserving all quantitative details. 
Write it in a way that makes sense to you, as the future audience 
will be another language model, not a human.
```

这将把原始文本状态报告总结成类似于标题为“非洲脊髓灰质炎实验室网络简报（第 1–42 周，2023 年）”的情况报告 …

```py
- Information comes from World Health Organization. 
- Weekly dengue cases decreasing, 12,634 reported this week. 
- 78 dengue-related deaths this week. 
- Cumulative cases since January 2023: 267,680, including 1,333 deaths. 
- Cases decreasing in all divisions except Sylhet. 
- Increase in dengue cases in Faridpur, Khulna, Rajshahi, Rajbari districts. 
- WHO orientation session for entomologists in Dhaka occurred on 26 October. 
- Overall Case Fatality Rate (CFR) is 0.5%, but 0.62% for this week. 
- Dhaka division reports highest number of cases and deaths. 
- In this week, 13,430 people released from hospital after treatment. 
- Male-to-female death ratio: 43% vs 57%. 
- Most deaths among 16-55 years age group. 
- Four dengue virus types circulating: DENV-1 (2.2%), DENV-2 (68.1%), DENV-3 (25.4%), DENV-4 (0.2%). 
- Dengue prevention & clean-up week launched nationwide from 29 October to 4 November. 
- WHO expert entomologist deployed to Bangladesh for technical support in outbreak response. 
- In the Rohingya camps, weekly dengue cases continue to decline. Total cumulative cases are 12,969 including 17 deaths (CFR: 0.1%).
```

当然，这比原始文件要少得多。

我不建议在没有进行重要分析和检查以控制信息遗漏的情况下使用这种压缩技术，但对于我们的测试来说，这足够了。

这里是用于压缩报告的代码……

```py
# gpt4 turbo is 128k 
chunk_size = 100000

llm_query = {
    "prompt": "",
    "temperature": 1.0,
    "max_tokens": 2000,
    "top_p": 0.95,
    "frequency_penalty": 0,
    "presence_penalty": 0,
    "system_prompt":"""
        # MISSION
        You are a Sparse Priming Representation (SPR) writer. An SPR is a 
        particular kind of use of language for advanced NLP, NLU, and NLG 
        tasks, particularly useful for the latest generation of Large Language 
        Models (LLMs). You will be given information by the USER which you 
        are to render as an SPR.

        # THEORY
        LLMs are a kind of deep neural network. They have been demonstrated 
        to embed knowledge, abilities, and concepts, ranging from reasoning 
        to planning, and even to theory of mind. These are called latent abilities and latent content, collectively referred to as latent space. The latent space of an LLM can be activated with the correct series of words as inputs, which will create a useful internal state of the neural network. This is not unlike how the right shorthand cues can prime a human mind to think in a certain way. Like human minds, LLMs are associative, meaning you only need to use the correct associations to "prime" another model to think in the same way. The exception are quantitative metrics, which you MUST preserve.

        # METHODOLOGY
        Render the input as a distilled list of succinct statements, 
        assertions, associations, concepts, analogies, and metaphors. 
        The idea is to capture as much, conceptually, as possible but with 
        as few words as possible while preserving all quantitative details. 
        Write it in a way that makes sense to you, as the future audience 
        will be another language model, not a human.

    """
}

# Save texts
for index, row in articles.iterrows():
    date = row['date']['created']
    source = row['source'][0]['name']
    report = row['title']
    id = row['id']
    text = row['article_text']
    primary_country = row['primary_country']['name']
    disaster = ''
    disaster_types = ''
    for d in ast.literal_eval(row['disaster']):
        disaster += f"{d['name']}; "
        for t in d['type']:
            if 'primary' in t and t['primary'] == True:
                disaster_types += f"{t['name']}; "
    d = {
        "disaster": disaster,
        "date": date,
        "disaster_types": disaster_types
    }
    prefix = ""
    filename = f'{get_safe_name(report)}__{id}_{get_safe_name(date)}.txt'
    header = f'- report: "{report}"\n- disaster: "{disaster}"\n' + \
             f'- disaster_types: "{disaster_types}"\n' + \
             f'- primary_country: "{primary_country}"\n- source: "{source}"\n' + \
             f'- date: "{date}"\n- id: "{id}"\n'

    text_splitter = RecursiveCharacterTextSplitter.from_tiktoken_encoder(
        chunk_size=chunk_size, chunk_overlap=100
    )
    texts = text_splitter.split_text(text)
    print(f"\n\n================ {report} =================\n")
    print(primary_country)
    print(disaster)
    print(len(texts))

    summarized_text = ''

    for i, t in enumerate(texts):
        response = openai.ChatCompletion.create(
        model=model,
        messages=[  {
                "role":"system",
                "content":llm_query['system_prompt']
            },
            {
                "role":"user",
                "content":t
            }
        ],
        temperature=llm_query['temperature'],
        max_tokens=llm_query['max_tokens'],
        top_p=llm_query['top_p'],
        frequency_penalty=llm_query['frequency_penalty'],
        presence_penalty=llm_query['presence_penalty'],
        stop=None)  

        summary = response['choices'][0]['message']['content']
        summarized_text += "\n" + summary

    summarized_text = auto_translate(summarized_text)
    summarized_text = header + summarized_text

    summarized_text = summarized_text.split("\n")
    summarized_text_prefixed = ''
    for s in summarized_text:
        summarized_text_prefixed += f"{prefix}{s}\n"

    print(summarized_text_prefixed)
    save_text(summarized_text_prefixed, docs_folder2 + '/' + filename)
```

您会注意到在上述内容中，我们添加了关于报告的一些元数据，以及由GPT-4-Turbo返回的SPR摘要。然后将压缩的报告保存为文本文件。

# 提取一个灾害的高级列表

我们还将从ReliefWeb中提取一份灾害的高级列表，用作我们系统提示的信息请求辅助……

```py
filter = {
    "operator": "AND",
    "conditions": [
        {
            "field": "status",
            "value": "ongoing"
        },
        {
            "field": "date.event",
            "value": {
                "from": "2020-01-01T00:00:00+00:00",
                "to": "2023-11-30T23:59:59+00:00"
            }
        }
    ]
}
sort = ["date.event:desc"]
endpoint = "disasters"
fields = {  
    "include": ["name", "description", "date", "url", "id","status","glide"] 
} 
reliefweb_query = ""
disasters = get_rw_data(reliefweb_query, filter, sort, fields, endpoint, 1000, False)
display(disasters)
disasters.to_csv('disasters.csv')
```

这给我们提供了一个简明的列表……

![](../Images/008e025214645e66b0e5c853c919fa92.png)

使用ReleiefWeb API灾难终端提取的灾难列表

# 为GPT-4-Turbo创建一个提示

现在我们有一份灾难和压缩情况报告的列表 — 从11月1日到11月10日 — 列出了这些灾难的关键事实。

让我们将它们合并成一个文本文件，作为GPT-4-Turbo系统提示的一部分使用……

```py
disasters = pd.read_csv('disasters.csv')
concatenated_content = "=========== this section gives a list of DISASTERS =========== \n\n "+ disasters.to_csv() 
concatenated_content += "\n\n=========== this section provides disater REPORTS for each disaster =========== "
for f in os.listdir(docs_folder2):
  with open(f"{docs_folder2}/{f}", "r") as file:
      file_content = file.read()
      concatenated_content += f"\n\n----- report: {f} ----- \n\n"
      concatenated_content += file_content + "\n\n"
```

有多少令牌和成本是多少？

```py
def num_tokens_from_string(string: str, encoding_name: str) -> int:
    encoding = tiktoken.get_encoding(encoding_name)
    num_tokens = len(encoding.encode(string))
    gpt4_token_cost_per_1000 = 0.01
    cost = (num_tokens/1000.0)*gpt4_token_cost_per_1000
    return num_tokens, cost

tokens, cost = num_tokens_from_string(concatenated_content,"cl100k_base")

OpenAI Tokens: 82001 ($0.82001)
```

因此，鉴于每1,000个令牌输入的成本为[$0.01](https://openai.com/pricing)，上面创建的文本每次提示的成本为$0.82。还有一些完成令牌成本，每1,000个令牌为$0.03，但由于令牌较少，这应该远低于输入成本。

哎呀！

我们知道这种蛮力技术并不是解决我们特定任务的最佳方式，但高成本是另一个原因。

现在我们有了文本，我们可以构建一个系统提示……

```py
def run_llm(query, reference_content):

    llm_query = {
        "temperature": 1.0,
        "max_tokens": 2000,
        "top_p": 0.95,
        "frequency_penalty": 0,
        "presence_penalty": 0,
    }

    response = openai.ChatCompletion.create(
        model=model,
        messages=[  {
                "role":"system",
                "content": f"""You are a ReliefWeb disasters bot. You 
                provide information on diasters. 

                If asked for a list of disasters, just provide the list 
                ONLY using the 'Disasters' section below. 

                If asked for details of a disaster, extract that information 
                from 'Situation Reports' section below.   

                {reference_content}"""
            },
            {
                "role":"user",
                "content": query
            }
        ],
        temperature=llm_query['temperature'],
        max_tokens=llm_query['max_tokens'],
        top_p=llm_query['top_p'],
        frequency_penalty=llm_query['frequency_penalty'],
        presence_penalty=llm_query['presence_penalty'],
        stop=None
    )  

    answer = response['choices'][0]['message']['content']
    return answer
```

您会注意到系统提示包括有关在连接文本中使用不同部分的说明。通过一些快速的提示实验 — 我真的不会称之为“工程” — 这样可以产生更好的结果。

# 测试我们的人道灾难情况报告机器人

好的，开始……

```py
answer = run_llm("What's the latest status regaring ther Nepalese earthquake 
this year?", concatenated_content)
print(answer)
```

```py
The latest status regarding the Nepal earthquake as of November 2023 is as 
follows:

- Earthquake: Western Nepal, 6.4 magnitude, occurred on 3 November 2023, 
with an epicenter in Ramidanda, Jajarkot.

- Casualties: 154 deaths, 364+ injuries.

- Displacement: Approximately 250,000 people affected, with significant 
numbers of homes damaged or destroyed.

- Infrastructure: Over 62,000 houses across 13 districts sustained 
damage (26,557 fully, 35,455 partially). Several government buildings were also damaged.

- Humanitarian Response: The United Nations and other humanitarian 
organizations have been active in relief efforts, providing medical tents, child-friendly spaces, WASH supplies, and psychosocial support, among other assistance.

- Recovery and Rehabilitation: The recovery and rehabilitation phase is 
in planning, with international reinforcements arriving to aid in coordination and strategy development.

- Education and Health Services: Education has been disrupted for 
about 80,000 children due to damaged schools, and health services 
have been impacted, increasing the risk of malnutrition and complicating maternal and newborn care.

Please note that this is a condensed summary of the available data up until 
10 November 2023, and further updates may follow as the situation evolves 
and more assessments are made.
```

总的来说，这是对灾难的一个相当不错的摘要。事实与[ReliefWeb灾难页面](https://reliefweb.int/disaster/eq-2023-000214-npl)（请注意，自这篇文章以来，此页面可能会更改，因为灾难正在发展）和用于本研究的[最新情况报告](https://reliefweb.int/report/nepal/nepal-western-nepal-earthquake-2023-situation-report-no-02-10-november-2023)很好地一致。

不过，有一个事实略微不一致，数据显示，不幸的是有153人死亡，而模型返回了154人。

```py
answer = run_llm("How many people died in the Nepal earthquake in 2023 occur?", 
concatenated_content)
print(answer)

In the Nepal Earthquake of November 2023, there were 153 fatalities.
```

因此，在第一个提示中，“已知”但在第一个提示中产生了幻觉。与LLM的所有应用一样，验证和检查对于捕捉此类问题至关重要。

让我们获取一些关于创建情况报告的组织的信息（用于11月1日至10日的期间）……

```py
answer = run_llm("List the organizations providing sitreps for the Nepal 
Earthquake", concatenated_content)
print(answer)

The organizations providing situation reports (sitreps) for the Nepal Earthquake include:

- UN Office for the Coordination of Humanitarian Affairs (OCHA)
- Nepal Red Cross Society
- International Federation of Red Cross And Red Crescent Societies (IFRC)
- UN Resident Coordinator in Nepal
- World Health Organization (WHO)
- UN Children's Fund (UNICEF)
- UN Country Team in Nepal
```

相比于[同一查询在ReliefWeb中的结果](https://reliefweb.int/updates?advanced-search=%28D51797%29_%28F10%29_%28DA20231101-20231110%29)，这个看起来是正确的。

让我们问一些更细致的问题……

```py
answer = run_llm("How many displaced people are children?", 
concatenated_content)
print(answer)

In the provided data, there are references to children affected by disasters 
in different reports, but the specific number of displaced children is not 
directly mentioned in any of the excerpts. It would be necessary to review 
more detailed reports or data sources that focus specifically on displacement 
figures for children to determine the exact number affected by displacement.
```

查看情况报告时，我也未能找到这些信息。不过需要进行更详细的反查来确认这一点是否正确。

让我们探讨一下潜在的疾病和健康影响……

```py
answer = run_llm("What disease risks are there for the Nepal 2023 earthquake?", 
concatenated_content)
print(answer)

The disease risks for the Nepal 2023 earthquake, as reported by UNICEF in 
their Humanitarian Situation Report No. 2, are primarily focused on 
malnutrition, particularly among children under five years old, and 
psychosocial stress affecting the affected population. The disruption of 
healthcare and WASH (Water, Sanitation, and Hygiene) services due to 
the earthquake has further exacerbated the health risks.

The key disease risks and health concerns include:

1\. Malnutrition: The earthquake and its aftermath have disrupted the food 
supply chain, which can lead to an increased risk of malnutrition among 
vulnerable populations, especially children.

2\. Maternal and Newborn Health: Pregnant women and newborns might face 
increased health risks due to the disruption of health care services and 
a lack of access to essential medical care and facilities.

3\. Psychosocial Stress: The trauma caused by the earthquake, the loss of 
loved ones, and the displacement can lead to significant psychosocial 
stress and mental health issues.

4\. Injuries and Physical Health: With many injured in the aftermath, 
there is an increased need for medical treatment for physical injuries 
and trauma care. 

5\. Communicable Diseases: Overcrowding in temporary shelters can lead 
to the spread of communicable diseases, particularly in the absence of 
proper sanitation and hygiene facilities.

6\. Waterborne and Vector-borne Diseases: Lack of clean water and 
sanitation increases the risk of waterborne diseases like diarrhea, 
cholera, and dysentery. There is also a concern for vector-borne 
diseases such as malaria and dengue fever.

7\. Non-communicable Diseases: Disruption of routine health services 
can affect the management and treatment of chronic non-communicable diseases.

8\. Disruption of Routine Immunization: The potential disruption of 
routine immunization services can increase the risk of vaccine-preventable 
diseases.

In response to these health risks, UNICEF and other health sector partners 
have been providing health services, medical supplies, and psychosocial 
support, as well as ensuring continued access to nutrition for children 
and mothers.
```

从数据中的 [UNICEF 报告](https://reliefweb.int/report/nepal/unicef-nepal-humanitarian-situation-report-no-1-earthquake-08-november-2023)来看，即使通过我们的 SPF 摘要，上述内容似乎捕捉到了主要观点。然而，我们这里使用的数据只有报告第 1 号，但上述内容提到了报告第 2 号。这是另一个小的错误，再次表明任何 LLM 的回应都需要自动回查。

# 结论

这是一个非常快速的分析，旨在了解 GPT-4-Turbo — 利用其增加的 128k 上下文窗口和改进的指令跟随能力 — 在包括所有信息检索所需的文本数据作为系统提示时的表现，且没有其他处理。这种蛮力方法是否是我们任务的最佳技术？

可能不是，至少目前还不是。

性能是一个严重的考虑因素。提示处理时间为 30 秒或更长，这对用户体验来说不太理想。

成本也是一个限制因素。每次提示需要更多的令牌来提供完整语料库，这增加了成本——在这次分析中，每次提示需要 $0.82！——因此，其他技术无疑对许多组织更具吸引力，至少在 LLM 成本高昂的情况下。不过，我想起了 [存储成本的历史](https://ourworldindata.org/grapher/historical-cost-of-computer-memory-and-storage)，也许我们会看到 LLM 成本的相同下降。

生成代码以查询数据、使用注册到 LLM 的函数和多代理验证结果等替代方案可能提供更便宜和更准确的选择。它们还将消除为了适应上下文窗口而压缩文档的需要，从而避免信息丢失。

也就是说，我们能够展示，提示 GPT-4-Turbo 可以支持在系统提示中提供的压缩文档语料库上的基本信息检索。这具有非常直接的实施优点——你只需将所有内容提供给语言模型并提问。随着 AI 的进步和成本的降低，这在未来可能会成为一种非常普遍的技术。
