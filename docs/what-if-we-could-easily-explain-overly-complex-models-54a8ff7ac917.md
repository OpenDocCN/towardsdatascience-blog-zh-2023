# 我们是否可以轻松解释过于复杂的模型？

> 原文：[https://towardsdatascience.com/what-if-we-could-easily-explain-overly-complex-models-54a8ff7ac917?source=collection_archive---------6-----------------------#2023-09-28](https://towardsdatascience.com/what-if-we-could-easily-explain-overly-complex-models-54a8ff7ac917?source=collection_archive---------6-----------------------#2023-09-28)

## 生成反事实解释变得容易多了，但什么是反事实解释，我该如何使用它们？

[](https://mazzine.medium.com/?source=post_page-----54a8ff7ac917--------------------------------)[![Raphael Mazzine, Ph.D.](../Images/0b05145a98a5a531d55f5a88fada0af5.png)](https://mazzine.medium.com/?source=post_page-----54a8ff7ac917--------------------------------)[](https://towardsdatascience.com/?source=post_page-----54a8ff7ac917--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----54a8ff7ac917--------------------------------) [Raphael Mazzine, Ph.D.](https://mazzine.medium.com/?source=post_page-----54a8ff7ac917--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F10c8c0e5ab30&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-if-we-could-easily-explain-overly-complex-models-54a8ff7ac917&user=Raphael+Mazzine%2C+Ph.D.&userId=10c8c0e5ab30&source=post_page-10c8c0e5ab30----54a8ff7ac917---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----54a8ff7ac917--------------------------------) · 12分钟阅读 · 2023年9月28日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F54a8ff7ac917&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-if-we-could-easily-explain-overly-complex-models-54a8ff7ac917&user=Raphael+Mazzine%2C+Ph.D.&userId=10c8c0e5ab30&source=-----54a8ff7ac917---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F54a8ff7ac917&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-if-we-could-easily-explain-overly-complex-models-54a8ff7ac917&source=-----54a8ff7ac917---------------------bookmark_footer-----------)![](../Images/54b393e95beac8c94c9a0394e88e75dd.png)

图像由 Illusion Diffusion 模型生成，CFNOW 文本作为幻觉（试着眯眼并从一定距离观察） | 作者使用 [Stable Diffusion 模型（许可）](https://huggingface.co/spaces/CompVis/stable-diffusion-license) 生成的图像

本文基于以下文章：[https://www.sciencedirect.com/science/article/abs/pii/S0377221723006598](https://www.sciencedirect.com/science/article/abs/pii/S0377221723006598)

这里是 CFNOW 仓库的地址：[https://github.com/rmazzine/CFNOW](https://github.com/rmazzine/CFNOW)

如果你正在阅读这篇文章，你可能已经知道人工智能（AI）在我们今天的世界中变得多么关键。然而，需要注意的是，虽然看似有效的新颖机器学习方法及其广泛的普及可能会导致不可预见的/不良的后果。

这就引出了为什么可解释人工智能（XAI）在确保人工智能的伦理和负责任发展中是至关重要的。这一领域表明，解释包含数百万甚至数十亿参数的模型并非易事。对此的回答是多方面的，因为有许多方法揭示模型的不同方面，LIME [1] 和 SHAP [2] 是流行的例子。

然而，这些方法生成的解释复杂度可能导致复杂的图表或分析，这可能会导致那些未充分了解的专家产生误解。绕过这种复杂性的一个可能方法是一个简单自然的解释方法，称为反事实解释 [3]。

反事实解释利用了自然的人类行为来解释事物——创建“替代世界”，在这些世界中，通过改变一些参数可以改变结果。这是一种常见的做法，你可能已经做过类似的事情——“*如果我早点起床，我就不会错过公交车*”，这种解释以直接的方式突出结果的主要原因。

更深入地探讨，**反事实不仅仅是简单的解释**；它们可以作为变更的指导，帮助调试异常行为，并验证某些特征是否可能修改预测（虽然对评分的影响不大）。这种多功能性强调了解释你的预测的重要性。这不仅仅是负责任的人工智能问题；它也是改进模型并在预测范围之外使用它们的途径。反事实解释的一个显著特征是它们以决策为驱动，使其与预测变化直接相关 [6]，这与LIME和SHAP更适合于解释评分的特性不同。

鉴于明显的好处，人们可能会想知道为什么反事实解释没有更受欢迎。这是一个合理的问题！反事实解释广泛采用的主要障碍有三方面 [4, 5]： (1) 缺乏用户友好且兼容的反事实生成算法，(2) 算法在生成反事实方面的低效，(3) 以及缺乏全面的可视化表现。

但我有一些好消息要告诉你！一个新的包，CFNOW（CounterFactuals NOW 或 CounterFactual Nearest Optimal Wololo），正致力于解决这些挑战。CFNOW是一个多功能的Python包，能够为各种数据类型（如表格、图像和文本（嵌入）输入）生成多个反事实。它采用模型无关的方法，只需要最少的数据——（1）实际点（需要解释的点）和（2）预测函数。

此外，CFNOW的结构允许开发和集成基于自定义逻辑的新策略来查找和微调反事实。它还具有CounterPlots，这是一种用于视觉化表示反事实解释的新策略。

CFNOW的核心是一个框架，它将数据转换为CF生成器可以处理的单一结构。接着，通过一个两步过程找到并优化反事实。为了防止陷入局部最小值，包实现了Tabu Search，这是一种数学启发式方法，使其能够探索新的区域，以便在这些区域中可能更好地优化目标函数。

本文的后续部分将集中展示如何高效利用CFNOW生成表格、图像和文本（嵌入）分类器的解释。

# 表格分类器

在这里，我们展示了通常的内容，你有一个包含多种数据类型的表格数据。在下面的示例中，我将使用一个数据集，该数据集具有数值连续型、分类二元型和分类独热编码数据，以展示CFNOW的全部功能。

首先，你需要安装CFNOW包，要求Python版本高于3.8：

```py
 pip install cfnow
```

> （这是该示例的完整代码： [https://colab.research.google.com/drive/1GUsVfcM3I6SpYCmsBAsKMsjVdm-a6iY6?usp=sharing](https://colab.research.google.com/drive/1GUsVfcM3I6SpYCmsBAsKMsjVdm-a6iY6?usp=sharing)）

在第一部分中，我们将使用成人数据集创建一个分类器。然后，这里没有太多新鲜事：

```py
import warnings

import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score
warnings.filterwarnings("ignore", message="X does not have valid feature names, but RandomForestClassifier was fitted with feature names")
```

我们导入基本包来构建分类模型，同时我们还会禁用与在没有列名的情况下进行预测相关的警告。

然后，我们继续编写分类器，其中类1表示收入低于或等于50k（<=50K），类0表示高收入。

```py
# Make the classifier
import warnings

import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score
warnings.filterwarnings("ignore", message="X does not have valid feature names, but RandomForestClassifier was fitted with feature names")

# Load the Adult dataset
dataset_url = "https://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.data"
column_names = ['age', 'workclass', 'fnlwgt', 'education', 'education-num', 'marital-status',
                'occupation', 'relationship', 'race', 'sex', 'capital-gain', 'capital-loss',
                'hours-per-week', 'native-country', 'income']

data = pd.read_csv(dataset_url, names=column_names, na_values=" ?", skipinitialspace=True)

# Drop rows with missing values
data = data.dropna()

# Identify the categorical features that are not binary
non_binary_categoricals = [column for column in data.select_dtypes(include=['object']).columns 
                           if len(data[column].unique()) > 2]

binary_categoricals = [column for column in data.select_dtypes(include=['object']).columns 
                       if len(data[column].unique()) == 2]

cols_numericals = [column for column in data.select_dtypes(include=['int64']).columns]

# Apply one-hot encoding to the non-binary categorical features
data = pd.get_dummies(data, columns=non_binary_categoricals)

# Convert the binary categorical features into numbers
# This will also binarize the target variable (income)
for bc in binary_categoricals:
    data[bc] = data[bc].apply(lambda x: 1 if x == data[bc].unique()[0] else 0)

# Split the dataset into features and target variable
X = data.drop('income', axis=1)
y = data['income']

# Split the dataset into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Initialize a RandomForestClassifier
clf = RandomForestClassifier(random_state=42)

# Train the classifier
clf.fit(X_train, y_train)

# Make predictions on the testing set
y_pred = clf.predict(X_test)

# Evaluate the classifier
accuracy = accuracy_score(y_test, y_pred)
print("Accuracy:", accuracy)
```

使用上面的代码，我们创建了一个数据集，对其进行预处理，创建了一个分类模型，并对测试集进行了预测和评估。

现在，让我们选择一个点（测试集中的第一个）并验证其预测：

```py
clf.predict([X_test.iloc[0]])
# Result: 0 -> High income
```

现在是时候使用CFNOW来计算我们如何通过最小修改特征来改变这个预测：

```py
from cfnow import find_tabular
# Then, we use CFNOW to generate the minimum modification to change the classification
cf_res = find_tabular(
    factual=X_test.iloc[0],
    feat_types={c: 'num' if c in cols_numericals else 'cat' for c in X.columns},
    has_ohe=True,
    model_predict_proba=clf.predict_proba,
    limit_seconds=60)
```

上面的代码中，我们：

+   `factual`将实际实例添加为pd.Series

+   `feat_types`指定特征类型（“num”表示数值连续型，“cat”表示分类变量）

+   `has_ohe`指示我们是否有OHE特征（它通过汇总那些具有相同前缀并以下划线开头的特征来自动检测OHE特征，例如，country_brazil，country_usa，country_ireland）。

+   `model_predict_proba` 包含预测函数

+   `limit_seconds` 定义了运行的总时间阈值，这一点很重要，因为微调步骤可能会无限期继续（默认是120秒）。

然后，经过一段时间，我们可以首先评估最佳反事实的类别（`cf_res.cfs`的第一个索引）。

```py
clf.predict([cf_obj.cfs[0]])
# Result: 1-> Low income
```

这里出现了一些与CFNOW的不同之处，由于它还集成了CounterPlots，我们可以绘制它们的图表，并获得更多有洞察力的信息，如下所示：

![](../Images/4b72425b5446259d2270b6591d6e6df6.png)

CounterShapley图表 | 图片由作者提供

下面的CounterShapley图显示了每个特征在生成反事实预测中的相对重要性。在这里，我们得到了一些有趣的见解，显示marial_status（如果组合在一起）占CF类别贡献的50%以上。

![](../Images/e0f2e1dad7699245bdb965c889669f7e.png)

我们的CF的贪婪图表 | 图片由作者提供

贪婪图表显示的内容与CounterShapley非常相似，主要区别在于变化的顺序。虽然CounterShapley没有考虑任何特定的顺序（使用Shapley值计算贡献），贪婪图表使用最贪婪的策略来修改实际实例，每一步都改变对CF类别贡献最大的特征。这在某些情况下可能很有用，即在贪婪的方式下给出一些指导（每一步选择实现目标的最佳方法）。

![](../Images/db0fa976f82a4cac6be00a0dcb10d107.png)

我们的CF的星座图 | 图片由作者提供

最后，我们有了最复杂的分析，即星座图。尽管它看起来令人望而却步，但其实很容易解释。每个大的红点代表一个特征的单独变化（相对于标签），而小的点代表两个或多个特征的组合。最后，大的蓝点代表CF评分。在这里，我们可以看到，获得这些特征的CF的唯一方法是将它们全部修改为各自的值（即，没有子集能生成CF）。我们还可以深入探讨并研究特征之间的关系，可能会发现有趣的模式。

在这种特定情况下，有趣的是观察到，如果这个人是女性、离婚且有一个孩子，预测的高收入会发生变化。这一反事实可以引发对不同社会群体经济影响的进一步讨论。

# 图像分类器

如前所述，CFNOW可以处理多种数据类型，因此它也可以为图像数据生成反事实。然而，对于图像数据集来说，拥有反事实意味着什么呢？

响应可能会有所不同，因为生成反事实的方法有多种。它可以是用随机噪声替换单个像素（对抗攻击使用的方法），也可以是更复杂的方法，涉及先进的分割技术。

CFNOW 使用了一种名为 quickshift 的分割方法，这是一种可靠且快速的“语义”分段检测方法。然而，可以集成（我建议你这样做）其他分割技术。

仅靠分段检测不足以生成反事实解释。我们还需要修改这些分段，用修改过的版本替换它们。对于这种修改，CFNOW 在 `replace_mode` 参数中定义了四种选项，我们可以选择：（默认）`blur`——为替换的分段添加模糊滤镜，`mean`——用平均颜色替换分段，`random`——用随机噪声替换，`inpaint`——基于邻域像素重建图像。

> 如果你想要完整的代码，可以在这里找到：[https://colab.research.google.com/drive/1M6bEP4x7ilSdh01Gs8xzgMMX7Uuum5jZ?usp=sharing](https://colab.research.google.com/drive/1M6bEP4x7ilSdh01Gs8xzgMMX7Uuum5jZ?usp=sharing)

接下来，我将展示 CFNOW 对这种数据类型的代码实现：

首先，再次，如果你还没有安装 CFNOW 包，请安装它。

```py
pip install cfnow
```

现在，让我们添加一些额外的包来加载预训练模型：

```py
pip install torch torchvision Pillow requests
```

那么，让我们加载数据，加载预训练模型，并创建一个与 CFNOW 必须接收的数据格式兼容的预测函数：

```py
import requests
import numpy as np
from PIL import Image
from torchvision import models, transforms
import torch

# Load a pre-trained ResNet model
model = models.resnet50(pretrained=True)
model.eval()

# Define the image transformation
transform = transforms.Compose([
    transforms.Resize(256),
    transforms.CenterCrop(224),
    transforms.ToTensor(),
    transforms.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225]),
])

# Fetch an image from the web
image_url = "https://upload.wikimedia.org/wikipedia/commons/thumb/4/41/Sunflower_from_Silesia2.jpg/320px-Sunflower_from_Silesia2.jpg"
response = requests.get(image_url, stream=True)
image = np.array(Image.open(response.raw))

def predict(images):
    if len(np.shape(images)) == 4:
        # Convert the list of numpy arrays to a batch of tensors
        input_images = torch.stack([transform(Image.fromarray(image.astype('uint8'))) for image in images])
    elif len(np.shape(images)) == 3:
        input_images = transform(Image.fromarray(images.astype('uint8')))
    else:
        raise ValueError("The input must be a list of images or a single image.")

    # Check if a GPU is available and if not, use a CPU
    device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
    input_images = input_images.to(device)
    model.to(device)

    # Perform inference
    with torch.no_grad():
        outputs = model(input_images)

    # Return an array of prediction scores for each image
    return torch.asarray(outputs).cpu().numpy()

LABELS_URL = "https://raw.githubusercontent.com/anishathalye/imagenet-simple-labels/master/imagenet-simple-labels.json"
def predict_label(outputs):
    # Load the labels used by the pre-trained model
    labels = requests.get(LABELS_URL).json()

    # Get the predicted labels
    predicted_idxs = [np.argmax(od) for od in outputs]
    predicted_labels = [labels[idx.item()] for idx in predicted_idxs]

    return predicted_labels

# Check the prediction for the image
predicted_label = predict([np.array(image)])
print("Predicted labels:", predict_label(predicted_label))
```

大部分代码工作与构建模型、获取数据和调整数据有关，因为要用 CFNOW 生成反事实，我们只需要：

```py
from cfnow import find_image

cf_img = find_image(img=image, model_predict=predict)

cf_img_hl = cf_img.cfs[0]
print("Predicted labels:", predict_label(predict([cf_img_hl])))

# Show the CF image
Image.fromarray(cf_img_hl.astype('uint8'))
```

在上面的示例中，我们使用了所有默认的可选参数，因此，我们使用 quickshift 对图像进行分割，并用模糊的图像替换这些分割区域。结果，我们得到了以下的实际预测：

![](../Images/ac67aab285c70f8c692c90648dcf713d.png)

实际图像被分类为“雏菊” | 图像标题：向日葵（Helianthus L）。Słonecznik，来自 [Pudelek](https://commons.wikimedia.org/wiki/User:Pudelek)（由 [Yzmo](https://commons.wikimedia.org/wiki/User:Yzmo) 和 [Vassil](https://commons.wikimedia.org/wiki/User:Vassil) 编辑）来自 [Wikimedia](https://commons.wikimedia.org/wiki/File:Sunflower_from_Silesia_Edit_2.jpg)，根据 [**GNU 自由文档许可证**](https://en.wikipedia.org/wiki/en:GNU_Free_Documentation_License) 第 1.2 版

到以下内容：

![](../Images/f19664b505a98eebc99f23c465ad9b9f.png)

CF 图像被分类为“蜜蜂” | 图像标题：向日葵（Helianthus L）。Słonecznik，来自 [Pudelek](https://commons.wikimedia.org/wiki/User:Pudelek)（由 [Yzmo](https://commons.wikimedia.org/wiki/User:Yzmo) 和 [Vassil](https://commons.wikimedia.org/wiki/User:Vassil) 编辑）来自 [Wikimedia](https://commons.wikimedia.org/wiki/File:Sunflower_from_Silesia_Edit_2.jpg)，根据 [**GNU 自由文档许可证**](https://en.wikipedia.org/wiki/en:GNU_Free_Documentation_License) 第 1.2 版

那么，这项分析的结果是什么呢？实际上，图像反事实可以成为极其有用的工具，以检测模型如何进行分类。这可以应用于以下情况：（1）我们想验证模型为何做出正确分类——确保其使用了正确的图像特征：在这种情况下，尽管模型将向日葵误分类为雏菊，但我们可以看到，模糊化花朵（而不是背景特征）会使其更改预测。它还可以（2）帮助诊断误分类的图像，这可能为图像处理和/或数据采集提供更好的洞察。

# 文本分类器

最后，我们有基于嵌入的文本分类器。尽管简单的文本分类器（使用更像表格数据的数据结构）可以使用表格反事实生成器，但基于嵌入的文本分类器，这一点不那么明确。

理由是嵌入具有可变数量的输入和单词，这些都可以显著影响预测分数和分类。

CFNOW通过两种策略解决这个问题：（1）通过移除证据或（2）通过添加反义词。第一种策略很简单，为了测量每个单词对文本的影响，我们只需移除它们并查看哪些需要移除以翻转分类。而添加反义词，我们可以可能保持语义结构（因为移除一个单词可能会严重损害语义结构）。

然后，下面的代码展示了如何在这个上下文中使用CFNOW。

> 如果你想要完整的代码，可以在这里查看：[https://colab.research.google.com/drive/1ZMbqJmJoBukqRJGqhUaPjFFRpWlujpsi?usp=sharing](https://colab.research.google.com/drive/1ZMbqJmJoBukqRJGqhUaPjFFRpWlujpsi?usp=sharing)

首先，安装CFNOW包：

```py
pip install cfnow
```

然后，安装文本分类所需的包：

```py
pip install transformers
```

然后，如前面部分所述，首先我们将构建分类器：

```py
from transformers import DistilBertTokenizer, DistilBertForSequenceClassification
from transformers import pipeline

import numpy as np

# Load pre-trained model and tokenizer for sentiment analysis
model_name = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = DistilBertTokenizer.from_pretrained(model_name)
model = DistilBertForSequenceClassification.from_pretrained(model_name)

# Define the sentiment analysis pipeline
sentiment_analysis = pipeline("sentiment-analysis", model=model, tokenizer=tokenizer)

# Define a simple dataset
text_factual = "I liked this movie because it was funny but my friends did not like it because it was too long and boring."

result = sentiment_analysis(text_factual)
print(f"{text_factual}: {result[0]['label']} (confidence: {result[0]['score']:.2f})")

def pred_score_text(list_text):
    if type(list_text) == str:
        sa_pred = sentiment_analysis(list_text)[0]
        sa_score = sa_pred['score']
        sa_label = sa_pred['label']
        return sa_score if sa_label == "POSITIVE" else 1.0 - sa_score
    return np.array([sa["score"] if sa["label"] == "POSITIVE" else 1.0 - sa["score"] for sa in sentiment_analysis(list_text)])
```

对于这段代码，我们将看到我们的事实文本具有高置信度（≥0.9）的NEGATIVE情感，然后让我们尝试为其生成反事实：

```py
from cfnow import find_text
cf_text = find_text(text_input=text_factual, textual_classifier=pred_score_text)
result_cf = sentiment_analysis(cf_text.cfs[0])
print(f"CF: {cf_text.cfs[0]}: {result_cf[0]['label']} (confidence: {result_cf[0]['score']:.2f})")
```

使用上述代码，只需更改一个单词（but），分类就从NEGATIVE变为POSITIVE，并且置信度很高。这展示了反事实如何有用，因为这些最小的修改可以对理解模型如何预测句子和/或帮助调试不良行为产生影响。

# 结论

这是一篇对 CFNOW 和反事实解释的（相对）简要介绍。关于反事实的文献非常广泛（且不断增加），如果你想深入了解，必读的开创性文章是由（我的博士生导师，大卫·马滕斯教授）撰写的[3]，这是对反事实解释有更好介绍的绝佳方式。此外，还有由 Verma 等人撰写的很好的综述[7]。总之，反事实解释是一种简单且方便的方式来解释复杂的机器学习算法决策，如果正确应用，它们还能做到远超过解释的效果。CFNOW 可以提供一种简单、快速和灵活的方式来生成反事实解释，使从业者不仅能解释，还能最大限度地利用数据和模型的潜力。

# 参考文献：

[1] — [https://github.com/marcotcr/lime](https://github.com/marcotcr/lime)

[2] — [https://github.com/shap/shap](https://github.com/shap/shap)

[3] — [https://www.jstor.org/stable/26554869](https://www.jstor.org/stable/26554869)

[4] — [https://www.mdpi.com/2076-3417/11/16/7274](https://www.mdpi.com/2076-3417/11/16/7274)

[5] — [https://arxiv.org/pdf/2306.06506.pdf](https://arxiv.org/pdf/2306.06506.pdf)

[6] — [https://arxiv.org/abs/2001.07417](https://arxiv.org/abs/2001.07417)

[7] — [https://arxiv.org/abs/2010.10596](https://arxiv.org/abs/2010.10596)
