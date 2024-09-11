# 使用 `lazy_static` 在运行时初始化 Rust 常量

> 原文：[https://towardsdatascience.com/initialize-rust-constants-at-runtime-with-lazy-static-e05b1df46c79?source=collection_archive---------6-----------------------#2023-08-05](https://towardsdatascience.com/initialize-rust-constants-at-runtime-with-lazy-static-e05b1df46c79?source=collection_archive---------6-----------------------#2023-08-05)

## 定义具有惰性初始化的非恒定静态变量

[](https://medium.com/@nic-obert?source=post_page-----e05b1df46c79--------------------------------)[![Nicholas Obert](../Images/d70330063c9edc2f63e53f62a78f82ec.png)](https://medium.com/@nic-obert?source=post_page-----e05b1df46c79--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e05b1df46c79--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e05b1df46c79--------------------------------) [Nicholas Obert](https://medium.com/@nic-obert?source=post_page-----e05b1df46c79--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Feeb884cbf705&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finitialize-rust-constants-at-runtime-with-lazy-static-e05b1df46c79&user=Nicholas+Obert&userId=eeb884cbf705&source=post_page-eeb884cbf705----e05b1df46c79---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e05b1df46c79--------------------------------) ·4 min read·2023年8月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe05b1df46c79&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finitialize-rust-constants-at-runtime-with-lazy-static-e05b1df46c79&user=Nicholas+Obert&userId=eeb884cbf705&source=-----e05b1df46c79---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe05b1df46c79&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finitialize-rust-constants-at-runtime-with-lazy-static-e05b1df46c79&source=-----e05b1df46c79---------------------bookmark_footer-----------)![](../Images/d3a48ff6643b48687887a2af35a5c63e.png)

图片由 [Christian Lue](https://unsplash.com/@christianlue?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在编程中，编译时初始化常量的好处不言而喻。这不仅减少了初始化开销，而且通过提前知道常量的值，使编译器更容易巧妙地优化你的代码。

然而，有时候，在编译时初始化每一个常量是不可能的，因为这需要执行非恒定操作或获取仅在运行时可用的数据。例如，假设我们在程序中重复使用数字`√7`。与其每次都计算它，不如像下面这样定义一个常量：

```py
const ROOT_OF_SEVEN: f64 = 7_f64.sqrt();
```

然而，这段代码是无效的。Rust 编译器返回以下错误：

```py
cannot call non-const fn `f64::<impl f64>::sqrt` in constants
calls in constants are limited to constant functions, tuple structs and tuple variants
```

如果我们尝试用环境变量初始化一个常量，也会出现相同的问题：

```py
const LANG: String = env::var("LANG").unwrap(); 
```

来自 Rust 编译器：

```py
cannot call…
```
