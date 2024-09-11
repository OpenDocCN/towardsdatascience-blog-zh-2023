# 使用 PostgreSQL 和 OpenAI 嵌入实现语义搜索

> 原文：[`towardsdatascience.com/semantic-search-with-postgresql-and-openai-embeddings-4d327236f41f?source=collection_archive---------3-----------------------#2023-11-21`](https://towardsdatascience.com/semantic-search-with-postgresql-and-openai-embeddings-4d327236f41f?source=collection_archive---------3-----------------------#2023-11-21)

[](https://curiousdima.medium.com/?source=post_page-----4d327236f41f--------------------------------)![Dima Timofeev](https://curiousdima.medium.com/?source=post_page-----4d327236f41f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4d327236f41f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4d327236f41f--------------------------------) [Dima Timofeev](https://curiousdima.medium.com/?source=post_page-----4d327236f41f--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1df0a90be7e9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsemantic-search-with-postgresql-and-openai-embeddings-4d327236f41f&user=Dima+Timofeev&userId=1df0a90be7e9&source=post_page-1df0a90be7e9----4d327236f41f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4d327236f41f--------------------------------) ·4 分钟阅读·2023 年 11 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4d327236f41f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsemantic-search-with-postgresql-and-openai-embeddings-4d327236f41f&user=Dima+Timofeev&userId=1df0a90be7e9&source=-----4d327236f41f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4d327236f41f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsemantic-search-with-postgresql-and-openai-embeddings-4d327236f41f&source=-----4d327236f41f---------------------bookmark_footer-----------)![](img/2c0f934e64c66aa85851890637f67bfa.png)

图片由 [Igor Omilaev](https://unsplash.com/@omilaev) 提供，来源于 [Unsplash](https://unsplash.com/photos/a-blue-background-with-a-bunch-of-cookies-and-a-red-object-Z2PahC-Fi08)

在公司数据库中实现语义搜索可能是具有挑战性的，并且需要付出大量的努力。然而，真的非得如此吗？在这篇文章中，我将展示如何利用 PostgreSQL 和 OpenAI 嵌入技术来在你的数据上实现语义搜索。如果你不希望使用 OpenAI 嵌入 API，我还会为你提供免费的嵌入模型链接。

从很高的层面来看，具有 LLM 的向量数据库允许对可用数据（存储在数据库、文档等中）进行语义搜索。感谢 “[Efficient Estimation of Word Representations in Vector Space](https://arxiv.org/pdf/1301.3781.pdf)” 论文（也称为 “Word2Vec 论文”），由传奇 [Jeff Dean](https://en.wikipedia.org/wiki/Jeff_Dean) 共同作者，我们知道如何将单词表示为实值向量。词嵌入是单词在向量空间中的密集向量表示，其中意义相似的单词彼此接近。词嵌入捕捉了单词之间的语义关系，并且有多种技术来创建它们。

![](img/615281e0ab21b19949fcf1800e50fe06.png)

图片由作者提供

让我们实践并使用 OpenAI 的 [*text-embedding-ada*](https://openai.com/blog/new-and-improved-embedding-model) 模型！距离函数的选择通常没那么重要。OpenAI 推荐使用余弦相似度。如果你不想使用 OpenAI 的嵌入，倾向于本地运行不同的模型而不是进行 API 调用，我建议考虑其中一种 [SentenceTransformers 预训练模型](https://www.sbert.net/docs/pretrained_models.html)。选择你的模型时要谨慎。

```py
import os

import openai
from openai.embeddings_utils import cosine_similarity

openai.api_key = os.getenv("OPENAI_API_KEY")

def get_embedding(text: str) -> list:
 response = openai.Embedding.create(
     input=text,
     model="text-embedding-ada-002"
 )
 return response['data'][0]['embedding']

good_ride = "good ride"
good_ride_embedding = get_embedding(good_ride)
print(good_ride_embedding)
# [0.0010935445316135883, -0.01159335020929575, 0.014949149452149868, -0.029251709580421448, -0.022591838613152504, 0.006514389533549547, -0.014793967828154564, -0.048364896327257156, -0.006336577236652374, -0.027027441188693047, ...]
len(good_ride_embedding)
# 1536
```

既然我们已经理解了嵌入的概念，让我们利用它来排序一些评论。

```py
good_ride_review_1 = "I really enjoyed the trip! The ride was incredibly smooth, the pick-up location was convenient, and the drop-off point was right in front of the coffee shop."
good_ride_review_1_embedding = get_embedding(good_ride_review_1)
cosine_similarity(good_ride_review_1_embedding, good_ride_embedding)
# 0.8300454513797334

good_ride_review_2 = "The drive was exceptionally comfortable. I felt secure throughout the journey and greatly appreciated the on-board entertainment, which allowed me to have some fun while the car was in motion."
good_ride_review_2_embedding = get_embedding(good_ride_review_2)
cosine_similarity(good_ride_review_2_embedding, good_ride_embedding)
# 0.821774476808789

bad_ride_review = "A sudden hard brake at the intersection really caught me off guard and stressed me out. I wasn't prepared for it. Additionally, I noticed some trash left in the cabin from a previous rider."
bad_ride_review_embedding = get_embedding(bad_ride_review)
cosine_similarity(bad_ride_review_embedding, good_ride_embedding)
# 0.7950041130579355
```

虽然绝对差异可能看起来很小，但考虑到数以千计的评论排序函数。在这种情况下，我们可以优先突出显示顶部的正面评论。

一旦一个单词或文档被转换为嵌入，它可以存储在数据库中。然而，这个动作并不会自动将数据库分类为向量数据库。只有当数据库开始支持对向量的快速操作时，我们才能称其为向量数据库。

目前有许多商业和开源的向量数据库，这使得它成为一个高度讨论的话题。我将通过使用 [pgvector](https://www.postgresql.org/about/news/pgvector-050-released-2700/)，一个开源的 PostgreSQL 扩展，来演示向量数据库的功能，它为 arguably 最受欢迎的数据库提供了向量相似性搜索功能。

让我们运行带有 pgvector 的 PostgreSQL 容器：

```py
docker pull ankane/pgvector

docker run --env "POSTGRES_PASSWORD=postgres" --name "postgres-with-pgvector" --publish 5432:5432 --detach  ankane/pgvector
```

让我们启动 [pgcli](https://github.com/dbcli/pgcli) 以连接到数据库 (*pgcli postgres://postgres:postgres@localhost:5432*)，创建一个表，插入我们计算的嵌入，然后选择相似的项目：

```py
-- Enable pgvector extension.
CREATE EXTENSION vector;

-- Create a vector column with 1536 dimensions.
-- The `text-embedding-ada-002` model has 1536 dimensions.
CREATE TABLE reviews (text TEXT, embedding vector(1536));

-- Insert three reviews from the above. I omitted the input for your convinience.
INSERT INTO reviews (text, embedding) VALUES ('I really enjoyed the trip! The ride was incredibly smooth, the pick-up location was convenient, and the drop-off point was right in front of the coffee shop.', '[-0.00533589581027627, -0.01026702206581831, 0.021472081542015076, -0.04132508486509323, ...');
INSERT INTO reviews (text, embedding) VALUES ('The drive was exceptionally comfortable. I felt secure throughout the journey and greatly appreciated the on-board entertainment, which allowed me to have some fun while the car was in motion.', '[0.0001858668401837349, -0.004922827705740929, 0.012813017703592777, -0.041855424642562866, ...');
INSERT INTO reviews (text, embedding) VALUES ('A sudden hard brake at the intersection really caught me off guard and stressed me out. I was not prepared for it. Additionally, I noticed some trash left in the cabin from a previous rider.', '[0.00191772251855582, -0.004589076619595289, 0.004269456025213003, -0.0225954819470644, ...');

-- sanity check
select count(1) from reviews;
-- +-------+
-- | count |
-- |-------|
-- | 3     |
-- +-------+
```

我们现在已经准备好搜索相似的文档。我再次缩短了“good ride”的嵌入，因为打印 1536 维度是多余的。

```py
--- The embedding we use here is for "good ride"
SELECT substring(text, 0, 80) FROM reviews ORDER BY embedding <-> '[0.0010935445316135883, -0.01159335020929575, 0.014949149452149868, -0.029251709580421448, ...';

-- +--------------------------------------------------------------------------+
-- | substring                                                                |
-- |--------------------------------------------------------------------------|
-- | I really enjoyed the trip! The ride was incredibly smooth, the pick-u... |
-- | The drive was exceptionally comfortable. I felt secure throughout the... |
-- | A sudden hard brake at the intersection really caught me off guard an... |
-- +--------------------------------------------------------------------------+
SELECT 3
Time: 0.024s
```

完成了！如你所见，我们已经为多个文档计算了嵌入向量，将其存储在数据库中，并进行了向量相似性搜索。这些应用的潜力巨大，涵盖了从企业搜索到医疗记录系统中的相似症状患者识别等众多领域。此外，这种方法不仅限于文本；也可以计算声音、视频和图像等其他类型数据的相似性。

享受吧！
