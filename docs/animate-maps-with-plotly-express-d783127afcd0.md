# 使用 Plotly Express 动画地图

> 原文：[`towardsdatascience.com/animate-maps-with-plotly-express-d783127afcd0?source=collection_archive---------9-----------------------#2023-07-11`](https://towardsdatascience.com/animate-maps-with-plotly-express-d783127afcd0?source=collection_archive---------9-----------------------#2023-07-11)

## 让你的信息图焕发活力！

[](https://medium.com/@lee_vaughan?source=post_page-----d783127afcd0--------------------------------)![Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----d783127afcd0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d783127afcd0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d783127afcd0--------------------------------) [Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----d783127afcd0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5d604015c08b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanimate-maps-with-plotly-express-d783127afcd0&user=Lee+Vaughan&userId=5d604015c08b&source=post_page-5d604015c08b----d783127afcd0---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d783127afcd0--------------------------------) ·9 min 阅读·2023 年 7 月 11 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd783127afcd0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanimate-maps-with-plotly-express-d783127afcd0&user=Lee+Vaughan&userId=5d604015c08b&source=-----d783127afcd0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd783127afcd0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanimate-maps-with-plotly-express-d783127afcd0&source=-----d783127afcd0---------------------bookmark_footer-----------)![](img/c173db5b859806e3f3e30abb0f52171a.png)

按照它们加入联邦的年份着色的选定州（所有图片均为作者提供，除非另有说明）

动画地图是吸引注意力和传达信息的绝佳工具。无论你是准备演讲的商务人士、制作信息图的记者，还是准备课堂教学的教师，动画都能提升观众的参与感、专注度和记忆程度。即使你不打算展示实时动画，这些功能对于准备不同时间段的静态展示也是非常有用的。

在这个*快速成功数据科学*项目中，我们将使用 Python、pandas 和 Plotly Express 来可视化美国的发展。具体来说，我们将使用[分级色图](https://en.wikipedia.org/wiki/Choropleth_map)来展示美国各州按日、按年及更大时间范围加入联邦的动态。

# 代码

以下代码是在 Jupyter Lab 中编写的，并以*单元格*形式展示。

## 安装和导入库

Plotly Express 是 Plotly 图形库的高级版本，并且需要 Plotly 作为依赖。你可以通过 conda 或 pip 来安装它。

这里是 conda 安装命令：

`conda install -c plotly plotly_express`

这是 pip 版本的安装命令：
