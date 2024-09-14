# Python OOP 教程：如何创建类和对象

> 原文：[`towardsdatascience.com/python-oop-tutorial-how-to-create-classes-and-objects-c36a92b01552`](https://towardsdatascience.com/python-oop-tutorial-how-to-create-classes-and-objects-c36a92b01552)

## 关于在面向对象编程（OOP）中使用类和对象的简单指南

[](https://medium.com/@yazihejazi?source=post_page-----c36a92b01552--------------------------------)![Yasmine Hejazi](https://medium.com/@yazihejazi?source=post_page-----c36a92b01552--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c36a92b01552--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c36a92b01552--------------------------------) [Yasmine Hejazi](https://medium.com/@yazihejazi?source=post_page-----c36a92b01552--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c36a92b01552--------------------------------) ·阅读时间 6 分钟·2023 年 1 月 4 日

--

![](img/0dde12a92beeefc82d69df98b05a3724.png)

由 [Taylor Heery](https://unsplash.com/@taylorheeryphoto?utm_source=medium&utm_medium=referral) 提供的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

在 Python 编程中，一切都是对象。变量甚至函数都是对象。**类是一个模具**，用于创建对象。

*想象一个冰棒托盘。* 首先，你制造冰棒托盘以创建你所需的大小、形状和深度；这就是**类**。然后，你可以决定向冰棒托盘中倒入什么来冻结——也许你加入水并简单制作冰块，或者你加入不同种类的水果和果汁制作冰棒。你创建的每个冰棒都是一个**对象**，对象可以有不同的“数据”或口味。

本文将通过代码演示如何创建自己的类并在 Python 代码中使用它。类的不同组件可以分解为以下内容：构造函数、获取器和设置器、属性、装饰器、私有命名、类方法、属性和继承。

**何时使用类/对象与模块：**

+   当你需要多个具有类似行为但数据不同的独立实例时使用类

+   当你需要支持继承时使用类；模块不支持继承

+   如果你只需要一个东西，就使用模块

+   使用最简单的解决方案；模块通常比类更简单

# 类的介绍示例

以下是一个简单类的示例。在这个类中，我们看到三个组件：`__init__` 方法，它是初始化方法或构造函数，一个称为 `toss` 的设置方法，以及一个称为 `get_sideup` 的获取方法。

```py
class Coin():
  def __init__(self): # Constructor
    self.sideup = "Heads"

  def toss(self): # Method
    if(random.randint(0, 1) == 0):
      self.sideup = "Heads"
    else:
      self.sideup = "Tails"

  def get_sideup(self): # Method
    return self.sideup
```

如何在你的主 Python 脚本中使用它？在你的脚本中，你只需调用对象并将其设置为一个新变量。然后你可以开始使用它的组件。

```py
my_coin = Coin() # Creates the object
my_coin.toss()
print("This side is up: ", my_coin.get_sideup())
```

让我们来分解一下。

# 类组件

## 对象初始化方法

当你看到一个方法具有特殊名称`__init__`时，你会知道这是对象初始化方法。这被称为**构造函数**，因为它在内存中构造了对象。当你创建类的对象时，这个方法会自动运行。

```py
class Person():
  def __init__(self, name):
    self.name = name
```

我们上面的 `__init__` 方法需要一个名为**name**的参数。当我们使用 `Person` 类创建对象时，我们应该像这样传入一个名字：`Person("Bob")`。

**self** 参数指定它指的是对象本身。记住，类是一个**模板**，我们可以使用这个模板来初始化（然后稍后修改）多个对象。例如，我们可以用 `Person` 类创建两个对象：

```py
me = Person("Author")
me.name    # --> "Author"

you = Person("Reader")
you.name   # --> "Reader"
```

## Getter 和 Setter

一些面向对象语言支持私有对象属性，这些属性不能从外部直接访问。因此，你需要**getter**和**setter**方法来读取和写入私有属性的值。

在 Python 中，所有属性和方法都是公开的。我们不需要 getter 和 setter。为了做到“Pythonic”，使用**属性**。

```py
class Duck():
  def __init__(self, input_name):
    self.hidden_name = input_name # The user won't know to try duck.hidden_name

  def get_name(self): # Getter
    print("inside the getter") 
    return self.hidden_name

  def set_name(self, input_name): # Setter
    print("inside the setter") 
    self.hidden_name = input_name

  name = property(get_name, set_name)
```

最后一行将 getter 和 setter 方法定义为**name**属性的属性。现在它会在以下情况下调用 getter 和 setter 方法：

```py
pet = Duck("Donald")
pet.name
  # --> inside the getter
  # --> "Harold"
pet.name = "Daffy"
  # --> inside the setter
```

## 装饰器

装饰器是另一种定义属性的方法（即我们上面做的事情）。

```py
class Duck():
  def __init__(self, input_name):
    self.hidden_name = input_name # The user won't know to try duck.hidden_name

  @property 
  def name(self): # Getter
    print("inside the getter") 
    return self.hidden_name

  @name.setter
  def name(self, input_name): # Setter
    print("inside the setter") 
    self.hidden_name = input_name

  name = property(get_name, set_name)
```

## 隐私命名

首先在名称中使用两个下划线。这使得一旦你创建了对象，属性就无法在类定义外部访问。这也有助于防止子类意外覆盖属性。

在我们的 Duck 类中，代替使用 `hidden_name`，使用 `__name`。

`self.hidden_name = input_name` → `self.__name = input_name`

## 类方法

到目前为止，我们演示的都是**实例方法**。我们怎么知道？实例方法的第一个参数是**self**。当你调用实例方法时，调用只会影响你正在使用的对象的副本。

类方法影响整个类（因此影响所有对象副本）。类方法使用**cls**参数，而不是**self**参数。类方法可以通过使用类装饰器`@classmethod`来定义。

```py
@classmethod
def count_objects(cls):
  print("The class has", cls.count, "objects") 
```

**静态方法**是第三种类型的方法，它既不影响类也不影响其对象。它不使用 self 或 cls 参数。它只是为了方便而存在。

```py
@staticmethod
def commercial():
  print("This product is brought to you by Medium.") 
```

## 属性

实例属性是我们希望对象实例共享的外部行为。一个学生类可能具有以下属性：

+   方法：student.get_gpa()，student.add_class()，student.get_schedule()

+   数据：student.first_name，student.last_name，student.class_list

`dir(object_instance)` 给你提供该对象的属性列表。

`object_instance.__dict__` 为你提供特定于该实例的所有实例属性（及其值）

类属性是类的属性，而不是类的 *实例* 的属性。这是类的所有对象共享的属性。假设我们想跟踪每个学生都是人类：

```py
class Student():
  isHuman = True   # --> class attribute

  def __init__(self, ...):
    ...
```

如果你想了解更多，可以查看这个[关于 Python 类属性的详尽指南](https://www.toptal.com/python/python-class-attributes-an-overly-thorough-guide)。

# 继承

继承允许你创建一个类的层次结构，其中一个类继承了父类的所有属性和行为。然后，你可以在子类上进行自己的规格定义，这些定义不同于父类。

例如，我们有一个父类 `Animal`，它具有吃和睡的能力。然后我们创建一个子类 `Cat`，它继承了 `Animal` 的属性，并增加了自己特有的属性。

```py
class Animal():
  def eat(self):
    print("Munch munch")

  def sleep(self):
    print("Zzz...")

class Cat(Animal):
  def meow(self):
    print("Meow!")
```

你需要做的就是将 `Animal` 类传递给 `Cat`。现在 `Cat` 类有了 `eat()` 和 `sleep()` 方法。你可以通过在 `Cat` 中定义方法来覆盖 `eat` 或 `sleep` 方法。你也可以通过 `__init__()` 方法覆盖任何方法。

子类可以添加父类中没有的方法（例如 `meow()`）。父类将不包含此方法。

当子类自己做某些事情但仍需要从父类中获取某些内容时，使用 `super()`：

```py
class Person():
  def __init__(self, name):
    self.name = name

class EmailPerson(Person):
  def __init__(self, name, email):
    super().__init__(name)
    self.email = email
```

**继承的好处：**

+   允许子类重用父类的代码

+   不必从头开始创建类，你可以专门化或扩展一个类

+   父类可以定义一个接口，以允许子类与程序进行交互

+   允许程序员组织相关对象

# 总结

+   类是一个模具（冰棒托），对象是从该类中创建的（冰棒）

+   对象可以调用其类的实例方法（使用 self）来接收和更改数据

+   隐私命名有助于防止子类意外覆盖属性

+   类本身具有方法（使用 cls），你可以跟踪和操作该类的所有对象实例

+   继承允许我们扩展相似的类
