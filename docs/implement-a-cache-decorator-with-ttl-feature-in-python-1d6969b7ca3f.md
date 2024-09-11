# 在Python中实现具有生存时间功能的缓存装饰器

> 原文：[https://towardsdatascience.com/implement-a-cache-decorator-with-ttl-feature-in-python-1d6969b7ca3f?source=collection_archive---------14-----------------------#2023-04-03](https://towardsdatascience.com/implement-a-cache-decorator-with-ttl-feature-in-python-1d6969b7ca3f?source=collection_archive---------14-----------------------#2023-04-03)

## 一个基于@functools.lru_cache的装饰器支持缓存过期

[](https://qtalen.medium.com/?source=post_page-----1d6969b7ca3f--------------------------------)[![Peng Qian](../Images/9ce9aeb381ec6b017c1ee5d4714937e2.png)](https://qtalen.medium.com/?source=post_page-----1d6969b7ca3f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1d6969b7ca3f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1d6969b7ca3f--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----1d6969b7ca3f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e2fe735546d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplement-a-cache-decorator-with-ttl-feature-in-python-1d6969b7ca3f&user=Peng+Qian&userId=8e2fe735546d&source=post_page-8e2fe735546d----1d6969b7ca3f---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----1d6969b7ca3f--------------------------------) ·5分钟阅读·2023年4月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1d6969b7ca3f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplement-a-cache-decorator-with-ttl-feature-in-python-1d6969b7ca3f&user=Peng+Qian&userId=8e2fe735546d&source=-----1d6969b7ca3f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1d6969b7ca3f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplement-a-cache-decorator-with-ttl-feature-in-python-1d6969b7ca3f&source=-----1d6969b7ca3f---------------------bookmark_footer-----------)![](../Images/1f1d5294aa688496b0d210d8cce05051.png)

图片由[Aron Visuals](https://unsplash.com/@aronvisuals?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# **问题：**

Python的`functools`包中的[lru_cache](https://docs.python.org/3/library/functools.html#functools.lru_cache)装饰器提供了基于LRU缓存的实现。使用这个装饰器，相同参数的函数在第二次执行时会显著更快。

然而，lru_cache 不能支持缓存过期。如果你希望缓存在一定时间后过期，以便下次函数调用时更新缓存，lru_cache 无法实现这一点。

# **解决方法：**

## **1\. 实现一个带 TTL 功能的 lru_cache**

因此，我实现了一个基于 lru_cache 的新装饰器。这个装饰器可以接受一个 ttl 参数。该参数可以接受一个以秒为单位的时间，当这个时间过期后，下一次函数调用将返回一个新值并刷新缓存。

对于那些急需解决问题的人，这里是源代码：
