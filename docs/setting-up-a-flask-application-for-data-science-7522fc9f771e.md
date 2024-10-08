# 为数据科学设置 Flask 应用

> 原文：[`towardsdatascience.com/setting-up-a-flask-application-for-data-science-7522fc9f771e`](https://towardsdatascience.com/setting-up-a-flask-application-for-data-science-7522fc9f771e)

## 构建一个 Flask 应用的基本结构，以支持模块化开发

[](https://philip-wilkinson.medium.com/?source=post_page-----7522fc9f771e--------------------------------)![Philip Wilkinson, Ph.D.](https://philip-wilkinson.medium.com/?source=post_page-----7522fc9f771e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7522fc9f771e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7522fc9f771e--------------------------------) [Philip Wilkinson, Ph.D.](https://philip-wilkinson.medium.com/?source=post_page-----7522fc9f771e--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7522fc9f771e--------------------------------) ·阅读时间 9 分钟·2023 年 3 月 10 日

--

![](img/7e9b18fe28a0c2adf0b7354b9a41f272.png)

照片由 [KOBU Agency](https://unsplash.com/@kobuagency?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数据科学工作流程通常涉及笔记本和 Python 脚本的使用。这些都是很棒的工具，但通常意味着你的输出可能会一直保留在这些文件中，无法展示。然而，改变这种情况的一个好方法是创建一个网站来展示和讨论你的发现，或者创建一个 API 将你的模型提供给全世界。Flask 是一个可以帮助实现这一点的框架。

Flask 允许你构建网站和 API，使你可以更广泛地分享你的成果。无论是通过一个展示你的工作和结果的界面，还是通过一个其他人可以调用以获取模型预测的 API，Flask 都能实现。Flask 是一个轻量级框架，易于学习和使用，非常适合希望专注于构建模型和分析数据的 Data Scientist，而不是学习复杂的 Web 开发框架。它也使用 Python，因此数据科学工作流程中的许多步骤可以很容易地转移到 Flask 框架中。

在本文中，我将向你展示如何设置一个 Flask 应用的基本框架，之后你可以在其基础上进行扩展。这将包括解释应用的基本结构以及你希望放入每个文件的内容。

## Flask 项目结构

我们可以从基本的 Flask 结构概述开始。它的形式如下：

```py
application/
|-static/
|--css/
|--images/
|--scipts/
|-templates/
|--includes/
|--index.html
|-__init__.py
|-routes.py
tests/
flask_env/
.env
.env.template
.gitignore
flask_config.py
app.py
requirements.txt
```

其中：

+   `application/` 目录包含主要的 Flask 应用代码

+   `static/` 目录包含网站中使用的静态文件，如 CSS 样式表、JavaScript 脚本和图片

+   `templates/` 目录包含 Flask 应用使用的 HTML 模板

+   `__init__.py` 初始化 Flask 应用程序并设置配置和数据库连接。

+   `routes.py` 定义了 Flask 应用程序的路由。

+   `tests/` 目录包含 Flask 应用程序的测试文件。

+   `flask_env` 包含 Flask 应用程序的虚拟环境。

+   `.env` 文件包含 Flask 应用程序使用的环境变量。

+   `flask_config.py` 包含 Flask 应用程序的配置设置。

+   `app.py` 是 Flask 应用程序的入口点，并运行 Flask 开发服务器。

那么，我们如何开始呢？

## 设置环境。

要开始构建这个 Flask 应用程序，你需要创建一个 Python 虚拟环境。这是一个好习惯，有助于将应用程序的依赖项与机器上的全局 Python 环境隔离开来。这使得管理依赖项变得更容易，并确保应用程序在不同机器上顺利运行。

为此，你需要首先打开一个终端窗口，并导航到你想要创建虚拟环境的目录。然后运行以下命令：

```py
python -m venv <venv_name>
```

在这种情况下，我将其命名为 `flask_application`，但也可以简单地命名为 `env`。

然后你可以使用以下命令激活环境：

```py
#on windows
<venv_name>\Scripts\activate

#on Max or Linux
source <venv_name>/bin/activate
```

并安装常用库。在这种情况下，我们将使用的基本库是 `flask`（当然）、`python-dotenv` 和 `pytest`。要安装这些库，你可以运行以下命令：

```py
pip install flask python-dotenv pytest
```

这些库应该安装在你的虚拟环境中。请注意，有时这可能需要一些时间，所以不要太担心！

当对虚拟环境进行更改时，例如安装新库时，最好冻结要求，以便其他人知道你使用了哪些库及其版本。这可以通过以下命令完成：

```py
pip freeze > requirements.txt
```

这将创建 `requirements.txt` 文件。这将使其他人更容易在自己的机器上复制你的环境。

## 基础文件。

一旦你设置好虚拟环境并创建了 `requirements.txt` 文件，你就可以开始构建你的应用程序。因此，我们可以从目录结构底部的文件开始：

```py
.env
.env.template
.gitignore
flask_config.py
app.py
```

你需要首先从 `.env` 文件开始，该文件用于设置应用程序的环境变量。刚开始时，它看起来像这样：

```py
# Flask server configurations
FLASK_APP=app
FLASK_ENV=development
FLASK_DEBUG=1

# Secret key
SECRET_KEY = b'nnz\x89\x0f\xde\x8a\xc4\x13\xe0\xf0\xca>=\xe0\xfe'
```

第一行告诉 Flask 应用程序应用程序文件叫做 `app.py`（尚未创建）。使用 `flask run` 命令时，这将告诉应用程序运行 `app.py` 文件以启动应用程序。

我们还从开发环境开始，因为这只是目前在本地机器上。为此，我们设置`FLASK_ENV = development`和`FLASK_DEBUG=1`，这表明我们有一个开发环境，并在环境启动时运行 Flask 调试器。这允许热重载，因此当您对代码进行任何更改时，不必重启实例即可在浏览器中查看这些更改！

此文件中的最后一行是秘密密钥。这是一个加密密钥，用于加密会话数据和客户端与服务器之间传输的其他敏感数据，是重要的安全措施。在这种情况下，我们使用一个 16 位密钥，该密钥通过以下命令生成：

```py
python3 -c "import os; print(os.urandom(16))"
```

如果您有其他信息或密钥需要应用程序使用，例如 API 密钥，可以将这些信息添加到此文件中。由于这些是环境变量，通常使用蛇形命名法（空格替换为`_`），所有字母都大写。

当然，除非您与他人合作并希望安全地共享，否则您实际上不希望与任何人分享您的秘密或 API 密钥。这就是为什么您需要复制`.env`文件来创建`.env.template`文件，并删除您希望保密的任何信息。其他开发人员可以使用`.env.template`文件创建自己的`.env`文件，填入自己的密钥和信息，或在必要时安全地共享自己的密钥。

这意味着在您的`.gitignore`文件中，您只需写入：

```py
.env
```

这将告诉 git 忽略对`.env`文件的任何更改，以确保您的秘密不会被共享。

下一阶段是创建`flask_config.py`文件，用于为您的应用程序创建一个配置对象。在我们的案例中，这个对象的形式为：

```py
import os 
from dotenv import load_dotenv

load_dotenv()

class Config:
    def __init__(self):
        """Base configuration variables"""
        self.SECRET_KEY = os.environ.get("SECRET_KEY")
        if not self.SECRET_KEY:
            raise("Secret key is missing. Something is wrong")
```

该密钥使用了在`.env`文件中定义的`SECRET_KEY`变量。如果没有找到秘密密钥，将会引发错误，并阻止应用程序运行。这是为了确保应用程序按预期运行，并且是安全的，您可以根据需要添加其他配置参数。

最后，我们有`app.py`文件，它是 Flask 应用程序的入口点，用于运行 Flask 开发服务器。目前，这仅包含：

```py
from application import app

if __name__ == "__main__":
    app.run()
```

该结构导入`application`文件夹中的`app`并运行它。建立了基础结构后，我们可以继续创建应用程序结构。

## 应用程序结构

我们在仓库中创建一个`application`文件夹，以结构化和模块化的方式组织应用程序代码和资源。具体而言，这种结构使得应用程序可以在需要时扩展为更全面的应用程序，通过创建多个路由、模型、视图模型和模板，从而提高代码的可维护性和可扩展性。然而，目前我们只需要一个简单的结构。

我们可以通过创建 `__init__.py` 文件开始，该文件用于初始化从基础文件夹中的 `app.py` 文件调用的应用程序。它包含：

```py
from flask import Flask
from flask_config import Config

app = Flask(__name__)
app.config.from_object(Config)

from application import routes
```

它导入 `flask` 库和 `Config` 对象，初始化应用程序，然后导入路由。

重要的是，我们可以看到这个文件中没有定义任何路由或视图。为了保持模块化结构，这些决定应用程序行为的路由是在 `routes.py` 文件中定义的，与初始化分开。

因此，我们可以将 `routes.py` 文件定义如下：

```py
from flask import render_template
from application import app

@app.route("/")
def index():
    return render_template("index.html")
```

这里有两个主要要点需要注意：

1.  奇怪的装饰器语法 `@app.route("/")`。这个装饰器用于定义用户可以访问的路由，接受一个 URL 模式参数并将其与视图函数关联。在这个示例中，我们为根 URL (`/`) 定义了一个路由，并将其与 `index()` 函数关联。

1.  `index()` 函数不是返回一个值，而是返回函数 `render_template("index.html")`。这个函数由 flask 提供，用于允许函数返回预定义的 HTML 模板。

那么这些 HTML 模板是什么呢？

好吧，在 `index()` 函数中我们返回了 `index.html` 模板。从文件结构来看，它位于 `templates` 文件夹中，并包含：

```py
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <link rel="stylesheet" type="text/css" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
    <link rel="stylesheet" href="{{url_for('static', filename='css/style.css')}}">
    <title>{% block title %}Hello World{% endblock %}</title>
  </head>
  <body>

    <div class = "container body-content">
        <h1>Hello World!</h1>
    </div>

    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
  </body>
</html>
```

这本质上是一个 HTML 文件，在我们的例子中简单地显示“Hello World”，可以在 `container` 类中的 `<h1></h1>` 标签之间看到。

我们还在头部和主体中添加了一些额外的内容，主要用于导入 bootstrap，一个使开发变得更容易的 CSS 框架，以及 JQuery，使应用程序的交互更容易。但如果你愿意，可以忽略这些，你可以简单地在主体中定义 `<h1>Hello World!</h1>`！

## 其他文件和文件夹

我们已经覆盖了应用程序的基本结构及其内容，但还有一些其他文件和文件夹没有涉及：

+   `application/templates` — 这个文件夹将包含应用程序使用的所有 HTML 模板。

+   `application/templates/includes` — 这个文件夹可以包含你可能想要在布局或其他文件中包含的辅助 HTML 结构，例如导航栏、页脚或大屏幕。

+   `tests` — 这个文件夹将包含所有对 flask 应用程序的测试，进一步分为 `functional` 和 `unit` 测试两个文件夹。

+   `static` — 这个文件夹包含了应用程序中使用的所有静态文件，包括 `css`、`images` 和可能使用的 `scripts`。

## 运行应用程序

现在我们已经完成了所有结构设置和环境配置，只需运行命令 `flask run`。你应该会看到类似如下的内容：

![](img/ccd19436518b7859f97b988e65facc57.png)

图片由作者提供

这告诉你应用程序正在运行，并且运行在 `http://127.0.0.1:5000`。

这意味着你应该能够在浏览器中导航到这个 URL 或 `localhost:5000` 并查看输出：

![](img/41cce9b6fffbf6d7c600ecef90dd6a57.png)

图片由作者提供

这表明应用程序正在运行！恭喜你，成功运行了第一个 Flask 应用！

## 扩展功能

超越这里展示的基本结构，Flask 具有相当大的灵活性，允许各种不同的扩展：

+   Flask-wtf：一个可以以安全和可靠的方式处理用户输入的库

+   Flask-SQLAlchemy：一个允许你与 SQLAlchemy ORM 及数据库集成的库（还有其他库可以与如 MongoDB 等常见数据库进行交互）

+   Flask-Login：一个提供用户认证功能的库，如果你想根据用户角色管理对应用程序某些部分的访问权限。

在许多更多的库和用例中！这种结构通过允许你以模块化的方式构建应用程序，从而促进了这些扩展，提升了代码的可维护性、可扩展性和模块化。不论你是想通过路由服务你的机器学习算法的 API，还是想在网站上展示你的故事！

本文的代码可以在：[`github.com/PhilipDW183/flask_structure`](https://github.com/PhilipDW183/flask_structure) 获取
