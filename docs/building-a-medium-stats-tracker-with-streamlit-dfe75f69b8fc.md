# 使用 Streamlit 构建 Medium 统计跟踪器

> 原文：[`towardsdatascience.com/building-a-medium-stats-tracker-with-streamlit-dfe75f69b8fc`](https://towardsdatascience.com/building-a-medium-stats-tracker-with-streamlit-dfe75f69b8fc)

## 使用 Python Streamlit 库跟踪和监控 Medium 统计数据

[](https://andymcdonaldgeo.medium.com/?source=post_page-----dfe75f69b8fc--------------------------------)![Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----dfe75f69b8fc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dfe75f69b8fc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dfe75f69b8fc--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----dfe75f69b8fc--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dfe75f69b8fc--------------------------------) ·9 分钟阅读·2023 年 2 月 27 日

--

![](img/d22a37957e1a52e038e3944f7b5719a4.png)

照片由 [Markus Winkler](https://unsplash.com/es/@markuswinkler?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我已经在 Medium 上写作了一段时间，最近开始使用 Excel 跟踪我的统计数据。但最近，我在考虑使用 Streamlit 构建一个应用，以提供更好的体验。所以我想，为什么不试试，并在构建的过程中写一篇文章呢？

在本文中，我们将介绍构建一个简单的 Streamlit 仪表板的过程，并提供一种方法，让最终用户通过表单输入他们的最新统计数据。到文章末尾，我们将拥有一个可以进一步构建并在以后变得更加完善的基本仪表板。

![](img/de3327290d6f9e8d5748484e610df3db.png)

用于查看和分析 Medium 统计数据的 Streamlit 仪表板。图片由作者提供

如果你刚刚开始使用 Streamlit，你可能想查看我下面的文章。

[](/getting-started-with-streamlit-web-based-applications-626095135cb8?source=post_page-----dfe75f69b8fc--------------------------------) ## 入门 Streamlit 网络应用

### 温和地介绍创建 Streamlit 网络应用

towardsdatascience.com [](/getting-started-with-streamlit-5-functions-you-need-to-know-when-starting-out-b35ed7d872b9?source=post_page-----dfe75f69b8fc--------------------------------) ## 入门 Streamlit：开始时需要知道的 5 个函数

### 利用这 5 个函数简化你的 Streamlit 学习

[towardsdatascience.com

# 导入库并设置 CSV 文件

第一步是导入我们的关键 Python 库：[streamlit](https://streamlit.io/)、[pandas](https://pandas.pydata.org/)和[plotly_express](https://plotly.com/)。接着，我们需要将应用程序配置为默认的宽屏显示，同时添加应用程序的标题。

```py
import streamlit as st
import pandas as pd
import plotly_express as px

st.set_page_config(layout='wide')

st.header('Medium Stats Dashboard')
```

接下来，我们需要在与应用程序相同的目录中创建一个新的 CSV 文件，并将其命名为`medium_stats`。

我们需要打开文件并填写四列。我们可以通过代码完成此操作，但这是一种快速且简单的方式来设置文件。

![](img/0b4aa3f958567cf4cd36080c96fca776.png)

Streamlit 中的 Medium Stats 仪表板的 CSV 起始文件。图像由作者提供。

# 从 Streamlit 应用程序向 CSV 文件中写入统计数据的功能

我们将设置的第一个功能是允许用户在仪表板中输入他们的统计数据并更新 CSV 文件。

```py
def update_stats_csv(date, followers, email_subs, ref_members):
    with open('medium_stats.csv', 'a+') as f:
        f.write(f'\n{date},{followers},{email_subs},{ref_members}')
```

这段代码将打开 CSV 文件，并在其上追加一行，其中每个统计数据由逗号分隔。

接下来，我们需要创建一个简单的表单，允许用户输入统计数据。这是通过 Streamlit 中的`st.form`功能实现的。然后我们可以使用 with 语句，开始添加我们希望在仪表板中跟踪的每个关键统计数据的输入框数量。

我们还会将其添加到侧边栏中，以免主显示区域显得杂乱。

```py
with st.sidebar.form('Enter the latest stats from your Medium dashboard'):

    date = st.date_input('Date')

    follower_num = st.number_input('Followers', step=1)
    email_subs = st.number_input('Email Subscribers', step=1)
    ref_members = st.number_input('Referred Members', step=1)

    submit_stats = st.form_submit_button('Write Stats to CSV')

    if submit_stats:
        update_stats_csv(date, follower_num, email_subs, ref_members)
        st.info(f'Stats for {date} have been added to the csv file')
```

当我们重新运行 Streamlit 应用时，应该会在侧边栏看到以下表单。

![](img/a1b77aa3efd2bd2033228c4d3ae69f17.png)

Streamlit 侧边栏中的 Medium Stats 录入表单。图像由作者提供。

当我们选择一个日期并添加几个数字时，我们可以点击`Write Stats to CSV`按钮，将值提交到 CSV 文件中。我们还会看到一个弹出提示，表示统计数据已被添加到文件中。

![](img/d4b8074c7c6dc83eaaff93f00e3bf8b5.png)

添加一个月统计数据后的 Streamlit Medium Stats 录入表单。图像由作者提供。

如果我们返回到 CSV 文件中，将会看到文件中新增了一行，我们刚刚添加的统计数据就在其中。

![](img/035fa7142690e068a849ad84ad18ba01.png)

从 Streamlit 应用程序写入统计数据后的 CSV 文件。图像由作者提供。

很好！我们确认可以直接将统计数据添加到 CSV 文件中。现在我们可以继续加载 CSV 文件，并开始创建所有月份的图表。

# 读取包含所有统计数据的 CSV 文件

在 Streamlit 应用的主要部分，我们将添加一个展开器。当点击并展开它时，我们将能够在 pandas 数据框中查看 CSV 文件中的原始数据。

```py
df = pd.read_csv('medium_stats.csv')

with st.expander('View Raw Data'):
    st.dataframe(df)
```

如果我们返回到 Streamlit 应用中，我们将看到展开窗口，并查看 CSV 文件中的所有统计数据。

![](img/db09249d554579f907b1a1f6b0e5fd0b.png)

使用 Streamlit 的 st.expander 查看来自统计 CSV 文件的原始数据。图片由作者提供。

## 创建 Plotly 图表以查看 Medium 统计数据

现在我们已经成功将所有统计数据加载到 Streamlit 中，我们可以开始制作一些互动图表，以帮助我们理解趋势。对于这个应用程序，我们将使用 Plotly Express 创建图表。

使用起来非常简单，并且只需很少的代码就能获得强大且互动的图表。

我们将创建的第一个图表是每月关注者数量的线图。

```py
fig_followers = px.line(df, x=df.date, y='followers', title='Followers')
st.plotly_chart(fig_followers, use_container_width=True)
```

当我们回到 Streamlit 应用程序时，我们现在可以看到我们的图表。

![](img/644fa219eb8bda7bfc17fddd420addca.png)

Streamlit 中的基础 Plotly Express 图表。图片由作者提供。

我们的文件中只有一个数据点，因此在线图中没有可查看的内容。为了解决这个问题，我们需要在 CSV 文件中添加更多数据。

在下面的示例中，我添加了从八月到二月底的多个月份。

![](img/9de1b145723f7db8b3f7cf5aa131b14e.png)

添加多个日期后的 medium_stats CSV 文件。图片由作者提供。

现在我们可以回到 Streamlit 应用程序，查看 Plotly Express 线图。

![](img/0d9eb1b04f5b0b134da5fc8eadc14689.png)

Plotly Express 线图显示 Medium 关注者随时间的变化。图片由作者提供。

在我们的数据集中，我们跟踪了另外两个统计数据：电子邮件订阅者数量和推荐成员数量。我们将这两个数据并排显示在两个柱状图中。

为此，我们首先需要使用 `st.columns()` 函数创建两列，然后使用 `px.bar` 来设置这两个柱状图。

为了确保图表填满每一列，我们需要包括 `use_container_width` 参数并将其设置为 `True`。

```py
plot_col1, plot_col2 = st.columns(2)

fig_subscribers = px.bar(df, x=df.date, y='email_subs', title='Email Subscribers')
plot_col1.plotly_chart(fig_subscribers, use_container_width=True)

fig_subscribers = px.bar(df, x=df.date, y='referred_members', title='Referred Members')
plot_col2.plotly_chart(fig_subscribers, use_container_width=True)
```

我们的应用程序现在有了两个额外的柱状图，并且我们可以很好地概览 Medium 统计数据随时间的变化。

![](img/c1e8a9c140e1bf160b96eeb7117f2c18.png)

Medium 统计仪表板，包括跟踪关注者数、电子邮件订阅者和推荐成员的图表。图片由作者提供。

随着我们开始向 CSV 文件中添加更多日期，我们可能需要考虑引入日期过滤器或按年分组数据，尤其是当我们在平台上待了很长时间时。

# 将最新月份的指标添加到 Streamlit 应用程序中

拥有互动图表是很棒的，然而，如果我们真的想知道当前月份的关注者数或电子邮件订阅者数，我们必须查看图表并尝试解决这个问题。我们需要为每个图表进行这项工作。

一种替代且更快捷的方法是使用 Streamlit 的指标将最新月份的统计数据作为突出数字显示在仪表板上。

如果我们假设数据框中的最后一行是最新月份，我们可以使用 pandas 的 `iloc` 函数提取每一列的最后值。

```py
def get_latest_monthly_metrics(df):
    # Assumes the last row is the latest entry

    df['date'] = pd.to_datetime(df['date'])
    df['month'] = df['date'].dt.month_name()
    latest_month = df['month'].iloc[-1]
    latest_follower_count = df['followers'].iloc[-1]
    latest_sub_count = df['email_subs'].iloc[-1]
    latest_refs_count = df['referred_members'].iloc[-1]

    return latest_month, latest_follower_count, latest_sub_count, latest_refs_count
```

我们可以通过确保数据始终按日期排序来使这段代码更健壮。

下一步是通过调用 `get_latest_monthly_metrics()` 函数来获取最新统计数据，然后将这些统计数据传递给 Streamlit 的 `metric` 调用，如下所示：

```py
month, followers, subs, refs = get_latest_monthly_metrics(df)

st.write(f'### Medium Stats for {month}')

col1, col2, col3 = st.columns(3)
col1.metric('Followers', followers)
col2.metric('Email Subscribers', subs)
col3.metric('Referred Members', refs)
```

当我们回到应用时，我们现在可以在应用顶部显著地看到最新统计数据。

![](img/0bb56c509ffd3ddb310d1bf62285bb83.png)

在包含 Streamlit 指标后的 Medium Stats 仪表盘。图片由作者提供。

Streamlit 指标函数的一个优点是我们可以包含每个指标的变化量。

为了准备我们的数据，我们可以创建一个函数，为每个统计数据的 dataframe 添加一个差异列，如下所示：

```py
def calculate_differences(df):
    for x in ['followers', 'email_subs', 'referred_members']:
        df[f'{x}_increase'] = df[x].diff()
    return df
```

然后我们需要更新 `get_latest_monthly_metrics()` 函数，以考虑新的列。我们将返回每个最新的统计数据作为包含两个值的列表：一个是实际的统计数据值，另一个是变化量或增量。

```py
def get_latest_monthly_metrics(df):
    # Assumes the last row is the latest entry

    df['date'] = pd.to_datetime(df['date'])
    df['month'] = df['date'].dt.month_name()
    latest_month = df['month'].iloc[-1]
    latest_follower_count = [df['followers'].iloc[-1],
                             df['followers_increase'].iloc[-1]]

    latest_sub_count = [df['email_subs'].iloc[-1], 
                        df['email_subs_increase'].iloc[-1]]

    latest_refs_count = [df['referred_members'].iloc[-1], 
                         df['referred_members_increase'].iloc[-1]]

    return latest_month, latest_follower_count, latest_sub_count, latest_refs_count
```

最后，我们更新了对这个函数创建的变量的调用，以便引用每个列表中的正确值：

```py
col1, col2, col3 = st.columns(3)
col1.metric('Followers', followers[0], delta=round(followers[1]))
col2.metric('Email Subscribers', subs[0], delta=round(subs[1]))
col3.metric('Referred Members', refs[0], delta=round(refs[1]))
```

当我们回到 Streamlit 应用时，我们现在将拥有每个统计数据的增量。

![](img/c1d74f442fdea6cd16548bbb8d0f60dc.png)

Medium Stats 仪表盘，正确显示了过去一个月和当前月之间的差异。图片由作者提供。

就这样——一个基本的 Streamlit 仪表盘，用于可视化我们 Medium 账户的关键统计数据。

# Medium Stats Streamlit 应用的完整代码

如果你想复制这个仪表盘并进行一些操作，这里是应用的完整代码。

```py
import streamlit as st
import pandas as pd
import plotly_express as px

st.set_page_config(layout='wide')

st.header('Medium Stats Dashboard')

#Main Functions
def update_stats_csv(date, followers, email_subs, ref_members):
    with open('medium_stats.csv', 'a+') as f:
        f.write(f'\n{date},{followers},{email_subs},{ref_members}')

def calculate_differences(df):
    for x in ['followers', 'email_subs', 'referred_members']:
        df[f'{x}_increase'] = df[x].diff()
    return df

def get_latest_monthly_metrics(df):
    # Assumes the last row is the latest entry

    df['date'] = pd.to_datetime(df['date'])
    df['month'] = df['date'].dt.month_name()
    latest_month = df['month'].iloc[-1]
    latest_follower_count = [df['followers'].iloc[-1],
                             df['followers_increase'].iloc[-1]]

    latest_sub_count = [df['email_subs'].iloc[-1], 
                        df['email_subs_increase'].iloc[-1]]

    latest_refs_count = [df['referred_members'].iloc[-1], 
                         df['referred_members_increase'].iloc[-1]]

    return latest_month, latest_follower_count, latest_sub_count, latest_refs_count

#Sidebar
with st.sidebar.form('Enter the latest stats from your Medium dashboard'):

    date = st.date_input('Date')

    follower_num = st.number_input('Followers', step=1)
    email_subs = st.number_input('Email Subscribers', step=1)
    ref_members = st.number_input('Referred Members', step=1)

    submit_stats = st.form_submit_button('Write Stats to CSV')

    if submit_stats:
        update_stats_csv(date, follower_num, email_subs, ref_members)
        st.info(f'Stats for {date} have been added to the csv file')

# Main page
df = pd.read_csv('medium_stats.csv')

with st.expander('View Raw Data'):
    st.dataframe(df)

df = calculate_differences(df)

month, followers, subs, refs = get_latest_monthly_metrics(df)

st.write(f'### Medium Stats for {month}')

col1, col2, col3 = st.columns(3)
col1.metric('Followers', followers[0], delta=round(followers[1]))
col2.metric('Email Subscribers', subs[0], delta=round(subs[1]))
col3.metric('Referred Members', refs[0], delta=round(refs[1]))

fig_followers = px.line(df, x=df.date, y='followers', title='Followers')
st.plotly_chart(fig_followers, use_container_width=True)

plot_col1, plot_col2 = st.columns(2)

fig_subscribers = px.bar(df, x=df.date, y='email_subs', title='Email Subscribers')
plot_col1.plotly_chart(fig_subscribers, use_container_width=True)

fig_subscribers = px.bar(df, x=df.date, y='referred_members', title='Referred Members')
plot_col2.plotly_chart(fig_subscribers, use_container_width=True)
```

# 总结

在本文中，我们看到如何利用我们的 Medium 统计数据来创建直接在 Streamlit 内部的交互式图表。这是一个基本的应用，但通过样式和更多功能（如包括 Medium 会员收入），可以变得更好。

通过 Medium API 自动更新数据是非常好的。然而，我不相信所有的统计数据都可以通过 API 访问。

*感谢阅读。在你离开之前，你应该订阅我的内容，将我的文章发送到你的收件箱。* [***你可以在这里做到这一点！***](https://andymcdonaldgeo.medium.com/subscribe)*或者，你可以* [***注册我的新闻通讯***](https://fabulous-founder-2965.ck.page/2ca286e572) *以免费获取额外的内容。*

*其次，你可以通过注册会员来获得完整的 Medium 体验，支持我和成千上万的其他作家。这仅需每月$5，你将可以访问所有精彩的 Medium 文章，还可以通过写作赚取收入。如果你通过* [***我的链接***](https://andymcdonaldgeo.medium.com/membership)***注册***，*你将通过你的费用的一部分直接支持我，而且不会额外增加费用。如果你这样做了，非常感谢你的支持！*
