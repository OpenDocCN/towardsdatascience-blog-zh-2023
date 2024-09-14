# 如何使用日志模块调试 Python 脚本

> 原文：[`towardsdatascience.com/how-to-troubleshoot-python-scripts-with-the-logging-module-aaf630c35fe3`](https://towardsdatascience.com/how-to-troubleshoot-python-scripts-with-the-logging-module-aaf630c35fe3)

## 打印语句只能带你走到一定程度…

[](https://medium.com/@aashishnair?source=post_page-----aaf630c35fe3--------------------------------)![Aashish Nair](https://medium.com/@aashishnair?source=post_page-----aaf630c35fe3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----aaf630c35fe3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----aaf630c35fe3--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----aaf630c35fe3--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----aaf630c35fe3--------------------------------) ·阅读时间 7 分钟·2023 年 8 月 26 日

--

![](img/88282bab14f50541e0bba68c4d0cf0bc.png)

图片来源：Tima Miroshnichenko: [`www.pexels.com/photo/a-person-writing-on-a-notebook-5336909/`](https://www.pexels.com/photo/a-person-writing-on-a-notebook-5336909/)

## 目录

∘ 介绍

∘ 日志模块

∘ 日志级别

∘ 配置级别

∘ 调试级别配置

∘ 创建日志文件

∘ 格式化日志消息

∘ 关键要点

## 介绍

考虑以下情况：你编写了一段代码，该代码要么返回错误，要么产生意外的值。

```py
x1 = function1(x)
x2 = function2(x1)
x3 = function3(x2)
```

为了找出错误的代码行，你会写一个打印语句…

```py
x1 = function1(x)
print(x1)
x2 = function2(x1)
x3 = function3(x2)
```

然后再添加一个打印语句…

```py
x1 = function1(x)
print(x1)
x2 = function2(x1)
print(x2)
x3 = function3(x2)
```

然后再跟进另一个打印语句。

```py
x1 = function1(x)
print(x1)
x2 = function2(x1)
print(x2)
x3 = function3(x2)
print(x3)
```

一旦你识别并修复了问题，这些打印语句就变得无用。所以，你一个个地删除或注释掉它们：

```py
x1 = function1(x)
#print(x1)
x2 = function2(x1)
#print(x2)
x3 = function3(x2)
#print(x3)
```

如果你的故障排除经验类似于上述场景，你一定对使用打印语句处理错误代码行的挫败感感同身受。

幸运的是，Python 中有一个工具提供了更有效的调试策略：日志模块。

在这里，我们深入探讨日志模块的基本功能，并探索使其成为强大故障排除工具的特点。

## 日志模块

日志模块是为那些希望有效跟踪程序中某些事件的程序员设计的。

幸运的是，学习使用该工具没有任何前提条件，特别是如果你已经熟悉 Python 的打印语句。

从语法上看，日志模块的命令与打印语句非常相似，允许用户使用简单的一行代码生成消息。

```py
 import logging

logging.warning('Hello World')
```

![](img/cfb5aa4dd8e09a88fb20087738a2030d.png)

代码输出（由作者创建）

话虽如此，日志记录包括了常规 print 语句所不提供的功能，我们现在将介绍这些功能。

## 日志等级

在日志模块中，并非所有消息都是*相等*的。日志模块中的事件被分解为不同的重要性等级。

日志模块的[文档](https://docs.python.org/3/library/logging.html)列出了 5 个日志等级，并解释了它们的使用场景。

![](img/0465e1d48a4b88bd3e482395b22fbe69.png)

日志等级（作者创建）

换句话说，当用户使用日志记录消息时，他们还可以设置该消息的重要性。INFO 消息最适合监控事件状态，WARNING 消息最适合发出警告，而 ERROR 消息最适合报告错误。

因此，用户可以根据消息的目的使用等级来分配其消息的重要性。例如，在加载数据集时，可以编写 INFO 和 ERROR 等级的消息。

```py
try:
    mock_data = pd.read_csv('random_data_source.csv')
    logging.info('random_data_source.csv has been loaded')
except:
    logging.error('random_data_source.csv does not exist')
```

通过对消息分配不同的重要性等级，用户可以*定义*他们希望看到或不希望看到的消息的重要性阈值。

默认情况下，日志模块只考虑 WARNING 及以上等级的日志。

![](img/08354bb0fac575276cfb8b2163a96dd7.png)

日志等级（作者创建）

这意味着，未进行额外配置的消息记录不会返回 DEBUG 和 INFO 等级的消息。

![](img/24c3353aee9ff74e3c7fa500650c135e.png)

代码输出（作者创建）

如输出所示，实际上只有 WARNING、ERROR 和 CRITICAL 等级的消息被生成。

## 配置等级

与 print 语句不同，日志允许用户配置应该生成的消息的重要性等级。

例如，假设我们需要记录 INFO 及以上等级的消息。

![](img/ee69937bacb51685334abe11c844f4e0.png)

日志等级（作者创建）

我们可以通过使用`basicConfig`方法并定义`level`参数来指示程序生成 INFO 等级的消息：

现在，所有 INFO 及以上等级的消息都会被显示。

![](img/b2bc07db37eb40e5967110a343b9922e.png)

代码输出（作者创建）

由于程序已配置为接受 INFO 及以上等级的日志，因此记录了所有非 DEBUG 等级的日志。

## **调试等级配置**

通过`basicConfig`方法，用户可以调整日志等级的阈值，以包含所需等级而忽略不需要的等级。

例如，假设我们正在对一个变量进行转换，并希望在每次转换后找到该变量的值。在这里，我们对变量 x 执行数学运算：

```py
import logging 

logging.basicConfig(level=logging.DEBUG)

x = 5
logging.info('Performing mathematical operations on x')

x = x * 2
logging.debug(f'x * 2 = {x}')

x = x + 6
logging.debug(f'x + 6 = {x}')

x = x / 4
logging.debug(f'x divided by 4 = {x}')
```

阈值日志等级已设置为 DEBUG，这意味着所有日志消息都会被创建。运行代码将返回如下结果：

![](img/ead65573e741b39b7b40ad6c5d81b5ba.png)

代码输出（作者创建）

这些消息可能很有用，但一旦调试完成，它们便没有任何价值。那么，我们如何阻止这些消息在未来的迭代中被生成呢？

如果我们使用打印语句，解决方案是删除或注释掉每一个不需要的打印语句。幸运的是，使用日志记录时，可以通过简单地修改`basicConfig`方法中的日志级别阈值来移除不需要的调试消息。

现在，我们可以将日志级别更改为 INFO，从而省略所有 DEBUG 级别重要性的日志消息。

```py
 logging.basicConfig(level=logging.INFO) # Change from DEBUG to INFO

x = 5
logging.info('Performing mathematical operations on x')

x = x * 2
logging.debug(f'x * 2 = {x}')

x = x + 6
logging.debug(f'x + 6 = {x}')

x = x / 4
logging.debug(f'x divided by 4 = {x}')
```

![](img/8423850df155d0c9d7b699a22598cc87.png)

代码输出（由作者创建）

这一次，即使日志命令仍在使用，它们也会被省略，因为 DEBUG 级别低于分配的 INFO 级别阈值。

同样，当我们希望开始调试过程时，我们可以使用日志记录来做相反的事情：降低日志消息的阈值重要性，以便程序生成较低重要性的消息。

## 创建日志文件

使用打印语句时，消息会显示在命令行或控制台上，但不会存储在任何位置。

使用日志模块的`basicConfig`方法，用户可以创建一个专门记录所有由模块创建的消息的文件。

例如，以下代码片段使用`basicConfig`方法将所有消息保存到一个名为“demo.log”的新创建的文件中。

```py
import logging

logging.basicConfig(filename='demo.log',
                    level=logging.DEBUG)

logging.debug('Debug message')
logging.info('Info message')
```

现在，当运行脚本时，控制台不会显示任何消息。相反，消息将直接存储在“demo.log”文件中。

![](img/c99c75915c4ac7911131a7d9bd887a14.png)

demo.log（由作者创建）

此外，这些记录的消息不会被覆盖；当添加更多日志时，它们会保留在“demo.log”文件中。如果我们再次运行相同的脚本，新日志消息将会被添加到新的一行中。

![](img/047e5706bfa4ac5f610eb7561c402344.png)

demo.log（由作者创建）

## 格式化日志消息

日志中的消息并不总是那么具有信息性和可读性。

为了生成更适合给定用例的消息，用户可以使用`basicConfig`方法配置消息的格式。

在下面的代码片段中，我们配置消息的格式为时间戳、日志级别和消息内容。

```py
logging.basicConfig(filename='demo.log',
                    level=logging.DEBUG,
                    format='%(asctime)s %(levelname)s %(message)s'
                    )

logging.debug('Debug message')
logging.info('Info message')
```

![](img/bd4a19bd578a493db50bcec4894254b5.png)

代码输出（由作者创建）

确立日志的格式可以使生成的消息既具有信息性又具有可读性。当然，理想的格式取决于程序员的个人偏好。

## 关键要点

![](img/9916e1f7b6d3302c5e191eea149e4b56.png)

照片由[Prateek Katyal](https://unsplash.com/@prateekkatyal?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

总的来说，尽管打印语句通常被视为检查或调试代码的首选工具，但日志模块是更合适的工具。

日志提供了一个系统，将消息分类为不同的重要级别。此外，它们还具备一些功能，使用户能够轻松配置生成哪些消息、如何格式化这些消息以及它们应该存储在哪里。

日志模块能够让调试过程更快捷、无压力，因此，如果你厌倦了使用普通的、通用的打印语句，考虑采用它。

感谢阅读！
