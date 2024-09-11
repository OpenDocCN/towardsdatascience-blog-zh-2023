# 机器学习系统的技术债务管理

> 原文：[https://towardsdatascience.com/managing-the-technical-debts-of-machine-learning-systems-5b85d420ab9d?source=collection_archive---------4-----------------------#2023-09-26](https://towardsdatascience.com/managing-the-technical-debts-of-machine-learning-systems-5b85d420ab9d?source=collection_archive---------4-----------------------#2023-09-26)

## 探索用于可持续减轻快速交付成本的实践（设计模式、版本控制和监控系统）——包括实现代码

[](https://medium.com/@johnleungTJ?source=post_page-----5b85d420ab9d--------------------------------)[![John Leung](../Images/ef45063e759e3450fa7f3c32b2f292c3.png)](https://medium.com/@johnleungTJ?source=post_page-----5b85d420ab9d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5b85d420ab9d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5b85d420ab9d--------------------------------) [John Leung](https://medium.com/@johnleungTJ?source=post_page-----5b85d420ab9d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6125e8835d3b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmanaging-the-technical-debts-of-machine-learning-systems-5b85d420ab9d&user=John+Leung&userId=6125e8835d3b&source=post_page-6125e8835d3b----5b85d420ab9d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5b85d420ab9d--------------------------------) ·10 分钟阅读·2023年9月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5b85d420ab9d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmanaging-the-technical-debts-of-machine-learning-systems-5b85d420ab9d&user=John+Leung&userId=6125e8835d3b&source=-----5b85d420ab9d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5b85d420ab9d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmanaging-the-technical-debts-of-machine-learning-systems-5b85d420ab9d&source=-----5b85d420ab9d---------------------bookmark_footer-----------)

随着机器学习（ML）社区的进步，开发ML项目的资源非常丰富。例如，我们可以依赖通用的Python包 [scikit-learn](https://scikit-learn.org/stable/)，该包建立在NumPy、SciPy和matplotlib之上，用于数据预处理和基本预测任务。或者，我们可以利用来自Hugging Face的[预训练模型](https://huggingface.co/models?sort=downloads)的开源集合来分析各种类型的数据集。这些工具使得当前的数据科学家能够迅速而轻松地处理标准ML任务，同时取得适度良好的模型性能。

然而，ML工具的丰富性往往使业务利益相关者甚至从业者低估了构建企业级ML系统所需的努力。尤其是在面对紧迫的项目截止日期时，团队可能会加快将系统部署到生产环境的速度，而没有充分考虑技术因素。因此，ML系统往往不能以技术上可持续和可维护的方式满足业务需求。

> 随着系统的发展和部署，技术债务会不断累积——隐含的成本拖延不解决，修复的成本会越来越高。

![](../Images/6afa6cb84b498a0bd7f7dfbbe0e3e9d4.png)

[Andrea De Santis](https://unsplash.com/@santesson89?utm_source=medium&utm_medium=referral) 拍摄于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在机器学习（ML）系统中存在多种技术债务来源。以下是一些例子。

## #1 硬性代码设计以应对不可预见的需求

为了验证ML是否能解决当前的企业挑战，许多ML项目开始时会进行 [概念验证（PoC）](https://en.wikipedia.org/wiki/Proof_of_concept)。我们最初创建了一个Jupyter Notebook或Google Colab环境来探索数据，然后开发了几个临时函数，给利益相关者制造了项目即将完成的假象。**直接从PoC构建的系统最终可能大部分由** [**胶水代码**](https://en.wikipedia.org/wiki/Glue_code) **构成**——这是一种连接特定不兼容组件的支持代码，但本身并不具备数据分析的功能。它们可能像意大利面一样，难以维护且容易出错。

![](../Images/716da1df4e65571dd6cac1d5c19b74dd.png)

[Jakob Owens](https://unsplash.com/@jakobowens1?utm_source=medium&utm_medium=referral) 拍摄于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

业务利益相关者经常提出不同的要求或希望扩大项目，例如尝试新的数据源或新的算法。因此，我们经常发现自己频繁地重新访问涵盖当前预处理管道和模型开发过程的代码库。灵活性差的代码设计可能导致对新变化的反应困难，甚至需要重写大部分代码以进行小的调整。

## #2 ML系统配置中的混乱

软件工程编程通过定义计算机执行的规则来自动化任务，确保相同输入的精确输出。软件工程师还关注每个角落情况的正确性。另一方面，ML系统通过收集和输入特征数据到模型中来自动化任务，以实现期望的目标结果。这个实验过程接受不确定性和可变性。**随着ML系统的成熟，它们通常包含多个版本的配置选项**，如具有不同特征组合的数据集和特定算法的学习设置。

![](../Images/23231cb5995aa6008d7558cf89247e5d.png)

照片来自 [Ricardo Viana](https://unsplash.com/@ricardoviana?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

ML系统中的输入特征本质上是相互关联的。考虑一种情况，如果特征A在生产环境中不再可用，你需要重新评估剩余特征的权重。然而，2个月后，特征A又变得可用。如果你没有系统地保存或甚至错误地修改了原始配置，恢复性能下降将需要额外的计算资源和时间精力。

## #3 适应不断变化的外部世界的能力有限

ML系统通常依赖于外部世界，各种隐藏因素不断演变，但未得到适当考虑和监控。

![](../Images/7b1d10792b83f7438ee96a5287fcf3c2.png)

导致模型性能潜在下降的外部因素（图像来源：作者）

+   **来自上游生产者的不稳定数据输出**：我们的ML系统的输入信号可能来自一个随着时间更新的其他机器学习模型。此外，系统可能依赖于来自物联网（IoT）设备的信号、网页抓取数据或音频到文本转换器的输出数据。如果上游生产者的这些工具的维护没有得到适当声明或实施了有缺陷的修补，它可能会意外地降低ML系统的性能。

+   [**输入数据的漂移**](/monitoring-machine-learning-models-in-production-why-and-how-13d07a5ff0c6)：以零售行业的需求预测为例。输入数据可以周期性地（例如购买行为的季节性循环）、渐进性地（如供应商商品的通货膨胀成本）和突然地（新竞争者的进入）表现出新的分布。

在接下来的部分中，我们将深入探讨构建机器学习系统的一些最佳实践，并提供示例 Python 代码以演示其实现。

想象一下你希望为城市建立一个强大的交通系统，准备应对交通高峰，因此你收集了过去两年的传感器交通数据。你的目标是预测未来六个月的交通模式（即车辆数量）。

+   训练数据集：ID、日期时间以及车辆数量

+   测试数据集：ID和日期时间

![](../Images/20275de30cd63401f3cef5a63a684bf9.png)

图片来源：[Joey Kyber](https://unsplash.com/@jtkyber1?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## #1 对于机器学习代码库使用设计模式

为了使代码设计在未来需求中更灵活和可重用，我们可以利用[设计模式](https://en.wikipedia.org/wiki/Software_design_pattern)。这些模式作为解决各种情况中常见问题的模板，使我们能够解耦代码库的不同部分。因此，这有助于提高对代码库的理解，并建立一种共同的语言，以快速沟通解决方案。

机器学习项目中的两个主要组成部分是数据和算法，这些部分可以从设计模式中受益。

+   工厂模式

这种创建模式为在运行时生成对象提供了一层抽象。在机器学习系统中，我们可以通过创建一个数据加载类（例如`CSVDataLoader`）来实现这种模式，该类处理训练/测试数据的加载、保存和返回，并保持一致的数据类型。然后我们可以声明`DataProcessor`接口，而不指定具体实现。

```py
# CSV Data loader class
class CSVDataLoader:
  def __init__(self, file_path):
    self.file_path = file_path

  def get_data(self):
    return pd.read_csv(self.file_path)

  def save_data(self, df):
    df.to_csv(f'data/transformed_{self.file_path}', index=False)

# Interface
class DataProcessor:
  def __init__(self, train_data_loader, test_data_loader):
    self.train_data_loader = train_data_loader
    self.test_data_loader = test_data_loader

  def run(self):
    # Load training and test data using data loaders
    train_df = self.train_data_loader.get_data()
    test_df = self.test_data_loader.get_data()

# Create a data processor instance
process = DataProcessor(
 train_data_loader=CSVDataLoader(file_path='train.csv'),
 test_data_loader=CSVDataLoader(file_path='test.csv')
)

# Run the data processing pipeline
process.run()
```

这种方法允许你扩展代码，而不必重新实现`DataProcessor`。例如，如果你想从 JSON 文件加载数据集，你可以简单地创建一个新的类`JSONDataLoader`作为数据加载器的实例进行声明。

+   策略模式

由于没有适用于所有机器学习问题的通用算法，我们经常在原型设计或项目增强过程中切换和试验不同的算法。我们可以应用策略模式，通过创建一个新的类`DataTransformer`进行特征工程，以及另一个类`LGBMModel`来封装使用 LightGBM 模型进行拟合和预测的策略。

```py
class DataTransformer:
  def transform_data(self, train_df, test_df):
    for idx, df in enumerate([train_df, test_df]):
      df[‘DateTime’] = pd.to_datetime(df['DateTime'])

      # Build ‘Time’ column
      df['Time'] = [date.hour * 3600 + date.minute * 60 + date.second for date in df['DateTime']]

      # Convert DateTime to Unix timestamp
      unixtime = [time.mktime(date.timetuple()) for date in df['DateTime']]
      df['DateTime'] = unixtime

      # Perform one-hot encoding on the DataFrame
      df = pd.get_dummies(df)

      if idx == 0:
        # Split training DataFrame into features (X_train) and target (y_train)
        X_train = df.drop(['Vehicles'], axis=1)
        y_train = df[['Vehicles']]
      elif idx == 1:
        # Store test DataFrame
        X_test = df

    return X_train, y_train, X_test
```

```py
class LGBMModel:
  def __init__(self, num_leaves, n_estimators):
    self.model = lgb.LGBMRegressor(
      num_leaves=num_leaves,
      n_estimators=n_estimators
    )

  def fit(self, X, y):
    self.model.fit(X, y)
    self.model.booster_.save_model('model/lgbm_model.txt')
    return self

  def predict(self, X):
    predictions = self.model.predict(X)
    return predictions
```

接口`DataProcessor`的实现和声明如下。这是一个端到端的过程，包括分别使用`train_data_loader`和`test_data_loader`加载训练和测试数据，使用`data_transformer`转换数据，并使用`model`将模型拟合到转换后的数据。因此，我们可以预测测试数据集中每条记录中的车辆数量。

```py
# Interface
class DataProcessor:
  def __init__(self, train_data_loader, test_data_loader, data_transformer, model):
    self.train_data_loader = train_data_loader
    self.test_data_loader = test_data_loader
    self.data_transformer = data_transformer
    self.model = model

  def run(self):
    # Load train and test data using data loaders
    train_df = self.train_data_loader.get_data()
    test_df = self.test_data_loader.get_data()

    # Transform the data using the data transformer
    X_train, y_train, X_test = self.data_transformer.transform_data(train_df, test_df)

    # Fit the model and make prediction
    self.model.fit(X_train, y_train)
    test_df['Vehicles'] = self.model.predict(X_test)

    # Save the transformed training data and test data
    self.train_data_loader.save_data(pd.concat([X_train, y_train], axis=1))
    self.test_data_loader.save_data(test_df)

# Create a data processor instance
process = DataProcessor(
    train_data_loader=CSVDataLoader(file_path='train.csv'),
    test_data_loader=CSVDataLoader(file_path='test.csv'),
    data_transformer=DataTransformer(),
    model=LGBMModel(num_leaves=16, n_estimators=80)
)

# Run the data processing pipeline
process.run()
```

你可以轻松地添加新的代码块来实现其他数据转换想法或算法。类似于工厂模式，这些更改不需要你修改接口`DataProcessor`。这种设计使得即使有很长的算法列表，也更容易维护代码。ML 系统的行为可以根据选择的策略动态变化。

当然，上述代码实现仅是开发的初步模板。例如，我们可以进一步通过涵盖数据验证、实施超参数调优机制以及评估模型来增强代码。

## #2 ML 系统的版本控制

在复杂的模型开发和管理过程中，我们需要适当的[版本控制](https://en.wikipedia.org/wiki/Version_control)。这使我们能够维护自己或团队成员所做的修改历史，并跟踪本地环境中相对于 ML 系统组件的数据、训练模型和超参数的版本。因此，我们可以解决一些常见问题，包括：

+   是什么更改导致了模型的失败？

+   哪些修改导致了模型性能的提升？

+   最近发布的是哪个版本的模型？

在这里，我们展示了如何利用[DVC](https://dvc.org/doc/use-cases)中的版本控制功能，它在[Git](https://git-scm.com/)库中效果最佳，用于跟踪我们的原始交通数据、转换后的交通数据和 LGBM 模型。

```py
# Initialise a Git and DVC project in the current working directory
git init
dvc init

# Capture the current state of transformed data in folder 'data' and latest LGBM model in folder 'model'
dvc add data model

# Commit the current state of 1st version
git add data.dvc model.dvc .gitignore
git commit -m “First LGBM model, with Time feature”
git tag -a “v1.0” -m “model v1.0, Time feature”
```

让我们考虑一个场景，我们在第二个版本的数据处理过程中进行了以下更改：

+   在`DataTransformer`类中添加 Weekday 特性

```py
df[‘Weekday’] = [datetime.weekday(date) for date in df.DateTime]
```

+   在`DataProcessor`接口中设置 LGBM 模型的新配置参数

```py
model=LGBMModel(num_leaves=20, n_estimators=90)
```

使用以下命令，我们可以在 DVC 中跟踪数据和模型的第二个版本，并用 Git 提交指向它们的 .dvc 文件。

```py
git add data.dvc model.dvc
git commit -m “Second LGBM model, with Time and Weekday features”
git tag -a “v2.0” -m “model v2.0, Time and Weakday features”
```

尽管工作区当前定位于数据和模型的第二个版本，但我们可以在必要时轻松地切换回并恢复到第一个快照。

```py
git checkout v1.0
dvc checkout
```

上述命令涵盖了基本操作。我们可以进一步利用该工具进行项目组织和协作。使用案例的例子包括理解数据最初是如何构建的，并比较实验中的模型指标。

## #3 持续测试和监控 ML 系统

一旦增强的模型能够生成预测，必须在将其投入生产之前执行 [合理性检查](https://en.wikipedia.org/wiki/Sanity_check)。这可以通过将一组随机的在线数据集拟合到最新模型中进行离线检查，并从不同角度审视结果来实现。

+   **确保正确的访问权限**：模型结果可以存储在目标路径中（例如，将它们写入 Hive 表）。

+   **消除** [**语义错误**](https://www.encyclopedia.com/computing/dictionaries-thesauruses-pictures-and-press-releases/semantic-error)：可视化模型拟合的变换特征的分布，以识别任何异常行为。

+   **评估模型性能**：使用最新模型重新评分，并使用适当的性能指标（例如，对于不平衡类问题，F1-score 是比准确度更好的衡量指标）比较结果。

即使最新版本的机器学习系统发布后，持续监控仍然是必要的，以应对不断变化的外部环境。

+   [**监控数据漂移和模型漂移**](https://medium.com/towards-data-science/monitoring-machine-learning-models-in-production-why-and-how-13d07a5ff0c6)：通过模型性能指标、统计测试和自适应窗口技术来检测漂移条件。

+   **跟踪上游生产者**：了解上游流程中的变化，并定期监控它们，以确保它们符合服务水平目标。

## **总结**

我们已经探索了几种有效的实践，这些实践可以用来解决在开发和部署机器学习系统时出现的技术债务。

+   使用设计模式，创建一个模块化且灵活的数据处理管道，以适应不可预见的需求。

+   利用版本控制，跟踪和管理机器学习工件，如数据和模型，确保工作流程更少混乱。

+   测试和监控机器学习系统，以便及时顺利地处理动态外部世界中的变化。

## 离开之前

如果你喜欢这篇阅读，我邀请你关注我的 [Medium 页面](https://medium.com/@johnleungTJ) 和 [Linkedin 页面](https://www.linkedin.com/in/john-leung-639800115/)。这样，你可以及时获得有关数据科学侧项目、机器学习运营（MLOps）演示以及项目管理方法的精彩内容。

[](/monitoring-machine-learning-models-in-production-why-and-how-13d07a5ff0c6?source=post_page-----5b85d420ab9d--------------------------------) [## 生产环境中的机器学习模型监控：为何与如何？

### 我们的模型在不断变化的世界中如何受到影响？一个集中于漂移示例的分析，并实现基于 Python 的…

[towardsdatascience.com](/monitoring-machine-learning-models-in-production-why-and-how-13d07a5ff0c6?source=post_page-----5b85d420ab9d--------------------------------) [](/optimizing-your-strategies-with-approaches-beyond-a-b-testing-bf11508f8930?source=post_page-----5b85d420ab9d--------------------------------) [## 利用超越 A/B 测试的方法优化策略

### 对经典 A/B 测试的深入解释：Epsilon-greedy、Thompson Sampling、Contextual…

[towardsdatascience.com](/optimizing-your-strategies-with-approaches-beyond-a-b-testing-bf11508f8930?source=post_page-----5b85d420ab9d--------------------------------)
