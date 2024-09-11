# Julia 中的金属编程

> 原文：[`towardsdatascience.com/metal-programming-in-julia-2db5fe8ee32c?source=collection_archive---------4-----------------------#2023-12-04`](https://towardsdatascience.com/metal-programming-in-julia-2db5fe8ee32c?source=collection_archive---------4-----------------------#2023-12-04)

![](img/1b0fa479f9906bf1570e41eed0082af9.png)

稍重 | 作者图片

## 利用 macOS GPU 的 Metal.jl 框架的强大功能。

[](https://lausena.medium.com/?source=post_page-----2db5fe8ee32c--------------------------------)![Gabriel Sena](https://lausena.medium.com/?source=post_page-----2db5fe8ee32c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2db5fe8ee32c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2db5fe8ee32c--------------------------------) [Gabriel Sena](https://lausena.medium.com/?source=post_page-----2db5fe8ee32c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff2cc6e7e5fc6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmetal-programming-in-julia-2db5fe8ee32c&user=Gabriel+Sena&userId=f2cc6e7e5fc6&source=post_page-f2cc6e7e5fc6----2db5fe8ee32c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2db5fe8ee32c--------------------------------) ·11 分钟阅读·2023 年 12 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2db5fe8ee32c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmetal-programming-in-julia-2db5fe8ee32c&user=Gabriel+Sena&userId=f2cc6e7e5fc6&source=-----2db5fe8ee32c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2db5fe8ee32c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmetal-programming-in-julia-2db5fe8ee32c&source=-----2db5fe8ee32c---------------------bookmark_footer-----------)

## 介绍

就在去年，我们首次接触到了[Metal.jl](https://github.com/JuliaGPU/Metal.jl)框架，这是一个用于苹果硬件的 GPU 后端。这对希望充分发挥其 macOS M 系列芯片潜力的[Julia](https://julialang.org/)用户来说是个令人兴奋的消息。特别是数据科学家和机器学习工程师可以通过利用 GPU 的并行处理能力，加速他们的计算工作流程，从而缩短训练和推理时间。Metal.jl 的融入标志着 Julia 生态系统的重要推进，使得这门语言的能力能够与苹果平台上科学计算和机器学习领域的不断演变保持一致。

> 在[2020](https://www.apple.com/newsroom/2020/06/apple-announces-mac-transition-to-apple-silicon/)年，苹果开始将其 Mac 系列产品从基于 Intel 的处理器过渡到苹果自家硅芯片，从 M1 芯片开始。尽管这是苹果公司一个历史性的、令人印象深刻的成就，但也伴随着不少批评和问题。自从我拿到 32 核的 Mac Studio M1 芯片以来，我一直在尝试充分利用 GPU 并探索新的应用程序。我必须说，这并不是全程都很顺利。从[ARM](https://en.wikipedia.org/wiki/ARM_architecture_family)架构的兼容性问题到不支持的机器学习库，有时确实很难建立一个稳定的工作环境。这在任何…
