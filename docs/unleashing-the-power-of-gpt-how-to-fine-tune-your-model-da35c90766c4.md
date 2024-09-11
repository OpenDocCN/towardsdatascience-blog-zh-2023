# 释放 GPT-3 的力量：超级英雄描述的微调

> 原文：[https://towardsdatascience.com/unleashing-the-power-of-gpt-how-to-fine-tune-your-model-da35c90766c4?source=collection_archive---------0-----------------------#2023-02-18](https://towardsdatascience.com/unleashing-the-power-of-gpt-how-to-fine-tune-your-model-da35c90766c4?source=collection_archive---------0-----------------------#2023-02-18)

## GPT-3 微调的逐步指南

[](https://medium.com/@ocaelen?source=post_page-----da35c90766c4--------------------------------)[![奥利维耶·卡伦](../Images/5315295f68999af7c14b456694d19979.png)](https://medium.com/@ocaelen?source=post_page-----da35c90766c4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----da35c90766c4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----da35c90766c4--------------------------------) [奥利维耶·卡伦](https://medium.com/@ocaelen?source=post_page-----da35c90766c4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd7268030c8a8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funleashing-the-power-of-gpt-how-to-fine-tune-your-model-da35c90766c4&user=Olivier+Caelen&userId=d7268030c8a8&source=post_page-d7268030c8a8----da35c90766c4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----da35c90766c4--------------------------------) ·11 min 阅读·2023年2月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fda35c90766c4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funleashing-the-power-of-gpt-how-to-fine-tune-your-model-da35c90766c4&user=Olivier+Caelen&userId=d7268030c8a8&source=-----da35c90766c4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fda35c90766c4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funleashing-the-power-of-gpt-how-to-fine-tune-your-model-da35c90766c4&source=-----da35c90766c4---------------------bookmark_footer-----------)![](../Images/d11e6869ddaac2af15c755bc2460aae9.png)

[摄影：h heyerlein](https://unsplash.com/@heyerlein?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

自2022年底以来，OpenAI 推出的 ChatGPT 被许多人认为是人工智能的 iPhone 时刻。然而，OpenAI 的聊天机器人并不是第一个生成式 AI 文本机器学习模型，它跟随的是两年前推出的 GPT-3。

OpenAI 为我们提供了一个现成的 GPT-3 训练模型。此外，特定任务可以在较小的数据集上进行微调。例如，假设你想创建一个针对你公司特定的电子邮件回复生成器。首先，你必须收集大量有关你特定业务领域的数据，如客户电子邮件咨询和回复。然后，你可以使用这些数据来微调 GPT-3，以学习你公司特定的语言模式和短语。通过微调 GPT-3，可以创建一个高度定制和专业化的电子邮件回复生成器，专门针对特定业务领域使用的语言模式和词汇。

在这篇博客文章中，我将向你展示如何微调 GPT-3。我们将使用 python 代码进行操作，并且不假设对 GPT-3 有任何先验知识。

# 微调需要什么？

与当前在 Hugging Face 上提供的 GPT-2 模型不同（撰写本文时），我们没有直接访问 GPT-3 模型的权限。因此，你首先需要从 OpenAI 获取一个 API 密钥，并安装 Python 包 *openai*，可以通过 *pip* 快速完成。

获取 OpenAI 的 API 密钥：

+   访问 [https://platform.openai.com/account/api-keys](https://platform.openai.com/account/api-keys) ，

+   创建一个账户，

+   点击‘*创建新密钥*’，然后

+   复制密钥。

关键是以‘sk-’开头的一长串字符。确保你保密！一旦你拥有了你的密钥，一个简单的获取密钥的方法是在终端中执行以下操作：（个人而言，为了简便，我将其放在我的*.bachrc*中）：

```py
export OPENAI_API_KEY=sk-t59pgejhtrff5(...)
```

使用 GPT-3 模型是有成本的。我们需要积分。撰写本文时，当你创建一个新账户时，你会获得免费的积分来尝试这个工具。我不知道这种情况是否会持续…

现在我们有了密钥和 Python 包，是时候考虑我们需要微调的数据了。首先，我们需要一个用于微调的示例文件，其中每个示例都是一个 *prompt*，后跟相应的 *completion*。

# 一个超级英雄描述生成工具

![](../Images/96f93f7ff3d3b406b69c8ec1a2ffb322.png)

来自 DALL-E 2 的超级英雄

我们将为这个演示构建一个工具，以创建虚构超级英雄的描述。最终，工具将接收超级英雄的年龄、性别和能力，并自动生成超级英雄的描述。

在以下示例中，经过模型微调后，我们只需输入'*40, woman, Healing ->*'，模型将自动生成一个描述。

![](../Images/6cd099cf773d7c6e17b6b7f8867d23b5.png)

这就是一切的关键！！ 😃

# 创建一个合成的数据集用于微调

在某些情况下，你可能有一个数据集想要用于微调。但由于我没有现成的数据集，我们来看看如何直接从 GPT-3 创建一个合成数据集，用于超级英雄的描述。以下代码将给我一个包含 *prompts* 和相应 *completions* 的 CSV 文件。

```py
import os
import openai
import pandas as pd

openai.api_key = os.getenv("OPENAI_API_KEY")

l_age = ['18', '20', '30', '40', '50', '60', '90']
l_gender = ['man', 'woman']
l_power = ['invisibility', 'read in the thoughts', 'turning lead into gold', 'immortality', 'telepathy', 'teleport', 'flight'] 

f_prompt = "Imagine a complete and detailed description of a {age}-year-old {gender} fictional character who has the superpower of {power}. Write out the entire description in a maximum of 100 words in great detail:"
f_sub_prompt = "{age}, {gender}, {power}"

df = pd.DataFrame()
for age in l_age:
 for gender in l_gender:
  for power in l_power:
   for i in range(3): ## 3 times each
    prompt = f_prompt.format(age=age, gender=gender, power=power)
    sub_prompt = f_sub_prompt.format(age=age, gender=gender, power=power)
    print(sub_prompt)

    response = openai.Completion.create(
     model="text-davinci-003",
     prompt=prompt,
     temperature=1,
     max_tokens=500,
     top_p=1,
     frequency_penalty=0,
     presence_penalty=0
    )

    finish_reason = response['choices'][0]['finish_reason']
    response_txt = response['choices'][0]['text']

    new_row = {
      'age':age, 
      'gender':gender, 
      'power':power, 
      'prompt':prompt, 
      'sub_prompt':sub_prompt, 
      'response_txt':response_txt, 
      'finish_reason':finish_reason}
    new_row = pd.DataFrame([new_row])
    df = pd.concat([df, new_row], axis=0, ignore_index=True)

df.to_csv("out_openai_completion.csv")
```

让我们深入了解这段代码是如何工作的 🧐。

变量*f_prompt*包含以下句子，其中*{age}*、*{gender}*和*{power}*是缺失的。

> 想象一下一个详细描述的{age}岁{gender}虚构角色，他拥有{power}的超级能力。用最多100个字写出整个详细描述：

在代码的前三个*for*循环中，我们遍历不同的{age}、{gender}和{power}值。在每一步循环中，我们用不同的值替换3个缺失的变量。然后，我们使用*openai.Completion.create*函数请求GPT生成对我们提示的响应。

这个函数的最重要参数是

+   *model*：用于生成响应的模型。OpenAI提供了四种标准GPT-3模型（`ada`、`babbage`、`curie`或`davinci`），它们在规模和使用价格上有所不同。这里使用的是*davinci*——最大的模型。

+   *prompt*：我们希望GPT-3完成的提示。

+   *temperature*：温度是一个介于0和1之间的数字，控制输出的随机性。我们将温度设置为最大值，以便模型在生成响应时尽可能具有创造力。

+   *max_tokens*：定义响应的最大长度。

在这个脚本的最后，我们有一个存储在文件*out_openai_completion.csv*中的Pandas表格。这个表格中我们感兴趣的两个主要列是*sub_prompt*和*response_txt*。

+   *sub_prompt*例如会是‘*18, man, invisibility*’。它包含了三个用逗号分隔的被替换的值。

+   *response_txt*包含GPT模型的输出。

# 微调GPT模型

以下代码检索之前创建的文件*out_openai_completion.csv*，并使用*openai*对GPT-3模型进行微调。

```py
import pandas as pd
import openai
import subprocess

df = pd.read_csv("out_openai_completion.csv")

prepared_data = df.loc[:,['sub_prompt','response_txt']]
prepared_data.rename(columns={'sub_prompt':'prompt', 'response_txt':'completion'}, inplace=True)
prepared_data.to_csv('prepared_data.csv',index=False)

## prepared_data.csv --> prepared_data_prepared.json
subprocess.run('openai tools fine_tunes.prepare_data --file prepared_data.csv --quiet'.split())

## Start fine-tuning
subprocess.run('openai api fine_tunes.create --training_file prepared_data_prepared.jsonl --model davinci --suffix "SuperHero"'.split())
```

让我们花时间来理解这段代码 🤓！

首先，文件*out_openai_completion.csv*的内容被加载到数据框*df*中。为了提醒一下，在我们的任务中，如果用户输入'40, female, healing'，我们希望得到一个关于40岁女性角色拥有治愈能力的描述。为了进行微调，需要向GPT提供用户可能输入的示例以及相应的期望响应。在数据框*df*中，*sub_prompt*和*response_txt*列包含了输入示例和对应的期望响应。在上面的代码中，我们首先提取这两列，然后将其分别重命名为*prompt*和*completion*。结果数据框被存储在一个新的文件*prepared_data.csv*中，仅包含这两列。

这个文件*prepared_data.csv*的内容如下：

```py
prompt,completion
"18, man, invisibility","He looks to be in his late teens, with dark eyes and unruly black hair which reach his shoulders. His build is athletic and strong, but not muscular. He often wears clothing that makes him blend in with his surroundings, in order to stay as inconspicuous as possible.He has the ability of invisibility, which he can use to become an invisible observer or to make himself or objects around him disappear from view. He has honed his skills to make himself undetectable, able to even make sounds vanish, allowing him to go unnoticed. His invisibility power is his greatest asset, and he uses it to protect those he cares about."
"18, man, invisibility","18 year-old Jackson is a powerful young man with the incredible power of invisibility. He stands at 6'2 and has a strong athletic frame with wavy brown hair, startling blue eyes and a warm and gentle smile. His strength and agility make him especially good at using his ability. He can become invisible in an instant, moving through walls and other obstacles without being detected. What's more, he can move objects with his mind and use his power to protect those he loves. His power is a blessing and a curse, as it can be abused to do harm. Jackson is a brave and noble person who uses his power to do good and make the world a better place."
"18, man, invisibility","Brandon is an 18-year-old of average build, standing roughly 5'10 with an inquisitive look. He has naturally wavy chestnut brown hair and bright blue eyes. His demeanor is usually quite serious, but he also has an easy and gentle smile. He has a natural gift of invisibility, which he uses to protect himself and his family from harm. He's an inquisitive and creative young man who uses his gift to explore the world, observe people, and uncover the truth. His consistent practice of mindfulness helps him remain unseen, even when his emotions are visible. His intelligence and persistent passion for truth drives him to help those in need."
"18, man, read in the thoughts","This 18-year-old man has a muscular stature and long, ash blonde hair. His bright blue eyes are full of knowledge and power, hinting at the strange ability he has - he can read minds. Now and then, strange sparks of electricity streak from his eyes when he concentrates hard enough. He is often calm and collected, but when provoked has the ability to blend his learning of your thoughts with his immense strength. His wit, intelligence and empathy are weapons that are often underestimated - yet, very lethal."
"18, man, read in the thoughts","He's an 18-year-old man with short, cropped dark hair and bright green eyes. His usual outfit consists of an olive green hoodie, blue jeans and purple trainers. He's relaxed, confident and always reads the atmosphere well. One of his great skills is read in the thoughts of others. By doing so, he is able to understand people more easily and relate to their concerns out of the blue. He always uses his superpower with the utmost dignity and respect, never using it to manipulate. He uses it to seek the truth, identify people's emotions and gain insight into their perspectives. He's the sort of person that can make anyone feel comfortable and is never afraid of a challenge. His superpower is merely an extension of his already charismatic disposition."
(...)
```

Python的*subprocess.run()*函数将命令作为子进程运行。它通常用于执行外部程序，就像在终端中运行一样。

我们使用 *subprocess.run()* 执行‘*openai tools fine_tunes.prepare_data*’。这个函数接受输入文件 *prepared_data.csv*，检查数据是否正确，并生成一个名为 *prepared_data_prepared.jsonl* 的 JSONL 文件。JSONL 文件是一种将每个 JSON 对象存储在单独一行的格式。JSONL 文件包含一系列 JSON 对象，每个对象之间用换行符分隔。

注意，我们已添加选项“- - quiet”以自动接受‘*openai tools fine_tunes.prepare_data*’所做的所有建议。例如，它建议在所有提示的末尾添加‘-->’，并在每个响应的末尾添加一个*END*标记。

这个 JSONL 文件的前几行看起来像这样：

```py
{"prompt":"18, man, invisibility ->","completion":" \n\nHe looks to be in his late teens, with dark eyes and unruly black hair which reach his shoulders. His build is athletic and strong, but not muscular. He often wears clothing that makes him blend in with his surroundings, in order to stay as inconspicuous as possible.\n\nHe has the ability of invisibility, which he can use to become an invisible observer or to make himself or objects around him disappear from view. He has honed his skills to make himself undetectable, able to even make sounds vanish, allowing him to go unnoticed. His invisibility power is his greatest asset, and he uses it to protect those he cares about. END"}
{"prompt":"18, man, invisibility ->","completion":" \n\n18 year-old Jackson is a powerful young man with the incredible power of invisibility. He stands at 6'2 and has a strong athletic frame with wavy brown hair, startling blue eyes and a warm and gentle smile. His strength and agility make him especially good at using his ability. He can become invisible in an instant, moving through walls and other obstacles without being detected. What's more, he can move objects with his mind and use his power to protect those he loves. His power is a blessing and a curse, as it can be abused to do harm. Jackson is a brave and noble person who uses his power to do good and make the world a better place. END"}
(...)
```

GPT-3 模型的微调实际上是在第二个 *subprocess.run()* 中实现的，其中执行了 *openai api fine_tunes.create*。在这个函数中，我们首先提供刚刚创建的 JSONL 文件的名称。然后你需要选择要微调的模型。OpenAI 提供了四个主要模型，具有不同的性能水平，适用于各种任务。*Davinci* 是最强大的模型，而 Ada 是最快的。*Davinci* 也是最昂贵的模型 😨。

由于我模型的目的是创建超级英雄的描述，因此我们给我的新模型添加了后缀“*超级英雄*”。

就这样😉 几分钟后，你将拥有一个可以使用的微调模型🌟。

# 现在是时候测试你的新模型了。

使用模型进行补全有不同的方法。主要通过 OpenAI 提供的 Playground 或者通过像 Python 这样的编程语言。

最简单的方法可能是使用 [playground](https://platform.openai.com/playground)。

+   访问 [https://platform.openai.com/playground](https://platform.openai.com/playground)。

+   点击‘模型’并搜索带有后缀“*超级英雄*”的模型。

+   在‘停止序列’中添加标记‘END’。

![](../Images/94c1ac78c2f2ebb1b5694840743e4dbf.png)

现在是时候要求我们的模型进行新的预测了。我们将要求描述一个18岁的男性角色，他真的有一个不必要的能力😉 我们将要求描述一个拥有‘*吃很多*’能力的角色……看看会发生什么……😆

![](../Images/e5746b85ae0ca808c0b93c6dc9905c3b.png)

不错 😅

你想用 Python 来做吗？很简单！点击屏幕右上角的‘查看代码’。

![](../Images/17fc2789ecac12e829d6a248e8dd1050.png)

在我们的案例中，在‘查看代码’中我们有：

```py
import os
import openai

openai.api_key = os.getenv("OPENAI_API_KEY")

response = openai.Completion.create(
  model="davinci:ft-klhm:superhero-2023-02-01-14-56-48",
  prompt="18, Man, can eat a lot ->\n",
  temperature=0.7,
  max_tokens=256,
  top_p=1,
  frequency_penalty=0,
  presence_penalty=0,
  stop=["END"]
)
```

你只需要复制并粘贴即可👍。

# 总结

在这个博客中，我们已经了解了如何生成合成数据以优化我们的模型以及如何进行微调。我们使用了创建超级英雄的用例，但相同的方法也可以用于你可能拥有的任何用例。最重要的是要有足够的质量示例，包含提示和期望的响应。

# 感谢阅读！

如果你希望保持最新的出版物并增加这个博客的可见性，请考虑关注我。
