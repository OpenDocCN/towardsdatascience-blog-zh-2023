# 使用 spacy-llm 进行优雅的提示版本管理和 LLM 模型配置

> 原文：[https://towardsdatascience.com/elegant-prompt-versioning-and-llm-model-configuration-with-spacy-llm-126b836daad1?source=collection_archive---------3-----------------------#2023-07-26](https://towardsdatascience.com/elegant-prompt-versioning-and-llm-model-configuration-with-spacy-llm-126b836daad1?source=collection_archive---------3-----------------------#2023-07-26)

## 使用 spacy-llm 简化提示管理并创建数据提取任务

[](https://medium.com/@dehhmesquita?source=post_page-----126b836daad1--------------------------------)[![Déborah Mesquita](../Images/3b77b7eb569e24f2679875429173daf1.png)](https://medium.com/@dehhmesquita?source=post_page-----126b836daad1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----126b836daad1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----126b836daad1--------------------------------) [Déborah Mesquita](https://medium.com/@dehhmesquita?source=post_page-----126b836daad1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdd9e06a0a640&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Felegant-prompt-versioning-and-llm-model-configuration-with-spacy-llm-126b836daad1&user=D%C3%A9borah+Mesquita&userId=dd9e06a0a640&source=post_page-dd9e06a0a640----126b836daad1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----126b836daad1--------------------------------) ·5分钟阅读·2023年7月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F126b836daad1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Felegant-prompt-versioning-and-llm-model-configuration-with-spacy-llm-126b836daad1&user=D%C3%A9borah+Mesquita&userId=dd9e06a0a640&source=-----126b836daad1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F126b836daad1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Felegant-prompt-versioning-and-llm-model-configuration-with-spacy-llm-126b836daad1&source=-----126b836daad1---------------------bookmark_footer-----------)![](../Images/64fa1ffb2743af291cb93acc5f61d321.png)

一张 [整洁的桌子](https://unsplash.com/pt-br/fotografias/zCQsBI7ZltQ)，如果你使用 spacy-llm，你的代码将会像这样哈哈

管理提示和处理 OpenAI 请求失败可能是一个具有挑战性的任务。幸运的是，spaCy 发布了 [spacy-llm](https://github.com/explosion/spacy-llm)，这是一个强大的工具，可以简化提示管理，并消除了从头创建自定义解决方案的需求。

在本文中，你将学习如何利用 spacy-llm 创建一个从文本中提取数据的任务。我们将深入了解 spaCy 的基础知识，并探索一些 spacy-llm 的功能。

# spaCy 和 spacy-llm 101

spaCy 是一个用于 Python 和 Cython 的高级 NLP 库。在处理文本数据时，通常需要几个处理步骤，例如分词和词性标注。为了执行这些步骤，spaCy 提供了 `nlp` 方法，它会调用一个处理管道。

spaCy v3.0 引入了 `config.cfg`，这是一个我们可以在其中包括这些管道详细设置的文件。

`config.cfg` 使用了 [confection](https://github.com/explosion/confection)，这是一个允许创建任意对象树的配置系统。例如，confection 解析以下 `config.cfg`：

```py
[training]
patience = 10
dropout = 0.2
use_vectors = false

[training.logging]
level = "INFO"

[nlp]
# This uses the value of training.use_vectors
use_vectors = ${training.use_vectors}
lang = "en"
```

进入：

```py
{
  "training": {
    "patience": 10,
    "dropout": 0.2,
    "use_vectors": false,
    "logging": {
      "level": "INFO"
    }
  },
  "nlp": {
    "use_vectors": false,
    "lang": "en"
  }
}
```

每个管道使用组件，spacy-llm 将管道组件存储到使用 [catalogue](https://github.com/explosion/catalogue) 的注册表中。这个库，同样来自 Explosion，引入了函数注册表，以便高效地管理组件。`llm` 组件被定义在 [两个主要设置](https://github.com/explosion/spacy-llm/tree/a3ee82eae366d101d37afd055606c9ead0186251#-api) 中：

+   一个 [**任务**](https://github.com/explosion/spacy-llm/tree/a3ee82eae366d101d37afd055606c9ead0186251#tasks)，定义要发送到 LLM 的提示以及解析生成响应的功能

+   一个 [**模型**](https://github.com/explosion/spacy-llm/tree/a3ee82eae366d101d37afd055606c9ead0186251#models)，定义模型及其连接方式

要在我们的管道中包含一个使用 LLM 的组件，我们需要遵循几个步骤。首先，我们需要创建一个任务并将其注册到注册表中。接下来，我们可以使用模型来执行提示并检索响应。现在是时候完成这些操作，以便我们可以运行管道了

# 创建一个从文本中提取数据的任务

我们将使用 [https://dummyjson.com/](https://dummyjson.com/quotes) 上的引用，并创建一个任务来从每个引用中提取上下文。我们将创建提示、注册任务，并最终创建配置文件。

## **1\. 提示**

spacy-llm 使用 Jinja 模板来定义指令和示例。 `{{ text }}` 将被我们提供的引用所替换。这是我们的提示：

```py
You are an expert at extracting context from text. 
Your tasks is to accept a quote as input and provide the context of the quote.
This context will be used to group the quotes together. 
Do not put any other text in your answer and provide the context in 3 words max.
{# whitespace #}
{# whitespace #}
Here is the quote that needs classification
{# whitespace #}
{# whitespace #}
Quote:
'''
{{ text }}
'''
Context
```

## **2\. 任务类**

现在让我们创建任务的类。这个类应该实现两个函数：

+   `**generate_prompts(docs: Iterable[Doc]) -> Iterable[str]**`：一个函数，它接受一个 spaCy `[Doc](https://spacy.io/api/doc)` 对象的列表，并将其转换为一个提示的列表

+   `**parse_responses(docs: Iterable[Doc], responses: Iterable[str]) -> Iterable[Doc]**`：一个将 LLM 输出解析为 spaCy `[Doc](https://spacy.io/api/doc)` 对象的函数

`**generate_prompts**` 将使用我们的 Jinja 模板，而 `**parse_responses**` 将为我们的 Doc 添加上下文属性。这是 `QuoteContextExtractTask` 类：

```py
from pathlib import Path
from spacy_llm.registry import registry
import jinja2
from typing import Iterable
from spacy.tokens import Doc

TEMPLATE_DIR = Path("templates")

def read_template(name: str) -> str:
    """Read a template"""

    path = TEMPLATE_DIR / f"{name}.jinja"

    if not path.exists():
        raise ValueError(f"{name} is not a valid template.")

    return path.read_text()

class QuoteContextExtractTask:
  def __init__(self, template: str = "quotecontextextract.jinja", field: str = "context"):
    self._template = read_template(template)
    self._field = field

  def _check_doc_extension(self):
     """Add extension if need be."""
     if not Doc.has_extension(self._field):
         Doc.set_extension(self._field, default=None)

  def generate_prompts(self, docs: Iterable[Doc]) -> Iterable[str]:
    environment = jinja2.Environment()
    _template = environment.from_string(self._template)
    for doc in docs:
        prompt = _template.render(
            text=doc.text,
        )
        yield prompt  

  def parse_responses(
      self, docs: Iterable[Doc], responses: Iterable[str]
  ) -> Iterable[Doc]:
    self._check_doc_extension()
    for doc, prompt_response in zip(docs, responses):      
      try:
        setattr(
            doc._,
            self._field,
            prompt_response.replace("Context:", "").strip(),
        ),
      except ValueError:
        setattr(doc._, self._field, None)

    yield doc
```

现在我们只需将任务添加到 spacy-llm 的 `llm_tasks` 注册表中：

```py
@registry.llm_tasks("my_namespace.QuoteContextExtractTask.v1")
def make_quote_extraction() -> "QuoteContextExtractTask":
    return QuoteContextExtractTask()
```

## **3. config.cfg 文件**

我们将使用 OpenAI 的 GPT-3.5 模型。spacy-llm 为此提供了一个模型，所以我们只需确保秘密密钥作为环境变量可用：

```py
export OPENAI_API_KEY="sk-..."
export OPENAI_API_ORG="org-..."
```

为了构建运行管道的 `nlp` 方法，我们将使用 spacy-llm 的 `assemble` 方法。该方法从 `.cfg` 文件中读取。文件应引用 GPT-3.5 模型（它已在注册表中）和我们创建的任务：

```py
[nlp]
lang = "en"
pipeline = ["llm"]
batch_size = 128

[components]

[components.llm]
factory = "llm"

[components.llm.model]
@llm_models = "spacy.GPT-3-5.v1"
config = {"temperature": 0.1}

[components.llm.task]
@llm_tasks = "my_namespace.QuoteContextExtractTask.v1"
```

## **4. 运行管道**

现在我们只需将所有内容组合在一起并运行代码：

```py
import os
from pathlib import Path

import typer
from wasabi import msg

from spacy_llm.util import assemble
from quotecontextextract import QuoteContextExtractTask

Arg = typer.Argument
Opt = typer.Option

def run_pipeline(
    # fmt: off
    text: str = Arg("", help="Text to perform text categorization on."),
    config_path: Path = Arg(..., help="Path to the configuration file to use."),
    verbose: bool = Opt(False, "--verbose", "-v", help="Show extra information."),
    # fmt: on
):
    if not os.getenv("OPENAI_API_KEY", None):
        msg.fail(
            "OPENAI_API_KEY env variable was not found. "
            "Set it by running 'export OPENAI_API_KEY=...' and try again.",
            exits=1,
        )

    msg.text(f"Loading config from {config_path}", show=verbose)
    nlp = assemble(
        config_path
    )
    doc = nlp(text)

    msg.text(f"Quote: {doc.text}")
    msg.text(f"Context: {doc._.context}")

if __name__ == "__main__":
    typer.run(run_pipeline)
```

然后运行：

```py
python3 run_pipeline.py "We must balance conspicuous consumption with conscious capitalism." ./config.cfg
>>> 
Quote: We must balance conspicuous consumption with conscious capitalism.
Context: Business ethics.
```

如果你想更改提示，只需创建另一个 Jinja 文件，并以与第一次创建相同的方式创建 `my_namespace.QuoteContextExtractTask.v2` 任务。如果你想更改温度，只需在 `config.cfg` 文件中更改参数。不错，对吧？

# 最后的思考

处理 OpenAI REST 请求的能力及其直接存储和版本控制提示的方法是我最喜欢 spacy-llm 的地方。此外，该库提供了一个用于缓存每个文档的提示和响应的缓存功能，一个为少量示例提示提供示例的方法，以及日志记录功能等。

你可以在这里查看今天的完整代码：[https://github.com/dmesquita/spacy-llm-elegant-prompt-versioning](https://github.com/dmesquita/spacy-llm-elegant-prompt-versioning)。

一如既往，谢谢你的阅读！
