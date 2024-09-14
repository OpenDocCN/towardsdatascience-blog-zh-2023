# 在编写 Apache Beam 管道时使用示例进行 Map、Filter 和 CombinePerKey 转换

> 原文：[`towardsdatascience.com/map-filter-and-combineperkey-transforms-in-writing-apache-beam-pipelines-with-examples-e06926124a02`](https://towardsdatascience.com/map-filter-and-combineperkey-transforms-in-writing-apache-beam-pipelines-with-examples-e06926124a02)

![](img/3c0bc71612c5632ec1b351c55f68aa03.png)

图片由 [JJ Ying](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 让我们用一些真实数据进行练习

[](https://rashida00.medium.com/?source=post_page-----e06926124a02--------------------------------)![Rashida Nasrin Sucky](https://rashida00.medium.com/?source=post_page-----e06926124a02--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e06926124a02--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e06926124a02--------------------------------) [Rashida Nasrin Sucky](https://rashida00.medium.com/?source=post_page-----e06926124a02--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e06926124a02--------------------------------) ·8 分钟阅读·2023 年 7 月 12 日

--

Apache Beam 作为统一的编程模型，在高效和可移植的大数据处理管道中越来越受欢迎。它可以处理批量数据和流数据。这也是名字的由来。Beam 是 Batch 和 Stream 两个词的组合：

B（来自**B**atch）+ eam（来自 str**eam**）= Beam

便携性也是一个很棒的特性。你只需专注于运行管道，它可以在任何地方运行，例如 Spark、Flink、Apex 或 Cloud Dataflow。你不需要更改逻辑或语法。

在这篇文章中，我们将专注于学习如何通过示例编写一些 ETL 管道。我们将尝试使用一个好的数据集进行一些转换操作，希望你会发现这些转换操作在工作中也非常有用。

请随意下载这个[公共数据集](https://creativecommons.org/publicdomain/zero/1.0/)并跟随练习：

[示例销售数据 | Kaggle](https://www.kaggle.com/datasets/kyanyoga/sample-sales-data)

这个练习使用了 Google Colab notebook。因此，安装非常简单。只需使用这一行代码：

```py
!pip install --quiet apache_beam
```

安装完成后，我为这个练习创建了一个名为‘data’的目录：

```py
mkdir -p data
```

让我们深入探讨今天的话题——转换操作。首先，我们将处理一个最简单的管道，即读取 CSV 文件并将其写入文本文件。

这不像 Padas 的 read_csv() 方法那么简单。它需要一个 coder() 操作。首先，在这里定义了一个 CustomCoder() 类，该类首先将对象编码为字节字符串，然后将字节解码为其对应的对象，并最终指定这个 coder 是否保证对值进行确定性编码。请查看 [文档这里。](https://beam.apache.org/releases/pydoc/2.2.0/apache_beam.coders.coders.html)

如果这是你的第一个管道，请注意管道的语法。在 CustomCoder() 类之后是最简单的管道。我们首先将空管道初始化为‘p1’。然后我们编写了‘sales’管道，其中首先从我们之前创建的数据文件夹中读取 CSV 文件。在 Apache beam 中，管道中的每个转换操作都以 | 符号开始。读取 CSV 文件中的数据后，我们只是将其写入文本文件。最后，通过 run() 方法我们运行了管道。这是 Apache beam 中标准和常用的管道语法。

```py
import apache_beam as beam
from apache_beam.coders.coders import Coder

class CustomCoder(Coder):
    """A custom coder used for reading and writing strings as UTF-8."""

    def encode(self, value):
        return value.encode("utf-8", "replace")

    def decode(self, value):
        return value.decode("utf-8", "ignore")

    def is_deterministic(self):
        return True
p1 = beam.Pipeline()

sales = (p1
         |beam.io.ReadFromText('data/sales_data_sample.csv', coder=CustomCoder(), skip_header_lines=1)
         |beam.io.WriteToText('data/output'))
p1.run()
```

如果你现在检查你的‘data’文件夹，你会看到一个‘output-00000-of-00001’文件。从这个文件中打印前 5 行以检查数据：

```py
!head -n 5 data/output-00000-of-00001
```

输出：

```py
10107,30,95.7,2,2871,2/24/2003 0:00,Shipped,1,2,2003,Motorcycles,95,S10_1678,Land of Toys Inc.,2125557818,897 Long Airport Avenue,,NYC,NY,10022,USA,NA,Yu,Kwai,Small
10121,34,81.35,5,2765.9,5/7/2003 0:00,Shipped,2,5,2003,Motorcycles,95,S10_1678,Reims Collectables,26.47.1555,59 rue de l'Abbaye,,Reims,,51100,France,EMEA,Henriot,Paul,Small
10134,41,94.74,2,3884.34,7/1/2003 0:00,Shipped,3,7,2003,Motorcycles,95,S10_1678,Lyon Souveniers,+33 1 46 62 7555,27 rue du Colonel Pierre Avia,,Paris,,75508,France,EMEA,Da Cunha,Daniel,Medium
10145,45,83.26,6,3746.7,8/25/2003 0:00,Shipped,3,8,2003,Motorcycles,95,S10_1678,Toys4GrownUps.com,6265557265,78934 Hillside Dr.,,Pasadena,CA,90003,USA,NA,Young,Julie,Medium
10159,49,100,14,5205.27,10/10/2003 0:00,Shipped,4,10,2003,Motorcycles,95,S10_1678,Corporate Gift Ideas Co.,6505551386,7734 Strong St.,,San Francisco,CA,,USA,NA,Brown,Julie,Medium
```

## Map

让我们来看一下如何在上述管道中使用 Map 转换。这是最常见的转换操作。你在 Map 中指定的转换将应用于 PCollection 中的每一个元素。

例如，我想添加一个 split 方法以从 PCollection 中的每个元素创建列表。在这里，我们将使用 lambda 进行 Map 转换。如果你不熟悉 lambda，请查看这里的 lambda 代码。lambda 后我们提到‘row’，任何其他变量名也可以。我们对‘row’应用的任何函数或方法将应用于 PCollection 中的每个元素。

```py
p2 = beam.Pipeline()
sales = (p2
         |beam.io.ReadFromText('data/sales_data_sample.csv', coder=CustomCoder(), skip_header_lines=1)
         |beam.Map(lambda row: row.split(','))
         |beam.io.WriteToText('data/output2'))
p2.run()
```

看，它是完全相同的语法。只是我在读取和写入操作之间多了一行代码。再次打印输出的前 5 行进行检查：

```py
!head -n 5 data/output2-00000-of-00001
```

输出：

```py
['10107', '30', '95.7', '2', '2871', '2/24/2003 0:00', 'Shipped', '1', '2', '2003', 'Motorcycles', '95', 'S10_1678', 'Land of Toys Inc.', '2125557818', '897 Long Airport Avenue', '', 'NYC', 'NY', '10022', 'USA', 'NA', 'Yu', 'Kwai', 'Small']
['10121', '34', '81.35', '5', '2765.9', '5/7/2003 0:00', 'Shipped', '2', '5', '2003', 'Motorcycles', '95', 'S10_1678', 'Reims Collectables', '26.47.1555', "59 rue de l'Abbaye", '', 'Reims', '', '51100', 'France', 'EMEA', 'Henriot', 'Paul', 'Small']
['10134', '41', '94.74', '2', '3884.34', '7/1/2003 0:00', 'Shipped', '3', '7', '2003', 'Motorcycles', '95', 'S10_1678', 'Lyon Souveniers', '+33 1 46 62 7555', '27 rue du Colonel Pierre Avia', '', 'Paris', '', '75508', 'France', 'EMEA', 'Da Cunha', 'Daniel', 'Medium']
['10145', '45', '83.26', '6', '3746.7', '8/25/2003 0:00', 'Shipped', '3', '8', '2003', 'Motorcycles', '95', 'S10_1678', 'Toys4GrownUps.com', '6265557265', '78934 Hillside Dr.', '', 'Pasadena', 'CA', '90003', 'USA', 'NA', 'Young', 'Julie', 'Medium']
['10159', '49', '100', '14', '5205.27', '10/10/2003 0:00', 'Shipped', '4', '10', '2003', 'Motorcycles', '95', 'S10_1678', 'Corporate Gift Ideas Co.', '6505551386', '7734 Strong St.', '', 'San Francisco', 'CA', '', 'USA', 'NA', 'Brown', 'Julie', 'Medium']
```

看，每个元素都变成了一个列表。

## Filter

接下来，我将把 Filter 转换也添加到上述代码块中。这里的 lambda 也可以用于过滤。我们将过滤掉所有数据，只保留 Produc line 中的‘经典汽车’数据。数据集的第 11 列是产品线。正如你所知，Python 是零索引的。所以，列号的计数也从零开始。

```py
p3 = beam.Pipeline()
sales = (p3
         |beam.io.ReadFromText('data/sales_data_sample.csv', coder=CustomCoder(), skip_header_lines=1)
         |beam.Map(lambda row: row.split(','))
         |beam.Filter(lambda row:row[10] == "Classic Cars")
         |beam.io.WriteToText('data/output3'))
p3.run()
```

如之前所示，打印前 5 行进行检查：

```py
!head -n 5 data/output3-00000-of-00001
```

输出：

```py
['10103', '26', '100', '11', '5404.62', '1/29/2003 0:00', 'Shipped', '1', '1', '2003', 'Classic Cars', '214', 'S10_1949', 'Baane Mini Imports', '07-98 9555', 'Erling Skakkes gate 78', '', 'Stavern', '', '4110', 'Norway', 'EMEA', 'Bergulfsen', 'Jonas', 'Medium']
['10112', '29', '100', '1', '7209.11', '3/24/2003 0:00', 'Shipped', '1', '3', '2003', 'Classic Cars', '214', 'S10_1949', '"Volvo Model Replicas', ' Co"', '0921-12 3555', 'Berguvsvgen  8', '', 'Lule', '', 'S-958 22', 'Sweden', 'EMEA', 'Berglund', 'Christina', 'Large']
['10126', '38', '100', '11', '7329.06', '5/28/2003 0:00', 'Shipped', '2', '5', '2003', 'Classic Cars', '214', 'S10_1949', '"Corrida Auto Replicas', ' Ltd"', '(91) 555 22 82', '"C/ Araquil', ' 67"', '', 'Madrid', '', '28023', 'Spain', 'EMEA', 'Sommer', 'Martn', 'Large']
['10140', '37', '100', '11', '7374.1', '7/24/2003 0:00', 'Shipped', '3', '7', '2003', 'Classic Cars', '214', 'S10_1949', 'Technics Stores Inc.', '6505556809', '9408 Furth Circle', '', 'Burlingame', 'CA', '94217', 'USA', 'NA', 'Hirano', 'Juri', 'Large']
['10150', '45', '100', '8', '10993.5', '9/19/2003 0:00', 'Shipped', '3', '9', '2003', 'Classic Cars', '214', 'S10_1949', '"Dragon Souveniers', ' Ltd."', '+65 221 7555', '"Bronz Sok.', ' Bronz Apt. 3/6 Tesvikiye"', '', 'Singapore', '', '79903', 'Singapore', 'Japan', 'Natividad', 'Eric', 'Large']
```

查看上面输出中每个列表的第 11 个元素。它是‘经典汽车’。

## 回答一些问题

> **每种类型的汽车订购了多少数量？**

为了找出这一点，我们首先将创建元组，其中第一个元素或键将来自数据集的第 11 个元素，第二个元素即值将是数据集的第二个元素，即‘订购数量’。在下一步中，我们将使用 CombinePerKey() 方法。正如名字所示，它将为每个键结合具有聚合函数的值。

当你看到代码时会更清楚。这里是代码。

```py
p3a = beam.Pipeline()
sales = (p3a
         |beam.io.ReadFromText('data/sales_data_sample.csv', coder=CustomCoder(), skip_header_lines=1)
         |beam.Map(lambda row: row.split(','))
         #|beam.Filter(lambda row:row[10] == "Classic Cars")
         |beam.Map(lambda row: (row[10], int(row[1])))
         |beam.io.WriteToText('data/output3a'))
p3a.run()
!head -n 10 data/output3a-00000-of-00001
```

如你所见，我们在这里使用了两次 Map 函数。第一次是分割并像之前一样生成列表，然后从每行数据中提取第 10 列的产品线和第二列的数量。

这里是输出：

```py
('Motorcycles', 30)
('Motorcycles', 34)
('Motorcycles', 41)
('Motorcycles', 45)
('Motorcycles', 49)
('Motorcycles', 36)
('Motorcycles', 29)
('Motorcycles', 48)
('Motorcycles', 22)
('Motorcycles', 41)
```

只是打印了输出的前 10 行。如你所见，这里每行数据的订单数量都列出了。回答上述问题的下一步也是最后一步是将每项的所有值结合起来。Apache Beam 中有 CombinePerKey 方法可以实现。顾名思义，它会为每个键使用聚合函数来结合值。在这种情况下，我们需要的是“总和”。

```py
p4 = beam.Pipeline()
sales = (p4
         |beam.io.ReadFromText('data/sales_data_sample.csv', coder=CustomCoder(), skip_header_lines=1)
         |beam.Map(lambda row: row.split(','))
         #|beam.Filter(lambda row:row[10] == "Classic Cars")
         |beam.Map(lambda row: (row[10], int(row[1])))
         |beam.CombinePerKey(sum)
         |beam.io.WriteToText('data/output4'))
p4.run()
!head -n 10 data/output4-00000-of-00001
```

输出：

```py
('Motorcycles', 11663)
('Classic Cars', 33992)
('Trucks and Buses', 10777)
('Vintage Cars', 21069)
('Planes', 10727)
('Ships', 8127)
('Trains', 2712)
```

所以，我们得到了每个产品的总订单数量。

> **哪些州的订单数量超过了 2000 个？**

这是一个有趣的问题，我们需要每一个之前做过的变换加上另一个过滤变换。我们需要计算每个州的总订单数量，就像在前面的例子中计算每个产品的总订单数量一样。然后，将数量超过 2000 的订单进行过滤。

在所有之前的例子中，lambda 函数被用于 Map 和 Filter 变换。这里我们将看到如何定义一个函数并在 Map 或 Filter 函数中使用它。这里定义了一个函数 quantity_filter()，它返回值数量大于 2000 的项。

```py
def quantity_filter(row):
  name, count = row 
  if count > 2000:
    return row 

p7 = beam.Pipeline() 
sales = (p7
         |beam.io.ReadFromText('data/sales_data_sample.csv', coder=CustomCoder(), skip_header_lines=1)
         |beam.Map(lambda row: row.split(','))
         |beam.Map(lambda row: (row[17], int(row[1])))
         |beam.CombinePerKey(sum)
         |beam.Map(quantity_filter)
         |beam.io.WriteToText('data/output7'))
p7.run()
!head -n 10 data/output7-00000-of-00001
```

输出：

```py
('NYC', 5294)
None
None
None
('San Francisco', 2139)
None
('', 33574)
None
None
None
```

这是输出，其中如果数量不超过 2000，则返回‘None’。我不喜欢看到所有这些‘None’值。我将添加另一个过滤变换来过滤掉这些‘None’值。

```py
p8 = beam.Pipeline() 
sales = (p8
         |beam.io.ReadFromText('data/sales_data_sample.csv', coder=CustomCoder(), skip_header_lines=1)
         |beam.Map(lambda row: row.split(','))
         |beam.Map(lambda row: (row[17], int(row[1])))
         |beam.CombinePerKey(sum)
         |beam.Map(quantity_filter)
         |beam.Filter(lambda row: row != None)
         |beam.io.WriteToText('data/output8'))
p8.run()
!head -n 10 data/output8-00000-of-00001
```

输出：

```py
('NYC', 5294)
('San Francisco', 2139)
('', 33574)
('New Bedford', 2043)
('San Rafael', 6366)
```

所以，我们总共有 5 个返回的值，其中订单数量大于 2000。

## 结论

在本教程中，我想展示如何在 Apache Beam 中使用 Map、Filter 和 CombinePerKey 变换来编写 ETL 管道。希望它们足够清晰，能够在你的项目中使用。我将在下一篇文章中解释如何使用 ParDo。

随意关注我 [Twitter](https://twitter.com/rashida048) 并点赞我的 [Facebook](https://www.facebook.com/rashida.smith.161) 页面。

## 相关阅读

[关于 Python 中多项式回归的详细教程、概述、实现和过拟合 | 作者：Rashida Nasrin Sucky | 2023 年 6 月 | Towards AI](https://pub.towardsai.net/a-detailed-tutorial-on-polynomial-regression-in-python-overview-implementation-and-overfitting-e319fc7e5b8f)

迷你 VGG 网络图像识别的完整实现 | 作者：Rashida Nasrin Sucky | Towards Data Science

如何在 TensorFlow 中定义自定义层、激活函数和损失函数 | 作者：Rashida Nasrin Sucky | Towards Data Science

在 TensorFlow 中开发多输出模型的逐步教程 | 作者：拉希达·纳斯林·苏基 | 数据科学前沿

OpenCV Python 中的简单边缘检测方法 | 作者：拉希达·纳斯林·苏基 | 数据科学前沿
