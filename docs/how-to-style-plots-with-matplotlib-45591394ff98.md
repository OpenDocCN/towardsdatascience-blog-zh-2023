# 如何用Matplotlib设置图表样式

> 原文：[https://towardsdatascience.com/how-to-style-plots-with-matplotlib-45591394ff98?source=collection_archive---------7-----------------------#2023-11-08](https://towardsdatascience.com/how-to-style-plots-with-matplotlib-45591394ff98?source=collection_archive---------7-----------------------#2023-11-08)

## 快速成功的数据科学

## 不要满足于默认设置！

[](https://medium.com/@lee_vaughan?source=post_page-----45591394ff98--------------------------------)[![Lee Vaughan](../Images/9f6b90bb76102f438ab0b9a4a62ffa3f.png)](https://medium.com/@lee_vaughan?source=post_page-----45591394ff98--------------------------------)[](https://towardsdatascience.com/?source=post_page-----45591394ff98--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----45591394ff98--------------------------------) [Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----45591394ff98--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5d604015c08b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-style-plots-with-matplotlib-45591394ff98&user=Lee+Vaughan&userId=5d604015c08b&source=post_page-5d604015c08b----45591394ff98---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----45591394ff98--------------------------------) ·8 分钟阅读·2023年11月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F45591394ff98&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-style-plots-with-matplotlib-45591394ff98&user=Lee+Vaughan&userId=5d604015c08b&source=-----45591394ff98---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F45591394ff98&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-style-plots-with-matplotlib-45591394ff98&source=-----45591394ff98---------------------bookmark_footer-----------)![](../Images/b56ed3118211d9fd51f6edb70d57dec3.png)

设置样式（来源：Cole Keister，拍摄于Unsplash）

几十年前，我母亲给我送了一件栗色的天鹅绒运动服作为圣诞礼物。这是一件糟糕透顶的衣服，我回答说它其实不再流行了。她嗤之以鼻地说：“*你*设定了风格！做个趋势引领者！”

不用多说，我确实没有设置样式，但我妻子仍然用“你设置了样式！”这句话来取笑我。不过，我确实在使用Matplotlib时设置了样式，这与穿着天鹅绒运动服不同，那是件好事。

为了方便，Python的Matplotlib库允许你覆盖其默认绘图选项。你可以利用这一强大功能，不仅自定义图表，还可以为报告、出版物和演示文稿应用一致、自动且可重用的样式。

在这个*快速成功数据科学*项目中，我们将快速查看如何使用Matplotlib来设置图形样式。

# Matplotlib中的样式选项

如果你使用过Matplotlib，你可能已经通过向绘图方法传递*新*值来更改了图形的默认设置，比如线条的颜色。但是，如果你想要*同时*为*多个*图形设置这些值，使得所有曲线都具有相同的颜色，或者*循环*...
