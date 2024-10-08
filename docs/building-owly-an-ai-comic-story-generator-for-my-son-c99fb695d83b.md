# 为我儿子打造的 AI 漫画视频生成器

> 原文：[`towardsdatascience.com/building-owly-an-ai-comic-story-generator-for-my-son-c99fb695d83b`](https://towardsdatascience.com/building-owly-an-ai-comic-story-generator-for-my-son-c99fb695d83b)

## 利用在 Amazon SageMaker JumpStart 上精细调整的 Stable Diffusion 2.1，我开发了一种名为 Owly 的 AI 技术，能够制作带有音乐的个性化漫画视频，以我儿子的玩具作为主角。

[](https://agustinus-nalwan.medium.com/?source=post_page-----c99fb695d83b--------------------------------)![Agustinus Nalwan](https://agustinus-nalwan.medium.com/?source=post_page-----c99fb695d83b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c99fb695d83b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c99fb695d83b--------------------------------) [Agustinus Nalwan](https://agustinus-nalwan.medium.com/?source=post_page-----c99fb695d83b--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c99fb695d83b--------------------------------) ·24 分钟阅读·2023 年 4 月 11 日

--

![](img/e337e899f2f9184b93e905364049f799.png)

Owly AI 漫画故事讲述者 [AI 生成图像]

每天晚上，与我 4 岁的儿子 Dexie 分享睡前故事已经成为一种珍贵的例行公事，他非常喜欢这些故事。他的书籍收藏相当丰富，但他特别着迷于我从零开始编写的故事。以这种方式创作故事也让我能够融入我希望他学习的道德价值观，这些在商店购买的书籍中可能很难找到。随着时间的推移，我磨练了编写个性化叙事的技巧，点燃了他的想象力——从有裂缝的龙到寻找陪伴的孤独天灯。最近，我编造了像 Slow-Mo Man 和 Fart-Man 这样的虚构超级英雄故事，这些故事已经成为他的最爱。

尽管这段时间对我来说充满了乐趣，但在经过半年的每晚讲故事后，我的创意储备正在经受考验。为了让他持续获得新鲜而有趣的故事而不使自己精疲力竭，我需要一个更可持续的解决方案——一种能够自动生成引人入胜的故事的 AI 技术！我给她起了个名字叫 Owly，以他最喜欢的鸟类——猫头鹰来命名。

Pookie 和通往魔法森林的秘密门——由 AI 漫画生成器生成。

# 概念

当我开始组装我的愿望清单时，它很快膨胀起来，这种膨胀源于我对测试现代技术前沿的渴望。普通的文本故事是不够的——我设想了一个 AI 制作一个完整的漫画，最多可有 10 个面板。为了增加 Dexie 的兴奋感，我计划使用他熟悉和喜爱的角色，如 Zelda 和 Mario，甚至可以加入他的玩具。坦率地说，个性化的角度源于对漫画条纹的视觉一致性的需求，我稍后会深入讨论。但别急，这还不是全部——我还希望 AI 朗读故事，并配上合适的音乐来营造气氛。完成这个项目对我来说既有趣又具有挑战性，而 Dexie 将会享受到一场量身定制的互动故事盛宴。

![](img/df7699d891b1c4df5249699786443d18.png)

Dexie 的玩具作为漫画故事的主要角色 [作者提供的图片]

# 脑力风暴

为了征服上述要求，我意识到需要组装五个奇妙的模块：

1.  故事脚本生成器，编写多段故事，每段故事将被转换为漫画条纹部分。此外，它会推荐一种音乐风格，以从我的库中挑选合适的曲调。为了实现这一目标，我请来了强大的 OpenAI GPT3.5 大型语言模型 (LLM)。

1.  漫画图像生成器，为每个故事片段生成图像。Stable Diffusion 2.1 与 Amazon SageMaker JumpStart、SageMaker Studio 和 Batch Transform 联手，将这一切变为现实。

1.  文本转语音模块，将书面故事转换为音频叙述。Amazon Polly 的神经引擎跃然而出，提供了救援。

1.  视频制作器，将漫画条纹、音频叙述和音乐编织成一个自播放的杰作。MoviePy 是这个节目的明星。

1.  最终，控制器将这四个模块的宏伟交响曲协调起来，建立在强大的 AWS Batch 基础上。

游戏计划？让故事脚本生成器编织一个 7-10 段的叙述，每段转变为漫画条纹部分。然后，漫画图像生成器为每个片段生成图像，而文本转语音模块则制作音频叙述。根据故事生成器的推荐选择一段悦耳的音乐。最后，视频制作器将图像、音频叙述和音乐结合在一起，创造一个异想天开的视频。Dexie 将会在这个独一无二的互动故事冒险中获得极大的享受！

# 漫画图像生成器

在深入探讨故事脚本生成器之前，让我们首先探索图像生成模块，以便为任何关于图像生成过程的参考提供背景。有许多文本到图像的 AI 模型可供选择，但我选择了 Stable Diffusion 2.1 模型，因为它在使用 Amazon SageMaker 和更广泛的 AWS 生态系统进行构建、微调和部署方面非常受欢迎且容易。

Amazon SageMaker Studio 是一个集成开发环境（IDE），提供了一个统一的基于 Web 的界面，处理所有机器学习（ML）任务，简化数据准备、模型构建、训练和部署。这提高了数据科学团队的生产力，最多可提高 10 倍。在 SageMaker Studio 中，用户可以无缝上传数据、创建笔记本、训练和调整模型、调整实验、与团队协作，并将模型部署到生产环境中。

Amazon SageMaker JumpStart 是 SageMaker Studio 中的一个宝贵功能，提供了大量广泛使用的预训练 AI 模型。一些模型，包括 Stable Diffusion 2.1 base，可以使用你自己的训练集进行微调，并附带一个示例 Jupyter Notebook。这使得你可以快速高效地对模型进行实验。

![](img/4d446f4bbd308eb24a093ea3e66506d9.png)

在 Amazon SageMaker JumpStart 上启动 Stable Diffusion 2.1 Notebook [图片来源：作者]

我导航到 Stable Diffusion 2.1 base 视图模型页面，并通过点击 Open Notebook 按钮启动了 Jupyter Notebook。

![](img/f68f92e541642429129259ed5ff0db03.png)

Stable Diffusion 2.1 Base 模型卡 [图片来源：作者]

几秒钟内，Amazon SageMaker Studio 提供了示例笔记本，其中包含所有必要的代码，用于从 JumpStart 加载文本到图像模型、部署模型，甚至为个性化图像生成进行微调。

![](img/7922b6473c613664816317eaea0948be.png)

Amazon SageMaker Studio IDE [图片来源：作者]

有许多文本到图像的模型可用，许多模型由其创建者为特定风格量身定制。利用 JumpStart API，我筛选并列出了所有文本到图像的模型，使用 filter_value “task == txt2img” 并在下拉菜单中展示以便于选择。

```py
from ipywidgets import Dropdown
from sagemaker.jumpstart.notebook_utils import list_jumpstart_models

# Retrieves all Text-to-Image generation models.
filter_value = "task == txt2img"
txt2img_models = list_jumpstart_models(filter=filter_value)

# display the model-ids in a dropdown to select a model for inference.
model_dropdown = Dropdown(
    options=txt2img_models,
    value="model-txt2img-stabilityai-stable-diffusion-v2-1-base",
    description="Select a model",
    style={"description_width": "initial"},
    layout={"width": "max-content"},
)
display(model_dropdown)

# Or just hard code the model id and version=*. 
# Eg. if we want the latest 2.1 base model
self._model_id, self._model_version = (
    "model-txt2img-stabilityai-stable-diffusion-v2-1-base",
    "*",
)
```

我需要的模型是 model-txt2img-stabilityai-stable-diffusion-v2–1-base，它允许微调。

![](img/a191eade754916822826a83a3a418f1c.png)

大量文本到图像的模型 [图片来源：作者]

在不到 5 分钟的时间内，利用提供的代码，我将模型部署到了运行 g4dn.2xlarge GPU 实例的 SageMaker 端点。我迅速从我的文本提示中生成了第一张图片，你可以在下面查看展示效果。

![](img/0ffecd6b0b6b30089fc97a64ec9aeaa5.png)

我的图像生成器创建了一张海龟在水下游泳的图像 [图片来源：作者]

Amazon SageMaker Studio 简化了我的实验和原型制作过程，使我能够迅速尝试各种图像生成提示，并通过文件浏览器和预览窗口直接在 IDE 中查看生成的图像。此外，我可以在 IDE 中上传图像，利用内置终端启动 AWS CLI 将图像上传到 S3 存储桶或从中下载图像，并执行 SageMaker 批量转换作业以对模型生成大量图像进行大规模测试。

# **故事脚本生成器**

这个模块的任务非常简单：根据故事主题和角色名称制作故事脚本。使用 GPT3.5 API 生成关于特定主题的故事非常简单。

```py
openai.api_key = self._api_key
prompt = "Write me a 1000-word story about Bob the penguin who wants to travel to Europe to see famous landmarks"
response = openai.Completion.create(
    model="text-davinci-003",
    prompt=prompt,
    temperature=0.7,
    max_tokens=2089,
    top_p=1,
    frequency_penalty=0,
    presence_penalty=0
)
```

例如，使用提示“写一篇关于企鹅 Bob 想要去欧洲看著名地标的 1000 字故事。他发现自己的勇气和好奇心让他体验了许多令人兴奋的事情。” GPT3.5 会轻松地编写出引人入胜的故事，就像由专业讲故事的人撰写的一样，下面就是一个类似的例子。

> 企鹅 Bob 一直梦想着去欧洲旅行，参观著名的地标。他听朋友们讲过巴黎的埃菲尔铁塔、罗马的斗兽场和伦敦的大本钟。他对南极洲的日常生活感到厌倦，渴望冒险。
> 
> 一天，Bob 决定勇敢地开始规划他的旅行。他花了几个小时研究最佳的旅行路线和最实惠的住宿。经过仔细考虑，他决定从巴黎开始他的旅程。
> 
> 船程漫长而疲惫，但他对终于到达欧洲感到兴奋。他登记入住酒店后，立即出发去看埃菲尔铁塔。当他走在巴黎的街道上时，他感到前所未有的惊奇和兴奋。
> 
> 在接下来的几天里，他参观了像卢浮宫、巴黎圣母院和凡尔赛宫这样的著名地标。他尝试了新的食物，结识了新朋友，每一次经历都为他的冒险增添了色彩。

故事本身很棒，但为了将其转化为漫画，我需要将故事分成若干部分，并为每一部分创建一幅图像。最合逻辑的方法是将每个段落转换为一个部分。然而，正如你所见，从这些段落生成的图像呈现出一些重大挑战。

![](img/a794e2ca793257bf93cbc91d361dddd5.png)

我们的企鹅 Bob 被描绘成不同角色 [AI 生成的图像]

+   角色混乱出现了！每幅漫画中，Bob 都被描绘成完全不同的角色。在第一幅中，他是一个企鹅玩偶；在第二幅中，他是一个人的腿；在第三幅中，他是一个穿着西装的绅士；在第四幅中，他是一个穿蓝衬衫的男人。这种情况发生是因为只有第一段提到了“企鹅 Bob”，而第二段仅称他为“Bob”，其余段落则用“他”来指代。鉴于这些稀少的信息，难怪图像生成器将 Bob 以多种不同的形象呈现。

+   这些场景也缺乏焦点。第一幅漫画展示了一个企鹅玩偶坐在桌子上，而不是一个居住在南极的企鹅梦想着欧洲冒险。在第二幅漫画中也出现了类似的问题，展示了某个人的腿高高飞过城市。看来图像生成器将“一天，鲍勃决定冒险”理解成了从飞机跳伞，而该段落的重点应该是鲍勃计划他的欧洲之行。长段落中有多个焦点往往会使图像生成器感到困惑，导致图像脱离上下文。

为了解决场景缺乏焦点的问题并提高角色的一致性，我对提示进行了细化，要求每段包含一个简洁的一句话场景描述，格式为[]，以便可以程序化提取。这使我能够提供详细的示例，并指导图像生成器创建更集中和准确的图像。

> 对于每个部分，请详细描述场景，并始终在一段中包含位置的句子，格式为[a photo of character in the location]、[a photo of character in front of an object]、[a photo of character next to an object]、[a photo of a location]。

使用更新后的提示，这里是生成的故事。

> [鲍勃企鹅在南极的照片]
> 
> 鲍勃企鹅是一只快乐和好奇的企鹅，生活在南极。他对在那里与朋友和家人一起的生活感到满足。但有一天，他决定冒险，探索冰冷大陆之外的世界。他听说了许多美丽而异国情调的地方，并希望亲自体验。
> 
> [鲍勃企鹅在读书的照片]
> 
> 鲍勃企鹅开始研究世界，查看地图并阅读不同国家和文化的信息。他特别被欧洲吸引，那里的许多著名地标和景点让他向往。他决定欧洲是他想要访问的地方，于是开始计划他的旅行。
> 
> [鲍勃企鹅在游轮上的照片]
> 
> 他开始了漫长的船程。他感到兴奋，迫不及待地想要到达那里，并且决心去欧洲。在几周的旅行后，他终于到达了目的地。
> 
> [鲍勃企鹅在埃菲尔铁塔的照片]
> 
> 鲍勃企鹅开始探索欧洲，对他访问的各种不同地方感到惊叹。他去了巴黎的埃菲尔铁塔、罗马的斗兽场和爱尔兰的莫赫悬崖。他去的每个地方都让他充满了敬畏和喜悦。

正如你所观察到的，生成的场景描述要集中得多。它们提到单一场景、位置和/或正在进行的活动，通常以角色的名字开头。这些简洁的提示对于我的图像生成器效果显著，如下所示的改进图像就是证明。

![](img/f68791a0532c23e24307fe61b207cef9.png)

我们的 Bob 企鹅的更一致外观 [AI 生成的图像]

Bob 企鹅已成功回归，但他在每个漫画条幅中依然呈现出全新的形象。由于图像生成过程将每张图像视为独立的，而且没有提供有关 Bob 的颜色、大小或企鹅类型的信息，因此一致性仍然难以实现。

我之前考虑过将详细的角色描述作为故事生成的一部分，以保持图像中的角色一致性。然而，这种方法由于两个原因被证明是不切实际的：

+   有时，很难在不 resorting 到大量文本的情况下详细描述一个角色。虽然企鹅的种类可能不多，但考虑到鸟类的一般情况——有着无数形状、颜色和品种如葵花鹦鹉、鹦鹉、金丝雀、鹈鹕和猫头鹰，这项任务变得令人生畏。

+   生成的角色不总是符合提示中提供的描述。例如，一个描述绿色鹦鹉带红色喙的提示可能会生成一只绿色鹦鹉带黄色喙的图像。

所以，尽管我们尽了最大努力，我们的企鹅朋友 Bob 仍然经历着某种身份危机。

# 为图像生成器添加个性化

我们解决企鹅困境的方法在于为 Stable Diffusion 模型提供一个关于我们企鹅角色应有外观的视觉提示，以影响图像生成过程并保持生成图像的一致性。在 Stable Diffusion 的世界中，这一过程被称为微调，你需要提供一些（通常是 5 到 15 张）包含相同对象的图像以及描述它的句子。这些图像将被称为训练图像。

结果是，这种个性化不仅仅是解决方案，还是我漫画生成器的一个非常酷的功能。现在，我可以将 Dexie 的许多玩具作为故事中的主要角色，例如他节日的圣诞企鹅，为 Bob 企鹅注入新生命，使其对我年轻但坚韧的观众更加个性化和易于关联。因此，一致性的追求变成了为定制故事的胜利！

![](img/260183bbcad7490bf76ee61a63799b66.png)

Dexie 的玩具现在是 Bob 企鹅 [作者提供的图像]

在我令人兴奋的实验日子里，我发现了一些智慧的结晶可以分享，以在微调模型时实现最佳结果，减少过拟合的可能性：

+   保持训练图像中的背景多样化。这样，模型就不会将背景与对象混淆，防止在生成图像中出现不必要的背景配角。

+   从不同角度捕捉目标对象。这有助于提供更多视觉信息，使模型能够生成具有更大角度范围的对象，从而更好地匹配场景。

+   混合特写镜头和全身镜头。这确保了模型不会假设特定姿势是必要的，从而为生成的对象与场景的和谐提供了更多灵活性。

为了执行稳定扩散模型的精细调整，我启动了一个 SageMaker Estimator 训练作业，使用 Amazon SageMaker Python SDK 在 ml.g5.2xlarge GPU 实例上，并将训练过程定向到 S3 桶中的训练图像集合。生成的精细调整模型文件将保存在 s3_output_location 中。仅需几行代码，魔力便开始显现！

```py
# [Optional] Override default hyperparameters with custom values
hyperparams["max_steps"] = 400
hyperparams["with_prior_preservation"] = False
hyperparams["train_text_encoder"] = False

training_job_name = name_from_base(f"stable-diffusion-{self._model_id}-transfer-learning")

# Create SageMaker Estimator instance
sd_estimator = Estimator(
    role=self._aws_role,
    image_uri=image_uri,
    source_dir=source_uri,
    model_uri=model_uri,
    entry_point="transfer_learning.py",  # Entry-point file in source_dir and present in train_source_uri.
    instance_count=self._training_instance_count,
    instance_type=self._training_instance_type,
    max_run=360000,
    hyperparameters=hyperparams,
    output_path=s3_output_location,
    base_job_name=training_job_name,
    sagemaker_session=session,
)

# Launch a SageMaker Training job by passing s3 path of the training data
sd_estimator.fit({"training": training_dataset_s3_path}, logs=True)
```

为准备训练集，确保它包含以下文件：

1.  一系列名为 instance_image_x.jpg 的图像，其中 x 是从 1 到 N 的数字。在这种情况下，N 代表图像的数量，理想情况下应大于 10。

1.  一个 dataset_info.json 文件，其中包含一个必需字段，称为 instance_prompt。该字段应提供对象的详细描述，并在对象名称前加上唯一标识符。例如，“Bob 企鹅的照片”，其中‘Bob’充当唯一标识符。通过使用该标识符，你可以引导精细调整后的模型生成标准企鹅（称为“企鹅”）或来自训练集中的企鹅（称为“Bob 企鹅”）。一些来源建议使用像 sks 或 xyz 这样的唯一名称，但我发现这样做并非必要。

dataset_info.json 文件还可以包含一个可选字段，称为 class_prompt，它提供对象的一般描述，而没有唯一标识符（例如，“一只企鹅的照片”）。此字段仅在 prior_preservation 参数设置为 True 时使用；否则，将被忽略。我将在下面的高级精细调整部分进一步讨论。

```py
{"instance_prompt": "a photo of bob penguin",
   "class_prompt": "a photo of a penguin"
}
```

经过几次使用 Dexie 的玩具进行测试后，图像生成器产生了一些真正令人印象深刻的结果。它将 Dexie 的袋鼠磁性积木创作栩栩如生地呈现出来，使其跃入虚拟世界。生成器还巧妙地描绘了他心爱的淋浴乌龟玩具在水下游泳的场景，周围环绕着色彩斑斓的鱼群。图像生成器确实捕捉到了 Dexie 玩耍时最喜欢的魔力！

![](img/4595906178a31df2bd32e1b36bf7deb1.png)

Dexie 的玩具栩栩如生 [AI 生成的图像]

**针对精细调整的稳定扩散模型进行批量转换**

由于我需要为每个漫画条生成一百多张图像，因此部署 SageMaker 端点（可以将其视为 Rest API）并一次生成一张图像并不是最有效的方法。因此，我选择对我的模型进行批量转换，向其提供包含生成图像提示的 S3 桶中的文本文件。

我会提供更多关于这个过程的细节，因为我最初对它感到困惑，我希望我的解释能为你节省一些时间。你需要为每个图像提示准备一个文本文件，文件中包含以下 JSON 内容：{“prompt”: “a photo of Bob the penguin in Antarctica”}。虽然似乎有一种方法可以使用 MultiRecord 策略将多个输入合并到一个文件中，但我未能弄清楚它的工作原理。

我遇到的另一个挑战是对我的微调模型执行批量转换。你不能使用 `Estimator.transformer()` 返回的转换器对象执行批量转换，这通常在我之前的项目中有效。相反，你需要先创建一个 SageMaker 模型对象，将微调模型的 S3 位置指定为 `model_data`。之后，你可以使用这个模型对象创建转换器对象。

```py
def _get_model_uris(self, model_id, model_version, scope):
    # Retrieve the inference docker container uri
    image_uri = image_uris.retrieve(
        region=None,
        framework=None,  # automatically inferred from model_id
        image_scope=scope,
        model_id=model_id,
        model_version=model_version,
        instance_type=self._inference_instance_type,
    )
    # Retrieve the inference script uri. This includes scripts for model loading, inference handling etc.
    source_uri = script_uris.retrieve(
        model_id=model_id, model_version=model_version, script_scope=scope
    )
    if scope == "training":
        # Retrieve the pre-trained model tarball to further fine-tune
        model_uri = model_uris.retrieve(
            model_id=model_id, model_version=model_version, model_scope=scope
        )
    else:
        model_uri = None

    return image_uri, source_uri, model_uri

image_uri, source_uri, model_uri = self._get_model_uris(self._model_id, self._model_version, "inference")

# Get model artifact location by estimator.model_data, or give an S3 key directly
model_artifact_s3_location = f"s3://{self._bucket}/output-model/{job_id}/{training_job_name}/output/model.tar.gz"

env = {
    "MMS_MAX_RESPONSE_SIZE": "20000000",
}

# Create model from saved model artifact
sm_model = model.Model(
    model_data=model_artifact_s3_location,
    role=self._aws_role,
    entry_point="inference.py",  # entry point file in source_dir and present in deploy_source_uri
    image_uri=image_uri,
    source_dir=source_uri,
    env=env
)

transformer = sm_model.transformer(instance_count=self._inference_instance_count, instance_type=self._inference_instance_type,
                                output_path=f"s3://{self._bucket}/processing/{job_id}/output-images",
                                accept='application/json')
transformer.transform(data=f"s3://{self._bucket}/processing/{job_id}/batch_transform_input/",
                      content_type='application/json')
```

就这样，我的定制图像生成器已经准备好了！

**高级 Stable Diffusion 模型微调**

虽然这对我的漫画生成器项目并非必需，但我想提到一些涉及 `max_steps`、`prior_reservation` 和 `train_text_encoder` 超参数的高级微调技术，以防它们对你的项目有用。

Stable Diffusion 模型的微调由于你提供的训练图像数量与基础模型使用的图像数量之间的巨大差异，非常容易过拟合。例如，你可能只提供了 10 张 Bob the penguin 的图像，而基础模型的训练集包含了数千张企鹅图像。图像数量更多会降低过拟合的可能性，并减少目标对象与其他元素之间的错误关联。

当将 `prior_reservation` 设置为 `True` 时，Stable Diffusion 使用提供的 `class_prompt` 生成默认数量的 x 张图像（通常为 100），并在微调过程中将这些图像与 `instance_images` 结合起来。或者，你可以通过将这些图像放置在 `class_data_dir` 子文件夹中来手动提供这些图像。根据我的经验，`prior_preservation` 在对 Stable Diffusion 进行微调以生成真人面孔时通常至关重要。当使用 `prior_reservation` 时，确保提供一个提到最合适的通用名称或与角色相似的常见对象的 `class_prompt`。对于 Bob the penguin，这个对象显然是一只企鹅，所以你的类提示应该是 “a photo of a penguin”。这种技术也可以用来生成两个角色之间的混合体，稍后我会讨论。

另一个对高级微调有帮助的参数是 `train_text_encoder`。将其设置为 `True` 以启用在微调过程中对文本编码器的训练。结果模型将更好地理解更复杂的提示，并更准确地生成真人面孔。

根据你具体的使用案例，不同的超参数值可能会产生更好的结果。此外，你需要调整 max_steps 参数来控制所需的微调步骤数。请记住，较大的训练集可能会导致过拟合。

# 文本转语音和视频生成

通过利用 Amazon Polly 的神经文本转语音（NTTS）功能，我能够为故事的每个段落创建音频叙述。音频叙述的质量非常出色，因为它听起来非常自然和像人类发声，使其成为理想的讲故事者。

为了适应年轻观众，如 Dexie，我采用了 SSML 格式，并利用了 <prosody rate> 标签将说话速度降低到正常速度的 90%，确保内容不会传递得太快，以便他们能够跟上。

```py
self._pollyClient = boto3.Session(
                region_name=aws_region).client('polly')
ftext = f"<speak><prosody rate=\"90%\">{text}</prosody></speak>"
response = self._pollyClient.synthesize_speech(VoiceId=self._speaker,
                OutputFormat='mp3',
                Engine='neural',
                Text=ftext,
                TextType='ssml')

with open(mp3_path, 'wb') as file:
    file.write(response['AudioStream'].read())
    file.close()
```

在所有辛勤工作之后，我使用了 MoviePy —— 一个非常棒的 Python 框架 —— 神奇地将所有照片、音频叙述和音乐转变成一个令人惊叹的 mp4 视频。说到音乐，我让我的技术选择完美的配乐来匹配视频的氛围。怎么做的呢？好吧，我只是修改了我的故事脚本生成器，使用一些巧妙的提示从预定列表中返回一种音乐风格。这有多酷？

> 在故事开始时，请从以下列表中建议匹配故事的歌曲风格，并用<>标出。歌曲风格列表包括 action、calm、dramatic、epic、happy 和 touching。

一旦选择了音乐风格，下一步是从相关文件夹中随机挑选一个 MP3 曲目，该文件夹包含了一些 MP3 文件。这有助于为最终产品增添一点不可预测性和兴奋感。

# 控制模块

为了协调整个系统，我需要一个以 Python 脚本形式存在的控制模块，它能够无缝地运行每个模块。当然，我还需要一个计算环境来执行这个脚本。我有两个选择 — 第一个是我偏好的选择 — 使用 AWS Lambda 的无服务器架构。这涉及使用多个 AWS Lambda，配合 SQS。第一个 Lambda 作为公共 API，使用 API Gateway 作为入口点。这个 API 会接收训练图像的 URL 和故事主题文本，并对数据进行预处理，将其放入 SQS 队列中。另一个 Lambda 会从主题中提取数据并进行数据准备 —— 比如图像调整大小、创建 dataset_info.json，并触发下一个 Lambda 调用 Amazon SageMaker Jumpstart 来准备 Stable Diffusion 模型，并执行 SageMaker 训练作业以微调模型。唷，这真是一口气说完。最后，Amazon EventBridge 将作为事件总线，用于检测训练作业的完成，并触发下一个 Lambda 使用微调后的模型执行 SageMaker Batch Transform 生成图像。

然而，这个选项不可行，因为 AWS Lambda 函数的最大存储限制为 10GB。在对 SageMaker 模型执行批处理转换时，SageMaker Python SDK 会在本地 /tmp 临时下载并提取 model.tar.gzip 文件，然后将其发送到运行批处理转换的管理系统。不幸的是，我的模型压缩后为 5GB，因此 SageMaker Python SDK 抛出一个错误，说“磁盘空间不足。”对于大多数模型尺寸较小的使用场景，这将是最佳和最清洁的解决方案。

因此，我不得不选择第二个选项——AWS Batch。它运行良好，但成本稍高，因为 AWS Batch 计算实例必须在整个过程中运行——即使在对模型进行微调和执行批处理转换时，这些操作都是在 SageMaker 内的独立计算环境中执行的。我本可以将过程拆分为多个 AWS Batch 实例，并通过 Amazon EventBridge 和 SQS 将它们连接起来，就像我之前使用无服务器方法一样。但由于 AWS Batch 启动时间较长（约 5 分钟），这会给整体过程增加过多的延迟。因此，我选择了集成的 AWS Batch 选项。

![](img/53df06845e734287275aa29ada8b07b1.png)

Owly 系统架构

欣赏 Owly 壮丽的架构图吧！我们的冒险从通过 AWS 控制台启动 AWS Batch 开始，为其配备充满训练图像的 S3 文件夹、一个引人入胜的故事主题和一个令人愉快的角色，这些都通过 AWS Batch 环境变量提供。

```py
# Basic settings
JOB_ID = "penguin-images" # key to S3 folder containing the training images
STORY_TOPIC = "bob the penguin who wants to travel to Europe"
STORY_CHARACTER = "bob the penguin"

# Advanced settings
TRAIN_TEXT_ENCODER = False
PRIOR_RESERVATION = False
MAX_STEPS = 400
NUM_IMAGE_VARIATIONS = 5
```

AWS Batch 立即启动，从 JOB_ID 指定的 S3 文件夹中检索训练图像，将其调整为 768x768，并在将其放入暂存 S3 桶之前创建 dataset_info.json 文件。

接下来，我们调用 OpenAI GPT3.5 模型 API 来编写一个引人入胜的故事，并与所选主题和角色相协调的补充歌曲风格。然后我们召唤 Amazon SageMaker JumpStart 来释放强大的 Stable Diffusion 2.1 基础模型。使用该模型，我们启动 SageMaker 训练作业，对其进行微调以适应我们精心挑选的训练图像。经过短短 30 分钟的间歇后，我们为每个故事段落制作图像提示，以文本文件的形式，然后将其放入 S3 桶中，作为图像生成盛宴的输入。Amazon SageMaker Batch Transform 被用来在短短 5 分钟内批量生成这些图像。

完成后，我们借助 Amazon Polly 为故事中的每个段落制作音频讲解，仅需 30 秒即可将其保存为 mp3 文件。然后我们从按歌曲风格分类的库中随机挑选一个 mp3 音乐文件，基于我们巧妙的故事生成器的选择。

最终的工作将生成的图像、音频讲述 mp3 文件和音乐.mp3 文件巧妙地编织成一个视频幻灯片，借助 MoviePy 完成。为了增加优雅感，我们添加了平滑的过渡和肯·伯恩斯效果。最后的杰作，完成的视频，随后被上传到输出 S3 存储桶，等待你的急切下载！

# 测试并添加一些增强功能

我必须说，我对结果感到相当满意！故事脚本生成器真的是超乎预期地表现出色。几乎每一个制作的故事脚本不仅写得很好，而且充满了积极的道德观，展示了大型语言模型（LLM）的令人惊叹的能力。至于图像生成，嘛，这就有点参差不齐了。

根据我之前描述的所有改进，一五分之一的故事可以直接用于最终视频。其余的四分之一，通常有一到两张图像存在常见问题。

+   首先，我们仍然遇到不一致的角色。有时模型会生成一个与训练集中原始角色略有不同的角色，通常选择现实主义版本而不是玩具版本。但不要担心！在文本提示中添加期望的照片风格，如“一个卡通风格的海龟雷克斯在海里游泳”，有助于缓解这个问题。然而，这确实需要人工干预，因为某些角色可能需要现实主义风格。

+   然后就是缺失身体部位的奇特情况了。偶尔，我们生成的角色会出现四肢或头部缺失的情况。哎呀！为了解决这个问题，我们在稳定扩散模型中添加了负面提示，比如“缺失四肢，缺失头部”，以鼓励生成避免这些奇特属性的图像。

![](img/23405b7a5b6b62c47bf19fd3a0880da3.png)

雷克斯海龟的不同风格（右下角的图像为现实主义风格，右上角的图像为混合风格，其余为玩具风格）和缺少头部（左上角的图像）[AI 生成的图像]

+   当处理对象之间不常见的互动时，会出现奇异的图像。生成角色在特定位置的图像通常会得到令人满意的结果。然而，当涉及到描绘角色与其他对象互动时，尤其是以不寻常的方式，结果往往不尽如人意。例如，尝试描绘刺猬汤姆挤奶牛时，会产生刺猬与奶牛的奇特混合体。同时，制作汤姆·刺猬拿着花束的图像则会变成一个人同时抓着刺猬和花束。不幸的是，我尚未制定出解决这一问题的策略，这使我得出结论，这是当前图像生成技术的局限。如果你试图生成的图像中的对象或活动非常不寻常，模型缺乏先验知识，因为训练数据中从未出现过这样的场景或活动。

![](img/a53697ea5844f4941eb1f4e660e0a834.png)

从“汤姆刺猬在挤奶牛”提示生成的混合体（顶部图像）是刺猬和奶牛的混合。 从“汤姆刺猬拿着花”生成的图像（底部左侧图像）则是一个人同时抱着刺猬和花束 [AI 生成的图像]

最终，为了提高故事生成的成功率，我巧妙地调整了我的故事生成器，使其每段生成三个不同的场景。此外，对于每个场景，我指示我的图像生成器创建五个图像变体。通过这种方法，我增加了从十五个可用图像中获得至少一个优质图像的可能性。拥有三种不同的提示变体也有助于生成完全独特的场景，尤其是当一个场景过于稀有或复杂以至于无法创建时。以下是我更新后的故事生成提示。

```py
"Write me a {max_words} words story about a given character and a topic.\nPlease break the story down into " \
"seven to ten short sections with 30 maximum words per section. For each section please describe the scene in " \
"details and always include the location in one sentence within [] with the following format " \
"[a photo of character in the location], [a photo of character in front of an object], " \
"[a photo of character next to an object], [a photo of a location]. Please provide three different variations " \
"of the scene details separated by |\\nAt the start of the story please suggest song style from the following " \
"list only which matches the story and put it within <>. Song style list are action, calm, dramatic, epic, " \
"happy and touching."
```

唯一的额外成本是在图像生成步骤完成后进行一点手动干预，我会挑选每个场景的最佳图像，然后继续进行漫画生成过程。尽管有这一小小的不便，我现在在制作精彩漫画方面的成功率达到了 9/10！

# 上演时刻

随着 Owly 系统的全面组装，我决定在一个美好的星期六下午对这项技术奇迹进行测试。我从他的玩具收藏中生成了一些故事，准备利用我购买的巧妙便携式投影仪来提升 Dexie 的睡前故事体验。那天晚上，当我看到 Dexie 的脸上绽放出兴奋的光芒和眼睛睁大的神情，漫画在他的卧室墙上播放时，我知道我的努力都值得了。

![](img/9232d0f6ba9ade5709222aa2a16ab996.png)

Dexie 正在看着他卧室墙上的漫画 [图片来源于作者]

最棒的是，现在我只需不到两分钟就能利用我已经拍摄的玩具角色照片创作一个新故事。此外，我可以无缝地将我希望他从每个故事中学到的宝贵道德融入其中，比如不与陌生人交谈、勇敢冒险或对他人友善和乐于助人。以下是这个神奇系统生成的一些令人愉快的故事。

《超级刺猬汤姆拯救他的城市免受龙的威胁》 — 由 AI 漫画生成器生成。

《勇敢的企鹅鲍勃：欧洲历险记》 — 由 AI 漫画生成器生成。

# 一些有趣的实验

作为一个好奇的发明家，我忍不住尝试图像生成模块，以推动 Stable Diffusion 的边界，并将两个角色合并成一个宏伟的混合体。我用科瓦兹·奥克诺特的图像对模型进行了微调，但我加了一点变化，把塞尔达设定为既独特又经典的角色名。设置`prior_preservation`为 True，我确保 Stable Diffusion 会“奥克诺特化”塞尔达，同时保持她的独特本质。

我巧妙地利用了适度的`max_step`为 400，刚好能够保留塞尔达的原始魅力，同时避免她被科瓦兹·奥克诺特的无法抗拒的魅力完全吞噬。请看塞尔达与科瓦兹的辉煌融合，合而为一！

Dexie 兴奋不已地见证了他最喜欢的两个角色在他的睡前故事中大展拳脚。他踏上了激动人心的冒险，打击外星生物并寻找隐藏的宝藏！

不幸的是，为了保护知识产权，我不能展示生成的图像。

生成型 AI，特别是大型语言模型（LLMs），将长期存在，并成为不仅仅是软件开发，还包括许多其他行业的强大工具。我在一些项目中亲身体验了 LLMs 的真正力量。就在去年，我构建了一个[名为 Ellie 的机器人泰迪熊，能够移动头部并像真正的人类一样进行对话](https://medium.com/towards-data-science/building-ellee-a-gpt-3-and-computer-vision-powered-talking-robotic-teddy-bear-with-human-level-db7d08259583)。虽然这项技术无疑强大，但重要的是要谨慎使用，以确保生成结果的安全性和质量，因为它可能是一把双刃剑。

各位，这就是全部了！希望你们觉得这个博客有趣。如果是的话，请给我多多点赞。随时在[LinkedIn](https://www.linkedin.com/in/agustinus-nalwan/)与我联系，或查看我在[Medium 个人主页](https://medium.com/@agustinus-nalwan)上的其他 AI 项目。敬请关注，我将在接下来的几周内分享完整的源代码！

最后，我要感谢来自 AWS 的[Mike Chambers](https://www.linkedin.com/in/mikegchambers/)，他帮助我排查了微调后的 Stable Diffusion 模型批量转换代码的问题。
