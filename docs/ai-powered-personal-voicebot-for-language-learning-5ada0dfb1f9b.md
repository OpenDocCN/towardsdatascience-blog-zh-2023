# AI驱动的个人语音助手用于语言学习

> 原文：[https://towardsdatascience.com/ai-powered-personal-voicebot-for-language-learning-5ada0dfb1f9b?source=collection_archive---------2-----------------------#2023-08-28](https://towardsdatascience.com/ai-powered-personal-voicebot-for-language-learning-5ada0dfb1f9b?source=collection_archive---------2-----------------------#2023-08-28)

[](https://medium.com/@gamzsaglam?source=post_page-----5ada0dfb1f9b--------------------------------)[![Gamze Zorlubas](../Images/30d8e1604a22dc6f1bd3ef96c8092cc1.png)](https://medium.com/@gamzsaglam?source=post_page-----5ada0dfb1f9b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5ada0dfb1f9b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5ada0dfb1f9b--------------------------------) [Gamze Zorlubas](https://medium.com/@gamzsaglam?source=post_page-----5ada0dfb1f9b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd24f99cbdd78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fai-powered-personal-voicebot-for-language-learning-5ada0dfb1f9b&user=Gamze+Zorlubas&userId=d24f99cbdd78&source=post_page-d24f99cbdd78----5ada0dfb1f9b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5ada0dfb1f9b--------------------------------) · 11分钟阅读 · 2023年8月28日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5ada0dfb1f9b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fai-powered-personal-voicebot-for-language-learning-5ada0dfb1f9b&user=Gamze+Zorlubas&userId=d24f99cbdd78&source=-----5ada0dfb1f9b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5ada0dfb1f9b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fai-powered-personal-voicebot-for-language-learning-5ada0dfb1f9b&source=-----5ada0dfb1f9b---------------------bookmark_footer-----------)![](../Images/7ec79878a9cb3bf3da2b34b1d22f9b95.png)

由 Stable Diffusion XL 创建 — 图片由作者提供

掌握一门新语言的最有效方法是什么？就是说出来！但我们都知道，在别人面前尝试新单词和短语可能会很令人畏惧。如果你有一个耐心且理解的朋友来练习，没有评判，没有羞耻感，那会怎么样？

你寻找的那位耐心且理解的朋友可能正是一个由大型语言模型驱动的虚拟语言导师！这可能是掌握一门语言的终极方式，尽在你自己的空间内。

最近，大型语言模型已经进入我们的视野，它们正在改变我们的工作方式。这些强大的工具创建了可以像人类一样回应的聊天机器人，并迅速融入我们生活的各个方面，以数百种不同的方式使用。其中一个特别有趣的用途是在语言学习中，尤其是在口语练习方面。

当我不久前搬到德国时，我意识到学习一门新语言并找到练习口语的机会是多么具有挑战性。课程和语言小组可能昂贵或难以融入繁忙的日程。面对这些挑战，我有了一个想法：为什么不使用聊天机器人进行口语练习呢？单纯的文字交流是不够的，因为语言学习不仅仅涉及写作。因此，通过将AI驱动的聊天机器人与语音转文本和文本转语音技术结合起来，我成功创建了一个感觉像与真实人对话的学习体验。

在本文中，我将分享我选择的工具，解释过程，并介绍通过语音命令和语音回应与AI聊天机器人进行口语练习的概念。项目流程包括三个主要部分：语音转文本转录、使用语言模型和文本转语音转换。这些将在以下三个标题下进行解释。

# 1. 语音转文本转录

语言辅导中的语音识别作为用户语音输入与AI基于文本的理解之间的桥梁，用于生成回应。它是关键组件，使语音驱动的互动成为可能，从而提供更具沉浸感和有效的语言学习体验。

准确的转录对于与Chatbot的顺畅互动至关重要，尤其在语言学习的背景下，发音、口音和语法是关键因素。有各种语音识别工具可用于在Python中转录语音输入，如[OpenAI的Whisper](https://openai.com/research/whisper)和[Google Cloud的Speech-to-Text](https://cloud.google.com/speech-to-text/?utm_source=google&utm_medium=cpc&utm_campaign=emea-de-all-en-dr-bkws-all-all-trial-e-gcp-1011340&utm_content=text-ad-none-any-DEV_c-CRE_574560786447-ADGP_Hybrid+%7C+BKWS+-+EXA+%7C+Txt+%7E+AI+%26+ML+%7E+Speech-to-Text%23v19-KWID_43700053284440638-aud-606988877734%3Akwd-316837061214-userloc_9043449&utm_term=KW_google+audio+transcription-NET_g-PLAC_&gad=1&gclsrc=aw.ds)。

在为语言辅导项目选择语音识别工具时，需要考虑准确性、语言支持、成本以及是否需要离线解决方案等因素。

Google 提供了一个 [Python API](https://cloud.google.com/speech-to-text/)，需要互联网连接，并提供每月 60 分钟的免费转录服务。与 Google 不同，OpenAI 发布了他们的 Whisper 模型，你可以在本地运行它，只要你有足够的计算能力，就不依赖于互联网速度。这就是为什么我选择 Whisper 以尽可能减少转录延迟。

# 2\. 语言模型

语言模型是这个项目的核心。由于我已经非常熟悉 ChatGPT 及其 [API](https://openai.com/blog/introducing-chatgpt-and-whisper-apis)，我决定在这个项目中也使用它。不过，如果你有足够的计算能力，你也可以在本地部署 [Llama](https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGML)，这将是免费的。ChatGPT 需要花费一些钱，但使用起来更方便，因为你只需几行代码即可运行它。

为了提高响应的一致性并使其符合特定模板，你还可以对语言模型进行微调（例如，[如何根据你的使用情况对 ChatGPT 进行微调](https://www.enterprisebot.ai/blog/how-to-finetune-chatgpt-on-your-use-case)）。你需要生成示例句子和相应的最佳响应，并将其输入微调训练中。然而，我想要构建的基础教程不需要微调，我将在项目中使用通用的 GPT-3.5-turbo。

我将提供一个 API 调用示例，以方便通过其 API 在 Python 中实现用户与 ChatGPT 之间的对话。首先，如果你还没有一个，你需要开设一个 OpenAI 账户并设置 API 密钥以与 ChatGPT 进行交互。详细说明 [在这里](https://www.geeksforgeeks.org/how-to-use-chatgpt-api-in-python/)。

一旦你设置好 API 密钥，就可以使用 `openai.ChatCompletion.create` 方法开始生成文本。该方法需要两个参数：`model` 参数，指定要通过 API 访问的特定 GPT 模型，以及 `messages` 参数，包含与 ChatGPT 对话的结构。`messages` 参数由两个关键组成部分构成：`role` 和 `content`。

下面是一个代码片段来说明这个过程：

```py
# Initialize the messages with setting behavior(s).
messages = [{"role": "system", "content": "Enter behaviour(s) here."}]

# Start an infinite loop to continue the conversation with the user.
while True:
    content = input("User: ") # Get input from the user to respond.
    messages.append({"role": "user", "content": content})# Append the user's input to the messages.

    # Use the OpenAI GPT-3.5 model to generate a response to the user's input.
    completion = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=messages
    )

    chat_response = completion.choices[0].message.content # Extract the chat response from the API response.
    print(f'ChatGPT: {chat_response}') # Print the response.

    # Append the response to the messages with the role "assistant" to store the chat history.
    messages.append({"role": "assistant", "content": chat_response})
```

+   `system` 角色被定义用来通过在消息列表开头的内容中添加指令来确定 ChatGPT 的行为。

+   在聊天过程中，`user` 消息是通过提到的语音识别模型从用户处接收的，以便从 ChatGPT 获取响应。

+   最后，ChatGPT 的响应被附加到消息列表中的 `assistant` 角色，以记录对话历史。

# 3\. 文本转语音

在语音转文本转录部分，我解释了用户如何利用语音命令模拟对话体验，仿佛与真实的人交谈。为了进一步增强这种感觉并创建一个更动态和互动的学习体验，下一步涉及使用像gTTS这样的文本转语音工具将GPT的文本输出转换为可听的语音。这不仅有助于创建一个更具吸引力且易于跟随的体验，还解决了语言学习的一个关键方面：通过听力而不是阅读来理解。通过整合这一听觉组件，我们促进了更全面的实践，更贴近真实世界的语言使用。

有各种TTS工具可供选择，例如Google的Text-to-Speech（gTTS）和IBM Watson的Text to Speech。在这个项目中，我选择了gTTS，因为它超级易于使用，提供了自然的语音质量，而且完全免费。要使用gTTS库，你需要有互联网连接，因为该库需要访问Google的服务器来将文本转换为语音。

# 流程的详细说明

在我们深入探讨流程之前，你可能想先查看我在[Github](https://github.com/gamzez/language_tutor)页面上的完整代码，因为我将会引用其中的一些部分。

下图解释了AI驱动的虚拟语言导师的工作流程，该导师旨在建立一个实时的基于语音的对话学习体验：

![](../Images/23b3b3a1169fa7c5e3c19235f59c9e99.png)

流程图 — 作者提供的图片

+   用户通过开始录制他们的语音来启动对话，临时将其保存为.wav文件。这是通过按住空格键来实现的，录音在释放空格键时停止。下面解释了实现这种按键对话功能的Python代码部分。

以下全局变量用于管理录音过程的状态：

```py
recording = False       # Indicates whether the system is currently recording audio
done_recording = False  # Indicates that the user has completed recording a voice command 
stop_recording = False  # Indicates that the user wants to exit the conversation
```

`listen_for_keys`函数用于检查键盘的按压和释放。它根据空格键和esc按钮的状态设置全局变量。

```py
def listen_for_keys():
    # Function to listen for key presses to control recording
    global recording, done_recording, stop_recording
    while True:
        if keyboard.is_pressed('space'):  # Start recording on spacebar press
            stop_recording = False
            recording = True
            done_recording = False
        elif keyboard.is_pressed('esc'):  # Stop recording on 'esc' press
            stop_recording = True
            break
        elif recording:  # Stop recording on spacebar release
            recording = False
            done_recording = True
            break
        time.sleep(0.01)
```

`callback`函数用于处理录音时的音频数据。它检查`recording`标志以决定是否录制传入的音频数据。

```py
def callback(indata, frames, time, status):
    # Function called for each audio block during recording.
    if recording:
        if status:
            print(status, file=sys.stderr)
        q.put(indata.copy())
```

`press2record`函数是主要功能，负责处理当用户按住空格键时的语音录制。

它初始化全局变量以管理录音状态，并确定采样率，还创建一个临时文件来存储录制的音频。

函数随后打开一个SoundFile对象来写入音频数据，并使用先前提到的`callback`函数创建一个InputStream对象来从麦克风捕获音频。启动一个线程以监听按键，特别是录制的空格键和停止的'esc'键。在一个循环内，函数检查录制标志，并在录制活动时将音频数据写入文件。如果录制停止，函数返回-1；否则，它返回录制音频的文件名。

```py
def press2record(filename, subtype, channels, samplerate):
    # Function to handle recording when a key is pressed
    global recording, done_recording, stop_recording
    stop_recording = False
    recording = False
    done_recording = False
    try:
        # Determine the samplerate if not provided
        if samplerate is None:
            device_info = sd.query_devices(None, 'input')
            samplerate = int(device_info['default_samplerate'])
            print(int(device_info['default_samplerate']))
        # Create a temporary filename if not provided
        if filename is None:
            filename = tempfile.mktemp(prefix='captured_audio',
                                       suffix='.wav', dir='')
        # Open the sound file for writing
        with sf.SoundFile(filename, mode='x', samplerate=samplerate,
                          channels=channels, subtype=subtype) as file:
            with sd.InputStream(samplerate=samplerate, device=None,
                                channels=channels, callback=callback, blocksize=4096) as stream:
                print('press Spacebar to start recording, release to stop, or press Esc to exit')
                listener_thread = threading.Thread(target=listen_for_keys)  # Start the listener on a separate thread
                listener_thread.start()
                # Write the recorded audio to the file
                while not done_recording and not stop_recording:
                    while recording and not q.empty():
                        file.write(q.get())
        # Return -1 if recording is stopped
        if stop_recording:
            return -1

    except KeyboardInterrupt:
        print('Interrupted by user')

    return filename
```

最后，`get_voice_command`函数调用`press2record`来录制用户的语音命令。

```py
def get_voice_command():
    # ...
    saved_file = press2record(filename="input_to_gpt.wav", subtype = args.subtype, channels = args.channels, samplerate = args.samplerate)
    # ...
```

+   在捕捉并保存了语音命令到一个临时的.wav文件后，我们现在进入转录阶段。在这一阶段，录制的音频将通过Whisper转换成文本。下面是一个简单运行.wav文件转录任务的脚本：

```py
def get_voice_command():
    # ...
    result = audio_model.transcribe(saved_file, fp16=torch.cuda.is_available())
    # ...
```

这个方法接受两个参数：录制音频文件的路径`saved_file`和一个可选的标志，用于在CUDA可用的情况下使用FP16精度以增强兼容硬件上的性能。它简单地返回转录的文本。

+   然后，转录的文本被发送到ChatGPT，以在`interact_with_tutor()`函数中生成适当的回应。相关的代码段如下：

```py
def interact_with_tutor():
    # Define the system role to set the behavior of the chat assistant
    messages = [
        {"role": "system", "content" : "Du bist Anna, meine deutsche Lernpartnerin. 
                                        Du wirst mit mir chatten. Ihre Antworten werden kurz sein.
                                        Mein Niveau ist B1, stell deine Satzkomplexität auf mein Niveau ein. 
                                        Versuche immer, mich zum Reden zu bringen, indem du Fragen stellst, und vertiefe den Chat immer."}
    ]
    while True:
        # Get the user's voice command
        command = get_voice_command()  
        if command == -1:
            # Save the chat logs and exit if recording is stopped
            save_response_to_pkl(messages)
            return "Chat has been stopped."

        # Add the user's command to the message history
        messages.append({"role": "user", "content": command})  

        # Generate a response from the chat assistant
        completion = openai.ChatCompletion.create(
            model="gpt-3.5-turbo",
            messages=messages
        )  

        # Extract the response from the completion
        chat_response = completion.choices[0].message.content  # Extract the response from the completion
        print(f'ChatGPT: {chat_response} \n')  # Print the assistant's response
        messages.append({"role": "assistant", "content": chat_response})  # Add the assistant's response to the message history
        # ...
```

`interact_with_tutor`函数开始时定义ChatGPT的系统角色，以塑造其在对话中的行为。由于我的目标是练习德语，我相应地设置了系统角色。我称呼我的虚拟导师为“Anna”，并为她设置了我的语言熟练度，以便她调整她的回应。此外，我指示她通过提问保持对话的吸引力。

接下来，用户转录的语音命令以“user”的角色追加到消息列表中。然后将该消息发送给ChatGPT。由于对话在一个while循环中继续进行，用户命令和GPT回应的整个历史被记录在消息列表中。

+   在每次ChatGPT回应后，我们使用gTTS将文本消息转换为语音。

```py
def interact_with_tutor():
  # ...
  # Convert the text response to speech
  speech_object = gTTS(text=messages[-1]['content'],tld="de", lang=language, slow=False)
  speech_object.save("GPT_response.wav")
  current_dir = os.getcwd()
  audio_file = "GPT_response.wav"
  # Play the audio response
  play_wav_once(audio_file, args.samplerate, 1.0)
  os.remove(audio_file) # Remove the temporary audio file
```

`gTTS()`函数接受4个参数：`text`，`tld`，`lang`和`slow`。`text`参数被赋予`messages`列表中最后一条消息的内容（由`[-1]`表示），你想将其转换为语音。`tld`参数指定Google翻译服务的顶级域。将其设置为`"de"`意味着使用德语域，这对确保发音和语调适合德语非常重要。`lang`参数指定文本应以何种语言朗读。在此代码中，`language`变量设置为`'de'`，意味着文本将以德语朗读。`slow=False`：`slow`参数控制语音的速度。将其设置为`False`意味着语音将以正常速度朗读。如果设置为`True`，语音将更慢。

+   ChatGPT响应的转换语音将保存为临时的.wav文件，向用户播放，然后删除。

+   当用户再次按下空格键继续对话时，`interact_with_tutor`函数会重复运行。

+   如果用户按下“esc”，对话结束，并且整个对话保存到一个pickle文件`chat_log.pkl`中，您可以稍后用于分析。

# 命令行用法

要运行脚本，只需在终端中运行以下Python代码：

```py
sudo python chat.py
```

`sudo`是必需的，因为该脚本需要访问麦克风并使用键盘库。如果您使用Anaconda，您也可以通过“以管理员身份运行”来启动Anaconda终端，以赋予完全访问权限。

这里有一个演示视频展示了代码在我的笔记本电脑上的运行情况。您可以感受一下性能：

作者创建的演示视频

# 最后的备注

我通过简单设置ChatGPT的系统角色并调整gTTs函数中的参数，将导师的语言设置为德语。然而，您也可以轻松切换到其他语言。只需几秒钟即可为目标语言配置它。

如果您想要讨论特定主题，您也可以在ChatGPT的系统角色中添加它。例如，使用它练习面试可能是一个不错的用例。您还可以指定您的语言水平以调整其响应。

一个重要的备注是，聊天的整体速度取决于您的互联网连接（由于ChatGPT API和gTTS）以及您的硬件（由于Whisper的本地部署）。在我的情况下，我的输入后的整体响应时间在4-10秒之间。
