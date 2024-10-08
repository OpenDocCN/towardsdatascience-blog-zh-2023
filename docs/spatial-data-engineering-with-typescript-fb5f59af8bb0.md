# 使用 Typescript 进行空间数据工程

> 原文：[`towardsdatascience.com/spatial-data-engineering-with-typescript-fb5f59af8bb0`](https://towardsdatascience.com/spatial-data-engineering-with-typescript-fb5f59af8bb0)

![](img/d8b3269364c41a6de125247805d7042d.png)

[T K](https://unsplash.com/@realaxer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/9AxFJaNySB8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 建立自动化空间数据科学的数据管道

[](https://sutan.co.uk/?source=post_page-----fb5f59af8bb0--------------------------------)![Sutan Mufti](https://sutan.co.uk/?source=post_page-----fb5f59af8bb0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fb5f59af8bb0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fb5f59af8bb0--------------------------------) [Sutan Mufti](https://sutan.co.uk/?source=post_page-----fb5f59af8bb0--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fb5f59af8bb0--------------------------------) ·9 分钟阅读·2023 年 9 月 5 日

--

# 介绍

我们可以把数据看作水，把公司看作城镇。就像一个城镇随着人口增长而需要更多的水来服务其居民一样，一家公司在成长时也需要现成的数据来支持其运营。这类公司需要一个数据管道系统，类似于将水送到城镇家庭的管道和基础设施。在我们的数据类比中，数据工程师就是那些建立和维护这些数据管道的人。对于常规的数组或表格数据来说，这很简单，但对于空间数据来说，就会复杂一些。

空间数据与常规数据略有不同；它包含空间属性。这些属性使我们能够建立空间关系，也称为 [地理空间拓扑](https://en.wikipedia.org/wiki/Geospatial_topology)。即使两个表没有主键和外键，只要它们都有空间属性，我们仍然可以将它们连接起来。如果我们可视化空间属性，就会得到一张地图！

[](/spatial-data-science-sql-join-spatially-ecd2f7400753?source=post_page-----fb5f59af8bb0--------------------------------) ## 空间数据科学：SQL 空间连接

### 如果表格之间有空间关系，则将它们连接起来。附加说明：我正在使用 Ms. Excel 进行操作

towardsdatascience.com

构建空间数据管道与创建普通数据管道有所不同。在这种情况下，我们主要使用空间 SQL 处理空间数据属性，这是数据科学家社区中[较少见的技能](https://carto.com/blog/top-insights-state-of-spatial-data-science-report)。一旦数据管道建立，数据分析师可以继续分析来自数据管道的空间数据。这意味着数据分析师可以专注于生成空间洞察，而无需担心数据的可用性。这也意味着可以使用最新的空间数据自动生成地图。

## 本文

本文讨论了如何使用 TypeScript 和 NodeJS 构建空间数据管道。我们可以将其称为[ETL](https://aws.amazon.com/what-is/etl/)（提取、转换、加载）过程，但处理的是空间数据。首先，我们将演示如何使用 TypeScript 从源头获取数据（提取）。接下来，我们将把这些数据转换成适合存储的结构（转换）。最后，我们将操作空间 SQL 来管理和存储数据到我们的数据库中（加载）。

开发使用 TypeScript 和 Node.js 完成，主要使用了[node-postgres](https://node-postgres.com/)库。演示代码可在以下链接中找到。

[](https://github.com/sutanmufti/spatial-data-engineering-typescript?source=post_page-----fb5f59af8bb0--------------------------------) [## GitHub - sutanmufti/spatial-data-engineering-typescript: 使用 TypeScript 进行空间数据工程…

### 使用 TypeScript 和 Node.js 进行空间数据工程。贡献者为 sutanmufti/spatial-data-engineering-typescript…

github.com](https://github.com/sutanmufti/spatial-data-engineering-typescript?source=post_page-----fb5f59af8bb0--------------------------------)

# 先决条件

有三个先决条件：你需要有 Node.JS 和 TypeScript，以及一个按需的 PostGIS 服务器。我在这篇文章中使用的是 Mac OS（UNIX），但这篇文章适用于任何 Linux 或类似 UNIX 的服务器操作系统。

## Node.JS

Node.js 是一个在服务器上执行 JavaScript 代码的 JavaScript 运行时。它的功能类似于 Python，充当 JavaScript 代码的解释器。对于本文，基本理解 JavaScript 语法即可。

[](https://nodejs.org/en/download?source=post_page-----fb5f59af8bb0--------------------------------) [## 下载 | Node.js

### Node.js® 是一个基于 Chrome 的 V8 JavaScript 引擎构建的 JavaScript 运行时。

nodejs.org](https://nodejs.org/en/download?source=post_page-----fb5f59af8bb0--------------------------------)

## TypeScript

TypeScript 是 JavaScript 的超集，为 JavaScript 添加了类型检查功能。这种“类型检查功能”限制了 JavaScript 编码，从而降低了出错的风险。我们在 TypeScript 中开发的代码会被编译成 JavaScript 代码，然后由 Node.JS 执行。虽然 TypeScript 是可选的，但我喜欢在我的项目中使用类型检查。

[## 如何设置 TypeScript

### 将 TypeScript 添加到你的项目中，或者全局安装 TypeScript

[TypeScript 官方网站](https://www.typescriptlang.org/download?source=post_page-----fb5f59af8bb0--------------------------------)

## Postgis 服务器

你必须运行 PostgreSQL 服务器，并安装 Postgis。Postgis 是一个 PostgreSQL 扩展，允许我们处理和存储地理空间数据。这就是我们如何进行空间 SQL 查询的方式。当然，你必须了解基本的 SQL 才能操作 PostgreSQL。

[](/spatial-data-science-spatial-queries-8d6709fd9747?source=post_page-----fb5f59af8bb0--------------------------------) [## 空间数据科学：空间查询

### 用 SQL 回答每一个“在哪里”的问题；带有示例

[Towards Data Science](https://towardsdatascience.com/spatial-data-science-spatial-queries-8d6709fd9747?source=post_page-----fb5f59af8bb0--------------------------------)

你可以通过 Docker 拥有自己的 Postgis 服务器，或者直接在你的 PC 上安装它。另一种选择是部署一个云实例，例如使用 Amazon RDS、Google Cloud SQL，或一个安装了 PostgreSQL 的普通虚拟机。

[## Docker

### 编辑描述

[Docker Hub 上的 Postgis 镜像](https://registry.hub.docker.com/r/postgis/postgis/?source=post_page-----fb5f59af8bb0--------------------------------)

# 理念

我们可以将这项工作分解为几个任务：

+   提取：从数据源获取数据。对于本文，数据源可以是任何东西，只要我们接收的是 [GeoJSON](https://geojson.org/) 格式 ([RFC7946](https://datatracker.ietf.org/doc/html/rfc7946))！在这个演示中，我们将采用 [Stuart Grange](https://skgrange.github.io/data.html) 的数据，该数据源自伦敦政府的 [数据存储](https://data.london.gov.uk/dataset/statistical-gis-boundary-files-london/resource/9ba8c833-6370-4b11-abdc-314aa020d5e0)。我们也可以检索其他地理空间格式如 shapefile，但那意味着我们必须使用其他库来处理。为了简便起见，我们就使用 GeoJSON。

+   转换：验证数据（再次说明，这不是本文的重点）并处理无效数据。然后，我们将原始数据转换成数据分析师使用的结构；或者说我们如何设计我们的数据环境。

+   加载：将数据插入到表中。这使用空间 SQL。

在文章的最后，我们可以看到我们的数据存储在 Postgis 服务器中。让我们进入代码部分。

# 构建数据管道

主要代码可以在以下链接中找到。让我们分解任务并逐步分析代码。

[GitHub - sutanmufti/spatial-data-engineering-typescript: 使用 TypeScript 进行空间数据工程…](https://github.com/sutanmufti/spatial-data-engineering-typescript?source=post_page-----fb5f59af8bb0--------------------------------)

### 使用 TypeScript 和 Node 进行空间数据工程。贡献于 sutanmufti/spatial-data-engineering-typescript…

[GitHub - sutanmufti/spatial-data-engineering-typescript](https://github.com/sutanmufti/spatial-data-engineering-typescript?source=post_page-----fb5f59af8bb0--------------------------------)

[主要功能](https://github.com/sutanmufti/spatial-data-engineering-typescript/blob/main/src/index.ts)是以下代码。如你所见，它主要包含了 2 个函数：`ExtractData`和`transformAndLoad`。此外，还有一个名为`pool`的常量变量，用于处理 PostGIS 服务器的认证。

```py
async function main(){
    // this creates the pool connection to the postgis server
    const pool = new Pool({
        user: process.env.POSTGRES_USER,
        host: process.env.POSTGRES_HOST,
        database: process.env.POSTGRES_DB,
        password: process.env.POSTGRES_PASSWORD,
        port: Number(process.env.POSTGRES_PORT),
      });

    // this fetches the data
    const data = await ExtractData()
    // this transforms and load the data based on the Pool connection
    await transformAndLoad(data.features,pool)

}
```

# 提取数据

这个任务是由`ExtractData()`函数完成的。我们期望这个函数能返回类似 GeoJSON 的数据。

```py
// located in /src/lib/functions.ts

export async function ExtractData() {
    // link used in demo is https://skgrange.github.io/www/data/london_low_emission_zones.json
    const res = await fetch('https://<linktodata>/')
    const data = <GeoData> await res.json()
    // add validation function to handle error here.
    return data
}
```

在这个函数中，我们有一个名为`ExtractData`的异步函数。`export`语句表明这个函数可以在另一个 TypeScript 文件中导入，这使得项目模块化。`async`语法表示这是一个异步函数。这使我们可以同时运行多个函数，主要因为我们将使用 fetch API 来执行 HTTP GET 请求。

Fetch API 是一个异步函数，更容易嵌套在异步函数中。因此，我们有`await`语句来等待 HTTP Get 请求完成。我们将以`GeoJSON`格式获得`data`。

我发现 fetch API 和异步函数对初学者来说比较混乱。此外，异步函数如何与 Fetch API 互补的内容在我的另一篇文章中有介绍。

[](https://insight.sutan.co.uk/using-fetch-api-faa44d4d9a12?source=post_page-----fb5f59af8bb0--------------------------------) [## 使用 Fetch API

### 如何在 Javascript 中使用 Fetch？为什么 Fetch 返回一个 Promise？介绍异步函数和 Promises

insight.sutan.co.uk](https://insight.sutan.co.uk/using-fetch-api-faa44d4d9a12?source=post_page-----fb5f59af8bb0--------------------------------)

# 转换和加载数据

在获取数据后，我们可以将其转换并加载到 PostGIS 中。我们将使用`node-postgres`作为客户端来用 nodeJS 执行 SQL。我偏爱`node-postgres`因为我喜欢编写原始 SQL 代码。此外，我们对空间 SQL 有更细粒度的控制。缺点是我们的代码可能会显得复杂，因为我们可能会编写多行 SQL 查询。

[](https://node-postgres.com/?source=post_page-----fb5f59af8bb0--------------------------------) [## node-postgres

### node-postgres 是一个用于与 PostgreSQL 数据库接口的 node.js 模块集合。

node-postgres.com](https://node-postgres.com/?source=post_page-----fb5f59af8bb0--------------------------------)

这部分稍微长一点。我将突出重点，主要是空间 SQL 部分。

```py
// This function transforms and loads the data into postgis
export async function transformAndLoad(geojsonFeatures: Feature[], pool: pg.Pool) {
    const client = await pool.connect();

    try {
      await client.query('BEGIN'); // Start a new transaction

      for (const geojsonPolygon of geojsonFeatures) {
        const geometryType = geojsonPolygon.geometry.type;
        const coordinates = geojsonPolygon.geometry.coordinates;

        const insertQuery = `
          INSERT INTO public.data (geometry, name, type,id)
          VALUES (ST_setsrid(ST_GeomFromGeoJSON($1),4326), $2, $3,$4)
        `;

        const values = [
          JSON.stringify({ type: geometryType, coordinates }),
          geojsonPolygon.properties.name,    
          geojsonPolygon.properties.type,    
          getISOstring()
        ];

        await client.query(insertQuery, values); // Insert the GeoJSON data
      }

      await client.query('COMMIT'); // Commit the transaction if successful
        // await client.query('ROLLBACK');
        console.log('Bulk insert of POLYGON features successful');
    } catch (error) {
      await client.query('ROLLBACK'); // Roll back the transaction on error
      console.error('Bulk insert failed:', error);
    } finally {
      client.release(); // Release the database connection\
      pool.end()
    }
  }
```

## 使用 Node-postgres 进行安全数据事务

在 PostgreSQL 中插入批量行的基本代码如下：

```py
await client.query('BEGIN');
try {
// ... start inserting data...
    await client.query('COMMIT');
} catch {
    // this code runs when we can't insert the data somehow. handle the error.
    // cancels the data transactions as a whole, avoiding duplicates.
    await client.query('ROLLBACK');
}
```

`client.query("BEGIN")` 允许我们为每条数据记录进行插入过程的分阶段处理。这意味着如果在插入过程中出现问题（例如，第 n 行的数据无效，导致 PostgreSQL 抛出错误），我们可以取消整个数据事务。这个取消操作通过 `client.query('ROLLBACK')` 完成。如果所有行都正确插入且没有错误，我们可以将其声明为安全插入，并调用 `client.query("COMMIT")` 来提交数据事务。

如果我们不以 `client.query("BEGIN")` 开始，那么在插入过程中发生的任何错误都会导致数据被插入到数据库中。在大多数情况下，我们不希望这样。

## 参数化查询

让我们看看在调用 `client.query("COMMIT")` 后我们会做什么。这是我们转换并插入空间数据的地方。转换的部分不多，因为我只是提取了一些属性。

```py
// transformation
const geometryType = geojsonPolygon.geometry.type;
const coordinates = geojsonPolygon.geometry.coordinates;
```

最重要的一点是如何使用参数化查询。一个常见的 SQL 漏洞是 [SQL 注入](https://www.w3schools.com/sql/sql_injection.asp#:~:text=SQL%20injection%20is%20a%20code,statements%2C%20via%20web%20page%20input.)。参数化查询通过不将原始 SQL 插入到我们的数据事务中来缓解这个漏洞。看看下面的 SQL 命令。

```py
// Correct way
const insertQuery = `
          INSERT INTO public.data (geometry, name, type,id)
          VALUES (ST_setsrid(ST_GeomFromGeoJSON($1),4326), $2, $3,$4)
        `;
```

请注意字符串 `$1` 、 `$2` 、 `$3` 和 `$4`。这些是我们插入的实际值的占位符。我们**不应该**使用 [模板字面量](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) 来格式化我们的字符串。在 JavaScript Node-postgres 中，你不应该这样做。

```py
// DON'T DO THIS
let value: string;
const insertQuery = `
          INSERT INTO public.data (name)
          VALUES ("${value}")
        `;
```

你可能会让数据库面临恶意查询。例如，如果`${value}`的值是`DROP TABLE`命令，那么函数将执行`DROP TABLE`命令！如果我们使用`$1`参数化查询，这种情况是不会发生的。

## 使用空间 SQL 插入

现在让我们来看一下插入语句以及我如何处理空间数据。

```py
-- inserting spatial data
INSERT INTO public.data (geometry, name, type,id)
VALUES (ST_setsrid(ST_GeomFromGeoJSON($1),4326), $2, $3,$4)
```

`ST_GeomFromGeoJSON` 是一个 postgis 函数，它接受一个*字符串化*的几何体并将其转换为几何对象。例如，

```py
ST_GeomFromGeoJSON('{"type":"Point","coordinates":[-48.23456,20.12345]}')
```

然后我们将其包装在`ST_SetSRID`函数中以声明其投影。我在这里使用 EPSG 代码`4326`来表示这是经纬度值。你可能会收到不同投影的数据。例如，在英国，数据通常会投影到 British National Grid，其代码为`27700`；北坐标和东坐标。这对于这个演示不适用。

## 释放连接

最后，我们释放客户端，以便服务器可以处理其他客户端。

```py
client.release(); 
pool.end();
```

# 运行代码

运行代码使用`npm run start`，这会运行`npx tsc`（这将 TypeScript 代码编译成 JavaScript）和`node build`（这实际上执行运行 ETL 过程的 JavaScript 代码）。日志显示操作成功。

![](img/cbea3ca1aeb7ac9b4931717945b754db.png)

执行代码（来源：作者，2023）

让我们通过选择数据在 PgAdmin 或普通的 psql 中查看数据。类似于，

```py
SELECT name, type, id, geometry FROM data
```

使用 PgAdmin，我们可以看到数据在地图上是合理的！这意味着数据分析师现在可以开始分析数据了。

![](img/44a8b40409f93190c41f39bef0a381ff.png)

查看数据（来源：作者，2023）

我们还可以在 QGIS 中开始玩转数据，QGIS 是一个开源的 GIS 软件，我们可以用来分析地理空间数据。

![](img/eb70196573cf6c3160d828837f909d30.png)

使用 QGIS 查看数据（来源：作者，2023）

# 结论

空间数据工程的主要思想是创建一个数据管道，从一个来源提取原始数据，对其进行处理，并将其存储以供使用。这使得空间数据分析师可以专注于数据分析，而不必担心数据的可用性；这由空间数据工程师负责处理。空间数据工程的特殊之处在于我们如何处理空间数据，即使用空间 SQL。使用 TypeScript 和 Node.JS，借助 `node-postgres`，我们可以构建一个简单的数据管道。数据管道获取 geojson 数据，对其进行转换，并将其存储在启用了 Postgis 的 PostgreSQL 数据库服务器中。这些存储的数据随后可以使用 GIS 软件进行分析。
