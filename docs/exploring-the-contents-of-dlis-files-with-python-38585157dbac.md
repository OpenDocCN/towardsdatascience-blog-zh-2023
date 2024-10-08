# 使用 Python 探索 DLIS 文件的内容

> 原文：[`towardsdatascience.com/exploring-the-contents-of-dlis-files-with-python-38585157dbac`](https://towardsdatascience.com/exploring-the-contents-of-dlis-files-with-python-38585157dbac)

## 使用 Pandas 和 dlisio 探索井日志数据

[](https://andymcdonaldgeo.medium.com/?source=post_page-----38585157dbac--------------------------------)![Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----38585157dbac--------------------------------)[](https://towardsdatascience.com/?source=post_page-----38585157dbac--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----38585157dbac--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----38585157dbac--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----38585157dbac--------------------------------) ·8 分钟阅读·2023 年 7 月 14 日

--

![](img/36ce0ae3f2ac656818b83f047ec06850.png)

图片由 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

[DLIS 文件](http://w3.energistics.org/RP66/V1/Toc/main.html) 是一种标准的石油和天然气行业数据格式。它们是结构化的二进制文件，包含井信息、工具信息和井日志数据的表格。与平面 LAS（日志 ASCII 标准）文件相比，它们要复杂得多，也更难以打开。这可能使得处理这些文件变得更加困难，通常需要专用工具来查看和探索其内容。

幸运的是，Equinor 发布了一个名为 [dlisio](https://github.com/equinor/dlisio) 的 Python 库，这使得探索这些文件的过程变得更加容易。

[dlsio](https://github.com/equinor/dlisio) 是由 Equinor ASA 开发的一个 Python 库，用于读取 dlis 文件和 Log Information Standard 79 (LIS79) 文件。开发这个库的主要思想是减少探索和提取这些文件中数据的负担和工作量，而不必完全理解它们的结构。这使得用户可以专注于访问和处理数据。

要获取有关 dlisio 库的更多信息，请查看以下文档

[## dlisio 0.3.7 文档

### 欢迎使用 dlisio。dlisio 是一个用于读取数字日志交换标准（DLIS）v1 的 Python 包。版本 2 存在……

[dlisio.readthedocs.io](https://dlisio.readthedocs.io/en/latest/index.html?source=post_page-----38585157dbac--------------------------------)

在本教程中，我们将看到如何通过将信息和数据转换为一个[pandas 数据框](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html)来访问 dlis 文件的内容，这是一种在数据科学中更为常见的数据格式。

# 导入库

如果你还没有安装 dlisio，你可以在你的 Jupyter Notebook 中使用以下命令直接安装。

```py
!pip install dlisio
```

一旦安装了库，我们可以开始导入必要的库。在本教程中，我们需要从 dlisio 导入`dlis`模块，以及[pandas](https://pandas.pydata.org/docs/index.html)库。

```py
from dlisio import dlis
import pandas as pd
```

# 使用 DLISIO 加载 DLIS 数据文件

一旦导入了所需的库，我们可以使用以下`代码`加载我们的 DLIS 数据。

本教程中使用的数据是从[**NLOG.nl**](https://www.nlog.nl/en/welcome-nlog)下载的，这是一个包含整个荷兰北海地区井筒记录数据的网站。

数据是免费提供下载和使用的。数据许可的完整细节可以在本文末尾找到。

```py
f, *tail = dlis.load('Data/NLOG Data/NPN_TVN-1_23_Dec_2009_E3_Main_SONIC_057PUC.DLIS')
```

你会注意到我们在开始时有两个变量：`f`和`*tail`。这是为了适应 dlis 文件可以包含多个逻辑文件的事实，这些逻辑文件代表了额外的测井过程或在数据采集后处理的其他数据集。

如果我们有多个逻辑文件，第一个将被放入`f`中，任何后续的逻辑文件将被放入`*tail`中。

如果我们想检查第一个逻辑文件的内容，可以调用以下`代码`。

```py
f.describe()
```

这将生成逻辑文件的以下汇总。此汇总包括有关帧和通道（曲线）的信息。它还包括有关测井工具设置、环境和其他重要参数的信息。

![](img/9c2a43489d49c53dc4298d06382af328.png)

从 DLISIO 获取的 DLIS 文件内容的汇总输出。图片由作者提供。

# 处理 DLIS 帧

DLIS 文件中的帧也可以代表不同的测井过程或不同阶段的处理数据。这可以从原始的井筒测量数据到岩石物理解释或高级处理数据。

我们可以通过调用以下内容来访问 DLIS 文件中的帧：

```py
f.frames
```

这将返回一个帧的列表。

```py
[Frame(60B), Frame(10B), Frame(15B)]
```

通过查看上面的帧名称，可能很难确定其中存储的信息和数据。

我们可以遍历每个帧并打印出其属性。然而，为了使视图更美观并创建可以重用的代码，我们可以创建一个生成汇总`pandas`数据框的函数。

该函数遍历 DLIS 文件中的每个帧，提取关键信息并将这些信息放入汇总数据框中。

```py
def frame_summary(dlis_file: dlis) -> pd.DataFrame:
    """
    Generates a summary DataFrame of the frames contained within a given DLIS file.

    This function iterates through the frames and channels in the DLIS file, 
    converting depth values from inches to meters if necessary, and then compiles 
    the information into a DataFrame. The resulting DataFrame contains the frame 
    name, index type, index curve, minimum and maximum index, spacing, direction, 
    number of channels, and channel names for each frame.

    Parameters:
    dlis_file (DLIS): The DLIS file to summarise.

    Returns:
    DataFrame: A DataFrame summarising the frames and channels of the DLIS file.

    """

    temp_dfs = []

    for frame in dlis_file.frames:

        for channel in frame.channels:
            # Get the index units
            if channel.name == frame.index:
                depth_units = channel.units

            # In cases where units are stored in inches, we need to convert to m
            if depth_units == "0.1 in":
                multiplier = 0.00254
            else:
                multiplier = 1

        df = pd.DataFrame(data= [[frame.name,
                                  frame.index_type,
                                  frame.index,
                                  (frame.index_min * multiplier),
                                  (frame.index_max * multiplier),
                                  (frame.spacing * multiplier), 
                                  frame.direction,
                                  len(frame.channels), 
                                  [channel.name for channel in frame.channels]]],
                          columns=['Frame Name', 
                                   'Frame Index Type', 
                                   'Index Curve',
                                   'Index Min',
                                   'Index Max',
                                   'Spacing', 
                                   'Direction',
                                   'Number of Channels', 
                                   'Channel Names'])
        temp_dfs.append(df)

    final_df = pd.concat(temp_dfs)
    return final_df.reset_index(drop=True)
```

当我们传入第一个逻辑文件时，我们会得到以下信息。

这包含了每个帧使用的索引信息（例如时间或深度）、深度范围、间隔、测井方向、测井测量的数量及其名称。

![](img/037ef71f7aef3d120b00801ac558401b.png)

DLIS 文件中帧的 pandas 数据框总结。图片由作者提供。

# 将 DLIS 曲线/通道转换为 Pandas 数据框

从上面可以看到，我们的 dlis 文件中的每个帧都包含通道。这些通道代表了井测量数据。然而，它们可能直接处理起来比较困难。

将 dlis 通道转换为 pandas 数据框可以使数据分析和探索变得更加便捷。默认情况下，dlisio 不会输出数据框。但是，通过几行代码，我们可以轻松地将通道数据转换为数据框。

我们首先调用 pandas 中的`pd.DataFrame`方法，并传入我们的逻辑文件。然后，我们调用该逻辑文件中包含的帧，并通过传入帧的索引位置来访问所需的帧。然后，我们可以调用曲线方法来访问单独的测井曲线。

```py
df = pd.DataFrame(f.frames[1].curves())
```

当我们运行上述代码（假设我们没有多维列）时，我们将得到以下数据框。

![](img/917cd5187187e4528b9a1504c4a4413d.png)

DLIS 文件中存储的曲线的数据框。图片由作者提供。

你会注意到上面的数据框中`TDEP`值似乎非常大。这是因为测量单位是 0.1 英寸。要将其转换为米，我们需要将`TDEP`列乘以 0.00254。

# 处理 dlis 通道中的数组

当我们的通道包含数组数据时，前面的代码将不起作用。如果我们有多维数据，我们将收到一个`数据必须是 1 维`的错误。

处理这一点的一种方法是排除任何包含数组的通道，只创建具有一维数据的数据框。

```py
df = pd.DataFrame()

for frame in f.frames:
    for channel in frame.channels:
        # Check if the channel is 1-dimensional
        if channel.dimension[0] == 1:
            # Get the data for the channel
            data = channel.curves() 

            # Get the data for the channel
            data = channel.curves()

            # Add the channel data to the DataFrame as a new column
            df[channel.name] = pd.Series(data)
```

在运行上述代码后，我们现在可以查看包含所有规则采样测量的`df`数据框。

请注意，你可能会在同一帧内有多个采样率，在转换之前应该彻底探索这一点。

![](img/921bb4d66e7143e4596c2b447241af16.png)

从 DLIS 文件帧创建的 pandas 数据框。图片由作者提供。

由于这个特定的 dlis 文件使用 0.1 英寸索引帧，我们需要将 TDEP 列乘以 0.00254 以转换为米。

```py
df['TDEP'] = df['TDEP'] * 0.00254
```

当我们在计算后查看数据框时，现在我们将深度列转换为公制单位。

![](img/8b0593645bffd945b93d14e8609f20e6.png)

转换深度到米后的数据框。图片由作者提供。

为了整理数据框，我们可以将 TDEP 列按升序排序，以便从顶部的最浅测量到底部的最深测量。

```py
df = df.sort_values(by='TDEP', ascending=True)
```

# 总结

在本文中，我们已经了解了如何加载 dlis 文件，这是一种复杂的二进制格式，用于存储从地下勘探中获得的井日志数据。使用 Equinor 的 dlisio 库，我们可以轻松地将这些文件加载到 Python 中，并探索不同的组件，如帧和通道。

一旦这些数据被加载，我们可以轻松地使用 pandas 创建 dlis 文件内容的摘要数据框，并将井日志数据从通道导出到更易于处理的格式。

要了解更多关于如何处理 DLIS 文件的信息，请查看我之前的文章：

+   [**使用 Python 从 DLIS 加载井日志数据**](https://medium.com/towards-data-science/loading-well-log-data-from-dlis-using-python-9d48df9a23e2)

# 本教程中使用的数据集

来自 NLOG.nl 的数据可以免费下载和使用。数据许可证的详细信息可以在 [**这里**](https://www.nlog.nl/en/disclaimer) 找到，但这里提供了来自知识产权部分的使用总结：

> *NLOG.NL 对通过本网站提供的信息（除了域名、商标权、专利和其他知识产权）不主张任何权利。用户可以在未经 NLOG.NL 事先书面许可或相关方合法同意的情况下，以任何方式复制、下载、披露、分发或简化本网站提供的信息。用户还可以复制、重复、处理或编辑这些信息和/或布局，只要注明 NLOG.NL 为来源。*

*感谢阅读。在离开之前，您应该一定要订阅我的内容，并将我的文章送入您的收件箱。* [***您可以在这里做到这一点！***](https://andymcdonaldgeo.medium.com/subscribe)

*其次，您可以通过注册会员，获得完整的 Medium 体验，并支持成千上万的其他作者和我。它每月只需 $5，您可以全面访问所有精彩的 Medium 文章，还可以通过写作赚钱。*

*如果您使用* [***我的链接***](https://andymcdonaldgeo.medium.com/membership)***注册***，*您将直接通过您的费用的一部分支持我，而不会增加额外费用。如果您这样做，非常感谢您的支持。*
