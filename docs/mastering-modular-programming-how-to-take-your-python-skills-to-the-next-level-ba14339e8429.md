# 精通模块化编程：如何提升你的 Python 技能

> 原文：[`towardsdatascience.com/mastering-modular-programming-how-to-take-your-python-skills-to-the-next-level-ba14339e8429`](https://towardsdatascience.com/mastering-modular-programming-how-to-take-your-python-skills-to-the-next-level-ba14339e8429)

## 编写模块化 Python 代码的最佳实践

[](https://federicotrotta.medium.com/?source=post_page-----ba14339e8429--------------------------------)![Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----ba14339e8429--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ba14339e8429--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ba14339e8429--------------------------------) [Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----ba14339e8429--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----ba14339e8429--------------------------------) ·阅读时间 9 分钟·2023 年 4 月 10 日

--

![](img/7aee29745d7f4d3995e0b85e5c9f67d3.png)

图片由[全尚烨](https://pixabay.com/it/users/jeonsango-1594796/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1498954)提供，来源于[Pixabay](https://pixabay.com/it//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1498954)

我最近写了一篇关于[Python 类](https://medium.com/towards-data-science/python-classes-made-easy-the-definitive-guide-to-object-oriented-programming-881ed609fb6)的文章，其中提到类在 Python 中经常作为模块使用。

在这篇文章中，我将描述什么是模块，如何使用模块，以及为什么你应该在 Python 中使用模块。

在这篇文章的最后，如果你从未使用过模块，你将改变看法。你会改变你的编程方式，并在每次可能的情况下使用模块。我对此深信不疑。

我能理解你的感受：当我发现了模块及其强大功能时，我开始在每次可能的情况下使用它们。

我和你之间唯一的区别是我没有找到任何完整而有价值的资源来讲解**你需要**了解的所有关于 Python 模块的知识：这就是我创建这篇文章的原因。

我发现了一些零碎的资源。感谢这些资源和高级开发者的建议，经过几天的学习，我对 Python 模块有了全面的了解。

在阅读完这篇文章后，我相信你也会清楚地理解这个主题，并不需要其他资料去了解它。因此，如果你是 Python 新手，或者你是一个“摸索”中的 Python 爱好者但从未使用过模块，那么这篇文章绝对适合你。

# 为什么你应该将 Python 代码模块化

首先，让我们从一些你应该在 Python 中使用模块的理由开始讨论。我找到了 6 个理由：

1.  **更好的代码组织**。模块允许开发者将代码组织成可重用的单元；这些单元可以被导入并在其他程序中使用。这意味着你不再需要把你的 Python 应用程序看作是一个写满所有代码的整体程序。你会有一个主文件，在其中加载和使用模块。我们将在接下来的段落中以实际的方式看到这个理念。

1.  **代码重用性**。这是模块的一个优点：你创建一个独立的程序，可以在其他程序中导入和重用。可能需要稍微修改一下；但通常，你可以直接使用。这帮助我们节省了大量时间和编写代码的量：编写类似的代码发生的频率比你想象的要高。

1.  **命名空间管理**。如果你曾经用任何语言编码，你肯定遇到过需要命名变量的情况。随着代码在应用程序中增长，你可能需要写出具有相似名称的变量。模块化帮助我们修复名称，以便我们不必每次都重新发明轮子，因为我们将使用在模块中创建的名称，即使在主文件中也是如此。

1.  **协作**。当在公司工作或与朋友合作进行分布式项目（特别是使用 Git）时，模块化允许我们独立工作在模块上。这样，我们可以避免在同一文件中出现重叠问题。

1.  **更容易调试**。如果你调试过一个 200 行的应用程序，你就知道这有多痛苦。找到一个有几十行代码的错误可能需要几天时间。模块化避免了所有这些痛苦，因为模块彼此独立。它们代表了小程序，因此我们可以比调试一个整体应用程序更容易地调试它们。

1.  **性能优化**。模块可以优化你的机器性能，因为我们可以只导入我们在特定应用程序中需要的代码。

# Python 中的模块是什么？

正如[1]所说：“*在 Python 中，模块指的是包和库，但也包括任何可以与其他代码分离且可以独立工作的代码片段*”。

基本上，每个 Python 程序都是一个模块，因为它可以独立工作。但这并不意味着我们可以导入所有模块。或者，更好地说：这并不意味着将一个模块导入到另一个文件中总是有意义的。

例如，假设你想创建一个 Python 文件，检查是否存在三个文件夹。如果不存在，它将创建这些文件夹。它可能类似于这样：

```py
import os

# Define folders name
folders = ["fold_1", "fold_2", "fold_3"]

# Check if folders exist. If not, create folders
for folder in folders:
  if not os.path.isdir(folder):
    os.makedirs(folder)
```

这个文件可以保存为`folders.py`并运行。它可以独立工作，因此是一个模块。但让我问你一个问题：将它导入到另一个 Python 文件中是否有意义？

停下来思考一下，并回答这个问题。

好吧，答案是否定的，原因很简单：它太具体了。我的意思是：如果你需要将这三个文件夹重新创建到另一个文件夹中，你可以将文件移动到新文件夹中并运行它。

因此，当我们导入模块时，我们想要的是通用性。这就是为什么我们创建类和函数作为模块：因为它们是通用的。如果你想在其他文件中重用上述模块，你必须使它通用。例如，我们可以创建一个如下的函数：

```py
import os

def create_folders(fold_1, fold_2, fold_3):
  # Define folders name
  folders = [fold_1, fold_2, fold_3]

  # Check if folders exist. If not, create folders
  for folder in folders:
    if not os.path.isdir(folder):
      os.makedirs(folder)
```

因此，这个函数可以用来检查在一个特定的文件夹中是否存在三个通用的文件夹。但这次，名称可以是任何的，因为它们作为函数的参数传递。如果它们不存在，则会被创建。所以，例如，我们可以这样调用它：

```py
# Invoke the function
create_folders("audio", "image", "documents")
```

然后我们的函数会检查目录中是否存在“audio”、“image”和“documents”作为文件夹。如果不存在，它会创建这些文件夹。

这段代码适合被导入到另一个 Python 文件中，因为它是通用的：无论何时我们需要检查三个文件夹，我们都可以使用它。我们只需声明这三个文件夹的名称即可。

# 如何在 Python 中使用模块

从示意图来看，模块是这样工作的：

![](img/64d582ba1b9dd824fd71bafc7de85593.png)

模块工作示意图。作者图片。

我们有一个通常可以称为 `main.py` 的“主” Python 文件。在该 Python 文件中，我们导入了两个模块。

也许你没有注意到，但在上述代码中，我们导入了 `os` 模块，它是一个帮助创建、管理和删除目录的 Python 模块。这意味着在管理目录时，我们不需要创建自定义的函数（或类）：我们可以使用 `os` 模块并将其应用于我们的具体情况。例如，我们使用了 `os.makedirs` 来创建文件夹。

但除了已知的模块和包，我们如何在日常编程活动中使用 Python 的模块化呢？好吧，如果你从未使用过模块化的力量，你需要“转换”你的思维；但相信我，这值得一试！

如果你不使用模块，你的 Python 应用程序看起来会像这样：

![](img/8a41ae08b68f7b7e1c62481ffd3000f8.png)

一个单体 Python 应用程序。作者图片。

所以，在一个文件中你：

+   创建函数和类。

+   调用使用变量或其他你可能需要的内容创建的函数和类。

相反，如果你使用模块，你的 Python 应用程序会变成这样：

![](img/1450b35cf5692b0d77caa10d15d31fe5.png)

一个模块化的 Python 应用程序。作者图片。

因此，这样，你在单独的 Python 文件中创建两个函数和两个类——这些都是模块。然后，你将它们导入到主文件中。

此时，你的文件夹组织应该是这样的：

```py
project_name

├── main.py
│   └── packages
│       └── __init__.py
│       └── module_1.py
│       └── module_2.py
│       └── module_3.py
└──     └── module_4.py
```

当然，你可以创建你所需的任何数量的 `packages` 子文件夹。

重要的是，每个子文件夹中必须有一个 `__init__.py` 文件。

让我们看看它是什么，以及如何使用它。

# Python 的一个实际示例

那么，让我们做一个实际的 Python 示例。我们创建一个名为 `my_project` 的项目，其中包含：

+   一个 `main.py` 文件是我们的主要文件。

+   一个名为 `operations` 的子文件夹，其中我们将有三个 Python 文件：`__init__.py`、`addition.py`、`subtraction.py`。

所以，这是我们的结构：

```py
my_project

├── main.py
│   └── operations
│       └── __init__.py
│       └── addition.py
└──     └── subtraction.py
```

我们想创建两个简单的模块，用于计算两个整数的和与差。如果输入不是整数，程序将返回错误。

所以，这两个模块可以是这样的：

```py
def sum(x: int, y: int) -> int:
        """ this function sums two integers.

        Args:
            param 1: integer                         
            param 2: integer

        Returns:
            an integer that is the sum of the two arguments
        """
        if type(x) and type(y) == int:
            print(f"the sum of these two numbers is: {x+y}")
        else:
            print("arguments are not integers!")

if __name__ == "__main__":
     sum()
```

和这些：

```py
def diff(x: int, y: int) -> int:
        """ this function subtracts two integers.

        Args:
            param 1: integer                         
            param 2: integer

        Returns:
            an integer that is the difference of the two arguments
        """
        if type(x) and type(y) == int:
            print(f"the difference of these two numbers is: {x-y}")
        else:
            print("arguments are not integers!")

if __name__ == "__main__":
     diff()
```

```py
**NOTE:**

If you are not familiar with this way of writing Python with type hints,
and if you don't know how ' if __name__ == "__main__"' work, you
have to read my article [here](https://medium.com/towards-data-science/python-classes-made-easy-the-definitive-guide-to-object-oriented-programming-881ed609fb6) and eveything will be clarified.
```

现在，我们希望将这两个模块导入到主文件中。为此，我们必须在 `__init__.py` 中写下以下内容：

```py
__all__ = ["sum", "diff"]
```

所以，`__init__.py` 是一个在处理模块时必需的 Python 文件，因为它以某种方式告诉主文件在 `operations` 文件夹中有一些包需要导入。因此，在 `__init__.py` 中，我们必须声明所有包中的函数，如上所示。

现在，`main.py` 可以像这样编程：

```py
from operations.addition import sum
from operations.subtraction import diff

# Sum two numbers
a = sum(1, 2)
# Subtract two numbers
b = diff(3,2)

# Print results
print(a)
print(b)
```

关于主文件，有几个考虑因素：

1.  由于这些模块每个只包含一个函数，我们可以写 `from operations.addition import *`。不过，从模块中仅导入我们使用的函数和类是一种良好的实践。

1.  如我们所见，主文件非常简洁。它仅包含导入、使用导入方法声明的变量和打印结果。

# 最后的提示

我从前辈那里得到的建议是为每个模块创建一个类，因为这样更容易维护。然而，这并不是必须的：这取决于你编写的代码。

在上面的示例中，我创建了两个包含一个函数的单独模块。一种替代方案是创建一个名为 `Operations` 的类，并将这两个函数作为其方法来编写。

这将有助于我们在一个文件中创建“类似函数”，并在主文件中进行一次导入。

所以，不要总是把建议当作必须的：接受它们，但要根据你需要创建的东西来思考，同时考虑你需要的优化。

# 结论

现在，告诉我：你对这种方法有多兴奋？好吧，当我理清了对模块的理解后，我开始将它们应用到我的日常工作中，相信我，情况只会变得更好。

希望我已经为你澄清了这个话题；如果没有，请：在评论中告诉我。我可以创作其他文章作为补充。

+   *订阅* [*我的 Substack 新闻通讯*](https://federicotrotta.substack.com/?r=1ep1nf&utm_campaign=pub-share-checklist) *以获取更多 Python 相关内容。*

+   *通过* [***我的推荐链接***](https://federicotrotta.medium.com/membership)*加入 Medium*：以 5$/月（无额外费用）解锁 Medium 上的所有内容。

+   *在这里找到/联系我：* [*https://bio.link/federicotrotta*](https://bio.link/federicotrotta)

+   *觉得有用吗？* ***请给我买杯*** [***Ko-fi***](https://ko-fi.com/federicotrotta)***。***

*更多来自我的：*

[](/python-classes-made-easy-the-definitive-guide-to-object-oriented-programming-881ed609fb6?source=post_page-----ba14339e8429--------------------------------) [## Python 类简易教程：面向对象编程的终极指南

### 通过这本全面的类参考提升你的 Python 技能

[Python 循环：如何在 Python 中迭代的完整指南](https://towardsdatascience.com/python-loops-a-complete-guide-on-how-to-iterate-in-python-b29e0d12211d?source=post_page-----ba14339e8429--------------------------------)

### 利用 Python 中循环的强大功能

[Python 循环：如何在 Python 中迭代的完整指南](https://towardsdatascience.com/python-loops-a-complete-guide-on-how-to-iterate-in-python-b29e0d12211d?source=post_page-----ba14339e8429--------------------------------)

*视频制作：*

[1] 如果 __name__ == “__main__” 对于 Python 开发者 ([视频](https://www.youtube.com/watch?v=NB5LGzmSiCs))
