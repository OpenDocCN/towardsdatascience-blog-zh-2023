# 列表、元组、字典和数据框在 Python 中的完全指南

> 原文：[`towardsdatascience.com/lists-tuples-dictionaries-and-data-frames-in-python-the-complete-guide-7ab54d4819ee`](https://towardsdatascience.com/lists-tuples-dictionaries-and-data-frames-in-python-the-complete-guide-7ab54d4819ee)

## 掌握 Python 中最常用的数据结构所需的所有知识

[](https://federicotrotta.medium.com/?source=post_page-----7ab54d4819ee--------------------------------)![Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----7ab54d4819ee--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7ab54d4819ee--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7ab54d4819ee--------------------------------) [Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----7ab54d4819ee--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7ab54d4819ee--------------------------------) ·阅读时间 16 分钟·2023 年 5 月 24 日

--

![](img/30c2615c37ccc11a8c930496d8e8e34a.png)

图片由 [Pexels](https://pixabay.com/it/users/pexels-2286921/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1284475) 提供，发布在 [Pixabay](https://pixabay.com/it//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1284475)

如果你已经开始学习 Python，无论你想成为一名软件工程师还是数据科学家，你都必须掌握数据结构。

Python 具有许多数据结构，可以用来存储数据。在本文中，我们将深入了解最常用的数据结构。所以，如果你刚开始你的职业生涯，并需要学习数据结构，那么这篇文章绝对适合你。

这里是你会找到的内容：

```py
**Table of Contents:** 
Lists
  Definitions and creation examples
  Lists manipulation
  List comprehension
  List of lists
Tuples
Dictionaries
  Dictionaries manipulation
  Nested dictionaries
  Dictionary comprehension
Data frames
   Basic data frames manipulations with Pandas
```

# 列表

## 定义和创建示例

在 Python 中，列表是一个有序元素的集合，这些元素可以是任何类型：字符串、整数、浮点数等……

要创建一个列表，项必须放在方括号中，并用逗号分隔。例如，这里是如何创建一个整数列表的：

```py
# Create list of integers
my_integers = [1, 2, 3, 4, 5, 6]
```

但列表也可以包含“混合”类型的数据。例如，创建一个同时包含整数和字符串的列表：

```py
# Create a mixed list
mixed_list = [1, 3, "dad", 101, "apple"]
```

要创建一个列表，我们还可以使用 Python 内置函数 `list()`。这就是我们如何使用它的：

```py
# Create list and print it
my_list = list((1, 2, 3, 4, 5))
print(my_list)

>>>  

    [1, 2, 3, 4, 5]
```

这个内置函数在某些特殊情况下非常有用。例如，假设我们想创建一个范围在 (1–10) 之间的数字列表。以下是我们可以这样做的方式：

```py
# Create a list in a range
my_list = list(range(1, 10))
print(my_list)

>>>

  [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

```py
**NOTE**:

Remember that the built-in function "range" includes the first value,
and excludes the last one.
```

现在，让我们看看如何操作列表。

## 列表操作

由于列表是可变的，我们有很多可能性来操作它们。例如，假设我们有一个名字列表，但我们犯了一个错误，需要更改其中一个。以下是我们可以这样做的方式：

```py
# List of names
names = ["James", "Richard", "Simon", "Elizabeth", "Tricia"]
# Change the wrong name
names[0] = "Alexander"
# Print list
print(names)

>>>

    ['Alexander', 'Richard', 'Simon', 'Elizabeth', 'Tricia']
```

所以，在上述示例中，我们将列表中的第一个名字从 James 更改为 Alexander。

```py
**NOTE:** 
In case you didn't know, note that in Python the first element
is always accessed by "0", regarding of the type we're manipulating.
So, in the above example, "names[0]" represents the first element
of the list "names".
```

现在，假设我们忘记了一个名字。我们可以这样将其添加到我们的列表中：

```py
# List of names
names = ["James", "Richard", "Simon", "Elizabeth", "Tricia"]
# Append another name
names.append("Alexander")
# Print list
print(names) 

>>>

    ['James', 'Richard', 'Simon', 'Elizabeth', 'Tricia', 'Alexander']
```

如果我们需要连接两个列表，我们有两种选择：`concatenate`方法或`extend()`方法。让我们来看一下：

```py
# Create list1
list1 = [1, 2, 3]
# Create list2
list2 = [4, 5, 6]
# Concatenate lists
concatenated_list = list1 + list2
# Print concatenated list
print(concatenated_list)

>>>

  [1, 2, 3, 4, 5, 6]
```

所以，这种方法创建了一个包含其他列表之和的列表。我们来看一下`extend()`方法：

```py
# Create list1
list1 = [1, 2, 3]
# Create list2
list2 = [4, 5, 6]
# Extend list1 with list2
list1.extend(list2)
# Print new list1
print(list1)

>>>

  [1, 2, 3, 4, 5, 6]
```

正如我们所看到的，结果是相同的，但语法不同。此方法用`list2`扩展了`list1`。

如果我们想要删除元素，我们有两种选择：可以使用`remove()`方法或`del`。让我们来看看：

```py
# Create list
my_list = [1, 2, 3, 'four', 5.0]
# Remove one element and print
my_list.remove('four')
print(my_list)

>>>

  [1, 2, 3, 5.0]
```

我们来看看另一种方法：

```py
# Create list
my_list = [1, 2, 3, 'four', 5.0]
# Delete one element and print
del my_list[3]
print(my_list)

>>>

    [1, 2, 3, 5.0]
```

因此，我们可以使用这两种方法获得相同的结果，但`remove()`方法允许我们明确指定要删除的元素，而`del`则需要访问列表中元素的位置。

```py
**NOTE:** 
If you've gained familiarity with accessing positions, in the above
example my_list[3] = 'four'. Because, remember: in Python we start counting
positions from 0.
```

## 列表推导式

有很多情况下我们需要从现有列表中创建新列表，一般是对现有数据应用一些过滤条件。为此，我们有两种选择：

1.  我们使用循环和语句。

1.  我们使用列表推导式。

实际上，它们都是写相同内容的方式，但列表推导式更简洁、更优雅。

但在讨论这些方法之前，你可能需要深入了解循环和语句。这里有几个我过去写的文章，可能会对你有帮助：

[](/loops-and-statements-in-python-a-deep-understanding-with-examples-2099fc6e37d7?source=post_page-----7ab54d4819ee--------------------------------) ## Python 中的循环和语句：深入理解（带示例）

### 当它们看起来被理解时，还有更多的内容

towardsdatascience.com [](/python-loops-a-complete-guide-on-how-to-iterate-in-python-b29e0d12211d?source=post_page-----7ab54d4819ee--------------------------------) ## Python 循环：如何在 Python 中遍历的完整指南

### 利用 Python 中循环的力量

towardsdatascience.com

现在，让我们直接使用循环和语句来看几个例子。

假设我们有一个购物清单。我们希望我们的程序打印出我们喜欢一种水果，而不喜欢清单上的其他水果。以下是实现的方法：

```py
# Create shopping list
shopping_list = ["banana", "apple", "orange", "lemon"]
# Print the one I like
for fruit in shopping_list:
    if fruit == "lemon":
        print(f"I love {fruit}")
    else:
        print(f"I don't like {fruit}")

>>>

    I don't like banana
    I don't like apple
    I don't like orange
    I love lemon
```

另一个例子可能是这样的。假设我们有一个数字列表，并且我们只想打印出偶数。以下是实现的方法：

```py
# Create list
numbers = [1,2,3,4,5,6,7,8]
# Create empty list
even_list = []
# Print even numbers
for even in numbers:
    if even %2 == 0:
        even_list.append(even)
    else:
        pass

print(even_list)

>>>

    [2, 4, 6, 8]
```

```py
**NOTE:**

If you are not familiar with the sintax %2 == 0 it means that we are
dividing a number by 2 and expect a reminder of 0\. In other words,
we are asking our program to intercept the even numbers.
```

因此，在上述示例中，我们创建了一个数字列表。然后，我们创建了一个空列表，在循环后用于附加所有偶数。这样，我们就从包含“常规”数字的列表中创建了一个偶数列表。

现在……这种使用循环和语句创建新列表的方式有点“繁琐”。我的意思是：这需要很多代码。我们可以使用列表推导式以更简洁的方式获得相同的结果。

例如，要创建一个包含偶数的列表，我们可以使用如下的列表推导式：

```py
# Create list
numbers = [1,2,3,4,5,6,7,8]
# Create list of even numbers
even_numbers = [even for even in numbers if even %2 == 0]
# Print even list
print(even_numbers)

>>>

    [2, 4, 6, 8]
```

所以，列表推导式直接创建一个新列表，并在其中定义条件。如我们所见，我们获得了与之前相同的结果，但只用了一行代码：不错吧！

现在，让我们使用列表推导式创建一个包含我喜欢的水果（和我不喜欢的水果）的列表：

```py
# Create shipping list
shopping_list = ["banana", "apple", "orange", "lemon"]
# Create commented list and print it
commented_list = [f"I love {fruit}" if fruit == "banana"
                  else f"I don't like {fruit}"
                  for fruit in shopping_list]
print(commented_list)

>>>

  ['I love banana', "I don't like apple", "I don't like orange",
   "I don't like lemon"]
```

所以，我们获得了与之前相同的结果，但只用了一行代码。唯一的区别是这里我们打印了一个列表（因为列表推导式创建了一个！），而之前我们只是打印了结果。

## 列表的列表

还有可能创建列表的列表，即嵌套在一个列表中的列表。当我们想将列出的数据表示为一个唯一的列表时，这种可能性很有用。

例如，假设我们想创建一个学生及其成绩的列表。我们可以创建如下的内容：

```py
# Create lis with students and their grades
students = [
    ["John", [85, 92, 78, 90]],
    ["Emily", [77, 80, 85, 88]],
    ["Michael", [90, 92, 88, 94]],
    ["Sophia", [85, 90, 92, 87]]
]
```

如果我们想计算每个学生的平均成绩，这是一种有用的表示方式。我们可以这样做：

```py
# Iterate over the list
for student in students:
    name = student[0] # Access names
    grades = student[1] # Access grades
    average_grade = sum(grades) / len(grades) # Calculate mean grades
    print(f"{name}'s average grade is {average_grade:.2f}")

>>>

    John's average grade is 86.25
    Emily's average grade is 82.50
    Michael's average grade is 91.00
    Sophia's average grade is 88.50
```

# 元组

元组是 Python 中的另一种数据结构类型。它们用圆括号定义，并且像列表一样，可以包含任何数据类型，数据类型之间用逗号分隔。例如，我们可以这样定义一个元组：

```py
# Define a tuple and print it
my_tuple = (1, 3.0, "John")
print(my_tuple)

>>>

    (1, 3.0, 'John')
```

元组和列表的区别在于元组是**不可变**的。这意味着元组的元素不能被更改。例如，如果我们尝试向元组中添加一个值，我们会得到一个错误：

```py
# Create a tuple with names
names = ("James", "Jhon", "Elizabeth")
# Try to append a name
names.append("Liza")

>>>

    AttributeError: 'tuple' object has no attribute 'append'
```

所以，由于我们不能修改元组，它们在我们希望数据不可变时很有用；例如，在我们不想犯错误的情况下。

一个实际的例子可能是电子商务的购物车。我们可能希望这种数据不可变，以便在操作时不犯错误。假设有人从我们的电子商务网站购买了一件衬衫、一双鞋子和一只手表。我们可以将这些数据以数量和价格的形式记录在一个元组中：

```py
# Create a chart as a tuple
cart = (
    ("Shirt", 2, 19.99),
    ("Shoes", 1, 59.99),
    ("Watch", 1, 99.99)
)
```

当然，准确地说，这是一个元组的元组。

由于元组是不可变的，它们在性能方面更高效，这意味着它们节省了计算机的资源。但在操作时，我们可以使用与列表相同的代码，所以我们不会再写一次。

最后，类似于列表，我们可以用内置函数`tuple()`创建一个元组，如下所示：

```py
# Create a tuple in a range
my_tuple = tuple(range(1, 10))
print(my_tuple)

>>>

  (1, 2, 3, 4, 5, 6, 7, 8, 9)
```

# 字典

字典是一种以键和值对存储数据的方式。我们可以这样创建一个字典：

```py
# Create a dictionary
my_dictionary = {'key_1':'value_1', 'key_2':'value_2'}
```

所以，我们用大括号创建一个字典，并在其中存储一对对的键和值，用冒号分隔。键值对用逗号分隔。

现在，让我们看看如何操作字典。

## 字典操作

字典的键和值可以是任何类型：字符串、整数或浮点数。例如，我们可以这样创建一个字典：

```py
# Create a dictionary of numbers and print it
numbers = {1:'one', 2:'two', 3:'three'}
print(numbers)

>>>

    {1: 'one', 2: 'two', 3: 'three'}
```

但我们也可以这样创建：

```py
# Create a dictionary of numbers and print it
numbers = {'one':1, 'two':2.0, 3:'three'}
print(numbers)

>>>

  {'one': 1, 'two': 2.0, 3: 'three'}
```

选择值和键的类型取决于我们需要解决的问题。无论如何，考虑到我们之前看到的字典，我们可以这样访问值和键：

```py
# Access values and keys
keys = list(numbers.keys())
values = tuple(numbers.values())
# Print values and keys
print(f"The keys are: {keys}")
print(f"The values are: {values}")

>>>

    The keys are: ['one', 'two', 3]
    The values are: (1, 2.0, 'three')
```

因此，如果我们的字典叫做 `numbers`，我们用 `numbers.keys()` 访问其键。使用 `numbers.values()` 我们可以访问其值。此外，请注意，我们使用之前看到的表示法创建了一个包含键的列表和一个包含值的元组。

当然，我们也可以遍历字典。例如，假设我们想打印出大于某个阈值的值：

```py
# Create a shopping list with fruits and prices
shopping_list = {'banana':2, 'apple':1, 'orange':1.5}
# Iterate over the values
for values in shopping_list.values():
    # Values greater than threshold
    if values > 1:
        print(values)

>>>

    2
    1.5
```

像列表一样，字典是可变的。因此，如果我们想向字典中添加一个值，我们必须定义要添加的键和值。我们可以这样做：

```py
# Create the dictionary
person = {'name': 'John', 'age': 30}
# Add value and key and print
person['city'] = 'New York'
print(person)

>>>

    {'name': 'John', 'age': 30, 'city': 'New York'}
```

要修改字典的值，我们需要访问其键：

```py
# Create a dictionary
person = {'name': 'John', 'age': 30}
# Change age value and print
person['age'] = 35
print(person)

>>>

    {'name': 'John', 'age': 35}
```

要从字典中删除一对键值，我们需要访问其键：

```py
# Create dictionary
person = {'name': 'John', 'age': 30}
# Delete age and print
del person['age']
print(person)

>>>

    {'name': 'John'}
```

## 嵌套字典

我们之前已经看到，我们可以创建列表的列表和元组的元组。类似地，我们可以创建嵌套字典。假设，例如，我们想创建一个字典来存储与一个班级学生相关的数据。我们可以这样做：

```py
# Create a classroom dictionary
classroom = {
    'student_1': {
        'name': 'Alice',
        'age': 15,
        'grades': [90, 85, 92]
    },
    'student_2': {
        'name': 'Bob',
        'age': 16,
        'grades': [80, 75, 88]
    },
    'student_3': {
        'name': 'Charlie',
        'age': 14,
        'grades': [95, 92, 98]
    }
```

因此，每个学生的数据表示为一个字典，所有字典都存储在一个唯一的字典中，表示整个课堂。正如我们所见，字典的值甚至可以是列表（或者元组，如果我们愿意的话）。在这个例子中，我们使用列表来存储每个学生的成绩。

要打印一个学生的值，我们只需记住，从课堂字典的角度来看，我们需要访问键，在这种情况下，键就是学生本身。这意味着我们可以这样做：

```py
# Access student_3 and print
student_3 = classroom['student_3']
print(student_3)

>>>

    {'name': 'Charlie', 'age': 14, 'grades': [95, 92, 98]}
```

## 字典推导

字典推导允许我们简洁高效地创建字典。它类似于列表推导，但不创建列表，而是创建一个字典。

假设我们有一个字典，其中存储了一些物品及其价格。我们想知道价格低于某个阈值的物品。我们可以这样做：

```py
# Define initial dictionary
products = {'shoes': 100, 'watch': 50, 'smartphone': 250, 'tablet': 120}
# Define threshold
max_price = 150
# Filter for threshold
products_to_buy = {fruit: price for fruit, price in products.items() if price <= max_price}
# Print filtered dictionary
print(products_to_buy)

>>>

    {'shoes': 100, 'watch': 50, 'tablet': 120}
```

所以，使用字典推导的语法是：

```py
new_dict = {key:value for key, value in iterable}
```

其中 iterable 是任何可迭代的 Python 对象。它可以是一个列表、一个元组、另一个字典等…

使用“标准”方法创建字典会需要大量的代码，包括条件、循环和语句。相反，如我们所见，字典推导允许我们仅用一行代码，基于条件创建字典。

字典推导在我们需要从其他来源或数据结构中检索数据来创建字典时特别有用。例如，假设我们需要从两个列表中检索值来创建一个字典。我们可以这样做：

```py
# Define names and ages in lists
names = ['John', 'Jane', 'Bob', 'Alice']
cities = ['New York', 'Boston', 'London', 'Rome']
# Create dictionary from lists and print results
name_age_dict = {name: city for name, city in zip(names, cities)}
print(name_age_dict)

>>>

   {'John': 'New York', 'Jane': 'Boston', 'Bob': 'London', 'Alice': 'Rome'}
```

# 数据框

![](img/04d94eadfc725ebd55688664c54ce861.png)

数据框是表格数据的表示。图片来源于熊猫网站：[`pandas.pydata.org/docs/getting_started/index.html`](https://pandas.pydata.org/docs/getting_started/index.html)

数据框是由列和行组成的二维数据结构。因此，它在某种程度上类似于电子表格或 SQL 数据库中的表格。它们具有以下特点：

1.  每一行代表一个独立的观察或记录。

1.  每一列代表一个变量或数据的特定属性。

1.  它们有标记的行（称为索引）和列，使得操作数据变得容易。

1.  列可以包含不同类型的数据，如整数、字符串或浮点数。即使是单列也可以包含不同的数据类型。

虽然数据框是数据分析和数据科学中使用的典型数据结构，但 Python 软件工程师也可能需要操作数据框，这就是我们进行数据框概述的原因。

这是数据框的显示方式：

![](img/9a79c8ab7583d84cf5c90c917a03e7aa.png)

数据框。图片由 Federico Trotta 提供。

所以，在左侧（蓝色矩形中）我们可以看到索引，意味着行数。我们可以看到数据框可以包含不同类型的数据。特别是，列“Age”包含不同的数据类型（一个字符串和两个整数）。

## 使用 Pandas 进行基本的数据框操作

尽管最近开始流行一种叫做“Polars”的新库来操作数据框，但在这里我们将看到一些 Pandas 的数据操作，Pandas 仍然是目前使用最广泛的库。

首先，通常我们可以通过从 `.xlsx` 或 `.cvs` 文件导入数据来创建数据框。在 Pandas 中我们可以这样做：

```py
import pandas as pd

# Import cvs file
my_dataframe = pd.read_csv('a_file.csv')

# Import xlsx
my_dataframe_2 = pd.read_excel('a_file_2.xlsx')
```

如果我们想创建一个数据框：

```py
import pandas as pd

# Create a dictionary with different types of data
data = {
    'Name': ['John', 'Alice', 'Bob'],
    'Age': ['twenty-five', 30, 27],
    'City': ['New York', 'London', 'Sydney'],
    'Salary': [50000, 60000.50, 45000.75],
    'Is_Employed': [True, True, False]
}

# Create the dataframe
df = pd.DataFrame(data)
```

这是我们上面展示的数据框。所以，正如我们所见，我们首先创建一个字典，然后用 `pd.DataFrame()` 方法将其转换为数据框。

我们有三种方式来可视化数据框。假设我们有一个名为 `df` 的数据框：

1.  第一个是 `print(df)`。

1.  第二个是`df.head()`，它将显示我们数据框的前 5 行。如果我们有一个包含很多行的数据框，我们可以显示超过前五行。例如，`df.head(20)`显示前 20 行。

1.  第三个是`df.tail()`，它的功能与`head()`完全相同，但它显示的是最后几行。

在可视化方面，使用上述 `df`，这是 `df.head()` 显示的内容：

![](img/c29ad0eb546983665b1aae6dc4c6c565.png)

`df.head()` 显示的内容。图片由 Federico Trotta 提供。

这是 `print(df)` 显示的内容：

![](img/b2673cd6325428d6a8b64a2c8c52618e.png)

`print(df)` 显示的内容。图片由 Federico Trotta 提供。

在像这样的小数据集的情况下，差异只是个人喜好（我更喜欢`head()`因为它“显示数据的表格性”）。但在大数据集的情况下，`head()`要好得多。试试看，告诉我！

考虑到 Pandas 是一个非常广泛的库，这意味着它允许我们以多种方式操作表格数据，因此需要单独处理。这里我们只是展示最基本的内容，所以我们将学习如何添加和删除列（数据框的列也称为“Pandas 系列”）。

假设我们想向上面提到的数据框`df`添加一列，表示人们是否已婚。我们可以这样做：

```py
# Add marital status
df["married"] = ["yes", "yes", "no"]
```

```py
**NOTE:**

this is the same notation we used to add values to a dictionary.
Return back on the article and compare the two methods.
```

显示头部，我们得到：

![](img/cc1912fe5f8149e37fcfe4b078ec6221.png)

包含婚姻状况的数据框 df。图片由 Federico Trotta 提供。

删除一列：

```py
# Delete the "Is_Employed" column
df = df.drop('Is_Employed', axis=1)
```

我们得到：

![](img/2da1e2c02460c0505484f6ac000518ee.png)

删除与就业数据相关的列后的数据框。图片由 Federico Trotta 提供。

请注意，我们需要使用`axis=1`，因为这里我们告诉 Pandas 删除列，并且由于数据框是二维数据结构，`axis=1`表示垂直方向。

相反，如果我们想要删除一行，我们需要使用`axis=0`。例如，假设我们想删除索引为 1 的行（即第二行，因为我们从 0 开始计数）：

```py
# Delete the second row 
df = df.drop(1, axis=0)
```

我们得到：

![](img/17e4b949cc3b9e2e85656ae2e5e4e0d6.png)

删除第二行后的数据框。图片由 Federico Trotta 提供。

# 结论

到目前为止，我们已经看到了 Python 中最常用的数据结构。这些不是唯一的数据结构，但肯定是最常用的。

此外，使用其中一种数据结构并没有对错之分：我们只需要理解我们需要存储什么数据，并选择最适合这种任务的数据结构。

希望这篇文章帮助您理解这些数据结构的使用以及何时使用它们。

**免费 PYTHON 电子书：**

开始学习 Python 数据科学但感到困难？[***订阅我的新闻通讯并获取我的免费电子书：这将为您提供正确的学习路径，帮助您通过实践学习 Python 数据科学。***](https://federico-trotta.ck.page/a3970f33f4)

喜欢这个故事吗？成为 Medium 会员，享受 5$/月 [通过我的推荐链接](https://medium.com/@federicotrotta/membership)：我将获得一小笔佣金，但不会增加您的额外费用：

[](https://medium.com/@federicotrotta/membership?source=post_page-----7ab54d4819ee--------------------------------) [## 使用我的推荐链接加入 Medium - Federico Trotta

### 阅读 Federico Trotta 的每一个故事（以及 Medium 上的其他成千上万的作家）。您的会员费用直接支持…

medium.com](https://medium.com/@federicotrotta/membership?source=post_page-----7ab54d4819ee--------------------------------)
