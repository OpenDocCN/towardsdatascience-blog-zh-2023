# 如何在你的代码中集成 Microsoft Translator API

> 原文：[`towardsdatascience.com/how-to-integrate-the-microsoft-translator-api-in-your-code-89bad979028e`](https://towardsdatascience.com/how-to-integrate-the-microsoft-translator-api-in-your-code-89bad979028e)

## 一份全面且适合初学者的指南

[](https://namiyousef96.medium.com/?source=post_page-----89bad979028e--------------------------------)![Yousef Nami](https://namiyousef96.medium.com/?source=post_page-----89bad979028e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----89bad979028e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----89bad979028e--------------------------------) [Yousef Nami](https://namiyousef96.medium.com/?source=post_page-----89bad979028e--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----89bad979028e--------------------------------) ·13 分钟阅读·2023 年 1 月 24 日

--

![](img/af8adbcc9c869a0985b8e5a4eed8d533.png)

图片来自 [Unsplash](https://unsplash.com/photos/5Z8mR4vqJD4)，由 [Edurne](https://unsplash.com/@edurnetx) 提供。

目前有许多优秀的翻译服务，但其中最通用且易于设置的之一是 Microsoft Translator [[1](https://www.google.com/search?q=microsoft+translator&oq=microsoft+translator&aqs=chrome.0.35i39j69i59l2j0i512l2j69i60l3.2307j0j7&sourceid=chrome&ie=UTF-8)]，它为你提供了多种低资源和高资源语言的翻译服务，且免费（有些每月翻译限制）。

在本教程中，我将讲解如何在 Azure 上设置翻译器实例，以及如何在你的代码中编写接口以连接它，并遵循最佳实践。如果你对 Azure 已经很熟悉并且已经设置了翻译器实例，那么可以直接访问项目 [代码库](https://github.com/namiyousef/ml-utils) 获取代码。

本教程将涵盖：

1.  设置 Azure 翻译器实例

1.  发送你的第一次翻译请求

1.  清理你的代码和结构化你的项目

1.  使用 Jupyter Notebooks 的注意事项

1.  结论

# 在 Azure 上设置翻译器实例

## 创建 Azure 账户

第一步是创建一个 Microsoft Azure [帐户](https://azure.microsoft.com/en-us/free/)。这需要你拥有：

+   一个有效的地址

+   一个有效的电子邮件账户

+   一个有效的电话号码

+   一张有效的信用卡或借记卡*

一旦你创建了账户，你将被询问是否想使用免费服务或按需付费订阅。**选择免费服务，** 如果你觉得按需付费订阅更适合你，可以随时切换回去。** Azure 会积极尝试让你转到按需付费服务，但你可以始终坚持使用免费服务。

> ***注意：** 信用卡/借记卡用于验证您的身份。如果您使用的是免费套餐，则不会扣取任何费用。
> 
> ****注意：** 要了解翻译服务的定价信息，请访问 [翻译服务定价文档](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/translator/)。

## 设置翻译器 API

登录到 Azure 后，点击“创建资源”，然后搜索“翻译器”。最后，点击翻译服务并点击创建。

![](img/35bcb719ff446fc4d866696606afc9f6.png)

完成后，您会看到一个需要填写多个参数的页面：

+   **资源组：** 用于收集属于同一项目的多个资源的名称。这会影响您如果选择非免费订阅时的计费方式。请为其命名一个与您的项目相关的名称。

+   **区域*：** 您的实例运行所在的区域。这与微软如何管理资源和灾难 [恢复](https://learn.microsoft.com/en-us/azure/reliability/availability-zones-overview) 相关。推荐的区域是全球。

+   **名称：** 您的翻译服务的名称。对于翻译目的，这没有影响，但如果您需要文档翻译，它将影响资源端点的名称。

+   **定价层*：** 起初选择免费版本

填写完成后，点击创建。Azure 将进行简单验证，并将您带到另一个页面，在那里您可以确认资源的创建。

![](img/32f3c0de3c658d51d30c143d00bf02ce.png)

> ***注意：** 您不能在相同的区域和定价层中拥有多个翻译器实例。例如，如果您在东部美国地区有一个免费套餐实例，则要添加另一个免费实例，您需要更改区域。

## 查找有关您资源的信息

默认情况下，Azure 会将您带到您创建的资源。但是，下次登录时，您需要自己查找它。您可以通过点击首页上的翻译图标来做到这一点。这将带您到翻译页面，在那里您可以找到所有实例。

![](img/fdc59f99c3b38ab73b0e13d8238303a3.png)

点击您的实例将带您到实例页面，在那里您可以找到所有相关的配置和详细信息。这些将在下一节中变得重要。

现在，您可以在浏览器中使用您的翻译实例，以了解输入文本是如何表示的，输出文本的样子：

![](img/594c029e96b5be7789fc4696716a5c12.png)

# 发送您的第一个翻译请求

默认情况下，Azure 会提供您可以复制的默认代码，以进行第一个翻译请求。然而，如果您对请求的工作原理不熟悉，可能会很难理解它的作用，从而无法在代码中有效使用它。在这里，我将逐步讲解进行第一个翻译请求的相关概念。

## HTTPs 请求简要介绍

在编写代码之前，了解几个与翻译 API 相关的概念是很有价值的。

+   API 有一个 URL 允许你访问它。对于 Microsoft Translator，这是一个公开的 URL：

[](https://api.cognitive.microsofttranslator.com/?source=post_page-----89bad979028e--------------------------------) [## Azure Cognitive Services Translator 文档 - 快速入门，教程，API 参考 - Azure…

### Azure Cognitive Services Translator 是一个基于云的机器翻译服务，你可以用来翻译文本……

[api.cognitive.microsofttranslator.com](https://api.cognitive.microsofttranslator.com/?source=post_page-----89bad979028e--------------------------------)

+   API 有 **端点**（这些类似于 URL 中的路径），你可以向其发送 HTTPS 请求。例如，最基本的端点是 **languages** 端点。这个端点简单地返回你可以选择的所有语言。它是一个 **get** 端点，因为它“获取”来自 API 或资源的资源或数据。

+   每个端点都有参数来指定你从端点请求的内容。例如，语言端点有一个参数 **api-version**，它指示你使用的是哪个版本的翻译器。

例如，使用 Microsoft API 版本 3.0 的语言端点的完整 URI 如下：

![](img/abcbe6622a55c588a257a2f71a77236d.png)

你可以在 Python 中使用 `requests` 模块调用语言端点：

```py
import requests
microsoft_api_url = 'https://api.cognitive.microsofttranslator.com/languages?api-version=3.0'
resp = requests.get(microsoft_api_url)
print(resp.json())
```

这会向 API 发送一个 HTTPS 请求，以检索翻译器 3.0 版本可用的语言。实际上，因为这个端点是公开的，你可以将那个 URL 复制并粘贴到浏览器*中，以获取与你在代码中得到的相同的输出：

[`api.cognitive.microsofttranslator.com/languages?api-version=3.0`](https://api.cognitive.microsofttranslator.com/languages?api-version=3.0)

> ***注意：** 在后台，你的浏览器正在向 URL 发送 **get** 请求并返回输出。

你可以在官方 API [文档](https://learn.microsoft.com/en-us/azure/cognitive-services/translator/reference/v3-0-reference) 上找到有关可用端点的更多信息。

## 翻译端点

这是允许我们翻译文本的端点。与 **languages** 端点不同，这是一个 **post** 请求，而不是 **get** 请求。这意味着你正在发送一些数据以生成输出。你不是单纯地“获取”资源。你将数据作为 **request body** 的一部分发送。这些是作为请求的一部分传输的数据字节，但它们与参数不同，因为它们不会附加到 URI 路径上。

**translate** 端点具有以下要求：

**参数**

+   **api-version (必需)：** 你希望使用的翻译器版本

+   **to (必需)：** [ISO 639–1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) 语言代码，你希望将文本翻译成的语言

**请求体**

+   你想要翻译的文本数组，格式如下：`{"text": "这是我想要翻译的句子"}`

在 Python 中，你可以通过以下方式**发送**请求。我特意添加了两种翻译语言，以展示如何将多个相同名称的参数添加到请求 URL 中。

```py
import requests
body = [{"text": "First sentence I want to translate"}, {"text": "Second sentence I want to translate"}]
api_version = "3.0"
german_iso_code = 'de'
arabic_iso_code = 'ar'
endpoint = 'translate'

url = f'https://api.cognitive.microsofttranslator.com/{endpoint}?api-version={api_version}&to={german_iso_code}&to={arabic_iso_code}'

resp = requests.post(url, json=body)
```

## 管理访问权限

现在，如果你运行了上述代码，你应该会遇到错误。这是因为我们不能仅仅运行 POST 服务。我们需要进行身份验证。

这就是我们最初需要创建账户和翻译器实例的原因。

附加到你的实例上的唯一密钥允许 Microsoft：

a) 验证你发送的请求是否来自拥有 Azure 账户的来源

b) 计算你对服务的使用情况，用于计费或限制目的*

> ***注意：** 记住在免费版本中，虽然你不被收费，但你每月可以进行的翻译次数是有限的。

这个唯一的密钥可以通过使用请求头与 Microsoft 进行通信。这些是 HTTPs 中的关键概念。它们可以告诉服务器以下关于你的请求的信息：

+   IP 地址和端口

+   预期的数据类型

+   身份验证详情

翻译器 API 需要以下项目在头部：

+   **订阅密钥：** 这是验证你有权使用服务的密钥。它与教程开始时你创建的翻译器资源相关联。

+   **订阅区域：** 这是你的项目所在的区域。

+   **内容类型：** 正在发送的数据类型

+   **客户端追踪 ID：** 唯一标识你的计算机的 ID。你可以在[这里](https://www.geeksforgeeks.org/generating-random-ids-using-uuid-python/)阅读更多关于此的信息。

你可以在 Azure 项目页面找到你的订阅密钥：

![](img/158d7dbebfb17c753a84ca7d37cc6e95.png)

在“密钥和端点”页面中，你可以找到两个 API 密钥（其中任何一个都可以用来验证你的身份）。

最后，你可以定义头部并将其添加到你上面创建的 POST 请求中，以获得成功的翻译输出：

```py
 # code as before, new additions enclosed in ------

import requests
body = [{"text": "First sentence I want to translate"}, {"text": "Second sentence I want to translate"}]
api_version = "3.0"
german_iso_code = 'de'
arabic_iso_code = 'ar'
endpoint = 'translate'

### -----------------------------------------------------
import uuid

# YOUR PROJECT CREDENTIALS
your_key = "your_key_keep_this_a_secret"
your_project_location = "your_project_location"

# headers
headers = {
  'Ocp-Apim-Subscription-Key': your_key,
  'Ocp-Apim-Subscription-Region': your_project_location,
  # default values
  'Content-type': 'application/json',
  'X-ClientTraceId': str(uuid.uuid4())
}
### -----------------------------------------------------

url = f'https://api.cognitive.microsofttranslator.com/{endpoint}?api-version={api_version}&to={german_iso_code}&to={arabic_iso_code}'

resp = requests.post(
  url,
  headers=headers,  # add the headers
  json=body
)
```

> 你的 API 密钥是允许你使用服务的凭证。这些密钥绝不能泄露，建议每隔几个月重新生成一次。下一节我们将讨论减少泄露风险的最佳实践。

# 清理代码和结构化你的项目

本节将介绍在你的代码和项目中集成 Microsoft Translate API 功能的良好软件开发实践。我们将涵盖：

+   目录结构

+   如何隐藏凭证

+   如何将请求打包成函数并添加基本日志记录

+   如何添加有用的文档

## 目录结构

在开发应用程序时，你可能会与多个外部 API 交互。因此，最好将外部 API 的功能存储在单独的文件中，然后在主应用程序代码中调用它们。我建议将所有外部 API 放在一个名为‘external_apis’的子文件夹下，并将包含调用每个 API 函数的 Python 文件分开。我还建议在 external_apis 子文件夹中添加一个`config.py`文件，以添加外部 API 的配置。

![](img/0fc0c43358870353da55a8dcfb86b3e0.png)

## 使用环境变量隐藏凭据

> **记住：** 你绝不应该泄露你的 API 密钥。如果泄露了，请立即重新生成它们。

然而，你需要它们来进行翻译请求。一般来说，你应该避免（按严重程度排序）：

+   **在代码中硬编码密钥：** 即使你私下托管你的代码，密钥仍然会出现在提交历史中。

+   **在任何地方打印你的密钥：** 风险较小，但打印语句增加了密钥被推送到 GitHub 作为 Jupyter Notebook 输出的一部分或存储在服务器日志中的可能性。

+   **将你的密钥保存在配置文件中：** 风险更小，因为意外推送配置文件的可能性较低，而`.gitignore`可以使其几乎不可能。不过，仍然有更好的方法。

使用环境变量来管理代码中的凭据是最佳方法。这些是基于会话的变量，意味着它们仅在你运行代码的终端会话期间保存，从而大大减少人为错误。

为此，我们可以使用`config.py`文件：

```py
import os

MICROSOFT_TRANSLATE_API_KEY = os.environ.get('MICROSOFT_TRANSLATE_API_KEY', 'default_key')
```

这样，默认情况下我们的密钥取值为“default_key”。在运行任何使用该密钥的代码之前，我们需要在终端中显式设置它：

```py
python -c "from package_name.external_apis.config import MICROSOFT_TRANSLATE_API_KEY; print(MICROSOFT_TRANSLATE_API_KEY)"

export MICROSOFT_TRANSLATE_API_KEY="your_actual_key"

python -c "from package_name.external_apis.config import MICROSOFT_TRANSLATE_API_KEY; print(MICROSOFT_TRANSLATE_API_KEY)"
```

如果你想要更加谨慎，可以对 API 密钥添加额外的抽象层，使其难以意外提取其值。例如，你可以创建一个`Password`类，将密码存储为隐藏变量，然后添加一个显式的“get_password”方法：

```py
import os

class Password:
  def __init__(self, password):
    self.__password = password

  def get_password():
    return self.__password

MICROSOFT_TRANSLATE_API_KEY_CLASS = Password(os.environ.get('MICROSOFT_TRANSLATE_API_KEY', 'default_key'))

print(MICROSOFT_TRANSLATE_API_KEY_CLASS.get_password())  # prints password
print(MICROSOFT_TRANSLATE_API_KEY_CLASS.password)  # error
print(MICROSOFT_TRANSLATE_API_KEY_CLASS.__password)  # error
```

这样，在定义请求的`headers`时，你调用`get_password`方法。

## 将代码打包成函数并添加日志记录

现在我们了解了基础知识，可以进行一些改进：

+   **在** `**config.py**` **文件中添加所有 Microsoft Translator API 的标识符**

```py
"""
config.py file
"""
import os

# MICROSOFT API CONFIGS
MICROSOFT_TRANSLATE_URL = 'https://api.cognitive.microsofttranslator.com'
MICROSOFT_TRANSLATE_LOCATION = os.environ.get('MICROSOFT_TRANSLATE_LOCATION', 'default_location')
MICROSOFT_TRANSLATE_API_KEY = os.environ.get('MICROSOFT_TRANSLATE_API_KEY', 'default_key')
```

这里我们还添加了你实例的位置作为环境变量。

+   **为每个端点添加单独的函数**

```py
"""
microsoft.py file
"""

import uuid
from package_name.external_apis.config import (
  MICROSOFT_TRANSLATE_URL,
  MICROSOFT_TRANSLATE_LOCATION,
  MICROSOFT_TRANSLATE_API_KEY
)

# -- prepare headers
HEADERS = {
  'Ocp-Apim-Subscription-Key': MICROSOFT_TRANSLATE_API_KEY,
  'Ocp-Apim-Subscription-Region': MICROSOFT_TRANSLATE_LOCATION,
  'Content-type': 'application/json',
  'X-ClientTraceId': str(uuid.uuid4())
}

# -- utils
def _is_response_valid(status_code):
    if str(status_code).startswith('2'):
        return True

# -- functions for endpoints

# /languages endpoint
def get_languages(api_version='3.0'):

    # prepare url
    url = f'{MICROSOFT_TRANSLATE_URL}/languages?api-version={api_version}'

    # send request and process outputs
    resp = requests.get(url)
    status_code = resp.status_code
    if _is_response_valid(status_code):
        return resp.json(), status_code

    return resp.text, status_code

# /translate endpoint
def translate_text(text, target_language, source_language=None, api_version='3.0'):

    # send request and process outputs
    url = f'{MICROSOFT_TRANSLATE_URL}/translate?api-version={api_version}'

    # standardise target language type
    if isinstance(target_language, str):
        target_language = [target_language]

    # dynamically add array parameter to url
    for lang in target_language:
        url = f'{url}&to={lang}'

    if source_language:
        url = f'{url}&from={source_language}'

    # standardise text type
    if isinstance(text, str):
        text = [text]

    # dynamically build the request body
    body = [{'text': text_} for text_ in text]

    # send request and process outputs
    resp = requests.post(url, headers=HEADERS, json=body)
    status_code = resp.status_code

    if _is_response_valid(status_code)
        return resp.json(), status_code

    return resp.text, status_code
```

+   **使用** `**typing**` **和** `**sphinx**` **风格的文档字符串添加日志记录和文档**

```py
"""
microsoft.py file
"""

import uuid
import logging
from package_name.external_apis.config import (
  MICROSOFT_TRANSLATE_URL,
  MICROSOFT_TRANSLATE_LOCATION,
  MICROSOFT_TRANSLATE_API_KEY
)

# imports for typing annotations
from typing import Optional, Union, List

# -- configure logger. Taken from official python docs
LOGGER = logging.getLogger(__name__)
LOGGER.setLevel(logging.DEBUG)
ch = logging.StreamHandler()
ch.setLevel(logging.DEBUG)
date_format = '%Y-%m-%d %H:%M:%S'
formatter = logging.Formatter('%(asctime)s:%(name)s:%(levelname)s:%(message)s', datefmt=date_format)
ch.setFormatter(formatter)
LOGGER.addHandler(ch)

# -- prepare headers
HEADERS = {
  'Ocp-Apim-Subscription-Key': MICROSOFT_TRANSLATE_API_KEY,
  'Ocp-Apim-Subscription-Region': MICROSOFT_TRANSLATE_LOCATION,
  'Content-type': 'application/json',
  'X-ClientTraceId': str(uuid.uuid4())
}

# -- utils
def _is_response_valid(status_code: int) -> Optional[bool]:
    """ Function to check response is valid or not

    :param status_code: status code from response
    :returns: True if valid response, None otherwise
    """
    if str(status_code).startswith('2'):
        return True

# -- functions for endpoints

# /languages endpoint
def get_languages(api_version: str = '3.0') -> tuple:
    """ get languages available from API for specific version

    :param api_version: version of API to use
    :returns: (available languages, status_code)

    """
    # prepare url
    url = f'{MICROSOFT_TRANSLATE_URL}/languages?api-version={api_version}'

    # send request and process outputs
    LOGGER.info(f'Getting languages available on api_version={api_version}')
    resp = requests.get(url)
    status_code = resp.status_code
    if _is_response_valid(status_code):
        return resp.json(), status_code

    LOGGER.error('Failed to get languages')
    return resp.text, status_code

# /translate endpoint
def translate_text(text: Union[str, List[str]], target_language: Union[str, List[str]], source_language: Optional[str] = None, api_version: str = '3.0') -> tuple:
    """translates txt using the microsoft translate API

    :param text: text to be translated. Either single or multiple (stored in a list)
    :param target_language: ISO format of target translation languages
    :param source_language: ISO format of source language. If not provided is inferred by the translator, defaults to None
    :param api_version: api version to use, defaults to "3.0"
    :return: for successful response, (status_code, [{"translations": [{"text": translated_text_1, "to": lang_1}, ...]}, ...]))        
    """
    # send request and process outputs
    url = f'{MICROSOFT_TRANSLATE_URL}/translate?api-version={api_version}'

    # standardise target language type
    if isinstance(target_language, str):
        target_language = [target_language]

    # dynamically add array parameter to url
    for lang in target_language:
        url = f'{url}&to={lang}'

    if source_language:
        url = f'{url}&from={source_language}'

    # standardise text type
    if isinstance(text, str):
        text = [text]

    # dynamically build the request body
    body = [{'text': text_} for text_ in text]

    LOGGER.info(f'Translating {len(text)} texts to {len(target_language)} languages')
    # send request and process outputs
    resp = requests.post(url, headers=HEADERS, json=body)
    status_code = resp.status_code

    if _is_response_valid(status_code)
        return resp.json(), status_code
    LOGGER.error('Failed to translate texts')
    return resp.text, status_code
```

# 使用 Jupyter Notebook 的注意事项

使用 Jupyter Notebook 时，仅在终端设置环境变量是不够的，因为 Jupyter 默认无法看到它们。相反，以下是我推荐的做法：

+   在终端设置环境变量时，附加“_jupyter”，然后运行`jupyter notebook`

```py
export MICROSOFT_API_CREDENTIALS_JUPYTER='my_key'
jupyter notebook
```

+   使用 `dot_env` 包（你可能需要通过 `pip` 安装）来通过读取“_jupyter”环境变量来设置正确的环境变量。添加 `%%capture` 魔法命令以确保环境变量不会被打印。

```py
%%capture
import os
import json
from dotenv import load_dotenv
load_dotenv() # loads key values pairs into env
MICROSOFT_TRANSLATE_API_KEY = os.environ.get('MICROSOFT_TRANSLATE_API_KEY_JUPYTER')
%set_env MICROSOFT_TRANSLATE_API_KEY=$MICROSOFT_TRANSLATE_API_KEY
```

你现在应该能够在 Jupyter Notebooks 中用 Microsoft 认证你的请求。

# 结论

在本文中，我们详细讲解了如何在 Azure 上设置 Microsoft Translator 实例，并将其集成到项目中，使用最佳实践。

值得一提的是，虽然免费版非常好，但它有资源限制（每月 200 万字符）。虽然看起来很多，但很快就会用完。我最近在一个项目中使用 Translate API 进行数据增强时遇到了这个问题。此外，每个翻译请求有 50000 字符的限制，这意味着在翻译较大文本时需要非常小心。请求计算方式为：**总字符数 * 语言数**。所以在处理较大文本时，分语言或按语言批量翻译更为合理。

我将发布一个关于使用 Microsoft API 的高级指南，其中会介绍用于自动批处理文本的函数，以便你能充分利用最大字符限制。在此之前，你可以在这里找到本文的代码：

[## ml-utils/microsoft.py at develop · namiyousef/ml-utils](https://github.com/namiyousef/ml-utils/blob/develop/mlutils/external_apis/microsoft.py?source=post_page-----89bad979028e--------------------------------#L213)

### 有用的 ML 工具函数。通过在 GitHub 上创建一个账户来贡献于 namiyousef/ml-utils 的开发。

[github.com](https://github.com/namiyousef/ml-utils/blob/develop/mlutils/external_apis/microsoft.py?source=post_page-----89bad979028e--------------------------------#L213)

## 作者注释

如果你喜欢这篇文章或学到了新东西，请考虑通过我的推荐链接获取会员：

[## 加入 Medium 使用我的推荐链接 — Yousef Nami](https://namiyousef96.medium.com/membership?source=post_page-----89bad979028e--------------------------------)

### 阅读 Youssef Nami（以及 Medium 上的其他成千上万的作家）的每个故事。你的会员费将直接支持……

[namiyousef96.medium.com](https://namiyousef96.medium.com/membership?source=post_page-----89bad979028e--------------------------------)

这将使你可以无限制地访问 Medium，同时帮助我在不额外增加你的费用的情况下制作更多内容。

# 参考文献

[1] Microsoft Translator. 可从 [`www.google.com/search?q=microsoft+translator&oq=microsoft+translator&aqs=chrome.0.35i39j69i59l2j0i512l2j69i60l3.2307j0j7&sourceid=chrome&ie=UTF-8`](https://www.google.com/search?q=microsoft+translator&oq=microsoft+translator&aqs=chrome.0.35i39j69i59l2j0i512l2j69i60l3.2307j0j7&sourceid=chrome&ie=UTF-8) 获取

*所有图片由作者提供，除非另有说明*
