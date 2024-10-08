# BigQuery 中的新生成 AI 功能

> 原文：[`towardsdatascience.com/the-new-generative-ai-function-in-bigquery-38d7a16d4efc`](https://towardsdatascience.com/the-new-generative-ai-function-in-bigquery-38d7a16d4efc)

## 如何使用 BigQuery 的 GENERATE_TEXT 远程函数

[](https://medium.com/@martosi?source=post_page-----38d7a16d4efc--------------------------------)![Marina Tosic](https://medium.com/@martosi?source=post_page-----38d7a16d4efc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----38d7a16d4efc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----38d7a16d4efc--------------------------------) [Marina Tosic](https://medium.com/@martosi?source=post_page-----38d7a16d4efc--------------------------------)

·发表于[数据科学之路](https://towardsdatascience.com/?source=post_page-----38d7a16d4efc--------------------------------) ·阅读时间 10 分钟·2023 年 12 月 1 日

--

![](img/6bc5b9d4bfe58fdc6f69a6a33a2454d3.png)

“每个人都可以通过 SQL 知识和良好的提示结构在 BigQuery 中进行编程和自然语言处理分析” [照片来源：[Adi Goldstein](https://unsplash.com/@adigold1?utm_source=medium&utm_medium=referral)于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)]

# 介绍

自从我开始使用 Google 平台以来，Google 一直没有停止用其[BigQuery](https://cloud.google.com/bigquery)（BQ）功能和开发给我带来惊喜。

四年前我经历了真正的“哇”时刻。

我记得就像昨天一样，当时我坐在[大数据伦敦](https://bigdataldn.com/)2019 年会议的前排。那时我完全不知道只用 BQ 函数就能创建机器学习模型的可能性，或者更准确地说，不知道 BQ 机器学习（[BQML](https://cloud.google.com/bigquery/docs/bqml-introduction)）是什么。

至少在会议环节之前，Google 的同事展示了如何仅使用 Google 的 SQL 创建[分类](https://cloud.google.com/bigquery/docs/logistic-regression-prediction)、[聚类](https://cloud.google.com/bigquery/docs/kmeans-tutorial)和[时间序列预测](https://cloud.google.com/bigquery/docs/arima-single-time-series-forecasting-tutorial)模型。

当时我脑海中闪过的第一个念头是“你一定是在开玩笑”。

我脑海中的第二个念头是：“这是否意味着所有只知道 SQL 的人都能创建机器学习模型？”

正如你可以想象的那样，如果你使用 BigQuery 作为数据仓库，那么答案是“***是的***”。

现在，在使用了[**BQML**](https://cloud.google.com/bigquery/docs/bqml-introduction)函数一段时间后，上述问题的正确答案是“***也许***”。

这意味着即使`[CREATE MODEL](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-create)`语法是用 SQL 编写的，仍然需要了解机器学习建模和统计学知识。

> 换句话说，你仍然需要理解不同类型机器学习用例的模型背后的数学知识（[监督学习](https://cloud.google.com/discover/what-is-supervised-learning#section-1)/[无监督学习](https://cloud.google.com/discover/what-is-unsupervised-learning)），进行特征工程、超参数调整和模型评估任务。

快进到 2023 年，BigQuery 通过其新功能进一步让我惊叹。

这一次，我们讨论的是新的生成性 AI BigQuery 机器学习函数。

利用这些新功能，数据工程师和分析师可以在 BQ 表中存储的文本数据上执行生成性自然语言任务，只需几行查询代码。

因此，本博客文章的目标是展示 BQ 在生成性 AI 中的新分析进展，重点介绍一个功能**—** [***GENERATE_TEXT***](https://cloud.google.com/bigquery/docs/generate-text-tutorial)**功能。**

# 关于 GENERATE_TEXT 函数

*GENERATE_TEXT*函数的主要思想是通过 BigQuery 中的 SQL 和提示指令来协助数据专业人员完成以下任务[[1](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-generate-text#:~:text=GENERATE_TEXT%20function,%20which%20lets%20you,Sentiment%20Analysis)]：

+   ***分类***

+   ***情感分析***

+   ***实体提取***

+   ***提取式问答***

+   ***总结***

+   ***以不同风格重写文本***

+   ***广告文案生成***

+   ***概念构思***

换句话说，该函数可以利用[Vertex AI](https://cloud.google.com/vertex-ai) `[text-bison](https://cloud.google.com/vertex-ai/docs/generative-ai/model-reference/text)`自然语言[基础模型](https://cloud.google.com/vertex-ai/docs/generative-ai/learn/models#foundation_models) [[1](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-generate-text#:~:text=GENERATE_TEXT%20function,%20which%20lets%20you,Sentiment%20Analysis)]在 BQ 中对存储的文本属性执行生成性自然语言任务。

它的工作原理如下[[1](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-generate-text#:~:text=GENERATE_TEXT%20function,%20which%20lets%20you,Sentiment%20Analysis)]：

+   ***首先***，它向代表 Vertex AI `text-bison`自然语言基础模型（LLM）的 BigQuery ML 远程模型发送请求。

+   ***然后***，它返回带有定义输入参数的响应。

这意味着不同的*函数参数*和*输入提示设计*（分析的提示指令）会影响 LLM 的响应。

说到这里，可以将以下函数特定参数传递给 *GENERATE_TEXT* 函数，并影响响应质量 [[1](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-generate-text#:~:text=GENERATE_TEXT%20function,%20which%20lets%20you,Sentiment%20Analysis)]：

**#1:** `model` [`STRING`] → 指定使用 `text-bison` Vertex AI LLMs 之一的远程模型的名称。

**#2:** `query_statement` [`STRING`] → 指定用于生成提示数据的 SQL 查询。

**#3:** `max_output_tokens` [`INT64`] → 设置模型输出的最大 token 数量。对于较短的响应，可以指定较低的值。

+   ***注意***: 一个 token 可能比一个词小，约为四个字符。

**#4:** `temperature` [`FLOAT64`] → 范围在`[0.0,1.0]`内的参数，用于在响应生成期间进行采样，当 `top_k` 和 `top_p` 被应用时会发生采样。

+   ***注意：*** 该参数表示 token 选择的随机程度，即，较低的值适用于需要更确定性响应的提示，而较高的值可能导致更具多样性的结果。

**#5:** `top_k` [`FLOAT64`] → 范围在`[1,40]`内的参数，决定模型如何选择输出的 token。

+   ***注意：*** 为了获得更少的随机响应，应指定较低的值。

**#6:** `top_p` [`FLOAT64`] → 范围在`[0.0,1.0]`内的参数，决定模型如何选择输出的 token。

+   ***注意：*** 为了获得更少的随机响应，应指定较低的值。 Tokens 从最可能的（基于 `top_k` 值）到最不可能的进行选择，直到它们的概率总和等于 `top_p` 值。

在了解了函数的目的和每个参数的作用之后，我们可以开始演示如何使用 BigQuery 的 *GENERATE_TEXT* 函数。

# 使用该函数的 4 步方法

本节将介绍测试生成 AI 功能*GENERATE_TEXT*的四个方法步骤。

![](img/75f192b921bf431562d0830f63059cb9.png)

GENERATE_TEXT 函数工作流程 [图片由作者提供]

总的来说，该方法包括：

+   使用 ChatGPT 生成小型模拟数据集并将其导出到 Google Sheets 文档中。

+   在 Google Sheets 文档的基础上创建 BigQuery 数据集。

+   设置 Google 服务 Vertex AI 和 BQ 之间的连接，以便使用远程生成 AI 模型。

+   在 BigQuery 的模拟数据集上测试 *GENERATE_TEXT* 函数的实际用例示例。

每一步的更多背景信息请参见下面的子节。

## *步骤 1:* ***使用 ChatGPT 生成模拟数据集***

由于我没有现实生活中的数据集，我决定创建一个用于测试目的的模拟数据集。

为此，我使用 ChatGPT 输入了以下提示文本：

*“我需要为 50 种不同的虚构头发产品自动生成客户评价。*

*对于每个产品，我需要 2 条正面评论、2 条负面评论和 1 条中性评论。*

*此外，我希望评论至少有 4 句话，并包含不同的信息：产品名称、购买产品的商店位置和产品价格。*

*在负面评论中，请包括不同的原因，如产品质量问题、价格问题和配送问题。*

结果是一个小型数据集表，包含五个属性（下图预览）：

+   ***product_name —*** 包含虚假产品名称的值，

+   ***review_type —*** 包含评论情感类型（正面、负面或中性），

+   ***store_location —*** 包含随机的城市和州名称，

+   ***product_price —*** 包含随机的美元产品价格***,*** 和

+   ***product_review—*** 包含五句话长的虚假产品评论文本。

![](img/fa3e3249317a2d778593bff93708693a.png)

BigQuery 模拟数据集预览 [作者提供的图片]

完整数据集可以在 Git 仓库中找到 [这里](https://github.com/CassandraOfTroy/bigquery-generate_text-AI_function/blob/main/product_review_table%20-%20ChatGPT_Mockup_Reviews.csv) [4]。

在准备好模拟数据集并将其存储在 Google Sheets 后，下一步是将其转移到 BigQuery。

## *步骤* ***2:*** *在 Google Sheet 文档上创建 BQ 表*

当我有一个小型数据集，需要频繁手动调整或更改时，我的首选方法是创建一个 BigQuery 表，基于可编辑的 Google Sheet 文档。

为此，需要以下子步骤 [[2](https://supermetrics.com/blog/bigquery-query-google-sheets?utm_source=google&utm_medium=cpc&utm_campaign=G_CEU_PPC_Prod_GS&utm_adgroup=g-sheets-dsa&utm_category=search-nonbrand&utm_term=&location=&google_ads_redirect=true&hsa_acc=9495023515&hsa_cam=20418026394&hsa_grp=151612487469&hsa_ad=668219731129&hsa_src=g&hsa_tgt=dsa-1517773330948&hsa_kw=&hsa_mt=&hsa_net=adwords&hsa_ver=3&gad_source=1&gclid=CjwKCAiAmZGrBhAnEiwAo9qHiUUYA_iYfKl6S-mCF7nbb7e2Xzia1ny9M5OwHTw2CQGwai6ZL4CY9BoC1BMQAvD_BwE)]：

***子步骤 #1: 在 BigQuery 项目中创建数据集和表，通过指定文件位置和格式***

应选择 BigQuery 环境右上角的 `CREATE TABLE` 选项来创建新数据集和表。需要输入标有星号（*******) 的值，如下图所示。

![](img/a509be96d50efc83bb4e722d980288b1.png)

在 Google Sheets 中的模拟数据集上创建 BQ 数据集和表 [作者提供的图片]

从上图可以看出，新创建的 BQ 数据集名称为 `hair_shop` ，新创建的表名称为 `product_review_table`。

需要注意的重要事项：

+   如果你不想在上图所示的 `Schema` 部分定义表模式，则从 Google Sheets 导入的所有属性默认的数据类型为 `STRING`。

***子步骤 #2: 在 BQ 中查询模拟数据集***

第二个子步骤是直接在 BigQuery 中探索 `hair_shop.product_review_table` 数据集。

![](img/28426c2e311e275229f48f9e0d88ef1c.png)

在 BQ 中查询模拟数据集 [作者图片]

现在你可以访问 BQ 中的模拟数据集，是时候将生成 AI 远程模型连接到它了。

## *步骤* ***3:*** *将 Vertex AI 服务连接到 BQ*

详细说明该步骤的指南可以在 Google 的文档 [这里](https://cloud.google.com/bigquery/docs/generate-text-tutorial#create_a_connection) [3] 中找到。

总结提供的 Google 教程，将 Vertex AI 连接到 BQ 的三个主要子步骤是：

+   ***子步骤 #1:*** 从 BigQuery 创建一个云资源连接，以获取连接的服务帐户。

+   ***子步骤 #2:*** 授予连接的服务帐户适当的角色，以访问 Vertex AI 服务。

+   ***子步骤 #3:*** 创建代表托管 Vertex AI 大型语言模型的 `text-bison` 远程模型，在创建的 BigQuery 数据集 `hair_shop` 中。

一旦这些子步骤完成，新的对象 `Model` 将在 `hair_shop` 数据集中可见。

![](img/0bc518332fed9cee2cf5b52cdcfa4467.png)

选定 BQ 数据集中 `Model` 的新数据库对象的预览 [作者图片]

最后，魔法可以开始了，我们可以开始使用 *GENERATE_TEXT* 函数。

## *步骤* ***4: 使用*** `*GENERATE_TEXT*` *函数在 BQ 中*

在此步骤中，我们将关注两个用于测试函数使用的用例：***情感分析*** 和 ***实体提取***。

选择这两个用例的原因是我们已经在输入模拟数据集中创建了两个属性（`review_type` 和 `product_review`），可以用来测试函数结果。函数结果指的是验证 AI 生成的值在新属性中的准确性。

现在让我们展示生成每个用例的新属性的具体输入输出流程。

## ***情感分析示例***

情感分析的查询显示在下面的代码块中。

```py
--Generate the positive/negative sentiment
SELECT
  ml_generate_text_result['predictions'][0]['content'] AS review_sentiment,
  * EXCEPT (ml_generate_text_status,ml_generate_text_result)
FROM
  ML.GENERATE_TEXT(
    MODEL `hair_shop.llm_model`,
    (
      SELECT
        CONCAT('Extract the one word sentiment from product_review column. Possible values are positive/negative/neutral ', product_review) AS prompt,
        *
      FROM
        `macro-dreamer-393017.hair_shop.product_review_table`
      LIMIT 5
    ),
    STRUCT(
      0.1 AS temperature,
      1 AS max_output_tokens,
      0.1 AS top_p, 
      1 AS top_k));
```

查询的详细信息如下：

***外部 SELECT 语句:***

+   外部查询从 LLM 获取的新生成的字符串属性 `review_sentiment` 通过将 `ML.GENERATE_TEXT` 函数应用于 `hair_shop.llm_model` 对象来选择。

+   此外，它还选择了输入数据集 `hair_shop.product_review_table` 中的所有其他列。

***内部 SELECT 语句:***

+   内部查询语句选择了一个 `CONCAT` 函数，并对每个来自输入数据集 `hair_shop.product_review_table` 的 `product_review` 应用特定的提示指令。

+   换句话说，提示指令引导模型从 `product_review` 属性中提取一个单词的情感（积极、消极或中立）。

***模型参数的 STRUCT:***

+   `temperature: 0.1` - 较低的值 0.1 将导致更可预测的文本生成。

+   `max_output_tokens: 1` - 将模型的输出限制为 1 个标记（情感分析结果可以是积极的、消极的或中性的）。

+   `top_p: 0.1` 影响下一个标记的可能性分布。

+   `top_k: 1` 限制考虑的前几个标记的数量。

在触发所示查询后，对前五个结果进行了分析：

![](img/72c6a4277dbb9312eb9b01b097525f33.png)

GENERATE_TEXT 函数的情感分析结果 [图片由作者提供]

从图像中可以看到，新生成的属性`review_sentiment`被添加到模拟数据集中。`review_sentiment`属性的值与`review_type`属性的值进行了比较，并且它们的记录匹配。

在情感分析结果之后，目标是识别每个情感背后的原因，即进行实体提取。

## ***实体提取示例***

实体提取分析的查询在下面的代码块中显示。

```py
--Check what's the reason behind the positive/negative sentiment
SELECT
  ml_generate_text_result['predictions'][0]['content'] AS generated_text,
  * EXCEPT (ml_generate_text_status,ml_generate_text_result)
FROM
  ML.GENERATE_TEXT(
    MODEL `hair_shop.llm_model`,
    (
      SELECT
        CONCAT('What is the reason for a review of the product? Possible options are: good/bad/average product quality, delivery delay, low/high price', product_review) AS prompt,
        *
      FROM
        `macro-dreamer-393017.hair_shop.product_review_table`
      LIMIT 5
    ),
    STRUCT(
      0.2 AS temperature,
      20 AS max_output_tokens,
      0.2 AS top_p, 
      5 AS top_k));
```

查询的分解与情感分析查询的分解类似，只是提示指令和模型参数值已调整为实体提取分析。

在触发所示查询后，对前五个结果进行了分析：

![](img/3011b6722be6624291f5ed5515b21956.png)

GENERATE_TEXT 函数的实体提取分析结果 [图片由作者提供]

`review_reason`属性的结果与`product_review`属性的值进行了比较。与之前的示例一样，结果匹配。

各位，这就是了。这个教程展示了如何仅通过使用 SQL 和提示文本从非结构化文本中创建新属性的两个用例。所有这些都借助 BigQuery 中新生成的 AI 功能完成。

现在，我们可以分享这篇博客文章的简要总结。

# 结论

这篇博客文章的目标是介绍 BigQuery 中新的`GENERATE_TEXT` ML 功能的使用。

通过此功能，SQL 导向的数据专业人士现在可以从直接存储在 BQ 数据仓库表中的非结构化文本中创建新的洞察（属性）。

换句话说，该功能使数据专业人士能够直接在 BigQuery 环境中利用[NLP 机器学习](https://en.wikipedia.org/wiki/Natural_language_processing)任务。

此外，它弥合了使用 Python 开发新分析洞察的数据专业人士与那些更注重数据库的专业人士之间的知识差距。

最后，我将以一句话总结这篇博客文章：“每个人都可以使用 SQL 知识和良好的提示结构在 BigQuery 中进行编码和执行 NLP 分析。”

> 感谢阅读我的文章。请关注[Medium](https://medium.com/@martosi/subscribe)和[Linkedin](https://www.linkedin.com/in/martosi/)以获取更多故事。

# 知识参考

+   [[1](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-generate-text#:~:text=GENERATE_TEXT%20function,%20which%20lets%20you,Sentiment%20Analysis)] Google 文章《ML.GENERATE_TEXT 函数》，访问时间：2023 年 11 月 10 日，[`cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-generate-text`](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-generate-text)

+   [[2](https://supermetrics.com/blog/bigquery-query-google-sheets?utm_source=google&utm_medium=cpc&utm_campaign=G_CEU_PPC_Prod_GS&utm_adgroup=g-sheets-dsa&utm_category=search-nonbrand&utm_term=&location=&google_ads_redirect=true&hsa_acc=9495023515&hsa_cam=20418026394&hsa_grp=151612487469&hsa_ad=668219731129&hsa_src=g&hsa_tgt=dsa-1517773330948&hsa_kw=&hsa_mt=&hsa_net=adwords&hsa_ver=3&gad_source=1&gclid=CjwKCAiAmZGrBhAnEiwAo9qHiUUYA_iYfKl6S-mCF7nbb7e2Xzia1ny9M5OwHTw2CQGwai6ZL4CY9BoC1BMQAvD_BwE)] Supermetrics 文章《Google Sheets 到 BigQuery：逐步指南》，访问时间：2023 年 11 月 22 日，[`t.ly/ZZ2lN`](https://t.ly/ZZ2lN)

+   [[3](https://cloud.google.com/bigquery/docs/generate-text-tutorial#create_a_connection)] Google 教程《使用 ML.GENERATE_TEXT 函数生成文本》，章节《创建连接》，访问时间：2023 年 11 月 10 日，[`cloud.google.com/bigquery/docs/generate-text#generate_text`](https://cloud.google.com/bigquery/docs/generate-text-tutorial#create_a_connection)

+   [[4](https://github.com/CassandraOfTroy/bigquery-generate_text-AI_function/tree/main)] 作者的 Git 代码库：[`github.com/CassandraOfTroy/bigquery-generate_text-AI_function/tree/main`](https://github.com/CassandraOfTroy/bigquery-generate_text-AI_function/tree/main)
