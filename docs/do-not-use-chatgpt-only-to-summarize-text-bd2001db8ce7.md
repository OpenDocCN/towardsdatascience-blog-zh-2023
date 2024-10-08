# 不要仅仅使用 ChatGPT 来“总结文本”

> 原文：[`towardsdatascience.com/do-not-use-chatgpt-only-to-summarize-text-bd2001db8ce7`](https://towardsdatascience.com/do-not-use-chatgpt-only-to-summarize-text-bd2001db8ce7)

## 放开猛狮。

[](https://sonery.medium.com/?source=post_page-----bd2001db8ce7--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----bd2001db8ce7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bd2001db8ce7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd2001db8ce7--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----bd2001db8ce7--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd2001db8ce7--------------------------------) ·7 分钟阅读·2023 年 5 月 30 日

--

![](img/b50cc82c90749e47fb28af66868219cd.png)

照片由 [Mika Brandt](https://unsplash.com/es/@mikabr?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/images/animals/lion?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

总结文本是 ChatGPT 非常擅长的众多任务之一。给它一段内容并要求一个摘要。你会惊讶于它如何快速响应一个写得很好的摘要。

但不要只是要求一个摘要。

通过自定义提示，你可以让 ChatGPT 创建比普通摘要更多的内容。

*注意：我将使用 OpenAI API 的 Python 库。你也可以在网页* [*接口*](https://chat.openai.com/) *中使用相同的提示。*

第一步是导入库并创建 API 密钥：

```py
import openai
import os
from dotenv import load_dotenv, find_dotenv

_ = load_dotenv(find_dotenv())
openai.api_key  = os.getenv('OPENAI_API_KEY')
```

`OPENAI_API_KEY` 是保存 API 密钥的环境变量，可以从 [API Keys](https://platform.openai.com/account/api-keys) 菜单中获得。

下一步是创建一个辅助函数来运行提示。

```py
def get_completion(prompt, model="gpt-3.5-turbo"):
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=0, 
    )
    return response.choices[0].message["content"]
```

给定一个提示，这个功能将返回指定模型的输出，默认是 gpt-3.5-turbo。

## 只是一个摘要

以下是我想要总结的产品评论。

```py
review = """
I ordered this device (N300) to better extend WiFi signal to a Firestick on my small kitchen flat screen. 
Device phone set-up was a bit convoluted, in the steps were keyed to some other phone, non an iPhone, 
but close enough to figure it out. Once set up, the device definitely improved the push to the Firestick. 
However, began to get curious about monitoring the Macard device while in operation. On the Firestick end, 
one simply goes into setting/network, then WiFi ... push the (>II ) button to check connectivity. 
Currently returns both signal and video quality 'very good.' So that's what the Firestick sees.

In the iPhone App Store, there is a one-star ""macard Wifi Wizard,"" which is supposed to be useful to 
check the extender, download firmware updates, and so on. I downloaded it. It requests the email 
one registered the device with, then advises it is sending an activation code. The code never arrives. 
I contacted their support via chat; the rep acknowledged what I was saying and said he'd have 
""the concerned department"" send me a code within 24 hours. It is now 48 hours. Nothing. 
Thankfully, the device works.
"""
```

我们可以从仅要求“创建一个产品评论摘要”开始。

```py
prompt = f"""
Your task is to summarize a given product review.
The review is given below, delimited by triple quotes.

Review: ```{review}```py
"""

response = get_completion(prompt)
```

> **回应：** 评审者购买了 N300 设备，以将 WiFi 信号扩展到他们厨房里的 Firestick。设备设置有点混乱，但设置好后，提升了对 Firestick 的推送。评审者尝试使用“macard Wifi Wizard”应用程序来监控设备，但激活码从未到达。他们联系了支持，但没有收到回应。尽管如此，设备工作良好。

## 限制长度

它确实足够好，但可以变得更好，符合我们的特定需求。例如，我们可以限制摘要的长度。

```py
prompt = f"""
Your task is to summarize a given product review.
Use at most 30 words.
The review is given below, delimited by triple quotes.

Review: ```{review}```py
"""

response = get_completion(prompt)
```

我在提示中添加了“最多使用 30 个字。”

> **回应：** N300 WiFi 扩展器改善了 Firestick 的信号，但手机设置令人困惑。Macard 应用程序无效，但设备本身运行良好。

它能够从评论中提取更重要的点，并将输出限制为 30 个字。

## 仅限投诉

考虑一种情况，产品经理只搜索关于产品的负面评论和投诉，这将用于制定改进产品的路线图。

这可以很容易地在提示中完成。

```py
prompt = f"""
Your task is to summarize a given product review.
Include only negative comments, concerns, and complaints in the summary.
The review is given below, delimited by triple quotes.

Review: ```{review}```py
"""

response = get_completion(prompt)
```

> **回应：** 设备的手机设置过程复杂，iPhone 监控设备的应用程序无效。应用程序的激活码从未收到，客户支持未能履行承诺，在 24 小时内发送代码。

摘要仅包含用户遇到的问题。例如，与之前的摘要不同，它不包括“设备本身运行良好”的部分。

## 来自多条评论的具体点

我们正在分析设备设置，并希望了解用户在设置设备时是否遇到问题。我们可以从多条评论中提取与设备设置相关的部分，如下所示：

```py
prompt = f"""
Your task is to summarize multiple product reviews.
Include only the parts related to device setup.
The reviews are given below.

review_1: {review_1}
review_2: {review_2}
review_3: {review_3}
review_4: {review_4}

"""

response = get_completion(prompt)
```

> **回应：** 所有四条评论中都提到了设备设置过程。评论 1 表示与客户服务沟通后，设置过程非常顺利。评论 2 提到设备手机设置有些复杂，但一旦设置完成，设备运行良好。评论 3 在 PC 上设置设备遇到困难，但使用 Kindle 和 Silk 浏览器成功设置。评论 4 发现设置过程非常简单，并能够将设备与他们的互联网电话一起用于工作。

通过正确自定义提示，我们能够获取与设备设置相关的部分内容。

如果你想阅读这些评论或自己测试，这里是上述使用的评论：

```py
review_1 = """
If anyone is curious - the reset is a light sequence of all 3 lights \
The scanning is 1 solid blue and 1 flashing blue \
And the successful connection is 2 solid blue lights. \
I had difficulty with my original setup but spoke with the customer service line \
and was able to walk through the process very seamlessly. The company is great at \
making sure you are enjoying the product. So far it does seem to be helping, \
the TV in my bedroom hasn't buffered since setting up.
"""

review_2 = """
I ordered this device (N300) to better extend WiFi signal to a Firestick on my small kitchen flat screen. \ 
Device phone set-up was a bit convoluted, in the steps were keyed to some other phone, non an iPhone, \
but close enough to figure it out. Once set up, the device definitely improved the push to the Firestick. \ 
However, began to get curious about monitoring the Macard device while in operation. On the Firestick end, \
one simply goes into setting/network, then WiFi ... push the (>II ) button to check connectivity. \
Currently returns both signal and video quality 'very good.' So that's what the Firestick sees. \
In the iPhone App Store, there is a one-star ""macard Wifi Wizard,"" which is supposed to be useful to \
check the extender, download firmware updates, and so on. I downloaded it. It requests the email \
one registered the device with, then advises it is sending an activation code. The code never arrives. \
I contacted their support via chat; the rep acknowledged what I was saying and said he'd have \
""the concerned department"" send me a code within 24 hours. It is now 48 hours. Nothing. \
Thankfully, the device works.
"""

review_3 = """
I first tried to set this up using the procedure for PC. It got part way through it and froze. \
After that I could not get it to even try. Running Microsoft 10 and Google Chrome. Don't know if that \ 
has anything to do with it or not. Then I used my Kindle with the Silk browser, and it set up perfectly \ 
first time. I then moved it half way across the house, plugged it in, turned it on, and I had strong Wi-Fi \ 
all the way to the corner of the house. 1600 sq. ft., not at all "open concept".
"""

review_4 = """
I got this product to plug my internet based phone for work from home (Avaya desktop phone). \
It works! The setup was EXTREMELY easy. At completion, I plugged the phone into the extender's ethernet \
port and made a few phone calls which all worked perfectly with complete clarity. VERY happy with this \
purchase since a cordless headset is around $250 (which I would have needed since the phone had to be \
at the ethernet port on the wall). I recommend this product!
"""
```

## 优先级列表

假设我们有数百条评论，并且需要创建一个关于改进产品的问题的优先级列表。该列表将基于客户对我们产品的不满或问题。

ChatGPT 可以为我们创建这个列表。

```py
prompt = f"""
Your task is to summarize multiple product reviews.
I want to improve the product. 
Create a priority list of 3 issues mentioned in the reviews.
The reviews are given below.

review_1: {review_1}
review_2: {review_2}
review_3: {review_3}
review_4: {review_4}

"""

response = get_completion(prompt)
```

> **回应：**
> 
> 评论中提到的 3 个问题的优先级列表：
> 
> 设置设备时遇到的困难以及对更清晰说明的需求。
> 
> Macard Wifi Wizard 应用程序存在问题，并且需要更好的客户支持。
> 
> 需要更清晰的说明关于设备与不同类型手机的兼容性。

即使我们仅对 4 条评论进行分析，它已经非常有用。想象一下你有数百条评论需要分析以创建这样的优先级列表。ChatGPT 节省的时间和精力真是不可思议。

## 释放狮子

ChatGPT 的全部潜力超出了我们初次观察的范围。发现利用其真正能力和效率的关键在于精心设计和定制提示。

通过战略性地构建提示，我们可以引导模型生成更准确、更具上下文相关性和更详细的回答。

*你可以成为* [*Medium 会员*](https://sonery.medium.com/membership) *，以解锁对我所有写作内容的完全访问权限，还有 Medium 上的其他内容。如果你已经是会员，不要忘记* [*订阅*](https://sonery.medium.com/subscribe) *，这样每当我发布新文章时，你会收到邮件通知。*

感谢阅读。如果你有任何反馈，请告诉我。
