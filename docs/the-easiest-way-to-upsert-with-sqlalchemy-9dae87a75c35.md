# 使用 SQLAlchemy 进行 UPSERT 的最简单方法

> 原文：[`towardsdatascience.com/the-easiest-way-to-upsert-with-sqlalchemy-9dae87a75c35`](https://towardsdatascience.com/the-easiest-way-to-upsert-with-sqlalchemy-9dae87a75c35)

## 一个命令同时`INSERT`新数据和`UPDATE`数据库中现有的记录

[](https://mikehuls.medium.com/?source=post_page-----9dae87a75c35--------------------------------)![Mike Huls](https://mikehuls.medium.com/?source=post_page-----9dae87a75c35--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9dae87a75c35--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9dae87a75c35--------------------------------) [Mike Huls](https://mikehuls.medium.com/?source=post_page-----9dae87a75c35--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9dae87a75c35--------------------------------) ·5 min read·2023 年 3 月 20 日

--

在这篇短文中，我们将深入探讨如何在 SQLAlchemy 中进行`UPSERT`：我们将`INSERT`新数据到数据库中，并用新提供的值`UPDATE`已经存在的记录。我们将从一个用例开始，准备数据库，然后进行 UPSERT！非常简单！让我们开始编码吧！

![](img/c6efe92cefc9eca0364df4857d41e585.png)

一行行地进行 UPSERT（图片来源 [Tom Fisk](https://www.pexels.com/@tomfisk/) 在 [Pexels](https://www.pexels.com/photo/aerial-shot-of-green-milling-tractor-1595108/))

# 什么时候我需要进行 UPSERT？

Upserting 是将新记录`INSERT`到数据库表中，同时`UPDATE`数据集中已经存在的记录。数据库通过比较表的主键列来“知道哪些记录已经存在”。

`UPDATE`现有记录很重要，以便我们的外键关系保持完整。在下一部分中，我们将通过一个示例清楚地解释我们在实践中的目标。

[](/dramatically-improve-your-database-inserts-with-a-simple-upgrade-6dfa672f1424?source=post_page-----9dae87a75c35--------------------------------) ## 通过简单升级显著提高数据库插入速度

### 在 Python 中创建超快速数据库连接的 4 个级别

towardsdatascience.com

# 设置示例

想象一下我们经营一个吉他店的网店。当然，我们必须跟踪哪些吉他有库存、库存数量以及价格。这就是`inventory`表的作用：

```py
**sku               name                  price   stock** LPTD19HSNH1       Gibson Les Paul        2000       6
1-014-9763-300    Fender Jazzmaster      1799       3
IRONMK1KH80BK     BC Rich Ironbird       1099       4
```

每一行都基于`sku`是唯一的：这是表的`primary key`。主键在插入时非常重要。

我们每天接收到的数据集可能如下所示：

```py
**sku               name                 price   stock** LPTD19HSNH1       Gibson Les Paul       2000       4
1-014-9763-300    Fender Jazzmaster     1700       3
DSV00ANCH1        Gibson Flying V       2499       1
```

我们的目标是将新数据`UPSERT`到`inventory`表中：

+   `INSERT` 新记录（如 Gibson Flying V）

+   `UPDATE` 现有记录（例如 Fender Jazzmaster 的新价格）

[](/thread-your-python-program-with-two-lines-of-code-3b474407dbb8?source=post_page-----9dae87a75c35--------------------------------) ## 用两行代码处理你的 Python 程序

### 通过同时执行多个操作来加速你的程序

towardsdatascience.com

# 使用 SQLAlchemy 进行 UPSERT

我们将使用 SQLAlchemy 进行 3 个步骤的 UPSERT。

*在这篇文章中，我假设你对 SQLAlchemy 已经有所了解，所以我不会详细讲解，因为这超出了我们当前的范围，但我会提供更多深入解释的文章链接。还可以查看* [***这个链接***](https://mikehuls.com/articles?tags=sqlalchemy%2Cdatabase%2Csql) *以获取有关 SQLAlchemy 的更多文章。*

## 1\. 设置我们的数据库和连接

首先，我们声明一个名为‘Guitar’的模型。这对应于我们库存表中的一行。注意，它有一个名为`sku`的列，作为主键。

```py
# SQLAlchemy needs a declarative_base when we use declarative models like Guitar
Base = declarative_base()

# Guitar will be one record in our inventory table
class Guitar(Base):
    __tablename__ = 'inventory'
    sku: typing.Optional[str] = sa.Column(type_=sa.String, primary_key=True)
    name: str = sa.Column(type_=sa.String)
    stock:int = sa.Column(type_=sa.Integer)
    price: int = sa.Column(type_=sa.Integer)

    def __repr__(self):
        return f"<{self.name} (price=${self.price} - stock=#{self.stock}>"

# Create a database engine (sqlite in this case)
dbEngine = sa.create_engine(url="sqlite:///inventory_db.db", echo=False)

# Create all tables in the database engine
Base.metadata.create_all(bind=dbEngine)
```

接下来，我们为我们的 sqlite 数据库创建一个数据库引擎（查看 [**这篇文章**](https://mikehuls.medium.com/how-to-make-a-database-connection-in-python-for-absolute-beginners-e2bfd8e4e52) 以获取更多信息），并使用它来创建`inventory`表。

[](/no-need-to-ever-write-sql-again-sqlalchemys-orm-for-absolute-beginners-107be0b3148f?source=post_page-----9dae87a75c35--------------------------------) ## 永远不需要再写 SQL：SQLAlchemy 的 ORM 为绝对初学者

### 使用这个 ORM，你可以创建表格、插入、读取、删除和更新数据，而无需编写一行 SQL

towardsdatascience.com

## 2\. 将数据插入到`inventory`表中

接下来，我们将创建一些`Guitar`模型的实例，并使用 SQLAlchemy 的`Session`对象将它们插入数据库：

```py
with Session(dbEngine) as session:
    session.add_all([
        Guitar(sku="LPTD19HSNH1", name="Gibson Les Paul", price=2000, stock=6),
        Guitar(sku="1-014-9763-300", name="Fender Jazzmaster", price=1799, stock=3),
        Guitar(sku="IRONMK1KH80BK", name="BC Rich Ironbird", price=1099, stock=4),
    ])
    session.commit()
```

当我们 `SELECT * FROM inventory` 时，我们可以看到`inventory`表中的内容：

```py
**sku               name                  price   stock** LPTD19HSNH1       Gibson Les Paul        2000       6
1-014-9763-300    Fender Jazzmaster      1799       3
IRONMK1KH80BK     BC Rich Ironbird       1099       4
```

接下来，我们将接收一个包含新每日库存的数据集。我们需要插入新的`Guitar`记录，并更新我们当前`inventory`中的记录。

[](/getting-started-with-postgres-in-docker-616127e2e46d?source=post_page-----9dae87a75c35--------------------------------) ## 在 Docker 中开始使用 Postgres

### 为初学者创建一个 Docker 容器中的 Postgres 数据库

towardsdatascience.com

## 3\. 将数据 UPSERT 到库存表中

最后是 UPSERT 的时候了！开始吧：

```py
# Convert the new stock to a list of Guitar objects
new_stock:[Guitar] = [
    Guitar(sku="LPTD19HSNH1", name="Gibson Les Paul", price=2000, stock=4),      # sold 2 les pauls
    Guitar(sku="1-014-9763-300", name="Fender Jazzmaster", price=1700, stock=3), # lowered price on jazzmaster
    Guitar(sku="DSV00ANCH1", name="Gibson Flying V", price=2499, stock=1),       # new guitar in stock!
]

# Use the session and UPSERT using the "merge" method
with Session(dbEngine) as session:
    for guitar in new_stock:
        session.merge(guitar)
    session.commit()
```

正如你所见，我们使用 `session.merge`。此方法分析我们的 new_stock 并检查提供的 `sku` 值是否已存在于 `inventory` 表中。如果 `sku` 存在，它会执行更新，否则它会插入新的吉他。

这是结果表：

```py
**sku               name                  price   stock** LPTD19HSNH1       Gibson Les Paul        2000       4    << stock decreased by 2
1-014-9763-300    Fender Jazzmaster      1700       3    << price lowered by $99
IRONMK1KH80BK     BC Rich Ironbird       1099       4    << no chnages
DSV00ANCH1        Gibson Flying V        2499       1    << new in stock!
```

一点需要注意的是，`merge` 方法与 SQL 中的 MERGE 不同，因为它仅执行插入和更新。如果你想要更细粒度的控制，查看 [**这篇文章**](https://mikehuls.medium.com/sql-insert-delete-and-update-in-one-statement-sync-your-tables-with-merge-14814215d32c)，它展示了如何在 SQL 中用一条语句进行删除/更新/插入。

[](/image-analysis-for-beginners-creating-a-motion-detector-with-opencv-4ca6faba4b42?source=post_page-----9dae87a75c35--------------------------------) ## 使用 OpenCV 进行运动检测 — 初学者的图像分析

### 如何使用 OpenCV 检测和分析移动物体

towardsdatascience.com

# 结论

正如我们所见，当使用 SQLAlchemy ORM 时，UPSERT 非常简单。查看我的其他文章 [**这里**](https://mikehuls.com)，以了解 Python、SQLAlchemy、Docker 和数据库的一些用途！

我希望这篇文章能够如我所期望的那样清晰，但如果不是这种情况，请告诉我我可以做些什么来进一步澄清。同时，查看我关于各种编程相关主题的 [其他文章](https://mikehuls.com/articles?tags=python)：

+   [Git 完全初学者指南：通过视频游戏理解 Git](https://mikehuls.medium.com/git-for-absolute-beginners-understanding-git-with-the-help-of-a-video-game-88826054459a)

+   [创建并发布你自己的 Python 包](https://mikehuls.medium.com/create-and-publish-your-own-python-package-ea45bee41cdc)

+   [用 FastAPI 在 5 行代码中创建一个快速的自动文档化、可维护且易于使用的 Python API](https://mikehuls.medium.com/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e)

编程愉快！

— Mike

*附言：喜欢我所做的工作吗？* [*关注我！*](https://mikehuls.medium.com/membership)

[](https://mikehuls.medium.com/membership?source=post_page-----9dae87a75c35--------------------------------) [## 通过我的推荐链接加入 Medium - Mike Huls

### 阅读 Mike Huls 的每个故事（以及 Medium 上其他成千上万的作家的故事）。你的会员费用直接支持 Mike…

mikehuls.medium.com](https://mikehuls.medium.com/membership?source=post_page-----9dae87a75c35--------------------------------)
