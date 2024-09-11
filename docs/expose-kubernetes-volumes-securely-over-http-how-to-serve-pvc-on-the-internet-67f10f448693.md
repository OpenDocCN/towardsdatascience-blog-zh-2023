# 通过 HTTP 安全地暴露 Kubernetes 卷：如何在互联网上服务 PVC

> 原文：[`towardsdatascience.com/expose-kubernetes-volumes-securely-over-http-how-to-serve-pvc-on-the-internet-67f10f448693?source=collection_archive---------10-----------------------#2023-02-08`](https://towardsdatascience.com/expose-kubernetes-volumes-securely-over-http-how-to-serve-pvc-on-the-internet-67f10f448693?source=collection_archive---------10-----------------------#2023-02-08)

## 创建 Kubernetes 清单以暴露 PersistentVolumeClaims

[](https://meysam.io/?source=post_page-----67f10f448693--------------------------------)![Meysam](https://meysam.io/?source=post_page-----67f10f448693--------------------------------)[](https://towardsdatascience.com/?source=post_page-----67f10f448693--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----67f10f448693--------------------------------) [Meysam](https://meysam.io/?source=post_page-----67f10f448693--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Feb4a2e2f734a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexpose-kubernetes-volumes-securely-over-http-how-to-serve-pvc-on-the-internet-67f10f448693&user=Meysam&userId=eb4a2e2f734a&source=post_page-eb4a2e2f734a----67f10f448693---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----67f10f448693--------------------------------) · 7 min read · 2023 年 2 月 8 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F67f10f448693&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexpose-kubernetes-volumes-securely-over-http-how-to-serve-pvc-on-the-internet-67f10f448693&user=Meysam&userId=eb4a2e2f734a&source=-----67f10f448693---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F67f10f448693&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexpose-kubernetes-volumes-securely-over-http-how-to-serve-pvc-on-the-internet-67f10f448693&source=-----67f10f448693---------------------bookmark_footer-----------)![](img/0699da88213fc3e1b6e62163670dea6f.png)

图片来源：[Uriel Soberanes](https://unsplash.com/@soberanes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/deep-ocean?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

你可能在日常的产品开发中遇到过这样一种情况，需要获取 Kubernetes 集群中某些持久化文件。一种常见且安全的方法是进行端口转发，无论是通过 Kubectl 还是使用堡垒主机进行纯 SSH。

无论哪种情况，完成任务后，你会终止会话，并且每次未来的交互中，你都需要重复相同的手动过程。

从安全角度看，理想情况下，你的环境应该尽可能密封，不给对手任何机会，这也是保持这种状态的合理原因。

但是，如果你想要长期在互联网上暴露底层存储，这篇文章就是为你准备的。

# 首先：认证

由于该文件服务器将公开暴露在互联网上，你的首要和最重要的防线是认证层。为了阐明这一点，有必要提供**认证**的正式定义。

> 认证是证明一个主张的行为，例如计算机系统用户的身份。[[source](https://en.wikipedia.org/wiki/Authentication)]

通俗来说，认证发生在系统用户证明他是他所声称的人的时候！

既然我们已经清楚了这一点，让我们深入探讨一些将认证集成到我们的 web 服务器中的选项（见下文）。

+   使用 Nginx 或 Apache 作为**代理**，借助 `htpasswd`，这是一个[Apache 工具](https://httpd.apache.org/docs/2.4/programs/htpasswd.html)，它允许在文件中存储加密的用户名-密码对，之后可以用来验证给定的密码。

+   [Ory Oathkeeper](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&cd=&cad=rja&uact=8&ved=2ahUKEwjrrMvg_vr8AhVqyXMBHesLD2YQFnoECBYQAQ&url=https%3A%2F%2Fwww.ory.sh%2Fdocs%2Foathkeeper&usg=AOvVaw3lE0kJQsgKh6i4iwjk7_dX) 作为**代理**，借助 [Kratos](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&cd=&cad=rja&uact=8&ved=2ahUKEwiEue_r_vr8AhX2ErcAHeuXAUMQFnoECBUQAQ&url=https%3A%2F%2Fwww.ory.sh%2Fkratos%2F&usg=AOvVaw0nIrUC8QOSo2f1dZW32Yhg)，这是 Ory 的另一个产品，作为**身份提供者**。这比早期的方法稍微复杂一些，需要一些学习曲线来掌握配置和这些工具的提供。我会在稍后的文章中覆盖这一点，所以请继续关注！😉

当然，你可以将更多内容添加到这个列表中，但为了保持本文简洁，老实说，因为我对其他解决方案不太了解，我暂时就以上两个项目满足要求。

另一个我要提到的点是，由于这篇文章涉及到**互联网的暴露**，我这里不谈论私人网络解决方案。不过，你可以想象这也会是一个**安全**的选项。

既然我们知道 Ory 的产品不是最容易配置的，而且作者也不是认证专家 😁，让我们[保持简单](https://en.wikipedia.org/wiki/KISS_principle)并采用第一种方法。

## 创建 `htpasswd` 文件

`htpasswd`是一个相当简单的工具，用于将[**基本**认证](https://www.rfc-editor.org/rfc/rfc7617#section-2)机制应用到任何平台。它通过接收用户名和密码作为输入来工作。结果将是一个单向哈希密码，存储在文件或标准输出中，随后可以用来**验证**用户凭证。然而，它[不能在合理的时间内被还原（解哈希）为原始密码](https://www.quora.com/Is-it-possible-to-reverse-a-password-hashed-with-bcrypt)，至少[在 2023 年，考虑到我们当前的计算能力](https://www.quora.com/How-would-quantum-computers-render-one-way-hashing-algorithms-like-SHA-256-Bitcoin-useless-Would-they-somehow-be-able-to-reverse-the-hashes-or-would-they-just-be-better-at-finding-collisions)!

要进行简单演示，请查看下面的片段。

这将仅为用户创建一个新文件，并尝试用正确的密码和错误的密码进行验证。

我们将在我们的“安全文件服务器”中使用相同的设置，公开暴露到互联网。

## 反向代理

除非你想在文件服务器层处理认证（我知道我不会），你将使用反向代理来位于前面，接收所有流量，并拒绝所有凭证错误的请求。你甚至可以添加其他限制措施，包括但不限于速率限制、日志记录、仪表化、报告等。

Apache 和 Nginx 都可以使用`htpasswd`生成的文件来验证凭证。有关每个的更多信息，请参见以下链接：

+   [`httpd.apache.org/docs/2.4/howto/auth.html`](https://httpd.apache.org/docs/2.4/howto/auth.html)

+   [`docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/`](https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/)

我相信其他 Web 服务器也可以，做同样的事情。

在这篇文章中，我将使用 Nginx，并且由于这将托管在 Kubernetes 中，它将是一个 Nginx 的 docker 容器。这允许我将任意数量的配置文件挂载到`/etc/nginx/conf.d`目录中，Nginx Web 服务器进程会拾取这些配置文件。

因此，如果我可以将任何配置文件挂载到目录中，我可以在[Kubernetes ConfigMap](https://kubernetes.io/docs/concepts/configuration/configmap/)中编写配置文件，并将其挂载到容器的目标目录。这既强大又相当灵活。

这是我即将挂载到 Nginx 容器中的配置。

配置文件中名为`proxy_pass`的条目指向将通过 HTTP 协议暴露文件系统目录的文件服务器。更多内容请见下一节。

![](img/441ddc1ea43bca0cf61688ae43b84759.png)

照片由 [Carl Barcelo](https://unsplash.com/@barcelocarl?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/gymnastic?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 通过 HTTP 提供文件

在许多其他静态 web 服务器中，只提到其中的一些。

+   [Python 模块:](https://docs.python.org/3/library/http.server.html) `[http.server](https://docs.python.org/3/library/http.server.html)`

+   [Npm:](https://www.npmjs.com/package/serve/v/14.2.0) `[serve](https://www.npmjs.com/package/serve/v/14.2.0)`

+   [Nginx 静态服务](https://docs.nginx.com/nginx/admin-guide/web-server/serving-static-content/)

当然，这个列表可以更多，但我们试图保持简短和信息丰富。😇

在本文中，我将使用 Python 的内置模块：`http.server`。它具有直接和直观的接口，使得使用非常简单。

使用它提供静态内容的方式如下：

```py
ADDRESS=0.0.0.0 PORT=8000 DIRECTORY=/tmp
python -m http.server -b $ADDRESS -d $DIRECTORY $PORT
```

这效果很好，特别是因为你不需要做很多复杂的操作来使它工作。

运行并**可访问**的 web 服务器从 Nginx 容器中意味着你可以将 PersistentVolumeClaims 挂载到静态 web 服务器上，并将上述 Nginx 放置在前面，以防止未认证的访问你的 Kubernetes 集群中的宝贵数据。

## 挂载 Kubernetes ConfigMap 为 Volume

在我们将所有内容汇总为一个统一的清单之前，还有一个最后的关键信息在这种方法中使用，需要一点解释。但如果你已经是 Kubernetes 中如何将 ConfigMap 挂载为容器 Volume 的大师，可以跳过这一部分。

要将 Kubernetes ConfigMap 挂载为 Volume，你可以在容器定义的 volumes 部分使用 projection，如下所示 [[source](https://kubernetes.io/docs/reference/kubernetes-api/config-and-storage-resources/volume/#projections)]：

在 `containers` 相同级别下，定义了 `volumes`，它可以接受几种 volume 类型，其中之一是 ConfigMap。这允许定义一些脚本并将其作为 volume 传递给运行中的容器。

创建上述清单后，以下是容器日志中显示的内容。

```py
kubectl logs job/demo -c demo
total 0      
drwxr-xr-x    2 root     root          45 Feb  4 06:58 ..2023_02_04_06_58_34.2149416564
lrwxrwxrwx    1 root     root          32 Feb  4 06:58 ..data -> ..2023_02_04_06_58_34.2149416564
lrwxrwxrwx    1 root     root          21 Feb  4 06:58 favorite-color -> ..data/favorite-color
lrwxrwxrwx    1 root     root          16 Feb  4 06:58 names.txt -> ..data/names.txt
red
Alex
Jane
Sam
```

# 结合所有内容

现在我们已经逐步了解了所有信息，是时候将它们结合在一起，作为一个统一的清单，用于一个唯一的目的：**一个 HTTP 文件服务器**。

第二个文件，一个 Ingress 资源，是可选的，但仍然包含在内，因为这篇文章是关于公开暴露 HTTP 静态网页服务器的。只有创建 Ingress 才能获得互联网曝光。

为了增加另一层安全措施，你可以为子域分配一个 UUID 生成的值，以避免仅使用用户名和密码作为唯一的安全措施。它可以是这样的：

```py
415cb00c-5310-4877-8e28-34b05cadc99d.example.com
```

否则，你的安全性仅与用户名和密码相同，如果你的品牌暴露了你分配的用户名，那么你的安全性就仅仅取决于你的密码，而这与你希望的安全状态不同！

此外，请记得你需要 HTTPS。你绝不希望有陌生人窃听你的连接，监视你通过互联网传输宝贵的客户数据。

![](img/c07e959a8e5d41359349f86ddb138797.png)

图片由 [Haley Phelps](https://unsplash.com/@haleyephelps?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/s/photos/swimming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 结论

由于我们讨论的是 Kubernetes，这与任何云服务提供商无关，你只需在任何 Kubernetes 集群中应用这个做法。

这将有效地意味着你可以安全地将动态配置的持久卷暴露到互联网上！

对于“安全”部分，需持保留态度，因为这并不是最安全的选择，但如果你给你的 Ingress 的子域名分配一个随机字符串，你仍然可以保护自己。这样，攻击者将不得不在互联网上找到多个组合中的 URL，这将需要很多年。到那时，我们可能都已经离开了！

祝你今天过得愉快。[保持关注](https://meysam.io/)，保重！

# 致谢

希望你觉得这篇文章对你有帮助。以下是一些你可能会喜欢的我以前的工作列表。

[](/how-to-set-up-ingress-controller-in-aws-eks-d745d9107307?source=post_page-----67f10f448693--------------------------------) ## 如何在 AWS EKS 中设置 Ingress Controller

### 在 AWS EKS 上以正确的方式部署 Ingress Controller

towardsdatascience.com [](https://meysam.io/what-is-haproxy-how-to-get-the-most-from-of-it-a9009b67f618?source=post_page-----67f10f448693--------------------------------) [## 什么是 HAProxy 及如何充分利用它？

### 负载均衡、SSL/TLS、缓存等等。

meysam.io](https://meysam.io/what-is-haproxy-how-to-get-the-most-from-of-it-a9009b67f618?source=post_page-----67f10f448693--------------------------------) [](/how-to-write-your-own-github-action-59cc4746a57a?source=post_page-----67f10f448693--------------------------------) ## 如何编写自己的 GitHub Action

### 用 GitHub Workflows 完善你的 CI/CD 工具箱。

towardsdatascience.com [](https://medium.com/licenseware/12-factor-app-for-dummies-d905d894d9f8?source=post_page-----67f10f448693--------------------------------) [## 12-Factor App 入门

### 12-Factor App 是一组应用于现代 Web 开发的原则和最佳实践，以使应用程序更加……

medium.com](https://medium.com/licenseware/12-factor-app-for-dummies-d905d894d9f8?source=post_page-----67f10f448693--------------------------------) [](https://medium.com/licenseware/stop-committing-configurations-to-your-source-code-fb37be351492?source=post_page-----67f10f448693--------------------------------) [## 停止将配置提交到源代码中

### 通过更改配置使应用程序在不同环境中可复现。

medium.com](https://medium.com/licenseware/stop-committing-configurations-to-your-source-code-fb37be351492?source=post_page-----67f10f448693--------------------------------)

# 参考资料

+   [`nginx.org/en/docs/`](http://nginx.org/en/docs/)

+   [`kubernetes.io/docs/`](https://kubernetes.io/docs/)
