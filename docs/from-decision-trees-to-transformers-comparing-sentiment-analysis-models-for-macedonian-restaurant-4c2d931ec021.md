# 从决策树到变换器：比较马其顿餐厅评论的情感分析模型

> 原文：[https://towardsdatascience.com/from-decision-trees-to-transformers-comparing-sentiment-analysis-models-for-macedonian-restaurant-4c2d931ec021?source=collection_archive---------4-----------------------#2023-03-03](https://towardsdatascience.com/from-decision-trees-to-transformers-comparing-sentiment-analysis-models-for-macedonian-restaurant-4c2d931ec021?source=collection_archive---------4-----------------------#2023-03-03)

## 分析马其顿餐厅评论的机器学习技术

[](https://medium.com/@danilo.najkov?source=post_page-----4c2d931ec021--------------------------------)[![Danilo Najkov](../Images/e0e8976f1f9f78ae58ba7efcc90a4f00.png)](https://medium.com/@danilo.najkov?source=post_page-----4c2d931ec021--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4c2d931ec021--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4c2d931ec021--------------------------------) [Danilo Najkov](https://medium.com/@danilo.najkov?source=post_page-----4c2d931ec021--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F19802d0e7d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-decision-trees-to-transformers-comparing-sentiment-analysis-models-for-macedonian-restaurant-4c2d931ec021&user=Danilo+Najkov&userId=19802d0e7d&source=post_page-19802d0e7d----4c2d931ec021---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4c2d931ec021--------------------------------) ·10分钟阅读·2023年3月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4c2d931ec021&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-decision-trees-to-transformers-comparing-sentiment-analysis-models-for-macedonian-restaurant-4c2d931ec021&user=Danilo+Najkov&userId=19802d0e7d&source=-----4c2d931ec021---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4c2d931ec021&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-decision-trees-to-transformers-comparing-sentiment-analysis-models-for-macedonian-restaurant-4c2d931ec021&source=-----4c2d931ec021---------------------bookmark_footer-----------)![](../Images/af0e8f1d709eb059137751a9703ae73d.png)

作者提供的图形

-   尽管自然语言处理的机器学习模型传统上集中于英语和西班牙语等流行语言，但较少使用的语言的发展则相对较少。然而，由于COVID-19大流行导致电子商务的兴起，即使是像马其顿语这样较少使用的语言也通过在线评论产生了大量数据。这为开发和训练马其顿语餐馆评论的情感分析机器学习模型提供了机会，这可以帮助企业更好地理解客户情感并改善服务。在这项研究中，我们解决了这一问题中出现的挑战，并探讨和比较了用于分析马其顿餐馆评论情感的各种情感分析模型，从经典的随机森林到现代的深度学习技术和变压器。

**内容**

+   挑战与数据预处理

+   创建向量嵌入

    - LASER 嵌入

    - 多语言通用文本编码器

    - OpenAI Ada v2

+   机器学习模型

    - 随机森林

    - XGBoost

    - 支持向量机

    - 深度学习

    - 变压器

+   结果与讨论

+   未来工作

+   结论

# 数据预处理

-   语言是独特的人类交流工具，计算机在没有适当处理技术的情况下无法解释它。为了让机器分析和理解语言，我们需要以计算机可处理的方式表示复杂的语义和词汇信息。实现这一目标的一种流行方法是使用向量表示。近年来，除了特定语言的表示模型外，多语言模型也出现了。这些模型可以捕捉大量语言的文本语义上下文。

-   然而，对于使用西里尔字母的语言，额外的挑战是由于互联网用户经常使用拉丁字母表达自己，导致数据中混合了拉丁和西里尔文本。为了解决这一挑战，我使用了来自本地餐馆的大约500条评论的数据集，其中包含拉丁和西里尔字母。该数据集还包括一小部分英文评论，有助于评估在混合数据上的表现。此外，在线文本可能包含如表情符号等需要去除的符号。因此，数据预处理是进行任何文本嵌入之前的关键步骤。

```py
import pandas as pd
import numpy as np

# load the dataset into a dataframe
df = pd.read_csv('/content/data.tsv', sep='\t')

# see the distribution of the sentiment classes
df['sentiment'].value_counts()

# -------
# 0    337
# 1    322
# Name: sentiment, dtype: int64
```

-   数据集包含正类和负类，分布几乎相等。为了去除表情符号，我使用了Python库`emoji`，它可以轻松去除表情符号和其他符号。

```py
!pip install emoji
import emoji

clt = []
for comm in df['comment'].to_numpy():
  clt.append(emoji.replace_emoji(comm, replace=""))

df['comment'] = clt
df.head()
```

-   对于西里尔和拉丁文本的问题，我将所有文本转换成其中一种，以便在两者上测试机器学习模型，以比较其性能。我使用了“cyrtranslit”库来完成这一任务。它支持大多数西里尔字母，如马其顿语、保加利亚语、乌克兰语等。

```py
import cyrtranslit
latin = []
cyrillic = []
for comm in df['comment'].to_numpy():
  latin.append(cyrtranslit.to_latin(comm, "mk"))
  cyrillic.append(cyrtranslit.to_cyrillic(comm, "mk"))

df['comment_cyrillic'] = cyrillic
df['comment_latin'] = latin
df.head()
```

![](../Images/df4ff53d87f8ed5066a9fc78eb489af6.png)

图1\. 转换后的输出

对于我使用的嵌入模型，通常不需要移除标点符号、停用词及其他文本清理。这些模型设计用来处理自然语言文本，包括标点符号，并且当文本保持完整时，通常能够更准确地捕捉句子的含义。这样文本的预处理就完成了。

# 向量嵌入

目前，没有大规模的马其顿语表示模型可用。然而，我们可以使用在马其顿语文本上训练的多语言模型。虽然有几个这样的模型可用，但对于这项任务，我发现LASER和Multilingual Universal Sentence Encoder是最合适的选择。

## LASER

LASER（语言无关句子表示）是一种生成高质量多语言句子嵌入的语言无关方法。LASER模型基于一个两阶段过程，其中第一阶段是预处理文本，包括分词、小写转换和应用sentencepiece。这部分是特定语言的。第二阶段涉及将预处理后的输入文本映射到固定长度的嵌入，使用的是多层双向LSTM。

LASER已被证明在一系列基准数据集上优于其他流行的句子嵌入方法，如fastText和InferSent。此外，LASER模型是开源的且免费提供，方便所有人使用。

使用LASER创建嵌入是一个简单的过程：

```py
!pip install laserembeddings
!python -m laserembeddings download-models

from laserembeddings import Laser

# create the embeddings
laser = Laser()
embeddings_c = laser.embed_sentences(df['comment_cyrillic'].to_numpy(),lang='mk')
embeddings_l = laser.embed_sentences(df['comment_latin'].to_numpy(),lang='mk')

# save the embeddings
np.save('/content/laser_multi_c.npy', embeddings_c)
np.save('/content/laser_multi_l.npy', embeddings_l)
```

## 多语言通用句子编码器

多语言通用句子编码器（MUSE）是一个预训练模型，用于生成句子嵌入，由Facebook开发。MUSE旨在将多种语言的句子编码到一个共同的空间中。

该模型基于一个深度神经网络，使用编码器-解码器架构来学习句子与其对应的高维空间嵌入向量之间的映射。MUSE在一个大规模的多语言语料库上进行训练，该语料库包括维基百科、新闻文章和网页文本。

```py
!pip install tensorflow_text
import tensorflow as tf
import tensorflow_hub as hub
import numpy as np
import tensorflow_text

# load the MUSE module
module_url = "https://tfhub.dev/google/universal-sentence-encoder-multilingual-large/3"
embed = hub.load(module_url)

sentences = df['comment_cyrillic'].to_numpy()
muse_c = embed(sentences)
muse_c = np.array(muse_c)

sentences = df['comment_latin'].to_numpy()
muse_l = embed(sentences)
muse_l = np.array(muse_l)

np.save('/content/muse_c.npy', muse_c)
np.save('/content/muse_l.npy', muse_l)
```

## OpenAI Ada v2

在2022年底，OpenAI宣布了他们全新的最先进嵌入模型 [text-embedding-ada-002](https://openai.com/blog/new-and-improved-embedding-model/)。由于该模型建立在GPT-3之上，它具有多语言处理能力。为了比较斯拉夫字母和拉丁字母评论之间的结果，我在这两个数据集上运行了该模型。

```py
!pip install openai

import openai
openai.api_key = 'YOUR_KEY_HERE'

embeds_c = openai.Embedding.create(input = df['comment_cyrillic'].to_numpy().tolist(), model='text-embedding-ada-002')['data']
embeds_l = openai.Embedding.create(input = df['comment_latin'].to_numpy().tolist(), model='text-embedding-ada-002')['data']

full_arr_c = []
for e in embeds_c:
  full_arr_c.append(e['embedding'])
full_arr_c = np.array(full_arr_c)

full_arr_l = []
for e in embeds_l:
  full_arr_l.append(e['embedding'])
full_arr_l = np.array(full_arr_l)

np.save('/content/openai_ada_c.npy', full_arr_c)
np.save('/content/openai_ada_l.npy', full_arr_l)
```

# 机器学习模型

本节探讨了用于预测马其顿餐馆评论情感的各种机器学习模型。从传统机器学习模型到深度学习技术，我们将深入研究每种模型的优缺点，并比较它们在数据集上的表现。

在运行任何模型之前，应将数据按每种嵌入类型分为训练集和测试集。这可以通过`sklearn`库轻松完成。

```py
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(embeddings_c, df['sentiment'], test_size=0.2, random_state=42)
```

## 随机森林

![](../Images/f3c3e1b81c5fa1eb8e8f42e7fb391761.png)

图2\. 随机森林分类的简化表示。构建了100棵决策树，结果通过对每棵决策树的结果进行多数投票来计算。（作者绘制）

随机森林是一种广泛使用的机器学习算法，它使用决策树的集成来分类数据点。该算法通过在完整数据集的子集和特征的随机子集上训练每棵决策树来工作。在推断过程中，每棵决策树生成情感的预测，最终输出通过对所有树的多数投票来获得。这种方法有助于防止过拟合，并能导致更稳健和准确的预测。

```py
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report, confusion_matrix

rfc = RandomForestClassifier(n_estimators=100)
rfc.fit(X_train, y_train)
print(classification_report(y_test,rfc.predict(X_test)))
print(confusion_matrix(y_test,rfc.predict(X_test)))
```

## XGBoost

![](../Images/069088713baa3757823045c72cc6597c.png)

图3\. 提升基算法的顺序过程。每棵后续决策树在前一棵树的残差（错误）上进行训练。（作者绘制）

XGBoost（极端梯度提升）是一种强大的集成方法，主要用于表格数据。与随机森林一样，XGBoost也使用决策树来分类数据点，但采用不同的方法。XGBoost不是一次训练所有树，而是以顺序方式训练每棵树，从前一棵树的错误中学习。这一过程称为提升，即将弱模型组合成一个更强的模型。尽管XGBoost主要在表格数据上产生出色的结果，但用向量嵌入测试它也很有趣。

```py
from xgboost import XGBClassifier
from sklearn.metrics import classification_report, confusion_matrix

rfc = XGBClassifier(max_depth=15)
rfc.fit(X_train, y_train)
print(classification_report(y_test,rfc.predict(X_test)))
print(confusion_matrix(y_test,rfc.predict(X_test)))
```

## 支持向量机

![](../Images/a7582c92959dbf18690e24b78a04b7ac.png)

图4\. 支持向量分类的简化表示。在具有1024个输入特征的情感分析中，超平面将是1023维的。（作者绘制）

支持向量机（SVM）是一种流行且强大的机器学习算法，用于分类和回归任务。它通过寻找将数据分成不同类别的最佳超平面，同时最大化类别之间的间隔来工作。SVM特别适用于高维数据，并可以使用核函数处理非线性边界。

```py
from sklearn.svm import SVC
from sklearn.metrics import classification_report, confusion_matrix

rfc = SVC()
rfc.fit(X_train, y_train)
print(classification_report(y_test,rfc.predict(X_test)))
print(confusion_matrix(y_test,rfc.predict(X_test)))
```

## 深度学习

![](../Images/2357db5eea9f7f39d928d7476ad3857d.png)

图5\. 该问题中使用的神经网络的简化表示。（作者绘制）

深度学习是一种先进的机器学习方法，利用由多层神经元组成的人工神经网络。深度学习网络在文本和图像数据上表现出色。使用库Keras实现这些网络是一个简单的过程。

```py
import tensorflow as tf
from tensorflow import keras
from sklearn.model_selection import train_test_split
from sklearn.metrics import classification_report, confusion_matrix

model = keras.Sequential()
model.add(keras.layers.Dense(256, activation='relu', input_shape=(1024,)))
model.add(keras.layers.Dropout(0.2))
model.add(keras.layers.Dense(128, activation='relu'))
model.add(keras.layers.Dense(1, activation='sigmoid'))

model.compile(optimizer='adam', loss='binary_crossentropy', metrics=['accuracy'])

history = model.fit(X_train, y_train, epochs=11, validation_data=(X_test, y_test))

test_loss, test_acc = model.evaluate(X_test, y_test)
print('Test accuracy:', test_acc)
y_pred = model.predict(X_test)

print(classification_report(y_test,y_pred.round()))
print(confusion_matrix(y_test,y_pred.round()))
```

在这里，使用了具有两个隐藏层和一个整流线性单元（ReLU）激活函数的神经网络。输出层包含一个带有 sigmoid 激活函数的单一神经元，使得网络能够对正面或负面情感进行二分类预测。二元交叉熵损失函数与 sigmoid 激活函数配对，用于训练模型。此外，还使用了 Dropout 来帮助防止过拟合，并提高模型的泛化能力。我测试了各种不同的超参数，发现这种配置最适合这个问题。

使用以下函数我们可以可视化模型的训练过程。

```py
import matplotlib.pyplot as plt

def plot_accuracy(history):
    plt.plot(history.history['accuracy'])
    plt.plot(history.history['val_accuracy'])
    plt.title('Model Accuracy')
    plt.xlabel('Epoch')
    plt.ylabel('Accuracy')
    plt.legend(['Train', 'Validation'], loc='upper left')
    plt.show()
```

![](../Images/8848ddc6401ccfdb6b39638a2638dfc6.png)

图 6\. 示例训练输出

## 变换器

![](../Images/4f481ee30e9e5081d647526efa18634f.png)

图 7\. BERT 大型语言模型的预训练和微调过程。（来源：[原始 BERT 论文](https://arxiv.org/pdf/1810.04805v2.pdf)）

微调变换器是自然语言处理中的一种流行技术，涉及调整预训练的变换器模型以适应特定任务。变换器，如 BERT、GPT-2 和 RoBERTa，经过大量文本数据的预训练，能够学习语言中的复杂模式和关系。然而，为了在特定任务上表现良好，如情感分析或文本分类，这些模型需要在任务特定的数据上进行微调。

对于这些类型的模型，我们之前创建的向量表示是不需要的，因为它们直接处理从文本中提取的标记。对于马其顿的情感分析任务，我使用了 `bert-base-multilingual-uncased`，这是 BERT 模型的多语言版本。

[HuggingFace](https://huggingface.co/) 使得微调变换器变得非常简单。首先需要将数据加载到变换器数据集中。然后对文本进行标记化，最后训练模型。

```py
from sklearn.model_selection import train_test_split
from datasets import load_dataset
from transformers import TrainingArguments, Trainer
from sklearn.metrics import classification_report, confusion_matrix

# create csv of train and test sets to be loaded by the dataset
df.rename(columns={"sentiment": "label"}, inplace=True)
train, test = train_test_split(df, test_size=0.2)
pd.DataFrame(train).to_csv('train.csv',index=False)
pd.DataFrame(test).to_csv('test.csv',index=False)

# load the dataset
dataset = load_dataset("csv", data_files={"train": "train.csv", "test": "test.csv"})

# tokenize the text
tokenizer = AutoTokenizer.from_pretrained('bert-base-multilingual-uncased')
encoded_dataset = dataset.map(lambda t: tokenizer(t['comment_cyrillic'],  truncation=True), batched=True,load_from_cache_file=False)

# load the pretrained model
model = AutoModelForSequenceClassification.from_pretrained('bert-base-multilingual-uncased',num_labels =2)

# fine-tune the model
arg = TrainingArguments(
    "mbert-sentiment-mk",
    learning_rate=5e-5,
    num_train_epochs=5,
    per_device_eval_batch_size=8,
    per_device_train_batch_size=8,
    seed=42,
    push_to_hub=True
)
trainer = Trainer(
    model=model,
    args=arg,
    tokenizer=tokenizer,
    train_dataset=encoded_dataset['train'],
    eval_dataset=encoded_dataset['test']
)
trainer.train()

# get predictions
predictions = trainer.predict(encoded_dataset["test"])
preds = np.argmax(predictions.predictions, axis=-1)

# evaluate
print(classification_report(predictions.label_ids,preds))
print(confusion_matrix(predictions.label_ids,preds))
```

我们已经成功地对 BERT 进行了情感分析的微调。

# 结果与讨论

![](../Images/9db813559915c7d0a87f0d2eadf8bfcf.png)

图 8\. 所有模型的结果

对马其顿餐馆评论的情感分析结果令人鼓舞，多个模型达到了高精度和 F1 分数。实验表明，深度学习模型和变换器在表现上优于传统的机器学习模型，如随机森林和支持向量机，尽管差距不大。使用新的 OpenAI 嵌入的变换器和深度神经网络成功突破了 0.9 的准确率障碍。

OpenAI 嵌入模型 `textembedding-ada-002` 大幅提升了即使是经典机器学习模型的结果，尤其是在支持向量机上。在这项研究中，使用该嵌入在深度学习模型上对西里尔文本取得了最佳结果。

总的来说，拉丁文本的表现比西里尔文本差。虽然我最初假设这些模型的表现会更好，考虑到拉丁语中类似词汇的普遍性以及嵌入模型是基于这种数据训练的，但研究结果并未支持这一假设。

# 未来工作

在未来的工作中，收集更多数据以进一步训练和测试模型将非常有价值，特别是涵盖更多样化的评论主题和来源。此外，尝试将更多特征（如元数据（例如，评论者的年龄、性别、位置）或时间信息（例如，评论时间））纳入模型中，可能会提高其准确性。最后，将分析扩展到其他不那么常见的语言，并比较这些模型与马其顿评论训练的模型的表现，将是很有趣的。

# 结论

总结来说，本文展示了多种机器学习模型和嵌入技术在马其顿餐馆评论情感分析中的有效性。探讨并比较了几种经典的机器学习模型，如随机森林和支持向量机，以及现代深度学习技术，包括神经网络和变压器。结果表明，经过微调的变压器模型和使用最新OpenAI嵌入的深度学习模型表现优于其他方法，验证准确率高达90%。

感谢阅读！
