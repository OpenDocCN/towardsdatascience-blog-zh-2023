# 如何使用IPyWidgets和Plotly在Python中构建自定义标注工具

> 原文：[https://towardsdatascience.com/how-to-build-a-custom-labeler-in-python-with-ipywidgets-and-plotly-f6cc1fd7e3cc?source=collection_archive---------21-----------------------#2023-01-10](https://towardsdatascience.com/how-to-build-a-custom-labeler-in-python-with-ipywidgets-and-plotly-f6cc1fd7e3cc?source=collection_archive---------21-----------------------#2023-01-10)

## 在Jupyter环境中创建一个分割工具

[](https://medium.com/@jacky.kaub?source=post_page-----f6cc1fd7e3cc--------------------------------)[![Jacky Kaub](../Images/e66c699ee5a9d5bbd58a1a72d688234a.png)](https://medium.com/@jacky.kaub?source=post_page-----f6cc1fd7e3cc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f6cc1fd7e3cc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f6cc1fd7e3cc--------------------------------) [Jacky Kaub](https://medium.com/@jacky.kaub?source=post_page-----f6cc1fd7e3cc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7ccb7065ef90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-a-custom-labeler-in-python-with-ipywidgets-and-plotly-f6cc1fd7e3cc&user=Jacky+Kaub&userId=7ccb7065ef90&source=post_page-7ccb7065ef90----f6cc1fd7e3cc---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f6cc1fd7e3cc--------------------------------) ·11分钟阅读·2023年1月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff6cc1fd7e3cc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-a-custom-labeler-in-python-with-ipywidgets-and-plotly-f6cc1fd7e3cc&user=Jacky+Kaub&userId=7ccb7065ef90&source=-----f6cc1fd7e3cc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff6cc1fd7e3cc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-a-custom-labeler-in-python-with-ipywidgets-and-plotly-f6cc1fd7e3cc&source=-----f6cc1fd7e3cc---------------------bookmark_footer-----------)![](../Images/a47b2b2f32879bd5ccc1adbd9766dfb0.png)

从肾脏中分割出的肾小球。原始图像由[HuBMAP组织映射中心](https://medschool.vanderbilt.edu/biomic/) 提供，来自范德堡大学

你知道吗？你可以在几行代码内将一个简单的**Jupyter Notebook** 转变成**一个**强大的标注工具。

将一个简单的笔记本转变为标注工具在项目开始时可以节省大量时间，尤其是在你还在评估一个想法的潜力，并且不想在构建一个稳定的网页应用上花费太多时。

在这篇文章中，我将向你展示如何使用**plotly**和**ipywidget**构建一个分割工具，以生成python中图像的二进制掩模。在图像分割中，二进制掩模可以用于训练深度学习模型，以高效地自动识别和隔离感兴趣区域。

我假设你已经熟悉ipywidgets和plotly的基础知识。如果不是，我强烈建议你先查看[这篇文章](https://medium.com/@jacky.kaub/build-custom-widgets-with-ipywidgets-and-plotly-a454cb3b2b4f)，我们将介绍基础知识。

## 关于用例的一句话

我正在使用[HuBMAP数据集](https://www.kaggle.com/competitions/hubmap-kidney-segmentation/data)，这是一个2021年在Kaggle上举办的竞赛中的数据集。

我选择这个数据集，因为它完美地展示了现实生活中的应用：一个客户联系你，提供一堆图像，并询问你是否可以找到一种方法来自动隔离这些图像中的特定感兴趣区域。这些区域（在这个例子中）被称为肾小体，是肾脏中帮助过滤血液中废物和多余水分的小部分，见下图。

下图展示了你的客户在这里的期望：一个能够快速识别那些感兴趣区域的模型。

![](../Images/075e02aa0858e1ed84f463e96c2de637.png)

项目目标：提供肾脏图像的分割区域。原始图像由[HuBMAP组织映射中心](https://medschool.vanderbilt.edu/biomic/)提供，位于范德比尔特大学。

在探索阶段，时间至关重要。通常，我们只有原始数据，并需要快速生成标签来训练和评估模型，并评估项目的可行性。

与其花费数小时寻找可能仍需定制的开源工具，不如跟随我一步步构建你自己的自定义工具。你会发现，只需一点努力，你就可以轻松创建任何类型的部件来帮助你的日常任务。

## 构建部件的技术规格

在直接进入编码部分之前，我们需要退一步，考虑一下我们到底希望从我们的部件中得到什么。我们需要回答的问题包括：

+   输入的格式是什么？是jpeg图像吗？Numpy数组？栅格？等等。

+   它们存储在哪里？本地？多个区域？还是在云端？

+   你期望的输出是什么？另一个jpeg图像还是numpy数组？也许是存储在json中的注释？

+   你期望的交互是什么？点击按钮做某事，具有一些UI元素以有效地从数据到数据导航，直接与图形交互，等等。

+   对于每次交互，实际操作中你将如何管理？这将如何影响部件的内部状态？

+   尝试在某处（即使是在纸上）绘制你希望你的部件看起来的样子。这将帮助你结构化部件的布局。

在我们的案例中：

我假设主要图像已经被分割成 256x256 像素的子图像，并存储为 .npy 文件在本地的 imgs/ 文件夹中。

```py
main/
  |-- imgs/
  |     |-- 1.npy
  |     |-- 2.npy
  |     |-- etc..
  |-- masks/
  |-- widget_notebook.ipynb
```

生成的掩膜将存储在 masks/ 文件夹中，格式为 .npy，文件名与原始图像相同。

我们工具所需的功能包括：

+   小部件将显示两个图像：一个是带有掩膜透明覆盖的原始图像（正在进行中），另一个是当前保存的掩膜，这将允许用户可视化当前完成的工作，并在需要时完成它。

+   用户可以多次点击图像以生成定义区域的多边形。这将在小部件内用于生成一个掩膜，其中区域外的像素为0，区域内的像素为1。

+   应该使用一个按钮来保存掩膜，当用户对其标签感到满意时，这样结果就会被存储并可以由他或其他服务（如深度学习模型）检索。

+   应该使用一个按钮来删除当前的多边形，这将方便修复绘图错误。

+   应该使用一个按钮来重置本地保存的掩膜，允许在需要时恢复到原始状态，而不必手动删除掩膜。

+   用户可以通过下拉菜单在图像之间导航

![](../Images/b7679ccbba9d3ebc483e098f4ebca07b.png)

我们希望实现的小部件布局的粗略视图。原始图像由 [HuBMAP组织组织](https://medschool.vanderbilt.edu/biomic/) 提供，来自范德比尔特大学

# 构建小部件

## 开始之前的一些良好实践

我建议你在自定义类中构建你的小部件。这将方便你在状态管理方面，因为所有内容都将保留在内部，并且更容易打包。

同时，作为一般建议，你应该先构建主要布局，然后在第二阶段再构建交互功能。

最后，尽量将各部分分开！创建每个按钮的专用函数或单独处理每个回调的函数不会增加额外的成本，但可能会为测试和调试节省宝贵的时间。

总结来说，为了高效构建各种小部件：

1.  创建一个类来保存小部件中使用的所有内部状态

1.  构建初始化图像和组件的函数，在第一阶段没有交互

1.  添加交互并逐步测试

1.  保持代码整洁有序

## 小部件内部状态逻辑

我们的小部件需要保持并修改多个信息：

+   _current_id 用于存储当前图像的名称，同时也用于访问和保存二进制掩膜。

+   _polygon_coordinates 列表将包含用户点击的点的坐标，并将用于生成二进制掩膜（使用 **skimage** 函数）

+   _initialize_widget 函数将生成构建布局所需的所有其他状态，如按钮、图像和中间掩膜。

```py
class SegmentWidget:

    def __init__(self, path_imgs, path_masks):

        self._path_imgs = path_imgs
        self._path_masks = path_masks
        self._ids = sorted([os.path.splitext(e)[0] for e in  os.listdir(path_imgs)], key=int)
        self._current_id = self.ids[0]
        #This list will be used later to save in memory the coordinates 
        #of the clicks by the user
        self._polygon_coordinates = []
        #TODO: Initiation of all the layout components
        self._initialize_widget()
```

*注意：Python 不管理公共和私有变量，但作为良好的 pep8 实践，非公共变量和函数可以用“_”作为前缀。*

## 读取图像

我们需要开发一个函数，用于读取图像及其相关的掩码（如果存在）。

这个函数将在小部件初始化期间调用，但每当用户切换到新图像时也会调用。

```py
 def _load_images(self):
        '''This method will be used to load image and mask when we select another image'''
        img_path = os.path.join(self._path_imgs,f"{self._current_id}.npy")
        self._current_img = np.load(img_path)
        h,w, _ = self._current_img.shape

        #There is not always a mask saved. When no mask is saved, we create an empty one.
        mask_path = os.path.join(self._path_masks,f"{self._current_id}.npy")
        if os.path.exists(mask_path):
            self._current_mask = np.load(mask_path)
        else:
            self._current_mask = np.zeros((h,w))
        #initiate an intermediate mask which will be used to store ongoing work
        self._intermediate_mask = self._current_mask.copy()
```

*注意：我更倾向于在这里打包一个专用函数。我本可以把所有东西直接放在 __init__ 中，但那样会更难消化。如果你发现一个整体上合理的任务，使用一个函数。*

## 初始化图形

下一步是初始化我们的 FigureWidgets。

我们将使用 plotly.express.imshow 方法显示 RGB 图像，同时对二进制掩码使用 plotly.graph_objects.Heatmap。

我们目前专注于小部件的总体布局，因此将回调函数的定义留到以后。

```py
def _initialize_figures(self):
    '''This function is called to initialize the figure and its callback'''
    self._image_fig = go.FigureWidget()
    self._mask_fig = go.FigureWidget()

    self._load_images() #Update the state loading the images

    #We use plotly express to generate the RGB image from the 3D array loaded
    img_trace = px.imshow(self._current_img).data[0]
    #We use plotly HeatMap for the 2D mask array
    mask_trace = go.Heatmap(z=self._current_mask, showscale=False, zmin=0, zmax=1)

    #Add the traces
    self._image_fig.add_trace(img_trace)
    self._image_fig.add_trace(mask_trace)
    self._mask_fig.add_trace(mask_trace)

    #A bit of chart formating
    self._image_fig.data[1].opacity = 0.3 #make the mask transparent on image 1
    self._image_fig.data[1].zmax = 2 #the overlayed mask above the image can have values in range 0..2
    self._image_fig.update_xaxes(visible=False)
    self._image_fig.update_yaxes(visible=False)
    self._image_fig.update_layout(margin={"l": 10, "r": 10, "b": 10, "t": 50}, 
                                  title = "Define your Polygon Here",
                                  title_x = 0.5, title_y = 0.95)
    self._mask_fig.update_layout(yaxis=dict(autorange='reversed'), margin={"l": 0, "r": 10, "b": 10, "t": 50},)
    self._mask_fig.update_xaxes(visible=False)
    self._mask_fig.update_yaxes(visible=False)

    #Todo: add the callbacks to the two charts
```

*不要犹豫花一点时间来格式化，可能看起来很烦人，但拥有一个合适大小、信息量刚好的工具会在你操作时节省时间*

## 按钮、下拉菜单和最终布局

我们现在将添加函数以将界面组件集成到小部件中。正如规格中所述，我们需要 3 个按钮（水平显示）、一个下拉菜单和两个并排的图形。

```py
def _build_save_button(self):
    self._save_button = Button(description="Save Configuration")
    #Todo: add the callback

def _build_delete_current_config_button(self):
    self._delete_current_config_button = Button(description="Delete Current Mask")
    #Todo: add the callback

def _build_delete_all_button(self):
    self._delete_all_button = Button(description="Delete All Mask")
    #Todo: add the callback

def _build_dropdown(self):
    #The ids are passed as option for the dropdown
    self._dropdown = Dropdown(options = self._ids)
    #Todo: add the callback

def _initialize_widget(self):
    '''Function called during the init phase to initialize all the components
       and build the widget layout
    '''

    #Initialize the components
    self._initialize_figures()
    self._build_save_button()
    self._build_delete_current_config_button()
    self._build_delete_all_button()
    self._build_dropdown()

    #Build the layout
    buttons_widgets = HBox([self._save_button,
                            self._delete_current_config_button,
                            self._delete_all_button])

    figure_widgets = HBox([self._image_fig, self._mask_fig])

    self.widget = VBox([self._dropdown, buttons_widgets, figure_widgets])

def display(self):
    display(self.widget)
```

我添加了一个“公共” display() 方法（小部件中唯一的“公共”）。它将显示小部件的初始状态。

拥有这样的方法允许用户直接显示小部件，而无需在其内部状态中查找你的部件名称……

![](../Images/ad364ac049395129d9098cf7d48bd310.png)

由[HuBMAP 组织的组织映射中心](https://medschool.vanderbilt.edu/biomic/)在范德堡大学提供的原始图像

# 处理交互

现在我们的小部件已完全初始化，我们可以开始处理不同的交互。

## 下拉菜单交互

下拉菜单允许用户浏览数据集中的不同图像。为了正常工作，下拉菜单的回调函数应：

+   读取新图像和新掩码

+   更新两个图形

+   清空 _polygon_coordinates 中的点列表以开始一个新的多边形

要更新 px.imshow() 轨迹，我们需要修改其“source”参数。

要更新 go.Heatmap() 轨迹，我们修改轨迹的“z”参数。

*请注意，每个轨迹通常显示为一个包含图表所有信息的对象。不要犹豫，展示它以查看你可以更轻松地修改什么。*

```py
def _callback_dropdown(self, change):

    #Set the new id to the new dropdown value
    self._current_id = change['new']

    #Load the new image and the new mask, we already have a method to do this
    self._load_images()

    img_trace = px.imshow(self._current_img).data[0]

    #Update both figure
    with self._image_fig.batch_update():
        #Update the trace 0 and the trace 1 containing respectively
        #the image and the mask
        self._image_fig.data[0].source = img_trace.source
        self._image_fig.data[1].z = self._current_mask

    with self._mask_fig.batch_update():
        self._mask_fig.data[0].z = self._current_mask

    #Reset the list of coordinates used to store current work in progress
    self._polygon_coordinates = []

def _build_dropdown(self):
    #The ids are passed as option for the dropdown
    self._dropdown = Dropdown(options = self._ids)
    self._dropdown.observe(self._callback_dropdown, names="value") 
```

我们现在可以浏览不同的图像。

![](../Images/f0471ed71b38d443101d39db3807bfee.png)

由[HuBMAP 组织的组织映射中心](https://medschool.vanderbilt.edu/biomic/)在范德堡大学提供的原始图像

## FigureWidget 的 on_click 交互

点击图像中的一个像素将把它的坐标存储在小部件状态中。

存储 3 个或更多像素应触发一个函数，该函数将：

+   如果像素位于由坐标列表定义的多边形内或外，则生成值为 0 或 2 的新掩模数组。

+   创建一个中间掩模，其中包含前一个掩模的信息和新生成的掩模信息。

+   在左侧图形上显示新生成的掩模。

我们使用 skimage.draw 从坐标列表中高效生成多边形掩模。

*Scikit-image 是处理各种图像处理任务的强大工具。查看* [*他们的文档*](https://scikit-image.org/) *以了解更多可能性！*

```py
def _gen_mask_from_polygon(self):
    '''This function set to 2 the values inside the polygon defined by the list of points provided'''
    h,w = self._current_mask.shape
    new_mask = np.zeros((h,w), dtype=int)
    #Get coordinates inside the polygon using skimage.draw.polygon function
    rr, cc = polygon([e[0] for e in self._polygon_coordinates], 
                     [e[1] for e in self._polygon_coordinates], shape=new_mask.shape)

    #Recreate the intermediate_mask and set values inside ongoing polygon
    #to 2
    self._intermediate_mask = self._current_mask.copy()
    self._intermediate_mask[rr,cc]=2

def _on_click_figure(self, trace, points, state):
    #Retrieve coordinates of the clicked point
    i,j = points.point_inds[0]
    #Add the point to the list of points
    self._polygon_coordinates.append((i,j))

    #If more than 2 click have been done, create the new intermediate polygon
    #and update the mask on the image
    if len(self._polygon_coordinates)>2:
        self._gen_mask_from_polygon()
        with self._image_fig.batch_update():
            self._image_fig.data[1].z = self._intermediate_mask
```

然后我们可以更新 _initialize_figures 方法以附加回调函数。

*注意：由于图形由多个图像层组成，我们将回调函数附加到顶层。*

```py
def _initialize_figures(self):

    #[...Rest of the function...]

    self._image_fig.data[-1].on_click(self._on_click_figure)
```

就这样！我们现在可以可视化我们图表交互的效果：

![](../Images/54b58906563ba78c3c1e3c671f2c326c.png)

原始图像由[HuBMAP组织的组织映射中心](https://medschool.vanderbilt.edu/biomic/)提供，位于范德比尔特大学。

## 按钮交互

为完成我们的应用程序，让我们编写按钮交互的代码。

“保存配置”按钮。这个按钮通过以下方式保存掩模：

1.  将 0 和 2 像素从 _intermediate_mask 复制到 _current_mask。

1.  将 _current_mask 保存到 /masks 文件夹。

1.  刷新图形。

1.  重置 _polygon_coordinates 列表。

```py
def _callback_save_button(self, button):
    self._current_mask[self._intermediate_mask==2]=1
    self._current_mask[self._intermediate_mask==0]=0
    mask_path = os.path.join(self._path_masks,f"{self._current_id}.npy")
    np.save(mask_path,self._current_mask)
    self._intermediate_mask = self._current_mask.copy()
    with self._image_fig.batch_update():
        self._image_fig.data[1].z = self._current_mask     
    with self._mask_fig.batch_update():
        self._mask_fig.data[0].z = self._current_mask
    self._polygon_coordinates = []

def _build_save_button(self):
    self._save_button = Button(description="Save Configuration")
    self._save_button.on_click(self._callback_save_button)
```

![](../Images/6a8c1ae8d5c1c0148806595935b6d4a8.png)

原始图像由[HuBMAP组织的组织映射中心](https://medschool.vanderbilt.edu/biomic/)提供，位于范德比尔特大学。

“删除当前掩模”按钮。这个按钮只是将 _intermediate_mask 重置为 _current_mask 的值，并刷新图形和 _polygon_coordinates 列表。

```py
def _callback_delete_current_config_button(self, button):
    self._intermediate_mask = self._current_mask.copy()
    with self._image_fig.batch_update():
        self._image_fig.data[1].z = self._intermediate_mask
    self._polygon_coordinates = []

def _build_delete_current_config_button(self):
    self._delete_current_config_button = Button(description="Delete Current Mask")
    self._delete_current_config_button.on_click(self._callback_delete_current_config_button)
```

![](../Images/f5b07125ecbd2e53123b2cb61545af7c.png)

原始图像由[HuBMAP组织的组织映射中心](https://medschool.vanderbilt.edu/biomic/)提供，位于范德比尔特大学。

“删除所有掩模”按钮。这个按钮只是将 _intermediate_mask 重置为 0，并将 _polygon_coordinates 重置为空列表。

```py
def _callback_delete_all_button(self, button):
    self._intermediate_mask[:] = 0
    with self._image_fig.batch_update():
        self._image_fig.data[1].z = self._intermediate_mask
    self._polygon_coordinates = []

def _build_delete_all_button(self):
    self._delete_all_button = Button(description="Delete All Mask")
    self._delete_all_button.on_click(self._callback_delete_all_button)
```

![](../Images/e2d73c91b19af88ef415856921842b00.png)

原始图像由[HuBMAP组织的组织映射中心](https://medschool.vanderbilt.edu/biomic/)提供，位于范德比尔特大学。

想自己试试吗？完整代码请见[这里](https://github.com/jkaub/segmentation-widget)！

# 结论

几小时的工作后，我们已经能够设置我们的工具，并现在具备快速标记图像的能力，这将帮助我们满足客户的原始请求。在准备了几百张图像的掩模后，我们可以训练一个初步模型，并评估是否需要动用更多资源来推动项目进一步发展。

## 发挥创造力

如果你到这里来了，恭喜你！你现在可以直接在你的 Jupyter Notebook 中生成一个自定义标签工具了。我们仅仅探讨了使用 plotly 和 ipywidgets 可以快速利用的众多用例中的一个，你现在拥有了开发自己应用程序的所有关键。
