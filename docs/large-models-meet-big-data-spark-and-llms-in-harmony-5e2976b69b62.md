# 大型模型遇见大数据：Spark 和 LLMs 的和谐

> 原文：[`towardsdatascience.com/large-models-meet-big-data-spark-and-llms-in-harmony-5e2976b69b62`](https://towardsdatascience.com/large-models-meet-big-data-spark-and-llms-in-harmony-5e2976b69b62)

## 数据工程与生成式 AI

## *使用 Apache Spark 和大型语言模型的逐步指南*

[](https://tamimi-naser.medium.com/?source=post_page-----5e2976b69b62--------------------------------)![Naser Tamimi](https://tamimi-naser.medium.com/?source=post_page-----5e2976b69b62--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5e2976b69b62--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5e2976b69b62--------------------------------) [Naser Tamimi](https://tamimi-naser.medium.com/?source=post_page-----5e2976b69b62--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5e2976b69b62--------------------------------) ·6 分钟阅读·2023 年 12 月 5 日

--

![](img/4b2f8379a79a83aae121976d9e91f2c7.png)

该图像由 Midjourney 生成。

生成式 AI，包括大型语言模型（LLMs），正在革新人类生活的各个方面。在过去五年里，生成式 AI 从一个研究项目发展成为许多人日常生活中的实际应用。作为一名对生成式 AI 感兴趣的数据工程师，我一直在问自己，这项技术为我的工作和数据工程应用带来了什么？对于工程师来说，生成式 AI 和 LLMs 的一些常见应用包括自动编码、协助文档编写等等。但是，在这里，我正在评估生成式 AI 和 LLMs 在数据工程中的一些更专业的使用。如果你对这个话题感兴趣，请阅读这篇文章，并关注我在 [Medium](https://tamimi-naser.medium.com/) 和 [Linkedin](https://www.linkedin.com/in/nasertamimi/) 上的其他文章，以获取更多关于其他用例的内容。

# LLMs：强大的变革工具

数据工程师喜欢结构化和抽象化数据，这已经不是什么新鲜事了。但是，世界上充满了需要数据工程师关注的非结构化和杂乱的数据。对非结构化数据的转换总是很复杂，有时用传统工具是无法完成的。历史上，其中一种具有挑战性的非结构化数据是文本（例如评论、评价、对话）。对文本的简单转换并不难，但复杂的转换可以从文本中提取更多信息，我们可以生成更丰富的数据集。

复杂文本转换的例子可能包括从文本中提取姓名和对象、对评论或意见进行情感分析、在存储的文本中掩盖重要信息（例如私人数据、用户数据）、从一种语言翻译到标准语言、文本摘要等。好消息是现在的 LLM 可以完成所有这些转换。因此，我相信在数据工程中，LLM 的应用之一就是作为复杂数据（如文本）的转换函数。

在这篇文章中，我将通过 Apache Spark 展示 LLM 的这一能力，Apache Spark 是一个强大的分布式数据处理系统。更具体地说，我将使用 Hugging Face 的一个小型 LLM（t5-small）作为 Apache Spark UDF 函数，并对一个示例数据集应用特定的转换（情感分析）。

# 设置项目

首先，我们需要一个 Apache Spark 系统。你可以在本地系统上安装 Apache Spark，也可以使用 AWS EMR 等服务。我使用了 AWS EMR 并设置了一个小型集群进行测试。有很多关于如何在本地机器上安装 Apache Spark 或使用 AWS EMR 的文章，你可以参考它们。在这里，我假设你已经在系统上安装了 Apache Spark。

我还为这个项目选择了 Python 3.8。我们将使用 Hugging Face 库，它们与 Python 的兼容性很好。如果你使用 AWS EMR，你的系统上应该已经有 Python 3.8。否则，你需要安装 Python 3.8。

现在你已经在系统上安装了 Apache Spark 和 Python 3，接下来是安装测试此项目所需的库。运行以下 pip 命令来安装库。我们在这里安装 PySpark 以运行 Spark 任务，以及来自 Hugging Face 的 Transformers 库（这使我们能够使用包括 LLM 在内的数百种模型）。

```py
pip3 install torch==1.13.1
pip3 install transformers==4.30.2
pip3 install pyspark==3.4.2
pip3 install urllib3==1.26.6
```

# 编码时间

首先创建一个新的 Python 文件。我将其命名为 spark_llm_test.py。首先，我们需要导入库并创建一个新的 Spark 会话。

```py
from pyspark.sql import SparkSession, Row
from pyspark.sql.functions import udf
from pyspark.sql.types import StructType, StructField, StringType, LongType
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM

# Create a Spark session
spark = SparkSession.builder.appName("FlanT5Seq2SeqExample").getOrCreate()
```

对于这个测试，我创建了一个仅有两列的虚拟表。第一列是索引，第二列是随机句子。我为这个项目设定的目标是对第二列进行情感分析。显然，在实际使用情况下，你可能会从一个包含更复杂数据的表（例如 Hive 表）中读取 Spark 数据帧。

```py
# Create an Example Spark DataFrame
schema = StructType([
  StructField("id", LongType(), nullable=False),
  StructField("sentence", StringType(), nullable=False)
])

data = [
  Row(1, "It is a good test for Spark."),
  Row(2, "Spark DataFrames are powerful."),
  Row(3, "LLMs could be very slow."),
  Row(4, "It is a naive statement.")
]

input_df = spark.createDataFrame(data, schema=schema)
```

为此，我使用了“Flan T5”模型，该模型经过针对各种任务的微调，包括情感分析。如你所见，使用 Hugging Face Transformers 库，模型和分词器的设置非常简单。

```py
# Loading Flan T5 Model and Tokenizer
model = AutoModelForSeq2SeqLM.from_pretrained("google/flan-t5-small")
tokenizer = AutoTokenizer.from_pretrained("google/flan-t5-small")
```

现在是定义我们的 Spark UDF 函数的时候了。Spark 将在每一行数据上调用这个函数，以按照我们在函数中指定的方式转换数据。

关于这个 UDF 函数，有一点很重要。正如你所见，我们没有在 UDF 函数内部实例化模型和分词器。原因是定义模型和分词器（即使是这样一个小的 LLM 模型）需要较大的处理开销。这意味着每次调用这个 UDF 函数时，模型和分词器都需要在工作节点上加载，然后进行转换（这里是推断）。这无疑会显著减慢我们的处理速度。为避免这种情况，我们在 UDF 函数外部加载了模型和分词器。

```py
# Defining the Spark UDF
def t5_seq2seq_udf(input_text):
  prompt = f"sentiment of the text: {input_text}"
  input = tokenizer(prompt, return_tensors="pt")
  output = model.generate(**input)
  output_text = tokenizer.decode(output[0], skip_special_tokens=True)
  return output_text
```

最后，我们需要注册 UDF 函数并创建一个新列以保存情感分析结果。

```py
t5_udf = udf(t5_seq2seq_udf, returnType=StringType())

results_df = input_df.withColumn('output_column', t5_udf(input_df['sentence']))

results_df.show(truncate=False)
```

下面，你可以找到完整的代码。

```py
from pyspark.sql import SparkSession, Row
from pyspark.sql.functions import udf
from pyspark.sql.types import StructType, StructField, StringType, LongType
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM

# Create a Spark session
spark = SparkSession.builder.appName("T5Seq2SeqExample").getOrCreate()

# Create an Example Spark DataFrame
schema = StructType([
  StructField("id", LongType(), nullable=False),
  StructField("sentence", StringType(), nullable=False)
])

data = [
  Row(1, "It is a good test for Spark."),
  Row(2, "Spark DataFrames are powerful."),
  Row(3, "LLMs could be very slow."),
  Row(4, "It is a naive statement.")
]

input_df = spark.createDataFrame(data, schema=schema)

# Loading t5 Model and Tokenizer
model = AutoModelForSeq2SeqLM.from_pretrained("google/flan-t5-small")
tokenizer = AutoTokenizer.from_pretrained("google/flan-t5-small")

# Defining the Spark UDF
def t5_seq2seq_udf(input_text):
  prompt = f"sentiment of the text: {input_text}"
  input = tokenizer(prompt, return_tensors="pt")
  output = model.generate(**input)
  output_text = tokenizer.decode(output[0], skip_special_tokens=True)
  return output_text

t5_udf = udf(t5_seq2seq_udf, returnType=StringType())

results_df = input_df.withColumn('output_column', t5_udf(input_df['sentence']))

results_df.show(truncate=False)
```

如果你使用 spark-submit 命令运行代码，你将得到以下结果。正如你所见，Flan T5 模型在识别正面句子与负面句子方面表现良好。

```py
+---+------------------------------+-------------+                              
|id |sentence                      |output_column|
+---+------------------------------+-------------+
|1  |It is a good test for Spark.  |positive     |
|2  |Spark DataFrames are powerful.|positive     |
|3  |LLMs could be very slow.      |negative     |
|4  |It is a naive statement.      |negative     |
+---+------------------------------+-------------+
```

# Spark 和 LLMs 的未来

恭喜！你已成功将 Flan T5 作为 Spark 作业执行。为了简化，我们省略了一些细节，包括主节点和工作节点如何传输模型权重以进行推断。这个话题可能会在未来的讨论中涉及，即 Spark 是否可以高效地用于使用 LLMs 进行推断。

此外，我们在这里使用了 Spark 进行数据转换（一个应用）。但如果 Spark 可以在模型开发过程中高效使用，那将是 Apache Spark 的另一个重大胜利。

最后，这只是一个简单的批处理示例。Spark 和 LLMs 的另一个应用可能是流处理，并对这些数据流提供实时分析。毫无疑问，这是使用 Spark 和 LLMs 的良好开端，但这两个工具之间的应用和协作是无穷无尽的。
