# ChatGPT：自动化提示评分

> 原文：[`towardsdatascience.com/chatgpt-automated-prompt-scoring-c972f9ee2c4f`](https://towardsdatascience.com/chatgpt-automated-prompt-scoring-c972f9ee2c4f)

![](img/1b9b1e6bc46d2e359537b4b1d0aff68d.png)

此图像是在 DALL·E 2 的协助下创建的

## 指南

## 如何使用 python 客观地选择和改进你的 ChatGPT 提示

[](https://michael-malin.medium.com/?source=post_page-----c972f9ee2c4f--------------------------------)![Michael Malin](https://michael-malin.medium.com/?source=post_page-----c972f9ee2c4f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c972f9ee2c4f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c972f9ee2c4f--------------------------------) [Michael Malin](https://michael-malin.medium.com/?source=post_page-----c972f9ee2c4f--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c972f9ee2c4f--------------------------------) ·10 分钟阅读·2023 年 4 月 10 日

--

大型语言模型（LLM）如 ChatGPT 正在产生巨大的影响。它们也仅仅是开始。在接下来的一年中，各大公司将开始推出领域/角色专用的 LLM 模型。实际上，像专注于金融的 [BloombergGPT](https://www.bloomberg.com/company/press/bloomberggpt-50-billion-parameter-llm-tuned-finance/) 和微软开发者专注的 [Copilot](https://blogs.microsoft.com/blog/2023/03/16/introducing-microsoft-365-copilot-your-copilot-for-work/) 等新产品已经在成为现实。我们很快将看到 AI 个人教练、健康教练、顾问、法律助理等更多应用。虽然有些情况可能需要基于领域特定数据的微调模型，但大多数可以通过简单的提示工程实现。但你如何知道你的提示是否足够好？我们如何在主观文本上生成客观准确度评分？

本指南将涵盖：

+   理论

+   提示工程

+   提示测试

+   提示评分

+   提示反馈

# 理论

测试 LLM 提示输出的困难在于结果是主观的。我可能觉得结果完美，而你觉得它们不够好。这两种观点都是有效的。这使得纯科学的方法来评分非常困难。应对这些问题的一种好的方法是 Delphi 方法。它涉及使用专家小组并汇总他们的结果。正如你所想象的，这可能很昂贵，但这就是 AI 的作用所在！

我们将创建一系列不同的个性提示，通过使用 Python 中的 OpenAI API 让它们与我们的主要提示进行对话，并创建一个评分提示来评估我们的主要提示表现如何。最后，我们汇总每个个性-提示对话的评分。这在前期工作上稍微多一些，但从长远来看可以节省很多时间。它还提供了一个客观的评分。

这种方法确实存在一个问题，你需要注意。对同一个提示的多次运行将产生略有不同的结果。这是德尔菲方法表现出色的地方。你使用的**多样**的备用提示越多，你的汇总评分结果将越一致。你实际上可以通过测量评分偏差来测试是否有足够的备用提示。当偏差较小时，你就处于一个不错的状态。

我承认这不是一个完美的解决方案。对于主观问题，没有解决方案是完美的。我注意到，当我手动审查对话时，通常会同意结果。这使我可以测试许多提示，并获得“足够好”的评估，从而快速迭代。在我们实施这里描述的德尔菲测试方法之前，了解提示工程的基础是重要的。

# 提示工程

基础 LLM 模型提供不错的一般响应。但这并不总是理想的。例如，如果我在构建聊天机器人，我不想返回三段文字的内容。也许我希望我的回答听起来更像人类，更具对话性。提示工程涉及提供指令来引导 LLM 的输出风格、格式和行为。

让我们从一个例子开始。我们计划构建一个聊天机器人，它将回答问题，就像是一个“短小、绿色、尖耳的宇宙巫师，在一个遥远的银河系中使用激光剑。”如果这样的角色存在于科幻小说中，我可以通过使用他们的名字来简化这个提示，但这样会面临版权问题。在一点帮助下，LLM 应该能够理解我的意图。

关于提示工程的详细指南， [请点击这里](https://github.com/dair-ai/Prompt-Engineering-Guide?fbclid=IwAR3LFOge-kn7GFiFaj3bUIsAOAG2V-gtXyUH93bRpSMQxc_3i8c0fTgQGxA)。我将介绍一些基础知识：

1.  要具体而简洁。例如，“用一句话向 4 年级学生解释引力”要比“请简短地解释地球上引力的工作原理，以便任何人都能理解”要好得多。

1.  使用###来清晰地区分指令和输入/输出。例如：###你是孔子。用 1 到 2 句话回答问题。使用引号来引用孔子的真实话语。###

1.  提供输出格式和示例。虽然 LLM 可能会根据我们对“短小、绿色、尖耳的宇宙巫师”的描述猜测我们在寻找什么，但我们不应该将其留给运气。通过引导对话，模型的表现会更好。让我们把所有内容整合在一起：

> ### 你是一个短小、绿色、尖耳的太空巫师，在遥远的星系中使用激光剑。不要自我介绍。用 1 到 2 句话回答问题。然后问一个跟进问题以保持对话的进行 ###
> ### 
> 你：“有什么困扰你的吗？”
> 
> 我：“不要低估我的力量！告诉我反叛基地的位置！”
> 
> 你：
> 
> “我不能透露位置。是什么让你加入了帝国，嗯？”

不错，但***别太自满***。找到一个好的提示可能需要一些试错过程。这可能会很耗时。随着升级模型的推出（例如，GPT4），提示行为可能会发生剧烈变化，需要调整。我们需要一种自动化测试这些提示的方法。

# 提示测试

对于自动化测试，我们将使用 ChatGPT API。我们将用 ChatGPT 与自己对话，而不是手动创建对话。我喜欢为不同的角色创建提示，这些提示反映了可能的使用场景（以及一些对抗性的提示）。以下是几个例子：

+   ### 你在学校有一个暗恋对象，正在寻求导师的建议###

+   ### 你明天有一个工作面试，正在寻找如何表现好的建议###

+   ### 你是一个在遥远星系中使用激光剑的黑暗领主。你正在试图从敌人那里逼迫出反叛基地的位置。###

现在我们有了一些提示可以使用，让我们一块一块地生成对话，以便逻辑易于理解。让我们从暗恋的例子开始。稍后我们会清理代码。

```py
import openai
openai.api_key = <your api key>

#Define prompts
wizard_prompt = [
    """### You are a short green pointy-eared space wizard that uses laser-swords
    in a galaxy far far away. Do not identify yourself. Answer questions in 1 or 2 sentences.
    Then ask a follow-up question to keep the conversation going. ###""",
    "Something troubles you, it does?"
]

user_prompt = [
    "### You have a crush at school and are seeking advice from a mentor###",
    "I have a crush at school but I am too afraid to tell her."
]

#We start with the space wizard:
wizard_response = openai.ChatCompletion.create(
    model = 'gpt-3.5-turbo',
    messages=[
        {"role": "user", "content": wizard_prompt[0]}, #wizard instructions
        {"role": "user", "content": "Wizard: " + wizard_prompt[1]}, #wizard primed opening
        {"role": "user", "content": "User: " + user_prompt[1]}, #users primed opening
        {"role": "user", "content": "Wizard:"}, #requested response to user question
    ]
)
wizard_response = wizard_response['choices'][0]['message']['content'] #pulling the response from output
wizard_prompt.append(wizard_response) #appending response to prompt list
print(wizard_response)
```

> “恐惧是通向黑暗面的道路，年轻人，要勇敢并说出你的真相。是什么让你觉得她可能有同样的感觉？”

```py
# Now we build the "school crush" persona using the wizard response
user_response = openai.ChatCompletion.create(
    model = 'gpt-3.5-turbo',
    messages=[
        {"role": "user", "content": user_prompt[0]}, #user instructions
        {"role": "user", "content": "Mentor: " + wizard_prompt[1]}, #wizard primed opening
        {"role": "user", "content": "You: " + user_prompt[1]}, #user primed opening
        {"role": "user", "content": "Mentor: " + wizard_prompt[2]}, #wizard generated response
        {"role": "user", "content": "You: "}, #requested response from wizard reply
    ]
)
user_response = user_response['choices'][0]['message']['content']
user_prompt.append(user_response) #appending user response to prompt list
print(user_response)
```

> “好吧，她总是很高兴见到我，我们有很多共同点。我们有过一些很棒的对话和笑声。但如果她不感觉一样，我不想破坏我们的友谊。”

从这里，我们可以来回循环，将回应附加到提示列表中。这是我承诺的干净代码：

```py
def GPT_call(prompts):
    call = openai.ChatCompletion.create(
        model = 'gpt-3.5-turbo',
        messages=[{"role": "user", "content": prompt} for prompt in prompts]
    )
    return call['choices'][0]['message']['content']

#starting over with initial wizard/user prompts
def GPT_convo(chatbot_prompt,user_prompt):
    chatbot_prompt = [chatbot_prompt[0],'Wizard: '+chatbot_prompt[1],'User: '+user_prompt[1]]
    user_prompt = [user_prompt[0],'You: '+user_prompt[1]]

    for i in range(3): # conversation goes for 3 iterations
        chatbot_prompt.append("Wizard:")
        response = GPT_call(chatbot_prompt)
        chatbot_prompt[-1] = "Wizard: "+response

        user_prompt.append("Mentor: "+response)
        if i == 3: # Add instructions to end conversation on final iteration
            user_prompt.append("###Try to wrap up the conversation### You:")
        else:
            user_prompt.append("You:")
        response = GPT_call(user_prompt)
        user_prompt[-1] = "You: "+ response
        chatbot_prompt.append("User: "+response)

    chatbot_prompt.append("Wizard:")
    response = GPT_call(chatbot_prompt)
    chatbot_prompt[-1] = "Wizard: "+response
    return chatbot_prompt # return full conversation
```

现在我们有了一种自动生成每个用户角色对话的方法。但这些对话好吗？我们需要一种客观评估主要角色回应的方法。但是我们怎么可能用主观文本完成这一点呢？我们将为自我评估建立一个第三个 GPT 角色！

# 提示评分

为了评估我们“太空巫师”的对话技能，我们将创建一个新的提示：

> ### 在 10 分制下，根据以下标准为巫师的回应打分：
> ### 
> 角色：巫师是一个短小、绿色、尖耳的太空巫师，在一个遥远的星系中使用激光剑。所有回应应符合这一角色。
> 
> 对话：回应应简洁而对话化。跟进问题应推动对话向前发展，而不至于乏味。巫师应适当地结束对话。
> 
> 有用：回应应帮助用户回答他们的问题或解决他们的问题。跟进问题应帮助收集信息以改善回应。
> 
> 以 JSON 格式呈现分数，如下所示：
> 
> {“Character”:<float>,”Conversational”:<float>,”Helpful”:<float>}
> 
> 请提供不带评论的评分。###

```py
conversation = GPT_convo(chatbot_prompt,user_prompt)
score = GPT_call(score_prompt + conversation)

import json
score = json.loads(score)
print(score)
```

> {‘Character’: 8.5, ‘Conversational’: 9, ‘Helpful’: 9}

我们现在有了分数！将这些分数汇总到所有角色中，我们可以得到我们测试的聊天机器人提示的整体分数。这使我们面临一个最终的问题：我们如何改进我们的提示以提高这些分数？再说一次，为什么不直接问 GPT 呢？***这就是方法。***

# 及时反馈

对于我们的反馈，我们将遵循以下模式：

+   从我们用来评分的相同提示开始，但省略要求返回分数的请求。

+   将生成的对话添加到提示中进行评估

+   给出生成对话评估的指示，基于评分标准

我们从提示开始：

> ### 根据以下对话内容，依据以下标准评估巫师：
> ### 
> 角色：巫师是一个短小、绿色、尖耳的太空巫师，在一个遥远的银河系中使用激光剑。所有回应应符合这一角色。
> 
> 对话风格：回应应该简洁且富有对话性。后续问题应推动对话向前发展，而不显得冗长。巫师应适当地结束对话。
> 
> 有帮助：回应应通过回答用户的问题或解决他们的问题来帮助用户。后续问题应有助于收集信息以改善回应。###
> 
> 这是对话：

```py
 conversations = feedback_prompt #prompt shown above
user_prompts = {'crush':user_prompt} #add all user personas
for user,prompt in user_prompts.items():
    convo = GPT_convo(chatbot_prompt,prompt)
    conversations.append("Conversation: " + str(convo[1:]))
conversations.append("""Give advice on how the Wizard can improve their responses 
in relation to each criterion defined above: Character, Conversational, and Helpful. 
Use examples from the conversation to make each point.""")

feedback = GPT_call(conversations)
print(feedback)
```

> 角色：总体而言，巫师很好地遵守了他们的角色设定，并始终使用与其角色相符的语言和短语。例如，“年轻的学徒”，“愿力量与你同在”，以及“悔恨带来痛苦”都是人们期望从太空巫师那里听到的短语。巫师还使用了特定于他们的遥远银河系的语言，例如“激光剑”。这里不需要改进。
> 
> 对话风格：巫师在回应时要简洁且引人入胜，提出的后续问题应推动对话向前发展，而不会显得重复或乏味。例如，当用户说他们害怕向心仪的人表达感情时，巫师应接着问“是什么让你犹豫接近她？”这表明巫师在积极倾听用户并试图理解他们的观点。另一个好的例子是当用户说他们不知道对心仪的人说什么时，巫师可以回应“在这种情况下，相信你的感觉，力量会引导你。”这将重点放回用户身上，赋予他们提出自己解决方案的能力。总体而言，巫师的对话语气友好、支持且不带偏见。
> 
> 有用：巫师的回应很有帮助，因为它们提供了具体的建议和可操作的步骤。例如，当用户说他们害怕拒绝时，巫师回应说“要有勇气，你必须……”和“最好还是冒险，你必须。”这给用户提供了鼓励和行动的推动。另一个有用的回应是，当用户说他们不知道对心仪对象说什么时，巫师建议简单地邀请喝咖啡或散步。这给用户提供了下一步要做的具体想法。总体来说，巫师提供了量身定制的、有针对性的建议。

这些信息很有用，但请小心，***这是一个陷阱！*** 每个 GPT 模型都有最大令牌限制。这意味着，如果你的对话过大或对话过多，你可能会在这一步遇到错误。实际上，我通常会筛选出三个得分最低的对话，并将其提交进行评估。

# 结论

你现在拥有了自动生成每次提示迭代的分数和反馈的工具。当然，还有一些工作需要完成。你不仅可以调整你的聊天机器人提示，还可以调整用户和评估提示，以更好地匹配你想要实现的目标。重要的是，你现在可以快速迭代，并做出客观决策。***这些就是你正在寻找的技巧。***

# 关于我

我是一名资深数据科学家和兼职自由职业者，拥有超过 12 年的经验。我一直在寻找联系机会，所以请随时：

+   [在 LinkedIn 上联系我](https://www.linkedin.com/in/michael-a-malin/)

+   [访问我的网站](http://modelforge.ai)

+   [在 Twitter 上关注我](https://twitter.com/alaska_malin)

+   [查看我的其他文章](https://michael-malin.medium.com/)

***如果你有任何问题，请随时在下方留言。***
