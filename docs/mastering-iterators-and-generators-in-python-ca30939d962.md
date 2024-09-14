# 掌握 Python 中的迭代器和生成器

> 原文：[`towardsdatascience.com/mastering-iterators-and-generators-in-python-ca30939d962`](https://towardsdatascience.com/mastering-iterators-and-generators-in-python-ca30939d962)

## Python 为 AI 工程师和数据科学家

## 创建自定义迭代器和生成器以实现高效的数据处理

[](https://jvision.medium.com/?source=post_page-----ca30939d962--------------------------------)![Joseph Robinson, Ph.D.](https://jvision.medium.com/?source=post_page-----ca30939d962--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ca30939d962--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ca30939d962--------------------------------) [Joseph Robinson, Ph.D.](https://jvision.medium.com/?source=post_page-----ca30939d962--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ca30939d962--------------------------------) ·11 分钟阅读·2023 年 1 月 17 日

--

![](img/8c930dd3fe4ded63beb436fb0128f391.png)

视觉效果由作者创建。

本博客文章深入探讨了 Python 中迭代器和生成器的世界，提供了如何使用它们来优化代码并提高程序性能和效率的详细指南。文章涵盖了迭代器和生成器的概念，包括它们的工作原理以及如何为特定用例创建自定义迭代器和生成器。博客还探讨了结合迭代器和生成器的高级技术，以实现特定功能并创建复杂的数据处理管道。最后，我们回顾了在 Keras 中用于训练深度网络的示例数据加载器。通过本指南，你将学习如何掌握 Python 中迭代器和生成器编程的艺术，并将数据处理能力提升到一个新的水平。

# 目录

· 介绍

· 理解迭代器

· 创建自定义迭代器

· 理解生成器

· 创建自定义生成器

· 组合迭代器和生成器

· 深度学习中的迭代器和生成器

· 结论

· 附加资源

· 联系

# 介绍

Python 是一种多功能且强大的编程语言，提供了多种开发者可以使用的特性。其中之一就是创建和使用迭代器和生成器的能力。理解并掌握 Python 中迭代器和生成器的概念可以显著提升你的编码技能，并改善程序的性能。

迭代器是可以被迭代（循环）的对象。它们代表一个数据流，并实现了迭代器协议，该协议包含 `iter()` 和 `next()` 方法。另一方面，生成器是一种迭代器，允许你声明一个像迭代器一样工作的函数。它们使用 yield 语句返回数据，并且是一种处理大型数据集时更节省内存的方式。

本博客文章将深入探讨 Python 的迭代器和生成器世界。我们将探讨迭代器和生成器的概念以及它们在底层的工作原理。我们还将介绍如何创建自定义迭代器和生成器，并查看如何将它们结合起来以在程序中实现特定功能的高级技巧。阅读完本文后，你将对如何在 Python 中使用迭代器和生成器编写高效且有效的代码有一个深入的理解。

# 理解迭代器

在 Python 中，迭代器是一个可以被迭代（循环）的对象。它是一个返回数据的对象，一次一个元素。它们代表一个数据流，并实现了包含 `iter()` 和 `next()` 方法的迭代器协议。`iter()` 方法返回迭代器对象本身，`next()` 方法返回迭代器中的下一个项。

要在 Python 中创建迭代器，我们需要在类中实现迭代器协议，即 `iter()` 和 `next()` 方法。一旦完成，类的实例可以用作迭代器。

了解每个实现了 `iter()` 方法的 Python 对象都可以被视为可迭代对象，但并非所有对象都是迭代器。一个 `Iterator` 还有另一个方法 `next()`，用于获取下一个项。

对象要被视为迭代器必须遵循迭代协议。该协议要求对象实现两个方法：`iter()` 和 `next()`。`iter()` 方法应返回迭代器对象，而 `next()` 方法应返回迭代器中的下一个项。如果没有更多项可返回，`next()` 方法应引发 `StopIteration` 异常。

总结来说，迭代器是可以被迭代的对象，它们实现了包含 `iter()` 和 `next()` 方法的迭代器协议。它们是用于遍历容器元素的对象。

# 创建自定义迭代器

在 Python 中创建自定义迭代器允许你为特定的使用案例定义迭代器的行为。要创建自定义迭代器，你需要定义一个实现了迭代器协议的类，该类包含 `iter()` 和 `next()` 方法。

`iter()` 方法应返回迭代器对象本身，通常是返回 `self`。`next()` 方法应返回迭代器中的下一个项，并在没有更多项可返回时引发 StopIteration 异常。

这是一个简单的自定义迭代器类的示例，它迭代一个数字范围：

```py
class MyIterator:
    def __init__(self, start, end):
        self.start = start
        self.end = end
        self.current = start

    def __iter__(self):
        return self

    def __next__(self):
        if self.current > self.end:
            raise StopIteration
        else:
            self.current += 1
            return self.current - 1
```

这个迭代器类接受起始值和结束值作为参数，并在迭代时返回范围[`start`, `end`]内的数字。

实现上述类并创建其实例后，我们可以将其用作迭代器，例如：

```py
for i in MyIterator(1,5):
    print(i)
```

这将打印从 1 到 5 的数字。

为大型数据集创建自定义迭代器是一种高效的处理方式。通过创建自定义迭代器，你可以控制数据的访问方式，并限制任何给定时间加载到内存中的数据量。

总结起来，创建自定义迭代器在 Python 中允许你通过实现迭代器协议、`iter()`和`next()`方法来定义迭代器的行为，以适应特定的使用案例。这是一种高效处理大型数据集的方法，通过控制数据的访问方式并限制任何给定时间加载到内存中的数据量。

# 理解生成器

生成器是 Python 中的一种迭代器类型，它允许你声明一个像迭代器一样行为的函数。它们使用`yield`语句返回数据，并且是处理大型数据集时更节省内存的方式。

生成器函数的定义类似于常规函数，但它使用`yield`代替`return`语句。当生成器函数被调用时，它返回一个生成器对象，但不会立即执行函数体。函数体只有在调用其`next()`方法时才会被执行。

下面是一个简单的生成器函数示例，它生成一个范围内的数字：

```py
def my_gen(start, end):
    current = start
    while current < end:
        yield current
        current += 1
```

这个生成器接受起始值和结束值作为参数，并生成该范围内的数字以进行迭代。

你可以将这个生成器函数用作迭代器，例如：

```py
for i in my_gen(1,5):
    print(i)
```

这将打印从 1 到 5 的数字。

生成器是一种更节省内存的处理大数据的方式，因为整个数据集不会存储在内存中。而是每次调用`next()`时生成一个数据项。

总结起来，生成器是 Python 中的一种迭代器类型，它允许你声明一个像迭代器一样行为的函数。它们使用`yield`语句返回数据，并且是处理大型数据集时更节省内存的方式。它们的定义类似于常规函数，返回一个生成器对象，一个迭代器，当生成器的`next()`方法被调用时执行函数体。

# 创建自定义生成器

在 Python 中创建自定义生成器允许你为特定的使用案例定义生成器的行为。要创建自定义生成器，你需要定义一个使用`yield`语句返回数据的函数。

下面是一个简单的自定义生成器函数示例，它生成斐波那契数列：

```py
def fibonacci(n):
    a, b = 0, 1
    for i in range(n):
        yield a
        a, b = b, a + b
```

这个生成器函数接受一个参数`n`，并在迭代时生成斐波那契数列的前*n*个数字。

你可以将这个生成器函数用作迭代器，例如：

```py
for i in fibonacci(5):
    print(i)
```

这将打印斐波那契数列的前五个数字。

基于生成器的协程是另一种在 Python 中使用生成器的方法，它允许你在没有显式锁、线程或多进程的情况下同步编写并发代码。协程是可以被暂停和恢复的函数，它们使用`yield`语句返回数据。

为大型数据集创建自定义生成器是一种高效的处理方法。然后，你可以控制数据的访问方式，并限制在任何给定时间内加载到内存中的数据量。

总之，在 Python 中创建自定义生成器允许你通过定义一个使用`yield`语句的函数来为特定用例定义生成器的行为。基于生成器的协程是另一种使用生成器的方法。它们允许你同步编写并发代码，而不需要显式的锁、线程或多进程。

# 组合迭代器和生成器

在 Python 中，迭代器和生成器可以以各种方式组合，以实现程序中的特定功能。通过链式操作迭代器和生成器，复杂的数据处理管道变得易于理解和维护。

迭代器链式操作是一种将多个迭代器链在一起以创建一个新的迭代器，该迭代器遍历原始迭代器的元素的技术。这可以通过内置函数`itertools.chain()`完成。

```py
from itertools import chain

list1 = [1, 2, 3]
list2 = [4, 5, 6]
list3 = [7, 8, 9]
for i in chain(list1, list2, list3):
    print(i)
```

这将打印 1 到 9 之间的数字。

管道迭代器是一种将多个迭代器链在一起的技术，每个迭代器对数据执行特定操作，并将结果传递给下一个迭代器。这可以通过生成器表达式、列表推导式或像`map()`和`filter()`这样的函数工具完成。

```py
squared_numbers = (x**2 for x in range(10))
even_squared_numbers = (x for x in squared_numbers if x%2==0)
```

```py
for i in even_squared_numbers:
    print(i)
```

这将打印 0 到 9 之间数字的偶数平方。

高级迭代器技术也可以实现特定功能，例如实现装饰器以为迭代器添加额外功能，或使用`zip()`和`enumerate()`同时迭代多个序列。

总之，在 Python 中组合迭代器和生成器可以创建复杂的数据处理管道，这些管道易于理解和维护。通过将迭代器和生成器链在一起，你可以创建新的迭代器，这些迭代器遍历所有原始迭代器的元素。迭代器链式操作和管道迭代器是组合迭代器和生成器的两种最常见方法。高级迭代器技术如装饰器、`zip()`和`enumerate()`也可以用来实现特定功能。

# 用于深度学习的迭代器和生成器

在机器学习中使用迭代器和生成器的一个例子是处理无法全部加载到内存中的大型数据集。与其将整个数据集加载到内存中，不如使用自定义生成器一次加载一批数据。这节省了内存，并允许增量处理数据，这对在线学习算法非常有帮助。

这是一个使用生成器加载深度学习模型的大型数据集（包括图像和标签）的示例：

```py
import os
from PIL import Image

def image_generator(data_dir, batch_size):
    images = os.listdir(data_dir)
    for i in range(0, len(images), batch_size):
        batch = images[i:i+batch_size]
        X, y = [], []
        for img in batch:
            image = Image.open(os.path.join(data_dir, img))
            X.append(np.array(image))
            label = img.split(".")[0]
            y.append(int(label))
        yield np.array(X), np.array(y)

data_dir
```

在机器学习中，迭代器和生成器可以提高模型的性能和效率。以下是一些示例：

1.  **数据加载**：在处理大型数据集时，将所有数据加载到内存中可能会成为问题。通过使用生成器加载数据，你可以一次只加载一个小批次，并将其提供给模型。这可以通过创建一个生成器函数来完成，该函数从磁盘读取数据，处理它，并以小批次提供数据。

1.  **数据预处理**：在处理图像或视频数据时，可能需要对数据进行广泛的预处理，然后才能输入模型。通过使用生成器，你可以实时预处理数据，并将预处理后的数据提供给模型。

1.  **数据增强**：数据增强是一种通过对数据应用随机变换来增加数据集大小的技术。使用生成器，你可以实时应用数据增强，并将增强后的数据提供给模型。

1.  **模型训练**：在训练模型时，通常需要多次迭代数据。通过使用迭代器，你可以重复迭代。

这是一个使用生成器函数加载和预处理图像数据以用于机器学习模型的示例：

```py
import os
from keras.preprocessing.image import ImageDataGenerator

# create a generator function that loads and preprocesses images
def image_generator(directory, target_size, batch_size):
    data_gen = ImageDataGenerator(rescale=1./255) #rescale images
    generator = data_gen.flow_from_directory(directory,
                                             target_size=target_size,
                                             batch_size=batch_size,
                                             class_mode='categorical')
    for images, labels in generator:
        yield images, labels

# specify the directory containing the images
directory = './data/train'

# specify the target size and batch size
target_size = (200, 200)
batch_size = 32

#create an iterator
image_iter = image_generator(directory, target_size, batch_size)

# use the iterator to train the model
model.fit_generator(image_iter, steps_per_epoch=len(image_iter), epochs=10) 
```

本示例使用了 Keras 的 `ImageDataGenerator` 类来加载和预处理图像。`image_generator` 函数接收包含图像的目录、目标尺寸和批次大小作为参数，并返回一个生成预处理图片和标签的迭代器。然后使用 `model.fit_generator` 方法通过传递迭代器和每个 epoch 的步数来训练模型。

这样，生成器函数从磁盘加载图像，预处理它们，并一次提供一个批次。模型可以对其进行迭代，减少内存使用，并允许处理大数据集。

# 结论

在这篇博客文章中，我们已经深入探讨了 Python 的迭代器和生成器世界。我们探讨了迭代器和生成器的概念及其内部工作原理。我们还介绍了如何创建自定义迭代器和生成器，并查看了将它们结合起来以在程序中实现特定功能的高级技术。

我们已经看到，迭代器是可以被迭代的对象，它们实现了具有 `iter()` 和 `next()` 方法的迭代器协议。另一方面，生成器是一种允许你声明一个行为类似于迭代器的函数的迭代器类型。它们使用 yield 语句来返回数据，并且是处理大型数据集的更节省内存的方式。

创建自定义迭代器和生成器可以让你为特定的用例定义迭代器或生成器的行为。此外，将迭代器和生成器串联起来可以创建复杂的数据处理管道，这些管道易于理解和维护。

在这篇文章结束时，你应该对如何在 Python 中使用迭代器和生成器以编写高效而有效的代码有一个扎实的理解。我们鼓励你尝试本篇文章中讨论的概念，并继续学习 Python 的其他功能和能力。

在线可以找到官方文档、书籍、教程和视频等额外资源，以继续学习和掌握 Python 中的迭代器和生成器。

# 额外资源

Python 中的迭代器和生成器文档：

[## 9\. 类](https://docs.python.org/3/tutorial/classes.html?source=post_page-----ca30939d962--------------------------------#iterators)

### 类提供了一种将数据和功能捆绑在一起的方法。创建一个新类会创建一个新的对象类型…

[docs.python.org](https://docs.python.org/3/tutorial/classes.html?source=post_page-----ca30939d962--------------------------------#iterators)

"Python 迭代器：一步步指南" 由 Corey Schafer 提供：

"[**如何在 Python 中使用生成器和 yield**](https://realpython.com/introduction-to-python-generators/)" 由 Real Python 提供：

[## 如何在 Python 中使用生成器和 yield](https://realpython.com/introduction-to-python-generators/?source=post_page-----ca30939d962--------------------------------) - Real Python

### 生成器函数由 PEP 255 引入，是一种特殊类型的函数，返回一个惰性迭代器。这些是…

[realpython.com](https://realpython.com/introduction-to-python-generators/?source=post_page-----ca30939d962--------------------------------)

"Python 迭代器和生成器" 由 Trey Hunner 提供：

[## 如何在 Python 中创建一个迭代器](https://treyhunner.com/2018/06/how-to-make-an-iterator-in-python/?source=post_page-----ca30939d962--------------------------------)

### 我之前写过一篇关于驱动 Python `for` 循环的迭代器协议的文章。那篇文章中有一件事我没有提及…

[treyhunner.com](https://treyhunner.com/2018/06/how-to-make-an-iterator-in-python/?source=post_page-----ca30939d962--------------------------------)

"[**什么是 Python 生成器？**](https://dbader.org/blog/python-generators)" 由 Dan Bader 提供：

[## 什么是 Python 生成器？](https://dbader.org/blog/python-generators?source=post_page-----ca30939d962--------------------------------) - dbader.org

### 生成器是 Python 中一个棘手的主题。通过本教程，你将从基于类的迭代器跃迁到使用…

[dbader.org](https://dbader.org/blog/python-generators?source=post_page-----ca30939d962--------------------------------)

"[**Python 迭代器与生成器**](https://medium.com/geekculture/python-iterators-generators-ea63c5821550)**"** 作者：[Kurtis Pykes](https://medium.com/u/5ba760786877?source=post_page-----ca30939d962--------------------------------)

[](https://medium.com/geekculture/python-iterators-generators-ea63c5821550?source=post_page-----ca30939d962--------------------------------) [## Python 迭代器与生成器

### 理解关键差异

medium.com](https://medium.com/geekculture/python-iterators-generators-ea63c5821550?source=post_page-----ca30939d962--------------------------------)

"Python 中的异步迭代器" 作者：[Dinesh Kumar K B](https://medium.com/u/f3e3395e9bbf?source=post_page-----ca30939d962--------------------------------)

[](https://medium.com/geekculture/asynchronous-iterators-in-python-fdf55198287d?source=post_page-----ca30939d962--------------------------------) [## Python 中的异步迭代器

### 如何在 Python 中编写异步 for 循环

medium.com](https://medium.com/geekculture/asynchronous-iterators-in-python-fdf55198287d?source=post_page-----ca30939d962--------------------------------)

"[**迭代器函数**](https://itnext.io/iterator-functions-33265a99e5d1)**"** 作者：[Thijmen Dam](https://medium.com/u/63098707ef90?source=post_page-----ca30939d962--------------------------------)

[](https://itnext.io/iterator-functions-33265a99e5d1?source=post_page-----ca30939d962--------------------------------) [## 迭代器函数

### 使用 Python 内置函数高效迭代

itnext.io](https://itnext.io/iterator-functions-33265a99e5d1?source=post_page-----ca30939d962--------------------------------)

"**揭开 Python 中迭代器和生成器的神秘面纱****"** 作者：[Lynn Kwong](https://medium.com/u/f649eccbbc3d?source=post_page-----ca30939d962--------------------------------)

[](/demystify-iterators-and-generators-in-python-f21878c9897?source=post_page-----ca30939d962--------------------------------) ## 揭开 Python 中迭代器和生成器的神秘面纱

### 学习高效处理大型数据集的方法

towardsdatascience.com

# 联系我们

想要联系？关注罗宾逊博士的 [LinkedIn](https://www.linkedin.com/in/jrobby/)、[Twitter](https://twitter.com/jrobvision)、[Facebook](https://www.facebook.com/joe.robinson.39750) 和 [Instagram](https://www.instagram.com/doctor__jjj/)。访问我的主页获取论文、博客、邮箱注册等更多信息！

[](https://www.jrobs-vision.com/?source=post_page-----ca30939d962--------------------------------) [## AI 研究工程师及企业家 | 约瑟夫·P·罗宾逊

### 研究员与企业家问候！作为研究员，罗宾逊博士提出并使用了先进的 AI 来理解…

www.jrobs-vision.com.](https://www.jrobs-vision.com/?source=post_page-----ca30939d962--------------------------------)
