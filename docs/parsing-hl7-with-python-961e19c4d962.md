# 使用 Python 解析 HL7

> 原文：[`towardsdatascience.com/parsing-hl7-with-python-961e19c4d962`](https://towardsdatascience.com/parsing-hl7-with-python-961e19c4d962)

## 使用 python-hl7 提取健康等级 7 数据的指南

[](https://medium.com/@terrah27?source=post_page-----961e19c4d962--------------------------------)![Tara (Boyle) Hunter](https://medium.com/@terrah27?source=post_page-----961e19c4d962--------------------------------)[](https://towardsdatascience.com/?source=post_page-----961e19c4d962--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----961e19c4d962--------------------------------) [Tara (Boyle) Hunter](https://medium.com/@terrah27?source=post_page-----961e19c4d962--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----961e19c4d962--------------------------------) ·阅读时间 4 分钟·2023 年 3 月 6 日

--

![](img/0d69d501b5083c1b37d8376348218dd6.png)

照片由 [Christina Victoria Craft](https://unsplash.com/@victoriabcphotographer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/medical?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

解析 HL7 消息对于许多医疗数据专业人员来说是一个重要任务。HL7 在医院和其他医疗机构中常用于在不同系统、应用程序和提供者之间交换病人数据。

根据 [维基百科](https://en.wikipedia.org/wiki/Health_Level_7)的定义，HL7 或健康等级 7 “指一套用于在不同医疗提供者使用的软件应用程序之间交换电子健康信息的国际标准”。这些标准使医疗提供者能够以一致且易于解释的方式共享临床信息。

在这里，我们将学习如何使用 Python 解析 HL7 消息。

## 介绍

在开始解析之前，我们需要对 HL7 消息结构有一个基本的了解。虽然 HL7 有不同的版本，这里我们将重点关注最常用的 2.x 系列版本。

HL7 消息通常是在病人事件（如接收、出院或转院）发生时创建和发送的。

HL7 消息由多个部分组成。每一行称为一个段落。段落由三字符标签标识，并由回车符（\r）分隔。段落包含由管道符（|）分隔的字段。字段包含由脱字符（^）分隔的组件。最后，组件可以包含由 & 符号分隔的子组件。

## 常见的 HL7 段落

+   MSH — 消息头：消息的第一个段落，包含有关发送和接收系统、消息类型和消息控制 ID 的信息。

+   PID — 病人识别：该段包含病人识别信息，包括病人 ID、姓名和出生日期。

+   DG1 — 诊断：该段包含与病人诊断相关的信息。

+   Z 段：这些段不是原始 HL7 标准的一部分。它们用于创建自定义消息并增加 HL7 的灵活性。自定义段的格式和内容由发送和接收应用程序协商。

关于这些和其他段类型的更多信息可以在[这里](https://hl7-definition.caristix.com/v2/HL7v2.5.1/Segments)找到。

## 示例消息

在这里我们将处理这个虚构的入院消息：

```py
## made up sample admit message
MSH|^~\&|HIS|HOSPITAL|LAB|LAB|20230131111929||ADT^A01|1000027|P|2.3||||
EVN|A01|20220131111924
PID|1||0012345678^^^MRN^MRN||Doe^John^R||19700101|M|||123 Main St.^^Anytown^CA⁹¹²³⁴^USA|||||||||||||||||||||
PV1|1||^^¹⁰⁰¹|||||||||||||||||||1||||||||||||||||||||||||||||||||||
DG1|1||123456789^Diagnosis^I9||Confirmed
ZCP|1|Custom Segment Data
```

在此消息中，MSH 段包含消息头信息，包括发送应用程序、发送机构、接收应用程序和接收机构。

EVN 段包含事件信息，表明这是一个 ADT A01 消息，并在 2023 年 1 月 31 日 11:19:24 发送。

PID 段包含病人识别信息，包括病人标识符、姓名、出生日期和地址。

PV1 段包含病人访问信息，如病人位置和主治医生。

DG1 段包含一个虚构的诊断代码“123456789”和描述“诊断”。诊断状态字段标记为“已确认”。

最终我们看到自定义 ZCP 段，包含段 ID 为“ZCP”和两个数据字段“1”和“自定义段数据”。

## 解析 HL7 消息

现在我们对 HL7 消息有了基本了解，接下来开始解析吧！

在这里我们将使用`[python-hl7](https://python-hl7.readthedocs.io/en/latest/)` [库](https://python-hl7.readthedocs.io/en/latest/)。它提供了简单的方法来使用 Python 解析 HL7 消息。

`python-hl7`可以通过`pip`轻松安装。

```py
pip install hl7 
```

安装完成后，我们可以导入库并解析我们的示例消息：

```py
import hl7

## sample message
msg = '''
MSH|^~\&|HIS|HOSPITAL|LAB|LAB|20230131111929||ADT^A01|1000027|P|2.3||||
EVN|A01|20220131111924
PID|1||0012345678^^^MRN^MRN||Doe^John^R||19700101|M|||123 Main St.^^Anytown^CA⁹¹²³⁴^USA|||||||||||||||||||||
PV1|1||^^¹⁰⁰¹|||||||||||||||||||1||||||||||||||||||||||||||||||||||
DG1|1||123456789^Diagnosis^I9||Confirmed
ZCP|1|Custom Segment Data
'''

parsed = hl7.parse(msg)
print(parsed)
```

![](img/e844389e207dcb2dec03958110b36720.png)

解析的 HL7 消息 — 作者提供的图像

在这里我们可以看到解析后的消息。我们可以检查消息的长度以确保它按预期被解析：

```py
print(len(parsed))
>>> 1
```

在这里，我们看到解析后的消息长度为 1。由于我们可以在上述消息中数到 6 个不同的段，这似乎不正确。

这是由于我们的换行符问题。`python-hl7`期望换行符是回车符（\r），而不是换行符（\n）。我们可以通过将换行符替换为回车符来解决这个问题：

```py
msg = msg.replace('\n','\r')
parsed = hl7.parse(msg)
print(len(parsed))
>>> 6
```

这看起来好多了！现在我们的长度为 6，与我们对消息的视觉检查一致。

现在让我们提取病人的姓名：

```py
## get patient name
print(parsed[2][5])
>>> [[['Doe'], ['John'], ['R']]]

## get only first name
print(parsed[2][5][0][1][0])
>>> 'John'
```

我们成功提取了病人的全名和名字！

我们还可以通过指定段来访问这些信息：

```py
## we can access the same information by specifying segement
parsed.segments('PID')[0][5]
>>> [[['Doe'], ['John'], ['R']]]

## and only the first name
parsed.segments('PID')[0][5][0][1][0]
>>> 'John'
```

这种方法的一个好处是可以让我们的代码更容易理解。

## 结论

在这里，我们了解了 HL7 消息以及如何使用 Python 的`python-hl7`库来解析它们。

这个库提供了一种方便的方式来使用 Python 读取 HL7 消息，并且支持大多数 HL7 消息类型和段落。

使用合适的工具，开发人员可以有效地和高效地用 Python 解析 HL7 消息！由于 HL7 是医疗保健领域广泛使用的标准，解析这些消息的知识对从事医疗行业的数据专业人士来说是一个有价值的技能。
