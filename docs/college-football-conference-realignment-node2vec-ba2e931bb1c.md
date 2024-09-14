# 大学橄榄球联盟重组——node2vec

> 原文：[`towardsdatascience.com/college-football-conference-realignment-node2vec-ba2e931bb1c`](https://towardsdatascience.com/college-football-conference-realignment-node2vec-ba2e931bb1c)

[](https://medium.com/@gspmalloy?source=post_page-----ba2e931bb1c--------------------------------)![Giovanni Malloy](https://medium.com/@gspmalloy?source=post_page-----ba2e931bb1c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ba2e931bb1c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ba2e931bb1c--------------------------------) [Giovanni Malloy](https://medium.com/@gspmalloy?source=post_page-----ba2e931bb1c--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ba2e931bb1c--------------------------------) ·阅读时间 16 分钟·2023 年 8 月 8 日

--

你已经来到了这篇四部分博客的最后一部分。在这篇博客的第三部分中，我们尝试探索基于聚类的联盟世界，其中相似的团队可以共享联盟。在本博客中，我们将从电视和媒体网络的角度进行分析。我们将专注于创建一系列为电视定制的精彩对决：每周想象一下[Camping World Kickoff Game](https://campingworldkickoff.com/about/)。换句话说，如果 ESPN 或 FOX 可以根据自己的喜好（以及股东的喜好）调整联盟，那么大学橄榄球的格局将会是什么样的。 在许多方面，这是一种比前面博客更现实的方法。我们的想法是计算大学橄榄球中每场可能比赛的预期收益，贪婪地填充赛程以最大化收益，创建一个“梦想”赛季，基于选择的对决定义网络图，并根据图的结构创建联盟。

![](img/1b43a1347bf189e9d1cfde98814ea1da.png)

图片由 [Jacob Rice](https://unsplash.com/@jrice_photography?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本系列分为四个部分（详细动机见第一部分）：

1.  大学橄榄球联盟重组——Python 中的探索性数据分析

1.  大学橄榄球联盟重组——回归分析

1.  大学橄榄球联盟重组——聚类

1.  [大学橄榄球联盟重组——node2vec](https://medium.com/towards-data-science/college-football-conference-realignment-node2vec-ba2e931bb1c)

希望系列的每一部分能为你提供对受人喜爱的大学橄榄球未来的新视角。对于那些没有阅读第一部分或第二部分的人，快速概述是我创建了一个从网络来源编制的数据集。这些数据包括[每个 FBS 项目的基本信息](https://en.wikipedia.org/wiki/List_of_NCAA_Division_I_FBS_football_programs)、所有[大学橄榄球对抗赛的非标准近似](https://en.wikipedia.org/wiki/List_of_NCAA_college_football_rivalry_games)、[体育场规模](https://en.wikipedia.org/wiki/List_of_NCAA_Division_I_FBS_football_stadiums)、[历史表现](https://en.wikipedia.org/wiki/List_of_NCAA_Division_I_FBS_football_bowl_records)、[AP 前 25 名投票频率](https://collegepollarchive.com/football/index.cfm)、学校是否为[AAU](https://www.aau.edu/)或[R1](https://en.wikipedia.org/wiki/List_of_research_universities_in_the_United_States)机构（对 Big Ten 和 Pac 12 的会员资格历史上很重要）、[NFL 选秀球员数量](https://www.ncaa.com/news/football/article/2020-04-27/college-football-teams-most-nfl-draft-picks-2000)、[2017–2019 年的项目收入数据](https://graphics.wsj.com/table/NCAA_2019)和[关于大学橄榄球粉丝基础的最新估计](https://drive.google.com/file/d/1MiUOwx8X3H2bSkUOz8a1YhceyJWLLCoJ/view)。在第一部分，我们发现有几个特征与粉丝基础规模强相关，因此在第二部分，我们开发了线性回归和随机森林回归模型来预测粉丝基础规模。在第三部分，我们使用受限的 k-means 聚类建议了 10 个新的联盟。其中一些是传统区域对手的网络，而其他则是混合的不匹配的超级联盟。这种方法将如何与我们利润最大化的方法相比？

**特征工程**

鉴于模型方法的复杂性，我们首先从数据开始。这次，我们不需要做太多的处理：

```py
import numpy as np
import pandas as pd

cfb_info_df = pd.read_csv(r'.\FBS_Football_Team_Info.csv', encoding = 'unicode_escape')
```

现在，我们将设置一些用户指定的输入来帮助我们计算特定比赛的预期收益。我们知道前 25 名比赛和对抗赛是精彩的电视节目，因此我们为每场比赛中的每支前 25 名球队分配了一个额外的乘数（每队 1.25 倍）。对于两支前 25 名球队的比赛，这相当于略高于 1.5 倍。对抗赛的额外乘数为 1.5 倍。我们还定义了每个赛季的联盟比赛数量，以帮助按联盟计算收益。目前，许多联盟每赛季进行 9 场联盟比赛。

```py
multiplier_per_top_25_team = 0.25
multiplier_rivalry_game = 0.5

conference_games_per_team = 9
```

**现有联盟的预期收益**

现在，我们准备将预期收益模型投入实际应用。对于每个现有的联盟，我们可以计算每场可能的联盟比赛的收益。首先，我们可以创建每个联盟中所有球队的列表：

```py
acc_teams = list(cfb_info_df['Team'][np.where(cfb_info_df['Current_conference_2025'] == 'ACC')[0]])
american_teams = list(cfb_info_df['Team'][np.where(cfb_info_df['Current_conference_2025'] == 'American')[0]])
big_12_teams = list(cfb_info_df['Team'][np.where(cfb_info_df['Current_conference_2025'] == 'Big 12')[0]])
big_ten_teams = list(cfb_info_df['Team'][np.where(cfb_info_df['Current_conference_2025'] == 'Big Ten')[0]])
cusa_teams = list(cfb_info_df['Team'][np.where(cfb_info_df['Current_conference_2025'] == 'C-USA')[0]])
independent_teams = list(cfb_info_df['Team'][np.where(cfb_info_df['Current_conference_2025'] == 'Independent')[0]])
mac_teams = list(cfb_info_df['Team'][np.where(cfb_info_df['Current_conference_2025'] == 'MAC')[0]])
mountain_west_teams = list(cfb_info_df['Team'][np.where(cfb_info_df['Current_conference_2025'] == 'Mountain West')[0]])
pac_12_teams = list(cfb_info_df['Team'][np.where(cfb_info_df['Current_conference_2025'] == 'Pac-12')[0]])
sec_teams = list(cfb_info_df['Team'][np.where(cfb_info_df['Current_conference_2025'] == 'SEC')[0]])
sun_belt_teams = list(cfb_info_df['Team'][np.where(cfb_info_df['Current_conference_2025'] == 'Sun Belt')[0]])
```

这不是绝对必要的，但我认为它有助于消除之后的混淆。接下来，我们可以开始按会议进行分析。为了本博客的目的，我们以 ACC 为例。我们的策略是循环遍历每支球队及其可能的对手，以获得一个可能的会议比赛数据框。对于每场比赛，我们将假设预期回报是每支球队球迷基础的大小、其中一个、两个或没有球队进入 AP top 25，以及比赛是否为对抗赛的函数。因此，在循环过程中我们会跟踪这些因素。在这个例子中，我们跟踪对抗赛的方式是通过从对抗赛边列表创建一个网络。

```py
import networkx as nx

rivalry_edge_list = pd.read_csv(r'.\cfb_rivalry_edge_list.csv', encoding = 'unicode_escape')

G = nx.Graph()

node_df = pd.read_csv(r'.\cfb_node_list.csv', encoding = 'unicode_escape')

for team in np.array(node_df['Team']):
    G.add_node(team)

for index in range(0, len(rivalry_edge_list['i'])):
    G.add_edge(rivalry_edge_list['i'][index], rivalry_edge_list['j'][index], attr = rivalry_edge_list['weight'][index])
```

现在，我们准备好开始循环：

```py
curr_conf_teams = acc_teams

team_i_name = []
team_j_name = []
game_is_rivalry = []
team_i_prob_top_25 = []
team_j_prob_top_25 = []
team_i_fanbase = []
team_j_fanbase = []

for i in range(len(curr_conf_teams)):
    for j in range((i+1), len(curr_conf_teams)):
        team_i_name.append(curr_conf_teams[i])
        team_j_name.append(curr_conf_teams[j])

        if (curr_conf_teams[i] in list(G.neighbors(curr_conf_teams[j]))) | (curr_conf_teams[j] in list(G.neighbors(curr_conf_teams[i]))):
            game_is_rivalry.append(1)
        else:
            game_is_rivalry.append(0)

        team_i_prob_top_25.append(float(cfb_info_df['p_AP_Top_25_2001_to_2021'][np.where(cfb_info_df['Team'] == curr_conf_teams[i])[0]]))
        team_j_prob_top_25.append(float(cfb_info_df['p_AP_Top_25_2001_to_2021'][np.where(cfb_info_df['Team'] == curr_conf_teams[j])[0]]))

        team_i_fanbase.append(float(cfb_info_df['tj_altimore_fan_base_size_millions'][np.where(cfb_info_df['Team'] == curr_conf_teams[i])[0]]))
        team_j_fanbase.append(float(cfb_info_df['tj_altimore_fan_base_size_millions'][np.where(cfb_info_df['Team'] == curr_conf_teams[j])[0]]))
```

然后，我们使用 pandas 创建一个包含所有会议对阵信息的数据框。我们知道 AP Top 25 每周都会变化，因此我们假设进入 AP Top 25 的概率是一个球队在 2001–2021 年间出现在 AP Top 25 调查中的频率。对于每场比赛，有三种情况：两个 top 25 球队、一支 top 25 球队或零支 top 25 球队。我们计算每种情况的概率。然后，对于这些情况中的每一种，我们计算预期回报为 (1+muliplier_top_25)^(# teams in top 25) * (1 + (multiplier_rivalry_game * is_rivalry_game) * (两个球队球迷基础的总和)。最后，我们通过将比赛情况的概率与该比赛情况的回报相乘来获得比赛的预期回报。

```py
acc_game_df = pd.DataFrame(team_i_name, columns = ['team_i'])
acc_game_df['team_j'] = team_j_name
acc_game_df['is_rivalry_game'] = game_is_rivalry
acc_game_df['p_top_25_team_i'] = team_i_prob_top_25
acc_game_df['p_top_25_team_j'] = team_j_prob_top_25
acc_game_df['fanbase_millions_team_i'] = team_i_fanbase
acc_game_df['fanbase_millions_team_j'] = team_j_fanbase

acc_game_df['p_2_top_25_teams'] = acc_game_df['p_top_25_team_i'] * acc_game_df['p_top_25_team_j']
acc_game_df['p_0_top_25_teams'] = (1 - acc_game_df['p_top_25_team_i']) * (1 - acc_game_df['p_top_25_team_j'])
acc_game_df['p_1_top_25_teams'] = 1 - acc_game_df['p_0_top_25_teams'] - acc_game_df['p_2_top_25_teams']

acc_game_df['payoff_2_top_25_teams'] = ((1 + multiplier_per_top_25_team) ** 2) * (1 + (multiplier_rivalry_game * acc_game_df['is_rivalry_game'])) * (acc_game_df['fanbase_millions_team_i'] + acc_game_df['fanbase_millions_team_j'])
acc_game_df['payoff_0_top_25_teams'] = (1 + (multiplier_rivalry_game * acc_game_df['is_rivalry_game'])) * (acc_game_df['fanbase_millions_team_i'] + acc_game_df['fanbase_millions_team_j'])
acc_game_df['payoff_1_top_25_teams'] = (1 + multiplier_per_top_25_team) * (1 + (multiplier_rivalry_game * acc_game_df['is_rivalry_game'])) * (acc_game_df['fanbase_millions_team_i'] + acc_game_df['fanbase_millions_team_j'])

acc_game_df['expected_payoff_game'] = (acc_game_df['p_2_top_25_teams'] * acc_game_df['payoff_2_top_25_teams']) + (acc_game_df['p_1_top_25_teams'] * acc_game_df['payoff_1_top_25_teams']) + (acc_game_df['p_0_top_25_teams'] * acc_game_df['payoff_0_top_25_teams'])
```

结果是所有可能的会议对阵的预期回报分布，我们可以使用 plotly 直方图绘制出来：

```py
import plotly.express as px

fig = px.histogram(acc_game_df, x="expected_payoff_game", 
                   title='ACC Conference Games',
                   labels={'expected_payoff_game':'Expected Payoff per Game'})
fig.show()
```

![](img/c95e8b566727f159c3e592223e9b980f.png)

大多数 ACC 比赛的预期回报在 0 到 7 之间。有一些异常值的回报非常高。

我们可以看到，通常情况下，比赛的预期回报在 0 到 7 之间。像 Florida State — Miami、Clemson — Florida State 和 Virginia Tech — Miami 这样的比赛是回报高于 10 的异常值。这些比赛作为持续的黄金时段对决是合理的，这验证了我们的模型。

我们可以对每个会议重复这一过程，并通过箱形图轻松比较各个会议的分布。在这里，我们将使用 seaborn 和 matplotlib 来创建箱形图。

```py
from matplotlib import pyplot as plt
import seaborn as sns

combined_payoff_df = pd.DataFrame({'ACC': acc_game_df['expected_payoff_game'],
                                   'American': american_game_df['expected_payoff_game'],
                                   'Big 12': big_12_game_df['expected_payoff_game'],
                                   'B1G': big_ten_game_df['expected_payoff_game'],
                                   'C-USA': cusa_game_df['expected_payoff_game'],
                                   'MAC': mac_game_df['expected_payoff_game'],
                                   'Mountain West': mountain_west_game_df['expected_payoff_game'],
                                   'Pac-12': pac_12_game_df['expected_payoff_game'],
                                   'SEC': sec_game_df['expected_payoff_game'],
                                   'Sun Belt': sun_belt_game_df['expected_payoff_game']})

plt.figure(figsize=(16, 6))
sns.set_style('white')
sns.boxplot(data=combined_payoff_df)
sns.despine()
plt.show()
```

![](img/76f9d2fbee72fae989947c688cd9d590.png)

在经济回报方面，我们可以看到 Big Ten 和 SEC 明显占优势，而其他 Power 5 会议紧密分布，Mountain West 会议则在 Group of 5 会议中处于领先地位。

在这里，我们可以数学上确认体育媒体世界自最新一轮重组以来一直声称的：大十和 SEC 正在巩固权力和价值。这对于诸如“大游戏”这样的会议的精彩对决尤其如此：俄亥俄州立大学——密歇根大学。它确实名副其实。我们看到 ACC、大 12 和 Pac-12 之间存在相对的平衡，也看到山西西部会议是 Group of 5 会议中的明确首选。在下面的表格中，我们可以详细了解按会议划分的回报情况。

![](img/fafc3d03100f7127c348443611a3e032.png)

表格比较了每场比赛和每队每会议的预期回报。

正如我们之前所指出的，大十和 SEC 主导了比赛，但 SEC 的方差几乎是大十的一半。另一个有趣的结果是，大 12 的每队回报大约是 Pac-12 的一半。那么，为什么这两个联盟最近似乎在[不断重组争吵](https://www.outkick.com/whose-bigger-pac-12-vs-big-12/#:~:text=Big%2012%20Commissioner%20Brett%20Yormark%20made%20waves%20when,if%20we%E2%80%99re%20going%20shopping%20there%20yet%20or%20not.%E2%80%9D)呢？这可能与 Pac-12 剩余的宝贵资产：俄勒冈州和华盛顿的未来不确定性有关。另一方面，大 12 的球队并不担心被挖角。

**贪婪调度**

现在，我们对我们的模型已经足够自信，可以计算每场比赛的预期回报，我们可以计算 FBS 中所有 8,778 种可能的对决的预期回报。

```py
fbs_teams = list(cfb_info_df['Team'])

curr_conf_teams = fbs_teams

team_i_name = []
team_j_name = []
game_is_rivalry = []
team_i_prob_top_25 = []
team_j_prob_top_25 = []
team_i_fanbase = []
team_j_fanbase = []

for i in range(len(curr_conf_teams)):
    for j in range((i+1), len(curr_conf_teams)):
        team_i_name.append(curr_conf_teams[i])
        team_j_name.append(curr_conf_teams[j])

        if (curr_conf_teams[i] in list(G.neighbors(curr_conf_teams[j]))) | (curr_conf_teams[j] in list(G.neighbors(curr_conf_teams[i]))):
            game_is_rivalry.append(1)
        else:
            game_is_rivalry.append(0)

        team_i_prob_top_25.append(float(cfb_info_df['p_AP_Top_25_2001_to_2021'][np.where(cfb_info_df['Team'] == curr_conf_teams[i])[0]]))
        team_j_prob_top_25.append(float(cfb_info_df['p_AP_Top_25_2001_to_2021'][np.where(cfb_info_df['Team'] == curr_conf_teams[j])[0]]))

        team_i_fanbase.append(float(cfb_info_df['tj_altimore_fan_base_size_millions'][np.where(cfb_info_df['Team'] == curr_conf_teams[i])[0]]))
        team_j_fanbase.append(float(cfb_info_df['tj_altimore_fan_base_size_millions'][np.where(cfb_info_df['Team'] == curr_conf_teams[j])[0]]))

fbs_game_df = pd.DataFrame(team_i_name, columns = ['team_i'])
fbs_game_df['team_j'] = team_j_name
fbs_game_df['is_rivalry_game'] = game_is_rivalry
fbs_game_df['p_top_25_team_i'] = team_i_prob_top_25
fbs_game_df['p_top_25_team_j'] = team_j_prob_top_25
fbs_game_df['fanbase_millions_team_i'] = team_i_fanbase
fbs_game_df['fanbase_millions_team_j'] = team_j_fanbase

fbs_game_df['p_2_top_25_teams'] = fbs_game_df['p_top_25_team_i'] * fbs_game_df['p_top_25_team_j']
fbs_game_df['p_0_top_25_teams'] = (1 - fbs_game_df['p_top_25_team_i']) * (1 - fbs_game_df['p_top_25_team_j'])
fbs_game_df['p_1_top_25_teams'] = 1 - fbs_game_df['p_0_top_25_teams'] - fbs_game_df['p_2_top_25_teams']

fbs_game_df['payoff_2_top_25_teams'] = ((1 + multiplier_per_top_25_team) ** 2) * (1 + (multiplier_rivalry_game * fbs_game_df['is_rivalry_game'])) * (fbs_game_df['fanbase_millions_team_i'] + fbs_game_df['fanbase_millions_team_j'])
fbs_game_df['payoff_0_top_25_teams'] = (1 + (multiplier_rivalry_game * fbs_game_df['is_rivalry_game'])) * (fbs_game_df['fanbase_millions_team_i'] + fbs_game_df['fanbase_millions_team_j'])
fbs_game_df['payoff_1_top_25_teams'] = (1 + multiplier_per_top_25_team) * (1 + (multiplier_rivalry_game * fbs_game_df['is_rivalry_game'])) * (fbs_game_df['fanbase_millions_team_i'] + fbs_game_df['fanbase_millions_team_j'])

fbs_game_df['expected_payoff_game'] = (fbs_game_df['p_2_top_25_teams'] * fbs_game_df['payoff_2_top_25_teams']) + (fbs_game_df['p_1_top_25_teams'] * fbs_game_df['payoff_1_top_25_teams']) + (fbs_game_df['p_0_top_25_teams'] * fbs_game_df['payoff_0_top_25_teams'])
```

结果分布如下：

```py
fig = px.histogram(fbs_game_df, x="expected_payoff_game", 
                   title='All Possible NCAA FBS Games',
                   labels={'expected_payoff_game':'Expected Payoff per Game'})
fig.show()
```

![](img/0bb0af1b9aab840704479df3219f5e81.png)

整个 FBS 中每场比赛的预期回报分布严重右偏。在近 9,000 种可能的对决中，大多数回报较低。

直方图揭示了一个预期结果：有一些比赛的回报非常高，而大多数则是低回报。分布是右偏的，游戏的中位数预期回报大约为 2，平均预期回报大约为 3。虽然预期回报数字本身难以解释，但定性结果无疑是启发性的。会议重组的动机在于最大化每队的会议预期回报（这通常，但并非总是，与最大化每场比赛的预期回报相一致）。

虽然我们都希望每年能看到 8,778 场大学橄榄球比赛，但每支球队的赛程是有限的。通常，球队会打 12 场常规赛（除非他们在夏威夷打比赛，那他们可以打 13 场），但为了我们的目的，假设每支球队每赛季可以打 15 场比赛，以增加一些额外的灵活性，方便[扩展季后赛](https://www.cbssports.com/college-football/news/college-football-playoff-expansion-remains-unsettled-as-committee-eyes-12-team-field-in-2024/)。我们可以创建一个数据框来跟踪我们新安排的比赛：

```py
num_games_per_school = 15

schedule_df = pd.DataFrame(cfb_info_df['Team'])
schedule_df['games_scheduled'] = 0

for i in range(num_games_per_school):
    col_name = 'game_' + str(i+1)
    schedule_df[col_name] = ''
```

我们将安排视为一个[背包问题](https://en.wikipedia.org/wiki/Knapsack_problem)，我们将使用贪婪算法来解决。实质上，我们将尽可能多地将最高回报率的比赛添加到赛程中，前提是该比赛的两支球队的赛程中仍有空余。结果应该是媒体公司所期望的最佳赛季。他们将尽可能多地让最顶尖的球队对战。当然，这种策略忽略了任何现有的会议边界，这正是重点：我们将首先生成赛程，然后使用这些赛程来确定会议。以下是生成我们主 FBS 赛程的代码：

```py
#Sort all games in descending order
sorted_fbs_game_df = fbs_game_df.sort_values(by = 'expected_payoff_game', ascending = False)
sorted_fbs_game_df['is_game_scheduled'] = 0

# For each game, pick it if the teams have space on their schedule
for index in range(len(sorted_fbs_game_df['expected_payoff_game'])):

    #Get index of team in schedule df
    team_i_sched_id = np.where(schedule_df['Team'] == sorted_fbs_game_df['team_i'].iloc[index])[0][0]
    team_j_sched_id = np.where(schedule_df['Team'] == sorted_fbs_game_df['team_j'].iloc[index])[0][0]

    #Check that both teams have room in the schedule
    if (schedule_df['games_scheduled'].iloc[team_i_sched_id] < num_games_per_school) and (schedule_df['games_scheduled'].iloc[team_j_sched_id] < num_games_per_school):

        #Find num games scheduled
        num_games_i = schedule_df['games_scheduled'].iloc[team_i_sched_id]
        num_games_j = schedule_df['games_scheduled'].iloc[team_j_sched_id]

        #Add team j to team i's schedule
        team_i_name = sorted_fbs_game_df['team_i'].iloc[index]
        team_j_name = sorted_fbs_game_df['team_j'].iloc[index]

        schedule_df['game_'+str(num_games_i + 1)].iloc[team_i_sched_id] = team_j_name

        #Add team i to team j's schedule
        schedule_df['game_'+str(num_games_j + 1)].iloc[team_j_sched_id] = team_i_name

        #Increment games scheduled
        schedule_df['games_scheduled'].iloc[team_i_sched_id] = schedule_df['games_scheduled'].iloc[team_i_sched_id] + 1
        schedule_df['games_scheduled'].iloc[team_j_sched_id] = schedule_df['games_scheduled'].iloc[team_j_sched_id] + 1

        #Mark game as scheduled in sorted games df
        sorted_fbs_game_df['is_game_scheduled'].iloc[index] = 1

    if index % 100 == 0:
        print(str(round((index+1)/len(sorted_fbs_game_df['expected_payoff_game']), 2)*100)+'% complete')
```

最高回报率的比赛不被安排？俄亥俄州立大学对阵佛罗里达州立大学，预计回报率为 19.84。最低回报率的比赛？杰克逊维尔州立大学对阵萨姆休斯顿州立大学，预计回报率为 0.08。我们可以创建一个直方图来比较我们贪婪选择的比赛的预计回报率与所有可能比赛的预计回报率：

```py
fig = px.histogram(chosen_games_df, x="expected_payoff_game", 
                   title='Chosen NCAA FBS Games',
                   labels={'expected_payoff_game':'Expected Payoff per Game'})
fig.show()
```

![](img/83f7e9835808d8478d0649da131f07bd.png)

大多数低回报率的比赛没有被选择，而高回报率的比赛被选择了。

根据直方图，我们可以看到，我们的新会议模型保留了最高回报率的比赛，并大幅减少了最低回报率的比赛，相较于可能的 FBS 比赛。

**网络模型**

要从主赛程转换到会议，我们需要理解现在球队之间的关系。我们将采用的方法是使用[networkx](https://networkx.org/)将我们的主 FBS 赛程转换为无向图。（如果你对图论不熟悉，我建议你从我多年前开始的地方入手：[维基百科](https://en.wikipedia.org/wiki/Graph_theory)。）每支球队将成为一个顶点（或节点），每场比赛将成为图中的一条边。我们还将为图添加边权重，其中每条边的权重是连接该边的两支球队之间比赛的预计回报率。让我们创建网络：

```py
#Get dataframe of only chosen games
chosen_games_df = sorted_fbs_game_df.iloc[np.where(sorted_fbs_game_df['is_game_scheduled'] == 1)]

#Create graph
chosen_G = nx.from_pandas_edgelist(chosen_games_df, source='team_i', target='team_j')

#Add edge weights to graph
for c in range(len(chosen_games_df['team_i'])):
    chosen_G[chosen_games_df['team_i'].iloc[c]][chosen_games_df['team_j'].iloc[c]]['weight'] = chosen_games_df['expected_payoff_game'].iloc[c]
```

我们可以通过使用 draw_circular()来可视化地确定这种方法是否适用于创建会议。我们可以直观地检查模型是否看起来像一个[小世界](https://www.nature.com/articles/30918)。这是一个好兆头，因为我们期望同一会议的球队彼此对战。在我们的网络中，这意味着大量的共同邻居。

![](img/a3118f9f0d9245019548f13fa2c00397.png)

主 FBS 赛程显示出小世界特性。

**node2vec**

将我们的大学橄榄球球队网络转化为一组联盟的关键在于一种名为[node2vec](https://dl.acm.org/doi/10.1145/2939672.2939754)的算法，该算法由 Aditya Grover 和 Jure Leskovec 提出。node2vec 算法的简短总结是，它使用偏置随机游走过程来生成图中节点的低维表示。它的灵感来自于那些处理自然语言处理的人员所熟悉的 word2vec 算法。然而，node2vec 并不是词的 skip gram 模型，而是图中的偏置随机游走。稍后，我会尝试做一个博客帖子，比较这两者。如果我们在 Python 中使用了 SNAP 包来创建网络，那里有一个内置的[node2vec](http://snap.stanford.edu/node2vec/)函数。然而，我们将使用与 networkx 兼容的[实现](https://github.com/eliorc/node2vec)。我必须承认，在线文档不像我在博客系列中使用的大多数其他包那样详尽，因此我会尽量详细解释。

```py
from node2vec import Node2Vec

#Precompute probabilities and generate walks
node2vec = Node2Vec(chosen_G, dimensions = 64, walk_length = 30, num_walks = 50, workers = 1, seed = 0)

#Embed nodes
model = node2vec.fit(window = 10, min_count = 1, batch_words = 4)
```

在上面的代码中，我们导入了 networkx 的 node2vec 包，首先生成随机游走。维度输入是每个节点的数组输出长度，游走长度是每次随机游走包含的转移数量，游走数量是从每个节点开始的随机游走数量。workers 输入用于并行执行，但对于 Windows 机器，必须设置为 1。然后，我们运行 node2vec 模型以获取嵌入。我们创建了一个仅包含嵌入的数据框和一个包含球队名称的数据框。

```py
# Data frame of embeddings only
node2vec_df_clean = pd.DataFrame(model.wv.vectors)

#Data frame of embeddings and team name
node2vec_df = pd.DataFrame(model.wv.vectors)
node2vec_df['Team'] = model.wv.key_to_index.keys()
```

**聚类**

和之前一样，我们将使用 k-means 聚类来创建我们的联盟。然而，这次，聚类将使用 node2vec 嵌入来创建。通过这种方式，网络结构中具有相似位置的球队被分组到一起形成联盟。鉴于网络基于贪婪调度，我们应该看到那些经常一起比赛的球队被聚集到新的联盟中。由于我在这一系列的第三部分中已经详细描述了 k-means 聚类，所以现在我将跳过解释。相反，这里是实现 8 个重新调整联盟的聚类的代码。这次，我们的联盟数量将是总球队数量和每支球队每赛季比赛数量的函数。

```py
from sklearn.cluster import KMeans

num_conferences = int(len(node2vec_df['Team']) / num_games_per_school)
kmeans = KMeans(n_clusters = num_conferences, random_state=0).fit(node2vec_df_clean)
kmeans_conferences = kmeans.labels_
node2vec_df['Conference'] = kmeans_conferences
```

**数据驱动的 FBS 联盟**

现在，揭晓时刻到了！如果媒体高管能够随心所欲地展示每年大学橄榄球中最有回报的比赛，球队将如何重新划分为新的联盟？这种方法既受到数据的驱动，也受到金钱的推动，因此这是我们迄今为止最强有力的推荐。

+   *ACC+*：雪城大学、迈阿密（佛罗里达）、佛罗里达州立大学、内布拉斯加大学、克莱姆森大学、北卡罗来纳大学、南卡罗来纳大学、密苏里大学、阿肯色大学、爱荷华大学、弗吉尼亚理工大学、华盛顿大学、奥尔·密斯大学、西弗吉尼亚大学、加州大学洛杉矶分校、肯塔基大学、马里兰大学、亚利桑那州立大学、乔治亚理工学院。我忍不住在这个博客中用一个“+”来命名一个会议。这太时髦了，这个会议看起来像是 ACC 加上一些其他的。在这种情况下，“+”包括了中美洲和太平洋沿岸的存在。这对那些无法在超级 20 中获利的媒体高管来说真是一个真正的喜悦。

+   *Mountain South*：马歇尔大学、特洛伊大学、路易斯安那大学、UTEP 大学、阿巴拉契亚州立大学、路易斯安那理工大学、佛罗里达大西洋大学、圣荷西州立大学、UTSA 大学、阿肯色州立大学、南卫理公会大学、UNLV 大学、阿克伦大学、布法罗大学、犹他州立大学、FIU 大学、北德克萨斯大学、路易斯安那-门罗大学。这些球队都来自当前的山地西部会议或美国南部，除了布法罗和阿克伦这两个明显的例外。这些球队有可能打乱一个高期望的赛季（参见[阿巴拉契亚州立大学对密歇根大学](https://en.wikipedia.org/wiki/2007_Appalachian_State_vs._Michigan_football_game)）。

+   *Tuesday Group*：新墨西哥州立大学、南阿拉巴马大学、乔治亚州立大学、鲍灵格林大学、中密歇根大学、德克萨斯州立大学、鲍尔州立大学、肯特州立大学、杰克逊维尔州立大学、东密歇根大学、詹姆斯·麦迪逊大学、萨姆·休斯顿州立大学。部分以[温和的共和党核心小组](https://en.wikipedia.org/wiki/Republican_Governance_Group)命名，部分因为这个会议最有可能在 ESPNU 的热门周二夜晚时段比赛。

+   *Power Group*：海军学院、杜克大学、弗雷斯诺州立大学、西北大学、维吉尼亚大学、波士顿学院、印第安纳大学、中佛罗里达大学、爱荷华州立大学、贝勒大学、华盛顿州立大学、范德比大学、圣地亚哥州立大学、堪萨斯大学、陆军学院、俄勒冈州立大学。这个会议包括了在五大会议中粉丝基础较低的球队和五小会议中粉丝基础较高的球队。

+   *Super 20*：俄亥俄州立大学、诺特丹大学、德克萨斯大学、佛罗里达大学、密歇根大学、宾夕法尼亚州立大学、阿拉巴马大学、俄勒冈大学、路易斯安那州立大学、乔治亚大学、威斯康星大学、南加州大学、俄克拉荷马大学、奥本大学、德克萨斯农工大学、密歇根州立大学、田纳西大学、德克萨斯理工大学、伊利诺伊大学、TCU 大学。这是一个会让 ESPN、福克斯、CBS 和 NBC 争夺的会议。大名鼎鼎的球队意味着大粉丝基础和丰厚的回报。这是一个媒体高管的梦想，实际上如果 Big Ten 和 SEC 的最佳球队希望联合起来并加入一些其他球队，也并非不可能。

+   *Pan-American Conference 17 (Pac-17)*：匹兹堡大学、明尼苏达大学、NC 州立大学、俄克拉荷马州立大学、普渡大学、斯坦福大学、亚利桑那大学、路易斯维尔大学、博伊西州立大学、犹他大学、密西西比州立大学、BYU 大学、加州大学伯克利分校、堪萨斯州立大学、康涅狄格大学、科罗拉多大学、拉格斯大学。这是我们新调整的世界中最大的会议之一。它从康涅狄格州延伸到加利福尼亚州，拥有许多粉丝基础较小的稳固球队。预测是高质量的足球，但不总是回报最高的足球。

+   *中西部*: UAB、中田纳西州立大学、西密歇根大学、自由大学、塔尔萨大学、迈阿密（OH）、莱斯大学、托莱多大学、北伊利诺伊大学、夏洛特大学、旧统治大学、俄亥俄大学、海岸卡罗莱纳大学、杜兰大学、UMass、乔治亚南方大学、西肯塔基大学。这个会议结合了来自目前五大组别的各支球队。大多数这些球队位于国家的内陆地区。因此，提出了这个名字。

+   *AAMWC*: 休斯顿、孟菲斯、新墨西哥、怀俄明、夏威夷、东卡罗来纳、内华达、南密西西比、科罗拉多州、南佛罗里达、辛辛那提、空军、天普、维克森林大学。这个会议是当前美国竞技会议和山西会议的完美融合。这些球队过去都表现非常出色，但在他们的赛季表现上却缺乏一致性。

node2vec 算法旨在将相似的节点分组，但我们仍然应该验证每支球队的会议比赛数量在各会议之间是否稳定。

```py
num_teams_per_conf = np.zeros(num_conferences)

for i in range(num_conferences):
    print(node2vec_df['Team'][np.where(node2vec_df['Conference'] == i)[0]])
    num_teams_per_conf[i] = len(node2vec_df['Team'][np.where(node2vec_df['Conference'] == i)[0]])

num_conference_games_per_conf = np.zeros(num_conferences)
num_non_conference_games_per_conf = np.zeros(num_conferences)

for e in chosen_G.edges():
    conference_i = node2vec_df['Conference'][np.where(node2vec_df['Team'] == e[0])[0][0]]
    conference_j = node2vec_df['Conference'][np.where(node2vec_df['Team'] == e[1])[0][0]]

    if conference_i == conference_j:
        num_conference_games_per_conf[conference_i] = num_conference_games_per_conf[conference_i] + 2
    else:
        num_non_conference_games_per_conf[conference_i] = num_non_conference_games_per_conf[conference_i] + 1
        num_non_conference_games_per_conf[conference_j] = num_non_conference_games_per_conf[conference_j] + 1

plot_df = pd.DataFrame(num_conference_games_per_conf, columns = ['num_conf_games'])
plot_df['num_non_conf_games'] = num_non_conference_games_per_conf
plot_df['Conference'] = ['ACC+', 'Mountain South', 'Tuesday Group', 'Power Group', 'Super 20', 'Pac-17', 'Middle West', 'AAMWC']
plot_df['Percent Conference Games'] = plot_df['num_conf_games']/(plot_df['num_conf_games']+plot_df['num_non_conf_games'])
plot_df['Average Number Conference Games'] = num_games_per_school*(plot_df['num_conf_games']/(plot_df['num_conf_games']+plot_df['num_non_conf_games']))

import plotly.express as px

fig = px.bar(plot_df, x='Conference', y='Average Number Conference Games', height=400)
fig.show()
```

上述代码生成了新调整的 FBS 世界中每所学校会议比赛平均数量的对比。

![](img/7d1fe5c056a4bb83ed0c29b243120a6a.png)

所有会议的每支球队平均进行 10.1 到 11.3 场会议比赛。

所有新的会议平均每年进行 10 到 11 场会议比赛，总赛程为 15 场。这种会议之间的一致性验证了会议之间赛程的稳定性。

至此，我们结束了这个博客系列。我们学习了如何探索数据，使用监督和无监督机器学习，并实施决策模型以创建理想的大学橄榄球会议。也许这个博客中介绍的方法已经在幕后用于指导 FBS 中的会议重新调整。如果没有，那它们应该被使用。我们钟爱的比赛正进入一个由金钱驱动的新纪元。这个框架提出了一种由经济驱动的方法，同时融入了传统的竞争，这使得大学橄榄球与众不同。如果有一天我们看到会议按照我在这里呈现的方式对齐，你会在这里第一个听到。直到那时，我会继续享受我的周六，观看并等待。

感谢阅读！一如既往，请在下方评论你的想法。我知道这是一个思维实验，所以让我知道你的反应。你的球队最终在哪儿？你是否更喜欢这些联盟而不是今天的会议？

在 Twitter 上表达一些爱意: [@malloy_giovanni](https://twitter.com/malloy_giovanni)

对我的内容感兴趣？请考虑 [在 Medium 上关注我](https://medium.com/@gspmalloy)。
