# 使用 Python 并行下载多个文件（或 URL）

> 原文：[https://towardsdatascience.com/use-python-to-download-multiple-files-or-urls-in-parallel-1759da9d6535?source=collection_archive---------1-----------------------#2023-09-08](https://towardsdatascience.com/use-python-to-download-multiple-files-or-urls-in-parallel-1759da9d6535?source=collection_archive---------1-----------------------#2023-09-08)

## 在更短时间内获取更多数据

[](https://khafen.medium.com/?source=post_page-----1759da9d6535--------------------------------)[![Konrad Hafen](../Images/146548d8de62ca1d5d15c3c7c3300940.png)](https://khafen.medium.com/?source=post_page-----1759da9d6535--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1759da9d6535--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1759da9d6535--------------------------------) [Konrad Hafen](https://khafen.medium.com/?source=post_page-----1759da9d6535--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F56d662f7324a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-python-to-download-multiple-files-or-urls-in-parallel-1759da9d6535&user=Konrad+Hafen&userId=56d662f7324a&source=post_page-56d662f7324a----1759da9d6535---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1759da9d6535--------------------------------) ·5 min read·2023年9月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1759da9d6535&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-python-to-download-multiple-files-or-urls-in-parallel-1759da9d6535&user=Konrad+Hafen&userId=56d662f7324a&source=-----1759da9d6535---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1759da9d6535&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-python-to-download-multiple-files-or-urls-in-parallel-1759da9d6535&source=-----1759da9d6535---------------------bookmark_footer-----------)![](../Images/0c55252812a33ed88ec684dce29b169d.png)

图片来源于 [Wesley Tingey](https://unsplash.com/@wesleyphotography?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我们生活在一个大数据的世界里。大数据通常以多个小数据集（即由多个文件组成的大数据集）的形式组织。获取这些数据通常会因为下载（或获取负担）而令人沮丧。幸运的是，通过一些代码，可以自动化并加快文件下载和获取的过程。

自动化文件下载可以节省大量时间。有几种方法可以使用 Python 自动化文件下载。下载文件的最简单方法是使用一个简单的 Python 循环来遍历 URL 列表。这种串行方法适用于少量小文件，但如果你要下载很多文件或大文件，你会希望使用并行方法来最大化计算资源。

使用并行文件下载例程，你可以更好地利用计算机资源来同时下载多个文件，从而节省时间。本教程演示了如何在 Python 中开发通用文件下载函数，并应用于串行和并行方式下载多个文件。本教程中的代码仅使用 Python 标准库中提供的模块，因此无需安装其他模块。

# 导入模块

对于这个示例，我们只需要 `requests` 和 `multiprocessing` Python 模块来并行下载文件。`requests` 和 `multiprocessing` 模块都可以从 Python 标准库中获取，因此你不需要进行任何安装。

我们还将导入 `time` 模块来跟踪下载单个文件所需的时间，并比较串行和并行下载例程之间的性能。`time` 模块也是 Python 标准库的一部分。

```py
import requests import time from multiprocessing import cpu_count from multiprocessing.pool import ThreadPool
```

# 定义 URL 和文件名

我将使用 [gridMET](https://www.climatologylab.org/gridmet.html) NetCDF 文件来演示如何在 Python 中并行下载文件，这些文件包含了美国的每日降水数据。

在这里，我在列表中指定了四个文件的 URL。在其他应用中，你可以通过编程方式生成一个要下载的文件列表。

```py
urls = ['https://www.northwestknowledge.net/metdata/data/pr_1979.nc', 'https://www.northwestknowledge.net/metdata/data/pr_1980.nc', 'https://www.northwestknowledge.net/metdata/data/pr_1981.nc', 'https://www.northwestknowledge.net/metdata/data/pr_1982.nc']
```

每个 URL 必须与其下载位置相关联。在这里，我将文件下载到 Windows 的‘Downloads’目录。我在列表中硬编码了文件名，以便简化和透明。根据你的应用程序，你可能想编写代码来解析输入的 URL 并将其下载到特定目录。

```py
fns = [r'C:\Users\konrad\Downloads\pr_1979.nc', r'C:\Users\konrad\Downloads\pr_1980.nc', r'C:\Users\konrad\Downloads\pr_1981.nc', r'C:\Users\konrad\Downloads\pr_1982.nc']
```

多进程要求并行函数只接受一个参数（有一些解决方法，但我们在这里不讨论）。为了下载一个文件，我们需要传递两个参数，一个是 URL，另一个是文件名。因此，我们将 `urls` 和 `fns` 列表打包成一个元组列表。列表中的每个元组将包含两个元素；一个 URL 和该 URL 的下载文件名。这样我们就可以传递一个包含两个信息的单一参数（元组）。

```py
inputs = zip(urls, fns)
```

# 下载 URL 的函数

现在我们已经指定了下载的 URL 以及它们关联的文件名，我们需要一个函数来下载这些 URL（`download_url`）。

我们将向 `download_url` 传递一个参数（`arg`）。这个参数将是一个可迭代对象（列表或元组），其中第一个元素是要下载的 URL（`url`），第二个元素是文件名（`fn`）。这些元素被分配给变量（`url` 和 `fn`）以提高可读性。

现在创建一个 try 语句，其中在文件创建后检索 URL 并将其写入文件。当文件写入时，将返回 URL 和下载时间。如果发生异常，则会打印一条消息。

`download_url` 函数是我们代码的核心。它完成实际的下载和文件创建工作。我们现在可以使用这个函数来串行（使用循环）和并行下载文件。让我们看一下这些示例。

```py
def download_url(args): 
  t0 = time.time() 
  url, fn = args[0], args[1] 
  try: 
    r = requests.get(url) 
    with open(fn, 'wb') as f: 
      f.write(r.content) 
      return(url, time.time() - t0) 
  except Exception as e: 
    print('Exception in download_url():', e)
```

# 使用 Python 循环下载多个文件

要将 URL 列表下载到相关文件，请遍历我们创建的可迭代对象（`inputs`），将每个元素传递给 `download_url`。每次下载完成后，我们将打印下载的 URL 及下载所需的时间。

下载所有 URL 的总时间将在所有下载完成后打印。

```py
t0 = time.time() 
for i in inputs: 
  result = download_url(i) 
  print('url:', result[0], 'time:', result[1]) 
  print('Total time:', time.time() - t0)
```

输出：

```py
url: https://www.northwestknowledge.net/metdata/data/pr_1979.nc time: 16.381176710128784 
url: https://www.northwestknowledge.net/metdata/data/pr_1980.nc time: 11.475878953933716 
url: https://www.northwestknowledge.net/metdata/data/pr_1981.nc time: 13.059367179870605
url: https://www.northwestknowledge.net/metdata/data/pr_1982.nc time: 12.232381582260132 
Total time: 53.15849542617798
```

下载单个文件的时间在 11 到 16 秒之间。总下载时间稍少于一分钟。你的下载时间将根据你特定的网络连接有所不同。

让我们将这种串行（循环）方法与下面的并行方法进行比较。

# 使用 Python 并行下载多个文件

首先，创建一个函数（`download_parallel`）来处理并行下载。该函数（`download_parallel`）将接受一个参数，一个包含 URL 和相关文件名的可迭代对象（我们之前创建的 `inputs` 变量）。

接下来，获取可用于处理的 CPU 数量。这将决定并行运行的线程数量。

现在使用 `multiprocessing` `ThreadPool` 将 `inputs` 映射到 `download_url` 函数。在这里我们使用 `ThreadPool` 的 `imap_unordered` 方法，并将 `download_url` 函数及其输入参数（`inputs` 变量）传递给它。`imap_unordered` 方法将同时运行指定线程数量的 `download_url`（即并行下载）。

因此，如果我们有四个文件和四个线程，则所有文件可以同时下载，而不是等待一个下载完成后再开始下一个。这可以节省大量处理时间。

在 `download_parallel` 函数的最后部分，将打印下载的 URL 及下载每个 URL 所需的时间。

```py
def download_parallel(args): 
  cpus = cpu_count() 
  results = ThreadPool(cpus - 1).imap_unordered(download_url, args) 
  for result in results: 
    print('url:', result[0], 'time (s):', result[1])
```

一旦定义了 `inputs` 和 `download_parallel`，就可以用一行代码并行下载文件。

```py
download_parallel(inputs)
```

输出：

```py
url: https://www.northwestknowledge.net/metdata/data/pr_1980.nc time (s): 14.641696214675903 
url: https://www.northwestknowledge.net/metdata/data/pr_1981.nc time (s): 14.789752960205078 
url: https://www.northwestknowledge.net/metdata/data/pr_1979.nc time (s): 15.052601337432861 
url: https://www.northwestknowledge.net/metdata/data/pr_1982.nc time (s): 23.287317752838135 
Total time: 23.32273244857788
```

请注意，这种方法下载每个单独文件所需的时间更长。这可能是由于网络速度变化，或将下载映射到各自线程所需的开销所致。尽管单独文件下载时间较长，但并行方法使总下载时间减少了50%。

你可以看到并行处理如何大大减少多个文件的处理时间。随着文件数量的增加，使用并行下载方法可以节省更多时间。

# 结论

在你的开发和分析流程中自动化文件下载可以节省大量时间。正如本教程所示，实现并行下载例程可以大大减少文件获取时间，特别是当你需要下载许多文件或大文件时。

*最初发布于* [*https://opensourceoptions.com*](https://opensourceoptions.com/blog/use-python-to-download-multiple-files-or-urls-in-parallel/)*.*
