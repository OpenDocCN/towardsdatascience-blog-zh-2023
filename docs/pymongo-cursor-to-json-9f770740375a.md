# 如何修复 TypeError: ObjectId 不是 JSON 可序列化的

> 原文：[`towardsdatascience.com/pymongo-cursor-to-json-9f770740375a`](https://towardsdatascience.com/pymongo-cursor-to-json-9f770740375a)

## 将 Mongo 游标转换为 Python 中的 JSON 对象

[](https://gmyrianthous.medium.com/?source=post_page-----9f770740375a--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----9f770740375a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9f770740375a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9f770740375a--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----9f770740375a--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9f770740375a--------------------------------) ·4 分钟阅读·2023 年 1 月 7 日

--

![](img/eaab2de46fd1a4afdef1ebe34e4c49af.png)

照片由 [Ciprian Boiciuc](https://unsplash.com/@ciprian?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，出处 [Unsplash](https://unsplash.com/photos/TrNSWatUW5g?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

欢迎来到我们的教程，介绍如何将 PyMongo 游标序列化为 JSON。在本文中，我们将讨论如何使用自定义 `JSONEncoder` 正确处理 `ObjectId` 和 `datetime` 对象以及其他对象。

在使用 PyMongo 时，一个常见的任务是将数据序列化以便存储或通过网络传输。在本教程中，我们将探讨如何将 PyMongo 游标（这是一个常用于存储 MongoDB 查询结果的数据结构）序列化为 JSON 格式。

常见的错误是 `TyperError`：

```py
TypeError: ObjectId('') is not JSON serializable
```

我们还将深入探讨如何正确处理复杂数据类型，如 `ObjectId` 和 datetime 对象，这些对象不能直接序列化为 JSON。我们将向您展示如何使用自定义 `JSONEncoder` 正确处理这些对象及您 PyMongo 游标中的其他自定义对象类型。

所以，如果您想了解如何将 PyMongo 游标序列化为 JSON 并处理复杂数据类型，请继续阅读！

## 创建自定义 `JSONEncoder`

`JSONEncoder` —— 这是标准库 `json` 模块的一个成员 —— 是一个可扩展的 JSON 编码器，用于 Python 数据结构。默认情况下，它支持以下序列化：

```py
+-----------------------------------------+--------+
|   Python                                | JSON   |
+-----------------------------------------+--------+
| dict                                    | object |
| list, tuple                             | array  |
| str                                     | string |
| int, float, int & float-derived Enums   | number |
| True                                    | true   |
| False                                   | false  |
| None                                    | null   |
+-----------------------------------------+--------+
```

这意味着每当观察到不同数据类型的对象（未列在上述表格中）时，将引发 `TypeError`。

在处理 Mongo 中的文档时，每个文档默认都会有一个分配的`_id`，这对应于集合中每个文档的唯一标识符。现在每当你查询 Mongo 集合时，将返回一个游标，该游标包含（指向）检索到的文档，每个文档还将有一个`ObjectId`类型的`_id`字段。

因此，如果你尝试使用默认的`JSONEncoder`来序列化这些文档，你将会遇到在本教程介绍中提到的错误：

```py
TypeError: ObjectId('') is not JSON serializable
```

因此，为了能够序列化 PyMongo 游标中包含的这些对象，我们需要扩展默认的`JSONEncoder`，以便它能够按照我们希望的方式正确处理这些数据类型。为此，我们还需要实现`default`方法以返回我们希望的映射，如文档中所述。

> 要扩展以识别其他对象，请子类化并实现一个`[default()](https://docs.python.org/3/library/json.html#json.JSONEncoder.default)`方法，该方法返回一个可序列化的对象`o`（如果可能），否则应调用超类实现（以引发`[TypeError](https://docs.python.org/3/library/exceptions.html#TypeError)`）。
> 
> — [Python 文档](https://docs.python.org/3/library/json.html#json.JSONEncoder)

在我们的自定义`JSONEncoder`中，我将把任何`bson.ObjectId`和`datetime.datetime`实例序列化为`str`。根据你自己 Mongo 游标中的文档，你可能需要指定并处理额外（或更少）的数据类型。

```py
import json
from datetime import datetime
from typing import Any

from bson import ObjectId

class MongoJSONEncoder(json.JSONEncoder):
    def default(self, o: Any) -> Any:
        if isinstance(o, ObjectId):
            return str(o)
        if isinstance(o, datetime):
            return str(o)
        return json.JSONEncoder.default(self, o)
```

## 使用 MongoJSONEncoder 编码 Mongo 游标

现在我们已经扩展了默认的`JSONEncoder`，使其能够编码`bson.ObjectId`和`datetime.datetime`类型的对象，我们现在可以编码 Mongo 游标了。

```py
data_json = MongoJSONEncoder().encode(list(cursor))
```

## 从 JSON 对象创建 Python 对象

最后，如果你希望将新创建的 JSON 对象转换为 Python 对象（即包含文档值的键值对的字典列表），你只需要调用`json.loads()`函数：

```py
data_obj = json.loads(data_json)
```

## 最终思考

在本教程中，我们学习了如何将 PyMongo 游标序列化为 JSON 并正确处理复杂的数据类型，如`ObjectId`和`datetime`对象。我们通过创建一个扩展了默认`JSONEncoder`并实现了`default()`方法的自定义`JSONEncoder`来完成这一任务。

然后我们使用这个自定义编码器来编码 PyMongo 游标，最后，我们使用`json.loads()`函数将结果 JSON 对象转换为 Python 对象。本教程演示了如何处理`ObjectId`和`datetime`对象，但自定义的`JSONEncoder`也可以扩展以处理 PyMongo 游标中可能存在的其他自定义对象类型。

[**成为会员**](https://gmyrianthous.medium.com/membership) **，在 Medium 上阅读每一篇故事。您的会员费直接支持我和其他您阅读的作者。您还将获得对 Medium 上每一篇故事的完全访问权限。**

[](https://gmyrianthous.medium.com/membership?source=post_page-----9f770740375a--------------------------------) [## 使用我的推荐链接加入 Medium — Giorgos Myrianthous

### 作为 Medium 会员，您的会员费的一部分将用于支持您阅读的作者，您也将获得对每一篇故事的完全访问权限…

[gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership?source=post_page-----9f770740375a--------------------------------)

**您可能也感兴趣的相关文章**

[](/diagrams-as-code-python-d9cbaa959ed5?source=post_page-----9f770740375a--------------------------------) ## Python 中的图表代码

### 使用 Python 创建云系统架构图

towardsdatascience.com [](/infrastructure-as-code-f153d810428b?source=post_page-----9f770740375a--------------------------------) ## 基础设施即代码

### 使用代码管理基础设施资源

towardsdatascience.com
