# 使用 LangChain、Google Maps API 和 Gradio 构建智能旅行行程建议器（第 3 部分）

> 原文：[https://towardsdatascience.com/building-a-smart-travel-itinerary-suggester-with-langchain-google-maps-api-and-gradio-part-3-90dc7be627fb?source=collection_archive---------5-----------------------#2023-09-26](https://towardsdatascience.com/building-a-smart-travel-itinerary-suggester-with-langchain-google-maps-api-and-gradio-part-3-90dc7be627fb?source=collection_archive---------5-----------------------#2023-09-26)

## 了解如何构建一个可能激发你下一次公路旅行灵感的应用

[](https://medium.com/@rmartinshort?source=post_page-----90dc7be627fb--------------------------------)[![Robert Martin-Short](../Images/e3910071b72a914255b185b850579a5a.png)](https://medium.com/@rmartinshort?source=post_page-----90dc7be627fb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----90dc7be627fb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----90dc7be627fb--------------------------------) [Robert Martin-Short](https://medium.com/@rmartinshort?source=post_page-----90dc7be627fb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F83d38eb39498&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-smart-travel-itinerary-suggester-with-langchain-google-maps-api-and-gradio-part-3-90dc7be627fb&user=Robert+Martin-Short&userId=83d38eb39498&source=post_page-83d38eb39498----90dc7be627fb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----90dc7be627fb--------------------------------) ·6 分钟阅读·2023年9月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F90dc7be627fb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-smart-travel-itinerary-suggester-with-langchain-google-maps-api-and-gradio-part-3-90dc7be627fb&user=Robert+Martin-Short&userId=83d38eb39498&source=-----90dc7be627fb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F90dc7be627fb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-smart-travel-itinerary-suggester-with-langchain-google-maps-api-and-gradio-part-3-90dc7be627fb&source=-----90dc7be627fb---------------------bookmark_footer-----------)

> **本文是一个三部分系列中的最后一篇，我们使用OpenAI和Google API构建了一个旅行行程建议应用，并通过Gradio生成的简单UI展示它。在这一部分，我们讨论如何构建该UI，将我们在第1部分和第2部分中构建的Agent和RouteFinder模块组合在一起。只想查看代码？请在**[**这里**](https://github.com/rmartinshort/travel_mapper)**找到**。

# 1\. 第二部分回顾

在这个三部分系列的[第二部分](/building-a-smart-travel-itinerary-suggester-with-langchain-google-maps-api-and-gradio-part-2-86e9d2bcae5)，我们构建了一个系统，该系统从一系列LLM调用中获取解析后的路标列表（第1部分），并使用Google Maps API和Folium生成它们之间的路线，并将其绘制在交互式地图上。回顾一下，我们在这个项目中的目标是构建一个应用程序，允许用户轻松输入旅行请求，例如*“从柏林到苏黎世的四天行程，我想尝试大量当地啤酒和食物”*，并返回详细的行程安排和地图供他们探索。感谢第1部分和第2部分，我们已经组装了所有组件，现在只需将它们组合在一个使使用变得简单的UI中即可。

# **2\. 将地图连接到Gradio**

[gradio](https://www.gradio.app/)是一个出色的库，用于快速构建能够展示机器学习模型的交互式应用程序。它有一个`gradio.Plot`组件，旨在与Matplotlib、Bokeh和Plotly配合使用（详细信息[这里](https://www.gradio.app/guides/plot-component-for-maps)）。然而，我们在第二部分生成的地图是使用Folium制作的。虽然可以使用这些其他库重新制作这些地图，但幸运的是，我们不需要这么做。相反，我们可以使用[leafmap](https://leafmap.org/)包，它允许我们重用已有的Folium代码，并强制输出Gradio可以理解的HTML。详细信息可以在[这里](https://leafmap.org/notebooks/66_gradio/)找到。

让我们来看一个简单的例子，了解它是如何工作的。首先，我们将创建一个函数，从中输出所需格式的HTML。

```py
import leafmap.foliumap as leafmap
import folium
import gradio as gr

def generate_map(center_coordinates, zoom_level):

    coords = center_coordinates.split(",")
    lat, lon = float(coords[0]), float(coords[1])
    map = leafmap.Map(location=(lat,lon), tiles="Stamen Terrain", zoom_start=zoom_level)

    return map.to_gradio()
```

在这里，函数`generate_map`接收一个格式为*“lat,lon”*的坐标字符串和一个Folium地图的缩放级别。它生成地图并将其转换为Gradio可以读取的格式。

接下来，让我们构建一个非常简单的Gradio界面来展示我们的地图。

```py
demo = gr.Blocks()

with demo:
    gr.Markdown("## Generate a map")
    with gr.Row():
      with gr.Column():
        # first col is for buttons
        coordinates_input = gr.Textbox(value="",label="Your center coordines",lines=1)
        zoom_level_input = gr.Dropdown(choices=[1,2,3,4,5,6,7,8,9],label="choose zoom level")
        map_button = gr.Button("Generate map")
      with gr.Column():
        # second col is for the map
        map_output = gr.HTML(label="Travel map")

    map_button.click(generate_map, inputs=[coordinates_input,zoom_level_input], outputs=[map_output])

# run this in a notebook to display the UI
demo.queue().launch(debug=True)
```

在这里，我们利用`Blocks` API，它为我们提供了设置应用程序UI的灵活性。我们创建了一行组件，分为两列。第一列包含三个元素：一个文本框供用户输入所需的中心坐标，一个下拉菜单选择缩放级别，以及一个名为*“生成地图”*的按钮，用户需要点击这个按钮。

在第二列，我们有`map_output`，这是一个`gradio.HTML()`组件，用于显示地图的HTML。

然后，我们需要做的就是定义点击 `map_button` 时发生的事情。当发生这种情况时，我们将运行 `generate_map` 函数，传入从 `coordinates_input` 和 `zoom_input` 中选择的值。结果将被发送到 `map_output` 变量。

运行此代码会产生以下用户界面

![](../Images/20d1d726bf80511528ef8b0c48695346.png)

使用 leafmap 和 gradio 生成的基本映射用户界面

这确实不复杂也不精美，但它包含了使用 gradio 构建映射工具的基本元素。

# **3\. 为我们的旅行代理商创建一个简单的用户界面**

让我们首先看看 gradio 应用程序在我们检查代码之前的一些功能。不过要注意，gradio 提供了大量组件来创建复杂且美观的用户界面，而这个旅行地图用户界面仍然处于 POC 阶段。

![](../Images/7e9da427e20d22d644547d8d2282cff7.png)

旅行地图应用最终版本中所有组件的描述

我们的应用程序本质上有两列。第一列包含一个文本框，供用户输入查询，一个单选按钮组，允许我们在模型之间切换，以及一个显示验证检查输出的文本框。

第二列包含由 `leafmap.folium` 生成的地图和一个显示 LLM 调用完整文本行程输出的文本框。*“生成地图”* 按钮在底部，截图中不可见。

多亏了 gradio 在后台完成的所有工作，所有这些代码都非常简洁。

```py
import gradio as gr
from travel_mapper.TravelMapper import TravelMapperForUI, load_secrets, assert_secrets
from travel_mapper.user_interface.utils import generate_generic_leafmap
from travel_mapper.user_interface.constants import EXAMPLE_QUERY

def main():

    # load the AP keys
    secrets = load_secrets()
    assert_secrets(secrets)

    # set up travel mapper (see part 2)
    travel_mapper = TravelMapperForUI(
        openai_api_key=secrets["OPENAI_API_KEY"],
        google_maps_key=secrets["GOOGLE_MAPS_API_KEY"],
        google_palm_api_key=secrets["GOOGLE_PALM_API_KEY"],
    )

    # build the UI in gradio
    app = gr.Blocks()

    # make a generic map to display when the app first loads 
    generic_map = generate_generic_leafmap()

    with app:
        gr.Markdown("## Generate travel suggestions")

        # make multple tabs
        with gr.Tabs():
            # make the first tab
            with gr.TabItem("Generate with map"):
                # make rows 1 within tab 1
                with gr.Row():
                    # make column 1 within row 1
                    with gr.Column():
                        text_input_map = gr.Textbox(
                            EXAMPLE_QUERY, label="Travel query", lines=4
                        )

                        radio_map = gr.Radio(
                            value="gpt-3.5-turbo",
                            choices=["gpt-3.5-turbo", "gpt-4", "models/text-bison-001"],
                            label="models",
                        )

                        query_validation_text = gr.Textbox(
                            label="Query validation information", lines=2
                        )

                    # make column 2 within row 1
                    with gr.Column():
                        # place where the map will appear
                        map_output = gr.HTML(generic_map, label="Travel map")
                        # place where the suggested trip will appear
                        itinerary_output = gr.Textbox(
                            value="Your itinerary will appear here",
                            label="Itinerary suggestion",
                            lines=3,
                        )
                # generate button
                map_button = gr.Button("Generate")

            # make the second tab
            with gr.TabItem("Generate without map"):
                # make the first row within the second tab
                with gr.Row():
                    # make the first column within the first row
                    with gr.Column():
                        text_input_no_map = gr.Textbox(
                            value=EXAMPLE_QUERY, label="Travel query", lines=3
                        )

                        radio_no_map = gr.Radio(
                            value="gpt-3.5-turbo",
                            choices=["gpt-3.5-turbo", "gpt-4", "models/text-bison-001"],
                            label="Model choices",
                        )

                        query_validation_no_map = gr.Textbox(
                            label="Query validation information", lines=2
                        )
                    # make the second column within the first row
                    with gr.Column():
                        text_output_no_map = gr.Textbox(
                            value="Your itinerary will appear here",
                            label="Itinerary suggestion",
                            lines=3,
                        )
                # generate button
                text_button = gr.Button("Generate")

        # instructions for what happens whrn the buttons are clicked 
        # note use of the "generate_with_leafmap" method here. 
        map_button.click(
            travel_mapper.generate_with_leafmap,
            inputs=[text_input_map, radio_map],
            outputs=[map_output, itinerary_output, query_validation_text],
        )
        text_button.click(
            travel_mapper.generate_without_leafmap,
            inputs=[text_input_no_map, radio_no_map],
            outputs=[text_output_no_map, query_validation_no_map],
        )

    # run the app
    app.launch()
```

# 4\. 创建包

从 github 上查看存储库可以看出，旅行地图代码是通过 [cookiecutter](https://github.com/cookiecutter/cookiecutter) 的标准模板构建的，但模板中的一些重要部分尚未填充。理想情况下，我们会包括单元测试和集成测试，并完成存储库设置，以便使用持续集成/持续交付（CI/CD）概念。如果项目在这个 POC 阶段之后进一步发展，这些方面将在未来添加。

代码可以通过几种方式在本地运行。如果我们将上述 `main` 函数放入一个名为 `driver.py` 的脚本中，我们应该能够从 `travel_mapper` 项目的顶层从终端运行它。如果包成功运行，终端中应出现类似以下的消息

```py
Running on local URL:  http://127.0.0.1:7860
```

将此网址复制粘贴到网络浏览器中应该会显示出在本地运行的 gradio 应用。当然，如果我们真的想将其部署到网络上（我不推荐，因为 API 调用的费用），需要更多的步骤，但这超出了这些文章的范围。

驱动程序也可以从名为 `run.sh` 的 bash 脚本中运行，该脚本可以在代码库的 `user_interface` 模块中找到。

```py
# Run the UI
# run this from the top level directory of the travel mapper project
export PYTHONPATH=$PYTHONPATH:$(pwd)
echo "Starting travel mapper UI"
$(pwd)/travel_mapper/user_interface/driver.py
```

从项目的顶层运行时，这也会正确设置`PYTHONPATH`，确保项目特定的导入语句始终被识别。

这就是本系列的全部内容，感谢你一直看到最后！请随时在这里探索完整的代码库 [https://github.com/rmartinshort/travel_mapper](https://github.com/rmartinshort/travel_mapper)。任何改进建议或功能扩展的意见都非常受欢迎！
