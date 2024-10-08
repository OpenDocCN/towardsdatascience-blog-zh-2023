# Python 类型提示：从类型别名到类型变量和新类型

> 原文：[`towardsdatascience.com/python-type-hinting-from-type-aliases-to-type-variables-and-new-types-a4a9e0400b6b`](https://towardsdatascience.com/python-type-hinting-from-type-aliases-to-type-variables-and-new-types-a4a9e0400b6b)

## PYTHON 编程

## 查看类型别名、类型变量和新类型的实际应用

[](https://medium.com/@nyggus?source=post_page-----a4a9e0400b6b--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----a4a9e0400b6b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a4a9e0400b6b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4a9e0400b6b--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----a4a9e0400b6b--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4a9e0400b6b--------------------------------) ·15 分钟阅读·2023 年 4 月 26 日

--

![](img/7194d200a270aae3a3eee222159f61df.png)

Python 提供了类型提示。选择权仍在你手中。图片来自 [William Felker](https://unsplash.com/@gndclouds?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

正如我在下面的文章中所写，如果你想在 Python 中使用类型提示，请以正确的方式进行：

[](https://betterprogramming.pub/pythons-type-hinting-friend-foe-or-just-a-headache-73c7849039c7?source=post_page-----a4a9e0400b6b--------------------------------) [## Python 的类型提示：朋友、敌人还是只是个麻烦？

### 类型提示在 Python 社区中的受欢迎程度正在增加。这会把我们带向何方？我们可以做些什么来使用它……

betterprogramming.pub](https://betterprogramming.pub/pythons-type-hinting-friend-foe-or-just-a-headache-73c7849039c7?source=post_page-----a4a9e0400b6b--------------------------------)

什么是 *正确的方式*？简单来说，就是使你的代码从静态类型检查器的角度看起来 *可读* 和 *正确* 的方式。所以，两件事：*可读* 和 *正确*。

在上面的文章中我提到的事情之一是创建 [类型别名](https://mypy.readthedocs.io/en/stable/kinds_of_types.html#type-aliases) 是提高可读性的好方法。我们将从它们开始讨论，重点讨论它们何时确实能提供帮助。然后，我们转向使用类型变量 (`typing.TypeVar`) 和新类型 (`typing.NewType`)，这些将帮助我们实现常规类型别名无法实现的目标。

我将使用 Python 3.11 和 `mypy` 版本 1.2.0。

简而言之，使用类型别名的目的有两个：

1.  以相对简单的方式让用户知道参数应该是什么类型（*应该*，因为我们仍在谈论 *类型提示*），以及

1.  让静态检查器满意。

让静态检查器满意也应该让*我们*满意：一个不满意的类型检查器通常意味着错误，或至少是一些不一致性。

对于一些用户来说，第二点是唯一值得提及的——因为静态检查是他们使用类型提示的唯一原因。它帮助他们避免错误。

当然，这很棒——但这不是全部。类型提示可以帮助我们做更多的事情。并且请注意，如果我们的唯一目标是满足静态检查器，类型别名将没有用，因为它们根本不帮助静态检查器。它们帮助的是用户。

对我来说，这两个方面同样重要。如今，当我阅读一个函数时，我会特别注意其注释。注释写得好，它们能帮助我理解函数；注释写得不好——更不用说写得错误了——它们会使函数的可读性不如没有注释时那样好。

我们从类型别名开始。我会向你展示它们的两个主要用例。接着，我们将看到类型别名在相对简单的情况下如何提供帮助，有时我们需要更多的东西。在我们的案例中，类型变量和新类型将提供帮助。

# 复杂注释的类型别名

类型别名提供了一种简单而强大的工具，使类型提示更清晰。我将在这里重用[Python 文档中的类型别名](https://docs.python.org/3/library/typing.html#type-aliases)中的一个很好的且有说服力的例子：

```py
from collections.abc import Sequence

ConnectionOptions = dict[str, str]
Address = tuple[str, int]
Server = tuple[Address, ConnectionOptions]

def broadcast_message(message: str,
                      servers: Sequence[Server]
                      ) -> None:
    ... 
```

正如文档所说，上述`servers`的类型签名*正好*等于下面使用的签名：

```py
def broadcast_message(
        message: str,
        servers: Sequence[tuple[tuple[str, int], dict[str, str]]]
    ) -> None:
    ...
```

正如你所见，等价性并不完全：虽然这两个签名在代码上确实是等效的，但它们在可读性上有所不同。关键在于这个类型签名：

```py
servers: Sequence[tuple[tuple[str, int], dict[str, str]]] 
```

尽管阅读和理解起来比较困难，但通过使用几个类型别名将其重定义为`Sequence[Server]`后，已经变得更加清晰。类型别名在签名中传达的信息很有帮助。良好的命名可以产生奇迹。

请注意，我们可以通过添加一个更多的类型别名来使这个类型签名有所不同：

```py
Servers = Sequence[Server]

servers: Servers
```

对我来说，`Sequence[Server]`比`Servers`要好得多，因为我立刻看到我处理的是一个实现了`Sequence`协议的对象。它可以是一个列表。例如，我们已经有了参数的名称`servers`，所以创建一个类型别名`Servers`似乎是多余的。

当然，理解这个签名的每一个细节，使用这些类型别名并不简单：

```py
ConnectionOptions = dict[str, str]
Address = tuple[str, int]
Server = tuple[Address, ConnectionOptions]
servers: Sequence[Server]
```

但由于类型别名`ConnectionOptions`、`Address`和`Server`及其明确的含义，这比理解以下签名要简单得多：

```py
servers: Sequence[tuple[tuple[str, int], dict[str, str]]]
```

简而言之，面对如此复杂的类型，原始的类型签名虽然让静态检查器满意——但不太可能让用户的生活变得更轻松。类型别名可以帮助实现这一点——它们有助于将关于变量、函数、类或方法的附加信息传达给用户。它们充当了一种沟通工具。

## 类型别名作为沟通工具：进一步的考虑

好吧，让我们跳到另一个例子。这次，我们将尝试利用类型别名来改善与用户的沟通，在一个比之前更简单的情况中。

正如我们所见，最重要的沟通工具是良好的命名。一个好的函数、类或方法名称应该明确表明其责任。当你看到 `calculate_distance()` 的名称时，你知道这个函数会计算距离；你会对看到一个返回两个字符串的元组的函数感到惊讶。当你看到 `City` 类时，你知道这个类会以某种方式表示一个城市——而不是一个动物、一辆车或一只海狸。

注释可以传达比函数（类、方法）及其参数名称更多的信息。换句话说，我们希望类型提示不仅能提示应该使用哪些类型，还能帮助用户理解我们的函数和类——并帮助他们提供正确的值。正如之前提到的，这可以通过命名良好的类型别名来实现。

让我们从一个简单的例子开始，这次使用变量类型提示。假设我们有如下的东西：

```py
length = 73.5
```

当然，我们知道这个变量表示某物的长度。但这就是我们所知道的。首先，是什么长度？一个更好的名字可能会有所帮助：

```py
length_of_parcel = 73.5
```

现在清楚了。想象一下你是一名送货员，你需要决定包裹是否能放进你的车里。那么，它能放进去吗？

如果有人根据上述知识做出了决定，他要么是那种“我会处理任何包裹”的人，要么是“最好不要冒险”的人。在这两种情况下，这都不是一个经过深思熟虑的决定。我们缺少单位，不是吗？

```py
length_of_parcel = 73.5 # in cm
```

更好！但这仍然只是一个注释，如果代码本身提供这些信息会更好；上面没有提供，但这里提供了：

```py
Cm = float
length_of_parcel: Cm = 73.5
```

我们再次使用了类型别名。但请记住，这只是一个类型别名，对于 Python 来说，`length_of_parcel` 仍然只是一个 `float`，别无其他。然而，对我们来说，这意味着很多——这个包裹的长度是 73.5 厘米。

让我们进入一个更复杂的情况，即从变量注释到函数注释。假设我们想实现一个计算矩形周长的函数。我们从没有注释开始： 

```py
def get_rectangle_circumference(x, y):
    return 2*x + 2*y
```

简单。符合 Python 习惯¹。正确。

我们已经熟悉了这个问题：没有注释，用户不知道函数期望什么样的数据。厘米？英寸？米？公里？实际上，函数将处理字符串：

```py
>>> get_rectangle_circumference("a", "X")
'aaXX'
```

嗯。确实，这有效——但没有意义。我们希望用户能够用我们的函数处理这样的东西吗？我们希望用户说：

> 嘿，他们的函数告诉我，当我用边长为 `"a"` 和 `"X"` 的矩形时，这个矩形的周长是 `"aaXX"`，哈哈！

不，还是不行。确实，函数的名称说明了函数的作用，但如果能让用户知道函数期望什么样的数据会更有帮助。然后我们可以回应：

> 嘿，你不能读吗？难道你看不出这个函数期望浮点数吗？或者你认为字符串是浮点数，哈哈？

我认为最好避免这种哈哈式讨论。所以，类型提示是一个大好的选择。我们继续吧。

好的，我们有一个矩形，它有四条边，`x`和`y`是它们的长度。用户提供什么单位并不重要，因为函数适用于*任何*长度单位；它可以是厘米、英寸、公里，任何长度单位。真正重要的是——实际上，区别很大——是`x`和`y`都必须使用*相同*单位。否则，函数将无法正确工作。这是可以的：

```py
>>> x = 10                  # in cm
>>> y = 200                 # in cm
>>> get_rectangle_circumference(x, y) # in cm
420
```

但这不是：

```py
>>> x = 10                  # in cm
>>> y = 2                   # in m
>>> get_rectangle_circumference(x, y) # incorrect!
24
```

问题是，即使这个调用毫无意义，我们也知道这一点，但从 Python 的角度来看，它*是正确的*——两者都一样。

+   动态地：我们会得到`24`；以及

+   静态地：`x`和`y`都是浮点数。

问题是，我们没有让用户——以及 Python——知道两个参数`x`和`y`应该是*相同单位*的，只是他们应该使用浮点数。对于 Python 而言，浮点数就是浮点数，它不区分公里和英寸，更不用说千克了。

让我们检查一下是否可以使用类型提示来做些事情。换句话说：我们能否使用类型提示让用户知道他们应该为两个参数使用相同的类型，并且返回值也是这种类型呢？

最简单的注解是使用浮点数：

```py
def get_rectangle_circumference(
    x: float,
    y: float) -> float:
    return 2*x + 2*y
```

这个函数签名比没有注解的稍好，因为至少用户知道他们应该使用`float`。但还是，英寸？厘米？米？实际上，为什么不使用千克？

那么，让我们尝试一个类型别名：

```py
Cm = float

def get_rectangle_circumference(x: Cm, y: Cm) -> Cm:
    return 2*x + 2*y
```

清楚了吧？`mypy`会鼓掌：

![](img/caaa035c5928dcf8bd14424204d2e14d.png)

`Pylance`也是如此。用户知道他们应该提供厘米，并且函数会以厘米为单位返回周长。`Cm`是一个类型别名，这基本上意味着它仍然是`float`，`Cm`和`float`之间没有区别。但关键是，*用户知道*。

然而，静态检查器在这种情况下不会太有帮助。你可以提供一个`float`的额外类型别名，它将与`Cm`以及任何`float`一样被对待：

```py
Cm = float
M = float

def get_rectangle_circumference(x: Cm, y: Cm) -> Cm:
    return 2*x + 2*y

x: Cm = 10
y: M = 10

get_rectangle_circumference(x, y)
```

类型检查器对此完全没问题，因为`Cm`和`M`只是相同类型的别名，即`float`。基本上，对于静态检查器而言，`Cm`不仅等同于`float`，也等同于`M`。因此，如果你想在这种情况下使用类型别名，你必须记住它们只是……别名——仅此而已！

我相信你已经注意到使用`Cm`类型别名的上面签名的另一个大缺点。为什么用户要用厘米提供`x`和`y`，而他们的单位是英寸或其他单位？转换？然后怎么办，转换回来？那*简直疯狂*！

嗯……也许我们可以创建一个与距离（或长度）相关的`float`别名？

```py
DistanceUnit = float

def get_rectangle_circumference(
    x: DistanceUnit,
    y: DistanceUnit
    ) -> DistanceUnit:
    return 2*x + 2*y
```

`mypy`将再次发出警告，因为我们唯一更改的是名称。但这并没有改变其他任何东西：用户仍然可以犯提供不同单位值的相同错误，这些值都将是`DistanceUnit`，如厘米和英寸。至少用户知道他们不应该提供千克。

正如你所见，类型别名无法帮助我们解决这个问题。一方面，我认为我们可以假设使用 Python 的人应该知道在计算矩形周长时，应该以相同的单位提供边的长度。这不是 Python 知识。这是简单的数学。

然而，在一些其他场景中，你可能想要让事情变得清晰，因为并非所有事情都像计算矩形周长那样清晰。我们知道类型别名没有帮助，所以让我们转向`typing`的其他两个工具：类型变量（`TypeVar`）和新类型（`NewType`）。它们会有帮助吗？

# 类型变量和新类型

如果你真的想实现如此详细的类型提示，你可以这么做。然而，请注意，这会使代码变得更复杂。为此，`typing.NewType`和`typing.TypeVar`可以提供帮助。

让我们从`NewType`开始。这是一个`typing`工具，用于创建具有最小运行时开销的新类型（参见附录 1）。以这种方式创建的类型提供的功能很有限，因此当你只需要明确的类型提示和将值转换到这种类型的可能性时，你应该优先使用它们。它的优点是它与静态检查工具兼容（正如我们稍后将看到的）。它的缺点——在我看来，这是一个相当大的缺点——是使用`typing.NewType`创建的类型不被`isinstance`视为类型（至少在 Python 3.11.2 中如此——我希望将来版本会有所改变）：

![](img/573e1532ff99a3a8921a0981a95e8067.png)

Python 3.11.2 的截图：`typing.NewType`类型不被`isinstance()`视为类型。图片由作者提供。

对我来说，这是一个严重的问题。但正如你将看到的，`typing.NewType`类型仍然非常有用，开销较小（如附录 1 所示）。

因此，我们想要创建代表我们距离相关单位的类型。问题是，我们需要创建的类型数量与我们要考虑的单位数量相同。为了简化，让我们将它们限制为几个基于[国际单位制（SI 单位）](https://en.wikipedia.org/wiki/International_System_of_Units)的最重要的长度单位。这是你在处理项目时的做法，其中类型数量有限。然而，当你在开发一个供他人使用的框架时，你应该创建更多的类型。

在我们的情况下，四种类型就足够了：

```py
from typing import NewType

Mm = NewType("Mm", float)
Cm = NewType("Cm", float)
M = NewType("M", float)
Km = NewType("Km", float) 
```

`NewType` 创建子类型——因此，`Mm`、`Cm`、`M` 和 `Km` 都是 float 的子类型。它们可以在任何 `float` 可以使用的地方使用，但静态检查器将不接受任何这些四种子类型应使用的普通 `float` 值。你需要将这样的 `float` 值转换为所需的类型；例如，你可以执行 `distance = Km(30.24)`，意味着距离为 `30` 公里和 `240` 米。

让我们看看用于注解这个简单函数的类型：

```py
def km_to_mm(x: Km) -> Mm:
    return x * 1_000_000
```

`Pylance` 听到：

![](img/89d5185db608ee6ffcebf2f716fd73ae.png)

来自 VSCode 的 Pylance 截图。图片由作者提供

这是因为 `x / 1_000_000` 给出一个浮点数，而我们指明函数返回 `Mm` 类型的值。为实现这一点，我们需要将返回值转换为预期的类型：

```py
def km_to_mm(x: Km) -> Mm:
    return Mm(x * 1_000_000)
```

如你所见，使用 `typing.NewType` 创建的类型可以作为可调用对象（在 Python 3.10 之前它们是函数；现在它们是类）用于将值转换为它们的类型。这在这种情况下非常方便。

但这将如何帮助我们处理 `get_rectangle_circumference()` 函数？我们仍然有四种不同的 `float` 子类型，我们希望函数返回其 `x` 和 `y` 参数的确切类型。

现在是引入新 `typing` 工具——类型变量，或 `typing.TypeVar` 的时候了。事实证明，类型变量可以帮助我们实现所需的功能：

```py
from typing import NewType, TypeVar

Mm = NewType("Mm", float)
Cm = NewType("Cm", float)
M = NewType("M", float)
Km = NewType("Km", float)

DistanceUnit = TypeVar("DistanceUnit", Mm, Cm, M, Km)

def get_rectangle_circumference(
    x: DistanceUnit,
    y: DistanceUnit) -> DistanceUnit:
    t = type(x)
    return t(2*x + 2*y)
```

与之前使用类型别名时不同，这次你不能混合不同的类型。让我们看看静态类型检查器 `Pylance` 如何处理此函数的三种不同调用：

*浮点数无效：*

![](img/2a403237066cb4f86b45ff1770b3eb7e.png)

(1) 浮点数无效。图片由作者提供

*你不能混合不同的类型：*

![](img/787e98ba19a1f03db104b625d668aae0.png)

(2) 两种不同的类型无效。图片由作者提供

*函数通过静态检查的唯一方法是对两个长度使用相同类型：*

![](img/d747748400c93f00038b053464f1bcc9.png)

(3) 仅相同类型的两个参数有效。图片由作者提供

当然，返回值的类型将与两个参数的类型匹配——例如，当你提供米时，你会得到米。这就是为什么我们需要 `t = type(x)` 行的原因。我们可以使函数稍微简短一些：

![](img/26ff8eb2b0ef6c685618d1160274405e.png)

更短版本的函数。图片由作者提供

对于中级和高级 Python 使用者，两种版本的可读性可能相当；然而，对于初学者来说，前者可能更容易理解。

注意，`DistanceUnit` 类型别名*不会*以相同方式工作：

![](img/44b7b3f233b954db4567d6150f638644.png)

DistanceUnit 的类型别名无法按要求工作。图片由作者提供

在这里，你可以在调用 `get_rectangle_circumference()` 时混合不同类型，这正是我们想要避免的；而类型变量帮助我们实现了这一点。

所以，我们达到了我们想要的目标。尽管任务看起来不算复杂，但类型别名并不足以实现我们的目的。然而，`typing` 的类型变量（`TypeVar`）和新类型（`NewType`）提供了帮助。

# 结论

类型提示在 Python 中不是必需的；它们是可选的。有时最好完全省略它们。然而，当你不得不使用它们时，应该明智地使用它们：让它们对你和你的代码用户有所帮助，而不是成为障碍。

我希望你现在已经准备好在自己的项目中使用 `typing` 的类型别名、类型变量和新类型，至少在类似的、相对简单的场景中使用。在这样做时，请记住*不要过度使用*这些工具。老实说，我很少决定使用类型变量和新类型。因此，在决定打开这些门之前，请三思。你的代码肯定会变得复杂得多，所以你必须有充分的理由去做这个决定。

我们已经涵盖了在 Python 类型提示系统中使用类型别名、类型变量和新类型的基本概念。这个话题还有很多内容，因为 Python 的静态检查系统仍在发展，但这种*更多*会带来更大的复杂性。今天就先说到这里，我们以后会在准备好专注于 Python 类型提示的更高级方面时再回到这个话题。

# 脚注

¹ 如果有人想对我大喊这不是*Pythonic*，因为函数没有注解，那么请让我提醒这个人，类型提示在 Python 中是*可选的*。如果某样东西是可选的，它不能作为声明代码是否 Pythonic 的决定性因素。

# 附录 1

与例如基于浮点数的自定义类相比，`typing.NewType` 的时间开销明显更小。下面的简单代码片段使用 `perftester` 来基准测试这两个方面：

+   使用 `typing.NewType` 或自定义类创建新类型哪个更快？

+   哪种类型的使用更快（具体来说，将浮点值转换为该类型）？

```py
import perftester

from typing import NewType

def typing_type_create():
    TypingFloat = NewType("TypingFloat", float)

def class_type_create():
    class ClassFloat(float): ...

TypingFloat = NewType("TypingFloat", float)
class ClassFloat(float): ...

def typing_type_use(x):
    return TypingFloat(x)

def class_type_use(x):
    return ClassFloat(x)

if __name__ == "__main__":
    perftester.config.set_defaults("time", Number=1_000_000)

    t_typing_create = perftester.time_benchmark(typing_type_create)
    t_class_create = perftester.time_benchmark(class_type_create)

    t_typing_use = perftester.time_benchmark(
        typing_type_use, x = 10.0034
    )
    t_class_use = perftester.time_benchmark(
        class_type_use, x = 10.0034
    )

    perftester.pp(dict(
        create=dict(typing=t_typing_create["min"],
                    class_=t_class_create["min"]),
        use=dict(typing=t_typing_use["min"],
                 class_=t_class_use["min"]),
    ))
```

这是我在我的机器上得到的结果：

![](img/ba00d3201791a4548a40489ee91a88a5.png)

基准测试结果：基于 typing 的方法更快。图片作者提供。

显然，`typing.NewType` 创建新类型的速度显著比自定义类快一个数量级。然而，它们在创建新类实例方面的速度差异不大。

上面的基准测试代码很简单，表明 `perftester` 提供了一个非常简单的 API。如果你想了解更多，阅读下面的文章：

[基准测试 Python 函数的简单方法：perftester](https://towardsdatascience.com/benchmarking-python-functions-the-easy-way-perftester-77f75596bc81?source=post_page-----a4a9e0400b6b--------------------------------) [## 基准测试 Python 函数的简单方法：perftester

### 你可以使用 perftester 以简单的方式基准测试 Python 函数

[前往数据科学](https://towardsdatascience.com/benchmarking-python-functions-the-easy-way-perftester-77f75596bc81?source=post_page-----a4a9e0400b6b--------------------------------)

你当然可以使用 `timeit` 模块进行这种基准测试：

[](/benchmarking-python-code-with-timeit-80827e131e48?source=post_page-----a4a9e0400b6b--------------------------------) ## 使用 timeit 进行 Python 代码基准测试

### 最受欢迎的 Python 代码时间基准测试工具，内置的 timeit 模块提供了超出大多数工具的功能…

towardsdatascience.com

感谢阅读。如果你喜欢这篇文章，你可能也会喜欢我写的其他文章；你可以在[这里](https://medium.com/@nyggus)看到。如果你想加入 Medium，请使用我下面的推荐链接：

[](https://medium.com/@nyggus/membership?source=post_page-----a4a9e0400b6b--------------------------------) [## 使用我的推荐链接加入 Medium - Marcin Kozak

### 阅读 Marcin Kozak 的每一个故事（以及 Medium 上成千上万其他作家的故事）。你的会员费直接支持…

medium.com](https://medium.com/@nyggus/membership?source=post_page-----a4a9e0400b6b--------------------------------)
