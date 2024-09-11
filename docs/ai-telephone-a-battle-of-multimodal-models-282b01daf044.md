# AI 电话 — 多模态模型的对决

> 原文：[https://towardsdatascience.com/ai-telephone-a-battle-of-multimodal-models-282b01daf044?source=collection_archive---------5-----------------------#2023-06-15](https://towardsdatascience.com/ai-telephone-a-battle-of-multimodal-models-282b01daf044?source=collection_archive---------5-----------------------#2023-06-15)

## DALL-E2、Stable Diffusion、BLIP 等！

[](https://medium.com/@jacob_marks?source=post_page-----282b01daf044--------------------------------)[![Jacob Marks, Ph.D.](../Images/94d9832b8706d1044e3195386613bfab.png)](https://medium.com/@jacob_marks?source=post_page-----282b01daf044--------------------------------)[](https://towardsdatascience.com/?source=post_page-----282b01daf044--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----282b01daf044--------------------------------) [Jacob Marks, Ph.D.](https://medium.com/@jacob_marks?source=post_page-----282b01daf044--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff7dc0c0eae92&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fai-telephone-a-battle-of-multimodal-models-282b01daf044&user=Jacob+Marks%2C+Ph.D.&userId=f7dc0c0eae92&source=post_page-f7dc0c0eae92----282b01daf044---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----282b01daf044--------------------------------) · 14 min 阅读 · 2023年6月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F282b01daf044&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fai-telephone-a-battle-of-multimodal-models-282b01daf044&user=Jacob+Marks%2C+Ph.D.&userId=f7dc0c0eae92&source=-----282b01daf044---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F282b01daf044&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fai-telephone-a-battle-of-multimodal-models-282b01daf044&source=-----282b01daf044---------------------bookmark_footer-----------)![](../Images/c69c273d766460fcee87fbfc328b6a01.png)

*AI 电话游戏的艺术渲染图。图像由作者使用 DALL-E2 生成。*

生成式 AI 现在正处于火热阶段。特别是过去几个月，多模态机器学习的爆炸性增长——AI 连接了不同“模态”如文本、图像和音频的概念。例如，[Midjourney](https://www.midjourney.com/) 是一个多模态文本到图像模型，因为它接受自然语言输入，并输出图像。最近多模态协同的巅峰之作是 Meta AI 的 ImageBind，它可以接受 6 种(!) 类型的输入，并将它们表示在相同的“空间”中。

鉴于所有这些兴奋，我想测试多模态模型，看看它们 *实际上* 有多好。特别是，我想回答三个问题：

1.  哪种文本到图像的模型最好？

1.  哪种图像到文本的模型最好？

1.  什么更重要——图像到文本，还是文本到图像？

> *当然，每个模型都有其自身的偏见，从训练数据到模型架构，所以实际上并没有一个最好的模型。但我们仍然可以在一般背景下测试模型！*

为了回答这些问题，我决定玩一个 AI 电话游戏，灵感来自于我们全家都喜欢一起玩的棋盘游戏 [Telestrations](https://en.wikipedia.org/wiki/Telestrations)。

Telestrations 非常类似于 [电话游戏](https://www.wikihow.com/Play-the-Telephone-Game)：玩家们围成一个圈，接收来自一侧的人的信息，并将他们的解释传达给另一侧的人。随着游戏的进行，原始信息通常会被改变，甚至完全丧失。然而，Telestrations 的不同之处在于加入了双模态通信：玩家在绘制（或 *插图*）描述和用文本描述之间交替进行。

鉴于我对 *比较* 模型更感兴趣，我调整了游戏以适应这一目的。

这是 AI 电话游戏的工作方式：

1.  每个“游戏”将配对一个图像到文本（I2T）模型和一个文本到图像（T2I）模型

1.  给定一个初始提示，我们使用 T2I 模型生成一个图像。

1.  然后我们将这个图像传入 I2T 模型以生成描述。

1.  我们重复步骤 2 和 3 固定次数 `n`（在我们的案例中 `n=10`）。

1.  最后，我们量化原始提示与最终描述之间的差异。

在这篇文章中，我将带你经历整个过程，这样你也可以玩 AI 电话游戏！最后，我会回答三个激励性的问题。

*注意：这个 AI 电话游戏与* [*循环一致性*](https://openaccess.thecvf.com/content_ECCVW_2018/papers/11132/Cornia_Towards_Cycle-Consistent_Models_for_Text_and_Image_Retrieval_ECCVW_2018_paper.pdf)*的概念密切相关。通过在训练过程中在损失函数中加入循环一致性项，可以有效地促使模型最小化电话游戏中的退化现象。据我所知，本实验中考虑的所有模型在训练时并未将循环一致性作为考虑因素。*

文章的结构如下：

1.  [选择多模态模型](#6fb8)

1.  [生成提示](#cc8c)

1.  [创建电话线路](#50e9)

1.  [进行对话](#5551)

1.  [可视化和分析结果](#715e)

运行这个实验和玩 AI 电话游戏的所有代码可以在 [这里](https://github.com/voxel51/fiftyone-examples/blob/master/examples/ai_telephone.ipynb) 找到。

要运行这段代码，你需要安装用于数据集策划的 [FiftyOne 开源库](https://github.com/voxel51/fiftyone)、[OpenAI Python 库](https://github.com/openai/openai-python) 和 [Replicate Python 客户端](https://replicate.com/docs/get-started/python)。

```py
pip install fiftyone openai replicate
```

DALL-E2 和 BLIP 之间的 AI 电话游戏中的图像进展。

# 选择竞争者

多模态模型的领域非常庞大：截至撰写时，仅 Hugging Face 就有 4,425 个 T2I 模型和 155 个 I2T 模型。用所有这些模型——甚至是其中一个不可忽视的部分——进行 AI 电话游戏是完全不可行的。我的第一任务是将这些潜在候选者缩减到一个更可管理的竞争者集合。

## 选择 API

在开始这个项目时，我知道自己将使用许多模型。一些潜在的模型非常大，许多模型需要各自的环境和独特的要求。鉴于我计划将每个 T2I 模型与每个 I2T 模型配对，本地安装这些模型进行 AI 电话游戏会带来潜在的依赖地狱——尤其是因为我使用的是 MacBook Pro M1！

为了绕过这个问题，我决定坚持使用可以通过 API 访问的模型。特别是，我选择了主要使用 [Replicate](https://replicate.com/)，其简单的接口允许我以即插即用的方式使用 T2I 和 I2T 模型。我使用的几乎每个模型都是开源的，所以如果你比我更勇敢，可以在本地运行这些模型并避免费用。也就是说，总的来说，这个实验花费了不到 15 美元。

## 文本到图像模型

在选择 T2I 模型时，我从 Replicate 的 [Text to image](https://replicate.com/collections/text-to-image) 集合中进行挑选。我的选择标准是模型需要便宜、快速且相对受欢迎（通过 Replicate 上模型的“运行”次数来判断）。此外，模型需要是 *通用的*，意味着我不会考虑图像扩展、标志生成或动漫风格模型。如果你愿意，完全可以尝试使用这些类型的模型进行 AI 电话游戏！

根据这些要求，我选择了 [Stable Diffusion](https://replicate.com/stability-ai/stable-diffusion) 和 [Feed forward VQGAN CLIP](https://replicate.com/mehdidc/feed_forward_vqgan_clip)。最初，我也尝试了 [DALL-E Mini](https://replicate.com/kuprel/min-dalle)，但在早期测试中对模型的表现感到失望，因此我将模型更换为 OpenAI 的 DALL-E2，通过 OpenAI 的 [图像生成端点](https://platform.openai.com/docs/guides/images/usage)进行访问。

作为一个附带说明，限制我的关注范围到 API 可访问的模型意味着我没有考虑 [Midjourney](https://www.midjourney.com/)。没有官方 API，我不想使用非官方 API，也不想逐个在 Discord 中输入提示并一一下载生成的图像。

为了使这个过程尽可能即插即用，我采取了面向对象的方法。我定义了一个基本的 `Text2Image` 类，暴露了一个 `generate_image(text)` 方法：

```py
import replicate

class Text2Image(object):
    """Wrapper for a Text2Image model."""
    def __init__(self):
        self.name = None
        self.model_name = None

    def generate_image(self, text):
        response = replicate.run(self.model_name, input={"prompt": text})
        if type(response) == list:
            response = response[0]
        return response
```

对于 Replicate 模型，只需要设置 `model_name` 属性，以在 Replicate 上识别模型。例如，对于 Stable Diffusion，类定义如下：

```py
class StableDiffusion(Text2Image):
    """Wrapper for a StableDiffusion model."""
    def __init__(self):
        self.name = "stable-diffusion"
        self.model_name = "stability-ai/stable-diffusion:27b93a2413e7f36cd83da926f3656280b2931564ff050bf9575f1fdf9bcd7478"
```

对于其他模型，如 DALL-E2，`generate_image(text)` 方法可以被重载：

```py
import openai
class DALLE2(Text2Image):
    """Wrapper for a DALL-E 2 model."""
    def __init__(self):
        self.name = "dalle-2"

    def generate_image(self, text):
        response = openai.Image.create(
            prompt=text,
            n=1,
            size="512x512"
        )
        return response['data'][0]['url']
```

这些 T2I 模型中的每一个都会返回生成的图像的 URL，然后我们可以将其直接传递给我们的 I2T 模型。

## 图像到文本模型

我遵循了类似的过程来确定 I2T 竞争者，评估了 Replicate 的 [Image to text](https://replicate.com/collections/image-to-text) 收藏中的候选模型。在查看了收藏中所有模型的示例后，有六个模型脱颖而出：[BLIP](https://replicate.com/salesforce/blip)、[BLIP-2](https://replicate.com/andreasjansson/blip-2)、[CLIP prefix captioning](https://replicate.com/rmokady/clip_prefix_caption)、[Fine-grained Image Captioning with CLIP Reward](https://replicate.com/j-min/clip-caption-reward)、[mPLUG-Owl](https://replicate.com/joehoover/mplug-owl) 和 [MiniGPT-4](https://replicate.com/daanelson/minigpt-4)。其他模型也很吸引人，比如 [CLIP Interrogator](https://replicate.com/pharmapsychotic/clip-interrogator)，它试图反向工程一个提示，然后你可以用来生成类似的图像。但就 AI 电话而言，这感觉有点像作弊！

在尝试这六个 I2T 候选模型时，我能够迅速淘汰两个模型：BLIP-2 生成的响应总是过短，不够实用，而 CLIP Caption Reward 模型生成的响应往往不连贯。

与 T2I 模型直接类比，我定义了一个基本的 `Image2Text` 类，暴露了一个 `generate_text(image_url)` 方法：

```py
class Image2Text(object):
    """Wrapper for an Image2Text model."""
    def __init__(self):
        self.name = None
        self.model_name = None
        self.task_description = "Write a detailed description of this image."

    def generate_text(self, image_url):
        response = replicate.run(
            self.model_name, 
            input={
                "image": image_url,
                "prompt": self.task_description,
                }
            )
        return response 
```

然后，我为每个模型创建了子类。下面是 BLIP 子类的样子：

```py
class BLIP(Image2Text):
    """Wrapper for a BLIP model."""
    def __init__(self):
        super().__init__()
        self.name = "blip"
        self.model_name = "salesforce/blip:2e1dddc8621f72155f24cf2e0adbde548458d3cab9f00c0139eea840d0ac4746"
```

所有模型都使用相同的任务描述进行实例化——“写一个关于这张图片的详细描述”。

在 DALL-E2 和 mPLUG-Owl 之间的 AI 电话游戏中的图像进展。

# 提示

为了尽可能“科学”，我认为最好不要自己生成初始提示。相反，（仅仅为了好玩）我将任务外包给了 ChatGPT。我问：

```py
I'm playing a game of telephone using text-to-image and image-to-text AI models. 
I want to evaluate these models based on their ability to retain complex semantic
information over the course of long conversations. Your job is to give me 10 text
prompts that I can use to run these games of telephone. You must give me one 3 
easy, 3 medium, 3 hard, and 1 ultra-hard prompt
```

我正在使用文本到图像和图像到文本的AI模型进行电话游戏。我希望根据这些模型在长时间对话中保持复杂语义信息的能力进行评估。你的任务是给我10个文本提示，我可以用来运行这些电话游戏。你必须给我3个简单的、3个中等的、3个困难的和1个超难（“不可能”）的提示。

这里是一些ChatGPT生成的提示：

```py
Easy:

"A red apple sitting on a wooden table with sunlight streaming in from a window."

Medium:

"An astronaut floating in the International Space Station, looking out at Earth through the window, with a space capsule docked in the background."

Hard:

"A bustling marketplace in an ancient Middle Eastern city. Traders haggling over spices and silks, camels carrying goods, the sun setting behind a mosque with a crescent moon visible."

Impossible:

"A panoramic scene of an advanced alien civilization on a distant exoplanet. Interstellar vehicles flying in an indigo sky above towering crystalline structures. Aliens with varying physical features are interacting, engaging in activities like exchanging energy orbs, communicating through light patterns, and tending to exotic, bio-luminescent flora. The planet’s twin moons are visible in the horizon over a glistening alien ocean."
```

*更严格的科学方法会对所使用的提示以及它们的分类更加用心。*

我将ChatGPT生成的文本提示转化为`Prompt`对象，其中包含提示的文本，以及ChatGPT分配的“难度”级别：

```py
class Prompt(object):
    def __init__(self, text, level):
        self.text = text
        self.level = level

levels = ["easy", "medium", "hard", "impossible"]
level_prompts = [easy_texts, medium_texts, hard_texts, impossible_texts]

def get_prompts():
    prompts = []
    for level, texts in zip(levels, level_prompts):
        for text in texts:
            prompts.append(Prompt(text, level))
    return prompts
```

AI电话游戏中VQGAN-CLIP与MiniGPT-4之间图像的进展。

# 电话线

玩AI电话游戏的最后一个组件是“电话线”本身。我创建了一个`TelephoneLine`类来封装T2I模型和I2T模型之间的连接。给定一条电话线，通过调用`play(prompt, nturns=10)`来进行“游戏”，对话从`prompt`开始，并进行`nturns`轮回合。

```py
import os
import hashlib
import fiftyone as fo
from fiftyone import ViewField as F

class TelephoneLine(object):
    """Class for playing telephone with AI."""
    def __init__(self, t2i, i2t):
        self.t2i = t2i
        self.i2t = i2t
        self.name = f"{t2i.name}_{i2t.name}"
        self.conversations = {}

    def get_conversation_name(self, text):
        full_name = f"{self.name}{text}"
        hashed_name = hashlib.md5(full_name.encode())
        return hashed_name.hexdigest()[:6]

    def play(self, prompt, nturns = 10):
        """Play a game of telephone."""
        print(f"Connecting {self.t2i.name} <-> {self.i2t.name} with prompt: {prompt.text[:20]}...")
        texts = [prompt.text]
        image_urls = []

        for _ in range(nturns):
            image_url = self.t2i.generate_image(texts[-1])
            text = self.i2t.generate_text(image_url)
            texts.append(text)
            image_urls.append(image_url)

        conversation_name = self.get_conversation_name(prompt.text)
        self.conversations[conversation_name] = {
            "texts": texts,
            "image_urls": image_urls,
            "level": prompt.level
        }
```

对于每个进行的游戏，使用唯一名称记录对话，该名称由T2I模型名称、I2T模型名称和提示文本（`get_conversation_name()`方法）哈希生成。

我还为该类配备了一个`save_conversations_to_dataset()`方法，该方法将所有游戏中的图像和描述保存到FiftyOne `Dataset`中：

```py
 def save_conversations_to_dataset(self, dataset):
        """Save conversations to a dataset."""
        for conversation_name in self.conversations.keys():
            conversation = self.conversations[conversation_name]
            prompt = conversation["texts"][0]
            level = conversation["level"]
            image_urls = conversation["image_urls"]
            texts = conversation["texts"]

            for i in range(len(image_urls)):
                filename = f"{conversation_name}_{i}.jpg"
                filepath = os.path.join(IMAGES_DIR, filename)
                download_image(image_urls[i], filepath)

                sample = fo.Sample(
                    filepath = filepath,
                    conversation_name = conversation_name,
                    prompt = prompt,
                    level = level,
                    t2i_model = self.t2i.name,
                    i2t_model = self.i2t.name,
                    step_number = i,
                    text_before = texts[i],
                    text_after = texts[i+1]
                )

                dataset.add_sample(sample)
```

AI电话游戏中Stable Diffusion与CLIP Prefix Captioning之间图像的进展。

# 进行对话

所有构建模块到位后，玩AI电话游戏简直是小菜一碟！

我们可以实例化T2I和I2T模型：

```py
## Image2Text models
mplug_owl = MPLUGOwl()
blip = BLIP()
clip_prefix = CLIPPrefix()
mini_gpt4 = MiniGPT4()
image2text_models = [mplug_owl, blip, clip_prefix, mini_gpt4]

## Text2Image models
vqgan_clip = VQGANCLIP()
sd = StableDiffusion()
dalle2 = DALLE2()
text2image_models = [sd, dalle2, vqgan_clip]
```

然后为每对创建一条电话线：

```py
combos = [(t2i, i2t) for t2i in text2image_models for i2t in image2text_models]
lines = [TelephoneLine(*combo) for combo in combos]
```

然后我们加载我们的提示：

```py
prompts = get_prompts()
```

并创建一个FiftyOne `Dataset`，我们将用来存储生成的图像以及来自对话的所有相关信息：

```py
import fiftyone as fo

dataset = fo.Dataset(name = 'telephone', persistent=True)
dataset.add_sample_field("conversation_name", fo.StringField)
dataset.add_sample_field("prompt", fo.StringField)
dataset.add_sample_field("level", fo.StringField)
dataset.add_sample_field("t2i_model", fo.StringField)
dataset.add_sample_field("i2t_model", fo.StringField)
dataset.add_sample_field("step_number", fo.IntField)
dataset.add_sample_field("text_before", fo.StringField)
dataset.add_sample_field("text_after", fo.StringField)
```

然后我们可以运行所有120场电话游戏：

```py
from tqdm import tqdm

for line in tqdm(lines):
    for prompt in prompts:
        line.play(prompt, nturns = 10)
    line.save_conversations_to_dataset(dataset)

session = fo.launch_app(dataset)
```

在FiftyOne应用程序中，点击菜单栏中的分割图标，以对话为单位对图像进行分组，从下拉菜单中选择`conversation_name`，然后将选择器切换到`ordered`并选择`step_number`。

# 结果和结论

为了评估对话的质量——纯粹是从最终描述的意义与初始提示的意义的接近程度来看，我决定生成提示和描述的嵌入，并计算两个之间的[余弦距离](https://docs.scipy.org/doc/scipy/reference/generated/scipy.spatial.distance.cosine.html)（在`[0, 2]`范围内）。

```py
from scipy.spatial.distance import cosine as cosine_distance
```

对于嵌入模型，我希望选择一个能够同时嵌入文本和图像的模型，以适应该任务的多模态特性。最终，我选择了使用ImageBind，原因有三：

1.  其他流行的联合图像-文本嵌入模型如 CLIP 和 BLIP 与我在实验中使用的一些模型（BLIP 和 CLIP prefix captioning）有关，我希望避免使用相同类型模型进行评估带来的任何潜在偏差。

1.  许多文本嵌入模型有一个较小的 `max_token_count`——即允许在文本中嵌入的最大标记数。例如，CLIP 的 `max_token_count=77`。我们的一些描述明显比这长。幸运的是，ImageBind 具有更长的最大标记数。

1.  我一直想尝试 ImageBind，这正是一个绝佳的机会！

我将 Replicate 的 ImageBind API 封装在一个函数 `embed_text(text)` 中：

```py
MODEL_NAME = "daanelson/imagebind:0383f62e173dc821ec52663ed22a076d9c970549c209666ac3db181618b7a304"
def embed_text(text):
    response = replicate.run(
        MODEL_NAME,
        input={
            "text_input": text,
             "modality": "text"
            }
    )
    return np.array(response)
```

为了避免冗余计算，我对提示进行了哈希处理，并将提示嵌入存储在字典中。这样，我们只需为每个提示嵌入一次，而不是为每个 12 条电话线路的每个提示嵌入：

```py
import hashlib
def hash_prompt(prompt):
    return hashlib.md5(prompt.encode()).hexdigest()[:6]

### Embed initial prompts
prompt_embeddings = {}
dataset.add_sample_field("prompt_hash", fo.StringField)

## Group samples by initial prompt
## Add hash to all samples in group
prompt_groups = dataset.group_by("prompt")
for pg in prompt_groups.iter_dynamic_groups():
    prompt = pg.first().prompt
    hash = hash_prompt(prompt)
    prompt_embeddings[hash] = embed_text(prompt)
    view = pg.set_field("prompt_hash", hash)
    view.save("prompt_hash")
```

然后我们可以按对话名称对样本进行分组，迭代这些组，计算每一步的文本嵌入，并记录文本嵌入与初始提示嵌入之间的余弦距离（距离越小越好！）：

```py
dataset.add_sample_field("text_after_dist", fo.FloatField)

prompt_groups = dataset.group_by("conversation_name")
for cg in conversation_groups.iter_dynamic_groups(progress=True):
    hash = cg.first().prompt_hash
    prompt_embedding = prompt_embeddings[hash]

    ordered_samples = cg.sort_by("step_number")
    for sample in ordered_samples.iter_samples(autosave=True):
        text_embedding = embed_text(sample.text_after)
        sample["text_embedding"] = text_embedding        
        sample.text_after_dist = cosine_distance(
            prompt_embedding,
            text_embedding
        )
```

然后，我计算了每个 T2I-I2T 对在特定难度级别下的平均分数，并绘制了结果。在每个视频中，I2T 和 T2I 模型被打印在生成的图像上，以及用于生成该图像的文本（红色），以及从该图像生成的描述（绿色）。

## 简单

![](../Images/0f19f4db954511a26ab1c95cb13526c9.png)

对于简单的提示，性能往往最依赖于文本到图像模型。DALL-E2 和 Stable Diffusion 显著优于 VQGAN-CLIP。MiniGPT-4 是这两个顶级表现对中的成员。

以下是上面介绍的简单提示的一些示例：

简单提示的 AI 电话，包括文本到图像和图像到文本模型对。

在使用 MiniGPT-4 的游戏中（以及在稍微小一点程度上的 BLIP），苹果始终保持在中心，而对于涉及 CLIP Prefix 的游戏，苹果随着时间的推移逐渐被淘汰。

## 中等

![](../Images/cc664b641e6ec5bbbc202e6fa1dbd1e7.png)

当提示变得稍微困难时，情况开始发生变化。

中等难度提示的 AI 电话，包括文本到图像和图像到文本模型对。

对于几乎所有的游戏，主题在第四或第五步左右发生变化。早期，MiniGPT-4 具有优势。但到游戏结束时，这种优势似乎完全丧失了。

## 困难

![](../Images/9e02437c072b5f6388b7e36483df716b.png)

当提示变得具有挑战性时，我们开始看到有趣的现象：在早期步骤中，图像到文本模型最为重要（MiniGPT-4 最佳，CLIP Prefix 通常最差）。但在后期阶段，文本到图像模型变得最为重要。更复杂的是，VQGAN-CLIP 在这里表现最佳！

有人可能担心“更好”只是意味着保持一致性，而没有准确表示原始概念。然而，当我们查看例子时，可以看到情况并非如此。

AI 电话对于一个困难的提示，涉及成对的文本到图像和图像到文本模型。

以视频中突出显示的例子为例，其中最初的提示是上述“繁忙市场”的“困难”提示。虽然 VQGAN-CLIP 生成的图像无疑是颗粒状的，但主题仍然可以辨认出来，并且与原始提示相当接近。

## 不可能

![](../Images/00ae40dcc7622c335c9b871b46224e71.png)

不出所料，我们的竞争对手在这里表现得都不太好。有人可能会争辩说 VQGAN-CLIP 是赢家。但在大多数情况下，这一切只是噪音。在视频中，即使是涉及 VQGAN-CLIP 的游戏，主题也基本上无法识别。

AI 电话对于一个“无法完成”的提示，涉及成对的文本到图像和图像到文本模型。

# 要点

这次探索远非科学；我只查看了十个提示，没有真正验证它们的难度。我仅将对话进行到十轮往返步骤；而且我只在一个指标上评估了性能。

很明显，哪种 T2I 和 I2T 模型表现最佳在很大程度上取决于提示的复杂性，以及你希望模型对话保持多长时间。然而，值得注意几个关键观察点：

1.  VQGAN-CLIP 对于更具挑战性的提示可能表现更好，但这并不意味着它是一个*更好的* T2I 模型。VQGAN-CLIP 生成的图像通常远不如 Stable Diffusion 或 DALL-E2 生成的图像那样连贯和全球一致。

1.  上述分析全部关于语义相似性——它没有考虑*风格*。这些图像的风格在 AI 电话游戏过程中可能会变化很多。根据我的经验，I2T 模型如 mPLUG-Owl 的风格更一致，因为它们提供了长描述，而像 BLIP 这样的模型风格则更多地集中在特定主题上。

1.  到了大约五六轮迭代时，游戏大多数已经收敛到稳定的平衡状态。

1.  尽管嵌入模型 ImageBind 是多模态的，但连续图像嵌入和文本嵌入之间的距离远大于连续图像或连续描述之间的距离。一般来说，它们遵循相同的趋势，但不那么明显，这也是我没有在图中包含这些的原因。

我希望这能激励你进行自己的生成 AI 实验——无论你是在玩 AI 电话，还是做其他完全不同的事情！

如果你尝试这个变体并获得有趣的结果，请在这个帖子下评论！
