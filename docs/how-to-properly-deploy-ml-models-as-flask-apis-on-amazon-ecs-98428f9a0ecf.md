# 如何在 Amazon ECS 上将 ML 模型正确部署为 Flask APIs

> 原文：[`towardsdatascience.com/how-to-properly-deploy-ml-models-as-flask-apis-on-amazon-ecs-98428f9a0ecf`](https://towardsdatascience.com/how-to-properly-deploy-ml-models-as-flask-apis-on-amazon-ecs-98428f9a0ecf)

## 在 Amazon ECS 上部署 XGBoost 模型以推荐完美的小狗

[](https://medium.com/@nikola.kuzmic945?source=post_page-----98428f9a0ecf--------------------------------)![Nikola Kuzmic](https://medium.com/@nikola.kuzmic945?source=post_page-----98428f9a0ecf--------------------------------)[](https://towardsdatascience.com/?source=post_page-----98428f9a0ecf--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----98428f9a0ecf--------------------------------) [Nikola Kuzmic](https://medium.com/@nikola.kuzmic945?source=post_page-----98428f9a0ecf--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----98428f9a0ecf--------------------------------) ·阅读时间 8 分钟·2023 年 3 月 16 日

--

![](img/d488dd1778ca6099a3740b2e610a8a63.png)

图片由 [Carissa Weiser](https://unsplash.com/@carissaweiser?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

随着 ChatGPT 的大获成功，AI 技术对我们生活的影响变得越来越明显。然而，除非这些出色的 ML 模型向所有人开放并且正确部署以应对高用户需求，否则它们将无法对世界产生任何积极影响。因此，能够不仅开发 AI 解决方案，还要了解如何正确部署它们是如此重要。更不用说，这项技能将使你在市场上变得更有价值，并开启有利可图的 ML 工程师职业机会。

在这篇文章中，我们将把 **XGBoost** 模型部署为 **Flask API**，并使用 **Gunicorn** 应用服务器在 Amazon Elastic Container Service 上进行部署。该模型将根据某人家的大小推荐一只腊肠犬或德国牧羊犬小狗。

![](img/dc2a92633d6bb52863eea43d0f470682.png)

图片由作者提供，来源: [1–2]

## 👉 游戏计划

1.  训练一个 XGBoost 模型

1.  构建一个简单的 Gunicorn-Flask API 进行推荐

1.  为 API 构建 Docker 镜像

1.  在 Amazon ECS 上部署 Docker 容器

完整源码 GitHub 仓库: [链接](https://github.com/kuzmicni/flask-on-ecs)🧑‍💻

```py
flask-on-ecs - repo structure
.
├── Dockerfile
├── README.md
├── myapp.py
├── requirements.txt
└── train_xgb.ipynb
```

## 在云上部署 ML 模型

我们通常需要将本地训练的 ML 模型部署到生产环境中供互联网用户使用。这种方法需要首先将 ML 模型封装成 API，然后进行 Docker 化。AWS 提供了一个名为 Elastic Container Service（ECS）的专用工具，它消除了管理计算环境（如 EC2）的麻烦，使我们可以使用名为 **Fargate** 的无服务器工具简单地部署 Docker 容器。

## 关于 Web 服务器与应用服务器的说明

在传统的 web 开发世界中，通常的做法是让 Web 服务器（如 NGINX）处理来自客户端的大量流量，并与提供动态内容的后台应用程序（API）进行交互。可以将 Web 服务器视为餐厅中的服务员，服务员接收并处理顾客的订单。类似地，Web 服务器接收并处理来自 Web 客户端（如 web 浏览器）的请求。服务员随后与厨房沟通以准备订单，并将完成的餐点送到顾客那里。同样，Web 服务器与后台应用程序沟通以处理请求，并将响应发送回 Web 客户端。设置通常如下：

![](img/3113e0d3262d2d67c619c31913ef2db8.png)

作者提供的图片

Web 服务器非常棒，因为它们能够将客户端请求分发到多个后台应用程序，从而提高性能、安全性和可扩展性。然而，在我们的案例中，由于我们将在 AWS 上部署 Flask API，因此有云原生的负载均衡器可以处理流量路由到后台 API，并且还可以强制执行 SSL 加密。因此，包含 NGINX 会显得有些多余。仅使用 Gunicorn 应用服务器对于大多数 ML 模型部署情况而言是足够的，前提是你打算使用 AWS 负载均衡器。好吧，但……

## 什么是 WSGI 和 Gunicorn？

**WSGI**（Web 服务器网关接口）只是一个约定或 **规则集**，在 web 服务器与 web 应用程序通信时需要遵循。**Gunicorn**（绿色独角兽）是一个遵循 WSGI 规则并处理客户端请求的 **应用服务器**，将请求发送到 Python Flask 应用程序。Flask 本身提供了 WSGI Werkzeug 的开发服务器用于初步开发，但如果我们要在生产中部署 API，那么我们的 Flask 应用程序可能需要同时处理多个请求，因此我们需要 Gunicorn。

## 实时 API 的典型云架构

在云中运行 API 得到了应用负载均衡器（ALBs）的极大提升，因为它们通常可以代替 NGINX 的功能并将流量路由到我们的后台应用程序。这个教程将仅专注于在 **ECS 上部署 Flask API**，我们可以在未来的帖子中讨论 ALBs。

![](img/2b2f52668a668d49376f4ea002d06a9a.png)

作者提供的图片

好了，背景介绍完了，我们开始构建和部署一些 API 吧！

## 👉 第一步：训练 XGBoost 模型

训练一个 XGBoost 模型，根据房屋面积预测 Dachshund（腊肠犬）或 German Shepherd（德国牧羊犬），并将模型保存为 pickle 文件。

要在 VS Code 中运行它，我们来创建一个单独的 Python 3.8 环境：

```py
conda create --name py38demo python=3.8 

conda activate py38demo

pip install ipykernel pandas flask gunicorn numpy xgboost scikit-learn 
```

然后重新启动 VS Code 并在 Jupyter Notebook 中 -> 选择 '**py38demo**' 作为内核。

训练并 pickle XGBoost 模型：

![](img/3dc22e313eb54d85806635d6a98169b2.png)

如您所见，我们成功训练了模型，在 300 和 600 平方英尺的房屋上进行了测试，并将 XGBoost 模型保存为 **pickle** (.pkl) 文件。

## 👉 步骤 2：构建一个简单的 Gunicorn-Flask API

让我们构建一个非常简单的 Flask API 来提供我们的 XGBoost 模型预测。我们有一个简单的助手函数，将 0/1 模型预测转换为 ‘wiener dog’/‘german shepherd’ 输出：

要运行 API，请在终端中：

```py
python myapp.py
```

在另一个终端中，测试一下发送 POST 请求：

```py
curl -X POST http://0.0.0.0:80/recms -H 'Content-Type: application/json' -d '{"area":"350"}'
```

我是如何在我的 Mac 上本地运行它的：

![](img/1f73f9a77c74818c53bcbb1581b96dd5.png)

我们的 API 工作得很好，但你可以看到我们收到一个警告，说明这是一个开发服务器：

![](img/4ea674183a38197dd0514885ed2aca3f.png)

让我们停止 API，改用 Gunicorn 生产级服务器：

```py
gunicorn --workers 4 --bind 0.0.0.0:80 myapp:flask_app_obj
```

回到我们的 VS Code：

![](img/a5f26cfb620603f0ccfbae2869d69ed3.png)

现在我们准备好将 API Docker 化了！📦

## 👉 3. 为 API 构建 Docker 镜像

以下是一个 **Dockerfile**，它使用 python3.8 基础镜像。我们需要 3.8 版本，因为我们在本地使用该版本训练了 XGBoost 模型。

> 注意：由于我在 Mac 上构建镜像，我需要指定
> 
> - -platform linux/amd64

使其与 ECS Fargate Linux 环境兼容。

这是我们构建和运行镜像的方法。

> 注意：我们将主机（即笔记本电脑）的 80 端口绑定到 Docker 容器的 80 端口：

```py
docker build --platform linux/amd64 -t flaskapi .
docker run -dp 80:80 flaskapi
```

让我们快速再测试一下：

![](img/0c387e865a0efd17425aaf980c0b1daa.png)

现在我们知道我们的 API 在 Docker 容器内运行良好，是时候将其推送到 AWS 了！🌤️

## 👉 步骤 4：在 Amazon ECS 上部署 Docker 容器

这个部分看起来可能很复杂，但实际上，如果我们将过程分解为 6 个简单步骤，就会很简单。

![](img/48d378716110eaaf9830785bc81ba3e1.png)

图片由作者提供

**i) 将 Docker 镜像推送到 ECR**

让我们创建一个名为 **demo** 的 ECR 仓库，在这里我们可以推送 Docker 镜像。

![](img/052bc48fc29df4f521e507c1ce4522c1.png)

然后我们可以使用 ECR 提供的推送命令：

```py
# autheticate
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <Your-aws-acc-no>.dkr.ecr.us-east-1.amazonaws.com

#tag the image
docker tag <Your-local-docker-image-name>:latest <Your-aws-acc-no>.dkr.ecr.us-east-1.amazonaws.com/<Your-ECR-repo-name>:latest
#push the image to ECR
docker push <Your-aws-acc-no>.dkr.ecr.us-east-1.amazonaws.com/<Your-ECR-repo-name>:latest
```

> 假设：你已经在本地计算机上配置了 AWS CLI，并设置了具有正确权限的 IAM 用户来与 ECR 进行交互。你可以在此 [链接](https://docs.aws.amazon.com/AmazonECR/latest/userguide/getting-started-cli.html) 中找到更多信息。

运行上述 3 个命令后，我们可以看到我们的镜像已经在 ECR 上 🎉

![](img/0d986a7d46f60dccfb5000753d6c4285.png)

> **复制并粘贴图像 URI** 到某处，因为我们在接下来的步骤中需要它。

**ii) 创建一个 IAM 执行角色**

我们需要创建一个执行角色，以便我们的 ECS 任务能够从 ECR 拉取镜像。我们将其命名为：**simpleRole**

![](img/baa3344183dc1d1c70b2124b4fd2f84e.png)

**iii) 创建一个安全组**

安全组是必需的，以允许互联网中的任何人向我们的 API 发送请求。在现实中，你可能希望将其限制为特定 IP 集合，但我们将其开放给所有人，并称之为：**simpleSG**

![](img/b03a4f6fdaa10334799b8dd0fc1e057b.png)

**iv) 创建一个 ECS 集群**

这一步很简单，只需几秒钟。我们称之为：**flaskCluster**

![](img/7e86f7cfaede8933a5bacb1fb73dbbc1.png)

在我们的集群正在配置的同时，让我们创建一个任务定义。

**v) 创建任务定义**

