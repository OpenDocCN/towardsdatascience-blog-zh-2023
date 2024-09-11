# 从RGB视频创建3D视频

> 原文：[https://towardsdatascience.com/creating-3d-videos-from-rgb-videos-491a09fa1e79?source=collection_archive---------3-----------------------#2023-08-03](https://towardsdatascience.com/creating-3d-videos-from-rgb-videos-491a09fa1e79?source=collection_archive---------3-----------------------#2023-08-03)

## 生成一致的深度图和点云视频的指南

[](https://medium.com/@berkanzorlubas?source=post_page-----491a09fa1e79--------------------------------)[![Berkan Zorlubas](../Images/6d13c115064dfa1bf3918ef009a30797.png)](https://medium.com/@berkanzorlubas?source=post_page-----491a09fa1e79--------------------------------)[](https://towardsdatascience.com/?source=post_page-----491a09fa1e79--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----491a09fa1e79--------------------------------) [Berkan Zorlubas](https://medium.com/@berkanzorlubas?source=post_page-----491a09fa1e79--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7d74427941be&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-3d-videos-from-rgb-videos-491a09fa1e79&user=Berkan+Zorlubas&userId=7d74427941be&source=post_page-7d74427941be----491a09fa1e79---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----491a09fa1e79--------------------------------) ·8 min read·2023年8月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F491a09fa1e79&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-3d-videos-from-rgb-videos-491a09fa1e79&user=Berkan+Zorlubas&userId=7d74427941be&source=-----491a09fa1e79---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F491a09fa1e79&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-3d-videos-from-rgb-videos-491a09fa1e79&source=-----491a09fa1e79---------------------bookmark_footer-----------)![](../Images/c89b7361a6263a9248708884ab00f1d8.png)

作者提供的图片（视频帧的编辑版本，来自 [库存镜头](https://www.videvo.net/video/businessman-and-businesswoman-finishing-meeting-with-handshake/1112997/) 提供者Videvo，下载自 [www.videvo.net](http://www.videvo.net/)）

我一直对我们将数字记忆以2D格式存档这一事实感到不满——尽管照片和视频很清晰，却缺乏它们所捕捉的经历的深度和沉浸感。在机器学习模型足够强大到理解照片和视频的3D感的时代，这似乎是一个任意的限制。

来自图像或视频的3D数据不仅使我们能够更生动、互动地体验记忆，还提供了编辑和后处理的新可能性。试想能够轻松地从场景中移除对象、更换背景，甚至改变视角以从新的视点查看一个瞬间。深度感知处理也为机器学习算法提供了更丰富的上下文来理解和处理视觉数据。

在寻找生成一致深度视频的方法时，我发现了一篇[研究论文](https://github.com/google/dynamic-video-depth)，它建议了一种很好的方法。这种方法涉及使用整个输入视频训练两个神经网络：一个卷积神经网络（CNN）来预测深度，一个MLP来预测场景中的运动或“场景流”。这个流预测网络以特殊的方式应用，在不同时间段内重复应用。这使它能够识别场景中的小变化和大变化。小变化有助于确保3D中的运动从一个时刻到下一个时刻是平滑的，而大变化有助于确保整个视频在不同视角下是一致的。这样，我们可以创建既局部又全球准确的3D视频。

这篇论文的[代码库](https://github.com/google/dynamic-video-depth)是公开的，但处理任意视频的管道并没有完全解释，至少对我来说，如何使用所提议的管道处理任何视频仍然不清楚。在这篇博客文章中，我将尝试填补这个空白，并逐步介绍如何在你的视频上使用这个管道。

你可以查看我在[GitHub](https://github.com/berkanz/dynamic-video-depth)页面上的代码版本，我将会参考这个版本。

# 第1步：从视频中提取帧

在管道中的第一步是从选择的视频中提取帧。我添加了一个脚本用于这个目的，你可以在`scripts/preprocess/custom/extract_frames_from_video.py`中找到。要运行代码，只需在终端中使用以下命令：

```py
python extract_frames_from_video.py ^
  -- video_path = 'ENTER YOUR VIDEO PATH HERE' ^
  -- output_dir = '../../../datafiles/custom/JPEGImages/640p/custom/' ^
  -- resize_factor = 0.5
```

使用`resize_factor`参数，你可以对帧进行下采样或上采样。

我选择了[这个](https://medium.com/r?url=https%3A%2F%2Fwww.videvo.net%2Fvideo%2Fbusinessman-and-businesswoman-finishing-meeting-with-handshake%2F1112997%2F)视频进行测试。最初，它的分辨率是1280x720，但为了加快后续步骤的处理速度，我将其缩小到640x360，使用了0.5的`resize_factor`。

# 第2步：在视频中分割前景对象

我们过程中的下一步需要对视频中的一个主要前景物体进行分割或隔离，这对估计相机在视频中的位置和角度至关重要。原因是？离相机较近的物体对姿态估计的影响比离相机较远的物体要大。举个例子，想象一个距离1米远的物体移动10厘米——这将导致图像发生较大的变化，可能是几十个像素。但如果同样的物体距离10米远且移动相同的距离，图像变化则不那么明显。因此，我们生成了一个‘遮罩’视频，以便关注对姿态估计相关的区域，简化我们的计算。

我偏好使用[Mask-RCNN](https://github.com/matterport/Mask_RCNN)来分割帧。你也可以使用其他你喜欢的分割模型。对于我的视频，我决定对右侧人物进行分割，因为他在整个视频中都出现在画面中，并且看起来离相机足够近。

要生成遮罩视频，需要对你的视频进行一些特定的手动调整。由于我的视频中包含两个人物，我首先对这两个人物进行了遮罩分割。之后，我通过硬编码提取了右侧人物的遮罩。根据你选择的前景物体及其在视频中的位置，你的方法可能会有所不同。负责创建遮罩的脚本可以在`./render_mask_video.py`中找到。我指定遮罩选择过程的脚本部分如下：

```py
 file_names = next(os.walk(IMAGE_DIR))[2]
    for index in tqdm(range(0, len(file_names))):
        image = skimage.io.imread(os.path.join(IMAGE_DIR, file_names[index]))
        # Run detection
        results = model.detect([image], verbose=0)
        r = results[0]
        # In the next for loop, I check if extracted frame is larger than 16000 pixels, 
        # and if it is located minimum at 250th pixel in horizontal axis.
        # If not, I checked the next mask with "person" mask.
        current_mask_selection = 0
        while(True):
            if current_mask_selection<10:
                if (np.where(r["masks"][:,:,current_mask_selection]*1 == 1)[1].min()<250 or 
                    np.sum(r["masks"][:,:,current_mask_selection]*1)<16000):
                    current_mask_selection = current_mask_selection+1
                    continue
                elif (np.sum(r["masks"][:,:,current_mask_selection]*1)>16000 and 
                      np.where(r["masks"][:,:,current_mask_selection]*1 == 1)[1].min()>250):
                    break
            else:
                break
        mask = 255*(r["masks"][:,:,current_mask_selection]*1)
        mask_img = Image.fromarray(mask)
        mask_img = mask_img.convert('RGB')
        mask_img.save(os.path.join(SAVE_DIR, f"frame{index:03}.png"))
```

原始视频和遮罩视频在以下动画中并排显示：

![](../Images/5415b89d0edd7d56e8c8dfe2a6510436.png)

**（左）** 由Videvo提供的库存视频，从[www.videvo.net](http://www.videvo.net/)下载 | **（右）** 作者创建的遮罩视频

# 步骤 3: 估计相机姿态和内部参数

创建遮罩帧后，我们现在开始计算相机姿态和内部估计。为此，我们使用一个叫做[Colmap](https://colmap.github.io/)的工具。它是一个多视图立体视觉工具，可以从多个图像创建网格，并估计相机的移动和内部参数。它既有图形用户界面，也有命令行界面。你可以从[这个链接](https://colmap.github.io/install.html)下载该工具。

启动工具后，你需要点击顶部栏上的“重建”（见下图），然后选择“自动重建”。在弹出窗口中，

+   进入`./datafiles/custom/triangulation`到“工作空间文件夹”

+   进入`./datafiles/custom/JPEGImages/640p/custom`到“图像文件夹”

+   进入`./datafiles/custom/JPEGImages/640p/custom`到“图像文件夹”

+   进入`./datafiles/custom/Annotations/640p/custom`到“遮罩文件夹”

+   勾选“共享内部参数”选项

+   点击“运行”。

计算可能需要一些时间，具体取决于你有多少图像以及图像的分辨率。计算完成后，点击“文件”下的“将模型导出为文本”并将输出文件保存在`./datafiles/custom/triangulation`中。这将创建两个文本文件和一个网格文件（.ply）。

![](../Images/2e8c3c0eadcfec9d044a8fea1d67cb91.png)

Colmap 的说明 — 图片由作者提供

这一步还没有结束，我们需要处理Colmap的输出。我编写了一个脚本来自动化这一过程。只需在终端中运行以下命令：

```py
python scripts/preprocess/custom/process_colmap_output.py
```

它将创建“custom.intrinsics.txt”、“custom.matrices.txt”和“custom.obj”。

![](../Images/42b639e07be8b2bbd996a698e0c04fe0.png)

Colmap的输出文件 — 图片由作者提供

现在我们准备好进行训练的数据集生成。

# 步骤 4：为训练准备数据集

训练需要一个数据集，该数据集包括每帧的深度估计由[MiDas](https://github.com/isl-org/MiDaS)提供，相应的跨帧光流估计和深度序列。创建这些的脚本在原始仓库中提供，我只是更改了其中的输入和输出目录。通过运行下面的命令，将创建所有所需的文件并放置在适当的目录中：

```py
python scripts/preprocess/custom/generate_frame_midas.py  &
python scripts/preprocess/custom/generate_flows.py  &
python scripts/preprocess/custom/generate_sequence_midas.py 
```

在训练之前，请检查`datafiles/custom_processed/frames_midas/custom`、`datafiles/custom_processed/flow_pairs/custom`和`datafiles/custom_processed/sequences_select_pairs_midas/custom`中是否存在 .npz 和 .pt 文件。验证后，我们可以继续进行训练。

# 步骤 5：训练

训练部分很简单。要用自定义数据集训练神经网络，只需在终端中运行以下命令：

```py
python train.py --net scene_flow_motion_field ^
 --dataset custom_sequence --track_id custom ^
 --log_time  --epoch_batches 2000 --epoch 10 ^
 --lr 1e-6 --html_logger --vali_batches 150  ^
 --batch_size 1 --optim adam --vis_batches_vali 1 ^
 --vis_every_vali 1 --vis_every_train 1 ^
 --vis_batches_train 1 --vis_at_start --gpu 0 ^
 --save_net 1 --workers 1 --one_way ^
 --loss_type l1 --l1_mul 0 --acc_mul 1 ^
 --disp_mul 1 --warm_sf 5 --scene_lr_mul 1000 ^
 --repeat 1 --flow_mul 1 --sf_mag_div 100 ^
 --time_dependent --gaps 1,2,4,6,8 --midas ^
 --use_disp --logdir 'logdir/' ^
 --suffix 'track_{track_id}' ^
 --force_overwrite
```

经过10次迭代的神经网络训练后，我观察到损失开始饱和，因此决定不再继续训练更多的迭代。以下是我的训练损失曲线图：

![](../Images/3d9745795ce3b24a84d736bc6567cb44.png)

损失 vs. 迭代曲线 — 图片由作者提供

在训练过程中，所有检查点都保存在目录`./logdir/nets/`中。此外，每次迭代后，训练脚本会在目录`./logdir/visualize`中生成测试可视化。这些可视化可以特别有助于识别训练过程中可能出现的任何潜在问题，除了监控损失外。

# 步骤 6：使用训练好的模型创建每帧的深度图

使用最新的检查点，我们现在用`test.py`脚本生成每帧的深度图。只需在终端中运行以下命令：

```py
python test.py --net scene_flow_motion_field ^
 --dataset custom_sequence --workers 1 ^
 --output_dir .\test_results\custom_sequence ^
 --epoch 10 --html_logger --batch_size 1 ^
 --gpu 0 --track_id custom --suffix custom ^
 --checkpoint_path .\logdir
```

这将为每一帧生成一个 .npz 文件（一个包含RGB帧、深度、相机姿态、流向下一张图像等的字典文件），以及每一帧的三个深度渲染（真实值、MiDaS和训练网络的估计）。

# 步骤 7：创建点云视频

在最后一步，我们逐帧加载批处理的 .npz 文件，并利用深度和 RGB 信息创建彩色点云。我使用 [open3d](http://www.open3d.org/docs/0.9.0/index.html) 库在 Python 中创建和渲染点云。这是一个强大的工具，你可以在 3D 空间中创建虚拟相机并用它们捕捉点云。你还可以编辑/操作点云；我应用了 open3d 内置的异常点移除功能来去除闪烁和噪声点。

虽然我不会详细讨论我使用 open3d 的具体细节，以保持本文的简洁性，但我已包含了脚本 `render_pointcloud_video.py`，该脚本应当易于理解。如果你有任何问题或需要进一步澄清，请随时问我。

这是我处理的视频的点云和深度图像的视频效果。

![](../Images/9fa90df27e34da041b4929750695d159.png)

**（左）** 视频素材由 Videvo 提供，下载自 [www.videvo.net](http://www.videvo.net/) | **（右）** 作者制作的深度图视频 | **（下）** 作者制作的彩色点云视频

*此动画的高分辨率版本已上传至* [*YouTube*](https://youtu.be/0AOiY2Sn-4E)*。*

好吧，深度图和点云很酷，但你可能想知道你可以用它们做什么。与传统的效果添加方法相比，深度感知效果可以非常强大。例如，深度感知处理可以创建各种电影效果，否则很难实现。通过视频的估计深度，你可以无缝地加入合成的相机对焦和虚焦，产生真实且一致的散景效果。

此外，深度感知技术提供了实现动态效果如“推镜变焦”的可能性。通过调整虚拟相机的位置和内参，这种效果可以生成惊艳的视觉序列。此外，深度感知的对象插入确保虚拟对象在视频中真实固定，保持整个场景中的一致位置。

深度图和点云的结合为引人入胜的叙事和富有创意的视觉效果开辟了无限可能，推动了电影制作人和艺术家的创意潜力达到新的高度。

点击本文的“发布”按钮后，我将挽起袖子开始制作这些效果。

祝你有美好的一天！
