# 自然语言处理中的字符编码：ASCII 和 Unicode 的角色

> 原文：[https://towardsdatascience.com/character-encoding-in-nlp-the-role-of-ascii-and-unicode-9349b4fe3cee?source=collection_archive---------13-----------------------#2023-01-12](https://towardsdatascience.com/character-encoding-in-nlp-the-role-of-ascii-and-unicode-9349b4fe3cee?source=collection_archive---------13-----------------------#2023-01-12)

## 更详细地审视技术细节和实际应用

[](https://medium.com/@javi_sanchez?source=post_page-----9349b4fe3cee--------------------------------)[![Javi Sánchez](../Images/4b24d1e1aaef9581f1d085c9a6a5990d.png)](https://medium.com/@javi_sanchez?source=post_page-----9349b4fe3cee--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9349b4fe3cee--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9349b4fe3cee--------------------------------) [Javi Sánchez](https://medium.com/@javi_sanchez?source=post_page-----9349b4fe3cee--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdadb8e5314e0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcharacter-encoding-in-nlp-the-role-of-ascii-and-unicode-9349b4fe3cee&user=Javi+S%C3%A1nchez&userId=dadb8e5314e0&source=post_page-dadb8e5314e0----9349b4fe3cee---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9349b4fe3cee--------------------------------) ·7 min read·2023年1月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9349b4fe3cee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcharacter-encoding-in-nlp-the-role-of-ascii-and-unicode-9349b4fe3cee&user=Javi+S%C3%A1nchez&userId=dadb8e5314e0&source=-----9349b4fe3cee---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9349b4fe3cee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcharacter-encoding-in-nlp-the-role-of-ascii-and-unicode-9349b4fe3cee&source=-----9349b4fe3cee---------------------bookmark_footer-----------)

# 介绍

在本文中，我们将探讨字符编码标准的主题，特别是ASCII和Unicode系统。我们将深入了解它们的工作原理及其在深度学习中的作用。此外，我们还将提供一些使用 Tensorflow 进行字符编码的示例，以便概览该库如何处理字符串。

![](../Images/2a0c219212d87bfa8b058ae41b488b43.png)

[Giammarco](https://unsplash.com/es/@giamboscaro) 提供的照片，来源于 [Unsplash](https://unsplash.com/)

首先，我们将介绍一些重要的概念。

# 什么是字符编码标准？

字符编码是一种将字符表示为数值（即编码点）的系统。这些编码点允许计算机存储和处理文本，然后可以以其他方式显示或使用。在本文中，我们将解释 ASCII 和 Unicode 字符编码系统，并讨论它们在自然语言处理（NLP）领域的实用性。

# ASCII

ASCII（美国信息交换标准代码）是一种字符编码标准，为书面文本中的每个字母、数字和其他符号分配唯一的数字。它被广泛使用，但也存在一些限制。

ASCII 具有 128 个编码点，这意味着它可以表示 128 个字符和符号。其中一些编码点代表计算机的指令，而另一些代表可打印的字符，如字母和数字。

ASCII 中使用的映射系统可以在此表格中找到：

![](../Images/c443857bcfbabbafc942f1f70d1e6549.png)

ASCII 表格, [链接](https://commons.wikimedia.org/wiki/File:ASCII-Table-wide.svg)

如我们所见，在 128 个编码点中，只有 94 个是可打印的。

例如，使用此表格的十六进制列，我们可以将字符串“Language”编码为“4C 61 6E 67 75 61 67 65”。

## ASCII 的限制

正如我们之前所说，ASCII 码的主要限制是它只有 94 个可打印字符。这些字符包括大小写英文字母（52 个字符）、数字（10 个字符）以及标点符号和符号（32 个字符）。因此，ASCII 不适用于使用超过基本拉丁字母的语言。其他语言中有不同的字符（如中文、俄文、挪威文），甚至有法语和西班牙语中的带重音字母，这些都无法使用这种字符编码系统显示。此外，像表情符号或货币符号这样的特殊符号也不包含在 ASCII 中，这限制了它的潜力。因此，需要一种新的字符编码系统，以使其更具扩展性，并考虑到所有这些在 ASCII 码中被忽略的字符和符号。这就是 Unicode 标准的出现。

# Unicode

Unicode 是一种字符编码标准，开发于1980年代末至1990年代初，旨在扩展ASCII及其他现有标准的功能。其主要开发动机之一是需要一个可以表示任何语言文本的单一字符编码标准。为了解决这个问题，Unicode 联盟应运而生，创建了一个能够表示世界所有语言的单一通用字符编码标准。Unicode 使用16位编码方案，这使其能够表示超过65,000种不同的字符。这比ASCII可以表示的128个字符要多得多。它已经成为WWW的主流字符编码标准，并被现代计算系统和软件广泛支持。它可以编码和显示多种语言的文本，包括使用拉丁字母以外的字符集的语言（如中文、日文、阿拉伯文），以及像表情符号和货币符号等特殊符号。

你可以在他们的[网站](https://home.unicode.org/?source=post_page-----9349b4fe3cee--------------------------------)上找到更多信息。

[](https://home.unicode.org/?source=post_page-----9349b4fe3cee--------------------------------) [## 首页

### 编辑描述

home.unicode.org](https://home.unicode.org/?source=post_page-----9349b4fe3cee--------------------------------)

## 它是如何工作的？

Unicode 定义了一个代码空间，一组从0到10FFFF（十六进制）的数值，称为代码点，并以U开头表示，因此范围从U+0000到U+10FFFF。我们将使用U后跟字符的十六进制代码点值，并在必要时使用前导零（例如，U+00F7）。Unicode代码空间分为十七个平面，编号从0到16。每个平面由65,536（2¹⁶）个连续的代码点组成。平面0，称为基本多语言平面（BMP），包含了最常用的字符。其余的平面（1到16）被称为补充平面。在每个平面中，字符被分配在命名的相关字符块中。Unicode 块是多个连续的数字字符代码范围之一。它们用于组织Unicode标准中的大量字符。每个块通常但不总是用来提供一种或多种特定语言使用的字形，或者在某些通用应用领域中使用。

## 映射和编码

Unicode 定义了两种映射方法：UTF编码和UCS编码。编码将Unicode代码点范围映射到固定范围内的一系列值。所有UTF编码将代码点映射到唯一的字节序列。编码名称中的数字表示每个代码单元的位数。UTF-8和UTF-16是最常用的编码。

+   UTF-8 对每个代码点使用一到四个字节。它与ASCII非常兼容。

+   UTF-16 对每个代码点使用一个或两个16位代码单元。

# Unicode 在自然语言处理中的应用

在本节中，我们将看到如何在NLP任务中使用Unicode以及它的作用。我们将使用一些Tensorflow代码使这些示例更加生动。

NLP模型通常处理具有不同字符集的不同语言。最具代表性的任务可能是神经机器翻译（NMT），在这种任务中，模型必须将句子翻译成其他语言。但一般来说，所有语言模型都必须使用字符串序列作为输入，因此Unicode是一个相当重要的步骤。使用Unicode表示通常是最有效的选择。

在这里我们将看到如何在Tensorflow中表示字符串并使用Unicode对其进行操作。基本的TensorFlow tf.string类型允许我们构建字节字符串的张量。Unicode字符串默认是UTF-8。

```py
tf.constant(u"Hello world 🌎")
```

```py
>>> tf.Tensor(b'Hello world \\xf0\\x9f\\x8c\\x8e', shape=(), dtype=string)
```

在这里我们可以看到，表情符号被编码为“\xf0\x9f\x8c\x8e”。它以UTF-8表示。

## 表示

我们可以在Tensorflow中使用两种标准表示Unicode字符串：

+   字符串标量——其中代码点序列使用已知字符编码（例如Unicode）进行编码。

+   int32 向量——其中每个位置包含一个单一的代码点。

例如，以下值都表示Unicode字符串“语言处理”（在中文中意为“language processing”）。

```py
# Unicode string, represented as a UTF-8 encoded string scalar
text_utf8 = tf.constant(u"语言处理")
print(text_utf8)
```

```py
>>> tf.Tensor(b'\\xe8\\xaf\\xad\\xe8\\xa8\\x80\\xe5\\xa4\\x84\\xe7\\x90\\x86', shape=(), dtype=string)
```

我们也可以使用UTF-16进行表示。

```py
# Unicode string, represented as a UTF-16-BE encoded string scalar
text_utf16be = tf.constant(u"语言处理".encode("UTF-16-BE"))
print(text_utf16be)
```

```py
>>> tf.Tensor(b'\\x8b\\xed\\x8a\\x00Y\\x04t\\x06', shape=(), dtype=string)
```

最终，在一个Unicode代码点向量中。

```py
# Unicode string, represented as a vector of Unicode code points
text_chars = tf.constant([ord(char) for char in u"语言处理"])
print(text_chars)
```

```py
>>> tf.Tensor: shape=(4,), dtype=int32, numpy=array([35821, 35328, 22788, 29702], dtype=int32)
```

## 转换

Tensorflow提供了在这些不同表示之间转换的操作：

+   *tf.strings.unicode_decode*：将编码的字符串标量转换为代码点向量。

```py
text_chars_converted = tf.strings.unicode_decode(text_utf8, input_encoding='UTF-8')
print(text_chars)
print(text_chars_converted)
```

```py
>>> tf.Tensor([35821 35328 22788 29702], shape=(4,), dtype=int32)
>>> tf.Tensor([35821 35328 22788 29702], shape=(4,), dtype=int32)
```

+   *tf.strings.unicode_encode*：将代码点向量转换为编码的字符串标量。

```py
text_utf8_converted = tf.strings.unicode_encode(text_chars, output_encoding='UTF-8')
print(text_utf8)
print(text_utf8_converted)
```

```py
>>> tf.Tensor(b'\\xe8\\xaf\\xad\\xe8\\xa8\\x80\\xe5\\xa4\\x84\\xe7\\x90\\x86', shape=(), dtype=string)
>>> tf.Tensor(b'\\xe8\\xaf\\xad\\xe8\\xa8\\x80\\xe5\\xa4\\x84\\xe7\\x90\\x86', shape=(), dtype=string)
```

+   *tf.strings.unicode_transcode*：将编码的字符串标量转换为不同的编码。

```py
text_utf16be_converted = tf.strings.unicode_transcode(text_utf8, input_encoding='UTF-8', output_encoding='UTF-16-BE')
print(text_utf16be)
print(text_utf16be_converted)
```

```py
>>> tf.Tensor(b'\\x8b\\xed\\x8a\\x00Y\\x04t\\x06', shape=(), dtype=string)
>>> tf.Tensor(b'\\x8b\\xed\\x8a\\x00Y\\x04t\\x06', shape=(), dtype=string)
```

## 字符长度

我们可以使用*tf.strings.length*操作的unit参数来指示字符长度的计算方式。默认的单位值是“BYTE”，但可以设置为其他值，例如“UTF8_CHAR”或“UTF16_CHAR”，以确定每个编码字符串中的Unicode代码点数量。

```py
# Note that the final character (emoji) takes up 4 bytes in UTF8.
helloWorld = u"Hello World 🌍".encode('UTF-8')
print(helloWorld)
```

```py
>>> b'Hello World \\xf0\\x9f\\x8c\\x8d'
```

```py
num_bytes = tf.strings.length(helloWorld).numpy()
num_chars = tf.strings.length(helloWorld, unit='UTF8_CHAR').numpy()
print('{} bytes; {} UTF-8 characters'.format(num_bytes, num_chars))
```

```py
>>> 16 bytes; 13 UTF-8 characters
```

如果你计算字符串“Hello World \xf0\x9f\x8c\x8d”的字节数（包括每个字母、空格和字节），你会看到总共有16个字节，如输出代码所示。

如果我们像之前一样计算这字符串的字符数，但将表情符号视为一个字符而不是4个字节，这个字符串包含13个UTF-8字符。

如果你想了解更完整的教程，我建议你访问这个TensorFlow教程。

[## Unicode strings | Text | TensorFlow](https://www.tensorflow.org/text/guide/unicode?source=post_page-----9349b4fe3cee--------------------------------)

### NLP模型通常处理具有不同字符集的不同语言。Unicode是一个标准编码系统，它...

[www.tensorflow.org](https://www.tensorflow.org/text/guide/unicode?source=post_page-----9349b4fe3cee--------------------------------)

# 结论

总之，字符编码是计算机系统和自然语言处理（NLP）中的一个重要方面。ASCII 是一种广泛使用的标准，为文本中的每个字母、数字和符号分配唯一的编号，但它在字符表示上存在局限性。Unicode 标准的开发旨在解决 ASCII 的局限性，它使用 16 位编码方案，使其能够表示超过 65,000 个不同的字符，并支持任何语言的文本。Unicode 已成为全球互联网和现代计算系统中主流的字符编码标准，对于显示和处理各种语言和符号的文本至关重要。在这篇文章中，我们详细概述了 ASCII 和 Unicode 编码系统，以及 Tensorflow 如何管理 Unicode 中的字符串。

如果你有任何疑问或建议，请留下评论。感谢阅读！
