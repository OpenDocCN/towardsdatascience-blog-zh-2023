# CLIP：无需数据即可创建图像分类器

> 原文：[https://towardsdatascience.com/clip-creating-image-classifiers-without-data-b21c72b741fa?source=collection_archive---------1-----------------------#2023-02-22](https://towardsdatascience.com/clip-creating-image-classifiers-without-data-b21c72b741fa?source=collection_archive---------1-----------------------#2023-02-22)

## 这是一个实践教程，解释如何使用预训练的 CLIP 模型生成自定义的 Zero-Shot 图像分类器，而无需进行训练。完整代码包含在内。

[](https://medium.com/@lihigurarie?source=post_page-----b21c72b741fa--------------------------------)[![Lihi Gur Arie, 博士](../Images/7a1eb30725a95159401c3672fa5f43ab.png)](https://medium.com/@lihigurarie?source=post_page-----b21c72b741fa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b21c72b741fa--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b21c72b741fa--------------------------------) [Lihi Gur Arie, 博士](https://medium.com/@lihigurarie?source=post_page-----b21c72b741fa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F418175cbf131&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclip-creating-image-classifiers-without-data-b21c72b741fa&user=Lihi+Gur+Arie%2C+PhD&userId=418175cbf131&source=post_page-418175cbf131----b21c72b741fa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b21c72b741fa--------------------------------) ·7 分钟阅读·2023年2月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb21c72b741fa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclip-creating-image-classifiers-without-data-b21c72b741fa&user=Lihi+Gur+Arie%2C+PhD&userId=418175cbf131&source=-----b21c72b741fa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb21c72b741fa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclip-creating-image-classifiers-without-data-b21c72b741fa&source=-----b21c72b741fa---------------------bookmark_footer-----------)![](../Images/0f4fc943b125ed58be2a7564ff5da20c.png)

图像由作者使用 Midjourney 生成

# **介绍**

想象一下你需要分类判断人们是否戴眼镜，但你没有数据或资源来训练自定义模型。在本教程中，你将学习如何使用预训练的CLIP模型创建一个自定义分类器，无需任何训练。这个方法被称为**零样本**图像分类，它使得对原CLIP模型训练过程中没有明确见过的类别图像进行分类成为可能。下面提供了一个易于使用的Jupyter笔记本，包含完整代码，供你方便使用。

# CLIP: 理论背景

CLIP（对比语言-图像预训练）模型，由OpenAI开发，是一个多模态视觉和语言模型。它将图像和文本描述映射到同一潜在空间，从而能够确定图像和描述是否匹配。CLIP以**对比**方式进行训练，以预测数据集中超过4亿对图像-文本对中哪个描述对应哪个图像[1]。令人惊讶的是，预训练CLIP生成的分类器显示出与监督模型基准相当的竞争结果，在本教程中，我们将利用这一预训练模型来生成一个眼镜检测器。

***CLIP对比训练***

CLIP模型由图像编码器和文本编码器组成（图1）。在训练过程中，一批图像通过图像编码器（ResNet变体或ViT）处理，以获得图像表示张量（嵌入）。与此同时，它们的对应描述通过文本编码器（Transformer）处理，以获得文本嵌入。CLIP模型的训练目的是预测图像嵌入属于哪个文本嵌入。通过联合训练图像编码器和文本编码器来最大化真实配对的图像和文本嵌入的余弦相似度[2]（图1，对角轴上的蓝色方块），同时最小化错误配对的嵌入之间的余弦相似度（图1，白色方块）。优化是通过对这些相似度分数进行对称交叉熵损失来实现的。

![](../Images/73a32d9c8f11303d6607eedf21b25b46.png)

图1 — CLIP训练过程的迷你批次示意图。T1是class1的嵌入向量，I1是image1的嵌入向量，等等。| 图片来源于Radford等人，2021 [1]

***创建自定义分类器***

要使用CLIP创建自定义分类器，首先将类别名称转换为文本嵌入向量，由预训练的文本编码器完成，同时图像则由预训练的图像编码器嵌入（图2）。然后计算图像嵌入与每个文本嵌入之间的余弦相似度，并将图像分配给具有最高余弦相似度分数的类别。

![](../Images/aff008258be6fe18bd9efae5cda75b55.png)

图 2 — 使用 CLIP 的零样本分类 | 图像来自 Radford 等人，2021 [1]，由作者编辑。人脸图像取自 Kaggle 上的‘有眼镜还是没有眼镜’数据集 [3]。

# **代码实现**

***数据集***

在本教程中，我们将创建一个图像分类器，用于检测人们是否戴眼镜，并使用 Kaggle 上的‘有眼镜还是没有眼镜’数据集 [3] 来评估我们分类器的性能。尽管数据集包含 5000 张图像，但我们仅使用前 100 张以加快演示。数据集包含一个包含所有图像的文件夹和一个包含标签的 CSV 文件。为了方便加载图像路径和标签，我们将自定义 Pytorch `Dataset` 类来创建 `CustomDataset()` 类。您可以在提供的 notebook 中找到相应的代码。

![](../Images/bb29b5a302ddcb5f1827689d1f02a49f.png)

来自 Kaggle 上‘有眼镜还是没有眼镜’数据集的随机图像 [3]

***加载 CLIP 模型***

在安装和导入 CLIP 及相关库之后，我们加载模型和指定模型所需的 torchvision 转换管道。文本编码器是 Transformer，而图像编码器可以是 Vision Transformer (ViT) 或 ResNet 变体，如 ResNet50。要查看可用的图像编码器，可以使用命令 `clip.available_models()`。

```py
print( clip.available_models() )
model, preprocess = clip.load("RN50")
```

***提取文本嵌入***

文本标签首先由文本分词器 (`clip.tokenize()`) 处理，将标签词转换为数值。这生成一个大小为 N x 77 的填充张量（N 是类别数，二分类中为 2 x 77），作为文本编码器的输入。文本编码器将张量转换为 N x 512 的文本嵌入张量，其中每个类别由一个向量表示。要编码文本并检索嵌入，您可以使用 `model.encode_text()` 方法。

```py
preprocessed_text = clip.tokenize(['no glasses','glasses'])
text_embedding = model.encode_text(preprocessed_text)
```

***提取图像嵌入***

在输入到图像编码器之前，每张图像会经过预处理，包括中心裁剪、归一化和调整大小，以满足图像编码器的要求。一旦预处理完成，图像将传递给图像编码器，生成 1 x 512 的图像嵌入张量作为输出。

```py
preprocessed_image = preprocess(Image.open(image_path)).unsqueeze(0)
image_embedding = model.encode_image(preprocessed_image)
```

***相似性结果***

为了衡量图像编码与每个文本标签编码之间的相似性，我们将使用余弦相似度距离度量。`model()`接收预处理后的图像和文本输入，将它们通过图像和文本编码器，并计算对应图像和文本特征之间的余弦相似度，乘以 100（`image_logits`）。然后使用 Softmax 将 logits 归一化为每个类别的概率分布列表。由于我们不训练模型，我们将使用`torch.no_grad()`禁用梯度计算。

```py
with torch.no_grad():
    image_logits, _ = model(preprocessed_image, preprocessed_text)
proba_list = image_logits.softmax(dim=-1).cpu().numpy()[0]
```

具有最高概率的类别被设置为预测类别，并提取其索引、概率和对应的标记。

```py
y_pred = np.argmax(proba_list)
y_pred_proba = np.max(proba_list)
y_pred_token = ['no glasses','glasses'][y_pred_idx]
```

***封装代码***

我们可以创建一个名为 `CustomClassifier` 的 Python 类来封装这些代码。初始化时，预训练的 CLIP 模型会被加载，并为每个标签生成嵌入的文本表示向量。我们将定义一个 `classify()` 方法，该方法接受一个图像路径作为输入，并返回预测的标签及其概率分数（存储在名为`df_results`的 DataFrame 中）。为了评估模型的性能，我们将定义一个 `validate()` 方法，该方法使用 PyTorch 数据集实例（`CustomDataset()`）来检索图像和标签，然后通过调用 `classify()` 方法来预测结果，并评估模型的性能。该方法返回一个包含所有图像预测标签和概率分数的 DataFrame。`max_images` 参数用于限制图像数量为 100。

```py
class CustomClassifier:

    def __init__(self, prompts):

        self.class_prompts = prompts
        self.device = "cuda" if torch.cuda.is_available() else "cpu"
        self.model, self.preprocess = clip.load("RN50", device=self.device) # "ViT-B/32"
        self.preprocessed_text = clip.tokenize(self.class_prompts).to(self.device)
        print(f'Classes Prompts: {self.class_prompts}')

    def classify(self, image_path, y_true = None):

        preprocessed_image = self.preprocess(Image.open(image_path)).unsqueeze(0).to(self.device)

        with torch.no_grad():
            image_logits, _ = self.model(preprocessed_image, self.preprocessed_text)
            proba_list = image_logits.softmax(dim=-1).cpu().numpy()[0]

        y_pred = np.argmax(proba_list)
        y_pred_proba = np.max(proba_list)
        y_pred_token = self.class_prompts[y_pred]
        results = pd.DataFrame([{'image': image_path, 'y_true': y_true, 'y_pred': y_pred, 'y_pred_token': y_pred_token, 'proba': y_pred_proba}])
        return results

    def validate (self, dataset, max_images):

        df_results = pd.DataFrame()
        for sample in tqdm(range(max_images)):
            image_path, class_idx = dataset[sample]
            image_results = self.classify(image_path, class_idx)
            df_results = pd.concat([df_results, image_results])

        accuracy = accuracy_score(df_results.y_true, df_results.y_pred)
        print(f'Accuracy - {round(accuracy,2)}')
        return accuracy, df_results
```

可以使用 `classify()` 方法对单张图像进行分类：

```py
prompts = ['no glasses','glasses']
image_results = CustomClassifier(prompts).classify(image_path)
```

分类器的性能可以通过 `validate()` 方法进行评估：

```py
accuracy, df_results = CustomClassifier(prompts).validate(glasses_dataset, max_images =100)
```

值得注意的是，使用原始的 [‘无眼镜’，‘眼镜’] 类别标签，我们在没有训练任何模型的情况下取得了 0.82 的不错准确率，并且我们可以通过提示工程进一步提高结果。

***提示工程***

CLIP 分类器将文本标签（称为提示）编码到一个学习到的潜在空间中，并将其与图像潜在空间进行比较。修改提示的措辞可能会导致不同的文本嵌入，这会影响分类器的性能。为了提高预测准确率，我们将通过反复试验来探索多个提示，选择结果最好的那个。例如，使用提示‘没有眼镜的男人的照片’和‘戴眼镜的男人的照片’的准确率为 0.94。

```py
prompts = ['photo of a man with no glasses', 'photo of a man with glasses']
accuracy, df_results = CustomClassifier(prompts).validate(glasses_dataset, max_images =100)
```

分析多个提示产生了以下结果：

+   [‘无眼镜’，‘眼镜’] — 0.82 的准确率

+   [‘无眼镜的脸’，‘戴眼镜的脸’] — 0.89 的准确率

+   [‘没有眼镜的男人的照片’，‘戴眼镜的男人的照片’] — 0.94 的准确率

正如我们所看到的，调整措辞可以显著提升性能。通过分析多个提示，我们将准确率从 0.82 提升到了 0.94。然而，需要注意的是，避免对提示进行过拟合。

# **总结**

CLIP 模型是开发零样本分类器的一个非常强大的工具，适用于各种任务。使用 CLIP，我能够轻松地在我的项目中生成实时分类器，并取得了非常满意的准确率。然而，CLIP 在细粒度分类、抽象或系统性任务（如计数物体）以及预测真正不在其预训练数据集中覆盖的图像时可能会遇到困难。因此，在进行新任务之前应对其性能进行评估。

使用下面提供的 Jupyter notebook，你可以轻松创建自己的自定义分类器。只需按照说明进行操作，添加你的数据，你就能快速启动并运行一个个性化的分类器。

# 感谢阅读！

**想了解更多？**

+   [**探索**](https://medium.com/@lihigurarie) 我所写的其他文章

+   [**订阅**](https://medium.com/@lihigurarie/subscribe) 以便在我发布文章时收到通知

+   在 [**Linkedin**](https://www.linkedin.com/in/lihi-gur-arie/) 上关注我

# 完整的 Jupyter Notebook 代码

教程的完整代码可以在第一个参考文献 [0] 中找到。

***参考文献***

[0] 代码: [https://gist.github.com/Lihi-Gur-Arie/844a4c3e98a7561d4e0ddb95879f8c11](https://gist.github.com/Lihi-Gur-Arie/9356a2018c3420a01e3033875405f605)

[1] CLIP 文章: [https://arxiv.org/pdf/2103.00020v1.pdf](https://arxiv.org/pdf/2103.00020v1.pdf)

[2] 余弦相似度回顾: [https://towardsdatascience.com/understanding-cosine-similarity-and-its-application-fd42f585296a](/understanding-cosine-similarity-and-its-application-fd42f585296a)

[3] 来自 Kaggle 的‘眼镜与非眼镜’数据集，许可证 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/): [https://www.kaggle.com/datasets/jeffheaton/glasses-or-no-glasses](https://www.kaggle.com/datasets/jeffheaton/glasses-or-no-glasses)
