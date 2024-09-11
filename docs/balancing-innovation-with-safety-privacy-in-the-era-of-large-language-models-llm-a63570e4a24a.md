# 在大语言模型（LLM）时代平衡创新与安全性和隐私

> 原文：[https://towardsdatascience.com/balancing-innovation-with-safety-privacy-in-the-era-of-large-language-models-llm-a63570e4a24a?source=collection_archive---------2-----------------------#2023-09-20](https://towardsdatascience.com/balancing-innovation-with-safety-privacy-in-the-era-of-large-language-models-llm-a63570e4a24a?source=collection_archive---------2-----------------------#2023-09-20)

## 关于为你的生成式 AI 应用实施安全性和隐私机制的指南

[](https://medium.com/@anjanava.biswas?source=post_page-----a63570e4a24a--------------------------------)[![Anjan Biswas](../Images/d8ba1231ad138fdc44a95ce12adeb1e2.png)](https://medium.com/@anjanava.biswas?source=post_page-----a63570e4a24a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a63570e4a24a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a63570e4a24a--------------------------------) [Anjan Biswas](https://medium.com/@anjanava.biswas?source=post_page-----a63570e4a24a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdbbc0b48552b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbalancing-innovation-with-safety-privacy-in-the-era-of-large-language-models-llm-a63570e4a24a&user=Anjan+Biswas&userId=dbbc0b48552b&source=post_page-dbbc0b48552b----a63570e4a24a---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----a63570e4a24a--------------------------------) ·12分钟阅读·2023年9月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa63570e4a24a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbalancing-innovation-with-safety-privacy-in-the-era-of-large-language-models-llm-a63570e4a24a&user=Anjan+Biswas&userId=dbbc0b48552b&source=-----a63570e4a24a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa63570e4a24a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbalancing-innovation-with-safety-privacy-in-the-era-of-large-language-models-llm-a63570e4a24a&source=-----a63570e4a24a---------------------bookmark_footer-----------)![](../Images/a667a614c9ee7df2114edacede095dca.png)

照片由[Jason Dent](https://unsplash.com/@jdent?utm_source=medium&utm_medium=referral)提供，拍摄于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

AI时代已将大语言模型（LLMs）推向技术前沿，这在2023年引起了广泛关注，并可能在未来许多年内继续受到关注。LLMs是像[ChatGPT](https://chat.openai.com/)这样的AI模型背后的强大引擎。这些AI模型通过大量的数据和计算能力，实现了从生成类人文本到协助自然语言理解（NLU）任务的卓越能力。它们迅速成为了无数应用程序和软件服务的基础，或至少被用来增强这些服务。

然而，正如任何突破性创新一样，LLMs的兴起带来了一个关键问题——*“我们如何在追求技术进步的同时平衡安全和隐私的迫切需要？”* 这不仅仅是一个哲学性问题，而是一个需要积极且深思熟虑的行动的挑战。

# 安全和隐私

为了在我们基于LLM的应用程序中优先考虑安全和隐私，我们将重点关注关键领域，包括控制个人数据（[个人身份信息](https://www.dol.gov/general/ppii)，即PII）和有害或有毒内容的传播。无论是用自己的数据集对LLM进行微调，还是仅仅用于文本生成任务，这一点都是至关重要的。*为什么这很重要？* 有几个原因说明了它的重要性。

+   遵守政府法规，保护用户个人信息（如[GDPR](https://gdpr-info.eu/)、[CCPA](https://oag.ca.gov/privacy/ccpa)、[HIPAA隐私规则](https://www.hhs.gov/hipaa/for-professionals/privacy/special-topics/de-identification/index.html)等）

+   遵守LLM提供商的最终用户许可协议（EULA）或可接受使用政策（AUP）

+   遵守组织内设定的InfoSec政策

+   减少模型中可能存在的偏差和偏颇；进行后期微调

+   确保道德使用大语言模型（LLMs）并维护品牌声誉

+   为任何可能出现的[AI法规](https://www.barrons.com/articles/washington-ai-regulation-meetings-senate-33e8c1a5)做好准备

## 微调的考虑因素

在准备对LLM进行微调时，第一步是数据准备。在研究、教育或个人项目之外，您很可能会遇到训练数据中包含PII（个人身份信息）信息的情况。第一步是识别数据中是否存在这些PII实体，第二步是清理数据以确保这些PII实体被妥善匿名化。

![](../Images/6e37eaa775478d72974668177f2337e8.png)

LLM微调

## 文本生成的考虑因素

对于使用LLMs进行文本生成，需要注意几点。首先，我们确保任何包含有毒内容的提示不会传播到LLM中，其次我们确保提示中不包含任何PII实体。接下来，在某些情况下，可能适合对**由LLM生成的文本**进行这些验证，或者对“*机器生成的文本*”进行验证。这提供了双重保护，以确保我们的安全和隐私原则。第三个方面是确定提示本身的*意图*，这在一定程度上可以遏制诸如[*提示注入攻击*](https://arxiv.org/pdf/2306.05499.pdf)等攻击。然而，我将主要关注PII和毒性问题，并在另一个讨论中讨论*意图*分类及其对LLMs的影响。

![](../Images/f5438261e7c25eff870f22c2a90bc311.png)

使用LLM进行文本生成

# 实现

我们将采取两步法来实现这一解决方案。首先，我们使用**命名实体识别（NER）**模型，该模型能够识别文本中的PII实体，并允许我们对这些实体进行匿名化。PII实体通常包括个人姓名、地点或地址、电话号码、信用卡号码、社会安全号码等。其次，我们使用**文本分类**模型来分类文本是`toxic`（有毒）还是`neutral`（中立）。有毒文本的例子通常包括包含辱骂、粗言秽语、骚扰、欺凌等内容的文本。

对于PII NER模型，一个最常见的选择是[BERT Base模型](https://huggingface.co/bert-base-uncased)，它可以进行微调以检测特定的PII实体。你也可以对预训练的[transformer](https://huggingface.co/docs/transformers/index)模型进行微调，例如[Robust DeID](https://huggingface.co/obi/deid_roberta_i2b2)（去标识化）预训练模型，它是一个[RoBERTa](https://arxiv.org/pdf/1907.11692.pdf)模型，针对医疗笔记的去标识化进行了微调，主要关注个人健康信息（即PHI）。一个更简单的选项是使用[spaCy ER（EntityRecognizer）](https://spacy.io/api/entityrecognizer)开始实验。

```py
import spacy

nlp = spacy.load("en_core_web_lg")
text = "Applicant's name is John Doe and he lives in Silver St. \
        and his phone number is 555-123-1290"
doc = nlp(text)

displacy.render(doc, style="ent", jupyter=True)
```

这给我们提供了

![](../Images/d79f386de85671b87d776398390aa039.png)

spaCy检测到的PII实体的标注

spaCy的`EntityRecognizer`能够识别三个实体——`PERSON`（人名，包括虚构人物）、`FAC`（地点或地址）和`CARDINAL`（不属于其他类型的数字）。spaCy还提供了检测到的实体的开始和结束偏移量（文本中的字符位置），我们可以利用这些信息进行匿名化。

```py
ent_positions = [(ent.start_char, ent.end_char) for ent in doc.ents]

for start, end in reversed(ent_positions):
    text = text[:start] + '#' * (end - start) + text[end:]

print(text)
```

这给我们提供了

```py
Applicant's name is ######## and his he lives in ###################and his phone number is ###-123-1290
```

但这里有一些明显的问题。spaCy ER 的默认实体列表并不涵盖所有类型的 PII 实体。例如，在我们的案例中，我们希望将 `555-123-1290` 识别为 `PHONE_NUMBER`，而不是将其视为文本的一部分，作为 `CARDINAL` 导致实体检测不完整。当然，就像基于转换器的 NER 模型一样，spaCy 也可以用您自己的自定义名称实体数据集进行训练，以提高其鲁棒性。然而，我们将使用 **开源** [**Presidio**](https://microsoft.github.io/presidio/) **SDK**，它是一个更专门的工具包，旨在实现 *数据保护* 和 *去标识化*。

## 使用 Presidio 进行 PII 检测和匿名化

Presidio SDK 提供了一整套 PII 检测功能，并支持一长串的 [支持的 PII 实体](https://microsoft.github.io/presidio/supported_entities/)。Presidio 主要使用模式匹配，同时结合 spaCy 和 [Stanza](https://stanfordnlp.github.io/stanza/) 的 ML 功能。然而，Presidio 是可定制的，可以插入使用基于转换器的 PII 实体识别模型，或者使用诸如 [Azure 文本分析 PII 检测](https://learn.microsoft.com/en-us/azure/ai-services/language-service/personally-identifiable-information/how-to-call) 或 [Amazon Comprehend PII 检测](https://docs.aws.amazon.com/comprehend/latest/dg/how-pii.html) 的云基础 PII 功能。它还配备了一个内置的可自定义匿名化工具，可以帮助清理和编辑文本中的 PII 实体。

```py
from presidio_analyzer import AnalyzerEngine

text="""
Applicant's name is John Doe and his he lives in Silver St.
and his phone number is 555-123-1290.
"""

analyzer = AnalyzerEngine()
results = analyzer.analyze(text=text,
                           language='en')
for result in results:
  print(f"PII Type={result.entity_type},",
        f"Start offset={result.start},",
        f"End offset={result.end},",
        f"Score={result.score}")
```

这给我们

```py
PII Type=PERSON, Start=21, End=29, Score=0.85
PII Type=LOCATION, Start=50, End=60, Score=0.85
PII Type=PHONE_NUMBER, Start=85, End=97, Score=0.75
```

和

![](../Images/f00a219f85855b7d41a62eb8927cf806.png)

Presidio 检测到的 PII 实体的注释

正如我们之前所见，由于我们拥有文本中每个实体的开始和结束偏移量，因此匿名化文本是一个相对简单的任务。然而，我们将利用 Presidio 的内置 `AnonymizerEngine` 来帮助我们完成这项任务。

```py
from presidio_anonymizer import AnonymizerEngine

anonymizer = AnonymizerEngine()
anonymized_text = anonymizer.anonymize(text=text,analyzer_results=results)
print(anonymized_text.text)
```

这给我们

```py
Applicant's name is <PERSON> and his he lives in <LOCATION>
and his phone number is <PHONE_NUMBER>.
```

到目前为止，这很好，但如果我们希望匿名化只是简单的掩码呢？在这种情况下，我们可以将自定义配置传递给 `AnonymizerEngine`，它可以对 PII 实体执行简单的掩码。例如，我们只用星号（`*`）字符来掩盖实体。

```py
from presidio_anonymizer import AnonymizerEngine
from presidio_anonymizer.entities import OperatorConfig

operators = dict()

# assuming `results` is the output of PII entity detection by `AnalyzerEngine`
for result in results:
  operators[result.entity_type] = OperatorConfig("mask", 
                                 {"chars_to_mask": result.end - result.start, 
                                  "masking_char": "*", 
                                  "from_end": False})

anonymizer = AnonymizerEngine()
anonymized_results = anonymizer.anonymize(
    text=text, analyzer_results=results, operators=operators
)

print(anonymized_results.text)
```

给我们

```py
Applicant's name is ******** and he lives in ********** and his phone number is ************.
```

## 匿名化的考虑事项

当您决定在文本中匿名化 PII 实体时，需要注意一些事项。

+   Presidio 的默认 `AnonymizerEngine` 使用模式 `<ENTITY_LABEL>` 来掩盖 PII 实体（如 `<PHONE_NUMBER>`）。这可能会引发问题，尤其是在 LLM 微调时。用实体类型标签替换 PII 可能会引入具有语义意义的词汇，从而潜在地影响语言模型的行为。

+   [***伪匿名化***](https://en.wikipedia.org/wiki/Pseudonymization)是一个有用的数据保护工具，但在对你的训练数据进行伪匿名化时应谨慎。例如，将所有`NAME`实体替换为伪名`John Doe`，或将所有`DATE`实体替换为`01-JAN-2000`，可能会导致你微调后的模型出现极端偏差。

+   注意你的LLM对提示中的某些字符或模式的反应。有些LLM可能需要非常特定的提示模板方式才能充分发挥模型的作用，例如，Anthropic建议使用[*提示标签*](https://docs.anthropic.com/claude/docs/constructing-a-prompt#mark-different-parts-of-the-prompt)*.* 了解这一点将有助于决定你可能想如何进行匿名化。

匿名化数据对模型微调可能会产生其他一般副作用，例如*上下文丧失*、*语义漂移*、*模型* *幻觉*等等。重要的是要进行迭代和实验，以确定适合你需求的匿名化程度，同时尽量减少对模型性能的负面影响。

## 通过文本分类进行毒性检测

为了识别文本是否包含毒性内容，我们将使用二分类方法——如果文本是中性的则为`0`，如果文本是有毒的则为`1`。我决定训练一个[DistilBERT基础模型（无大小写）](https://arxiv.org/abs/1910.01108)，这是一个[BERT基础模型](https://arxiv.org/abs/1810.04805)的精简版本。训练数据使用了[Jigsaw数据集](https://www.kaggle.com/c/jigsaw-multilingual-toxic-comment-classification)。

我不会详细介绍模型的训练过程和模型指标等，但你可以参考这篇关于[训练DistilBERT基础模型](https://medium.com/geekculture/hugging-face-distilbert-tensorflow-for-custom-text-classification-1ad4a49e26a7)的文章进行文本分类任务。你可以在[这里](https://github.com/annjawn/llm-safety-privacy/blob/main/toxicity.ipynb)查看我编写的模型训练脚本。该模型可以在HuggingFace Hub中找到，地址是`[tensor-trek/distilbert-toxicity-classifier](https://huggingface.co/tensor-trek/distilbert-toxicity-classifier)`。让我们通过推断运行一些示例文本，看看模型会告诉我们什么。

```py
from transformers import pipeline

text = ["This was a masterpiece. Not completely faithful to the books, but enthralling from beginning to end. Might be my favorite of the three.", 
        "I wish i could kill that bird, I hate it."]

classifier = pipeline("text-classification", model="tensor-trek/distilbert-toxicity-classifier")
classifier(text)
```

这给我们提供了—

```py
[
  {'label': 'NEUTRAL', 'score': 0.9995143413543701},
  {'label': 'TOXIC', 'score': 0.9622979164123535}
]
```

该模型以相当高的置信度正确地将文本分类为`NEUTRAL`或`TOXIC`。这个文本分类模型，与我们之前讨论的PII实体分类结合起来，现在可以用于创建一个机制，以在我们的LLM驱动的应用程序或服务中执行隐私和安全。

# 综合考虑

我们通过 PII 实体识别机制解决了 *隐私* 问题，通过文本有毒内容分类器解决了 *安全* 部分。你可以考虑其他可能与贵组织的安全和隐私定义相关的机制。例如，医疗组织可能更关心 PHI 而不是 PII，等等。最终，无论你想引入什么控制措施，整体实施方法保持不变。

考虑到这一点，现在是将一切付诸实践的时候了。我们希望能够将隐私和安全机制与 LLM 结合使用，以便在我们希望引入生成式 AI 功能的应用中使用。我将使用流行的 [LangChain](https://www.langchain.com/) 框架的 [Python 版本](https://python.langchain.com/docs/get_started/introduction)（也可以在 [JavaScript/TS](https://js.langchain.com/docs/get_started/introduction/) 中使用）来构建一个包含这两种机制的生成式 AI 应用。这就是我们的总体架构。

![](../Images/0184314e1d57b457e6e7fa7e1113517e.png)

LangChain 的隐私和安全流程

在上述架构中，我首先检查文本是否包含有毒内容，模型准确率至少需达到 80% 以上。如果是，整个 LangChain 应用的执行会在此时停止，并向用户显示适当的消息。如果文本被大致分类为 *中性*，则我会将其传递到下一步以识别 PII 实体。如果这些实体的检测置信度得分超过 50%，我将对文本中的这些实体进行匿名化。一旦文本完全匿名化，它将作为提示传递给 LLM 进行模型的进一步文本生成。请注意，准确率阈值（80% 和 50%）是任意的，你需要在你的数据上测试两个检测器（PII 和有毒内容）的准确性，并决定一个适合你用例的阈值。*阈值越低，系统变得越严格；阈值越高，检查的执行力度越弱*。

另一种更保守的方法是如果检测到任何 PII 实体，则停止执行。这对那些完全未获得处理 PII 数据认证的应用可能很有用，你希望确保无论如何，包含 PII 的文本不会作为输入被喂入应用。

![](../Images/b387590c1db20c3bec017720c12e683e.png)

LangChain 的隐私和安全流程——***备用流程***

## LangChain 实现

为了使其与 LangChain 配合使用，我创建了一个 [自定义 *链*](https://python.langchain.com/docs/modules/chains/how_to/custom_chain) 叫做 `PrivacyAndSafetyChain`。这个链可以与任何 [LangChain 支持的 LLMs](https://python.langchain.com/docs/modules/model_io/models/llms/) 连接，以实现隐私和安全机制。这就是它的样子——

```py
from langchain import HuggingFaceHub
from langchain import PromptTemplate, LLMChain
from PrivacyAndSafety import PrivacyAndSafetyChain

safety_privacy = PrivacyAndSafetyChain(verbose=True,
                                       pii_labels = ["PHONE_NUMBER", "US_SSN"])

template = """{question}"""

prompt = PromptTemplate(template=template, input_variables=["question"])
llm = HuggingFaceHub(
    repo_id=repo_id, model_kwargs={"temperature": 0.5, "max_length": 256}
)

chain = (
    prompt 
    | safety_privacy 
    | {"input": (lambda x: x['output'] ) | llm}
    | safety_privacy 
)

try:
    response = chain.invoke({"question": """What is John Doe's address, phone number and SSN from the following text?

John Doe, a resident of 1234 Elm Street in Springfield, recently celebrated his birthday on January 1st. Turning 43 this year, John reflected on the years gone by. He often shares memories of his younger days with his close friends through calls on his phone, (555) 123-4567\. Meanwhile, during a casual evening, he received an email at johndoe@example.com reminding him of an old acquaintance's reunion. As he navigated through some old documents, he stumbled upon a paper that listed his SSN as 338-12-6789, reminding him to store it in a safer place.
"""})
except Exception as e:
    print(str(e))
else:
    print(response['output'])
```

默认情况下，`PrivacyAndSafetyChain` 首先执行毒性检测。如果检测到任何有毒内容，它将会报错，从而如前所述，停止链条。如果没有检测到，它将把输入的文本传递给 PII 实体识别器，并根据使用的掩码字符，链条将对检测到的 PII 实体进行文本匿名化。前面的代码输出如下所示。由于没有有毒内容，链条没有停止，它检测到了 `PHONE_NUMBER` 和 `SSN` 并正确地进行了匿名化。

```py
> Entering new PrivacyAndSafetyChain chain...
Running PrivacyAndSafetyChain...
Checking for Toxic content...
Checking for PII...

> Finished chain.

> Entering new PrivacyAndSafetyChain chain...
Running PrivacyAndSafetyChain...
Checking for Toxic content...
Checking for PII...

> Finished chain.
1234 Elm Street, **************, ***********
```

# 结论

本文的最大收获是，随着我们继续在大型语言模型方面进行创新，平衡创新与安全和隐私变得至关重要。围绕 LLM 的热情以及我们对将其与各种可能用例整合的不断增长的渴望是不可否认的。然而，潜在的陷阱——如数据隐私泄露、无意的偏见或滥用——同样真实，值得我们立即关注。我介绍了如何建立一个检测 PII 和有毒内容的机制，并讨论了与 LangChain 的实现。

仍然有很多研究和开发工作待完成——或许需要更好的架构、更可靠和无缝的数据隐私和安全保障方式。本文中的代码经过简化以便于说明，但我鼓励你查看我的 [GitHub 仓库](https://github.com/annjawn/llm-safety-privacy/tree/main)，在这里我整理了每个步骤的详细笔记本以及我们讨论的自定义 LangChain 的完整源代码。使用它、分叉它、改进它，前进并创新！

## 参考文献

[1] Jacob Devlin, Ming-Wei Chang 等人 [BERT: 语言理解的深度双向变换器预训练](https://arxiv.org/abs/1810.04805)

[2] Victor Sanh, Lysandre Debut 等人 [DistilBERT, BERT 的蒸馏版本：更小、更快、更便宜、更轻量](https://arxiv.org/abs/1910.01108)

[3] 数据集 — [Jigsaw 多语言毒性评论分类 2020](https://www.kaggle.com/c/jigsaw-multilingual-toxic-comment-classification)

*除非另有说明，所有图片均由作者提供*
