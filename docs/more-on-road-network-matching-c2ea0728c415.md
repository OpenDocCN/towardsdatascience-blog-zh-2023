# 关于道路网络匹配的更多内容

> 原文：[https://towardsdatascience.com/more-on-road-network-matching-c2ea0728c415?source=collection_archive---------16-----------------------#2023-02-10](https://towardsdatascience.com/more-on-road-network-matching-c2ea0728c415?source=collection_archive---------16-----------------------#2023-02-10)

## 道路网络匹配的恶作剧

[](https://medium.com/@joao.figueira?source=post_page-----c2ea0728c415--------------------------------)[![João Paulo Figueira](../Images/54e4176f66e4ab0324d86ec71d8b033d.png)](https://medium.com/@joao.figueira?source=post_page-----c2ea0728c415--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c2ea0728c415--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c2ea0728c415--------------------------------) [João Paulo Figueira](https://medium.com/@joao.figueira?source=post_page-----c2ea0728c415--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F64bc009cedeb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmore-on-road-network-matching-c2ea0728c415&user=Jo%C3%A3o+Paulo+Figueira&userId=64bc009cedeb&source=post_page-64bc009cedeb----c2ea0728c415---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c2ea0728c415--------------------------------) ·9 分钟阅读·2023年2月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc2ea0728c415&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmore-on-road-network-matching-c2ea0728c415&user=Jo%C3%A3o+Paulo+Figueira&userId=64bc009cedeb&source=-----c2ea0728c415---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc2ea0728c415&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmore-on-road-network-matching-c2ea0728c415&source=-----c2ea0728c415---------------------bookmark_footer-----------)![](../Images/c573f363fdc89bade23f26c4bc50e04e.png)

图片由 [Denys Nevozhai](https://unsplash.com/de/@dnevozhai?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本文的目的是对[之前关于同一主题的文章](/road-network-edge-matching-with-triangles-5dc989a77edf)进行补充和修正。在那篇文章中，我介绍了一种从[扩展车辆能量数据集](https://arxiv.org/abs/2203.08630)¹（EVED）重建丢失地图匹配数据的方法。我的技术探索了三角形的数学特性，以便快速找到道路网络数据库中的丢失数据。不幸的是，代码中的一个细微错误使得在某些情况下失败。

在这里，我展示了对代码的修正，并讨论了应用这些技术重建三万两千多个轨迹的结果。

[## 使用三角形进行道路网络边缘匹配](https://towardsdatascience.com/road-network-edge-matching-with-triangles-5dc989a77edf?source=post_page-----c2ea0728c415--------------------------------)

### 三角形在地理空间查询中具有强大的属性

[towardsdatascience.com](https://towardsdatascience.com/road-network-edge-matching-with-triangles-5dc989a77edf?source=post_page-----c2ea0728c415--------------------------------)

# 网络的细微差别

在上一篇文章中，我展示了两种方法来将地图匹配的位置拟合到道路网络段上，使用了三角形的性质。支持该思想的几何概念是适当的，但代码需要修正。问题出现在节点过滤部分，影响了两种匹配方法，即基于比例和基于距离的方法。让我们再次查看使用基于比例的函数的原始代码：

```py
def get_matching_edge(self, latitude, longitude, bearing=None):
    loc = np.array([latitude, longitude])
    _, r = self.geo_spoke.query_knn(loc, 1)
    radius = self.max_edge_length + r[0]
    node_idx, dists = self.geo_spoke.query_radius(loc, radius)
    nodes = self.ids[node_idx]
    distances = dict(zip(nodes, dists))
    adjacent_set = set()
    graph = self.graph

    best_edge = None
    for node in nodes:
        if node not in adjacent_set:
            adjacent_nodes = np.intersect1d(np.array(graph.adj[node]),
                                            nodes, assume_unique=True)

            adjacent_set.update(adjacent_nodes)
            for adjacent in adjacent_nodes:
                edge_length = graph[node][adjacent][0]['length']
                ratio = (distances[node] + distances[adjacent]) / \
                        edge_length
                if best_edge is None or ratio < best_edge[2]:
                    best_edge = (node, adjacent, ratio)

        if bearing is not None:
            best_edge = fix_edge_bearing(best_edge, bearing, graph)
    return best_edge
```

该函数使用一个集合来存储所有处理过的相邻节点，以避免重复。遗憾的是，它确实避免了重复。这就是错误所在，因为通过排除一个节点，函数也阻止了所有其他相邻边缘的测试。这种情况在具有多个链接的节点中特别普遍。修正代码需要一个概念上的变化，即移除处理过的道路网络*边缘*而不是处理过的*节点*。

但是在使用代码处理几个不同轨迹后出现了另一个问题，即“黑天鹅”事件。当输入的 GPS 位置接近某个节点时会发生什么？在一些罕见情况下，采样的 GPS 位置与某个道路网络节点非常接近，这会引起微妙的浮点不稳定性。结果是选择了一个不合理的边缘，破坏了路径重建。为了解决这个问题，我添加了一个默认参数，设置从输入的 GPS 位置到最近节点的最小距离。当这个距离小于一米（三英尺）时，函数拒绝选择边缘并返回一个空值。这种解决方案比较温和，因为这种情况相对不常发生，而且随后的重建机制可以补偿这一点。

你可以看到下面的修正代码，其中包含了测试过的道路网络边缘集和最小距离检查。

```py
def get_matching_edge(self, latitude, longitude,
                      bearing=None, min_r=1.0):
    best_edge = None
    loc = np.array([latitude, longitude])
    _, r = self.geo_spoke.query_knn(loc, 1)
    if r > min_r:
        radius = self.max_edge_length + r[0]
        node_idx, dists = self.geo_spoke.query_radius(loc, radius)
        nodes = self.ids[node_idx]
        distances = dict(zip(nodes, dists))
        tested_edges = set()
        graph = self.graph
        node_set = set(nodes)

        for node in nodes:
            adjacent_nodes = node_set & set(graph.adj[node])

            for adjacent in adjacent_nodes:
                if (node, adjacent) not in tested_edges:
                    edge_length = graph[node][adjacent][0]['length']
                    ratio = edge_length / (distances[node] + distances[adjacent])

                    if best_edge is None or ratio > best_edge[2]:
                        best_edge = (node, adjacent, ratio)
                    tested_edges.add((node, adjacent))
                    tested_edges.add((adjacent, node))

        if bearing is not None:
            best_edge = fix_edge_bearing(best_edge, bearing, graph)
    return best_edge
```

请注意，我还更改了速率公式，使其范围在零到一之间，较大的值对应于更好的拟合。有趣的是，基于速率的函数比基于距离的函数更有效且更快速。

# 重建旅行

路径重建的第一部分意味着将上述函数应用于输入轨迹的每一点，只保留独特的道路网络边缘。下面的代码展示了一个实现这一点的 Python 函数。

```py
def match_edges(road_network, trajectory):
    edges = []
    unique_locations = set()
    edge_set = set()
    for p in trajectory:
        if p not in unique_locations:
            e = road_network.get_matching_edge(*p, min_r=1.0)
            if e is not None:
                n0, n1, _ = e
                edge = (n0, n1)
                if edge not in edge_set:
                    edge_set.add(edge)
                    edges.append(edge)
                unique_locations.add(p)
    return edges
```

该函数为每个独特的轨迹位置分配一个道路网络边缘。**图 1** 显示了一个匹配样本。

![](../Images/d764592ef9b23ed84cd8f059317b0a86.png)

**图 1** — 上面的地图显示了红色的输入轨迹位置和蓝色的匹配道路网络边。（图片来源：作者使用 Folium 和 OpenStreetMap 图像）

如您所见，由于 GPS 采样频率，匹配的链接之间存在一些间隙。重建路径要求我们填补这些间隙，我们通过在道路网络边之间找到最短路径来实现。下面的代码通过保持连接的链接在一起，并使用 OSMnx 的服务填补其他所有间隙来完成这一任务 [2]。

```py
def build_path(rn, edges):
    path = []
    for e0, e1 in pairwise(edges):
        if not len(path):
            path.append(e0[0])
        if e0[0] != e1[0] and e0[1] != e1[1]:
            if e0[1] == e1[0]:
                path.extend([e0[1], e1[1]])
            else:
                n0, n1 = int(e0[1]), int(e1[0])
                sp = ox.distance.shortest_path(rn, n0, n1)
                if sp is not None:
                    path.extend(sp[1:])
    return path
```

在对先前的匹配调用此函数后，我们应该将轨迹完全重建为一系列道路网络边。通过将轨迹位置叠加到修补后的道路网络边上，我们可以看到一个连续的结果，如下方的**图 2**所示。

![](../Images/56b1a9dfacfdf059db7364b05c4ec5ea.png)

**图 2** — 上面的地图显示了重建的链接序列与原始轨迹位置叠加在一起。（图片来源：作者使用 Folium 和 OpenStreetMap 图像）

## 最小距离的案例

在审查这个解决方案时，我发现了一个在迭代各个轨迹时的问题。下方的**图 3**详细描述了这个问题。

![](../Images/e03eb82f3222d1dc3d7142ff6ad95ca3.png)

**图 3** — 上面的图像显示了一个角落案例，在这个道路网络边匹配系统崩溃时发生。轨迹的一个位置正好落在某个节点上，从而欺骗了匹配过程。（图片来源：作者使用 Folium 和 OpenStreetMap 图像）

路由中的虚假循环源于修补过程中的输入数据中的意外值。我们可以通过返回显示算法如何将这些 GPS 位置匹配到道路网络边的视图来深入了解发生了什么。

下方的**图 4**说明了在叉路口的情况，其中匹配的 GPS 位置与道路网络节点重合（或非常接近）。这可能会导致一些数值不稳定性，产生错误结果。一个稳健的算法应该避免这些情况，但如何做到呢？

![](../Images/d8e93483a483ccf1bd37d0294400eefb.png)

**图 4** — 一旦看到位置分配，匹配问题变得明显。显然，在上述情况下，GPS 位置非常接近某个道路网络节点，这导致了边选择过程中的不稳定性。（图片来源：作者使用 Folium 和 OpenStreetMap 图像）

考虑后，我决定更改两个匹配算法，并使用与最近的道路网络节点的距离作为包含或排除输入位置的标准。如果这个距离小于给定的阈值（默认为一米或三英尺），匹配函数返回一个空值，通知调用代码无法安全地分配一个链接。这对接下来的步骤是可以的，因为它应该推断缺失的链接，因为该算法仅依赖于道路网络边，而不是输入位置。

现在我们已经对整个算法进行了清理，让我们看看它在整个数据集上的表现。

# 性能评估

为了评估算法在整个EVED上的表现，我创建了一个新的独立[脚本](https://github.com/joaofig/eved-explore/blob/main/calculate-matches.py)，它遍历所有轨迹，匹配边缘，生成路径，并比较原始轨迹长度与重建路径的长度。Python脚本记录了所有的差异，以便我们可以使用标准统计工具集进行后续分析。

脚本的核心在于其主循环，详见下面的代码。

```py
def process_trajectories():
    rn = download_network()
    road_network = RoadNetwork(rn)

    state = load_state()
    if state is None:
        state = {
            "trajectories": get_trajectories(),
            "errors": []
        }

    save_counter = 0
    trajectories = state["trajectories"]
    while len(trajectories) > 0:
        trajectory_id = trajectories[0]
        trajectory = load_trajectory_points(trajectory_id,
                                            unique=True)
        if len(trajectory) > 3:
            edges = match_edges(road_network, trajectory)
            path = build_path(rn, edges)

            if len(path) > 0:
                diff = calculate_difference(rn, path, trajectory)
                print(f"Trajectory: {trajectory_id}, Difference: {diff}")
                state["errors"].append((trajectory_id, diff))

        trajectories = trajectories[1:]
        state["trajectories"] = trajectories

        save_counter += 1
        if save_counter % 100 == 0:
            save_state(state)

    save_state(state)
```

该函数首先从OpenStreetMap下载并预处理道路网络数据。由于这是一个长时间运行的脚本，我添加了一个持久化状态，序列化为JSON格式的文本文件，并保存了每100条轨迹。这一功能允许用户中断Python脚本而不会丢失已处理轨迹的所有数据。计算原始输入轨迹与重建路径之间长度差异的函数如下所示。

```py
def calculate_difference(rn, path, trajectory):
    p_loc = np.array([(rn.nodes[n]['y'], rn.nodes[n]['x']) for n in path])
    t_loc = np.array([(t[0], t[1]) for t in trajectory])

    p_length = vec_haversine(p_loc[1:, 0], p_loc[1:, 1], 
                             p_loc[:-1, 0], p_loc[:-1, 1]).sum()
    t_length = vec_haversine(t_loc[1:, 0], t_loc[1:, 1], 
                             t_loc[:-1, 0], t_loc[:-1, 1]).sum()
    return p_length - t_length
```

该代码将两个输入转换为地理位置序列，计算它们的累积长度，并最终计算它们的差异。当脚本完成时，我们可以加载状态数据并使用Pandas进行分析。

我们可以在下面的**图5**中看到两个距离之间的差异分布（标记为"*error*"）。

![](../Images/930d63837c65b13b0933007d02a7c6c3.png)

**图5** — 上面的直方图展示了原始输入轨迹与重建路径之间的距离差异的分布。横轴显示距离（以米为单位）。（图像来源：作者）

为了更详细地查看零附近的情况，我裁剪了直方图，并创建了**图6**。

![](../Images/0283303813be15f3d81659363e2037da.png)

**图6** — 上面的直方图详细说明了差异分布，突出了负差异和大多数正差异。大多数差异很小，暗示匹配算法表现良好。（图像来源：作者）

如您所见，光谱的负部分存在一些差异。不过，它主要是正的，这意味着重建路径的长度通常比轨迹更长，这是可以预期的。请记住，轨迹点很少与道路网络节点匹配，这意味着最常见的情况应该是在一个段的中间匹配。

我们还可以通过绘制箱线图来检查差异的中间五十个百分点，如下图**图7**所示。

![](../Images/c4bb17c27c5477ac28a5f10c27367ab2.png)

**图7** — 上面的箱线图展示了分布中间50%的位置。25%、50%和75%的百分位数分别为13.0、50.2和156.5米。我们还可以看到，内点带相对较窄，范围从-202.3到371.8米。（图像来源：作者）

上图包含了有关差异分布和异常值数量的关键信息。使用[Tukey’s fences](https://en.wikipedia.org/wiki/Outlier#Tukey's_fences)方法，我们可以将32,528条轨迹中的5,332²条标记为异常值。这一比例代表了相对较高的16.4%值，这意味着可能有更好的重建方法，或者存在大量处理不当的轨迹。我们可以查看一些最剧烈的距离差异异常值，试图理解这些结果。

## 异常值分析

如前所述，有超过五千条轨迹匹配效果较差。我们可以快速检查其中的一些，来确定采样轨迹与重建之间的差异为何如此显著。我们从一个揭示性的案例开始，即**图 8**中的轨迹编号 59。

![](../Images/bf48d4ed1c398976fcdff8eb5f292408.png)

**图 8** — 上图所示的轨迹展示了一个地图匹配错误，导致重建过程生成了一条比必要的路径更长的路径。（图片来源：作者使用 Folium 和 OpenStreetMap 图像）

上述案例表明，地图匹配过程错误地将三个 GPS 样本放置在了一条平行的道路上，迫使重建过程找到一个虚假的绕行路径。这种错误可能由于地图匹配过程的参数设置不当造成，需在后续文章中进一步研究。

最后，我们可以看到下方的**图 9**中的一个非常极端的案例。采样的 GPS 位置距离道路如此之远，以至于地图匹配过程放弃了。正如你所见，重建过程分配了与预期轨迹无关的道路网络链接。

![](../Images/d4a3f7d4817474b1d51dec756747a11f.png)

**图 9** — 上图中的轨迹包含许多错误，初始的地图匹配过程无法消除这些错误。（图片来源：作者使用 Folium 和 OpenStreetMap 图像）

# 结论

在本文中，我修正了先前发布的道路网络边缘匹配代码，并使用 EVED 数据集评估了其性能。我使用专门的 Python 脚本收集了性能评估数据，揭示了匹配轨迹中测量距离似乎异常偏差较高的情况。为了了解为何结果可以更好，我将接下来转向原始的地图匹配软件，[Valhalla](https://github.com/valhalla/valhalla)，以尝试修复上述问题。

# 注释

1.  原论文作者将数据集以 Apache 2.0 许可证授权（请参阅[VED](https://github.com/gsoh/VED)和[EVED](https://github.com/zhangsl2013/eVED) GitHub 仓库）。请注意，这也适用于衍生作品。

1.  所有图像和计算的来源均在一个 [Jupyter notebook](https://github.com/joaofig/eved-explore/blob/main/08-road-network-analysis.ipynb) 中，来自公共 [GitHub 仓库](https://github.com/joaofig/eved-explore)。

# 参考文献

[1] Zhang, S., Fatih, D., Abdulqadir, F., Schwarz, T., & Ma, X. (2022). 扩展车辆能源数据集（eVED）：用于深度学习的车辆旅行能耗的大规模增强数据集。*ArXiv*. [https://doi.org/10.48550/arXiv.2203.08630](https://doi.org/10.48550/arXiv.2203.08630)

[2] Boeing, G. 2017\. [OSMnx：获取、构建、分析和可视化复杂街道网络的新方法](https://geoffboeing.com/publications/osmnx-complex-street-networks/)。*Computers, Environment and Urban Systems* 65, 126–139\. doi:10.1016/j.compenvurbsys.2017.05.004

João Paulo Figueira 在葡萄牙里斯本的 [tb.lx by Daimler Trucks and Buses](https://tblx.io/) 担任数据科学家。
