# 在 Grafana 中创建时间序列图

> 原文：[`towardsdatascience.com/creating-time-series-plots-in-grafana-f9dade30dff4`](https://towardsdatascience.com/creating-time-series-plots-in-grafana-f9dade30dff4)

## 学习如何使用 Python 和 Grafana 绘制动态时间序列图

[](https://weimenglee.medium.com/?source=post_page-----f9dade30dff4--------------------------------)![Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----f9dade30dff4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f9dade30dff4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f9dade30dff4--------------------------------) [魏孟 李](https://weimenglee.medium.com/?source=post_page-----f9dade30dff4--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f9dade30dff4--------------------------------) ·5 分钟阅读·2023 年 2 月 9 日

--

![](img/d9cd7b7c201a9cd6cf930c74708e8d41.png)

图片来源：[Dan Lohmar](https://unsplash.com/@dlohmar?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Grafana 是一个多平台开源分析和交互式可视化 Web 应用程序。如果你进行数据分析，Grafana 是一个宝贵的工具，它允许你使用各种数据源以及不同的内置可视化类型来构建仪表板。

> 你可以从 [`grafana.com/grafana/download`](https://grafana.com/grafana/download) 下载并安装 Grafana。支持的平台包括 Mac、Windows、Linux、Docker 和 ARM。

在这篇文章中，我将向你展示如何构建一个显示特定地点的温度和湿度的时间序列图。

> 对于这篇文章，我将假设你已经熟悉 Grafana 和 MySQL 的基础知识。如果你对 Grafana 不熟悉，可以查看我在 Code Magazine 上的文章 ([`www.codemag.com/Article/2207061/Developing-Dashboards-Using-Grafana`](https://www.codemag.com/Article/2207061/Developing-Dashboards-Using-Grafana))。

[](https://www.codemag.com/Article/2207061/Developing-Dashboards-Using-Grafana?source=post_page-----f9dade30dff4--------------------------------) [## 使用 Grafana 开发仪表板

### 作者：魏孟 李 发表在：CODE Magazine: 2022 - 七月/八月 最后更新：2022 年 8 月 31 日 在我早期的文章中…

www.codemag.com](https://www.codemag.com/Article/2207061/Developing-Dashboards-Using-Grafana?source=post_page-----f9dade30dff4--------------------------------)

# 示例应用

对于这篇文章，我将涉及三个组件：

+   一个 Python 脚本用于持续将值写入 MySQL 服务器。在现实生活中，Python 脚本可能会从传感器读取值。本文中，我将模拟一些随机的温度和湿度值。这些值将每五秒发送到 MySQL 服务器。

+   一个 MySQL 服务器，包含一个数据库，用于存储所有由 Python 脚本发送的值。

+   Grafana 仪表盘从 MySQL 服务器中提取数据。

![](img/5c7f71dcb93c2fe8d9d31492ed259e14.png)

图片由作者提供；Python 的徽标来自 [`commons.wikimedia.org/wiki/File:Python_Windows_source_code_icon_2016.svg`](https://commons.wikimedia.org/wiki/File:Python_Windows_source_code_icon_2016.svg)

MySQL 服务器将有一个名为**WeatherReadings**的数据库，其中包含一个名为**Readings**的表。表的架构如下：

> 本文中的所有图片均由作者创建。

![](img/496fb361395dc0c6da540eb7bae3409e.png)

# 创建数据库和表

以下 SQL 语句在 MySQL 中创建数据库和表：

```py
CREATE DATABASE `WeatherReadings`; 
USE WeatherReadings;
CREATE TABLE `Readings` (
  `datetime` datetime NOT NULL,
  `temperature` float DEFAULT NULL,
  `humidity` float DEFAULT NULL,
  PRIMARY KEY (`datetime`)
) 
```

# 创建 Python 脚本

以下 Python 脚本使用**mysqlclient**包连接到 MySQL 服务器，并将值写入数据库表：

```py
import datetime
import random
import threading
import MySQLdb                                 # conda install mysqlclient

db = MySQLdb.connect (user='user1',            # database user account
                      passwd='password',       # password for user1
                      host='127.0.0.1',        # IP address of MySQL
                      db='WeatherReadings')    # database name

cur = db.cursor()

def insert_record(datetime, temperature, humidity) :    
    try:
        cur.execute("""
            INSERT INTO Readings (datetime, temperature, humidity) VALUES
            (%s, %s, %s)
        """, (datetime, temperature, humidity))
        db.commit()
    except MySQLdb.Error as e:
        print ("Error %d: %s" % (e.args[0], e.args[1]))
        db.rollback()

def update():
    threading.Timer(5.0, update).start()      # call update() every 5s
    insert_record(datetime.datetime.utcnow(), # datetime in UTC
                  random.uniform(20, 39),     # temperature
                  random.uniform(0.7, 0.9))   # humidity

update()
```

上述脚本执行了以下操作：

+   使用 `user1` 账户和“*password*”作为密码连接到 MySQL 服务器的**WeatherReadings**数据库。

+   定义 `insert_record()` 函数，将新的温度和湿度值插入到**读数**表中。

+   `update()` 函数每五秒调用 `insert_record()` 函数，模拟温度和湿度值。

> 注意，当保存日期和时间时，我使用了 utcnow() 函数而不是 `now()` 函数。`utcnow()` 函数返回当前的 UTC（协调世界时）日期和时间。例如，新加坡的时区是 UTC +8，因此如果新加坡当前的日期和时间是 2023–02–08 **10**:07:38，那么 UTC 时间是 2023–02–08 **02**:07:38（减去 8 小时）。存储时间为 UTC 的原因是 Grafana 将根据浏览器的时区自动将 UTC 时间转换为本地时间。

# 配置 Grafana

在 Python 脚本完成之后，现在可以配置 Grafana 了。

## 添加 MySQL 数据源

在 Grafana 中，添加一个新的**MySQL 数据源**并按如下配置：

![](img/9f0fe2e3e42f5a9ee34cb902f545c0b7.png)

## 向仪表盘添加面板

在 Grafana 中创建一个新的仪表盘，并点击**添加新面板**按钮：

![](img/657942338ad00dbd69acc31587398786.png)

使用默认的**时间序列**面板，并使用以下值配置面板：

![](img/0d453db9ab58a036846ddd40cda67551.png)

注意使用的 SQL 语句：

```py
Select
    datetime as time,
    temperature,
    humidity * 100
FROM Readings
```

你需要将 `datetime` 字段设置为 `time`，以便**时间序列**面板能够识别数据作为时间序列。

配置面板的**标题**属性如下：

![](img/e2a1377e43f82c2a89674643051a23fd.png)

按如下配置面板的**图表样式**属性：

![](img/17c22e4cf042725acebed6a0007d6bda.png)

点击**应用**按钮以退出面板并返回到仪表板。现在你应该能看到面板显示温度和湿度读数的时间序列：

![](img/9a80cfc081e0d7586a6aad1c48ad0d33.png)

## 动态更新时间序列

点击显示在刷新按钮旁边的箭头，然后选择**5 秒**：

![](img/54d753844f3556db90d96b5a2646771d.png)

这将导致面板每五秒从 MySQL 数据源获取数据。现在面板应该每五秒刷新一次：

## 如果你喜欢阅读我的文章并且它对你的职业/学习有所帮助，请考虑注册成为 Medium 会员。每月费用为 5 美元，这将为你提供对 Medium 上所有文章（包括我的文章）的无限制访问。如果你使用以下链接注册，我将获得少量佣金（对你没有额外费用）。你的支持意味着我将能够投入更多时间来撰写类似的文章。

[](https://weimenglee.medium.com/membership?source=post_page-----f9dade30dff4--------------------------------) [## 使用我的推荐链接加入 Medium - 韦梦李

### 阅读韦梦李的每一篇文章（以及 Medium 上其他成千上万位作者的文章）。你的会员费直接支持……

weimenglee.medium.com](https://weimenglee.medium.com/membership?source=post_page-----f9dade30dff4--------------------------------)

# 摘要

在这篇文章中，你学会了如何使用 Grafana 和 MySQL 服务器绘制时间序列。在实际应用中，你的数据将不断地发送到数据库服务器，Grafana 将配置为每隔几秒刷新一次数据（由你配置）。
