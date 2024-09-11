# 不要仅仅使用 ChatGPT 来“总结文本”

> 原文：[https://towardsdatascience.com/do-not-use-chatgpt-only-to-summarize-text-bd2001db8ce7?source=collection_archive---------7-----------------------#2023-05-30](https://towardsdatascience.com/do-not-use-chatgpt-only-to-summarize-text-bd2001db8ce7?source=collection_archive---------7-----------------------#2023-05-30)

## 释放狮子。

[](https://sonery.medium.com/?source=post_page-----bd2001db8ce7--------------------------------)[![Soner Yıldırım](../Images/c589572e9d1ee176cd4f5a0008173f1b.png)](https://sonery.medium.com/?source=post_page-----bd2001db8ce7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bd2001db8ce7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bd2001db8ce7--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----bd2001db8ce7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdo-not-use-chatgpt-only-to-summarize-text-bd2001db8ce7&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----bd2001db8ce7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd2001db8ce7--------------------------------) ·7分钟阅读·2023年5月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbd2001db8ce7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdo-not-use-chatgpt-only-to-summarize-text-bd2001db8ce7&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=-----bd2001db8ce7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbd2001db8ce7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdo-not-use-chatgpt-only-to-summarize-text-bd2001db8ce7&source=-----bd2001db8ce7---------------------bookmark_footer-----------)![](../Images/b50cc82c90749e47fb28af66868219cd.png)

图片由[Mika Brandt](https://unsplash.com/es/@mikabr?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/images/animals/lion?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

总结文本是 ChatGPT 擅长的许多任务之一。给它一段内容并要求总结，你会惊讶于它快速回应的高质量总结。

但不要仅仅要求总结。

通过自定义你的提示，你可以让 ChatGPT 创造出比简单总结更多的内容。

*注意：我将使用 Python 库中的 OpenAI API。你也可以在 web* [*界面*](https://chat.openai.com/) *中使用相同的提示。*

第一步是导入库并创建 API 密钥：

```py
import openai
import os
from dotenv import load_dotenv, find_dotenv

_ = load_dotenv(find_dotenv())
openai.api_key  = os.getenv('OPENAI_API_KEY')
```

`OPENAI_API_KEY` 是环境变量，保存着 API 密钥，可以从[API Keys](https://platform.openai.com/account/api-keys)菜单中获取，该菜单位于 OpenAI 网站上。

接下来的步骤是创建一个辅助函数来运行提示。

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
