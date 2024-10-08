# 映射全球自然再造林项目的潜力

> 原文：[`towardsdatascience.com/mapping-the-global-potential-of-natural-reforestation-projects-c425d2998fa5`](https://towardsdatascience.com/mapping-the-global-potential-of-natural-reforestation-projects-c425d2998fa5)

## 使用地面观测、遥感和机器学习

[](https://steve-klosterman.medium.com/?source=post_page-----c425d2998fa5--------------------------------)![Steve Klosterman](https://steve-klosterman.medium.com/?source=post_page-----c425d2998fa5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c425d2998fa5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c425d2998fa5--------------------------------) [Steve Klosterman](https://steve-klosterman.medium.com/?source=post_page-----c425d2998fa5--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c425d2998fa5--------------------------------) ·阅读时长 11 分钟·2023 年 8 月 21 日

--

作者：Stephen Klosterman 和 Earthshot 科学团队。内容最初在[2022 年 12 月美国地球物理联盟秋季会议](https://agu.confex.com/agu/fm22/meetingapp.cgi/Paper/1185690)上展示。

# 介绍

生态恢复项目通常需要投资以启动活动。为了为森林增长和保护项目创造碳融资机会，必须能够预测木质生物量中的碳积累，或在防止森林砍伐的情况下避免的排放。此外，还需要尝试理解广泛的其他生态系统属性的可能变化，例如植物和动物物种组成及水质。为了创建碳积累预测，常见的方法是对特定地点的项目给予个别关注和研究努力，这些项目可能分布在全球各地。因此，拥有一张局部准确且全球适用的生长率或其他感兴趣参数值的地图，将便于快速“勘探”生态系统恢复机会。在这里，我们描述了创建这样的地图的方法，该地图源于基于先前发布的文献综述数据训练的机器学习模型。[随后，我们展示了如何在 Google Earth Engine 应用中实施该地图](https://steve-klosterman.users.earthengine.app/view/africaglobalunrmodel)。

# 数据与方法

我们使用了一份[最近发布的数据集](https://zenodo.org/record/3983644#.Y7hoXezMJ80)，该数据集包含森林群体生物量测量值、年龄和地理位置（Cook-Patton et al. 2020），用于训练一个机器学习模型，以预测常用的查普曼-理查兹（CR）生长函数的一个参数。

在清理了异常值和不现实观测数据后，我们剩下了大约 2000 个观测值，如下图所示，符号大小与每个地点的观测数量成正比：

![](img/dbf3aa05b3f28ce82d88afd3768d992e.png)

基于地点的全球数据分布；符号大小与每个地点的测量数量成正比。作者提供的图片。

观测数据分布在 390 个地点。大多数地点（64%）只有一个测量点，而有一个地点有 274 个测量点。

Cook-Patton 等（2020）将这些数据与来自美国和瑞典的其他库存数据结合，创建了全球碳积累速率地图。然而，该工作假设了一个线性碳积累模型，而有更多生物学上更为现实的替代模型。这里我们展示了如何为曲线拟合方程（CR 函数）创建全球参数层。我们的方法类似于 Chazdon 等（2016）的工作，创建了拉丁美洲热带地区 Michaelis-Menten 方程的参数值地图。然而，这里我们没有将曲线拟合参数模型限制为环境协变量的线性组合，而是将曲线拟合参数作为统计学习方法（XGBoost）的响应变量，以捕捉特征之间的任何非线性或交互行为。我们选择用 CR 方程表示生长，因为它是一个灵活的曲线，可以呈现 S 形或对数形状，是林业行业中一种简单而生物学上现实的树木生长模型的标准方法（Bukoski 等，2022）：

![](img/ad4b487d2eb925568867504c54c8d1f7.png)

Chapman-Richards（CR）方程。作者提供的图片。

其中*y*是时间*t*的生物量，*y_max*是森林成熟时的生物量上限，*b*控制时间 0 时的生物量，*m*是形状参数，*k*是我们估计的生长参数。在这项工作中，我们假设*b* = 1，将生物量限制为零，*m* = ⅔，与类似工作中的做法一致（Bukoski 等，2022）。这就只剩下*y*、*y_max*、*t*和*k*作为未知量。以下是几个示例 CR 曲线，*m* = ⅔，*b* = 1，*y_max* = 1，和不同的*k*值：

![](img/bab3b6ca568cc96b83eb766e5613ef03.png)

作者提供的图片

参数*k*明显影响达到接近最大潜在生物量水平所需的时间：

![](img/4090d1a6f9b050a77b9e954f027b4d86.png)

作者提供的图片

我们从 Walker 等（2022）的潜在生物量图中提取了相关地点的*y_max*值。与这篇出版物一起提供的地图包括了当前和最大潜在生物量的预测，涵盖了各种假设和条件。特别是，我们使用了[这里](https://doi.org/10.7910/DVN/DSDDQK)提供的 Base_Pot_AGB_MgCha_500m.tif 地图。通过为每个测量点估算最大生物量，我们将 Cook-Patton 等（2020）中的生物量和森林立地年龄的配对值分别代入*y*和*t*，并计算了该数据集中每个测量值的*k*值。以下是我们获得的*k*值分布，显示出一个广泛的范围和较长的右尾：

![](img/9b03f7f02f3694b76811541af1d1801b.png)

由作者提供的图片

在解读这些数据时，重要的是要注意，由于我们使用了树木生长数据源，我们的模型将适用于自然再生森林。最近已经完成了对单一物种种植园的文献回顾（Bukoski 等，2022），并且正在进行对农林系统（[Cook-Patton](https://www.nature.org/en-us/about-us/who-we-are/our-people/susan-cook-patton/)等，正在准备中）和多样化树种种植项目（[Werden](https://lwerden.mystrikingly.com/)等，正在准备中——请联系 Leland Werden（leland@crowtherlab.com）参与公民科学文献回顾，特别是如果你会讲除英语外的其他语言）。一个类似的相关工作涉及对自然再生和单一栽培在缓解气候变化方面相对有效性的经济计量比较（[Busch](https://www.jonahbusch.com/)等，正在准备中）。

我们为我们的模型使用了 61 个空间显式特征，包括[生物群落](https://developers.google.com/earth-engine/datasets/catalog/RESOLVE_ECOREGIONS_2017)（在进行一次性编码后 12 个特征）、[来自 SoilGrids 项目的土壤属性](https://www.isric.org/news/soilgrids-data-now-available-google-earth-engine)（11 个特征）、[Terraclimate](https://developers.google.com/earth-engine/datasets/catalog/IDAHO_EPSCOR_TERRACLIMATE)提供的月度气候数据（1960–1990 年期间的 14 个特征）以匹配[Bioclim](https://developers.google.com/earth-engine/datasets/catalog/WORLDCLIM_V1_BIO)数据（19 个特征），以及[高程、坡度、方位和阴影](https://developers.google.com/earth-engine/apidocs/ee-terrain-products)的地形特征（4 个特征）。这些特征类似于其他研究中应用机器学习来绘制碳积累速率（Cook-Patton 等，2020）或其他相对时间不变的生态系统属性（如某地点的最大潜在生物量）（Walker 等，2022）所使用的特征。

我们使用了[XGBoost](https://xgboost.readthedocs.io/en/stable/)来构建多个回归模型以预测*k*并探索响应变量与潜在特征列表之间的关系。为了帮助解释，我们的目标之一是选择一个特征尽可能少但性能良好的模型。我们使用[SHAP](https://shap.readthedocs.io/en/latest/index.html)（SHapley Additive exPlanation）值来确定特征的重要性，并发现模型性能和选定特征的显著差异，取决于用于训练与测试的数据。因此，我们对所有模型开发使用了 10 折交叉验证，而不是留出单独的测试集。我们使用[GroupKFold](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GroupKFold.html)以确保来自同一地点的测量不会在折叠之间被分割；换句话说，没有任何一个折叠在训练数据和测试数据中都有来自同一地点的数据，从而减少模型评估中的空间自相关效应。

1.  **修剪相关预测变量：** 为了开始选择特征，我们最初根据平均绝对 SHAP 值对所有特征进行排序。为此，我们为每个 10 个训练折叠训练了一个模型，然后计算相应验证折叠的平均绝对 SHAP 值，以查看特征在训练样本之外进行预测时的重要性。我们对所有 10 个折叠中每个特征的平均绝对 SHAP 值进行了汇总，并按降序排序以进行“排名选择投票”特征选择程序。从列表顶部的特征开始，按顺序进行，我们丢弃了所有 Pearson's r > 0.8 且排名较低的预测变量，以减少多重共线性，并开始修剪特征集的过程。此步骤对最终模型性能的影响微乎其微，并将特征集减少到 41 个特征。

1.  **每折训练单独模型并结合见解：** 使用这个较小的特征集，我们对 10 个折叠中的每一个分别进行了反向选择：在每次迭代中，我们根据验证集上的平均绝对 SHAP 值对特征进行排序。根据这个排序，我们确定了特征最少的模型，其生物量估计的 RMSE（均方根误差）在 1 Mg AGB/ha（每公顷地上生物量）内，约为 1%的差异——换句话说，最简单的模型几乎“与最佳模型一样好”。由于这个步骤，不同折叠的最佳模型具有不同的特征和特征数量。为了结合各折叠的见解，我们执行了类似于前一步的排名选择投票，以确定哪些特征应该考虑用于最终模型及其排序（27 个特征）。

1.  **使用每次折叠的共同特征集进行最终特征选择：** 再次使用向后选择程序，我们检查了 10 个折叠的验证集上的均值 RMSE 和 R²，但对于每个折叠使用相同的特征集。我们发现模型验证性能在折叠间通常很嘈杂，这突显了需要额外的数据收集以开发更稳健的模型。我们还发现，相对较少特征的模型在验证得分上几乎与更多特征的模型一样好。

![](img/e372bd4d220d2a3f1584b50b791a9f05.png)

作者提供的图像

我们选择了一个具有 5 个特征的模型（最高验证 R²，RMSE 略低于第二低）。最终的向后选择（步骤 3）的结果如上所示，而最终模型的 SHAP 值（在验证集上收集）如下所示。像这里展示的 SHAP 汇总图，也称为“蜜蜂群”图，有一行代表每个特征，在每行中有一个数据集（此处为验证数据）中的每个预测点。点的颜色表示特征值，垂直偏移表示该 SHAP 值的数据密度，x 轴坐标表示 SHAP 值，或该样本对模型预测的有符号影响（Lundberg 和 Lee，2017）。SHAP 汇总图显示了特征的高值或低值是否对模型预测产生了正面或负面的影响及其大小。

![](img/560a010b540b92468fa1bf429a15a6fe.png)

作者提供的图像

为了实现这一模型以创建预测的* k *值的全图，我们利用了我们的开源库[earthshot_model_utils](https://github.com/Earthshot-Labs/earthshot_model_utils)来导出空间平铺预测因子的 CSV 文件，执行模型推断，然后将结果导出并合并成一个单一的 geotiff（见下一节）。

# 讨论与结论

与相关研究工作的比较，我们的模型表现似乎相似。交叉验证 R²的平均值（±标准误差）为 0.485 ± 0.04，而使用这些及其他数据进行训练的线性碳积累率模型的 20%保留测试集的 R²为 0.445（Cook-Patton 等，2020）。

在模型的五个特征中，最重要的特征是等温性，它表示某个区域昼夜温差相对于夏冬温差的大小。其值范围从 0 到 100，其中昼夜温差表示为年温差的百分比。这种昼夜温差与年温差的关系被认为影响物种分布，并且对“热带、岛屿和海洋环境”有用（O’Donnell 和 Ignizio，2012）。它可能是环境支持树木生长的一个指标，较大的特征值（相对于年温差较大的昼夜温差）会导致较高的*k*预测（相对于最大值的较快增长，如上图的 SHAP 值图所示）。等温性在赤道附近相对较大，年温差最小，因此也一般代表较暖的温度。从剩余的特征来看，SHAP 值清楚地表明，树木在风速较小的地方以及土壤湿度较高的地方（无论是物理上还是生物上）预测生长速度较快，这些都是模型行为上合理的。与 Palmer 干旱严重性指数和土壤碳的关系则更加复杂和互动，我们在这里不再详细讨论。

使用 [earthshot_model_utils](https://github.com/Earthshot-Labs/earthshot_model_utils)，我们使用训练好的模型在 1 公里空间分辨率下对非洲进行了推断，生成了* k* 值的大陆地图。将这些值代入 CR 公式，并结合潜在的生物量*y_max*，我们可以生成森林再生开始后的任何时间点的 AGB 地图。我们的 [Google Earth Engine 应用](https://steve-klosterman.users.earthengine.app/view/africaglobalunrmodel) 显示了生长开始后 10 年、20 年和 30 年的生物量；30 年的生物量如下图所示。这个数据产品可以用于几乎即时的自然森林再生预测，具有高空间分辨率和大范围地理尺度，能够快速评估碳项目。

![](img/165334664f3e98a8015529da0d514e9e.png)

这是对自然再生开始 30 年后的生物量的估算。图片由作者提供。

虽然我们选择使用地面数据来创建这项工作的生物量积累模型，但我们团队也在投入研究其他可能的方法。这些方法包括更多依赖遥感观测而非地面观测的统计建模方法，以及基于过程的模型。遥感指标如冠层高度和生物量使用 LiDAR（激光雷达）或 SAR（合成孔径雷达）技术进行测量，这些技术通常没有多时相组件，因此无法跟踪生物量的增长轨迹。学术研究人员和公司正在利用多时相遥感数据（如 Landsat）作为特征构建机器学习模型，以有效地在空间（例如 Walker 等人 2020 年）和时间上（如[CTREEs](https://ctrees.org/)、[Chloris Geospatial](https://www.chloris.earth/) 和[Sylvera](https://www.sylvera.com/blog/mapping-forest-structure-across-the-landscape)的努力）外推 LiDAR 测量。这开辟了广阔的可能性，但需要注意从太空评估树木大小的局限性和必要的地面验证。基于过程的建模将提供另一种有价值的视角，特别是其在建模生物多样性和其他生态系统服务细节方面的能力（参见例如 Fisher 和 Koven, 2020），并努力与遥感数据结合以提供基准数据（Ma 等人 2022）。

为了继续这里描述的工作，我们计划创建模型预测的不确定性地图，使用多种基于自助抽样的模型，以及蒙特卡洛方法来考虑最大潜在生物量的不确定性。我们计划在未来的工作中探索和比较多种建模技术，利用多个基于地面数据集的方法。

# 参考文献

Bukoski, J.J., Cook-Patton, S.C., Melikov, C. 等人. 全球单一栽培森林中地上碳积累的速率及驱动因素. 《自然通讯》13, 4206 (2022). [`doi.org/10.1038/s41467-022-31380-7`](https://doi.org/10.1038/s41467-022-31380-7)

Chazdon, R.L., Broadbent, E.N., Rozendaal, D.M.A. 等人. 拉丁美洲热带地区二次生长森林再生的碳封存潜力. 《科学进展》2, 5 (2016). [`doi.org/10.1126/sciadv.1501639`](https://doi.org/10.1126/sciadv.1501639)

Cook-Patton, S.C., Leavitt, S.M., Gibbs, D. 等人. 全球自然森林再生的碳积累潜力地图. 《自然》585, 545–550 (2020). [`doi.org/10.1038/s41586-020-2686-x`](https://doi.org/10.1038/s41586-020-2686-x)

Fisher, R.A., 和 Koven, C.D. 关于陆地表面模型未来发展及代表复杂陆地系统的挑战的观点. 《先进地球系统模型杂志》12, 4 (2020). [`doi.org/10.1029/2018MS001453`](https://doi.org/10.1029/2018MS001453)

Lundberg, S.M. 和 Lee, Su-In。《统一模型预测解释方法》。Advances in Neural Information Processing Systems 30 (2017)。 [`proceedings.neurips.cc/paper_files/paper/2017/file/8a20a8621978632d76c43dfd28b67767-Paper.pdf`](https://proceedings.neurips.cc/paper_files/paper/2017/file/8a20a8621978632d76c43dfd28b67767-Paper.pdf)

Ma, L., Hurtt, G., Ott, L. 等。《生态系统人口模型 (ED v3.0) 的全球评估》。Geosci Model Dev 15, 1971–1994 (2022)。 [`doi.org/10.5194/gmd-15-1971-2022`](https://doi.org/10.5194/gmd-15-1971-2022)

O’Donnell, M.S. 和 Ignizio, D.A. 《支持美国大陆生态应用的生物气候预测因子：美国地质调查局数据系列 691(2012)》。 [`pubs.usgs.gov/ds/691/ds691.pdf`](https://pubs.usgs.gov/ds/691/ds691.pdf)

Walker, W.S., Gorelik, S.R., Cook-Patton, S.C. 等。《陆地上碳存储增加的全球潜力》。Proc Natl Acad Sci USA 119, 23 (2022)。 [`doi.org/10.1073/pnas.2111312119`](https://doi.org/10.1073/pnas.2111312119)

*最初发布于* [*https://www.earthshot.eco*](https://www.earthshot.eco/post/mapping-the-global-potential-of-natural-reforestation-projects)*。*
