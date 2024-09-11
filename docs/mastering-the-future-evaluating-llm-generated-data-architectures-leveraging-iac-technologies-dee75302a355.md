# 掌握未来：评估利用 IaC 技术生成 LLM 数据架构

> 原文：[https://towardsdatascience.com/mastering-the-future-evaluating-llm-generated-data-architectures-leveraging-iac-technologies-dee75302a355?source=collection_archive---------4-----------------------#2023-10-16](https://towardsdatascience.com/mastering-the-future-evaluating-llm-generated-data-architectures-leveraging-iac-technologies-dee75302a355?source=collection_archive---------4-----------------------#2023-10-16)

## 评估 LLM 在生成基础设施代码以配置、配置和部署现代应用程序方面的适用性

[](https://medium.com/@josu.arcaya?source=post_page-----dee75302a355--------------------------------)[![Josu Diaz de Arcaya](../Images/1476305f0be9c4171a7dd09ae5a2a662.png)](https://medium.com/@josu.arcaya?source=post_page-----dee75302a355--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dee75302a355--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----dee75302a355--------------------------------) [Josu Diaz de Arcaya](https://medium.com/@josu.arcaya?source=post_page-----dee75302a355--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F97cdebb24692&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-the-future-evaluating-llm-generated-data-architectures-leveraging-iac-technologies-dee75302a355&user=Josu+Diaz+de+Arcaya&userId=97cdebb24692&source=post_page-97cdebb24692----dee75302a355---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dee75302a355--------------------------------) ·9分钟阅读·2023年10月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdee75302a355&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-the-future-evaluating-llm-generated-data-architectures-leveraging-iac-technologies-dee75302a355&user=Josu+Diaz+de+Arcaya&userId=97cdebb24692&source=-----dee75302a355---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdee75302a355&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-the-future-evaluating-llm-generated-data-architectures-leveraging-iac-technologies-dee75302a355&source=-----dee75302a355---------------------bookmark_footer-----------)![](../Images/7fe40321056c9eb3d7292d48e6c4dbae.png)

照片由 [ZHENYU LUO](https://unsplash.com/@mrnuclear?utm_source=medium&utm_medium=referral) 提供，出处为 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

在这篇文章中，我们探讨了大型语言模型（LLMs）在利用真实应用程序生命周期方面的适用性，包括从基础设施提供到配置管理和部署。由此产生的源代码在 GitHub¹¹ 上公开可用。基础设施即代码（IaC）解决方案通过代码而非手动过程来促进基础设施的管理和提供¹。它们正变得越来越普遍，主要的云服务提供商已实现了自己的 IaC 解决方案，以与其服务互动。在这方面，AWS CloudFormation、Google Cloud Deployment Manager 和 Azure Resource Manager Templates 简化了云服务的提供，消除了 IT 运营手动建立服务器、数据库和网络的需求。然而，这些多种可能性引入了供应商锁定的风险，因为为特定云提供商定义的 IaC 不能移植，如果需要不同的云提供商，则需要进行翻译。在这方面，像 Terraform² 或 Pulumi³ 这样的工具提供了对不同云提供商各种实现的抽象，并促进了可移植部署的开发。这样，供应商锁定的风险显著降低，组织可以动态响应其需求，而无需承担巨大的实施成本。此外，IaC 技术还带来了许多好处⁴：

+   一致性：它允许通过实现可重复的部署来自动化基础设施提供。

+   降低风险：它通过最小化人工干预，促成了一种更少出错的基础设施管理方法。

+   成本优化：它使得识别不必要资源更容易，并能够更快地在云提供商之间迁移，以应对计费变化。

+   改进的协作：脚本可以集成到版本控制工具中，这促进了个人和团队之间的协作。

然而，应用生命周期超出了基础设施提供的范围。下图显示了不同 IaC 技术支持的应用生命周期⁵。

![](../Images/171a40d7108f995f9712816d2860a60e.png)

基础设施即代码技术支持的应用生命周期。| 来源：Josu Diaz-de-Arcaya 等⁵

在这种情况下，IaC 技术的目标超出了简单的基础设施资源提供。在建立了必要的基础设施后，配置管理阶段确保所有要求都得到适当安装。这个阶段通常使用工具如 ansible⁶、chef⁷ 或 puppet⁸ 来完成。最后，应用部署阶段负责在各种基础设施设备上进行应用的协调。

# 了解 LLMs

大型语言模型（LLMs）是指一类人工智能模型，它们旨在根据提供给它们的输入理解和生成类似人类的文本。这些模型以其大量的参数而闻名，这使它们能够捕捉语言中复杂的模式和细微的差异⁹。

+   **文本生成**：LLMs生成的文本可以与其周围的内容连贯且相关。它们被用于完成文本、生成材料，甚至进行创意写作活动。

+   **语言理解**：LLMs能够理解并从文本中提取信息。它们能够进行态度分析、文本分类和信息检索。

+   **翻译**：LLMs可以将文本从一种语言翻译成另一种语言。这对机器翻译应用非常有益。

+   **回答问题**：LLMs可以根据给定的上下文回答问题。它们用于聊天机器人和虚拟助手以回答用户的查询。

+   **文本摘要**：LLMs可以将长篇文章总结为更短、更连贯的摘要。这对于快速消化信息非常有用。

在上述能力中，我们将专注于文本生成。特别是在根据输入提示生成正确的IaC代码方面，大型语言模型（LLMs）在自然语言处理领域取得了显著进展，但也提出了**几个挑战和关注点**。当时与LLMs相关的一些关键问题和关注点包括：

+   **偏见和公平性**：LLMs可以学习数据中存在的偏见，这可能导致有偏见或不公平的结果。

+   **错误信息和虚假信息**：LLMs可能生成虚假或误导性信息，这可能促成在线错误信息和虚假信息的传播。这些模型有潜力创建看似可信但事实上不正确的内容。

+   **安全和隐私**：LLMs可能被误用来生成恶意内容，例如深度伪造文本、假新闻或钓鱼邮件。

下表显示了各种LLMs的比较¹⁰。

# 使用LLMs生成IaC

为了测试当前LLM工具在IaC领域的表现，设计了一个用例。最终目标是使用FastAPI框架在虚拟机中构建一个API，允许客户端使用HTTP方法在Elasticsearch集群中执行搜索和删除任务。该集群将由三个节点组成，每个节点都在自己的虚拟机中，并且在另一台机器上将是Kibana，这是支持可视化的集群管理工具。所有内容都必须在AWS云中完成。下图显示了这个架构：

![架构图](../Images/ec4f6f1048255259e2ed89c1e5c1056c.png)

设计用例，旨在测试LLMs生成IaC的可行性。

挑战是使用 LLM 工具成功完成以下三个任务。在这篇文章中，我们使用了 OpenAI 的 ChatGPT。

1.  用于在 AWS 上构建五个虚拟机器的 Terraform 代码。

1.  用于通过 API 的 HTTP 方法执行文档搜索和删除操作的 FastAPI 应用的源代码。

1.  用于在三个节点上部署和安装 Elasticsearch 集群、在另一个节点上部署 Kibana、在剩余节点上部署 FastAPI 应用的 Ansible 代码。

**任务 #1**

对于这个挑战，我们使用了以下提示：

> 我需要通过 Terraform 创建五个虚拟机器，选择你想要的公共云提供商。这些虚拟机器的目的如下：其中三个用于部署 Elasticsearch 集群，每天处理 2G 的数据；另一个用于 Kibana；最后一个用于部署 FastAPI 应用。你应该为每个虚拟机器选择硬件和云提供商。对于你不知道的变量，使用占位符变量。选择便宜的实例。

初始回应是一个不错的尝试，但我们需要不断迭代。例如，我们希望所有的变量都定义在一个单独的文件中，这导致了以下图像。

variables.tf 的代码片段，包含了需要配置的变量。

同样，我们希望知道部署的 IP 地址，并且我们希望这个配置在一个单独的文件中。

output.tf 的代码片段，包含了刚刚配置好的虚拟机器的 IP 地址。

AI 在描述我们想要的实例以及为它们配置所需的安全组方面做得非常出色。

main.tf 的代码片段，包含了需要配置的虚拟机器

它还创建了我们需要的安全组所需的资源，并在定义各种端口时使用了占位符。

main.tf 的代码片段，包含了要使用的安全组。

总的来说，ChatGPT 在完成这个任务时表现得很好。然而，我们花了一段时间来获得一个可行的配置，确保网络配置正确。例如，我们希望连接到每个配置好的虚拟机器，这一点我们以如下方式进行说明。

> 我希望从我的笔记本电脑上对它们进行 ssh 访问，并且 Kibana 实例需要从我的笔记本电脑上进行 http 和 https 访问。

上述提示生成的代码几乎正确，因为 AI 对 ingress 和 egress 策略感到困惑。然而，这一点很容易发现并修正。

在能够访问虚拟机器后，我们遇到了由于权限不足而无法连接的问题。这导致了更长时间的对话，最终我们发现自己添加这些行更容易。

**任务 #2**

对于这个挑战，我们使用了以下提示：

> 我需要创建一个 FastAPI 应用程序。这些 API 的目的是提供存储单个 JSON 文档到 Elasticsearch 集群、存储多个文档以及删除文档的方法。Elasticsearch 集群部署在 3 个节点上，并且具有基本的身份验证，用户名为“tecnalia”，密码为“iac-llm”。

这个提示的结果非常成功。该应用程序使用 Elasticsearch python 包¹²与 Elasticsearch 集群进行交互，完全有效。我们只需要记住要更改部署集群的节点的 IP 地址。在下图中，第一个方法是用于在 Elasticsearch 中插入单个文档。

存储单个文档方法的代码摘录。

然后，第二个方法用于在一次调用中批量插入多个文档。

存储多个文档方法的代码摘录。

最后，最后一个方法可以用于从 Elasticsearch 集群中删除单个文档。

删除文档方法的代码摘录。

我们认为这个实验非常成功，因为它正确地选择了适合执行任务的库。然而，仍需进一步手动调整以将这段代码变成生产就绪的软件。

**任务 #3**

对于这个挑战，我们使用了以下提示：

> 生成 Ansible 代码以在三个节点上安装 Elasticsearch 集群。请同时添加一个连接到集群的 Kibana 节点。

这个提示在生成所需的 Ansible 脚本方面表现尚可。它在将源代码组织到各种文件中方面做得非常出色。首先是包含所有节点详细信息的清单。请记住，此文件需要根据任务 #1 生成的正确 IP 地址进行调整。

inventory.ini 的代码摘录

然后，安装 Elasticsearch 的主要 Ansible 脚本在以下图中显示。这是它的一个摘录，完整示例可以在仓库¹¹中找到。

elasticsearch_playbook.yml 的代码摘录

另一方面，每个 Elasticsearch 节点所需的配置已经方便地生成了 Jinja 文件。在这种情况下，我们不得不手动添加 *path.logs* 和 *path.data* 配置，因为 Elasticsearch 无法启动，原因是权限问题。

elasticsearch.yml.j2 的代码摘录。

类似地，ChatGPT 能够为 Kibana 实例生成类似的配置。然而，在这种情况下，我们手动将配置分离到一个单独的文件中以方便管理。以下图片展示了该文件的一个摘录。

kibana_playbook.yml 的代码摘录。

类似地，以下 jinja 文件涉及 Kibana 实例，虽然 IP 地址最好进行参数化，但似乎也不错。

kibana.yml.j2 的代码摘录

总体来说，我们发现 ChatGPT 在生成项目框架方面非常出色。然而，将框架转化为生产级应用程序仍需进行大量操作。在这方面，需要对所使用的技术具有深入的专业知识来调整项目。

# 结论

本文讨论了使用大语言模型（LLMs）来监督应用程序生命周期的情况。以下将讨论这一工作的优缺点。

**优点**

+   使用大语言模型（LLMs）支持应用生命周期的各个阶段在启动项目时特别有利，尤其是在知名技术方面。

+   初始框架结构良好，提供了否则不会使用的结构和方法。

**缺点**

+   大语言模型（LLMs）面临使用 AI 解决方案时的偏见风险；在这种情况下，ChatGPT 选择了 AWS 而非类似选项。

+   将项目打磨至生产就绪可能会很麻烦，有时手动调整代码更为方便，这需要对所使用的技术有深入了解。

# 致谢

本工作由巴斯克政府资助的SIIRSE Elkartek项目（面向工业5.0的鲁棒、安全和伦理的智能工业系统：规范、设计、评估和监控的先进范式）（ELKARTEK 2022 KK-2022/00007）提供资金支持。

# 作者贡献

概念化、分析、调查和写作是 [Juan Lopez de Armentia](https://www.linkedin.com/in/juan-lopez-de-armentia/)、[Ana Torre](https://www.linkedin.com/in/anaistobas/) 和 [Gorka Zárate](https://www.linkedin.com/in/gorka-z%C3%A1rate-martinez-748a4961/) 的共同努力。

# 参考文献

1.  *什么是基础设施即代码（IaC）？*（2022）。 [https://www.redhat.com/en/topics/automation/what-is-infrastructure-as-code-iac](https://www.redhat.com/en/topics/automation/what-is-infrastructure-as-code-iac)

1.  *Terraform by HashiCorp*。（无日期）。检索于2023年10月5日，来自 [https://www.terraform.io](https://www.terraform.io/)

1.  *Pulumi — 通用基础设施即代码*。（无日期）。检索于2023年10月5日，来自 [https://www.pulumi.com/](https://www.pulumi.com/)

1.  *基础设施即代码的七大主要好处 — DevOps*。（无日期）。检索于2023年10月5日，来自 [https://duplocloud.com/blog/infrastructure-as-code-benefits/](https://duplocloud.com/blog/infrastructure-as-code-benefits/)

1.  Diaz-De-Arcaya, J., Lobo, J. L., Alonso, J., Almeida, A., Osaba, E., Benguria, G., Etxaniz, I., & Torre-Bastida, A. I.（2023）。*IEM: 用于多语言 IaC 部署的统一生命周期编排器 ACM 引用格式*。 [https://doi.org/10.1145/3578245.3584938](https://doi.org/10.1145/3578245.3584938)

1.  *Ansible 是简单的 IT 自动化*。（无日期）。检索于2023年10月5日，来自 [https://www.ansible.com/](https://www.ansible.com/)

1.  *Chef 软件 DevOps 自动化解决方案 | Chef*。（无日期）。检索于2023年10月5日，来自 [https://www.chef.io/](https://www.chef.io/)

1.  *Puppet Infrastructure & IT Automation at Scale | Puppet by Perforce*。（无日期）。检索于2023年10月5日，来自 [https://www.puppet.com/](https://www.puppet.com/)

1.  Kerner, S. M. （无日期）。*什么是大语言模型？ | TechTarget 定义*。检索于2023年10月5日，来自 [https://www.techtarget.com/whatis/definition/large-language-model-LLM](https://www.techtarget.com/whatis/definition/large-language-model-LLM)

1.  Sha, A. （2023）。*2023年12大最佳大语言模型（LLMs） | Beebom*。 [https://beebom.com/best-large-language-models-llms/](https://beebom.com/best-large-language-models-llms/)

1.  Diaz-de-Arcaya, J.，Lopez de Armentia, J.，& Zarate, G. （无日期）。*iac-llms GitHub*。检索于2023年10月5日，来自 [https://github.com/josu-arcaya/iac-llms](https://github.com/josu-arcaya/iac-llms)

1.  Elastic Client Library 维护者。（2023）。*elasticsearch · PyPI*。 [https://pypi.org/project/elasticsearch/](https://pypi.org/project/elasticsearch/)
