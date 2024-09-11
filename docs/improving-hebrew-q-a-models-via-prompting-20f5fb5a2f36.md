# 通过智能提示改进希伯来语问答模型

> 原文：[https://towardsdatascience.com/improving-hebrew-q-a-models-via-prompting-20f5fb5a2f36?source=collection_archive---------8-----------------------#2023-03-17](https://towardsdatascience.com/improving-hebrew-q-a-models-via-prompting-20f5fb5a2f36?source=collection_archive---------8-----------------------#2023-03-17)

[](https://medium.com/@erap129?source=post_page-----20f5fb5a2f36--------------------------------)[![Elad Rapaport](../Images/2ad958f92cd8b5735da900ae8f5559f3.png)](https://medium.com/@erap129?source=post_page-----20f5fb5a2f36--------------------------------)[](https://towardsdatascience.com/?source=post_page-----20f5fb5a2f36--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----20f5fb5a2f36--------------------------------) [Elad Rapaport](https://medium.com/@erap129?source=post_page-----20f5fb5a2f36--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd2d1ff8f0490&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimproving-hebrew-q-a-models-via-prompting-20f5fb5a2f36&user=Elad+Rapaport&userId=d2d1ff8f0490&source=post_page-d2d1ff8f0490----20f5fb5a2f36---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----20f5fb5a2f36--------------------------------) ·13分钟阅读·2023年3月17日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F20f5fb5a2f36&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimproving-hebrew-q-a-models-via-prompting-20f5fb5a2f36&source=-----20f5fb5a2f36---------------------bookmark_footer-----------)![](../Images/460a2dc30eb62d6e636e117e21cf5c2f.png)

DALL-E 2: “一个穿着长袍的机器人用希伯来语书写圣经句子。数字艺术。”

大家好，

我分享了一个我参与的短期项目，涉及通过智能提示提高 `text-davinci-003` 模型的性能。我首先要说的是，这项工作受到 [James Briggs](https://www.youtube.com/@jamesbriggs/featured) 的优秀视频教程的启发——特别是这一个 — [https://youtu.be/dRUIGgNBvVk](https://youtu.be/dRUIGgNBvVk)，我使用的许多代码也取自他的示例。

本文的提纲如下：

1.  简介 — 大型语言模型（LLMs）中的独特和专有数据

1.  问题陈述 — 用希伯来语回答问题

1.  解决方案建议 — 提示与查询

1.  实验 — 希伯来语问答

1.  结论

完整代码的 Google Colab 笔记本在这里 — [https://colab.research.google.com/drive/1_UqPHGPW1yLf3O_BOySRjf3bWqMe6A4H#scrollTo=4DY7XgilIr-H](https://colab.research.google.com/drive/1_UqPHGPW1yLf3O_BOySRjf3bWqMe6A4H#scrollTo=4DY7XgilIr-H)

# 第1部分 问题陈述 — 大型语言模型中的独特和专有数据

以色列的主要官方语言是希伯来语，作为以色列人，我有兴趣提高大型语言模型在希伯来语中的表现。虽然近期有所改善，但众所周知，ChatGPT 在希伯来语中的表现相较于其在英语中的表现仍有待提高。本文中，希伯来语只是代表那些在大型语言模型中被低估的数据来源，本文将探讨如何尝试帮助 LLM 提供基于这些数据的有用结果。

在非常大型语言模型出现之前，常见的一个选择是对模型进行额外的训练，加入新的数据/任务。当模型的参数规模达到百万级时，这种方法对许多人和用例来说是可行的，并且可以使用常见的消费级硬件来实现。

在过去几年中，大型语言模型的规模爆炸性增长，如今已经达到数百亿个参数。另一方面，消费级硬件并没有出现如此大规模的效率提升。因此，针对这些模型的微调选项已经被大多数用户和组织排除在外。

ChatGPT 已经广泛为人所知，在许多任务上表现出色，证明了它是一个不可或缺的 AI 助手。然而，如果我希望利用这项技术来帮助说稀有语言的人，或帮助处理公司内部的专有数据（这些数据在线上不可用），那么 ChatGPT 很可能因为之前未见过这些数据而失败。那么我们如何在这些模型中利用特殊/专有数据，而不需要花费数百万美元来使用特殊硬件进行训练呢？一个可行的选择是通过查询提示，这一点我们今天将进行探索。

# **第2部分** 问题陈述 — 希伯来语中的问答

我们将使用 OpenAI API 来回答三个希伯来语问题，这些问题的灵感来自于[SQuAD](https://rajpurkar.github.io/SQuAD-explorer/explore/v2.0/dev/)数据集，并翻译成希伯来语。以下代码片段展示了这些问题、预期的答案和英文翻译。

```py
questions_answers = [
     {'subject': 'אוסטרליה',
      'subject_translation': 'Australia',
      'question': "איזו עיר בויקטוריה נחשבת לבירת הספורט של אוסטרליה?",
      'answer': "מלבורן",
      'question_translation': "Which city in Victoria is considered the sporting capital of Australia?",
      'answer_translation': "Melbourne"},
     {'subject': 'אוניברסיטה',
      'subject_translation': 'University',
      'question': "איזה נהר ממוקם בקרבת אוניברסיטת הארווארד?",
      'answer': "נהר צ'ארלס",
      'question_translation': "Which river is located in the vicinity of Harvard University?",
      'answer_translation': "Charles River"},
     {'subject': 'המוות השחור',
      'subject_translation': 'The black plague',
      'question': "כמה אנשים ברחבי העולם נספו בעקבות המוות השחור?",
      'answer': "בין 75 ל200 מיליון איש",
      'question_translation': "How many people in the world died from the black plague?",
      'answer_translation': "Between 75 and 200 million"},
]
```

**免责声明 — 最新版本的 ChatGPT 对这些查询（希伯来语）的回答表现非常好。本文的重点不是“竞争”，而是展示一个可能在其他模型/数据中出现的问题的解决方案。**

我们将使用 `text-davinci-003` 模型来回答这些问题，使用的代码将调用 OpenAI API：

```py
from googletrans import Translator
import openai

def complete(prompt):
    # query text-davinci-003
    res = openai.Completion.create(
        engine='text-davinci-003',
        prompt=prompt,
        temperature=0,
        max_tokens=400,
        top_p=1,
        frequency_penalty=0,
        presence_penalty=0,
        stop=None
    )
    return res['choices'][0]['text'].strip()

def print_question_answer(question, query, expected_answer, question_translation, expected_answer_translation):
  translator = Translator()
  answer = complete(query)
  print(f'Question: {question}\n' \
        f'Question (translation): {question_translation}\n' \
        f'Answer: {answer}\n' \
        f'Answer (translation): {translator.translate(answer).text}\n' \
        f'Expected Answer: {expected_answer}\n' \
        f'Expected Answer (translation): {expected_answer_translation}'
        )

for i, qa_dct in enumerate(questions_answers):
  print(f'####################')
  print(f'Question #{i + 1}:')
  print_question_answer(qa_dct['question'],
                        qa_dct['question'],
                        qa_dct['answer'],
                        qa_dct['question_translation'],
                        qa_dct['answer_translation'],
                        )
```

输出：

```py
####################
Question #1:
Question: איזו עיר בויקטוריה נחשבת לבירת הספורט של אוסטרליה?
Question (translation): Which city in Victoria is considered the sporting capital of Australia?
Answer: סידני.
Answer (translation): Sydney.
Expected Answer: מלבורן
Expected Answer (translation): Melbourne
####################
Question #2:
Question: איזה נהר ממוקם בקרבת אוניברסיטת הארווארד?
Question (translation): Which river is located in the vicinity of Harvard University?
Answer: הנהר הקרוב ביותר לאוניברסיטת הארווארד הוא נהר הקונגו.
Answer (translation): The closest river to Harvard University is the Congo River.
Expected Answer: נהר צ'ארלס
Expected Answer (translation): Charles River
####################
Question #3:
Question: כמה אנשים ברחבי העולם נספו בעקבות המוות השחור?
Question (translation): How many people in the world died from the black plague?
Answer: בעקבות המוות השחור של ג'ורג' פלאי, הפגנו אלפי אנשים ברחבי העולם בכדי להגיב לפעולות הפולישיות של המשטרה האמריקאית. אלפי אנשים הפגינו בערים בכל הארצות האמריקאיות, כמו ג'קסוןביל, ניו יורק, לוס אנג'לס, פילדלפיה, אתרוג, אוסטין, סנט לואיס, אוסטין, אינדיאנה, מישיגן, אורלנדו, קליפורניה, אוהיו, אריזונה, אוקלהומה, ארצות הברית החדשות ועוד. אף אנשים
Answer (translation): Following the black death of George Flay, thousands of people demonstrated around the world to respond to the police actions of the American police. Thousands of people demonstrated in cities in all American countries, such as Jacksonville, New York, Los Angeles, Philadelphia, Etrog, Austin, St. Louis, Austin, Indiana, Michigan, Orlando, California, Ohio, Arizona, Oklahoma, New United States and more . no people
Expected Answer: בין 75 ל200 מיליון איש
Expected Answer (translation): Between 75 and 200 million
```

我们看到 `text-davinci-003` 在默认情况下回答这些问题的表现非常差。它把所有三个问题都答错了，并且对第二和第三个问题产生了一些荒谬的答案。

所以问题来了——我们如何解决这个问题？我们如何从 `text-davinci-003` 获得良好的答案，而不必在一堆希伯来数据上对其进行微调？

# 第三部分\. 解决方案建议——提示与查询

正如我在介绍中写的那样，向 LLM 添加信息的一种方法是提示，即在模型的输入中包含相关信息。但为每个查询包含相关信息可能是一个非常繁琐且耗时的任务，我们能否对此进行自动化？

当然，我们可以！通过查询来进行提示。我们将使用希伯来语的维基百科的一个较小的快照，这个快照中肯定包含了我们问题的答案。我们将把这个快照分割成多行，使用 OpenAI 的嵌入模型对每一行进行嵌入，并将这些嵌入插入到一个名为 Pinecone DB 的向量数据库中。然后，对于我们提出的每一个问题，我们将搜索向量数据库中与问题语义相似的信息片段，并将这些片段附加到模型输入中作为附加信息。这个过程如图 1 所示。希望模型能够利用提供的上下文，并给出我们问题的正确答案。

![](../Images/37163f1f594e22773dcc5dfc74fb4106.png)

图 1\. 通过查询进行提示

受到 Jason Briggs 的启发，我将展示一个提示模板，这将帮助我们生成可理解的提示给 `text-davinci-003`，并希望使其正确回答我们之前的三个问题。

# 第四部分\. 实验——希伯来语问答

首先——我们需要用相关信息填充我们的向量数据库。为此，我们将使用一个从这里下载的小型希伯来语维基百科快照——[https://u.cs.biu.ac.il/~yogo/hebwiki/](https://u.cs.biu.ac.il/~yogo/hebwiki/)（在 Creative Commons Attribution-ShareAlike 4.0 International License 下提供）。

```py
with open('/content/drive/MyDrive/Datasets/hebrew_wikipedia/full.txt') as f:
  hebrew_wiki = f.readlines()

openai_price_per_1k = 0.00004
n_tokens = sum([len(x) for x in hebrew_wiki])
print(f'This hebrew wikipedia dataset contains {len(hebrew_wiki)} sentences')
print(f'In total it amounts to {n_tokens} tokens')
print(f'With the current pricing of openAI it will cost {(openai_price_per_1k * n_tokens) / 1000:.3f}$ to embed everything')
```

```py
This hebrew wikipedia dataset contains 3833140 sentences
In total it amounts to 380692471 tokens
With the current pricing of openAI it will cost 15.228$ to embed everything
```

对于一般信息，我计算了一下使用 OpenAI API 嵌入整个维基百科快照的费用——还不错！但为了我们的目的，我们只需要特定的主题，所以我们可以通过选择更具体的句子来节省一些费用：

```py
def get_lines_per_subject(hebrew_wiki, subject, subject_translation):
  subject_lines = [(i, x) for i, x in enumerate(hebrew_wiki) if subject in x]
  n_tokens = sum([len(x[1]) for x in subject_lines])
  print(f'There are {len(subject_lines)} sentences that have the words {subject_translation} in them')
  print(f'In total it amounts to {n_tokens} tokens')
  print(f'With the current pricing of openAI it will cost {(openai_price_per_1k * n_tokens) / 1000:.3f}$ to embed everything')
  return subject_lines

subject_lines = {}
for qa_dct in questions_answers:
  subject_lines[qa_dct['subject']] = get_lines_per_subject(hebrew_wiki, qa_dct['subject'], qa_dct['subject_translation'])
```

```py
There are 7772 sentences that have the words Australia in them
In total it amounts to 1026137 tokens
With the current pricing of openAI it will cost 0.041$ to embed everything
There are 15908 sentences that have the words University in them
In total it amounts to 1904404 tokens
With the current pricing of openAI it will cost 0.076$ to embed everything
There are 160 sentences that have the words The black plague in them
In total it amounts to 19410 tokens
With the current pricing of openAI it will cost 0.001$ to embed everything
```

好一些了。我们现在准备填充向量数据库：

```py
import pinecone

index_name = 'hebrew-wikipedia'

# initialize connection to pinecone (get API key at app.pinecone.io)
pinecone.init(
    api_key=pinecone_key,
    environment="us-east1-gcp"
)

# check if index already exists (it shouldn't if this is first time)
if index_name not in pinecone.list_indexes():
    # if does not exist, create index
    pinecone.create_index(
        index_name,
        dimension=len(res['data'][0]['embedding']),
        metric='cosine',
        metadata_config={'indexed': ['contains_keywords']}
    )
# connect to index
index = pinecone.Index(index_name)
# view index stats
index.describe_index_stats()
```

我们生成了一个新的 Pinecone DB 索引（如果它尚不存在的话）。让我们用我们的数据填充它：

```py
from tqdm.auto import tqdm
import datetime
from time import sleep

embed_model = "text-embedding-ada-002"
batch_size = 32  # how many embeddings we create and insert at once

def insert_into_pinecone(lines, subject):
  print(f'Inserting {len(lines)} of subject: {subject} into pinecone')
  for i in tqdm(range(0, len(lines), batch_size)):
      # find end of batch
      i_end = min(len(lines), i+batch_size)
      meta_batch = lines[i:i_end]
      ids_batch = [str(x[0]) for x in meta_batch]
      texts = [x[1] for x in meta_batch]
      try:
          res = openai.Embedding.create(input=texts, engine=embed_model)
      except:
          done = False
          while not done:
              sleep(5)
              try:
                  res = openai.Embedding.create(input=texts, engine=embed_model)
                  done = True
              except:
                  pass
      embeds = [record['embedding'] for record in res['data']]
      # cleanup metadata
      meta_batch = [{
          'contains_keywords': subject,
          'text': x,
      } for x in texts]
      to_upsert = list(zip(ids_batch, embeds, meta_batch))
      # upsert to Pinecone
      index.upsert(vectors=to_upsert)

for subject, lines in subject_lines.items():
  insert_into_pinecone(lines, subject)
```

很好！我们已经填充了向量数据库。让我们为第一个问题（维多利亚州哪个城市被认为是澳大利亚的体育之都？）获取前两个最相关的上下文：

```py
res = openai.Embedding.create(
    input=[questions_answers[0]['question']],
    engine=embed_model
)

# retrieve from Pinecone
xq = res['data'][0]['embedding']

# get relevant contexts (including the questions)
res = index.query(xq, top_k=2, include_metadata=True)

res
```

```py
{'matches': [{'id': '2642286',
              'metadata': {'contains_keywords': 'אוסטרליה',
                           'text': 'העיר שוכנת לחופה הצפון-מערבי של '
                                   'אוסטרליה.\n'},
              'score': 0.899194837,
              'values': []},
             {'id': '2282112',
              'metadata': {'contains_keywords': 'אוסטרליה',
                           'text': 'אוסטרלאסיה היה השם של נבחרת הספורטאים '
                                   'מאוסטרליה וניו זילנד שהשתתפה באולימפיאדת '
                                   'לונדון (1908) ובאולימפיאדת סטוקהולם '
                                   '(1912).\n'},
              'score': 0.893916488,
              'values': []}],
 'namespace': ''}
```

我们看到，在我们的向量数据库中（以与问题嵌入的余弦相似度为准），前两行并不太有用。第一行是关于澳大利亚的非常一般的句子，第二行确实谈论了澳大利亚的体育，但没有提到我们期望的答案——墨尔本。

但仍然，让我们挑战我们的模型，看看在它现在可以访问额外的希伯来语信息的情况下表现如何。请注意，我们指示模型在无法从给定上下文中推导出答案时回答“I don’t know”。

```py
limit = 3000

def retrieve(query, filter_subjects=None, add_dont_know=False):
    res = openai.Embedding.create(
        input=[query],
        engine=embed_model
    )

    # retrieve from Pinecone
    xq = res['data'][0]['embedding']

    # get relevant contexts
    res = index.query(xq,
                      {'contains_keywords': {'$in': filter_subjects}}
                      if filter_subjects is not None else None,
                      top_k=100,
                      include_metadata=True)
    contexts = [
        x['metadata']['text'] for x in res['matches']
    ]

    # build our prompt with the retrieved contexts included
    idk_str = '''. If the question cannot be answered using the
    information provided answer with "I don't know"''' if add_dont_know else ''
    prompt_start = (
        "Answer the question based on the context below." + idk_str + "\n\n" +
        "Context:\n"
    )
    prompt_end = (
        f"\n\nQuestion: {query}\nAnswer:"
    )
    # append contexts until hitting limit
    for i in range(1, len(contexts)):
        if len("\n\n---\n\n".join(contexts[:i])) >= limit:
            prompt = (
                prompt_start +
                "\n\n---\n\n".join(contexts[:i-1]) +
                prompt_end
            )
            break
        elif i == len(contexts)-1:
            prompt = (
                prompt_start +
                "\n\n---\n\n".join(contexts) +
                prompt_end
            )
    return prompt

original_subjects = ['אוסטרליה', 'אוניברסיטה', 'המוות השחור']

for i, qa_dct in enumerate(questions_answers):
  print(f'####################')
  print(f'Question #{i + 1} with context:')
  query_with_contexts = retrieve(qa_dct['question'],
                                 filter_subjects=original_subjects,
                                 add_dont_know=True)
  print(f'Length of query with contexts: {len(query_with_contexts)}')
  print_question_answer(qa_dct['question'],
                        query_with_contexts,
                        qa_dct['answer'],
                        qa_dct['question_translation'],
                        qa_dct['answer_translation'],
                        )
```

```py
####################
Question #1 with context:
Length of query with contexts: 3207
Question: איזו עיר בויקטוריה נחשבת לבירת הספורט של אוסטרליה?
Question (translation): Which city in Victoria is considered the sporting capital of Australia?
Answer: קנברה
Answer (translation): Canberra
Expected Answer: מלבורן
Expected Answer (translation): Melbourne
####################
Question #2 with context:
Length of query with contexts: 3179
Question: איזה נהר ממוקם בקרבת אוניברסיטת הארווארד?
Question (translation): Which river is located in the vicinity of Harvard University?
Answer: I don't know
Answer (translation): I don't know
Expected Answer: נהר צ'ארלס
Expected Answer (translation): Charles River
####################
Question #3 with context:
Length of query with contexts: 3138
Question: כמה אנשים ברחבי העולם נספו בעקבות המוות השחור?
Question (translation): How many people in the world died from the black plague?
Answer: I don't know.
Answer (translation): I don't know.
Expected Answer: בין 75 ל200 מיליון איש
Expected Answer (translation): Between 75 and 200 million
```

好的，稍微有所改善。第一个答案仍然错误。在第二个和第三个答案中，我们得到了“I don’t know”，我认为这比错误答案要好得多。但我们仍然希望提供当前的答案，我们如何改进呢？

让我们向向量数据库中添加一些额外的主题，也许它没有足够的相关信息来提取相关上下文。

```py
additional_subjects = [
    ('מלבורן', 'Melbourne'),
    ('סידני', 'Sydney'),
    ('אדלייד', 'Adelaide'),
    ('הרווארד', 'Harvard'),
    ("מסצ'וסטס", 'Massachusetts'),
    ("צ'ארלס", 'Charles'),
    ('בוסטון', 'Boston'),
    ('מגפה', 'plague'),
    ('האבעבועות השחורות', 'smallpox'),
    ('אבעבועות שחורות', 'smallpox'),
    ('מונגוליה', 'mongolia')
]

additional_subject_lines = {}
for subject, subject_translation in additional_subjects:
  additional_subject_lines[subject] = get_lines_per_subject(hebrew_wiki, subject, subject_translation)

for subject, lines in additional_subject_lines.items():
  insert_into_pinecone(lines, subject)
```

```py
There are 948 sentences that have the words Melbourne in them
In total it amounts to 119669 tokens
With the current pricing of openAI it will cost 0.005$ to embed everything
There are 2812 sentences that have the words Sydney in them
In total it amounts to 347608 tokens
With the current pricing of openAI it will cost 0.014$ to embed everything
There are 213 sentences that have the words Adelaide in them
In total it amounts to 24480 tokens
With the current pricing of openAI it will cost 0.001$ to embed everything
There are 1809 sentences that have the words Harvard in them
In total it amounts to 228918 tokens
With the current pricing of openAI it will cost 0.009$ to embed everything
There are 1476 sentences that have the words Massachusetts in them
In total it amounts to 179227 tokens
With the current pricing of openAI it will cost 0.007$ to embed everything
There are 5025 sentences that have the words Charles in them
In total it amounts to 647501 tokens
With the current pricing of openAI it will cost 0.026$ to embed everything
There are 2912 sentences that have the words Boston in them
In total it amounts to 361337 tokens
With the current pricing of openAI it will cost 0.014$ to embed everything
There are 1007 sentences that have the words plague in them
In total it amounts to 116961 tokens
With the current pricing of openAI it will cost 0.005$ to embed everything
There are 39 sentences that have the words smallpox in them
In total it amounts to 5417 tokens
With the current pricing of openAI it will cost 0.000$ to embed everything
There are 135 sentences that have the words smallpox in them
In total it amounts to 15815 tokens
With the current pricing of openAI it will cost 0.001$ to embed everything
There are 742 sentences that have the words mongolia in them
In total it amounts to 90607 tokens
With the current pricing of openAI it will cost 0.004$ to embed everything
```

很好，我们添加了更丰富的上下文（你甚至可以说我们有点作弊 — 请注意，我们添加了所有包含“墨尔本”和“查尔斯”等词汇的句子，这些句子是我们问题的答案。但这只是一个演示！）

```py
for i, qa_dct in enumerate(questions_answers):
  print(f'####################')
  print(f'Question #{i + 1} with context:')
  query_with_contexts = retrieve(qa_dct['question'], add_dont_know=Tue)
  print(f'Length of query with contexts: {len(query_with_contexts)}')
  print_question_answer(qa_dct['question'],
                        query_with_contexts,
                        qa_dct['answer'],
                        qa_dct['question_translation'],
                        qa_dct['answer_translation'],
                        )
```

```py
####################
Question #1 with context:
Length of query with contexts: 3158
Question: איזו עיר בויקטוריה נחשבת לבירת הספורט של אוסטרליה?
Question (translation): Which city in Victoria is considered the sporting capital of Australia?
Answer: מלבורן
Answer (translation): Melbourne
Expected Answer: מלבורן
Expected Answer (translation): Melbourne
####################
Question #2 with context:
Length of query with contexts: 3106
Question: איזה נהר ממוקם בקרבת אוניברסיטת הארווארד?
Question (translation): Which river is located in the vicinity of Harvard University?
Answer: נהר צ'ארלס
Answer (translation): Charles River
Expected Answer: נהר צ'ארלס
Expected Answer (translation): Charles River
####################
Question #3 with context:
Length of query with contexts: 3203
Question: כמה אנשים ברחבי העולם נספו בעקבות המוות השחור?
Question (translation): How many people in the world died from the black plague?
Answer: לפי הערכות שונות, כ-35 מיליון בני אדם בסין לבדה, ובין 20 ל-25 מיליון בני אדם באירופה.
Answer (translation): According to various estimates, about 35 million people in China alone, and between 20 and 25 million people in Europe.
Expected Answer: בין 75 ל200 מיליון איש
Expected Answer (translation): Between 75 and 200 million
```

不错！我们得到了答案！虽然第三个答案仍然不是 100% 正确，但比“I don’t know”好得多，也比最初的幻觉要好。

# 第五部分：结论

在本文中，我们已经看到如何使用向量数据库自动提供相关上下文给 LLM，以改善模型输出。在这个具体的案例中，这可能不是必需的，因为 ChatGPT（作为 API 提供）在希伯来语上的表现已经大幅提升，能够很好地回答这些问题。

然而，本文的重点是介绍这一概念，这一概念可以推广到其他目的，例如：

+   使用 LLM 分析未在网上公开的专有数据

+   通过将用于生成输出的上下文附加到用户输出中，为模型添加一种可解释性（以证明答案的可靠性）。

希望你喜欢这篇文章，并激发了你自己尝试的动力。下次见！

Elad

# 参考文献

+   希伯来语维基百科数据 — [https://u.cs.biu.ac.il/~yogo/hebwiki/](https://u.cs.biu.ac.il/~yogo/hebwiki/)

+   SQuAD 数据集 — [https://rajpurkar.github.io/SQuAD-explorer/](https://rajpurkar.github.io/SQuAD-explorer/)

+   James Briggs 的视频教程 — [https://youtu.be/dRUIGgNBvVk](https://youtu.be/dRUIGgNBvVk)

+   OpenAI API — [https://openai.com/blog/openai-api](https://openai.com/blog/openai-api)

+   Pinecone 数据库 — [https://www.pinecone.io/](https://www.pinecone.io/)
