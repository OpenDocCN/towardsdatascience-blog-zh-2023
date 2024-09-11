# 领域适应：微调预训练的 NLP 模型

> 原文：[https://towardsdatascience.com/domain-adaption-fine-tune-pre-trained-nlp-models-a06659ca6668?source=collection_archive---------1-----------------------#2023-07-04](https://towardsdatascience.com/domain-adaption-fine-tune-pre-trained-nlp-models-a06659ca6668?source=collection_archive---------1-----------------------#2023-07-04)

![](../Images/60b46d488ea28b302a949054bded404e.png)

图片由 [Pietro Jeng](https://unsplash.com/@pietrozj?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## [实用教程](https://towardsdatascience.com/tagged/hands-on-tutorials)

## 关于为任何领域微调预训练 NLP 模型的逐步指南

[](https://medium.com/@shashank.kapadia?source=post_page-----a06659ca6668--------------------------------)[![Shashank Kapadia](../Images/347e4cb92a7d27f032c5761e4526f2fa.png)](https://medium.com/@shashank.kapadia?source=post_page-----a06659ca6668--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a06659ca6668--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a06659ca6668--------------------------------) [Shashank Kapadia](https://medium.com/@shashank.kapadia?source=post_page-----a06659ca6668--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcc7314ace45c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdomain-adaption-fine-tune-pre-trained-nlp-models-a06659ca6668&user=Shashank+Kapadia&userId=cc7314ace45c&source=post_page-cc7314ace45c----a06659ca6668---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a06659ca6668--------------------------------) ·9 min read·Jul 4, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa06659ca6668&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdomain-adaption-fine-tune-pre-trained-nlp-models-a06659ca6668&user=Shashank+Kapadia&userId=cc7314ace45c&source=-----a06659ca6668---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa06659ca6668&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdomain-adaption-fine-tune-pre-trained-nlp-models-a06659ca6668&source=-----a06659ca6668---------------------bookmark_footer-----------)

*前言：本文总结了有关给定主题的信息。这不应被视为原创研究。本文中包含的信息和代码可能受到我过去阅读或看到的各种在线文章、研究论文、书籍和开源代码的影响。*

*合著者：* [*比利·海因斯*](https://medium.com/u/f6bf67256839?source=post_page-----a06659ca6668--------------------------------)

# 内容表

+   引言

+   理论框架

+   数据概览

+   起点：基础模型

+   微调模型

+   结果评估

+   结束语

# 引言

在当今世界，预训练NLP模型的可用性大大简化了使用深度学习技术解释文本数据的过程。然而，虽然这些模型在一般任务中表现优异，但它们在特定领域的适应性往往不足。本指南旨在带您了解如何微调预训练NLP模型，以在特定领域获得更好的性能。

## 动机

尽管像BERT和通用句子编码器（USE）这样的预训练NLP模型在捕捉语言细节方面很有效，但由于其训练所用数据集的多样性，它们在特定领域应用中的表现可能受到限制。这种限制在分析特定领域内的关系时尤为明显。

例如，在处理就业数据时，我们希望模型能识别出“数据科学家”和“机器学习工程师”角色之间的更紧密关系，或“Python”和“TensorFlow”之间的更强关联。不幸的是，通用模型往往会遗漏这些细微的关系。

下表演示了从基础多语言USE模型中获得的相似度差异：

![](../Images/fd70480995a62829117960a1a4fa1d88.png)

图1：基础[多语言通用句子编码器模型](https://tfhub.dev/google/universal-sentence-encoder-multilingual/3)中两个文本向量之间的相似度得分

为了解决这个问题，我们可以通过高质量的领域特定数据集微调预训练模型。这一适应过程显著提升了模型的性能和精准度，充分释放了NLP模型的潜力。

> 处理大型预训练NLP模型时，建议先部署基础模型，只有在其性能对具体问题不满足要求时才考虑微调。

本教程专注于使用易于获取的开源数据微调通用句子编码器（USE）模型。

# 理论概述

微调ML模型可以通过多种策略实现，例如监督学习和强化学习。在本教程中，我们将集中于结合了孪生网络架构的一次（少次）学习方法。

## 方法论

在本教程中，我们使用了一种 Siamese 神经网络，这是一种特定类型的人工神经网络。该网络利用共享权重，同时处理两个不同的输入向量，以计算可比较的输出向量。受一次性学习的启发，这种方法在捕捉语义相似性方面特别有效，尽管可能需要较长的训练时间，并且缺乏概率输出。

Siamese 神经网络创建了一个‘嵌入空间’，在该空间中，相关的概念被紧密地放置，使模型能够更好地辨别语义关系。

![](../Images/e04c290c089ac82b1e99511c2e8c6df0.png)

图 2\. Siamese 架构用于微调预训练 NLP 模型

+   **双分支与共享权重**：该架构由两个相同的分支组成，每个分支包含一个具有共享权重的嵌入层。这两个分支同时处理两个输入，无论是相似的还是不相似的。

+   **相似性与转换**：输入通过预训练的 NLP 模型转换为向量嵌入。然后，架构计算这些向量之间的相似性。相似性分数在 -1 和 1 之间，量化两个向量之间的角距离，作为它们语义相似性的度量。

+   **对比损失与学习**：模型的学习由“对比损失”引导，该损失是期望输出（来自训练数据的相似性分数）与计算出的相似性之间的差异。这个损失指导模型权重的调整，以最小化损失并提高学习嵌入的质量。

要了解更多关于一次（少量）学习、Siamese 架构和对比损失的信息，请参阅以下资源：

[](https://www.projectpro.io/article/siamese-neural-networks/718?source=post_page-----a06659ca6668--------------------------------) [## Siamese 神经网络架构简介

### “Siamese 神经网络架构：概述和关键概念解释与示例 | ProjectPro

[www.projectpro.io](https://www.projectpro.io/article/siamese-neural-networks/718?source=post_page-----a06659ca6668--------------------------------) [## 什么是一次性学习？ — TechTalks

### 一次性学习允许深度学习算法测量两个图像之间的相似性和差异。

[对比损失解释](https://bdtechtalks.com/2020/08/12/what-is-one-shot-learning/?source=post_page-----a06659ca6668--------------------------------) [## 对比损失解释

### 对比损失最近在许多论文中得到应用，展示了最先进的无监督结果…

[towardsdatascience.com](/contrastive-loss-explaned-159f2d4a87ec?source=post_page-----a06659ca6668--------------------------------)

完整代码可以在 [GitHub上的Jupyter Notebook](https://github.com/kapadias/medium-articles/blob/master/natural-language-processing/embedding-models/domain_adaption_fine_tune_nlp_model.ipynb) 中找到

# 数据概述

对于使用这种方法对预训练 NLP 模型进行微调，训练数据应包含文本字符串对及其间的相似度评分。

训练数据遵循以下格式：

![](../Images/002a9075851dd4c80026dc760b2c3255.png)

图3\. 训练数据的示例格式

在本教程中，我们使用来自 [ESCO 分类数据集](https://esco.ec.europa.eu/en) 的数据集，这些数据集已被转换为根据不同数据元素之间的关系生成相似度评分。

> 准备训练数据是微调过程中的关键步骤。假设你可以访问所需的数据并有方法将其转换成指定格式。由于本文重点展示微调过程，我们将省略如何使用 ESCO 数据集生成数据的细节。
> 
> ESCO 数据集可供开发者自由使用，作为提供自动补全、建议系统、职位搜索算法和职位匹配算法等服务的各种应用的基础。本教程使用的数据集已被转换并作为示例提供，允许无限制地用于任何目的。

让我们开始检查训练数据：

```py
import pandas as pd

# Read the CSV file into a pandas DataFrame
data = pd.read_csv("./data/training_data.csv")

# Print head
data.head()
```

![](../Images/f63c7e896d9f23fa4d83168d3cf29e1f.png)

图4\. 用于微调模型的示例数据

# 起点：基线模型

首先，我们建立 [多语言通用句子编码器](https://tfhub.dev/google/universal-sentence-encoder-multilingual/3) 作为我们的基线模型。在进行微调过程之前，设定这个基线模型是至关重要的。

在本教程中，我们将使用 STS 基准和示例相似度可视化作为度量指标，以评估微调过程中的变化和改进。

> STS Benchmark 数据集由英文句子对组成，每对句子都关联有一个相似度评分。在模型训练过程中，我们会在这个基准集上评估模型的性能。每次训练的持久化评分是预测相似度评分与数据集中实际相似度评分之间的皮尔逊相关系数。
> 
> 这些评分确保当模型使用我们特定上下文的训练数据进行微调时，它仍然保持一定程度的泛化能力。

```py
# Loads the Universal Sentence Encoder Multilingual module from TensorFlow Hub.
base_model_url = "https://tfhub.dev/google/universal-sentence-encoder-multilingual/3"
base_model = tf.keras.Sequential([
    hub.KerasLayer(base_model_url,
                   input_shape=[],
                   dtype=tf.string,
                   trainable=False)
])

# Defines a list of test sentences. These sentences represent various job titles.
test_text = ['Data Scientist', 'Data Analyst', 'Data Engineer',
             'Nurse Practitioner', 'Registered Nurse', 'Medical Assistant',
             'Social Media Manager', 'Marketing Strategist', 'Product Marketing Manager']

# Creates embeddings for the sentences in the test_text list. 
# The np.array() function is used to convert the result into a numpy array.
# The .tolist() function is used to convert the numpy array into a list, which might be easier to work with.
vectors = np.array(base_model.predict(test_text)).tolist()

# Calls the plot_similarity function to create a similarity plot.
plot_similarity(test_text, vectors, 90, "base model")

# Computes STS benchmark score for the base model
pearsonr = sts_benchmark(base_model)
print("STS Benachmark: " + str(pearsonr))
```

![](../Images/ddf3d02dfb50c6ebf6c98aba0e9a055a.png)

图5\. 测试词汇的相似度可视化

> STS Benchmark (dev)：0.8325

# 微调模型

下一步涉及使用基线模型构建孪生网络模型架构，并用我们特定领域的数据对其进行微调。

```py
# Load the pre-trained word embedding model
embedding_layer = hub.load(base_model_url)

# Create a Keras layer from the loaded embedding model
shared_embedding_layer = hub.KerasLayer(embedding_layer, trainable=True)

# Define the inputs to the model
left_input = keras.Input(shape=(), dtype=tf.string)
right_input = keras.Input(shape=(), dtype=tf.string)

# Pass the inputs through the shared embedding layer
embedding_left_output = shared_embedding_layer(left_input)
embedding_right_output = shared_embedding_layer(right_input)

# Compute the cosine similarity between the embedding vectors
cosine_similarity = tf.keras.layers.Dot(axes=-1, normalize=True)(
    [embedding_left_output, embedding_right_output]
)

# Convert the cosine similarity to angular distance
pi = tf.constant(math.pi, dtype=tf.float32)
clip_cosine_similarities = tf.clip_by_value(
    cosine_similarity, -0.99999, 0.99999
)
acos_distance = 1.0 - (tf.acos(clip_cosine_similarities) / pi)

# Package the model
encoder = tf.keras.Model([left_input, right_input], acos_distance)

# Compile the model
encoder.compile(
    optimizer=tf.keras.optimizers.Adam(
        learning_rate=0.00001,
        beta_1=0.9,
        beta_2=0.9999,
        epsilon=0.0000001,
        amsgrad=False,
        clipnorm=1.0,
        name="Adam",
    ),
    loss=tf.keras.losses.MeanSquaredError(
        reduction=keras.losses.Reduction.AUTO, name="mean_squared_error"
    ),
    metrics=[
        tf.keras.metrics.MeanAbsoluteError(),
        tf.keras.metrics.MeanAbsolutePercentageError(),
    ],
)

# Print the model summary
encoder.summary()
```

![](../Images/efd1cd25e0ddc3ef8fe842e8185ea96d.png)

图6\. 微调的模型架构

## 适应模型

```py
# Define early stopping callback
early_stop = keras.callbacks.EarlyStopping(
    monitor="loss", patience=3, min_delta=0.001
)

# Define TensorBoard callback
logdir = os.path.join(".", "logs/fit/" + datetime.now().strftime("%Y%m%d-%H%M%S"))
tensorboard_callback = keras.callbacks.TensorBoard(log_dir=logdir)

# Model Input
left_inputs, right_inputs, similarity = process_model_input(data)

# Train the encoder model
history = encoder.fit(
    [left_inputs, right_inputs],
    similarity,
    batch_size=8,
    epochs=20,
    validation_split=0.2,
    callbacks=[early_stop, tensorboard_callback],
)

# Define model input
inputs = keras.Input(shape=[], dtype=tf.string)

# Pass the input through the embedding layer
embedding = hub.KerasLayer(embedding_layer)(inputs)

# Create the tuned model
tuned_model = keras.Model(inputs=inputs, outputs=embedding)
```

# 评估

现在我们已经有了微调后的模型，让我们重新评估它，并将结果与基础模型进行比较。

```py
# Creates embeddings for the sentences in the test_text list. 
# The np.array() function is used to convert the result into a numpy array.
# The .tolist() function is used to convert the numpy array into a list, which might be easier to work with.
vectors = np.array(tuned_model.predict(test_text)).tolist()

# Calls the plot_similarity function to create a similarity plot.
plot_similarity(test_text, vectors, 90, "tuned model")

# Computes STS benchmark score for the tuned model
pearsonr = sts_benchmark(tuned_model)
print("STS Benachmark: " + str(pearsonr))
```

![](../Images/c882c7db246a72c1d21308cd8dc462a9.png)

> STS 基准（开发集）：0.8349

基于在相对较小的数据集上对模型进行微调，STS基准得分与基线模型相当，这表明调优后的模型仍具有一定的泛化能力。然而，相似性可视化展示了相似标题之间的相似性得分得到了增强，而不相似标题的得分则减少了。

# 结束语

对预训练自然语言处理（NLP）模型进行领域适配的微调是一种强大的技术，可以提高其在特定上下文中的表现和精度。通过利用高质量的领域特定数据集和利用孪生神经网络，我们可以增强模型捕捉语义相似性的能力。

本教程提供了微调过程的逐步指南，以Universal Sentence Encoder（USE）模型为例。我们探讨了理论框架、数据准备、基线模型评估以及实际的微调过程。结果展示了在特定领域内微调的有效性，增强了相似性得分。

通过遵循这种方法并将其适应到你的特定领域，你可以充分发挥预训练NLP模型的潜力，并在自然语言处理任务中取得更好的结果。

感谢阅读。*如果你有任何反馈，请随时通过评论这篇文章、在* [*LinkedIn*](https://www.linkedin.com/in/shashankkapadia/) *上给我发消息，或者发邮件到（smhkapadia[at]gmail.com）与我联系*

*如果你喜欢这篇文章，请访问我的其他文章*

[evaluate-topic-model-in-python-latent-dirichlet-allocation-lda-7d57484bb5d0?source=post_page-----a06659ca6668--------------------------------](https://towardsdatascience.com/evaluate-topic-model-in-python-latent-dirichlet-allocation-lda-7d57484bb5d0?source=post_page-----a06659ca6668--------------------------------) [## 评估主题模型：潜在狄利克雷分配（LDA）

### 构建可解释的主题模型的逐步指南

[evaluate-topic-model-in-python-latent-dirichlet-allocation-lda-7d57484bb5d0?source=post_page-----a06659ca6668--------------------------------](https://towardsdatascience.com/evaluate-topic-model-in-python-latent-dirichlet-allocation-lda-7d57484bb5d0?source=post_page-----a06659ca6668--------------------------------) [](https://medium.com/aimonks/the-evolution-of-natural-language-processing-56ce27916e10?source=post_page-----a06659ca6668--------------------------------) [## 自然语言处理的演变

### 语言模型发展的历史视角

[medium.com](https://medium.com/aimonks/the-evolution-of-natural-language-processing-56ce27916e10?source=post_page-----a06659ca6668--------------------------------) [](/recommendation-system-in-python-lightfm-61c85010ce17?source=post_page-----a06659ca6668--------------------------------) [## Python中的推荐系统：LightFM

### 在Python中使用LightFM构建推荐系统的逐步指南

[towardsdatascience.com](/recommendation-system-in-python-lightfm-61c85010ce17?source=post_page-----a06659ca6668--------------------------------)
