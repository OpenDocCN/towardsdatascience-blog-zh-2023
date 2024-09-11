# 使用部分和 Pydantic 编写 DRY 数据模型

> 原文：[https://towardsdatascience.com/write-dry-data-models-with-partials-and-pydantic-b0b13d0eeb3a?source=collection_archive---------4-----------------------#2023-02-14](https://towardsdatascience.com/write-dry-data-models-with-partials-and-pydantic-b0b13d0eeb3a?source=collection_archive---------4-----------------------#2023-02-14)

## 构建干净的嵌套数据模型，用于数据工程管道

[](https://medium.com/@Carobert?source=post_page-----b0b13d0eeb3a--------------------------------)[![查尔斯·门德尔森](../Images/0a8dea9bab2a49da65687095d31065e9.png)](https://medium.com/@Carobert?source=post_page-----b0b13d0eeb3a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b0b13d0eeb3a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b0b13d0eeb3a--------------------------------) [查尔斯·门德尔森](https://medium.com/@Carobert?source=post_page-----b0b13d0eeb3a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa6f4d278f87e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwrite-dry-data-models-with-partials-and-pydantic-b0b13d0eeb3a&user=Charles+Mendelson&userId=a6f4d278f87e&source=post_page-a6f4d278f87e----b0b13d0eeb3a---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b0b13d0eeb3a--------------------------------) ·7 min 阅读·2023年2月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb0b13d0eeb3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwrite-dry-data-models-with-partials-and-pydantic-b0b13d0eeb3a&user=Charles+Mendelson&userId=a6f4d278f87e&source=-----b0b13d0eeb3a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb0b13d0eeb3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwrite-dry-data-models-with-partials-and-pydantic-b0b13d0eeb3a&source=-----b0b13d0eeb3a---------------------bookmark_footer-----------)![](../Images/a5f33117bda3e5651248696317ad5a7a.png)

图片由 [Didssph](https://unsplash.com/ko/@didsss?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/PB80D_B4g7c?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

Pydantic 是一个功能极其强大的数据建模和验证库，应该成为你数据管道的标准部分。

使它们如此强大的部分原因在于，它们可以轻松地容纳嵌套的数据文件，例如 JSON。

简单回顾一下，Pydantic 是一个 Python 库，让你以 Pythonic 的方式定义数据模型，并使用该模型验证数据输入，主要通过类型提示。

Pydantic 的类型提示比标准库的更强大，更具自定义性。

另一方面，Partial 允许你预加载带有特定参数和关键字参数的函数调用，这在你多次调用相同参数的同一函数时特别有用。

[在之前的文章中](https://medium.com/towards-data-science/easily-validate-user-generated-data-using-pydantic-5ace695cc3c8)，我讨论了使用枚举来定义用于验证的有效字符串输入。

我们可以进一步通过将 Pydantic 数据模型与其他 Pydantic 数据模型结合来推进。

# 数据

我使用各种随机名称生成器创建了我们的示例数据。该数据集表示一个 Dungeons and Dragons 类型游戏中的角色。

让我们开始检查我们的数据：

![](../Images/c3bb3d39f2feb2387af43a34175a2db6.png)

作者提供的图像，由 Carbon 生成

如我们所见，与其扁平数据结构，我们现在有了数据中的数据。

检查我们之前的数据模型，我们可以看到现在必须进行一些更改以适应嵌套的数据结构：

```py
import pydantic
class RpgCharacterModel(pydantic.BaseModel):
    CREATION_DATE: datetime
    NAME: str = pydantic.Field(...)
    GENDER: GenderEnum
    RACE: RaceEnum = pydantic.Field(...)
    CLASS: ClassEnum = pydantic.Field(...)
    HOME: str
    GUILD: str
    PAY: int = pydantic.Field(..., ge=1, le=500)
```

# 视觉化问题

我发现一个有用的工具是 [JSONCrack](https://jsoncrack.com/)，它提供了 JSON 数据的精彩可视化：

![](../Images/ec7fd0634f18cdb24d4708c2154d393c.png)

图像从 JSON Crack 下载，使用 [GNU 通用公共许可证 v3.0](https://github.com/AykutSarac/jsoncrack.com/blob/main/LICENSE)

我们可以看到它下面的 4 个其他模型支持我们的顶级模型。

# DRY 定义我们的模型：

使用我们可以从 JSON Crack 轻松查看的字段，我们可以像这样对模型进行第一次处理。

```py
import pydantic
class RpgRaceModelBasic(pydantic.BaseModel):
    RACE_ID: int = pydantic.Field(..., ge=10, le=99)
    RACE: RaceEnum = pydantic.Field(...)
    HP_MODIFIER_PER_LEVEL: int = pydantic.Field(..., ge=-6, le=6)
    STR_MODIFIER: int = pydantic.Field(..., ge=-6, le=6, description="Character's strength")
    CON_MODIFIER: int = pydantic.Field(..., ge=-6, le=6, description="Character's strength")
    DEX_MODIFIER: int = pydantic.Field(..., ge=-6, le=6, description="Character's strength")
    INT_MODIFIER: int = pydantic.Field(..., ge=-6, le=6, description="Character's strength")
    WIS_MODIFIER: int = pydantic.Field(..., ge=-6, le=6, description="Character's strength")
    CHR_MODIFIER: int = pydantic.Field(..., ge=-6, le=6, description="Character's strength")
```

`pydantic.Field()` 允许我们为模型指定超出类型提示的额外参数。

+   `...` 表示这是一个必填字段。

+   `ge` 表示字段必须大于或等于此值。

+   `le` 表示字段必须小于或等于此值。

但是，字段定义我们的属性修饰符中也有很多重复的代码。我们的 7 个修饰符必须在 -6 和 6 之间。 因此，在未来的更改中，我们必须更改 7 行代码。

我们可以使用来自 `functools` 库的 `partial` 函数来简化我们的定义。partial 函数允许我们为函数固定参数，非常适合这种多次调用相同参数的情况：

```py
import functools
import pydantic

id_partial = functools.partial(pydantic.Field, ..., ge=10, le=99)
attribute_partial = functools.partial(pydantic.Field, ..., ge=-6, le=6)

class RpgRaceModelDry(pydantic.BaseModel):
    RACE_ID: int = id_partial(..., ge=10, le=99)
    RACE: RaceEnum = pydantic.Field(...)
    HP_MODIFIER_PER_LEVEL: int = attribute_partial()
    STR_MODIFIER: int = attribute_partial(description="Character's strength")
    CON_MODIFIER: int = attribute_partial(description="Character's constitution")
    DEX_MODIFIER: int = attribute_partial(description="Character's dexterity")
    INT_MODIFIER: int = attribute_partial(description="Character's intelligence")
    WIS_MODIFIER: int = attribute_partial(description="Character's wisdom")
    CHR_MODIFIER: int = attribute_partial(description="Character's charisma")
```

现在，我们有一个可以在需要更改属性修饰符范围时修改代码的单一位置。这是一种更干净且更 DRY 的编程方式。

另外，请注意我们仍然可以将其他参数传递给 partial 函数，在这种情况下是 `description`

部分函数还很灵活，可以覆盖你已经传递给它们的关键字参数。

# 完善模型：

现在我们还需要定义其他三个模型以及几个与之配套的枚举：

```py
import enum
import pydantic

class AttributeEnum(enum.Enum):
    STR = 'STR'
    DEX = 'DEX'
    CON = 'CON'
    INT = 'INT'
    WIS = 'WIS'
    CHR = 'CHR'

class AlignmentEnum(enum.Enum):
    LAWFUL_GOOD = 'Lawful Good'
    LAWFUL_NEUTRAL = 'Lawful Neutral'
    LAWFUL_EVIL = 'Lawful Evil'
    NEUTRAL_GOOD = 'Neutral Good'
    TRUE_NEUTRAL = 'True Neutral'
    NEUTRAL_EVIL = 'Neutral Evil'
    CHAOTIC_GOOD = 'Chaotic Good'
    CHAOTIC_NEUTRAL = 'Chaotic Neutral'
    CHAOTIC_EVIL = 'Chaotic Evil'

class RpgClassModel(pydantic.BaseModel):
    CLASS_ID: int = id_partial()
    CLASS: ClassEnum = pydantic.Field(...)
    PRIMARY_CLASS_ATTRIBUTE: AttributeEnum = pydantic.Field(...)
    SPELLCASTER: bool = pydantic.Field(...)

class RpgPolityModel(pydantic.BaseModel):
    KINGDOM_ID: int = id_partial(ge=100, le=999)
    POLITY: str = pydantic.Field(...)
    TYPE: str = pydantic.Field(...)

class RpgGuildModel(pydantic.BaseModel):
    GUILD_ID: int = id_partial(ge=100, le=999)
    GUILD: str = pydantic.Field(...)
    ALIGNMENT: AlignmentEnum
    WEEKLY_DUES: int = pydantic.Field(..., ge=10, le=100)
```

特别注意 `KINGDOM_ID` 和 `GUILD_ID`。我们在部分函数中覆盖了 `ge` 和 `le` 参数，这没有问题。它仍然保留了 `...`，这表明它是一个必填字段。

通过对我们的 ID 列调用部分函数，我们不必担心忘记将它们设置为必填字段。

# 定义我们的顶层模型

现在所有支持模型都已构建完成，我们可以定义顶层模型，其样子如下：

```py
import pydantic
class RpgCharacterModel(pydantic.BaseModel):
    CREATION_DATE: datetime
    NAME: str = pydantic.Field(...)
    GENDER: GenderEnum
    RACE_NESTED: RpgRaceModelDry = pydantic.Field(...)
    CLASS_NESTED: RpgClassModel = pydantic.Field(...)
    HOME_NESTED: RpgPolityModel
    GUILD_NESTED: RpgGuildModel
    PAY: int = pydantic.Field(..., ge=1, le=500)
```

关注 `HOME_NESTED` 和 `GUILD_NESTED`：

注意这些模型在顶层模型中不是必需的，但这些模型内部包含的字段是必需的。这意味着，如果你向字段传递数据，它必须符合模型要求，但如果你没有向字段传递数据，它仍然被视为有效。

这实际上意味着你可以传递一个有效的模型或什么都不传。

# 结论

将 `functools.partial()` 与 Pydantic 数据模型结合使用，可以使你的代码更加简洁、易于理解，并确保以可扩展的方式正确处理无效数据。

在我们的示例中，我们只构建了一个嵌套层级，但你可以重复嵌套以管理遇到的任何复杂 JSON 对象。

同样，一个构建良好的数据模型可以让下游消费者相信你发送给他们的数据正是他们所期望的。

你认为可以如何将这些技术应用到你的数据管道中？

# 关于

Charles Mendelson 是 PitchBook 数据公司的数据工程师。 [如果你想联系他，最好的方式是在 LinkedIn 上。](https://www.linkedin.com/in/charles-mendelson-carobert/)

# 所有代码：

```py
# Standard Library imports
from datetime import datetime
import enum
import functools

# 3rd Party package imports
import pydantic

# Enums for limiting string data in our model

class GenderEnum(enum.Enum):
    M = 'M'
    F = 'F'
    NB = 'NB'

class ClassEnum(enum.Enum):
    Druid = 'Druid'
    Fighter = 'Fighter'
    Warlock = 'Warlock'
    Ranger = 'Ranger'
    Bard = 'Bard'
    Sorcerer = 'Sorcerer'
    Paladin = 'Paladin'
    Rogue = 'Rogue'
    Wizard = 'Wizard'
    Monk = 'Monk'
    Barbarian = 'Barbarian'
    Cleric = 'Cleric'

class RaceEnum(enum.Enum):
    Human = 'Human'
    Dwarf = 'Dwarf'
    Halfling = 'Halfling'
    Elf = 'Elf'
    Dragonborn = 'Dragonborn'
    Tiefling = 'Tiefling'
    Half_Orc = 'Half-Orc'
    Gnome = 'Gnome'
    Half_Elf = 'Half-Elf'

class AttributeEnum(enum.Enum):
    STR = 'STR'
    DEX = 'DEX'
    CON = 'CON'
    INT = 'INT'
    WIS = 'WIS'
    CHR = 'CHR'

class AlignmentEnum(enum.Enum):
    LAWFUL_GOOD = 'Lawful Good'
    LAWFUL_NEUTRAL = 'Lawful Neutral'
    LAWFUL_EVIL = 'Lawful Evil'
    NEUTRAL_GOOD = 'Neutral Good'
    TRUE_NEUTRAL = 'True Neutral'
    NEUTRAL_EVIL = 'Neutral Evil'
    CHAOTIC_GOOD = 'Chaotic Good'
    CHAOTIC_NEUTRAL = 'Chaotic Neutral'
    CHAOTIC_EVIL = 'Chaotic Evil'

# partial function for siloing our logic in one place:
id_partial = functools.partial(pydantic.Field, ..., ge=10, le=99))
attribute_partial = functools.partial(pydantic.Field, ..., ge=-6, le=6)

# models that make up our main model

class RpgRaceModelDry(pydantic.BaseModel):
    RACE_ID: int = id_partial()
    RACE: RaceEnum = pydantic.Field(...)
    HP_MODIFIER_PER_LEVEL: int = attribute_partial()
    STR_MODIFIER: int = attribute_partial()
    CON_MODIFIER: int = attribute_partial()
    DEX_MODIFIER: int = attribute_partial()
    INT_MODIFIER: int = attribute_partial()
    WIS_MODIFIER: int = attribute_partial()
    CHR_MODIFIER: int = attribute_partial()

class RpgClassModel(pydantic.BaseModel):
    CLASS_ID: int = id_partial()
    CLASS: ClassEnum = pydantic.Field(...)
    PRIMARY_CLASS_ATTRIBUTE: AttributeEnum = pydantic.Field(...)
    SPELLCASTER: bool = pydantic.Field(...)

class RpgPolityModel(pydantic.BaseModel):
    KINGDOM_ID: int = id_partial(ge=100, le=999)
    POLITY: str = pydantic.Field(...)
    TYPE: str = pydantic.Field(...)

class RpgGuildModel(pydantic.BaseModel):
    GUILD_ID: int = id_partial(ge=100, le=999)
    GUILD: str = pydantic.Field(...)
    ALIGNMENT: AlignmentEnum
    WEEKLY_DUES: int = pydantic.Field(..., ge=10, le=100)

# Our top level model:
class RpgCharacterModel(pydantic.BaseModel):
    CREATION_DATE: datetime
    NAME: str = pydantic.Field(...)
    GENDER: GenderEnum
    RACE_NESTED: RpgRaceModelDry = pydantic.Field(...)
    CLASS_NESTED: RpgClassModel = pydantic.Field(...)
    HOME_NESTED: RpgPolityModel
    GUILD_NESTED: RpgGuildModel
    PAY: int = pydantic.Field(..., ge=1, le=500)

# We didn't talk about this one, but from a previous article,
# this validates each row of data

def validate_data(list_o_dicts, model: pydantic.BaseModel, index_offset: int = 0):
    list_of_dicts_copy = list_o_dicts.copy()
    #capturing our good data and our bad data
    good_data = []
    bad_data = []
    for index, row in enumerate(list_of_dicts_copy):
        try:
            model(**row)  #unpacks our dictionary into our keyword arguments
            good_data.append(row)  #appends valid data to a new list of dictionaries
        except pydantic.ValidationError as e:
            # Adds all validation error messages associated with the error
            # and adds them to the dictionary
            row['Errors'] = [error_message for error_message in e.errors()]
            row['Error_row_num'] = index + index_offset
            #appends bad data to a different list of dictionaries
            bad_data.append(row)

    return (good_data, bad_data)
```

*最初发布于* [*https://charlesmendelson.com*](https://charlesmendelson.com/tds/pydantic-and-partials/) *2023年2月23日。*
