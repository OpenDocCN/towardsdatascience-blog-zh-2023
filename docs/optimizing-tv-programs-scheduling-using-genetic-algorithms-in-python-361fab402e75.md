# 使用遗传算法在 Python 中优化电视节目调度

> 原文：[https://towardsdatascience.com/optimizing-tv-programs-scheduling-using-genetic-algorithms-in-python-361fab402e75?source=collection_archive---------6-----------------------#2023-07-26](https://towardsdatascience.com/optimizing-tv-programs-scheduling-using-genetic-algorithms-in-python-361fab402e75?source=collection_archive---------6-----------------------#2023-07-26)

## 一个实践教程，讲解如何在 Python 中使用遗传算法优化电视节目调度

[](https://medium.com/@esersaygin?source=post_page-----361fab402e75--------------------------------)[![Eser Saygın](../Images/4ace0a0f71f2887b4cc70f5deab0bc69.png)](https://medium.com/@esersaygin?source=post_page-----361fab402e75--------------------------------)[](https://towardsdatascience.com/?source=post_page-----361fab402e75--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----361fab402e75--------------------------------) [Eser Saygın](https://medium.com/@esersaygin?source=post_page-----361fab402e75--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc6c2253ada5f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimizing-tv-programs-scheduling-using-genetic-algorithms-in-python-361fab402e75&user=Eser+Sayg%C4%B1n&userId=c6c2253ada5f&source=post_page-c6c2253ada5f----361fab402e75---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----361fab402e75--------------------------------) ·11 分钟阅读·2023年7月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F361fab402e75&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimizing-tv-programs-scheduling-using-genetic-algorithms-in-python-361fab402e75&user=Eser+Sayg%C4%B1n&userId=c6c2253ada5f&source=-----361fab402e75---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F361fab402e75&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimizing-tv-programs-scheduling-using-genetic-algorithms-in-python-361fab402e75&source=-----361fab402e75---------------------bookmark_footer-----------)![](../Images/19841fafc7ef4dd243752825dcddad0e.png)

图片来源：[Glenn Carstens-Peters](https://unsplash.com/@glenncarstenspeters?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我已经很久没有在 Medium 上写新帖子了。在过去的两年里，我一直在研究通过机器学习和深度学习对传统媒体行业可以进行哪些改进。其中一个研究领域是优化技术。就像在每个行业一样，优化在媒体中也至关重要。因此，在这篇文章中，我想通过将电视节目规划整合到一种进化算法——遗传算法中来分享。请记住，这只是一个简单的实现。现实生活远比这种简单性复杂得多。

## 什么是优化？我们为什么需要优化？

什么是优化？我想从这个问题开始。优化是寻找使给定目标函数最小化或最大化的值。那目标函数是什么？目标函数是我们试图最大化或最小化的绩效度量的数学表示。如果问题是最小化问题，我们可以称之为成本函数；如果是最大化问题，我们可以称之为适应度函数。

让我们通过一个例子来丰富这个解释。停下来想象一下你拥有一家餐馆。我们的目标是通过修改菜单（菜单指的是菜单上的菜肴）来最大化其利润。首先想到的方法是使用更便宜的原料。通过降低所用原料的质量，你可以获得更多的利润。但这并不是现实中的运作方式。当你降低产品质量时，顾客会减少对低质量原料制作的菜肴的需求。因此，无法实现预期的目标。

正如你所理解的，餐馆老板创建一个菜单以最大化利润是一个优化问题。例如，餐馆老板可以分析哪些菜肴在什么时间销售，并制定一份路线图。优化技术将使餐馆老板能够做出基于数据的决策，并取得最佳结果。

现在想象你作为一个电视台的节目规划师。记住你的竞争对手很强，但你仍然有能够竞争的节目。主要需要决定的问题是哪些节目应该在什么时间播出。这看起来很简单。笔和一些纸就足够了。真的那么简单吗？

尽管电视节目规划看起来简单，但随着各种因素的参与，它变得非常复杂。以下是其中一些因素：

**观众偏好：** 观众偏好什么类型的电视内容？

**时间段：** 观众在什么时间段偏好什么样的节目？

**引导节目和过渡节目：** 一些节目会将它们在播出期间收集到的观众转移到下一个节目。

**竞争频道的节目偏好：** 竞争频道在什么时间播出什么节目？

**假期、特殊场合和季节性趋势：** 观众的偏好是如何变化的？是否存在任何现有趋势？

**新内容与旧内容：** 广播节目是新的吗？还是重播？

**故事情节和悬念：** 这个程序有故事情节吗？还是有悬念？

这些只是一些因素。你可能已经注意到，数十个因素会影响电视节目规划。因此，优化算法来解决这些问题是合适的。

## 什么是评价和遗传算法？

我将在这一部分简要介绍评价和遗传算法。进化算法（EAs）是一种优化技术，可以解决许多具有挑战性的优化问题，而无需特定的关于问题结构的知识；换句话说，它们是问题独立的。进化算法（EAs）可以处理线性和非线性目标函数，而无需关于问题结构的信息。

另一方面，遗传算法属于搜索算法家族，使用进化原理。通过实现繁殖和自然选择过程，它可以产生高质量的解决方案。遗传算法是解决优化问题的非常有效的技术。

你可以看到下面的简单遗传算法流程图。我们的第一步是创建初始种群。初始种群包含随机选择的染色体（更准确地说，初始种群是一组染色体）。创建种群后，为每个个体计算一个适应度函数值。遗传算法使用染色体来表示每个个体。每个个体的适应度值是独立的。在这种方式下，可以同时进行多次计算。适应度值计算完毕后，遗传算法的三个不同阶段就会开始发挥作用——选择、交叉和变异。选择阶段负责从种群中选择染色体。目标是创造更好的世代。交叉过程负责从选择的个体中产生新的后代。这个过程通常是通过一次选择两个个体，然后交换它们的染色体部分，来创建两个新的代表后代的染色体。最后，操作员在变异阶段改变一个或多个基因。这种变化的概率非常低。变异阶段最重要的特点是防止系统陷入局部最小值。

![](../Images/53d1dc20c4d6497ffabace6a82586816.png)

遗传算法流程图（Eser Saygın）

## 实施

我刚刚介绍了关于遗传算法的一些简单信息。现在我将使用 Python 逐步解释遗传算法。正如标题所示，我们的问题是哪个节目将在什么时间播出。首先，我应该强调一个重要的点。我们将要实现的问题是一个简单的示例。正如我提到的，许多因素影响了现实生活中问题的实现。因此，问题识别阶段是最耗时的部分。

## 步骤

首先，我们从定义数据集开始。正如我之前提到的，下面的集合是一个简单的示例。数据集显示了各种节目在18小时（06:00–24:00）内的收视率。在现实生活中，有必要在每个时间段内播出节目，以测量节目在不同时间段的收视率。

```py
# Sample rating programs dataset for each time slot.
ratings = {
    'news': [0.1, 0.1, 0.4, 0.3, 0.5, 0.4, 0.3, 0.2, 0.1, 0.2, 0.3, 0.5, 0.5, 0.4, 0.3, 0.2, 0.1, 0.2],
    'live_soccer': [0.0, 0.0, 0.0, 0.2, 0.1, 0.3, 0.2, 0.1, 0.4, 0.3, 0.4, 0.5, 0.4, 0.6, 0.4, 0.3, 0.4, 0.3],
    'movie_a': [0.1, 0.1, 0.2, 0.4, 0.3, 0.2, 0.1, 0.2, 0.3, 0.4, 0.5, 0.4, 0.3, 0.4, 0.3, 0.5, 0.3, 0.4],
    'movie_b': [0.2, 0.1, 0.1, 0.3, 0.2, 0.1, 0.2, 0.3, 0.4, 0.5, 0.4, 0.3, 0.4, 0.5, 0.4, 0.3, 0.4, 0.5],
    'reality_show': [0.3, 0.4, 0.3, 0.4, 0.4, 0.5, 0.3, 0.4, 0.5, 0.4, 0.3, 0.2, 0.1, 0.2, 0.3, 0.2, 0.2, 0.3],
    'tv_series_a': [0.2, 0.3, 0.2, 0.1, 0.1, 0.2, 0.2, 0.4, 0.4, 0.3, 0.3, 0.3, 0.5, 0.6, 0.4, 0.5, 0.4, 0.3],
    'tv_series_b': [0.1, 0.2, 0.3, 0.3, 0.2, 0.3, 0.3, 0.1, 0.4, 0.3, 0.4, 0.3, 0.5, 0.3, 0.4, 0.6, 0.4, 0.3],
    'music_program': [0.3, 0.3, 0.3, 0.2, 0.2, 0.1, 0.2, 0.4, 0.3, 0.3, 0.3, 0.3, 0.2, 0.3, 0.2, 0.3, 0.5, 0.3],
    'documentary': [0.3, 0.3, 0.4, 0.3, 0.2, 0.2, 0.3, 0.4, 0.4, 0.3, 0.2, 0.2, 0.2, 0.1, 0.1, 0.3, 0.3, 0.2],
    'Boxing': [0.4, 0.3, 0.3, 0.2, 0.2, 0.1, 0.1, 0.1, 0.1, 0.3, 0.3, 0.3, 0.2, 0.3, 0.4, 0.3, 0.4, 0.6]
}
```

下面，你可以看到其他变量。这些变量是用于遗传算法的超参数。我还创建了两个不同的列表以供后续使用。

```py
GEN = 100
POP = 50
CO_R = 0.8
MUT_R = 0.2
EL_S = 2

all_programs = list(ratings.keys()) # all programs
all_time_slots = list(range(6, 24)) # time slots
```

正如我们在文章中提到的，我们的首要任务是初始化种群。你可以在下面找到我为此目的创建的函数。正如你所见，该函数需要两个输入列表：一个节目列表和一个时间段列表。我们已经在上面定义了这些列表。该函数生成所有潜在的日程安排。

```py
def initialize_pop(programs, time_slots):
    if not programs:
        return [[]]

    all_schedules = []
    for i in range(len(programs)):
        for schedule in initialize_pop(programs[:i] + programs[i + 1:], time_slots):
            all_schedules.append([programs[i]] + schedule)

    return all_schedules
```

接下来，我们将定义我们的适应度函数。适应度函数负责测量每个日程安排的质量。它以日程安排为输入，并返回总的收视率分数。（我们称之为日程安排的列表是由电视节目组成的播出日程。）

```py
def fitness_function(schedule):
    total_rating = 0
    for time_slot, program in enumerate(schedule):
        total_rating += ratings[program][time_slot]
    return total_rating
```

在定义了适应度函数后，我们可以进入选择阶段。选择阶段的目的是找到最优的日程安排。为此，我们可以使用我们创建的以下函数。该函数检查每个日程安排的适应度值，并选择具有最高值的那个。

```py
def finding_best_schedule(all_schedules):
    best_schedule = []
    max_ratings = 0

    for schedule in all_schedules:
        total_ratings = fitness_function(schedule)
        if total_ratings > max_ratings:
            max_ratings = total_ratings
            best_schedule = schedule

    return best_schedule
```

选择阶段之后是交叉阶段。在交叉阶段，两个父代解在遗传算法的帮助下被结合形成新的后代。在电视节目安排问题中，这个过程改变了两个解决方案中找到的节目（基因）。这个过程创造了各种电视节目的组合。你可以在下面看到交叉函数。

```py
def crossover(schedule1, schedule2):
    crossover_point = random.randint(1, len(schedule1) - 2)
    child1 = schedule1[:crossover_point] + schedule2[crossover_point:]
    child2 = schedule2[:crossover_point] + schedule1[crossover_point:]
    return child1, child2
```

最终阶段是突变阶段。正如我们之前提到的，在突变阶段，通过改变染色体的遗传物质来形成新的后代。在电视节目优化问题中，我们可以将其视为随机更改节目。记住，突变的概率非常低。此外，你还可以将这个可能性作为一个超参数进行分配。

```py
def mutate(schedule):
    mutation_point = random.randint(0, len(schedule) - 1)
    new_program = random.choice(all_programs)
    schedule[mutation_point] = new_program
    return schedule
```

现在我们已经定义了所有函数，可以运行适应度函数了。

```py
# calling the fitness func.
def evaluate_fitness(schedule):
    return fitness_function(schedule)
```

我们需要的数据已经准备好。现在我们可以定义算法。这个算法将使用initial_schedule、generations、population_size、crossover_rate、mutation_rate和elitism_size。我们之前已经描述过这些。由于它们是超参数，我们可以修改它们，但不需要。函数首先创建具有提供的初始计划的初始种群，然后添加随机计划。之后，它会对指定的代数运行一个循环，并为每一代生成一个新种群，使用选择、交叉和突变操作。精英策略有助于根据适应度评分保留上一代中最成功的个体。一旦种群更新，它就成为下一代的当前种群。之后，函数返回上一代的最佳计划。

```py
def genetic_algorithm(initial_schedule, generations=GEN, population_size=POP, crossover_rate=CO_R, mutation_rate=MUT_R, elitism_size=EL_S):

    population = [initial_schedule]

    for _ in range(population_size - 1):
        random_schedule = initial_schedule.copy()
        random.shuffle(random_schedule)
        population.append(random_schedule)

    for generation in range(generations):
        new_population = []

        # Elitism
        population.sort(key=lambda schedule: fitness_function(schedule), reverse=True)
        new_population.extend(population[:elitism_size])

        while len(new_population) < population_size:
            parent1, parent2 = random.choices(population, k=2)
            if random.random() < crossover_rate:
                child1, child2 = crossover(parent1, parent2)
            else:
                child1, child2 = parent1.copy(), parent2.copy()

            if random.random() < mutation_rate:
                child1 = mutate(child1)
            if random.random() < mutation_rate:
                child2 = mutate(child2)

            new_population.extend([child1, child2])

        population = new_population

    return population[0]
```

现在我们准备好获取结果。

```py
initial_best_schedule = finding_best_schedule(all_possible_schedules)

rem_t_slots = len(all_time_slots) - len(initial_best_schedule)
genetic_schedule = genetic_algorithm(initial_best_schedule, generations=GEN, population_size=POP, elitism_size=EL_S)

final_schedule = initial_best_schedule + genetic_schedule[:rem_t_slots]

print("\nFinal Optimal Schedule:")
for time_slot, program in enumerate(final_schedule):
    print(f"Time Slot {all_time_slots[time_slot]:02d}:00 - Program {program}")

print("Total Ratings:", fitness_function(final_schedule))
```

在遗传算法运行后，我们将初始最佳计划和遗传计划结合起来，创建最终的*最优计划*。最后，我们打印出分配程序的*最优计划*，显示时间段、相应的程序以及在最终*最优计划*中获得的总评分。

![](../Images/e213f831e6ba9becc21c0c0b615c7ac8.png)

## 结论

节目规划对传统媒体行业的电视台至关重要，在竞争激烈的环境中尤其如此。在这种情况下，我们展示了如何利用遗传算法来改进电视节目排期，这是一个强大的工具，可以帮助最大化观众评分。考虑使用遗传算法来优化排程问题，比如电视节目排期。凭借其强大的能力，它可以帮助你创建一个最大化观众参与和评分的计划。

在我即将发布的文章中，我计划探讨各种遗传算法，如竞争性协同进化（CCQGA）和量子算法（QGA）。我可能还会在中间加入额外的内容。

感谢你抽出时间阅读这篇文章。如果你想与我联系，欢迎通过LinkedIn添加我。

[https://www.linkedin.com/in/esersaygin/](https://www.linkedin.com/in/esersaygin/)

## 来源

> 《动手实践遗传算法与Python：应用遗传算法解决现实世界的深度学习和人工智能问题》 **作者：埃亚尔·维尔桑斯基**
> 
> 《使用Python的工程师应用进化算法（第1版）》
> 
> **作者：莱昂纳多·阿泽维多·斯卡尔杜亚**

## 完整代码

```py
import random

##################################### DEFINING PARAMETERS AND DATASET ################################################################
# Sample rating programs dataset for each time slot.
ratings = {
    'news': [0.1, 0.1, 0.4, 0.3, 0.5, 0.4, 0.3, 0.2, 0.1, 0.2, 0.3, 0.5, 0.5, 0.4, 0.3, 0.2, 0.1, 0.2],
    'live_soccer': [0.0, 0.0, 0.0, 0.2, 0.1, 0.3, 0.2, 0.1, 0.4, 0.3, 0.4, 0.5, 0.4, 0.6, 0.4, 0.3, 0.4, 0.3],
    'movie_a': [0.1, 0.1, 0.2, 0.4, 0.3, 0.2, 0.1, 0.2, 0.3, 0.4, 0.5, 0.4, 0.3, 0.4, 0.3, 0.5, 0.3, 0.4],
    'movie_b': [0.2, 0.1, 0.1, 0.3, 0.2, 0.1, 0.2, 0.3, 0.4, 0.5, 0.4, 0.3, 0.4, 0.5, 0.4, 0.3, 0.4, 0.5],
    'reality_show': [0.3, 0.4, 0.3, 0.4, 0.4, 0.5, 0.3, 0.4, 0.5, 0.4, 0.3, 0.2, 0.1, 0.2, 0.3, 0.2, 0.2, 0.3],
    'tv_series_a': [0.2, 0.3, 0.2, 0.1, 0.1, 0.2, 0.2, 0.4, 0.4, 0.3, 0.3, 0.3, 0.5, 0.6, 0.4, 0.5, 0.4, 0.3],
    'tv_series_b': [0.1, 0.2, 0.3, 0.3, 0.2, 0.3, 0.3, 0.1, 0.4, 0.3, 0.4, 0.3, 0.5, 0.3, 0.4, 0.6, 0.4, 0.3],
    'music_program': [0.3, 0.3, 0.3, 0.2, 0.2, 0.1, 0.2, 0.4, 0.3, 0.3, 0.3, 0.3, 0.2, 0.3, 0.2, 0.3, 0.5, 0.3],
    'documentary': [0.3, 0.3, 0.4, 0.3, 0.2, 0.2, 0.3, 0.4, 0.4, 0.3, 0.2, 0.2, 0.2, 0.1, 0.1, 0.3, 0.3, 0.2],
    'Boxing': [0.4, 0.3, 0.3, 0.2, 0.2, 0.1, 0.1, 0.1, 0.1, 0.3, 0.3, 0.3, 0.2, 0.3, 0.4, 0.3, 0.4, 0.6]
}

GEN = 100
POP = 50
CO_R = 0.8
MUT_R = 0.2
EL_S = 2

all_programs = list(ratings.keys()) # all programs
all_time_slots = list(range(6, 24)) # time slots

######################################### DEFINING FUNCTIONS ########################################################################
# defining fitness function
def fitness_function(schedule):
    total_rating = 0
    for time_slot, program in enumerate(schedule):
        total_rating += ratings[program][time_slot]
    return total_rating

# initializing the population
def initialize_pop(programs, time_slots):
    if not programs:
        return [[]]

    all_schedules = []
    for i in range(len(programs)):
        for schedule in initialize_pop(programs[:i] + programs[i + 1:], time_slots):
            all_schedules.append([programs[i]] + schedule)

    return all_schedules

# selection
def finding_best_schedule(all_schedules):
    best_schedule = []
    max_ratings = 0

    for schedule in all_schedules:
        total_ratings = fitness_function(schedule)
        if total_ratings > max_ratings:
            max_ratings = total_ratings
            best_schedule = schedule

    return best_schedule

# calling the pop func.
all_possible_schedules = initialize_pop(all_programs, all_time_slots)

# callin the schedule func.
best_schedule = finding_best_schedule(all_possible_schedules)

############################################# GENETIC ALGORITHM #############################################################################

# Crossover 
def crossover(schedule1, schedule2):
    crossover_point = random.randint(1, len(schedule1) - 2)
    child1 = schedule1[:crossover_point] + schedule2[crossover_point:]
    child2 = schedule2[:crossover_point] + schedule1[crossover_point:]
    return child1, child2

# mutating
def mutate(schedule):
    mutation_point = random.randint(0, len(schedule) - 1)
    new_program = random.choice(all_programs)
    schedule[mutation_point] = new_program
    return schedule

# calling the fitness func.
def evaluate_fitness(schedule):
    return fitness_function(schedule)

# genetic algorithms with parameters

def genetic_algorithm(initial_schedule, generations=GEN, population_size=POP, crossover_rate=CO_R, mutation_rate=MUT_R, elitism_size=EL_S):

    population = [initial_schedule]

    for _ in range(population_size - 1):
        random_schedule = initial_schedule.copy()
        random.shuffle(random_schedule)
        population.append(random_schedule)

    for generation in range(generations):
        new_population = []

        # Elitsm
        population.sort(key=lambda schedule: fitness_function(schedule), reverse=True)
        new_population.extend(population[:elitism_size])

        while len(new_population) < population_size:
            parent1, parent2 = random.choices(population, k=2)
            if random.random() < crossover_rate:
                child1, child2 = crossover(parent1, parent2)
            else:
                child1, child2 = parent1.copy(), parent2.copy()

            if random.random() < mutation_rate:
                child1 = mutate(child1)
            if random.random() < mutation_rate:
                child2 = mutate(child2)

            new_population.extend([child1, child2])

        population = new_population

    return population[0]

##################################################### RESULTS ###################################################################################

# brute force
initial_best_schedule = finding_best_schedule(all_possible_schedules)

rem_t_slots = len(all_time_slots) - len(initial_best_schedule)
genetic_schedule = genetic_algorithm(initial_best_schedule, generations=GEN, population_size=POP, elitism_size=EL_S)

final_schedule = initial_best_schedule + genetic_schedule[:rem_t_slots]

print("\nFinal Optimal Schedule:")
for time_slot, program in enumerate(final_schedule):
    print(f"Time Slot {all_time_slots[time_slot]:02d}:00 - Program {program}")

print("Total Ratings:", fitness_function(final_schedule))
```
