# 帮助初创公司创始人找到最佳孵化器：一个端到端的项目。

> 原文：[`towardsdatascience.com/building-a-matching-tool-to-help-start-up-founders-find-the-best-incubators-an-end-to-end-bd65c41175bd`](https://towardsdatascience.com/building-a-matching-tool-to-help-start-up-founders-find-the-best-incubators-an-end-to-end-bd65c41175bd)

## 一个自由职业项目的演示，使用 Python、Pinecone、FastAPI、Pydantic 和 Docker 提出最佳孵化器的建议

[](https://medium.com/@jeremyarancio?source=post_page-----bd65c41175bd--------------------------------)![Jeremy Arancio](https://medium.com/@jeremyarancio?source=post_page-----bd65c41175bd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bd65c41175bd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd65c41175bd--------------------------------) [Jeremy Arancio](https://medium.com/@jeremyarancio?source=post_page-----bd65c41175bd--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd65c41175bd--------------------------------) ·15 min 阅读·2023 年 11 月 26 日

--

[Harness](https://www.joinharness.com/)，一个致力于帮助创始人创业的初创公司，找到我开发了一个帮助其社区找到最合适孵化器的工具：**匹配工具**。

在本文中，我们将介绍这个项目的不同阶段，从解决方案设计到交付。

![](img/2488d4d84449aeb3c7adad531d8346bf.png)

[Rames Quinerie](https://unsplash.com/@ramesquinerie?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上的照片

# 背景

该公司及其联合创始人希望创建一个工具，使他们的初创公司创始人社区能够找到全球最佳的孵化器和加速器。

为了实现这一目标，他们手动从孵化器网站收集数据，包括位置、各种要求、资金机会等详细信息。此外，他们还利用了一个活跃的创始人社区。

利用孵化器和其社区的数据，他们需要找到一种方法来检索基于初创公司信息的**前 k 名孵化器**。

挑战接受。

# 解决方案设计

## 概述

乍一看，这个项目看起来像是一个推荐系统，比如 Netflix 或 Amazon 用于向用户推荐最佳的系列或产品。通过用户行为，如点击、评论或点赞，公司可以预测并推荐最合适的产品。

然而，在这种特定情况下，我们缺乏关于创始人偏好的任何先前数据。因此，在这种情况下构建推荐系统是不可行的。

另一种方法可以涉及将**孵化器**和初创企业数据嵌入到向量空间中进行*相似性搜索*。简而言之，这种方法涉及测量向量之间的距离，以确定最接近给定初创企业的孵化器。

但这种方法在这种情况下有很多缺陷。

孵化器具有我所称的*硬标准*，这些因素可能导致任何不符合要求的初创企业被立即拒绝。这可能包括如果孵化器要求混合或面对面的出席，位置不在同一城市，或缺乏资金。

那些*硬标准*会使*嵌入*（数据的向量表示）在这种情况下不是一个好的方法。例如，一个孵化器可能完全匹配一个初创企业，但如果申请未开放，则不应向创始人推荐这个孵化器。

这些*硬标准*的存在使得在整个数据集上使用嵌入不适合这种情况。例如，即使一个孵化器与初创企业完美对接，如果当前没有开放申请，也不适合向创始人推荐。

最后，即使大多数特征可以转化为数值（*融资金额，接受的前期融资金额，初创企业收入预期*）或分类（*国家，出席要求，MVP 准备好*），某些特征由于其多样性却无法分类：

+   ***融资工具：*** *赠款，140k$，股权（SAFE），…*

+   ***行业重点：*** *医疗科技，人工智能，金融科技，…*

此外，这些特征必须在匹配工具中考虑，但可能不会被视为*硬标准*。例如，创始人可能会选择一个专注于*健康科技*的孵化器，并且仍然愿意接受一个*生物技术*初创企业。

## 混合方法

为了解决这些问题，我们来考虑最佳的两全其美的方案。

如果某些孵化器的*硬* *标准*会导致不匹配，可以考虑根据初创企业的信息*筛选*这些孵化器。经过缩小潜在匹配的列表后，我们可以使用剩余的*软标准*进行*相似性搜索*，将其转化为统一的文本并嵌入到向量中。

好消息是：**Pinecone** 向其向量数据库提供了这一功能！

[](https://www.pinecone.io/learn/vector-search-filtering/?source=post_page-----bd65c41175bd--------------------------------) [## 向量搜索中的缺失 WHERE 子句 | Pinecone]

### 向量相似性搜索使得庞大的数据集可以在几分之一秒内进行检索。然而，尽管其卓越的表现和…

www.pinecone.io](https://www.pinecone.io/learn/vector-search-filtering/?source=post_page-----bd65c41175bd--------------------------------)

项目路径现在已经明确：

1.  孵化器的数据需要**预处理**以便**过滤***硬标准*和**相似性搜索***软标准*。然后将数据存储在 Pinecone 向量数据库中。

1.  *过滤对象*必须根据 Pinecone Python 库构建。此外，它还需要保持**灵活**，以便客户可以轻松修改标准而无需修改算法。

1.  *软标准*需要统一，并转换为嵌入格式，使用适当的*嵌入模型*。

1.  数据是关键，我们需要为启动信息实现数据验证步骤，也需要为*upserting*新的孵化器数据到向量数据库中进行验证。我们将使用[**Pydantic**](https://docs.pydantic.dev/latest/)。

1.  该算法将作为**API**在**docker 容器**中提供。我们将使用 FastAPI 并创建一个 Dockerfile，以确保代码在任何环境下都能正常工作。

1.  *额外说明*：**单元测试**和**集成测试**将被设置，以便任何人可以以 CI/CD 方式修改代码。

所有这些点都与利益相关者讨论过并被接受了。

我们准备出发了！

# **数据预处理**

我收到了孵化器的解析信息在一个电子表格中。乍一看，数据相当混乱：*手动提取没有明确的过程，字符串而不是布尔值，同一特征内的一致性缺乏，……*

需要做大量的工作来使数据可用。

![](img/9d5f5a7fd0af5d42c3e37054d0823421.png)

相同特征的不同日期“格式”

关于数据集中*空值*，每个特征都是独立处理的。

例如，*出勤要求*可能是*面对面、混合*或*远程*。在这种情况下，缺少此特征的孵化器被认为是要求*面对面*出勤。

另一个例子是启动公司的*注册*：*注册*或*未注册*。与其选择这两个类别中的一个，不如添加第三个类别作为默认值：*无论如何*。这将在过滤阶段有用，不仅选择主要类别之一，还选择所有未明确说明的孵化器。我们将在**过滤**部分讨论这个问题。

最终，我们将*软标准*转化为一个单一的提示以嵌入。为此，我们简单地使用了一个提示模板。如果在项目后期需要添加新特性，只需更新该提示即可。

```py
# config.py
class Templates:
    embedding_template = """Industries accepted:
{industry_focus}

Funding vehicle:
{funding_vehicle}"""

Templates.embedding_template.format(
    industry_focus=industry_focus, 
    funding_vehicle=funding_vehicle
)
```

一旦孵化器数据经过预处理，就会导出到**Pinecone 向量数据库**中。

# 使用孵化器数据构建向量数据库

Pinecone 提供了一个易于使用的 Python SDK，用于插入、修改和查询向量数据库中的数据。

在我们的案例中，我们需要*upsert*（插入或更新）一个表示*软标准*的向量，此外还有*硬标准*。

根据 Pinecone，数据应遵循以下格式：

```py
# List[(id, vector, metadata)]
[
  ("A", [0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1], {"genre": "comedy", "year": 2020}),
  ("B", [0.2, 0.2, 0.2, 0.2, 0.2, 0.2, 0.2, 0.2], {"genre": "documentary", "year": 2019}),
  ("C", [0.3, 0.3, 0.3, 0.3, 0.3, 0.3, 0.3, 0.3], {"genre": "comedy", "year": 2019}),
  ("D", [0.4, 0.4, 0.4, 0.4, 0.4, 0.4, 0.4, 0.4], {"genre": "drama"}),
  ("E", [0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5], {"genre": "drama"})
]
```

## 嵌入

有许多模型，无论是开源的还是非开源的，可以将文本嵌入到向量表示中。在这种情况下，我们将使用[**sentence-bert**](https://www.sbert.net/)，一个旨在利用开源嵌入模型的 Python 库。你可以查看我之前的文章，其中描述了它的工作原理：

[](https://medium.com/@jeremyarancio/semantic-search-using-sequence-bert-2116dabecfa3?source=post_page-----bd65c41175bd--------------------------------) [## 使用 Sentence-BERT 进行语义搜索

### 随着大型语言模型推动的 AI 最新趋势和 ChatGPT（OpenAI）的成功，企业已经…

medium.com](https://medium.com/@jeremyarancio/semantic-search-using-sequence-bert-2116dabecfa3?source=post_page-----bd65c41175bd--------------------------------)

这个库的简洁性使其成为构建第一个匹配工具版本的良好选择。

```py
# pip install -U sentence-transformers
from sentence_transformers import SentenceTransformer

class SentenceTransformersEmbedding:
    """Embedding using the SentenceTransformers library (https://www.sbert.net)"""

    def __init__(
            self,
            model_name: str = "all-MiniLM-L6-v2"
        ) -> None:
        self.model = SentenceTransformer(model_name)

    def get_embeddding(self, texts: Union[str, List[str]]) -> List:
        # We need to return a list instead of an array for Pinecone
        return self.model.encode(texts).tolist()
```

## 准备并导出孵化器数据。

要将新的孵化器数据插入到向量数据库中，我们按照 Pinecone 文档中介绍的方式准备数据。

```py
def prepare_from_payload(self, incubators: List[Incubator]) -> List[Tuple[str, List[float], Mapping[str, Any]]]:
        """Prepare payload containing incubators data to export to Pinecone vector database.

        Args:
            incubators (List[Incubator]): List of Incubator containing the incubator information that will be sent to Pinecone. 

        Returns:
            List[Tuple[str, List[float], Mapping[str, Any]]]: Prepared data for Pinecone. Check official documentation (https://docs.pinecone.io/docs/metadata-filtering#inserting-metadata-into-an-index). 
        """
        data = []
        for incubator in incubators:
            metadata = {key: value for key, value in incubator.model_dump(exclude={"incubator_id"}).items()}
            additional_information_text = Templates.embedding_template.format(incubator.industry_focus, incubator.funding_vehicle)
            embedding = self.embedding_generator.get_embeddding(additional_information_text)
            incubator_data = (incubator.incubator_id, embedding, metadata)
            data.append(incubator_data)
        return data
```

正如你在代码中看到的，我们使用 Pydantic 的`BaseModel`创建了一个`Incubators`对象。

```py
from pydantic import BaseModel
from datetime import date

class Incubator(BaseModel):
    incubator_id: str
    name: str 
    application_open: int = 1
    next_deadline: date = date.max
    funding_amount: int = 0 # Maximal amount the incubator can fund
    attendance_requirement: Literal["in-person", "remote", "hybrid"] = "in-person"
    incorporation: Literal["incorporated", "unincorporated"] = "regardless"
    minimum_cofounders: int = 0
    minimum_employees: int = 0
    previous_funding_accepted: int = 1
    ...

class Incubators(BaseModel):
    incubators: List[Incubator]
```

这个`BaseModel`类有两个主要好处。它不仅确保数据符合我们算法和查询的正确格式，而且还定义了孵化器数据的默认模式。

```py
print(Incubator(
  incubator_id="id", 
  name="incubator_on_fire",
  industry_focus="Health tech",
  funding_vehicle="Grant"
))

# Output
{
    'id': 'id'
    'name': 'incubator_on_fire', 
    'application_open': 1, 
    'next_deadline': datetime.date(9999, 12, 31), 
    'funding_amount': 0, 
    'attendance_requirement': 'in-person', 
    'incorporation': 'regardless', 
    'minimum_cofounders': 0, 
    'minimum_employees': 0, 
    'woman_founders': 0,
    'student_founders': 0,
    'industry_focus': 'Health tech',
    'funding_vehicle': 'Grant'
     ...
}
```

孵化器数据随后使用 Pinecone Python 库导出到向量数据库。为了让其他开发人员能够在应用程序的整体架构中实现这段代码，我们使用了 FastAPI：

```py
import os

from fastapi import FastAPI, HTTPException
from app.models import Incubators

from features import FeatureEngine
from embedding import SentenceTransformersEmbedding

PINECONE_API_KEY = os.getenv("PINECONE_API_KEY")
ENVIRONMENT = os.getenv("ENVIRONMENT")

app = FastAPI()

@app.post("/upsert")
def upsert(incubators: Incubators):
    try:
        embedding_generator = SentenceTransformersEmbedding()
        feature_engine = FeatureEngine(embedding_generator=embedding_generator)
        data = feature_engine.prepare_from_payload(incubators=incubators.incubators)
        vectors = [pinecone.Vector(id=id, values=values, metadata=metadata) for id, values, metadata in data]
        pinecone.init(api_key=PINECONE_API_KEY, environment=ENVIRONMENT)
        index = pinecone.Index(index_name=VectorDatabaseConfig.index_name)
        index.upsert(vectors=vectors)
    except Exception as e:
        raise HTTPException(status_code=500, detail=str(e))
```

数据导出后，我们能够开始使用初创公司信息查询向量数据库。

# 构建匹配算法

该算法在两个步骤中执行 top-k 孵化器的检索：

1.  过滤掉不相关的孵化器，

1.  使用嵌入向量执行相似性搜索。

我们还需要确保算法足够灵活，以便在项目后期添加或更改任何数据而不触及算法的核心。

那么如何做到这一点呢？

这是我想到的解决方案：

Pinecone 使用与 MongoDB 相同的语言来过滤数据库[[source](https://docs.pinecone.io/docs/metadata-filtering#inserting-metadata-into-an-index)]。它看起来是这样的：

```py
import pinecone

pinecone.init(api_key=PINECONE_API_KEY, environment=ENVIRONMENT)
index = pinecone.Index("example-index")
index.query(
    vector=[0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1],
    filter={
        "genre": {"$eq": "documentary"},
        "year": 2019
    },
    top_k=5,
    include_metadata=True
)
```

过滤映射也可以更为复杂：

```py
# $in statement
{
  "genre": { "$in": ["comedy", "documentary", "drama"] }
}

# Multi criteria
{
  "genre": { "$eq": "drama" },
  "year": { "$gte": 2020 }
}

# $or statement
{
  "$or": [{ "genre": { "$eq": "drama" } }, { "year": { "$gte": 2020 } }]
}
```

通过在查询中实现初创公司信息，我们能够检索出符合要求的孵化器：`$gte` — *大于，* `$eq` — *等于，* 等等*…*

但有些情况更为复杂。

例如，*位置*和*出席要求*是配对使用的。如果一个孵化器只接受*混合*或*面对面*，那么初创公司逻辑上应该位于与孵化器相同的城市/国家。但匹配工具也应该展示所有接受*远程*的孵化器，无论初创公司位于何处。

另一个示例：假设初创公司由 *女性创始人* 领导，或者初创公司已经构建了 *MVP*。因此，具有此陈述为真的初创公司应被提议孵化器，该孵化器仅接受女性创始人，或要求 MVP，此外还包括所有其他孵化器。

正如这些示例所示，标准可以分为不同的“*模板*”称为 `Criterion`。这些标准模板将用于构建 `filter_object`，这是 Pinecone/MongoDB 使用的过滤映射。

使用 Python 类，它看起来是这样的：

```py
class Criterion(ABC):
        """Incubators criterion template used to build the filter object.
        Each subclass of this class is a specific rule case used incubators and start-ups data.

        Args:
                name (str): incubators metadata name as it is in the vectordatabase.
        """
        def __init__(
                self,
                name: str,
        ) -> None:
                self.name = name

class NormalCriterion(Criterion):
    """Basic rule for creating to filter data based on this criterion.
    It takes this form:

    ```python

    criterion.name = {criterion.condition_type: payload[criterion.startup_correspondance]}

    ```py
    With `payload` the start-up information.

    Example:
    ```python

    max_funding_amount = {$gte: 10000}

    ```py
    This will filter all incubators with a maximal funding capacity greater than 10000.

    Args:
        condition_type (str): comparison element like "$eq" (equal), "$lte" (lower than or equal), "$gt" (greater than)
The complete list is available on the pinecone documentaton (https://docs.pinecone.io/docs/metadata-filtering#metadata-query-language).
        startup_correspondance (str): start-up correspondance from the payload
    """
    def __init__(
        self, 
        name: str, 
        condition_type: str,
        startup_correspondance: str
    ) -> None:
        self.condition_type = condition_type
        self.startup_correspondance = startup_correspondance
        super().__init__(name=name)
```

父类对象 `Criterion` 用于构建多个子类，表示每种情况。如果我们以上面介绍的 *女性创始人/MVP* 情况为例：

```py
class InclusiveCriterion(Criterion):
    """If condition validated, considers all.

    Example:

    Being women founders should match women-founders-only incubators, but also the other incubators.
    Same for MVP, Ready_to_pay, Students founders, etc...

    ```

    if woman_founders_startup (False) != condition (True):

        {"woman_founders_incubator": {"$eq": woman_founders_startup_value (false)}}

    参数：

        condition_type (str): 比较元素，如 "$eq"（等于），"$lte"（小于或等于），"$gt"（大于）

    完整列表可在 pinecone 文档中找到 (https://docs.pinecone.io/docs/metadata-filtering#metadata-query-language)。

        startup_correspondance (str): 从 payload 中的初创公司对应（见 matching_tool/app/models.py）

        condition (bool): 如果条件得到验证，考虑标准

    """

    def __init__(

            self,

            name: str,

            condition_type: str,

            startup_correspondance: str,

            condition: bool

        ) -> None:

        self.condition_type = condition_type

        self.startup_correspondance = startup_correspondance

        self.condition = condition

        super().__init__(name)

```py

Those `Criterion` classes are used along their respective method to build the `filter_object` :

```

def normal_case(

    payload: Mapping,

    criterion: NormalCriterion,

    filter_object: Dict

) -> Dict:

    """最简单的情况：取启动值（资金额，之前的资助等）并在 vectordatabase 中按此过滤

    condition_type（$eq, $lte, $gte, $gt, ...）

    参数：

        payload (Mapping): 启动信息

        criterion (NormalCriterion): 普通标准

        filter_object (Dict): 在 vectordatabase 查询期间的元数据过滤器

    返回：

        Dict:

        ```pypython
        {metadata_name: {condition_type: startup_value}}
        ```

    """

    filter_object[criterion.name] = {

        criterion.condition_type: payload[criterion.startup_correspondance]

    }

    return filter_object

```py

```

def inclusive_case(

        payload: Mapping,

        criterion: InclusiveCriterion,

        filter_object: Dict

) -> Dict:

    """包容性案例：为包容性案例准备过滤器：女性创始人，学生创始人，MVP，其他费用...

    如果条件满足（初创公司中的女性创始人 == 1），因此不要考虑过滤标准 => 获取所有（仅接受女性的孵化器和其他所有孵化器）

    否则：只考虑没有女性创始人的孵化器 => {women_founders: {"$eq: 0}}

    参数：

        payload (Mapping): 启动信息

        criterion (NormalCriterion): 普通标准

        filter_object (Dict): 在 vectordatabase 查询期间的元数据过滤器

    """

    if payload[criterion.startup_correspondance] != criterion.condition:

        filter_object[criterion.name] = {criterion.condition_type: payload[criterion.startup_correspondance]}

    return filter_object

```py

All these `Criterion` classes are stored inside another class object we call `Criteria` . This class acts as a repository of all the criteria to consider for filtering the database and can be easily modified to add or remove any criterion.

```

class Criteria:

    """使用 Criterion 模板进行过滤。

    使用适当的 Criterion 模板添加或删除任何条件。

    """

    country = DependendantCriterion(

        name="country",

        condition_type="$eq",

        startup_correspondance="country"

    )

    city = DependendantCriterion(

        name="city",

        condition_type="$eq",

        startup_correspondance="city"

    )

    attendance_requirement = ConditionalCriterion(

        name="attendance_requirement",

        condition=["remote"],

        true_criteria=[],

        else_criteria=[country, city]

    )

    minimum_cofounders = NormalCriterion(

        name="minimum_cofounders",

        condition_type="$lte",

        startup_correspondance="n_cofounders"

    )

    working_product_requirement = InclusiveCriterion(

        name="working_product_requirement",

        condition_type="$eq",

        startup_correspondance="working_product",

        condition=True

    )

    woman_founders = InclusiveCriterion(

        name="woman_founders",

        condition_type="$eq",

        startup_correspondance="woman_founders",

        condition=True

    )

...

```py

Once all the criteria are added to the `Criteria` object, we iterate over it and build the `filter_object` based on the start-up information. For each `Criterion` case, we add a filter element to the `filter_object` .

```

class Matcher:

    "从向量数据库中检索与初创公司信息匹配的孵化器。"

    def __init__(

        self,

        index: Index,

        criteria: Criteria = Criteria(),

        embedder: Embedding = SentenceTransformersEmbedding(),

    ) -> None:

        """

        参数:

            index (Index): 向量数据库索引 / 表

            criteria (Criteria, optional): 孵化器元数据以进行搜索。默认为 Criteria()。

            embedder (Embedding, optional): 嵌入方法，用于将文本转换为向量表示

        语义搜索。默认为 SentenceTransformersEmbedding()。

        """

        self.index = index

        self.criteria = criteria

        self.embedder = embedder

def _get_filter(

        self,

        payload: Dict[str, Any],

    ) -> Mapping[str, Any]:

        """构建用于在 Pinecone 上过滤元数据的字典。

        过滤对象应遵循以下格式。有关更多信息，请查看官方 Pinecone 文档：

        https://docs.pinecone.io/docs/metadata-filtering

        参数:

            payload (Dict[str, Any]): 初创公司信息

        返回:

            Mapping[str, Any]: 过滤对象

        ```pybash
          filter={
              'application_open': 1,
              '$or': [{'attendance_requirement': {'$in': ['remote']}}, {'country': {'$eq': 'estonia'}, 'city': {'$eq': 'tallinn'}}],
              'funding_amount': {'$gte': 12000},
              'other_costs': {'$eq': 0},
              'previous_funding_accepted': {'$eq': 1},
              'working_product_requirement': {'$eq': 0}
          }
          ```

        """

        # 初始过滤器

        filter_object = {"application_open": 1}

        criteria = self.criteria.get_criteria()

        for criterion in criteria:

            if isinstance(criterion, NormalCriterion):

                if check_correspondance_in_payload(payload, criterion):

                    filter_object = normal_case(

                        payload=payload,

                        criterion=criterion,

                        filter_object=filter_object,

                    )

            if isinstance(criterion, InclusiveCriterion):

                if check_correspondance_in_payload(payload, criterion):

                    filter_object = inclusive_case(

                        payload=payload,

                        criterion=criterion,

                        filter_object=filter_object,

                    )

            if isinstance(criterion, ConditionalCriterion):

                if check_dependencies(payload, conditional_criterion=criterion):

                    filter_object = conditional_case(

                        payload=payload,

                        criterion=criterion,

                        filter_object=filter_object,

                    )

            if isinstance(criterion, DefaultCriterion):

                if check_correspondance_in_payload(payload, criterion):

                    filter_object = default_case(

                        payload=payload,

                        criterion=criterion,

                        filter_object=filter_object,

                    )

        return filter_object

```py

As you can see in the code, we built four different `Criterion` templates to consider many cases: `NormalCriterion` , `InclusiveCriterion` , `ConditionalCriterion` , and `DefaultCriterion` .

In the future of the project, more categories can be added without changing the algorithm core, making it **customizable**.

Once the `filter_object` is created with the `_get_filter()` method, the vector database can be queried with the Pinecone `index.query()` method:

```

matches = self.index.query(

            vector=embedding,

            filter=filter_object,

            include_metadata=True,

            top_k=top_k

        )

```py

The matching tool algorithm is created. We then served it through an API endpoint using FastAPI and Pydantic.

```

@app.post("/match")

def search(payload: StartUp, top_k: int = 5) -> Mapping:

    LOGGER.info("开始匹配。")

    try:

        payload = preprocess_payload(dict(payload))

        pinecone.init(api_key=PINECONE_API_KEY, environment=ENVIRONMENT)

        index = pinecone.Index(index_name=VectorDatabaseConfig.index_name)

        matching_tool = Matcher(index=index)

        matches = matching_tool.match(payload=payload, top_k=top_k)

        return matches

    except Exception as e:

        LOGGER.error(f"{str(e)}")

        raise HTTPException(status_code=500, detail=str(e))

```py

As `Incubator` built with Pydantic, we created the object `Startup` object to ensure the start-up data comes in the right format:

```

class StartUp(BaseModel):

    country: Optional[str] = None

    city: Optional[str] = None

    funding_amount: Optional[int] = None

    n_cofounders: Optional[int] = None

    n_employees: Optional[int] = None

    woman_founders: Optional[bool] = None

    industry_focus: str = ""

    funding_vehicle: str = ""

    ...

```py

An advantage of using Pydantic with FastAPI is that the API payload (here the start-up information) doesn’t have to be complete. For example, if there is missing information, Pydantic will automatically replace it with its default value, or not consider it at all in the algorithm (defined by the `None` statement).

The core of the API is now set up. We can now make the code ready for shipment using Docker and CI/CD with Pytest.

# Delivering the API

## Integration test with Pytest

During the development of the code, unitests and integration tests were created to ensure no modifications would break the algorithm.

Furthermore, creating the test algorithms not only provides a CI/CD process but also gives my client indications about how the code is supposed to work.

To build an integration test with FastAPI, we used the `TestClient` provided within the library. It uses the `httpx` library instead of `requests` making a call to the API.

The data used as validation of the code is stored in an external JSON file `data/integration_test_data.json`

```

# integration_test.py

# pip install httpx

from fastapi.testclient import TestClient

URL = "/match"

client = TestClient(app)

DATA_PATH = Path(os.path.realpath(__file__)).parent / "data/integration_test_data.json"

with open(DATA_PATH, 'r') as data:

    DATA = json.load(data)

def test_match():

    for test in DATA["match_tests"]:

        response = client.post(URL, json=test["payload"])

        assert response.status_code == 200

        payload: Dict = json.loads(response.content)

        match_ids = [match["incubator_id"] for match in payload.values()]

        for expected_id in test["expected"]:

            assert expected_id in match_ids

```py

![](img/22eebeee23c8c90f06356030ea650f9f.png)

Run Pytest on all “test” scripts

Once all tests passed, we created the **Dockerfile** to containerize the code.

## Docker

To create a Docker container, we simply create a Dockerfile within the repository:

```

FROM python:3.9

WORKDIR /src

ENV PYTHONPATH=/src

COPY requirements.txt requirements.txt

COPY matching_tool/ .

RUN pip install -r requirements.txt

EXPOSE 8001

CMD ["uvicorn", "app.api:app", "--host", "0.0.0.0", "--port", "8001"]

```py

![](img/7e49fcd169430e8cf431ed67184378f2.png)

Structure of the repository

Here’s what each line does:

*   `FROM` import the docker image from the hub with all the basic elements required to run Python 3.9 in this case.
*   `WORKDIR` specifies the location of the code within the container
*   `ENV PYTHONPATH = /src` specifies which directory Python has to look into to import internal modules.
*   `COPY` copies the files in the attributed directory.
*   `RUN` is triggered during the Docker image creation, and before the Docker container build. This way, `pip install -r requirements.txt` only runs once.
*   `EXPOSE` exposes a container port of our choice, here’s the port 8001\. The API port should match the container port.
*   `CMD ['uvicorn”, “app.api.app”, “ — host”, “0.0.0.0”, “ — port 8001]`runs the FastAPI API. It is important here to indicate the host as `0.0.0.0` to enable calls from outside the container.

We then created the Docker image by running in the CLI:

```

docker build -t matching-tool:latest -f Dockerfile .

```py

Finally, to run the container, one has just to write:

```

docker run -p 8001:8001 --name matching-tool matching-tool

```

一旦容器运行，任何人都可以通过端口 8001 调用 API。也可以将 Docker 容器部署到任何云提供商，**使匹配工具立即生效**。

项目已准备好交付。

# 结论

在这篇文章中，我分享了我为一家美国初创公司进行的实际项目。

根据我所提供的数据，以及与利益相关者的多次迭代，我开发了一个工具，帮助初创企业创始人找到最适合他们需求的孵化器。我逐步解释了我所遵循的过程和解决此问题的不同策略。

下一步将是将此算法嵌入到整体应用中，并开始收集用户数据。这将启动任何机器学习功能所需的**飞轮**。确实，从这些代表用户偏好的数据中，将能够构建一个会随时间学习的推荐系统，并为当前和未来的创始人提供最佳输出。

与[Harness](https://www.joinharness.com/)在这个项目中合作非常愉快。我祝愿他们一切顺利。他们知道未来有合作的机会可以随时联系我。

如果你喜欢这篇文章，[**欢迎订阅我的新闻通讯**](https://medium.com/@jeremyarancio/subscribe)**。我分享有关 NLP、MLOps 和创业的内容。**

你可以通过[Linkedin](https://www.linkedin.com/in/jeremy-arancio/)联系我，或者查看我的[Github](https://github.com/JeremyArancio)。

如果你是企业并希望将机器学习应用到你的产品中，你也可以[**预约通话**](https://topmate.io/jeremyarancio/555697)。

再见，祝编码愉快！
