# 学习 RabbitMQ 用于事件驱动架构（EDA）

> 原文：[`towardsdatascience.com/learn-rabbitmq-for-event-driven-architecture-eda-e1e7377db2b`](https://towardsdatascience.com/learn-rabbitmq-for-event-driven-architecture-eda-e1e7377db2b)

## 一份适合初学者的教程，介绍 RabbitMQ 的工作原理以及如何在 Go 中使用 RabbitMQ，这是学习 EDA 的第一步。

[](https://programmingpercy.medium.com/?source=post_page-----e1e7377db2b--------------------------------)![Percy Bolmér](https://programmingpercy.medium.com/?source=post_page-----e1e7377db2b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e1e7377db2b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e1e7377db2b--------------------------------) [Percy Bolmér](https://programmingpercy.medium.com/?source=post_page-----e1e7377db2b--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e1e7377db2b--------------------------------) ·阅读时长 39 分钟·2023 年 4 月 5 日

--

![](img/4e653e64d73dd82ca8b9ea9f8cdbc21e.png)

照片由 [Bradyn Trollip](https://unsplash.com/es/@bradyn?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

事件驱动架构（EDA）是我在编程中最喜欢的东西之一。这种架构允许我们构建微服务并轻松地在它们之间共享信息。

在常规的顺序软件中，你会有一个函数触发另一个函数，或者一个定期脚本来检查某些任务。

在事件驱动架构中，我们利用队列或发布/订阅模式。允许不同的服务之间通知或传送信息，以触发代码执行。

事件驱动架构通常用于构建高度灵活和可扩展的软件。这是因为可以通过简单地监听事件来轻松添加或移除功能。

这使得影子部署和测试新服务与生产环境并行变得非常容易，因为你可以让新服务对相同事件做出反应，而不会干扰正在运行的系统。

然而，并非所有的情况都是一帆风顺的，一些人认为事件驱动架构（EDA）系统可能略显复杂，而且在考虑到完整的服务流程时，测试有时会更困难。我认为测试其实更简单，因为我们可以轻松触发事件并查看相关服务或单个服务的反应。但如果没有适当的架构文档，也可能很难理解是什么触发了什么以及原因。

本教程将探讨如何使用 RabbitMQ 构建两个通过事件进行通信的微服务。我们将研究 RabbitMQ 中使用的不同范式，虽然我们将学习如何在 Go 中使用 RabbitMQ，但我们主要集中于学习 RabbitMQ 的概念。涵盖一些常见错误和一些最佳实践。

RabbitMQ 支持多种协议来发送数据，但在本教程中，我们将专注于使用**AMQP**。

在本教程中，我们将学习以下内容

+   使用 Docker 设置 RabbitMQ

+   虚拟主机、用户和权限

+   使用 CLI 管理 RabbitMQ，通过[rabbitmqctl](https://www.rabbitmq.com/rabbitmqctl.8.html)和[rabbitmqadmin](https://www.rabbitmq.com/management-cli.html)

+   了解生产者、消费者以及如何编写它们。

+   了解队列、交换机和绑定

+   使用工作队列（先进先出）

+   使用 RabbitMQ 进行发布/订阅

+   使用基于 RPC 的模式和回调。

+   使用 TLS 加密流量

+   使用配置来声明 RabbitMQ 中的资源

该教程的视频录制，适合喜欢视频的人。

本文中使用的所有代码可以在[这里](https://github.com/percybolmer/event-driven-rabbitmq)找到。

## 安装 RabbitMQ — 设置用户、虚拟主机和权限

使 RabbitMQ 运行可以通过[下载和安装](https://www.rabbitmq.com/download.html)中的示例完成。我建议在生产环境中遵循该指南，但为了本教程和实验，我们可以使用更简单的方法。

像往常一样，最简单的方法是运行 Docker！

此命令将下载最新的 RabbitMQ 并将其作为后台进程启动，暴露端口**5672**和**15672**。

```py
docker run -d --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.11-management
```

**端口 5672** 用于启用 AMQP 连接。[AMQP](https://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol) 是 RabbitMQ 和许多其他消息中间件使用的网络协议。

**端口 15672** 被开启，因为管理 UI 和管理界面托管在该端口，管理 RabbitMQ 的 API 也在该端口上。

有关端口的更多细节，请参阅 RabbitMQ 的[网络](https://www.rabbitmq.com/networking.html)指南。

一旦 Docker 启动，让我们开始访问托管在[localhost:15672](http://localhost:15672/)上的管理 UI。

![](img/fa1e17aca41710048e248924e3550620.png)

RabbitMQ 管理 UI — 图片由 Percy Bolmer 提供

哎呀，我们需要一个用户！让我们使用[RabbitMQCLI](https://www.rabbitmq.com/man/rabbitmqctl.8.html#)创建一个。别担心安装问题，它已经存在于我们运行的 Docker 容器中。

我们可以使用命令`add_user`来创建新用户，后跟用户名和密码。我们使用`docker exec rabbitmq`在 docker 内部执行命令，将`rabbitmq`替换为你为 docker 容器指定的名称。

```py
docker exec rabbitmq rabbitmqctl add_user percy secret
```

我建议在探索期间也授予管理员权限。我们可以通过给新用户添加管理员标签来实现这一点。

```py
docker exec rabbitmq rabbitmqctl set_user_tags percy administrator
```

哦，最后一件事，默认情况下有一个 `guest` 用户，我强烈建议删除此用户！此用户仅对使用本地主机的用户可用，但还是安全起见比较好。

```py
docker exec rabbitmq rabbitmqctl delete_user guest
```

就这样，回到管理 UI 并登录。

登录后你会看到一个看起来相当老旧的用户界面，但这非常好，因为我们可以真正从这里监控 RabbitMQ，并查看发生了什么。我们还不会玩弄这个界面，我们需要先有一个实际连接并发送数据的服务。

![](img/fa729b79b78b778f1fd5d9900932f8b0.png)

管理 UI 显示正在运行的实例指标 — 图片由 Percy Bolmer 提供

在我们开始操作之前，我们需要再修复两个问题。

RabbitMQ 中的资源，如队列以及我们将很快学习的其他内容，按照逻辑层进行分隔，这个逻辑层称为虚拟主机（Vhost）。

解释虚拟主机最简单的方法可能是将其与命名空间进行比较，但这可能在某些方面不完全正确。

我们可以使用这些虚拟主机将某些资源分组在一起，并通过添加允许使用虚拟主机的用户来限制访问。

让我们开始使用 `add_vhost` 命令创建虚拟主机，它接受一个输入，即虚拟主机的名称。

```py
docker exec rabbitmq rabbitmqctl add_vhost customers
```

现在我们有了一个虚拟主机，我们可以为之前创建的用户添加权限，以便它可以连接。

添加权限是通过 `set_permissions` 命令完成的，我们使用 `-p` 标志来指定要添加权限的虚拟主机。语法中的下一个项是要添加权限的用户。

命令的最后部分是令人害怕的部分，它是一个定义要添加权限的正则表达式，添加所有权限的示例如下，或者对所有以 `customer-` 开头的资源的权限为 `"^customer-*"`.

会有 3 个正则表达式槽位，按顺序配置以下权限。

+   **配置** — 对匹配正则表达式的资源的配置权限

+   **写** — 对匹配正则表达式的资源的写权限

+   **读取** — 对匹配正则表达式的资源的读取权限

为我的用户 `percy` 添加对 customer 虚拟主机的全面配置、写入和读取权限的完整命令如下。注意，我给予了 `.*` 的访问权限，即所有权限。

```py
docker exec rabbitmq rabbitmqctl set_permissions -p customers percy ".*" ".*" ".*"
```

创建完成后，你应该能在管理 UI 的右上角看到新的虚拟主机。

![](img/b41f62732624137163942d85b40401e7.png)

选择新的虚拟主机。 — 图片由 Percy Bolmer 提供

## RabbitMQ 基础知识 — 生产者、消费者、交换机和队列

![](img/1d186c50fdf4e89cd994e08072fbf8c0.png)

展示生产者、交换机、队列和消费者如何协同工作 — 图片由 Percy Bolmer 提供

当我们构建事件驱动架构时，有一些术语需要理解。

+   **生产者** — 任何发送消息的软件。

+   **消费者** — 任何接收消息的软件。

+   **队列** — 一个队列接受一个消息，输出这个消息，可以将其视为一个大型缓冲区。队列是 FIFO（先进先出）的，这意味着消息按插入队列的顺序输出。

+   **交换机** — 一种路由器，是生产者发送消息的地方。交换机接受消息，并根据交换机的类型和应用的绑定（规则）将其发送到正确的队列。

一般来说，我们可以使用这个来在服务之间发送和接收消息。值得一提的是，生产者和消费者不需要在同一主机上运行，这使得系统具有很好的扩展性。

首先创建一个新的 Go 项目，如果你还没有安装 Go，请从[这里](https://go.dev/dl/)进行安装。

> 在实际的 Go 项目设置中，我可能会使用 Cobra，但为了避免新用户感到困惑，我将简单地创建两个主要包。

让我们在 Go 中构建一个生产者，能够开始在队列上发送消息。

开始为生产者创建一个新的项目，并获取由 RabbitMQ 团队官方维护的 AMQP 库。

该项目将有一个`cmd`文件夹，包含所有不同的服务，每个服务都是一个独立的可运行程序。

我们还将有一个`internal`文件夹，用于存储共享库等。

```py
mkdir eventdriven
cd eventdriven
mkdir -p cmd/producer
mkdir internal
touch cmd/producer/main.go
touch internal/rabbitmq.go
go mod init programmingpercy.tech/eventdrivenrabbit
go get github.com/rabbitmq/amqp091-go
```

你的文件夹结构应该如下所示。

![](img/d56f2c2dba053362e913ddd9601c1c70.png)

`cmd`文件夹和`internal`文件夹已准备好 — 图片来自 Percy Bolmer

首先在`internal/rabbitmq.go`中添加一个与 RabbitMQ 实例的连接。

我们将创建一个小的助手函数，用于通过`amqp`协议连接到 RabbitMQ。我们将允许用户指定凭证、主机以及要连接的 vhost。

我将简单地返回指向连接的指针，即网络连接和用于并发发送消息的`amqp.Channel`。将连接的管理留给用户。

```py
package internal

import (
 "context"
 "fmt"

 amqp "github.com/rabbitmq/amqp091-go"
)

// RabbitClient is used to keep track of the RabbitMQ connection
type RabbitClient struct {
 // The connection that is used
 conn *amqp.Connection
 // The channel that processes/sends Messages
 ch *amqp.Channel
}

// ConnectRabbitMQ will spawn a Connection
func ConnectRabbitMQ(username, password, host, vhost string) (*amqp.Connection, error) {
 // Setup the Connection to RabbitMQ host using AMQP
 conn, err := amqp.Dial(fmt.Sprintf("amqp://%s:%s@%s/%s", username, password, host, vhost))
 if err != nil {
  return nil, err
 }
 return conn, nil
}

// NewRabbitMQClient will connect and return a Rabbitclient with an open connection
// Accepts a amqp Connection to be reused, to avoid spawning one TCP connection per concurrent client
func NewRabbitMQClient(conn *amqp.Connection) (RabbitClient, error) {
 // Unique, Conncurrent Server Channel to process/send messages
 // A good rule of thumb is to always REUSE Conn across applications
 // But spawn a new Channel per routine
 ch, err := conn.Channel()
 if err != nil {
  return RabbitClient{}, err
 }

 return RabbitClient{
  conn: conn,
  ch:   ch,
 }, nil
}

// Close will close the channel
func (rc RabbitClient) Close() error {
 return rc.ch.Close()
}
```

一个很好的经验法则是，在整个应用程序中重用一个连接，并为并发任务创建新的通道。原因是连接是 TCP 连接，而通道是在分配的 TCP 连接中的多路复用连接。遵循这个经验法则可以实现更具可扩展性的解决方案。

让我们将这个简单的客户端导入到`cmd/producer/main.go`中并尝试连接，看看会发生什么。

现在，我们将简单地连接并在关闭连接前休眠 30 秒。

```py
package main

import (
 "log"
 "programmingpercy/eventdrivenrabbit/internal"
 "time"
)

func main() {
 conn, err := internal.ConnectRabbitMQ("percy", "secret", "localhost:5672", "customers")

 if err != nil {
  panic(err)
 }
 defer conn.Close()
 client, err := internal.NewRabbitMQClient(conn)
 if err != nil {
  panic(err)
 }
 defer client.Close()

 time.Sleep(30 * time.Second)

 log.Println(client)
}
```

一旦完成这些设置，就运行生产者。

```py
go run cmd/producer/main.go
```

一旦运行，返回管理 UI 并查看我们是否可以看到现在有一个连接和一个通道。

![](img/0b425b3b04b7c48200a4bc00a617274d.png)

我们现在有一个连接和一个通道，而不是零个 — 图片来自 Percy Bolmer

通道是一种非常聪明的处理 TCP 层的方法，你可以在[文档](https://www.rabbitmq.com/channels.html)中阅读更多内容。它允许用户在多个通道之间重用一个打开的 TCP 连接，而不是打开多个 TCP 连接。这是一种复用技术。

现在是开始发送数据的时候了，这是在上述通道上完成的。通道的功能远超过你可能想象的，它不仅仅是一个简单的管道，还可以在创建时配置一些巧妙的选项。

我们可以从 UI 创建队列，但我喜欢在测试时通过代码创建队列。在生产环境中，我喜欢使用配置文件来声明一些基本设置，我们稍后会讨论这个问题。

我们可以通过调用 `amqp.QueueDeclare` 来创建队列，这个函数有许多输入参数，我们需要理解这些参数以获得想要的队列行为。函数签名如下所示。

```py
func (*amqp.Channel).QueueDeclare(name string, durable bool, autoDelete bool, exclusive bool, noWait bool, args amqp.Table) (amqp.Queue, error)
```

+   **名称** — 用于引用队列的名称。此项可以为空，在这种情况下，服务器将生成一个名称。

+   **持久化** — 如果队列在 Broker 重启（RabbitMQ 重启）时应该被保留

+   **自动删除** — 如果队列在最后一个消费者离开时应自动删除

+   **独占** — 仅适用于创建队列的相同连接。

+   **无等待** — 假定队列在服务器上创建

+   **参数** — 提供用户提供的参数的选项。

为了让这件事更简单，我会创建一个接受 `name`、`durable` 和 `autodelete` 参数的包装函数。我将默认禁用其他参数。

```py
// CreateQueue will create a new queue based on given cfgs
func (rc RabbitClient) CreateQueue(queueName string, durable, autodelete bool) error {
 _, err := rc.ch.QueueDeclare(queueName, durable, autodelete, false, false, nil)
 return err
}
```

让我们更新 `producer/main.go` 以执行新的 CreateQueue 函数，我将创建一个持久化队列，因为我希望处理新客户的队列保持活跃和持久，我还会将自动删除设置为 `false`。

我还会创建一个名为 `customers_test` 的非持久化队列，以展示区别。

```py
func main() {
 conn, err := internal.ConnectRabbitMQ("percy", "secret", "localhost:5672", "customers")

 if err != nil {
  panic(err)
 }
 defer conn.Close()

 client, err := internal.NewRabbitMQClient(conn)
 if err != nil {
  panic(err)
 }
 defer client.Close()

 if err := client.CreateQueue("customers_created", true, false); err != nil {
  panic(err)
 }
 if err := client.CreateQueue("customers_test", false, true); err != nil {
  panic(err)
 }

 time.Sleep(10 *time.Second)

 log.Println(client)

}
```

添加完后，请确保执行生产者。

```py
go run cmd/producer/main.go
```

你可以访问 UI 并查看应该都可用的队列。请注意，一旦程序退出，`customers_test` 队列没有被删除，这是因为我们还没有消费者连接。只有连接了消费者的队列才会被删除。

![](img/17f2a047cdbff9a7c58cb0322e5b3e47.png)

customers-test 使用自动删除创建，一旦程序退出，它将被删除。— 图片来源：Percy Bolmer

为了好玩，你可以尝试现在重启 RabbitMQ，看看 `customers_test` 是如何消失的，因为它没有被标记为持久化。

```py
docker restart rabbitmq
```

## 探索交换机和绑定

在我们开始在队列上发送消息之前，我们需要创建一个**交换机**。已经创建了一些默认的交换机，但我们将创建自己的交换机以了解更多信息。

交换机是 RabbitMQ 的一个重要部分，它是我们发送消息的资源。交换机的工作是将消息发送到正确的队列。

要开始接收队列上的消息，该队列需要绑定到一个交换上，这被称为**绑定**。绑定基本上是一个路由规则。一个重要的点是，队列可以绑定到多个交换，这也使得不同交换类型的意义更加明确。

有几种不同类型的交换，每种交换在消息发送方式上有不同的行为。

首先，我们有最基本的**Direct**交换。这种交换非常简单，消息是基于其**确切的**路由键进行路由的。在示例图中，我们看到发送到`customer_created`的消息仅通过交换`customer_events`路由到那个特定的队列。直接交换在需要将工作分配给一组工作者时非常有用。

![](img/66d92b7e3d7bb700613ef8ffc855ce32.png)

直接交换（Direct Exchange）—— 只有直接匹配`customer_created`的消息才会收到匹配 —— 图片由 Percy Bolmer 提供

第二种类型是**Fanout**交换，它用于将消息发送到所有绑定的队列。任何绑定到交换的队列都会接收到消息，路由键会被简单地忽略！这通常用于将消息广播给任何感兴趣的方。

![](img/dbcf592768bda32121c1836b6598b169.png)

扩展交换（Fanout Exchange）—— 任何绑定的队列都接收消息 —— 图片由 Percy Bolmer 提供

然后我们有**Topic**交换，这种交换非常酷。它们允许绑定指定规则以根据路由键选择消息的子集。

路由键在每个词之间用`.`分隔，例如`customers.eu.stockholm`。这可能是来自瑞典斯德哥尔摩的客户的路由键，然后我们可以有一个绑定来告诉交换机某个队列想要这些消息，但不包括`customers.us.florida`。

有几个特殊字符，`#`表示零个或多个匹配，因此例如`customers.#`将匹配任何以`customers.`开头的路由键。

还有`*`，它是特定位置的特定词，例如`customers.*.stockholm`将仅匹配具有第一个词`customers`和第三个词`stockholm`的路由键。

当然，这对于某些服务只接收与特定主题子集相关的消息是非常有用的。下面的例子展示了如何在二月份创建一个新客户，队列`customer_created`会接收到消息，因为绑定规则是`customers.created.#`，而队列`customer_emailed`则不会接收到消息，因为它不匹配绑定`customers.created.march`。

![](img/bc6c90f304513314f60c0569f7badc07.png)

主题交换（Topic Exchange）—— 允许使用简单的正则表达式根据路由键选择子集 —— 图片由 Percy Bolmer 提供

最终的交换是**Header**交换，每条我们在 RabbitMQ 上发送的消息都有可能添加头信息，这是一个键值字段。当我们需要基于更高级别的内容进行路由时，这非常有用。

比如说我们添加一个`browser`头信息，指示用户在注册时使用了什么网页浏览器。例如，我们可以将所有 Linux 用户路由到某个特定的队列。

你可以指定多个头信息，并且它们都必须匹配，或者只需一个匹配。这通过将`x-match`设置为`all`或`any`来完成。

![](img/6a1551378f4499cf5018bc3627c73422.png)

Header Exchange — 允许基于消息中可以提供的额外头信息进行路由 — 图片由 Percy Bolmer 提供

让我们停止讨论，创建一个我们可以使用的交换机。

要添加一个交换机，我们将使用与之前使用的`rabbitmqcli`非常类似的`rabbitmqadmin` CLI 工具。

我们使用`declare exchange`命令，后跟交换机的名称和类型。对于这个教程，我将使用`Topic`交换机。

我们将创建一个名为`customer-events`的交换机。我们还需要指定虚拟主机以及管理员的用户名和密码。如果你希望交换机在重启时保持存在，记得将 durable 设置为 true。

```py
docker exec rabbitmq rabbitmqadmin declare exchange --vhost=customers name=customer_events type=topic -u percy -p secret durable=true
```

我们还需要授权用户在这个交换机上发送消息。我们使用`set_topic_permissions`命令设置特定主题上的权限。以下命令将用户`percy`设置为允许在交换机`customer_events`上的虚拟主机`customers`上发布，路由键以`customers`开头。

```py
docker exec rabbitmq rabbitmqctl set_topic_permissions -p customers percy customer_events "^customers.*" "^customers.*" 
```

现在在这个交换机上发布将不会有任何反应，因为队列和交换机之间没有绑定。

任何发送的消息都会被丢弃。

## 发布消息到交换机

要开始发布消息，我们首先需要在`customers_created`和`customers_test`队列与`customers_events`交换机之间创建绑定。

打开`rabbitmq.go`并添加一个添加绑定的`CreateBinding`函数。

```py
// CreateBinding is used to connect a queue to an Exchange using the binding rule
func (rc RabbitClient) CreateBinding(name, binding, exchange string) error {
 // leaveing nowait false, having nowait set to false wctxill cause the channel to return an error and close if it cannot bind
 // the final argument is the extra headers, but we wont be doing that now
 return rc.ch.QueueBind(name, binding, exchange, false, nil)
}
```

然后在`producer/main.go`中添加绑定，以便连接所有内容。我们预计客户会在主题`customers.created`上发布，后面跟着他们来自的国家。但是绑定不会关心国家，只要它匹配模式即可。

```py
 ...
 // Create binding between the customer_events exchange and the customers-created queue
 if err := client.CreateBinding("customers-created", "customers.created.*", "customer_events"); err != nil {
  panic(err)
 }
 // Create binding between the customer_events exchange and the customers-test queue
 if err := client.CreateBinding("customers-test", "customers.*", "customer_events"); err != nil {
  panic(err)
 }
```

如果你执行生产者，我们可以访问管理用户界面并查看可用的绑定。

```py
go run cmd/producer/main.go
```

然后进入用户界面并访问你的交换机。

![](img/c815c366cba69808a3756e8e0b11bcf1.png)

交换机显示当前的绑定和它们的路由键 — 图片由 Percy Bolmer 提供

现在我们已经有了绑定，可以开始查看发布消息的内容。我们从最简单的开始。

我们创建一个名为`Send`的包装函数，该函数接受关于要发布到哪个交换机和路由键的参数。该函数还会接受一个上下文和一个`amqp.Publishing` 结构体。

`amqp.Publishing` 结构体非常重要，因为它允许我们自定义我们发送的消息的功能和行为。

我们将一步步地探索它们，因为它们有很多。

```py
// Send is used to publish a payload onto an exchange with a given routingkey
func (rc RabbitClient) Send(ctx context.Context, exchange, routingKey string, options amqp.Publishing) error {
 return rc.ch.PublishWithContext(ctx,
  exchange,   // exchange
  routingKey, // routing key
  // Mandatory is used when we HAVE to have the message return an error, if there is no route or queue then
  // setting this to true will make the message bounce back
  // If this is False, and the message fails to deliver, it will be dropped
  true, // mandatory
  // immediate Removed in MQ 3 or up https://blog.rabbitmq.com/posts/2012/11/breaking-things-with-rabbitmq-3-0§
  false,   // immediate
  options, // amqp publishing struct
 )
}
```

返回到`producer/main.go`，我们将创建一条消息进行发送。我们将发送两条消息，每个队列一条。这是为了展示`deliveryMode`参数，这个参数非常重要。如果将其设置为持久性，消息将被保存直到某个消费者获取它，但这会带来开销和更长的延迟。

如果有些东西不需要持久化，则将其设置为**Transient**以提高性能。

记住，如果你发送的是持久消息，你的队列也需要是持久的；如果队列本身不存在，那么保存消息也没有意义。

```py
... 
// Create context to manage timeout
 ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
 defer cancel()
 // Create customer from sweden
 if err := client.Send(ctx, "customer_events", "customers.created.se", amqp091.Publishing{
  ContentType:  "text/plain",       // The payload we send is plaintext, could be JSON or others..
  DeliveryMode: amqp091.Persistent, // This tells rabbitMQ that this message should be Saved if no resources accepts it before a restart (durable)
  Body:         []byte("An cool message between services"),
 }); err != nil {
  panic(err)
 }
 if err := client.Send(ctx, "customer_events", "customers.test", amqp091.Publishing{
  ContentType:  "text/plain",
  DeliveryMode: amqp091.Transient, // This tells rabbitMQ that this message can be deleted if no resources accepts it before a restart (non durable)
  Body:         []byte("A second cool message"),
 }); err != nil {
  panic(err)
 }

 log.Println(client)
}
```

现在是执行生产者的时候了。

```py
go run cmd/producer/main.go
```

你现在应该在 UI 的`Queue`页面下看到每个队列的一条消息。

![](img/17f239cecab1d2c4960ec0eba230237d.png)

每个队列都有消息发送到它们 — 图片来源：Percy Bolmer

如果你愿意，可以进入每个队列并消费消息进行查看，但我建议重新启动 RabbitMQ，以显示 Transient 和 Persistent 之间的区别。

```py
docker restart rabbitmq
```

重新启动后尝试重新加载 UI，你应该会看到整个`customers-test`队列被删除了，但`customers-created`队列实际上保留了旧消息。

这是因为持久消息会被写入磁盘，以便在崩溃等情况发生时能够保存。

我们将很快介绍更高级的发布技术。

## 消费消息、确认、拒绝和重新入队

我们知道如何发布消息，但如果我们不能在另一个服务中消费这些消息，那是没有用的。

消费是从队列中获取消息的过程。

让我们创建一个新的二进制文件，以便我们可以用来消费消息。

```py
mkdir cmd/consumer
touch cmd/consumer/main.go
```

在我们开始消费之前，我们将在`Rabbitmq.go`中添加一个`Consume`函数，它将封装通道消费函数。

消费时有几个选项需要考虑。

+   **Exclusive** — 如果设置为 true，将确保这是该队列上的唯一消费者；如果设置为 false，服务器将公平地将消息分配给多个消费者。

+   **AutoAck** — 当设置为 true 时，将自动确认交付；当设置为 false 时，将期望消费者在完成时调用确认。AutoAck 可能听起来很棒，但它很棘手，如果你的消费者在确认耗时的过程后失败，消息会丢失，因为服务器认为它已经完成。

+   [**NoLocal**](https://www.rabbitmq.com/amqp-0-9-1-reference.html#domain.no-local) — 在 RabbitMQ 中不支持，这是 AMQP 字段，用于避免在同一领域中发布和消费。

+   [**NoWait**](https://www.rabbitmq.com/amqp-0-9-1-reference.html#domain.no-wait) — 不会等待服务器确认。

让我们将`Consume`函数添加到`Rabbitmq.go`中。

```py
// Consume is a wrapper around consume, it will return a Channel that can be used to digest messages
// Queue is the name of the queue to Consume
// Consumer is a unique identifier for the service instance that is consuming, can be used to cancel etc
// autoAck is important to understand, if set to true, it will automatically Acknowledge that processing is done
// This is good, but remember that if the Process fails before completion, then an ACK is already sent, making a message lost
// if not handled properly
func (rc RabbitClient) Consume(queue, consumer string, autoAck bool) (<-chan amqp.Delivery, error) {
 return rc.ch.Consume(queue, consumer, autoAck, false, false, false, nil)
}
```

现在我们可以进行消费了，让我们填写`consumer/main.go`，使其连接到 RabbitMQ 并开始从队列中获取消息。

```py
package main

import (
 "log"
 "programmingpercy/eventdrivenrabbit/internal"
)

func main() {

 conn, err := internal.ConnectRabbitMQ("percy", "secret", "localhost:5672", "customers")
 if err != nil {
  panic(err)
 }

 mqClient, err := internal.NewRabbitMQClient(conn)
 if err != nil {
  panic(err)
 }

 messageBus, err := mqClient.Consume("customers_created", "email-service", false)
 if err != nil {
  panic(err)
 }

 // blocking is used to block forever
 var blocking chan struct{}

 go func() {
  for message := range messageBus {
   // breakpoint here
   log.Printf("New Message: %v", message)
  }
 }()

 log.Println("Consuming, to close the program press CTRL+C")
 // This will block forever
 <-blocking

}
```

运行该消费者时，一旦发布者有消息发送，它应该会打印出一条消息。

> 记住，复用连接，但为每个并行处理创建一个新的通道，在我们的例子中，将创建一个第二个 RabbitMQ 客户端来管理`customers-test`队列。

```py
go run cmd/consumer/main.go
```

如果你没有看到任何消息，可能是因为你需要先运行生产者。

```py
2023/02/12 22:17:24 New Message: {0xc0000b0000 map[] text/plain  2 0     0001-01-01 00:00:00 +0000 UTC    ema
il-service 0 1 false customer_events customers.created.se [65 110 32 99 111 111 108 32 109 101 115 115 97 103
 101 32 98 101 116 119 101 101 110 32 115 101 114 118 105 99 101 115]}
```

可能值得探索通过通道传递的结构体，即 `amqp.Delivery` 结构体，它提供了所有字段的良好视图。

```py
// Delivery captures the fields for a previously delivered message resident in
// a queue to be delivered by the server to a consumer from Channel.Consume or
// Channel.Get.
type Delivery struct {
 Acknowledger Acknowledger // the channel from which this delivery arrived

 Headers Table // Application or header exchange table

 // Properties
 ContentType     string    // MIME content type
 ContentEncoding string    // MIME content encoding
 DeliveryMode    uint8     // queue implementation use - non-persistent (1) or persistent (2)
 Priority        uint8     // queue implementation use - 0 to 9
 CorrelationId   string    // application use - correlation identifier
 ReplyTo         string    // application use - address to reply to (ex: RPC)
 Expiration      string    // implementation use - message expiration spec
 MessageId       string    // application use - message identifier
 Timestamp       time.Time // application use - message timestamp
 Type            string    // application use - message type name
 UserId          string    // application use - creating user - should be authenticated user
 AppId           string    // application use - creating application id

 // Valid only with Channel.Consume
 ConsumerTag string

 // Valid only with Channel.Get
 MessageCount uint32

 DeliveryTag uint64
 Redelivered bool
 Exchange    string // basic.publish exchange
 RoutingKey  string // basic.publish routing key

 Body []byte
}
```

如果你重新运行当前的消费者，你会看到相同的消息再次出现。这是因为我们从未确认消费者已经使用了这条消息。这必须在迭代消息或使用自动确认标志时手动完成。

在确认时，我们可以传递一个 `multiple` 标志，指示是否一次确认多条消息，我们可以将其设为 false。

我们可以确认或 `NACK` 消息，确认表示一切正常，`NACK` 表示我们处理失败，然后消息会被重新放回队列中。

让我们更新处理消息的代码，以便确认它们。

```py
 go func() {
  for message := range messageBus {
   // breakpoint here
   log.Printf("New Message: %v", message)
   // Multiple means that we acknowledge a batch of messages, leave false for now
   if err := message.Ack(false); err != nil {
    log.Printf("Acknowledged message failed: Retry ? Handle manually %s\n", message.MessageId)
    continue
   }
   log.Printf("Acknowledged message %s\n", message.MessageId)
  }
 }()
```

现在重新运行代码，你应该会看到消息再次打印，但在重新启动后消息就消失了。

这非常有用，以避免消费者接收一条消息，在处理时失败，然后消息就会丢失。

为了展示自动确认可能是危险的，这里是一个修改后的例子，我们将自动确认设置为 true，但在处理过程中失败了。

```py
 // Auto Ack is now True
 messageBus, err := mqClient.Consume("customers-created", "email-service", true)
 if err != nil {
  panic(err)
 }

 // blocking is used to block forever
 var blocking chan struct{}

 go func() {
  for message := range messageBus {
   log.Printf("New Message: %v", message)
   panic("Whops I failed here for some reason")

  }
 }()
```

运行消费者两次，你会看到它实际上只在第一次执行时接受了消息。如果你没有妥善管理，这可能是危险的行为。这就是我不断提到它的原因！

要处理失败，你可以使用 `Nack` 告诉 RabbitMQ 失败了，你可以使用 `redelivered` 字段来避免过多重试。

`Nack` 接受一个重新排队的参数，这非常方便！

这是一个例子，我们在消息到达第一次时失败，重新排队，然后在下一次到达时确认它。

```py
 messageBus, err := mqClient.Consume("customers-created", "email-service", false)
 if err != nil {
  panic(err)
 }

 // blocking is used to block forever
 var blocking chan struct{}

 go func() {
  for message := range messageBus {
   log.Printf("New Message: %v", message)

   if !message.Redelivered {
    // Nack multiple, Set Requeue to true
    message.Nack(false, true)
    continue
   }

   // Multiple means that we acknowledge a batch of messages, leave false for now
   if err := message.Ack(false); err != nil {
    log.Printf("Acknowledged message failed: Retry ? Handle manually %s\n", message.MessageId)
    continue
   }
   log.Printf("Acknowledged message %s\n", message.MessageId)
  }
 }()
```

这里还有更多需要考虑的，目前我们使用的处理程序是单线程的，这意味着我们一次只能处理一条消息。我们可以通过实现一个工作组来修复这一点，该工作组允许一定数量的并发任务。

我将添加一个 `errgroup`，因此这种方法需要 Go 1.2。使用 ErrGroup 非常简单，我们可以将其限制为每个消费者处理 10 条消息。

`errgroup` 来自 `golang.org/x/sync/errgroup` 包。

```py
..... 
// Set a timeout for 15 secs
 ctx := context.Background()
 ctx, cancel := context.WithTimeout(ctx, 15*time.Second)
 defer cancel()
 // Create an Errgroup to manage concurrecy
 g, ctx := errgroup.WithContext(ctx)
 // Set amount of concurrent tasks
 g.SetLimit(10)
 go func() {
  for message := range messageBus {
   // Spawn a worker
   msg := message
   g.Go(func() error {
    log.Printf("New Message: %v", msg)

    time.Sleep(10 * time.Second)
    // Multiple means that we acknowledge a batch of messages, leave false for now
    if err := msg.Ack(false); err != nil {
     log.Printf("Acknowledged message failed: Retry ? Handle manually %s\n", msg.MessageId)
     return err
    }
    log.Printf("Acknowledged message %s\n", msg.MessageId)
    return nil
   })
  }
 }() 
```

添加这个使得消费者变得稍微更好一些。

> `SetLimit` 目前仅用于此，还有另一种管理消费消息数量的方法，即使用 RabbitMQ，我推荐使用的叫做 Prefetch，我们稍后会讲到。

我们可以通过将 `Send` 函数包裹在 `for` 循环中来更新发布者以发送更多的消息。

```py
 // Create context to manage timeout
 ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
 defer cancel()
 // Create customer from sweden
 for i := 0; i < 10; i++ {
  if err := client.Send(ctx, "customer_events", "customers.created.se", amqp091.Publishing{
   ContentType:  "text/plain",       // The payload we send is plaintext, could be JSON or others..
   DeliveryMode: amqp091.Persistent, // This tells rabbitMQ that this message should be Saved if no resources accepts it before a restart (durable)
   Body:         []byte("An cool message between services"),
  }); err != nil {
   panic(err)
  }
 }

 if err := client.Send(ctx, "customer_events", "customers.test", amqp091.Publishing{
  ContentType:  "text/plain",
  DeliveryMode: amqp091.Transient, // This tells rabbitMQ that this message can be deleted if no resources accepts it before a restart (non durable)
  Body:         []byte("A second cool message"),
 }); err != nil {
  panic(err)
 }

 log.Println(client)
}
```

尝试一下，看看消费者现在是否接受多条消息，或者尝试启动多个消费者来进行一些测试。

注意到生产者在发送消息后立即退出了吗？目前，`Send` 函数不会等待来自服务器的任何确认。有时，我们可能希望阻塞，直到服务器确认它已接收到消息。

高兴的是，我们可以！我们需要将 RabbitMQ 中使用的`Publish`函数更改为`PublishWithDeferredConfirmWithContext`，这将返回一个可以用来`Wait`等待服务器确认的对象。

这个对象将始终是 NIL，除非将通道设置为`Confirm`模式，将其设置为 Confirm 模式将使服务器在收到发布的消息时发送确认。

在`Rabbitmq.go`中，让我们修改发布方法并添加一个等待。

```py
// Send is used to publish a payload onto an exchange with a given routingkey
func (rc RabbitClient) Send(ctx context.Context, exchange, routingKey string, options amqp.Publishing) error {
 // PublishWithDeferredConfirmWithContext will wait for server to ACK the message
 confirmation, err := rc.ch.PublishWithDeferredConfirmWithContext(ctx,
  exchange,   // exchange
  routingKey, // routing key
  // Mandatory is used when we HAVE to have the message return an error, if there is no route or queue then
  // setting this to true will make the message bounce back
  // If this is False, and the message fails to deliver, it will be dropped
  true, // mandatory
  // immediate Removed in MQ 3 or up https://blog.rabbitmq.com/posts/2012/11/breaking-things-with-rabbitmq-3-0§
  false,   // immediate
  options, // amqp publishing struct
 )
 if err != nil {
  return err
 }
 // Blocks until ACK from Server is receieved
 log.Println(confirmation.Wait())
 return nil
}
```

让我们也更新`NewRabbitMQClient`以始终将通道设置为`Confirm`模式。

```py
// NewRabbitMQClient will connect and return a Rabbitclient with an open connection
// Accepts a amqp Connection to be reused, to avoid spawning one TCP connection per concurrent client
func NewRabbitMQClient(conn *amqp.Connection) (RabbitClient, error) {
 // Unique, Conncurrent Server Channel to process/send messages
 // A good rule of thumb is to always REUSE Conn across applications
 // But spawn a new Channel per routine
 ch, err := conn.Channel()
 if err != nil {
  return RabbitClient{}, err
 }
 // Puts the Channel in confirm mode, which will allow waiting for ACK or NACK from the receiver
 if err := ch.Confirm(false); err != nil {
  return RabbitClient{}, err
 }

 return RabbitClient{
  conn: conn,
  ch:   ch,
 }, nil
}
```

对`Rabbitmq.go`的一个更好方法可能是添加一个`NewChannel`函数，然后让每个函数接受一个 Channel 作为输入参数。

现在运行程序，你应该会看到`publisher.go`在每次服务器确认消息时打印 TRUE，注意这与 Consumer 的`ACK`不同。我们只是等待服务器确认发布的消息已被接受。

## 发布和订阅（PubSub）

![](img/d1a769e3d40c94df6effe0acdb6a5dae.png)

RabbitMQ 中的 Pub/Sub 模式使用 Fanout 交换机 — 图片由 Percy Bolmer 提供

直到目前为止，我们一直在使用 FIFO 队列（先进先出）。这意味着每条消息只发送给一个消费者。

在发布和订阅模式中，你会希望每个消费者接收到相同的消息。

我们关于绑定等的所有知识仍然适用，使用方式相同。我们可以使用 Fanout 交换机（将消息推送到所有绑定的队列）而不管队列名称。

这个想法是让每个消费者创建一个未命名的队列，未命名的队列将由 RabbitMQ 服务器生成一个随机的唯一名称。

> 这是在代码中创建队列非常适合的一个好例子。

我们可能希望将`customers_event`发送到多个服务。例如，我们可能希望有一个电子邮件服务和一个日志记录服务来记录每个客户事件。

让我们来构建它。（由于这是一个学习 RabbitMQ 的教程，我们将简单地启动两个 Consumer 实例。）

我们首先删除现有的交换机，因为它的**类型**不正确。我们还创建一个新的交换机，但类型为**Fanout**。这一次我们没有为权限指定特定的前缀，而是给予了完全访问权限。

```py
docker exec rabbitmq rabbitmqadmin delete exchange name=customer_events --vhost=customers -u percy -p secret
docker exec rabbitmq rabbitmqadmin declare exchange --vhost=customers name=customer_events type=fanout -u percy -p secret durable=true
docker exec rabbitmq rabbitmqctl set_topic_permissions -p customers percy customer_events ".*" ".*"
```

由于我们在当前代码中创建一个未命名的队列时无法知道队列名称，因此需要进行修改。让我们返回来自`CreateQueue`的 RabbitMQ 包中的队列信息。该对象将包含随机创建的名称。

```py
// CreateQueue will create a new queue based on given cfgs
func (rc RabbitClient) CreateQueue(queueName string, durable, autodelete bool) (amqp.Queue, error) {
 q, err := rc.ch.QueueDeclare(queueName, durable, autodelete, false, false, nil)
 if err != nil {
  return amqp.Queue{}, nil
 }

 return q, nil
}
```

现在是更新`Publisher`的时候了，在教程早些时候我们在 Publisher 中创建了 Channel 绑定。依我看这样做并不完全合理，这只是为了不走得太快，同时展示功能。

`Consumer` 声明绑定更有意义，因为它与消费者相关。在发布和订阅中，消费者的数量和路径可能未知，现在这更没有意义。让我们更新 `publisher.go` 使其变得更小。

```py
package main

import (
 "context"
 "log"
 "programmingpercy/eventdrivenrabbit/internal"
 "time"

 "github.com/rabbitmq/amqp091-go"
)

func main() {
 conn, err := internal.ConnectRabbitMQ("percy", "secret", "localhost:5672", "customers")

 if err != nil {
  panic(err)
 }
 defer conn.Close()
 client, err := internal.NewRabbitMQClient(conn)
 if err != nil {
  panic(err)
 }
 defer client.Close()

 // Create context to manage timeout
 ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
 defer cancel()
 // Create customer from sweden
 for i := 0; i < 10; i++ {
  if err := client.Send(ctx, "customer_events", "customers.created.se", amqp091.Publishing{
   ContentType:  "text/plain",       // The payload we send is plaintext, could be JSON or others..
   DeliveryMode: amqp091.Persistent, // This tells rabbitMQ that this message should be Saved if no resources accepts it before a restart (durable)
   Body:         []byte("An cool message between services"),
  }); err != nil {
   panic(err)
  }
 }

 log.Println(client)
}
```

我们将更新 `consumer.go` 以创建一个未命名的队列，创建绑定，然后开始消费该队列。

```py
package main

import (
 "context"
 "log"
 "programmingpercy/eventdrivenrabbit/internal"
 "time"

 "golang.org/x/sync/errgroup"
)

func main() {

 conn, err := internal.ConnectRabbitMQ("percy", "secret", "localhost:5672", "customers")
 if err != nil {
  panic(err)
 }

 mqClient, err := internal.NewRabbitMQClient(conn)
 if err != nil {
  panic(err)
 }

 // Create Unnamed Queue which will generate a random name, set AutoDelete to True
 queue, err := mqClient.CreateQueue("", true, true)
 if err != nil {
  panic(err)
 }
 // Create binding between the customer_events exchange and the new Random Queue
 // Can skip Binding key since fanout will skip that rule
 if err := mqClient.CreateBinding(queue.Name, "", "customer_events"); err != nil {
  panic(err)
 }

 messageBus, err := mqClient.Consume(queue.Name, "email-service", false)
 if err != nil {
  panic(err)
 }
 // blocking is used to block forever
 var blocking chan struct{}
 // Set a timeout for 15 secs
 ctx := context.Background()
 ctx, cancel := context.WithTimeout(ctx, 15*time.Second)
 defer cancel()
 // Create an Errgroup to manage concurrecy
 g, ctx := errgroup.WithContext(ctx)
 // Set amount of concurrent tasks
 g.SetLimit(10)
 go func() {
  for message := range messageBus {
   // Spawn a worker
   msg := message
   g.Go(func() error {
    log.Printf("New Message: %v", msg)

    time.Sleep(10 * time.Second)
    // Multiple means that we acknowledge a batch of messages, leave false for now
    if err := msg.Ack(false); err != nil {
     log.Printf("Acknowledged message failed: Retry ? Handle manually %s\n", msg.MessageId)
     return err
    }
    log.Printf("Acknowledged message %s\n", msg.MessageId)
    return nil
   })
  }
 }()

 log.Println("Consuming, to close the program press CTRL+C")
 // This will block forever
 <-blocking

}
```

这个设置可以用来正确展示 Pub/Sub，我们可以先启动两个消费者，然后是发布者。它将展示所有消费者如何看到所有消息。

![](img/79915ffe5e95a64145f33fb403ce4baa.png)

所有消费者都会接收到消息。

我们现在知道如何使用常规队列和 PubSub。

还有一件事，第三种非常常见的场景是基于 RPC 的范式。

## 使用 RabbitMQ 的远程过程调用（RPC）

![](img/752a50ce893c27d8a0ae136bd8e16c21.png)

RabbitMQ 中的 RPC 使用消息中的 ReplyTo 头部。— 图片由 Percy Bolmer 提供

有时，我们希望在消息上进行一些回调。比如说，生产者希望知道客户何时发送了电子邮件。

这是常见且容易解决的问题。我们可以在消息中设置一个名为 `ReplyTo` 的字段，这可以用于告诉消费者在特定队列上回复响应。

我们可能需要知道回调与哪个消息相关，因此我们还可以添加 `correlationID`，以便了解响应与哪个请求相关。

开始创建一个 **Direct** 类型的新交换机。我会将其命名为 `customer_callbacks`。Direct 类型在这里效果很好。

```py
docker exec rabbitmq rabbitmqadmin declare exchange --vhost=customers name=customer_callbacks type=direct -u percy -p secret durable=true
docker exec rabbitmq rabbitmqctl set_topic_permissions -p customers percy customer_callbacks ".*" ".*"
```

我们需要了解的第一件事是目前的一个重要最佳实践。

拥有回调将要求相同的服务既进行发布又进行消费，这没什么问题。

一个著名的规则是，**但绝不要在同一连接上进行发布和消费**。

![](img/4779cfaaa76b6550a1b599287a47c55c.png)

压力回溯可能会阻止 ACK 消息的发送 — 图片由 Percy Bolmer 提供

想象一下，如果你有一个同时进行生产和消费的服务，并且在同一个连接上进行，那么假设服务正在消费大量消息。如果消息数量超过了服务能够处理的范围，消息开始堆积。RabbitMQ 可能会施加回压，开始阻塞 TCP 连接的发送，结果，ACK 消息必须被发送来处理消息。突然间，由于连接被阻塞，你的代码无法发送 ACK 消息。这可能会导致延迟。

黄金规则是

+   在应用程序中重用连接

+   一个用于消费，一个用于发布

+   为每个 Goroutine 创建新的通道

让我们更新 `producer.go` 来启动两个连接，一个用于发布，一个用于消费。我们还将创建一个未命名的队列并将其绑定到交换机，然后我们将消费这些响应。

我们还将在消息中添加`replyTo`，这告诉消费者回复的地址，以及`correlationId`，它解释了消息关联的唯一事件。

```py
package main

import (
 "context"
 "fmt"
 "log"
 "programmingpercy/eventdrivenrabbit/internal"
 "time"

 "github.com/rabbitmq/amqp091-go"
)

func main() {
 conn, err := internal.ConnectRabbitMQ("percy", "secret", "localhost:5672", "customers")
 if err != nil {
  panic(err)
 }
 defer conn.Close()
 // Never use the same Connection for Consume and Publish
 consumeConn, err := internal.ConnectRabbitMQ("percy", "secret", "localhost:5672", "customers")
 if err != nil {
  panic(err)
 }
 defer consumeConn.Close()

 client, err := internal.NewRabbitMQClient(conn)
 if err != nil {
  panic(err)
 }
 defer client.Close()

 consumeClient, err := internal.NewRabbitMQClient(consumeConn)
 if err != nil {
  panic(err)
 }
 defer consumeClient.Close()

 // Create Unnamed Queue which will generate a random name, set AutoDelete to True
 queue, err := consumeClient.CreateQueue("", true, true)
 if err != nil {
  panic(err)
 }

 if err := consumeClient.CreateBinding(queue.Name, queue.Name, "customer_callbacks"); err != nil {
  panic(err)
 }

 messageBus, err := consumeClient.Consume(queue.Name, "customer-api", true)
 if err != nil {
  panic(err)
 }
 go func() {
  for message := range messageBus {
   log.Printf("Message Callback %s\n", message.CorrelationId)
  }
 }()
 // Create context to manage timeout
 ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
 defer cancel()
 // Create customer from sweden
 for i := 0; i < 10; i++ {
  if err := client.Send(ctx, "customer_events", "customers.created.se", amqp091.Publishing{
   ContentType:  "text/plain",       // The payload we send is plaintext, could be JSON or others..
   DeliveryMode: amqp091.Persistent, // This tells rabbitMQ that this message should be Saved if no resources accepts it before a restart (durable)
   Body:         []byte("An cool message between services"),
   // We add a REPLYTO which defines the
   ReplyTo: queue.Name,
   // CorrelationId can be used to know which Event this relates to
   CorrelationId: fmt.Sprintf("customer_created_%d", i),
  }); err != nil {
   panic(err)
  }
 }
 var blocking chan struct{}

 log.Println("Waiting on Callbacks, to close the program press CTRL+C")
 // This will block forever
 <-blocking
}
```

消费者需要更新，以便它也使用两个连接。当我们完成处理消息时，我们将其添加到`replyTo`队列，以便发送响应。再次，我们必须使用两个不同的连接，一个用于消费，另一个用于发布。

```py
package main

import (
 "context"
 "log"
 "programmingpercy/eventdrivenrabbit/internal"
 "time"

 "github.com/rabbitmq/amqp091-go"
 "golang.org/x/sync/errgroup"
)

func main() {

 conn, err := internal.ConnectRabbitMQ("percy", "secret", "localhost:5672", "customers")
 if err != nil {
  panic(err)
 }
 defer conn.Close()

 publishConn, err := internal.ConnectRabbitMQ("percy", "secret", "localhost:5672", "customers")
 if err != nil {
  panic(err)
 }
 defer publishConn.Close()

 mqClient, err := internal.NewRabbitMQClient(conn)
 if err != nil {
  panic(err)
 }

 publishClient, err := internal.NewRabbitMQClient(publishConn)
 if err != nil {
  panic(err)
 }
 // Create Unnamed Queue which will generate a random name, set AutoDelete to True
 queue, err := mqClient.CreateQueue("", true, true)
 if err != nil {
  panic(err)
 }
 // Create binding between the customer_events exchange and the new Random Queue
 // Can skip Binding key since fanout will skip that rule
 if err := mqClient.CreateBinding(queue.Name, "", "customer_events"); err != nil {
  panic(err)
 }

 messageBus, err := mqClient.Consume(queue.Name, "email-service", false)
 if err != nil {
  panic(err)
 }
 // blocking is used to block forever
 var blocking chan struct{}
 // Set a timeout for 15 secs
 ctx := context.Background()
 ctx, cancel := context.WithTimeout(ctx, 15*time.Second)
 defer cancel()
 // Create an Errgroup to manage concurrecy
 g, ctx := errgroup.WithContext(ctx)
 // Set amount of concurrent tasks
 g.SetLimit(10)
 go func() {
  for message := range messageBus {
   // Spawn a worker
   msg := message
   g.Go(func() error {
    // Multiple means that we acknowledge a batch of messages, leave false for now
    if err := msg.Ack(false); err != nil {
     log.Printf("Acknowledged message failed: Retry ? Handle manually %s\n", msg.MessageId)
     return err
    }

    log.Printf("Acknowledged message, replying to %s\n", msg.ReplyTo)

    // Use the msg.ReplyTo to send the message to the proper Queue
    if err := publishClient.Send(ctx, "customer_callbacks", msg.ReplyTo, amqp091.Publishing{
     ContentType:   "text/plain",      // The payload we send is plaintext, could be JSON or others..
     DeliveryMode:  amqp091.Transient, // This tells rabbitMQ to drop messages if restarted
     Body:          []byte("RPC Complete"),
     CorrelationId: msg.CorrelationId,
    }); err != nil {
     panic(err)
    }
    return nil
   })
  }
 }()

 log.Println("Consuming, to close the program press CTRL+C")
 // This will block forever
 <-blocking

}
```

尝试一下代码，你应该看到生产者接收到 RPC 响应并将其打印出来。

注意这段代码可以进行清理，但本教程重点在于 RabbitMQ 的工作原理，而不是清洁代码。

## 预取限制——限制发送的消息数量。

记得我们之前通过使用`errgroup`限制了消费者的工作量吗？这只是一个软限制，由代码施加，但 RabbitMQ 仍然可以向消费者发送更多消息。

还有更好的解决方案，实际上，如果你希望消费者并发处理消息，应该使用组合方案。

AMQP 协议允许我们应用预取限制。这告诉 RabbitMQ 服务器每次可以发送到频道的未确认消息数量。这样我们可以添加一个硬限制。

通过应用一组服务质量规则（QOS）来完成这一点。让我们在`rabbitmq.go`中添加一个方法，应用这三条可用的规则。

以下是参数：

+   预取计数——服务器可以发送多少未确认的消息。

+   预取大小——服务器可以发送多少字节的未确认消息。

+   全局——一个标志，用于确定规则是否应用于连接或全局。

```py
// ApplyQos is used to apply qouality of service to the channel
// Prefetch count - How many messages the server will try to keep on the Channel
// prefetch Size - How many Bytes the server will try to keep on the channel
// global -- Any other Consumers on the connection in the future will apply the same rules if TRUE
func (rc RabbitClient) ApplyQos(count, size int, global bool) error {
 // Apply Quality of Serivce
 return rc.ch.Qos(
  count,
  size,
  global,
 )
}
```

然后在`consumer.go`中，我们可以简单地调用它并应用我们想要允许的消息数量。

```py
 // Create an Errgroup to manage concurrecy
 g, ctx := errgroup.WithContext(ctx)
 // Set amount of concurrent tasks
 g.SetLimit(10)

 // Apply Qos to limit amount of messages to consume
 if err := mqClient.ApplyQos(10, 0, true); err != nil {
  panic(err)
 }
 go func() {
  for message := range messageBus {
```

## 使用 TLS 保护连接。

现在是 2023 年，在投入生产之前，我认为我们应该加密流量是非常安全的。

RabbitMQ 有一个 GitHub [repository](https://github.com/rabbitmq/tls-gen)来帮助我们创建 rootCA 和所需的证书，这是加密流量的第一步。

我们需要克隆此存储库并执行内部的 make 文件，以生成所需的文件。

```py
git clone https://github.com/rabbitmq/tls-gen tls-gen
cd tls-gen/basic
make PASSWORD=
make verify
```

所有生成的文件将出现在一个名为`result`的新文件夹中。为使其在 Docker 中正常工作，我们需要更改它们的权限。

```py
 sudo chmod 644 tls-gen/basic/result/*
```

我们需要删除正在运行的 RabbitMQ 容器，我们需要用配置文件创建一个新的容器。

```py
sudo docker container rm -f rabbitmq 
```

配置文件名为`rabbitmq.conf`，应放置在容器中的`/etc/rabbitmq/rabbitmq.conf`内。

这个配置文件不仅可以配置 TLS，但我们现在只讨论 TLS。在项目根目录下创建一个具有正确名称的新文件。

```py
cd ../../ # Go to root of Project
touch rabbitmq.conf
```

我们需要在启动容器时将配置文件挂载到 Docker 中。我们还将把 TLS-Gen 工具生成的证书挂载到`/certs`，以便容器可以找到它们。请注意，这两个端口都减少了一，以符合 RabbitMQ 协议的标准。

```py
docker run -d --name rabbitmq -v "$(pwd)"/rabbitmq.conf:/etc/rabbitmq/rabbitmq.conf:ro -v "$(pwd)"/tls-gen/basic/result:/certs -p 5671:5671 -p 15671:15671 rabbitmq:3.11-management
```

一旦完成，我们可以开始将 TLS 配置添加到这个容器中。

在`rabbitmq.conf`中添加证书和根 CA 的路径。我的计算机名为`blackbox`，你需要将证书名称替换为你计算机生成的名称。

```py
# Disable NON TCP
listeners.tcp = none
# TCP port
listeners.ssl.default = 5671
# SSL Certs
ssl_options.cacertfile = /certs/ca_certificate.pem
ssl_options.certfile   = /certs/server_blackbox_certificate.pem
ssl_options.keyfile    = /certs/server_blackbox_key.pem
# Peer verification
ssl_options.verify     = verify_peer
ssl_options.fail_if_no_peer_cert = true
```

然后重新启动 RabbitMQ。

```py
docker restart rabbitmq
```

为了验证一切是否正常工作，你可以使用`docker logs rabbitmq`查看 Docker 日志。搜索有关监听器的日志。

```py
2023-02-19 07:35:15.566316+00:00 [info] <0.738.0> Ready to start client connection listeners
2023-02-19 07:35:15.567418+00:00 [info] <0.885.0> started TLS (SSL) listener on [::]:5671
```

现在，旧程序将无法再工作。它尝试在没有 TLS 的情况下进行连接，所以我们来修复一下。

程序需要更新以使用客户端证书。我们将其作为输入添加到`ConnectRabbitMQ`函数中。

```py
// ConnectRabbitMQ will spawn a Connection
func ConnectRabbitMQ(username, password, host, vhost, caCert, clientCert, clientKey string) (*amqp.Connection, error) {
 ca, err := os.ReadFile(caCert)
 if err != nil {
  return nil, err
 }
 // Load the key pair
 cert, err := tls.LoadX509KeyPair(clientCert, clientKey)
 if err != nil {
  return nil, err
 }
 // Add the CA to the cert pool
 rootCAs := x509.NewCertPool()
 rootCAs.AppendCertsFromPEM(ca)

 tlsConf := &tls.Config{
  RootCAs:      rootCAs,
  Certificates: []tls.Certificate{cert},
 }
 // Setup the Connection to RabbitMQ host using AMQPs and Apply TLS config
 conn, err := amqp.DialTLS(fmt.Sprintf("amqps://%s:%s@%s/%s", username, password, host, vhost), tlsConf)
 if err != nil {
  return nil, err
 }
 return conn, nil
}
```

请注意，我们现在使用的是`amqps`协议。证书路径是绝对路径，我们需要更新`consumer`和`producer`以插入这些路径，我现在会使用硬编码的值，但在实际应用中你不应该这样做。

```py
 conn, err := internal.ConnectRabbitMQ("percy", "secret", "localhost:5671", "customers",
  "/home/pp/development/blog/event-driven-rabbitmq/tls-gen/basic/result/ca_certificate.pem",
  "/home/pp/development/blog/event-driven-rabbitmq/tls-gen/basic/result/client_blackbox_certificate.pem",
  "/home/pp/development/blog/event-driven-rabbitmq/tls-gen/basic/result/client_blackbox_key.pem",
 )
 if err != nil {
  panic(err)
 }
 defer conn.Close()
 // Never use the same Connection for Consume and Publish
 consumeConn, err := internal.ConnectRabbitMQ("percy", "secret", "localhost:5671", "customers",
  "/home/pp/development/blog/event-driven-rabbitmq/tls-gen/basic/result/ca_certificate.pem",
  "/home/pp/development/blog/event-driven-rabbitmq/tls-gen/basic/result/client_blackbox_certificate.pem",
  "/home/pp/development/blog/event-driven-rabbitmq/tls-gen/basic/result/client_blackbox_key.pem",
 )
 defer consumeConn.Close()
```

BAM！太棒了，我们有了 TLS。

尝试运行生产者或消费者，然后使用`docker logs rabbitmq`查看 Docker 日志。

```py
2023-02-19 07:49:53.015732+00:00 [error] <0.948.0> Error on AMQP connection <0.948.0> (172.17.0.1:49066 -> 172.17.0.2:5671, state: starting):
2023-02-19 07:49:53.015732+00:00 [error] <0.948.0> PLAIN login refused: user 'percy' - invalid credentials
```

对，删除 Docker 时我们删除了虚拟主机、用户、交换机以及所有内容，因为我们没有持久化存储。

这很好，因为这将引导我们进入本教程的下一步也是最后一步，默认配置。

## RabbitMQ 配置与管理

相信我，你不想使用 AdminCLI 来管理多个用户的 RabbitMQ，因为如果你因为某些原因重置集群，这将是重复的繁重工作。

支持插入定义文件、定义用户、虚拟主机、权限、队列和交换机的 JSON 文件，甚至是绑定。

它们真的很容易使用，让我们添加我的旧用户，并赋予其在`customers`虚拟主机上读写权限，添加一个基本交换机。

在此之前，我们需要一个密码[哈希](https://www.rabbitmq.com/passwords.html)，这可能比想象的要复杂。它取决于你拥有的 RabbitMQ 设置以及你配置的算法。默认的是 SHA256。

我在[stackoverflow](https://stackoverflow.com/questions/41306350/how-to-generate-password-hash-for-rabbitmq-management-http-api)上找到了一个很棒的 bash 脚本来为我生成它。创建一个名为`encodepassword.sh`的文件，并将`secret`替换为你要编码的密码。

```py
#!/bin/bash

function encode_password()
{
    SALT=$(od -A n -t x -N 4 /dev/urandom)
    PASS=$SALT$(echo -n $1 | xxd -ps | tr -d '\n' | tr -d ' ')
    PASS=$(echo -n $PASS | xxd -r -p | sha256sum | head -c 128)
    PASS=$(echo -n $SALT$PASS | xxd -r -p | base64 | tr -d '\n')
    echo $PASS
}

encode_password "secret"
```

运行脚本`bash encodepassword.sh`并存储 Hash。

更新`rabbitmq.conf`以包含`load_definitions`字段，这个字段可以在启动时加载定义文件。

```py
log.console = true
# Disable NON TCP
listeners.tcp = none
# TCP port
listeners.ssl.default = 5671
# SSL Certs
ssl_options.cacertfile = /certs/ca_certificate.pem
ssl_options.certfile   = /certs/server_blackbox_certificate.pem
ssl_options.keyfile    = /certs/server_blackbox_key.pem
# Peer verification
ssl_options.verify     = verify_peer
ssl_options.fail_if_no_peer_cert = true
# Load definitions file
load_definitions = /etc/rabbitmq/rabbitmq_definitions.json
```

我会指向一个名为`/etc/rabbitmq/rabbitmq_definitions.json`的文件。

在项目根目录下创建一个名为 `rabbitmq_definitions.json` 的文件，并用以下 JSON 填充它。目前，我认为我们不需要详细讲解 JSON 字段，一切应该是可以理解的。它与我们之前运行的 CLI 命令非常相似。

以下定义文件创建了两个交换机：`customer_events` 和 `customer_callbacks`。当前代码会生成自己的队列，因此我们仅在示例中定义一个以便于理解。

```py
{
    "users": [
        {
            "name": "percy",
            "password_hash": "dPOoDgfw31kjUy41HSmqQR+X2Q9PCA5fD++fbxQCgPvKZmnX",
            "tags": "administrator"
        }
    ],
    "vhosts": [
        {
            "name": "/"
        },{
            "name": "customers"
        }
    ],
    "permissions": [
        {
            "user": "percy",
            "vhost": "customers",
            "configure": ".*",
            "write": ".*",
            "read": ".*"
        }
    ],
    "exchanges": [
        {
            "name": "customer_events",
            "vhost": "customers",
            "type": "fanout",
            "durable": true,
            "auto_delete": false,
            "internal": false,
            "arguments": {}
        },
        {
            "name": "customer_callbacks",
            "vhost": "customers",
            "type": "direct",
            "durable": true,
            "auto_delete": false,
            "internal": false,
            "arguments": {}
        }
    ],
    "queues": [
        {
            "name": "customers_created",
            "vhost": "customers",
            "durable": true,
            "auto_delete": false,
            "arguments": {}
        }
    ],
    "bindings": [
        {
            "source": "customers_events",
            "vhost": "customers",
            "destination": "customers_created",
            "destination_type": "queue",
            "routing_key": "customers.created.*",
            "arguments": {}
        }
    ]
}
```

一旦两个文件都到位，删除旧的 Docker，并重启一个新的，但这次我们为定义添加了第三个挂载点。

```py
docker run -d --name rabbitmq -v "$(pwd)"/rabbitmq_definitions.json:/etc/rabbitmq/rabbitmq_definitions.json:ro -v "$(pwd)"/rabbitmq.conf:/etc/rabbitmq/rabbitmq.conf:ro -v "$(pwd)"/tls-gen/basic/result:/certs -p 5671:5671 -p 15672:15672 rabbitmq:3.11-management
```

运行后，检查日志，确认它们打印了创建用户的相关信息。

```py
2023-02-19 08:17:53.467218+00:00 [info] <0.867.0> Started message store of type persistent for vhost 'customers'
2023-02-19 08:17:53.467310+00:00 [info] <0.867.0> Recovering 0 queues of type rabbit_classic_queue took 3ms
2023-02-19 08:17:53.467348+00:00 [info] <0.867.0> Recovering 0 queues of type rabbit_quorum_queue took 0ms
2023-02-19 08:17:53.467371+00:00 [info] <0.867.0> Recovering 0 queues of type rabbit_stream_queue took 0ms
2023-02-19 08:17:53.468487+00:00 [info] <0.698.0> Importing concurrently 1 permissions...
2023-02-19 08:17:53.469946+00:00 [info] <0.680.0> Successfully set permissions for 'percy' in virtual host 'customers' to '.*', '.*', '.*'
```

完成这些后，尝试运行消费者和生产者，你应该会看到一切按预期工作。唯一的不同是，我们现在使用配置在 RabbitMQ 中创建基础设施，而不是使用 CLI，并且流量是加密的。

## 结论

很遗憾，这个漫长但令人兴奋的 RabbitMQ 冒险到此结束。

让我们回顾一下我们学到的内容。

我们已经学习了如何用虚拟主机配置 RabbitMQ，以及如何在这些虚拟主机上创建具有权限的用户。

我们还学习了如何在队列和交换机上生产和消费消息。

你应该对所有资源，如队列、交换机和绑定有一定了解。

我们还涵盖了如何创建发布和订阅模式、RPC 模式以及常规工作队列。

希望你已经清楚如何使用连接和通道以及它们之间的区别。连接是一个 TCP 连接，而通道是在连接上的复用虚拟通道。在同一个软件中重用连接，但为每个并行进程创建新的通道。

我们了解到永远不要在同一个连接上进行生产和消费。

我们还涵盖了如何设置 TLS 以及如何为 RabbitMQ 添加预定义配置的定义。

我真的希望你喜欢这个教程，你可以在 GitHub 上找到所有使用的代码。

随时向我提问！
