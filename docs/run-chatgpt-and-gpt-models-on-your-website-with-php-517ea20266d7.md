# 使用 PHP 在你的网站上运行 ChatGPT 和 GPT 模型

> 原文：[`towardsdatascience.com/run-chatgpt-and-gpt-models-on-your-website-with-php-517ea20266d7`](https://towardsdatascience.com/run-chatgpt-and-gpt-models-on-your-website-with-php-517ea20266d7)

## 一种非常简单的解决方案，将 GPT 模型的 AI 交付给你的用户

[](https://medium.com/@bnjmn_marie?source=post_page-----517ea20266d7--------------------------------)![Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----517ea20266d7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----517ea20266d7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----517ea20266d7--------------------------------) [Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----517ea20266d7--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----517ea20266d7--------------------------------) ·12 分钟阅读·2023 年 5 月 2 日

--

![](img/674c26153559ff00625bba226d4b3458.png)

图片来自[Pixabay](https://pixabay.com/illustrations/media-internet-message-network-3683580/)。

GPT 模型可以提升网站和网络应用的用户体验。它们可以翻译、总结、回答问题，还能完成许多其他任务。

将所有这些功能集成到你的在线服务中，通过 OpenAI API 相当简单。目前，OpenAI 仅提供对 Python 和 NodeJS 绑定的官方支持。

许多[第三方绑定](https://github.com/openai-php/client)已经由社区开发，以便在其他编程语言中进行部署。

在这篇文章中，我将展示如何将你的网站连接到 OpenAI 的 API。我还会解释如何解析和解读 API 返回的结果。

我将只涵盖 GPT 模型，但你可以使用相同的流程来处理 DALL-E 和 Whisper 模型。

# 先决条件

## GPT 模型

你不需要熟悉 GPT 模型就可以理解和实现这篇文章，但我仍建议你阅读我关于 GPT 模型的简单介绍：

[](/a-gentle-introduction-to-gpt-models-e02b093a495b?source=post_page-----517ea20266d7--------------------------------) ## GPT 模型的简单介绍

### 欢迎来到令牌生成器的新世界

towardsdatascience.com

## PHP

你只需了解 PHP 的基础知识。

我将使用一个可以通过 Composer 安装的 PHP 库（所以你需要 Composer），并且要求至少 PHP 8.1。*注意：你无法在旧版本的 PHP 上安装该库。*

## OpenAI 账户

你需要一个 OpenAI 账户。如果你没有，请参考我的指南，了解如何创建和管理 OpenAI 账户：

[## OpenAI 账户：文档、游乐场和模型的超参数](https://medium.com/@bnjmn_marie/openai-account-documentation-playground-and-models-hyperparameters-fb1dada13260?source=post_page-----517ea20266d7--------------------------------)

### 使用 OpenAI API 所需了解的全部内容

[medium.com](https://medium.com/@bnjmn_marie/openai-account-documentation-playground-and-models-hyperparameters-fb1dada13260?source=post_page-----517ea20266d7--------------------------------)

如果你想运行示例，你需要在账户中创建一个 API 密钥并保留几分钱的积分。

# OpenAI PHP

我们将使用由 [OpenAI PHP](https://github.com/openai-php/client)（MIT 许可证）维护的客户端与 OpenAI API 进行通信。

其他 PHP 库也能做到这一点，但我选择这个库是因为以下原因：

+   它由 OpenAI 列出，合理保证了这个库可以信任。

+   在所有 PHP 绑定 OpenAI API 的库中，它在 GitHub 上拥有最多的 stars。

+   它易于安装和使用。

+   它会定期更新，以考虑 API 的变化和新的 OpenAI 模型。

要安装它，打开终端，进入你的网站/应用程序父目录，并按如下方式运行 composer：

```py
composer require openai-php/client
```

如果没有任何错误，你可以开始使用 PHP 的 OpenAI API。

# 在 PHP 中设置你的 API 密钥

你必须在你的 OpenAI 账户中创建一个 API 密钥。

出于安全原因，我建议为每个你希望连接到 API 的 Web 应用程序创建一个新的 API 密钥。

如果你的某个产品发生了安全漏洞，你可以只销毁 OpenAI 账户中的密钥，而不会影响其他应用。

你不应该直接在 PHP 文件中写入这个密钥，而是使用操作系统环境变量来存储它。例如，在 Ubuntu/Debian 上，运行：

```py
export MY_OPENAI_KEY={your key}
#Replace {your key} by your OpenAI API key
```

在你的 PHP 脚本中，你可以使用以下方式获取这个环境变量的值：

```py
<?php
$yourApiKey = getenv('MY_OPENAI_KEY');
....//remainder of your script
?>
```

如果你无法访问操作系统环境变量，最简单的替代方案是在一个单独的文件中定义一个 PHP 常量，并在所有使用 API 的 PHP 脚本中引入该文件。

例如，创建一个文件“key.php”，最好不要放在你网站的主目录中，并写入：

```py
<?php
define('MY_OPENAI_KEY'. '{your key}');
?>
```

然后在所有将使用 API 的文件顶部写入以下内容：

```py
<?php
require_once("path/to/key.php"); //the path to your key.php file
$yourApiKey = MY_OPENAI_KEY;
....//remainder of your script
?>
```

# 使用 GPT 模型的 PHP 补全任务

OpenAI PHP 客户端支持通过 OpenAI API 访问的所有任务。在这篇文章中，我将重点讨论使用 GPT 模型的“补全任务”。

补全任务是指我们 **提示** 模型一个文本，API 通过在此提示后添加文本来作出回应。

API 提供了两种不同类型的补全任务：

+   standard: 提示 GPT-3 或 GPT-4 模型并生成跟随该提示的 tokens

+   chat: 给定一个描述对话历史的消息列表，模型将返回一个响应。因此，这里的提示是一组包含关于是模型还是用户写的信息的消息。

我将演示如何使用 OpenAI PHP 客户端来完成这两种类型的任务。

## 使用 GPT-3 完成任务

首先，我们需要一个目标。我们希望 GPT 模型完成什么？

对于这篇文章，我们可以设定目标是“将”文本翻译成表情符号。

使用 GPT 模型时最关键的步骤之一是找到适合我们任务的良好提示。如果你的提示不好，模型的回答也不会很出色。

*什么是好的提示？*

提示工程是一个非常活跃的研究领域。我不会在这里讨论这个话题，但我计划在我的下一篇文章中进行探讨。

对于我们的任务，[受到之前使用大型语言模型的机器翻译工作的启发](https://medium.com/towards-data-science/translate-with-chatgpt-f85609996a7f)，我提出了以下提示，取得了相当不错的结果：

> 将以下文本翻译成表情符号：
> 
> [TXT]

其中 [TXT] 将被替换为要翻译成表情符号的文本。

这个提示的优点是简短。使用它不会花费太多。

例如，我们将尝试将以下文本翻译成表情符号：

*我想要一个不加洋葱的汉堡。*

所以我们的提示变成了：

> 将以下文本翻译成表情符号：
> 
> 我想要一个不加洋葱的汉堡。

使用 OpenAI PHP 客户端，我们可以通过以下代码实现：

```py
<?php
//This line is necessary to load the PHP client installed by Composer
require_once('../vendor/autoload.php');

//Change the next line to $yourApiKey = MY_OPENAI_KEY; if you didn't use an environment variable and set your key in a separate file
$yourApiKey = getenv('MY_OPENAI_KEY');

//Create a client object
$client = OpenAI::client($yourApiKey);

//The $prompt variable stores our entire prompt
$prompt = "Translate the following text into emoji:

I would like an hamburger without onions.
";

//We send our prompt along with parameters to the API
//It creates a completion task
$result = $client->completions()->create([
    'model' => 'text-davinci-003',
    'prompt' => $prompt
]);

//After a few seconds the response will be stored in $results
//We can print the text answered by GPT
echo $result['choices'][0]['text']; 

?>
```

在这个代码中，我假设你在你的网站的根目录下。

它应该打印一系列表情符号。我得到了这个：

> *🍔🚫🧅*

你可能会得到不同的序列，因为 GPT 模型是“*非确定性的*”。

我使用了“text-davinci-003” GPT 模型，这是最强大的 GPT-3 模型。

如果你的任务非常简单，你可以使用更便宜的 GPT 模型。例如，我们可以尝试用“ada”替换“text-davinci-003”模型。

```py
'model' => 'ada',
```

我得到了以下回答：

> 例如，输入 这是文本 “Looking For a hamburger”

是的，这相当糟糕。这个回应中没有任何表情符号。选择正确的模型是你在将 OpenAI API 集成到产品中时必须做出的最关键的选择。

+   如果你选择一个旧的或小型的模型，结果会很低质量，并且可能无法完成请求的任务。

+   如果你选择一个更大的模型，你可能会得到最好的结果，但成本会更高。

你需要尝试多个模型，以确定哪个是最适合你目标的选项。作为起点，OpenAI 提供了一些使用[建议和可用模型列表](https://platform.openai.com/docs/models/overview)。

除了模型名称和提示，完成任务还可以接受更多参数。它们都在[API 文档](https://platform.openai.com/docs/api-reference/completions/create)中描述。

我们可以指定例如响应中的最大标记数，如下所示：

```py
$result = $client->completions()->create([
    'model' => 'text-davinci-003',
    'prompt' => $prompt,
    'max_tokens' => 2
]);
```

这不应该生成任何内容，只有 1 行空白。*为什么？*

1 个表情符号由 text-davinci-003 中的 3 个标记组成。所以如果我们将‘max_tokens’设置为 2，模型甚至无法生成 1 个表情符号。

*我怎么知道一个表情符号由 3 个标记组成？*

我在我的 OpenAI 用户账户的 playground 中简单检查了一下。例如，如果你在那里输入“🍔🚫🧅”，模型会计算出 9 个 tokens。

此外，GPT 模型在 emoji 序列前生成一个换行符。它算作一个额外的 token。总的来说，GPT 给了我 10 个 tokens 的回答。

请注意，“$result”变量包含所有这些信息。我们将在下面的下一部分中查看它。

但在此之前，让我们看看聊天完成任务。

## 聊天完成任务

聊天完成任务与我们使用 GPT-3 时略有不同。聊天任务由 gpt-3.5-turbo 提供支持，它也为 ChatGPT 提供支持。

在 gpt-3.5-turbo 中，“prompt” 参数被“messages” 替代。

从技术上讲，“messages” 是包含两个必需键和一个可选键的关联数组，如下所示：

+   role (required): 可以是“system”，“assistant”或“user”。在我撰写本文时，OpenAI 文档中几乎忽略了“system”。剩下的是“assistant”即模型，以及“user”即人类。

+   content (required): 这是我们放置提示或提示的上下文的地方，例如聊天历史。

+   name (optional): 如果你想给消息的作者指定一个特定的名字。

消息的长度和数量几乎是无限的。这样，gpt-3.5-turbo 可以接受非常长的聊天历史作为输入。

聊天完成可以执行与标准 GPT-3 相似的任务。在文档中，OpenAI 写了如下内容：

> 因为 `gpt-3.5-turbo` 的能力与 `text-davinci-003` 相似，但每个 token 的价格仅为 10%，所以我们推荐在大多数用例中使用 `gpt-3.5-turbo`。

让我们用翻译文本为 emoji 的任务来检查它。

我们只需进行少量修改：

```py
<?php
//This line is necessary to load the PHP client installed by Composer
require_once('../vendor/autoload.php');

//Change the next line to $yourApiKey = MY_OPENAI_KEY; if you set your key in a separate file
$yourApiKey = getenv('MY_OPENAI_KEY');

//Create a client object
$client = OpenAI::client($yourApiKey);

//The $prompt variable stores our entire prompt
$prompt = "Translate the following text into emoji:

I would like an hamburger without onions.
";

//We send our prompt along with parameters to the API
//It creates a chat completion task
$result = $client->chat()->create([
    'model' => 'gpt-3.5-turbo',
    'messages' => [
        ['role' => 'user', 'content' => $prompt],
    ],
]);

//After a few seconds the respone will be store in results
//We can print the text answer by GPT
echo $result['choices'][0]['message']['content']; 

?>
```

我获得了与 text-davinci-003 相同的答案，“🍔🚫🧅”，但价格仅为 text-davinci-003 的 10%。

现在你知道如何在 PHP 中与 OpenAI API 通信，我们可以更仔细地查看 API 返回的内容。正如我们将看到的，响应中包含有用的数据，我们可以用来监控 API 成本、跟踪用户活动（例如标记禁止的行为）等。

# 使用 PHP 解读 OpenAI API 响应

我们可以这样制作“$result”变量的可打印版本：

```py
print_r($result->toArray());
```

对于聊天完成任务，它将打印出如下内容：

```py
Array
(
    [id] => chatcmpl-7AJFw****
    [object] => chat.completion
    [created] => 1682691656
    [model] => gpt-3.5-turbo-0301
    [choices] => Array
        (
            [0] => Array
                (
                    [index] => 0
                    [message] => Array
                        (
                            [role] => assistant
                            [content] => 🍔🚫🧅
                        )

                    [finish_reason] => stop
                )

        )

    [usage] => Array
        (
            [prompt_tokens] => 23
            [completion_tokens] => 9
            [total_tokens] => 32
        )
```

*注意：我手动遮蔽了部分“id”。*

我们有以下条目：

+   id: OpenAI 为响应分配的唯一 ID。这些信息可以帮助跟踪 API 和用户之间的交互。

+   object: 执行的任务类型。

+   created: 响应创建的时间戳。

+   model: 用于生成响应的模型。

+   choices: 默认情况下，你将仅获得一个聊天完成任务的消息，除非你在调用 API 时更改“n”选项。

+   index: 从 0 开始的消息索引。

+   message: 关于生成的消息的信息。

+   role: 消息发送者的角色。

+   content: 消息本身。

+   finish_reason：API 停止生成消息的原因。默认情况下，它将是“stop”，即模型在没有任何约束的情况下停止生成。如果你在调用 API 时指定了“stop”参数，则可能会发生变化。然后，模型会在生成了你在“stop”中提到的一个标记后停止生成。

+   usage：有关令牌长度的信息。它可以用于监控 API 成本。

+   prompt_tokens：你提示中的令牌数量。

+   completion_tokens：API 生成的消息中的令牌数量。

+   total_tokens： “prompt_tokens”和“completion_tokens”的总和。

最重要的字段是“choices”，因为这是你将要交付给用户的内容，以及“usage”，因为这是唯一能够告诉你生成这个答案花费了多少的指标。

要知道 API 调用的确切成本，你必须将“total_tokens”的值乘以每个令牌的模型成本。注意 OpenAI 显示的是 1,000 个令牌的价格，因此你需要将这个数字除以 1,000 来获得每个令牌的价格。

例如，如果我们使用每 1,000 个令牌花费 $0.002 的模型，而“total_tokens”为 32，我们可以按如下方式计算总成本：

0.002 / 1000 * 32 = 0.000064

这个 API 调用将花费 $0.000064。

标准 GPT-3 完成的响应字段与聊天完成任务的字段几乎相同。

唯一显著的区别是，“text.completion”任务还可以返回 *t* 个最可能的令牌的日志概率。你可以在调用 API 时使用“logprobs”参数来指示“t”。*t* 的最大值是 5。*注意：OpenAI 的 API 参考文档表示，如果你的应用需要更大的值，你可以手动请求 OpenAI。*

# 在网页应用程序/网站中集成的下一步是什么？

我们已经学会了如何用 PHP 与 OpenAI API 通信。你的在线服务现在可以利用 GPT 模型的全部功能。

下一步将是实现前端。你不需要为此做过于复杂的事情。一个简单的 AJAX 脚本，例如使用 jQuery，就足够异步地从执行 API 调用的 PHP 脚本中获取响应。

它可以简单到这样：

```py
$.ajax({  
            type:"POST",  
            url:"call.php",  
            data:{ prompt: my_prompt //my_prompt stores the prompt
            },
            success:function(data){  
            data = $.parseJSON(data);
            $('#my_GPT_response').html(data["choices"][0]["message"]["content"]);
            }  
        }); 
```

这将把聊天完成的内容打印在一个 HTML 对象中，该对象的 id 属性设置为“my_GPT_response”。

你的 PHP 脚本必须接收“prompt”作为 $_POST 变量，并且 API 回答应该编码为 JSON 对象，如下所示：

```py
<?php
//This line is necessary to load the PHP client installed by Composer
require_once('../vendor/autoload.php');

//At least check that the prompt is sent
//Of course you should also check the content of the variable according to what you want to do with it
if (isset($_POST['prompt'])){
  //Change the next line to $yourApiKey = MY_OPENAI_KEY; if you set your key in a separate file
  $yourApiKey = getenv('MY_OPENAI_KEY');

  //Create a client object
  $client = OpenAI::client($yourApiKey);

  //The $prompt variable stores our entire prompt
  $prompt = "Translate the following text into emoji:

  ".$_POST['prompt']."
  ";

  //We send our prompt along with parameters to the API
  //It creates a chat completion task
  $result = $client->chat()->create([
      'model' => 'gpt-3.5-turbo',
      'messages' => [
          ['role' => 'user', 'content' => $prompt],
      ],
  ]);
  $result = $response->toArray();
  echo json_encode($result);
}

?>
```

总结这篇文章，我应该再次提到，你必须始终检查你发送给 API 的内容，以确保你没有违反 OpenAI 的政策和使用条款。

你可以利用 [审查模型](https://platform.openai.com/docs/api-reference/moderations)，这是 OpenAI 提供的免费服务，可以在将内容发送到 GPT 模型之前标记不安全的内容。

重要的是检查用户的年龄。[OpenAI 的使用条款](https://openai.com/policies/terms-of-use)禁止 13 岁以下的儿童使用其服务，而 18 岁以下的儿童只能在成人监督下使用这些服务。

*如果你喜欢这篇文章并且对接下来的文章感兴趣，支持我工作的最佳方式是通过这个链接成为 Medium 会员：*

[](https://medium.com/@bnjmn_marie/membership?source=post_page-----517ea20266d7--------------------------------) [## 通过我的推荐链接加入 Medium - 本杰明·玛丽

### 加入我们的 AI 社区，获取前沿研究成果。本博客旨在揭示最近在 AI 领域的进展……

medium.com](https://medium.com/@bnjmn_marie/membership?source=post_page-----517ea20266d7--------------------------------)

*如果你已经是会员并希望支持这项工作，* [*请在 Medium 上关注我*](https://medium.com/@bnjmn_marie)*。*
