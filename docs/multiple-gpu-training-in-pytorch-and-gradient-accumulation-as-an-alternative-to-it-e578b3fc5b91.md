# **PyTorch 中的多 GPU 训练及梯度累积作为其替代方案**

> 原文：[`towardsdatascience.com/multiple-gpu-training-in-pytorch-and-gradient-accumulation-as-an-alternative-to-it-e578b3fc5b91?source=collection_archive---------10-----------------------#2023-07-24`](https://towardsdatascience.com/multiple-gpu-training-in-pytorch-and-gradient-accumulation-as-an-alternative-to-it-e578b3fc5b91?source=collection_archive---------10-----------------------#2023-07-24)

## **代码与理论**

[](https://medium.com/@alexml0123?source=post_page-----e578b3fc5b91--------------------------------)![Alexey Kravets](https://medium.com/@alexml0123?source=post_page-----e578b3fc5b91--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e578b3fc5b91--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e578b3fc5b91--------------------------------) [Alexey Kravets](https://medium.com/@alexml0123?source=post_page-----e578b3fc5b91--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcf3e4a05b535&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultiple-gpu-training-in-pytorch-and-gradient-accumulation-as-an-alternative-to-it-e578b3fc5b91&user=Alexey+Kravets&userId=cf3e4a05b535&source=post_page-cf3e4a05b535----e578b3fc5b91---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e578b3fc5b91--------------------------------) ·7 分钟阅读·2023 年 7 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe578b3fc5b91&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultiple-gpu-training-in-pytorch-and-gradient-accumulation-as-an-alternative-to-it-e578b3fc5b91&user=Alexey+Kravets&userId=cf3e4a05b535&source=-----e578b3fc5b91---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe578b3fc5b91&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultiple-gpu-training-in-pytorch-and-gradient-accumulation-as-an-alternative-to-it-e578b3fc5b91&source=-----e578b3fc5b91---------------------bookmark_footer-----------)![](img/ed6fca5026469bec09b620c9620bc331.png)

[`unsplash.com/photos/vBzJ0UFOA70`](https://unsplash.com/photos/vBzJ0UFOA70)

在这篇文章中，我们将首先了解数据并行（DP）和分布式数据并行（DDP）算法之间的区别，然后解释什么是梯度累积（GA），最后展示如何在 PyTorch 中实现 DDP 和 GA，并使它们得到相同的结果。

## **简介**

在训练深度神经网络（DNN）时，一个重要的超参数是批量大小。通常，批量大小不应过大，因为网络会倾向于过拟合，但也不应过小，因为这会导致收敛缓慢。

当处理高分辨率图像或其他占用大量内存的数据时，假设目前大多数大规模 DNN 模型的训练是在 GPU 上进行的，适应小批量大小可能会根据可用 GPU 的内存而变得有问题。因为，正如我们所说，小批量大小会导致收敛速度缓慢，我们可以使用三种主要方法来增加有效批量大小：

1.  使用多个小型 GPU 在 mini-batch 上并行运行模型——DP 或 DDP 算法

1.  使用更大的 GPU（昂贵）

1.  在多个步骤中积累梯度
