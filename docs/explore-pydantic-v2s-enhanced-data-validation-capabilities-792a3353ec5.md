# 探索 Pydantic V2 的增强数据验证功能

> 原文：[`towardsdatascience.com/explore-pydantic-v2s-enhanced-data-validation-capabilities-792a3353ec5`](https://towardsdatascience.com/explore-pydantic-v2s-enhanced-data-validation-capabilities-792a3353ec5)

## 了解 Pydantic V2 的新功能和语法

[](https://lynn-kwong.medium.com/?source=post_page-----792a3353ec5--------------------------------)![Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----792a3353ec5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----792a3353ec5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----792a3353ec5--------------------------------) [Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----792a3353ec5--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----792a3353ec5--------------------------------) ·7 min read·2023 年 10 月 25 日

--

![](img/84d995a0acb8b86cb58dee8da4027427.png)

图片由 jackmac34 在 Pixabay 提供

数据验证是数据工程和软件开发领域中稳健应用的基石。确保数据的清洁性和准确性不仅对应用的可靠性至关重要，也对用户体验有很大影响。

Pydantic 是 Python 中使用最广泛的数据验证库。Pydantic 最新版本（V2）的核心已经用 Rust 重新编写，相比于之前的版本性能大大提升。此外，在功能方面也有一些重大改进，例如支持严格模式、无模型验证、模型命名空间清理等。

本文将深入探讨 Pydantic 强大数据验证功能的最新特性和增强性能，为开发者提供一个全面的数据处理工具集。

## 准备工作

要跟随本文中的示例，您应该安装现代版本的 Python（≥ 3.10）和最新版本的 Pydantic V2。建议使用 [conda](https://superdataminer.com/2022/01/04/how-to-create-virtual-environments-with-venv-and-conda-in-python/) 虚拟环境来管理不同版本的 Python 和库：

```py
conda create -n pydantic2 python=3.11
conda activate pydantic2

pip install -U pydantic
```

## 基本用法

通常使用 Pydantic 时，我们需要先通过模型定义数据的模式，这些模型只是继承自 `BaseModel` 的类。在这些模型中，每个字段的数据类型由类型提示定义。

```py
from pydantic import BaseModel

class ComputerModel(BaseModel):
    brand: str
    cpu: str
    storage: int
    ssd: bool = True
```

要使用此模型进行验证，我们可以通过传递每个字段的值来创建一个实例：

```py
input_dict = {"brand": "HP", "cpu": "Intel i7 1265U", "storage": "256"}

computer = ComputerModel(**input_dict)

print(computer)
# brand='HP' cpu='Intel i7 1265U' storage=256 ssd=True
```

`storage` 字符串数据会被强制转换为模型中定义的整数。

为了演示的简便性，我们在本文中仅使用两个字段，即`brand`和`storage`，这可以轻松扩展到其他字段。

```py
# Basic model used in this post
from pydantic import BaseModel

class ComputerModel(BaseModel):
    brand: str
    storage: int
```

## 直接验证数据

在上面的示例中，为数据验证创建了一个 Pydantic 模型的实例。在 Pydantic V2 中，我们还可以直接使用`model_validate()`和`model_validate_json()`来验证字典或 JSON 数据：

```py
ComputerModel.model_validate({"brand": "HP", "storage": "256"})
# ComputerModel(brand='HP', storage=256)

import json
input_json = json.dumps({"brand": "HP", "storage": "256"})

ComputerModel.model_validate_json(input_json)
# ComputerModel(brand='HP', storage=256)
```

在 Pydantic V2 中，所有模型的方法都以`model_`开头，因此字段名称不允许以`model_`开头。然而，如果需要，可以使用字段别名。

## 在严格模式下验证数据

默认情况下，严格模式是关闭的，这意味着数据类型会被强制转换（如果可能的话）。例如，在上述示例中，`storage`字段的类型从`str`被强制转换为`int`。我们可以禁用严格模式，这样所有字段的数据类型必须完全匹配：

```py
ComputerModel.model_validate({"brand": "HP", "storage": "256"}, strict=True)

# ValidationError: 1 validation error for ComputerModel
# storage
#  Input should be a valid integer [type=int_type, input_value='256', input_type=str]
```

我们还可以在模型的字段级别设置严格模式，这样我们在验证步骤中就不需要指定它：

```py
from pydantic import Field

class ComputerModelStrict(BaseModel):
    brand: str
    storage: int = Field(strict=True)

ComputerModelStrict.model_validate({"brand": "HP", "storage": "256"})

# ValidationError: 1 validation error for ComputerModel
# storage
#  Input should be a valid integer [type=int_type, input_value='256', input_type=str]
```

## 使用`model_config`配置模型

在 Pydantic V2 中，为了指定模型的配置，我们可以将类属性`model_config`设置为一个字典，该字典包含将用于配置的键/值对。通常，我们通过一个称为`ConfigDict`的特殊字典来做到这一点，它是一个用于配置 Pydantic 行为的`TypedDict`。

例如，我们可以在模型级别设置`strict`模式，而不是在字段级别，如上所示：

```py
from pydantic import BaseModel, ConfigDict

class ComputerModelStrict(BaseModel):
    model_config = ConfigDict(strict=True, str_min_length=2)

    brand: str
    storage: int

ComputerModelStrict.model_validate({"brand": "HP", "storage": "256"})

# ValidationError: 1 validation error for ComputerModel
# storage
#  Input should be a valid integer [type=int_type, input_value='256', input_type=str]

ComputerModelStrict.model_validate({'brand': 'X', 'storage': 256})
# ValidationError: 1 validation error for ComputerModelStrict
# brand
#  String should have at least 2 characters [type=string_too_short, input_value='X', input_type=str]
```

我们还指定了字符串字段的最小长度为 2，因此像`X`这样的品牌将被拒绝。

## 使用`typing.Annotated`来处理字段

不必将`Field`值分配给字段以指定字段的行为，也可以使用类型提示`typing.Annotated`来完成：

```py
from typing import Annotated

class ComputerModelStrict(BaseModel):
    brand: str
    storage: Annotated[int, Field(strict=True, gt=0)]

ComputerModelStrict.model_validate({'brand': 'HP', 'storage': '256'})

# ValidationError: 1 validation error for ComputerModel
# storage
#  Input should be a valid integer [type=int_type, input_value='256', input_type=str]

ComputerModelStrict.model_validate({'brand': 'HP', 'storage': 0})
# ValidationError: 1 validation error for ComputerModelStrict
# storage
#   Input should be greater than 0 [type=greater_than, input_value=0, input_type=int]
```

使用`Annotated`时，传递的第一个类型参数（这里是`int`）是实际类型，其余的是其他工具（这里是 Pydantic）的元数据。元数据可以包含任何内容，如何使用由其他工具决定。

## 具有动态默认值的字段

我们可以为字段设置动态默认值，这样它可以自动生成，并且每个模型实例可能不同。例如，我们可以将当前时间戳设置为模型的创建时间，并为其设置唯一的 ID。这可以通过使用`default_factory`来完成，它接受一个工厂函数作为输入。

```py
from datetime import datetime
from typing import Annotated
from uuid import UUID, uuid4

from pydantic import BaseModel, Field

class ComputerModel(BaseModel):
    uid: UUID = Field(default_factory=uuid4)
    brand: str
    storage: int
    created: datetime = Field(default_factory=datetime.utcnow)

ComputerModel.model_validate({'brand': 'HP', 'storage': 256})
# ComputerModel(uid=UUID('81474288-f691-4e37-b5e3-d28f0656d972'), brand='HP', storage=256, created=datetime.datetime(2023, 9, 29, 0, 5, 2, 958755))
```

这表明`uid`和`created`字段是自动创建的，并且每个模型将会不同。

## 字段和模型验证器

类似于在字段上应用严格模式，我们可以使用`Annotated`语法为字段应用自定义验证器。让我们添加一个自定义验证器，检查`storage`是否是有效值：

```py
from typing import Annotated
from pydantic.functional_validators import AfterValidator

def check_storage(storage: int):
    allowed = (128, 256, 512, 1000, 1024, 2000, 2048)
    if storage not in allowed:
        raise ValueError(f"Invalid storage, storage must be one of {allowed}")
    return storage

class ComputerModel(BaseModel):
    brand: str
    storage: Annotated[int, AfterValidator(check_storage)]

ComputerModel.model_validate({'brand': 'HP', 'storage': 256})
# ComputerModel(brand='HP', storage=256)

ComputerModel.model_validate({'brand': 'HP', 'storage': 250})
# ValidationError: 1 validation error for ComputerModel
# storage
#  Value error, Invalid storage, storage must be one of (128, 256, 512, 1000, 1024, 2000, 2048) [type=value_error, input_value=250, input_type=int] 
```

`AfterValidator`表示验证将在 Pydantic 的内部验证逻辑之后应用。它相当于使用`@field_validator()`装饰器的`after`模式，如下所示。

请注意，验证代码不应抛出`ValidationError`本身，而应抛出`ValueError`或`AssertionError`（或其子类），这些异常将被捕获并用于填充`ValidationError`。

我们还可以使用`@field_validator()`装饰器为字段应用自定义验证器：

```py
from typing import Annotated

from pydantic import BaseModel, Field, field_validator
from pydantic.functional_validators import AfterValidator

class ComputerModel(BaseModel):
    brand: str
    storage: int

    @field_validator('storage', mode='after')
    @classmethod
    def check_storage(cls, storage: int):
        allowed = (128, 256, 512, 1000, 1024, 2000, 2048)
        if storage not in allowed:
            raise ValueError(f"Invalid storage, storage must be one of {allowed}")
        return storage
```

使用`@field_validator()`的效果应与上述`Annotated`语法完全相同。每种语法都有其优缺点：

+   使用`Annotated`可以更轻松地重用自定义验证函数。

+   使用`field_validator`我们可以更轻松地将相同的验证函数应用于多个字段。

因此，你需要根据具体的实际使用案例来决定使用哪种语法。

我们还可以使用`@model_validator()`将自定义验证器应用于整个模型。在这种情况下，我们可以访问所有字段的数据。例如，假设如果品牌是“Apple”，则存储必须至少为 256GB。

```py
from __future__ import annotations
from typing import Annotated

from pydantic import BaseModel, Field, model_validator
from pydantic.functional_validators import AfterValidator

class ComputerModel(BaseModel):
    brand: str
    storage: int

    @model_validator(mode='after')
    def check_brand_storage(self) -> ComputerModel:
        if self.brand.upper() == 'APPLE' and self.storage < 256:
            raise ValueError("For Apple, the storage must be at least 256GB.")
        return self

ComputerModel.model_validate({'brand': 'Apple', 'storage': 256})
# ComputerModel(brand='HP', storage=256)

ComputerModel.model_validate({'brand': 'Apple', 'storage': 128})
# ValidationError: 1 validation error for ComputerModel
#   Value error, For Apple, the storage must be at least 256GB. [type=value_error, input_value={'brand': 'Apple', 'storage': 128}, input_type=dict]
```

请注意，`@model_validator()`是一个实例方法装饰器，而不是像`@field_validator()`那样的[类方法装饰器](https://superdataminer.com/2021/11/16/how-to-decorate-classes-in-python/)。

## 转储或序列化

我们可以使用`[model_dump](https://docs.pydantic.dev/latest/concepts/serialization/#modelmodel_dump)()`将 Pydantic 模型的实例转换为包含实例值的字典。我们将使用文章开头介绍的更详细的模型来演示序列化：

```py
from pydantic import BaseModel

class ComputerModel(BaseModel):
    brand: str
    cpu: str
    storage: int
    ssd: bool = True

input_dict = {"brand": "HP", "cpu": "Intel i7 1265U", "storage": "256"}
model = ComputerModel(**input_dict)

output_dict_default = model.model_dump()
# {'brand': 'HP', 'cpu': 'Intel i7 1265U', 'storage': 256, 'ssd': True}

output_dict_no_unset = model.model_dump(exclude_unset=True)
# {'brand': 'HP', 'cpu': 'Intel i7 1265U', 'storage': 256}

output_dict_included = model.model_dump(include={'brand', 'storage'})
# {'brand': 'HP', 'storage': 256}

output_dict_excluded = model.model_dump(exclude={'cpu'})
# {'brand': 'HP', 'storage': 256, 'ssd': True}
```

Pydantic 可以将许多常用类型序列化为 JSON，这些类型否则会与简单的`json.dumps()`不兼容（例如`datetime`、`date`或`UUID`）。如果需要，我们还可以使用`[@field_serializer](http://twitter.com/field_serializer)()`装饰器自定义字段的序列化方式。例如，我们可以在转储时将品牌转换为大写。

```py
from pydantic import BaseModel, field_serializer

class ComputerModel(BaseModel):
    brand: str
    cpu: str
    storage: int
    ssd: bool = True

    @field_serializer('brand')
    def serialize_dt(self, brand: str, _info):
        return brand.upper()

input_dict = {"brand": "Apple", "cpu": "M1", "storage": "512"}
model = ComputerModel(**input_dict)
# {'brand': 'APPLE', 'cpu': 'M1', 'storage': 512, 'ssd': True}
```

请注意，`_info`表示 Pydantic 自动提供的元数据。

在这篇文章中，我们介绍了如何使用最新版本的 Pydantic（V2）进行数据验证。在这个版本中引入了许多语法变化和新特性，这些在官方文档中可能会显得非常冗长和复杂。幸运的是，我们在日常工作中只使用了一小部分功能，并且大多数功能在这篇文章中通过简单的示例进行了介绍，这些示例可以为开发者提供全面的数据处理工具集。

## 相关帖子

+   [如何使用 Pydantic 模型的自定义验证器验证数据](https://superdataminer.com/2022/08/05/how-to-validate-your-data-with-custom-validators-of-pydantic-models-in-python/)

+   [理解和掌握 Python 中的装饰器](https://superdataminer.com/2021/05/14/understand-and-master-the-decorator-in-python/)