任务定义，顾名思义，是一组与要运行的镜像、要打开的端口以及我们希望分配的虚拟 CPU 和内存相关的指令。我们称之为：**demoTask**

![](img/713bffc8c5e6f1cd4fdd219169bb96af.png)

**vi) 运行任务**

让我们在**flaskCluster**上运行我们的**demoTask**，使用来自***步骤 iii)***的**simpleSG**。

![](img/f46cd2d6ea10f104037ae766e2a28ad7.png)

该测试我们部署的 API 从**公共 IP**地址开始！ 🥁

```py
curl -X POST http://<PUBLIC-IP>:80/recms -H 'Content-Type: application/json' -d '{"area":"200"}'
```

![](img/d95cf31e132277c595c0d4eb4d33366c.png)

它运行正常！ 🥳

正如你所见，我们通过向 ECS 提供的**公共 IP**发送 POST 请求，成功获得了完美的狗狗推荐。 🔥

感谢阅读，希望你发现这对入门 Flask、Gunicorn、Docker 和 ECS 有所帮助！

# 想要更多关于机器学习工程的实用文章吗？

[*免费订阅*](https://medium.com/@nikola.kuzmic945/subscribe) *以便在我发布新故事时收到通知。*

*成为 Medium 会员以阅读我和其他成千上万位作者的更多故事。你可以通过使用我的* [*推荐链接*](https://medium.com/@nikola.kuzmic945/membership) *来支持我。你不会额外支付费用，但我会获得佣金。*

## 参考资料

[1] 腊肠犬图片：[链接](https://www.wisdompanel.com/en-us/dog-breeds/dachshund)

[2] 德国牧羊犬图片：[链接](https://www.lovetoknowpets.com/dogs/german-shepherd)
