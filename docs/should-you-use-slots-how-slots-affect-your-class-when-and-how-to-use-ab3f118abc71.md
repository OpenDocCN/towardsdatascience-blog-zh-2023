# 你应该使用 slots 吗？Slots 如何影响你的类，何时以及如何使用它们

> 原文：[`towardsdatascience.com/should-you-use-slots-how-slots-affect-your-class-when-and-how-to-use-ab3f118abc71`](https://towardsdatascience.com/should-you-use-slots-how-slots-affect-your-class-when-and-how-to-use-ab3f118abc71)

## 一行代码能带来 20%的性能提升？

[](https://mikehuls.medium.com/?source=post_page-----ab3f118abc71--------------------------------)![Mike Huls](https://mikehuls.medium.com/?source=post_page-----ab3f118abc71--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ab3f118abc71--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab3f118abc71--------------------------------) [Mike Huls](https://mikehuls.medium.com/?source=post_page-----ab3f118abc71--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab3f118abc71--------------------------------) ·6 分钟阅读·2023 年 8 月 12 日

--

![](img/a7016fdf3f746ca53c9033b17ddc4026.png)

(图片来源：[Sébastien Goldberg](https://unsplash.com/@sebastiengoldberg) 在 [Unsplash](https://unsplash.com/photos/J8N79W2EdB8))

Slots 是一种机制，它允许你声明类属性并限制其他属性的创建。你可以确定你的类有哪些属性，从而防止开发者动态添加新属性。这通常会导致**20%的速度提升**。

Slots 在有大量类实例且属性集已知的程序中特别有用。比如视频游戏或物理模拟；在这些情况下，你跟踪大量的实体。

你可以通过**添加一行代码**将 slots 添加到你的类中，但这总是一个好主意吗？在本文中，我们将探讨**为什么**和**如何**使用**slots**使你的类更快以及何时使用它们。总体目标是更好地理解 Python 的类内部工作原理。开始编码吧！

# Slots 让 Python 类更快

通过使用`slots`，你可以提高类的内存使用效率和性能。一个具有 slots 的类占用更少的内存，执行速度更快。

## 如何让我的类使用 slots？

告诉 Python 让一个类使用 slots 非常简单。你只需添加一个特殊的属性`__slots__`，它指定所有其他属性的名称：

```py
class Person:
  first_name:str
  last_name:str
  age:int

  __slots__ = ['first_name', 'last_name', 'age']    # <-- this adds slots

  def __init__(self, first_name:str, last_name:str, age:int):
    self.first_name = first_name
    self.last_name = last_name
    self.age = age
```

在上述类中，我们看到`Person`有三个属性：`first_name`、`last_name`和`age`。我们可以告诉 Python 我们希望`Person`类使用 slots，通过添加`__slots__`属性来实现。这个属性必须指定所有其他属性的名称。

[](/args-vs-kwargs-which-is-the-fastest-way-to-call-a-function-in-python-afb2e817120?source=post_page-----ab3f118abc71--------------------------------) ## 参数与关键字参数：哪种方式在 Python 中调用函数最快？

### `timeit`模块的清晰演示

towardsdatascience.com

## slotted 类的速度提升了多少？

我们上面使用的 `Person` 类使用 slots 后几乎 **小了 60%**（从 488 字节减少到 206 字节）。

关于速度，我已经对实例化、访问和赋值进行了基准测试。我发现 **速度提高了多达 20%**！你需要对这些结果持保留态度；虽然这些百分比看起来相当令人印象深刻，但这 20% 仅代表 **10 万次实例化类的 0.44 秒**。这相当于每个实例 **可忽略的 44 纳秒**（大约比一秒小 3030 万倍）。

查看用于基准测试的 [**内存**](https://gist.github.com/mike-huls/75ad4a7840e36a33606192aeabe4a1c7) 和 [**速度**](https://gist.github.com/mike-huls/45593ba9c642d6a85b6a7ef79530a6dc)代码；

[](/why-is-python-so-slow-and-how-to-speed-it-up-485b5a84154e?source=post_page-----ab3f118abc71--------------------------------) ## 为什么 Python 很慢以及如何加速

### 看看底层，了解 Python 的瓶颈所在

towardsdatascience.com

## 为什么 slotted 类更小且更快？

这与 Python 类的 **动态字典** 有关。这个字典让你可以为 Python 类分配属性：

```py
class Person:
  pass

mike = Person()

mike.age = 33  # <-- create a new attribute
```

在上面的例子中，我们定义了一个没有任何属性的类，创建了一个实例，然后动态地创建 `age` 属性并赋值。

在底层，Python 将所有属性信息存储在一个字典中。通过调用类上的 `__dict__` 魔法方法可以访问这个字典：

```py
# 1\. Define class
class Person:
  name:str

  def __init__(self, name:str):
      self.name = name

# 2\. Create instance
mike = Person(name='mike')
# 3\. Create a new variable
mike.age = 33
# 4\. Create new attribute throught the __dict__
mike.__dict__['website'] = 'mikehuls.com'
# 5\. Print out the dynamic dictionary
print(mike.__dict__)  
# -> {'name': 'mike', 'age': 33, 'website': 'mikehuls.com'}
```

动态字典使得 Python 类非常灵活，但它有一个缺点：使用属性时 Python 会在这个字典中进行搜索，这相对较慢。

[](/thread-your-python-program-with-two-lines-of-code-3b474407dbb8?source=post_page-----ab3f118abc71--------------------------------) ## 用两行代码线程化你的 Python 程序

### 通过同时做多件事来加速你的程序

towardsdatascience.com

## `slots`如何影响动态字典？

当你告诉 Python 为你的类使用 `slots` 时，**不会创建动态字典**。相反，Python 创建了一个 **固定大小的数组**，其中包含对变量的引用。这就是你必须将属性名称传递给 `__slots__` 属性的原因。

访问这个数组不仅速度更快，而且占用的内存空间也更少。较小的内存占用对内存分配和垃圾回收也有积极的影响。

## 插槽有什么副作用？

**插槽改变了你的类**；它变得有点**不灵活**，因为你的类变得更**静态**。这意味着你不能在运行时添加属性；你必须事先指定你的属性：

```py
# 1\. Define class
class Person:
  name:str

  def __init__(self, name:str):
      self.name = name

# 2\. Create instance
mike = Person(name='mike')

# 3\. Add a new attribute?
mike.website = 'mikehuls.com'     # this will not work!
# ERROR: AttributeError: 'Person' object has no attribute 'website'

# 4\. Print out dynamic dict
print(mike.__dict__)              # this will not work
# ERROR: AttributeError: 'Person' object has no attribute '__dict__'
```

有一种（虽然有点乱的）变通方法：通过将 `"__dict__"` 的值添加到你的 `__slots__` 数组中：

```py
# 1\. Define class
class Person:
  name: str

  __slots__ = ["name", "__dict__"] # <- We've added __dict__

  def __init__(self, name: str):
    self.name = name

# 2\. Create instance
mike = Person(name='mike')

# 3\. Add a new attribute
mike.website = 'mikehuls.com'     # no error this time!
```

最后一个需要注意的事项是，有些包可能期望使用“普通”的 Python 类，而不是使用插槽类。

towardsdatascience.com ## 6 步骤让 Pandas DataFrame 操作快 100 倍

### Cython 用于数据科学：将 Pandas 与 Cython 结合，以实现令人难以置信的速度提升

[towardsdatascience.com

## 这在数据类中也适用吗？

是的！从 Python 3.10 开始，你还可以添加**插槽**数据类。使用数据类更简单，只需向 @dataclass 装饰器添加一个参数即可。只需像下面这样定义你的数据类：

```py
@dataclasses.dataclass(slots=True)
class Person:
    name: str
```

## 使用插槽有什么好处？

显然，**速度**和**内存**效率，但也许还有**安全性**：如果我想覆盖类中的 `age` 属性但打错字，例如输入 `mike.aage = 34`，那么未使用插槽的类将创建一个新属性，而保持 `age` 属性不变。当你使用插槽时，Python 会抛出一个错误，因为它不知道类中有 `aage` 属性。

## 何时使用插槽？

**速度**：尽管插槽从百分比上加速了你的类，但每次操作的绝对时间增加是相当微不足道的。因此，如果你需要创建**大量**实例，或者需要多次重写或访问属性，插槽的使用会更具吸引力。

**内存**：如果你内存不足且希望节省每一个字节，使用插槽可能会有好处，因为它们显著减少了内存使用量。我们的简单类减少了 60% 的内存占用。

**安全性**：插槽防止你使用错误的属性和动态创建新属性。如果你尝试修改一个未知的属性，插槽类会抛出错误。

towardsdatascience.com ## 绝对初学者的 Cython：两步实现 30 倍更快的代码

### 轻松编译 Python 代码，实现极快的应用程序

[towardsdatascience.com

# 结论

正如我们在这篇文章中看到的，slots 以三种方式影响你的类：

+   **大小**：slots 消除了 Python 创建动态字典的需要，而是依赖于 **更小** 的固定大小数组，这间接通过减少对垃圾回收的需求来加速你的应用。

+   **速度**：slots 允许直接访问内存，绕过搜索字典的需要，这样会更快。速度提升在绝对意义上是相当微小的；节省了几纳秒。

+   **灵活性**：slots 防止在运行时添加属性，因此你的类变得有点不那么灵活。这也可能是件好事，因为当你使用动态属性创建时，你的代码可能会变得杂乱无章。

在我看来，减少的灵活性是我不常遇到的缺点：我从不动态创建属性，我喜欢 slots 保持属性静态。因此，我会尽可能使用 slots。在最坏的情况下，依赖关系可能会出问题，但在这种情况下，很容易再次移除 slots。

我希望这篇文章能像我希望的那样清晰，但如果不是这种情况，请告诉我我可以做些什么来进一步澄清。与此同时，查看我的[其他文章](https://mikehuls.com/articles)，涵盖了各种编程相关的话题，例如：

+   [绝对初学者的 Git：通过视频游戏理解 Git](https://mikehuls.medium.com/git-for-absolute-beginners-understanding-git-with-the-help-of-a-video-game-88826054459a)

+   [创建并发布你自己的 Python 包](https://mikehuls.medium.com/create-and-publish-your-own-python-package-ea45bee41cdc)

+   [用 FastAPI 在 5 行代码中创建一个快速的自动文档化、可维护、易于使用的 Python API](https://mikehuls.medium.com/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e)

+   [使用环境变量和文件与 docker 和 compose 的完整指南](https://mikehuls.medium.com/a-complete-guide-to-using-environment-variables-and-files-with-docker-and-compose-4549c21dc6af)

编程愉快！

— Mike

*附言：喜欢我做的事吗？* [*关注我！*](https://mikehuls.medium.com/membership)

[## 使用我的推荐链接加入 Medium — Mike Huls](https://mikehuls.medium.com/membership?source=post_page-----ab3f118abc71--------------------------------)

### 阅读 Mike Huls 的每一篇故事（以及 Medium 上其他数千名作家的故事）。你的会员费用直接支持 Mike...

[mikehuls.medium.com](https://mikehuls.medium.com/membership?source=post_page-----ab3f118abc71--------------------------------)
