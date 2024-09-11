# 转身面对陌生事物

> 原文：[https://towardsdatascience.com/turn-and-face-the-strange-a3a59fdb69e5?source=collection_archive---------7-----------------------#2023-08-11](https://towardsdatascience.com/turn-and-face-the-strange-a3a59fdb69e5?source=collection_archive---------7-----------------------#2023-08-11)

## 如何利用异常检测方法来改善您的监督学习

[](https://medium.com/@efowler_86027?source=post_page-----a3a59fdb69e5--------------------------------)[![Evie Fowler](../Images/74aa5bfa8932dbca98a4bde7777e03ea.png)](https://medium.com/@efowler_86027?source=post_page-----a3a59fdb69e5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a3a59fdb69e5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a3a59fdb69e5--------------------------------) [Evie Fowler](https://medium.com/@efowler_86027?source=post_page-----a3a59fdb69e5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fabec8950b7b6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fturn-and-face-the-strange-a3a59fdb69e5&user=Evie+Fowler&userId=abec8950b7b6&source=post_page-abec8950b7b6----a3a59fdb69e5---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a3a59fdb69e5--------------------------------) ·7 min read·Aug 11, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa3a59fdb69e5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fturn-and-face-the-strange-a3a59fdb69e5&user=Evie+Fowler&userId=abec8950b7b6&source=-----a3a59fdb69e5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa3a59fdb69e5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fturn-and-face-the-strange-a3a59fdb69e5&source=-----a3a59fdb69e5---------------------bookmark_footer-----------)![](../Images/b8b76ad770f0f7491e6863f3e079d911.png)

照片由 [Stefan Fluck](https://unsplash.com/@sfluck?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供于 [Unsplash](https://unsplash.com/photos/twIzCL3YSRI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

传统的预测分析提供了两种视角来查看大多数问题：点估计和分类。现代数据科学主要关注后者，将许多问题框定为分类（想想保险公司如何试图识别哪些客户会产生高成本，而不是预测每个客户的成本；或者营销人员如何更感兴趣于哪些广告会带来正的ROI，而不是预测每个广告的具体ROI）。鉴于此，数据科学家已经开发并熟悉了多种分类方法，从逻辑回归到基于树和森林的方法，再到神经网络。然而，有一个问题——许多这些方法在数据结果类别大致平衡时效果最佳，而现实世界中的应用往往无法提供这种平衡。在这篇文章中，我将展示如何使用异常检测方法来缓解监督学习中不平衡结果类别所带来的问题。

设想一下，我计划从我的家乡宾夕法尼亚州的匹兹堡出发。我对去哪里不挑剔，但我真的希望避免旅行中的问题，比如航班被取消、转机，甚至是严重延误。分类模型可以帮助我识别哪些航班可能会遇到问题，而[Kaggle](https://www.kaggle.com/datasets/robikscube/flight-delay-dataset-20182022)有一些数据可以帮助我构建这样一个模型。

我从读取数据并制定自己对糟糕航班的定义开始——任何被取消、转机或到达延迟超过30分钟的航班。

```py
import pandas as pd
import numpy as np
from sklearn.compose import make_column_transformer
from sklearn.ensemble import GradientBoostingClassifier, IsolationForest
from sklearn.metrics import accuracy_score, confusion_matrix
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import OneHotEncoder

# read in data
airlines2022 = pd.read_csv('myPath/Combined_Flights_2022.csv')
print(airlines2022.shape)
# (4078318, 61)

# subset by my target departure city
airlines2022PIT = airlines2022[airlines2022.Origin == 'PIT']
print(airlines2022PIT.shape)
# (24078, 61)

# combine cancellations, diversions, and 30+ minute delays into one Bad Flight outcome
airlines2022PIT = airlines2022PIT.assign(arrDel30 = airlines2022PIT['ArrDelayMinutes'] >= 30)
airlines2022PIT = (airlines2022PIT
                   .assign(badFlight = 1 * (airlines2022PIT.Cancelled 
                                            + airlines2022PIT.Diverted
                                            + airlines2022PIT.arrDel30))
                  )
print(airlines2022PIT.badFlight.mean())
# 0.15873411412908048
```

大约15%的航班属于我的“糟糕航班”类别。这还不够低，不能传统地认为这是一个异常检测问题，但也足够低，以至于监督方法的表现可能没有我希望的那么好。尽管如此，我会开始构建一个简单的梯度提升树模型，以预测航班是否会遇到我想要避免的问题。

首先，我需要确定在我的模型中使用哪些特征。为了这个例子，我将选择一些看起来有前景的特征进行建模；实际上，特征选择是任何数据科学项目中非常重要的一部分。这里大多数可用的特征是分类特征，需要在数据准备阶段进行编码；城市之间的距离需要进行缩放。

```py
# categorize columns by feature type
toFactor = ['Airline', 'Dest', 'Month', 'DayOfWeek'
            , 'Marketing_Airline_Network', 'Operating_Airline']
toScale = ['Distance']

# drop fields that don't look helpful for prediction
airlines2022PIT = airlines2022PIT[toFactor + toScale + ['badFlight']]
print(airlines2022PIT.shape)
# (24078, 8)

# split original training data into training and validation sets
train, test = train_test_split(airlines2022PIT
                               , test_size = 0.2
                               , random_state = 412)
print(train.shape)
# (19262, 8)
print(test.shape)
# (4816, 8)

# manually scale distance feature
mn = train.Distance.min()
rng = train.Distance.max() - train.Distance.min()
train = train.assign(Distance_sc = (train.Distance - mn) / rng)
test = test.assign(Distance_sc = (test.Distance - mn) / rng)
train.drop('Distance', axis = 1, inplace = True)
test.drop('Distance', axis = 1, inplace = True)

# make an encoder
enc = make_column_transformer(
    (OneHotEncoder(min_frequency = 0.025, handle_unknown = 'ignore'), toFactor)
    , remainder = 'passthrough'
    , sparse_threshold = 0)

# apply it to the training dataset
train_enc = enc.fit_transform(train)

# convert it back to a Pandas dataframe for ease of use
train_enc_pd = pd.DataFrame(train_enc, columns = enc.get_feature_names_out())

# encode the test set in the same way
test_enc = enc.transform(test)
test_enc_pd = pd.DataFrame(test_enc, columns = enc.get_feature_names_out())
```

树模型的开发和调优可能会单独成篇，所以我在这里不详细讨论。我使用了初始模型的特征重要性评分进行了一些反向特征选择，并从那里对模型进行了调优。最终模型在识别延误、取消或转机的航班方面表现良好。

```py
# feature selection - drop low importance terms|
lowimp = ['onehotencoder__Airline_Delta Air Lines Inc.'
          , 'onehotencoder__Dest_IAD'
          , 'onehotencoder__Operating_Airline_AA'
          , 'onehotencoder__Airline_American Airlines Inc.'
          , 'onehotencoder__Airline_Comair Inc.'
          , 'onehotencoder__Airline_Southwest Airlines Co.'
          , 'onehotencoder__Airline_Spirit Air Lines'
          , 'onehotencoder__Airline_United Air Lines Inc.'
          , 'onehotencoder__Airline_infrequent_sklearn'
          , 'onehotencoder__Dest_ATL'
          , 'onehotencoder__Dest_BOS'
          , 'onehotencoder__Dest_BWI'
          , 'onehotencoder__Dest_CLT'
          , 'onehotencoder__Dest_DCA'
          , 'onehotencoder__Dest_DEN'
          , 'onehotencoder__Dest_DFW'
          , 'onehotencoder__Dest_DTW'
          , 'onehotencoder__Dest_JFK'
          , 'onehotencoder__Dest_MDW'
          , 'onehotencoder__Dest_MSP'
          , 'onehotencoder__Dest_ORD'
          , 'onehotencoder__Dest_PHL'
          , 'onehotencoder__Dest_infrequent_sklearn'
          , 'onehotencoder__Marketing_Airline_Network_AA'
          , 'onehotencoder__Marketing_Airline_Network_DL'
          , 'onehotencoder__Marketing_Airline_Network_G4'
          , 'onehotencoder__Marketing_Airline_Network_NK'
          , 'onehotencoder__Marketing_Airline_Network_WN'
          , 'onehotencoder__Marketing_Airline_Network_infrequent_sklearn'
          , 'onehotencoder__Operating_Airline_9E'
          , 'onehotencoder__Operating_Airline_DL'
          , 'onehotencoder__Operating_Airline_NK'
          , 'onehotencoder__Operating_Airline_OH'
          , 'onehotencoder__Operating_Airline_OO'
          , 'onehotencoder__Operating_Airline_UA'
          , 'onehotencoder__Operating_Airline_WN'
          , 'onehotencoder__Operating_Airline_infrequent_sklearn']
lowimp = [x for x in lowimp if x in train_enc_pd.columns]
train_enc_pd = train_enc_pd.drop(lowimp, axis = 1)
test_enc_pd = test_enc_pd.drop(lowimp, axis = 1)

# separate potential predictors from outcome
train_x = train_enc_pd.drop('remainder__badFlight', axis = 1); train_y = train_enc_pd['remainder__badFlight']
test_x = test_enc_pd.drop('remainder__badFlight', axis = 1); test_y = test_enc_pd['remainder__badFlight']
print(train_x.shape)
print(test_x.shape)

# (19262, 25)
# (4816, 25)

# build model
gbt = GradientBoostingClassifier(learning_rate = 0.1
                                 , n_estimators = 100
                                 , subsample = 0.7
                                 , max_depth = 5
                                 , random_state = 412)

# fit it to the training data
gbt.fit(train_x, train_y)

# calculate the probability scores for each test observation
gbtPreds1Test = gbt.predict_proba(test_x)[:,1]

# use a custom threshold to convert these to binary scores
gbtThresh = np.percentile(gbtPreds1Test, 100 * (1 - obsRate))
gbtPredsCTest = 1 * (gbtPreds1Test > gbtThresh)

# check accuracy of model
acc = accuracy_score(gbtPredsCTest, test_y)
print(acc)
# 0.7742940199335548

# check lift
topDecile = test_y[gbtPreds1Test > np.percentile(gbtPreds1Test, 90)]
lift = sum(topDecile) / len(topDecile) / test_y.mean()
print(lift)
# 1.8591454794381614

# view confusion matrix
cm = (confusion_matrix(gbtPredsCTest, test_y) / len(test_y)).round(2)
print(cm)
# [[0.73 0.11]
# [0.12 0.04]]
```

但它可以更好吗？也许使用其他方法可以学习到更多关于航班模式的信息。隔离森林是一种基于树的异常检测方法。它通过从输入数据集中迭代地选择一个随机特征，以及该特征范围内的一个随机分裂点来工作。它继续以这种方式构建树，直到输入数据集中的每个观察值都被分裂成自己的叶子。其理念是，异常值或数据离群点与其他观察值不同，因此用这种挑选和分裂过程隔离它们更容易。因此，只需几轮挑选和分裂就被隔离的观察值被视为异常值，而那些无法迅速与邻近点分离的则不是。

隔离森林是一种无监督方法，因此不能用来识别数据科学家自己选择的特定异常类型（例如取消、转移或非常晚的航班）。不过，它在识别与其他观察值在某些未指定方式上不同的观察值时可能仍然有用（例如，在某些方面不同的航班）。

```py
# build an isolation forest
isf = IsolationForest(n_estimators = 800
                      , max_samples = 0.15
                      , max_features = 0.1
                      , random_state = 412)

# fit it to the same training data
isf.fit(train_x)

# calculate the anomaly score of each test observation (lower values are more anomalous)
isfPreds1Test = isf.score_samples(test_x)

# use a custom threshold to convert these to binary scores
isfThresh = np.percentile(isfPreds1Test, 100 * (obsRate / 2))
isfPredsCTest = 1 * (isfPreds1Test < isfThresh)
```

将异常分数与监督模型分数结合提供了额外的见解。

```py
# combine predictions, anomaly scores, and survival data
comb = pd.concat([pd.Series(gbtPredsCTest), pd.Series(isfPredsCTest), pd.Series(test_y)]
                 , keys = ['Prediction', 'Outlier', 'badFlight']
                 , axis = 1)
comb = comb.assign(Correct = 1 * (comb.badFlight == comb.Prediction))

print(comb.mean())
#Prediction    0.159676
#Outlier       0.079942
#badFlight     0.153239
#Correct       0.774294
#dtype: float64

# better accuracy in majority class
print(comb.groupby('badFlight').agg(accuracy = ('Correct', 'mean')))
 #          accuracy
#badFlight          
#0.0        0.862923
#1.0        0.284553

# more bad flights among outliers
print(comb.groupby('Outlier').agg(badFlightRate = ('badFlight', 'mean')))

#        badFlightRate
#Outlier               
#0             0.148951
#1             0.202597
```

这里有几点需要注意。一点是监督模型在预测“好”航班方面优于“坏”航班——这是稀有事件预测中的常见动态，因此查看精准度和召回率等指标而不仅仅是简单的准确性很重要。更有趣的是，“坏航班”率在被隔离森林分类为异常的航班中几乎高出1.5倍。尽管隔离森林是一种无监督方法，并且一般识别的是非典型航班，而不是那些在我想要避免的特定方式上非典型的航班，这似乎对监督模型来说是有价值的信息。二元异常标志已经是一个很好的格式，可以用作我的监督模型中的预测因子，因此我将其纳入并看看是否能提高模型性能。

```py
# build a second model with outlier labels as input features
isfPreds1Train = isf.score_samples(train_x)
isfPredsCTrain = 1 * (isfPreds1Train < isfThresh)

mn = isfPreds1Train.min(); rng = isfPreds1Train.max() - isfPreds1Train.min()
isfPreds1SCTrain = (isfPreds1Train - mn) / rng
isfPreds1SCTest = (isfPreds1Test - mn) / rng

train_2_x = (pd.concat([train_x, pd.Series(isfPredsCTrain)]
                       , axis = 1)
             .rename(columns = {0:'isfPreds1'}))
test_2_x = (pd.concat([test_x, pd.Series(isfPredsCTest)]
                      , axis = 1)
            .rename(columns = {0:'isfPreds1'}))

# build model
gbt2 = GradientBoostingClassifier(learning_rate = 0.1
                                  , n_estimators = 100
                                  , subsample = 0.7
                                  , max_depth = 5
                                  , random_state = 412)

# fit it to the training data
gbt2.fit(train_2_x, train_y)

# calculate the probability scores for each test observation
gbt2Preds1Test = gbt2.predict_proba(test_2_x)[:,1]

# use a custom threshold to convert these to binary scores
gbtThresh = np.percentile(gbt2Preds1Test, 100 * (1 - obsRate))
gbt2PredsCTest = 1 * (gbt2Preds1Test > gbtThresh)

# check accuracy of model
acc = accuracy_score(gbt2PredsCTest, test_y)
print(acc)
#0.7796926910299004

# check lift
topDecile = test_y[gbt2Preds1Test > np.percentile(gbt2Preds1Test, 90)]
lift = sum(topDecile) / len(topDecile) / test_y.mean()
print(lift)
#1.9138477764819217

# view confusion matrix
cm = (confusion_matrix(gbt2PredsCTest, test_y) / len(test_y)).round(2)
print(cm)
#[[0.73 0.11]
# [0.11 0.05]]
```

将异常值状态作为预测因子纳入监督模型确实可以提升其前十百分位的提升几分。看来“异常”在某种未定义的方式上与我期望的结果有足够的相关性，从而提供了预测能力。

当然，这种特殊情况的有效性是有限的。它并不适用于*所有*不平衡分类问题，如果解释性对最终产品非常重要，这种方法也不特别有帮助。不过，这种替代框架可以为各种分类问题提供有益的见解，值得一试。
