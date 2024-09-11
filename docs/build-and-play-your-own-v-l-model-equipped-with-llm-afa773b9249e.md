# 建立并玩耍！你自己的 V&L 模型配备 LLM！

> 原文：[https://towardsdatascience.com/build-and-play-your-own-v-l-model-equipped-with-llm-afa773b9249e?source=collection_archive---------6-----------------------#2023-09-07](https://towardsdatascience.com/build-and-play-your-own-v-l-model-equipped-with-llm-afa773b9249e?source=collection_archive---------6-----------------------#2023-09-07)

## 开发集成 LLM 的 GIT 视觉语言模型。

[](https://medium.com/@inoichan?source=post_page-----afa773b9249e--------------------------------)[![Yuichi Inoue](../Images/d25793aea6ddcdb90ecb21be5031a434.png)](https://medium.com/@inoichan?source=post_page-----afa773b9249e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----afa773b9249e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----afa773b9249e--------------------------------) [Yuichi Inoue](https://medium.com/@inoichan?source=post_page-----afa773b9249e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff3eff720c79a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-and-play-your-own-v-l-model-equipped-with-llm-afa773b9249e&user=Yuichi+Inoue&userId=f3eff720c79a&source=post_page-f3eff720c79a----afa773b9249e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----afa773b9249e--------------------------------) ·21 分钟阅读·2023年9月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fafa773b9249e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-and-play-your-own-v-l-model-equipped-with-llm-afa773b9249e&user=Yuichi+Inoue&userId=f3eff720c79a&source=-----afa773b9249e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fafa773b9249e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-and-play-your-own-v-l-model-equipped-with-llm-afa773b9249e&source=-----afa773b9249e---------------------bookmark_footer-----------)

本文摘要：

+   *解释由微软开发的 GIT 视觉语言模型。*

+   *使用 PyTorch 和 Hugging Face 的 Transformers 替换 GIT 的语言模型为大型语言模型（LLMs）。*

+   *介绍如何使用 LoRA 微调 GIT-LLM 模型。*

+   *测试和讨论开发的模型。*

+   *探讨由 GIT 的图像编码器嵌入的“图像嵌入”是否在与“文本嵌入”相同的空间中指示特定字符。*

大型语言模型（LLM）正展现出越来越多的价值。将图像纳入LLM使其作为视觉语言模型更加有用。在这篇文章中，我将解释一个称为GIT-LLM的模型的开发，这是一种简单但强大的视觉语言模型。某些部分，如代码解释，可能会显得有些繁琐，所以可以直接跳到结果部分。我进行了各种实验和分析，希望你能喜欢我所取得的成果。

实现已公开发布，所以请试试。

[](https://github.com/turingmotors/heron?source=post_page-----afa773b9249e--------------------------------) [## GitHub - turingmotors/heron

### 通过在GitHub上创建一个账户来为turingmotors/heron的发展做贡献。

github.com](https://github.com/turingmotors/heron?source=post_page-----afa773b9249e--------------------------------)

# 将GIT转化为LLM

让我们深入探讨这篇技术博客的主要话题。

# 什么是GIT？

生成式图像到文本变换器（Generative Image-to-text Transformer），或称GIT，是微软提出的一种视觉语言模型。

arXiv: [https://arxiv.org/abs/2205.14100](https://arxiv.org/abs/2205.14100)

代码： [https://github.com/microsoft/GenerativeImage2Text](https://github.com/microsoft/GenerativeImage2Text)

它的架构相当简单。它将从图像编码器提取的特征向量转换为可以像文本一样处理的向量，使用一个投影模块。这些向量随后输入到语言模型中，以生成图像的标题或进行问答。该模型也可以以类似的方式处理视频。

![](../Images/e26bc6f23c908cb230eb975a8c6eaac7.png)

该图摘自[“GIT: A Generative Image-to-text Transformer for Vision and Language”](https://arxiv.org/abs/2205.14100)

尽管它很简单，但如果你查看“Paper with code”的排行榜，你会发现它在许多任务中排名靠前。

[https://paperswithcode.com/paper/git-a-generative-image-to-text-transformer](https://paperswithcode.com/paper/git-a-generative-image-to-text-transformer)

最初，GIT使用像CLIP这样的强大模型作为其图像编码器，并从头开始训练语言模型部分。然而，在这篇文章中，我尝试使用一个强大的LLM并对其进行微调。在这里，我称该模型为“GIT-LLM”。

# 使用Hugging Face的Transformers来实现LLM

我将使用Hugging Face的[Transformers](https://huggingface.co/docs/transformers/index)库来开发GIT-LLM。Transformers是一个用于处理机器学习模型的Python库。它提供了许多最先进的预训练模型，你可以立即进行推理。它还提供了训练和微调模型的工具。我相信Transformers在最近的LLM衍生品的发展中做出了重要贡献。几乎所有可用的LLM都可以用Transformers处理，许多从这些LLM衍生出的多模态模型也使用Transformers作为基础进行开发和微调。

这是使用Transformers模型的最简单代码。你可以通过使用*AutoModel*和*AutoTokenizer*轻松尝试LLMs。

```py
from transformers import AutoModelForCausalLM, AutoTokenizer

model_name = "facebook/opt-350m"

model = AutoModelForCausalLM.from_pretrained(model_name).to("cuda")
tokenizer = AutoTokenizer.from_pretrained(model_name)

prompt = "Hello, I'm am conscious and"
input_ids = tokenizer(prompt, return_tensors="pt").to("cuda")

sample = model.generate(**input_ids, max_length=64)
print(tokenizer.decode(sample[0]))
# Hello, I'm am conscious and I'm a bit of a noob. I'm looking for a good place to start.
```

让我们查看OPT模型所包含的参数。打印由*AutoModelForCausalLM*创建的模型。

```py
OPTForCausalLM(
  (model): OPTModel(
    (decoder): OPTDecoder(
      (embed_tokens): Embedding(50272, 512, padding_idx=1)
      (embed_positions): OPTLearnedPositionalEmbedding(2050, 1024)
      (project_out): Linear(in_features=1024, out_features=512, bias=False)
      (project_in): Linear(in_features=512, out_features=1024, bias=False)
      (layers): ModuleList(
        (0-23): 24 x OPTDecoderLayer(
          (self_attn): OPTAttention(
            (k_proj): Linear(in_features=1024, out_features=1024, bias=True)
            (v_proj): Linear(in_features=1024, out_features=1024, bias=True)
            (q_proj): Linear(in_features=1024, out_features=1024, bias=True)
            (out_proj): Linear(in_features=1024, out_features=1024, bias=True)
          )
          (activation_fn): ReLU()
          (self_attn_layer_norm): LayerNorm((1024,), eps=1e-05, elementwise_affine=True)
          (fc1): Linear(in_features=1024, out_features=4096, bias=True)
          (fc2): Linear(in_features=4096, out_features=1024, bias=True)
          (final_layer_norm): LayerNorm((1024,), eps=1e-05, elementwise_affine=True)
        )
      )
    )
  )
  (lm_head): Linear(in_features=512, out_features=50272, bias=False)
)
```

这非常简单。初始*embed_tokens*的输入维度和最终*lm_head*的输出维度为50,272，表示训练此模型时使用的标记数量。让我们验证一下分词器词汇表的大小：

```py
print(tokenizer.vocab_size)
# 50265
```

包括像*bos_token*、*eos_token*、*unk_token*、*sep_token*、*pad_token*、*cls_token*和*mask_token*这样的特殊标记，它预测了从总共50,272种标记中下一个单词的概率。

你可以通过查看[实现](https://github.com/huggingface/transformers/blob/v4.30.0/src/transformers/models/opt/modeling_opt.py)来理解这些模型是如何连接的。一个简单的图示将表示如下流程：

![](../Images/5352c8349ab3b070c14a69644b854f65.png)

OPT的简化模型架构（图像由作者制作）

结构和数据流非常简单。〇〇Model和〇〇ForCausalLM在不同的语言模型中具有类似的框架。〇〇Model类主要表示语言模型的“Transformer”部分。例如，如果你想执行文本分类任务，你只需使用这一部分。〇〇ForCausalLM类用于文本生成，将分类器应用于处理后转换器的向量中的标记计数。损失计算也是在该类的前向方法中完成的。*embed_positions*表示位置编码，它会被加到*project_in*上。

# 使用GIT与Transformers

我将根据[GIT的官方文档页面](https://huggingface.co/docs/transformers/model_doc/git)尝试一下。由于我也会处理图像，所以我会使用一个同时包含Tokenizer的Processor。

```py
from PIL import Image
import requests
from transformers import AutoProcessor, AutoModelForCausalLM

model_name = "microsoft/git-base-coco"

model = AutoModelForCausalLM.from_pretrained(model_name)
processor = AutoProcessor.from_pretrained(model_name)

# Downloading and preprocess an image
url = "http://images.cocodataset.org/val2017/000000039769.jpg"
image = Image.open(requests.get(url, stream=True).raw)
pixel_values = processor(images=image, return_tensors="pt").pixel_values

# Preprocessing text
prompt = "What is this?"
inputs = processor(
            prompt,
            image,
            return_tensors="pt",
            max_length=64
        )

sample = model.generate(**inputs, max_length=64)
print(processor.tokenizer.decode(sample[0]))
# two cats sleeping on a couch
```

给定[输入图像](http://images.cocodataset.org/val2017/000000039769.jpg)生成的输出为“两只猫在沙发上睡觉”，这表明它的效果很好。

让我们也来看看模型的结构：

```py
GitForCausalLM(
  (git): GitModel(
    (embeddings): GitEmbeddings(
      (word_embeddings): Embedding(30522, 768, padding_idx=0)
      (position_embeddings): Embedding(1024, 768)
      (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
      (dropout): Dropout(p=0.1, inplace=False)
    )
    (image_encoder): GitVisionModel(
      (vision_model): GitVisionTransformer(
        ...
      )
    )
    (encoder): GitEncoder(
      (layer): ModuleList(
        (0-5): 6 x GitLayer(
          ...
        )
      )
    )
    (visual_projection): GitProjection(
      (visual_projection): Sequential(
        (0): Linear(in_features=768, out_features=768, bias=True)
        (1): LayerNorm((768,), eps=1e-05, elementwise_affine=True)
      )
    )
  )
  (output): Linear(in_features=768, out_features=30522, bias=True)
)
```

虽然有点长，但如果你拆解开来，它其实也很简单。在GitForCausalLM中，有一个GitModel，内部包含以下模块：

+   embeddings (GitEmbeddings)

+   image_encoder (GitVisionModel)

+   encoder (GitEncoder)

+   visual_projection (GitProjection)

+   output (Linear)

与 OPT 的主要区别在于存在*GitVisionModel*和*GitProjection*，这正是将图像转换为类似提示的向量的模块。虽然语言模型对 OPT 使用 Decoder，对 GIT 使用 Encoder，但这仅意味着注意力掩码构建方式的不同。变压器层可能存在细微差别，但它们的功能本质上是相同的。GIT 使用 Encoder 这一名称，因为它使用独特的注意力掩码，该掩码对图像的所有特征应用注意力，并对文本特征使用因果掩码。

查看模型的连接；

![](../Images/8b563994c34c6404fe9b5c7dcbd6d595.png)

GIT 的简化模型架构（图像由作者制作）

图像信息由*GitVisionModel*和*GitProjection*处理，以匹配文本的嵌入。之后，它与文本的嵌入一起输入到语言模型的“Transformer”层中。虽然存在细微差别，但与语言模型相关的部分几乎以相同的方式开发。

# GIT 的注意力掩码

一般语言模型和 GIT 语言模型的架构几乎相同，但应用注意力掩码的方式不同。

对于语言模型，应用注意力掩码以避免在预测未来标记时查看过去的标记。这是一种称为“因果注意力”的方法，对应于下图的左侧。第一列标记仅引用自身，确保对后续词没有自注意力。第二列对第二个词应用自注意力，而从第三个词开始变为 0。这样的掩码使得模型能够有效地训练以预测下一个词。

GIT 输入有两种类型的标记：图像标记和文本标记。由于所有图像标记是同时使用的，并且不用于预测下一个标记，因此因果注意力不适用。另一方面，文本标记仍然需要因果注意力。设计了如图右侧所示的掩码来实现这一点。对于图像信息的前三行，自注意力应用于所有标记信息。从文本标记开始，向下移动一列会增加可以参考的单词数量。

![](../Images/2274c8b7bd321af9b8894f9c179b5b3d.png)

因果注意力掩码与 Git 注意力掩码的区别（图像由作者制作）

让我们还检查一下制作 GIT 掩码的代码。创建 GIT 掩码的代码片段如下：

```py
import torch

def create_git_attention_mask(
    tgt: torch.Tensor,
    memory: torch.Tensor,
) -> torch.Tensor:
    num_tgt = tgt.shape[1]
    num_memory = memory.shape[1]

    # Areas where attention is applied are 0, areas without attention are -inf
    top_left = torch.zeros((num_memory, num_memory))
    top_right = torch.full(
        (num_memory, num_tgt),
        float("-inf"),
    )
    bottom_left = torch.zeros(
        (num_tgt, num_memory),
    )

    # Causal Attention Mask
    bottom_right = torch.triu(torch.ones(tgt.shape[1], tgt.shape[1]), diagonal=1)
    bottom_right = bottom_right.masked_fill(bottom_right == 1, float("-inf"))

    # Concatenate masks
    left = torch.cat((top_left, bottom_left), dim=0)
    right = torch.cat((top_right, bottom_right), dim=0)

    # add axis for multi-head
    full_attention_mask = torch.cat((left, right), dim=1)[None, None, :]

    return full_attention_mask

# batch_size, sequence, feature_dim
visual_feature = torch.rand(1, 3, 128)
text_feature = torch.rand(1, 4, 128)

mask = create_git_attention_mask(tgt=text_feature, memory=visual_feature)
print(mask)

"""
tensor([[[[0., 0., 0., -inf, -inf, -inf, -inf],
          [0., 0., 0., -inf, -inf, -inf, -inf],
          [0., 0., 0., -inf, -inf, -inf, -inf],
          [0., 0., 0., 0., -inf, -inf, -inf],
          [0., 0., 0., 0., 0., -inf, -inf],
          [0., 0., 0., 0., 0., 0., -inf],
          [0., 0., 0., 0., 0., 0., 0.]]]])
"""
```

你将掩码添加到注意力权重中。因此，自注意力发生的部分为 0，而不包括在注意力中的部分为 -inf。通过向前提供此掩码，只有文本部分可以进行因果注意力。对于视觉语言模型来说，像这样有效地创建和使用掩码非常重要。

# 连接 GIT 和 OPT

现在，让我们连接 GIT 和 OPT。目标是创建如图所示的模型。

![](../Images/15317072b84b09432c4e5fb70e7acb6a.png)

GIT-OPT的简化模型架构（图由作者制作）

对于通用实现，你可以参考`[modeling_git.py](https://github.com/huggingface/transformers/blob/main/src/transformers/models/git/modeling_git.py)`。

最重要的部分是*GitOPTModel*。在其中，一个视觉编码器需要与LLM连接。我会解释一些关键组件。

```py
class GitOPTModel(OPTModel):
    def __init__(self, config: OPTConfig):
        super(GitOPTModel, self).__init__(config)

        self.image_encoder = CLIPVisionModel.from_pretrained(config.vision_model_name)
        self.visual_projection = GitProjection(config)
```

在*__init__*函数内部，实例化了各种模块。*super*初始化了*OPTModel*。在GIT中，推荐使用训练有素的CLIP图像编码器，因此我使其与CLIP训练的ViT兼容。*GitProjection*来自原始GIT实现。

让我们看看forward函数内部。实现基于*OPTDecoder*的forward部分，并添加了来自图像编码器的信息。虽然实现有点冗长，但我在代码中添加了注释，请按步骤进行。

```py
class GitOPTModel(OPTModel):
    ...

    def forward(
        self,
        input_ids: Optional[torch.Tensor] = None,
        attention_mask: Optional[torch.Tensor] = None,
        pixel_values: Optional[torch.Tensor] = None,
    ) -> BaseModelOutputWithPooling:
        seq_length = input_shape[1]

        # 1\. Extract image features using ViT
        visual_features = self.image_encoder(pixel_values).last_hidden_state

        # 2\. Convert features extracted by ViT into prompt-like Image Embeddings
        projected_visual_features = self.visual_projection(visual_features)

        # 3\. Vectorize the tokens
        inputs_embeds = self.decoder.embed_tokens(input_ids)

        # 4\. Obtain Positional Encoding
        pos_embeds = self.embed_positions(attention_mask, 0)

        # 5\. Dimension adjustment of Text Embeddings specific to OPT
        inputs_embeds = self.decoder.project_in(inputs_embeds)

        # 6\. Text Embeddings + Positional Encoding
        embedding_output = inputs_embeds + pos_embeds

        # 7\. Concatenate Image Embeddings and Text Embeddings
        hidden_states = torch.cat((projected_visual_features, embedding_output), dim=1)

        # 8\. Create Causal Attention Mask for Text region
        tgt_mask = self._generate_future_mask(
            seq_length, embedding_output.dtype, embedding_output.device
        )

        # 9\. Create Attention Mask for GIT
        combined_attention_mask = self.create_attention_mask(
            tgt=embedding_output,
            memory=projected_visual_features,
            tgt_mask=tgt_mask,
            past_key_values_length=0,
        )

        # 10\. Pass through the Decoder layer repeatedly, the main part of the language model
        for idx, decoder_layer in enumerate(self.decoder.layers):
            layer_outputs = decoder_layer(
                hidden_states,
                attention_mask=combined_attention_mask,
                output_attentions=output_attentions,
                use_cache=use_cache,
            )

            hidden_states = layer_outputs[0]

        # 11\. Dimension adjustment MLP specific to OPT
        hidden_states = self.decoder.project_out(hidden_states)

        # 12\. Align the output interface
        return BaseModelOutputWithPast(
            last_hidden_state=hidden_states,
            past_key_values=next_cache,
            hidden_states=all_hidden_states,
            attentions=all_self_attns,
        )
```

虽然看起来可能很复杂，但如果你逐步了解每个步骤，你会发现它遵循了图示中的流程。实际代码可能看起来有点复杂，但先掌握主要流程将使理解其他部分更容易。这是伪代码，对于详细部分，请参考[发布的实现](https://github.com/Ino-Ichan/GIT-LLM/blob/main/git_llm/git_opt/modeling_git_opt.py#L178)。

最后，让我们简要看看*GITOPTForCausalLM*部分。

```py
class GitOPTForCausalLM(OPTForCausalLM):
    def __init__(
        self,
        config,
    ):
        super(GitOPTForCausalLM, self).__init__(config)
        self.model = GitOPTModel(config)

    def forward(
        ...
    ) -> CausalLMOutputWithPast:

        outputs = self.model(
            ...
        )

        sequence_output = outputs[0]
        logits = self.lm_head(sequence_output)

        loss = None
        if labels is not None:
            # Predict the next word as the task
            num_image_tokens = self.image_patch_tokens
            shifted_logits = logits[:, num_image_tokens:-1, :].contiguous()
            labels = labels[:, 1:].contiguous()
            loss_fct = CrossEntropyLoss()
            loss = loss_fct(shifted_logits.view(-1, self.config.vocab_size), labels.view(-1))

        return CausalLMOutputWithPast(
            loss=loss,
            logits=logits,
            ...
        )
```

模型内部的处理很简单。当提供*labels*时，即在训练过程中，损失计算也在forward中进行。在*shifted_logits*中，从第一个token到文本tokens的倒数第二个token被提取。然后，它计算与*labels*偏移一个词的Cross Entropy Loss作为正确答案。

一点需要注意的是，在初始化函数中分配*GitOPTModel*的变量需要命名为*self.model*。如果你查看[父类*OPTForCausalLM*的实现](https://github.com/huggingface/transformers/blob/v4.31.0/src/transformers/models/opt/modeling_opt.py#L823)，你会看到OPT在*super*初始化期间首先被放置到*self.model*中。如果你更改这个实例变量名，你将最终持有两个OPT，这可能会增加内存负担。

# LoRA扩展

为了有效地微调LLM，我将使用一个名为Parameter-Efficient Fine-Tuning（[PEFT](https://github.com/huggingface/peft)）的库。由于它由Hugging Face开发，它与Transformers无缝集成。虽然PEFT中有各种方法，但这次我将使用一种常见的方法，即低秩适配（LoRA）进行实验。

如果模型支持PEFT，模型可以用LoRA在几行代码中应用。

```py
from transformers import AutoModelForCausalLM
from peft import get_peft_config, get_peft_model, LoraConfig

model = AutoModelForCausalLM.from_pretrained('microsoft/git-base')

peft_config = LoraConfig(
    task_type="CAUSAL_LM",
    r=8,
    lora_alpha=32,
    lora_dropout=0.1,
    target_modules=["v_proj"]
)

peft_model = get_peft_model(model, peft_config)
```

*target_modules* 参数指定了你想要转换为 LoRA 的模块。如果提供了列表作为 *target_modules*，则会将每个字符串结尾的模块转换为 LoRA。为了简化，LoRA 仅应用于自注意力模块的“value” (*v_proj*)。

在模型中，ViT 用于图像编码部分。请小心，因为这样指定的话，ViT 的自注意力部分可能也会应用 LoRA。这有点繁琐，但通过指定到键名不重叠的部分并将其传递给 *target_modules*，你可以避免这种情况。

```py
target_modules = [f"model.image_encoder.vision_model.encoder.{i}.self_attn.v_proj" for i in range(len(model.model.decoder))]
```

结果模型变成了 *PeftModelForCausalLM* 类的一个实例。它有一个名为 *base_model* 的实例变量，保存了转换为 LoRA 的原始模型。作为示例，我展示了 LoRA 如何应用于 ViT 的自注意力中的 *v_proj*。

```py
(self_attn): GitVisionAttention(
  (k_proj): Linear(in_features=768, out_features=768, bias=True)
  (v_proj): Linear(
    in_features=768, out_features=768, bias=True
    (lora_dropout): ModuleDict(
      (default): Dropout(p=0.1, inplace=False)
    )
    (lora_A): ModuleDict(
      (default): Linear(in_features=768, out_features=8, bias=False)
    )
    (lora_B): ModuleDict(
      (default): Linear(in_features=8, out_features=768, bias=False)
    )
    (lora_embedding_A): ParameterDict()
    (lora_embedding_B): ParameterDict()
  )
  (q_proj): Linear(in_features=768, out_features=768, bias=True)
  (out_proj): Linear(in_features=768, out_features=768, bias=True)
)
```

在 *v_proj* 线性层内部，你会发现添加了如 *lora_A* 和 *lora_B* 的全连接层。LoRA转换后的 *Linear* 模块是一个名字相同的 Linear 类，继承自 PyTorch 的 *Linear* 和 *LoraLayer*。这是一个有些独特的模块，有兴趣了解细节的人可以查看[实现](https://github.com/huggingface/peft/blob/main/src/peft/tuners/lora/layer.py#L153)。

请注意，使用 PEFT 创建的模型默认不会保存除 LoRA 部分之外的任何内容。虽然可以通过 *merge_and_unload* 方法保存，但你可能希望在 Trainer 训练过程中保存所有中途保存的模型。重载 Trainer 的 *_save_checkpoints* 方法是一种方法，但为了避免麻烦，我这次通过在训练阶段仅获取 *PeftModel* 中原始模型部分来处理。

```py
model = get_peft_model(model, peft_config)
model.base_model.model.lm_head = model.lm_head
model = model.base_model.model
```

我相信还有更高效的处理方法，所以我仍在研究中。

# **使用 GIT-LLM 进行实验**

现在让我们进行一些使用目前开发的模型的实验。

关于训练配置和其他设置的详细信息，请参考[已发布的实现](https://github.com/Ino-Ichan/GIT-LLM)，因为它们本质上遵循相同的方法。

# 数据集：M3IT

对于实验，我想使用一个将图像与文本配对并且易于集成的数据集。在浏览[Hugging Face 的 *Datasets*](https://huggingface.co/datasets)时，我发现了 M3IT，这是一个由上海 AI 实验室开发的用于 Instruction Tuning 的多模态数据集。Instruction Tuning 是一种即使在数据量有限的情况下也能产生令人印象深刻结果的方法。看起来 M3IT 重新标注了各种现有数据集，专门用于 Instruction Tuning。

[https://huggingface.co/datasets/MMInstruction/M3IT](https://huggingface.co/datasets/MMInstruction/M3IT)

这个数据集很容易使用，所以我决定在接下来的实验中利用它。

要使用M3IT进行训练，必须创建一个自定义的Pytorch Dataset。

```py
class SupervisedDataset(Dataset):
    def __init__(
        self,
        vision_model_name: str,
        model_name: str,
        loaded_dataset: datasets.GeneratorBasedBuilder,
        max_length: int = 128,
    ):
        super(SupervisedDataset, self).__init__()
        self.loaded_dataset = loaded_dataset
        self.max_length = max_length

        self.processor = AutoProcessor.from_pretrained("microsoft/git-base")

        # Setting up the corresponding Processor for each model
        self.processor.image_processor = CLIPImageProcessor.from_pretrained(vision_model_name)
        self.processor.tokenizer = AutoTokenizer.from_pretrained(
            model_name, padding_side="right", use_fast=False
        )

    def __len__(self) -> int:
        return len(self.loaded_dataset)

    def __getitem__(self, index) -> dict:
        # cf: https://huggingface.co/datasets/MMInstruction/M3IT#data-instances
        row = self.loaded_dataset[index]

        # Creating text input
        text = f'##Instruction: {row["instruction"]} ##Question: {row["inputs"]} ##Answer: {row["outputs"]}'

        # Loading the image
        image_base64_str_list = row["image_base64_str"]  # str (base64)
        img = Image.open(BytesIO(b64decode(image_base64_str_list[0])))

        inputs = self.processor(
            text,
            img,
            return_tensors="pt",
            max_length=self.max_length,
            padding="max_length",
            truncation=True,
        )
        # batch size 1 -> unbatch
        inputs = {k: v[0] for k, v in inputs.items()}
        inputs["labels"] = inputs["input_ids"]
        return inputs
```

在 *__init__* 函数中，image_processor 和 tokenizer 分别对应其各自的模型。传递的 *loaded_dataset* 参数应来自 MMInstruction/M3IT 数据集。

```py
coco_datasets = datasets.load_dataset("MMInstruction/M3IT", "coco")
test_dataset = coco_datasets["test"]
```

对于 COCO Instruction Tuning 数据集，训练、验证和测试的划分与原始数据集相同，分别为 566,747、25,010 和 25,010 对图像-文本对。其他数据集，如 VQA 或 Video，也可以类似处理，使其成为一个多用途的验证数据集。

示例数据如下：

![](../Images/357b932eebeffa2ad69452bccfd6b0da.png)

图像引用自 M3IT 数据。

该图片的说明如下：

> ##Instruction: 写一个简洁的图像描述，捕捉其主要组成部分、它们之间的关系以及任何显著细节。 ##Question: ##Answer: 一名戴红色头盔的男子骑在小型摩托车上，行驶在泥土道路上。

对于 COCO 数据集，该数据集用于描述，问题部分保持为空。

让我们*深入探讨*处理器的操作。本质上，它对图像进行归一化并对文本进行分词。短于 *max_length* 的输入也会被填充。处理器返回的数据是一个包含以下内容的字典：

+   *input_ids*: 一个分词文本的数组。

+   *attention_mask*: 用于分词文本的掩码（填充部分为 0）。

+   *pixel_values*: 归一化图像的数组，也转换为 Channel-first。

这些关键名称对应于模型的前向函数的参数，因此不应更改。最后，*input_ids* 直接传递给名为 *labels* 的关键。*GitOPTForCausalLM* 的前向函数通过预测下一个词（偏移一个标记）来计算损失。

# 实验 1：确定微调位置

在 GIT 模型的研究论文中，解释了使用了强大的视觉编码器，并且语言模型采用了随机参数。这一次，由于目标是最终使用 7B 类语言模型，因此将应用预训练模型。以下模块将用于微调。*GIT Projection* 作为一个初始化模块，总是包括在内。一些组合可能看起来冗余，但它们在此试验中被探讨而无需过多担忧。

设置为训练的模块会获得梯度，而其余模块则修改为没有梯度。

```py
# Specifying the parameters to train (training all would increase memory usage)
for name, p in model.model.named_parameters():
    if np.any([k in name for k in keys_finetune]):
        p.requires_grad = True
    else:
        p.requires_grad = False
```

本次检查所用的 Vision Encoder 和 LLM 是：

+   openai/clip-vit-base-patch16

+   facebook/opt-350m

训练使用 COCO 数据集，持续 5 轮。

以下是每个实验中训练的目标模块：

+   ***Proj:*** *GIT Projection。随机初始化，因此总是进行训练。*

+   ***LoRA:*** *语言模型中的自注意力的 Query、Key 和 Value 被应用。*

+   ***OPT:*** *所有层都经过训练。*

+   ***ViT:*** *所有层都经过训练。*

+   ***Head:*** *OPT 的最终 lm_head 已经过训练。*

（注意：虽然 LoRA 可以应用于 ViT，但为了避免使实验过于复杂，这次未包含在内。）

![](../Images/45eb87ab254a343616727c1da70b4302.png)

该图显示了训练损失。图例中的 Proj、LoRA、OPT、ViT 和 Head 是上述训练模块。（图由作者制作）

如训练损失图所示，一些组的表现明显不佳。这些情况发生在 OPT 被包括在训练中时。尽管所有实验在相似的条件下进行，但在微调语言模型时可能需要更详细的调整，如学习率。接下来将检查排除 OPT 的训练模型的结果。

![](../Images/74515f8bdfba43737c8cf1dd6be090c1.png)

该图显示了没有完全微调结果的训练损失。图例中的 Proj、LoRA、OPT、ViT 和 Head 是上述训练模块。（图由作者制作）

![](../Images/1861c8654a743bfba2184aff9fecd01a.png)

该图显示了验证损失。图例中的 Proj、LoRA、OPT、ViT 和 Head 是上述训练模块。（图由作者制作）

无论是训练还是验证损失，*Projection+LoRA*模型的减少幅度最大。对最终*Head*层进行微调显示出几乎相同的结果。如果 ViT 也被训练，损失值似乎略高，结果也显得不稳定。即使在 ViT 训练期间添加了 LoRA，损失仍然倾向于较高。对于这个数据的微调，似乎使用一个未更新参数的预训练 ViT 模型会产生更稳定的结果。LoRA 的有效性在多个地方得到了认可，从这个实验中可以明显看出，将 LoRA 添加到 LLM 中改善了训练和验证损失。

评估一些测试数据的推理结果：

![](../Images/a675af6f9da2189db8166911ca6bfccf.png)

GIT-OPT 的示例结果。图片引用自 M3IT 数据集，文本结果由作者的模型生成。

当训练 OPT 本身时，结果与损失结果一样差，使得模型无言以对。此外，训练 ViT 时，输出结果有语义意义，但描述的内容与给定的图像完全不同。然而，其他结果似乎在某种程度上捕捉到了图像的特征。例如，第一张图提到了“猫”和“香蕉”，第二张图识别为“交通标志”。比较有无 LoRA 的结果，后者倾向于重复使用类似的词汇，但使用 LoRA 似乎使其略微更自然。训练*Head*时得到的输出非常有趣，例如第一张图用“playing”代替“eating”。虽然这些结果中有些元素不自然，但可以推测训练成功捕捉了图像特征。

# 实验 2：比较亿级模型

对于早期实验中的微调条件，使用了稍小的语言模型OPT-350m。现在的意图是将语言模型切换到7B模型。不仅仅满足于OPT，还将引入更强的LLM，如LLaMA和MPT。

将这两个模型集成可以按照与OPT类似的方式进行。参考*LlamaModel*和*MPTModel*的前向函数，将投影的图像向量与文本标记结合，并将掩码从*Causal Attention Mask*更改为*GIT的Attention Mask*。需要注意的是：对于MPT，掩码不是(0, -inf)，而是(False, True)。随后的过程可以类似地实现。

要使用7B级模型与OPT，只需将模型名称从facebook/opt-350m更改为facebook/opt-6.7b。

对于LLaMA，考虑到LLaMA2的可用性，它将是首选模型。使用这个预训练模型需要Meta和Hugging Face的批准。需要一个Hugging Face账户，所以确保设置好。批准通常在几小时内完成。之后，登录到执行训练的终端上的Hugging Face。

```py
huggingface-cli login
```

你可以使用在Hugging Face账户中创建的令牌登录 → 设置 → 访问令牌。

训练参数保持一致，使用COCO数据集并持续3个epoch。根据实验1的结果，微调的模块设置为*Projection + LoRA*。

让我们来看看结果。

![](../Images/89a4c0d67844b35567a3da3cc56f3f41.png)

此图显示了训练损失（图由作者制作）

![](../Images/4e4594c8d3298607799e0b4d13f9156a.png)

此图显示了验证损失（图由作者制作）

通过查看损失，可以明显看出，使用LLaMA2和MPT作为LLM的模型显示了更令人满意的减少。让我们也观察一下推理结果。

![](../Images/ad700787927e227b883d10b05f775b25.png)

GIT-LLMs的示例结果。图片引用自M3IT数据集，文本结果由作者的模型生成。

关于第一张图片，对于所有模型，与OPT-350m相比，表情似乎更自然。没有像“一个香蕉和一个香蕉”这样的奇怪表情，突出了LLM的优势。对于第二张图片，仍然存在像“交通灯”或“建筑物”这样的短语困难。对于这种复杂的图像，可能需要考虑升级ViT模型。

最后，让我们对在GPT-4中变得流行的图像进行推理。

![](../Images/f4bac505068c01f309f3c06a5caa8a0c.png)

GIT-LLMs的示例结果。图片引用自[这里](https://www.barnorama.com/wp-content/uploads/2016/12/03-Confusing-Pictures.jpg)，文本结果由作者的模型生成。

尽管使用LLM时预期会有流畅的响应，但结果相当简单。这可能是因为模型仅在COCO上进行了训练。

# 实验3：增加数据量

鉴于之前实验的结果不尽如人意，决定在训练中引入COCO以外的数据。当前使用的M3IT数据集相当全面，能够处理与COCO格式相同的大量数据。

![](../Images/67595c86cfd8ce298c12c80d64cce6aa.png)

该表格引用自“M3IT：面向多模态多语言指令调优的大规模数据集”的表3

打算使用来自该来源的数据，但排除“中文”和“视频”类别。最初，COCO训练数据集包含566,747条数据。通过与其他来源结合，总数增加到1,361,650。尽管规模大致翻倍，但由于任务多样性增加，数据集的质量被认为有所提高。

使用*ConcatDataset*可以轻松处理多个Pytorch数据集。

```py
dataset_list = [
    datasets.load_dataset("MMInstruction/M3IT", i) for i in m3it_name_list
]
train_dataset = torch.utils.data.ConcatDataset([d["train"] for d in dataset_list])
```

训练进行了1轮，并使用LLaMA2模型对*Projection和LoRA*进行了微调，与实验2类似。

由于这次没有可以比较的损失值，我们直接进入推理结果。

![](../Images/121d2e67860d5e31dd3621bd2155961e.png)

GIT-LLaMA2的示例结果。图片来自M3IT数据集，文本结果由作者的模型生成

![](../Images/2a3ec608bceb6afd8eca13d9590fcf4c.png)

GIT-LLaMA2的示例结果。图片来自M3IT数据集，文本结果由作者的模型生成

![](../Images/37f3b927e5e64e8ff16c49504461c639.png)

GIT-LLaMA2的示例结果。图片来自M3IT数据集，文本结果由作者的模型生成

除了解决简单问题外，模型现在还处理更复杂的挑战。通过添加比仅仅是描述更复杂的任务数据集，能力显著扩展。仅用1轮训练就达到这样的准确性令人惊讶。

让我们用以下示例图像进行测试。鉴于数据集的多样性增加，问题的呈现方式略有修改。

![](../Images/f06c8e160a3d026ec3efe7f0224d4be1.png)

GIT-LLaMA2的示例结果。一张图片来自[这里](https://www.barnorama.com/wp-content/uploads/2016/12/03-Confusing-Pictures.jpg)，文本结果由作者的模型生成

尽管“伞状”这一描述仍有些奇怪，但感觉越来越好。为了进一步改进，需要增加训练轮次，添加更多类型或量的数据集，并利用更强大的ViT或LLM。尽管如此，能够在仅半天内开发出这样的模型，考虑到计算和数据资源，确实令人印象深刻。

# 奖励实验。*图像变成文字了吗？*

再看一下GIT结构。

![](../Images/4e2f4f77acb65e4c8a7772a1254074fb.png)

GIT-LLM的简化模型架构（图像由作者制作）

如图所示，在视觉编码器进行特征提取后，图像通过*Visual Projection*与向量化的文本平等对待。换句话说，*Visual Projection* 可能将图像向量转换为文本向量。进行了调查以查看*Visual Projection*之后的向量是什么样的。

虽然有使用*Head*将投影后的向量还原为文本的选项，但发现即使是使用*Embedding*模块向量化的向量也无法通过这种方法还原为原始文本。因此，应将与输入到 LLM 之前的文本向量最接近的向量分配为相应的单词。所有在分词器中注册的令牌都使用 Embedding 模块进行向量化，并且选择了余弦相似度最高的词作为目标词。

本实验使用的图像是一只猫。

![](../Images/4b8b2f288c78ac4a91884cca6859233e.png)

图片摘自 M3IT 数据集。

现在，让我们进行分析（完整分析可在[这里](https://github.com/Ino-Ichan/GIT-LLM/blob/main/notebooks/show_visual_words.ipynb)查看）。首先，对所有注册的令牌进行向量化。

```py
coco_datasets = datasets.load_dataset("MMInstruction/M3IT", "coco")
test_dataset = coco_datasets["test"]
supervised_test_dataset = SupervisedDataset(model_name, vision_model_name, test_dataset, 256)

ids = range(supervised_test_dataset.processor.tokenizer.vocab_size)
all_ids = torch.tensor([i for i in ids]).cuda()
token_id_to_features = model.model.embed_tokens(all_ids)
```

接下来，将提取本来会被 ViT 和 *Projection* 转换为单词的图像向量。

```py
inputs = supervised_test_dataset[0] # Picking a sample arbitrarily
pixel_values = inputs["pixel_values"]
out_vit = model.model.image_encoder(pixel_values).last_hidden_state
out_vit = model.model.visual_projection(out_vit)
```

计算了这些向量和单词向量的点积，最大值的结果被解码为相关的令牌 ID。

```py
# Dot product
nearest_token = out_vit[0] @ token_id_to_features.T

# The index of the maximum value corresponds to the relevant token ID
visual_out = nearest_token.argmax(-1).cpu().numpy()
decoded_text = supervised_test_dataset.processor.tokenizer.batch_decode(visual_out)
print(decoded_text)

"""
['otr', 'eg', 'anto', 'rix', 'Nas', ...]
"""
```

如打印出的*decoded_text*所示，一些不熟悉的单词出现了。由于一些单词重复出现，它们被统计了。

```py
print(pd.Series(decoded_text).value_counts())
"""
mess        43
atura       29
せ           10
Branch      10
Enum         9
bell         9
worden       7
...
"""
```

似乎出现了大量不熟悉的单词。根据位置，它们可能传达有意义的信息。让我们将这些单词绘制在猫的图像上。

```py
n_patches = 14
IMAGE_HEIGHT = 468
IMAGE_WIDTH = 640

y_list = np.arange(15, IMAGE_HEIGHT, IMAGE_HEIGHT//n_patches)
x_list = np.arange(10, IMAGE_WIDTH, IMAGE_WIDTH//n_patches)

plt.figure()
plt.axis("off")
plt.imshow(np.array(image), alpha=0.4)
for index in np.arange(n_patches ** 2):
    y_pos = index // n_patches
    x_pos = index - y_pos * n_patches

    y = y_list[y_pos]
    x = x_list[x_pos]

    # The first token is the bos token, so it is excluded
    word = decoded_text[index + 1]

    # For differentiating words by color
    plt.annotate(word, (x, y), size=7, color="blue")
plt.show()
plt.clf()
plt.close()
```

![](../Images/e3abd839ef5d0ca7bef4cfd2ad1ebfc4.png)

图片由作者制作

经常出现的单词用颜色编码。结果似乎表明它们并不仅仅是投射到有意义的单词上。虽然“***Cat***”这个词可能被叠加在猫的图像上，赋予它一定的相关性，但其含义仍不明确。

该实验中不确定的结果可能是由于强行选择了一个余弦相似度高的单词。无论如何，这种方法并不是简单地将单词投射到图像提示上。从图像中提取的向量通过*Visual Projection* 转换为令牌空间中的向量，这些向量似乎在意义上有些相似，充当神秘的提示。可能最好不要深入探讨这一点。

# 结论

在这篇技术博客文章中，我介绍了将 LLM 集成到视觉语言模型 GIT 的方法。此外，还使用开发的模型进行了各种实验。虽然有成功也有失败，但我希望继续进行视觉语言模型的实验，以积累见解。请将本文作为参考，并鼓励你创建自己的视觉语言模型，探索其潜力。

![](../Images/c0d65bb574d89d207d2ce05bd90c5dad.png)

这是一张使用 Stable Diffusion 创建的 GIT-LLM 插图。（图片由作者制作）
