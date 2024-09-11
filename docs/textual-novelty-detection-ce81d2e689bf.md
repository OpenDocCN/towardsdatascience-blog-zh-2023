# 文本新颖性检测

> 原文：[https://towardsdatascience.com/textual-novelty-detection-ce81d2e689bf?source=collection_archive---------6-----------------------#2023-10-02](https://towardsdatascience.com/textual-novelty-detection-ce81d2e689bf?source=collection_archive---------6-----------------------#2023-10-02)

## 如何使用最小协方差判别法（MCD）检测新颖的新闻头条

[](https://medium.com/@ilia.teimouri?source=post_page-----ce81d2e689bf--------------------------------)[![Ilia Teimouri PhD](../Images/0eb948c4d3f81c116cd16fa4d5016629.png)](https://medium.com/@ilia.teimouri?source=post_page-----ce81d2e689bf--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ce81d2e689bf--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ce81d2e689bf--------------------------------) [Ilia Teimouri PhD](https://medium.com/@ilia.teimouri?source=post_page-----ce81d2e689bf--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbf9b9036159&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftextual-novelty-detection-ce81d2e689bf&user=Ilia+Teimouri+PhD&userId=bf9b9036159&source=post_page-bf9b9036159----ce81d2e689bf---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ce81d2e689bf--------------------------------) · 8分钟阅读 · 2023年10月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fce81d2e689bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftextual-novelty-detection-ce81d2e689bf&user=Ilia+Teimouri+PhD&userId=bf9b9036159&source=-----ce81d2e689bf---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fce81d2e689bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftextual-novelty-detection-ce81d2e689bf&source=-----ce81d2e689bf---------------------bookmark_footer-----------)![](../Images/3a9fb3a16537768e9f8142215fd50175.png)

图片由 [Ali Shah Lakhani](https://unsplash.com/@alishahlakhani) 提供，发布于 [Unsplash](https://unsplash.com/)。

在今天的信息时代，我们每天都被新闻文章淹没。这些文章中的许多只是对相同事实的重复陈述，但也有一些包含真正的新信息，这些信息可能会对我们的决策产生重大影响。例如，想要投资Meta的人可能希望关注那些包含独家信息的文章，而不是那些仅仅重复先前发布数据的文章。能够区分新颖的新闻和冗余的新闻至关重要，这样我们才能在面对信息洪流时做出明智的决策。

这就是[新颖性检测](https://en.wikipedia.org/wiki/Novelty_detection)发挥作用的地方。新颖性检测是识别新数据或未知数据的任务，这些数据与以前见过的数据有所不同。这是一种无监督学习技术，用于检测数据中的异常、离群值或新模式。关键思想是建立一个“正常”数据的模型，然后利用该模型识别偏离正常的数据点。

在新闻文章的背景下，这涉及到检测文章是否包含在其他地方不可获得的新信息。为此，我们可以开发一个已知或可用的信息基线，然后将新信息与该基线进行比较。如果新信息与基线之间存在显著差异，则可以认为该信息是新颖的。

## 最小协方差行列式（MCD）

最小协方差行列式（MCD）方法是一种估计数据集协方差矩阵的技术。它可以用来创建一个包围高斯分布中心模式的椭圆形，任何位于该形状外的数据点都可以被视为新颖性（有时称为异常值）。MCD方法对于噪声大或含有离群值的数据集特别有用，因为它可以帮助识别那些可能不符合整体数据模式的异常数据点。（[见示例](https://wis.kuleuven.be/stat/robust/papers/2010/wire-mcd.pdf)）

MCD可以用来检测新闻头条中的新颖性。虽然该方法可以推广到完整文章中，我们的目标是提供一个简明的示例，展示如何在短文本中应用MCD进行新颖性检测。MCD是一个多变量位置和散布的鲁棒估计量，使其非常适合在高维数据（如文本）中识别离群值。在新闻头条的数据集上，MCD将基于协方差学习一个“正常”头条的模型。然后我们可以使用该模型来评分新头条，并标记那些显著偏离正常的头条，作为潜在的新颖或异常故事。示例代码和实验将说明MCD新颖性检测在实践中的运作方式。

## 步骤方法

**嵌入：** 在机器学习中，我们使用[嵌入](https://www.youtube.com/watch?v=4-QoMdSqG_I)作为一种更紧凑和高效的方式来表示数据。嵌入将原始数据转换为捕捉数据最重要特征的低维表示。

文本嵌入是一种特定类型的嵌入，用于将文本数据转换为向量表示。它考虑了单词、短语和句子之间的语义和关系，并将它们转换为捕捉文本含义的数值表示。这使我们能够执行诸如查找相似文本、基于语义意义对文本进行聚类等操作。

假设我们收集了过去几个月有关Meta的以下头条新闻：

```py
news = [
    "Mark Zuckerberg touts potential of remote work in metaverse as Meta threatens employees for violating return-to-office mandate",
    "Meta Quest 3 Shows Us the Metaverse Dream isn’t Dead Yet",
    "Meta has Apple to thank for giving its annual VR conference added sizzle this year",
    "Meta launches AI chatbots for Instagram, Facebook and WhatsApp",
    "Meta Launches AI Chatbots for Snoop Dogg, MrBeast, Tom Brady, Kendall Jenner, Charli D’Amelio and More",
    "Llama 2: why is Meta releasing open-source AI model and are there any risks?",
    "Meta's Mandatory Return to Office Is 'a Mess'",
    "Meta shares soar on resilient revenue and $40bn in buybacks",
    "Facebook suffers fresh setback after EU ruling on use of personal data",
    "Facebook owner Meta hit with record €1.2bn fine over EU-US data transfers"
]
```

我们可以使用 OpenAI 生成每个句子的文本嵌入，方法如下：

```py
def get_embedding(text, 
                 model = 'text-embedding-ada-002'):
    text = text.replace("\n", " ")
    return openai.Embedding.create(input = [text], engine = model)['data'][0]['embedding']

df['embedding'] = df.news.apply(lambda x: get_embedding(x))
df['embedding'] = df['embedding'].apply(np.array)

matrix = np.vstack(df['embedding'].values)
matrix.shape

# Output: (10, 1536)
```

来自 [OpenAI 的 `text-embedding-ada-002` 模型](https://openai.com/blog/new-and-improved-embedding-model) 是一个前沿的嵌入模型，它接受一个句子作为输入，并输出长度为 1536 的嵌入向量。该向量表示输入句子的语义含义，可以用于语义相似性、文本分类等任务。该模型的最新版本采用了最先进的语言表示技术，以生成高度准确和鲁棒的嵌入。如果你无法访问 OpenAI，你可以使用其他嵌入模型，如 [Sentence Transformers](https://www.sbert.net)。

一旦生成了嵌入，我们会创建一个矩阵变量，用于存储来自 `df[‘embedding’]` 列的嵌入矩阵表示。这是通过使用 `NumPy` 库中的 `vstack` 函数完成的，该函数将列中的所有向量（每个表示一个句子）垂直堆叠以创建一个矩阵。这使我们能够在下一步中使用矩阵运算。

**计算 MCD：** 我们使用嵌入作为特征，并计算 MCD 以估计中心数据云（多变量高斯分布的中心模式）的位置和形状。

**拟合椭圆包络：** 然后，我们使用计算得到的 MCD 拟合一个椭圆包络到中心模式。这个包络作为边界，将正常点与新颖点分开。

**预测新颖句子：** 最后，我们使用椭圆包络对嵌入进行分类。位于包络内部的点被认为是正常的，而位于包络外部的点被认为是新颖的或异常的。

为了完成这一切，我们使用 Python 中的 `scikit-learn` 库中的 `EllipticEnvelope` 类来应用 MCD：

```py
# Reduce the dimensionality of the embeddings to 2D using PCA
pca = PCA(n_components=2)
reduced_matrix = pca.fit_transform(matrix)
reduced_matrix.shape

# Fit the Elliptic Envelope (MCD-based robust estimator)
envelope = EllipticEnvelope(contamination=0.2)  
envelope.fit(reduced_matrix)

# Predict the labels of the sentences
labels = envelope.predict(reduced_matrix)

# Find the indices of the novel sentences
novel_indices = np.where(labels == -1)[0]
novel_indices

#Output: array([8, 9])
```

`contamination` 是一个参数，你可以根据期望的新颖句子数量进行调整。它表示数据集中异常值的比例。`predict` 方法返回一个标签数组，其中 `1` 表示正常点（内点），而 `-1` 表示异常点（新颖点）。

此外，为了将高维嵌入可视化为 2D 以及节省计算时间，我们使用 [PCA](https://en.wikipedia.org/wiki/Principal_component_analysis) 将高维嵌入向量投影到较低维度的 2D 空间，我们将其称为 `reduced_matrix`。

我们可以看到 `novel_indices` 输出了 `array([8, 9])`，这是被认为是新颖的句子索引。

**绘制结果：** 我们可以通过绘制嵌入和椭圆包络来可视化结果。正常点（内点）可以用一种颜色或标记绘制，而异常点（新颖点）可以用另一种颜色或标记绘制。椭圆包络可以通过绘制与马氏距离对应的椭圆来可视化。

为了实现可视化，我们：

1.  提取拟合的椭圆包络模型的位置和协方差矩阵。

1.  计算协方差矩阵的特征值和特征向量，以确定椭圆的方向和轴长。

1.  计算每个样本到拟合椭圆模型中心的马氏距离。

1.  根据污染参数确定一个阈值距离，该参数指定了预期的异常值百分比。

1.  根据阈值马氏距离缩放椭圆的宽度和高度。

1.  将椭圆内部的点标记为内点，外部的点标记为外点。

1.  绘制内点和外点，并添加缩放的椭圆补丁。

1.  用索引标注每个数据点以识别异常值。

```py
# Extract the location and covariance of the central mode
location = envelope.location_
covariance = envelope.covariance_

# Compute the angle, width, and height of the ellipse
eigenvalues, eigenvectors = np.linalg.eigh(covariance)
order = eigenvalues.argsort()[::-1]
eigenvalues, eigenvectors = eigenvalues[order], eigenvectors[:, order]
vx, vy = eigenvectors[:, 0]
theta = np.arctan2(vy, vx)

# Compute the width and height of the ellipse based on the eigenvalues (variances)
width, height = 2 * np.sqrt(eigenvalues)

# Compute the Mahalanobis distance of the reduced 2D embeddings
mahalanobis_distances = envelope.mahalanobis(reduced_matrix)

# Compute the threshold based on the contamination parameter
threshold = np.percentile(mahalanobis_distances, (1 - envelope.contamination) * 100)

# Scale the width and height of the ellipse based on the Mahalanobis distance threshold
width, height = width * np.sqrt(threshold), height * np.sqrt(threshold)

# Plot the inliers and outliers
inliers = reduced_matrix[labels == 1]
outliers = reduced_matrix[labels == -1]

# Re-plot the inliers and outliers along with the elliptic envelope with annotations
plt.scatter(inliers[:, 0], inliers[:, 1], c='b', label='Inliers')
plt.scatter(outliers[:, 0], outliers[:, 1], c='r', label='Outliers', marker='x')
ellipse = Ellipse(location, width, height, angle=np.degrees(theta), edgecolor='k', facecolor='none')
plt.gca().add_patch(ellipse)

# Annotate each point with its index
for i, (x, y) in enumerate(reduced_matrix):
    plt.annotate(str(i), (x, y), textcoords="offset points", xytext=(0, 5), ha='center')

plt.title('Novelty Detection using MCD with Annotations')
plt.xlabel('Feature 1')
plt.ylabel('Feature 2')
plt.legend()
plt.grid(True)
plt.show()
```

最后，我们得到内点和外点的可视化效果如下：

![](../Images/5bc2982a701f693fba2c5f3f4b40cebb.png)

绘制包络线以及标记为内点或外点的点

现在让我们查看标题，第8条和第9条是：

> Facebook在欧盟关于个人数据使用的裁决后遭遇新挫折。
> 
> Facebook母公司Meta因欧盟-美国数据传输问题被罚创纪录的12亿欧元。

这两个标题都与欧盟调节Meta在其平台上如何使用和传输个人数据的努力有关。

而内点标题则主要讨论Meta如何全力投入人工智能和虚拟现实。人工智能的重点在于新发布的AI聊天机器人，而虚拟现实的重点在于新发布的Meta Quest 3头戴设备。你还可以注意到第0条和第6条标题涉及远程办公设置，因此它们在图中的位置较为接近。

## 总结

在这篇文章中，我们展示了如何根据分布区分正常点和新颖点。简而言之，正常点是指位于数据分布的高密度区域的点，即它们在特征空间中接近大多数其他点。而新颖点则是位于数据分布的低密度区域的点，即它们在特征空间中远离大多数其他点。

在MCD和椭圆包络的背景下，正常点是指位于椭圆包络内部的点，椭圆包络是拟合于数据分布的中央模式的。而新颖点则位于椭圆包络外部。

我们还了解到有一些参数影响MCD的结果，这些参数包括：

+   **阈值：** 决策边界或阈值在确定一个点是正常还是新颖方面至关重要。例如，在椭圆包络方法中，位于包络线内部的点被认为是正常的，而那些在包络线外部的点被认为是新颖的。

+   **污染参数：** 这个参数通常用于新颖性检测方法中，定义了预期为新颖或被污染的数据比例。它影响包络线或阈值的紧密程度，从而影响一个点是否被分类为正常或新颖。

我们还应该注意，对于新的文章，由于每篇新闻文章来自不同的周，**新颖性检测方法**应考虑新闻的时间因素。如果该方法本身没有考虑时间顺序，你可能需要手动纳入这一方面，例如通过考虑话题或情感的变化，这将超出本帖的范围。
