# Web Speech API：什么有效，什么无效，以及如何通过将其与 GPT 语言模型链接来改进

> 原文：[`towardsdatascience.com/web-speech-api-what-works-what-doesnt-and-how-to-improve-it-by-linking-it-to-a-gpt-language-dc1afde54ced?source=collection_archive---------5-----------------------#2023-12-06`](https://towardsdatascience.com/web-speech-api-what-works-what-doesnt-and-how-to-improve-it-by-linking-it-to-a-gpt-language-dc1afde54ced?source=collection_archive---------5-----------------------#2023-12-06)

## 现代 AI 和其他技术如何促进更高效的人机交互系列的一部分

[](https://lucianosphere.medium.com/?source=post_page-----dc1afde54ced--------------------------------)![LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----dc1afde54ced--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dc1afde54ced--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dc1afde54ced--------------------------------) [LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----dc1afde54ced--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd28939b5ab78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fweb-speech-api-what-works-what-doesnt-and-how-to-improve-it-by-linking-it-to-a-gpt-language-dc1afde54ced&user=LucianoSphere+%28Luciano+Abriata%2C+PhD%29&userId=d28939b5ab78&source=post_page-d28939b5ab78----dc1afde54ced---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----dc1afde54ced--------------------------------) ·15 分钟阅读·2023 年 12 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdc1afde54ced&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fweb-speech-api-what-works-what-doesnt-and-how-to-improve-it-by-linking-it-to-a-gpt-language-dc1afde54ced&user=LucianoSphere+%28Luciano+Abriata%2C+PhD%29&userId=d28939b5ab78&source=-----dc1afde54ced---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdc1afde54ced&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fweb-speech-api-what-works-what-doesnt-and-how-to-improve-it-by-linking-it-to-a-gpt-language-dc1afde54ced&source=-----dc1afde54ced---------------------bookmark_footer-----------)![](img/a173108cd0621793832a4adc1033b257.png)

图片由[palesa](https://unsplash.com/@palesa08?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我认为现代技术使得今天的人机交互比目前的软件所提供的要简单自然得多。的确，我认为技术已经成熟到我们可以无需传统界面，直接迈向用户体验的革命。

大型语言模型无疑触发了这场革命的一个阶段，特别是在我们获取信息的方式上。然而，我认为技术仍然可以提供更多。例如，尽管 VR 头盔的成本正在下降，我们仍然在很大程度上停留在平面屏幕上；尽管眼动、语音识别和身体肢体追踪等技术已经取得了很大进步，我们仍然在使用鼠标、键盘和触控手势来操作设备；尽管语音合成技术已经取得了巨大进展，我们仍然在大量阅读。

> 我感觉当前的技术已经足够成熟，可以提供几乎像《星际迷航》中那样的人机交互（如果你不知道…）。
