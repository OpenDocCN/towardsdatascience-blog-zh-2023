# 如何通过 Python 访问 Amazon S3 资源（及其必要性）

> 原文：[`towardsdatascience.com/how-you-can-and-why-you-should-access-amazon-s3-resources-with-python-86462c6a8f97`](https://towardsdatascience.com/how-you-can-and-why-you-should-access-amazon-s3-resources-with-python-86462c6a8f97)

## 使用自动化将数据移动到云端和从云端移出

[](https://medium.com/@aashishnair?source=post_page-----86462c6a8f97--------------------------------)![Aashish Nair](https://medium.com/@aashishnair?source=post_page-----86462c6a8f97--------------------------------)[](https://towardsdatascience.com/?source=post_page-----86462c6a8f97--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----86462c6a8f97--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----86462c6a8f97--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----86462c6a8f97--------------------------------) ·阅读时间 7 分钟·2023 年 1 月 24 日

--

![](img/206b4eb369a3a357dd723f699da2f412.png)

[Lia Trevarthen](https://unsplash.com/@melodi2?utm_source=medium&utm_medium=referral) 拍摄的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

亚马逊简单存储服务（S3）为用户提供了廉价、安全且易于管理的存储基础设施。虽然可以通过 AWS 控制台直接移动文件进出 S3 桶，但 AWS 也提供了通过代码简化这些操作的选项。

对于 Python，AWS 软件开发工具包（SDK）提供了 boto3，允许用户通过代码创建、配置和利用 S3 桶及对象。在这里，我们将深入探讨基本的 boto3 功能，并考虑如何利用它们来自动化操作。

## 为什么选择 Boto3？

你可能会想，既然可以通过 AWS 控制台访问 S3 服务，学习使用另一个工具是否还有意义。的确，借助 AWS 简单且用户友好的用户界面（UI），将文件移动到 S3 桶以及从 S3 桶中移出很容易。

然而，当操作需要扩展时，典型的点击和拖放方法就不可行了。处理 1-10 个文件是一回事，但处理 100 或 1000 个文件呢？如果手动操作，这样的任务自然会耗时。

此外，手动任务还使用户容易出错。当手动移动大量数据时，你能保证不会错误地遗漏或包括错误的文件吗？

那些重视效率或一致性的人无疑会看到使用 Python 脚本自动化 S3 资源的优点。

## 前提条件

让我们从使用 boto3 的前提条件开始。

**1\. 安装**

要使用 boto3，你需要使用以下命令安装 AWS SDK（如果尚未安装）。

```py
pip install boto3
```

**2\. IAM 用户**

您还需要一个具有使用 S3 资源权限的 IAM 用户账户。

要获取用户身份，请使用 root 用户账户登录 AWS。前往身份与访问管理（IAM）部分，添加一个新用户。为该用户身份分配一个将授予 S3 资源访问权限的策略。最简单的选项是选择“AmazonS3FullAccess”权限策略，但您可以找到或创建更符合您需求的策略。

![](img/7d15f42d09f3e2e9133f7af27e76d55e.png)

AmazonS3FullAccess 策略（作者创建）

选择策略后，完成其余提示并创建用户身份。您应该能够在控制台中看到您新创建的身份。在此示例中，我们使用名为“s3_demo”的用户身份。

![](img/b6e864a43d0120b55d948d9ad02a1079.png)

接下来，点击用户名（而不是复选框），然后转到安全凭证选项卡。在访问密钥部分，选择“创建访问密钥”并回答所有提示。然后，您将收到您的访问密钥和秘密访问密钥。

![](img/e810b4460031006880daf76ad3ee6e65.png)

这些密钥是使用 boto3 访问 S3 资源所必需的，因此它们将自然地被纳入 Python 脚本中。

## 基本命令

Boto3 包含可以配置和管理各种 AWS 资源的函数，但本文的重点将放在处理 S3 桶和对象上。boto3 的关键好处是，它可以用简单的一行代码来执行如上传和下载文件等任务。

## 创建客户端

要使用 boto3（或任何 AWS 资源）访问 S3 资源，首先需要创建一个低级服务客户端。

创建客户端需要输入服务名称、区域名称、访问密钥和秘密访问密钥。

## 创建桶

让我们开始创建一个名为“a-random-bucket-2023”的桶。这可以通过`create_bucket`函数实现。

如果您刷新 AWS 控制台上的 S3 部分，您应该能看到新创建的桶。

![](img/4e540bf3df69b26e50fdd3d2b04c5a69.png)

创建桶（作者创建）

## 列出桶

要列出可用的桶，用户可以使用`list_buckets`函数。这会返回一个包含许多键值对的字典。要查看仅桶的名称，请检索字典中‘Buckets’键的值。

![](img/126bcdf1b4db7ff7737dd3f80b7d1623.png)

代码输出（作者创建）

## 将文件上传到桶

可以使用`upload_file`函数上传文件。在这种情况下，我们将“mock_data.xlsx”上传到桶中。

值得区分`Filename`和`Key`参数之间的差异。`Filename`指的是正在传输的文件的名称，而`Key`指的是分配给存储在桶中的对象的名称。

由于“First Mock Data.xlsx”被分配给了`Key`参数，所以这将是上传到桶中时对象的名称。

![](img/49b5fd4d6469d067446f2b440030d7a4.png)

添加对象（由作者创建）

## 将 Pandas 数据框上传到存储桶

由于我们正在使用 Python，因此值得了解如何直接将 Pandas 数据框上传到存储桶。这可以通过 io 模块实现。在以下代码片段中，我们将数据框“df”上传到存储桶。

![](img/5418fe0dc2eb05393e62a53aa96016b5.png)

上传 Pandas 数据框（由作者创建）

## 列出对象

可以使用 `list_objects` 函数列出给定存储桶中的对象。

函数本身的输出是一个包含大量元数据的大字典，但可以通过访问“Contents”键找到对象名称。

![](img/98160b3dcea31cf20ed03b1abbb42d66.png)

代码输出（由作者创建）

## 下载文件

可以使用 `download_file` 函数从存储桶中下载对象。

## 删除对象

可以使用 `deleted_object` 函数删除对象。

![](img/9442266dab87360ca06c05172d0d0922.png)

删除对象（由作者创建）

## 删除存储桶

最后，用户可以使用 `delete_bucket` 函数删除存储桶。

![](img/b3bd6671a5b85e13ccb3eb24213deceb.png)

删除存储桶（由作者创建）

## 案例研究

我们已经探索了一些用于使用 S3 资源的基本 boto3 函数。然而，进行案例研究是展示 boto3 为什么是如此强大工具的最佳方式。

**问题陈述 1：** 我们对不同日期出版的书籍感兴趣。使用 [NYT Books API](https://developer.nytimes.com/docs/books-product/1/overview) 获取出版书籍的数据并将其上传到 S3 存储桶。

我们再次开始创建一个低级服务客户端。

接下来，我们创建一个存储所有这些文件的存储桶。

![](img/50b284e8ca13c3c91068df58efad9fcc.png)

创建存储桶（由作者创建）

接下来，我们需要使用 NYT Books API 拉取数据并将其上传到存储桶。我们可以使用以下 `get_data` 函数进行指定日期的数据拉取：

以下是 `get_data` 函数输出的预览：

![](img/556947ff5f22c2473a6798db4f04c458.png)

代码输出（由作者创建）

有许多方法可以利用这个函数进行数据收集。一个选项是每天运行这个函数并将输出上传到 S3 存储桶（或使用作业调度程序）。另一个选项是使用循环收集多个日期出版的书籍数据。

如果我们对过去 7 天内出版的书籍感兴趣，可以使用循环逐天解析。对于每一天，我们：

+   使用 API 进行调用

+   将响应存储到数据框中

+   将数据框上传到存储桶

这些步骤可以通过以下代码片段执行：

![](img/a56a0a59a3d6ce5d3225c51cfa247cc2.png)

上传文件到存储桶（由作者创建）

只需几行代码，我们就能进行多次 API 调用，并将响应一次性上传到存储桶！

在某些时候，可能需要从上传到桶中的数据中提取一些见解。数据分析师通常会收到有关他们收集的数据的临时请求。

**问题陈述 2：** 查找所有日期排名最高的书籍的书名、作者和出版社，并将结果存储到本地。

使用 Python，我们可以将多个 S3 对象存储到 Pandas 数据框中，处理这些数据框，并将输出加载到平面文件中。

使用 boto3 有两个主要优点，这在这个案例研究中得到了展示。第一个优点是它具有良好的可扩展性；上传/下载 1 个文件和 1000 个文件所需的时间和精力差异可以忽略不计。第二个优点是，它允许用户在将数据移动到或从 S3 桶时，无缝地结合其他过程（例如数据收集、过滤）。

## 结论

![](img/dd4343dd7b4376960806899faac2061b.png)

图片由[Prateek Katyal](https://unsplash.com/fr/@prateekkatyal?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

希望这份简要的 boto3 入门指南不仅向你介绍了管理 S3 资源的基本 Python 命令，还展示了它们如何用于自动化过程。

使用 AWS SDK for Python，用户将能够更高效、一致地在云端移动数据。即使你现在对使用 AWS 的 UI 进行资源配置和利用感到满意，你也无疑会遇到可扩展性优先的情况。了解 boto3 的基础知识将确保你为这些情况做好充分准备。

感谢阅读！
