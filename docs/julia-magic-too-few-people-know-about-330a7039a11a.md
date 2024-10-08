# Julia 魔法 太少人知道

> 原文：[`towardsdatascience.com/julia-magic-too-few-people-know-about-330a7039a11a`](https://towardsdatascience.com/julia-magic-too-few-people-know-about-330a7039a11a)

## Julia 编程语言的一些不太为人知的功能。

[](https://emmaccode.medium.com/?source=post_page-----330a7039a11a--------------------------------)![Emma Boudreau](https://emmaccode.medium.com/?source=post_page-----330a7039a11a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----330a7039a11a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----330a7039a11a--------------------------------) [Emma Boudreau](https://emmaccode.medium.com/?source=post_page-----330a7039a11a--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----330a7039a11a--------------------------------) ·阅读时间 10 分钟·2023 年 10 月 15 日

--

![](img/b22e53ebd32ae5b1747afc050ab2696b.png)

（图像由作者提供）

## 介绍

就编程语言及其范式而言，Julia 的方法极为独特。与今天流行的其他多范式编程语言相比，Julia 在许多方面显著不同。尤其在范式方面，即语言中的类型（持有数据）如何与语言中的函数或方法协作。对此已经提出了许多不同的解决方案，每种语言往往都处于某一类别 — 尽管不一定完全如此。大多数现代语言通过这些范式桥接了多个编程概念，这使得大多数现代编程语言都是多范式的。在这个大伞下，Julia 语言也被包括在内。

尽管 Julia 是一种多范式编程语言，但它建立在一个以多重分派为中心的独特范式上。这并不是说这种范式完全新颖，这种范式的基础可以在标准元语言（SML）中找到。实际上，因为 Julia，我回去尝试了那个语言——这是一次非常有趣的经历，如果你想了解更多关于这个经历的内容，这里有一篇文章的链接：

[](/getting-introduced-to-sml-a-weird-polymorphic-language-originated-in-1983-797ff9d6622e?source=post_page-----330a7039a11a--------------------------------) ## 介绍 SML：一种源于 1983 年的奇异多态语言

### 试着用 SML 语言编写和编译一些基本代码

towardsdatascience.com

Julia 能做很多完全独特的事情，这些功能在语言中是独一无二的。这种范式及其特性集为编程语言带来了很多兴奋感和新能力，但也创造了许多障碍。利用这些复杂性对数据科学来说意义重大。能力带来了复杂性，考虑到 Julia 中某些有些独特的细微差别，用户可能会发现获得完整的 Julia 体验有些棘手。需要学习的东西很多，Julia 是一个非常棒的语言，值得充分利用！

就数据科学而言，Julia 是新手中的新秀，但其社区开发一直很稳固，语言每天都变得越来越有前景。也就是说，在你的数据科学项目中使用 Julia 时，了解这门语言非常有用。尤其是考虑到 Julia 的 `Base` 中有很多科学内容。Julia 是考虑到数据科学而设计的，因此以这种方式学习 Julia 对于掌握语言中的数据科学非常有帮助！

> [olive notebook](https://github.com/ChifiSource/OliveNotebooks.jl/blob/main/julia/random/less_known_jl.jl)

## 初始化参数

我有时会忘记 Julia 的一个特性，即初始化参数。这允许我们使用一些操作获得一个参数。这在许多地方都有应用，我发现这非常有用。这对于创建一个从另一个提供的参数中拉取数据的默认值特别有用。

```py
function remove!(vec::Vector{Int64}, index::Int64 = length(vec))
end
```

在这里我们创建了一个 `index` 参数，默认情况下它将始终是 `vec` 的 `length`。很有用！在我看来，这比那些没有这个特性的语言中的解决方案要好得多。考虑到这一点，我非常欣赏 Julia 中这个特性——一个新的 Julia 用户肯定会错过它，一个经验丰富的用户也可能会忘记它的存在——但在情况合适时，它确实是一个非常好的功能，并且使 API 远远优于其他！

## 检查方法

Julia 中的 `Methods` 带有一种在语言及其范式中独特的内涵。在 Julia 中，方法被定义为函数名称与为该函数编写方法的参数类型一起出现。换句话说，

```py
function example(x::Any)

end
```

在上面的例子中，`example`是一个函数，`example(::Any)`是它的第一个方法。我们不仅定义了函数，还定义了它的第一个方法。我们可以使用`methods`方法将这些方法放入`Base.MethodList`中。这对于任何我们可能想要进行的自省都很有用，并且`MethodList`的显示对于学习函数的分发非常有帮助。Julia 系统的显著优点是构建的函数更少——方法更多。显著的缺点是这意味着你需要学习每个函数的多个方法以及它们如何应用于每种类型。幸运的是，这种方法对于学习这些内容非常有帮助。

```py
methods(example)
```

![](img/74524d8f7019eec3f9e2db8f59ad775e.png)

（作者提供的图片）

不过，方法自省的应用不仅限于学习函数。这也是我项目 Olive 的**多重分发**笔记本的基础。在这种情况下，我使用方法列表来寻找构建单元的新方法——使加载扩展就像定义方法一样简单。如果你想查看该项目，这里是一个链接：

[](https://github.com/ChifiSource/Olive.jl?source=post_page-----330a7039a11a--------------------------------) [## GitHub - ChifiSource/Olive.jl: 纯 Julia 笔记本

### 纯 Julia 笔记本。在 GitHub 上创建一个账户，贡献于 ChifiSource/Olive.jl 的开发。

github.com](https://github.com/ChifiSource/Olive.jl?source=post_page-----330a7039a11a--------------------------------)

使用 Julia，我们可以轻松检查任何函数及其方法——以及`Main`中的几乎所有其他内容。自省在这种语言中非常突出，并且非常有用，因此这无疑是我非常欣赏 Julia 的另一个特性。

[](/runtime-introspection-julias-most-powerful-best-kept-secret-bf845e282367?source=post_page-----330a7039a11a--------------------------------) ## 运行时自省：Julia 最强大、最保密的秘密

### 观察如何在 Julia 中使用自省来处理类型

towardsdatascience.com

解决这个难题的最重要部分是`Method`类型中的字段`sig`。通过使用它，我们可以在代码中描述有关方法的内容，这在某些情况下非常有用。

```py
methods(example)[1].sig

Tuple{typeof(Main.NPaeMcfIjB.example), Any}
```

这个签名包含了我们的`Function`的类型和每个参数的类型，这个例子有一个，`Any`。

## 删除方法

我觉得 Julia 非常酷的一点是，我们既可以添加**也**可以删除方法。从本质上讲，这意味着任何`Method`都可以被替换，方法可以根据特定情况被移除。Julia 的 Base 部分，或者任何添加的 Julia 包，都对你可以做的事情非常开放。在 Julia 中，我们可以`import`并扩展一些方法。

```py
import Base: cd
```

我们还可以删除方法。很长一段时间我不知道可以这样做，当这种情况发生时，我总是很兴奋地分享它。现在我们已经导入了 `cd`，我们使用 `Base.delete_method` 对该函数的 `Method` 进行删除。我们通过 `methods` 方法检索这个 `Method` — *天哪*。

```py
Base.delete_method(methods(cd)[1])
```

## 调度一个参数

Julia 的另一个很棒的方面是类型参数。参数是**类型的区分**，它告诉我们关于结构是如何构造的一些信息。例如，以下的 `Vector` 具有一个表示其元素类型的参数 `Int64`。

```py
vec = Vector{Int64}([5, 10, 15, 20])
```

这在多种情况下都非常有用。对于新用户来说，我看到参数是学习 Julia 的一个常见障碍 — 不熟悉类型参数的确可能使这变得令人望而生畏。然而，它们非常有用，而且相对简单易用。这些参数用于创建参数化的多重调度或参数化多态性 — 根据参数变更多个类型的泛型函数。这也是 Julia 中一个非常强大的特性。

```py
function example(vec::Vector{Int64} = [1, 2])

end
```

另一个非常酷的方面是我们可以根据超类型进行调度。这意味着在我们的抽象层次结构中的任何点，我们可以使用多重调度来涵盖所有下面的类型。例如，我们希望所有 `Real` 数字都作为参数……我们通过在参数中的抽象类型前使用子类型操作符 `<:` 来实现这一点。

```py
function example(vec::Vector{<:Real})

end
```

现在 `example` 可以与任何包含 `Real` 子类型的 `Vector` 一起使用。如果我们不使用子类型。通过这种方式，我们可以尽可能通用地编写方法。结果是，Julia 项目所需的代码要少得多 — 这种范式使得精确定位所需的东西并为其编写方法变得极其简单。锦上添花的是，能够用任何 `Method` 完成这件事。

## 超棒的 comprehensions

Julia 具有我使用过的任何编程语言中**最佳**的理解能力。这部分得益于 Julia 表达性丰富的语法，它在多方面都有贡献。例如，Julia 的解析器不关心空白符 — 我们也可以将任何内容用 `begin` 和 `end` 包裹起来。由于 comprehensions 在 Julia 中非常有用（且快速），我很少看到使用传统的 `for` 循环的理由。唯一的例外是当我想使用 `continue` 或 `break` 时。另一个让这些在这种情况下非常有用的原因是它们总是返回一个 `Vector`。

```py
myvec = [x + 1 for x in 1:5]
5-element Vector{Int64}: 2 3 4 5 6
```

很少有编程语言的 comprehensions 如此灵活。我认为更多用户应该尝试利用这一点。在上面的例子中，我们有一个完整的条件和一个返回，没有附加或类似的操作。这是 Julia 的一个非常好的特性，可以使代码变得极其简洁。

```py
[begin 
    if x > 5
      true
    else
      false
    end
 end for x in myvec]
```

![](img/bafd2956f639f5cf1a848c9bf2f02845.png)

（图像由作者提供）

## 表达式检查

在元编程方面，Julia 是一门带来许多可能性的语言。通过一些 Julia 的经验，我们可能会熟悉 Julia 中元编程的基础知识。这主要通过 `eval` 和 `Meta.parse` 完成。

```py
exp = Meta.parse("x = 5")

eval(exp)
```

`Meta` 是 `Base` 内部的一个 `Module`，这与 `Base.parse` 是不同的方法。虽然这是这种类型检查的一个更基础形式，但我们也可以通过查看字段进一步了解 `Expr`。

![](img/b1606459ff634a3d8dd91b009f78ada9.png)

`Expr` 类型的两个主要字段是 `head` 和 `args`。在这种情况下，`head` 是函数调用，而 `args` 是提供的参数。

```py
fieldnames(Expr)
(:head, :args)

exp.head
:(=)

exp.args

2-element Vector{Any}: :x 5
```

## eval 是模块的一个字段

对于这最后三项，我们将转到 REPL，因为在某些情况下我们需要终端来进行这些示例。

在作用域方面，Julia 提供了所需的一切——就像许多语言一样。Julia 和大多数同类语言一样，使用词法作用域。这是一种向下的作用域，就像山一样，随着你在作用域层次中向上移动，它变得越来越小。这意味着每次打开一个新作用域时，它可以访问上层作用域，但不能访问下层作用域。考虑以下示例：

```py
y = 5
module Example
    x = 5
end
```

在上述示例中，`Example` 位于 `Main` 下面的一个新范围内。这意味着 `Example` 可以访问 `Main`，但 `Main` 无法访问 `Example`。因此，`y` 在 `Example` 中定义，但 `x` 在 `Main` 中未定义。在编程中执行大多数操作时，如循环、条件判断和方法调用，都会建立一个新的范围层次，并拥有自己的作用域。这是编程中需要考虑的重要因素。

在 Julia 中，模块是包含名称的容器，这些名称本质上只是字段。这些名称是该模块内定义的所有内容。在每个模块内部，我们还有一个本地的 `eval`。因此，在之前提到的 `Example` 函数的情况下，`Example.eval` 是一个函数。

```py
Example.eval(Meta.parse("x += 1"))

example.x

6
```

## 管道运算符

另一个我常常忘记的非常酷的功能是 Julia 具备管道功能。管道允许我们以非常易读的方式将多个函数调用链接在一起。通过管道，我们将多个函数调用合并成一行代码。Julia 中的管道功能也很简单而且简洁。这是通过 `|>` 管道运算符以实用的方式逐个函数完成的。

```py
julia> do_stuff(a::Int64) = a * 5 * 2 - a + 2
do_stuff (generic function with 1 method)

julia> add5(a::Int64) = a += 5
add5 (generic function with 1 method)
```

让我们将一个数字通过这两个函数：

```py
julia> a |> add5 |> do_stuff
92
```

## 重定向标准输出

我想谈的最后一点是，在 Julia 中重定向 IO 是意外简单的。在其他语言中，例如 Python，这是一项令人惊讶的艰巨任务，需要大量知识和工作。在 Julia 中，这只是一个简单的方法调用，非常容易使用。要在 Julia 中重定向标准输出，我们只需使用`redirect_stdout`方法！唯一的挑战是这个`Method`需要一个`IOStream`或`Pipe`。只有`Pipe`在内存中，因此最简单的用例是将其用来写入文件。

```py
open("myfile.txt", "w") do o
    redirect_stdout(o) do io
        println("hello!")
    end
end
```

这方面可以做很多事情，如果能存储在内存中，还能做更多。我有一整篇文章详细讨论了这个函数及其所有用法。如果你想了解更多，这里是那篇文章的链接：

[## 在 Julia 中重定向输出：详细解析](https://chifi.dev/redirecting-output-in-julia-the-rundown-7a1c814f842f?source=post_page-----330a7039a11a--------------------------------)

### 关于在 Julia 中重定向 STDIO，你需要知道的一切。

[chifi.dev](https://chifi.dev/redirecting-output-in-julia-the-rundown-7a1c814f842f?source=post_page-----330a7039a11a--------------------------------)

## 结论

Julia 的最大优势在于它的许多特性如何发挥其核心范式。Julia 的最大弱点在于这些许多特性为新用户使用它们设置了障碍。此外，这可能使新用户更容易在代码中迷失。Julia 的语法能够使代码尽可能简洁，同时仍然非常易读，但如果你不知道如何阅读它，仍然存在问题。话虽如此，Julia 还有很多令人惊叹的特性，我最喜欢的 Julia 方面是很多人甚至不知道它们的存在！希望今天分享这些内容对你有帮助！感谢阅读！
