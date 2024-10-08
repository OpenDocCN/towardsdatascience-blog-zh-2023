# 用 ChatGPT 超级提升你的电子表格

> 原文：[`towardsdatascience.com/supercharge-your-spreadsheets-with-chatgpt-4f49f7faf676`](https://towardsdatascience.com/supercharge-your-spreadsheets-with-chatgpt-4f49f7faf676)

## 重新定义你使用电子表格的方式。

[](https://sonery.medium.com/?source=post_page-----4f49f7faf676--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----4f49f7faf676--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4f49f7faf676--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4f49f7faf676--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----4f49f7faf676--------------------------------)

· 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4f49f7faf676--------------------------------) · 阅读时间 4 分钟 · 2023 年 5 月 29 日

--

![](img/0acc3162bcd438704499562553dfa515.png)

图片由 [Rubaitul Azad](https://unsplash.com/@rubaitulazad?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/GauA0hiEwDk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

插件数量的增加展示了 ChatGPT 或其他大型语言模型（LLMs）的强大功能。

这些插件使 OpenAI API 能够与各种工具集成，从而方便地使用 OpenAI 提供的模型，包括 GPT-3.5 和 GPT-4。

可以通过大型语言模型（LLMs）增强的工具之一是 Google 电子表格。电子表格的实用性和易于访问使其成为处理表格数据的顶级工具之一。当它们配备 LLMs 时，最终结果会令人震惊。

让我们设置“GPT for Sheets and Docs”插件，探索它如何在只需一次点击的情况下处理各种任务。

# 设置 GPT for Sheets and Docs

在浏览器中打开一个新的 Google 电子表格，在“扩展”菜单下找到“附加组件”，点击“获取附加组件”：

![](img/a3b694ebdef67c05ae852c4adb7d7f23.png)

(图片来源：作者)

在市场中找到“GPT for Sheets and Docs”并安装它。安装前务必阅读条款和条件，因为你需要授予它访问你 Google 账户中某些内容的权限：

![](img/68ddf5f8c89f532d02f98ce6cc60ceb1.png)

(图片来源：作者)

安装后，你需要提供一个 API 密钥，可以从 OpenAI 网站上的 [API Keys](https://platform.openai.com/account/api-keys) 菜单中获取。创建一个新密钥并复制它。然后，只需在插件的弹出菜单中输入该密钥即可。这里有一个逐步的 [指南](https://gptforwork.com/setup/how-to-set-up-api-key) 介绍如何为“GPT for Sheets and Docs”插件整合你的 API 密钥。这个操作只需要做一次，不是每次使用时都需要。

# 总结文本

假设你是产品经理，需要总结来自电子商务网站的大量产品评价。通常，阅读评价并提取关键点需要很长时间。

我们知道 LLM 在总结文本方面很出色。借助我们刚刚安装的插件，我们可以一键总结产品评价。

我们需要一个主提示语。我正在使用以下提示，但可以根据需要进行自定义。

*你的任务是为来自电子商务网站的给定产品评价生成一个简短的总结。最多使用 20 个词。*

然后，我们可以在`GPT`函数中使用以下提示语：

![](img/7cbee4a2b394a7101d812fdca7907357.png)

（作者提供的图片）

按下回车键，你就完成了。你可以像应用其他函数一样，将相同的提示语应用到所有单元格中。

这是我获得的 3 条产品评价的结果：

![](img/8c9b818b90d9f74a1b1b05cca79201f1.png)

（作者提供的图片）

如果你不想编写提示语，可以使用`GPT_SUMMARIZE`函数，它会总结给定单元格中的文本：

![](img/f072a7b3f49780902ed8a6540f997037.png)

（作者提供的图片）

我总是喜欢编写提示语，因为它可以让我根据需要自定义输出。

# 评价的情感

假设你不需要总结评价，而是想找出它们的情感。你可以自定义提示语，并以你喜欢的格式获得情感，例如 1 表示积极，0 表示消极。

我将使用以下提示语来提取评价的情感：

*你的任务是找出给定产品评价的情感。用以下一个词回答：positive，negative，neutral。*

这是两个产品评价的结果：

![](img/c70771a1053a4b654f39db33b990272b.png)

（作者提供的图片）

# 天高地迥

我们仅仅触及了我们讨论的两个用例的表面。将 LLM 集成到电子表格中有潜力加速各种任务。一旦你习惯了使用它，你会发现你的效率和效果达到新的高度。

你可以考虑在电子表格中使用 ChatGPT 插件的几种方式：

+   生成自动报告

+   数据验证

+   推测

+   扩展

真的，天高地迥。

它有可能重新定义我们与电子表格的工作方式，不仅提高我们的生产力，还提升我们的创造力。

*你可以成为* [*Medium 会员*](https://sonery.medium.com/membership) *以解锁我所有的写作内容，以及 Medium 的其余部分。如果你已经是会员，不要忘记* [*订阅*](https://sonery.medium.com/subscribe) *以便在我发布新文章时收到电子邮件。*

感谢阅读。如果你有任何反馈，请告诉我。
