# 使用 Streamlit 构建 LAS 文件数据探索应用

> 原文：[`towardsdatascience.com/building-a-las-file-data-explorer-app-with-streamlit-347289e0d000`](https://towardsdatascience.com/building-a-las-file-data-explorer-app-with-streamlit-347289e0d000)

## 使用 Python 和 Streamlit 探索 Log ASCII Standard 文件

[](https://andymcdonaldgeo.medium.com/?source=post_page-----347289e0d000--------------------------------)![Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----347289e0d000--------------------------------)[](https://towardsdatascience.com/?source=post_page-----347289e0d000--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----347289e0d000--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----347289e0d000--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----347289e0d000--------------------------------) ·14 min 阅读·2023 年 2 月 3 日

--

![](img/c6d1f16ce8d9cc0c63a877d3f82c55e0.png)

照片由 [Carlos Muza](https://unsplash.com/fr/@kmuza?utm_source=medium&utm_medium=referral) 提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上的

LAS 文件是石油和天然气行业中传输和存储井日志和/或岩石物理数据的标准且简单的方式。该格式在 80 年代末和 90 年代初由 [加拿大井日志学会](https://www.cwls.org/products/#products-las) 开发，旨在标准化和组织数字日志信息。LAS 文件本质上是结构化的 ASCII 文件，包含多个部分，其中有关于井及其数据的信息；因此，它们可以在典型的文本编辑器中轻松查看，如记事本或 TextEdit。

[Streamlit](https://streamlit.io/) 是我最喜欢的 Python 库之一，用于创建快速且易于使用的仪表板或交互式工具。如果你想创建一个应用程序，使你或最终用户无需担心代码，它也非常棒。因此，在本文中，我们将深入了解如何使用 Streamlit 构建 LAS 文件的数据探索应用。

如果你想查看完整的应用演示，请查看下面的短视频。

或者在 GitHub 上探索源代码：

[](https://github.com/andymcdgeo/las_explorer?source=post_page-----347289e0d000--------------------------------) [## GitHub - andymcdgeo/las_explorer: LAS Explorer 是一个 Streamlit 网络应用，允许你理解…

### LAS Explorer 是一个 Streamlit 网络应用，允许你理解 LAS 文件的内容。还包括…

github.com](https://github.com/andymcdgeo/las_explorer?source=post_page-----347289e0d000--------------------------------)

如果你想了解如何在 Python 中处理 LAS 文件，以下文章可能会引起你的兴趣：

+   [使用 Python 加载和显示 LAS 文件中的井日志数据](https://medium.com/@andymcdonaldgeo/loading-and-displaying-well-log-data-b9568efd1d8)

+   [使用 Python 加载多个井日志 LAS 文件](https://medium.com/towards-data-science/loading-multiple-well-log-las-files-using-python-39ac35de99dd)

# 安装和设置 Streamlit

我们应用的第一部分将涉及导入所需的库和模块。

这些是：

+   [Streamlit](https://streamlit.io/)

+   [lasio](https://lasio.readthedocs.io/en/latest/) 用于加载 las 文件

+   [missingno](https://github.com/ResidentMario/missingno) 用于识别缺失数据

+   [pandas](https://pandas.pydata.org/) 用于处理数据框

+   [StringIO](https://docs.python.org/3/library/io.html) 来自 io 以处理 las 文件中的内容

+   [plotly](https://plotly.com/) 用于显示交互式图表

导入这些库后，我们可以在最后添加一行代码，将页面宽度设置为全页，并更改浏览器窗口中的应用标题。

```py
import streamlit as st
import lasio
import pandas as pd
from io import StringIO

# Plotly imports
from plotly.subplots import make_subplots
import plotly.graph_objects as go
import plotly.express as px

st.set_page_config(layout="wide", page_title='LAS Explorer v.0.1')
```

为了检查 Streamlit 是否正常工作，我们可以在终端中运行以下命令：

```py
streamlit run app.py
```

这将打开一个浏览器窗口，显示一个空白的 Streamlit 应用。

![](img/ce56ef96bf1ce36e5180e244365386a2.png)

空白的 Streamlit 应用。图片来源：作者。

# 使用 st.file_uploader 加载 LAS 文件

我们要添加到应用中的第一段代码是调用 `st.sidebar`。这将创建一个位于应用左侧的列，我们将用它来存储我们的导航菜单和文件上传小部件。

```py
st.sidebar.write('# LAS Data Explorer')
st.sidebar.write('To begin using the app, load your LAS file using the file upload option below.')
```

我们可以使用 `st.sidebar.write` 添加一些消息和说明给最终用户。在这个示例中，我们将保持相对简单，提供应用名称和如何开始的消息。

一旦侧边栏到位，我们可以开始实现文件上传器的代码部分。

```py
las_file=None

uploadedfile = st.sidebar.file_uploader(' ', type=['.las'])
las_file, well_data = load_data(uploadedfile)

if las_file:
    st.sidebar.success('File Uploaded Successfully')
    st.sidebar.write(f'<b>Well Name</b>: {las_file.well.WELL.value}',
    unsafe_allow_html=True)
```

为此，我们需要调用 `st.file_uploader`。我们还将限制文件类型为 .las 文件。为了更实用，我们可能还希望包含大写版本的扩展名。

接下来，我们将调用 load data 函数，稍后我们将详细介绍。该函数将设置为返回 `las_file` 作为一个 [lasio](https://lasio.readthedocs.io/en/latest/) las 文件对象，以及 `well_data` 作为包含井日志测量数据的数据框。

随后，我们将检查是否有 las 文件。如果设置为 `None`，则不会发生任何事情；然而，如果文件通过 `load_data` 函数成功加载，则它不会是 `None`，因此会执行下面的代码。

if 函数中的代码本质上显示了一个彩色标注，后跟 las 文件的井名称。

在运行 Streamlit 应用之前，我们需要创建 `load_data` 函数。这将允许我们读取数据并生成 lasio las 文件对象和 pandas 数据框。

```py
@st.cache
def load_data(uploaded_file):
    if uploaded_file is not None:
        try:
            bytes_data = uploaded_file.read()
            str_io = StringIO(bytes_data.decode('Windows-1252'))
            las_file = lasio.read(str_io)
            well_data = las_file.df()
            well_data['DEPTH'] = well_data.index

        except UnicodeDecodeError as e:
            st.error(f"error loading log.las: {e}")
    else:
        las_file = None
        well_data = None

    return las_file, well_data
```

当我们运行 Streamlit LAS 数据浏览器应用时，我们将看到左侧的侧边栏以及文件上传小部件。

![](img/e007770c957c92d2c3c6141ad56f5809.png)

添加侧边栏到 LAS 文件数据浏览器 Streamlit 应用后。图片由作者提供。

然后我们可以点击浏览文件并搜索一个 las 文件。

一旦该文件被加载，我们将看到绿色的提示，表示文件加载成功，接着是文件中包含的井名。

![](img/51bdd3a62af01037c708cb0bb5093e0c.png)

成功读取 LAS 文件与 LAS 数据浏览器 Streamlit 应用。图片由作者提供。

# 向 Streamlit 应用中添加主页

当有人第一次启动 LAS 数据浏览器应用时，展示应用的名称和简要描述会很好。

```py
st.title('LAS Data Explorer - Version 0.2.0')
st.write('''LAS Data Explorer is a tool designed using Python and 
Streamlit to help you view and gain an understanding of the contents 
of a LAS file.''')
st.write('\n')
```

当我们重新运行应用时，现在将看到我们的主页。这可以扩展以包括额外的说明、有关应用的详细信息以及如果出现问题如何联系。

![](img/d1dd6594adfb16e968d7d2fea2d2b605.png)

创建主页后的 LAS 数据浏览器 Streamlit 应用。图片由作者提供。

在构建 Streamlit 应用时，最好将代码拆分成函数，并在适当的时间调用它们。这使得代码更具模块化，更易于导航。

对于我们的主页，我们将上述代码放入一个名为 `home()` 的函数中。

```py
def home():
    st.title('LAS Data Explorer - Version 0.2.0')
    st.write('''LAS Data Explorer is a tool designed using Python and 
    Streamlit to help you view and gain an understanding of the contents 
    of a LAS file.''')
    st.write('\n')
```

# 添加导航单选按钮

在构建 Streamlit 应用时，很容易陷入一个不断添加部分的陷阱，结果是生成一个很长的可滚动网页。

使 Streamlit 应用更具可导航性的一种方法是添加导航菜单。这允许你将内容拆分到多个页面上。

实现这一点的一种方法是使用一系列单选按钮，这些按钮在切换时将更改主界面上显示的内容。

首先，我们需要为导航部分指定一个标题，然后我们必须调用 `st.sidebar.radio` 并传入一个我们希望用户能够导航到的页面列表。

```py
# Sidebar Navigation
st.sidebar.title('Navigation')
options = st.sidebar.radio('Select a page:', 
    ['Home', 'Header Information', 'Data Information', 
     'Data Visualisation', 'Missing Data Visualisation'])
```

当我们运行应用时，我们将看到现在有一个由单选按钮表示的导航菜单。

![](img/6c2ca513cc7fcdf569e9f48ebab903d9.png)

添加了单选按钮导航菜单后的 LAS 数据浏览器。图片由作者提供。

目前，如果点击按钮，什么也不会发生。

我们需要告诉 Streamlit 在进行选择时该做什么。

通过创建如下的 if/elif 语句来实现。当选择了一个选项时，将调用一个特定的函数。

例如，如果用户选择了主页，则会显示先前创建的主页函数。

```py
if options == 'Home':
    home()
elif options == 'Header Information':
    header.header(las_file)
elif options == 'Data Information':
    raw_data(las_file, well_data)
elif options == 'Data Visualisation':
    plot(las_file, well_data)
elif options == 'Missing Data Visualisation':
    missing(las_file, well_data)
```

让我们开始实现其他部分，以便开始显示一些内容。

# 从 LAS 文件中检索井头信息

在每个 las 文件中，顶部有一个包含有关井的信息的部分。这包括井名、国家、操作员等。

![](img/3b4294ef005e261350ab4b7d7fbd200f.png)

Volve 田野的 LAS 文件头示例。图片由作者提供。

为了读取这些信息，我们将创建一个名为`header`的新函数，然后遍历头部中的每一行。

为了防止用户点击头信息单选按钮时出现错误，我们需要检查在加载过程中是否已经创建了 las 文件对象。否则，我们将向用户展示错误。

然后，对于每个头项，我们将显示描述名称（`item.descr`）、助记符（`item.mnemonic`）和相关值（`item.value`）。

```py
def header(las_file):
    st.title('LAS File Header Info')
    if not las_file:
        st.warning('No file has been uploaded')
    else:
        for item in las_file.well:
            st.write(f"<b>{item.descr.capitalize()} ({item.mnemonic}):</b> {item.value}", 
            unsafe_allow_html=True)
```

当应用程序重新运行，并从导航菜单中选择头信息页面时，我们现在会看到相关的井信息。

![](img/71b233ef386247fe9b6f0ddae9f4160c.png)

来自 LAS 文件的井日志头信息。图片由作者提供。

# 检索井日志测量信息

在成功读取头信息后，我们接下来要查看 las 文件中包含了哪些井日志测量。

为此，我们将创建一个简单的函数，名为`raw_data`，它将：

+   遍历 las 文件中的每个测量，写出它的助记符、单位和描述

+   提供测量总数的统计

+   使用 pandas 的`describe`方法为每个测量创建一个统计摘要表

+   创建一个包含所有原始值的数据表

对于一个单一的函数来说，这个工作量很大，可能需要整理一下，但对于这个简单的应用程序，我们将把它们保持在一起。

```py
def raw_data(las_file, well_data):
    st.title('LAS File Data Info')
    if not las_file:
        st.warning('No file has been uploaded')
    else:
        st.write('**Curve Information**')
        for count, curve in enumerate(las_file.curves):
            st.write(f"   {curve.mnemonic} ({curve.unit}): {curve.descr}", 
            unsafe_allow_html=True)
        st.write(f"<b>There are a total of: {count+1} curves present within this file</b>", 
        unsafe_allow_html=True)

        st.write('<b>Curve Statistics</b>', unsafe_allow_html=True)
        st.write(well_data.describe())
        st.write('<b>Raw Data Values</b>', unsafe_allow_html=True)
        st.dataframe(data=well_data)
```

当 Streamlit 应用重新运行时，我们将看到所有与井日志测量相关的信息。

首先，我们有井测量信息和相关统计数据。

![](img/7fb388fbacd25a036973c9e73523bfa7.png)

LAS 井日志测量信息。图片由作者提供。

然后是原始数据值。

![](img/c585ed15258da4b1499594eada13a222.png)

LAS 井日志测量信息。图片由作者提供。

# 使用 Plotly 在 Streamlit 中可视化井日志数据

与任何数据集一样，仅通过分析原始数字很难掌握数据的外观。为了进一步深入，我们可以使用交互式图表。

这些将使最终用户更容易更好地理解数据。

以下代码在 Streamlit 页面上生成多个图表。所有内容都包含在一个函数中，以便在这个应用程序中使用。请记住，每个函数代表 LAS 数据探索器应用中的一个页面。

为了避免使用多个页面，下面的代码将为三种不同的图生成三个展开器：折线图、直方图和散点图（在岩石物理学中也称为交叉图）。

```py
 def plot(las_file, well_data):
    st.title('LAS File Visualisation')

    if not las_file:
        st.warning('No file has been uploaded')

    else:
        columns = list(well_data.columns)
        st.write('Expand one of the following to visualise your well data.')
        st.write("""Each plot can be interacted with. To change the scales of a plot/track, click on the left hand or right hand side of the scale and change the value as required.""")
        with st.expander('Log Plot'):    
            curves = st.multiselect('Select Curves To Plot', columns)
            if len(curves) <= 1:
                st.warning('Please select at least 2 curves.')
            else:
                curve_index = 1
                fig = make_subplots(rows=1, cols= len(curves), subplot_titles=curves, shared_yaxes=True)

                for curve in curves:
                    fig.add_trace(go.Scatter(x=well_data[curve], y=well_data['DEPTH']), row=1, col=curve_index)
                    curve_index+=1

                fig.update_layout(height=1000, showlegend=False, yaxis={'title':'DEPTH','autorange':'reversed'})
                fig.layout.template='seaborn'
                st.plotly_chart(fig, use_container_width=True)

        with st.expander('Histograms'):
            col1_h, col2_h = st.columns(2)
            col1_h.header('Options')

            hist_curve = col1_h.selectbox('Select a Curve', columns)
            log_option = col1_h.radio('Select Linear or Logarithmic Scale', ('Linear', 'Logarithmic'))
            hist_col = col1_h.color_picker('Select Histogram Colour')
            st.write('Color is'+hist_col)

            if log_option == 'Linear':
                log_bool = False
            elif log_option == 'Logarithmic':
                log_bool = True

            histogram = px.histogram(well_data, x=hist_curve, log_x=log_bool)
            histogram.update_traces(marker_color=hist_col)
            histogram.layout.template='seaborn'
            col2_h.plotly_chart(histogram, use_container_width=True)

        with st.expander('Crossplot'):
            col1, col2 = st.columns(2)
            col1.write('Options')

            xplot_x = col1.selectbox('X-Axis', columns)
            xplot_y = col1.selectbox('Y-Axis', columns)
            xplot_col = col1.selectbox('Colour By', columns)
            xplot_x_log = col1.radio('X Axis - Linear or Logarithmic', ('Linear', 'Logarithmic'))
            xplot_y_log = col1.radio('Y Axis - Linear or Logarithmic', ('Linear', 'Logarithmic'))

            if xplot_x_log == 'Linear':
                xplot_x_bool = False
            elif xplot_x_log == 'Logarithmic':
                xplot_x_bool = True

            if xplot_y_log == 'Linear':
                xplot_y_bool = False
            elif xplot_y_log == 'Logarithmic':
                xplot_y_bool = True

            col2.write('Crossplot')

            xplot = px.scatter(well_data, x=xplot_x, y=xplot_y, color=xplot_col, log_x=xplot_x_bool, log_y=xplot_y_bool)
            xplot.layout.template='seaborn'
            col2.plotly_chart(xplot, use_container_width=True)
```

一旦上述代码实现后，我们可以看到 LAS 文件可视化页面，包含三个可展开的框。

![](img/b2277d27b9176219e7f258401c21e79d.png)

在地球科学和岩石物理学中，我们经常在折线图上绘制数据——通常称为日志图。y 轴通常表示井眼深度，而 x 轴表示我们希望可视化的数据。这使我们可以轻松地可视化这些测量数据随深度的趋势和模式。

在日志图部分，我们可以从数据框中选择特定列，并在交互式 Plotly 图表中显示它们。

![](img/fa2de2c2f4c290464a6c3795e6f31432.png)

使用 Plotly 创建的井日志图，并显示在 LAS 数据探索者 Streamlit 应用中。图片由作者提供。

直方图显示数据分布，并允许我们在一个小而简洁的图表中包含大量数据。

在直方图部分，我们有一些基本选项。我们可以从数据框中选择一列进行显示，并决定是否以线性或对数方式显示。

最后，我们可以使用 Streamlit 的颜色选择器。[这允许你为直方图选择颜色](https://medium.com/p/7929973393ea)，可以增强你在演示和报告中的可视化效果。

![](img/dc03b53c1ec75ba918ff8c6971dffdb4.png)

使用 Plotly 在 LAS 数据探索者 Streamlit 应用中创建的直方图。图片由作者提供。

[散点图（交叉图）通常在岩石物理学和数据科学中用于比较两个变量。](https://medium.com/towards-data-science/scatterplot-creation-and-visualisation-with-matplotlib-in-python-7bca2a4fa7cf) 从这种图表中，我们可以了解两个变量之间是否存在关系以及这种关系的强度。

在数据可视化页面的交叉图部分，我们可以选择 x 轴和 y 轴变量，以及一个第三变量，用于数据的颜色编码。

最后，我们可以将 x 轴和 y 轴设置为线性刻度或对数刻度。

![](img/348f7963bd9b23693ef3cc99d207eaeb.png)

使用 Plotly 在 LAS 数据探索者 Streamlit 应用中创建的散点图/交叉图。图片由作者提供。

# 识别井日志测量中的缺失数据

缺失数据是我们在处理数据集时面临的最常见的数据质量问题之一。它可能因多种原因而缺失，从传感器故障到不当和可能粗心的数据管理。

在处理数据集时，识别缺失数据并理解数据缺失的根本原因是至关重要的。对数据缺失原因的正确理解是开发务实解决方案的关键，尤其是许多机器学习算法无法处理缺失值。

在 Python 中，我们可以使用 pandas 的 `describe` 函数提供的文本数据摘要。虽然这很有用，但在图表中可视化缺失数据值通常更有帮助。这使我们能够轻松识别可能在基于文本的摘要中不明显的模式和关系。

为了创建数据完整性的交互式图表，我们可以利用 [Plotly 库](https://plotly.com/)。下面的代码设置了 LAS 数据浏览器应用中的缺失数据可视化页面。

首先，我们检查是否有有效的 LAS 文件；如果有，我们开始创建页面并添加一些说明文本。

接下来，我们为用户提供一个选项，以选择数据框中的所有数据或选择特定列。在这旁边，我们允许用户更改图表中条形的颜色。

然后，我们继续根据用户选择绘制数据。

```py
 def missing(las_file, well_data):
    st.title('LAS File Missing Data')

    if not las_file:
        st.warning('No file has been uploaded')

    else:
        st.write("""The following plot can be used to identify the depth range of each of the logging curves.
         To zoom in, click and drag on one of the tracks with the left mouse button. 
         To zoom back out double click on the plot.""")

        data_nan = well_data.notnull().astype('int')
        # Need to setup an empty list for len check to work
        curves = []
        columns = list(well_data.columns)
        columns.pop(-1) #pop off depth

        col1_md, col2_md= st.columns(2)

        selection = col1_md.radio('Select all data or custom selection', ('All Data', 'Custom Selection'))
        fill_color_md = col2_md.color_picker('Select Fill Colour', '#9D0000')

        if selection == 'All Data':
            curves = columns
        else:
            curves = st.multiselect('Select Curves To Plot', columns)

        if len(curves) <= 1:
            st.warning('Please select at least 2 curves.')
        else:
            curve_index = 1
            fig = make_subplots(rows=1, cols= len(curves), subplot_titles=curves, shared_yaxes=True, horizontal_spacing=0.02)

            for curve in curves:
                fig.add_trace(go.Scatter(x=data_nan[curve], y=well_data['DEPTH'], 
                    fill='tozerox',line=dict(width=0), fillcolor=fill_color_md), row=1, col=curve_index)
                fig.update_xaxes(range=[0, 1], visible=False)
                fig.update_xaxes(range=[0, 1], visible=False)
                curve_index+=1

            fig.update_layout(height=700, showlegend=False, yaxis={'title':'DEPTH','autorange':'reversed'})
            # rotate all the subtitles of 90 degrees
            for annotation in fig['layout']['annotations']: 
                    annotation['textangle']=-90
            fig.layout.template='seaborn'
            st.plotly_chart(fig, use_container_width=True)
```

当我们访问 LAS 数据浏览器的这一页时，我们会看到一个互动的 Plotly 图表，如下所示。如果用户选择了“所有数据”，则所有列都会显示出来。

![](img/05ec22356a228b728c275d467e1467f0.png)

使用 Streamlit 在 Plotly 图表中显示 pandas 数据框的所有列。图片由作者提供。

如果用户选择了“自定义选择”，则他们可以直接从数据框中选择列。

![](img/f58d870090025380fb3466a42ec3e98a.png)

使用 Streamlit 多选框从数据框中选择列，并在 Plotly 图表中显示它们。图片由作者提供。

如果你想查看使用 Python 识别缺失值的其他方法，请查看下面的文章：

+   使用 missingno Python 库识别和可视化机器学习前的缺失数据

# 摘要

在本文中，我们展示了如何使用 Streamlit 和 Python 构建一个用于探索 LAS 文件的应用程序。虽然这是一个基础应用，但它可以作为查看原始 LAS 文件的一种有用替代方案。还可以添加更多功能来编辑文件或将其转换为其他标准格式。可能性无穷无尽！

# 本教程中使用的数据

本教程中使用的数据是 Equinor 于 2018 年发布的 Volve 数据集的一个子集。数据集的完整详细信息，包括许可证，可以在下面的链接中找到。

[## Volve 田野数据集](https://www.equinor.com/energy/volve-data-sharing?source=post_page-----347289e0d000--------------------------------)

### Equinor 已正式提供了一整套来自北海油田的数据，用于研究、学习等……

[www.equinor.com](https://www.equinor.com/energy/volve-data-sharing?source=post_page-----347289e0d000--------------------------------)

Volve 数据许可证基于 CC BY 4.0 许可证。许可证协议的完整详细信息可以在这里找到：

[`cdn.sanity.io/files/h61q9gi9/global/de6532f6134b9a953f6c41bac47a0c055a3712d3.pdf?equinor-hrs-terms-and-conditions-for-licence-to-data-volve.pdf`](https://cdn.sanity.io/files/h61q9gi9/global/de6532f6134b9a953f6c41bac47a0c055a3712d3.pdf?equinor-hrs-terms-and-conditions-for-licence-to-data-volve.pdf=)

*感谢阅读。在离开之前，你应该肯定地订阅我的内容，并将我的文章发送到你的收件箱。* [***你可以在这里做到这一点！***](https://andymcdonaldgeo.medium.com/subscribe)*另外，你还可以* [***注册我的通讯***](https://fabulous-founder-2965.ck.page/2ca286e572) *以便免费获取额外的内容。*

*其次，你可以通过注册会员，获得完整的 Medium 体验，支持我和其他成千上万的作家。每月只需花费 5 美元，你即可全面访问所有精彩的 Medium 文章，还能有机会通过写作赚钱。*

*如果你使用* [***我的链接***](https://andymcdonaldgeo.medium.com/membership)***，*** *你将直接通过你的费用的一部分支持我，而且不会额外花费你更多。如果你这样做了，非常感谢你的支持。*
