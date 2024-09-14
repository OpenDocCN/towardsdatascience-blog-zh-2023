# 如何在 5 秒钟或更短时间内使用 Docker 部署 GitLab

> 原文：[`towardsdatascience.com/how-to-deploy-gitlab-with-docker-in-5-seconds-or-less-e1f9e95c0b76`](https://towardsdatascience.com/how-to-deploy-gitlab-with-docker-in-5-seconds-or-less-e1f9e95c0b76)

## 启动生产就绪的 GitLab 实例的最快方法

[](https://medium.knulst.de/?source=post_page-----e1f9e95c0b76--------------------------------)![保罗·克努尔斯特](https://medium.knulst.de/?source=post_page-----e1f9e95c0b76--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e1f9e95c0b76--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e1f9e95c0b76--------------------------------) [保罗·克努尔斯特](https://medium.knulst.de/?source=post_page-----e1f9e95c0b76--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e1f9e95c0b76--------------------------------) ·阅读时间 3 分钟·2023 年 3 月 27 日

--

![](img/6f7b205b9c7f1725f99062be645e2e6a.png)

[由 rawpixel.com 提供的图片](https://www.freepik.com/free-photo/development-growth-progress-icon-concept_2758711.htm) 来源于 Freepik

# 介绍

**GitLab 是一个基于 Web 的 Git 仓库管理工具**，帮助团队协作编写代码。同时，它提供了一个完整的 DevOps 平台，从版本控制、代码审查、问题跟踪到 CI/CD。

GitLab 的一个关键优势是其多样性和灵活性，因为它可以在本地托管，并且可以轻松定制以满足每个团队的需求。此外，它具有广泛的功能和集成，使其成为团队的不错选择。

**使用 Docker，** 设置 GitLab 实例极其简单快速。您可以通过一条命令启动 GitLab 实例，无需手动安装和配置依赖项。

**这使它成为希望快速轻松开始使用 GitLab 的软件开发人员的绝佳选择。**

# 使用 Sameersbn Compose 文件部署 GitLab

部署 GitLab 的第一步是下载最新版本的 Compose 文件：

```py
wget https://raw.githubusercontent.com/sameersbn/docker-gitlab/master/docker-compose.yml
```

现在，生成三个至少 64 个字符长的随机字符串，打开 Compose 文件，并使用这些字符串进行：

+   `GITLAB_SECRETS_OTP_KEY_BASE`：用于加密数据库中的 2FA 密钥。如果您丢失此密钥，用户将无法使用 2FA 登录。

+   `GITLAB_SECRETS_DB_KEY_BASE`：用于加密 CI 密钥和导入凭证。如果更改或丢失，您将无法再使用 CI 密钥。

+   `GITLAB_SECRETS_SECRET_KEY_BASE`：用于生成密码重置链接和标准认证功能。如果更改或丢失，您将无法通过电子邮件重置密码。

## 启动您的 GitLab 实例

```py
docker-compose up
```

**几分钟后，GitLab 就会启动并可用。**

# 使用 Docker 命令手动部署 GitLab

与其从 Sameersbn 下载最新的 Compose 文件，不如通过三个简单的步骤手动启动 GitLab 容器、Redis 容器和 PostgreSQL 容器。

## 步骤 1：启动 PostgreSQL 容器

```py
docker run --name gitlab-postgresql -d \
    --env 'DB_NAME=gitlabhq_production' \
    --env 'DB_USER=gitlab' --env 'DB_PASS=password' \
    --env 'DB_EXTENSION=pg_trgm,btree_gist' \
    --volume ./gitlab_postgresql:/var/lib/postgresql \
    sameersbn/postgresql:12-20200524
```

## 步骤 2：启动 Redis 容器

```py
docker run --name gitlab-redis -d \
    --volume ./gitlab_redis:/var/lib/redis \
    sameersbn/redis:latest
```

## 步骤 3：启动 GitLab 容器

```py
docker run --name gitlab -d \
    --link gitlab-postgresql:postgresql --link gitlab-redis:redisio \
    --publish 10022:22 --publish 10080:80 \
    --env 'GITLAB_PORT=10080' --env 'GITLAB_SSH_PORT=10022' \
    --env 'GITLAB_SECRETS_DB_KEY_BASE=long-and-random-alpha-numeric-string' \
    --env 'GITLAB_SECRETS_SECRET_KEY_BASE=long-and-random-alpha-numeric-string' \
    --env 'GITLAB_SECRETS_OTP_KEY_BASE=long-and-random-alpha-numeric-string' \
    --volume ./gitlab_data:/home/git/data \
    sameersbn/gitlab:15.10.0
```

*注意：docker 命令使用共享卷（通过* `*./*`*）在执行 Docker run 命令的目录中创建。*

# 访问你的 GitLab 实例

在 GitLab 应用程序成功启动后（可能需要几分钟），你可以切换到浏览器，指向 `http://localhost:10080`，并为 root 用户账户设置密码。

现在，GitLab 应用程序已经准备好进行测试，**你可以开始向其中推送代码！**

# 结论

设置个人 GitLab 并不是火箭科学，**不到 3 秒**就能完成（*不包括镜像下载和启动时间*）。

虽然本教程是针对你开发机器上的个人设置，但你也可以在任何服务器上轻松完成，以便让 GitLab 实例对所有人可用。

如果这样做，我建议使用 Sameersbn Compose 文件，并在前面添加一个 Traefik 代理，以便通过公共域名而不是 IP 访问。

以下教程涵盖了设置 Traefik 代理所需的所有步骤，以便通过主机名访问 GitLab 并用 SSL 证书保护连接：

[## 如何设置带有自动 Let's Encrypt 证书解析器的 Traefik v2](https://www.paulsblog.dev/how-to-setup-traefik-with-automatic-letsencrypt-certificate-resolver/?source=post_page-----e1f9e95c0b76--------------------------------)

### 今天拥有 SSL 加密的网站非常重要。本指南将展示如何轻松实现自动化……

[www.paulsblog.dev](https://www.paulsblog.dev/how-to-setup-traefik-with-automatic-letsencrypt-certificate-resolver/?source=post_page-----e1f9e95c0b76--------------------------------)

希望这篇文章为你提供了设置个人 GitLab 实例的快速而清晰的概述。

对于本教程有任何疑问吗？我很乐意听取你的想法并回答你的问题。请在评论中分享一切。

可以通过 [我的博客](https://www.paulsblog.dev/)、[LinkedIn](https://www.linkedin.com/in/paulknulst/)、[Twitter](https://twitter.com/paulknulst) 和 [GitHub](https://github.com/paulknulst) 随时联系我。

感谢阅读，**祝编程愉快！** 🥳 👨🏻‍💻

*此帖子最初发布在我的博客上* [*https://www.paulsblog.dev/how-to-deploy-gitlab-with-docker-in-5-seconds-or-less/*](https://www.paulsblog.dev/how-to-deploy-gitlab-with-docker-in-5-seconds-or-less/)
