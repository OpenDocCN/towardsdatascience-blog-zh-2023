# 一个好的描述就是你所需要的一切

> 原文：[https://towardsdatascience.com/a-good-description-is-all-you-need-1d0b47be10ec?source=collection_archive---------12-----------------------#2023-08-10](https://towardsdatascience.com/a-good-description-is-all-you-need-1d0b47be10ec?source=collection_archive---------12-----------------------#2023-08-10)

## 如何使用少量学习来提高文本分类性能

[](https://medium.com/@ilia.teimouri?source=post_page-----1d0b47be10ec--------------------------------)[![Ilia Teimouri PhD](../Images/0eb948c4d3f81c116cd16fa4d5016629.png)](https://medium.com/@ilia.teimouri?source=post_page-----1d0b47be10ec--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1d0b47be10ec--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1d0b47be10ec--------------------------------) [Ilia Teimouri PhD](https://medium.com/@ilia.teimouri?source=post_page-----1d0b47be10ec--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbf9b9036159&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-good-description-is-all-you-need-1d0b47be10ec&user=Ilia+Teimouri+PhD&userId=bf9b9036159&source=post_page-bf9b9036159----1d0b47be10ec---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----1d0b47be10ec--------------------------------) ·7分钟阅读·2023年8月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1d0b47be10ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-good-description-is-all-you-need-1d0b47be10ec&user=Ilia+Teimouri+PhD&userId=bf9b9036159&source=-----1d0b47be10ec---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1d0b47be10ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-good-description-is-all-you-need-1d0b47be10ec&source=-----1d0b47be10ec---------------------bookmark_footer-----------)![](../Images/03be7b5832a9d134961e51b08dd7a5a7.png)

由[Patrick Tomasso](https://unsplash.com/@impatrickt)拍摄，来源于[Unsplash](https://unsplash.com/)。

我已经使用大语言模型（LLMs）一段时间了，既用于个人项目，也作为日常工作的组成部分。像许多人一样，我对这些模型的强大能力感到兴奋。然而，重要的是要知道，尽管这些模型非常强大，但仍然可以针对各种任务进行改进。

而且不，我不会写关于[微调LLMs](https://platform.openai.com/docs/guides/fine-tuning)的内容，因为这可能会很昂贵，并且通常需要一台好的GPU设备。事实上，我将展示一种非常简单的使用少量学习来改善你的模型的方法。

少样本学习是一种机器学习技术，其中模型通过仅使用少量示例（通常每类仅1–5个示例）来解决新任务。少样本学习有一些关键点：

+   从小数据中学习归纳：少样本学习方法旨在学习能够从少量示例中很好地归纳模型，这与传统的深度学习方法（需要数千或数百万个示例）形成对比。

+   迁移学习：少样本学习方法利用从解决先前任务中获得的知识，并将这些知识转移到帮助更快地学习新任务和更少的数据。这种迁移学习能力是关键。

+   学习相似度度量：一些少样本学习技术专注于学习示例之间的相似度度量。这允许将新示例与现有标记示例进行比较以进行预测。

但如何在分类问题中使用少样本学习以提高模型性能？让我们通过一个例子来演示。

# 数据和准备

我通过从HuggingFace获取数据来开始我的分析。数据集名为[financial-reports-sec](https://huggingface.co/datasets/JanosAudran/financial-reports-sec)（该数据集具有Apache许可证2.0并允许商业使用），根据数据集作者的说法，它包含了1993–2020年美国上市公司向SEC [EDGAR系统](https://www.sec.gov/edgar/sec-api-documentation)提交的年度报告（10-K文件）。每份年度报告（10-K文件）被分为20个部分。

当前任务的两个相关属性对数据很有用：

+   **句子**：来自10-K文件报告的摘录

+   **部分**：标记句子所属的10-K文件部分

我关注了三个部分：

+   业务（项目1）：描述公司的业务，包括子公司、市场、最近事件、竞争、法规和劳动力。在数据中用0表示。

+   风险因素（项目1A）：讨论可能影响公司的风险，例如外部因素、潜在故障和其他警告投资者的披露。用1表示。

+   属性（项目2）：详细说明重要的实物资产。不包括知识产权或无形资产。在数据中用3表示。

对于每个标签，我抽取了10个示例（不放回）。数据结构如下：

![](../Images/7efd497ca2a4c2bd5f4fa465874d2a60.png)

# **现成**预测

一旦数据准备好，我所要做的就是制作一个分类器函数，该函数从数据框中获取句子并预测标签。

```py
Role = '''
You are expert in SEC 10-K forms. 
You will be presented by a text and you need to classify the text into either 'Item 1', 'Item 1A' or 'Item 2'. 
The text only belongs to one of the mentioned categories so only return one category.
'''
def sec_classifier(text): 

    response = openai.ChatCompletion.create(
        model='gpt-4',
        messages=[
            {
                "role": "system",
                "content": Role},
            {
                "role": "user",
                "content": text}],
        temperature=0,
        max_tokens=256,
        top_p=1,
        frequency_penalty=0,
        presence_penalty=0)

    return response['choices'][0]['message']['content']
```

我在这里使用GPT-4，因为这是迄今为止OpenAI最强大的模型。我还将温度设置为0，以确保模型不会偏离轨道。真正有趣的部分是如何定义角色——这就是我可以指导模型做我想要它做的事情的地方。角色指示模型保持专注并提供我所期望的输出。为模型定义一个清晰的角色有助于生成相关的、高质量的响应。这个功能中的提示是：

> 你是SEC 10-K表格的专家。
> 
> 你将收到一段文本，你需要将文本分类为‘第1项’，‘第1A项’或‘第2项’。
> 
> 文本只属于提到的一个类别，因此只返回一个类别。

在对所有数据行应用分类功能后，我生成了一个分类报告来评估模型的性能。宏平均F1分数为0.62，表明该多类问题的预测能力相当强。由于所有3个类别的示例数量均衡，宏平均和加权平均值收敛到相同的值。这个基准分数反映了在任何额外调整或优化之前，预训练模型的开箱即用的准确性。

```py
 precision    recall  f1-score   support

      Item 1       0.47      0.80      0.59        10
     Item 1A       0.80      0.80      0.80        10
      Item 2       1.00      0.30      0.46        10

    accuracy                           0.63        30
   macro avg       0.76      0.63      0.62        30
weighted avg       0.76      0.63      0.62        30
```

# 描述是你所需要的（少样本预测）

如前所述，少样本学习就是通过少量好的示例来推广模型。为此，我通过描述第1项、第1A项和第2项是什么（[基于维基百科](https://en.wikipedia.org/wiki/Form_10-K)）来修改了我的类别：

```py
Role_fewshot = '''
You are expert in SEC 10-K forms. 
You will be presented by a text and you need to classify the text into either 'Item 1', 'Item 1A' or 'Item 2'. 
The text only belongs to one of the mentioned categories so only return one category.
In your classification take the following definitions into account: 

Item 1 (i.e. Business) describes the business of the company: who and what the company does, what subsidiaries it owns, and what markets it operates in. 
It may also include recent events, competition, regulations, and labor issues. (Some industries are heavily regulated, have complex labor requirements, which have significant effects on the business.) 
Other topics in this section may include special operating costs, seasonal factors, or insurance matters.

Item 1A (i.e. Risk Factors) is the section where the company lays anything that could go wrong, likely external effects, possible future failures to meet obligations, and other risks disclosed to adequately warn investors and potential investors.

Item 2 (i.e. Properties) is the section that lays out the significant properties, physical assets, of the company. This only includes physical types of property, not intellectual or intangible property.

Note: Only state the Item.
'''
def sec_classifier_fewshot(text): 

    response = openai.ChatCompletion.create(
        model='gpt-4',
        messages=[
            {
                "role": "system",
                "content": Role_fewshot},
            {
                "role": "user",
                "content": text}],
        temperature=0,
        max_tokens=256,
        top_p=1,
        frequency_penalty=0,
        presence_penalty=0)

    return response['choices'][0]['message']['content']
```

现在的提示是：

> 你是SEC 10-K表格的专家。
> 
> 你将收到一段文本，你需要将文本分类为‘第1项’，‘第1A项’或‘第2项’。
> 
> 文本只属于提到的一个类别，因此只返回一个类别。
> 
> **在你的分类中考虑以下定义：**
> 
> **第1项（即业务）描述了公司的业务：公司是谁，做什么，拥有哪些子公司，以及运营的市场。**
> 
> 它还可能包括近期事件、竞争、法规和劳动问题。（一些行业受到严格监管，具有复杂的劳动要求，这些都对业务产生重大影响。）
> 
> 本节中的其他主题可能包括特殊操作成本、季节性因素或保险问题。**
> 
> **第1A项（即风险因素）是公司列出可能出现问题的部分，包括可能的外部影响、未来未能履行义务的可能性以及其他风险，以充分警示投资者和潜在投资者。**
> 
> **第2项（即属性）是列出公司重要属性、实物资产的部分。这仅包括实物类型的财产，而不包括知识产权或无形财产。**

如果我们在这些文本上运行，就会得到以下性能：

```py
 precision    recall  f1-score   support

      Item 1       0.70      0.70      0.70        10
     Item 1A       0.78      0.70      0.74        10
      Item 2       0.91      1.00      0.95        10

    accuracy                           0.80        30
   macro avg       0.80      0.80      0.80        30
weighted avg       0.80      0.80      0.80        30
```

**宏平均F1现在是0.80，也就是我们的预测提升了29%**，这仅仅是通过提供每个类别的良好描述。

最终，你可以查看完整的数据集：

![](../Images/ac490bda07bc6c8fd2c5bc560fd643b0.png)

实际上，我提供的示例为模型提供了具体的学习实例。通过查看多个示例，模型可以推断出模式和特征，开始注意到标志总体概念的共同点和差异。这有助于模型形成更为稳健的表示。此外，提供示例本质上充当了一种弱监督形式，引导模型朝着期望的行为发展，而不依赖于大型标记数据集。

在少量样本功能中，具体示例帮助引导模型关注应注意的信息和模式。总之，具体示例对少量样本学习至关重要，因为它们为模型提供了建立新概念初步表示的锚点，然后可以在提供的少量示例中进行细化。通过特定实例的归纳学习帮助模型形成抽象概念的细致表示。

如果你喜欢阅读这些内容并希望保持联系，你可以在我的[LinkedIn](https://www.linkedin.com/in/iliateimouri/)上找到我，或通过我的网页：[iliateimouri.com](https://www.iliateimouri.com)

*注意：所有图片，除非另有说明，均由作者提供。*
