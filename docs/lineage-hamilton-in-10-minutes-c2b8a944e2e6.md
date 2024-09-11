# **10分钟理解Lineage和Hamilton**

> 原文：[https://towardsdatascience.com/lineage-hamilton-in-10-minutes-c2b8a944e2e6?source=collection_archive---------8-----------------------#2023-05-26](https://towardsdatascience.com/lineage-hamilton-in-10-minutes-c2b8a944e2e6?source=collection_archive---------8-----------------------#2023-05-26)

## 通过使用 [Hamilton](https://github.com/dagworks-inc/hamilton) 的开箱即用的Lineage功能，减少调试管道的时间。

[](https://medium.com/@stefan.krawczyk?source=post_page-----c2b8a944e2e6--------------------------------)[![Stefan Krawczyk](../Images/150405abaad9590e1dc2589168ed2fa3.png)](https://medium.com/@stefan.krawczyk?source=post_page-----c2b8a944e2e6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c2b8a944e2e6--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c2b8a944e2e6--------------------------------) [Stefan Krawczyk](https://medium.com/@stefan.krawczyk?source=post_page-----c2b8a944e2e6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F193628e26f00&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flineage-hamilton-in-10-minutes-c2b8a944e2e6&user=Stefan+Krawczyk&userId=193628e26f00&source=post_page-193628e26f00----c2b8a944e2e6---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----c2b8a944e2e6--------------------------------) · 11 分钟阅读 · 2023年5月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc2b8a944e2e6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flineage-hamilton-in-10-minutes-c2b8a944e2e6&user=Stefan+Krawczyk&userId=193628e26f00&source=-----c2b8a944e2e6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc2b8a944e2e6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flineage-hamilton-in-10-minutes-c2b8a944e2e6&source=-----c2b8a944e2e6---------------------bookmark_footer-----------)![](../Images/18b9188673f75ef0764b9b84979467c2.png)

Hamilton + Lineage：使您能够可视化和理解事物之间的联系。这是通过driver.visualize_path_between()创建的。图像由作者提供。

[Hamilton](https://github.com/dagworks-inc/hamilton) 是一个通用的开源微框架，用于描述数据流。它非常适用于数据和机器学习（ML）工作。在这篇文章中，我们将引导你了解 [Hamilton](https://github.com/dagworks-inc/hamilton) 的血缘关系功能，这些功能可以帮助你快速回答在数据和机器学习工作中常见的问题，从而提高工作效率，更有效地与同事合作。如果你不熟悉 Hamilton，我们邀请你浏览以下内容：

+   [www.tryhamilton.dev](http://www.tryhamilton.dev) （浏览器中的互动概述）

+   [Introducing Hamilton](/functions-dags-introducing-hamilton-a-microframework-for-dataframe-generation-more-8e34b84efc1d) （背景故事和介绍）

+   [Hamilton + Pandas in 5 minutes](/how-to-use-hamilton-with-pandas-in-5-minutes-89f63e5af8f5)

+   Github — [https://github.com/dagworks-inc/hamilton](https://github.com/dagworks-inc/hamilton)

# 血缘关系

你可能会问，我所说的血缘关系是什么意思？在机器学习和数据工作中，“血缘关系”指的是数据在转化和处理成表格、统计模型等形式时的历史记录或可追溯性。血缘关系帮助确定“数据”的来源、质量和可靠性，因为它有助于理解数据如何被转化。

## 你为什么应该关注血缘关系？

如果你是你必须管理的内容的作者，你可能对你写的所有内容如何连接有一个大致的了解。**但**，对于继承你工作的其他人，或引入一个合作者，或者调试你六个月前写的东西，**迅速了解情况可能会是一个挑战**！我听说过有团队花费超过四分之一的时间来理解同事留下的工作（他们显然没有使用Hamilton）！在这种情况下，*血缘关系*可以提供很大的帮助。为了提供更多背景，以下是一些导致生产力损失、普遍不满甚至中断的常见情况：

(a) 调试你的训练集/模型中的数据问题（结果发现是上游数据问题）。

(b) 尝试确定他人的管道如何工作（因为你继承了它，或必须与他们合作）。

(c) 需要满足某些数据如何到达某处的审计要求（例如 GDPR、数据使用政策等）。

(d) 想要对一个功能/列进行更改，但没有好的方法来评估潜在影响（例如，业务发生了变化，收集的数据也发生了变化）。

从实际角度来看，大多数从事数据和机器学习的人不会遇到或理解“血统”的价值，因为往往做到这一点很麻烦。通常需要一个外部系统（如 [open lineage](https://openlineage.io/)、[Amundsen](https://www.amundsen.io/)、[Datahub](https://datahub.io/) 等），然后用户需要额外工作以填充信息。好消息是，如果你使用 Hamilton，不需要另一个系统来获取血统；使用血统也很直接！

## **血统即代码**

> *使用 Hamilton，你不需要添加任何其他内容，你就可以获得血统。*

![](../Images/7f3aaa34a9ff82b39fc82fd81e3932a9.png)

编写代码。获取类似这样的血统。这是使用 Hamilton Driver 的 visualize_execution() 函数创建的。图片由作者提供。

通过以 Hamilton 方式编写代码，你在函数中定义了计算，然后通过函数输入参数指定事物如何连接，编码血统。将此代码与例如版本控制系统（如 [git](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control)）连接起来，也为你提供了在时间点快照血统的手段！因为你必须更新代码以更改计算操作方式，因此，按定义，你也更新了血统，而无需做任何额外的操作 😎。

![](../Images/067e1140bf8a39c935158394428e1116.png)

Hamilton 范式概述。你编写的函数定义了计算应如何进行，而不是编写过程性代码。以这种方式定义函数完全映射到血统！

高层次的“血统即代码”及其后续访问的步骤是：

1.  编写 Hamilton 代码并进行版本控制。

1.  实例化一个 Hamilton Driver，它将会有一个表示数据和计算流动的方式，正如你的 Hamilton 代码所定义的。Driver 对象可以发出/提供关于血统的信息！

1.  使用你的版本控制系统，你可以回溯到过去，以理解血统如何随着时间的推移而变化，因为它被编码在代码中！

## 添加元数据以使血统更加有用！

当你还可以将元数据附加到血统上时，血统会变得更加有用。Hamilton 默认使你能够表达事物如何连接，但通过附加额外的元数据，我们现在可以通过 Hamilton 编码的血统连接业务和公司概念。

例如，你可以为提取数据的函数添加关切点，如[PII](https://en.wikipedia.org/wiki/Personal_data)、所有权、重要性等，并为创建重要文物的函数（如模型、数据集）添加类似信息。现在这些函数已被注解，能够回答的问题集就大大增加了！相比之下，其他系统要求你将这些元数据放在其他地方；例如一个 YAML 文件、代码库的不同部分，或在其他地方单独整理这些信息。在 Hamilton 的哲学中，*我们认为直接注解实际代码最为合理*，因为这样可以确保代码和元数据的真实来源始终保持最新。使用 Hamilton 更容易维护，因为只需在一个地方进行更改。

在 Hamilton 中，添加额外元数据的方法是通过使用 `[@tag](https://hamilton.dagworks.io/en/latest/reference/decorators/tag/#hamilton.function_modifiers.tag)` 和 `[@tag_outputs](https://hamilton.dagworks.io/en/latest/reference/decorators/tag/#hamilton.function_modifiers.tag)` 装饰器。它们允许指定任意的字符串键值对。这提供了灵活性，使你和你的组织能够定义适合你们上下文的标签和值。

例如，以下代码为：

> `titanic_data` 函数指定了其 `source`、`owner`、`importance`，包含 PII，并链接到一些内部维基。
> 
> `age` 和 `sex` 列添加了大量元数据，指定它们是 PII。

注意：我展示了一个稍微复杂的示例，使用了多个装饰器，只是为了说明情况不会比这更复杂——代码仍然相当易读！

```py
@tag_outputs(age={"PII": "true"}, sex={"PII": "true"})
@extract_columns(*columns_to_extract)
@tag(
   source="prod.titantic",
   owner="data-engineering",
   importance="production",
   info="https://internal.wikipage.net/",
   contains_PII="True", 
   target_="titanic_data",
)
def titanic_data(index_col: str, location: str) -> pd.DataFrame:
    """Returns a dataframe, that then has its columns extracted."""
    # ... contents of function not important ... code skipped for brevity
```

以下代码从业务角度提供有关正在创建的模型的更多上下文：

```py
@tag(owner="data-science", importance="production", artifact="model")
def fit_random_forest(
    prefit_random_forest: base.ClassifierMixin,
    X_train: pd.DataFrame,
    y_train: pd.Series,
) -> base.ClassifierMixin:
  """Returns a fit RF model."""
  # ... contents of function not important ... code skipped for brevity
```

现在这些额外的元数据附加到函数和血缘上，我们可以将其作为上下文来提出更有用的问题。更多内容见下文。

# 如何使用 Hamilton 回答血缘问题

回答血缘问题的基本机制依赖于实例化一个 [Hamilton Driver](https://hamilton.dagworks.io/en/latest/reference/drivers/Driver/#driver)，该 Driver 在底层创建一个有向无环图 (DAG) 来表示其世界观，然后使用 Driver 拥有的函数。让我们列出相关的 Driver 函数及其功能。

**可视化血缘**（即显示 DAG）：

+   [display_*()](https://hamilton.dagworks.io/en/latest/reference/drivers/Driver/#hamilton.driver.Driver.display_all_functions) 有三个 **display_*** 函数。一个帮助你显示定义的所有连接关系。然后一个函数用于仅可视化上游内容，最后一个函数可视化给定函数/节点的下游内容。

+   [visualize_execution()](https://hamilton.dagworks.io/en/latest/reference/drivers/Driver/#hamilton.driver.Driver.visualize_execution) 帮助可视化生成某些输出所需的一切。用于展示 `.execute()` 的作用。

+   [visualize_path_between()](https://hamilton.dagworks.io/en/latest/reference/drivers/Driver/#hamilton.driver.Driver.visualize_path_between) 帮助可视化两个节点之间的路径。

**获取谱系需求的元数据访问权限：**

+   [list_available_variables()](https://hamilton.dagworks.io/en/latest/reference/drivers/Driver/#hamilton.driver.Driver.list_available_variables) 使用户能够获取所有“函数/节点”及其标签。

+   [what_is_downstream_of()](https://hamilton.dagworks.io/en/latest/reference/drivers/Driver/#hamilton.driver.Driver.what_is_downstream_of) 使用户能够获取指定节点下游的所有“函数/节点”。

+   [what_is_upstream_of()](https://hamilton.dagworks.io/en/latest/reference/drivers/Driver/#hamilton.driver.Driver.what_is_upstream_of) 使用户能够获取指定节点上游的所有“函数/节点”。

+   [what_is_the_path_between()](https://hamilton.dagworks.io/en/latest/reference/drivers/Driver/#hamilton.driver.Driver.what_is_the_path_between) 使用户能够获取两个指定“函数/节点”之间路径的所有“函数/节点”。

使用 Hamilton 和上述函数，你可以对谱系进行编程访问以及可视化，这意味着你可以在 CI 系统中、在运行生产代码的笔记本中，或在任何 Python 运行的地方提出这些问题！

回顾一下到目前为止讨论的内容，回答谱系问题的一般方法如下：

1.  编写 Hamilton 代码。

1.  使用 `@tag` 来注释函数。

1.  实例化一个 Driver 来创建你的 DAG。

1.  使用 Driver 函数来提问/回答问题。

为了简短起见，我们不会深入探讨它们的使用，我们将简单展示一些你可以用 Hamilton 直接回答的一般问题，并结合上面提到的函数举例。我们将使用的示例问题是一个 Hamilton 中的端到端模型管道，用于预测泰坦尼克号的生存率；代码和更多信息可以在 [Hamilton 仓库的谱系示例](https://github.com/DAGWorks-Inc/hamilton/tree/main/examples/lineage) 中找到。术语说明：我们将 `function` 和 `node` 互换使用，因为在 Hamilton 中，`function` 在我们的可视化中会变成 `node`。

## (1) 生成这个数据/模型的操作序列是什么？

这是一个常见的问题，如何从**A->B**，其中**A**可以是某些数据，`**->**` 对我们来说是不透明的，**B** 可以是某个工件（可以是更多数据或模型/对象）。

借助Hamilton，通过以Hamilton规定的风格编写代码，您可以清晰而轻松地定义操作的顺序以及它们之间的关系！所以如果您不能通过查看代码本身来回答这个问题，可以请Hamilton提供帮助。在我们的示例中，为了了解`feature_encoders`是如何创建的，即使对它们知之甚少，我们也可以请求Hamilton Driver为我们可视化它们的创建过程：

```py
...
# create the driver
dr = driver.Driver(config, data_loading, features, sets, model_pipeline, adapter=adapter)
# visualize how it's created
dr.visualize_execution(
    [features.encoders], "encoder_lineage", {"format": "png"}, inputs=inputs
)
```

输出结果：

![](../Images/0e15e0f01d25dfbcbef5c52f3568aef0.png)

示例血缘可视化。图像由作者提供。

然后，您可以将其与浏览代码库相结合，以更轻松地导航和理解发生了什么。

**(2) 谁/哪些数据源导致了这个工件/模型？**

这对于调试数据问题以及理解一个工件/模型依赖于哪些团队和数据源非常有用。以我们的泰坦尼克号示例为例，假设我们的随机森林模型出现了一些异常，我们想要重新检查当前生产模型的数据源是什么以及它们的所有者，以便我们可以联系他们。为此，我们可以编写以下代码：

```py
# create the driver
dr = driver.Driver(config, data_loading, features, sets, model_pipeline, adapter=adapter)
# Gives us all the operations that are upstream of creating the output.
upstream_nodes = dr.what_is_upstream_of("fit_random_forest")
```

上述两行创建了一个Driver，然后提取`fit_random_forest`的所有上游节点。然后我们可以遍历这些节点并提取我们想要的信息：

```py
teams = []
# iterate through 
for node in upstream_nodes:
  # filter to nodes that we're interested in getting information about
  if node.tags.get("source"):
    # append for output
    teams.append({
      "team": node.tags.get("owner"),
      "function": node.name,
      "source": node.tags.get("source"),
    })
print(teams)
# [{'team': 'data-engineering', 
#   'function': 'titanic_data', 
#   'source': 'prod.titanic'}] 
```

## (3) 谁/什么在这个变换的下游？

回答这个问题实际上是(2)的补充。当有人想要更改特征或数据源时，您通常会遇到这个问题。以我们的泰坦尼克号示例为例，假设我们在数据工程部门，想要更改源数据。我们如何确定使用这些数据的工件是什么以及谁拥有它们？

我们使用`what_is_downstream_of()` Driver函数来获取下游的节点：

```py
# create the driver
dr = driver.Driver(config, data_loading, features, sets, model_pipeline, adapter=adapter)
# Gives us all the operations that are upstream of creating the output.
downstream_nodes = dr.what_is_downstream_of("titanic_data")
```

然后类似于(2)，我们只需遍历并提取我们想要的信息：

```py
artifacts = []
for node in downstream_nodes:
  # if it's an artifact function
  if node.tags.get("artifact"):
      # pull out the information we want
      artifacts.append({
          "team": node.tags.get("owner", "FIX_ME"),
          "function": node.name,
          "artifact": node.tags.get("artifact"),
      })
print(artifacts)
# [{'team': 'data-science', 'function': 'training_set_v1', 'artifact': 'training_set'}, 
#  {'team': 'data-science', 'function': 'fit_random_forest', 'artifact': 'model'}, 
#  {'team': 'data-science', 'function': 'encoders', 'artifact': 'encoders'}]
```

## (4) 什么被定义为PII数据，它最终会出现在哪些内容中？

随着目前的法规，这成为了一个越来越常见的问题。基于上述内容，我们可以结合几个Driver函数来回答这类问题。

在我们的泰坦尼克号示例中，假设我们的合规团队来找我们了解我们如何使用PII数据，即它最终会出现在哪些工件中？他们希望每个月都能收到这份报告。好吧，借助Hamilton，我们可以编写脚本以程序化地获取与PII数据相关的血缘信息。首先，我们需要获取所有标记为PII的内容：

```py
# create the driver
dr = driver.Driver(config, data_loading, features, sets, model_pipeline, adapter=adapter)
# using a list comprehension to get all things marked PII
pii_nodes = [n for n in dr.list_available_variables() 
             if n.tags.get("PII") == "true"]
```

然后，要获取所有下游的工件，我们只需请求所有下游的节点，然后筛选出带有“artifact”标签的节点：

```py
pii_to_artifacts = {}
# loop through each PII node
for node in pii_nodes:
  pii_to_artifacts[node.name] = []
  # ask what is downstream
  downstream_nodes = dr.what_is_downstream_of(node.name)
  for dwn_node in downstream_nodes:
    # Filter to nodes of interest
    if dwn_node.tags.get("artifact"):
      # pull out information
      pii_to_artifacts[node.name].append({
          "team": dwn_node.tags.get("owner"),
          "function": dwn_node.name,
          "artifact": dwn_node.tags.get("artifact"),
      })
print(pii_to_artifacts)
# {'age': [{'artifact': 'training_set',
#         'function': 'training_set_v1',
#         'team': 'data-science'},
#        {'artifact': 'model',
#         'function': 'fit_random_forest',
#         'team': 'data-science'}],
# 'sex': [{'artifact': 'training_set',
#         'function': 'training_set_v1',
#         'team': 'data-science'},
#        {'artifact': 'encoders', 'function': 'encoders', 'team': None},
#        {'artifact': 'model',
#         'function': 'fit_random_forest',
#         'team': 'data-science'}]} 
```

# 我可以在我的笔记本/IDE中获得这个吗？

你们中的一些人可能在想，为什么在开发时不使用这种 lineage 视图？好主意！由于我们是一个开源项目，我们很乐意在这方面获得一些帮助；如果你有兴趣进行测试/贡献，我们有一个由 Thierry Jean 发起的 *alpha* 版 [VSCode 扩展](https://marketplace.visualstudio.com/items?itemName=ThierryJean.hamilton&ssr=false#overview)，可以帮助你在输入时可视化 lineage。我们欢迎贡献。

![](../Images/fbb5be1f6fb6e320fed0d33a917f4e74.png)

Alpha Hamilton VSCode 扩展由 Thierry Jean 发起。图片作者提供。

# 最后

使用 Hamilton，**你的代码定义 lineage**。这意味着你可以开箱即用地获得 lineage，而无需另一个系统，并且当结合版本控制系统和额外的 metadata 时，是一种非常简单轻便的方式来理解数据与代码的连接。

Hamilton 允许对你编码的 lineage 和 metadata 进行程序化访问，这使你可以将其放置在 CI 作业、脚本或任何 Python 运行的地方。

希望你喜欢这个快速概述，如果你对其中任何内容感到兴奋或想要更多资源，这里有一些链接：

+   📣 [加入我们的 Slack 社区](https://join.slack.com/t/hamilton-opensource/shared_invite/zt-1bjs72asx-wcUTgH7q7QX1igiQ5bbdcg) — 我们很乐意帮助回答你的问题或帮助你入门。

+   ⭐️ 在 [github](https://github.com/DAGWorks-Inc/hamilton/) 给我们点赞。

+   📝 如果你发现了什么问题，请给我们留个[issue](https://github.com/DAGWorks-Inc/hamilton/issues)。

+   📚 浏览我们的 [文档](https://hamilton.dagworks.io/)。例如，查看 [lineage 部分](https://hamilton.dagworks.io/en/latest/how-tos/use-hamilton-for-lineage/)。

+   ▶️ 在浏览器中直接通过教程 [学习 Hamilton](http://www.tryhamilton.dev)。

# 你可能感兴趣的其他 Hamilton 文章：

+   [5 分钟内使用 Hamilton 与 Pandas](/how-to-use-hamilton-with-pandas-in-5-minutes-89f63e5af8f5)

+   [5 分钟内使用 Hamilton 与 Ray](/scaling-hamilton-with-ray-in-5-minutes-3beb1755fc09)

+   [如何在 Notebook 环境中使用 Hamilton](/how-to-iterate-with-hamilton-in-a-notebook-8ec0f85851ed)

+   [Hamilton 的背景故事和介绍](/functions-dags-introducing-hamilton-a-microframework-for-dataframe-generation-more-8e34b84efc1d)

+   [开发可扩展的特征工程 DAGs](https://outerbounds.com/blog/developing-scalable-feature-engineering-dags)（Hamilton 与 Metaflow）

+   [使用 Hamilton 创建数据流的好处](https://medium.com/@thijean/the-perks-of-creating-dataflows-with-hamilton-36e8c56dd2a)（关于 Hamilton 的用户文章！）
