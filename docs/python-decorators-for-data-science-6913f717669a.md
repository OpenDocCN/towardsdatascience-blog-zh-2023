# 我在几乎所有数据科学项目中使用的 5 个 Python 装饰器

> 原文：[`towardsdatascience.com/python-decorators-for-data-science-6913f717669a`](https://towardsdatascience.com/python-decorators-for-data-science-6913f717669a)

## 装饰器提供了一种新颖且便捷的方法，从缓存到发送通知应有尽有。

[](https://thuwarakesh.medium.com/?source=post_page-----6913f717669a--------------------------------)![Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----6913f717669a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6913f717669a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6913f717669a--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----6913f717669a--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6913f717669a--------------------------------) ·6 分钟阅读·2023 年 3 月 13 日

--

![](img/fc0bc39e9f13f04a39005e4e7610a8fe.png)

图片由 [Elena Mozhvilo](https://unsplash.com/@miracleday?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

一开始，每个开发者的目标都是让事情运作起来。逐渐地，我们开始关注可读性和可扩展性。这时我们开始考虑装饰器。

装饰器是为函数提供额外行为的绝佳方式。作为数据科学家，我们经常需要将一些小功能注入到函数定义中。

使用装饰器，你会惊讶于你能减少多少代码重复并提高可读性。我确实感到惊讶。

[](https://levelup.gitconnected.com/streamlit-openai-gpt3-example-app-b333da955ceb?source=post_page-----6913f717669a--------------------------------) [## 使用 Streamlit 在几分钟内创建 GPT3 驱动的应用程序

### 学会构建智能应用程序，而无需过多担心软件开发。

levelup.gitconnected.com](https://levelup.gitconnected.com/streamlit-openai-gpt3-example-app-b333da955ceb?source=post_page-----6913f717669a--------------------------------) [](/etl-github-actions-cron-383f618704b6?source=post_page-----6913f717669a--------------------------------) ## 如何使用 GitHub Actions 构建简单的 ETL 流水线

### ETL 不必复杂。如果是这样的话，可以使用 GitHub Actions。

towardsdatascience.com

这是我在几乎每个数据密集型项目中使用的五种最常见的装饰器。

## 1\. 重试装饰器

在数据科学项目和软件开发项目中，我们经常依赖外部系统。事情并不总是在我们的控制之下。

[](/3-important-sql-optimization-technique-d6da3e9c8442?source=post_page-----6913f717669a--------------------------------) ## 3 SQL 优化技巧，能立即提升查询速度

### 在完全转到不同的数据模型之前，可以尝试一些简单的技巧。

[towardsdatascience.com [](https://levelup.gitconnected.com/3-ways-of-web-scraping-in-python-e953c4a96ec2?source=post_page-----6913f717669a--------------------------------) [## Python 网络抓取的宁静交响曲——三重奏

### 在 Python 中进行网络抓取的最简单、最灵活和最全面的方法

[levelup.gitconnected.com](https://levelup.gitconnected.com/3-ways-of-web-scraping-in-python-e953c4a96ec2?source=post_page-----6913f717669a--------------------------------)

当发生意外事件时，我们可能希望我们的代码等待一段时间，以便外部系统自行修正并重新运行。

我倾向于在 Python 装饰器中实现这个重试逻辑，以便可以注解任何函数来应用重试行为。

这是一个重试装饰器的代码。

```py
import time
from functools import wraps
```

```py
def retry(max_tries=3, delay_seconds=1):
    def decorator_retry(func):
        @wraps(func)
        def wrapper_retry(*args, **kwargs):
            tries = 0
            while tries < max_tries:
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    tries += 1
                    if tries == max_tries:
                        raise e
                    time.sleep(delay_seconds)
        return wrapper_retry
    return decorator_retry@retry(max_tries=5, delay_seconds=2)
def call_dummy_api():
    response = requests.get("https://jsonplaceholder.typicode.com/todos/1")
    return response
```

在上述代码中，我们尝试获取 API 响应。如果失败，我们重试相同的任务 5 次。每次重试之间，我们等待 2 秒。

## 2\. 缓存函数结果

我们的代码库中有些部分很少改变其行为。然而，它们可能会占用大量计算资源。在这种情况下，我们可以使用装饰器来缓存函数调用。

[](/poetry-to-complement-virtualenv-44088cc78fd1?source=post_page-----6913f717669a--------------------------------) ## 你还在使用 Virtualenv 吗？

### 有一种更好的方法来管理依赖关系、打包和发布 Python 项目。

[towardsdatascience.com

如果输入相同，函数只会运行一次。在每次后续运行中，结果将从缓存中获取。因此，我们不必一直进行昂贵的计算。

```py
def memoize(func):
    cache = {}
    def wrapper(*args):
        if args in cache:
            return cache[args]
        else:
            result = func(*args)
            cache[args] = result
            return result
    return wrapper
```

装饰器使用字典来存储函数参数和返回值。当我们执行这个函数时，装饰器会检查字典中的先前结果。实际函数只有在没有存储值时才会被调用。

以下是一个计算斐波那契数的函数。由于这是一个递归函数，相同的函数调用会被执行多次。但有了缓存，我们可以加速这个过程。

```py
@memoize
def fibonacci(n):
    if n <= 1:
        return n
    else:
        return fibonacci(n-1) + fibonacci(n-2)
```

以下是这个函数在有缓存和无缓存情况下的执行时间。请注意，缓存版本的运行时间只有毫秒的一小部分，而未缓存版本几乎花了一分钟。

```py
Function slow_fibonacci took 53.05560088157654 seconds to run.
Function fast_fibonacci took 7.772445678710938e-05 seconds to run.
```

使用字典来保存先前的执行数据是一种直接的方法。然而，还有一种更复杂的方式来存储缓存数据。你可以使用内存数据库，比如 Redis。

## 3\. 计时函数

这并不令人惊讶。当处理数据密集型函数时，我们渴望了解运行所需的时间。

通常的做法是收集两个时间戳，一个在函数开始时，一个在函数结束时。然后我们可以计算持续时间，并将其与返回值一起打印出来。

但对多个函数重复进行这项工作是很麻烦的。

相反，我们可以让装饰器来完成这件事。我们可以对任何需要打印持续时间的函数进行注解。

这是一个示例 Python 装饰器，当函数被调用时，它会打印运行时间：

```py
import time

def timing_decorator(func):
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"Function {func.__name__} took {end_time - start_time} seconds to run.")
        return result
    return wrapper
```

你可以使用这个装饰器来计时函数的执行时间：

```py
@timing_decorator
def my_function():
    # some code here
    time.sleep(1)  # simulate some time-consuming operation
    return
```

调用函数将打印运行所需的时间。

```py
my_function()

>>> Function my_function took 1.0019128322601318 seconds to run.
```

## 4\. 记录函数调用

这个装饰器很大程度上是对前一个装饰器的扩展。但它有一些特定的用途。

如果你遵循软件设计原则，你会欣赏单一职责原则。这本质上意味着每个函数将有其唯一的责任。

[](/plotly-dashboards-in-python-28a3bb83702c?source=post_page-----6913f717669a--------------------------------) ## 这是我如何仅用 Python 创建炫目仪表盘的方法。

### Plotly dash 应用程序是用 Python 构建生产级仪表盘的最快方式。

[

当你以这种方式设计你的代码时，你还会希望记录函数的执行信息。这就是日志记录装饰器派上用场的地方。

以下示例说明了这一点。

```py
import logging
import functools

logging.basicConfig(level=logging.INFO)

def log_execution(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        logging.info(f"Executing {func.__name__}")
        result = func(*args, **kwargs)
        logging.info(f"Finished executing {func.__name__}")
        return result
    return wrapper

@log_execution
def extract_data(source):
    # extract data from source
    data = ...

    return data

@log_execution
def transform_data(data):
    # transform data
    transformed_data = ...

    return transformed_data

@log_execution
def load_data(data, target):
    # load data into target
    ...

def main():
    # extract data
    data = extract_data(source)

    # transform data
    transformed_data = transform_data(data)

    # load data
    load_data(transformed_data, target)
```

上面的代码是一个简化版的 ETL 流水线。我们有三个独立的函数来处理提取、转换和加载。我们用我们的`log_execution`装饰器包装了它们。

现在，每当代码被执行时，你会看到类似这样的输出：

```py
INFO:root:Executing extract_data
INFO:root:Finished executing extract_data
INFO:root:Executing transform_data
INFO:root:Finished executing transform_data
INFO:root:Executing load_data
INFO:root:Finished executing load_data
```

我们也可以让执行时间在这个装饰器中打印出来。但我更喜欢将它们分开到不同的装饰器中。这样，我可以选择在函数中使用哪一个（或两个）。

以下是如何在一个函数上使用多个装饰器的方法。

```py
@log_execution
@timing_decorator
def my_function(x, y):
    time.sleep(1)
    return x + y
```

## 5\. 通知装饰器

最后，一个在生产系统中非常有用的装饰器是通知装饰器。

再次，即使经过多次重试，即使是经过良好测试的代码库也会失败。当这种情况发生时，我们需要通知某人以便快速采取行动。

如果你曾经构建过数据管道并希望它能永久稳定工作，这并不新鲜。

以下装饰器会在内部函数执行失败时发送一封电子邮件。在你的情况下，它不一定是电子邮件通知。你可以配置它发送 Teams/slack 通知。

```py
import smtplib
import traceback
from email.mime.text import MIMEText

def email_on_failure(sender_email, password, recipient_email):
    def decorator(func):
        def wrapper(*args, **kwargs):
            try:
                return func(*args, **kwargs)
            except Exception as e:
                # format the error message and traceback
                err_msg = f"Error: {str(e)}\n\nTraceback:\n{traceback.format_exc()}"

                # create the email message
                message = MIMEText(err_msg)
                message['Subject'] = f"{func.__name__} failed"
                message['From'] = sender_email
                message['To'] = recipient_email

                # send the email
                with smtplib.SMTP_SSL('smtp.gmail.com', 465) as smtp:
                    smtp.login(sender_email, password)
                    smtp.sendmail(sender_email, recipient_email, message.as_string())

                # re-raise the exception
                raise

        return wrapper

    return decorator

@email_on_failure(sender_email='your_email@gmail.com', password='your_password', recipient_email='recipient_email@gmail.com')
def my_function():
    # code that might fail
```

## 结论

装饰器是一种非常方便的方式来为我们的函数应用新行为。如果没有它们，将会有很多代码重复。

在这篇文章中，我讨论了我最常用的装饰器。你可以根据你的特定需求扩展这些装饰器。例如，你可以使用 Redis 服务器来存储缓存响应，而不是使用字典。这将使你对数据有更多的控制，比如持久性。或者，你可以调整代码，逐步增加重试装饰器中的等待时间。

在我所有的项目中，我都会使用这些装饰器的某个版本。尽管它们的行为略有不同，但这些是我经常使用装饰器的共同目标。

希望这篇文章对你有所帮助。

> 感谢阅读，朋友！如果你喜欢我的文章，请在 [**LinkedIn**](https://www.linkedin.com/in/thuwarakesh/)、[**Twitter**](https://twitter.com/Thuwarakesh) 和 [**Medium**](https://thuwarakesh.medium.com/) 上保持联系。
> 
> 还不是 Medium 的会员？请使用这个链接 [**成为会员**](https://thuwarakesh.medium.com/membership)，因为在不增加额外费用的情况下，我可以获得一小部分推荐佣金。
