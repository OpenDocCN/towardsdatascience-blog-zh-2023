# 现代图像语义搜索

> 原文：[https://towardsdatascience.com/modern-semantic-search-for-images-cb1a3242631d?source=collection_archive---------7-----------------------#2023-11-14](https://towardsdatascience.com/modern-semantic-search-for-images-cb1a3242631d?source=collection_archive---------7-----------------------#2023-11-14)

## 一篇利用 Python、Pinecone、Hugging Face 和 Open AI CLIP 模型来创建云照片语义搜索应用程序的操作指南。

[](https://medium.com/@joshpoduska?source=post_page-----cb1a3242631d--------------------------------)[![Josh Poduska](../Images/89ee323bac41d31083206a37df1dd0a3.png)](https://medium.com/@joshpoduska?source=post_page-----cb1a3242631d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cb1a3242631d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cb1a3242631d--------------------------------) [Josh Poduska](https://medium.com/@joshpoduska?source=post_page-----cb1a3242631d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb6dae10267e5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmodern-semantic-search-for-images-cb1a3242631d&user=Josh+Poduska&userId=b6dae10267e5&source=post_page-b6dae10267e5----cb1a3242631d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cb1a3242631d--------------------------------) ·6 min read·2023年11月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcb1a3242631d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmodern-semantic-search-for-images-cb1a3242631d&user=Josh+Poduska&userId=b6dae10267e5&source=-----cb1a3242631d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcb1a3242631d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmodern-semantic-search-for-images-cb1a3242631d&source=-----cb1a3242631d---------------------bookmark_footer-----------)![](../Images/1c4e7abefc2b87c21d19e23600a9530b.png)

作者提供的图片

你想找到“那张几年前的照片”。你记得一些场景细节，想根据特定的短语进行搜索。Apple Photos 不提供语义搜索，而 Google Photos 仅限于少数预定的项目分类器。两者在这类搜索中表现都不佳。我将用两个不寻常的 Google Photos 查询来演示这个问题：“甜甜圈生日蛋糕”和“被雪仗打破的嘴唇”。然后我会分享如何构建自己的语义图像搜索应用程序。

# 演示：当前局限性与现代语义图像搜索的比较

## 示例 #1

我喜欢生日蛋糕。我也喜欢甜甜圈。去年，我有一个绝妙的主意，把两者结合起来，用一堆甜甜圈作为我的生日蛋糕。让我们尝试找到它。

**Google Photos 查询：“**甜甜圈生日蛋糕”

**结果：** 六张没有甜甜圈的蛋糕图片，接着是一张我想要的图片。

![](../Images/dd7019d79c347a6cfc0832aed02f9513.png)

作者提供的图片

**语义搜索应用查询：“**甜甜圈生日蛋糕”

**结果：** 两张和一个视频完全符合我想要的。

![](../Images/7aed89626ffb2d725c711f6c4a81564e.png)

作者提供的图片

## **示例 #2**

我和我的青少年儿子及他的大群朋友一起去滑雪。他们爬上了一个废弃的火车隧道。 “一次性扔雪球，我会拍慢动作视频！”，我喊道。那并不是我最聪明的时刻，因为我没有预见到一个明显的结论，即我最终会成为二十个拥有强壮臂膀的青少年作为目标练习的对象。

**Google Photos 查询：“**雪仗中的破裂嘴唇”

**结果：**

![](../Images/dab76c3befff8ce7b26fe2ae9da7df38.png)

作者创建的图片

当前的 Google 图像分类模型仅限于它所训练过的词汇。

**语义搜索应用查询：“**雪仗中的破裂嘴唇”

**结果：** 破裂的嘴唇照片（未展示）和破裂嘴唇之前的视频分别是结果一和二。

![](../Images/1c4e7abefc2b87c21d19e23600a9530b.png)

作者提供的图片

# OpenAI CLIP 模型和应用架构

[CLIP](https://openai.com/research/clip) 使模型能够学习如何将图像像素与文本关联，并赋予它寻找“甜甜圈蛋糕”和“破裂的嘴唇”等内容的灵活性——这些内容在训练图像分类器时你可能从未考虑过。它代表了对比语言-图像预训练。它是一个开源的多模态零样本模型。它在数百万张带有描述性字幕的图像上进行了训练。

给定图像和文本描述，模型可以预测该图像最相关的文本描述，而无需针对特定任务进行优化。

![](../Images/5b99366aac6000d76388a6ba9ff53b17.png)

[来源: Nikos Karfitsas, Towards Data Science](/clip-the-most-influential-ai-model-from-openai-and-how-to-use-it-f8ee408958b1)

大多数在线教程中的 CLIP 架构对于概念验证来说足够好，但不适合企业级应用。在这些教程中，CLIP 和 Hugging Face 处理器将嵌入保持在内存中，以作为运行相似度评分和检索的向量存储。

![](../Images/be21708790b6b623a350f154e8a4e891.png)

作者创建的图片

像 Pinecone 这样的向量数据库是扩展此类应用程序的关键组件。它提供了简化、稳健、企业级的功能，如图像的批处理和流处理、嵌入的企业管理、低延迟检索和元数据过滤。

![](../Images/9fa02718f8ecf9777750b0f46ad35495.png)

作者创建的图片

# 构建应用程序

此应用程序的代码和支持文件可在 GitHub 上找到，链接为 [https://github.com/joshpoduska/llm-image-caption-semantic-search](https://github.com/joshpoduska/llm-image-caption-semantic-search)。使用它们构建一个用于云照片的语义搜索应用程序。

该应用程序在配备足够内存的笔记本电脑上本地运行。我在 MacBook Pro 上进行了测试。

## 构建应用程序所需的组件。

+   **松果** 或类似的向量数据库用于嵌入存储和语义搜索（本教程使用免费版松果已足够）。

+   **Hugging Face** 模型和管道。

+   **OpenAI CLIP** **模型**用于图像和查询文本嵌入的创建（从 Hugging Face 可访问）。

+   **Google Photos API** 用于访问您的个人 Google Photos。

## 在开始之前提供有用信息。

+   GitHub 用户，polzerdo55862，通过 Python 在使用 Google Photos API 上有一个 [优秀的笔记本教程](https://github.com/polzerdo55862/google-photos-api/blob/main/Google_API.ipynb)。

+   [松果快速浏览](https://github.com/pinecone-io/examples/blob/master/docs/quick-tour/hello-pinecone.ipynb)展示了如何初始化、填充和删除松果“索引”。

+   松果示例展示了 [如何查询索引](https://github.com/pinecone-io/examples/blob/master/docs/semantic-search.ipynb)。

+   HuggingFace 示例展示了 [如何在独立查询和搜索管道中使用 CLIP](https://huggingface.co/openai/clip-vit-large-patch14)。

+   Antti Havanko 分享了一个示例，展示了如何使用 CLIP 生成嵌入以在向量搜索引擎中使用，链接为 [如何使用 CLIP 生成嵌入](https://anttihavanko.medium.com/building-image-search-with-openai-clip-5a1deaa7a6e2)。

## 访问您的图像。

Google Photos API 具有几个关键的数据字段值得注意。更多细节请参阅 [API 参考文档](https://developers.google.com/photos/library/guides/access-media-items)。

+   **Id** 是不可变的。

+   **baseUrl** 允许您访问媒体项的字节。它们有效期为 60 分钟。

您可以简单地使用 pandas、JSON 和 requests 库来加载包含图像 ID、URL 和日期的 DataFrame。

## 生成图像嵌入。

使用 Hugging Face 和 OpenAI CLIP 模型，这一步是整个应用程序中最简单的步骤。

```py
from sentence_transformers import SentenceTransformer
img_model = SentenceTransformer('clip-ViT-B-32')
embeddings = img_model.encode(images)
```

## 创建元数据。

语义搜索通常通过元数据过滤得到增强。在此应用程序中，我使用照片的日期来提取年、月和日。这些信息存储为 DataFrame 字段中的字典。松果查询可以使用此字典通过字典中的元数据来过滤搜索。

这是我 pandas DataFrame 的第一行，包含图像字段、向量和元数据字典字段。

![](../Images/942e28262b92ccdbd38c53267175c137.png)

图像由作者提供。

## 加载嵌入。

松果有关异步和并行加载的优化。基本加载功能如下所示。

```py
index.upsert(vectors=ids_vectors_chunk, async_req=True)
```

## 查询嵌入。

要使用 CLIP 模型查询图像，我们需要传递语义查询文本。通过加载 CLIP 文本嵌入模型来简化此过程。

```py
text_model = SentenceTransformer(‘sentence-transformers/clip-ViT-B-32-multilingual-v1’)
```

现在我们可以为我们的搜索短语创建一个嵌入，并将其与存储在Pinecone中的图像嵌入进行比较。

```py
# create the query vector
xq = text_model.encode(query).tolist()

# now query
xc = index.query(xq,
                filter= {
                "year": {"$in":years_filter},
                "month": {"$in":months_filter}
                        },
                top_k= top_k,
                include_metadata=True)
```

# 结论

CLIP模型非常了不起。它是一个通用知识的零样本模型，已经学会了将图像与文本关联，从而摆脱了在预定义类别上训练图像分类器的限制。当我们将这一点与像Pinecone这样的企业级向量数据库的强大功能结合时，我们可以创建具有低延迟和高保真度的语义图像搜索应用。这只是生成性AI每天涌现出的令人兴奋的应用之一。
