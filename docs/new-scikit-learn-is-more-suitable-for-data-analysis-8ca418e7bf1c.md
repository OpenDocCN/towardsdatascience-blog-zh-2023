# 新版 Scikit-Learn 更适合数据分析

> 原文：[`towardsdatascience.com/new-scikit-learn-is-more-suitable-for-data-analysis-8ca418e7bf1c`](https://towardsdatascience.com/new-scikit-learn-is-more-suitable-for-data-analysis-8ca418e7bf1c)

## Scikit-Learn 版本 ≥1.2.0 的 Pandas 兼容性及更多

[](https://saptashwa.medium.com/?source=post_page-----8ca418e7bf1c--------------------------------)![Saptashwa Bhattacharyya](https://saptashwa.medium.com/?source=post_page-----8ca418e7bf1c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8ca418e7bf1c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8ca418e7bf1c--------------------------------) [Saptashwa Bhattacharyya](https://saptashwa.medium.com/?source=post_page-----8ca418e7bf1c--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8ca418e7bf1c--------------------------------) ·阅读时间 5 分钟·2023 年 3 月 8 日

--

![](img/9b506c4e0906db732b941fdb09a6a595.png)

新版 Sklearn 的一些非常酷的更新！（来源：作者笔记本）

大约在去年 12 月，Scikit-Learn 发布了一个重要的 [稳定更新](https://scikit-learn.org/stable/auto_examples/release_highlights/plot_release_highlights_1_2_0.html) (v. 1.2.0–1)，我终于可以尝试一些突出的新功能。现在它与 Pandas 兼容性更好，还有一些新功能将帮助我们进行回归和分类任务。接下来，我将介绍一些新更新及其使用示例。我们开始吧！

## 与 Pandas 的兼容性：

在使用数据进行训练 ML 模型（如回归或神经网络）之前应用数据标准化是一种常见技术，以确保具有不同范围的特征在预测中获得相等的重要性（如果或当需要时）。Scikit-Learn 提供了各种预处理 API，如 `StandardScaler` 、`MaxAbsScaler` 等。随着新版本的发布，**可以在预处理后保持 Dataframe 格式**，让我们看看下面：

```py
from sklearn.datasets import load_wine
from sklearn.model_selection import train_test_split
########################
X, y = load_wine(as_frame=True, return_X_y=True) 
# available from version >=0.23; as_frame
########################
X_train, X_test, y_train, y_test = train_test_split(X, y, stratify=y, 
                                                    random_state=0)
X_train.head(3)
```

![](img/f29c45d59aed68fcb7074ef6b1f4fa2b.png)

数据集 Wine 的 Dataframe 格式

新版本包括一个选项，即使在标准化之后也能保持 Dataframe 格式：

```py
 ############
# v1.2.0
############

from sklearn.preprocessing import StandardScaler

scaler = StandardScaler().set_output(transform="pandas") 
## change here

scaler.fit(X_train)
X_test_scaled = scaler.transform(X_test)
X_test_scaled.head(3)
```

![](img/d88b2144e30e88fa9924b9fdacb565bc.png)

即使在标准化之后，Dataframe 格式仍保持不变。

之前，它会将格式转换为 Numpy 数组：

```py
###########
# v 0.24
########### 

scaler.fit(X_train)
X_test_scaled = scaler.transform(X_test)
print (type(X_test_scaled))

>>> <class 'numpy.ndarray'>
```

由于 Dataframe 格式保持不变，我们不需要像处理 Numpy 数组格式时那样关注列。分析和绘图变得更容易：

```py
 fig = plt.figure(figsize=(8, 5))
fig.add_subplot(121)
plt.scatter(X_test['proline'], X_test['hue'], 
            c=X_test['alcohol'], alpha=0.8, cmap='bwr')
clb = plt.colorbar()
plt.xlabel('Proline', fontsize=11)
plt.ylabel('Hue', fontsize=11)
fig.add_subplot(122)
plt.scatter(X_test_scaled['proline'], X_test_scaled['hue'], 
            c=X_test_scaled['alcohol'], alpha=0.8, cmap='bwr')
# pretty easy now in the newer version to see the effect

plt.xlabel('Proline (Standardized)', fontsize=11)
plt.ylabel('Hue (Standardized)', fontsize=11)
clb = plt.colorbar()
clb.ax.set_title('Alcohol', fontsize=8)
plt.tight_layout()
plt.show()
```

![](img/e312b86b52eb6baeafb0fe3bfd3554e9.png)

图 1：标准化前后特征的依赖关系！（来源：作者笔记本）

即使我们建立了一个管道，**管道中的每个转换器也可以配置为返回数据框**，如下所示：

```py
 from sklearn.pipeline import make_pipeline
from sklearn.svm import SVC

clf = make_pipeline(StandardScaler(), SVC())
clf.set_output(transform="pandas") # change here 
svm_fit = clf.fit(X_train, y_train)

print (clf[:-1]) # StandardScaler 
print ('check that set_output format indeed remains even after we build a pipleline: ', '\n')
X_test_transformed = clf[:-1].transform(X_test)

X_test_transformed.head(3)
```

![](img/bdb2208eda4f5e5b4d02e8fae995708f.png)

数据框格式即使在管道中也可以保持不变！

## 数据集获取更快、更高效：

[OpenML](https://www.openml.org/)是一个开放的数据集分享平台，而 Sklearn 中的数据集 API 提供了`fetch_openml`函数来获取数据；随着 Sklearn 的更新，这一步在内存和时间上更高效。

```py
 from sklearn.datasets import fetch_openml

start_t = time.time()
X, y = fetch_openml("titanic", version=1, as_frame=True, 
                    return_X_y=True, parser="pandas")
# # parser pandas is the addition in the version 1.2.0

X = X.select_dtypes(["number", "category"]).drop(columns=["body"])
print ('check types: ', type(X), '\n',  X.head(3))
print ('check shapes: ', X.shape)
end_t = time.time()
print ('time taken: ', end_t-start_t)
```

使用`parser='pandas'`可以显著提高运行时间和内存消耗的效率。可以通过`psutil`库轻松检查内存消耗，如下所示：

```py
print(psutil.cpu_percent())
```

## 部分依赖图：分类特征

部分依赖图之前也存在，但仅限于数值特征，现在已经扩展到分类特征。

如 Sklearn 文档中所述：

> 部分依赖图显示目标与感兴趣的一组输入特征之间的依赖关系，边际化所有其他输入特征（‘补充’特征）的值。直观上，我们可以将部分依赖性解释为目标响应与感兴趣的输入特征的函数。

使用上述的‘titanic’数据集，我们可以轻松绘制分类特征的部分依赖性：

使用上述代码块，我们可以得到如下的部分依赖图：

![](img/0dc981c467b471d286c099e503907c41.png)

图 2：分类变量的部分依赖图。（来源：作者笔记）

在 0.24 版本中，我们会遇到分类变量的值错误：

```py
>>> ValueError: could not convert string to float: ‘female’
```

## 直接绘制残差（回归模型）：

在分析分类模型的性能时，Sklearn 的度量 API 中，像`PrecisionRecallDisplay`、`RocCurveDisplay`这样的绘图例程在旧版本（0.24）中存在；在新版中，回归模型也可以进行类似的操作。下面是一个示例：

![](img/9b506c4e0906db732b941fdb09a6a595.png)

可以直接使用 Sklearn 绘制线性模型拟合及其对应的残差。（来源：作者笔记）

尽管总是可以使用 matplotlib 或 seaborn 绘制拟合线和残差，但在我们确定了最佳模型后，能够快速直接在 Sklearn 环境中检查结果是很棒的。

新版 Sklearn 中还有一些其他的改进/新增功能，但我发现这 4 个主要改进对标准数据分析特别有用。

## 参考文献：

[1] [Sklearn 版本亮点：V 1.2.0](https://scikit-learn.org/stable/auto_examples/release_highlights/plot_release_highlights_1_2_0.html)

[2] Sklearn 版本亮点: [视频](https://www.youtube.com/watch?v=5bCg8VfX2x8&feature=youtu.be&ab_channel=scikit-learn)

[3]所有图表和代码: [我的 GitHub](https://github.com/suvoooo/Machine_Learning/blob/master/SklearnV1d2/Scikit_Pandas_Output.ipynb)

***如果你对进一步的基础机器学习概念及更多内容感兴趣，可以考虑使用*** [***我的链接***](https://medium.com/@saptashwa/membership)***加入 Medium。你不会支付额外费用，但我将获得一小笔佣金。感谢大家！！***

[](https://medium.com/@saptashwa/membership?source=post_page-----8ca418e7bf1c--------------------------------) [## 使用我的推荐链接加入 Medium - Saptashwa Bhattacharyya

### 更多来自 Saptashwa（以及 Medium 上所有其他作者）的内容。你的会员费用直接支持 Saptashwa 和其他作者…

medium.com](https://medium.com/@saptashwa/membership?source=post_page-----8ca418e7bf1c--------------------------------)
