# 用 Go 构建 REST API：时间序列的数据分析

> 原文：[`towardsdatascience.com/build-a-rest-api-with-go-data-analytics-for-time-series-8d3f0747665c`](https://towardsdatascience.com/build-a-rest-api-with-go-data-analytics-for-time-series-8d3f0747665c)

## 一个关于使用 Go、Gin 和 Gorm 进行 CRUD 操作和统计分析的逐步示例。

[](https://medium.com/@davide.burba?source=post_page-----8d3f0747665c--------------------------------)![Davide Burba](https://medium.com/@davide.burba?source=post_page-----8d3f0747665c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8d3f0747665c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8d3f0747665c--------------------------------) [Davide Burba](https://medium.com/@davide.burba?source=post_page-----8d3f0747665c--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8d3f0747665c--------------------------------) ·阅读时间 10 分钟·2023 年 11 月 7 日

--

![](img/a6e61301baab7f2a0bed1c777ea8da26.png)

“学习 Go”，作者 [Giulia Roggia](https://www.instagram.com/giulia_roggia__/)。经许可使用。

+   介绍

+   模型与数据库

+   统计

+   处理程序

+   用法

# 介绍

本文介绍了一个使用 Go 构建的 REST API 示例，用于执行 CRUD（创建、读取、更新、删除）操作并计算时间序列数据的统计信息。

*本文中使用的完整代码可以在* [*这里*](https://github.com/davide-burba/code-collection/)*找到*。

## *为什么选择 Go？*

[Go](https://go.dev/) 是构建 REST API 的常见选择，原因有很多。尽管它是高效的编译语言，但其语法简单且可读性强。它使得实现并发处理变得容易。它提供了功能丰富的标准库，并且拥有一个出色的库和工具生态系统。

在这个例子中，我们使用了两个流行的 Go 库：

+   [Gin](https://gin-gonic.com/): 一个提供创建 Web 应用工具的 Web 框架。

+   [Gorm](https://gorm.io/): 一个功能全面的 ORM（对象关系映射），用于与数据库交互。

## 文件夹结构

我们为每个“服务”创建一个文件夹。在 Go 中，每个文件夹对应一个包，每个文件都可以访问同一包中其他文件定义的元素。以下是项目中使用的文件夹结构：

```py
├── database
│   └── database.go
├── models
│   └── models.go
├── handlers
│   ├── routes.go
│   ├── stats.go
│   ├── timeseries.go
│   └── timeseriesvalues.go
├── stats
│   └── stats.go
├── go.mod
├── go.sum
└── main.go
```

# 模型与数据库

首先在 *models.go* 中定义 ORM 模型，以表示时间序列数据。我们使用了两个模型，一个用于标识序列，另一个用于存储其值。每个值通过外键与时间序列相关联。

```py
type TimeSeries struct {
 ID   int    `gorm:"primaryKey"`
 Name string `gorm:"not null"`
}

type TimeSeriesValue struct {
 ID           int `gorm:"primaryKey"`
 Time         time.Time
 Value        float64
 TimeSeriesID int `gorm:"not null"`
}
```

我们还定义了两个用于时间序列值的 getter，这在计算统计数据时将非常有用。

```py
func (v TimeSeriesValue) GetTime() time.Time { return v.Time }
func (v TimeSeriesValue) GetValue() float64  { return v.Value }
```

在*database.go*中，我们定义了一个获取数据库的函数（在示例中使用*SQLite*），并应用迁移以创建每个模型的 SQL 表（如果尚不存在）。

```py
func GetDatabase(dbFile string) (*gorm.DB, error) {
 return gorm.Open(sqlite.Open(dbFile), &gorm.Config{})
}

func AutoMigrate(db *gorm.DB) {
 db.AutoMigrate(&models.TimeSeries{}, &models.TimeSeriesValue{})
}
```

# 统计量

注意：如果你只对 CRUD 部分感兴趣，可以跳过这一部分。

由于统计量的计算是一个常见任务，为了提高代码的可重用性，我们将其开发为一个“独立”包，这意味着它不依赖于项目中定义的其他包。为此，我们定义一个`TsValue`接口：统计函数的输入是一个值的切片，每个值必须定义接口方法`GetTime`和`GetValue`。

```py
// Interface for a data point in a time series
type TsValue interface {
 GetTime() time.Time
 GetValue() float64
}
```

现在我们为每个统计量定义一个函数：

+   `Count`：系列的长度。

```py
func Count(values []TsValue) float64 {
 return float64(len(values))
}
```

+   `Min`：最小值（以及`Max`，其功能类似）。

```py
func Min(values []TsValue) float64 {
 if len(values) == 0 {
  return math.NaN()
 }
 min := values[0].GetValue()
 for _, value := range values {
  if value.GetValue() < min {
   min = value.GetValue()
  }
 }
 return min
}
```

+   `Mean`：系列的均值。

```py
func Mean(values []TsValue) float64 {
 count := Count(values)
 if count == 0 {
  return math.NaN()
 }
 sum := 0.0
 for _, value := range values {
  sum += value.GetValue()
 }
 return sum / count
}
```

+   `StandardDeviation`：系列的标准差。

```py
func StandardDeviation(values []TsValue) float64 {
 count := Count(values)
 if count == 0 {
  return math.NaN()
 }
 mean := Mean(values)
 sumSquare := 0.0
 for _, value := range values {
  sumSquare += math.Pow(value.GetValue()-mean, 2)
 }
 return math.Sqrt(sumSquare / count)
}
```

我们可以将统计量集中在一个函数中：

```py
func ComputeStatistics(values []TsValue) map[string]float64 {
 result := make(map[string]float64)

 result["Count"] = Count(values)
 result["Min"] = Min(values)
 result["Max"] = Max(values)
 result["Mean"] = Mean(values)
 result["StdDev"] = StandardDeviation(values)

 return result
}
```

请注意，函数`ComputeStatistics`是顺序计算统计量的。如果每个系列中的值很多，这可能会变得耗时。如果是这样，我们可以使用 goroutines 并发计算统计量。

让我们定义一个并发版本`ComputeStatisticsConcurrent`，其执行以下步骤：

1.  创建一个通道来收集结果，一个同步 goroutines 的等待机制，以及一个包含要计算的统计量的映射。

1.  设置一个计数器来计算要计算的统计量数量。

1.  为每个统计函数启动一个 goroutine。

1.  设置一个特殊的 goroutine，以便在所有统计量计算完成后（即计数器归零时）关闭通道。

1.  从通道中收集结果。收集在通道关闭时停止。

```py
func ComputeStatisticsConcurrent(values []TsValue) map[string]float64 {
 result := make(map[string]float64)
 // 1) Create a WaitGroup to sync, a Channel to collect results, and
 // a map with the statistics to compute.
 var wg sync.WaitGroup
 ch := make(chan struct {
  string
  float64
 })
 statsToCompute := map[string]func([]TsValue) float64{
  "Count":             Count,
  "Min":               Min,
  "Max":               Max,
  "Mean":              Mean,
  "StandardDeviation": StandardDeviation,
 }

 // 2) Set how many stats to compute.
 wg.Add(len(statsToCompute))

 // 3) Compute each stat in a separate goroutine.
 for statName, statFunc := range statsToCompute {
  go func(name string, operation func([]TsValue) float64) {
   defer wg.Done()
   value := operation(values)
   ch <- struct {
    string
    float64
   }{name, value}
  }(statName, statFunc)
 }

 // 4) Set a goroutine to close the channel once all the
 // stats are computed.
 go func() {
  wg.Wait()
  close(ch)
 }()

 // 5) Collect the results from the channel.
 for stat := range ch {
  result[stat.string] = stat.float64
 }

 return result
}
```

**注意**：如果你想在不使用大数据的情况下测试性能提升，可以在每个统计函数内部运行`time.Sleep`进行模拟。

# 处理程序

现在让我们定义 REST API 的端点。因为：

+   端点逻辑依赖于数据库。

+   我们不想使用全局变量。

+   端点函数必须只接受 Gin 上下文指针作为输入，该指针包含请求信息。

我们创建一个数据库包装器，并将端点方法分配给它：

```py
// Create a new type that embeds database.Database to assign new
// methods to it
type wrapDB struct {
 DB *gorm.DB
}
```

我们定义一个函数来设置所有端点。每个端点指定一个路径，并指定一个接受 Gin 上下文指针的函数。

```py
func SetEndpoints(r *gin.Engine, db *gorm.DB) {

 wrapdb := &wrapDB{DB: db}

 // timeseries CRUD
 r.GET("/timeseries", wrapdb.listTimeSeries)
 r.GET("/timeseries/:tsid", wrapdb.getTimeSeries)
 r.POST("/timeseries", wrapdb.postTimeSeries)
 r.PUT("/timeseries/:tsid", wrapdb.putTimeSeries)
 r.DELETE("/timeseries/:tsid", wrapdb.deleteTimeSeries)

 // timeseries values CRUD
 r.GET("/timeseries/:tsid/values", wrapdb.listTimeSeriesValues)
 r.GET("/timeseries/:tsid/values/:valueid", wrapdb.getTimeSeriesValue)
 r.POST("/timeseries/:tsid/values", wrapdb.postTimeSeriesValues)
 r.PUT("/timeseries/:tsid/values/:valueid", wrapdb.putTimeSeriesValue)
 r.DELETE("/timeseries/:tsid/values/:valueid", wrapdb.deleteTimeSeriesValue)

 // statistics
 r.GET("/timeseries/:tsid/statistics", wrapdb.getTimeSeriesStats)
}
```

## 处理程序：系列

让我们从定义时间序列端点开始。下面是列出所有可用时间序列的实现。当调用`DB.Find`时，时间序列存储在`tseriesList`切片中。

```py
func (db *wrapDB) listTimeSeries(c *gin.Context) {
 var tseriesList []models.TimeSeries
 if err := db.DB.Find(&tseriesList).Error; err != nil {
  c.JSON(500, gin.H{"error": "Failed to retrieve timeseries"})
  return
 }
 c.JSON(200, tseriesList)
}
```

在开发`TimeSeries`的基础 CRUD 端点之前，让我们定义一个辅助函数来从请求中获取系列 ID 并执行一些检查。

```py
func (db *wrapDB) checkTimeSeriesID(c *gin.Context) (int, error) {
 id := c.Param("tsid")
 // Check if the time series exists can be converted to
 timeSeriesID, err := strconv.Atoi(id)
 if err != nil {
  c.JSON(400, gin.H{"error": "Invalid time series ID"})
  return timeSeriesID, err
 }

 // Check if the time series exists in the TimeSeries table
 var timeSeries models.TimeSeries
 if err := db.DB.First(&timeSeries, timeSeriesID).Error; err != nil {
  c.JSON(404, gin.H{"error": "Time series not found"})
  return timeSeriesID, err
 }
 return timeSeriesID, nil
}
```

我们现在可以定义`TimeSeries`的基本 CRUD 操作：

+   *创建*：POST 一个新的系列。

```py
func (db *wrapDB) postTimeSeries(c *gin.Context) {
 var tseries models.TimeSeries
 c.BindJSON(&tseries)
 if err := db.DB.Create(&tseries).Error; err != nil {
  c.JSON(500, gin.H{"error": "Failed to create timeseries"})
  return
 }
 c.JSON(201, tseries)
}
```

+   *读取*：GET 一个现有系列。

```py
func (db *wrapDB) getTimeSeries(c *gin.Context) {
 timeSeriesID, err := db.checkTimeSeriesID(c)
 if err != nil {
  return
 }
 var tseries models.TimeSeries
 if err := db.DB.Where("id = ?", timeSeriesID).First(&tseries).Error; err != nil {
  c.JSON(404, gin.H{"error": "Failed to retrieve timeseries"})
  return
 }
 c.JSON(200, tseries)
}
```

+   *更新*：PUT 一个现有系列。

```py
func (db *wrapDB) putTimeSeries(c *gin.Context) {
 timeSeriesID, err := db.checkTimeSeriesID(c)
 if err != nil {
  return
 }
 var tseries models.TimeSeries
 if err = db.DB.Where("id = ?", timeSeriesID).First(&tseries).Error; err != nil {
  c.JSON(404, gin.H{"error": "Time series not found"})
  return
 }
 c.BindJSON(&tseries)
 if err = db.DB.Save(&tseries).Error; err != nil {
  c.JSON(500, gin.H{"error": "Error while saving"})
 }
 c.JSON(200, tseries)
}
```

+   *删除*：删除一个系列及其值。为了在运行时出现错误时进行回滚，我们在数据库事务中执行这两个操作。

```py
// Delete a time series and its values
func (db *wrapDB) deleteTimeSeries(c *gin.Context) {
 timeSeriesID, err := db.checkTimeSeriesID(c)
 if err != nil {
  return
 }
 var tseries models.TimeSeries

 // Delete in a transaction
 db.DB.Transaction(func(tx *gorm.DB) error {

  // Delete values
  if err := tx.Where("time_series_id = ?", timeSeriesID).
   Delete(&models.TimeSeriesValue{}).Error; err != nil {
   c.JSON(500, gin.H{"error": "Deleting the timeseries failed."})
   return err
  }

  // Delete timeseries
  if err := tx.Where("id = ?", timeSeriesID).
   Delete(&tseries).Error; err != nil {
   c.JSON(500, gin.H{"error": "Deleting the timeseries failed."})
   return err
  }

  c.JSON(200, gin.H{"id #" + strconv.Itoa(timeSeriesID): "deleted"})
  return nil
 })

}
```

## 处理程序：值

我们现在可以定义 `TimeSeriesValues` 的端点。由于它们与我们刚刚定义的端点类似，我们仅展示 *Create* 方法的实现，它与系列方法不同，因为我们允许同时发布多个值。

```py
func (db *wrapDB) postTimeSeriesValues(c *gin.Context) {
 timeSeriesID, err := db.checkTimeSeriesID(c)
 if err != nil {
  return
 }

 // Bind the request body to a slice of TimeSeriesValue
 var timeSeriesValues []models.TimeSeriesValue
 if err := c.ShouldBindJSON(&timeSeriesValues); err != nil {
  c.JSON(400, gin.H{"error": "Invalid request data"})
  return
 }
 // Set the TimeSeriesID for the posted values
 for i := range timeSeriesValues {
  timeSeriesValues[i].TimeSeriesID = timeSeriesID
 }

 // Create the values in the database
 if err := db.DB.Create(&timeSeriesValues).Error; err != nil {
  c.JSON(500, gin.H{"error": "Failed to create time series values"})
  return
 }
 c.JSON(201, gin.H{"message": "Time series values created"})
}
```

## 处理程序：统计信息

我们需要定义的最后一个处理程序是计算给定系列的统计信息。根据其 ID，我们查询值并使用之前实现的 `ComputeStatisticsConcurrent` 函数计算统计信息。

```py
func (db *wrapDB) getTimeSeriesStats(c *gin.Context) {
 timeSeriesID, err := db.checkTimeSeriesID(c)
 if err != nil {
  return
 }
 // Query the database.Database to retrieve specific fields of time series values
 var tsValues []models.TimeSeriesValue
 if err := db.DB.Where("time_series_id = ?", timeSeriesID).
  Select("id, time, value").Find(&tsValues).Error; err != nil {
  c.JSON(500, gin.H{"error": "Failed to retrieve time series values"})
  return
 }
 // Convert the slice of TimeSeriesValue to a slice of TsValue interfaces
 values := []stats.TsValue{}
 for _, v := range tsValues {
  values = append(values, v)
 }
 c.JSON(200, serializeMap(stats.ComputeStatisticsConcurrent(values)))
}
```

在返回统计信息之前，我们将 `serializeMap` 函数应用于输出。这是为了将 *NaN* 值转换为 *nil*，以便使输出可以进行 JSON 序列化。注意，映射中的输出值类型是空接口 `interface{}`，它可以容纳任何类型的值。

```py
// SerializeMap serializes a map from string to float64,
// returning null for NaN values
func serializeMap(data map[string]float64) map[string]interface{} {
 serializedData := make(map[string]interface{})
 for key, value := range data {
  if math.IsNaN(value) {
   serializedData[key] = nil
  } else {
   serializedData[key] = value
  }
 }
 return serializedData
}
```

# 使用方法

使应用程序可用的最后一段代码是 `main` 函数，用于运行 Web 服务器。我们提供了三个可选的命令行参数：

+   `db`：指定数据库文件的名称（默认：`timeseries.db`）

+   `proxy`：设置服务器的受信任代理（默认：`127.0.0.1`）

+   `port`：定义服务器监听的端口号（默认：`8080`）

```py
func main() {
 // Define flags for command-line arguments
 dbName := flag.String("db", "timeseries.db", "Database name")
 trustedProxy := flag.String("proxy", "127.0.0.1", "Trusted proxy")
 port := flag.String("port", "8080", "Port number")
 flag.Parse()

 // Initialize the database
 db, err := database.GetDatabase(*dbName)
 if err != nil {
  panic("failed to connect to the database")
 }
 database.AutoMigrate(db)

 // Set the trusted proxies
 router := gin.Default()
 router.SetTrustedProxies([]string{*trustedProxy})

 // Set endpoints and run the server
 handlers.SetEndpoints(router, db)
 router.Run(fmt.Sprintf(":%s", *port))

}
```

## 构建

你现在可以通过以下方式运行 API：

1.  使用 `go mod init` 初始化一个新的 Go 模块。这将生成一个带有模块名称和 Go 版本的 *go.mod* 文件。

1.  使用 `go mod tidy` 跟踪依赖项。这将把项目依赖项添加到 *go.mod* 文件中，并创建一个包含所有依赖项校验和的 *go.sum* 文件（用于确保其完整性）。

1.  使用 `go build` 构建程序并运行生成的可执行文件。这也可以通过 `go run main.go` 一步完成。

*go.mod* 和 *go.sum* 文件已在 [项目仓库](https://github.com/davide-burba/code-collection/tree/main/examples/go-timeseries-api) 中提供。你可以在 [这里](https://pkg.go.dev/cmd/go) 阅读更多关于 `go` 命令的信息。

我们还可以使用 Docker 运行 API。在下面显示的 *Dockerfile* 中，首先，我们使用 Go 官方镜像构建可执行文件，然后将其复制到一个最小的镜像中，这样得到一个约 20Mb 的小文件。

```py
# Stage 1: Build the Go application

FROM golang:1.21-alpine3.18 AS build
WORKDIR /app
COPY . .
RUN apk add build-base
# Download (and cache) all dependencies.
RUN go mod download
# Build the Go app with static linking
RUN go build -ldflags="-w -s" -o tsapi

# Stage 2: Create a minimal image for running the application

FROM alpine:3.18
WORKDIR /app
COPY --from=build /app/tsapi .
EXPOSE 8080
CMD ["./tsapi"]
```

我们可以按照以下步骤使用 Docker 构建和执行 API：

```py
# Build the docker image
docker build -t go-timeseries-api .
# Run the docker image
docker run -p 8080:8080 go-timeseries-api
```

## 示例用法

应用程序启动并运行后，我们可以通过 API 调用来测试它。下面我们展示了一个使用 `curl` 的示例。我们创建一个时间序列，发布其值，计算统计信息，最后删除它。

```py
# Create a time series
curl -i -X POST http://localhost:8080/timeseries -d '{ "Name": "My time series"}'
> {"ID":1,"Name":"My time series"}

# Create time series values
curl -i -X POST http://localhost:8080/timeseries/1/values -d '[{"Time": "2023-10-28T12:00:00Z", "Value": 10.0},{"Time": "2023-10-28T12:15:00Z", "Value": 20.5}]'
> {"message":"Time series values created"}

# Get statistics
curl -i -GET http://localhost:8080/timeseries/1/statistics
> {"Count":2,"Max":20.5,"Mean":15.25,"Min":10,"StandardDeviation":5.25}

# Delete a time series
curl -i -X DELETE http://localhost:8080/timeseries/1
> {"id #1":"deleted"}
```

# 关于学习 Go 的说明

这是我在 Go 中的第一个项目。因此，如果你有任何反馈，我将非常感激！

我来自 Python 背景，我特别喜欢 Go 的速度和轻量性，并发现它比 C++ 学起来容易多了。以下是一些帮助我入门的资源：

+   [官方 Go 教程](https://go.dev/doc/tutorial/)

+   [Go 之旅](https://go.dev/tour/basics/1)：对 Go 的互动式介绍，也是官方的

+   [使用 Go、Gin 和 Gorm 开发简单的 CRUD API](https://cgrant.medium.com/developing-a-simple-crud-api-with-go-gin-and-gorm-df87d98e6ed1)

+   [寻找最佳 Go 项目结构](https://itnext.io/finding-the-best-go-project-structure-part-1-5290bc1d869d)

*本文中使用的完整代码可以在* [*这里*](https://github.com/davide-burba/code-collection/) *找到。*

*喜欢这篇文章吗？* [*查看我的其他文章*](https://medium.com/@davide.burba) *并关注我以获取更多内容！* [*点击这里*](https://medium.com/@davide.burba/membership) *以无限阅读文章并以零额外费用支持我* ❤️
