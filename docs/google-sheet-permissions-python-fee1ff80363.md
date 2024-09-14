# 如何使用 Python 更改 Google 表格权限

> 原文：[`towardsdatascience.com/google-sheet-permissions-python-fee1ff80363`](https://towardsdatascience.com/google-sheet-permissions-python-fee1ff80363)

## 使用 Python API 以编程方式与特定用户共享 Google 表格

[](https://gmyrianthous.medium.com/?source=post_page-----fee1ff80363--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----fee1ff80363--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fee1ff80363--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fee1ff80363--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----fee1ff80363--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fee1ff80363--------------------------------) ·阅读时间 5 分钟·2023 年 5 月 4 日

--

![](img/08ae206774c5ada897f91d3d70b78ba0.png)

由 [Artur Tumasjan](https://unsplash.com/es/@arturtumasjan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/zM8sSfYFDww?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

与他人共享 Google 表格是一个简单直接的任务，可以通过用户界面完成。然而，如果您需要将多个 Google 表格与特定用户或服务帐户共享呢？

想象一下，您在 BigQuery 上创建了数百个外部表，这些表从各种 Google 表格中获取数据。如果其他服务，例如 Apache Airflow，执行引用这些表的查询，您需要确保 Airflow 的服务帐户对所有这些表格具有足够的权限。但是，手动共享（即授予 `Viewer` 或 `Editor` 权限）数百个表格给特定主体几乎是不可能的，并且需要几个小时。

在本教程中，我们将演示如何使用 Python 和 Google Drive API 一次性更改数百个 Google 表格的权限

## 先决条件

我们首先需要确保成功获得了带有所需作用域的用户访问凭证。为此，只需运行以下命令：

```py
gcloud auth application-default login --scopes=https://www.googleapis.com/auth/spreadsheets,https://www.googleapis.com/auth/drive,https://www.googleapis.com/auth/iam.test
```

然后会弹出一个窗口，在默认浏览器中请求您登录 Google 帐户。请登录，因为凭证将适用于所有使用我们将在接下来几节中演示的应用程序默认凭证客户端库的 API 调用。

然后确保通过 pip 安装 Python API 客户端 `google-api-python-client`（最好在一个全新的虚拟环境中）：

```py
$ python3 -m pip install google-api-python-client
```

## 查找 Google 表格的 ID

在开始编写自动化解决方案以授予 Google Sheet 文件权限之前，我们首先需要找出所有感兴趣的每个文件的 ID。

要查找 Google Sheet 的文件 ID，只需在你喜欢的网页浏览器中打开它。链接应类似于下面显示的内容：

```py
https://docs.google.com/spreadsheets/d/abc-defg-1234/edit#gid=0 
```

`abc-defg-1234`对应于 Google Sheet 的 ID。

```py
https://docs.google.com/spreadsheets/d/spreadsheetId/edit#gid=0
```

有关 Google Sheets URL 是如何构建的更多详细信息，请参阅[Google Sheets API](https://developers.google.com/sheets/api/guides/concepts)概述。

## 使用 Python Google API 客户端更改 Google Sheet 权限

首先，让我们创建一个包含我们将要更改权限的 Google Sheet 文件 ID 的列表：

```py
google_sheet_ids = [
  'abc-1234',
  'def-5678',
  'ghi-9123',
]
```

现在我们需要做的第二件事是推断应用程序默认凭证，并创建 Google Drive 服务。

```py
import google.auth
from googleapiclient.discovery import build 

def create_service() -> Resource:
    """
    Creates a Google Drive (v3) service to interact with the API
    """
    scopes = [
        'https://www.googleapis.com/auth/spreadsheets',
        'https://www.googleapis.com/auth/drive',
    ]

    creds, project_id = google.auth.default(scopes=scopes)
    service = build('drive', 'v3', credentials=creds, cache_discovery=False)

    return service
```

现在，让我们创建一个只包含指定权限所需字段的 dataclass，基于[权限](https://developers.google.com/drive/api/reference/rest/v3/permissions#Permission) [REST 资源](https://developers.google.com/drive/api/reference/rest/v3/permissions#Permission)。

```py
from dataclasses import dataclass

@dataclass
class Permission:
    """
    Class that corresponds to the `permission` REST resource
    https://developers.google.com/drive/api/reference/rest/v3/permissions#Permission
    """
    type: str
    role: str
    emailAddress: str

    def __post__init__(self):
        """Validate input"""
        allowed_types = ['user', 'group', 'domain', 'anyone']
        if self.type not in allowed_types:
            raise ValueError(f'`{self.type}` is not a valid type. {allowed_types=}')

        allowed_roles = ['commenter', 'reader', 'writer', 'fileOrganizer', 'organizer', 'owner']
        if self.role not in allowed_roles:
            raise ValueError(f'`{self.role}` is not a valid role. {allowed_roles=}')
```

在下一步中，我们将编写一个函数，该函数本质上接受服务和权限的实例以及文件 ID，并尝试创建一个新的权限。

```py
from typing import Optional

from googleapiclient.discovery import Resource
from googleapiclient.errors import HttpError

def create_permission(
    service: Resource,
    permission: Permission,
    file_id: str,
    skip_on_failure: Optional[bool] = True,
):
    """
    Creates a new `permission` for the specified `file_id`
    """
    logging.info(f'Creating new permission {permission} for {file_id=}')
    try:
        request = service.permissions().create(
            fileId=file_id,
            body=asdict(permission),
            sendNotificationEmail=False,
        )
        response = request.execute()
        logging.info(f'New permission for {file_id=}: {response=}')
    except HttpError as error:
        logging.error(f'An error has occurred while trying to grant {permission=} to {file_id=}')
        logging.error(f'Error was: {error}')

        if not skip_on_failure:
            raise error
```

现在，让我们编写我们的`main()`方法，将所有部分整合在一起，最终与目标用户共享感兴趣的 Google Sheets。

```py
def main():
    google_sheet_ids = [
        'abc-1234',
        'def-5678',
        'ghi-9123',
    ]

    service = create_service()
    permission = Permission(type='user', role='writer', emailAddress='example@example.com')

    for file_id in google_sheet_ids:
        create_permission(service=service, permission=permission, file_id=file_id)
```

## 完整代码

这里有一个完全修订的代码版本，你可以用来指定新的

```py
import logging
from dataclasses import asdict, dataclass
from typing import Optional

from googleapiclient.discovery import build, Resource
from googleapiclient.errors import HttpError
import google.auth

logging.basicConfig(
    format='[%(asctime)s] {%(pathname)s:%(lineno)d} %(levelname)s - %(message)s',
    level=logging.INFO,
)

def create_service() -> Resource:
    """
    Creates a Google Drive (v3) service to interact with the API
    """
    scopes = [
        'https://www.googleapis.com/auth/spreadsheets',
        'https://www.googleapis.com/auth/drive',
    ]

    creds, project_id = google.auth.default(scopes=scopes)
    service = build('drive', 'v3', credentials=creds, cache_discovery=False)

    return service

def create_permission(
    service: Resource,
    permission: Permission,
    file_id: str,
    skip_on_failure: Optional[bool] = True,
):
    """
    Creates a new `permission` for the specified `file_id`
    """
    logging.info(f'Creating new permission {permission} for {file_id=}')
    try:
        request = service.permissions().create(
            fileId=file_id,
            body=asdict(permission),
            sendNotificationEmail=False,
        )
        response = request.execute()
        logging.info(f'New permission for {file_id=}: {response=}')
    except HttpError as error:
        logging.error(f'An error has occurred while trying to grant {permission=} to {file_id=}')
        logging.error(f'Error was: {error}')

        if not skip_on_failure:
            raise error

def main():
    google_sheet_ids = [
        'abc-1234',
        'def-5678',
        'ghi-9123',
    ]

    service = create_service()
    permission = Permission(type='user', role='writer', emailAddress='example@example.com')

    for file_id in google_sheet_ids:
        create_permission(service=service, permission=permission, file_id=file_id)

if __name__ == '__main__':
    main()
```

## 最终思考

授予用户对单个 Google Sheet 的访问权限是一个简单的任务，可以通过用户界面完成。只需点击电子表格右上角的‘分享’，输入用户的电子邮件地址，并选择他们的角色即可。然而，当涉及到为数百个电子表格或用户共享权限时，这一过程可能变得耗时且繁琐。

在本教程中，我们演示了如何使用 Google Drive API 和 Python Google API 客户端以编程方式为多个 Google Sheets 分配权限。我希望你觉得这篇文章有用。如果你在运行特定用例的代码片段时遇到任何困难，请在下面的评论中告诉我，我将尽力帮助你。

👉 [**成为会员**](https://gmyrianthous.medium.com/membership) **并阅读 Medium 上的每一个故事。你的会员费用直接支持我和你阅读的其他作者。你还将完全访问 Medium 上的每个故事。**

[](https://gmyrianthous.medium.com/membership?source=post_page-----fee1ff80363--------------------------------) [## 使用我的推荐链接加入 Medium — Giorgos Myrianthous

### 作为 Medium 会员，你的会员费用的一部分将支付给你阅读的作者，你可以完全访问每一个故事…

gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership?source=post_page-----fee1ff80363--------------------------------)

👇**相关的文章你也可能喜欢** 👇

[](/dbt-55b35c974533?source=post_page-----fee1ff80363--------------------------------) ## 什么是 dbt（数据构建工具）

### 轻松介绍 dbt，它正在主宰数据领域

towardsdatascience.com [](/etl-vs-elt-68e221d78719?source=post_page-----fee1ff80363--------------------------------) ## ETL 与 ELT：有什么区别？

### 在数据工程背景下对 ETL 和 ELT 的比较

towardsdatascience.com [](/setuptools-python-571e7d5500f2?source=post_page-----fee1ff80363--------------------------------) ## setup.py 与 setup.cfg 在 Python 中的区别

### 使用 setuptools 管理依赖项并分发你的 Python 包

towardsdatascience.com
