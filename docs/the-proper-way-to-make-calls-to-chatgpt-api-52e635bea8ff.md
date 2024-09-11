# 正确调用 ChatGPT API 的方法

> 原文：[https://towardsdatascience.com/the-proper-way-to-make-calls-to-chatgpt-api-52e635bea8ff?source=collection_archive---------0-----------------------#2023-07-15](https://towardsdatascience.com/the-proper-way-to-make-calls-to-chatgpt-api-52e635bea8ff?source=collection_archive---------0-----------------------#2023-07-15)

## 如何可靠地调用 ChatGPT API 以构建稳健的应用程序

[![Lucas Oliveira](../Images/e303751fcafde01db6d1182320a50797.png)](https://lucolivi.medium.com/?source=post_page-----52e635bea8ff--------------------------------) [![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----52e635bea8ff--------------------------------) [Lucas Oliveira](https://lucolivi.medium.com/?source=post_page-----52e635bea8ff--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb9eafd3d6f21&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-proper-way-to-make-calls-to-chatgpt-api-52e635bea8ff&user=Lucas+Oliveira&userId=b9eafd3d6f21&source=post_page-b9eafd3d6f21----52e635bea8ff---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----52e635bea8ff--------------------------------) · 11 分钟阅读 · 2023年7月15日

--

[收藏](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F52e635bea8ff&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-proper-way-to-make-calls-to-chatgpt-api-52e635bea8ff&source=-----52e635bea8ff---------------------bookmark_footer-----------)

现在 LLM 随处可见，尤其是 ChatGPT。正在基于它构建大量应用程序，如果你还没尝试过，应该试试看。

![](../Images/054928a295cef1538fd969eea16d31b4.png)

由 Midjourney 创建。

在构建基于 ChatGPT 的应用程序时，通常需要进行多个并行调用。不幸的是，你不是唯一一个需要这样做的人。由于许多应用程序每天处理数百万个请求（顺便说一句，给他们的工程团队点赞），API 经常会返回“请求过多”的错误。因此，我们需要一种有效的方法来处理这种错误，同时进行多个并行调用。

在这个简短的 Python 教程中，我们将涵盖两个重要主题，以有效地调用 ChatGPT API：

1.  并行执行多个调用

1.  在调用失败时重试调用

# 1\. 并行执行多个调用

执行调用的最简单方法是同步执行，也就是发送请求并等待响应到达以继续程序。我们可以简单地这样做：

```py
import requests

headers = {
    "Content-Type": "application/json",
    "Authorization": f"Bearer {OPENAI_API_KEY}"
}

response_json = requests.post("https://api.openai.com/v1/chat/completions", headers=headers, json={
    "model": "gpt-3.5-turbo",
    "messages": [{"role": "user", "content": "ping"}],
    "temperature": 0
}).json()

print(response_json["choices"][0]["message"]["content"])
```

```py
Pong!
```

如果我们在一个简单的系统中工作，这样做就可以了，但是，如果我们希望并行地对API或其他资源（如数据库）执行多个调用，我们可以使用异步方式获得更快的响应。

异步执行任务将触发每个操作并等待它们并行完成，从而减少等待时间。

实现这一点的基本方法是创建不同的线程来处理每个请求，然而，有一种更好的方法可以做到这一点[使用异步调用](https://realpython.com/async-io-python/)。

使用异步调用通常更高效，因为您可以指定应用程序应等待的确切位置，而在传统的线程处理中，系统将自动使线程等待，这可能是次优的。

下面我们展示了使用同步和异步调用之间的差异示例。

```py
# Sync call

import time

def delay_print(msg):
    print(msg, end=" ")
    time.sleep(1)

def sync_print():
    for i in range(10):
        delay_print(i)

start_time = time.time()
sync_print()
print("\n", time.time() - start_time, "seconds.")
```

```py
0 1 2 3 4 5 6 7 8 9 
 10.019574642181396 seconds.
```

```py
#Async Call

import asyncio

async def delay_print_async(msg):
    print(msg, end=" ")
    await asyncio.sleep(1)

async def async_print():
    asyncio.gather(*[delay_print_async(i) for i in range(10)])

start_time = time.time()
await async_print()
print("\n", time.time() - start_time, "seconds.")
```

```py
0.0002448558807373047 seconds.
0 1 2 3 4 5 6 7 8 9 
```

`asyncio.gather`方法将触发所有传递给它的异步调用，并在它们准备就绪后返回它们的结果。

遗憾的是，使用`requests`库进行异步调用是不可能的。您可以使用`aiohttp`库来实现。下面是使用`aiohttp`进行异步调用的示例。

```py
import aiohttp

async def get_completion(content):
    async with aiohttp.ClientSession() as session:
        async with session.post("https://api.openai.com/v1/chat/completions", headers=headers, json={
            "model": "gpt-3.5-turbo",
            "messages": [{"role": "user", "content": content}],
            "temperature": 0
        }) as resp:

            response_json = await resp.json()
            return response_json["choices"][0]['message']["content"]

await get_completion("Ping")
```

```py
Pong!
```

如前所述，要执行异步请求，我们需要使用`asyncio.gather`方法。

```py
async def get_completion_list(content_list):
    return await asyncio.gather(*[get_completion(content) for content in content_list])

await get_completion_list(["ping", "pong"]*5)
```

```py
['Pong!',
 'Ping!',
 'Pong!',
 'Ping!',
 'Pong!',
 'Ping!',
 'Pong!',
 'Ping!',
 'Pong!',
 'Ping!']
```

虽然这种方法有效，但是这样做调用不理想，因为我们为每次调用重新创建会话对象。我们可以通过以下方式重复使用同一个会话对象来节省资源和时间：

```py
async def get_completion(content, session):
    async with session.post("https://api.openai.com/v1/chat/completions", headers=headers, json={
        "model": "gpt-3.5-turbo",
        "messages": [{"role": "user", "content": content}],
        "temperature": 0
    }) as resp:

        response_json = await resp.json()
        return response_json["choices"][0]['message']["content"]

async def get_completion_list(content_list):
    async with aiohttp.ClientSession() as session:
        return await asyncio.gather(*[get_completion(content, session) for content in content_list])

await get_completion_list(["ping", "pong"]*5)
```

简单吧？通过这种方式，您可以轻松执行多个调用。然而，一个问题是通常不建议通过这种方式执行无限次调用，因为这样可能会过载系统，并且会受到惩罚，导致在某段时间内无法执行额外的请求（[相信我，您会](https://platform.openai.com/docs/guides/rate-limits)）。因此，建议限制同时执行的调用数量是一个好主意。您可以通过`asyncio.Semaphore`类轻松实现这一点。

`Semaphore`类创建了一个上下文管理器，用于管理其上下文中当前正在执行的异步调用的数量。如果达到最大数量，它将阻塞直到某些调用完成。

```py
async def get_completion(content, session, semaphore):
    async with semaphore:

        await asyncio.sleep(1)

        async with session.post("https://api.openai.com/v1/chat/completions", headers=headers, json={
            "model": "gpt-3.5-turbo",
            "messages": [{"role": "user", "content": content}],
            "temperature": 0
        }) as resp:

            response_json = await resp.json()
            return response_json["choices"][0]['message']["content"]

async def get_completion_list(content_list, max_parallel_calls):
    semaphore = asyncio.Semaphore(value=max_parallel_calls)

    async with aiohttp.ClientSession() as session:
        return await asyncio.gather(*[get_completion(content, session, semaphore) for content in content_list])

start_time = time.perf_counter()
completion_list = await get_completion_list(["ping", "pong"]*5, 100)
print("Time elapsed: ", time.perf_counter() - start_time, "seconds.")
print(completion_list)
```

```py
Time elapsed:  1.8094507199984946 seconds.
['Pong!', 'Ping!', 'Pong!', 'Ping!', 'Pong!', 'Ping!', 'Pong!', 'Ping!', 'Pong!', 'Ping!']
```

这里的一个可选事项是报告调用进度如何。您可以通过创建一个小类来实现这一点，该类将保存进度并在所有调用之间共享。您可以像下面这样做：

```py
class ProgressLog:
    def __init__(self, total):
        self.total = total
        self.done = 0

    def increment(self):
        self.done = self.done + 1

    def __repr__(self):
        return f"Done runs {self.done}/{self.total}."

async def get_completion(content, session, semaphore, progress_log):
    async with semaphore:

        await asyncio.sleep(1)

        async with session.post("https://api.openai.com/v1/chat/completions", headers=headers, json={
            "model": "gpt-3.5-turbo",
            "messages": [{"role": "user", "content": content}],
            "temperature": 0
        }) as resp:

            response_json = await resp.json()

            progress_log.increment()
            print(progress_log)

            return response_json["choices"][0]['message']["content"]

async def get_completion_list(content_list, max_parallel_calls):
    semaphore = asyncio.Semaphore(value=max_parallel_calls)
    progress_log = ProgressLog(len(content_list))

    async with aiohttp.ClientSession() as session:
        return await asyncio.gather(*[get_completion(content, session, semaphore, progress_log) for content in content_list])

start_time = time.perf_counter()
completion_list = await get_completion_list(["ping", "pong"]*5, 100)
print("Time elapsed: ", time.perf_counter() - start_time, "seconds.")
print(completion_list)
```

```py
Done runs 1/10.
Done runs 2/10.
Done runs 3/10.
Done runs 4/10.
Done runs 5/10.
Done runs 6/10.
Done runs 7/10.
Done runs 8/10.
Done runs 9/10.
Done runs 10/10.
Time elapsed:  1.755018908999773 seconds.
['Pong!', 'Ping!', 'Pong!', 'Ping!', 'Pong!', 'Ping!', 'Pong!', 'Ping!', 'Pong!', 'Ping!']
```

关于如何执行多个异步请求，这一节就到此为止。通过这种方式，您可以执行多个异步调用，限制每次调用的数量，并报告进度。然而，仍然有一些问题需要处理。

发出的请求可能由于服务器过载、连接中断、请求不当等多种原因而失败。这些可能会生成异常或返回不可预测的响应，因此我们需要处理这些情况，并自动重试失败的调用。

# 2\. 失败时重试调用

为了处理失败的调用，我们将使用`tenacity`库。Tenacity 提供了函数装饰器，可以在函数调用生成异常时自动重试。

```py
from tenacity import (
    retry,
    stop_after_attempt,
    wait_random_exponential,
)
```

为了给我们的调用提供重试功能，我们需要添加`@retry`装饰器。使用它而不添加额外参数将使函数在失败后立即并无限期地重试。这在某些情况下是不好的。

其中一个原因是我们的函数调用可能因服务器过载而失败，这使得在重新尝试之前等待一段时间是合理的。为了指示等待时间，我们将使用指数退避的方法，通过参数`wait=wait_random_exponential(min=min_value, max=max_value)`。这将使等待时间随着函数失败的次数增加。

一个可选的操作是每次重试时记录消息。我们可以通过提供某个函数给参数`before_sleep`来实现这一点。这里我们将使用`print`函数，但更好的方法是使用`logging`模块，并将`logging.error`或`logging.debug`函数传递给该参数。

为了演示，我们将生成随机异常。

```py
import random

class ProgressLog:
    def __init__(self, total):
        self.total = total
        self.done = 0

    def increment(self):
        self.done = self.done + 1

    def __repr__(self):
        return f"Done runs {self.done}/{self.total}."

@retry(wait=wait_random_exponential(min=1, max=60), before_sleep=print)
async def get_completion(content, session, semaphore, progress_log):
    async with semaphore:

        #await asyncio.sleep(1)
        if random.random() < 0.2:
            raise Exception("Random exception")

        async with session.post("https://api.openai.com/v1/chat/completions", headers=headers, json={
            "model": "gpt-3.5-turbo",
            "messages": [{"role": "user", "content": content}],
            "temperature": 0
        }) as resp:

            response_json = await resp.json()

            progress_log.increment()
            print(progress_log)

            return response_json["choices"][0]['message']["content"]

async def get_completion_list(content_list, max_parallel_calls):
    semaphore = asyncio.Semaphore(value=max_parallel_calls)
    progress_log = ProgressLog(len(content_list))

    async with aiohttp.ClientSession() as session:
        return await asyncio.gather(*[get_completion(content, session, semaphore, progress_log) for content in content_list])

start_time = time.perf_counter()
completion_list = await get_completion_list(["ping", "pong"]*5, 100)
print("Time elapsed: ", time.perf_counter() - start_time, "seconds.")
print(completion_list)
```

```py
<RetryCallState 133364377433616: attempt #1; slept for 0.74; last result: failed (Exception Random exception)>
<RetryCallState 133364377424496: attempt #1; slept for 0.79; last result: failed (Exception Random exception)>
Done runs 1/10.
Done runs 2/10.
Done runs 3/10.
Done runs 4/10.
Done runs 5/10.
Done runs 6/10.
Done runs 7/10.
Done runs 8/10.
Done runs 9/10.
Done runs 10/10.
Time elapsed:  1.1305301820011664 seconds.
['Pong!', 'Ping!', 'Pong!', 'Ping!', 'Pong!', 'Ping!', 'Pong!', 'Ping!', 'Pong!', 'Ping!']
```

这将使我们的函数在重试之前等待一段时间。然而，失败的原因可能是系统性的，例如服务器停机或错误的有效负载。在这种情况下，我们希望将重试次数限制在一个范围内。我们可以通过参数`stop=stop_after_attempt(n)`来实现。

```py
import random

class ProgressLog:
    def __init__(self, total):
        self.total = total
        self.done = 0

    def increment(self):
        self.done = self.done + 1

    def __repr__(self):
        return f"Done runs {self.done}/{self.total}."

@retry(wait=wait_random_exponential(min=1, max=60), stop=stop_after_attempt(2), before_sleep=print)
async def get_completion(content, session, semaphore, progress_log):
    async with semaphore:

        #await asyncio.sleep(1)
        if random.random() < 0.9:
            raise Exception("Random exception")

        async with session.post("https://api.openai.com/v1/chat/completions", headers=headers, json={
            "model": "gpt-3.5-turbo",
            "messages": [{"role": "user", "content": content}],
            "temperature": 0
        }) as resp:

            response_json = await resp.json()

            progress_log.increment()
            print(progress_log)

            return response_json["choices"][0]['message']["content"]

async def get_completion_list(content_list, max_parallel_calls):
    semaphore = asyncio.Semaphore(value=max_parallel_calls)
    progress_log = ProgressLog(len(content_list))

    async with aiohttp.ClientSession() as session:
        return await asyncio.gather(*[get_completion(content, session, semaphore, progress_log) for content in content_list])

start_time = time.perf_counter()
completion_list = await get_completion_list(["ping", "pong"]*5, 100)
print("Time elapsed: ", time.perf_counter() - start_time, "seconds.")
print(completion_list)
```

```py
<RetryCallState 133364608660048: attempt #1; slept for 0.1; last result: failed (Exception Random exception)>
<RetryCallState 133364377435680: attempt #1; slept for 0.71; last result: failed (Exception Random exception)>
<RetryCallState 133364377421472: attempt #1; slept for 0.17; last result: failed (Exception Random exception)>
<RetryCallState 133364377424256: attempt #1; slept for 0.37; last result: failed (Exception Random exception)>
<RetryCallState 133364377430928: attempt #1; slept for 0.87; last result: failed (Exception Random exception)>
<RetryCallState 133364377420752: attempt #1; slept for 0.42; last result: failed (Exception Random exception)>
<RetryCallState 133364377422576: attempt #1; slept for 0.47; last result: failed (Exception Random exception)>
<RetryCallState 133364377431312: attempt #1; slept for 0.11; last result: failed (Exception Random exception)>
<RetryCallState 133364377425840: attempt #1; slept for 0.69; last result: failed (Exception Random exception)>
<RetryCallState 133364377424592: attempt #1; slept for 0.89; last result: failed (Exception Random exception)>
---------------------------------------------------------------------------
Exception                                 Traceback (most recent call last)
/usr/local/lib/python3.10/dist-packages/tenacity/_asyncio.py in __call__(self, fn, *args, **kwargs)
     49                 try:
---> 50                     result = await fn(*args, **kwargs)
     51                 except BaseException:  # noqa: B902

5 frames
Exception: Random exception

The above exception was the direct cause of the following exception:

RetryError                                Traceback (most recent call last)
/usr/local/lib/python3.10/dist-packages/tenacity/__init__.py in iter(self, retry_state)
    324             if self.reraise:
    325                 raise retry_exc.reraise()
--> 326             raise retry_exc from fut.exception()
    327 
    328         if self.wait:

RetryError: RetryError[<Future at 0x794b5057a590 state=finished raised Exception>]
```

设置此参数后，一旦尝试次数达到最大值，将引发`RetryError`。然而，可能有时我们希望继续运行而不生成异常，只是将`None`值保存到调用返回中以便稍后处理。为此，我们可以使用回调函数`retry_error_callback`在发生`RetryError`错误时仅返回`None`值：

```py
import random

class ProgressLog:
    def __init__(self, total):
        self.total = total
        self.done = 0

    def increment(self):
        self.done = self.done + 1

    def __repr__(self):
        return f"Done runs {self.done}/{self.total}."

@retry(wait=wait_random_exponential(min=1, max=60), stop=stop_after_attempt(2), before_sleep=print, retry_error_callback=lambda _: None)
async def get_completion(content, session, semaphore, progress_log):
    async with semaphore:

        #await asyncio.sleep(1)
        if random.random() < 0.7:
            raise Exception("Random exception")

        async with session.post("https://api.openai.com/v1/chat/completions", headers=headers, json={
            "model": "gpt-3.5-turbo",
            "messages": [{"role": "user", "content": content}],
            "temperature": 0
        }) as resp:

            response_json = await resp.json()

            progress_log.increment()
            print(progress_log)

            return response_json["choices"][0]['message']["content"]

async def get_completion_list(content_list, max_parallel_calls):
    semaphore = asyncio.Semaphore(value=max_parallel_calls)
    progress_log = ProgressLog(len(content_list))

    async with aiohttp.ClientSession(timeout=aiohttp.ClientTimeout(1)) as session:
        return await asyncio.gather(*[get_completion(content, session, semaphore, progress_log) for content in content_list])

start_time = time.perf_counter()
completion_list = await get_completion_list(["ping", "pong"]*5, 100)
print("Time elapsed: ", time.perf_counter() - start_time, "seconds.")
print(completion_list)
```

```py
<RetryCallState 133364377805024: attempt #1; slept for 0.22; last result: failed (Exception Random exception)>
<RetryCallState 133364377799456: attempt #1; slept for 0.53; last result: failed (Exception Random exception)>
<RetryCallState 133364377801328: attempt #1; slept for 0.24; last result: failed (Exception Random exception)>
<RetryCallState 133364377810208: attempt #1; slept for 0.38; last result: failed (Exception Random exception)>
<RetryCallState 133364377801616: attempt #1; slept for 0.54; last result: failed (Exception Random exception)>
<RetryCallState 133364377422096: attempt #1; slept for 0.59; last result: failed (Exception Random exception)>
<RetryCallState 133364377430592: attempt #1; slept for 0.07; last result: failed (Exception Random exception)>
<RetryCallState 133364377425648: attempt #1; slept for 0.05; last result: failed (Exception Random exception)>
Done runs 1/10.
Done runs 2/10.
Done runs 3/10.
Time elapsed:  2.6409040250000544 seconds.
['Pong!', 'Ping!', None, None, None, None, None, 'Ping!', None, None]
```

这样，`None`值将被返回，而不是生成错误。

另一个尚未处理的问题是连接挂起问题。当我们执行请求时，由于某种原因，主机保持连接但既没有失败也没有返回任何内容，这种情况会发生。为了处理这种情况，我们需要设置超时，以便在调用在指定时间内未返回值时返回。为此，我们可以使用`aiohttp`库中的`timeout`参数，以及`aiohttp.ClientTimeout`类。如果发生超时，将引发`TimeoutError`，然后由`tenacity`的`retry`装饰器处理，并自动再次运行函数。

```py
class ProgressLog:
    def __init__(self, total):
        self.total = total
        self.done = 0

    def increment(self):
        self.done = self.done + 1

    def __repr__(self):
        return f"Done runs {self.done}/{self.total}."

@retry(wait=wait_random_exponential(min=1, max=60), stop=stop_after_attempt(20), before_sleep=print, retry_error_callback=lambda _: None)
async def get_completion(content, session, semaphore, progress_log):
    async with semaphore:

        async with session.post("https://api.openai.com/v1/chat/completions", headers=headers, json={
            "model": "gpt-3.5-turbo",
            "messages": [{"role": "user", "content": content}],
            "temperature": 0
        }) as resp:

            response_json = await resp.json()

            progress_log.increment()
            print(progress_log)

            return response_json["choices"][0]['message']["content"]

async def get_completion_list(content_list, max_parallel_calls):
    semaphore = asyncio.Semaphore(value=max_parallel_calls)
    progress_log = ProgressLog(len(content_list))

    async with aiohttp.ClientSession(timeout=aiohttp.ClientTimeout(10)) as session:
        return await asyncio.gather(*[get_completion(content, session, semaphore, progress_log) for content in content_list])

start_time = time.perf_counter()
completion_list = await get_completion_list(["ping", "pong"]*100, 100)
print("Time elapsed: ", time.perf_counter() - start_time, "seconds.")
```

```py
<RetryCallState 133364375201936: attempt #1; slept for 0.57; last result: failed (TimeoutError )>
Time elapsed:  12.705538211999738 seconds.
```

太棒了！现在我们有了一种强大的方法来运行多个并行请求，发生失败时会自动重试，并在失败是系统性时返回 `None` 值。所以最终的代码如下：

```py
import asyncio
import aiohttp
from tenacity import (
    retry,
    stop_after_attempt,
    wait_random_exponential,
)

headers = {
    "Content-Type": "application/json",
    "Authorization": f"Bearer {OPENAI_API_KEY}"
}

class ProgressLog:
    def __init__(self, total):
        self.total = total
        self.done = 0

    def increment(self):
        self.done = self.done + 1

    def __repr__(self):
        return f"Done runs {self.done}/{self.total}."

@retry(wait=wait_random_exponential(min=1, max=60), stop=stop_after_attempt(20), before_sleep=print, retry_error_callback=lambda _: None)
async def get_completion(content, session, semaphore, progress_log):
    async with semaphore:

        async with session.post("https://api.openai.com/v1/chat/completions", headers=headers, json={
            "model": "gpt-3.5-turbo",
            "messages": [{"role": "user", "content": content}],
            "temperature": 0
        }) as resp:

            response_json = await resp.json()

            progress_log.increment()
            print(progress_log)

            return response_json["choices"][0]['message']["content"]

async def get_completion_list(content_list, max_parallel_calls, timeout):
    semaphore = asyncio.Semaphore(value=max_parallel_calls)
    progress_log = ProgressLog(len(content_list))

    async with aiohttp.ClientSession(timeout=aiohttp.ClientTimeout(timeout)) as session:
        return await asyncio.gather(*[get_completion(content, session, semaphore, progress_log) for content in content_list])
```

总结来说，我们实现了以下功能：

1.  异步调用以减少等待时间。

1.  记录异步调用的进度。

1.  当调用失败时，自动触发重试。

1.  如果失败是系统性的，则返回 None 值。

1.  当调用超时且没有返回任何内容时，重试调用。

如果你有任何问题、发现错误或有改进的建议，请在下面留下评论！
