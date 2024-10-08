# 5 种简单有效的 Python 日志使用方法

> 原文：[`towardsdatascience.com/5-easy-and-effective-ways-to-use-python-logging-a9564bd17ccd`](https://towardsdatascience.com/5-easy-and-effective-ways-to-use-python-logging-a9564bd17ccd)

## 像专业人士一样使用 Python 日志

[](https://dmitryelj.medium.com/?source=post_page-----a9564bd17ccd--------------------------------)![Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----a9564bd17ccd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a9564bd17ccd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a9564bd17ccd--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----a9564bd17ccd--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a9564bd17ccd--------------------------------) ·5 分钟阅读·2023 年 6 月 16 日

--

![](img/15b156437e425ed1bab6abb488109fac.png)

图像由作者生成

我敢打赌，几乎每个 Python 开发人员有时都会使用“print”进行调试。对于原型设计来说没有什么问题，但对于生产环境来说，有更有效的方法来处理日志。在这篇文章中，我将展示 Python “logging” 比“print”更灵活和强大的五个实际原因，以及为什么如果你之前没有开始使用它，你绝对应该使用它。

让我们开始吧。

## 代码

为了使事情更实际，我们考虑一个玩具示例。我创建了一个小应用程序，用于计算两个 Python 列表的线性回归：

```py
import numpy as np
from sklearn.linear_model import LinearRegression
from typing import List, Optional

def do_regression(arr_x: List, arr_y: List) -> Optional[List]:
    """ LinearRegression for X and Y lists """
    try:
        x_in = np.array(arr_x).reshape(-1, 1)
        y_in = np.array(arr_y).reshape(-1, 1)
        print(f"X: {x_in}")
        print(f"y: {y_in}")

        reg = LinearRegression().fit(x_in, y_in)
        out = reg.predict(x_in)
        print(f"Out: {out}")
        print(f"Score: {reg.score(x_in, arr_y)}")
        print(f"Coef: {reg.coef_}")
        return out.reshape(-1).tolist()
    except ValueError as err:
        print(f"ValueError: {err}")
    return None

if __name__ == "__main__":
    print("App started")
    ret = do_regression([1,2,3,4], [5,6,7,8])
    print(f"LinearRegression result: {ret}")
```

这段代码可以运行，但我们能做得更好吗？显然可以。让我们看看在这段代码中使用“*logging*”而不是“*print*”的五个优势。

## 1\. 日志级别

让我们稍微修改一下我们的代码：

```py
import logging

def do_regression(arr_x: List, arr_y: List) -> Optional[List]:
    """LinearRegression for X and Y Python lists"""
    try:
        x_in = np.array(arr_x).reshape(-1, 1)
        y_in = np.array(arr_y).reshape(-1, 1)
        logging.debug(f"X: {x_in}")
        ...

    except ValueError as err:
        logging.error(f"ValueError: {err}")
    return None

if __name__ == "__main__":     
    logging.basicConfig(level=logging.DEBUG, format='%(message)s')

    logging.info("App started")
    ret = do_regression([1,2,3,4], [5,6,7,8])
    logging.info(f"LinearRegression result: {ret}")
```

在这里，我将“print”调用替换为“logging”调用。我们做了一个小的更改，但它使输出变得更加灵活。通过使用“level”参数，我们现在可以设置不同的 **日志级别**。例如，如果我们使用“*level=logging.DEBUG*”，则所有输出都将可见。当我们确定我们的代码已准备好投入生产时，我们可以将级别更改为“*logging.INFO*”，调试消息将不再显示：

![](img/34e99f94dbf967ee7ccad706bc985a91.png)

“INFO” 调试级别在左侧，“DEBUG” 在右侧，作者提供的图像

重要的是，除了日志的初始化之外，不需要任何代码更改！

顺便提一下，所有可用的常量可以在 *logging/__init__.py* 文件中找到：

```py
ERROR = 40
WARNING = 30
INFO = 20
DEBUG = 10
NOTSET = 0
```

正如我们所看到的，“ERROR”级别是最高的；通过启用“ERROR”日志级别，我们可以抑制所有其他消息，只显示错误。

## 2\. 格式化

从最后的截图可以看出，控制日志输出是很容易的。但我们可以做得更多来改进它。我们还可以通过提供“*format*”字符串来调整输出。例如，我可以指定如下格式：

```py
logging.basicConfig(level=logging.DEBUG,
                    format='[%(asctime)s] %(filename)s:%(lineno)d: %(message)s')
```

在没有其他代码更改的情况下，我将能够在输出中看到时间戳、文件名，甚至行号：

![](img/b3f1d4f3e7e2e74df608dad6ca20df85.png)

日志输出，作者提供的图片

大约有 20 个不同的参数，可以在手册的“[LogRecord attributes](https://docs.python.org/3/library/logging.html)”一节中找到。

## 3\. 将日志保存到文件中

Python 的 logging 是一个非常灵活的模块，其功能可以很容易地扩展。假设我们想将所有日志保存到一个文件中以备将来分析。为此，我们只需要添加两行代码：

```py
logging.basicConfig(level=logging.DEBUG, 
                    format='[%(asctime)s] %(message)s',
                    handlers=[logging.FileHandler("debug.log"), 
                              logging.StreamHandler()])
```

如我们所见，我添加了一个新的参数“handlers”。一个 *StreamHandler* 正在控制台上显示日志，而 *FileHandler*，顾名思义，将相同的输出保存到文件中。

这个系统非常灵活。Python 中有许多不同的“处理器”对象，我鼓励读者自己去 [查阅手册](https://docs.python.org/3/library/logging.handlers.html)。正如我们已经知道的，日志几乎可以自动工作；无需进一步的代码更改。

## 4\. 循环日志文件

将日志保存到文件中是一个不错的选择，但遗憾的是，磁盘空间不是无限的。我们可以通过使用循环日志文件轻松解决这个问题：

```py
from logging.handlers import TimedRotatingFileHandler

...

if __name__ == "__main__":
    file_handler = TimedRotatingFileHandler(
            filename="debug.log",
            when="midnight",
            interval=1,
            backupCount=3,
        )
    logging.basicConfig(level=logging.DEBUG, 
                        format='[%(asctime)s] %(message)s',
                        handlers=[file_handler, logging.StreamHandler()])
```

所有参数都是显而易见的。一个 *TimedRotatingFileHandler* 对象将创建一个日志文件，该文件会在每个午夜更换，并且只保存最近的三个日志文件。之前的文件将自动重命名为类似“debug.log.2023.03.03”的格式，经过 3 天的间隔后将被删除。

## 5\. 通过套接字发送日志

Python 的 logging 出奇地灵活。如果我们不想将日志保存到本地文件中，我们可以只需添加一个套接字处理程序，它将使用特定的 IP 和端口将日志发送到另一个服务：

```py
from logging.handlers import SocketHandler

logging.basicConfig(level=logging.DEBUG, format='[%(asctime)s] %(message)s',
                    handlers=[SocketHandler(host="127.0.0.1", port=15001), 
                              logging.StreamHandler()])
```

就是这样；无需更多的代码更改！

我们还可以创建另一个应用程序来监听相同的端口：

```py
import socket
import logging
import pickle
import struct
from logging import LogRecord

port = 15001
stream_handler = logging.StreamHandler()

def create_socket() -> socket.socket:
    """Create the socket"""
    sock = socket.socket(socket.AF_INET)
    sock.settimeout(30.0)
    sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    return sock

def read_socket_data(conn_in: socket.socket):
    """Read data from socket"""
    while True:
        data = conn_in.recv(4)  # Data: 4 bytes length + body
        if len(data) > 0:
            body_len = struct.unpack(">L", data)[0]
            data = conn_in.recv(body_len)
            record: LogRecord = logging.makeLogRecord(pickle.loads(data))
            stream_handler.emit(record)
        else:
            logging.debug("Socket connection lost")
            return

if __name__ == "__main__":
    logging.basicConfig(level=logging.DEBUG, format='[%(asctime)s] %(message)s',
                        handlers=[stream_handler])

    sock = create_socket()
    sock.bind(("127.0.0.1", port))  # Local connections only
    sock.listen(1)  # One client can be connected
    logging.debug("Logs listening thread started")
    while True:
        try:
            conn, _ = sock.accept()
            logging.debug("Socket connection established")
            read_socket_data(conn)
        except socket.timeout:
            logging.debug("Socket listening: no data")
```

这里的难点是使用 *emit* 方法，它将所有通过套接字接收到的远程数据添加到一个活动的 StreamHandler 中。

## 6\. 奖励：日志过滤器

最后，对那些足够细心到这一部分的读者来说，还有一个小小的奖励。添加自定义日志过滤器也很简单。假设我们只想将 X 和 Y 的值记录到文件中以备将来分析。创建一个新的 Filter 类也很简单，它将仅保存包含“x:”或“y:”记录的日志：

```py
from logging import LogRecord, Filter

class DataFilter(Filter):
    """Filter for logging messages"""

    def filter(self, record: LogRecord) -> bool:
        """Save only filtered data"""
        return "x:" in record.msg.lower() or "y:" in record.msg.lower()
```

然后我们可以轻松地将这个过滤器添加到文件日志中。我们的控制台输出将保持不变，但文件中只会有“x:”和“y:”的值。

```py
file_handler = logging.FileHandler("debug.log")
file_handler.addFilter(DataFilter())

logging.basicConfig(level=logging.DEBUG, 
                    format='[%(asctime)s] %(message)s',
                    handlers=[file_handler, logging.StreamHandler()])
```

## 结论

在这篇简短的文章中，我们学习了几种将日志集成到 Python 应用程序中的简单方法。Python 中的日志系统是一个非常灵活的框架，绝对值得花时间深入了解它的工作原理。

感谢阅读，祝你未来的实验好运。

如果你喜欢这个故事，可以随时[订阅](https://medium.com/@dmitryelj/membership)Medium，这样你会在我发表新文章时收到通知，并且可以全面访问其他作者的数千篇故事。
