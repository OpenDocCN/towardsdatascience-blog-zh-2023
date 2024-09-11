# 《终极 Ndarray 手册：掌握 Rust 的科学计算艺术》

> 原文：[https://towardsdatascience.com/the-ultimate-ndarray-handbook-mastering-the-art-of-scientific-computing-with-rust-ef5ab767212a?source=collection_archive---------1-----------------------#2023-05-02](https://towardsdatascience.com/the-ultimate-ndarray-handbook-mastering-the-art-of-scientific-computing-with-rust-ef5ab767212a?source=collection_archive---------1-----------------------#2023-05-02)

## 对 Rust 的不同内置数据结构进行概述，并深入探讨 Ndarray 库

[](https://wiseai.medium.com/?source=post_page-----ef5ab767212a--------------------------------)[![Mahmoud Harmouch](../Images/d61617549d25565399975debaad5908f.png)](https://wiseai.medium.com/?source=post_page-----ef5ab767212a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ef5ab767212a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ef5ab767212a--------------------------------) [Mahmoud Harmouch](https://wiseai.medium.com/?source=post_page-----ef5ab767212a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb15db3da5667&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-ndarray-handbook-mastering-the-art-of-scientific-computing-with-rust-ef5ab767212a&user=Mahmoud+Harmouch&userId=b15db3da5667&source=post_page-b15db3da5667----ef5ab767212a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ef5ab767212a--------------------------------) ·31分钟阅读·2023年5月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fef5ab767212a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-ndarray-handbook-mastering-the-art-of-scientific-computing-with-rust-ef5ab767212a&user=Mahmoud+Harmouch&userId=b15db3da5667&source=-----ef5ab767212a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fef5ab767212a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-ndarray-handbook-mastering-the-art-of-scientific-computing-with-rust-ef5ab767212a&source=-----ef5ab767212a---------------------bookmark_footer-----------)![](../Images/6fd226fc675887e1ef5fcb21042dc628.png)

照片由 [Crissy Jarvis](https://unsplash.com/@crissyjarvis?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## TL;DR

Rust 在全球编程语言中获得了巨大的受欢迎程度，这并非没有理由。此外，当讨论数据分析时，Rust 凭借其卓越的能力在同类中脱颖而出。丰富的库支持加上强大的工具使 Rust 成为许多专业人士处理复杂数据集的首选。此外，如果你希望使用 Rust 进行数据分析或其他相关任务，了解如何存储数据至关重要。

到文章结尾时，你将拥有一个坚实的基础，使你能够自信且轻松地开启 Rust 数据分析之旅。

> **注意**：本文假设你对 Rust 的引用和借用检查器有一定了解。
> 
> 为了这篇文章开发了名为 [2-ndarray-tutorial.ipynb](https://github.com/wiseaidev/rust-data-analysis/blob/main/2-ndarray-tutorial.ipynb) 的笔记本，可以在以下仓库中找到：

[](https://github.com/wiseaidev/rust-data-analysis?source=post_page-----ef5ab767212a--------------------------------) [## GitHub - wiseaidev/rust-data-analysis: 终极数据分析课程与 Rust。

### 这个仓库是一个 Jupyter 笔记本的集合，所有笔记本都由 Rust 内核提供支持。通过这些笔记本，你将会…

github.com](https://github.com/wiseaidev/rust-data-analysis?source=post_page-----ef5ab767212a--------------------------------)

# 目录（TOC）

∘ [这篇文章讲了什么？](#d2ef)

∘ [Rust 内置数据结构](#3498)

∘ [1\. 向量](#5a24)

∘ [2\. 数组](#c03f)

∘ [3\. 元组](#d625)

∘ [4\. 哈希集合](#2bba)

∘ [5\. 哈希映射](#31e1)

∘ [Ndarray 用于数据分析](#672d)

∘ [Ndarray 简介](#373b)

∘ [Ndarray 用例](#3b5f)

∘ [初始占位符](#7d56)

∘ [多维数组](#b787)

∘ [Ndarray 数组操作](#b4f3)

∘ [索引与切片](#8047)

∘ [重塑](#1131)

∘ [转置](#edbc)

∘ [交换轴](#7c44)

∘ [线性代数](#bc63)

∘ [结论](#aa63)

∘ [结束语](#aa3d)

∘ [资源](#f6a5)

## 这篇文章讲了什么？

![](../Images/e06c1558e0ba0e05b00c9891493a0b7a.png)

[Ashni](https://unsplash.com/@ashni_ahlawat?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上的照片

本文的重点是一个用于数据分析的重要 Rust 库，即 `**ndarray**`。`**ndarray**` 使用户能够处理大型多维数组和矩阵，同时提供了广泛的数学操作选择。

但在我们深入探讨 `**ndarray**` 之前，让我们退一步，探索一下不同的 **Rust 内置数据结构**，以及为什么 Rust 是进行数据分析的绝佳语言。

## Rust 内置数据结构

在本节中，我们将深入探讨构成这个出色的 Rust 编程语言基础的基本概念和强大工具。特别是，我们将涵盖 Rust 数据结构的基础知识，包括向量、元组、集合和哈希映射，深入理解它们的工作原理以及如何用它们来解决实际问题。

## 1\. 向量

![](../Images/1d7913fc0d7655a9d216be4d419e6f47.png)

向量内存布局（作者提供的图片）

**向量**在一些编程语言如 Python 中被称为“**列表**”，它们无处不在；从简单的购物清单到更复杂的食谱说明，它们可以帮助我们跟踪事物并在需要时找到它们。在编程中，向量是一个重要的数据结构，广泛应用于许多不同的场景和形式。

## 创建向量

在 Rust 中，向量是基本的数据结构，你可以通过不同的方式创建它们。要创建一个空向量，你可以调用`[**Vec::new()**](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.new)`函数，并添加类型注释，因为 Rust 不知道你打算存储什么元素：

```py
let v: Vec<i32> = Vec::new();
```

另外，你可以使用`[**vec!**](https://doc.rust-lang.org/std/macro.vec.html#)`宏来创建一个带有初始值的新向量：

```py
let v = vec![1, 2, 3];
```

Rust 编译器能够**推断向量的类型**，通过初始值来消除手动指定的需要。创建向量后，你有多种选项来根据需求修改它。

## 访问向量元素

在 Rust 中，我们可以通过两种方式访问存储在向量中的值：要么通过索引，要么使用`[**get**](https://doc.rust-lang.org/alloc/vec/struct.Vec.html#method.get)`方法。让我们探索这两种方法，并附上代码示例！

首先，让我们考虑以下具有一些值的向量`**v**`：

```py
let v = vec!["apple", "banana", "cherry", "date"];
```

索引操作符`**[]**`可以用来从向量中检索特定的值。要访问第二个元素，我们来看以下示例：

```py
// Get the second element
let second = &v[1];
println!("The second element is {}", second);

// Output:
// The second element is banana
```

在这里，我们使用索引`**[]**`来创建指向向量中第二个元素的引用`**&**`。当尝试访问一个不存在的索引时，Rust 编译器会触发终止/崩溃，并导致程序失败。为了避免这种情况，我们可以利用 get 函数，该函数返回一个`**Option<&T>**`而不是引用。它是如何工作的：

```py
let v = vec![
    ("apple", 3),
    ("banana", 2),
    ("cherry", 5),
    ("date", 1),
];

// Get the quantity of cherries
let quantity = v.get(2).map(|(_, q)| q);

match quantity {
    Some(q) => println!("There are {} cherries", q),
    None => println!("Cherries not found"),
}

// Output:
// There are 5 cherries
```

通过调用`**v.get(2)**`，该方法将返回一个`[**Option<&T>**](https://doc.rust-lang.org/std/option/index.html)`类型，如果元素存在，将返回`**Some**`形式的正面结果，否则返回`**None**`形式的负面结果。我们可以通过实现**match 表达式**来有效处理这两种情况。利用这些技巧，你可以轻松访问向量中的元素！

## 遍历值

在 Rust 中，遍历向量是一个常见任务，可以通过两种方式执行：利用 **不可变** 和 **可变** 引用。这种方法使我们能够对每个向量元素逐一操作。为了进一步理解，让我们通过一些代码示例来探索这两种方法！

```py
let fruits = vec![("apple", 3), ("banana", 2), ("orange", 5), ("peach", 4)];
let mut sum = 0;
for (_, num) in &fruits {
    sum += num;
}
let avg = sum as f32 / fruits.len() as f32;
println!("The average of the second elements is {}", avg);

// Output:
// The average of the second elements is 3.5
```

在上述代码片段中，我们使用 `**&**` 运算符为向量中的每个项获取不可变引用。然后，我们通过利用 `[**println**](https://doc.rust-lang.org/std/macro.println.html)**!**` 宏来显示每个元素的值。

此外，`[**iter()**](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.iter)` 函数创建一个向量值的迭代器。使用这种技术，我们可以获得向量中每个值的可变引用，从而无缝地添加 10。以下代码演示了如何使用 `**iter()**` 方法有效地优化向量的迭代。

```py
let mut values = vec![10, 20, 30, 40, 50];
for value in values.iter_mut() {
    *value += 10;
}
println!("The modified vector is {:?}", values);

// Output:
// The modified vector is [20, 30, 40, 50, 60]
```

我们可以通过利用 **for 循环** 和 **范围** 有效地遍历向量元素的 **部分**。为了说明这个概念，考虑以下代码片段，展示了如何使用 for 循环获取 **不可变引用**，从给定的向量中仅获取三个元素，然后将其输出到终端。

```py
let values = vec![10, 20, 30, 40, 50];
for value in &values[0..3] {
    println!("The value is {}", value);
}

// Output
// The value is 10
// The value is 20
// The value is 30
```

通过利用 Rust 的 `[**enumerate()**](https://doc.rust-lang.org/core/iter/trait.Iterator.html#method.enumerate)` 函数，我们可以轻松遍历向量并获得其值及对应的索引。下面的代码片段展示了如何使用 `**enumerate()**` 方法获取 **不可变引用**，用于从 `**i32**` 值类型的向量中检索每个元素，同时打印它们各自的索引和值。

```py
let values = vec![10, 20, 30, 40, 50];
for (index, value) in values.iter().enumerate() {
    println!("The value at index {} is {}", index, value);
}

// Output:
// The value at index 0 is 10
// The value at index 1 is 20
// The value at index 2 is 30
// The value at index 3 is 40
// The value at index 4 is 50
```

使用这些技巧，你可以轻松地 **迭代** 和 **操作** 向量中的元素！

## 修改向量

Rust 的向量的多功能性在于其动态调整大小的能力，允许在运行时添加或删除元素。本节将探讨在 Rust 中修改和更新向量的不同方法。

## 添加元素

![](../Images/add9570e28879f1d0e4854190a2708ff.png)

向量中添加元素（图像来源：作者）

我们可以使用 `[**push**](https://doc.rust-lang.org/alloc/vec/struct.Vec.html#method.push)` 方法向向量中添加元素，该方法将一个元素附加到向量的末尾：

```py
let mut v = vec!["apple", "banana", "orange"];

v.push("mango");

println!("{:?}", v);

// Output:
// ["apple", "banana", "orange", "mango"]
```

给定的示例涉及创建一个包含三个元素的向量，然后通过 **push** 操作将 **“mango”** 附加到向量的末尾。最后，我们通过 `**println!**` 宏在终端上显示修改后的向量。或者，我们可以使用 insert 方法在特定索引处添加元素：

```py
let mut v = vec!["apple", "mango", "banana", "orange"];

v.insert(v.len(), "mango");

println!("{:?}", v);

// Output:
// ["apple", "mango", "banana", "orange", "mango"]
```

上述示例涉及创建一个四元素的向量，然后通过使用`[**insert**](https://doc.rust-lang.org/alloc/vec/struct.Vec.html#method.insert)`方法在向量末尾插入**“mango”**。最后，我们通过`**println!**`宏在终端上显示修改后的向量。

## 修改元素

要修改字符串向量的元素，我们可以利用索引操作符`**[]**`来访问特定位置的元素，并用新值替换它。这种方法在修改给定向量中的值时非常有效。

```py
let mut v = vec!["apple", "banana", "orange"];

v[1] = "pear";
v[2] = "grapefruit";

println!("{:?}", v);

// Output:
// ["apple", "pear", "grapefruit"]
```

给定的示例涉及创建一个包含三个元素的向量`**v**`，然后将其第二个元素（位于索引1）修改为`**“pear”**`，并将第三个元素（在索引2）的值指定为`**“grapefruit”**`。最后，我们通过`**println!**`宏在终端上显示这个更新后的版本。

## 删除元素

![](../Images/78b67871e69275da49c456afea969603.png)

从向量中删除元素（图像由作者提供）

我们可以使用`[**pop()**](https://doc.rust-lang.org/alloc/vec/struct.Vec.html#method.pop)`方法从向量中删除一个元素，该方法会删除并返回向量中的最后一个元素：

```py
let mut v = vec!["apple", "banana", "orange", "mango"];

let removed_element = v.pop();

println!("Removed element: {:?}", removed_element.unwrap());
println!("{:?}", v);

// Output:
// Removed element: "mango"
// ["apple", "banana", "orange"]
```

在上面的示例中，我们创建了一个四元素的向量`**v**`，然后使用`**pop**`方法删除了最后一个元素。该方法还向我们提供了被删除的组件作为输出。最后，我们使用`**println!**`宏以有序的方式在终端屏幕上显示更新后的向量和提取的元素。

我们还可以使用`[**remove**](https://doc.rust-lang.org/alloc/vec/struct.Vec.html#method.remove)`方法来删除特定索引处的元素：

```py
let mut v = vec!["apple", "banana", "orange", "mango"];

let removed_element = v.remove(2);

println!("Removed element: {}", removed_element);
println!("{:?}", v);

// Output
// Removed element: orange
// ["apple", "banana", "mango"]
```

要从Rust中的向量中删除所有元素，使用`[**retain**](https://doc.rust-lang.org/alloc/vec/struct.Vec.html#method.retain)`方法来保留所有符合某个条件的元素：

```py
let mut v = vec!["A", "warm", "fall", "warm", "day"];
let elem = "warm"; // element to remove
v.retain(|x| *x != elem);
println!("{:?}", v);

// Output:
// ["A", "fall", "day"]
```

## 连接两个向量

要连接两个字符串向量，我们可以使用`**extend**`方法，该方法将一个**迭代器**作为参数，并将其所有元素附加到向量中：

```py
let mut v1 = vec!["apple", "banana"];
let mut v2 = vec!["orange", "mango"];

v1.extend(v2);

println!("{:?}", v1);

// Output:
// ["apple", "banana", "orange", "mango"]
```

在上面的示例中，我们首先创建了两个向量`**v1**`和`**v2**`，然后通过在`**v1**`上调用extend方法并传递`**v2**`作为参数来连接它们。

## 过滤与映射元素

我们可以使用`[**iter**](https://doc.rust-lang.org/core/iter/index.html)`、`[**filter**](https://doc.rust-lang.org/core/iter/struct.Filter.html)`和`[**map**](https://doc.rust-lang.org/core/iter/struct.Map.html)`方法来过滤和映射Rust中的向量元素。

## **过滤元素**

我们可以通过结合使用`**iter**`和`**filter**`方法有效地过滤向量元素。为了说明这一点，让我们考虑如何使用以下示例从整数集合中过滤出所有偶数：

```py
let v = vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
let odd_numbers: Vec<i32> = v.iter().filter(|x| *x % 2 != 0).map(|x| *x).collect();
println!("{:?}", odd_numbers);

// Output:
// [1, 3, 5, 7, 9]
```

在上述示例中，我们首先创建了一个包含十个元素的向量`**v**`，然后使用`**iter**`和`**filter**`方法创建一个新的向量`**odd_numbers**`，其中仅包含`v`中的**奇数**。最后，我们使用`**println!**`宏将新向量打印到终端。

## 映射元素

要**映射**向量中的元素，我们可以将`**iter**`和`**map**`方法一起使用。例如，将字符串向量转换为大写：

```py
let v = vec!["hello", "world", "rust"];
let uppercase_strings: Vec<String> = v.iter().map(|x| x.to_uppercase()).collect();
println!("{:?}", uppercase_strings);

// Output
// ["HELLO", "WORLD", "RUST"]
```

在上述示例中，我们首先创建了一个包含**三个**元素的向量`**v**`，然后使用`**iter**`和`**map**`方法创建一个新的向量`**uppercase_strings**`，该向量包含了`v`中元素的大写版本。最后，我们使用`**println!**`宏将新向量打印到控制台。

## 长度

要计算长度，我们可以使用 `[**len**](https://doc.rust-lang.org/core/iter/trait.ExactSizeIterator.html#method.len)` 方法：

```py
let v = vec!["hello", "world", "rust"];
println!("Size: {}", v.len());

// Output
// Size: 3
```

## 检查元素是否存在

我们可以使用 `[**contains**](https://doc.rust-lang.org/core/primitive.str.html#method.contains)` 来检查一个向量是否包含特定元素：

```py
let v = vec!["hello", "world", "rust"];
println!("{}", v.contains(&"hello"));

// Output
// true
```

注意该方法需要一个**借用副本**，因此参数中有`**&**`。如果你忘记了，编译器会提示你添加这个符号。

## 反转元素

我们可以使用 `[**reverse**](https://doc.rust-lang.org/core/primitive.slice.html#method.reverse)` 方法在Rust中反转一个向量。这个方法会原地修改向量，所以它不返回任何东西。

```py
let mut v = vec![1, 2, 3, 4, 5];
v.reverse();
println!("{:?}", v);

// Output:
// [5, 4, 3, 2, 1]
```

在上述示例中，创建了一个包含五个元素的向量`**v**`，然后使用`**reverse**`方法原地改变这些元素的顺序。最后，我们在终端上显示反转后的向量以供观察。

## 最大值与最小值

通过利用Rust的`**iter**`函数以及`**max**`和`**min**`方法，可以轻松找到向量中的最高值和最低值。这种方法在简化这些操作方面非常有效。

```py
let v = vec![1, 2, 3, 4, 5];
let max_element = *v.iter().max().unwrap();
let min_element = *v.iter().min().unwrap();
println!("Max element: {}", max_element);
println!("Min element: {}", min_element);

// Output
// Max element: 5
// Min element: 1
```

在上述示例中，我们初始化了一个包含五个元素的向量`**v**`。随后，使用`**iter**`方法创建一个迭代器，通过`**max**`和`**min**`方法来确定最大值和最小值。最后，使用`**println!**`在控制台上显示这两个结果。

现在你已经对使用和操作向量有了坚实的基础，让我们来看看另一个内置集合：数组。

## 2\. 数组

![](../Images/c5f5bed40ef9169713b78f1dfdc56123.png)

Rust数组内存布局（图片来源：作者）

使用**数组**是一种存储相同数据类型不同值的可行选项。**与向量不同**，数组中的每个元素必须具有**一致的数据类型**。与其他编程语言中的数组相比，它们是**固定大小的集合**，其中元素的数据类型相同。当你需要在**栈**上分配内存或知道它们的大小在运行时会保持不变时，这些集合具有优势。

## 创建数组

要创建数组，您可以使用方括号 `**[]**` 并用逗号分隔的值：

```py
let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
```

您还可以明确指定数组中元素的数量和类型，如下所示：

```py
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

使用此语法，可以形成一个由 `**i32**` 值组成且长度为 5 的数组。为了将此数组中的所有元素设置为一个典型值，可以使用以下方法：

```py
let zeros = [0; 5];
```

这会创建一个长度为 5 的数组，所有元素都初始化为 0。

## 访问元素

您可以使用带有元素索引的方括号访问数组的单个元素：

```py
let numbers = [1, 2, 3, 4, 5];
println!("{}", numbers[2]);

// Output:
// 3
```

## 修改元素

由于数组具有固定的大小，您无法像向量那样推送或移除元素。但是，可以通过使用 `**mut**` 关键字使数组可变，从而修改其元素：

```py
let mut numbers = [1, 2, 3, 4, 5];
numbers[1] = 10;
println!("{:?}", numbers);

// Output:
// [1, 10, 3, 4, 5]
```

## 迭代

要从数组中检索每个单独的元素，我们必须遍历它们，而不是依赖索引逐个访问。下面演示了有效检索并打印 `**i32**` 类型数组中每个值的 for 循环实现。

```py
let seasons = ["Winter", "Spring", "Summer", "Fall"];
for season in seasons {
    println!("{season}");
}
// or
for index in 0..seasons.len() {
    println!("{}", seasons[index]);
}
// or
for season in seasons.iter() {
    println!("{}", season);
}
```

## 切片数组

您还可以使用切片创建包含原始数组子集的新数组：

```py
let numbers = [1, 2, 3, 4, 5];
let slice = &numbers[1..4];
println!("{:?}", slice);

// Output:
// [2, 3, 4]
```

这将创建一个包含原始数组元素的新数组，其索引为 1、2 和 3。

总结一下，Rust 数组是多功能的数据结构，可以服务多种用途。其固定大小的特性使其在特定场景下比向量更为有效。当数组大小预先确定且不需要运行时修改时，使用数组是存储数据的理想选择。

## 3\. 元组

元组是 Rust 中的一种复合类型，允许将多个具有不同类型的值组合成一个对象。就大小而言，元组是固定的，一旦声明就不能调整大小，与数组类似。

## 创建元组

在 Rust 中，创建元组是一项简单的任务。只需将值括在括号中，并用逗号分隔它们。元组中的每个位置都有其类型，这些类型可能彼此不同，不会对其类型的一致性施加任何约束。

```py
let person = ("Mahmoud", 22, true, 6.6);
```

在创建元组时，可以加入可选的类型注解。下面的示例中展示了这一点：

```py
let person: (&str, i32, bool, f64) = ("Mahmoud", 22, false, 6.6);
```

## 更新元组

使用 `**mut**` 关键字，您可以将元组转换为可变形式并修改其内容。这允许通过引用后跟其各自 **索引** 值的 **点符号** 修改元组中的特定元素：

```py
let mut person = ("Mahmoud", 22, true);
```

您可以通过使用点符号和相应的元素索引来修改其元素。

```py
person.1 = 21;
```

## 元组的解构

从元组中提取不同组件并将它们分配给单独变量的过程称为**解构**，这在以下示例中进行了演示。

```py
let (name, age, is_male) = ("Mahmoud", 22, true);
println!("Name: {}, Age: {}, Gender: {}", name, age, if is_male { "Male" } else { "Female" });

// Output
// Name: Mahmoud, Age: 22, Gender: Male
```

我们还可以在解构时忽略元组中的某些元素：

```py
let (_, _, _, height) = ("Mahmoud", 22, false, 6.6);
println!("Height: {}", height);

// Output
// Height: 6.6
```

此外，我们可以使用索引访问元组中的特定元素：

```py
let person = ("Mahmoud", 3, true, 6.0);
println!("Experience: {}", person.1);

// Output
// Experience: 3
```

总结一下，元组是在 Rust 中将**不同类型的值**组合成一个对象的强大方式。它们是**不可变**的并且**大小固定**，但可以被设为可变以修改其内容。你还可以**解构**元组以访问其元素。凭借这些特性，元组是处理 Rust 数据的多功能工具！

## 4\. 哈希集合

如果你对 Python 比较熟悉，集合可能已经是一个熟悉的概念。这些集合由不同的元素组成，不重视顺序。在 Rust 编程语言中，**哈希集合**和**B 树集合**代表这些唯一的组；然而，前者在实践中更为常用。

## 创建集合

在 Rust 中创建一个哈希集合非常简单，只需从标准库中导入并调用`**new**`方法或相关函数：

```py
use std::collections::HashSet;
let mut my_set: HashSet<i32> = HashSet::new();
```

你也可以从元素的向量中创建一个集合：

```py
let my_vector = vec![1, 2, 3, 4];
let my_set: HashSet<i32> = my_vector.into_iter().collect();
```

你甚至可以从数组初始化它：

```py
let a = HashSet::from([1, 2, 3]);
```

## 更新集合

## 添加元素

向哈希集合中添加元素很简单，只需使用 `[**insert**](https://doc.rust-lang.org/std/collections/hash_set/struct.HashSet.html#method.insert)` 方法：

```py
let mut my_set: HashSet<i32> = HashSet::new();
my_set.insert(1);
my_set.insert(2);
my_set.insert(3);
```

## 移除元素

从哈希集合中移除元素可以使用 `[**remove**](https://doc.rust-lang.org/std/collections/hash_set/struct.HashSet.html#method.remove)` 方法：

```py
let mut my_set = HashSet::from([1, 2, 3, 4]);
my_set.remove(&2); // removes 2 from the set
```

## 遍历集合

你可以使用 for 循环轻松地遍历哈希集合：

```py
let my_set = HashSet::from([1, 2, 3]);

for element in &my_set {
    println!("{}", element);
}

// Output(not ordered):
// 1
// 3
// 2
```

## 集合操作

![](../Images/9398199d6d0f7c1e9d1c90dc219e4a59.png)

不同的集合操作（图像由作者提供）

Rust 的哈希集合提供了一系列集合操作，包括 `[**difference**](https://doc.rust-lang.org/std/collections/hash_set/struct.Difference.html)`、`[**intersection**](https://doc.rust-lang.org/std/collections/hash_set/struct.Intersection.html)` 和 `[**union**](https://doc.rust-lang.org/std/collections/hash_set/struct.Union.html)` 函数。这些功能使我们能够在哈希集合上执行集合运算，使其成为存储唯一数据的宝贵资源。为说明这一点，我们来看以下示例：

```py
use std::collections::HashSet;

let set_a = HashSet::from([1, 2, 3]);
let set_b = HashSet::from([4, 2, 3, 4]);

// elements in set_a that are not in set_b
let difference_set = set_a.difference(&set_b);

// elements common to both set_a and set_b
let intersection = set_a.intersection(&set_b);

// elements in either set_a or set_b
let union_set = set_a.union(&set_b);

for element in difference_set {
    println!("{}", element);
}

// Output:
// 1

for element in intersection {
    println!("{}", element);
}

// Output:
// 3
// 2

for element in union_set {
    println!("{}", element); 
}

// Output:
// 3
// 2
// 1
// 4
```

本质上，哈希集合是每个 Rust 开发者都必须熟悉的不可或缺的资产。它们具有显著的效率，并提供了大量的**集合运算**操作。通过提供的示例，你现在应该能够将哈希集合整合到你的个人 Rust 项目中。

欲了解更多信息，请参阅 [官方文档](https://doc.rust-lang.org/std/collections/struct.HashSet.html)。

## 5\. 哈希映射

![](../Images/0f1181e99cd25818afcc8b70220e3d3b.png)

哈希映射（图像由作者提供）

哈希映射是一种包含**键值对**的集合类型，通过利用键而非索引来快速有效地访问数据。Rust 通过 `[**std::collections::HashMap**](https://doc.rust-lang.org/std/collections/struct.HashMap.html)` 模块声明哈希映射，它是一种无序结构，速度非常快。让我们看看如何在 Rust 中创建、更新、访问和遍历哈希映射。

## 创建哈希映射

你可以通过多种方式在Rust中初始化哈希映射，其中一种方法是使用哈希映射结构体的`[**new**](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.new)`方法。

```py
use std::collections::HashMap;

let mut employees_map = HashMap::new();

// Insert elements to the HashMap
employees_map.insert("Mahmoud", 1);
employees_map.insert("Ferris", 2);

// Print the HashMap
println!("{:?}", employees_map);

// Output:
// {"Mahmoud": 1, "Ferris": 2}
```

在给定的示例中，我们引入了一个新的哈希映射（Hash Map），命名为`**employees_map**`。随后，利用`[**insert**](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.insert)`函数，我们向这个哈希映射中添加元素。最后，通过应用`**println!**`宏并使用`**{:?}**`格式化，我们展示了我们创建的HashMap的调试模式表示。另一种初始化哈希映射的方法是使用`[**HashMap::from**](https://doc.rust-lang.org/std/collections/struct.HashMap.html#impl-From%3C%5B(K,+V);+N%5D%3E-for-HashMap%3CK,+V,+RandomState%3E)`方法。

```py
use std::collections::HashMap;

let employees_map: HashMap<i32, &str> = HashMap::from([
    (1, "Mahmoud"),
    (2, "Ferris"),
]);
```

## 更新哈希映射

## 添加元素

正如我们在之前的示例中所见，我们可以使用`[**insert**](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.insert)`方法向哈希映射中添加元素（**键值对**）。例如：

```py
use std::collections::HashMap;

let mut employees_map = HashMap::new();

// Insert elements to the HashMap
employees_map.insert("Mahmoud", 1);
employees_map.insert("Ferris", 2);

// Print the HashMap
println!("{:?}", employees_map);

// Output:
// {"Mahmoud": 1, "Ferris": 2}
```

## 移除元素

我们可以使用`[**remove**](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.remove)`方法从哈希映射中移除一个元素（**键值对**）。例如：

```py
use std::collections::HashMap;

let mut employees_map: HashMap<i32, String> = HashMap::new();

// insert elements to hashmap
employees_map.insert(1, String::from("Mahmoud"));

// remove elements from hashmap
employees_map.remove(&1);
```

## 更新元素

我们可以通过使用`[**insert**](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.insert)`方法来更新哈希映射的元素。例如：

```py
let mut employees_map: HashMap<i32, String> = HashMap::new();

// insert elements to hashmap
employees_map.insert(1, String::from("Mahmoud"));

// update the value of the element with key 1
employees_map.insert(1, String::from("Ferris"));
println!("{:?}", employees_map);

// Output:
// {1: "Ferris"}
```

## 访问值

类似于Python，我们可以使用`**get**`来从给定的哈希映射中访问一个值。例如：

```py
use std::collections::HashMap;

let employees_map: HashMap<i32, &str> = HashMap::from([
    (1, "Mahmoud"),
    (2, "Ferris"),
]);

let first_employee = employees_map.get(&1);
```

## 遍历哈希映射

```py
use std::collections::HashMap;

fn main() {
    let mut employees_map: HashMap<i32, String> = HashMap::new();

    employees_map.insert(1, String::from("Mahmoud"));
    employees_map.insert(2, String::from("Ferris"));

    // loop and print values of hashmap using values() method
    for employee in employees_map.values() {
        println!("{}", employee)
    }

    // print the length of hashmap using len() method
    println!("Length of employees_map = {}", employees_map.len());
}

// Output:
// Ferris
// Mahmoud
// Length of employees_map = 2
```

本质上，Rust的哈希映射是一种强大的数据结构，它通过**键值对**有效地管理和排列数据。它们提供**快速访问**数据的能力，并且经常用于如**计数出现次数**、**记忆化**和**缓存**等任务。得益于Rust的集成哈希映射实现以及其广泛的技术组合，使用哈希映射是一个简单而不复杂的过程。

欲了解更多信息，你可以参考官方文档的[**这一页面**](https://doc.rust-lang.org/std/collections/struct.HashMap.html)。

当我们结束这一部分时，让我们回顾一下我们进入Rust内置数据结构广阔世界的旅程。我们的探索涉及了一些基本组件，如向量、数组、元组和哈希映射——这些都是任何熟练程序员在构建强健程序过程中不可或缺的元素。

通过我们对创建和访问数据结构的掌握，以及轻松操控它们的能力，我们获得了有关其定义特性和细微差别的宝贵见解。凭借这些知识，你将能够编写出高效且能够有效实现预期结果的Rust代码。

在牢固掌握了 Rust 内置数据结构的基本概念后，我们现在将其与本文后半部分探讨的 Ndarray 结合起来。这个出色的库因其在 Rust 中进行数值计算的能力而闻名。它具有类似于向量的数组对象，但具有增强的能力，能够无缝地执行数学运算。

## Ndarray 在数据分析中的应用

![](../Images/62fc6ce98c7ec14be3334f950bc8fbc7.png)

不同数组的维度（图片作者提供）

在接下来的章节中，我们将深入探讨 `[**ndarray**](https://docs.rs/ndarray/latest/ndarray/)`：一个强大的 Rust 库，能够轻松进行数值计算和数据操作。凭借其多种处理数组和矩阵中数值数据的方法，它是任何数据分析工具包中的重要资产。在接下来的章节中，我们将从零开始讲解如何使用 `**ndarray**`，包括如何处理数组和矩阵结构，以及如何轻松地对其进行数学运算。我们还将探索高级概念，如索引和切片，这些概念灵活地促进了对大数据集的高效处理。

通过在这些章节中的示例和实践练习，你可以掌握如何有效地利用 `ndarray` 数组来完成你的独特分析任务！

## Ndarray 介绍

`[**ArrayBase**](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html)` 结构提供了一种重要的数据结构，恰如其名为 n 维数组，有效地存储和管理大量的数据。这包括整数或浮点值。使用 `**ndarray**` 数组相较于 Rust 原生的 **数组** 或 **元组** 结构有多种好处：它更高效且用户友好。

## Ndarray 使用案例

以下是 `**ndarray**` 在数据分析中的一些实际应用案例：

+   **数据清洗与预处理**：Ndarray 提供了强大的数据清洗与预处理功能，包括过滤缺失值、转换各种数据类型以及缩放数据集的能力。假设你有一组记录中存在缺口；ndarray 的 nan（非数值）可以有效地表示这些缺失的条目。利用如 `[**填充**](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html#method.fill)` 这样的函数，你可以轻松地管理这些不完整的信息，毫不费力。

+   **数据可视化**：Ndarray 数组是数据存储的可靠选择，有助于可视化。`**ndarray**` 数组的多功能性使得它们可以与 `[**Plotters**](https://plotters-rs.github.io/home/)` 库配合使用，以实现可视化目的。例如，通过使用 Ndarrays 生成一个包含随机数的数组，我们可以通过 **Plotters** 的绘图能力将其以直方图的形式绘制出来。

+   **描述性统计**：Ndarray 提供了一系列强大的方法用于对数组进行描述性统计，包括计算均值、中位数、众数、方差和标准差。这些函数在分析数据时非常宝贵，因为它们提供了关键指标的快速概览。例如，通过利用 ndarray 的 `[**mean**](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html#method.mean)` 函数，我们可以轻松计算数据集中的平均值。

+   **机器学习**：Ndarray 是机器学习中的一个关键组件，提供了对大数据集的快速而有效的操作。数值数据通常必须以数组形式表示以便与这些算法配合使用，使得`**ndarray**`成为一个理想的解决方案，因为它使用方便且高效。借助这个工具，我们可以轻松生成特征和标签数组，这对任何机器学习算法的成功至关重要。

+   **线性代数**：Ndarray 提供了许多强大的方法来执行线性代数操作，如矩阵求逆、乘法和分解。这些功能在分析表示为矩阵或向量的数据时非常方便。例如，`[**dot**](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html#method.dot)`函数可以让我们轻松对两个数组进行矩阵乘法。

## 初始占位符

**Ndarray** 提供了多种生成和初始化数组的函数，被称为**初始占位符**或数组创建函数。这些强大的工具使我们能够创建具有特定形状和数据类型的自定义数组，并填充预定或随机值。以下是`**ndarray**`库中一些常用的初始占位符函数示例：

1.  `[**ndarray::Array::<type, _>::zeros(shape.f())**](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html#method.zeros)`：此函数创建一个填充了0的数组。`**shape**` 参数指定数组的维度，`**type**` 参数指定数组元素的数据类型。`**f**` 函数将数组从行优先转换为列优先。

1.  `[**ndarray::Array<::type, _>::ones(shape.f())**](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html#method.ones)`：此函数创建一个填充了1的数组。`**type**` 和 `**f**` 的效果与 `**ndarray::Array::zeros**` 相同。

1.  `[**ndarray::Array::<type, _>::range(start, end, step)**](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html#method.range)`：此函数创建一个范围内值的数组。start 参数指定范围的起点，end 参数指定范围的终点（不包括）。step 参数指定值之间的步长。`**type**` 参数指定数组元素的数据类型。

1.  `[**ndarray::Array::<type, _>::linspace(start, end, n)**](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html#method.linspace)`: 这个函数创建一个数组，其中的值在`**start**`和`**end**`之间均匀分布。参数`**n**`指定数组中的值的数量，而`end`参数指定是否包含停止值。参数`**type**`指定数组元素的数据类型。

1.  `[**ndarray::Array::<type, _>::fill(value)**](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html#method.fill)`: 这个函数用指定的值填充数组。参数`**value**`指定要填充数组的值。

1.  `[**ndarray::Array::<type, _>::eye(shape.f())**](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html#method.eye)`: 这个函数创建一个方阵的单位矩阵，对角线上为1，其他位置为0。参数`**n**`指定行和列的数量。参数`**type**`和`**f**`函数与`**ndarray::Array::zeros**`的意义相同。

1.  `[**ndarray::Array<type, _>::random(shape.f(), distribution_function)**](https://crates.io/crates/ndarray-rand)`: 这个函数使用给定的分布函数创建一个具有随机值的数组。参数`**shape**`指定数组的维度。

这些初始的占位函数对于在`**ndarray**`中生成和初始化数组非常有价值。它们提供了一种无需繁琐操作的方法来创建不同形状和数据类型的集合，允许用户指定特定或随机的值。以下是一个展示`**ndarray**`中各种占位符的简单 Rust 程序示例。

```py
use ndarray::{Array, ShapeBuilder};
use ndarray_rand::RandomExt;
use ndarray_rand::rand_distr::Uniform;

// Zeros

let zeros = Array::<f64, _>::zeros((1, 4).f());
println!("{:?}", zeros);

// Output:
// [[0.0, 0.0, 0.0, 0.0]], shape=[1, 4], strides=[1, 1], layout=CFcf (0xf), const ndim=2

// Ones

let ones = Array::<f64, _>::ones((1, 4));
println!("{:?}", ones); 

// Output:
// [[1.0, 1.0, 1.0, 1.0]], shape=[1, 4], strides=[4, 1], layout=CFcf (0xf), const ndim=2

// Range

let range = Array::<f64, _>::range(0., 5., 1.);
println!("{:?}", range); 

// Output:
// [0.0, 1.0, 2.0, 3.0, 4.0], shape=[5], strides=[1], layout=CFcf (0xf), const ndim=1

// Linspace

let linspace = Array::<f64, _>::linspace(0., 5., 5);
println!("{:?}", linspace); 

// Output:
// [0.0, 1.25, 2.5, 3.75, 5.0], shape=[5], strides=[1], layout=CFcf (0xf), const ndim=1

// Fill

let mut ones = Array::<f64, _>::ones((1, 4));
ones.fill(0.);
println!("{:?}", ones); 

// Output:
// [[0.0, 0.0, 0.0, 0.0]], shape=[1, 4], strides=[4, 1], layout=CFcf (0xf), const ndim=2

// Eye

let eye = Array::<f64, _>::eye(4);
println!("{:?}", eye); 

// Output:
// [[1.0, 0.0, 0.0, 0.0],
// [0.0, 1.0, 0.0, 0.0],
// [0.0, 0.0, 1.0, 0.0],
// [0.0, 0.0, 0.0, 1.0]], shape=[4, 4], strides=[4, 1], layout=Cc (0x5), const ndim=2

// Random

let random = Array::random((2, 5), Uniform::new(0., 10.));
println!("{:?}", random);

// Output:
// [[9.375493735188611, 4.088737328406999, 9.778579742815943, 0.5225866490310649, 1.518053969762827],
//  [9.860829919571666, 2.9473768443117, 7.768332993584486, 7.163926861520167, 9.814750664983297]], shape=[2, 5], strides=[5, 1], layout=Cc (0x5), const ndim=2
```

## 多维数组

Ndarray 可以构建具有多个维度的数组，例如 2D 矩阵和 3D 矩阵。我们可以轻松使用`[**from_vec**](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html#method.from_vec)`函数与向量的向量，或者使用`[**array!**](https://docs.rs/ndarray/latest/ndarray/macro.array.html)`宏来生成复杂的数据结构。例如，让我们看一个展示如何使用 ndarray 在各种维度上创建数组的示例程序。

```py
use ndarray::{array, Array, Array2, Array3, ShapeBuilder};

// 1D array
let array_d1 = Array::from_vec(vec![1., 2., 3., 4.]);
println!("{:?}", array_d1);

// Output:
// [1.0, 2.0, 3.0, 4.0], shape=[4], strides=[1], layout=CFcf (0xf), const ndim=1

// or 

let array_d11 = Array::from_shape_vec((1, 4), vec![1., 2., 3., 4.]);
println!("{:?}", array_d11.unwrap());

// Output:
// [[1.0, 2.0, 3.0, 4.0]], shape=[1, 4], strides=[4, 1], layout=CFcf (0xf), const ndim=2

// 2D array

let array_d2 = array![
    [-1.01,  0.86, -4.60,  3.31, -4.81],
    [ 3.98,  0.53, -7.04,  5.29,  3.55],
    [ 3.30,  8.26, -3.89,  8.20, -1.51],
    [ 4.43,  4.96, -7.66, -7.33,  6.18],
    [ 7.31, -6.43, -6.16,  2.47,  5.58],
];

// or

let array_d2 = Array::from_shape_vec((2, 2), vec![1., 2., 3., 4.]);
println!("{:?}", array_d2.unwrap());

// Output:
// [[1.0, 2.0],
// [3.0, 4.0]], shape=[2, 2], strides=[2, 1], layout=Cc (0x5), const ndim=2

// or

let mut data = vec![1., 2., 3., 4.];
let array_d21 = Array2::from_shape_vec((2, 2), data);

// 3D array

let mut data = vec![1., 2., 3., 4.];
let array_d3 = Array3::from_shape_vec((2, 2, 1), data);
println!("{:?}", array_d3);

// Output:
// [[[1.0],
//  [2.0]],
//  [[3.0],
//  [4.0]]], shape=[2, 2, 1], strides=[2, 1, 1], layout=Cc (0x5), const ndim=3
```

## Ndarray 数组操作

在本节中，我们将深入探讨**ndarray**数组的多种技术，例如**索引**、**切片**和**重塑**。

## [索引](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html#indexing-and-dimension) & [切片](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html#slicing)

![](../Images/a4a0c6b3556995118bfc2df99560956b.png)

数组切片（图片由作者提供）

Ndarray 通过 **索引** 和 **切片** 功能提供了令人印象深刻的能力，使我们能够访问和修改数组中的单个元素或子数组。与 Python 列表类似，在 **ndarray** 中，索引涉及使用索引值从数组中检索特定元素。作为这种功能的演示，请考虑使用以下代码访问数组的第二个元素：

```py
let array_d1 = Array::from_vec(vec![1., 2., 3., 4.]);
array_d1[1]
```

多维数组也支持索引和切片，而不仅仅是 1D 数组。为了说明这一点，请考虑以下代码，它通过指定行和列坐标从 2D 数组中检索一个元素：

```py
let zeros = Array2::<f64>::zeros((2, 4).f());
array_d1[1, 1]
```

切片是一种强大的技术，可以从数组中提取子数组。切片的语法类似于索引，但它使用点 `**..**` 来指定切片的开始和结束点。为了展示这种方法，请考虑以下代码，它生成一个只包含前三个元素的新数组：

```py
let array_d1 = Array::<i32, _>::from_vec(vec![1, 2, 3, 4]);
let slice = array_d1.slice(s![0..3]);
```

## 重塑

重塑是一种改变数组配置或排列的技术，同时保留其数据。ndarray 库提供了一系列强大的函数来重塑数组，如 `[**flatten**](https://docs.rs/ndarray-linalg/latest/ndarray_linalg/convert/fn.flatten.html)` 和最显著的 `[**reshape**](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html#method.reshape)`。

## 重塑

![](../Images/aee487bbadac24685a13ee9faaed8344.png)

数组重塑（图像由作者提供）

使用 `[**reshape**](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html#method.reshape)` 函数，这个函数只能应用于 `[**ArcArray**](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html#arcarray)`，你可以通过定义新配置的行数和列数来修改数组的形状。例如，以下代码片段将一个具有四个元素的 1D 数组转换为一个包含两行两列的 2D 数组：

```py
use ndarray::{rcarr1};
let array_d1 = rcarr1(&[1., 2., 3., 4.]); // another way to create a 1D array
let array_d2 = array_d1.reshape((2, 2));
```

## **展平**

![](../Images/c2722fbb99175769336a7d26d5090003.png)

数组展平（图像由作者提供）

`[**ndarray_linalg::convert::flatten**](https://docs.rs/ndarray-linalg/latest/ndarray_linalg/convert/fn.flatten.html)` 函数生成一个包含源数组所有元素的 1D 数组。然而，它会生成一个新的数据副本，而不是改变原始集合。这种方法确保了两个数组之间的区别，避免了由于数组重叠而引发的潜在混淆或错误。

```py
use ndarray::{array, Array2};
use ndarray_linalg::convert::flatten;

let array_d2: Array2<f64> = array![[3., 2.], [2., -2.]];
let array_flatten = flatten(array_d2);
print!("{:?}", array_flatten);

// Output:
// [3.0, 2.0, 2.0, -2.0], shape=[4], strides=[1], layout=CFcf (0xf), const ndim=1
```

`**ndarray**` 不仅提供了重塑数组的功能，还提供了一系列其他数组操作函数。这些包括转置和交换轴等。

## 转置

![](../Images/01ba4d4b77e0dea0ff774279f8515b96.png)

数组转置（图像由作者提供）

通过使用 `[**t**](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html#method.t)` 函数，可以生成一个其轴已转置的新数组。为了说明这一点，让我们考虑以下代码片段，该片段演示了如何转置一个二维数组：

```py
let array_d2 = Array::from_shape_vec((2, 2), vec![1., 2., 3., 4.]);
println!("{:?}", array_d2.unwrap());

// Output
// [[1.0, 2.0],
//  [3.0, 4.0]], shape=[2, 2], strides=[2, 1], layout=Cc (0x5), const ndim=2)

let binding = array_d2.expect("Expect 2d matrix");

let array_d2t = binding.t();
println!("{:?}", array_d2t);

// Output
// [[1.0, 3.0],
//  [2.0, 4.0]], shape=[2, 2], strides=[1, 2], layout=Ff (0xa), const ndim=2
```

## 交换轴

在 `**ndarray**` 中交换轴涉及交换数组中的行和列。这可以通过使用先前讨论的 `**t**` 方法或使用 ndarray 的 `[**swap_axes**](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html#method.swap_axes)` 方法来完成。交换轴是进行多维数组数据分析时的一个关键方面。

需要注意的是，轴是指多维数组中的每一个维度；例如，1D 数组只有一个轴，而 2D 数组具有两个轴——即行和列。类似地，3D 数组具有三个不同的轴：高度、宽度和深度——从零开始，直到添加额外的轴。

要使用 Rust 的 `**ndarray**` 库通过其内置方法如 `**swap_axes**` 执行这样的交换，你只需提供两个参数，表示要根据它们在不同维度平面上的位置进行交换的具体对。

```py
let array_d2 = Array::from_shape_vec((2, 2), vec![1., 2., 3., 4.]);
println!("{:?}", array_d2.unwrap());

// Output:
// [[1.0, 2.0],
//  [3.0, 4.0]], shape=[2, 2], strides=[2, 1], layout=Cc (0x5), const ndim=2

let mut binding = array_d2.expect("Expect 2d matrix");
binding.swap_axes(0, 1);
println!("{:?}", binding);

// Output:
// [[1.0, 3.0],
//  [2.0, 4.0]], shape=[2, 2], strides=[1, 2], layout=Ff (0xa), const ndim=2
```

## 线性代数

Ndarray 是一个功能丰富的 Rust 库，用于数值计算和数据处理，通过一个名为 `[**ndarray-linalg**](https://crates.io/crates/ndarray-linalg)` 的独立 crate 提供卓越的线性代数支持。本节将深入探讨 `**ndarray**` 在进行线性代数方面提供的多种函数，并展示如何有效地利用这些函数来轻松促进数据分析任务。

+   **矩阵乘法**：矩阵乘法的过程可以通过 `[**ArrayBase.dot**](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html#method.dot)` 函数执行，该函数有效地计算两个矩阵之间的点积。为了进一步说明这个概念，我们将利用它来确定将矩阵 `**a**` 和 `**b**` 相乘的结果，并将该结果存储在一个名为 `**c**` 的新矩阵中。

```py
extern crate blas_src;
use ndarray::{array, Array2};

let a: Array2<f64> = array![[3., 2.], [2., -2.]];
let b: Array2<f64> = array![[3., 2.], [2., -2.]];
let c = a.dot(&b);
print!("{:?}", c);

// Output
// [[13.0, 2.0],
//  [2.0, 8.0]], shape=[2, 2], strides=[2, 1], layout=Cc (0x5), const ndim=2
```

+   **逆运算**：在处理矩阵时，另一个重要操作是通过 `[**ndarray_linalg::solve::Inverse.inv**](https://docs.rs/ndarray-linalg/latest/ndarray_linalg/solve/trait.Inverse.html#tymethod.inv)` 函数来实现的，该函数计算任何给定矩阵的逆！例如，假设你想要对矩阵 `**array_d2**` 进行逆运算，调用其 `**inv**` 方法，并使用 `**match**` 语句来处理结果。

```py
use ndarray::Array;
use ndarray_linalg::solve::Inverse;
use std::result::Result::{Err, Ok};

let array_d2 = Array::from_shape_vec((2, 2), vec![1., 2., 2., 1.]);

match array_d2.expect("Matrix must be square & symetric!").inv() {
    Ok(inv) => {
        println!("The inverse of m1 is: {}", inv);
    }
    Err(err) => {
        println!("{err}");
    }
}

// Output:
// The inverse of m1 is: [[-0.3333333333333333, 0.6666666666666666],
//  [0.6666666666666666, -0.3333333333333333]]
```

+   **特征分解**：`[**use ndarray_linalg::Eig**](https://docs.rs/ndarray-linalg/latest/ndarray_linalg/eig/trait.Eig.html#tymethod.eig)`函数通过计算矩阵的特征值和特征向量来展示这一点。在我们的例子中，我们确定了矩阵`**array_d2**`的这些值，并分别将它们保存在矩阵`**eigs**`和`**vecs**`中。

```py
use ndarray::array;
use ndarray_linalg::Eig;
use std::result::Result::{Err, Ok};

let array_d2 = array![
    [-1.01,  0.86, -4.60],
    [ 3.98,  0.53, -7.04],
    [ 3.98,  0.53, -7.04],
];
match array_d2.eig() {
    Ok((eigs, vecs)) => {
        println!("Eigen values: {}", eigs);
        println!("Eigen vectors: {}", vecs);
    }
    Err(err) => {
        println!("{err}");
    }
}

// Output:
// Eigen values: [-3.759999999999999+2.706048780048134i, -3.759999999999999-2.706048780048134i, 0.00000000000000022759891370571733+0i]
// Eigen vectors: [[0.402993672209733+0.3965529218364603i, 0.402993672209733-0.3965529218364603i, 0.13921180485702092+0i],
//  [0.5832417510526318+0.00000000000000006939572631647882i, 0.5832417510526318-0.00000000000000006939572631647882i, 0.9784706726517249+0i],
//  [0.583241751052632+-0i, 0.583241751052632+0i, 0.15236540338584623+0i]]
```

+   **奇异值分解（SVD）**：`[**ndarray_linalg::svd::SVD**](https://docs.rs/ndarray-linalg/latest/ndarray_linalg/svd/trait.SVD.html#tymethod.svd)`函数的强大功能体现在它计算给定矩阵的左奇异向量、右奇异向量以及奇异值。为了说明这一点，我们对矩阵`**array_d2**`执行**SVD**，得到`**u**`保存左奇异向量，`**v**`存储奇异值，同时包含右奇异向量。

```py
use ndarray::array;
use ndarray_linalg::svd::SVD;
use std::result::Result::{Err, Ok};

let array_d2 = array![
    [-1.01,  0.86, -4.60],
    [ 3.98,  0.53, -7.04],
    [ 3.98,  0.53, -7.04],
];
match array_d2.svd(true, true) {
    Ok((u, sigma, v)) => {
        println!("The left singular vectors are: {:?}", u.unwrap());
        println!("The right singular vectors are: {:?}", v.unwrap());
        println!("The sigma vector: {:?}", sigma);
    }
    Err(err) => {
        println!("{err}");
    }
}

// Output:
// The left singular vectors are: [[-0.3167331446091065, -0.948514688924756, 0.0],
//  [-0.6707011685937435, 0.22396415437963857, -0.7071067811865476],
//  [-0.6707011685937436, 0.2239641543796386, 0.7071067811865475]], shape=[3, 3], strides=[3, 1], layout=Cc (0x5), const ndim=2
// The right singular vectors are: [[-0.4168301381758514, -0.0816682352525302, 0.9053081990455173],
//  [0.8982609360852509, -0.18954008048752713, 0.39648688325344433],
//  [0.13921180485702067, 0.9784706726517249, 0.1523654033858462]], shape=[3, 3], strides=[3, 1], layout=Cc (0x5), const ndim=2
// The sigma vector: [12.040590078046721, 3.051178554664221, 9.490164740574465e-18], shape=[3], strides=[1], layout=CFcf (0xf), const ndim=1
```

+   **矩阵迹**：`[**ndarray_linalg::trace::Trace**](https://docs.rs/ndarray-linalg/latest/ndarray_linalg/trace/trait.Trace.html#tymethod.trace)`函数是一个强大的函数，用于计算任何矩阵对角线元素的总和。通过将此方法应用于矩阵`**array_d2**`，我们得到其迹结果，并匹配其值以进行进一步分析。这种简单而有效的技术展示了数学函数如何轻松而精确地提升数据处理能力。

```py
use ndarray::array;
use ndarray_linalg::trace::Trace;
use std::result::Result::{Err, Ok};

let array_d2 = array![
    [-1.01,  0.86, -4.60],
    [ 3.98,  0.53, -7.04],
    [ 3.98,  0.53, -7.04],
];
match array_d2.trace() {
    Ok(value) => {
        println!("The sum of diagonal elements is: {:?}", value);
    }
    Err(err) => {
        println!("{err}");
    }
}

// Output:
// The sum of diagonal elements is: -7.52
```

+   **矩阵行列式**：通过使用[**ndarray_linalg::solve::Determinant**](https://docs.rs/ndarray-linalg/latest/ndarray_linalg/solve/trait.Determinant.html#method.det)函数来计算矩阵的行列式。我们关注的是计算矩阵`**array_d2**`的行列式值。

```py
use ndarray::array;
use ndarray_linalg::solve::Determinant;
use std::result::Result::{Err, Ok};

let array_d2 = array![
    [-1.01,  0.86, -4.60],
    [ 3.98,  0.53, -7.04],
    [ 3.98,  0.53, -7.04],
];
match array_d2.det() {
    Ok(value) => {
        println!("The determinant of this matrix is: {:?}", value);
    }
    Err(err) => {
        println!("{err}");
    }
}

// Output:
// The determinant of this matrix is: 2.822009292913204e-15
```

+   **求解线性方程**：`[**ndarray_linalg::solve**](https://docs.rs/ndarray-linalg/latest/ndarray_linalg/solve/index.html)`函数用于展示如何求解`**ax = b**`格式的线性方程组。在这个例子中，我们通过使用`**a**`作为常量数组来解决方程组`**ax=b**`，然后将结果存储在变量`**x**`中。

```py
use ndarray::{array, Array1, Array2};
use ndarray_linalg::Solve;

// a11x0 + a12x1 = b1    --->    3 * x0 + 2 * x1 = 1
// a21x0 + a22x1 = b2    --->    2 * x0 - 2 * x1 = -2:
let a: Array2<f64> = array![[3., 2.], [2., -2.]];
let b: Array1<f64> = array![1., -2.];
let x = a.solve_into(b).unwrap();
print!("{:?}", x);

// Output:
// [-0.2, 0.8], shape=[2], strides=[1], layout=CFcf (0xf), const ndim=1
```

在文章的这一部分，我们深入探讨了如何使用`**ndarray**`中的多维数组。这些数组是各个科学计算领域中一个至关重要的组件。`**array!**`宏函数在`**ndarray**`中使得多维数组的创建和操作变得轻而易举，这使其成为数据管理中的一项宝贵工具。

此外，我们还了解了如何利用`**ndarray**`数组进行算术运算。这些类型的数组能够支持基本的算术功能，如加、减、乘、除。可以对单个元素或整个数组同时进行这些计算。

最后，我们深入探讨了 `**ndarray**` 及其在线性代数中的应用。这种动态工具提供了广泛的函数，能够无缝进行矩阵操作，包括点积、转置、逆矩阵以及行列式。这些基本数学工具对于解决在金融、工程和物理等各个领域遇到的复杂问题至关重要。

## 结论

在本文中，我们深入探讨了 Rust 中的基本数据结构，并演示了如何使用**ndarray**库执行各种算术操作。此外，还强调了 Rust 在线性代数方面的潜力，这是数据科学的一个关键组件。

这个长期连载表明，Rust 是一种具有显著优势和广泛能力的语言，能够无缝地构建数据科学项目。它在处理复杂数据集时表现出色，同时相对简单。那些希望在数据科学领域取得有前途职业的人应该毫无疑问地将 Rust 作为首选之一。

## 结尾说明

![](../Images/bef1895c3809bb773cf033fd615b0884.png)

照片由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) 提供，刊登于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

一如既往，我想花一点时间向所有投入时间和精力阅读本文并跟进的人表示衷心的感谢。与大家一起展示 Rust 及其生态系统的能力是非常愉快的体验。

对数据科学充满热情的我承诺，每周都会写至少一篇全面的相关文章。如果你对跟进我的工作感兴趣，可以考虑在各种社交媒体平台上与我联系，或直接联系我，如果有其他需要帮助的地方。

谢谢！

## 资源

[](https://github.com/wiseaidev/rust-data-analysis?source=post_page-----ef5ab767212a--------------------------------) [## GitHub - wiseaidev/rust-data-analysis: 最终的数据分析课程，使用 Rust。

### 这个库包含了一系列 Jupyter notebook，所有这些 notebook 都由 Rust 内核驱动。通过这些 notebook，你将会……

github.com](https://github.com/wiseaidev/rust-data-analysis?source=post_page-----ef5ab767212a--------------------------------)  [## ndarray - Rust

### 展开描述 ndarray crate 提供了一个用于一般元素和数值的 n 维容器。在……

docs.rs](https://docs.rs/ndarray/latest/ndarray/?source=post_page-----ef5ab767212a--------------------------------) [](https://doc.rust-lang.org/std/index.html?source=post_page-----ef5ab767212a--------------------------------) [## std - Rust

### Rust 标准库是可移植 Rust 软件的基础，一套经过最小化和实战检验的共享库……

doc.rust-lang.org](https://doc.rust-lang.org/std/index.html?source=post_page-----ef5ab767212a--------------------------------)  [## Rust By Example

### Rust 是一种现代系统编程语言，注重安全性、速度和并发性。它实现这些目标的方式是…

[Rust By Example](https://doc.rust-lang.org/rust-by-example/?source=post_page-----ef5ab767212a--------------------------------) [## Rust 编程语言

### 作者：Steve Klabnik 和 Carol Nichols，感谢 Rust 社区的贡献。本版本的文本假设你已…

[Rust 编程语言](https://doc.rust-lang.org/book/?source=post_page-----ef5ab767212a--------------------------------)
