# Python 中的增强调试：追溯信息获得重大升级

> 原文：[`towardsdatascience.com/enhanced-debugging-in-python-tracebacks-just-got-a-major-upgrade-bd77fb32db38?source=collection_archive---------13-----------------------#2023-03-15`](https://towardsdatascience.com/enhanced-debugging-in-python-tracebacks-just-got-a-major-upgrade-bd77fb32db38?source=collection_archive---------13-----------------------#2023-03-15)

## 如何通过位置增强的追溯信息来提升 Python 3.11 的调试体验

[](https://thomasdorfer.medium.com/?source=post_page-----bd77fb32db38--------------------------------)![Thomas A Dorfer](https://thomasdorfer.medium.com/?source=post_page-----bd77fb32db38--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bd77fb32db38--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd77fb32db38--------------------------------) [Thomas A Dorfer](https://thomasdorfer.medium.com/?source=post_page-----bd77fb32db38--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7c54f9b62b90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhanced-debugging-in-python-tracebacks-just-got-a-major-upgrade-bd77fb32db38&user=Thomas+A+Dorfer&userId=7c54f9b62b90&source=post_page-7c54f9b62b90----bd77fb32db38---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd77fb32db38--------------------------------) ·4 分钟阅读·2023 年 3 月 15 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbd77fb32db38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhanced-debugging-in-python-tracebacks-just-got-a-major-upgrade-bd77fb32db38&source=-----bd77fb32db38---------------------bookmark_footer-----------)![](img/b7116c89ccd139a962a51f3ddcada150.png)

图片来源于 [Mohamed Hassan](https://pixabay.com/users/mohamed_hassan-5229782/) 在 [Pixabay](https://pixabay.com/vectors/error-warning-computer-crash-6641731/)

在 Python 中，追溯信息是当异常发生时显示的报告，通常还会附带一个（希望能）帮助用户定位问题的错误消息。

直到现在，追溯信息仅显示了异常发生的行，但没有提供有关该行受影响部分的位置信息。

为了说明这一点，假设我们有一个包含一些随机数据摘要统计信息的字典对象。基于这些信息，我们想要计算置信区间的下界和上界。可以按照以下步骤进行：

```py
import numpy as np

stats = {'n': 50,
         'mean': 20,
         'std': 2.5,
         'z': None}

def ci(stats):
    lower = stats['mean'] - stats['z'] * (stats['std'] / np.sqrt(stats['n']))
    upper = stats['mean'] + stats['z'] * (stats['std'] / np.sqrt(stats['n']))

    return lower, upper

ci(stats)
```

我故意将字典对象中的*z*值赋为`None`以引发异常。对于低于 3.11 版本的 Python，跟踪信息将会是这样的：
