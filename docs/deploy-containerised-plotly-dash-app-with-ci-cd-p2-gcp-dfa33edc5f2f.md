# 部署容器化的 Plotly Dash 应用程序与 CI/CD (P2: GCP)

> 原文：[https://towardsdatascience.com/deploy-containerised-plotly-dash-app-with-ci-cd-p2-gcp-dfa33edc5f2f?source=collection_archive---------14-----------------------#2023-01-24](https://towardsdatascience.com/deploy-containerised-plotly-dash-app-with-ci-cd-p2-gcp-dfa33edc5f2f?source=collection_archive---------14-----------------------#2023-01-24)

[](https://ropdam.medium.com/?source=post_page-----dfa33edc5f2f--------------------------------)[![Robin Opdam](../Images/9dba1f1b77cb1b0a46a29a41b2b08e54.png)](https://ropdam.medium.com/?source=post_page-----dfa33edc5f2f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dfa33edc5f2f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----dfa33edc5f2f--------------------------------) [Robin Opdam](https://ropdam.medium.com/?source=post_page-----dfa33edc5f2f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F49ce97f2f8f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploy-containerised-plotly-dash-app-with-ci-cd-p2-gcp-dfa33edc5f2f&user=Robin+Opdam&userId=49ce97f2f8f7&source=post_page-49ce97f2f8f7----dfa33edc5f2f---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----dfa33edc5f2f--------------------------------) ·5分钟阅读·2023年1月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdfa33edc5f2f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploy-containerised-plotly-dash-app-with-ci-cd-p2-gcp-dfa33edc5f2f&user=Robin+Opdam&userId=49ce97f2f8f7&source=-----dfa33edc5f2f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdfa33edc5f2f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploy-containerised-plotly-dash-app-with-ci-cd-p2-gcp-dfa33edc5f2f&source=-----dfa33edc5f2f---------------------bookmark_footer-----------)![](../Images/17751f3101d4975c25a140430c2c974c.png)

照片由[Dominik Lückmann](https://unsplash.com/@exdigy?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在 Heroku 将其 dyno 改为付费层后，我希望尝试使用 Google Cloud Platform (GCP) 部署相同的容器，此前该容器已在 Heroku 上通过 CI/CD 部署，详见[第一部分](https://medium.com/towards-data-science/deploy-containerized-plotly-dash-app-to-heroku-with-ci-cd-f82ca833375c)。

![](../Images/325a346e28c1a609e1ef8ceefc70be45.png)

示例应用已通过 [GCP](https://docker-dash-example.com/) 使用 [Github Actions CI/CD Pipeline](https://github.com/ROpdam/docker-dash-example/actions) 部署，镜像由作者提供。请注意最新的操作包含了测试

**注意：** 本指南将使用 GCP 中可计费的组件，初始部署后的成本将保持在最低（大多数情况下低于 1 欧元）。我们将在最后讲解如何限制和停止费用。

关于初始设置（Github、应用和部署到 Heroku），请参阅[本指南的 P1](https://medium.com/towards-data-science/deploy-containerized-plotly-dash-app-to-heroku-with-ci-cd-f82ca833375c)。

在 P2 中，我们将深入探讨 GCP 的部署部分及所需内容，从而修改 P1 的第 5 和第 6 步：

1.  文件结构

1.  创建 Plotly Dash 应用

1.  创建 Dockerfile，本地运行

1.  使用 Github Actions 构建 Docker 镜像

1.  **创建并配置 GCP 项目**

1.  **通过 Github Actions 部署到 Google Cloud Run**

你可以在 [Github 上找到该仓库](https://github.com/ROpdam/docker-dash-example) 和在 [https://docker-dash-example.com/](https://docker-dash-example.com/) 上找到应用。

# 5\. 创建并配置 GCP 项目

在 GCP 中，一切都通过[项目](https://cloud.google.com/docs/overview#projects)来组织，你可以启用 Cloud Storage 和 Cloud Run 等服务。通过[控制台](https://console.cloud.google.com/)创建你的 Google Cloud 账户和项目，并[启用计费](https://cloud.google.com/billing/docs/how-to/modify-project)。之后，确保安装 [gcloud CLI](https://cloud.google.com/sdk/gcloud)，以便你可以通过 [gcloud 命令](https://cloud.google.com/sdk/docs/cheatsheet) 从本地机器与 GCP 交互。

按照[这个教程](https://cloud.google.com/community/tutorials/cicd-cloud-run-github-actions) 的步骤（步骤：Cloud Run）设置你的服务账户，启用所需的服务（运行、存储、IAM），并生成用于后续身份验证的 key.json。

**注意：将 key.json 保存在安全的地方，最好不要放在你的 git 仓库中。**

# 6\. 通过 Github Actions 部署到 Google Cloud Run

**6.1 推送容器**

**6.2 在管道中编写部署部分**

**6.3 限制计费和清理**

## 6.1 在 GCP 上手动部署

在自动化这个过程之前，先通过 Google Container Register (GCRe) 手动部署你的应用到 Google Cloud Run (GCRu)。按照[Google 的指南](https://cloud.google.com/container-registry/docs/pushing-and-pulling)将你的镜像推送到 GCRe，最后你将运行：

```py
docker push HOSTNAME/PROJECT-ID/IMAGE:TAG
```

注意，在使用 Apple 的 Mx 芯片时，你需要一个额外的标志来进行构建：

```py
— platform linux/amd64
```

导航到你的项目的 GCRe，查看你刚刚推送的镜像。要在 GCRu 上部署此镜像，我们可以使用以下命令（假设所有权限正常）：

```py
gcloud run deploy SERVICE --image IMAGE_URL
```

其中 SERVICE 是你的 project_id，IMAGE_URL 包含我们在之前的 docker push 命令中填写的 HOSTNAME/PROJECT-ID/IMAGE:TAG。通过访问你应用的 GCRu 页面顶部的 URL 来检查你的容器是否在 Cloud Run 中运行。

![](../Images/147e5a829e3c8e11988ac2cfefac6ceb.png)

GCRe（左）GCRu（右）包含 Docker 镜像和将在 Cloud Run 中运行的应用，作者提供的截图

## 6.2 CI/CD 在 GCP 上的部署

如果你的应用通过手动部署工作，让我们自动化这个过程，并将部署步骤纳入管道[第 1 部分](/deploy-containerized-plotly-dash-app-to-heroku-with-ci-cd-f82ca833375c)。简要回顾，第 1 部分重点介绍了通过 Github Actions 在 Heroku 上部署你的 Plotly Dash 应用。现在，Heroku 的部署被 Github 工作流中的 GCP 部署所取代。

从设置用于部署管道的[Github Secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets) 开始：

+   **GCP_EMAIL**: 你创建的服务账户的电子邮件，格式为: `$ACCOUNT_NAME@$PROJECT_ID.iam.gserviceaccount.com`

+   **GCP_PROJECT_ID**: 你的项目名称

+   **GCP_CREDENTIALS**: 你的 key.json（复制粘贴内容）

+   **GCP_APP_NAME**: 你应用的名称（在 GCRe 内部）

有了这些密钥，我们可以通过 Github Actions 构建 CI/CD 工作流。请注意，工作流的初始构建步骤保持不变，因为我们首先在 Github Packages 中推送和构建容器，请在[第 1 部分](/deploy-containerized-plotly-dash-app-to-heroku-with-ci-cd-f82ca833375c?gi=ac91abf3ba6e)中查看有关构建部分的完整说明。

**构建步骤（未更改）:**

Github Actions 构建步骤，作者提供的摘要

**部署步骤（更改为 GCP）:**

Github Actions 部署步骤，作者提供的摘要

+   （行: 1–24）开始与部署到 Heroku 相似，但在环境中我们现在定义 IMAGE_NAME 作为你镜像在 GCRe 内部的位置。我们检出 master，然后从 Github Packages 中拉取和构建镜像。

+   ‘登录到 Google Cloud’: 使用[谷歌定义的工作流动作](https://github.com/google-github-actions/auth)我们进行 Google Cloud 认证。在这里我们使用来自 Github Secrets 的 GCP_CREDENTIALS 密钥。

+   ‘配置 Docker’: 在[谷歌推送和拉取指南](https://cloud.google.com/container-registry/docs/pushing-and-pulling)中，他们还需要你设置[Docker 的认证](https://cloud.google.com/container-registry/docs/advanced-authentication#methods)。这一步骤在部署期间配置镜像的认证。

+   ‘推送到注册表’: 将你的镜像推送到 GCRe

+   ‘部署’: 使用[谷歌定义的部署到 GCRu 的工作流动作](https://github.com/google-github-actions/deploy-cloudrun)，我们像之前手动操作一样通过 Cloud Run 部署推送的容器。

+   **备注：** `— port=8080` 是我在容器中暴露的端口，这可能与你的容器不同。`— allow-unauthenticated` 允许对你的 Cloud Run 应用程序进行无有效认证令牌的请求。我使用这个标志使我的应用程序公开可访问，请根据你的应用程序需要使用这个标志。

## 6.3 限制账单和清理

为了使应用程序运行，我们启用了多个服务。查看下面描述的服务以及如何监控和限制成本。

+   [Google Cloud Storage (GCS) 成本](https://cloud.google.com/storage/pricing#europe)，GCRe 在 [GCS](https://cloud.google.com/storage) 中存储你的容器镜像。每 GB 每月约 2 美分（在欧洲），如果你保持 GCRe 的清洁，这不会花费你太多。我们可以通过添加以下行来确保 GCRe 中没有旧镜像，该行会删除没有 ‘latest’ 标签的镜像：

```py
- name: Clean up old images
  run: gcloud container images list-tags ${{ env.IMAGE_NAME }} --filter='-tags:*' --format="get(digest)" --limit=10 > tags && while read p; do gcloud container images delete "${{ env.IMAGE_NAME }}@$p" --quiet; done < tags
```

+   [Google Cloud Run 成本](https://cloud.google.com/run/pricing)，让你的应用程序正常运行需要计算能力的费用，你将为此收费。如果你的应用程序使用不广泛，那么每月的费用应该也会低于 1 或 2 美元。

为了跟踪这些成本，我们可以使用 [Google Cloud Billing](https://cloud.google.com/billing/docs#:~:text=Cloud%20Billing%20is%20a%20collection,set%20of%20Google%20Cloud%20resources.) 中心，该中心跟踪所有支出。在这里，你可以为你的帐户或更具体的项目或服务设置费用提醒。

此外，你可以通过使用 [GCP 配额](https://cloud.google.com/docs/quota#viewing_your_quota_console) 来限制 [Google 分配给你的 GCRu 的原始资源](https://cloud.google.com/run/quotas)。在这里，你可以限制 Google Cloud Run 可以使用的资源。

## 结论

这让我学到了很多关于将应用程序容器化以便迁移到另一个云平台的优势。确保监控你的 Google Billing Overview 并设置警报，如果你让应用程序公开可访问。如果你是第一次使用 GCP，你可能仍然有资格获得他们的初始 *$*300 免费额度。

## 感谢阅读！

如有任何问题或意见，请随时联系我。

很高兴在 [LinkedIn](https://www.linkedin.com/in/robinopdam/) 上与您联系！

一些 [我的其他项目](http://ropdam.github.io/)。

## 改进事项

+   在 CI/CD 流水线的构建和部署步骤之间可以（并且应该）有一个测试步骤（在 [repo](https://github.com/ROpdam/docker-dash-example) 的最新更新中完成）。

+   使用多步骤构建可以加速部署。

+   通过适当的调整，可以使 Docker 镜像变得更小，这意味着构建和部署速度更快（使用 Alpine 基础镜像，跳过 pip 缓存等）。

## 额外的尝试事项

+   [Testdriven.io FastApi Docker](https://testdriven.io/courses/tdd-fastapi/continuous-delivery/) 课程，是一个很棒的课程，我从中学到了本指南中描述的大部分概念。
