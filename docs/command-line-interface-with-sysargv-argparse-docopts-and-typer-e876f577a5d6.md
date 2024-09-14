# 使用 sysargv、argparse、docopts 和 Typer 的命令行接口

> 原文：[`towardsdatascience.com/command-line-interface-with-sysargv-argparse-docopts-and-typer-e876f577a5d6`](https://towardsdatascience.com/command-line-interface-with-sysargv-argparse-docopts-and-typer-e876f577a5d6)

## 将参数传递给 Python 脚本的 4 种方法

[](https://kayjanwong.medium.com/?source=post_page-----e876f577a5d6--------------------------------)![Kay Jan Wong](https://kayjanwong.medium.com/?source=post_page-----e876f577a5d6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e876f577a5d6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e876f577a5d6--------------------------------) [Kay Jan Wong](https://kayjanwong.medium.com/?source=post_page-----e876f577a5d6--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e876f577a5d6--------------------------------) ·阅读时间 9 分钟·2023 年 11 月 24 日

--

![](img/25677cbf2ad7407485c144f23c0c8104.png)

图片由 [Florian Olivo](https://unsplash.com/@florianolv?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

部署一个管道时，通常有一个 *main* 脚本，或者一个运行整个管道的单一入口点。例如，在数据科学管道中，代码仓库的入口点应该协调并顺序运行数据、特征工程、建模和评估管道。

> 有时，您可能需要运行不同类型的管道或对管道进行临时调整。

调整可能包括省略代码的某些部分或使用不同的参数值运行管道。在数据科学中，可能会有训练和评分管道，或某些运行需要对数据进行完全或部分刷新。

**最简单的解决方案是创建多个主脚本**。然而，这会导致代码重复，并且从长远来看，很难维护多个脚本——考虑到可能有许多不同的调整组合。**更好的解决方案是让主脚本接受参数**，以值或标志的形式，然后通过命令行接口（CLI）运行适当类型的管道。

本文不会详细讨论主脚本如何决定使用参数，而是介绍将参数传递给主脚本的不同方法——**可以将其视为您的主脚本现在是一个接受参数的函数**！我还将详细说明每种方法的优缺点，并提供从基本到高级用法的代码示例。

# 内容目录

+   [**使用 sysargv**](https://medium.com/p/e876f577a5d6/#3a69)：最简单的方法

+   [**使用 argparse**](https://medium.com/p/e876f577a5d6/#6b56)：最常见的方法

+   [**使用 docopts**](https://medium.com/p/e876f577a5d6/#bc27)：一种替代方式

+   [**使用 Typer**](https://medium.com/p/e876f577a5d6/#3e1a)：最新且最简单的方式

# 使用 sysargv

> 传递参数的最简单方式

参数可以通过 `sysargv` 直接传递和读取，使其成为传递多个参数的最简单方法。

## **演示**

在下面的演示中，传入参数后，我们可以看到 `sysargv` 将其解释为一个值列表。第一个值是脚本名称，后续的值都是传入的参数，用空格分隔。注意，所有传入的参数都被解释为字符串！

**代码**：

```py
# main_sysargv.py
import sys

if __name__ == "__main__":
    print(sys.argv)
```

**通过 CLI 调用**：

```py
$ python main_sysargv.py train 2023-01-01 
['main_sysargv.py', 'train', '2023-01-01']
```

## 优点

+   **简单直观**的使用

+   **多个参数**：可以传入无限数量的参数，通过列表方法进行引用

## 缺点

+   **未记录**：参数未命名，难以追踪期望参数的确切顺序

+   **仅字符串参数**：参数被解释为字符串。可以通过处理或将参数转换为其他类型来解决（可能需要额外步骤来验证参数类型和值）

# 使用 argparse

> 传递参数的最常见方式

解决使用 `sysargv` 的缺点，`argparse` 可以接收命名参数、不同数据类型的参数，并且功能更强大！这使得 `argparse` 成为传递参数到 Python 脚本的最受欢迎方式。

## 简单演示

在简单演示中，我们初始化一个 `ArgumentParser` 对象，并使用 `.add_argument()` 方法指定期望的参数及其类型。

为了解释参数，我们通过调用 `.parse_args()` 获得一个 *Namespace* 对象。然后可以通过点符号从 *Namespace* 对象中检索参数。

**代码**：

```py
# main_argparse.py
import argparse
import datetime

if __name__ == "__main__":
    parser = argparse.ArgumentParser()

    # Specify expected arguments
    parser.add_argument(
        "--train",
        type=bool,
    )
    parser.add_argument(
        "--start_date",
        type=lambda dt: datetime.datetime.strptime(dt, "%Y-%m-%d"),
    )

    # Interpret passed arguments
    args = parser.parse_args()
    print(args)
    print(args.train, type(args.train))
    print(args.start_date, type(args.start_date))
```

**通过 CLI 调用**：

```py
$ python main_argparse.py --train true --start_date 2023-01-01
Namespace(train=True, start_date=datetime.datetime(2023, 1, 1, 0, 0))
True <class 'bool'>
2023-01-01 00:00:00 <class 'datetime.datetime'>
```

## 高级演示

在高级演示中，我们将进行以下增强：

1.  **在 `argparse.ArgumentParser()` 中包含描述和尾注**：有助于在帮助文档中显示

1.  添加**位置参数**：位置参数是必需的，需要指定。如果有多个位置参数，它们是无名的，必须按顺序指定。

1.  添加**选项参数**：选项参数可以实现命名参数，这些参数可以接受一个或多个值，还可以实现开/关开关。

1.  指定**复合数据类型**，如 `Enum` 类和列表

1.  **解释传入的参数**：参数可以通过命令行或在代码中手动指定来传递

**代码**：

```py
# main_argparse2.py
import argparse
from enum import Enum

class ConstantsSaveLocation(Enum):
    LOCAL = "local"
    DATABASE = "database"

if __name__ == "__main__":
    # 1\. Include description and epilog
    parser = argparse.ArgumentParser(
        description="Run the training/scoring pipeline (text at the top)",
        epilog="Created by Kay Jan (text at the bottom)",
    )

    # 2\. Positional arguments
    parser.add_argument(
        "train",
        type=bool,
    )

    # 3\. Option arguments
    parser.add_argument(
        "--n_estimator",          # long name
        "-n",                     # short name; alias
        type=int,                 # simple data type
        required=True,            # make mandatory
        choices=[100, 200, 300],  # for limiting options
        default=400,              # default value
        dest="n",                 # for Namespace reference
        help="For model training",  # for help docs
        metavar="N",              # for help docs
    )

    # 3\. Option arguments (on/off switch)
    parser.add_argument(
        "--verbose",
        "-v",
        action="store_true",      # on/off switch
    )

    # 4\. Composite data type (Enum class)
    parser.add_argument(
        "--save_loc",
        type=ConstantsSaveLocation,
    )

    # 4\. Composite data type (list)
    parser.add_argument(
        "--item",
        type=str,
        nargs="*",
    )

    # 5\. Interpret passed arguments (from the command line via sysargv)
    args = parser.parse_args()
    print(args)

    # 5\. Interpret passed arguments (from passing arguments)
    args = parser.parse_args(
        [
            "true", "-n", "100", "-v",
            "--save_loc", "local", "--item", "a", "b", "c",
        ]
    )
    print(args)
```

**通过 CLI 调用**：

```py
$ python main_argparse2.py -h                                                      
usage: main_argparse2.py [-h] --n_estimator N [--verbose] [--save_loc SAVE_LOC] [--item ITEM [ITEM ...]] train

Run the training/scoring pipeline (text at the top)

positional arguments:
  train

options:
  -h, --help            show this help message and exit
  --n_estimator N, -n N
                        For model training
  --verbose, -v
  --save_loc SAVE_LOC
  --item ITEM [ITEM ...]

Created by Kay Jan (text at the bottom)

$ python main_argparse2.py true -n 100 -v --save_loc local --item a b c
Namespace(train=True, n=100, verbose=True, save_loc=<ConstantsSaveLocation.LOCAL: 'local'>, item=['a', 'b', 'c'])
Namespace(train=True, n=100, verbose=True, save_loc=<ConstantsSaveLocation.LOCAL: 'local'>, item=['a', 'b', 'c'])
```

## 其他高级用法

`argparse` 支持以下用法：

+   **子命令**：类似于调用 `git add` 和 `git commit`，其中 `add` 和 `commit` 是接受不同参数集的子解析器

+   **FileType 参数**：通过修改 `type` 参数值，解析器可以将文件名作为参数，并在 *Namespace* 对象中打开其内容。

推荐访问 [官方文档](https://docs.python.org/3/library/argparse.html) 以获取最新和完整的信息。

## 优点

+   **文档化**：帮助信息可显示可用的参数。

+   **支持多参数和多种数据类型**：能够处理多种数据类型的多个命名参数。

## 缺点

+   **冗长**：占用的代码行数多于 `sysargv`，可能会使主脚本变得杂乱。可以通过将 `argparse` 代码抽象到另一个文件来解决。

+   **仅为接口**：代码对主脚本没有实际价值，仅作为用户传递参数的接口。这可以被视为额外的代码行和文档重复工作。

# 使用 docopts

> 传递参数的另一种方法

在 `docopts` 中，参数按照文档字符串中的说明传递，无需额外的代码行（与 `argparse` 相对）！

> 注意：这不是 Python 标准库，你需要执行 `pip install docopts-ng`。

## 演示

文档必须按照特定格式编写，包含“Usage”和“Options”部分。对于用法，`()` 代表必需的参数，`[]` 代表可选参数，`...` 表示多个参数。

调用 CLI 时，会进行字符串匹配以查看参数与哪个使用版本匹配。参数可以从字典对象中检索。

**代码**：

```py
# main_docopt.py
"""Project Name
Description of project

Usage:
    main_docopt.py (train|test) --n_estimator <N> [--save_loc <LOC>] [--item <ITEM>...] [-v]
    main_docopt.py --version

Options:
    -h --help               Show this screen.
    --version               Show version.
    -n --n_estimator <N>    Number of estimator.
    --save_loc <LOC>        Save location.
    --item <ITEM>           Items.
    -v --verbose            Verbosity.
"""
from docopt import docopt

if __name__ == "__main__":
    args = docopt(__doc__, version="0.1.0")
    print(args)
```

**使用 CLI**：

```py
$ python main_docopt.py -h
Project Name
Description of project

Usage:
    main_docopt.py (train|test) --n_estimator <N> [--save_loc <LOC>] [--item <ITEM>...] [-v]
    main_docopt.py --version

Options:
    -h --help               Show this screen.
    --version               Show version.
    -n --n_estimator <N>    Number of estimator.
    --save_loc <LOC>        Save location.
    --item <ITEM>           Items.
    -v --verbose            Verbosity.

$ python main_docopt.py train --n_estimator 100 --save_loc database --item a --item b
{'--item': ['a', 'b'],
 '--n_estimator': '100',
 '--save_loc': 'database',
 '--verbose': False,
 '--version': False,
 'test': False,
 'train': True}
```

## 优点

+   **文档化**：帮助信息可显示可用的参数。

+   **简洁**：无需额外代码，文档直接翻译。

## 缺点

+   **仅支持字符串或布尔参数**：参数被解释为字符串或布尔值。可以通过处理或转换参数为其他类型来解决（可能需要额外的步骤来验证参数类型和值）。

+   **多余的参数**：文档字符串示例中指示的任何参数都会在解释的字典中反映出来（例如，`--version` 可能是字典中不必要的键）。

# 使用 Typer

> 传递参数的最新和最简单的方法

由与 FastAPI 相同的创建者开发，Typer 是传递参数的最新和最简单的方法。

> 注意：这不是 Python 标准库，你需要执行 `pip install 'typer[all]'`，它内部依赖 `click` 和 `rich`。

## 简单演示

在简单演示中，我们按照正常方式在脚本中编写一个主函数，并添加一行代码 `typer.run(main)` 以与 CLI 交互。

**代码**：

```py
# main_typer.py
import typer

def main(train: bool, start_date: str = "2010-01-01"):
    print(train, start_date)

if __name__ == "__main__":
    typer.run(main)
```

**使用 CLI**：

```py
$ python main_typer.py --help

 Usage: main_typer.py [OPTIONS] TRAIN                          

╭─ Arguments ─────────────────────────────────────────────────╮
│ *    train        [default: None] [required]                │
╰─────────────────────────────────────────────────────────────╯
╭─ Options ───────────────────────────────────────────────────╮
│ --start-date        TEXT  [default: 2010-01-01]             │
│ --help                    Show this message and exit.       │
╰─────────────────────────────────────────────────────────────╯

$ python main_typer.py true --start-date 2023-01-01 
True 2023-01-01
```

## 高级演示

在高级演示中，我们将使用类似于 FastAPI 中的 *app* 的 `typer`。`argparse` 中的子命令可以通过 `@app.command()` 装饰器实现——这使得使用非常简单！

**代码**：

```py
# main_typer.py
import typer
from enum import Enum
from typing import List

app = typer.Typer(help="Run the training/scoring pipeline")

class ConstantsSaveLocation(Enum):
    LOCAL = "local"
    DATABASE = "database"

@app.command()
def train(n_estimators: int, start_date: str = "2010-01-01"):
    print(n_estimators, start_date)

@app.command()
def test(save_loc: ConstantsSaveLocation, items: List[str]):
    print(save_loc, items)

if __name__ == "__main__":
    app()
```

**通过 CLI 调用**：

```py
$ python main_typer2.py --help

 Usage: main_typer2.py [OPTIONS] COMMAND [ARGS]...             

 Run the training/scoring pipeline                             

╭─ Options ───────────────────────────────────────────────────╮
│ --install-completion          Install completion for the    │
│                               current shell.                │
│ --show-completion             Show completion for the       │
│                               current shell, to copy it or  │
│                               customize the installation.   │
│ --help                        Show this message and exit.   │
╰─────────────────────────────────────────────────────────────╯
╭─ Commands ──────────────────────────────────────────────────╮
│ test                                                        │
│ train                                                       │
╰─────────────────────────────────────────────────────────────╯

$ python main_typer2.py train --help

 Usage: main_typer2.py train [OPTIONS] N_ESTIMATORS            

╭─ Arguments ─────────────────────────────────────────────────╮
│ *    n_estimators      INTEGER  [default: None] [required]  │
╰─────────────────────────────────────────────────────────────╯
╭─ Options ───────────────────────────────────────────────────╮
│ --start-date        TEXT  [default: 2010-01-01]             │
│ --help                    Show this message and exit.       │
╰─────────────────────────────────────────────────────────────╯

$ python main_typer2.py train 100 --start-date 2023-01-01
100 2023-01-01

$ python main_typer2.py test local a b c
ConstantsSaveLocation.LOCAL ['a', 'b', 'c']
```

## 其他高级用法

`typer` 支持以下用法：

+   **自动生成文档**：这需要 `pip install typer-cli`，并且可以从 CLI 命令生成 Markdown 文档！

+   **内置方法**：`typer.Argument()`、`typer.Option()`、`typer.Prompt()` 等是内置的 Typer 方法，用于增强帮助信息，使 CLI 更具交互性等

+   **测试**：类似于 FastAPI，Typer 参数可以使用 `typer.testing.CliRunner()` 进行测试，这使得代码更具鲁棒性

推荐访问 [官方文档](https://typer.tiangolo.com/) 以获取最新和完整的信息。

## 优点

+   **文档化**：提供帮助信息，展示可用的参数

+   **支持多个参数和多种数据类型**：能够处理多种数据类型的多个命名参数

+   **简洁**：只需添加少量代码，即可与现有的 Python 函数无缝配合

## 缺点

+   **冗长**：对于高级用法，需要添加更多的 Typer 特定代码，这可能会使代码变得冗长

希望你了解了更多关于向 Python 脚本传递参数的不同方式及其优缺点。作为编码人员，编写用户友好的代码与编写优雅高效的代码同样重要——**构建 CLI 应用程序是让用户或其他应用程序与代码接口的一个方法**。在下方的官方文档中还有更多高级用法。

# 相关链接

`**sysargv**`

+   官方文档：[`docs.python.org/3/library/sys.html#sys.argv`](https://docs.python.org/3/library/sys.html#sys.argv)

`**argparse**`

+   官方文档：[`docs.python.org/3/library/argparse.html`](https://docs.python.org/3/library/argparse.html)

`**docopts**`

+   官方文档：[`docopt.org/`](http://docopt.org/)

+   GitHub：[`github.com/jazzband/docopt-ng`](https://github.com/jazzband/docopt-ng)

**Typer**

+   官方文档：[`typer.tiangolo.com/`](https://typer.tiangolo.com/)
