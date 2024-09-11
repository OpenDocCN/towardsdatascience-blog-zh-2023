# 如何使用日志模块调试 Python 脚本

> 原文：[`towardsdatascience.com/how-to-troubleshoot-python-scripts-with-the-logging-module-aaf630c35fe3?source=collection_archive---------8-----------------------#2023-08-26`](https://towardsdatascience.com/how-to-troubleshoot-python-scripts-with-the-logging-module-aaf630c35fe3?source=collection_archive---------8-----------------------#2023-08-26)

## 打印语句只能帮助你到此为止……

[](https://medium.com/@aashishnair?source=post_page-----aaf630c35fe3--------------------------------)![Aashish Nair](https://medium.com/@aashishnair?source=post_page-----aaf630c35fe3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----aaf630c35fe3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----aaf630c35fe3--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----aaf630c35fe3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3087ba81e065&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-troubleshoot-python-scripts-with-the-logging-module-aaf630c35fe3&user=Aashish+Nair&userId=3087ba81e065&source=post_page-3087ba81e065----aaf630c35fe3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----aaf630c35fe3--------------------------------) ·7 分钟阅读·2023 年 8 月 26 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faaf630c35fe3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-troubleshoot-python-scripts-with-the-logging-module-aaf630c35fe3&user=Aashish+Nair&userId=3087ba81e065&source=-----aaf630c35fe3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faaf630c35fe3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-troubleshoot-python-scripts-with-the-logging-module-aaf630c35fe3&source=-----aaf630c35fe3---------------------bookmark_footer-----------)![](img/88282bab14f50541e0bba68c4d0cf0bc.png)

图片来自 Tima Miroshnichenko: [`www.pexels.com/photo/a-person-writing-on-a-notebook-5336909/`](https://www.pexels.com/photo/a-person-writing-on-a-notebook-5336909/)

## 目录

∘ 简介

∘ 日志模块

∘ 日志的级别

∘ 配置级别

∘ 配置调试级别

∘ 创建日志文件

∘ 格式化日志消息

∘ 主要要点

## 简介

考虑以下场景：你编写了一段代码，它要么返回错误，要么产生了意外的值。

```py
x1 = function1(x)
x2 = function2(x1)
x3 = function3(x2)
```

为了找到错误的代码行，你写一个打印语句…

```py
x1 = function1(x)
print(x1)
x2 = function2(x1)
x3 = function3(x2)
```

然后添加另一个打印语句…

```py
x1 = function1(x)
print(x1)
x2 = function2(x1)
print(x2)
x3 = function3(x2)
```

然后再跟上另一个打印语句。

```py
x1 = function1(x)
print(x1)
x2 = function2(x1)
print(x2)
x3 = function3(x2)
print(x3)
```
