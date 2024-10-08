# 用 ChatGPT 总结最新的 Spotify 发布内容

> 原文：[`towardsdatascience.com/summarizing-the-latest-spotify-releases-with-chatgpt-553245a6df88`](https://towardsdatascience.com/summarizing-the-latest-spotify-releases-with-chatgpt-553245a6df88)

## 探索音乐发现的力量：使用 ChatGPT 或 GPT-4 和 Spotify API 总结新发布的音乐

[](https://medium.com/@luisroque?source=post_page-----553245a6df88--------------------------------)![Luís Roque](https://medium.com/@luisroque?source=post_page-----553245a6df88--------------------------------)[](https://towardsdatascience.com/?source=post_page-----553245a6df88--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----553245a6df88--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----553245a6df88--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----553245a6df88--------------------------------) ·10 分钟阅读·2023 年 3 月 16 日

--

在当今快节奏的世界中，自然语言处理（NLP）已成为各种应用中至关重要的组成部分。像 OpenAI 的 ChatGPT 和 GPT-4 这样的巨大模型，解锁了在摘要生成、语音转文字、语音识别、语义搜索、问答系统、聊天机器人等任务中令人难以置信的潜力。

我很高兴地宣布“**大型语言模型编年史**：驾驭 NLP 前沿”这一新的每周文章系列，将探讨如何利用大型模型的力量进行各种 NLP 任务。通过深入研究这些前沿技术，我们旨在赋能开发者、研究人员和爱好者，充分利用 NLP 的潜力，解锁新的可能性。

在本系列的第一篇文章中，我们将重点介绍如何使用 OpenAI 的 ChatGPT 和 Spotify API 创建一个智能摘要系统，以获取最新的音乐发布。随着系列的发展，我们将*深入探讨*多种 NLP 应用，提供洞见、技术和实际示例，展示大型模型在改变我们与语言互动和理解方式方面的能力。

敬请关注更多文章，随着我们踏上这段激动人心的 NLP 之旅，指导你掌握各种语言任务的最新大型模型。

![](img/0c896fdfc1fdb0b0607df3bec89f780c.png)

图 1：LLM 是否开始了人与机器之间的新合作？ ([source](https://images.unsplash.com/photo-1485796826113-174aa68fd81b?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1170&q=80))

代码可在我的[Github](https://github.com/luisroque/large_laguage_models)上找到。

# 介绍

ChatGPT 和 GPT-4，由 OpenAI 开发，是先进的语言模型，在各种自然语言处理任务中表现出色。它们能够理解上下文，生成类似人类的响应，甚至有效总结大段文本。这使它们成为总结 Spotify 上最新音乐发布的理想工具。

作为领先的音乐流媒体平台，Spotify 提供了一个广泛的 API，使开发者能够访问大量音乐数据，包括新发布、播放列表等。通过将 ChatGPT 强大的语言理解能力与 Spotify API 提供的丰富音乐数据结合起来，我们可以构建一个系统，让您及时了解 Spotify 目录中的最新内容。

我们将引导您完成构建这个智能音乐总结系统的过程。我们的方法将包括以下步骤：

1.  **访问 Spotify API**：我们将通过 Spotify API 获取最新音乐发布的数据。

1.  **使用 ChatGPT 总结**：然后，我们将使用 OpenAI 的 API 生成最新发布内容的简明总结。

1.  **结果**：最后，我们将以一种易于阅读和引人入胜的格式展示总结。

敬请关注，我们将深入探讨每一步的细节，助您创建自己的音乐总结工具！

# 访问 Spotify API

在本节中，我们将探讨如何从 Spotify API 获取最新的音乐发布及其相关曲目数据。然后我们将把这些数据保存到 JSON 文件中以供进一步处理。将使用以下 Python 函数实现此目标：

1.  `get_new_releases`：从 Spotify 获取新专辑发布。

1.  `get_album_tracks`：检索特定专辑的曲目信息。

1.  `save_data_to_file`：将获取的数据保存到 JSON 文件中。

1.  `load_data_from_file`：从 JSON 文件中加载保存的数据。

1.  `download_latest_albums_data`：从 Spotify 下载最新的专辑和曲目数据，并保存到 JSON 文件中。

让我们解析这些功能的关键组件，理解它们如何协同工作以访问 Spotify API。

## 获取新发布

`get_new_releases` 函数接受两个可选参数，`limit` 和 `offset`。`limit` 确定要返回的专辑结果的最大数量，而 `offset` 指定第一个结果的索引。默认情况下，`limit` 设置为 50，`offset` 设置为 0。然后，函数调用 Spotify API 的 `sp.new_releases`，它返回一个包含专辑信息的字典。相关的专辑项被提取并返回为字典列表。

```py
def get_new_releases(limit: int = 50, offset: int = 0) -> List[Dict[str, Any]]:
    """
    Fetch new releases from Spotify.

    Args:
        limit (int, optional): Maximum number of album results to return. Defaults to 50.
        offset (int, optional): The index of the first result to return. Defaults to 0.

    Returns:
        List[Dict[str, Any]]: A list of dictionaries containing album information.
    """
    new_releases = sp.new_releases(limit=limit, offset=offset)
    albums = new_releases["albums"]["items"]
    return albums
```

## 检索专辑曲目

`get_album_tracks` 函数接受一个参数 `album_id`，这是我们想要获取曲目信息的专辑的 Spotify ID。该函数调用 Spotify API 的 `sp.album_tracks`，返回一个包含曲目数据的字典。然后，曲目项被提取并作为字典列表返回。

```py
def get_album_tracks(album_id: str) -> List[Dict[str, Any]]:
    """
    Fetch tracks from a specific album.

    Args:
        album_id (str): The Spotify ID of the album.

    Returns:
        List[Dict[str, Any]]: A list of dictionaries containing track information.
    """
    tracks = sp.album_tracks(album_id)["items"]
    return tracks
```

## 保存和加载数据

`save_data_to_file`函数接受两个参数：`data`，这是一个包含专辑和曲目信息的字典列表；`file_path`，这是保存数据的 JSON 文件路径。该函数使用`json.dump`方法将数据写入指定的文件。

相反，`load_data_from_file`函数从指定的 JSON 文件中读取数据，并使用`json.load`方法将其作为字典列表返回。

```py
def save_data_to_file(data: List[Dict[str, Any]], file_path: str) -> None:
    """
    Save data to a JSON file.

    Args:
        data (List[Dict[str, Any]]): List of dictionaries containing album and track information.
        file_path (str): Path to the JSON file where the data will be saved.
    """
    with open(file_path, "w", encoding="utf-8") as file:
        json.dump(data, file, ensure_ascii=False, indent=4)

def load_data_from_file(file_path: str) -> List[Dict[str, Any]]:
    """
    Load data from a JSON file.

    Args:
        file_path (str): Path to the JSON file where the data is stored.

    Returns:
        List[Dict[str, Any]]: List of dictionaries containing album and track information.
    """
    with open(file_path, "r", encoding="utf-8") as file:
        return json.load(file)
```

## 下载最新专辑数据

`download_latest_albums_data`函数作为从 Spotify 下载最新专辑和曲目数据的主要驱动程序。它初始化了诸如`limit`、`offset`、`total_albums`、`album_count`等变量，并创建了一个空列表`all_albums`以存储获取的数据。

函数进入一个循环，直到获取到指定数量的专辑（`total_albums`）为止。在每次迭代中，函数调用`get_new_releases`和`get_album_tracks`以检索专辑和曲目信息。这些数据随后存储在`all_albums`列表中。

在获取数据后，函数将`offset`按`limit`值递增，以便在随后的迭代中获取下一组专辑。为了避免触及 Spotify API 的速率限制，添加了 1 秒的延迟。函数最后调用`save_data_to_file`将获取的数据存储到 JSON 文件中。

```py
def download_latest_albums_data() -> None:
    """
    Download the latest albums and tracks data from Spotify and save it to a JSON file.
    """
    limit = 50
    offset = 0
    total_albums = 30
    album_count = 0

    all_albums = []

    while total_albums is None or album_count < total_albums:
        new_releases = get_new_releases(limit, offset)
        if total_albums is None:
            total_albums = sp.new_releases()["albums"]["total"]

        for album in new_releases:
            album_info = {
                "album_name": album["name"],
                "artist_name": album["artists"][0]["name"],
                "album_type": album["album_type"],
                "release_date": album["release_date"],
                "available_markets": album["available_markets"],
                "tracks": [],
            }

            tracks = get_album_tracks(album["id"])

            for track in tracks:
                track_info = {
                    "track_name": track["name"],
                    "duration_ms": track["duration_ms"],
                    "preview_url": track["preview_url"],
                }
                album_info["tracks"].append(track_info)

            all_albums.append(album_info)
            album_count += 1

        offset += limit
        time.sleep(1)  # Add a delay to avoid hitting the rate limit
        print(f"Downloaded {album_count}/{total_albums}")

    save_data_to_file(all_albums, "albums_and_tracks.json")
```

通过使用这些函数，我们可以有效地访问 Spotify API，以收集最新音乐发行的数据。在下一节中，我们将探讨如何预处理这些数据并使用 ChatGPT 生成这些新发行的摘要。

# 使用 LangChain 通过 ChatGPT 生成摘要

在本节中，我们将讨论如何预处理从 Spotify API 获得的专辑和曲目数据，并利用 ChatGPT 通过 LangChain 库生成最新音乐发行的摘要。LangChain 是一个强大的工具，使开发者能够构建将 LLMs 与其他计算或知识源结合的应用程序。

我们将使用以下 Python 函数来实现这一目标：

1.  `preprocess_docs`: 将 JSON 数据转换为 Document 对象列表。

1.  `get_summary`: 使用提供的 JSON 数据生成摘要，该数据在 Document 对象的列表中。

## 数据预处理

`preprocess_docs`函数接受一个包含专辑和曲目信息的字典列表，这是我们从 Spotify API 检索到的数据。该函数将这些数据转换为 JSON 字符串，然后将其拆分为 3500 字符的段落。这些段落用于创建 Document 对象列表，并将传递给 ChatGPT 以生成摘要。

将数据拆分为较小的段落是为了处理 ChatGPT API 施加的文本长度限制。通过将文本拆分成较小的部分，我们可以更有效地处理数据，而不会超出模型的最大令牌限制。

```py
def preprocess_docs(data: List[Dict[str, Any]]) -> List[Document]:
    """
    Convert the JSON data to a list of Document objects.

    Args:
        data (List[Dict[str, Any]]): List of dictionaries containing album and track information.

    Returns:
        List[Document]: A list of Document objects containing the JSON data as strings, split into 3000-character segments.
    """
    json_string = json.dumps(data, ensure_ascii=False, indent=4)
    doc_splits = [json_string[i : i + 3500] for i in range(0, len(json_string), 3500)]
    docs = [Document(page_content=split_text) for split_text in doc_splits]
    return docs
```

## 使用 LangChain 通过 ChatGPT 生成摘要

LangChain 的 CombineDocuments 链旨在处理和组合多个文档的信息，使其非常适合诸如摘要和问答等任务。在我们的案例中，我们将专注于使用 MapReduce 方法生成最新 Spotify 发布内容的摘要。如果你已经可以访问 API，你可以轻松地使用 GPT-4。为此，你只需更新传递给`ChatOpenAI`类的`model_name`参数。

MapReduce 方法通过对每个数据块运行初始提示来工作，为每个数据块生成一个输出。例如，在摘要任务中，这涉及为每个单独的数据块创建一个摘要。在下一步中，会运行不同的提示，将所有这些初始输出合并成一个连贯的输出。

使用 MapReduce 方法的主要优势在于它可以扩展到更大的文档并处理比 Stuffing 方法更多的文档。此外，对每个文档的 LLM 调用是独立的，允许并行处理和更快的处理速度。

在我们的项目背景下，我们将应用 MapReduce 方法使用 ChatGPT 总结最新的 Spotify 发布内容。我们使用 MapReduce 方法为每个文档生成摘要，然后将这些摘要合并成一个简洁的总结。

```py
def get_summary(docs: List[Document]) -> str:
    """
    Generate a summary using the JSON data provided in the list of Document objects.

    Args:
        docs (List[Document]): A list of Document objects containing the JSON data as strings.

    Returns:
        str: The generated summary.
    """
    llm = ChatOpenAI(temperature=0, model_name="gpt-3.5-turbo")

    prompt_template = """Write a short summary about the latest songs in Spotify based on the JSON data below: \n\n{text}."""
    prompt_template2 = """Write an article about the latest music released in Spotify (below) and adress the change in music trends using the style of Rick Beato. : \n\n{text}"""

    PROMPT = PromptTemplate(template=prompt_template, input_variables=["text"])
    PROMPT2 = PromptTemplate(template=prompt_template2, input_variables=["text"])

    chain = load_summarize_chain(
        llm,
        chain_type="map_reduce",
        return_intermediate_steps=True,
        map_prompt=PROMPT,
        combine_prompt=PROMPT2,
        verbose=True,
    )

    res = chain({"input_documents": docs}, return_only_outputs=True)

    return res
```

# 结果

为了更好地理解我们使用 Spotify API 和 OpenAI API 的 ChatGPT 实现的摘要能力，我们将展示一个示例，演示系统如何处理数据并生成简洁的摘要。让我们检查输入数据、中间步骤和最终输出。

## 输入数据

输入数据包括几个专辑及其相应的曲目，如 Dillaz 的“Oitavo Céu”和 T-Rex 的“CASTANHO”。每张专辑包括专辑名称、艺术家名称、专辑类型、发行日期以及曲目名称和持续时间（以毫秒为单位）的列表。

## 中间步骤

中间步骤包括使用 MapReduce 方法处理输入数据。例如，以下是为部分输入数据生成的摘要：

> *Spotify 上的最新歌曲包括三张新专辑中的曲目：Dillaz 的“Oitavo Céu”、T-Rex 的“CASTANHO”和 Branko 的“OBG”。“Oitavo Céu”包含 12 首曲目，其中包括标题曲目和持续时间最长的“Maçã”，其持续时间为 219130 ms。“CASTANHO”有 11 首曲目，其中“LADO NENHUM”的持续时间最长，为 278190 ms。“OBG”有 10 首曲目，其中“ETA”的持续时间最长，为 226058 ms。所有三张专辑均于 2022 年 4 月发布。*

## 最终输出

最终输出结合了中间步骤的摘要，提供了最新 Spotify 发布内容的连贯、简洁概述：

> *近年来，音乐行业的趋势发生了显著变化，流媒体平台如 Spotify 的崛起以及嘻哈和电子舞曲（EDM）等流派的日益流行导致了这些变化。因此，Spotify 上发布的最新音乐反映了这些变化，呈现了多样化的艺术家和风格。*
> 
> *一个显著的趋势是葡萄牙语音乐的日益突出，比如 Dillaz 的“八重天”和 T-Rex 的“CASTANHO”在平台上占据了重要位置。这些专辑展示了葡萄牙音乐的独特声音和节奏，将传统风格与现代影响相结合。*
> 
> *另一个趋势是不同风格和背景的艺术家之间合作的日益受欢迎。例如，Don Toliver 的“Life of a DON”和 Pop Smoke 的“Faith”专辑中包含了与 Travis Scott、Kanye West、Rick Ross 和 Lil Tjay 等多位艺术家的合作。这些合作让艺术家们探索新的声音和风格，并创造出更广泛受众喜爱的音乐。*
> 
> *此外，Spotify 上的最新音乐反映了音乐风格和影响的日益多样化。像 Olivia Rodrigo 的“SOUR”和 Billie Eilish 的“Happier Than Ever”这样的专辑展示了年轻女性艺术家的独特声音和视角，为当代音乐提供了新鲜的视角。*

如所示，我们基于 ChatGPT 的系统有效地总结了最新的 Spotify 发布内容，为音乐爱好者提供了一个易于访问和参与的概述，以便随时了解和发现新内容。

# 结论

在这篇文章中，我们展示了将 Spotify API 和 OpenAI 的 ChatGPT 结合起来创建总结系统的强大功能，该系统使您能够及时了解最新的音乐发行。我们讨论了文档链技术，选择了因其可扩展性而被广泛使用的 MapReduce 方法，并展示了我们系统在生成连贯且信息丰富的总结方面的有效性。

AI 驱动的语言模型与像 Spotify 这样的热门平台 API 之间的协同作用为创新和个性化开辟了新的机会。随着 AI 技术的不断发展，它们在各种 NLP 任务中的应用将不断扩展，为我们日常生活的提升提供令人兴奋的方式。

总之，我们的探索作为尖端 AI 技术在解决现实世界挑战和创造有价值用户体验方面的潜力的激励性示例。我们希望这篇文章能鼓励您在自己的项目中进一步探索 AI 的应用，并激励您创造出能够带来改变的创新解决方案。

保持联系：[LinkedIn](https://www.linkedin.com/in/luisbrasroque/)
