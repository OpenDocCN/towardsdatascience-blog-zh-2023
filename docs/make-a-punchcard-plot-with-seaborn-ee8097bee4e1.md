# 使用Seaborn制作打卡图

> 原文：[https://towardsdatascience.com/make-a-punchcard-plot-with-seaborn-ee8097bee4e1?source=collection_archive---------4-----------------------#2023-09-17](https://towardsdatascience.com/make-a-punchcard-plot-with-seaborn-ee8097bee4e1?source=collection_archive---------4-----------------------#2023-09-17)

## 快速识别周期性趋势

[](https://medium.com/@lee_vaughan?source=post_page-----ee8097bee4e1--------------------------------)[![Lee Vaughan](../Images/9f6b90bb76102f438ab0b9a4a62ffa3f.png)](https://medium.com/@lee_vaughan?source=post_page-----ee8097bee4e1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ee8097bee4e1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ee8097bee4e1--------------------------------) [Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----ee8097bee4e1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5d604015c08b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmake-a-punchcard-plot-with-seaborn-ee8097bee4e1&user=Lee+Vaughan&userId=5d604015c08b&source=post_page-5d604015c08b----ee8097bee4e1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ee8097bee4e1--------------------------------) ·6分钟阅读·2023年9月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fee8097bee4e1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmake-a-punchcard-plot-with-seaborn-ee8097bee4e1&user=Lee+Vaughan&userId=5d604015c08b&source=-----ee8097bee4e1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fee8097bee4e1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmake-a-punchcard-plot-with-seaborn-ee8097bee4e1&source=-----ee8097bee4e1---------------------bookmark_footer-----------)![](../Images/9d2e6660c00c362b880ffac58e014488.png)

带有时间卡的打卡钟（图像来自UnSplash上的Hennie Stander）

*打卡图*，也称为*表格气泡图*，是一种用于突出数据中周期性趋势的可视化类型。它以严格的*矩阵*或*网格*格式显示数据，通常由一周中的天数与一天中的小时组成。圆圈表示行列交点上的数据点，其大小传达数据值。颜色可以用来包含额外的信息。

![](../Images/2c386b48028bb15000f4a0f66bb298f2.png)

表格气泡图（图像来自作者）

“打卡卡片”这一名称源于旧式的“时间卡”，工人们会在机器上打孔以记录他们的出入时间。

要构建打卡图，你需要 *时间戳* 数据。在这个 *快速成功的数据科学* 项目中，我们将使用 Kaggle 数据集来追踪在华盛顿特区租赁自行车的时间。

# 数据集

Kaggle 的 [*华盛顿特区自行车共享数据集*](https://www.kaggle.com/datasets/marklvl/bike-sharing-dataset) 包含了 2011 年和 2012 年在华盛顿特区的 [*Capital bikeshare 系统*](https://capitalbikeshare.com/system-data) 中租赁自行车的每小时和每日计数数据[1]。这些数据是根据 [CC0 1.0 许可协议](https://creativecommons.org/publicdomain/zero/1.0/) 发布的。有关数据集内容的详细信息，请访问 [*readme* 文件](https://www.kaggle.com/datasets/marklvl/bike-sharing-dataset?select=Readme.txt)。
