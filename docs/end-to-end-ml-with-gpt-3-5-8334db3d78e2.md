# 使用 GPT-3.5 进行端到端的机器学习

> 原文：[https://towardsdatascience.com/end-to-end-ml-with-gpt-3-5-8334db3d78e2?source=collection_archive---------2-----------------------#2023-05-24](https://towardsdatascience.com/end-to-end-ml-with-gpt-3-5-8334db3d78e2?source=collection_archive---------2-----------------------#2023-05-24)

![](../Images/9ec4de24c2c445ccedaec0940db9619a.png)

插图由我和 Midjourney 生成

## 了解如何使用 GPT-3.5 来处理数据采集、预处理、模型训练和部署的繁重任务

[](https://georgealexadam.medium.com/?source=post_page-----8334db3d78e2--------------------------------)[![亚历克斯·亚当](../Images/a2c18f61e6ed2bd1e6955ffb6232c9be.png)](https://georgealexadam.medium.com/?source=post_page-----8334db3d78e2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8334db3d78e2--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8334db3d78e2--------------------------------) [亚历克斯·亚当](https://georgealexadam.medium.com/?source=post_page-----8334db3d78e2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F29fd9b1a62ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fend-to-end-ml-with-gpt-3-5-8334db3d78e2&user=Alex+Adam&userId=29fd9b1a62ae&source=post_page-29fd9b1a62ae----8334db3d78e2---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----8334db3d78e2--------------------------------) ·14 分钟阅读·2023年5月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8334db3d78e2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fend-to-end-ml-with-gpt-3-5-8334db3d78e2&user=Alex+Adam&userId=29fd9b1a62ae&source=-----8334db3d78e2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8334db3d78e2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fend-to-end-ml-with-gpt-3-5-8334db3d78e2&source=-----8334db3d78e2---------------------bookmark_footer-----------)

在任何机器学习应用程序的模型开发阶段，都存在大量重复的样板代码。像 [PyTorch Lightning](https://lightning.ai/pytorch-lightning) 这样的流行库已被创建，用以标准化训练/评估神经网络时执行的操作，从而使代码更加简洁。然而，样板代码的范围远超训练循环。即使是机器学习项目的数据获取阶段也充满了必要但耗时的步骤。应对这一挑战的一种方式是创建一个类似于 PyTorch Lightning 的库，涵盖整个模型开发过程。它必须足够通用，以适用于各种模型类型，并能够集成各种数据源。

提取数据、预处理、模型训练和部署的代码示例在互联网上随处可见，尽管收集这些代码并将其集成到项目中需要时间。由于这些代码在互联网上，可能已经被大型语言模型（LLM）训练过，并且可以通过自然语言命令以各种有用的方式重新排列。本篇文章的目标是展示如何通过使用 OpenAI 的 GPT-3.5 API 来自动化许多常见的 ML 项目的步骤。我会展示一些失败案例，并在可能的情况下如何调整提示以修复错误。从头开始，甚至没有数据集，我们将最终得到一个可以在 AWS SageMaker 上部署的模型。如果你跟着做，请确保按照以下步骤设置 OpenAI API：

```py
import openai
openai.api_key = "YOUR KEY HERE"
```

此外，以下实用函数对调用 GPT-3.5 API 很有帮助：

```py
def get_api_result(prompt):
    request = openai.ChatCompletion.create(
        model="gpt-3.5-turbo-0301",
        messages=[{"role": "user", "content": prompt}]
    )

    result = request['choices'][0]['message']['content']

    print(result)
```

# 提取、转换、加载（ETL）

![](../Images/2665dfe67e5e8e270ff34942dadb03b9.png)

ETL 由我 + Midjourney 进行说明

本节经过简化，仅考虑了单一数据源，但原则上可以扩展到数据来自多个来源的情况（csv 文件、数据库、图像等）。第一步是 **提取** 一些数据。为了简单起见，我将使用 [Income Prediction](http://archive.ics.uci.edu/ml/datasets/adult)¹ 数据集，其目标是预测一个人是否年收入超过或低于 50k 美元，基于他们的教育、职位、行业等。下面的函数将用于生成下载我们数据的代码。注意提示模板是如何设计的，以使 API 生成 Python 代码：

```py
def extract(prompt):
    prompt_template = """You are a ChatGPT language model that can generate Python code. Please provide a natural language input text, and I will generate the corresponding Python code.\nInput: {}\nPython code:""".format(prompt)

    get_api_result(prompt_template)
```

在这里，我们要求 GPT-3.5：

> 使用 sklearn fetch_openml 函数从 openml 检索成人收入预测数据集。确保将数据作为单个数据框检索，其中包含一个名为“target”的列。将结果数据框命名为“df”。

这会给我们输出：

```py
from sklearn.datasets import fetch_openml

adult_income = fetch_openml(name='adult', version=2)
df = pd.DataFrame(adult_income.data, columns=adult_income.feature_names)
df['target'] = adult_income.target
```

这段代码没有错误，完全达到了我们想要的效果。如果我使用更简单的提示，去掉提及 openml 和用于检索的函数，我们会得到：

```py
import pandas as pd

df = pd.read_csv("adult_income_prediction.csv")
df.columns = ["age", "workclass", "fnlwgt", "education", "education-num",
              "marital-status", "occupation", "relationship", "race",
              "sex", "capital-gain", "capital-loss", "hours-per-week",
              "native-country", "target"]
```

这假设数据在本地可用。有趣的是，结果中有正确的列名列表，即使我们没有在API调用中包含它们！这些名称在网络上随处可见，特别是在[这篇](https://example.org/exploring-the-census-income-dataset-using-bubble-plot-cfa1b366313b) Medium文章中，目标列除外，它是由GPT添加的。下一步是**转换**数据为机器学习模型可以使用的格式。由于GPT需要了解列名及其类型，我们这里需要使用一个更高级的提示模板：

```py
def transform(columns, column_types, prompt):
    prompt_template = """You are a ChatGPT language model that can generate Python code. Please provide a natural language input text, and I will generate the corresponding Python code using the Pandas to preprocess the DataFrame. The DataFrame columns are {} and their corresponding dtypes are {}.\nInput: {}\nPython code:""".format(columns, column_types, prompt)

    get_api_result(prompt_template)
```

我接着做如下请求：

> 通过将所有类别列转换为其独热编码等效项，并标准化数值列来预处理数据框。删除任何列中有NA或NaN值的行。删除数值列的异常值行，这些异常值由其z分数确定。数值列的异常值是超出1到99分位范围的值。数值列应使用sklearn中的StandardScaler进行标准化。目标列中的值应转换为0或1，并应为int类型。

我们现在得到：

```py
import pandas as pd
from sklearn.preprocessing import StandardScaler

# Load the DataFrame
df = pd.read_csv('data.csv')

# Convert categorical columns to one-hot encoded equivalents
df = pd.get_dummies(df, columns=['workclass', 'education', 'marital-status', 'occupation', 'relationship', 'race', 'sex', 'native-country'])

# Normalize numerical columns
scaler = StandardScaler()
numerical_cols = ['age', 'fnlwgt', 'education-num', 'capital-gain', 'capital-loss', 'hours-per-week']
df[numerical_cols] = scaler.fit_transform(df[numerical_cols])

# Drop rows with NA or NaN values
df.dropna(inplace=True)

# Drop rows with numeric column outliers
z_scores = df[numerical_cols].apply(lambda x: (x - x.mean()) / x.std())
df = df[(z_scores > -3).all(axis=1) & (z_scores < 3).all(axis=1)]

# Convert target column to 0 or 1 and change type to int
df['target'] = df['target'].apply(lambda x: 1 if x == '>50K' else 0).astype(int)
```

如果我们排除尝试从csv文件加载数据的部分，这几乎是完美的。即使我们明确告诉GPT假设`df`已经存在且不需要加载，也很难说服它排除这一点。最后，我们需要**加载**数据到本地数据库中。虽然对于如此简单的用例来说这有些多余，但这是一个值得养成的好习惯。

```py
def load(prompt):
    prompt_template = """You are a ChatGPT language model that can generate Python code. Please provide a natural language input text, and I will generate the corresponding Python code.\nInput: {}\nPython code:""".format(prompt)

    get_api_result(prompt_template)
```

我使用以下指令：

> 连接到名为“data”的sqlite数据库。使用pandas将名为“df”的DataFrame中的数据插入到名为“income”的表中。不要包括索引列。提交更改后再关闭连接。

以获得：

```py
import sqlite3
import pandas as pd

# Create a connection to the database
connection = sqlite3.connect("data.db")

# Define the DataFrame
df = pd.DataFrame()

# Define the table name
table_name = "income"

# Insert the data into the table
df.to_sql(table_name, connection, if_exists="replace", index=False)

# Commit the changes to the database
connection.commit()

# Close the connection
connection.close()
```

这已经很完美，只是多了不必要的`df`创建。有了处理后的数据，我们可以准备训练一些模型。

# 模型训练

![](../Images/433e1a6b80dba4d54d04b33977243886.png)

我和Midjourney绘制的损失函数插图

在资源允许的情况下，尝试几种不同的模型类型以找出适合给定任务的复杂度水平是个好主意。因此，我们让GPT-3.5尝试几种不同的模型。首先，我们设置通用的模型训练提示模板：

```py
def train(prompt):
    prompt_template = """You are a ChatGPT language model that can generate Python code. Focus on using scikit-learn when applicable. Please provide a natural language input text, and I will generate the corresponding Python code.\nInput: {}\nPython code:""".format(prompt)

    get_api_result(prompt_template)
```

我们将使用的提示是：

> 训练各种分类模型来预测“目标”列，使用所有其他列。使用 5 折交叉验证来选择最佳模型和相应的超参数，并返回最佳的整体模型及相应的超参数设置。根据准确性选择最佳模型。假设存在一个名为“df”的数据框用于训练。使用 MLFlow 记录整个过程。在训练任何模型之前用 mlflow 开始记录，以便只存储一个运行记录。确保模型使用 mlflow 的 sklearn 模块记录。确保只记录最佳的整体模型，但记录所有模型类型的指标。以下指标在所有交叉验证折中的均值应记录：准确性、AUC、F1 分数

让我们看看生成的输出，这次深入了解为什么需要提供某些特定的指令。

```py
import pandas as pd
from sklearn.model_selection import GridSearchCV, cross_validate
from sklearn.metrics import accuracy_score, roc_auc_score, f1_score
from sklearn.linear_model import LogisticRegression
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier
import mlflow
import mlflow.sklearn

# load the dataframe
df = pd.read_csv('path/to/dataframe.csv')

# define the features and target columns
features = df.columns.drop('target')
target = 'target'

# set up mlflow
mlflow.set_tracking_uri('localhost:5000') # or set up your own server
mlflow.set_experiment("my_experiment")

# set up cross-validation
cv = 5

# define the models and corresponding hyperparameters to be tuned
models = [
    {
        'name': 'Logistic Regression',
        'estimator': LogisticRegression(),
        'hyperparameters': {
            'penalty': ['l1', 'l2'],
            'C': [0.1, 1, 10]
        }
    },
    {
        'name': 'Decision Tree',
        'estimator': DecisionTreeClassifier(),
        'hyperparameters': {
            'max_depth': [2, 4, 6, 8]
        }
    },
    {
        'name': 'Random Forest',
        'estimator': RandomForestClassifier(),
        'hyperparameters': {
            'n_estimators': [10, 25, 50, 100],
            'max_depth': [None, 2, 4, 6, 8]
        }
    }
]

# iterate through the models and corresponding hyperparameters to train and tune them
with mlflow.start_run():
    best_model = None
    best_metrics = {}

    for model in models:
        print(f"Training {model['name']} model...")

        # set up grid search for hyperparameter tuning
        gs = GridSearchCV(model['estimator'], model['hyperparameters'], cv=cv, verbose=2, n_jobs=-1, scoring='accuracy')
        gs.fit(df[features], df[target])

        # log metrics for all model types
        metrics = cross_validate(gs.best_estimator_, df[features], df[target], cv=cv, n_jobs=-1,
                                 scoring=["accuracy", "roc_auc", "f1"])
        for metric, values in metrics.items():
            mlflow.log_metric(f"{model['name']}_{metric}", values.mean())

        # check if this is the best model based on accuracy
        if best_model is None or gs.best_score_ > best_metrics['accuracy']:
            best_model = gs.best_estimator_
            best_metrics = {'accuracy': gs.best_score_,
                            'AUC': roc_auc_score(df[target], best_model.predict_proba(df[features])[:, 1]),
                            'F1': f1_score(df[target], best_model.predict(df[features]))
                           }

    # register the best model in mlflow
    mlflow.sklearn.log_model(sk_model=best_model, artifact_path="best_model")

    # log the best model's metrics
    for metric, value in best_metrics.items():
        mlflow.log_metric(f"best_model_{metric}", value)
```

如果我们移除 `df` 的加载和 `# set up mlflow` 部分，我们最终会得到完全期望的结果。即，循环遍历 3 种不同的模型类型，使用 5 折交叉验证进行网格搜索以识别给定模型类型的最佳超参数，同时跟踪指标。如果不指定“根据准确性选择最佳模型”，生成的代码将使用 `scoring=[“accuracy”, “roc_auc", “f1”]` 进行网格搜索，这将无法工作，因为如何根据多个指标选择最佳模型存在歧义。如果没有“确保模型使用 mlflow 的 sklearn 模块记录”，我们有时会得到 `mlflow.log_model()`，这是错误的。此外，“确保只记录最佳的整体模型”是必要的，以避免存储所有模型。总体而言，这个输出是可以接受的，但不稳定，多次运行可能会引入不同的错误。为了使一切为**服务**步骤做好准备，保存模型时添加模型签名是有用的。这个签名基本上是特征名称及其相应类型的集合。让 GPT-3.5 添加这个很麻烦，因此需要先添加导入的一些手动操作：

```py
from mlflow.models.signature import infer_signature
```

然后修改通过以下代码行记录模型：

```py
mlflow.sklearn.log_model(sk_model=best_model, artifact_path="best_model", signature=infer_signature(df[features], best_model.predict(df[features])))
```

# 模型服务

![](../Images/00f90a37907f52b68f7916a92f02cccb.png)

由我 + Midjourney 展示的部署示例

由于我们使用了 MLflow 来记录最佳模型，我们有几种选项来服务该模型。最简单的选项是将模型托管在本地。首先设计用于模型服务的一般提示模板：

```py
def serve_model(model_path, prompt):
    prompt_template = """You are a ChatGPT language model that can generate shell code for deploying models using MLFlow. Please provide a natural language input text, and I will generate the corresponding command to deploy the model. The model is located in the file {}.\nInput: {}\nShell command:""".format(model_path, prompt)

    get_api_result(prompt_template)
```

提示将是：

> 使用端口号 1111 来服务模型，并使用本地环境管理器

通过调用 `serve_model("<model path here>", question)` 我们得到：

```py
mlflow models serve -m <model path here> -p 1111 --no-conda
```

一旦在 shell 中运行该命令，我们就可以通过发送编码为 JSON 的数据到模型来进行预测。我们将首先生成发送数据到模型的命令，然后创建要插入命令中的 JSON 有效负载。

```py
def send_request(prompt):
    prompt_template = """You are a ChatGPT language model that can generate code for sending data to deployed MLFlow models. Please provide a natural language input text, and I will generate the corresponding command. \nInput: {}\nCommand:""".format(prompt)

    get_api_result(prompt_template)
```

以下请求将插入到`send_request()`的提示模板中：

> 使用“curl”命令将数据“<data here>”发送到本地主机上端口1111的mlflow模型。确保内容类型是“application/json”。

GPT-3.5生成的输出是：

```py
curl -X POST -H "Content-Type: application/json" -d '<data here>' http://localhost:1111/invocations
```

最好将URL放在`curl`命令后面，而不是命令的最后，即：

```py
curl http://localhost:1111/invocations -X POST -H "Content-Type: application/json" -d '<data here>'
```

让GPT-3.5完成这个任务并不容易。以下两个请求都未能成功：

> 使用“curl”命令将数据“<data here>”发送到本地主机上端口1111的mlflow模型。将URL放在“curl”后面。确保内容类型是“application/json”。
> 
> 使用“curl”命令，将URL放在任何参数之前，将数据“<data here>”发送到本地主机上端口1111的mlflow模型。确保内容类型是“application/json”。

如果我们让GPT-3.5修改一个现有的命令，而不是从头生成一个，可能会得到所需的输出。以下是修改命令的通用模板：

```py
def modify_request(prompt):
    prompt_template = """You are a ChatGPT language model that can modify commands for sending data using "curl". Please provide a natural language instruction, corresponding command, and I will generate the modified command. \nInput: {}\nCommand:""".format(prompt)

    get_api_result(prompt_template)
```

我们将按如下方式调用此函数：

```py
code = """curl -X POST -H "Content-Type: application/json" -d '<data here>' http://localhost:1111/invocations"""
prompt = """Please modify the following by placing the url before the "-X POST" argument:\n{}""".format(code)
modify_request(prompt)
```

最终给我们的结果是：

```py
curl http://localhost:1111/invocations -X POST -H "Content-Type: application/json" -d '<data here>'
```

现在是创建有效负载的时候：

```py
def create_payload(prompt):
    prompt_template = """You are a ChatGPT language model that can generate code for sending data to deployed MLFlow models. Please provide a natural language input text, and I will generate the corresponding command. \nInput: {}\nPython code:""".format(prompt)

    get_api_result(prompt_template)
```

为了获得所需的输出格式，这部分提示需要进行相当多的调整：

> 将“df”数据框转换为可以被部署的MLFlow模型接收的json格式。将结果json包裹在一个名为“dataframe_split”的对象中。生成的字符串不应有换行符，也不应转义引号。此外，“dataframe_split”应该用双引号而不是单引号括起来。不要包含“target”列。使用分割的“orient”参数。

如果没有明确指示避免换行符和转义引号，则调用`json.dumps()`生成的格式不是MLflow端点期望的格式。生成的命令是：

```py
json_data = df.drop("target", axis=1).to_json(orient="split", double_precision=15)
wrapped_data = f'{{"dataframe_split":{json_data}}}'
```

在用`wrapped_data`替换`curl`请求中的`<data here>`之前，我们可能只想发送几行数据进行预测，否则生成的有效负载可能过大。因此，我们将上述内容修改为：

```py
json_data = df[:5].drop("target", axis=1).to_json(orient="split", double_precision=15)
wrapped_data = f'{{"dataframe_split":{json_data}}}'
```

调用模型得到的结果是：

```py
{"predictions": [0, 0, 0, 1, 0]}
```

而实际的目标是[0, 0, 1, 1, 0]。

就这样了。在这篇文章的开头，我们甚至没有访问数据集，但我们最终成功部署了一个通过交叉验证选定的最佳模型。重要的是，GPT-3.5完成了所有繁重的工作，只需提供最少的辅助。然而，我确实需要指定使用的特定库和调用的方法，但这主要是为了消除模糊性。如果我指定“记录整个过程”而不是“使用MLFlow记录整个过程”，GPT-3.5将有太多库可供选择，生成的模型格式可能不适合使用MLFlow提供服务。因此，成功使用GPT-3.5需要对ML流程中执行各种步骤的工具有一些了解，但相较于从头编写代码所需的知识，这种了解是最小的。

另一个服务模型的选项是将其作为 SageMaker 端点托管在 AWS 上。尽管这在 MLflow 的 [网站](https://mlflow.org/docs/latest/models.html#deploy-a-python-function-model-on-amazon-sagemaker) 上看起来很简单，但我向你保证，就像很多涉及 AWS 的网页示例一样，事情会出错。首先，必须安装 Docker，以便使用以下命令生成 Docker 镜像：

```py
mlflow sagemaker build-and-push-container
```

其次，用于与 AWS 通信的 Python 库 `boto3` 也需要安装。此外，必须正确设置权限，以便 SageMaker、ECR 和 S3 服务能够代表你的账户进行通信。以下是我最终不得不用的命令：

```py
mlflow deployments run-local -t sagemaker -m <model path> --name income_classifier
mlflow deployments create -t sagemaker --name income_classifier -m model/ --config image_url=<docker image url> --config bucket=mlflow-serving --config region_name=us-east-1
```

以及在幕后进行一些手动调整，以确保 S3 存储桶位于正确的区域。

在 GPT-3.5 的帮助下，我们以（大部分）顺利的方式完成了 ML 流程，尽管最后的步骤有些棘手。注意我没有使用 GPT-3.5 生成在 AWS 上服务模型的命令。它在这个用例中效果较差，并且会生成虚构的参数名称。我只能推测，切换到 GPT-4.0 API 可能有助于解决一些上述的错误，并带来更轻松的模型开发体验。

尽管 ML 流程可以通过 LLMs 完全自动化，但目前还不安全让非专家负责这个过程。上述代码中的错误容易被识别，因为 Python 解释器会抛出错误，但还有更微妙的错误可能会造成伤害。例如，预处理代码中删除异常值可能会导致过多或不足的样本被丢弃。最糟糕的情况下，它可能会无意中丢弃整个子群体，加剧潜在的公平性问题。

此外，超参数的网格搜索可能是在选择不当的范围内进行的，这可能导致过拟合或欠拟合，具体取决于范围。对于没有多少 ML 经验的人来说，这将很难识别，因为代码看起来没有问题，但需要理解这些模型中正则化的工作原理。因此，目前还不适合让未专门从事 ML 的软件工程师代替 ML 工程师，但这一时刻正在快速到来。

[1] Dua, D. 和 Graff, C. (2019)。UCI 机器学习库 [http://archive.ics.uci.edu/ml]。加利福尼亚州欧文市：加州大学信息与计算机科学学院。（CC BY 4.0）
