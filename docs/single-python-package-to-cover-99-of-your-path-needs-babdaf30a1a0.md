# 单个 Python 包覆盖 99% 的 Path 需求

> 原文：[`towardsdatascience.com/single-python-package-to-cover-99-of-your-path-needs-babdaf30a1a0?source=collection_archive---------21-----------------------#2023-01-12`](https://towardsdatascience.com/single-python-package-to-cover-99-of-your-path-needs-babdaf30a1a0?source=collection_archive---------21-----------------------#2023-01-12)

## Pathlib：你一直梦想的库

[](https://medium.com/@arli94?source=post_page-----babdaf30a1a0--------------------------------)![Arli](https://medium.com/@arli94?source=post_page-----babdaf30a1a0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----babdaf30a1a0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----babdaf30a1a0--------------------------------) [Arli](https://medium.com/@arli94?source=post_page-----babdaf30a1a0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9b9b5a558522&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsingle-python-package-to-cover-99-of-your-path-needs-babdaf30a1a0&user=Arli&userId=9b9b5a558522&source=post_page-9b9b5a558522----babdaf30a1a0---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----babdaf30a1a0--------------------------------) ·5 分钟阅读·2023 年 1 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbabdaf30a1a0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsingle-python-package-to-cover-99-of-your-path-needs-babdaf30a1a0&user=Arli&userId=9b9b5a558522&source=-----babdaf30a1a0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbabdaf30a1a0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsingle-python-package-to-cover-99-of-your-path-needs-babdaf30a1a0&source=-----babdaf30a1a0---------------------bookmark_footer-----------)

*如果你想亲自体验 Medium，考虑通过* [***注册会员***](https://medium.com/@arli94/membership)*来支持我和其他数千名作家。每月只需 5 美元，这对我们作家帮助很大，而且你可以访问 Medium 上所有精彩的故事。*

![](img/3c0a5685f8c3c20cb4214c4c801e2afd.png)

图片来源：[Alice Donovan Rouse](https://unsplash.com/@alicekat?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

操作路径是任何生产项目中的一个基本任务。你可能需要从符合特定模式的远程服务器加载文件，移动或存储管道中的不同处理文件和版本，或仅仅是读取或写入文件。

在所有这些步骤中，你都会遇到路径操作的问题。通常，你会发现自己反复搜索互联网以解决诸如以下常见问题的解决方案：

+   *如何从文件夹中获取所有文件？*

+   *如何检查文件夹是否存在？*

+   *如何在 Python 中创建文件夹？*

尽管这些问题的答案很容易找到，但你会发现整个管道中的代码不一致。代码的某一部分可能使用`os.path`，另一部分可能使用`shutil`，还有另一部分可能使用`glob`。

为了避免这种不一致性，我建议使用`pathlib`模块，它提供了一致的...
