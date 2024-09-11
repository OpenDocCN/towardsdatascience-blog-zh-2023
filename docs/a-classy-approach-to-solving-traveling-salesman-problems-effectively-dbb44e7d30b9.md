# 一种优雅的方式来有效解决旅行推销员问题

> 原文：[https://towardsdatascience.com/a-classy-approach-to-solving-traveling-salesman-problems-effectively-dbb44e7d30b9?source=collection_archive---------5-----------------------#2023-08-28](https://towardsdatascience.com/a-classy-approach-to-solving-traveling-salesman-problems-effectively-dbb44e7d30b9?source=collection_archive---------5-----------------------#2023-08-28)

## 以类似 scikit-learn 的方式实现 TSP 模型，简化路线优化模型的构建和求解

[](https://medium.com/@carlosjuribe?source=post_page-----dbb44e7d30b9--------------------------------)[![Carlos J. Uribe](../Images/902c5f4ac5d404dd99916f145be6756c.png)](https://medium.com/@carlosjuribe?source=post_page-----dbb44e7d30b9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dbb44e7d30b9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----dbb44e7d30b9--------------------------------) [Carlos J. Uribe](https://medium.com/@carlosjuribe?source=post_page-----dbb44e7d30b9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4337eddb94ed&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-classy-approach-to-solving-traveling-salesman-problems-effectively-dbb44e7d30b9&user=Carlos+J.+Uribe&userId=4337eddb94ed&source=post_page-4337eddb94ed----dbb44e7d30b9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dbb44e7d30b9--------------------------------) ·27分钟阅读·2023年8月28日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdbb44e7d30b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-classy-approach-to-solving-traveling-salesman-problems-effectively-dbb44e7d30b9&source=-----dbb44e7d30b9---------------------bookmark_footer-----------)![](../Images/44dbc85e4fab1ee01aca3f8d40880deb.png)

图片由 DALL·E 3 根据作者的提示生成：“一系列优化路线穿越地球，背景中有 Python 代码”

> 👁️ **这是系列文章的第 #5 篇** **涵盖了项目“**[**Python中的智能旅游决策支持系统**](https://medium.com/@carlosjuribe/list/an-intelligent-decision-support-system-for-tourism-in-python-b6ba165b4236)**”**。我鼓励你查看它，以获得整个项目的一般概述。如果你只是对解决TSP感兴趣，这篇文章也适合你，因为我展示了一种方法，使解决任何TSP变得非常简单。**这篇文章确实建立在之前的文章基础上，但阅读它们是可选的**；如果你想了解“如何”，可以阅读；如果你想尽快得到成果，则可以跳过。

# 目录

## [1. 之前冲刺回顾](#9b5c)

## [2. 读取待访问地点的数据](#fa87)

## [3. 基本架构](#336e)

+   [3.1. 优化器类图](#7b91)

+   [3.2.](#eefc) `[**地理分析器**](#eefc)`[, 回顾](#eefc)

+   [3.3. 打下*基础*：优化工具的基类](#43ef)

## [4. 一个类来解决所有问题：](#c043) `[旅行推销员优化器](#c043)`

+   [4.1.](#127d) `[**旅行推销员优化器**](#127d)` [设计](#127d)

+   [4.2.](#419d) `[**旅行推销员优化器**](#419d)` [实现](#419d)

+   [4.3.](#9ab8) `[**旅行推销员优化器**](#9ab8)` [为初学者](#9ab8)

## [5. 超越最优解：使用优化器提取见解](#754e)

## [6. 结论（或下一次冲刺的计划）](#c3ec)

# 1. 之前冲刺回顾

在 [冲刺 1](https://medium.com/@carlosjuribe/plan-an-optimal-trip-for-your-next-holidays-with-the-help-of-operations-research-and-python-481b1ea38fef) 中，我们通过一个普遍的旅游规划问题进行了推理，并得出其**最小可行问题**为 [**旅行推销员问题** (TSP)](https://en.wikipedia.org/wiki/Travelling_salesman_problem)。因此，我们将 [冲刺 2](https://medium.com/@carlosjuribe/modeling-the-traveling-salesman-problem-from-first-principles-bd6530c9c07) 和 [冲刺 3](https://medium.com/@carlosjuribe/plan-optimal-trips-automatically-with-python-and-operations-research-models-part-2-fc7ee8198b6c) 专门用于开发 TSP 的数学模型和计算机模型。然而，当时的目标只是**展示使用优化建模解决此类问题的可行性**。一旦此概念验证被认为是可行的，新的目标是升级它，将其精炼为一个**最小可行产品** (MVP)，以**系统地和更通用地解决这种*类型*的问题**。我们注意到，作为前提条件，我们需要一种**从任意位置获取距离数据**的方法。我们在 [冲刺 4](https://medium.com/@carlosjuribe/compute-the-distance-matrix-of-a-set-of-sites-from-their-coordinates-in-python-d5fc92a0ba9e) 中解决了这个问题，在那里我们构建了 `GeoAnalyzer` 类来计算任何位置集合的距离矩阵，只需它们的坐标即可。

所有这些迭代进展将我们带到了第五阶段，我们最终达成了一个重大里程碑：创建一个**像估算器一样的类，可以快速直观地解决*通用* TSP**。我们将通过以可扩展的方式集成到目前为止所构建的内容来实现这一目标，从而为未来的增强铺平道路，这些增强将使我们的模型的能力远超 TSP。如果我们要解决现实世界的旅游问题，这是必须做的。也就是说，**旅行推销员问题**——可以说是所有运筹学中最著名的运输问题——**独具一格，因此在本文中，我们将为其提供一个*独特的类***。这个类将隐藏所有低级建模和解决方案解析的细节，从而使**通过方法调用即可无缝解决任何 TSP 问题**。如果你觉得这篇文章缺乏足够的背景或跳跃较大，请阅读准备工作，了解[如何在 Pyomo 中实现 TSP 模型](https://towardsdatascience.com/plan-optimal-trips-automatically-with-python-and-operations-research-models-part-2-fc7ee8198b6c?source=post_page-----dbb44e7d30b9--------------------------------)

[## 使用 Python 实现、解决和可视化旅行推销员问题](https://towardsdatascience.com/plan-optimal-trips-automatically-with-python-and-operations-research-models-part-2-fc7ee8198b6c?source=post_page-----dbb44e7d30b9--------------------------------)

### 学习如何将优化模型从数学转换为 Python，优化它，并可视化解决方案以快速获得结果…

[towardsdatascience.com](https://towardsdatascience.com/plan-optimal-trips-automatically-with-python-and-operations-research-models-part-2-fc7ee8198b6c?source=post_page-----dbb44e7d30b9--------------------------------)

以及[如何以自动方式查找地点之间的距离](https://towardsdatascience.com/compute-the-distance-matrix-of-a-set-of-sites-from-their-coordinates-in-python-d5fc92a0ba9e)

[## 从坐标计算一组站点的距离矩阵](https://towardsdatascience.com/compute-the-distance-matrix-of-a-set-of-sites-from-their-coordinates-in-python-d5fc92a0ba9e?source=post_page-----dbb44e7d30b9--------------------------------)

### 估算任意一对站点之间的距离，作为解决问题的一个步骤…

[## 从坐标计算一组站点的距离矩阵](https://towardsdatascience.com/compute-the-distance-matrix-of-a-set-of-sites-from-their-coordinates-in-python-d5fc92a0ba9e?source=post_page-----dbb44e7d30b9--------------------------------)

你将更深入地理解我们将在这里讨论的想法和代码的来源。如果你已经熟悉 TSP，可以直接跳入！

# 2\. 读取要访问的站点数据

通用 TSP 的基本输入是我们要访问的地点。在我们的示例中，我们有一个巴黎的兴趣点列表，包括我们的酒店。让我们将它们读入数据框：

```py
import pandas as pd

def read_data_sites_to_visit() -> pd.DataFrame:
    """ Reads in a dataframe the locations of the sites to visit """
    DATA_FOLDER = ("https://raw.githubusercontent.com/carlosjuribe/"
                   "traveling-tourist-problem/main/data")
    FILE_LOCATION_HOTEL = "location_hotel.csv"
    FILE_LOCATION_SITES = "sites_coordinates.csv"

    df_sites = pd.concat([
        # coordinates of our hotel, the starting location
        pd.read_csv(f"{DATA_FOLDER}/{FILE_LOCATION_HOTEL}", index_col='site'),
        # coordinates of the actual places we want to visit
        pd.read_csv(f"{DATA_FOLDER}/{FILE_LOCATION_SITES}", index_col='site'), 
    ])
    return df_sites

df_sites = read_data_sites_to_visit()

df_sites
```

![](../Images/a8ac65de957710474d2d97505db2075b.png)

**图 1.** 优化器的输入坐标（作者提供的图片）

# 3\. 基本架构

在我们开始编码之前，重要的是对我们即将做的工作和原因有一个高层次的理解和设计。我们的目标是创建一个**接受某些站点的地理坐标，并为它们解决TSP问题，*即*，输出我们应该访问这些站点的顺序**以最小化总旅行距离。我们不会创建一个完成所有任务的类；我们将把不同的功能保存在不同的类中，然后将它们组合在一起[¹](#0558)。

## 3.1\. 优化器类图

我认为我们的类采用类似scikit-learn的API是便利的。然而，我不会将我们的新类称为“*估算器*”，而是称为“*优化器*”[²](#4124)。一个自解释的名称是 `TravelingSalesmanOptimizer`。它的一个辅助属性将是[第4阶段](https://medium.com/@carlosjuribe/compute-the-distance-matrix-of-a-set-of-sites-from-their-coordinates-in-python-d5fc92a0ba9e)中构建的 `GeoAnalyzer` 类。此外，由于这将不是我们在这个项目中创建的唯一优化器类，我们将把所有与模型求解相关的功能存储在一个单独的类 `BaseOptimizer` 中。原因是所有优化器，无论它们实现什么内部模型，都需要优化它，因此最好将与优化本身相关的逻辑保存在一个单独的类中，作为所有优化器的基类。

在下面的类图中，我们可以看到这三个类如何组合在一起。在每个类内部，我包含了它们的**主要属性和方法**（但不是全部），以便我们能理解整体概念。

![](../Images/e8c714186abc4b21a798483e156f5f3a.png)

**图2。** TSP优化器的类图（作者提供的图像）

每个类的主要目的，简而言之：

+   `BaseOptimizer` 负责优化“适当”的优化器子类的模型，它不打算单独实例化。

+   `GeoAnalyzer` 是一个自包含的地理工具类。它在计算用户提供的坐标的距离矩阵的关键步骤中提供帮助，如果用户没有自定义距离数据，这一步是构建模型所必需的。

+   `TravelingSalesmanOptimizer` 是一个优化器，可以拟合具有站点坐标的数据框。一旦拟合，便可以检索这些站点的“访问顺序”。在内部，它实现了[旅行商问题的数学模型](https://medium.com/@carlosjuribe/plan-optimal-trips-automatically-with-python-and-operations-research-models-part-2-fc7ee8198b6c)作为一个[Pyomo模型](https://pyomo.readthedocs.io/en/stable/pyomo_overview/simple_examples.html)对象。

有了具体的设计，我们来组装它。

## 3.2\. `GeoAnalyzer` 的重新审视

由于`GeoAnalyzer`的代码已在前一次冲刺中开发完成，这里我们只是将其移动到一个新模块`geoutils.py`中，并从那里导入该类。如果你没有阅读[创建它的文章](https://medium.com/@carlosjuribe/compute-the-distance-matrix-of-a-set-of-sites-from-their-coordinates-in-python-d5fc92a0ba9e)，不要担心，你需要知道的是，它接受一些坐标数据框并输出一个距离矩阵，如下所示：

```py
from geoutils import GeoAnalyzer

geo_analyzer = GeoAnalyzer()
geo_analyzer.add_locations(df_sites)
df_distances = geo_analyzer.get_distance_matrix(precision=0)

display(df_distances)
```

![](../Images/4f12661ff8fbe9db8e0db9e08440b218.png)

**图 3.** GeoAnalyzer生成的距离矩阵（图片由作者提供）

这个距离数据框是我们将在`TravelingSalesmanOptimizer`内部用于创建内部模型的，因为建模旅行销售员问题所需的真实数据是距离，而不是坐标。

## 3.3\. 打下基础：优化工具的基类

在[第三次冲刺的文章](http://localhost:8888/lab/workspaces/auto-c/tree/Projects/TWDS/traveling_tourist_problem/TTPOptimizer_part3.ipynb#https://medium.com/@carlosjuribe/plan-optimal-trips-automatically-with-python-and-operations-research-models-part-2-fc7ee8198b6c)中，第一个代码片段是关于实例化Pyomo求解器并打印其版本。稍后在[2.1节](https://medium.com/@carlosjuribe/plan-optimal-trips-automatically-with-python-and-operations-research-models-part-2-fc7ee8198b6c#:~:text=validating%20the%20model-,2.1.%20Solving%20the%20model,-The%20next%20step)中，我们使用了这个求解器来优化模型。考虑到这一点，创建`BaseOptimizer`只需将这些部分组合成一个类即可。我们将稍微修改原始代码，以便更易于阅读，但本质上它是相同的，方便地封装起来。

```py
import sys

import pyomo.environ as pyo

class BaseOptimizer:
    """ Base class for common functionality shared among optimizers, 
    regarding generic handling and solving of optimization models.
    It's not intended to be used by itself but as an abstract class 
    to be extended by actual optimizers that implement concrete Pyomo models.

    Attributes
    ----------
    _solution_exists : bool (default=None)
        Initially None, it takes a boolean value after a (subclass) optimizer 
        has had a fitting attempt: True if an optimal solution was found, False 
        otherwise. If the value is None, it means the optimizer hasn't been fit 
        to any data yet.

    is_fitted : bool
        True if-and-only-if the optimizer has been fitted successfully 
        and thus an optimal solution was found.
    """
    def __init__(self):
        self._solution_exists = None  # updated to True/False inside `_optimize` 
        self._setup_solver()

    #######################    solver setup    #######################
    def _setup_solver(self, solver_nickname="glpk"):
        """ Instantiates and stores a MILP solver as an internal attribute """
        solver = pyo.SolverFactory(solver_nickname)
        if not solver.available(exception_flag=False):
            raise Exception(f"Solver '{solver_nickname}' not found. "
                            "You can install it by running:\n"
                            "conda install -y -c conda-forge glpk")
        self._solver = solver

    def _print_solver_info(self) -> None:
        print("Solver info:", 
              f"name: {self._solver.name}", 
              f"version: {self._solver.version()}", 
              sep="\n - ")

    ######################    model handling    ######################
    def _optimize(self, model: pyo.ConcreteModel) -> bool:
        """ Solve the model. If an optimal solution is found, the model
        provided will have the solution inside and ˋTrueˋ is returned. 
        If an optimal solution isn't found, a warning is printed, the 
        results of the optimization are stored in the ˋ_resultsˋ attribute
        (so post-mortem analysis can be done) and False is returned """
        res = self._solver.solve(model)
        self._results = res  # store output of solver for inspection
        self._solution_exists = pyo.check_optimal_termination(res)

        # _solution_exists is True iff an optimal solution is found
        if not self._solution_exists:
            print("Optimal solution not found, check attribute '_results' "
                  "for details", file=sys.stderr)
        return self._solution_exists

    def _store_model(self, model: pyo.ConcreteModel) -> None:
        """ Stores the Pyomo model as a public attribute """
        self.model = model

    @property
    def is_model_created(self) -> bool:
        """ True if the (sub)class has an attribute named `model` """
        return hasattr(self, 'model')

    @property
    def is_fitted(self) -> bool:
        """ Returns whether the model of the child class has been fitted 
        (i.e., solved) successfully. Returns False otherwise, i.e., 
        if model hasn't been optimized yet, or if an optimal solution 
        wasn't found """
        return bool(self._solution_exists)

    ########################    model inspection    ########################
    def print_model_info(self) -> None:
        """ High-level overview of the number of components 
        (i.e., constraints, variables, etc.) in the model """
        if not self.is_model_created:
            print("No internal model exists yet. Fit me to some data first")
            return

        print(f"Name: {self.model.name}", 
              f"Num variables: {self.model.nvariables()}",
              f"Num constraints: {self.model.nconstraints()}", 
              f"Num objectives: {self.model.nobjectives()}",
              sep="\n- ")
```

关于这个类需要记住的主要事项是：

1.  在实例化时，求解器由`_setup_solver`在内部设置。由于没有求解器我们无法解决任何模型，如果设置失败，将会抛出异常。如果找到了求解器，它会被作为私有属性保存。

1.  每当一个`BaseOptimizer`的子类调用类似`fit`的方法时，方法`_optimize`将会被调用。`_optimize`接受一个模型，并使用内部求解器**尝试**解决它。如果*存在*最优解，属性`_solution_exists`将不再是`None`，而是取值`True`。如果*没有最优解存在*[*³*](#f7e6)，它将取值`False`，并会打印一个警告。

剩余的方法在它们的文档字符串中进行了说明。现在，开始构建我们的第一个优化器。

# 4\. 一个类解决所有问题：`TravelingSalesmanOptimizer`

我们不会从头开始。正如前面所述，我们已经开发了构建TSP的Pyomo模型并在[sprint 3](http://localhost:8888/lab/workspaces/auto-c/tree/Projects/TWDS/traveling_tourist_problem/TTPOptimizer_part3.ipynb#https://medium.com/@carlosjuribe/plan-optimal-trips-automatically-with-python-and-operations-research-models-part-2-fc7ee8198b6c)中解决它的代码。相信我，那是最难的部分。现在，我们的任务是将我们所做的组织成一种通用的方式，隐藏细节同时保留*基本*元素。某种意义上，我们希望优化器看起来像一个“魔法盒子”，即使是对数学建模不熟悉的用户也能直观地解决他们的TSP问题。

## 4.1. `TravelingSalesmanOptimizer`设计

我们的优化器类将包含“核心”方法，处理大部分工作，还有“表面”方法，作为类的高级接口，调用底层的核心方法。

这些步骤将成为优化器逻辑的核心：

1.  **从距离矩阵创建一个Pyomo模型**。这由`_create_model`方法完成，该方法基本上封装了我们已经完成的[概念验证代码](/plan-optimal-trips-automatically-with-python-and-operations-research-models-part-2-fc7ee8198b6c#:~:text=start%20coding%20along!-,1.2.%20Math%20becomes%20code,First%2C%20let%E2%80%99s%20make%20sure%20the%20GLPK%20solver%20is%20findable%20by%20Pyomo,-%23%23%23%20%3D%3D%3D%3D%3D%20%20Code%20block%203.1)。它接受一个距离矩阵的数据框，并基于此构建一个Pyomo模型。我们所做的与我们现在要做的唯一重要区别在于，初始站点不再硬编码为简单的`"hotel"`，而是*假定*为`df_distances`中第一行的站点。一般情况下，**初始站点被认为是坐标数据框**[⁴](#8dd4) `df_sites`中的第一个。这一推广使得优化器能够解决任何实例。

1.  **（尝试）解决模型**。这是在继承自`BaseOptimizer`的`_optimize`方法中执行的，只有找到解决方案时才返回`True`。

1.  **从模型中提取解决方案并解析**成易于理解和使用的形式。这发生在`_store_solution_from_model`方法内部，该方法检查已解决的模型并提取决策变量的值，以及目标函数的值，以*创建*属性`tour_`和`tour_distance_`。此方法*仅在*存在解决方案时*被调用，因此如果未找到解决方案，“解决方案属性”`tour_`和`tour_distance_`将不会被创建。这样做的好处是，在拟合之后，这两个“解决方案属性”的存在将通知用户存在解决方案。此外，变量和目标的最优值可以在任何时刻方便地检索，而不仅仅是在拟合时。

最后的两个步骤——找到并提取解决方案——被封装在最后一个“核心”方法 `_fit_to_distances` 中。

“但请稍等”——你可能会想——“如名字所示，`_fit_to_distances` 需要距离作为输入；我们的目标不是仅使用 *坐标* 来解决 TSP 问题吗，而不是 *距离*？” 是的，这就是 `fit` 方法 *适配* 的地方。我们将 *坐标* 传递给它，并利用 `GeoAnalyzer` 构建距离矩阵，然后由 `_fit_to_distances` 正常处理。这样，如果用户不想自己收集距离，他可以通过使用 `fit` 委派任务。然而，如果他更愿意使用自定义数据，他可以将其组装成 `df_distances` 并传递给 `_fit_to_distances`。

## 4.2. `TravelingSalesmanOptimizer` 实现

让我们按照上面概述的设计逐步构建优化器。首先是一个简化版本，只构建模型并解决它——尚未进行任何解决方案解析。注意 `__repr__` 方法如何在我们打印优化器时让我们知道它包含的站点的名称和数量。

```py
from typing import Tuple, List

class TravelingSalesmanOptimizer(BaseOptimizer):
    """Implements the Miller–Tucker–Zemlin formulation [1] of the 
    Traveling Salesman Problem (TSP) as a linear integer program. 
    The TSP can be stated like: "Given a set of locations (and usually 
    their pair-wise distances), find the tour of minimal distance that 
    traverses all of them exactly once and ends at the same location 
    it started from. For a derivation of the mathematical model, see [2].

    Parameters
    ----------
    name : str
      Optional name to give to a particular TSP instance

    Attributes
    ----------
    tour_ : list
      List of locations sorted in visit order, obtained after fitting.
      To avoid duplicity, the last site in the list is not the initial 
      one, but the last one before closing the tour.

    tour_distance_ : float
      Total distance of the optimal tour, obtained after fitting.

    Example
    --------
    >>> tsp = TravelingSalesmanOptimizer()
    >>> tsp.fit(df_sites)  # fit to a dataframe of geo-coordinates
    >>> tsp.tour_  # list of sites sorted by visit order

    References
    ----------
    [1] https://en.wikipedia.org/wiki/Travelling_salesman_problem
    [2] https://towardsdatascience.com/plan-optimal-trips-automatically-with-python-and-operations-research-models-part-2-fc7ee8198b6c
    """
    def __init__(self, name=""):
        super().__init__()
        self.name = name

    def _create_model(self, df_distances: pd.DataFrame) -> pyo.ConcreteModel:
        """ Given a pandas dataframe of a distance matrix, create a Pyomo model 
        of the TSP and populate it with that distance data """
        model = pyo.ConcreteModel(self.name)

        # a site has to be picked as the "initial" one, doesn't matter which 
        # really; by lack of better criteria, take first site in dataframe 
        # as the initial one
        model.initial_site = df_distances.iloc[0].name

        #===========  sets declaration  ===========#
        list_of_sites = df_distances.index.tolist()

        model.sites = pyo.Set(initialize=list_of_sites, 
                              domain=pyo.Any, 
                              doc="set of all sites to be visited (𝕊)")

        def _rule_domain_arcs(model, i, j):
            """ All possible arcs connecting the sites (𝔸) """
            # only create pair (i, j) if site i and site j are different
            return (i, j) if i != j else None 

        rule = _rule_domain_arcs
        model.valid_arcs = pyo.Set(
            initialize=model.sites * model.sites,  # 𝕊 × 𝕊
            filter=rule, doc=rule.__doc__)

        model.sites_except_initial = pyo.Set(
            initialize=model.sites - {model.initial_site}, 
            domain=model.sites,
            doc="All sites except the initial site"
        )
        #===========  parameters declaration  ===========#
        def _rule_distance_between_sites(model, i, j):
            """ Distance between site i and site j (𝐷𝑖𝑗) """
            return df_distances.at[i, j]  # fetch the distance from dataframe

        rule = _rule_distance_between_sites
        model.distance_ij = pyo.Param(model.valid_arcs, 
                                      initialize=rule, 
                                      doc=rule.__doc__)

        model.M = pyo.Param(initialize=1 - len(model.sites_except_initial),
                            doc="big M to make some constraints redundant")

        #===========  variables declaration  ===========#
        model.delta_ij = pyo.Var(model.valid_arcs, within=pyo.Binary, 
                                 doc="Whether to go from site i to site j (𝛿𝑖𝑗)")

        model.rank_i = pyo.Var(model.sites_except_initial, 
                               within=pyo.NonNegativeReals, 
                               bounds=(1, len(model.sites_except_initial)), 
                               doc=("Rank of each site to track visit order"))

        #===========  objective declaration  ===========#
        def _rule_total_distance_traveled(model):
            """ total distance traveled """
            return pyo.summation(model.distance_ij, model.delta_ij)

        rule = _rule_total_distance_traveled
        model.objective = pyo.Objective(rule=rule, 
                                        sense=pyo.minimize, 
                                        doc=rule.__doc__)

        #===========  constraints declaration  ===========#
        def _rule_site_is_entered_once(model, j):
            """ each site j must be visited from exactly one other site """
            return sum(model.delta_ij[i, j] 
                       for i in model.sites if i != j) == 1

        rule = _rule_site_is_entered_once
        model.constr_each_site_is_entered_once = pyo.Constraint(
                                                  model.sites, 
                                                  rule=rule, 
                                                  doc=rule.__doc__)

        def _rule_site_is_exited_once(model, i):
            """ each site i must departure to exactly one other site """
            return sum(model.delta_ij[i, j] 
                       for j in model.sites if j != i) == 1

        rule = _rule_site_is_exited_once
        model.constr_each_site_is_exited_once = pyo.Constraint(
                                                  model.sites, 
                                                  rule=rule, 
                                                  doc=rule.__doc__)

        def _rule_path_is_single_tour(model, i, j):
            """ For each pair of non-initial sites (i, j), 
            if site j is visited from site i, the rank of j must be 
            strictly greater than the rank of i.
            """
            if i == j:  # if sites coincide, skip creating a constraint
                return pyo.Constraint.Skip

            r_i = model.rank_i[i]
            r_j = model.rank_i[j]
            delta_ij = model.delta_ij[i, j]
            return r_j >= r_i + delta_ij + (1 - delta_ij) * model.M

        # cross product of non-initial sites, to index the constraint
        non_initial_site_pairs = (
            model.sites_except_initial * model.sites_except_initial)

        rule = _rule_path_is_single_tour
        model.constr_path_is_single_tour = pyo.Constraint(
            non_initial_site_pairs,
            rule=rule, 
            doc=rule.__doc__)

        self._store_model(model)  # method inherited from BaseOptimizer
        return model

    def _fit_to_distances(self, df_distances: pd.DataFrame) -> None:
        self._name_index = df_distances.index.name
        model = self._create_model(df_distances)
        solution_exists = self._optimize(model)
        return self

    @property
    def sites(self) -> Tuple[str]:
        """ Return tuple of site names the optimizer considers """
        return self.model.sites.data() if self.is_model_created else ()

    @property
    def num_sites(self) -> int:
        """ Number of locations to visit """
        return len(self.sites)

    @property
    def initial_site(self):
        return self.model.initial_site if self.is_fitted else None

    def __repr__(self) -> str:
        name = f"{self.name}, " if self.name else ''
        return f"{self.__class__.__name__}({name}n={self.num_sites})"
```

让我们快速检查优化器的行为。实例化时，优化器不包含任何站点数量，如表示字符串所示，也没有内部模型，并且当然没有进行适配。

```py
tsp = TravelingSalesmanOptimizer("trial 1")

print(tsp)
#[Out]: TravelingSalesmanOptimizer(trial 1, n=0)
print(tsp.is_model_created, tsp.is_fitted)
#[Out]: (False, False)
```

我们现在将其适配到距离数据上，如果没有收到警告，意味着一切顺利。我们可以看到，现在表示字符串告诉我们我们提供了 9 个站点，存在一个内部模型，并且优化器已经适配到距离数据上：

```py
tsp._fit_to_distances(df_distances)

print(tsp)
#[Out]: TravelingSalesmanOptimizer(trial 1, n=9)
print(tsp.is_model_created, tsp.is_fitted)
#[Out]: (True, True)
```

最优解的发现通过模型的排名决策变量中的明确值得到了证实：

```py
tsp.model.rank_i.get_values()
```

```py
{'Sacre Coeur': 8.0,
 'Louvre': 2.0,
 'Montmartre': 7.0,
 'Port de Suffren': 4.0,
 'Arc de Triomphe': 5.0,
 'Av. Champs Élysées': 6.0,
 'Notre Dame': 1.0,
 'Tour Eiffel': 3.0}
```

这些排名变量表示最优旅行中的停靠点的时间顺序。如果你回忆一下 [它们的定义](/plan-optimal-trips-automatically-with-python-and-operations-research-models-part-2-fc7ee8198b6c#:~:text=rank%20variables%2C%20r%E1%B5%A2%3A%20to%20keep%20track%20of%20the%20order%20in%20which%20sites%20are%20visited%3A)，它们在所有站点上定义，除了初始站点[⁵](#f9a3)，这就是为什么酒店没有出现在其中。很简单，我们可以将酒店添加为排名 0，那么我们就有了 **问题的答案**。我们不需要提取 𝛿ᵢⱼ，即旅行的 *个别* 弧的决策变量，来知道我们应该以什么顺序访问站点。虽然这是真的，我们仍然会使用弧变量 𝛿ᵢⱼ 来从解决的模型中提取确切的停靠顺序。

> *💡* **敏捷 *不必是* 脆弱的**
> 
> *如果我们的唯一目标是解决TSP问题，而不考虑* 扩展 *模型以涵盖我们实际问题的更多细节，使用排名变量来提取最优旅行路径就足够了。然而，由于TSP仅仅是* ***将成为更复杂模型的初始原型****，我们最好从弧决策变量𝛿ᵢⱼ中提取解决方案，因为任何涉及路由决策的模型中都会包含这些变量。其他决策变量是辅助的，当需要时，它们的作用是表示状态或指示依赖于* 真实 *决策变量𝛿ᵢⱼ的条件。正如你将在接下来的文章中看到的，选择排名变量来提取旅行路径适用于纯TSP模型，但对于那些使访问某些站点* 可选 *的扩展则不适用。因此，如果我们从𝛿ᵢⱼ中提取解决方案，* ***我们的方法将是通用和可重复使用的，无论我们使用多么复杂的模型****。*

这种方法的好处将在接下来的文章中显现出来，这些文章中增加了新的要求，因此模型中需要额外的变量。在*为什么*部分讲解完毕后，让我们进入*如何*部分。

**4.2.1 从模型中提取最优旅行路径**

+   **我们有**变量𝛿ᵢⱼ，按可能的弧（i, j）索引，其中𝛿ᵢⱼ=0表示弧未被选择，𝛿ᵢⱼ=1表示弧被选择。

+   **我们想要**一个数据框，其中站点位于索引中（如我们的输入`df_sites`所示），并且停靠点编号在列 `"visit_order"`中指示。

+   **我们编写**一个方法从拟合的优化器中提取这样的数据框。以下是我们将遵循的步骤，每一步都封装在自己的辅助方法中：

1.  从𝛿ᵢⱼ中提取所选的弧，𝛿ᵢⱼ表示旅行路径。完成在`_get_selected_arcs_from_model`中。

1.  将弧（边）列表转换为停靠点（节点）列表。完成在`_get_stops_order_list`中。

1.  将停靠点列表转换为具有一致结构的数据框。完成在`_get_tour_stops_dataframe`中。

由于所选弧是混合的（*即*，不按“遍历顺序”），获得有序的停靠点列表并不那么直接。为了避免复杂的代码，我们利用弧表示*图*的事实，使用图对象`G_tour`按顺序遍历旅行路径节点，从而轻松地得到列表。

```py
import networkx as nx

# class TravelingSalesmanOptimizer(BaseOptimizer):
    # def __init__()
    # def _create_model()
    # def _fit_to_distances()
    # def sites()
    # def num_sites()
    # def initial_site()

    _Arc = Tuple[str, str]

    def _get_selected_arcs_from_model(self, model: pyo.ConcreteModel) -> List[_Arc]:
        """Return the optimal arcs from the decision variable delta_{ij}
        as an unordered list of arcs. Assumes the model has been solved"""
        selected_arcs = [arc 
                         for arc, selected in model.delta_ij.get_values().items()
                         if selected]
        return selected_arcs

    def _extract_solution_as_graph(self, model: pyo.ConcreteModel) -> nx.Graph:
        """Extracts the selected arcs from the decision variables of the model, stores 
        them in a networkX graph and returns such a graph"""
        selected_arcs = self._get_selected_arcs_from_model(model)
        self._G_tour = nx.DiGraph(name=model.name)
        self._G_tour.add_edges_from(selected_arcs)
        return self._G_tour

    def _get_stops_order_list(self) -> List[str]:
        """Return the stops of the tour in a list **ordered** by visit order"""
        visit_order = []
        next_stop = self.initial_site  # by convention...
        visit_order.append(next_stop)  # ...tour starts at initial site

        G_tour = self._extract_solution_as_graph(self.model)
        # starting at first stop, traverse the directed graph one arc at a time
        for _ in G_tour.nodes:
            # get consecutive stop and store it
            next_stop = list(G_tour[next_stop])[0]
            visit_order.append(next_stop)
        # discard last stop in list, as it's repeated (the initial site) 
        return visit_order[:-1]

    def get_tour_stops_dataframe(self) -> pd.DataFrame:
        """Return a dataframe of the stops along the optimal tour"""
        if self.is_fitted:
            ordered_stops = self._get_stops_order_list()
            df_stops = (pd.DataFrame(ordered_stops, columns=[self._name_index])
                          .reset_index(names='visit_order')  # from 0 to N
                          .set_index(self._name_index)  # keep index consistent
            )
            return df_stops

        print("No solution found. Fit me to some data and try again")
```

让我们看看这个新方法给了我们什么：

```py
tsp = TravelingSalesmanOptimizer("trial 2")
tsp._fit_to_distances(df_distances)
tsp.get_tour_stops_dataframe()
```

![](../Images/dc75bd513f652b2d63c68f741e8680e4.png)

**图 4**。解决方案（最优旅行路径）按排名站点（图由作者提供）

`visit_order`列表明我们应该从酒店前往巴黎圣母院，然后到卢浮宫，依此类推，直到最后一个停靠点，蒙马特高地。之后，显然必须返回酒店。很好，现在我们有了一个易于解释和使用的解决方案格式。但停靠点的顺序并不是我们唯一关心的。**目标函数的值也是一个重要的指标**，因为它是指导我们决策的标准。对于我们特别的TSP问题，这意味着获取最优路线的总距离。

**4.2.2 从模型中提取最优目标**

就像我们没有使用排名变量来提取停靠点序列一样，因为在更复杂的模型中，它们的值不会与路线停靠点一致，我们也不会*直接*使用目标函数来获得路线的总距离，尽管在这里这两种测量都是等效的。**在更复杂的模型中，目标函数还会包含其他目标**，因此这种等效性将不再成立。

现在，我们将保持简单，创建一个非公开方法`_get_tour_total_distance`，这明确表示了意图。这个距离来源的细节是隐藏的，并且会依赖于更高级模型关心的特定目标。目前，细节很简单：获取解决模型的目标值。

```py
# class TravelingSalesmanOptimizer(BaseOptimizer):
    # def __init__()
    # def _create_model()
    # def _fit_to_distances()
    # def sites()
    # def num_sites()
    # def initial_site()
    # def _get_selected_arcs_from_model()
    # def _extract_solution_as_graph()
    # def _get_stops_order_list()
    # def get_tour_stops_dataframe()

    def _get_tour_total_distance(self) -> float:
        """Return the total distance of the optimal tour"""
        if self.is_fitted:
            # as the objective is an expression for the total distance, 
            distance_tour = self.model.objective()  # we just get its value
            return distance_tour

        print("Optimizer is not fitted to any data, no optimal objective exists.")
        return None
```

现在看起来可能有些多余，但它将作为一个提醒，告诉我们未来的自己，应该遵循抓取目标值的设计。我们来看看：

```py
tsp = TravelingSalesmanOptimizer("trial 3")
tsp._fit_to_distances(df_distances)
print(f"Total distance: {tsp._get_tour_total_distance()} m")
# [Out]: Total distance: 14931.0 m
```

总距离约为14.9公里。由于最优路线及其距离都很重要，让我们使优化器在每次调用`_fit_to_distances`方法时，将它们一起存储，**并且只有在找到最优解时**。

**4.2.3 将解决方案存储在属性中**

在上面的`_fit_to_distances`实现中，我们只是创建了一个模型并解决了它，没有对模型中存储的解决方案进行任何解析。现在，我们将修改`_fit_to_distances`，**以便当模型解决方案成功时，会创建并提供两个新的属性**，即解决方案的两个相关部分：`tour_`和`tour_distance_`。为了简单起见，`tour_`属性不会返回我们之前创建的数据框，而是返回一个按顺序排列的停靠点列表。新的方法`_store_solution_from_model`负责这个任务。

```py
# class TravelingSalesmanOptimizer(BaseOptimizer):
    # def __init__()
    # def _create_model()
    # def sites()
    # def num_sites()
    # def initial_site()
    # def _get_selected_arcs_from_model()
    # def _extract_solution_as_graph()
    # def _get_stops_order_list()
    # def get_tour_stops_dataframe()
    # def _get_tour_total_distance()

    def _fit_to_distances(self, df_distances: pd.DataFrame):
        """Creates a model of the TSP using the distance matrix 
        provided in `df_distances`, and then optimizes it. 
        If the model has an optimal solution, it is extracted, parsed and 
        stored internally so it can be retrieved.

        Parameters
        ----------
        df_distances : pd.DataFrame
            Pandas dataframe where the indices and columns are the "cities" 
            (or any site of interest) of the problem, and the cells of the 
            dataframe contain the pair-wise distances between the cities, i.e.,
            df_distances.at[i, j] contains the distance between i and j.

        Returns
        -------
        self : object
            Instance of the optimizer.
        """
        model = self._create_model(df_distances)
        solution_exists = self._optimize(model)

        if solution_exists:
            # if a solution wasn't found, the attributes won't exist
            self._store_solution_from_model()

        return self

    #====================  solution handling  ====================
    def _store_solution_from_model(self) -> None:
        """Extract the optimal solution from the model and create the "fitted 
        attributes" `tour_` and `tour_distance_`"""
        self.tour_ = self._get_stops_order_list()
        self.tour_distance_ = self._get_tour_total_distance()
```

让我们再次将优化器拟合到距离数据上，看看现在获取解决方案有多容易：

```py
tsp = TravelingSalesmanOptimizer("trial 4")._fit_to_distances(df_distances)

print(f"Total distance: {tsp.tour_distance_} m")
print(f"Best tour:\n", tsp.tour_)
# [Out]:
# Total distance: 14931.0 m
# Best tour:
# ['hotel', 'Notre Dame', 'Louvre', 'Tour Eiffel', 'Port de Suffren', 'Arc de Triomphe', 'Av. Champs Élysées', 'Montmartre', 'Sacre Coeur']
```

很好。但我们可以做得更好。为了进一步**提高这个类的可用性**，让我们允许用户仅提供站点坐标的数据框来解决问题。由于不是每个人都能为他们感兴趣的站点收集距离矩阵，类可以处理这个问题并提供一个近似的距离矩阵。这在第3.2节中通过`GeoAnalyzer`实现了，这里我们只是将其放在新的`fit`方法下：

```py
# class TravelingSalesmanOptimizer(BaseOptimizer):
    # def __init__()
    # def _create_model()
    # def _fit_to_distances()
    # def sites()
    # def num_sites()
    # def initial_site()
    # def _get_selected_arcs_from_model()
    # def _extract_solution_as_graph()
    # def _get_stops_order_list()
    # def get_tour_stops_dataframe()
    # def _get_tour_total_distance()
    # def _store_solution_from_model()

    def fit(self, df_sites: pd.DataFrame):
        """Creates a model instance of the TSP problem using a 
        distance matrix derived (see notes) from the coordinates provided 
        in `df_sites`.

        Parameters
        ----------
        df_sites : pd.DataFrame
            Dataframe of locations "the salesman" wants to visit, having the 
            names of the locations in the index and at least one column 
            named 'latitude' and one column named 'longitude'.

        Returns
        -------
        self : object
            Instance of the optimizer.

        Notes
        -----
        The distance matrix used is derived from the coordinates of `df_sites`
        using the ellipsoidal distance between any pair of coordinates, as 
        provided by `geopy.distance.distance`."""
        self._validate_data(df_sites)

        self._name_index = df_sites.index.name
        self._geo_analyzer = GeoAnalyzer()
        self._geo_analyzer.add_locations(df_sites)
        df_distances = self._geo_analyzer.get_distance_matrix(precision=0)
        self._fit_to_distances(df_distances)
        return self

    def _validate_data(self, df_sites):
        """Raises error if the input dataframe does not have the expected columns"""
        if not ('latitude' in df_sites and 'longitude' in df_sites):
            raise ValueError("dataframe must have columns 'latitude' and 'longitude'")
```

现在我们达到了我们的目标：**从仅仅是站点位置中找到最佳旅行路线**（而不是像以前那样从距离中找到）：

```py
tsp = TravelingSalesmanOptimizer("trial 5")
tsp.fit(df_sites)

print(f"Total distance: {tsp.tour_distance_} m")
tsp.tour_
#[Out]:
# Total distance: 14931.0 m
# ['hotel',
# 'Notre Dame',
# 'Louvre',
# 'Tour Eiffel',
# 'Port de Suffren',
# 'Arc de Triomphe',
# 'Av. Champs Élysées',
# 'Montmartre',
# 'Sacre Coeur']
```

## 4.3. `TravelingSalesmanOptimizer` 简明指南

恭喜！我们已经达到了优化器非常直观的使用阶段。为了方便起见，我将添加另一种方法，这在我们做[敏感性分析]和比较不同模型的结果时会非常有用。优化器现在会以列表或由`get_tour_stops_dataframe()`返回的单独数据框的形式告诉我最佳的访问顺序，但我希望它能告诉我**通过*转换*我直接提供的位置数据框**的访问顺序——即返回具有最佳停靠顺序的新列的数据框。方法`fit_prescribe`将负责这一点：

```py
# class TravelingSalesmanOptimizer(BaseOptimizer):
    # def __init__()
    # def _create_model()
    # def sites()
    # def num_sites()
    # def initial_site()
    # def _get_selected_arcs_from_model()
    # def _extract_solution_as_graph()
    # def _get_stops_order_list()
    # def get_tour_stops_dataframe()
    # def _get_tour_total_distance()
    # def _fit_to_distances()
    # def _store_solution_from_model()
    # def fit()
    # def _validate_data()

    def fit_prescribe(self, df_sites: pd.DataFrame, sort=True) -> pd.DataFrame:
        """In one line, take in a dataframe of locations and return 
        a copy of it with a new column specifying the optimal visit order
        that minimizes total distance.

        Parameters
        ----------
        df_sites : pd.DataFrame
            Dataframe with the sites in the index and the geolocation 
            information in columns (first column latitude, second longitude).

        sort : bool (default=True)
            Whether to sort the locations by visit order.

        Returns
        -------
        df_sites_ranked : pd.DataFrame 
            Copy of input dataframe `df_sites` with a new column, 'visit_order', 
            containing the stop sequence of the optimal tour.

        See Also
        --------
        fit : Solve a TSP from just site locations.

        Examples
        --------
        >>> tsp = TravelingSalesmanOptimizer()
        >>> df_sites_tour = tsp.fit_prescribe(df_sites)  # solution appended
        """
        self.fit(df_sites)  # find optimal tour for the sites

        if not self.is_fitted:  # unlikely to happen, but still
            raise Exception("A solution could not be found. "
            "Review data or inspect attribute `_results` for details."
            )
        # join input dataframe with column of solution
        df_sites_ranked = df_sites.copy().join(self.get_tour_stops_dataframe())    
        if sort:
            df_sites_ranked.sort_values(by="visit_order", inplace=True)
        return df_sites_ranked
```

现在我们可以用**一行代码**解决任何 TSP 问题：

```py
tsp = TravelingSalesmanOptimizer("Paris")

tsp.fit_prescribe(df_sites)
```

![](../Images/5d2606e413b5c613d32d084a580c17d1.png)

**图 6**。解决方案（最佳旅行路线）附加到站点数据框中（图像来源于作者）

如果我们想保持位置的原始顺序，就像它们在`df_sites`中一样，我们可以通过指定`sort=False`来实现：

```py
tsp.fit_prescribe(df_sites, sort=False)
```

![](../Images/fba80c8ddb8a99700fb200517675ec64.png)

**图 7**。解决方案附加到站点数据框中，站点顺序保持不变（图像来源于作者）

如果我们好奇的话，也可以检查内部模型在解决我们特定的 TSP 实例时所需的变量和约束数量。这在进行调试或性能分析时会很有用。

```py
tsp.print_model_info()
#[Out]:
# Name: Paris
# - Num variables: 80
# - Num constraints: 74
# - Num objectives: 1
```

# 5. 超越最佳解决方案：利用优化器提取见解

在我们结束本文之前，我想给你展示一些简短的例子，说明使用这个类是多么简单，**不仅可以解决 TSP 问题，还能回答规划旅行时通常会出现的一般问题**。

例如，假设你已经制定了一份一天内在巴黎旅行中要访问的站点清单，并将它们放在名为`df_sites`的数据框中。你想知道*仅仅*是遍历它们的最佳旅行路线的总长度。你不需要去*极大地*了解这一点；这行代码就可以解决：

```py
print(f"Distance: "
      f"{TravelingSalesmanOptimizer().fit(df_sites).tour_distance_} m"
)
#[Out]: Distance: 14931.0 m
```

现在假设你对你的站点集合不是很确定，考虑稍微缩减一下。你正在考虑跳过对*凯旋门*的访问，因为它离酒店很远。**你可能会想：“如果我不去这个站点，最佳旅行路线会变化多少？”**。感谢优化器，得到答案非常简单：

```py
site_removed = 'Arc de Triomphe'

df_sites_but_one = df_sites.drop(site_removed, axis=0)
# baseline scenario
tsp_all = TravelingSalesmanOptimizer().fit(df_sites)
# alternative scenario
tsp_all_but_one = TravelingSalesmanOptimizer().fit(df_sites_but_one)

print(
    f"Distance tour ({tsp_all.num_sites} sites): {tsp_all.tour_distance_} m",
    f"Distance tour without {site_removed}: {tsp_all_but_one.tour_distance_} m",
    f"Difference: {tsp_all.tour_distance_ - tsp_all_but_one.tour_distance_} m",
    sep="\n"
)
#[Out]:
# Distance tour (9 sites): 14931.0 m
# Distance tour without Arc de Triomphe: 14162.0 m
# Difference: 768 m
```

通过这个，你可以知道如果你跳过*凯旋门*，你将节省大约 768 米的步行距离。是否值得就由你来决定。

**这不仅仅是关于最佳路线：我们也可以找到“最佳”酒店！**

作为另一个实际示例，假设你已经确定了要访问的景点列表，但还没有选择酒店。你有几个选择，为了更好地决策，**你想知道每个酒店选择对你将要进行的旅行总距离的影响**。然后，这只是**用包含新候选酒店的景点数据框拟合另一个优化器**，**并将最佳距离与使用初始酒店的最佳距离进行比较**：

```py
# make new locations dataframe with the alternative hotel
df_sites_hotel_2 = df_sites.copy()  # sites to visit remain the same
df_sites_hotel_2.loc['hotel'] = (48.828759, 2.329396)  # new hotel coordinates

# solve the TSP for each "hotel scenario"
tsp1 = TravelingSalesmanOptimizer("hotel 1").fit(df_sites)
tsp2 = TravelingSalesmanOptimizer("hotel 2").fit(df_sites_hotel_2)

print(f"Distance tour {tsp1.name}: {tsp1.tour_distance_} m")
print(f"Distance tour {tsp2.name}: {tsp2.tour_distance_} m")
print(f"Difference: {tsp2.tour_distance_ - tsp1.tour_distance_} m")
#[Out]:
# Distance tour hotel 1: 14931 m
# Distance tour hotel 2: 17772 m
# Difference: 2841 m
```

结论是，**如果我们选择“酒店2”，那么我们将比选择“酒店1”多*步行2.8公里*，在穿越相同景点的旅游中**。在其他条件相同的情况下，这告诉我们“酒店1”比“酒店2”更佳选择（*考虑到*我们想参观的景点）。

# 6\. 结论（或计划下一个迭代）

在这篇文章中，我们创建了两个新类（`BaseOptimizer` 和 `TravelingSalesmanOptimizer`）。在未来的迭代中，我们将使用它们、扩展它们，并将更高级的优化器添加到工具包中，所以为了做到干净整洁，让我们将它们移动到一个新模块`routimizers.py`中。

现在，最后一点需要记住的是这个MVP的范围。这个初始优化器给出了“**我应该以什么**顺序访问这些景点？”的问题的答案，但它没有告诉我们*如何从一个景点到下一个景点*。这没关系，因为这正是像Google Maps这样的GIS应用程序的工作，而不是我们谦逊的优化器。

> ***👁*** *我们的优化器解决了* ***战术*** *问题，即告诉我们在旅游中最佳的* 停留顺序，*而不是* ***操作性*** *问题，即引导我们完成这次旅行。后者问题可以通过许多优秀的GIS应用程序轻松解决，例如Google Maps。*

如果你对结果（旅行）感到满意，并且想要在*现实生活中*实际实现它，那么很简单：直接去Google Maps，将这些景点按*优化器指定的顺序*输入为停留点。点击“开始”，你就能得到*操作性*答案。

然而，仍然感觉应该能够*可视化*结果，而不仅仅满足于优化器在新列中放置的数字；即使不是为了实际应用，也应该是为了更好地理解解决方案。在第三次冲刺中，我们确实[稍微可视化了一下解决方案](https://medium.com/@carlosjuribe/plan-optimal-trips-automatically-with-python-and-operations-research-models-part-2-fc7ee8198b6c#:~:text=valuable%20final%20check.-,4.2.%20Plotting%20the%20augmented%20model%E2%80%99s%20solution,-Let%E2%80%99s%20solve%20the)，即使没有坐标。现在我们*确实有*了站点的坐标，我们可以做得更好，以更现实的方式可视化最终的最佳路线。这正是[我们下一次冲刺](/visualizing-routes-on-interactive-maps-with-python-part-1-44f8d25d0761)的目标：**创建出色的视觉效果，以便更好地理解优化器提供的解决方案**（路线），并在此过程中提出和回答更多、更好的问题，从而帮助我们规划更好的行程。拿一杯咖啡，直接开始吧：

[](/visualizing-routes-on-interactive-maps-with-python-part-1-44f8d25d0761?source=post_page-----dbb44e7d30b9--------------------------------) [## 使用Python在互动地图上可视化路线：第一部分

### 一份有关运输问题的互动数据可视化的务实指南，使用Folium

towardsdatascience.com](/visualizing-routes-on-interactive-maps-with-python-part-1-44f8d25d0761?source=post_page-----dbb44e7d30b9--------------------------------)

*脚注*

1.  这种分离的好处将在未来的冲刺中变得明显，特别是针对扩展TSP的问题。[↩](#7894)

1.  因为我们的课程将优化决策，而不是估计参数。两者有微妙的差别。确实，`scikit-learn` 估算器在模型训练期间确实会进行优化，但这种区别仍然很重要，因为 **优化在机器学习和运筹学中的意义不同**。一个 *估算器* 使用优化来 ***估计*** *模型参数* 的未知值，而一个 *优化器* 使用优化来 ***找到*** 代表我们需要做的 *决策* 的最佳值。在估算器中，目标函数始终是 *损失函数*，它衡量 **预测结果与真实结果之间的差异程度**。在优化器中（或更一般地，在运筹学模型中）**目标函数** 可以是任意的，因为它 **是衡量我们所期望的全球状态的标准**，取决于我们的决策。换句话说：*在机器学习中，我们寻找* ***最佳模型参数*** *以产生* ***最佳泛化*** *的数据*。然而，*在运筹学中*，我们的可能行动被表示在模型中，因此 *我们寻找* ***最佳行动*** *以产生* ***最佳结果*** *在我们拥有的资源下*。关键区别在于没有所谓的“真实决策”需要通过优化技术从数据中“估计”出来。**决策是根据数据给出的，而不是从数据中估计的**。[↩](#c61f)

1.  这只能因为 [两个原因](https://en.wikipedia.org/wiki/Linear_programming#:~:text=An%20optimal%20solution%20need,of%20the%20objective%20function.): 模型是 *不可行*（最常见），或者模型是 *无界*（较少见，但可能）。[↩](#97f0)

1.  这就是为什么，在将输入坐标读入数据框时，我们首先读取酒店的坐标。[↩](#d459)

1.  这是一个 [技术要求](https://medium.com/@carlosjuribe/plan-optimal-trips-automatically-with-python-and-operations-research-models-part-2-fc7ee8198b6c#:~:text=It%E2%80%99s%20crucial%20that,variable%3A%20the%20hotel) *准确* 地说，其中一个站点没有相关的排名变量（哪一个无关紧要，但通常选择“初始站点”）。由于酒店在 `df_distances` 的第一行的索引中，因此被认为是初始站点并存储在 Pyomo 模型中（见属性 `tsp.model.initial_site`），没有相关的排名变量。请记住，排名 rᵢ 的工作是 *不是* 指示访问顺序（这在 𝛿ᵢⱼ 中固有），而仅仅是防止形成子巡回。[↩](#7f87)

感谢阅读，下一篇文章见 [这里](/visualizing-routes-on-interactive-maps-with-python-part-1-44f8d25d0761)! 📈😊

随时可以关注我，向我提问，**给我反馈**，或通过 [LinkedIn](https://www.linkedin.com/in/carlosjuribe/) 联系我。
