# Llama 2中的停止生成挑战

> 原文：[https://towardsdatascience.com/challenges-in-stop-generation-within-llama-2-25f5fea8dea2?source=collection_archive---------1-----------------------#2023-09-10](https://towardsdatascience.com/challenges-in-stop-generation-within-llama-2-25f5fea8dea2?source=collection_archive---------1-----------------------#2023-09-10)

## 潜在解决方案的探索

[](https://medium.com/@vanillaxiangshuyang?source=post_page-----25f5fea8dea2--------------------------------)[![Shuyang Xiang](../Images/36a5fd18fd9b7b88cb41094f09b83882.png)](https://medium.com/@vanillaxiangshuyang?source=post_page-----25f5fea8dea2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----25f5fea8dea2--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----25f5fea8dea2--------------------------------) [Shuyang Xiang](https://medium.com/@vanillaxiangshuyang?source=post_page-----25f5fea8dea2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9b74bc8c860d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchallenges-in-stop-generation-within-llama-2-25f5fea8dea2&user=Shuyang+Xiang&userId=9b74bc8c860d&source=post_page-9b74bc8c860d----25f5fea8dea2---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----25f5fea8dea2--------------------------------) · 9 分钟阅读 · 2023年9月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F25f5fea8dea2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchallenges-in-stop-generation-within-llama-2-25f5fea8dea2&user=Shuyang+Xiang&userId=9b74bc8c860d&source=-----25f5fea8dea2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F25f5fea8dea2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchallenges-in-stop-generation-within-llama-2-25f5fea8dea2&source=-----25f5fea8dea2---------------------bookmark_footer-----------)![](../Images/919c3755f4c37f74e7796e7097458f19.png)

Llama: 图片由[柳德米拉·舒瓦洛娃](https://unsplash.com/@liudmila19)提供

Meta发布的Llama 2在社区中引发了兴奋，标志着一个新的时代的开始，此前大型语言模型的良好表现只能通过公司特定的API访问。

然而，重要的是要承认这些模型固有的一些缺陷。其中，生成停止问题尤为突出。我的个人经历表明，这些模型往往难以确定合适的‘停止’点，使它们在何时结束文本生成方面感到不确定。

在这篇博客文章中，我将深入探讨最小的Llama 2模型——Llama 2–7b模型中停止生成失败的问题，并讨论几种潜在的解决方案。接下来的实现可以在这个GoogleGolab [notebook](https://colab.research.google.com/drive/12R6HXUYMbhGh6dhMUH6FiOWWUIrJCaz6?authuser=1#scrollTo=VjJCzLmYmWEo)中找到，运行时类型为T4。

# 停止生成失败

在本节中，我们将利用Google Colab中配备充足高RAM资源的T4 GPU来操作Llama 2–7b模型（2.21积分/小时）。需要记住的是，T4 GPU具有16 GB的VRAM容量，刚好可以容纳Llama 2–7b的权重（7b × 2 字节 = 14 GB的FP16）。

为了有效管理VRAM的使用，我们将采用一种叫做量化的技术。量化是一种在推理过程中通过使用低精度数据类型表示权重和激活值来最小化计算和内存需求的方法。

现在让我们深入研究以下代码片段。在这里，我们将演示如何加载带有Bite和Byte配置的“meta-llama/Llama-2–7b-chat-hf”模型，并基于该加载模型设置文本生成管道。

```py
# bit and byte configuration
bnb_config = transformers.BitsAndBytesConfig(
    load_in_4bit=True,  # 4-bit quantization
    bnb_4bit_quant_type='nf4',  # Normalized float 4
    bnb_4bit_use_double_quant=True,  # Second quantization after the first
    bnb_4bit_compute_dtype=bfloat16  # Computation type
)
model_id= "meta-llama/Llama-2-7b-chat-hf"
device = f'cuda:{cuda.current_device()}' if cuda.is_available() else 'cpu'

# Llama 2 Tokenizer
tokenizer =transformers.AutoTokenizer.from_pretrained(model_id)

# Llama 2 Model
model = transformers.AutoModelForCausalLM.from_pretrained(
    model_id,
    trust_remote_code=True,
    quantization_config=bnb_config,
    device_map='auto',
)
model.eval()

# Difine pipeline 
 transformers.pipeline(model=model,
    tokenizer=tokenizer,
    return_full_text=True,  # langchain expects the full text
    task='text-generation',
    # we pass model parameters here too
    max_new_tokens=500,  # max number of tokens to generate in the output
    temperature=0.1,
    )
```

这个配置使我们能够在一分钟以内为给定提示生成文本。让我们用一个简单的问题进行测试：“我在巴黎可以做些什么？”下面是答案（请注意，由于温度设置不同，您的结果可能会有所不同）。

```py
Paris, the capital of France, is known for its stunning architecture, art museums, historical landmarks, and romantic atmosphere. everybody's dream destination. Here are some of the top things to do in Paris:

1\. Visit iconic landmarks: The Eiffel Tower, the Louvre Museum, Notre-Dame Cathedral, Arc de Triomphe, and the Champs-Élysées are must-visit attractions.

2\. Explore museums and art galleries: The Louvre, Musée d'Orsay, and Centre Pompidou are some of the world's most famous museums.

3\. Take a river cruise: A Seine River cruise is a great way to see the city's landmarks and bridges while learning about its history and culture.

4\. Discover the city's neighborhoods: From the fashionable Champs-Élysées to the charming Montmartre, each Parisian neighborhood has its own unique character and charm.

# I skipped some text here because of the limt of space# 

Of course, there are countless
```

很明显，这个模型难以生成令人满意的响应；它似乎在确定何时结束输出方面存在困难。通过对生成的文本进行分词处理，可以明显看出最终的标记不是2，而2代表了模型分词器中的eos（序列结束）标记。

通过仔细检查模型提供的标记分数（概率），我注意到**token_id 2 (eso_token_id) 的分数为“-inf。”**这意味着它不可能被生成。

# 问题解决尝试

在本节中，我们将探讨几种旨在解决当前问题的潜在解决方案。需要记住的是，这里讨论的解决方案代表了积极的努力，但它们可能并不总是能解决所面临的问题。

## Logits处理器

像Llama 2这样的语言模型处理一系列文本标记作为输入，并根据从初始标记到当前标记的上下文生成下一标记的条件概率。鉴于此，值得考虑在接近最大标记限制时手动调整这些概率，以提高遇到eos标记的可能性。我们通过定义一个名为“EosTokenRewardLogitsProcessor”的自定义Logits处理器来实现这一点，该处理器具有两个初始输入eos_token_id和max_length，其中后者表示模型应生成eos标记的最大长度：

```py
class EosTokenRewardLogitsProcessor(LogitsProcessor):
  def __init__(self,  eos_token_id: int, max_length: int):

        if not isinstance(eos_token_id, int) or eos_token_id < 0:
            raise ValueError(f"`eos_token_id` has to be a positive integer, but is {eos_token_id}")

        if not isinstance(max_length, int) or max_length < 1:
          raise ValueError(f"`max_length` has to be a integer bigger than 1, but is {max_length}")

        self.eos_token_id = eos_token_id
        self.max_length=max_length

  def __call__(self, input_ids: torch.LongTensor, scores: torch.FloatTensor) -> torch.FloatTensor:
    cur_len = input_ids.shape[-1]
    # start to increese the reward of the  eos_tokekn from 80% max length  progressively on length
    for cur_len in (max(0,int(self.max_length*0.8)), self.max_length ):
      ratio = cur_len/self.max_length
      num_tokens = scores.shape[1] # size of vocab
      scores[:, [i for i in range(num_tokens) if i != self.eos_token_id]] =\
      scores[:, [i for i in range(num_tokens) if i != self.eos_token_id]]*ratio*10*torch.exp(-torch.sign(scores[:, [i for i in range(num_tokens) if i != self.eos_token_id]]))
      scores[:, self.eos_token_id] = 1e2*ratio
    return scores
```

在类的“__call__”方法中，我们根据序列的长度增强eos_token的概率（得分）。当长度接近指定最大长度的80%时，我们将eos_token_id的得分设置为1e2乘以长度比例，并相应地调整其他令牌的得分。

现在在管道的定义中声明logits处理器：

```py
pipe = transformers.pipeline(model=model,
    tokenizer=tokenizer,
    return_full_text=True,  # langchain expects the full text
    task='text-generation',
    # we pass model parameters here too
    #stopping_criteria=stopping_criteria,  # without this model rambles during chat
    logits_processor=logits_process_list,
    max_new_tokens=500,  # max number of tokens to generate in the output
    temperature=0.1,
    )
```

使用相同的提示“What Can I do in Paris”再次运行管道，我们得到：

```py
Paris, the capital of France, is known for its stunning architecture, art museums, historical landmarks, and romantic atmosphere.
```

它运行得很好！我们即使得到的答案可能看起来很简短，但它是完整的。

## 微调

如果模型未能生成EOS令牌，为什么不考虑指示它这样做呢？通过使用包括以EOS令牌结尾的答案的数据集来微调模型，以提高模型性能的概念无疑是一个值得探索的有前景的途径。

在这一部分，我将毫不掩饰地使用这篇博客文章中奠定的基础，这篇文章采用了参数高效的微调（PEFT）方法，如QLoRA，来微调Llama 2–7b模型。与其前身LoRA类似，QLoRA利用一小组可训练的参数（适配器），同时保持核心模型参数不变。它引入了两个值得注意的创新：4-bit NormalFloat (NF4)，一种对正常数据信息理论上最优的数据量化方法，以及双重量化。欲了解更多深入信息，请参考[原始论文](https://arxiv.org/abs/2305.14314)，如果您对该主题有进一步的兴趣。

让我们在一个名为‘timdettmers/openassistant-guanaco’的数据集上训练模型，您可以在hugging face数据库中找到这个数据集。该数据集的格式如下，其中人类和助手的对话由“###”分隔。

![](../Images/a593fefb022a90da04e27e82ecb7e454.png)

图片作者：“timdettmers/openassistant-guanaco”的数据集

在训练之前，我们需要将数据转换为Llama 2提示模板：

```py
<s>[INST] <<SYS>>
{your_system_message}
<</SYS>> {user_message_1} [/INST]
```

我将在这里跳过数据集转换的细节。现在让我们看看以下代码给出的训练的主要部分：

```py
# Load LoRA configuration
peft_config = LoraConfig(
    lora_alpha=lora_alpha,
    lora_dropout=lora_dropout,
    r=lora_r,
    bias="none",
    task_type="CAUSAL_LM",
)

# Set supervised fine-tuning parameters
trainer = SFTTrainer(
    model=model,
    train_dataset=dataset,
    peft_config=peft_config,
    dataset_text_field="text",
    max_seq_length=max_seq_length,
    tokenizer=tokenizer,
    args=training_arguments,
    packing=packing,
)

# Train model
trainer.train()
```

在一个包含指令和响应的数据集中，我们的方法涉及使用监督训练器（SFTainer）与QLoRA方法相结合，以微调语言模型（LLM）中的权重参数。我们的主要目标是最小化生成的答案与真实响应之间的差异，真实响应作为我们的参考标签。

在这个配置中，一个重要的参数是“lora r”，它代表了一个相对较小的值，涉及到秩分解权重矩阵的第二维和第一维。训练仅在这两个矩阵上进行，补充了现有的权重。

我们训练模型250步，训练损失如下图所示：

![](../Images/21bae74012046266638a79cc1aba0c4c.png)

图片来源：Llama 2训练250步的损失

现在让我们使用微调后的模型运行管道。这一次，我们得到了：

```py
 Paris, the capital of France, is known for its stunning architecture, rich history, and cultural attractions. Here are some popular things to do in Paris:

1\. Visit iconic landmarks: The Eiffel Tower, Arc de Triomphe, Notre-Dame Cathedral, and the Louvre Museum are some of the most famous landmarks in Paris.
2\. Explore museums and art galleries: Paris is home to many world-class museums and art galleries, including the Louvre, the Musée d'Orsay, and the Pompidou Center.
3\. Enjoy the city's gardens and parks: Paris has several beautiful gardens and parks, including the Luxembourg Gardens, the Tuileries Garden, and the Champ de Mars.
4\. Take a river cruise: A river cruise along the Seine is a great way to see the city's sights and learn about its history.
5\. Shop on the Champs-Élysées: The famous avenue is lined with high-end boutiques, department stores, and cafes.
6\. Attend a cabaret show: The Moulin Rouge is a famous cabaret that offers a lively and entertaining show.
7\. Visit Montmartre: The charming neighborhood of Montmartre is known for its artistic vibe, street performers, and stunning views of the city.
8\. Take a food tour: Paris is known for its culinary delights, and a food tour is a great way to sample the city's famous cuisine.
9\. Visit the Palace of Versailles: The Palace of Versailles is a beautiful royal palace located just outside of Paris.
10\. Attend a fashion show: Paris is the fashion capital of the world, and attending a fashion show is a great way to see the latest trends and styles.

These are just a few of the many things to do in Paris. The city has something for everyone, from history and culture to food and entertainment.
```

这是一个相当美丽的答案！

## 附加：给出不同的提示

最终，我们手头有一个简单而有效的技巧，特别是当模型的冗长不是问题时。我们可以在提示中明确说明我们需要一个简洁的回答。例如，当我问模型：“在巴黎我可以做什么？请用五句话或更少的句子回答，”它会提供：

```py
Sure! Here are five things you can do in Paris: 1\. Visit the Eiffel Tower, a iconic landmark that offers stunning views of the city. 2\. Explore the Louvre Museum and see famous artworks like the Mona Lisa. 3\. Stroll along the Seine River and enjoy the city's picturesque bridges and charming streets. 4\. Visit the Notre-Dame Cathedral, a beautiful and historic church. 5\. Enjoy a delicious French meal at a traditional bistro or restaurant.
```

这是一个简短但干净且完整的回答。

## 停止标准：一个失败的尝试

对于感兴趣的用户，Hugging Face推出了另一个名为StoppingCriteria的API，旨在建立特定条件以强制序列停止。然而，当涉及到定义一个在遇到某些标记（例如‘\n’）时停止模型的自定义标准时，它可能无法提供一个全面的解决方案。例如，我尝试创建一个StopOnTokens类：

```py
# define custom stopping criteria object
class StopOnTokens(StoppingCriteria):
    def __call__(self, input_ids: torch.LongTensor, scores: torch.FloatTensor, **kwargs) -> bool:
        for stop_ids in stop_token_ids:
            if torch.eq(input_ids[0][-len(stop_ids):], stop_ids).all():
                return True
        return False

stopping_criteria = StoppingCriteriaList([StopOnTokens()])
```

但是，模型仍然无法给出完整的回答。

# 结论

在这篇博客文章中，我强调了Llama 2中生成停止的问题，并介绍了几种临时解决方案。再次，我跳过了很多实施细节，我建议你深入查看我的笔记本。

![](../Images/d6efeaff2e28e01d03f2e75eca9f4d09.png)

图片由[Jose Aragones](https://unsplash.com/@jodaarba)提供

但是，需要注意的是，这些解决方案旨在在短期内提高响应的用户友好性，但我们迫切期待一个永久的解决方案来解决这个问题。
