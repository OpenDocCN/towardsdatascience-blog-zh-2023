# 步骤基础：文本分类器

> 原文：[https://towardsdatascience.com/step-by-step-basics-text-classifier-e666c6bac52b?source=collection_archive---------2-----------------------#2023-02-17](https://towardsdatascience.com/step-by-step-basics-text-classifier-e666c6bac52b?source=collection_archive---------2-----------------------#2023-02-17)

## 构建一个监督学习文本分类器的指导手册和流程图（Python）

[](https://medium.com/@lucydickinson?source=post_page-----e666c6bac52b--------------------------------)[![Lucy Dickinson](../Images/5a075bb38f9133678d55a26b2683729f.png)](https://medium.com/@lucydickinson?source=post_page-----e666c6bac52b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e666c6bac52b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e666c6bac52b--------------------------------) [Lucy Dickinson](https://medium.com/@lucydickinson?source=post_page-----e666c6bac52b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F243c7ff13cc2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstep-by-step-basics-text-classifier-e666c6bac52b&user=Lucy+Dickinson&userId=243c7ff13cc2&source=post_page-243c7ff13cc2----e666c6bac52b---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e666c6bac52b--------------------------------) ·10 分钟阅读·2023年2月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe666c6bac52b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstep-by-step-basics-text-classifier-e666c6bac52b&user=Lucy+Dickinson&userId=243c7ff13cc2&source=-----e666c6bac52b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe666c6bac52b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstep-by-step-basics-text-classifier-e666c6bac52b&source=-----e666c6bac52b---------------------bookmark_footer-----------)![](../Images/7616f5c71c4500433b491ed8fdd0db92.png)

图片由 [Patrick Tomasso](https://unsplash.com/@impatrickt?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

说到重点，构建**文本分类器**和理解**自然语言处理 (NLP)** 的世界涉及很多步骤。这些步骤必须按特定顺序实施。如果数据中的目标类不平衡，还需要更多步骤。从头学习这一切可能有些困难。尽管网上有许多学习资源，但找到一个全面的高水平指南却不易。因此，我写这篇文章，希望能通过一个10步简单指南来提供一些过程透明度。

我将从提供一个我整理的流程图开始，其中包含了从明确任务到部署训练好的文本分类器的所有必要步骤和关键点。

首先，什么是文本分类器？

> 文本分类器是一种算法，它通过学习单词的出现或模式来预测某种目标或结果，通常是一个类别，比如判断一封邮件是否是垃圾邮件。

在这里需要提到的是，我将专注于使用监督学习方法来构建文本分类器。另一种方法是使用深度学习方法，如神经网络。

让我们来看看那个流程图。

![](../Images/cf105d0f0e197d6701c455062b55f7d4.png)

作者提供的图表

信息量很大。让我们将其分解成小块，逐步讲解每个部分。

## 1\. 明确任务

这是任何数据科学项目中最重要的步骤之一。确保你完全理解所提出的问题。你是否有相关数据来回答这个问题？你的方法是否与利益相关者的期望一致？如果需要利益相关者的认可，不要去构建一个复杂难懂的模型。从简单的开始，让所有人都参与到这个过程中来。

## 2\. 数据质量检查

任何项目中的另一个关键步骤。你的模型的好坏取决于输入的数据，因此确保删除重复数据并妥善处理缺失值。

## 3\. 探索性数据分析 (EDA)

现在我们可以进入一些文本数据特定的分析。EDA（探索性数据分析）主要是理解数据，感受从中可以得出的信息。这个步骤的一个关键点是理解**目标类分布**。你可以使用 pandas 的 .value_counts() 方法或绘制条形图来可视化数据集中每个类的分布。你将能够看到哪些是**主要类**和**次要类**。

![](../Images/1a7238087383fb8dd9dd6e49f45175eb.png)

二分类标记数据集的不平衡类分布

模型在处理不平衡数据时表现不好。模型往往会忽略少数类别，因为数据量不足以训练模型检测这些类别。不过，如果你发现自己有一个高度倾斜的目标类别的不平衡数据集，这其实并不是世界末日。这在实际上是非常正常的。只要在模型构建过程开始之前知道这一点，以便在后续可以进行调整，就很重要。

不平衡数据集的存在也应该让你思考应使用哪些指标来评估模型性能。在这种情况下，‘准确率’（正确预测的比例）真的**不是**你的朋友。假设你有一个二分类目标类的数据集，其中80%的数据标记为‘红色’，20%的数据标记为‘蓝色’。你的模型可以简单地对整个测试集预测‘红色’，并且仍然保持80%的准确率。因此，模型的准确率可能具有误导性，因为你的模型可能只会预测多数类。

一些更好的指标包括**召回率**（正确预测的真正例比例）、**精准率**（正确预测的正例比例）或这两者的平均值，即**F1分数**。在模型构建阶段，要特别关注这些指标，尤其是少数类的指标。你会希望提升这些得分。

## 4\. 文本预处理

现在进入一些有趣的内容！文本数据中可能包含大量对任何机器学习模型都不实用的内容（这取决于任务的性质）。这个过程实际上是为了去除数据集中的“噪声”，使单词同质化，并将其简化到最基本的状态，以便仅保留有用的单词和最终的特征。

通常，你会希望去除标点符号、特殊字符、停用词（如‘this’，‘the’，‘and’），并将每个单词还原到其词根或词干。你可以尝试编写自己的函数，了解数据中的内容，然后进行清洗。例如，考虑下面的函数：

```py
#  exploring patterns in the text to assess how best to cleanse the data
pat_list = [r'\d', '-', '\+', ':', '!', '\?', '\.', '\\n'] # list of special characters/punctuation to search for in data

def punc_search(df, col, pat):
    """
    function that counts the number of narratives
    that contain a pre-defined list of special
    characters and punctuation
    """
    for p in pat:
        v = df[col].str.contains(p).sum() # total n_rows that contain the pattern
        print(f'{p} special character is present in {v} entries')

punc_search(df, 'text', pat_list)

# the output will look something like this:

"""
\d special character is present in 12846 entries
- special character is present in 3141 entries
\+ special character is present in 71 entries
: special character is present in 1874 entries
! special character is present in 117 entries
\? special character is present in 53 entries
\. special character is present in 16962 entries
\n special character is present in 7567 entries
"""
```

当你对需要从数据中去除的内容有了更好的了解后，可以尝试编写一个函数，一次性完成所有操作：

```py
lemmatizer = WordNetLemmatizer()  # initiating lemmatiser object

def text_cleanse(df, col):
    """
    cleanses text by removing special
    characters and lemmatizing each
    word
    """
    df[col] = df[col].str.lower()  # convert text to lowercase
    df[col] = df[col].str.replace(r'-','', regex=True) # replace hyphens with '' to join hyphenated words together
    df[col] = df[col].str.replace(r'\d','', regex=True) # replace numbers with ''
    df[col] = df[col].str.replace(r'\\n','', regex=True) # replace new line symbol with ''
    df[col] = df[col].str.replace(r'\W','', regex=True)  # remove special characters
    df[col] = df[col].str.replace(r'\s+[a-zA-Z]\s+',' ', regex=True) # remove single characters
    df[col] = df.apply(lambda x: nltk.word_tokenize(x[col]), axis=1) # tokenise text ready for lemmatisation
    df[col] = df[col].apply(lambda x:[lemmatizer.lemmatize(word, 'v') for word in x]) # lemmatise words, use 'v' argument to lemmatise versbs (e.g. turns past participle of a verb to present tense)
    df[col] = df[col].apply(lambda x : " ".join(x)) # de-tokenise text ready for vectorisation
```

然后你可以在清洗后的数据上再次运行第一个函数，以检查所有你想要去除的内容是否确实被去除了。

对于那些注意到上述函数没有去除任何停用词的人，观察得很好。在向量化过程中的几步操作中，你可以去除停用词。

## 5\. 训练-测试拆分

这个步骤有自己的子标题，因为在开始调整特征之前执行这个步骤非常重要。使用sklearn的train_test_split()函数拆分数据，然后**保持测试数据不变**，以避免数据泄漏的风险。

如果你的数据不平衡，有一些可选参数（‘shuffle’ 和 ‘stratify’），你可以在测试训练拆分时指定，以确保目标类别之间的均匀分配。这确保了你的少数类别不会全部集中在训练集或测试集中。

```py
# create train and test data split
X_train, X_test, y_train, y_test = train_test_split(df['text'], # features
                                                    df['target'], # target
                                                    test_size=0.3, # 70% train 30% test
                                                    random_state=42, # ensures same split each time to allow repeatability
                                                    shuffle = True, # shuffles data prior to splitting
                                                    stratify = df['target']) # distribution of classes across train and test
```

## 6\. 文本向量化

模型无法解释词语。相反，词语必须通过一种称为向量化的过程转换为数字。向量化有两种方法：词袋模型和词嵌入。词袋模型寻找文本之间词语的精确匹配，而词嵌入方法考虑词语的上下文，因此可以在文本之间寻找相似的词。比较这两种方法的有趣文章可以在[这里](https://medium.com/swlh/word-embeddings-versus-bag-of-words-the-curious-case-of-recommender-systems-6ac1604d4424)找到。

对于词袋模型，句子会被分词，每个唯一的词成为一个特征。数据集中每个唯一的词将对应一个特征，每个特征会有一个整数值，这个整数值取决于该词在文本中出现的次数（词频向量 — sklearn 的 CountVectorizer()）或者一个加权整数，表示该词在文本中的重要性（TF-IDF 向量 — sklearn 的 TfidVectorizer()）。有关 TF-IDF 向量化的有用文章可以在[这里](/tf-idf-simplified-aba19d5f5530#:~:text=Term%20frequency%2Dinverse%20document%20frequency,specific%20term%20in%20a%20document.)找到。

确保在训练数据上训练向量化对象，然后使用它来转换测试数据。

## 7\. 模型选择

尝试几种分类模型，看看哪一种在你的数据上表现最好。然后，你可以使用性能指标选择最合适的模型进行优化。我通过运行一个 for 循环，使用 cross_validate() 函数对每个模型进行迭代来完成这个任务。

```py
#  defining models and associated parameters
models = [RandomForestClassifier(n_estimators = 100, max_depth=5, random_state=42), 
          LinearSVC(random_state=42),
          MultinomialNB(), 
          LogisticRegression(random_state=42)]

kf = StratifiedKFold(n_splits=5, shuffle=True, random_state=1) # With StratifiedKFold, the folds are made by preserving the percentage of samples for each class.

scoring = ['accuracy', 'f1_macro', 'recall_macro', 'precision_macro']

#  iterative loop print metrics from each model
for model in tqdm(models):
    model_name = model.__class__.__name__
    result = cross_validate(model, X_train_vector, y_train, cv=kf, scoring=scoring)
    print("%s: Mean Accuracy = %.2f%%; Mean F1-macro = %.2f%%; Mean recall-macro = %.2f%%; Mean precision-macro = %.2f%%" 
          % (model_name, 
             result['test_accuracy'].mean()*100, 
             result['test_f1_macro'].mean()*100, 
             result['test_recall_macro'].mean()*100, 
             result['test_precision_macro'].mean()*100))
```

## 8\. 基线模型

在你过于热衷于调整所选模型的超参数以提高性能指标之前，停下。记录下你模型的表现，然后再开始优化。只有通过与基线得分进行比较，你才能知道（并证明）你的模型是否有所改进。如果你需要向利益相关者展示你的方法，这也有助于你获得他们的认可和讲述故事。

创建一个空的 DataFrame，然后在每次模型迭代后，附加你选择的指标以及迭代的编号或名称，以便你可以清楚地看到模型在优化尝试中的进展。

## 9\. 模型调整 — 纠正数据不平衡

通常，微调你的模型可能涉及调整其超参数和特征工程，以提高模型的预测能力。然而，在这一部分中，我将重点关注用于减少类别不平衡影响的技术。

除了为少数类收集更多数据外，还有 5 种方法（我所知道的）可以用来解决类别不平衡问题。大多数方法是一种特征工程，目的是通过过采样少数类或欠采样多数类来平衡整体类别分布。

我们来快速看一下每种方法：

1.  **添加少数类惩罚**

分类算法有一个参数，通常称为‘class_weight’，你可以在训练模型时指定。这本质上是一个惩罚函数，如果少数类被错误分类，将给予更高的惩罚以防止误分类。你可以选择自动参数，也可以根据类别手动分配惩罚。请务必阅读你正在使用的算法的文档。

**2.** **过采样少数类**

随机过采样涉及随机复制少数类样本，并将其添加到训练数据集中以创建均匀的类分布。此方法可能导致过拟合，因为没有生成新的数据点，因此请务必检查这一点。

python 库 [imblearn](https://imbalanced-learn.org/stable/) 包含用于过采样和欠采样数据的函数。重要的是要知道，任何过采样或欠采样技术仅应用于训练数据。

如果你使用交叉验证方法来将数据拟合到模型中，你需要使用管道以确保仅对训练折进行过采样。Pipeline() 函数可以从 imblearn 库中导入。

```py
over_pipe = Pipeline([('RandomOverSample', RandomOverSampler(random_state=42)), 
                      ('LinearSVC', LinearSVC(random_state=42))])

params = {"LinearSVC__C": [0.001, 0.01, 0.1, 1, 10, 100]}

svc_oversample_cv = GridSearchCV(over_pipe, 
                                 param_grid = params, 
                                 cv=kf, 
                                 scoring='f1_macro',
                                 return_train_score=True).fit(X_train_vector, y_train)
svc_oversample_cv.best_score_  # print f1 score
```

**3\. 欠采样多数类**

与上述方法的另一种替代方案是对多数类进行欠采样，而不是对多数类进行过采样。有人可能会认为如果你拥有数据，就不值得删除数据，但这可能是一个值得尝试的选项。同样，imblearn 库中也有过采样函数可以使用。

**4\. 合成少数类的新实例**

少数类的新实例可以通过称为 SMOTE（合成少数过采样技术）的方法生成，该方法同样可以使用 imblearn 库来实现。这里有一篇很好的文章 [here](https://machinelearningmastery.com/smote-oversampling-for-imbalanced-classification/) 提供了一些实现 SMOTE 的示例。

**5\. 文本增强**

可以使用现有数据的同义词生成新数据，以增加少数类的数据点数量。方法包括同义词替换和回译（翻译成一种语言再翻译回原始语言）。[nlpaug library](https://nlpaug.readthedocs.io/en/latest/) 是一个探索这些选项的方便库。

逐步执行这些平衡处理步骤，并将得分与基准得分进行比较，将允许你查看哪种方法最适合你的数据。

## 10\. 部署训练好的分类器

现在是将经过训练的分类器推向生产环境的时候了，让它在未见过和未标记的数据上发挥魔力，前提是它已经经过测试。部署的方法取决于你的公司使用的平台，[这里](https://neptune.ai/blog/deploy-nlp-models-in-production)有一篇文章详细介绍了一些选项。

就这样！10个简单步骤，使用监督式机器学习方法在python中构建文本分类器。总结一下，我们学习了：

+   构建文本分类器所需的步骤顺序

+   检查类别分布的重要性，并理解这如何影响模型性能指标

+   文本预处理步骤

+   如何选择合适的模型并记录基准模型的性能

+   解决类别不平衡的方法

希望这对你有所帮助。请留下任何想法、评论或建议 :)
