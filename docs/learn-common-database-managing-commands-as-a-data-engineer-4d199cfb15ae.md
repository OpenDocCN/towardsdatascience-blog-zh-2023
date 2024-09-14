# 我们应该了解的重要 MySQL 数据定义语言（DDL）命令，用于管理我们的表

> 原文：[`towardsdatascience.com/learn-common-database-managing-commands-as-a-data-engineer-4d199cfb15ae`](https://towardsdatascience.com/learn-common-database-managing-commands-as-a-data-engineer-4d199cfb15ae)

## 作为数据工程师，学习常用的数据库管理命令

[](https://lynn-kwong.medium.com/?source=post_page-----4d199cfb15ae--------------------------------)![Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----4d199cfb15ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4d199cfb15ae--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4d199cfb15ae--------------------------------) [Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----4d199cfb15ae--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4d199cfb15ae--------------------------------) ·阅读时间 8 分钟·2023 年 2 月 28 日

--

![](img/9ab632bea48c228ba8500f79c5cc4dc1.png)

图片由 geralt 提供于 Pixabay

作为数据工程师，检查和更新表的模式是我们的日常工作。虽然网上已经有很多教程，但很少有教程关注应该遵循的约定。SQL 非常灵活，可以以“稳健”的方式工作。你可以使用小写或大写的查询，并以任何你想要的方式命名数据库/表/列/索引/视图。然而，代价是可读性降低，维护变得困难，因为不同的人可能会以不同的方式编写 SQL 查询。

在这篇文章中，我们将介绍一些用于管理 MySQL 表模式的常用命令，重点讲解每个操作的约定和最佳实践。它可以作为新数据工程师的手册（需进行必要的调整）。

## 准备工作

我们将使用 Docker 启动一个 MySQL 8 容器，它将作为这篇文章的 MySQL 服务器：

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

我们将在这个 MySQL 服务器中创建我们的数据库（在 MySQL 中也称为模式）和表。首先，让我们创建一个数据库来存储我们的虚拟数据：

```py
CREATE DATABASE sales;
```

数据库名称应具有描述性、简洁明了，并且不包含特殊字符，除了下划线之外。它最好是小写的，以便与 MySQL 关键字区分开来。相同的命名约定也适用于表名和列名。

在这篇文章中，我们将使用 DBeaver 来编写查询并查看表数据。

## 创建表

现在让我们创建第一个表，它将存储客户数据。

```py
CREATE TABLE `sales`.`customers` (
  `id` SMALLINT NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(50) NOT NULL,
  `job` VARCHAR(50) DEFAULT '',
  PRIMARY KEY (`id`),
  KEY `ix_name` (`name`)
);
```

你可以使用单数或复数表名。我更喜欢使用复数，因为表可以被视为数据记录的容器。

可以通过以下命令查找现有表的数据定义语言（DDL）查询：

```py
SHOW CREATE TABLE `sales`.`customers`;
```

推荐在编写查询时指定模式名称，以便更好地支持自动补全。

默认情况下，MySQL 对数据库名称、表名称和别名是区分大小写的。然而，对列名称不区分大小写。因此，列名称的命名可以非常灵活。然而，我们应该遵循一些相同数据库的命名约定。无论你使用驼峰命名还是蛇形命名，你只需要保持一致。然而，你可能会根据你的后端编程语言有一些偏好。例如，作为 Python 开发者，我们更倾向于使用蛇形命名。

此外，如你所见，我们将前缀`ix`（表示索引）加到索引名称上。我们通常应该避免给列名称加前缀，以使查询更简洁。然而，我们应该为索引或约束提供前缀，以便在某些错误中更具指示性。我们很少需要显式引用索引或约束，因此简洁不是问题。

一些常用的前缀是：

+   `ix`用于索引。

+   `fk`用于外键约束。

+   `uq`用于唯一键约束。

此外，还有一些关于如何命名索引或约束的约定：

+   索引：`ix_%(column_0_label)s`

+   外键：`fk_%(table_name)s_%(column_0_name)s_%(referred_table_name)s`

+   唯一键：`uq_*%(table_name)s*_*%(column_0_name)s*`，

我们将在本帖中稍后介绍其中的一些。

## 重命名或复制一个表

我们可以使用`RENAME`命令重命名一个表：

```py
RENAME TABLE sales.customers TO sales.customer  -- Not executed as I prefer plural
;
```

如果你想将一个表从一个模式复制到另一个模式，你需要分两步进行。例如，让我们创建一个新的`customers_data`模式，并将`customers`表复制到那里。

```py
CREATE DATABASE customers_data;

CREATE TABLE customers_data.customers LIKE sales.customers;
INSERT INTO customers_data.customers
SELECT * FROM sales.customers
;
```

这样，旧表的数据类型和索引将保留在新表中。如果你按照下面所示使用`CREATE TABLE … SELECT…`，则数据类型（实际上会被推断）和索引会丢失，这在几乎所有情况下都是不希望的：

```py
CREATE TABLE customers_data.customers_copy_direct
SELECT * FROM sales.customers 
;
```

你可以使用`SHOW CREATE TABLE`命令检查新创建表的模式。

如果你需要将一个表从一个数据库（不同的主机或端口）移动到另一个数据库，你可以将表导出到一个 SQL 文件中，然后在另一个数据库中加载：

```py
mysqldump -h HOST_1 -P PORT_1 -u USERNAME_1 -p \
    --single-transaction --skip-triggers --skip-column-statistics \
    SCHEMA_1 TABLE_NAME > TABLE_NAME.sql
mysql -h HOST_2 -P PORT_2 -u USERNAME_2 -p SCHEMA_2 < TABLE_NAME.sql
```

安装了 MySQL 客户端后，可以使用`mysqldump`：

```py
sudo apt-get update
sudo apt-get install mysql-client
```

两个数据库的主机可以相同。在这种情况下，端口会不同。

注意`mysqldump`指定的选项，这在大多数情况下是必要的。特别是，使用`--single-transaction`时，表在导出时不会被锁定。

## 添加/删除/更改列

为了演示命令，让我们执行以下操作。这些操作可能没有太大意义，重点是使用的命令：

+   使用`DROP`删除`name`列，

+   使用`ADD`重新添加`name`列，

+   使用`MODIFY`更改列的数据类型；

```py
ALTER TABLE `sales`.`customers`
DROP `name`,
ADD `name` VARCHAR(50) NOT NULL AFTER `id`,
MODIFY `job` VARCHAR(100) DEFAULT ''
;
```

注意，我们可以使用 `AFTER column_name` 关键字来改变列的顺序。如果列应该被改为第一个，则需要使用 `FIRST` 关键字，而不是 `AFTER column_name`。

例如，让我们将 `name` 列改为第一个：

```py
ALTER TABLE `sales`.`customers`
MODIFY `name` VARCHAR(50) NOT NULL FIRST
;
```

## 重命名列

我们可以仅使用 `RENAME COLUMN A TO B` 来重命名列而不更改数据类型：

```py
ALTER TABLE sales.customers 
RENAME COLUMN `job` TO `address`
;
```

注意，`COLUMN` 关键字可以在 `ADD`、`DROP`、`MODIFY` 以及即将介绍的 `CHANGE` 命令中省略，但在 `RENAME` 命令中不能省略。

我们还可以使用 `CHANGE` 命令重命名列并更改数据类型。让我们将 `name` 改为 `username` 并将长度改为 100：

```py
ALTER TABLE sales.customers 
CHANGE `name` `username` VARCHAR(100) NOT NULL
;
```

## 使用外键

让我们创建两个新表来演示外键的使用。一个新的 `products` 表将存储产品信息，`orders` 表将存储客户的订单：

```py
CREATE TABLE `sales`.`products` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(50) NOT NULL,
  `price` DECIMAL(12,2),
  PRIMARY KEY (`id`),
  KEY `ix_name` (`name`),
  KEY `ix_price` (`price`)
);

CREATE TABLE `sales`.`orders` (
  `customer_id` SMALLINT NOT NULL,
  `product_id` INT NOT NULL,
  `quantity` SMALLINT NOT NULL,
  PRIMARY KEY (`customer_id`, `product_id`),
  KEY `ix_product_id` (`product_id`),
  KEY `ix_quantity` (`quantity`),
  CONSTRAINT `fk_orders_customer_id_customers` FOREIGN KEY (`customer_id`) REFERENCES `customers` (`id`) ON DELETE CASCADE,
  CONSTRAINT `fk_orders_product_id_products` FOREIGN KEY (`product_id`) REFERENCES `products` (`id`) ON DELETE CASCADE
);
```

注意，`customers` 和 `products` 表中的 `id` 列没有前缀，但在 `orders` 表中有前缀。这是因为那里的 ID 有两个，一个用于客户，另一个用于产品。

如果检查 `id` 列的数据类型，你会发现它们与 `customers` 和 `products` 表中的数据类型相同。这是添加外键的要求，因为列的数据类型必须在当前表和引用表中相同。

使用 `customer_id` 和 `product_id` 创建了一个复合主键。注意，我们需要为 `product_id` 创建一个单独的索引，但不需要为 `customer_id` 创建，因为它被复合主键覆盖，因为它是复合键中的第一个。

还要注意外键约束的命名约定，遵循了[此链接](https://docs.sqlalchemy.org/en/14/core/constraints.html#configuring-a-naming-convention-for-a-metadata-collection)中介绍的命名约定。

要检查表的索引，我们可以运行以下两个查询之一：

```py
SHOW INDEX FROM sales.orders;

SELECT
  s.TABLE_SCHEMA,
  s.TABLE_NAME,
  s.INDEX_NAME,
  s.COLUMN_NAME,
  s.SEQ_IN_INDEX 
FROM INFORMATION_SCHEMA.STATISTICS s
WHERE 1
  AND s.TABLE_SCHEMA = 'sales'
  AND s.TABLE_NAME = 'orders'
;
```

注意，上述查询不会返回外键。如果需要检查外键，我们需要运行以下查询：

```py
SELECT 
  TABLE_SCHEMA,
  TABLE_NAME,
  CONSTRAINT_NAME,
  COLUMN_NAME,
  REFERENCED_TABLE_SCHEMA,
  REFERENCED_TABLE_NAME,
  REFERENCED_COLUMN_NAME
FROM
  INFORMATION_SCHEMA.KEY_COLUMN_USAGE
WHERE 1
  AND TABLE_SCHEMA = 'sales'
  AND TABLE_NAME = 'orders'
;
```

注意，两个特殊的表 `INFORMATION_SCHEMA.STATISTICS` 和 `INFORMATION_SCHEMA.KEY_COLUMN_USAGE` 是系统表，通常以大写字母引用。

## 添加和删除索引及约束

让我们演示如何添加和删除索引和约束。我们不能修改索引或约束，因为一旦条件改变，索引/约束必须重新生成。

首先，让我们删除 `orders` 表的主键、索引和外键：

```py
ALTER TABLE sales.orders 
DROP PRIMARY KEY,
DROP INDEX `ix_product_id`,
DROP INDEX `ix_quantity`,
DROP FOREIGN KEY `fk_orders_customer_id_customers`,
DROP FOREIGN KEY `fk_orders_product_id_products`
;
```

注意我们指定如何删除外键的方式。应该使用 `DROP FOREIGN KEY …`，而不是 `DROP CONSTRAINT …`。

现在让我们将它们添加回来：

```py
ALTER TABLE sales.orders 
ADD PRIMARY KEY (`customer_id`, `product_id`),
ADD KEY `ix_product_id` (`product_id`),
ADD KEY `ix_quantity` (`quantity`),
ADD CONSTRAINT `fk_orders_customer_id_customers` FOREIGN KEY (`customer_id`) REFERENCES `customers` (`id`) ON DELETE CASCADE,
ADD CONSTRAINT `fk_orders_product_id_products` FOREIGN KEY (`product_id`) REFERENCES `products` (`id`) ON DELETE CASCADE
;
```

语法类似于上面的 DDL 查询。

## 创建或更新视图

MySQL 视图与表的工作方式相同，并且被视为表。视图通常包含一个或多个表中的一些选定列，基于一些过滤条件。它可以用来快速查看一个或多个表中的特定数据，而无需编写 `JOIN` 和 `WHERE` 条件。

让我们为 `orders` 表创建一个视图，以便可以直接获取订单的客户和产品详情：

```py
CREATE OR REPLACE VIEW sales.orders_with_details AS
SELECT
  o.customer_id,
  c.username,
  c.address,
  o.product_id,
  p.name,
  p.price,
  o.quantity,
  p.price * o.quantity AS total
FROM sales.orders o 
JOIN sales.customers c 
  ON c.id = o.customer_id 
JOIN sales.products p 
  ON p.id = o.product_id 
;

SELECT * FROM sales.orders_with_details;
```

视图的名称应能指示其用途。在这种情况下，`orders_with_details` 比 `orders_view` 更好，因为前者更能说明视图中包含的内容。

## 编写 SQL 查询的标准

我们应以易于阅读的方式编写 SQL 查询。虽然没有严格的标准，但遵循以下规则将使你的查询更易于阅读和维护：

+   将所有 SQL 关键字写成大写字母。

+   将所有数据库名称、列名和别名写成小写字母。

+   为你的表提供标准缩写作为别名。例如，`products` => `p`，`product_attributes` => `pa` 等。不要使用任意的表别名，因为这会使查询变得更加难以阅读。

+   将 `SELECT`、`FROM`、`JOIN`、`WHERE`、`GROUP BY`、`ORDER BY` 等语句另起一行。

+   每个 `ON` 和 `AND` 条件应另起一行。

+   相同的格式化标准适用于嵌套查询。

你可以在 DBeaver 或 VS code（配合一些 SQL 扩展）中自动格式化 SQL 查询。不过，格式化效果并不完美，我们通常需要根据上述规则进行一些手动调整。

在这篇文章中，我们介绍了一些用于管理 MySQL 表模式的常见且实用的命令。我们涵盖了如何创建和更新数据库、表、列、索引和视图，并重点介绍了每个操作的约定和最佳实践，这些可以作为新数据工程师的起始指导。

## 相关文章：

+   一些使用 DBeaver 的技巧 — 一款通用数据库工具

+   [了解 MySQL 锁和死锁的基础知识（第一部分）](https://lynn-kwong.medium.com/understand-the-basics-of-locks-and-deadlocks-in-mysql-part-i-92f229db0a)
