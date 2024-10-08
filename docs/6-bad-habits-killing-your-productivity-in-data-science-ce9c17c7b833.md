# 6 个毁灭你数据科学生产力的坏习惯

> 原文：[`towardsdatascience.com/6-bad-habits-killing-your-productivity-in-data-science-ce9c17c7b833`](https://towardsdatascience.com/6-bad-habits-killing-your-productivity-in-data-science-ce9c17c7b833)

## 以及如何取得成功

[](https://donatoriccio.medium.com/?source=post_page-----ce9c17c7b833--------------------------------)![Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----ce9c17c7b833--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ce9c17c7b833--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ce9c17c7b833--------------------------------) [Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----ce9c17c7b833--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----ce9c17c7b833--------------------------------) ·阅读时间 8 分钟·2023 年 10 月 3 日

--

![](img/d046eb0051c8bcc86dfea9d2c7c0c412.png)

图片由作者提供。（AI 生成）

学习数据科学就像学习演奏乐器一样——你必须养成好习惯并打好基础才能成功。

就像音乐家需要练习音阶、琶音和节奏练习才能演奏协奏曲一样，数据科学家需要固化关键实践以开发他们的潜力。

避免有害习惯并培养生产性习惯可以让你将注意力从工作机制转移到工作的艺术性上。

养成使用虚拟环境和跟踪实验等数据科学习惯，可以将你的工作流程从挣扎转变为顺畅的创造过程。

在这篇文章中，我们将深入探讨六个可能会秘密破坏你作为数据科学家效率的日常坏习惯，并提供提升生产力的建议。

# 使用系统解释器

虚拟环境是与系统环境隔离的 Python 安装，它允许你为特定项目安装包和库而不影响系统的 Python 设置。不使用虚拟环境可能会导致*依赖地狱*。

比如，在我的第一个数据科学项目中，我为图像分类构建了一个机器学习模型。我全局安装了 TensorFlow 2.0 以开始工作。几周后，我的同事给了我一些需要 TensorFlow 1.x 的代码。安装这个版本导致了与我第一个项目依赖的各种冲突！我花了几个小时调试，才意识到应该使用虚拟环境来避免这种混乱。直到我设置了一个匹配我同事原始设置的虚拟环境，我才让继承的代码正常工作。

![](img/6c46ac60d70c4ec99d73e8149eb5992d.png)

虚拟环境可以拥有不同的 Python 版本。图片由作者提供（AI 生成元素）

虚拟环境巧妙地避免了这个问题，为每个项目提供了自己的沙盒空间。每个环境都有一个专用的 Python 解释器、pip 和库。

你可以安全地安装库，知道它不会影响其他工作流。不再担心破坏你的 Python 安装！

工具如 **Anaconda** 和 **virtualenv** 使得创建和管理虚拟环境变得轻而易举。每个新项目花费 1 分钟激活一个环境，避免了后续数小时的挫折。这是一个能带来巨大时间节省的习惯。

要使用 conda 创建虚拟环境，请使用以下命令。

`conda create -n env_name python=3.x`

然后，激活环境。

`conda activate env_name`

这两行代码将为你节省数小时的时间，并让你摆脱依赖地狱。

# 过度依赖笔记本

Jupyter 笔记本在数据科学中因其直观的工作流和将代码、可视化和文本交织在一起的能力而备受喜爱。

对于协作、可重复性和项目结构来说，将所有工作都放在笔记本中有相当大的缺点。

笔记本难以进行版本控制，且缺乏测试框架和其他环境中存在的 linting 功能。并行编辑很快会导致合并冲突。

也很容易将笔记本用作草稿纸，这会导致文件杂乱无章。

![](img/fe987ae2845f372feb6222f3fff03c51.png)

由 [Cookiecutter Data Science.](https://drivendata.github.io/cookiecutter-data-science/) 提供的组织良好的数据科学项目结构

一旦通过了初步探索和原型设计，将你的工作流转移到 `.py` 脚本和 `.py` 模块，以便从软件工程最佳实践中受益。

将笔记本代码转移到组织良好的函数和文件中。

使用笔记本来展示发现、建立叙事和分享可重复的结果。

结合笔记本和脚本可以让你兼得两者的优点。

# 使用鼠标过多

鼠标看起来很直观，但过度点击和菜单搜索浪费了宝贵的时间。键盘提供了更高效的方式来导航和操作你的工作空间。

你可以使用键盘来：

+   直接跳转到特定的行号，而不是滚动。

+   选择并同时编辑多行，而不是逐行编辑。

+   注释和取消注释代码块。

+   替换多个单词的出现。

+   直接导航到函数和变量。

+   立即格式化混乱的代码，而不是手动修复缩进和间距。

+   通过一步重命名变量和函数来安全地重构代码，跨文件进行操作。

![](img/a6f985df7222b9e7e97824c9b1250f4b.png)

我们的 Llama 正在尝试弄清楚如何使用键盘快捷键。图片由作者提供（AI 生成）

键盘快捷键可以加速你在代码编辑器中进行的几乎所有日常操作。

忘记鼠标，将手放在键盘上，以便更快地编写代码。

# 跳过数据版本控制

我在构建文本分类模型时，想要尝试不同的预处理方法。我创建了几个变体，但当我找到表现最佳的版本时，我意识到我忘记了使用的确切步骤，因此无法重现结果。

这非常令人沮丧。

代码可以使用 Git 轻松进行版本控制。但是，数据在版本控制最佳实践方面常常被忽视。这在实验和可重复性方面是一个巨大的机会损失。

对数据进行版本控制可以跟踪数据集随时间的变化。你可以自由实验，创建多种变体，以探索数据变化如何改进模型，并且知道如果需要的话，可以快速恢复。

存储元数据（如数据集描述、预处理步骤和预期用途）非常重要，以防数据衰退。记录每个数据版本所代表的内容，以防止可重复性问题。

> 版本控制让你可以自信地迭代增强数据集，快速探索建模思路，同时知道如果出现问题，你有退路。

我最喜欢的工具是数据版本控制（DVC），因为它易于设置，并且可以连接到不同的云存储服务，包括 Google Drive。

# 未跟踪实验

当我开始学习数据科学时，我是手动跟踪所有内容的。

从特征到模型架构的每一个细节都散落在不整洁的电子表格中，文件名不一致。

这导致了巨大的混乱，我发现很难重现结果。我浪费了时间手动添加参数，而实验工具可以轻松地自动化这个过程。

引入实验跟踪。机器学习实验涉及许多移动部分——数据样本、特征工程代码、模型配置、性能指标等。一个实验是一组导致特定结果的参数配置，通常在训练运行期间进行跟踪。

![](img/e788219498ec9c845975131e46a2777a.png)

MLFlow 中的实验。[来源。](https://mlflow.org/docs/latest/tutorials-and-examples/tutorial.html#comparing-the-models)

MLFlow、Comet 和 W&B 提供了方便的方式来记录你的机器学习工作流中的所有部分——代码版本、数据集、参数和指标。这些系统以有组织、可搜索的结构捕捉端到端的流程。

回顾你的实验跟踪报告，以识别值得追求的成功和从失败中得到的宝贵经验。

基于有效的经验进行构建，而不是每次都重新发明轮子。

# 未使用代码助手

成为 AI 编码的早期采用者，以最大化你作为数据科学家的生产力。能够快速将想法转化为代码，让你在实现数据科学项目时占据优势。

GitHub Copilot 是一个 AI 工具，它在你编码时在编辑器内建议整行代码和函数。它通过实时建议上下文相关的代码片段来帮助你更快地编写代码。Copilot 通过减少在模板代码和调试上的时间来提高你的生产力。

微软、GitHub 和 MIT 研究人员的[近期研究](https://arxiv.org/pdf/2302.06590.pdf)调查了 AI 工具对编程效率的影响。他们设计了一个实验，利用 Copilot 测试有无辅助的开发者在编码任务上的完成时间。

从自由职业平台抽取参与者，他们将 95 名志愿者分为处理组和对照组。处理组进行了 Copilot 演示，而两组都编写了一个 HTTP 服务器。通过测试成功率和完成时间来衡量表现。令人震惊的是，Copilot 组平均完成速度比对照组快超过 55%，仅需 71 分钟对比对照组的 161 分钟！

![](img/156fc5cad732498ea1775287ebd2641a.png)

任务完成时间。Copilot 组（处理组）与对照组。[来源](https://arxiv.org/pdf/2302.06590.pdf)。

然而，我关于将 AI 工具集成到你的工作流程中的建议不应导致盲目复制粘贴 AI 生成的代码。你不想在不理解你的代码在做什么和为什么的情况下使用它们。

此外，依赖 AI 可能对代码质量产生的影响仍然存在疑问。仔细评估这些工具，以确保它们适当地支持——而非取代——人类的问题解决和创造力。

# 设置数据科学项目

让我们一起设置一个结构化的项目，以便在 Titanic 数据集上进行二分类项目，使用 MLFlow 和 DVC。

首先，创建一个项目和 Git 仓库：

```py
mkdir titanic-project  
cd titanic-project
git init
```

创建一个 Conda 环境并安装一些库。

```py
conda create -n titanic python=3.11
conda activate titanic
pip install pandas sklearn jupyter mlflow
```

组织项目结构：

```py
mkdir data models notebooks figures src
cd data
mkdir raw processed
cd ../src 
mkdir features models visualization
```

在`src/models/train_model.py`中，使用 MLflow 记录参数和指标。这是一个非常简化的示例，应该能给你一些启示。它跟踪一个具有`n_estimators`和交叉验证准确率的随机森林模型。

```py
import mlflow
import mlflow.sklearn
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import cross_val_score, StratifiedKFold

def train_random_forest(n_estimators):
    # Load data
    ...

    # Model training
    rf = RandomForestClassifier(n_estimators=n_estimators)
    scores = cross_val_score(rf, X, y, cv=StratifiedKFold(5))

    return rf, scores

if __name__ == "__main__":
    n_estimators = 100
    with mlflow.start_run():
        rf, scores = train_random_forest(n_estimators)

        # Log parameter, metrics, and model to MLflow
        mlflow.log_param("n_estimators", n_estimators)
        mlflow.log_metric("cv_accuracy", scores.mean())
        mlflow.sklearn.log_model(rf, "model")
```

在`src/features/build_features.py`中，使用 DVC 来版本控制数据：

```py
# Feature Engineering function
def feature_engineering(df):
    # Placeholder function, add your feature engineering steps here
    return df

# Apply feature engineering
titanic_transformed = feature_engineering(titanic)

# Save transformed dataset
titanic_transformed.to_csv('../../data/processed/titanic_transformed.csv', index=False)

# Track with DVC
os.system("dvc add titanic.csv")
os.system("dvc add titanic_transformed.csv")
os.system("dvc commit -m 'Ran feature engineering'")
```

现在你的数据版本与模型参数和指标一起进行跟踪。每次更改内容时，请记得提交到 git 和 DVC。

模块化代码、MLflow 和 DVC 提供了端到端的实验和可重复性。版本控制数据和模型允许你迭代地改进工作流程。

一旦整理好，你应该会得到如下效果：

```py
├── data
│   ├── raw
│   └── processed
├── models
├── notebooks
│   ├── eda.ipynb
├── figures
└── src
    ├── features
    │   └── build_features.py
    ├── models
    │   ├── train_model.py
    │   └── predict_model.py
    └── visualization
        └── visualize.py
```

源代码和输出被分开，以便于跟踪和版本控制。

这只是一个概念，你应该根据自己的需求进行自定义。

## 生产力的新高度

数据科学的生产力不仅仅依赖于技术知识，还依赖于养成良好的习惯。避免常见的陷阱，如忽视虚拟环境、过度使用笔记本以及未能跟踪实验。相反，应培养以下习惯：利用键盘快捷键、对数据进行版本控制、系统地跟踪实验，并将重复性工作交给人工智能。

***喜欢这篇文章吗？通过订阅我的新闻通讯，每周将数据科学面试问题送到你的收件箱，*** [***数据面试***](https://thedatainterview.substack.com/)***。***

***此外，你也可以在*** [***LinkedIn***](https://www.linkedin.com/in/driccio/)***上找到我。***
