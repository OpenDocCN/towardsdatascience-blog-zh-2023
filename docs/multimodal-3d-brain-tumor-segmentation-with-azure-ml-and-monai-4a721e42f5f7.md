# 使用 Azure ML 和 MONAI 的多模态 3D 脑肿瘤分割

> 原文：[https://towardsdatascience.com/multimodal-3d-brain-tumor-segmentation-with-azure-ml-and-monai-4a721e42f5f7?source=collection_archive---------6-----------------------#2023-03-21](https://towardsdatascience.com/multimodal-3d-brain-tumor-segmentation-with-azure-ml-and-monai-4a721e42f5f7?source=collection_archive---------6-----------------------#2023-03-21)

## 在企业级 ML 平台上大规模运行医学影像框架

[](https://medium.com/@andreaskopp_89294?source=post_page-----4a721e42f5f7--------------------------------)[![Andreas Kopp](../Images/a3184ebc20f577c933a0e9a74eb0c291.png)](https://medium.com/@andreaskopp_89294?source=post_page-----4a721e42f5f7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4a721e42f5f7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4a721e42f5f7--------------------------------) [Andreas Kopp](https://medium.com/@andreaskopp_89294?source=post_page-----4a721e42f5f7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe712cdda5a0c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultimodal-3d-brain-tumor-segmentation-with-azure-ml-and-monai-4a721e42f5f7&user=Andreas+Kopp&userId=e712cdda5a0c&source=post_page-e712cdda5a0c----4a721e42f5f7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4a721e42f5f7--------------------------------) ·14 分钟阅读·2023年3月21日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4a721e42f5f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultimodal-3d-brain-tumor-segmentation-with-azure-ml-and-monai-4a721e42f5f7&user=Andreas+Kopp&userId=e712cdda5a0c&source=-----4a721e42f5f7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4a721e42f5f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultimodal-3d-brain-tumor-segmentation-with-azure-ml-and-monai-4a721e42f5f7&source=-----4a721e42f5f7---------------------bookmark_footer-----------)

作者：[Harmke Alkemade](https://www.linkedin.com/in/harmke-alkemade/) 和 [Andreas Kopp](https://www.linkedin.com/in/andreas-kopp-1947183/)

![](../Images/be12bbfcd17e35b5f16c263d1eb3b757.png)

3D 脑肿瘤分割（*图片来源于 Shutterstock，授权给 Andreas Kopp*）

我们感谢来自 NVIDIA 和 MONAI 团队的 Brad Genereaux、Prerna Dogra、Kristopher Kersten、Ahmed Harouni 和 Wenqi Li，他们在该资产的开发中给予了积极支持。

自2021年12月以来，我们发布了多个示例以支持[使用Azure机器学习的医学影像](https://github.com/Azure/medical-imaging)，我们收到了热烈的反馈。兴趣和咨询进一步证明了AI如何迅速成为现代医学实践的关键方面。

![](../Images/d06607cd34f626de5c5ed94ed3287380.png)

选定的医学影像库用例（*图片5,6通过Shutterstock获得Andreas Kopp授权*）

## 3D脑肿瘤分割应用案例

今天，我们介绍一个新的医学影像资产，用于[3D脑肿瘤分割](https://github.com/Azure/medical-imaging/tree/main/3d-brain-tumor-segmentation)，这是一个解决肿瘤学领域中的挑战性用例的解决方案。我们的解决方案利用来自多个MRI模态的体积视觉输入和不同的3D胶质瘤肿瘤分割，以产生准确的肿瘤边界和子区域预测。为了处理涉及的大量影像数据，我们的资产采用了在Azure ML上使用可扩展GPU资源的并行训练。此外，我们利用了医学人工智能开放网络（MONAI），这是一个领域特定的框架，提供了最先进的工具和方法用于医学影像分析。

结合Azure机器学习和MONAI，为可扩展的机器学习开发和操作提供了宝贵的协同效应，特别是在医学影像方面的创新。

[**Azure机器学习**](https://azure.microsoft.com/en-us/products/machine-learning)是一个基于云的平台，为数据科学家和机器学习工程师提供了一个协作环境，用于大规模开发和部署机器学习模型。它提供了多种创作选项，从无需编码/低编码的自动化机器学习到使用VSCode等流行工具的代码优先。用户可以访问可扩展的计算资源进行训练和部署，使其在处理大型数据集和复杂模型时非常理想。此外，它包括MLOps功能，可以实现机器学习资产的管理、维护和版本控制，并允许自动化的训练和部署管道。该平台包括负责任的AI工具，可以帮助解释模型并减轻潜在的偏见，使其成为希望开发和部署伦理AI解决方案的企业的理想选择。

[**医学人工智能开放网络（MONAI）**](https://monai.io/)是一个基于PyTorch的开源项目，旨在用于医学影像。它提供了一套全面的工具，用于构建和部署医疗影像的AI模型：

+   **MONAI标签**用于AI辅助标记医学影像数据

+   **MONAI核心**用于训练具有领域特定功能的AI模型

+   **MONAI部署**用于打包、测试和部署医学AI应用程序。

MONAI Core 是本文描述的解决方案的重点。它原生支持常用的医学成像格式，如 Nifty 和 DICOM。它还包括字典变换等功能，以确保在复杂的变换管道中图像和分割之间的一致性。此外，MONAI Core 提供了一系列网络架构，包括像 UNETR 这样的最先进的基于 Transformer 的 3D 分割算法。

欢迎探索我们的演示，并将其作为您自己 3D 分割用例的模板（无论是医学领域还是其他领域）。

我们使用的是 2021 版本的 [脑肿瘤分割 (BraTS) 挑战](http://braintumorsegmentation.org/) 数据集。它是来自不同机构的专家标注的多模态 3D 磁共振成像 (MRI) 扫描图像的集合。MRI 是一种医学成像技术，利用磁场和无线电波的组合来可视化体内结构。通过改变成像参数可以生成不同的 MRI 模态，从而产生不同的组织对比度，使得某些特征在图像中更为突出。BraTS 数据集使用了以下模态：

1.  原生 T1 (**T1**)：可用于区分各种组织类型和病理状态。原生 T1 通常用于提取有关组织特性的定量信息。

1.  T1加权对比剂钆 (**T1Gd**)：这种模态可用于划分肿瘤边界和识别活跃肿瘤生长区域。

1.  T2加权 (**T2**)：这些图像对检测水肿（液体积聚）、炎症和其他可能与肿瘤相关的脑组织变化非常有用。

1.  T2加权液体衰减反转恢复 (**T2-FLAIR**)：T2-FLAIR 图像对于识别侵袭性肿瘤边缘和非增强肿瘤成分非常有用。

以下插图提供了 BraTS 数据集中专家标注的肿瘤分割示例：肿瘤核心、整体肿瘤和增强结构。左上角的图像结合了这三种分割。

![](../Images/a00f76d7a1dc145e7f92caf8ee1b43e3.png)

从 BraTS 2021 数据集中提取的脑肿瘤分割（作者提供的图像）

能够区分这些结构对于诊断、预后和治疗规划至关重要。在 BraTS 数据集中，整体肿瘤指的是肿瘤的全部体积，包括核心部分和任何周围的水肿或肿胀。核心是肿瘤内部含有最具侵袭性的癌细胞的区域，即坏死和活跃的肿瘤。增强结构是指肿瘤区域在 MRI 扫描中出现明亮并在对比剂作用下增强的部分。这一区域与最活跃和侵袭性的癌细胞相关。

## 付诸实践

现在，我们已经对使用案例有了概述，让我们来看一下如何使用 Azure Machine Learning 和 MONAI 来训练和部署一个机器学习模型以完成这个有趣的任务。

这个使用案例的端到端工作流程在我们的 [编排笔记本](https://github.com/Azure/medical-imaging/blob/main/3d-brain-tumor-segmentation/3d-brain-tumor-seg-BRATS2021.ipynb) 中实现。这些是主要步骤及其相关输出：

![](../Images/0ade5cb0596b1028c7d46d2e2f823dcc.png)

脑肿瘤分割工作流程及输出（图片由作者提供）

第一步涉及下载和可视化数据，然后提交训练作业到 Azure ML 计算集群。模型训练完成并注册后，它被部署为 Azure ML 管理的端点。笔记本最后通过可视化验证集中的图像体上的模型预测来结束。训练和推理脚本单独存储在仓库中。

![](../Images/04290d9732eb031470492ec0e52b98a0.png)

在 itkwidgets 3D 查看器中可视化肿瘤核心（图片由作者提供）

为了在我们的工作流程中管理机器学习资产，如数据集、计算资源、实验和结果模型，我们使用 Azure ML SDK 创建与 Azure ML 工作区的连接。这个连接可以在任何 Python 环境中建立，无需运行 Azure ML 计算实例。通过使用 Azure ML 管理资产，它们可以在 Azure ML Studio 中找到以备将来参考。这有助于轻松比较不同的实验，并通过引用相关资产（数据集版本、训练代码、环境等）创建结果模型的沿袭。

数据是通过 Kaggle API 从 Kaggle 下载的，并注册为 Azure ML 管理的数据集。这有助于对训练数据进行版本控制，这在训练数据可能随时间变化的迭代模型开发场景中尤为重要。例如，BraTS 数据集自 2012 年以来持续更新。将数据版本与模型关联可以确保良好的治理。

[训练脚本](https://github.com/Azure/medical-imaging/blob/main/3d-brain-tumor-segmentation/src/train-brats21.py) 使用 MONAI 框架以及原生 PyTorch 类。数据使用 MONAI 框架提供的多个转换进行预处理，主要使用基于字典的包装器。训练图像的不同 MRI 模态被存储为单独的文件，分割掩膜则有一个文件。基于字典的包装器允许对所有训练数据使用单一的预处理管道，其中管道中的每个转换都可以指定应用于数据的哪一部分。

以下示例说明了 MONAI 字典变换在确保图像与相关分割标签之间的一致性方面的好处。假设我们使用一个随机变换的管道来进行数据增强，这些变换会影响图像的形状或位置（如调整大小、翻转、旋转、透视调整等）。我们必须确保对分割图像应用相同的变换，以确保图像和标签之间的一致性，便于后续的训练过程。在这种情况下，MONAI 字典变换是一个便利的工具：我们只需指定变换应应用于图像、标签还是两者。在这个例子中，我们将变换应用于两个对象。MONAI 还确保在随机变换的情况下保持一致性。

![](../Images/c905e76650b5865e7727d15d95bf05d8.png)

随机水平翻转的字典变换示例（图像由作者提供）

MONAI 提供了一系列基于 PyTorch 的网络架构。我们选择了 SegResNet（一个为分割任务适配的 ResNet 架构变体）来完成我们的任务。它基于编码器-解码器框架，其中编码器从输入图像中提取特征，解码器重建输出分割掩模。

```py
# Specify SegResNet for 3D segmentation
segresnet = SegResNet(
    blocks_down=[1, 2, 2, 4],
    blocks_up=[1, 1, 1],
    init_filters=16,
    in_channels=4,
    out_channels=3,
    dropout_prob=0.2,
).to(device)

# Wrap network for DDP
model = torch.nn.parallel.DistributedDataParallel(module=segresnet, device_ids=[local_rank])

# Specify Dice loss, optimizer and LR scheduler
loss_function = DiceLoss(smooth_nr=0, smooth_dr=1e-5, squared_pred=True, to_onehot_y=False, sigmoid=True)
optimizer = torch.optim.Adam(model.parameters(), initial_lr, weight_decay=1e-5)
lr_scheduler = torch.optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=max_epochs)
```

用于下采样的编码器块数量由 `blocks_down` 参数指定。因此，解码器部分的上采样块由 `blocks_up` 参数定义。第一个卷积层使用了 16 个滤波器。随着网络的深入，数量通常会翻倍，以便学习更复杂的特征。网络接受 4 个通道的输入，由 `in_channels=4` 指定。这代表了我们的多模态 MRI 数据，每个通道对应不同的 MRI 模态：T1、T1Gd、T2 和 T2-FLAIR。网络的输出具有 3 个通道，由 `out_channels=3` 指定。这对应于我们分割任务的类别：整个肿瘤、肿瘤核心和增强结构。

使用 PyTorch 原生特性以支持多 GPU 的分布式数据并行（DDP）训练。

对于损失函数，我们利用了 MONAI 的 Dice 损失。它是用来衡量预测分割与真实标签之间相似度的常用损失函数。它在类分布不均的情况下特别有用，例如医学图像分割，其中目标区域（如肿瘤）通常比背景小得多。

Adam 优化器与余弦退火学习率调度器（CosineAnnealingLR）一起使用。该调度器使学习率在初始值和指定的最小值之间振荡。CosineAnnealingLR 通过使模型在训练的初期阶段能够逃离局部最小值，并在后期阶段进行精确调整，从而帮助提高模型性能。

训练脚本通过命令作业提交到 Azure ML 计算集群，使用以下定义：

```py
job= command(
    inputs= {"input_data": Input(type=AssetTypes.URI_FOLDER, path= dataset_asset.path)},
    code= 'src/',
    command= "python train-brats21.py --epochs 50 --initial_lr 0.00025 --train_batch_size 1 --val_batch_size 1 --input_data ${{inputs.input_data}} --best_model_name BRATS21",
    environment= "monai-multigpu-azureml@latest", 
    compute= train_target,
    experiment_name= experiment,
    display_name= f"3d brain tumor segmentation based on BRATS21",
    description= "## Brain tumor segmentation on 3D MRI brain scans",
    shm_size= '300g',
    resources= dict(instance_count= 1), # cluster nodes 
    distribution= dict(type="PyTorch", process_count_per_instance= 4), # GPUs per node
    environment_variables= dict(AZUREML_ARTIFACTS_DEFAULT_TIMEOUT = 1000),
    services= {
    "My_jupyterlab": JobService(job_service_type="jupyter_lab"),
    "My_vscode": JobService(job_service_type="vs_code",),
    "My_tensorboard": JobService(job_service_type="tensor_board",),
        })

returned_job= ml_client.create_or_update(job)
```

当我们提交训练作业时，会在计算集群中启动一个容器，使用我们之前在笔记本中定义的环境“monai-multi-gpu”。这个环境是从包含训练脚本要求的 conda 规范文件构建的。生成的 Docker 镜像被缓存到链接到 Azure ML 工作区的 Azure 容器注册表中。

通过使用`distribution` 参数，我们启动了分布式数据并行训练的进程数量。由于我们在这次实验中使用了一台四 GPU 的机器，因此启动了四个进程，每个进程访问自己的 GPU 实例。此外，我们还可以通过使用`resources` 参数在计算集群的不同节点上并行化训练运行。

了解 GPU 利用率对于优化资源使用和防止在长时间的 PyTorch 训练过程中出现的内存溢出（OOM）错误至关重要。Azure ML 提供了一系列指标来跟踪网络和磁盘 I/O，以及 CPU 和 GPU 内存和处理器利用率。以下示例演示了在 Standard_NC24rs_v3 类型的多 GPU 集群节点上训练运行开始时的 GPU 内存和能耗情况。在这种情况下，我们可以看到四个 GPU 上每个 GPU 的 16 GB 内存正在有效利用。剩余的头部空间很小，这清楚地表明增加批量大小可能会提高遇到 OOM 错误的风险。此外，Azure ML 还允许我们监控每个 GPU 的能耗，以千焦耳为单位进行测量。

![](../Images/26e1fcb5788b50ab98cf8494bc6005a9.png)

监控训练期间的多 GPU 资源消耗（图像由作者提供）

使用大量数据集进行训练可能需要相当长的时间，可能需要几个小时甚至几天才能完成。使用像 Azure ML 这样的基于云的数据科学和机器学习平台的好处在于能够根据需要按需访问强大的资源。

对于我们的最终模型，我们使用了 NC96ads_A100_v4 集群节点，配备 96 核心、880 GB CPU 内存以及每个四个 NVIDIA A100 GPU 上 80 GB 内存，以训练超过 150 个 epoch。这种设置的主要优势是资源按需可用：一旦训练完成，集群会自动关闭，消除了任何进一步的费用。

在我们的案例中，我们选择了“低优先级” SKU，相比专用计算资源，它提供了更具成本效益的解决方案。然而，这种选择存在风险，即如果计算资源被用于其他任务，正在运行的训练作业可能会被抢占。

在训练作业运行时，JupyterLab、VSCode 和 Tensorboard 在容器中运行，使其可以进行监控和调试。这在 `services` 参数中指定。我们在训练脚本中使用 Tensorboard 和 MLFlow 记录训练指标，并使用 MLFlow 注册最终模型。由于 Azure ML 中对 MLFlow 的本机支持，指标和模型都可以在 Azure ML Studio 的实验部分找到。通过访问运行在容器中的 Tensorboard 实例，我们可以在模型训练时查看训练和验证指标。

![](../Images/a6777327e55b4160b10d16f897962e11.png)

使用 Tensorboard 进行交互式作业的训练性能监控（图像由作者提供）

Dice 系数用于评估模型性能，是一种常用于对象分割模型的度量指标。它衡量预测分割掩膜与真实数据掩膜之间的相似性。Dice 系数为 1.0 表示预测掩膜与真实数据掩膜完全重叠。对数据集中不同类别的 Dice 指标进行跟踪：肿瘤核心 (*val_dice_tc*)、整体肿瘤 (*val_dice_wt*) 和增强结构 (*val_dice_et*)。还跟踪不同类别的平均 Dice 指标 (*val_mean_dice*)。进行 150 个 epochs 的实验得到以下指标：

![](../Images/14a154808c996fae0e64eda9ef596375.png)

在 Azure ML Studio 中进行 150 个 epoch 训练期间的验证 DICE 指标（图像由作者提供）

注册模型后，我们进入部署阶段，这是使模型在生产环境中可用于预测新 MRI 图像的分割掩膜的一种方式。一个可能的脑肿瘤分割部署场景是将其与 MRI 图像查看器集成，以提供诊断辅助。在笔记本中，使用了 Azure ML 管理端点来进行部署。首先，创建一个管理在线端点，它提供了发送请求和接收模型推断输出的接口，并提供认证和监控功能。然后，我们通过定义模型、评分脚本、环境和目标计算类型来创建一个部署。我们参考了工作区中最新注册的 BraTS 模型进行此部署。

在将四种模态的二进制编码版本的 MRI 图像堆栈发送到我们的端点后，我们会收到作为 JSON 响应的一部分的预测分割结果。编排笔记本的最后部分包含了可视化这些预测的代码。我们的图像滑块将每个切片的预测分割结果与真实数据并排显示，以便进行比较。

![](../Images/9de27c8554fd29c9ed43f12a41d92d44.png)

比较专家标注的（真实数据）与预测的肿瘤结构（图像由作者提供）

## 人工智能在医疗保健中的负责任使用

将机器学习融入临床实践，例如肿瘤检测和分析，带来了诸多好处。例如，算法可以通过标记每日生成的海量图像中的可疑区域来辅助放射科医生。另一个应用是对肿瘤进展进行持续监测，以评估癌症治疗的效果。从每位患者的研究中计算出超过一百张切片的肿瘤体积可能非常耗时，但 AI 辅助测量可以显著加快这一过程。

尽管 AI 辅助图像分析取得了显著进展，但仍可能出现错误。未被检测出的肿瘤是一个重大问题，训练数据中稀有肿瘤或代表性不足的患者特征可能导致此类假阴性。因此，必须通过纳入多样化的数据集和不断完善算法来解决这些限制。

医生必须继续对诊断和治疗决策负责。机器学习预测应被视为有价值的支持工具，而不是可以替代人类专业知识的绝对替代品。医生始终处于整个过程的前沿，确保患者护理同时受到技术和专业判断的指导。

## 结论

在这篇文章中，我们探讨了 Azure 机器学习和 MONAI 在 3D 脑肿瘤分割中的应用，这是肿瘤学领域中的一个挑战性用例。我们讨论了 MRI 图像的各种模态以及区分肿瘤核心、整个肿瘤和增强结构在诊断、预后和治疗计划中的重要性。我们还概述了这一用例的端到端工作流程，包括数据可视化、模型训练和注册、部署以及模型预测的可视化。使用 Azure 机器学习和 MONAI 使我们能够利用可扩展的 GPU 资源进行并行训练，并为医疗成像中的 AI 模型训练提供了特定领域的能力。

我们希望 [我们的资产](https://github.com/Azure/medical-imaging/tree/main/3d-brain-tumor-segmentation) 能作为未来医学领域 3D 分割用例的模板，并为现代医学实践中 AI 的日益重要性做出贡献。

该资产的训练代码基于 [MONAI 3D 脑肿瘤分割教程](https://github.com/Project-MONAI/tutorials/blob/main/3d_segmentation/swin_unetr_brats21_segmentation_3d.ipynb) 的早期版本。

## 参考文献

[1]: Hatamizadeh, A., Nath, V., Tang, Y., Yang, D., Roth, H. and Xu, D., 2022\. Swin UNETR: Swin Transformers for Semantic Segmentation of Brain Tumors in MRI Images. arXiv preprint arXiv:2201.01266.

[2] Tang, Y., Yang, D., Li, W., Roth, H.R., Landman, B., Xu, D., Nath, V. 和 Hatamizadeh, A., 2022\. 自监督预训练的 swin 变换器用于三维医学图像分析。见于 IEEE/CVF 计算机视觉与模式识别会议论文集（第 20730–20740 页）。

[3] U.Baid 等人，RSNA-ASNR-MICCAI BraTS 2021 脑肿瘤分割和放射基因组分类基准，arXiv:2107.02314，2021。

[4] B. H. Menze, A. Jakab, S. Bauer, J. Kalpathy-Cramer, K. Farahani, J. Kirby 等人，“多模态脑肿瘤图像分割基准（BRATS）”，《IEEE 医学影像学报》34(10)，1993–2024（2015）DOI: 10.1109/TMI.2014.2377694

[5] S. Bakas, H. Akbari, A. Sotiras, M. Bilello, M. Rozycki, J.S. Kirby 等人，“通过专家分割标签和放射组学特征推动癌症基因组图谱胶质瘤 MRI 收集”，《自然科学数据》，4:170117（2017）DOI: 10.1038/sdata.2017.117

[6] S. Bakas, H. Akbari, A. Sotiras, M. Bilello, M. Rozycki, J. Kirby 等人，“TCGA-GBM 收集的术前扫描的分割标签和放射组学特征”，癌症影像档案，2017\. DOI: 10.7937/K9/TCIA.2017.KLXWJJ1Q
