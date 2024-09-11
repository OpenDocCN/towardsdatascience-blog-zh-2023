# 如何使用 Azure Synapse 遮蔽 PII 数据

> 原文：[https://towardsdatascience.com/how-to-mask-pii-data-with-fpe-using-azure-synapse-12902547af2b?source=collection_archive---------16-----------------------#2023-05-23](https://towardsdatascience.com/how-to-mask-pii-data-with-fpe-using-azure-synapse-12902547af2b?source=collection_archive---------16-----------------------#2023-05-23)

## 学习如何在大规模情况下使用格式保留加密（FPE），安全地将数据从生产环境移动到测试环境

[](https://rebremer.medium.com/?source=post_page-----12902547af2b--------------------------------)[![René Bremer](../Images/e422c4b84e225d2a949251ebc24dbd2c.png)](https://rebremer.medium.com/?source=post_page-----12902547af2b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----12902547af2b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----12902547af2b--------------------------------) [René Bremer](https://rebremer.medium.com/?source=post_page-----12902547af2b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F11e5e7fb3771&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-mask-pii-data-with-fpe-using-azure-synapse-12902547af2b&user=Ren%C3%A9+Bremer&userId=11e5e7fb3771&source=post_page-11e5e7fb3771----12902547af2b---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----12902547af2b--------------------------------) ·7 分钟读·2023 年 5 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F12902547af2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-mask-pii-data-with-fpe-using-azure-synapse-12902547af2b&user=Ren%C3%A9+Bremer&userId=11e5e7fb3771&source=-----12902547af2b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F12902547af2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-mask-pii-data-with-fpe-using-azure-synapse-12902547af2b&source=-----12902547af2b---------------------bookmark_footer-----------)![](../Images/02304fe2bd3a9beb0af28aeef250db7b.png)

数据遮蔽 — 图片来自[Mika Baumeister](https://unsplash.com/@mbaumi)在[Unsplash](https://unsplash.com/)

# 1\. 介绍

许多企业需要其测试环境中的代表性数据。通常，这些数据是从生产环境复制到测试环境中的。但是，个人可识别信息（PII）数据通常是生产环境的一部分，必须先进行遮蔽。Azure Synapse 可以利用[格式保留加密](https://en.wikipedia.org/wiki/Format-preserving_encryption)来遮蔽数据，然后将数据复制到测试环境中。参见下方的架构图。

![](../Images/2b1e55f049c3a995be4bab7adb18308c.png)

1\. 使用 Azure Synapse 的格式保留加密 — 作者图像

在这个博客和库`[azure-synapse_mask-data_format-preserved-encryption](https://github.com/rebremer/azure-synapse_mask-data_format-preserved-encryption)`中，讨论了如何在 Synapse 中创建可扩展且安全的掩码解决方案。在下一章中，将讨论项目的属性。然后在第 3 章中部署项目，第 4 章中测试，并在第 5 章中得出结论。

# 2\. Synapse 中 PII 掩码应用的属性

Synapse 中 PII 掩码应用程序的属性如下：

+   **可扩展的掩码功能**: 基于开源 Python 库如 [ff3](https://github.com/mysto/python-fpe)，可以实现对 ID、姓名、电话号码和电子邮件的 FPE 加密。加密示例为 06–23112312 => 48–78322271，

    Kožušček123a => Sqxbblkd659p, bremersons@hotmail.com => h0zek2fbtw@fr5wsdh.com

+   **安全性**: 使用的 Synapse Analytics 工作区具有以下安全措施：**私有端点**连接到存储账户、Azure SQL（公共访问可以禁用）和其他 100 个数据源（包括本地）；**托管身份**用于验证存储账户、Azure SQL 和 Azure 密钥保管库中的秘密，这些秘密由 ff3 用于加密；**RBAC 授权**授予对 Azure 存储、Azure SQL 和 Azure 密钥保管库的访问权限，以及 [**Synapse 数据外泄保护**](https://learn.microsoft.com/en-us/azure/synapse-analytics/security/workspace-data-exfiltration-protection) 防止数据被恶意内部人员窃取

+   **性能**: 可扩展的解决方案，使用了 Spark。可以通过使用更多 vcores、增加更多执行器（VM）和/或使用更多 Spark 池来扩展解决方案。在一次基本测试中，250MB 数据和 6 列被加密并写入存储中，使用中等规模的 Spark 池，2 个执行器（VM）和每个执行器 8 个 vcores（线程）（总共 16 个 vcores/线程），耗时 1 分 45 秒

+   **编排**: Synapse 管道可以端到端地编排整个过程。也就是说，可以使用超过 100 个不同的连接器从云端/本地数据库中提取数据，将其暂存到 Azure 存储中，进行掩码处理，然后再发送回低环境进行测试。

在下图中，定义了安全属性。

![](../Images/2322a3438bb88e3f22495d29c9ebfbd8.png)

2\. 掩码应用程序的安全属性 — 作者图像

在下一章中，将部署和配置掩码应用程序，包括测试数据。

# 3\. 在 Synapse 中部署 PII 掩码应用程序

在本章中，项目将变得生动，并在 Azure 中部署。将执行以下步骤：

+   3.1 前提条件

+   3.2 部署资源

+   3.3 配置资源

## 3.1 前提条件

本教程中需要以下资源：

+   [Azure 账户](https://azure.microsoft.com/en-us/free/)

+   [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)（推荐）

最后，将下面的 git 仓库克隆到本地计算机。如果你没有安装 git，可以从网页上直接下载 zip 文件。

## 3.2 部署资源

需要部署以下资源：

+   **3.2.1 Azure Synapse Analytics 工作区：** 部署启用了数据泄露保护的 Synapse。确保创建了主要存储帐户。还要确保 Synapse 部署时 1) 启用了管理 VNET，2) 有指向存储帐户的私有端点，3) 仅允许向批准的目标发送出站流量，见下图：

![](../Images/ea8172eac3eb4312ac797a312835e00a.png)

3.2\. 启用管理 VNET 和数据泄露保护的 Azure Synapse — 作者提供的图片

+   **3.2.2 Azure Key Vault：** 该 Key Vault 将用于存储用于在 `[Synapse/mask_data_fpe_prefixcipher.ipynb](https://github.com/rebremer/azure-synapse_mask-data_format-preserved-encryption/blob/main/Synapse/mask_data_fpe_prefixcipher.ipynb)` 和 `[Synapse/mask_data_fpe_ff3.ipynb](https://github.com/rebremer/azure-synapse_mask-data_format-preserved-encryption/blob/main/Synapse/mask_data_fpe_ff3.ipynb)` 中创建 HMAC 和加密的秘密

## 3.3\. 配置资源

需要配置以下资源

+   **3.3.1 存储帐户 - 文件系统：** 在存储帐户中，创建名为 `bronze` 和 `gold` 的新文件系统。然后上传 `[Data\SalesLT.Customer.txt](https://github.com/rebremer/azure-synapse_mask-data_format-preserved-encryption/blob/main/Data/SalesLT.Customer.txt)` 中的 csv 文件。如果需要处理更大的数据集，可以参考 [此](https://testhmacmaskstor.blob.core.windows.net/bronze/SalesLT.Customer_1M.txt?sp=r&st=2023-04-06T09%3A04%3A43Z&se=2024-04-01T17%3A04%3A43Z&spr=https&sv=2021-12-02&sr=b&sig=zYCOdxO40pWoTKBDfGpC%2FsR6ixpUiCneXGHQJSNlxuQ%3D) 250MB 和 1M 记录的集合

+   **3.3.2 Azure Key Vault - 秘密：** 创建名为 `fpekey` 和 `fpetweak` 的秘密。确保为两个秘密都添加了十六进制值。如果 Azure Key Vault 是在启用公共访问的情况下部署的（以便通过 Azure Portal 创建秘密），现在不再需要公共访问，可以禁用公共访问（因为在 3.3.4 中将创建 Synapse 和 Azure Key Vault 之间的私有链接连接）

+   **3.3.3 Azure Key Vault - 访问控制：** 确保在 Azure Key Vault 的访问策略中，Synapse 管理标识具有获取秘密的权限，见下图。

![](../Images/4e24becd9000e4df6f72e91beb06a6ec.png)

3.3.3 Synapse 管理标识在 Key Vault 中具有获取秘密的权限 — 作者提供的图片

+   **3.3.4 Azure Synapse Analytics - 私有链接到 Azure Key Vault**：从 Azure Synapse 工作区管理的 VNET 和你的密钥库创建一个私有终结点。请求从 Synapse 发起，需要在 AKV 网络中进行批准。见下图，其中私有终结点已获批准。

![](../Images/60acccebc45a07fa8529e5d619dfc40e.png)

3.3.4 Synapse 和 Key Vault 之间的私有链接连接 — 作者提供的图片

+   **3.3.5 Azure Synapse Analytics - 链接服务连接到 Azure Key Vault：** 从 Azure Synapse 工作区和你的密钥库创建一个链接服务，见下图。

![](../Images/a3fbd054ad53517bb855bdd05c57ea0e.png)

3.3.5 Synapse 和 Key Vault 之间的链接服务以获取密钥 — 作者提供的图片

+   **3.3.6 Azure Synapse Analytics - Spark 集群**：创建一个中型的 Spark 集群，具有 3 到 10 个节点，并且可以扩展到 2 到 3 个执行器，见下图。

![](../Images/682ae3bddc7fc8c9879f3c47545c049c.png)

3.3.6 在 Synapse 中创建 Spark 集群 — 作者提供的图片

+   **3.3.7 Azure Synapse Analytics - 库上传：** 笔记本`Synapse/mask_data_fpe_ff3.ipynb`使用了[ff3](https://github.com/mysto/python-fpe)进行加密。由于 Azure Synapse Analytics 创建时启用了数据泄露保护，因此不能通过从 pypi.org 拉取进行安装，因为这需要在 Azure AD 租户外的出站连接。下载 pycryptodome wheel [这里](https://files.pythonhosted.org/packages/14/58/77278d7a078241b55b515f6073b90108125fb0d197b384a0f372c5f61c80/pycryptodome-3.17-cp35-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)、ff3 wheel [这里](https://files.pythonhosted.org/packages/3a/c1/3550f1b97d6eedb2117521a149f379bb0d92cbb02e242110bb174f12c9a2/ff3-1.0.1-py3-none-any.whl) 和 Unidecode 库 [这里](https://files.pythonhosted.org/packages/be/ea/90e14e807da5a39e5b16789acacd48d63ca3e4f23dfa964a840eeadebb13/Unidecode-1.3.6-py3-none-any.whl)（Unidecode 库用于将 Unicode 转换为 ASCII，以防在 ff3 中使用大量字母进行数据加密）。然后将 wheels 上传到工作区以使其可信，最后将其附加到 Spark 集群，见下图。

![](../Images/b6b32ada497a3e326f26a41c0d1d07a0.png)

3.3.7 从 Synapse 工作区附加的 Python 包到 Spark 集群 — 作者提供的图片

+   **3.3.8 Azure Synapse Analytics - 笔记本上传：** 上传笔记本`[Synapse/mask_data_fpe_prefixcipher.ipynb](https://github.com/rebremer/azure-synapse_mask-data_format-preserved-encryption/blob/main/Synapse/mask_data_fpe_prefixcipher.ipynb)`和`[Synapse/mask_data_fpe_ff3.ipynb](https://github.com/rebremer/azure-synapse_mask-data_format-preserved-encryption/blob/main/Synapse/mask_data_fpe_ff3.ipynb)`到你的 Azure Synapse Analytics 工作区。确保在笔记本中替换存储帐户、文件系统、密钥库名称和 Keyvault 链接服务的值。

+   **3.3.9 Azure Synapse Analytics - 笔记本 - Spark 会话**：打开笔记本 `Synapse/mask_data_fpe_prefixcipher.ipynb` 的 Spark 会话，确保选择超过 2 个执行器，并使用托管身份运行，请参见下面的截图。

![](../Images/cebbcf4c41e7e399ce3152e761af51dc.png)

3.3.9 作为托管身份运行 Spark 会话 — 作者提供的图片

# 4\. 测试解决方案

部署和配置所有资源后，可以运行笔记本。笔记本 `Synapse/mask_data_fpe_prefixcipher.ipynb` 包含掩盖数字值、字母数字值、电话号码和电子邮件地址的功能，见下面的功能描述。

```py
000001 => 359228
Bremer => 6paCYa
Bremer & Sons!, LTD. => OsH0*VlF(dsIGHXkZ4dK
06-23112312 => 48-78322271
bremersons@hotmail.com => h0zek2fbtw@fr5wsdh.com
Kožušček123a => Sqxbblkd659p
```

如果使用 1M 数据集并加密 6 列，处理大约需要 2 分钟。这可以通过 1) 使用更多 vcores（从中型到大型）进行扩展，使用更多执行器进行扩展，或创建第二个 Spark 池来轻松扩展。请参见下面的截图。

![](../Images/76da8ab53bac7a689c6c1deb7b15a49f.png)

4\. 笔记本成功运行 — 作者提供的图片

在 Synapse 中，笔记本可以轻松嵌入到管道中。这些管道可以用于协调活动，首先将数据从生产源上传到存储，运行笔记本来掩盖数据，然后将掩盖后的数据复制到测试目标。一个示例管道可以在 `[Synapse/synapse_pipeline.json](https://github.com/rebremer/azure-synapse_mask-data_format-preserved-encryption/blob/main/Synapse/synapse_pipeline.json)` 找到。

# 5\. 结论

许多企业需要在测试环境中拥有代表性的样本数据。通常，这些数据是从生产环境复制到测试环境的。在本博客和 Git 仓库 `[-synapse_mask-data_format-preserved-encryption](https://github.com/rebremer/azure-synapse_mask-data_format-preserved-encryption)` 中，讨论了一种可扩展且安全的掩盖解决方案，该方案利用了 Spark、Python 和开源库 [ff3](https://github.com/mysto/python-fpe) 的强大功能，请参见下面的架构。

![](../Images/2b1e55f049c3a995be4bab7adb18308c.png)

5\. 使用 Azure Synapse 的格式保留加密 — 作者提供的图片
