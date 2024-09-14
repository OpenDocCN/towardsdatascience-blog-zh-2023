# 我的 Python 脚本如何更像自然对话

> 原文：[`towardsdatascience.com/python-code-like-natural-english-ff5cb09e97b9`](https://towardsdatascience.com/python-code-like-natural-english-ff5cb09e97b9)

## 管道是一种非常出色的技术，可以使代码更加人性化。

[](https://thuwarakesh.medium.com/?source=post_page-----ff5cb09e97b9--------------------------------)![Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----ff5cb09e97b9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ff5cb09e97b9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ff5cb09e97b9--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----ff5cb09e97b9--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ff5cb09e97b9--------------------------------) ·5 min 阅读·2023 年 3 月 22 日

--

![](img/5ac47c52ddaf312fc0a3540f558cc343.png)

图片由 [Pavel Danilyuk](https://www.pexels.com/photo/close-up-shot-of-white-toy-robot-on-blue-and-pink-background-8294666/) 提供，来自 Pexels。

你的代码也是你的文档。

人们说伟大的程序员不会给代码添加注释。他们相信，如果代码难以编写，其他人也应该很难理解和修改。因此，他们编写简单明了的代码。

虽然我并不主张完全不添加注释，但这句话中确实有一部分真理，我无法否认。代码应该能够被任何人阅读！

这就是为什么 SQL 代码很棒的原因。声明式语法比任何通用编程语言都要更具可读性。我们清楚地知道我们选择了什么，过滤条件，如何汇总等。

[](/5-advanced-sql-techniques-for-real-life-projects-f2db9b6680e2?source=post_page-----ff5cb09e97b9--------------------------------) ## 这 5 种 SQL 技术覆盖了 ~80% 的现实项目

### 加快你的 SQL 学习曲线。

towardsdatascience.com

我们能否组织我们的 Python 脚本以提高可读性？如果我们能使 Python 代码看起来更声明式，这会改善代码质量吗？而且会更有趣吗？

我一直在为我的一些项目尝试不同的代码风格。使用**管道操作符提高了可读性**到一个极致的水平。慢慢地，我将大部分代码库转换为利用这种技术。

但在深入了解管道之前，

# 什么让你的代码库难以阅读？

进化使我们在日常任务的各个方面创造了可接受的规范。我们可能没有意识到这些规范，但以不同方式进行会让我们和周围的人生活变得困难。

在编程中，我们也制定了可接受的规范。软件设计原则、模式和代码风格指南就是这些规范。

当我们在代码中遵循这些规范时，读者可以轻松理解我们代码的结构。如果不遵循这些规范，会给你和其他人带来困难。

如果这些是新的，可以查看 SOLID 设计原则和 PEP 8 风格指南。

在使用管道操作符重构代码时，**单一职责原则（SRP）**具有特别重要的意义。它指出我们代码的每个块（一个函数、一个类等）应该处理代码中的一个且仅有一个方面。

这是一个违反 SRP 的代码示例：

```py
import requests

def calculate_transaction_value(amount):
    response = requests.get('https://api.exchangeratesapi.io/latest?base=USD&symbols=EUR')
    exchange_rate = response.json()['rates']['EUR']
    transaction_value = amount * exchange_rate
    return transaction_value
```

上述代码查询 API 以获取最新的汇率，并计算以美元为单位的交易值。虽然这个函数很简单，但它处理了两个方面——从外部 API 获取汇率和计算值。

这种方法存在几个实际问题。由于这不是文章的范围，我将为另一篇文章保存这些问题。但最重要的是，我们可以通过引入模块化来提高代码的可读性。

这是应用 SRP 后的代码（仍未应用其他原则）。

```py
import os
import requests

def get_exchange_rate():
    url = "https://api.apilayer.com/exchangerates_data/latest?symbols=EUR&base=USD"

    payload = {}
    headers = {"apikey": os.environ["API_KEY"]}

    response = requests.request("GET", url, headers=headers, data=payload)
    result = response.json()["rates"]["EUR"]
    return result

def calculate_transaction_value(exchange_rate, amount):
    transaction_value = amount * exchange_rate
    return transaction_value
```

这是你在代码中使用它的方式：

```py
exchange_rate = get_exchange_rate()
transaction_value = calculate_transaction_value(100, exchange_rate)
print(f'Transaction value in EUR: {transaction_value}')

>> Transaction value in EUR: 93.71199999999999
```

现在，任何人都可以轻松阅读这些步骤并理解它们。主要的代码块非常简单。而且每当他们需要更多细节时，可以查看函数定义。

然而，当代码库变得庞大时，这在大多数实际项目中是如此，跟踪所有创建的变量并跟随进展是困难的。

这时管道就派上用场了。

## 管道提升我们代码库的可读性。

如果你使用 shell 命令，其中一个惊人的地方是我们可以将一个操作的输出传递给下一个操作。这为指令提供了逻辑流程。

我们还可以重构我们的 Python 代码，使其具有逻辑流程。我们可以使用 `pipe` 包来做到这一点。我们可以从 PyPI 安装它：

```py
pip install pipe
```

我们现在可以将函数转换为管道操作符。我们只需用管道装饰器标注函数即可。

[](/python-decorators-for-data-science-6913f717669a?source=post_page-----ff5cb09e97b9--------------------------------) ## 我在几乎所有数据科学项目中使用的 5 个 Python 装饰器

### 装饰器提供了一种新的便利方式，从缓存到发送通知都可以使用它们。

[towardsdatascience.com

```py
import os
import requests
from pipe import Pipe

def get_exchange_rate():
    url = "https://api.apilayer.com/exchangerates_data/latest?symbols=EUR&base=USD"

    payload = {}
    headers = {"apikey": os.environ["API_KEY"]}

    response = requests.request("GET", url, headers=headers, data=payload)
    result = response.json()["rates"]["EUR"]
    return result

@Pipe
def calculate_transaction_value(exchange_rate, amount):
    transaction_value = amount * exchange_rate
    return transaction_value
```

这是我们如何按顺序组织和调用函数的示例。

```py
transaction_value = (
    get_exchange_rate() 
    | calculate_transaction_value(100)
)
```

在上述示例中，请注意管道操作会自动从前一个操作中提取函数的第一个参数。我们并没有将`exchange_rate`参数传递给`calculate_transaction_value`函数。相反，我们只传递了金额。

我有意将这个示例保持简单。但在实际项目中，你可能会遇到更长的构造。

这是我最近项目中修改的摘录（仍然是简化版）。

```py
sales_regional_lead_data = (
    get_sales_leads(region="EMEA")
    | create_placeholder_dataframe(
        years=3
    )  # Create a dataset replicating each sales lead for 12 months x years
    | merge_budgets_to_sales_leads(
        get_project_budgets() | aggregate_project_budgets_to_sales_leads()
    )
    | merge_crm_data_to_sales_leads(
        get_crm_data() | aggregate_crm_data_to_sales_leads()
    )
    | merge_invoice_data_to_sales_leads(
        get_project_invoices() | aggregate_invoices_to_sales_leads()
    )
    | merge_work_in_progress_to_sales_leads(
        get_project_work_in_progress() | aggregate_work_in_progress_to_sales_leads()
    )
    | recognize_partial_invoices(finished_pct_cutoff=.8)
    | compute_sales_for_each_sales_lead()
    | compute_delivery_for_each_sales_lead()
    | compute_margin_for_each_sales_lead()
    | compute_average_margin_for_each_sales_lead()
    | compute_average_delivery_for_each_sales_lead()
    | compute_average_sales_for_each_sales_lead()
    | load_sales_leads_to_database(
        db_config, table_name="sales_leads", if_exists="append"
    )
)
```

这种代码版本更容易理解，因为它更接近自然对话。任何人都可以理解每个步骤及其改变行为的参数。即使是非技术人员的利益相关者也能愉快地将其作为小说阅读。

组织代码的最佳方法是将其拆分为模块并保存在单独的文件中。然后，这些文件在文件系统中以逻辑文件夹结构进行组织。

## 7 种方法使你的 Python 项目结构更优雅

### 以下是可管理、可扩展且易于理解的 Python 项目结构的最佳实践。

towardsdatascience.com

因此，只有这段代码存在于模块的`__init__.py`文件中。我从子模块中导入函数。因此，文件夹结构大致如下：

```py
sales/
├── __init__.py
├── crm_data.py
├── invoices.py
├── budgets.py
├── work_in_progress.py
├── sales_leads.py
├── matrices.py
├── db.py
```

## 结论

作为程序员，我们的首要任务往往是让事情运作起来。但如果这是唯一的目标，我们就会错过重点。

我们的代码应该足够容易阅读和理解，让其他人无需我们的帮助或不断查看文档页面。

尝试了几种技术后，给我留下深刻印象的是管道操作。本文主要讲述为什么我认为这种操作很棒。但这里并没有涵盖管道操作的全部范围。

我已经写了一篇关于管道操作的全面文章。请查看以获取更多使用方法。

## 在 Python 中使用管道操作以获得更可读和更快的编码

### 一个方便的 Python 包，可以节省大量编码时间，并通过类 Unix 风格的管道操作提高可读性。

towardsdatascience.com

> 感谢阅读，朋友！在 [**LinkedIn**](https://www.linkedin.com/in/thuwarakesh/)、[**Twitter**](https://twitter.com/Thuwarakesh) 和 [**Medium**](https://thuwarakesh.medium.com/) 上和我打个招呼吧。
> 
> 还不是 Medium 会员？请使用这个链接来 [**成为会员**](https://thuwarakesh.medium.com/membership)，因为这样你不会增加额外费用，我将因推荐你而获得少量佣金。
