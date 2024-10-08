# 使用无服务器容器服务将机器学习模型生产化

> 原文：[`towardsdatascience.com/productionize-machine-learning-models-with-serverless-container-services-9323fdb8f60c`](https://towardsdatascience.com/productionize-machine-learning-models-with-serverless-container-services-9323fdb8f60c)

## *如何使用 Azure 容器应用程序创建无服务器容器化推理端点以用于你的机器学习模型*

[](https://medium.com/@edwin.tan?source=post_page-----9323fdb8f60c--------------------------------)![Edwin Tan](https://medium.com/@edwin.tan?source=post_page-----9323fdb8f60c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9323fdb8f60c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9323fdb8f60c--------------------------------) [Edwin Tan](https://medium.com/@edwin.tan?source=post_page-----9323fdb8f60c--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9323fdb8f60c--------------------------------) ·7 分钟阅读·2023 年 1 月 9 日

--

![](img/b3117ecde779f597268070c22634e534.png)

照片由 [Jan Canty](https://unsplash.com/@jancanty?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上。

# 介绍

无服务器容器架构是一种构建和运行容器化应用程序和服务的方法，无需管理底层基础设施。在这种架构中，容器用于打包和部署应用程序，这些容器在云服务提供商提供的完全托管环境中运行。

云服务提供商负责运行容器所需的基础设施，如硬件和操作系统。开发者无需担心设置或维护这些基础设施，可以专注于编写代码和构建应用程序。容器通常在集群中运行，云服务提供商会根据需求自动扩展容器的数量。这使得应用程序能够处理流量波动，而无需手动干预。无服务器架构可能比传统架构更具成本效益，因为用户仅需为实际使用的资源付费，而不是为可能未完全利用的固定计算能力付费。

一些无服务器容器服务的例子包括 Azure Functions、Azure Container Apps、AWS Lambda 和 Google Cloud Functions。本文将演示如何利用 Azure Container Apps，这是一个完全托管的无服务器容器服务，用于大规模构建和部署应用程序，用于生产化机器学习模型。Azure Container Apps 的常见用途包括部署 API 端点、托管后台处理应用程序、处理事件驱动的处理和运行微服务 [2]。

这些步骤将帮助我们训练并部署一个 scikit-learn 模型到 Azure Container Apps。

1.  在本地训练模型

1.  使用 FastAPI 创建推断 API

1.  对应用程序进行 Docker 化

1.  部署到 Azure Container Apps

# 0\. 设置

这是以下示例使用的设置。

**开发环境**

+   Visual Studio Code

+   Azure CLI

+   Python 3.8

+   Docker

+   Python 包：有关 `requirements.txt` 的信息，请参阅第三部分

**项目结构**

项目文件夹结构如下：

```py
FastAPI-Azure-Container-Apps
├─ fastapp
│  └─ app.py
├─ model
│  ├─ heart-disease.joblib
│  └─ train.py
├─ data
│  └─ heart-disease.csv
├─ Dockerfile
├─ requirements.txt
└─ .dockerignore
```

**数据集**

UCI 心脏病数据集 [3] 是一个公共数据集，包含关于被诊断为心脏病的患者的数据。它包括各种患者特征，如年龄、性别、血压和胆固醇水平。`1` 和 `0` 值在 `condition` 列中分别表示心脏病的存在与否。

# 1\. 在本地训练模型

文件：`train.py`

为了演示目的，我们将使用仅 5 个特征来训练一个梯度提升分类器。

```py
import pathlib
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.ensemble import GradientBoostingClassifier
from joblib import dump

print ('Load Data')
df = pd.read_csv(pathlib.Path('data/heart-disease.csv'))
y = df.pop('condition')
X = df.loc[:, ['age', 'sex', 'cp', 'trestbps', 'chol']].to_numpy()

print ('Train-Test Split')
X_train, X_test, y_train, y_test = train_test_split(X,y, test_size = 0.2)

print ('Train Model')
gbc = GradientBoostingClassifier()
gbc.fit(X_train, y_train)

print ('Save Model')
dump(gbc, pathlib.Path('model/heart-disease.joblib'))
```

使用 CLI 运行训练：

```py
python model/train.py
```

# 2\. 使用 FastAPI 创建预测端点

文件：`app.py`

```py
from fastapi import FastAPI
from pydantic import BaseModel
import numpy as np
from joblib import load
import pathlib

app = FastAPI(title = 'Heart Disesase Prediction', version = '0.1.0')
model = load(pathlib.Path('model/heart-disease.joblib'))

class ModelData(BaseModel):
    age:int=30
    sex:int=0
    cp:int=2
    trestbps:int=115
    chol:int=0

class ResponseData(BaseModel):
    prediction_result:float=0.1

@app.post('/predict', response_model = ResponseData)
def predict(data:ModelData):
    input_data = np.array([v for k,v in data.dict().items()]).reshape(1,-1)
    prediction_result = model.predict_proba(input_data)[:,-1]

    return {'prediction_result':prediction_result}
```

在 `app.py` 文件中我们

1.  定义 `app`，这是 FastAPI 的一个实例。

1.  加载训练好的模型。

1.  定义 API 接受的输入数据格式（`ModelData`）

1.  定义 API 响应格式（`ResponseData`）

1.  定义 `/predict` 路由，当对该路由发出 POST 请求时，将触发 `predict` 函数。

1.  `predict` 函数接收来自 POST 请求的输入数据，进行预测并返回患者患有心脏病的概率。

此时我们可以在本地测试 FastAPI 应用程序。`--reload` 标志有助于加速开发过程。FastAPI 会在检测到代码更改时自动重新加载，这意味着开发人员不需要手动重启 FastAPI 来测试代码更改。

```py
# CLI
uvicorn fastapp.app:app --reload
```

你将在终端上看到以下消息：

```py
INFO:     Uvicorn running on <http://127.0.0.1:8000> (Press CTRL+C to quit)
```

给定的 URL 会将我们带到 Swagger UI，我们可以在这里测试 API。

# 3\. 使用 Docker 对应用程序进行容器化

**创建一个** `**requirements.txt**` **文件**

`requirements.txt` 文件包含所有所需的 python 包。

```py
fastapi>=0.68.0,<0.69.0
pydantic>=1.8.0,<2.0.0
uvicorn>=0.15.0,<0.16.0
numpy == 1.19.5
scikit-learn==0.23.2
joblib==1.1.0
nest_asyncio == 1.5.5
```

**创建一个** `**.dockerignore**` **文件**

`.dockerignore` 文件的目的是避免复制那些不用于推断的文件，例如训练脚本和数据。

```py
data/
model/train.py
```

**创建一个** `**Dockerfile**` **文件**

```py
FROM python:3.8
WORKDIR /app
COPY . /app
RUN pip install -r requirements.txt
CMD ["uvicorn", "fastapp.app:app", "--host", "0.0.0.0", "--port", "80"]
```

这是 Dockerfile 的简要描述：

+   使用 `python:3.8` 作为基础镜像

+   创建一个名为 `/app` 的工作目录

+   将项目文件夹中的所有文件复制到工作目录，除了 `.dockerignore` 文件中列出的文件或子目录。

+   安装 `requirements.txt` 中列出的 Python 包

+   `CMD` 在启动 Docker 容器时在 80 端口运行 FastAPI 应用。与本地测试不同，在这里运行 uvicorn 时不包括 --reload 标志。虽然 reload 标志对加快开发过程有帮助，但在生产环境中并不需要。

**构建 Docker 镜像**

```py
docker build . -t heart-disease-app
```

**启动 Docker 容器**

我们将容器中 FastAPI 运行的 80 端口映射到 Docker 主机上的 8080 端口。

```py
docker run -p 8080:80 -it heart-disease-app
```

**测试应用**

此时，我们可以通过访问以下 URL 再次通过 Swagger UI 测试应用：`[`127.0.0.1:8080/docs`](http://127.0.0.1:8080/docs)`

# 4. 部署到 Azure 容器应用

在本节中，我们将把 Docker 镜像推送到 Azure 容器注册表，然后在 Azure 容器应用中部署 Docker 容器。

要将容器化应用部署到 Azure 容器应用，我们需要满足以下先决条件：

+   Azure 资源组

+   Azure 容器注册表

+   Azure 容器应用环境

以下命令将在命令行中执行。

**创建资源组**

资源组是用于支持应用的 Azure 服务的逻辑分组。我们在 `eastus` 区域创建了一个 `heartdisease_rg` 资源组。所有后续资源将分配给 `heartdisease_rg`。

```py
az group create --location eastus --name heartdisease_rg
```

**创建 Azure 容器注册表**

Azure 容器注册表（ACR）是一个用于存储和管理容器镜像的仓库集合。我们在 `heartdisease_rg` 资源组下创建了一个名为 `heartdisease` 的容器注册表，并选择了 `Basic` SKU 定价计划。

```py
az acr create --name heartdisease --resource-group heartdisease_rg --sku Basic
```

一旦容器注册表被配置，我们可以使用以下命令检查 ACR 登录服务器

```py
az acr list --resource-group heartdisease_rg
```

上述命令返回一个包含登录服务器的长字符串。请注意登录服务器的详细信息，因为我们将在下一步中使用它。

```py
...
"location": "eastus",
"loginServer": "heartdisease.azurecr.io",
"name": "heartdisease"
...
```

**标记 Docker 镜像**

要将本地 Docker 镜像推送到 ACR，Docker 镜像 `heart-disease-app` 被标记为 `{login server}/{docker image name}/{version}` 格式。

```py
docker tag heart-disease-app heartdisease.azurecr.io/heart-disease-app:v0.1.0
```

**登录 ACR**

确保在推送镜像到 ACR 之前已登录。

```py
az acr login -n heartdisease
```

**将 Docker 镜像推送到 ACR**

Docker push 是一个将本地 Docker 镜像上传到容器注册表的命令。这将使 Docker 镜像可供 Azure 容器应用使用。

```py
docker push heartdisease.azurecr.io/heart-disease-app:v0.1.0
```

成功推送 Docker 镜像后，镜像将在 ACR 的 UI 中显示。

![](img/5609d71a4f8366eebe74890a3398c64a.png)

图片由作者提供。

**创建 Azure 容器应用环境**

在创建 Azure 容器应用之前，我们定义了容器应用将运行的 `heartdiseaseenv` 环境。

```py
az containerapp env create --name heartdiseaseenv --resource-group heartdisease_rg --location eastus
```

**创建 Azure 容器应用**

在这一步中，我们创建了 `heart-disease-container-app` Azure 容器应用，使用的是前一步中创建的 `heartdiseaseenv` 环境。我们还定义了应使用的 Docker 镜像：`heartdisease.azurecr.io/heart-disease-app:v0.1.0` 和容器注册表的登录服务器：`heartdisease.azurecr.io`。`ingress` 设置为 `external`，因为此 API 旨在公开发布到互联网。

```py
az containerapp create --name heart-disease-container-app --resource-group heartdisease_rg --environment heartdiseaseenv --image heartdisease.azurecr.io/heart-disease-app:v0.1.0 --target-port 80 --ingress external --query properties.configuration.ingress.fqdn --registry-identity system --registry-server heartdisease.azurecr.io
```

如果 `az containerapp create` 命令成功，它将返回一个访问应用程序的 URL。

```py
Container app created. Access your app at <https://heart-disease-container-app.nicehill-f0509673.eastus.azurecontainerapps.io/>
```

**测试应用程序**

我们可以使用 Swagger UI、curl 或 python 请求来测试应用程序。要访问 Swagger UI，只需在给定 URL 的末尾添加 `docs`：`[`heart-disease-container-app.nicehill-f0509673.eastus.azurecontainerapps.io/docs`](https://heart-disease-container-app.nicehill-f0509673.eastus.azurecontainerapps.io/docs.)`[.](https://heart-disease-container-app.nicehill-f0509673.eastus.azurecontainerapps.io/docs.)

使用 CURL，命令如下：

```py
curl -X 'POST' \\
  '<https://heart-disease-container-app.nicehill-f0509673.eastus.azurecontainerapps.io/predict>' \\
  -H 'accept: application/json' \\
  -H 'Content-Type: application/json' \\
  -d '{
  "age": 64,
  "sex": 1,
  "cp": 3,
  "trestbps": 120,
  "chol": 267
}'
```

我们也可以通过以下方式使用 Python 的请求库向预测端点 `https://heart-disease-container-app.nicehill-f0509673.eastus.azurecontainerapps.io/predict` 发送 POST 请求：

```py
import requests
response = requests.post(url = '<https://heart-disease-container-app.nicehill-f0509673.eastus.azurecontainerapps.io/predict>', 
                        json = {"age": 64,
                                "sex": 1,
                                "cp": 3,
                                "trestbps": 120,
                                "chol": 267
                        )
print (response.json())
# {'prediction_result': 0.8298846604381431}
```

# 结论

在本文中，我们讨论了使用无服务器容器化机器学习推理端点的优势，并演示了如何使用 FastAPI 创建 API 端点，用 Docker 容器化并使用 Azure 容器应用程序部署容器化应用程序的示例。

[加入 medium](https://medium.com/@edwin.tan/membership) 阅读更多类似的文章！

# 参考文献

[1] [无服务器计算和应用 | Microsoft Azure](https://azure.microsoft.com/en-us/resources/cloud-computing-dictionary/what-is-serverless-computing/)

[2] [Azure 容器应用概述 | Microsoft Learn](https://learn.microsoft.com/en-us/azure/container-apps/overview)

[3] 心脏病数据集来自 [UCI 机器学习库](https://archive-beta.ics.uci.edu/ml/datasets/heart+disease)。根据 CC BY 4.0 许可协议授权。
