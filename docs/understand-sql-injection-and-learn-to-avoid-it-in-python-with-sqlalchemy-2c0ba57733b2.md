# 理解 SQL 注入并学习如何在 Python 中使用 SQLAlchemy 避免它

> 原文：[`towardsdatascience.com/understand-sql-injection-and-learn-to-avoid-it-in-python-with-sqlalchemy-2c0ba57733b2`](https://towardsdatascience.com/understand-sql-injection-and-learn-to-avoid-it-in-python-with-sqlalchemy-2c0ba57733b2)

## 学习在 Python 中以安全的方式与数据库交互

[](https://lynn-kwong.medium.com/?source=post_page-----2c0ba57733b2--------------------------------)![Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----2c0ba57733b2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2c0ba57733b2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2c0ba57733b2--------------------------------) [Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----2c0ba57733b2--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2c0ba57733b2--------------------------------) ·5 分钟阅读·2023 年 4 月 12 日

--

![](img/a20548112fa3a883fe1500f087c263f2.png)

图片来自 Pixabay 的 mohamed_hassan（Hosting Web Man）

SQL 注入是最常见且最危险的网络安全漏洞之一，它允许黑客将恶意 SQL 代码注入到未经验证和清理的纯 SQL 查询中。这也是新开发人员常常忽视的一个问题。

SQL 注入的原因和解决方案其实非常简单。在这篇文章中，我们将通过一些简单的查询来探索 SQL 注入，并假装成为攻击者来利用我们的数据库。在文章的最后，你将完全理解 SQL 注入，并在意识到其威力和危险后不会再犯这个错误。

## 准备

和往常一样，我们将使用 Docker 创建一个 MySQL 数据库：

```py
# Create a volume to persist the data.
$ docker volume create mysql8-data

# Create the container for MySQL.
$ docker run --name mysql8 -d -e MYSQL_ROOT_PASSWORD=root -p 13306:3306 -v mysql8-data:/var/lib/mysql mysql:8

# Connect to the local MySQL server in Docker.
$ docker exec -it mysql8 mysql -u root -proot

mysql> SELECT VERSION();
+-----------+
| VERSION() |
+-----------+
| 8.0.31    |
+-----------+
1 row in set (0.00 sec)
```

请注意，本文中为了简化起见使用了 root 用户，但在实际应用中绝不应直接在我们的 Web 应用程序中使用。

然后，让我们创建一些数据库和表来进行测试。为了简单起见，数据集与之前系列文章中使用的相同。

```py
CREATE DATABASE `data`;

CREATE TABLE `data`.`student_scores` (
  `student_id` smallint NOT NULL,
  `subject` varchar(50) NOT NULL,
  `score` tinyint DEFAULT '0',
  PRIMARY KEY (`student_id`,`subject`),
  KEY `ix_subject` (`subject`),
  KEY `ix_score` (`score`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
;

INSERT INTO `data`.student_scores
    (student_id, subject, score)
VALUES
    (1, 'Literature', 90),
    (1, 'Math', 60),
    (2, 'Literature', 80),
    (2, 'Math', 80),
    (3, 'Literature', 70),
    (3, 'Math', 95)
;
```

由于我们在这篇文章中将使用 SQLAlchemy 进行数据库连接，我们需要安装必要的库。和往常一样，建议创建一个单独的虚拟环境，以便库不会影响系统及其他虚拟环境。

```py
conda create --name sql python=3.11
conda activate sql

pip install -U "SQLAlchemy>=2.0.0,<2.1.0"
pip install -U "pymysql>=1.0.0,<1.1.0"
pip install -U "cryptography>=40.0.0,<40.1.0"
```

## 探索 SQL 注入

现在让我们创建一个简单的函数来读取一些数据：

```py
from sqlalchemy import create_engine, text

db_url = "mysql+pymysql://root:root@localhost:13306/data"
engine = create_engine(db_url, pool_size=5, pool_recycle=3600)
conn = engine.connect()

def read_student_scores(student_id):
    sql_text = text(f"""
        SELECT subject, score
        FROM data.student_scores
        WHERE student_id = {student_id}
    """)
    result = list(conn.execute(sql_text))

    print(result)
```

`read_student_scores()` 函数从简单的编码角度来看似乎很正常。然而，它存在一个巨大的安全问题，可能被恶意用户利用。

如果我们正常使用，它将正常工作：

```py
read_student_scores(1)
# [('Literature', 90), ('Math', 60)]
```

然而，它也可能返回一些不应该由恶意用户返回的内容。黑客的第一个攻击点是返回所有记录，即使是用户不应该看到的记录：

```py
read_student_scores('-1 OR 1')
# [('Literature', 90), ('Math', 60), ('Literature', 80), ('Math', 80), ('Literature', 70), ('Math', 95)]
```

这是可能的，因为 `read_student_scores()` 函数没有清理和验证输入参数，而是将输入数据与原始查询简单地拼接在一起。

这对于许多开发者来说并不罕见。实际上，我见过不少以这种方式编写的遗留代码。很幸运的是，它们之前没有被黑客攻击。或者说，也许已经被黑过了……

SQL 注入可能比上面所示的更具危害性，实际上黑客可以返回任何信息。

现在，让我们假装自己是恶意用户，尝试获取一些不应该由此函数返回的信息。

黑客首先想知道的是返回了多少列。在这个示例中，很明显返回了两列。然而，当输出通过某些用户界面显示时，这可能不是那么明显。

有很多方法可以猜测返回了多少列，两种常见的方法是使用 `ORDER BY` 和 `UNION`。让我们看看它是如何工作的：

```py
read_student_scores('-1 ORDER BY 1')
# []
read_student_scores('-1 ORDER BY 2')
# []
read_student_scores('-1 ORDER BY 3')
# OperationalError: (pymysql.err.OperationalError) (1054, "Unknown column '3' in 'order clause'")
```

从上述查询、结果和错误中，我们知道返回了两列。

我们可以使用`UNION`得出相同的结论：

```py
read_student_scores('-1 UNION SELECT 1')
# OperationalError: (pymysql.err.OperationalError) (1222, 'The used SELECT statements have a different number of columns')
read_student_scores('-1 UNION SELECT 1,2')
# [('1', 2)]
```

使用 `UNION` 我们能够通过较少的测试猜测正确的列数。实际上，`UNION` 是最常用的黑客工具之一，用于攻击数据库。

让我们尝试读取一些正常情况下不应该返回的内容：

```py
read_student_scores('-1 UNION SELECT DATABASE(), @@VERSION')
# [('data', '8.0.31')]
```

数据库名称和版本被返回了！

让我们看看更可怕的情况：

```py
read_student_scores('-1 UNION SELECT user, authentication_string FROM mysql.user')
# [('root', '$A$005$j\x1cZ\x1aj*t\x16_aI\t.\tk\x1a0b8,6nT16rTboTxEGJsq8R.xLN1dlygQWOe12XurOijG5v9'), ('mysql.infoschema', '$A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED'), ('mysql.session', '$A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED'), ('mysql.sys', '$A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED'), ('root', '$A$005$\x0c=\x10gE\x7f]g\x18WQNnB`Y&I1\x18zPIQ3wM3cj43wk4Qq4/Tt88B0ypKrwYLYnD3BpGqfY5')]
```

所有数据库用户的用户名和认证字符串都被返回了！使用一些暴力猜测工具，黑客可以在短时间内破解密码，特别是当使用简单密码时。

## 如何避免 SQL 注入？

现在我们已经了解了 SQL 注入是什么以及它有多么危险，让我们看看如何在实践中避免它。

防止 SQL 注入的最有效方法是使用参数化查询，这可以通过 SQLAlchemy 中的 `:param_name` 语法实现：

```py
def read_student_scores(student_id):
    sql_text = text("""
        SELECT subject, score
        FROM data.student_scores
        WHERE student_id = :student_id
    """)
    result = list(conn.execute(sql_text, parameters={"student_id": student_id}))

    print(result)
```

请注意，本帖使用了 SQLAlchemy 2.0，因此指定参数的语法与 SQLAlchemy 1.x（通常是 1.4）中的语法会有所不同。

让我们看看使用参数化查询时恶意查询会返回什么：

```py
read_student_scores('-1 OR 1')
# []
read_student_scores('-1 UNION SELECT DATABASE(), @@VERSION')
# []
read_student_scores('-1 UNION SELECT user, authentication_string FROM mysql.user')
# []
```

所有这些恶意查询返回了空结果，比之前安全得多。

在这篇文章中，我们介绍了什么是 SQL 注入，它是如何工作的，以及如何使用简单的示例来避免它。

尽管使用参数化查询可以防止大多数 SQL 注入实例，但为了使我们的应用程序更加健壮，我们还应该应用以下策略：

+   限制数据库用户查询数据库的权限。为了简单起见，本示例使用了 root 用户，但在实际应用中绝不应该使用 root 用户。实际上，我们应该为我们的 Web 应用创建一个专用的数据库用户（并使用强密码），并仅授予最低权限。

+   清理并验证输入查询。如果输入数据的类型不一致，或者包含可疑字符如井号（`#`）、分号（`；`）、减号（`-`），甚至是词语`UNION`，应以安全、稳健且用户友好的方式处理这种情况。

+   永远不要将调试日志直接展示给最终用户。调试日志仅应供内部用户使用，因为它们可能包含敏感信息，恶意用户可能利用这些信息来利用系统。

## 相关文章：

+   [如何在 Python 中使用 SQLAlchemy 执行纯 SQL 查询](https://betterprogramming.pub/how-to-execute-plain-sql-queries-with-sqlalchemy-627a3741fdb1)

+   [如何在 Pandas 中使用 SQLAlchemy 查询、表和 ORM 类](https://medium.com/gitconnected/how-to-work-with-sqlalchemy-query-table-and-orm-classes-in-pandas-477599174b6)
