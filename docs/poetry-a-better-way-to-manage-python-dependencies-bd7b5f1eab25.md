# Python 依赖管理：你应该选择哪个工具？

> 原文：[`towardsdatascience.com/poetry-a-better-way-to-manage-python-dependencies-bd7b5f1eab25`](https://towardsdatascience.com/poetry-a-better-way-to-manage-python-dependencies-bd7b5f1eab25)

## Poetry、Pip 和 Conda 的深入比较

[](https://khuyentran1476.medium.com/?source=post_page-----bd7b5f1eab25--------------------------------)![Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----bd7b5f1eab25--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bd7b5f1eab25--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd7b5f1eab25--------------------------------) [Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----bd7b5f1eab25--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd7b5f1eab25--------------------------------) ·10 分钟阅读·2023 年 6 月 13 日

--

![](img/a87494b4a6efa276955127728c116a55.png)

作者提供的图片

*最初发表于* [*https://mathdatasimplified.com*](https://mathdatasimplified.com/2023/06/12/poetry-a-better-way-to-manage-python-dependencies/) *于 2023 年 6 月 13 日。*

# 动机

随着数据科学项目的扩展，依赖项的数量也会增加。为了保持项目环境的可重现性和可维护性，使用高效的依赖管理工具非常重要。

因此，我决定比较三种流行的依赖管理工具：Pip、Conda 和 Poetry。经过仔细评估，我相信 Poetry 在有效性和性能方面超越了其他两个选项。

在本文中，我们将深入探讨 Poetry 的优势，并突出其与 Pip 和 Conda 的主要区别。

# 可用的包

拥有广泛的包选择使开发人员更容易找到最适合其需求的特定包和版本。

## Conda

一些包，例如 “snscrape”，无法通过 conda 安装。此外，某些版本，例如 Pandas 2.0，可能无法通过 Conda 安装。

虽然你可以在 conda 虚拟环境中使用 pip 解决包的限制，但 conda 无法跟踪用 pip 安装的依赖项，这使得依赖管理变得具有挑战性。

```py
$ conda list
# packages in environment at /Users/khuyentran/miniconda3/envs/test-conda:
#
# Name                    Version                   Build  Channel$ conda list # packages in environment at /Users/khuyentran/miniconda3/envs/test-conda: # # Name Version Build Channel
```

## Pip

Pip 可以从 Python 包索引（PyPI）和其他仓库中安装任何包。

## Poetry

Poetry 还允许从 Python 包索引（PyPI）和其他仓库中安装包。

# 依赖项数量

减少环境中的依赖项数量简化了开发过程。

## Conda

Conda 提供了完整的环境隔离，管理 Python 包和系统级别的依赖项。这可能导致与其他包管理器相比，包的大小更大，在安装和分发过程中可能会消耗更多的存储空间。

```py
$ conda install pandas

$ conda list          

# packages in environment at /Users/khuyentran/miniconda3/envs/test-conda:
#
# Name              Version         Build           Channel             
blas                1.0             openblas                          
bottleneck          1.3.5           py311ha0d4635_0                    
bzip2               1.0.8           h620ffc9_4                        
ca-certificates     2023.05.30      hca03da5_0                        
libcxx              14.0.6          h848a8c0_0                        
libffi              3.4.4           hca03da5_0                        
libgfortran         5.0.0           11_3_0_hca03da5_28                 
libgfortran5        11.3.0          h009349e_28                       
libopenblas         0.3.21          h269037a_0                        
llvm-openmp         14.0.6          hc6e5704_0                        
ncurses             6.4             h313beb8_0                        
numexpr             2.8.4           py311h6dc990b_1                    
numpy               1.24.3          py311hb57d4eb_0                    
numpy-base          1.24.3          py311h1d85a46_0                    
openssl             3.0.8           h1a28f6b_0                        
pandas              1.5.3           py311h6956b77_0                    
pip                 23.0.1          py311hca03da5_0                    
python              3.11.3          hb885b13_1                        
python-dateutil     2.8.2           pyhd3eb1b0_0                      
pytz                2022.7          py311hca03da5_0                    
readline            8.2             h1a28f6b_0                        
setuptools          67.8.0          py311hca03da5_0                    
six                 1.16.0          pyhd3eb1b0_1                      
sqlite              3.41.2          h80987f9_0                        
tk                  8.6.12          hb8d0fd4_0                        
tzdata              2023c           h04d1e81_0                        
wheel               0.38.4          py311hca03da5_0                    
xz                  5.4.2           h80987f9_0                        
zlib                1.2.13          h5a0b063_0 
```

## Pip

Pip 只安装包所需的依赖项。

```py
$ pip install pandas

$ pip list

Package         Version
--------------- -------
numpy           1.24.3
pandas          2.0.2
pip             22.3.1
python-dateutil 2.8.2
pytz            2023.3
setuptools      65.5.0
six             1.16.0
tzdata          2023.3
```

## Poetry

Poetry 也只安装包所需的依赖项。

```py
$ poetry add pandas

$ poetry show

numpy           1.24.3 Fundamental package for array computing in Python
pandas          2.0.2  Powerful data structures for data analysis, time...
python-dateutil 2.8.2  Extensions to the standard Python datetime module
pytz            2023.3 World timezone definitions, modern and historical
six             1.16.0 Python 2 and 3 compatibility utilities
tzdata          2023.3 Provider of IANA time zone data
```

# 卸载包

卸载包及其依赖项可以释放磁盘空间，防止不必要的杂乱，并优化存储资源的使用。

## Pip

Pip 仅移除指定的包，而不是它的依赖项，这可能导致随着时间的推移积累未使用的依赖项。这可能导致存储空间使用增加以及潜在的冲突。

```py
$ pip install pandas

$ pip uninstall pandas

$ pip list

Package         Version
--------------- -------
numpy           1.24.3
pip             22.0.4
python-dateutil 2.8.2
pytz            2023.3
setuptools      56.0.0
six             1.16.0
tzdata          2023.3
```

## Conda

Conda 移除包及其依赖项。

```py
$ conda install -c conda pandas

$ conda uninstall -c conda pandas

Collecting package metadata (repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /Users/khuyentran/miniconda3/envs/test-conda

  removed specs:
    - pandas

The following packages will be REMOVED:

  blas-1.0-openblas
  bottleneck-1.3.5-py311ha0d4635_0
  libcxx-14.0.6-h848a8c0_0
  libgfortran-5.0.0-11_3_0_hca03da5_28
  libgfortran5-11.3.0-h009349e_28
  libopenblas-0.3.21-h269037a_0
  llvm-openmp-14.0.6-hc6e5704_0
  numexpr-2.8.4-py311h6dc990b_1
  numpy-1.24.3-py311hb57d4eb_0
  numpy-base-1.24.3-py311h1d85a46_0
  pandas-1.5.3-py311h6956b77_0
  python-dateutil-2.8.2-pyhd3eb1b0_0
  pytz-2022.7-py311hca03da5_0
  six-1.16.0-pyhd3eb1b0_1

Proceed ([y]/n)? 

Preparing transaction: done
Verifying transaction: done
Executing transaction: donePoetry
```

## Poetry

Poetry 也会移除包及其依赖项。

```py
$ poetry add pandas

$ poetry remove pandas

  • Removing numpy (1.24.3)
  • Removing pandas (2.0.2)
  • Removing python-dateutil (2.8.2)
  • Removing pytz (2023.3)
  • Removing six (1.16.0)
  • Removing tzdata (2023.3)
```

# 依赖文件

依赖文件通过指定所需包的确切版本或版本范围，确保软件项目环境的可重现性。

这有助于在不同系统或不同时间点重新创建相同的环境，确保开发者之间使用相同的依赖项进行协作。

## Conda

要在 Conda 环境中保存依赖项，你需要手动将它们写入文件。`environment.yml` 文件中指定的版本范围可能会导致安装不同的版本，可能在重现环境时引入兼容性问题。

假设我们已经安装了 pandas 版本 1.5.3 作为示例。这里是一个示例 `environment.yml` 文件，指定了依赖项：

```py
# environment.yml
name: test-conda
channels:
  - defaults
dependencies:
  - python=3.8
  - pandas>=1.5
```

如果新用户尝试在 pandas 的最新版本为 2.0 时重现环境，则会安装 pandas 2.0。

```py
# Create and activate a virtual environment
$ conda env create -n env
$ conda activate env

# List packages in the current environment
$ conda list
...
pandas                   2.0
```

如果代码库依赖于 pandas 版本 1.5.3 特有的语法或行为，而在版本 2.0 中语法发生了变化，那么使用 pandas 2.0 运行代码可能会引入错误。

## Pip

相同的问题也可能发生在 pip 上。

```py
# requirements.txt 
pandas>=1.5
```

```py
# Create and activate a virtual environment
$ python3 -m venv venv
$ source venv/bin/activate

# Install dependencies
$ pip install -r requirements.txt

# List packages
$ pip list
Package    Version
---------- -------
pandas       2.0
...
```

你可以通过在 `requirements.txt` 文件中冻结版本来固定版本：

```py
$ pip freeze > requirements.txt
```

```py
# requirements.txt

numpy==1.24.3
pandas==1.5.3
python-dateutil==2.8.2
pytz==2023.3
six==1.16.0
```

然而，这使得代码环境的灵活性降低，并且在长期维护中可能变得更加困难。任何对依赖项的更改都需要手动修改 `requirements.txt` 文件，这可能既耗时又容易出错。

## Poetry

Poetry 在安装包时会自动更新 `pyproject.toml` 文件。

在以下示例中，“pandas” 包被添加了版本约束 `¹.5`。这种灵活的版本管理方法确保了你的项目可以适应更新的版本而无需手动调整。

```py
$ poetry add 'pandas=¹.5'
```

```py
# pyproject.toml

[tool.poetry.dependencies]
python = "³.8"
pandas = "¹.5"
```

`poetry.lock` 文件存储了每个包及其依赖项的精确版本号。

```py
# poetry.lock
...
[[package]]
name = "pandas"
version = "1.5.3"
description = "Powerful data structures for data analysis, time series, and statistics"
category = "main"
optional = false
python-versions = ">=3.8"

[package.dependencies]
numpy = [
    {version = ">=1.20.3", markers = "python_version < \"3.10\""},
    {version = ">=1.21.0", markers = "python_version >= \"3.10\""},
    {version = ">=1.23.2", markers = "python_version >= \"3.11\""},
]
python-dateutil = ">=2.8.2"
pytz = ">=2020.1"
tzdata = ">=2022.1"
...
```

这确保了安装的包的一致性，即使包的 `pyproject.toml` 文件中指定了版本范围。在这里，我们可以看到安装了 pandas 1.5.3 而不是 pandas 2.0

```py
$ poetry install

$ poetry show pandas

name         : pandas                                                                  
version      : 1.5.3                                                                   
description  : Powerful data structures for data analysis, time series, and statistics 

dependencies
 - numpy >=1.20.3
 - numpy >=1.21.0
 - numpy >=1.23.2
 - python-dateutil >=2.8.1
 - pytz >=2020.1
```

# 开发和生产的独立依赖项

通过分离依赖项，你可以清楚地区分开发用途所需的包（例如测试框架和代码质量工具）与生产环境所需的包（通常包括核心依赖项）。

## Conda

Conda 本身不支持为不同环境分别设置依赖项，但可以通过创建两个环境文件来解决这个问题：一个用于开发环境，另一个用于生产环境。开发文件包含生产和开发依赖项。

```py
# environment.yml
name: test-conda
channels:
  - defaults
dependencies:
  # Production packages
  - numpy
  - pandas
```

```py
# environment-dev.yml
name: test-conda-dev
channels:
  - defaults
dependencies:
  # Production packages
  - numpy
  - pandas
  # Development packages
  - pytest
  - pre-commit
```

## Pip

Pip 也不直接支持分开的依赖项，但可以使用类似的方法通过分开的需求文件来实现。

```py
# requirements.txt
numpy 
pandas
```

```py
# requirements-dev.txt
-r requirements.txt
pytest
pre-commit
```

```py
# Install prod
$ pip install -r requirements.txt

# Install both dev and prod
$ pip install -r requirements-dev.txt
```

## Poetry

Poetry 通过支持一个文件中的分组来简化依赖项管理。这使你可以在一个地方跟踪所有依赖项。

```py
$ poetry add numpy pandas
$ poetry add --group dev pytest pre-commit
```

```py
# pyproject.toml
[tool.poetry.dependencies]
python = "³.8"
pandas = "².0"
numpy = "¹.24.3"

[tool.poetry.group.dev.dependencies]
pytest = "⁷.3.2"
pre-commit = "³.3.2"
```

要仅安装生产依赖项：

```py
$ poetry install --only main
```

要安装开发和生产依赖项：

```py
$ poetry install
```

# 更新环境

更新依赖项对于从 bug 修复、性能改进和新包版本中引入的新功能中受益是至关重要的。

## Conda

Conda 允许你只更新指定的包。

```py
$ conda install -c conda pandas
$ conda install -c anaconda scikit-learn
```

```py
# New versions available
$ conda update pandas
$ conda update scikit-learn
```

之后，你需要手动更新 environment.yaml 文件，以保持与更新的依赖项同步。

```py
$ conda env export > environment.yml
```

## Pip

Pip 也只允许你更新指定的包，并要求你手动更新 requirements.txt 文件。

```py
$ pip install -U pandas
```

```py
$ pip freeze > requirements.txt
```

## Poetry

使用 Poetry，你可以使用 `update` 命令来升级 pyproject.toml 文件中指定的所有包。此操作会自动更新 poetry.lock 文件，确保包规范和锁定文件之间的一致性。

```py
$ poetry add pandas scikit-learn

# New verisons available
poetry update

Updating dependencies
Resolving dependencies... (0.3s)

Writing lock file

Package operations: 0 installs, 2 updates, 0 removals

  • Updating pandas (2.0.0 -> 2.0.2)
  • Updating scikit-learn (1.2.0 -> 1.2.2)
```

# 依赖解析

依赖冲突发生在项目所需的包或库具有冲突的版本或不兼容的依赖项时。正确解决冲突对于避免错误、运行时问题或项目失败至关重要。

## Pip

pip 顺序地安装包，这意味着它会按照指定的顺序逐个安装每个包。这种顺序方式有时会导致当包具有不兼容的依赖项或版本要求时发生冲突。

例如，假设你首先安装 `pandas==2.0.2`，它需要 `numpy>=1.20.3`。后来，你使用 pip 安装 `numpy==1.20.2`。尽管这会导致依赖冲突，但 pip 会继续更新 numpy 的版本。

```py
$ pip install pandas==2.0.2

$ pip install numpy==1.22.2
Collecting numpy=1.20.2
  Attempting uninstall: numpy
    Found existing installation: numpy 1.24.3
    Uninstalling numpy-1.24.3:
      Successfully uninstalled numpy-1.24.3
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
pandas 2.0.2 requires numpy>=1.20.3; python_version < "3.10", but you have numpy 1.20.2 which is incompatible.
Successfully installed numpy-1.20.2
```

## Conda

Conda 使用 SAT 求解器来探索所有包版本和依赖项的组合，以找到兼容的集合。

例如，如果一个现有包对其依赖项有特定的约束（例如，statsmodels==0.13.2 需要 numpy>=1.21.2,<2.0a0），而你要安装的包不符合该要求（例如，numpy<1.21.2），conda 不会立即引发错误。相反，它会仔细搜索所有所需包及其依赖项的兼容版本，只有在找不到合适的解决方案时才报告错误。

```py
$ conda install 'statsmodels==0.13.2'

$ conda search 'statsmodels==0.13.2' --info
dependencies: 
  - numpy >=1.21.2,<2.0a0
  - packaging >=21.3
  - pandas >=1.0
  - patsy >=0.5.2
  - python >=3.9,<3.10.0a0
  - scipy >=1.3

$ conda install 'numpy<1.21.2'

...
Package ca-certificates conflicts for:
python=3.8 -> openssl[version='>=1.1.1t,<1.1.2a'] -> ca-certificates
openssl -> ca-certificates
ca-certificates
cryptography -> openssl[version='>1.1.0,<3.1.0'] -> ca-certificates

Package idna conflicts for:
requests -> urllib3[version='>=1.21.1,<1.27'] -> idna[version='>=2.0.0']
requests -> idna[version='>=2.5,<3|>=2.5,<4']
idna
pooch -> requests -> idna[version='>=2.5,<3|>=2.5,<4']
urllib3 -> idna[version='>=2.0.0']

Package numexpr conflicts for:
statsmodels==0.13.2 -> pandas[version='>=1.0'] -> numexpr[version='>=2.7.0|>=2.7.1|>=2.7.3']
numexpr
pandas==1.5.3 -> numexpr[version='>=2.7.3']

Package patsy conflicts for:
statsmodels==0.13.2 -> patsy[version='>=0.5.2']
patsy

Package chardet conflicts for:
requests -> chardet[version='>=3.0.2,<4|>=3.0.2,<5']
pooch -> requests -> chardet[version='>=3.0.2,<4|>=3.0.2,<5']

Package python-dateutil conflicts for:
statsmodels==0.13.2 -> pandas[version='>=1.0'] -> python-dateutil[version='>=2.7.3|>=2.8.1']
python-dateutil
pandas==1.5.3 -> python-dateutil[version='>=2.8.1']

Package setuptools conflicts for:
numexpr -> setuptools
pip -> setuptools
wheel -> setuptools
setuptools
python=3.8 -> pip -> setuptools
pandas==1.5.3 -> numexpr[version='>=2.7.3'] -> setuptools

Package brotlipy conflicts for:
urllib3 -> brotlipy[version='>=0.6.0']
brotlipy
requests -> urllib3[version='>=1.21.1,<1.27'] -> brotlipy[version='>=0.6.0']

Package pytz conflicts for:
pytz
pandas==1.5.3 -> pytz[version='>=2020.1']
statsmodels==0.13.2 -> pandas[version='>=1.0'] -> pytz[version='>=2017.3|>=2020.1']
```

尽管这种方法提高了找到解决方案的机会，但在处理广泛环境时可能会计算密集。

## 诗歌

通过专注于项目的直接依赖，Poetry 的确定性解析器缩小了搜索范围，使得解析过程更为高效。它评估指定的约束条件，如版本范围或特定版本，并立即识别任何冲突。

```py
$ poetry add 'seaborn==0.12.2'
$ poetry add 'matplotlib<3.1' 

Because poetry shell depends on seaborn (0.12.2) which depends on matplotlib (>=3.1,<3.6.1 || >3.6.1), matplotlib is required.
So, because poetry shell depends on matplotlib (<3.1), version solving failed.
```

这种即时反馈有助于防止潜在问题升级，并允许开发者在开发过程早期解决问题。例如，在以下代码中，我们可以放宽对 seaborn 的要求，以便安装特定版本的 matplotlib：

```py
poetry add 'seaborn<=0.12.2'  'matplotlib<3.1' 

Package operations: 1 install, 2 updates, 4 removals

  • Removing contourpy (1.0.7)
  • Removing fonttools (4.40.0)
  • Removing packaging (23.1)
  • Removing pillow (9.5.0)
  • Updating matplotlib (3.7.1 -> 3.0.3)
  • Installing scipy (1.9.3)
  • Updating seaborn (0.12.2 -> 0.11.2)
```

# 结论

总结而言，Poetry 相比 pip 和 conda 提供了几个优势：

1.  **广泛的软件包选择：** Poetry 提供对 PyPI 上广泛软件包的访问，允许你利用多样的生态系统来支持你的项目。

1.  **高效的依赖管理：** Poetry 只安装指定软件包所需的必要依赖，减少了环境中多余的软件包数量。

1.  **简化的软件包移除：** Poetry 简化了软件包及其相关依赖的移除，使得维护一个干净高效的项目环境变得更加容易。

1.  **依赖解决：** Poetry 的确定性解析器高效地解决依赖关系，及时识别和处理任何不一致或冲突。

虽然 Poetry 可能需要一些额外的时间和精力来让你的团队成员学习和适应，但长期来看，使用像 Poetry 这样的工具可以节省时间和精力。

我喜欢写关于数据科学概念的文章，并玩弄各种数据科学工具。你可以通过以下方式跟进我的最新帖子：

+   订阅我在[Data Science Simplified](https://mathdatasimplified.com/)上的新闻通讯。

+   在[LinkedIn](https://www.linkedin.com/in/khuyen-tran-1401/)和[Twitter](https://twitter.com/KhuyenTran16)上与我联系。
