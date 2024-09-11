# Python异常测试：清晰而有效的方法

> 原文：[https://towardsdatascience.com/python-exception-testing-clean-and-effective-methods-86799da86b90?source=collection_archive---------5-----------------------#2023-07-24](https://towardsdatascience.com/python-exception-testing-clean-and-effective-methods-86799da86b90?source=collection_archive---------5-----------------------#2023-07-24)

## 超越基础：针对Pytest和Unittest的高级Python异常测试

[](https://naomikriger.medium.com/?source=post_page-----86799da86b90--------------------------------)[![Naomi Kriger](../Images/14839f859e1375965c046912f00df5b9.png)](https://naomikriger.medium.com/?source=post_page-----86799da86b90--------------------------------)[](https://towardsdatascience.com/?source=post_page-----86799da86b90--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----86799da86b90--------------------------------) [Naomi Kriger](https://naomikriger.medium.com/?source=post_page-----86799da86b90--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fce7969d594d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-exception-testing-clean-and-effective-methods-86799da86b90&user=Naomi+Kriger&userId=ce7969d594d&source=post_page-ce7969d594d----86799da86b90---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----86799da86b90--------------------------------) ·4 min read·Jul 24, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F86799da86b90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-exception-testing-clean-and-effective-methods-86799da86b90&user=Naomi+Kriger&userId=ce7969d594d&source=-----86799da86b90---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F86799da86b90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-exception-testing-clean-and-effective-methods-86799da86b90&source=-----86799da86b90---------------------bookmark_footer-----------)![](../Images/57f35113c2a900546ce0274ca735c8b0.png)

[图片](https://pixabay.com/illustrations/woman-computer-work-working-5576945/) 由 [chenspec](https://pixabay.com/users/chenspec-7784448/) 在 [pixabay](http://pixabay.com) 上提供

测试异常不仅仅是一种形式 - 它是编写可靠代码的关键部分。在本教程中，我们将探讨测试Python代码的方法，这些方法包括处理和不处理异常，验证异常消息的准确性，涵盖`pytest`和`unittest`，并为每个框架提供带有和不带参数化的测试。

在本教程结束时，你将对如何为代码编写干净、高效和有用的异常测试有一个扎实的理解。

让我们查看以下示例：

```py
def divide(num_1: float, num_2: float) -> float:
    if not isinstance(num_1, (int, float)) \
            or not isinstance(num_2, (int, float)):
        raise TypeError("at least one of the inputs "
                        f"is not a number: {num_1}, {num_2}")

    return num_1 / num_2
```

我们可以为上述函数测试几种情况——正常流、零分母和非数字输入。

现在，让我们看看这样的测试在使用`pytest`时会是什么样子：

# pytest

```py
from contextlib import nullcontext as does_not_raise

import pytest

from operations import divide

def test_happy_flow():
    with does_not_raise():
        assert divide(30, 2.5) is not None
    assert divide(30, 2.5) == 12.0

def test_division_by_zero():
    with pytest.raises(ZeroDivisionError) as exc_info:
        divide(10.5, 0)
    assert exc_info.value.args[0] == "float division by zero"

def test_not_a_digit():
    with pytest.raises(TypeError) as exc_info:
        divide("a", 10.5)
    assert exc_info.value.args[0] == \
           "at least one of the inputs is not a number: a, 10.5"
```

我们还可以进行一个合理性检查，看看当我们测试无效流时，错误的异常类型或尝试检查在正常流中抛出的异常时会发生什么。在这些情况下，测试将失败：

```py
# Both tests below should fail

def test_wrong_exception():
    with pytest.raises(TypeError) as exc_info:
        divide(10.5, 0)
    assert exc_info.value.args[0] == "float division by zero"

def test_unexpected_exception_in_happy_flow():
    with pytest.raises(Exception):
        assert divide(30, 2.5) is not None
```

那么，上面的测试为什么会失败呢？`with`上下文捕获请求的特定类型异常，并验证异常类型是否确实是我们要求的。

在`test_wrong_exception_check`中，抛出了一个异常（`ZeroDivisionError`），但未被`TypeError`捕获。因此，在堆栈跟踪中，我们会看到`ZeroDivisionError`被抛出但未被`TypeError`上下文捕获。

在`test_redundant_exception_context`中，我们的`with pytest.raises`上下文尝试验证请求的异常类型（在这种情况下我们提供了`Exception`），但由于没有异常被抛出——测试失败，并显示`Failed: DID NOT RAISE <class `Exception`>`。

现在，进入下一个阶段，让我们探索如何通过使用`parametrize`使我们的测试更加简洁和清晰。

## 参数化

```py
from contextlib import nullcontext as does_not_raise

import pytest

from operations import divide

@pytest.mark.parametrize(
    "num_1, num_2, expected_result, exception, message",
    [
        (30, 2.5, 12.0, does_not_raise(), None),

        (10.5, 0, None, pytest.raises(ZeroDivisionError),
         "float division by zero"),

        ("a", 10.5, None, pytest.raises(TypeError),
         "at least one of the inputs is not a number: a, 10.5")

    ],
    ids=["valid inputs",
         "divide by zero",
         "not a number input"]
)
def test_division(num_1, num_2, expected_result, exception, message):
    with exception as e:
        result = divide(num_1, num_2)
    assert message is None or message in str(e)
    if expected_result is not None:
        assert result == expected_result
```

`ids`参数更改显示在IDE测试栏视图中的测试用例名称。在下面的截图中，我们可以看到它的实际效果：左侧有`ids`，右侧没有`ids`。

![](../Images/fdc3abb6e70438e37f387e7ad923862e.png)

作者截图

既然我们已经覆盖了`pytest`框架，让我们看看如何使用`unittest`编写相同的测试。

# unittest

```py
from unittest import TestCase

from operations import divide

class TestDivide(TestCase):
    def test_happy_flow(self):
        result = divide(0, 10.5)
        self.assertEqual(result, 0)

    def test_division_by_zero(self):
        with self.assertRaises(ZeroDivisionError) as context:
            divide(10, 0)
        self.assertEqual(context.exception.args[0], "division by zero")

    def test_not_a_digit(self):
        with self.assertRaises(TypeError) as context:
            divide(10, "c")
        self.assertEqual(context.exception.args[0],
                         "at least one of the inputs "
                         "is not a number: 10, c")
```

如果我们想在`unittest`中使用`parameterized`，我们需要安装该包。让我们看看在`unittest`中参数化的测试会是什么样的：

## 参数化

```py
import unittest

from parameterized import parameterized  # requires installation

from operations import divide

def get_test_case_name(testcase_func, _, param):
    test_name = param.args[-1]
    return f"{testcase_func.__name__}_{test_name}"

class TestDivision(unittest.TestCase):

    @parameterized.expand([
        (30, 2.5, 12.0, None, None, "valid inputs"),
        (10.5, 0, None, ZeroDivisionError,
         "float division by zero", "divide by zero"),
        ("a", 10.5, None, TypeError,
         "at least one of the inputs is not a number: a, 10.5",
         "not a number input")
    ], name_func=get_test_case_name)
    def test_division(self, num_1, num_2, expected_result, exception_type,
                      exception_message, test_name):
        with self.subTest(num_1=num_1, num_2=num_2):
            if exception_type is not None:
                with self.assertRaises(exception_type) as e:
                    divide(num_1, num_2)
                self.assertEqual(str(e.exception), exception_message)
            else:
                result = divide(num_1, num_2)
                self.assertIsNotNone(result)
                self.assertEqual(result, expected_result)
```

在`unittest`中，我们也修改了测试用例名称，类似于上面的`pytest`示例。然而，为了实现这一点，我们使用了`name_func`参数以及一个自定义函数。

总结一下，今天我们探讨了测试Python异常的有效方法。我们学会了如何识别预期的异常是否被抛出，并验证异常消息是否符合我们的期望。我们检查了多种测试`divide`函数的方法，包括使用`pytest`的传统方法和使用`parametrize`的更清晰方法。我们还探索了`unittest`等效的`parameterized`，它需要安装该库，以及不使用它的情况。使用`ids`和自定义测试名称在IDE的测试栏中提供了更清晰和更有信息量的视图，使我们更容易理解和导航测试用例。通过使用这些技术，我们可以改进单元测试，确保代码适当地处理异常。

祝测试愉快！

![](../Images/24b04dbae0eb55fa6f00e1992c4ff086.png)

[图片](https://pixabay.com/photos/code-program-software-digital-7198654/) 来自 [jakob5200](https://pixabay.com/users/jakob5200-10067216/) 在 [pixabay](http://pixabay.com)
