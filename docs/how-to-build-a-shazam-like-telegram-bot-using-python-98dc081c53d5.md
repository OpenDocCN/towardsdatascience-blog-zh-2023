# 如何使用 Python 构建一个类似 Shazam 的 Telegram 机器人

> 原文：[`towardsdatascience.com/how-to-build-a-shazam-like-telegram-bot-using-python-98dc081c53d5`](https://towardsdatascience.com/how-to-build-a-shazam-like-telegram-bot-using-python-98dc081c53d5)

## 一个教程，教你如何创建和部署一个可以实时接收音乐并帮助你查找歌曲标题和歌手的 Telegram 机器人

[](https://eugenia-anello.medium.com/?source=post_page-----98dc081c53d5--------------------------------)![Eugenia Anello](https://eugenia-anello.medium.com/?source=post_page-----98dc081c53d5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----98dc081c53d5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----98dc081c53d5--------------------------------) [Eugenia Anello](https://eugenia-anello.medium.com/?source=post_page-----98dc081c53d5--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----98dc081c53d5--------------------------------) ·阅读时间 9 分钟·2023 年 1 月 24 日

--

![](img/42e32cdfef8d602899d368abc141af42.png)

照片由 [Austin Neill](https://unsplash.com/@arstyy) 提供，来源于 [Unsplash](https://unsplash.com/photos/hgO1wFPXl3I)

当你在外面听到音乐，比如在酒吧或商店里，如果你非常喜欢这首歌，难道不会想知道这首歌的标题和歌手的名字吗？

我遇到过很多次这种情况，只有一个解决方案。Shazam。下载它并点击弹出按钮来识别音乐。

在这篇文章中，我们将构建一个类似的应用来发现歌曲和歌手。开发这个数据科学项目的一种快速简便的方法是使用 Python 创建一个 Telegram 机器人。

当你听音乐时，你可以录制声音。这个机器人会将音频转录成文本，并为你找到歌曲的标题和歌手。让我们开始吧！

## 目录：

+   **第一部分：用 Python 创建 Telegram 机器人**

+   **第二部分：将 Telegram 机器人部署到 Fly.io**

# 第一部分：用 Python 创建 Telegram 机器人

![](img/935e38aa1851d1376de171bdb5e0c6b4.png)

作者插图

本教程将分为两个部分。在第一部分中，我们将专注于创建一个可以转录音频并发现歌曲信息的 Telegram 机器人。之后，我们会将这个 Telegram 机器人部署到 fly.io 上，这个平台是众多支持云端应用构建、运行和扩展的平台之一。

+   **要求**

+   **安装库**

+   **创建 Telegram 机器人的第一步**

+   **转录歌曲的音频**

+   **了解有关歌曲的信息**

## **要求：**

在开始用 Python 编程之前，需要遵循一些步骤。

+   打开 Telegram，搜索 @botfather 并点击第一个结果。

![](img/89ebe7ebfee11ca39ed49a792ea0d9a9.png)

+   按下开始按钮以开始与 BotFather 对话。

![](img/a9f41f28ab708952446c933b9fec60e6.png)

+   输入`/newbot`并按照截图中的指示操作。创建机器人后，你将收到 BotFather 发给你的令牌，从而允许你访问 Telegram API。

![](img/95f6188ba265e10371d4f04f3d163509.png)

+   输入你的 Telegram 机器人名称。

![](img/dc4d237fe211c5d0fc89d07ac377ff51.png)

## **安装库**

首先，安装一个允许用少量代码创建 Telegram 机器人的 Python 库。它叫做**pyTelegramBotAPI**。

```py
pip install pyTelegramBotAPI
```

为了转录音频，我将使用**Steamship**的 API。在安装包之前，你需要安装 nodej。在 Windows 上，你需要从这里安装并将路径添加到 PATH 环境变量中，而在 ubuntu 上你需要运行两行代码：

```py
sudo apt install nodejs
sudo apt install npm
```

然后你可以在你的 Python 虚拟环境中安装 Steamship CLI。

```py
pip install steamship
npm install -g @steamship/cli
ship login
```

安装完成后，我们将进入第三个 API，它允许我们从音频中发现歌曲。这可以通过访问 Google 搜索结果来实现，使用的 API 叫做**SerpApi**。

```py
pip install google-search-results
```

## **创建 Telegram 机器人的第一步**

一旦你获得了 Telegram 的 API 令牌并创建了 Telegram 机器人，它应该会出现在你的 Telegram 应用中，我们可以开始探索[**pyTelegramBotAPI**](https://pypi.org/project/pyTelegramBotAPI/)，它提供了用少量代码创建 Telegram 机器人的 Python 实现。

还有许多其他不同的 Python 库可以用来构建 Telegram 机器人，但由于我发现许多教程使用了这个库，且文档做得很好，我选择了它来完成我的小项目。

```py
import os
import json
...
from steamship import Steamship, TaskState
import telebot
from serpapi import GoogleSearch

f = open("cred.json", "rb")
params = json.load(f)
BOT_TOKEN = params['BOT_TOKEN']
bot = telebot.TeleBot(BOT_TOKEN)
```

我们有一个文件`cred.json`，其中包含了机器人令牌以及 Steamship 和 SerpApi 的 API 密钥：

```py
{
"BOT_TOKEN":<your_bot_token>,
"API_KEY":<your_serpapi_key>,
"STEAM_API_KEY":<your_steamship_api_key>
 }
```

在第 11 行，我们简单地创建了一个`TeleBot`实例，它是一个提供处理 Telegram 消息功能的类。例如，让我们定义一个消息处理器，如果你在聊天中输入`/start`或`/hello`，它会返回消息“插入你的歌曲音频”。

```py
@bot.message_handler(commands=['start', 'hello'])
def send_welcome(message):
    bot.reply_to(message, "Insert the audio of your song!")

bot.infinity_polling()
```

在代码末尾，我们使用`bot.infinity_polling()`来启动机器人。

![](img/a6b60277203ad0829b5af830e3642555.png)

作者提供的截图

运行`python bot.py`后，你应该会看到类似于上图的结果。

在函数`send_welcome`之后，我们可以定义另一个函数，叫做`telegram_bot`，它将处理“语音”类型的音频文件。

```py
@bot.message_handler(content_types=['voice'])
def telegram_bot(message,bot_token=params["BOT_TOKEN"]):
    # insert audio
    file_info = bot.get_file(message.voice.file_id)
    # extract telegram's url of the audio 
    audio_url = 'https://api.telegram.org/file/bot{}/{}'.format(bot_token,file_info.file_path)

bot.infinity_polling()
```

我们希望获得你发送给机器人的音频文件的公开 URL。要检查 URL 是否正确，请在浏览器中复制以下路径，替换令牌和文件路径。如果有效，它应该会将音频下载到你的本地电脑。

```py
https://api.telegram.org/file/bot<bot_token>/<file_path>
```

我们需要做的最后一个操作是将 OGA 格式的音频转换为 MP3 格式。这是一个重要步骤，因为 OGA 文件不支持转录。

为了将音频转换为 mp3 文件，我们将使用 ffmpeg 包装器，它允许使用命令行转换各种音频文件。在使用之前，你需要先安装它。如果你在 Windows 上工作，请查看这个 [指南](https://www.wikihow.com/Install-FFmpeg-on-Windows)，它展示了所有步骤，而在 Linux 上，你只需使用命令 `sudo apt install ffmpeg`。

在函数 `convert_oga_to_mp3` 的第 4 行进行转换后，我们需要再次提取新音频文件的 URL，该文件的格式会有所不同。

## **转录音频的歌曲**

现在，我们已经到了教程中最有趣的部分，这使我们达到了 Telegram 机器人的目标。在继续之前，登录 [这里](http://hKFo2SBHcnZYcldQTWdySWwwUXFTMU9xYVhTcU5tTnkxOUlGMKFur3VuaXZlcnNhbC1sb2dpbqN0aWTZIFpPUXNzNnIzSEJzMzZnbmtaTGNRcndDbDRlSlhzSGp3o2NpZNkga21ZbWNKTFJpSzU4TER2WXR6cmNmQWlDUmNCMHVQR0s) 以获取 API 密钥。首先，我们想使用 Steamship API 转录音频。函数 `transcribe_audio` 以之前检索到的音频 URL 和 `Steamship` 的实例为输入，`Steamship` 是一个提供许多 AI 应用功能和方法的类。在这种情况下，它专注于音频转录。

代码可以用几行代码来总结：

+   在第 3 行，我们指定将使用 `audio_markdown` 包来转录音频，然后生成 Markdown 输出。

+   在下一行，我们通过调用 `invoke` 来调用方法 `transcribe_url`。我们还需要传递音频的 URL。

+   在函数的中间，我们调用方法 `get_markdown`，这将允许我们接收音频的转录。为了避免无限循环，如果不成功，重试次数被限制为 100。

+   该函数在最后返回转录结果，我们在 Telegram 机器人上获得结果文本。

## 发现有关歌曲的信息

在获得转录后，我们进入下一步，即发现歌曲的标题和歌手。这可以通过使用 **SerpApi** 来实现，它是一个 API，允许访问 Google 和其他网站的结果。

我们需要向 `GoogleSearch` 传递一个字典，包含如歌曲歌词、SerpApi 的 API 密钥等参数，并从 Google 获得结果。它返回一个 JSON 输出，可以转换为 Python 字典以获取信息。之后，一条文本消息将发送回发件人，这可能是未发现的/已实现的发现。

![](img/bad7d40ebf2e4fba5a8e5994d3dd797f.png)

应用程序应该如何工作的示意图。

# **第二部分：将 Telegram 机器人部署到 Fly.io**

到目前为止，我们可以通过运行以下命令行来使机器人工作：

```py
python bot.py
```

但这意味着机器人只有在你运行此代码时才会工作，这并不实用。最好是让 Telegram 机器人始终可用，让人们尝试。

因此，我们需要部署 Telegram 机器人。最初，我想使用 Heroku，这是另一个用于部署应用程序的闭源平台，但它不再提供免费层。

所以，我选择了 Fly.io 作为云服务，因为它提供了免费计划，并且文档齐全。

+   **创建 requirements.txt 和 Dockerfile**

+   **连接到 Fly.io**

+   **启动应用程序**

+   **部署应用程序**

## 1\. 创建 requirements.txt 和 Dockerfile

要将其投入生产，我们需要 `requirements.txt` 文件，其中包含所有的 Python 依赖项。可以通过在终端中运行命令行 `pipreqs` 自动创建该文件。你必须安装 [pipreqs](https://pypi.org/project/pipreqs/) 以使命令行有效。项目所需的 Python 包如下：

```py
google_search_results==2.4.1
pyTelegramBotAPI==4.9.0
python-dotenv==0.21.1
requests==2.28.1
steamship==2.3.5
```

除了这个文件，我们还需要另一个文件，名为 `Dockerfile`，其内容应如下：

```py
FROM python:3.10.2

WORKDIR /bot

COPY requirements.txt /bot/
RUN pip install -r requirements.txt
RUN apt-get update
RUN apt-get install ffmpeg -y
RUN apt-get install nodejs -y
RUN apt-get install npm -y

COPY . /bot

CMD python bot.py
```

## 2\. 连接到 Fly.io

第三个要求是安装 `flyctl`，一个可以部署我们应用程序的命令行工具。你可以在 [这里](https://fly.io/docs/hands-on/install-flyctl/) 找到说明，具体取决于你的操作系统。

如果你还没有创建 Fly.io 账户，你需要通过运行以下命令行来创建一个：

```py
flyctl auth signup
```

如果你已经有账户，你只需要通过终端复制登录：

```py
fly auth login
```

## 3\. 启动应用程序

要开始这个项目，我们可以输入以下命令行：

```py
flyctl launch
```

系统会要求你提供以下信息：

+   写下应用程序名称。

+   选择部署的区域。

+   如果你想要设置一个 Postgresql 数据库。此情况下，答案是否定的。

+   如果你现在想要设置 Upstash Redis 数据库。此情况下，答案是否定的。

+   如果我们现在想要部署的话。答案是现在不行。我们稍后再部署。

在部署应用程序之前，我还决定利用 fly.io 提供的一个功能，即秘密管理。正如你所知道的，凭据是敏感的，你可能希望不将你的秘密透露给除你自己以外的任何人。这可以通过以下命令行实现：

```py
flyctl secrets set BOT_TOKEN=<your_bot_token>
flyctl secrets set STEAM_TOKEN=<your_steam_api>
flyctl secrets set API_KEY=<your_serpapi_key>
```

运行这些命令行后，Python 代码需要进行修改。我们不再使用包含凭据的 JSON 文件，这些凭据直接存储在 fly.io 平台上。例如，我们可以通过以下方式获取 Telegram 机器人的令牌：

```py
from dotenv import load_dotenv

load_dotenv()
bot = telebot.TeleBot(os.getenv("BOT_TOKEN"))
```

你需要对其他 API 密钥做相同的操作。你可以通过以下命令行查看命令列表：

```py
flyctl secrets list
```

## 4\. 部署应用程序

最终，我们达到了珠穆朗玛峰的顶峰！只需再输入一条命令，就完成了：

```py
flyctl deploy
```

这样，我们的包含 Telegram 机器人的 Docker 容器就构建并部署完成了。代码将运行在云端，而不再是在本地计算机上。现在 Telegram 机器人将开始工作。

# 最终思考：

我希望你欣赏这个数据科学项目。它可以给你提供其他可能应用的想法。网上有很多资源可以满足你的需求。你只需要创造力、耐心和努力工作。如果你拥有这些条件，那么你能做的事没有限制。GitHub 代码在[这里](https://github.com/eugeniaring/shazam-like-telegram-bot)。感谢阅读。祝你有美好的一天！
