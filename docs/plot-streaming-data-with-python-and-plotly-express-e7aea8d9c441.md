# 使用 Python 和 Plotly Express 绘制流数据

> 原文：[https://towardsdatascience.com/plot-streaming-data-with-python-and-plotly-express-e7aea8d9c441?source=collection_archive---------1-----------------------#2023-03-26](https://towardsdatascience.com/plot-streaming-data-with-python-and-plotly-express-e7aea8d9c441?source=collection_archive---------1-----------------------#2023-03-26)

## 实时追踪国际空间站

[](https://medium.com/@lee_vaughan?source=post_page-----e7aea8d9c441--------------------------------)[![Lee Vaughan](../Images/9f6b90bb76102f438ab0b9a4a62ffa3f.png)](https://medium.com/@lee_vaughan?source=post_page-----e7aea8d9c441--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e7aea8d9c441--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e7aea8d9c441--------------------------------) [Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----e7aea8d9c441--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5d604015c08b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplot-streaming-data-with-python-and-plotly-express-e7aea8d9c441&user=Lee+Vaughan&userId=5d604015c08b&source=post_page-5d604015c08b----e7aea8d9c441---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7aea8d9c441--------------------------------) ·8分钟阅读·2023年3月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe7aea8d9c441&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplot-streaming-data-with-python-and-plotly-express-e7aea8d9c441&user=Lee+Vaughan&userId=5d604015c08b&source=-----e7aea8d9c441---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe7aea8d9c441&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplot-streaming-data-with-python-and-plotly-express-e7aea8d9c441&source=-----e7aea8d9c441---------------------bookmark_footer-----------)![](../Images/962b0ecd54ae50e1fb81942915e0c069.png)

国际空间站（图片来源于[NASA图像库](https://images.nasa.gov/)）

*流数据* 是指实时数据，它持续不断地从一个来源流向目标。它包括音频、视频、文本或由社交媒体平台、传感器和服务器等来源生成的数值数据。数据传输以稳定的流形式进行，没有固定的开始或结束。流数据在医疗保健、金融和交通等领域中非常重要，并且是[*物联网 (IoT)*](https://www.networkworld.com/article/3207535/what-is-iot-the-internet-of-things-explained.html)的关键组成部分。

处理流数据的能力是数据科学家必备的重要技能。在这个 *快速成功数据科学* 项目中，我们将使用流数据来跟踪国际空间站（ISS）绕地球运行。编码我们将使用 Python 和 Plotly Express 在 Jupyter Notebook 中。

# 国际空间站遥测

*遥测*是指将远程传感器数据*现场*收集并自动传输到接收设备进行监测。虽然有很多 ISS 遥测的来源，我们将使用 [*WTIA REST API*](https://wheretheiss.at/w/developer)（WTIA 代表 *Where the ISS at?*）。

这个 API 由 Bill Shupp 编写，旨在提供比典型的 ISS 跟踪/通知网站更多的功能。文章结尾，我会列出一些额外的…
