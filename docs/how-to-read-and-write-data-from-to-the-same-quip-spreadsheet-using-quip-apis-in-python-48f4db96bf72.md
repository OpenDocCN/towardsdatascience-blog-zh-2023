# 如何使用Quip Python APIs从/到相同的Quip电子表格读取和写入数据

> 原文：[https://towardsdatascience.com/how-to-read-and-write-data-from-to-the-same-quip-spreadsheet-using-quip-apis-in-python-48f4db96bf72?source=collection_archive---------8-----------------------#2023-11-17](https://towardsdatascience.com/how-to-read-and-write-data-from-to-the-same-quip-spreadsheet-using-quip-apis-in-python-48f4db96bf72?source=collection_archive---------8-----------------------#2023-11-17)

## 我们分析师经常被要求提供一种解决方案，使最终用户能够提供其输入，这些输入随后可以用作最终分析解决方案中的覆盖/附加上下文。

[](https://ishagarg2010.medium.com/?source=post_page-----48f4db96bf72--------------------------------)[![Isha Garg](../Images/bb9632981e38c7bb4f3df7f812e548e4.png)](https://ishagarg2010.medium.com/?source=post_page-----48f4db96bf72--------------------------------)[](https://towardsdatascience.com/?source=post_page-----48f4db96bf72--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----48f4db96bf72--------------------------------) [Isha Garg](https://ishagarg2010.medium.com/?source=post_page-----48f4db96bf72--------------------------------)

·

[跟进](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F48cdabd739e9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-read-and-write-data-from-to-the-same-quip-spreadsheet-using-quip-apis-in-python-48f4db96bf72&user=Isha+Garg&userId=48cdabd739e9&source=post_page-48cdabd739e9----48f4db96bf72---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----48f4db96bf72--------------------------------) ·7 min read·Nov 17, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F48f4db96bf72&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-read-and-write-data-from-to-the-same-quip-spreadsheet-using-quip-apis-in-python-48f4db96bf72&user=Isha+Garg&userId=48cdabd739e9&source=-----48f4db96bf72---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F48f4db96bf72&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-read-and-write-data-from-to-the-same-quip-spreadsheet-using-quip-apis-in-python-48f4db96bf72&source=-----48f4db96bf72---------------------bookmark_footer-----------)![](../Images/3a4b61ac25e63da7826df3dcacd6f1dc.png)

[Chris Ried](https://unsplash.com/@cdr6934?utm_source=medium&utm_medium=referral)的照片在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上发布

让我们以一个电子商务购物应用程序为例。系统中有一个逻辑，会在供应商收到 100 个负面客户评分后将其列入黑名单。现在，可能会出现负面评分是由于应用内体验或配送/退货体验引起的情况。因此，为了保持公平，供应商可以选择每六个月对黑名单提出异议。为了方便本文讨论，我们假设审批/拒绝会记录在离线电子表格中。

所有新的撤销黑名单申请在一周内会被导出到一个电子表格，并发送给团队进行审查。团队会审查数据并批准或拒绝这些申请。然后，他们会将数据返回系统进行更新。这是每周进行的工作。

现在，这些手动干预的数据需要被添加到系统中。可以有多种方法来实现。用户可以将他们的数据上传到一个 s3 桶中，然后可以安排将其读取到数据库中。或者，我们可以使用 quip，这样所有个人可以实时更新同一个电子表格，然后可以按照固定的节奏将其上传到数据库中。

> Quip 是一款协作软件，允许多人实时编辑文档和电子表格，用户可以自由选择任何终端客户端——桌面/移动设备。

在这篇文章中，我将展示如何将一个 quip 电子表格自动化，以读取用户输入的数据，上传到数据库表中，然后将新的数据写回到同一个电子表格。我将使用 Redshift 作为这次练习的数据库。

这个任务可以分为两个独立的部分。首先，从 Quip 电子表格读取数据并将其存储在数据库中的表中。其次，我们将对这些数据进行一些操作或检查，并与数据库中现有的数据进行联接，然后将处理后的数据写入到已经存在的 quip 电子表格中。我们将分别讨论这两个情况，以便如果您只想读取或只想写入，这篇文章也能帮助您完成这项工作。让我们来看第一个部分。

# 第 1 部分 — 从 Quip 电子表格读取数据并将其写入数据库中的表。

**步骤 1: 获取访问令牌以通过 Quip API 连接到 Quip。**

我们需要生成一个访问令牌，以提供对个人 Quip 账户的 API 访问权限。要生成个人访问令牌，请访问此页面：[https://quip.com/dev/token](https://quip.com/dev/token)。如果您有企业 SSO 启用的 Quip 账户，那么 URL 会有所不同，例如 — [https://quip-corporate.com/dev/token](https://quip-corporate.com/dev/token)

![](../Images/278d8b150a4f1c90012c4fd343cc5082.png)

一旦您点击上方的获取个人访问令牌按钮，您将获得一个令牌，我们将在后续部分使用该令牌通过 API 访问 quip 电子表格。

**步骤 2: 导入库**

首先，让我们导入所需的库。在这一部分，我们主要使用 *quipclient* 和 *pandas_redshift* 库。

```py
import quipclient as quip
import pandas as pd
import numpy as np
import pandas_redshift as pr
import boto3
from datetime import datetime as dt
import psycopg2
import warnings
warnings.filterwarnings('ignore')
import socket
```

**步骤 3: 使用令牌 ID 连接到 Quip**

QuipClient API 需要基本 URL、线程 ID 和访问令牌来访问任何文件。基本 URL 是你尝试读取（或写入）的 quip 服务器的 URL。在公司账户的情况下，这通常会在 URL 中包含公司名称。线程 ID 是 Quip 服务器上所有文件的唯一标识符。它是目标文件的基本 URL 之后的字母数字值，在这个例子中是一个电子表格。

如果文件的 URL 看起来像 — https://platform.quip-XXXXXXXXX.com/abcdefgh1234/，则基本 URL 将是 — https://platform.quip-XXXXXXXXX.com，thread_id 将是 — abcdefgh1234。

访问令牌是我们在第 1 步中刚刚生成的。

现在，使用 QuipClient API，我们通过访问令牌和 thread_id 连接到 URL。

```py
##################################### 
# declaring Quip variables 
#####################################
baseurl = 'https://platform.quip-XXXXXXXXX.com'
access_token = "************************************************************************" 
thread_id = 'abcdefgh1234'

##########################################
# connecting to Quip
##########################################
client = quip.QuipClient(access_token = access_token, base_url=baseurl)
rawdictionary = client.get_thread(thread_id)
```

**第 4 步：从 quip 中读取数据到数据框**

第 3 步中的 *rawdictionary* 输出返回一个 HTML 列表。Pandas 函数 *read_html* 将帮助将 HTML 部分读取到数据框 dfs 中。因此，dfs 是一个数据框的列表。这个列表中的每个数据框包含来自 quip 电子表格中每个选项卡的数据。在这个例子中，我们只考虑最后一个选项卡的数据。因此，使用索引 -1 来获取 *raw_df* 中的最后一个数据框。

```py
##########################################
# cleaning the data and creating a dataframe
##########################################
dfs=pd.read_html(rawdictionary['html'])
raw_df = dfs[-1]
raw_df.columns=raw_df.iloc[0] #Make first row as column header
raw_df=raw_df.iloc[1:,1:] #After the above step, the 1st two rows become duplicate. Delete the 1st row.
raw_df=raw_df.replace('\u200b', np.nan) #Replacing empty cells with nan 
```

**第 5 步：连接到数据库以将数据写入表中**

要访问 Redshift 实例，我们需要 [Redshift Endpoint URL](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-connect-to-cluster.html)。例如，实例将如下所示：

```py
datawarehouse.some_chars_here.region_name.redshift.amazonaws.com.
```

我们连接到数据库，并将数据框（在第 4 步中创建）写入新表或现有表中。*pandas_to_redshift* 函数允许你将数据附加到现有表中或完全覆盖它。请注意，如果选择 *append = False*，那么每次执行此操作时，表将被删除并重新创建。如果你希望在覆盖数据时保留某些列的数据类型或字符长度，或者用户权限，最好在执行此操作之前先截断表。你可以通过发出直接的 Truncate 命令来截断表。*SQLAlchemy* 和 *psycopg2* 是更简单的选项。截断表后，你可以使用 *append = True* 执行操作。我通常对需要保留历史数据的类型 2 表使用 *append=True*。

```py
#### Truncating the Table ####

##########################################
# Connecting to DataBase
##########################################
user1="user"
pswd1="password"
connection_db=psycopg2.connect(dbname = 'test_db', 
                             host='test_host.com', 
                             port= '1234', 
                             user= user1, 
                             password= pswd1)

##########################################
# Connection Established
##########################################

df = pd.read_sql_query("""Select distinct 
from test_table
order by 1 asc
""",connection_db)

result = df.to_markdown(index=False)

cur = connection_db.cursor()
cur.execute('TRUNCATE TABLE test_table')
```

```py
#### Writing to the table ####

##########################################
# connecting to redshift and s3
##########################################

pr.connect_to_redshift(dbname = 'db',
                        host = 'server.com',
                        port = 1234,
                        user = 'user',
                        password = 'password')

pr.connect_to_s3(aws_access_key_id = '*************',
                aws_secret_access_key = '*************************',
                bucket = 'test',
                subdirectory = 'subtest')

##########################################
# Write the DataFrame to S3 and then to redshift
##########################################
pr.pandas_to_redshift(data_frame = raw_df,
                        redshift_table_name = 'test_table',append = True,
                        region = 'xxxxxxx') 
```

这完成了第一部分，即从 quip 电子表格中读取数据并写入 Redshift 表。现在，让我们看一下第二部分。

# 第 2 部分：将数据写入现有的 Quip 电子表格。

对于这一部分，前三个步骤与第 1 部分相同。所以，请遵循上述第 1、2 和 3 步。我们将从这里的第 4 步开始。

**第 4 步：连接到数据库以读取数据**

我们将在这里使用 *psycopg2* 连接到 Redshift 实例，并从 Redshift 表中读取需要写入 Quip 电子表格的数据。在这里，我将数据框转换为 Markdown 以获得一个干净的表格，这也是 QuipClient 库的一个先决条件。

```py
##########################################
# Connecting to DataBase
##########################################
user1="user"
pswd1="password"
connection_db=psycopg2.connect(dbname = 'test_db', 
                             host='test_host.com', 
                             port= '1234', 
                             user= user1, 
                             password= pswd1)

##########################################
# Connection Established
##########################################

df = pd.read_sql_query("""Select distinct 
from test_table
order by 1 asc
""",connection_db)

result = df.to_markdown(index=False) 
```

**第5步：将数据写入Quip文件**

要将数据写入Quip电子表格，可以使用*QuipClient*库中的*edit_document*函数。此函数具有多个参数。格式可以是HTML或markdown。默认为HTML，这就是我们在第4步将数据框转换为markdown的原因。您需要指定*section_id*和*location*来指定要添加数据的位置——追加、前置、在特定部分之后/之前等。针对这种特定情况，我只想将数据追加到现有电子表格的新标签页中。您可以在[这里](https://quip.com/dev/automation/documentation/current#tag/Use-cases)了解更多信息。

有时，操作已执行，但脚本仍因API响应延迟而失败。try-except错误块用于捕获任何超时错误。

```py
##########################################
# Inserting the data to Quip
##########################################  

try:
  client.edit_document(thread_id=thread_id,
                      content = result,
                    format='markdown',
                    operation=client.APPEND)
  print("Test DB is updated.")
except socket.timeout:
  print("Error Excepted!")
```

我们完成了！

希望您找到本文有帮助。如果您有任何额外的问题，请随时联系。

感谢您的阅读！

*下次再见……*
