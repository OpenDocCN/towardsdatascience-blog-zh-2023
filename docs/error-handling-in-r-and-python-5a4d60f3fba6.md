# R 和 Python 的错误处理

> 原文：[`towardsdatascience.com/error-handling-in-r-and-python-5a4d60f3fba6`](https://towardsdatascience.com/error-handling-in-r-and-python-5a4d60f3fba6)

## 如果一个错误在函数中途出现，可以进行处理

[](https://gustavorsantos.medium.com/?source=post_page-----5a4d60f3fba6--------------------------------)![Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----5a4d60f3fba6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5a4d60f3fba6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5a4d60f3fba6--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----5a4d60f3fba6--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5a4d60f3fba6--------------------------------) ·阅读时间 5 分钟·2023 年 4 月 27 日

--

![](img/aa8bcccb535506d244fe491e4c34f330.png)

图片由 [Joshua Hoehne](https://unsplash.com/@mrthetrain?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，[Unsplash](https://unsplash.com/photos/WPrTKRw8KRQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

一件事是明确的：*如果你曾经编码过，你一定遇到过错误。就是这样。*

错误并不总是坏事。我同意，有时候它们可能让我们抓狂，特别是当我们反复查看代码却找不到错误时。然而，错误信息必须被理解为代码中某些地方未按预期工作。

说实话，我们可以创建 N 个测试场景，但最终用户会发现 N+1 个错误。这没关系，只要我们能计划最常见的错误。

在本文中，我们将探讨一些已知的错误处理函数，这些函数可以在发生错误时让我们的代码继续运行。我们还将看到 R 和 Python 的代码片段。

继续阅读。

# 错误处理

> 错误处理是一种编程资源，可以让你的代码在发现错误后继续运行，而不会中断。

想象一下，你正在编写一个函数，该函数接受两个数字，计算它们之间的差值，并返回差值相对于第一个数字的百分比。这是一个相当直接的函数，但如果你将数字作为字符串输入，代码将崩溃并显示错误信息：*我不能用文本进行数学运算，伙计。*

这时，错误处理可能会很有用。如果我们知道这是来自最终用户的一种常见错误，我可以计划让我的代码绕过这个错误并返回结果，即使输入的数字是作为字符串输入的。

程序员可以说：

+   尝试运行这段代码。

+   如果这不起作用，你可以运行这段代码。

让我们继续，看看如何编写代码。

# 编程

## R

在 R 中，用于错误处理的函数是`tryCatch`。如前所述，它将尝试运行主要代码，但如果它“捕获”到错误，则次要代码（解决方法）可以运行。

回到上一节的示例，一个没有错误处理的百分比差异计算函数如下。

```py
# Function to calculate the percentage difference between two numbers
pct_difference <- function(n1, n2) {
  "Function that takes two numbers and return the percentual difference
   between n1 and n2, being n1 the reference number"

  pct_diff <- (n1-n2)/n1

  return ( cat('The difference between', n1, 'and', n2, 'is', n1-n2,
               ', which is', pct_diff*100, '% of', n1) )
}

# Test 1
pct_difference(10, 2)
[OUT] The difference between 10 and 2 is 8 , which is 80 % of 10

# Test 2
pct_difference(10, '2')
[OUT] Error in n1 - n2 : non-numeric argument to binary operator
```

请注意，在 Test2 中，如果我们输入字符串，代码不会继续执行。它会中断。

现在，我们可以使用`tryCatch`并使函数即使在输入数字作为字符串时也返回值。

```py
# Function to calculate the percentage difference between two numbers
# with error handling
pct_difference_error_handling <- function(n1, n2) {
  "Function that takes two numbers and return the percentual difference
   between n1 and n2, being n1 the reference number"

# Try the main code
  tryCatch(pct_diff <- (n1-n2)/n1,

        # If you find an error, use this code instead
           error= return(
             cat( 'The difference between', as.integer(n1), 'and', as.integer(n2), 'is', 
                  (as.integer(n1)-as.integer(n2)), 'which is', 
                  100*(as.integer(n1)-as.integer(n2))/as.integer(n1),
                  '% of', n1 )#cat  
             )#return
           )#trycatch

  # If no error happens, return this statement
  return ( cat('The difference between', n1, 'and', n2, 'is', n1-n2,
               ', which is', pct_diff*100, '% of', n1) )
}

# Test 1
pct_difference_error_handling(10, 3)
[OUT] The difference between 10 and 3 is 7 which is 70 % of 10

# Test 2
pct_difference_error_handling('10', '3')
[OUT] The difference between 10 and 3 is 7 which is 70 % of 10
```

完美！它工作得很好。现在我们来看看如何用 Python 实现相同的代码。

## Python

在 Python 中编写相同的函数也非常简单。在这种情况下，我们应该使用`try ... except`格式。正如你可能已经得出的结论，`try` 是主要代码片段，但如果发生异常（错误），则运行次要代码片段，即解决方法。

```py
# Function to calculate the percentage difference between two numbers
# with error handling
def pct_difference_error_handling(n1, n2):
  '''Function that takes two numbers and return the percentual difference
   between n1 and n2, being n1 the reference number'''

  # Try the main code
  try:
    pct_diff = (n1-n2)/n1
    return f'The difference between {n1} and {n2} is {n1-n2}, which is {pct_diff*100}% of {n1}'

  # If you find an error, use this code instead
  except:
    pct_diff = (int(n1)-int(n2))/int(n1)
    return f'The difference between {n1} and {n2} is {int(n1)-int(n2)}, which is {pct_diff*100}% of {n1}'

# Test 1
pct_difference_error_handling(10,2)
[OUT] The difference between 10 and 2 is 8, which is 80.0% of 10

# Test2
pct_difference_error_handling('10', '2')
[OUT] The difference between 10 and 2 is 8, which is 80.0% of 10
```

非常好。代码运行正常。

## 最终

我们可以在两种语言中的任何一个函数中添加`finally`子句。这个子句会始终运行，无论 try 块是否引发错误。因此，它可以是一个完成消息或总结，例如。

```py
try:
  print(x)
except:
  print("There's no x")
finally:
  print("Code ended")

[OUT] 
# There's no x
# Code ended
```

```py
tryCatch( print(x),
          error= print('There is no x'),
          finally= print('Code ended') )

[OUT]
#[1] "There is no x"
#Error in value[[3L]](cond) : attempt to apply non-function
#[1] "Code ended"
```

我知道这些函数很简单，并且有更好的方法来创建它们而不使用错误处理。然而，我认为这是最温和的方式来教你如何编写带有异常的函数。

现在轮到你把它应用到你的工作中，以帮助你解决业务问题，适用于许多不同的用例和形式。

# 在你离开之前

在这个快速教程中，我们学习了如何使用 R 或 Python 处理错误。

错误处理是一个编程资源，它使你的代码在遇到预期错误时仍能继续运行。在我们的示例中，这个错误是输入了字符串而不是数字。

R 错误处理：

+   使用函数`tryCatch(expression, error, finally)`

在 Python 中：

+   使用函数`try: expression except: expression finally: expression`

如果你喜欢这个内容，请关注我的博客获取更多信息。

[](https://gustavorsantos.medium.com/?source=post_page-----5a4d60f3fba6--------------------------------) [## Gustavo Santos - Medium

### 阅读 Gustavo Santos 在 Medium 上的文章。数据科学家。我从数据中提取见解，帮助个人和公司……

gustavorsantos.medium.com](https://gustavorsantos.medium.com/?source=post_page-----5a4d60f3fba6--------------------------------)

也可以在[LinkedIn](https://www.linkedin.com/in/gurezende/)找到我。

现在，我还有一个[Topmate 页面](https://topmate.io/gustavo_santos)，你可以在那里预约时间，如果你想讨论数据科学的话。

# 参考

[](https://www.w3schools.com/python/python_try_except.asp?source=post_page-----5a4d60f3fba6--------------------------------) [## Python Try Except

### try 块让你测试一块代码是否有错误。except 块让你处理错误。else 块让……

www.w3schools.com](https://www.w3schools.com/python/python_try_except.asp?source=post_page-----5a4d60f3fba6--------------------------------)

[`en.wikipedia.org/wiki/Exception_handling`](https://en.wikipedia.org/wiki/Exception_handling)

[](https://www.statology.org/r-trycatch/?source=post_page-----5a4d60f3fba6--------------------------------) [## 如何在 R 中编写你的第一个 tryCatch() 函数 - Statology

### 你可以使用 `tryCatch()` 函数在 R 中返回某个表达式的值，或者在出现警告时生成自定义消息…

www.statology.org](https://www.statology.org/r-trycatch/?source=post_page-----5a4d60f3fba6--------------------------------)
