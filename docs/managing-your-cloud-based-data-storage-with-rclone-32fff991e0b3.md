# 使用 Rclone 管理你的云数据存储

> 原文：[`towardsdatascience.com/managing-your-cloud-based-data-storage-with-rclone-32fff991e0b3?source=collection_archive---------10-----------------------#2023-11-22`](https://towardsdatascience.com/managing-your-cloud-based-data-storage-with-rclone-32fff991e0b3?source=collection_archive---------10-----------------------#2023-11-22)

## 如何优化多个对象存储系统之间的数据传输

[](https://chaimrand.medium.com/?source=post_page-----32fff991e0b3--------------------------------)![Chaim Rand](https://chaimrand.medium.com/?source=post_page-----32fff991e0b3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----32fff991e0b3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----32fff991e0b3--------------------------------) [Chaim Rand](https://chaimrand.medium.com/?source=post_page-----32fff991e0b3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9440b37e27fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmanaging-your-cloud-based-data-storage-with-rclone-32fff991e0b3&user=Chaim+Rand&userId=9440b37e27fe&source=post_page-9440b37e27fe----32fff991e0b3---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----32fff991e0b3--------------------------------) ·7 分钟阅读·2023 年 11 月 22 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F32fff991e0b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmanaging-your-cloud-based-data-storage-with-rclone-32fff991e0b3&user=Chaim+Rand&userId=9440b37e27fe&source=-----32fff991e0b3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F32fff991e0b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmanaging-your-cloud-based-data-storage-with-rclone-32fff991e0b3&source=-----32fff991e0b3---------------------bookmark_footer-----------)![](img/31058489fd9aa402449e5f9e7e07f343.png)

图片由 [Tom Podmore](https://unsplash.com/@tompodmore86?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

随着公司越来越依赖基于云的存储解决方案，拥有合适的工具和技术以有效管理其[大数据](https://en.wikipedia.org/wiki/Big_data)变得至关重要。在之前的文章中（例如，[这里](https://medium.com/towards-data-science/streaming-big-data-files-from-cloud-storage-634e54818e75)和这里），我们探讨了几种从云存储中检索数据的方法，并展示了它们在不同任务中的有效性。我们发现，最优的工具可能会根据具体任务（例如文件格式、数据文件的大小、数据访问模式）以及我们希望优化的指标（例如延迟、速度或成本）而有所不同。在这篇文章中，我们探讨了另一个流行的云存储管理工具——有时被[称为](https://rclone.org/)“*云存储的瑞士军刀*”——[rclone](https://rclone.org/)命令行工具。rclone 支持超过[70 种存储服务提供商](https://rclone.org/#providers)，具备类似于供应商特定存储管理应用程序（如 AWS CLI（用于 Amazon S3）和[gsutil](https://cloud.google.com/storage/docs/gsutil)（用于 Google Storage））的功能。但它是否足够出色以构成一个可行的替代方案？在什么情况下 rclone 会是首选工具？在接下来的章节中，我们将展示 rclone 的使用，评估其性能，并**突出其在特定用例中的价值——在不同对象存储系统之间转移数据**。

## 免责声明

本文绝不是为了取代官方的[rclone 文档](https://rclone.org/)。也不意图为使用 rclone 或我们提到的其他工具提供认可。您在进行云数据管理时的最佳选择将大大依赖于项目的详细信息，并应根据详细的、特定用例的测试来做出。请确保在阅读时重新评估我们所做的陈述，并对照您手头的最新工具。

# 从云存储中检索数据与 Rclone

以下命令行使用[rclone sync](https://rclone.org/commands/rclone_sync/)来同步云对象存储路径与本地目录的内容。这个例子演示了[Amazon S3](https://rclone.org/s3/)存储服务的使用，但同样可以使用其他云存储服务。

```py
rclone sync -P \
            --transfers 4 \
            --multi-thread-streams 4 \
            S3store:my-bucket/my_files ./my_files 
```

rclone 命令有数十个 [标志](https://rclone.org/docs/) 用于编程其行为。* -P * 标志输出数据传输的进度，包括传输速率和总体时间。在上述命令中，我们包含了两个（众多）可能影响 rclone 运行时性能的控制：[*transfers*](https://rclone.org/flags/#performance) 标志确定要并发下载的最大文件数，而 [*multi-thread-streams*](https://rclone.org/docs/#multi-thread-streams-n) 确定用于传输单个文件的最大线程数。这里我们将两者都保留在默认值（4）。

rclone 的功能依赖于对 [rclone 配置文件](https://rclone.org/commands/rclone_config/) 的适当定义。下面我们演示了上面命令行中使用的远程 *S3store* 对象存储位置的定义。

```py
[S3store]
    type = s3
    provider = AWS
    access_key_id = <id>
    secret_access_key = <key>
    region = us-east-1
```

现在我们已经看到 rclone 的实际操作，接下来的问题是它是否相较于其他云存储管理工具（例如流行的 [AWS CLI](https://aws.amazon.com/cli/)）提供了任何额外的价值。在接下来的两个部分中，我们将评估 rclone 相比于其一些替代品在我们之前帖子中详细探讨的两个场景中的性能：1) 下载一个 2 GB 文件和 2) 下载数百个 1 MB 文件。

## 用例 1：下载大文件

以下命令行使用 [AWS CLI](https://docs.aws.amazon.com/cli/latest/) 从 Amazon S3 下载一个 2 GB 文件。这只是我们在 之前的帖子 中评估的众多方法之一。我们使用 linux 的 *time* 命令来测量性能。

```py
time aws s3 cp s3://my-bucket/2GB.bin .
```

报告的下载时间约为 26 秒（即 ~79 MB/s）。请注意，该值是在我们自己本地 PC 上计算的，可能会因运行时环境的不同而有很大差异。等效的 [rclone copy](https://rclone.org/commands/rclone_copy/) 命令如下：

```py
rclone sync -P S3store:my-bucket/2GB.bin .
```

在我们的设置中，我们发现 rclone 的下载时间比标准的 AWS CLI 慢两倍多。通过适当调整 rclone 控制标志，这一性能有可能得到显著改善。

## 用例 2：下载大量小文件

在这个用例中，我们评估了下载 *800* 个相对较小的 1 MB 文件的运行时性能。在 之前的博客文章 中，我们讨论了在将数据样本流式传输到深度学习训练工作负载的背景下的这个用例，并展示了 [s5cmd](https://github.com/peak/s5cmd) [*beast*](https://github.com/peak/s5cmd#beast-mode-s5cmd) 模式的优越性能。在 *beast* 模式下，我们创建一个包含对象文件操作列表的文件，s5cmd 使用 [多个并行工作线程](https://github.com/peak/s5cmd#numworkers)（默认值为 256）来执行这些操作。下面展示了 s5cmd beast 模式选项：

```py
time s5cmd --run cmds.txt
```

*cmds.txt* 文件包含 *800* 行，格式如下：

```py
cp s3://my-bucket/small_files/<i>.jpg <local_path>/<i>.jpg
```

s5cmd 命令平均耗时 9.3 秒（十次试验的平均值）。

Rclone 支持类似于 s5cmd 的 beast mode 的功能，通过 *files-from* 命令行选项实现。下面我们使用 *transfers* 值设置为 *256* 在我们的 *800* 个文件上运行 rclone copy，以匹配 s5cmd 的默认 [并发设置](https://github.com/peak/s5cmd#configuring-concurrency)。

```py
rclone -P --transfers 256 --files-from files.txt S3store:my-bucket /my-local
```

*files.txt* 文件包含 *800* 行，格式如下：

```py
small_files/<i>.jpg
```

我们的 *800* 个文件的 rclone copy 平均耗时 8.5 秒，比 s5cmd 略少（十次试验的平均值）。

我们承认目前展示的结果可能不足以说服您选择 rclone 而非现有工具。在下一节中，我们将描述一个用例，突出 rclone 的潜在优势。

# 对象存储系统之间的数据传输

现如今，开发团队维护多个对象存储并不罕见。这可能是为了防范存储故障的风险，或是决定使用多个云服务提供商的数据处理服务。例如，您可能会在 AWS 中使用存储在 Amazon S3 中的数据进行 AI 模型训练，同时在 Microsoft Azure 中运行数据分析，分析的数据存储在 Azure Storage 中。此外，您可能还希望在 [FlashBlade](https://www.purestorage.com/products/unstructured-data-storage/flashblade-s.html)、[Cloudian](https://cloudian.com/) 或 [VAST](https://vastdata.com/) 等本地存储基础设施中保持数据的备份。这些情况需要在安全、可靠和及时的方式下，在多个对象存储之间传输和同步数据的能力。

一些云服务提供商提供专门用于此类目的的服务。然而，这些服务并不总是满足您项目的具体需求，或者可能无法提供您所期望的控制级别。例如，[Google Storage Transfer](https://cloud.google.com/storage-transfer/docs/overview) 在指定存储文件夹内的 *所有数据* 迁移方面表现出色，但（截至本文撰写时）不支持从其中传输特定子集的文件。

我们可以考虑的另一个选项是将现有的数据管理方案应用于这个目的。问题在于，像 AWS CLI 和 s5cmd 这样的工具（截至目前）不支持为源存储系统和目标存储系统指定不同的[访问设置](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-endpoints.html)和[安全凭证](https://docs.aws.amazon.com/IAM/latest/UserGuide/security-creds.html)。因此，在存储位置之间迁移数据需要将数据转移到一个中间（临时）位置。下面的命令中，我们结合使用了 s5cmd 和 AWS CLI，通过系统内存和 Linux 管道将文件从 Amazon S3 复制到 Google Storage：

```py
s5cmd cat s3://my-bucket/file \
      | aws s3 cp --endpoint-url https://storage.googleapis.com
      --profile gcp - s3://gs-bucket/file
```

尽管这是一种合法的、尽管笨拙的传输*单个*文件的方式，但在实际操作中，我们可能需要能够传输数百万个文件。为了支持这一点，我们需要添加一个额外的层来启动和管理多个并行的工作进程/处理器。事情可能会迅速变得棘手。

## 使用 Rclone 进行数据传输

与 AWS CLI 和 s5cmd 等工具不同，rclone 使我们能够为源和目标指定不同的访问设置。在以下的 rclone 配置文件中，我们添加了 [Google Cloud Storage](https://rclone.org/googlecloudstorage/) (GCS) 访问设置。（在这里，我们将 GCS 视为 S3 提供商。请查看[这里](https://rclone.org/googlecloudstorage/)了解其他配置选项。）

```py
[S3store]
    type = s3
    provider = AWS
    access_key_id = <id>
    secret_access_key = <key>

[GSstore]
    type = s3
    provider = GCS
    access_key_id = <id>
    secret_access_key = <key>
    endpoint = https://storage.googleapis.com
```

在存储系统之间传输单个文件的格式与复制到本地目录相同：

```py
rclone copy -P S3store:my-bucket/file GSstore:gs-bucket/file
```

然而，rclone 的真正力量来自于将上述的*files-from*选项与这个功能结合。我们无需为数据迁移的并行化设计自定义解决方案，只需用一个命令就能传输一长串文件：

```py
rclone copy -P --transfers 256 --files-from files.txt \
            S3store:my-bucket/file GSstore:gs-bucket/file
```

实际上，我们可以通过将对象文件列表解析为较小的列表（例如，每个列表包含 10,000 个文件）并在单独的计算资源上运行每个列表，从而进一步加速数据迁移。虽然这种解决方案的具体影响因项目而异，但它可以显著提高开发的速度和效率。

# 总结

在这篇文章中，我们探讨了使用 rclone 进行基于云的存储管理，并展示了它在维护和同步多个存储系统数据方面的应用。数据传输确实有许多替代解决方案，但 rclone 基于的方法的便利性和优雅性无可置疑。

这只是我们在最大化基于云的存储解决方案效率方面撰写的众多文章之一。请务必查看一些[我们的其他文章](https://chaimrand.medium.com/)以了解这一重要话题。
