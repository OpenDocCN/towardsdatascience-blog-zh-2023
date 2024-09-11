# 结构化您的云实例启动脚本

> 原文：[https://towardsdatascience.com/structuring-your-cloud-instances-startup-scripts-2ce981825b8d?source=collection_archive---------9-----------------------#2023-11-09](https://towardsdatascience.com/structuring-your-cloud-instances-startup-scripts-2ce981825b8d?source=collection_archive---------9-----------------------#2023-11-09)

## 分辨首次启动与重启

[](https://medium.com/@teosiyang?source=post_page-----2ce981825b8d--------------------------------)[![Jake Teo](../Images/9687f43822fab69befb750a8ec58516d.png)](https://medium.com/@teosiyang?source=post_page-----2ce981825b8d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2ce981825b8d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2ce981825b8d--------------------------------) [Jake Teo](https://medium.com/@teosiyang?source=post_page-----2ce981825b8d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F52b0d82d5bf5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstructuring-your-cloud-instances-startup-scripts-2ce981825b8d&user=Jake+Teo&userId=52b0d82d5bf5&source=post_page-52b0d82d5bf5----2ce981825b8d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2ce981825b8d--------------------------------) ·7分钟阅读·2023年11月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2ce981825b8d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstructuring-your-cloud-instances-startup-scripts-2ce981825b8d&user=Jake+Teo&userId=52b0d82d5bf5&source=-----2ce981825b8d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2ce981825b8d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstructuring-your-cloud-instances-startup-scripts-2ce981825b8d&source=-----2ce981825b8d---------------------bookmark_footer-----------)

您的大多数机器学习任务通常会在初步探索阶段后被打包成镜像，并部署到本地或云服务器上。这将有助于快速迭代，以建立支持 MLOps 流水线运作的基础设施，涉及整个开发团队，包括数据科学家，以及数据、软件、云工程师等。

![](../Images/d582e642cfce4bde1f97ee0d72d95c17.png)

示例图显示了将机器学习任务典型地部署到服务器上的情况（VM = 虚拟机）。图像作者提供

启动脚本用于在云服务器实例启动时执行自动配置或其他任务。这在**AWS EC2**中被称为**用户数据**，在**Google Cloud Engine**中称为**启动脚本**，在**Azure Virtual Machine**中称为**自定义脚本扩展**。启动脚本中的内容可以是安装、元数据设置、环境变量等。其主要目的是每个实例在启动时始终配置为准备好为其内部或相邻服务中的应用程序提供服务。

与我们编写的所有脚本一样，我们应始终将其目标定为整洁、结构化和集中化，以便它们可以作为模板被重复使用。这将使您在项目中的不同实例中管理多个应用程序变得更加轻松。在接下来的几节中，我将展示您如何做到这一点。
