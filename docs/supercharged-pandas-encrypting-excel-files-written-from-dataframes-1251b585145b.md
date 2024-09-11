# 超强 pandas：加密从 DataFrames 写入的 Excel 文件

> 原文：[`towardsdatascience.com/supercharged-pandas-encrypting-excel-files-written-from-dataframes-1251b585145b?source=collection_archive---------7-----------------------#2023-06-12`](https://towardsdatascience.com/supercharged-pandas-encrypting-excel-files-written-from-dataframes-1251b585145b?source=collection_archive---------7-----------------------#2023-06-12)

## 介绍一个 ExcelHelper 类，它允许你使用强密码或自定义密码加密 Excel 文件

[](https://jiweiliew.medium.com/?source=post_page-----1251b585145b--------------------------------)![Ji Wei Liew](https://jiweiliew.medium.com/?source=post_page-----1251b585145b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1251b585145b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1251b585145b--------------------------------) [Ji Wei Liew](https://jiweiliew.medium.com/?source=post_page-----1251b585145b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbf5390a70cc8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsupercharged-pandas-encrypting-excel-files-written-from-dataframes-1251b585145b&user=Ji+Wei+Liew&userId=bf5390a70cc8&source=post_page-bf5390a70cc8----1251b585145b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1251b585145b--------------------------------) ·6 min read·2023 年 6 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1251b585145b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsupercharged-pandas-encrypting-excel-files-written-from-dataframes-1251b585145b&user=Ji+Wei+Liew&userId=bf5390a70cc8&source=-----1251b585145b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1251b585145b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsupercharged-pandas-encrypting-excel-files-written-from-dataframes-1251b585145b&source=-----1251b585145b---------------------bookmark_footer-----------)![](img/48f2ccc861b8edbdc2f949971790e150.png)

将数据写入 Excel 并加密（图片由作者提供）

# 介绍

在这篇文章中，我将分享如何结合使用 `ExcelHelper` 类来在将数据框写入 Excel 后打开并加密 Excel 文件。我在之前的 文章 中已将这一加密功能包含在 `to_excelp` 函数中。

[](/supercharged-pandas-reading-from-and-writing-to-excel-9379f39db4b3?source=post_page-----1251b585145b--------------------------------) ## 超强大的 pandas：从 Excel 中读取和写入

### 增强.read_excel 和.to_excel 方法，以便你可以专注于数据探索

towardsdatascience.com

对于数据科学家和机器学习爱好者来说，你会发现这非常有用，因为它可以加快将数据框导出到 Excel 时的工作速度。

# 动机

在最近的一个项目中，我需要分析数据并为几个人准备统计数据。由于数据包含敏感信息，需要对文件进行密码保护。这与我的技能非常匹配，如果你读过我之前的文章，你会发现，在`pypo.py`中，我已经有一个`to_excelp`函数，该函数在 Excel 文件通过`df.to_excel()`方法创建后打开它。虽然这对我有效，但现在似乎是重新审视实现方式并增加密码保护功能的好时机。

# 内容

1.  打开和加密 Excel 文件

1.  生成强密码

1.  整合所有内容

完整代码 [在这里](https://gist.github.com/jiweiliew/f07e24861ea42fd17caebf8566d8c2ac)。

# 第一部分 — 打开和加密 Excel 文件

在使用 python 和 pandas 时，打开 Excel 文件有很多理由，例如，在测试期间进行视觉检查，或者在发送给利益相关者之前进行格式化。如果有额外的需求来加密 Excel 文件，则需要处理 3 种情况：*仅打开*，*仅加密*，*打开并加密*。如果我们既不打开也不加密 Excel 文件，那么不需要做任何操作，因为`df.to_excel()`就足够了。

`ExcelHelper`是一个用于启动 Excel（应用程序）并根据提供的`path`打开工作簿的类。从程序角度看，这是一个两步过程。大多数人从未意识到这一点，因为当你双击一个 Excel 文件时，Excel 应用程序和工作簿会一起启动。

**初始化 ExcelHelper 类**

+   `__init__(self, launch=True, encrypt=False, password=None, length=20, quit_=True)` 这是`ExcelHelper`的初始化调用。

+   如果`launch`等于`True`，在加密完成后工作簿会被显示出来。

+   如果`encrypt`等于`True`，将调用`self._encrypt()`方法，稍后会进行解释。

+   `password`允许用户输入首选密码，否则将自动建议一个长度为`length`的强密码，最大长度为 255。

**打开工作簿**

+   `_open_wb(self, path, visible=False)` 将给定路径转换为绝对路径，然后打开它。将路径转换为绝对路径是必要的，否则由`win32com.client`调度的应用程序无法找到文件。（之前，我使用了`try-except`块来在路径前添加当前工作目录，但这显得过于冗长，并且需要花费一些时间才能真正理解我们要做的事情。）

+   `visible` 控制应用程序是否对用户可见。通常，只有在加密完成后才显示应用程序才有意义。因此，如果我们正在启动并加密，应该在`self._encrypt()`完成后才将`visible=True`设置为真。

**加密 Excel**

+   `_encrypt(self, visible=False)` 加密 Excel 工作簿，然后在加密完成后通过设置`self.xl.Visible`属性来显示应用程序。

+   将`self.xl.DisplayAlerts`设置为`True`是很重要的，否则启动的 Excel 文件不会发出任何警报（例如，如果你按 Ctrl+F 尝试查找一些无意义的字符而没有任何提示 😱；这曾经发生在我身上，我真的很困惑！）。

**执行方法**

+   `execute(self, path, launch, encrypt, quit_)` 处理上述 3 种情况。

+   `quit_` 参数关闭 Excel 应用程序（尾部下划线是一种约定，表示`quit`在 Python 中是一个保留关键字）。当`ExcelHelper`被初始化时，如果`launch=False`，Excel 应用程序将在后台运行，Excel 文件会被`打开`。如果用户现在双击 Excel 文件，他会被提示只能以只读模式打开。对于非技术用户来说，关闭文件相当困难；解决方法是打开任务管理器，选择 Excel 程序，然后结束任务。因此，需要调用`.Quit()`来终止 Excel 应用程序。我们本可以直接关闭工作簿，但也许现在不需要处理得如此细致。

# 第二部分 — 生成强密码

起初，我使用`from cryptography.fernet import Fernet; Fernet.generate_key()`来生成随机密码。虽然几个用户对密码的长度和随机性感到惊讶，但我不是很喜欢，因为它有点长，并且不包含多样的标点符号。我在 Google 上查找了一个更好的[方法](https://stackoverflow.com/a/39596292/8350440)在 StackOverflow 上。（我总是对 StackOverflow 上如何轻松获得高质量答案感到非常惊讶。所有困难的工作已经由所有前辈完成，我们只需搜索、复制、粘贴并做些小调整（例如更改变量名）。）这个函数相当简单直接，自解释性也很强。

```py
import secrets
import string

def gen_password(self, length):
    char = string.ascii_letters + string.digits + string.punctuation
    return ''.join(secrets.choice(char) for _ in range(length))
```

正当一切进展得过于顺利时，在测试我的代码时，我注意到有时密码无法用来打开文件！我真的很困惑。经过一番试错后，我开始怀疑可能有些字符是不*适合*用作密码的，因为这种现象只在密码包含两个反斜杠 `\\` 时发生。

这里有一些背景信息可以让你更好地理解情况：我使用 Powershell 和 Notepad++，我的代码将密码打印到 `stdout`。接下来，我在 Powershell 上突出显示打印出的密码，然后在 Excel 提示我输入密码时粘贴它。所以问题是 `\` 是一个转义字符，因此在我输入密码时，第一个 `\` 应该被忽略。处理起来很麻烦，对于密码的目的，我可以少用一个字符。因此，我所做的就是在 `string.punctuation` 中切除反斜杠。

```py
 def _get_password(self, length):
      string_punc = string.punctuation[:23] + string.punctuation[24:]
      char = string.ascii_letters + string.digits + string_punc
      return ''.join(secrets.choice(char) for _ in range(length))
```

# 第三部分 — 将一切整合起来

由于如果你不启动或加密 Excel 文件，实例化 `ExcelHelper` 对象几乎没有附加值，因此应该以 `if launch or encrypt:` 开始。接下来，仅需将关键字参数从 `to_excelp` 传递到 `ExcelHelper` 并返回对象和 `password`。

```py
def to_excelp(df, *arg, launch=True, encrypt=False, password=None, **kw):
    ''' Writes to Excel and opens it'''
    filename, *arg = arg

    if not filename.endswith(('.xlsx','.xls','.xlsm')):
        filename += '.xlsx'

    if os.path.isfile(filename):
        name, ext = filename.rsplit('.')
        filename = f'{name}_{timestr()}.{ext}'

    # Default index=False
    index = kw.get('index', False)
    if not index:
        kw['index']=False

    df.to_excel(filename, *arg, **kw)
    if launch or encrypt:
        xl = ExcelHelper(filename, launch=launch, encrypt=encrypt, password=password)
        return xl, xl.password
    else:
        return filename
```

如果你通过调用这个函数将数据框写入多个不同的 Excel 文件，我建议将结果存储在一个元组列表中。你可以随后遍历这个元组列表以获取 Excel 文件的路径及其密码。存储对象可能在未来很有用，特别是如果你打算为 `ExcelHelper` 添加更多功能的话。

```py
l_xl_pw = []

for df in (df1, df2, df3, df4):
  xl, pw = df.to_excelp(launch=False, encrypt=True, password=None)
  l_xl_pw.append((xl, pw))

l_path_pass = [[xl.path, pw] for (xl, pw) in l_xl_pw]
df_path_pass = pd.DataFrame(l_path_pass, columns=['Path', 'Pw'])

# df_path_pass can also be written to Excel using .to_excelp(), how elegant! :D
```

`ExcelHelper` 也可以添加到你现有的其他脚本中。

```py
def some_func():
    df = pd.read_excel('some_file.xlsx')
    # some data manipulation...
    df.to_excel('some_file_modified.xlsx')

def some_func(launch=False, encrypt=True, password='5tr0ngP@ssw0rd'):
    df = pd.read_excel('some_file.xlsx')
    # some data manipulation...
    df.to_excel('some_file_modified.xlsx')
    if launch or encrypt:
        xl = ExcelHelper('some_file_modified.xlsx', launch=launch, encrypt=encrypt, password=password)
        return xl, xl.password
```

# 结论

重新审视自己写的旧代码就像是走在记忆的长廊上，揭示了当时我知道的少。虽然对此感到非常尴尬，但我很高兴知道自己已经有所进步。

> “如果你对自己旧的代码不感到尴尬，那么你作为程序员就没有进步。” [匿名]

编写这些小类和函数需要时间，但它们的好处巨大，因为它们自动化了机械化且不太有趣的工作部分，并使人能够专注于重要任务。（想象一下，总是要考虑包含大写字母、小写字母、数字和标点符号的密码，并将它们存储在文件中。）
