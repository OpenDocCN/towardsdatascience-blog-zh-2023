# Loguru：像打印一样简单，像日志记录一样灵活

> 原文：[https://towardsdatascience.com/loguru-simple-as-print-flexible-as-logging-c964467f64aa?source=collection_archive---------5-----------------------#2023-07-17](https://towardsdatascience.com/loguru-simple-as-print-flexible-as-logging-c964467f64aa?source=collection_archive---------5-----------------------#2023-07-17)

## 为你的数据科学项目提供简单的日志记录解决方案

[](https://khuyentran1476.medium.com/?source=post_page-----c964467f64aa--------------------------------)[![Khuyen Tran](../Images/98aa66025ad29b618e875c75f1c400a5.png)](https://khuyentran1476.medium.com/?source=post_page-----c964467f64aa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c964467f64aa--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c964467f64aa--------------------------------) [Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----c964467f64aa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F84a02493194a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Floguru-simple-as-print-flexible-as-logging-c964467f64aa&user=Khuyen+Tran&userId=84a02493194a&source=post_page-84a02493194a----c964467f64aa---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c964467f64aa--------------------------------) ·8分钟阅读·2023年7月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc964467f64aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Floguru-simple-as-print-flexible-as-logging-c964467f64aa&user=Khuyen+Tran&userId=84a02493194a&source=-----c964467f64aa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc964467f64aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Floguru-simple-as-print-flexible-as-logging-c964467f64aa&source=-----c964467f64aa---------------------bookmark_footer-----------)![](../Images/3829b994881a3b1be639cbae3c28152d.png)

作者提供的图片

*最初发表于* [*https://mathdatasimplified.com*](https://mathdatasimplified.com/2023/07/17/simplify-your-python-logging-with-loguru/) *2023年7月17日。*

# 为什么在数据科学项目中使用日志记录？

数据科学家经常使用打印函数来调试代码。然而，随着打印语句数量的增加，由于缺乏行号或函数名称，识别输出来源变得困难。

```py
def encode_data(data: list):
    print("Encode data")
    data_map = {'a': 1, 'b': 2, 'c': 3}
    print(f"Data map: {data_map}")
    return [data_map[num] for num in data]

def add_one(data: list):
    print("Add one")
    return [num + 1 for num in data]

def process_data(data: list):
    print("Process data")
    data = encode_data(data)
    print(f"Encoded data: {data}")
    data = add_one(data)
    print(f"Added one: {data}")

process_data(['a', 'a', 'c'])
```

输出：

```py
Process data
Encode data
Data map: {'a': 1, 'b': 2, 'c': 3}
Encoded data: [1, 1, 3]
Add one
Added one: [2, 2, 4]
```

当将代码投入生产环境时，手动检查并删除所有调试行可能是一个繁琐且容易出错的任务。
