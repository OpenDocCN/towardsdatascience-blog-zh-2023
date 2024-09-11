# 使用 GPT-3 预测人道主义数据集的元数据

> 原文：[https://towardsdatascience.com/predicting-metadata-for-humanitarian-datasets-using-gpt-3-b104be17716d?source=collection_archive---------4-----------------------#2023-01-18](https://towardsdatascience.com/predicting-metadata-for-humanitarian-datasets-using-gpt-3-b104be17716d?source=collection_archive---------4-----------------------#2023-01-18)

[](https://medium.com/@astrobagel?source=post_page-----b104be17716d--------------------------------)[![Matthew Harris](../Images/4fa3264bb8a028633cd8d37093c16214.png)](https://medium.com/@astrobagel?source=post_page-----b104be17716d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b104be17716d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b104be17716d--------------------------------) [Matthew Harris](https://medium.com/@astrobagel?source=post_page-----b104be17716d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4a2cd25b8ff9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpredicting-metadata-for-humanitarian-datasets-using-gpt-3-b104be17716d&user=Matthew+Harris&userId=4a2cd25b8ff9&source=post_page-4a2cd25b8ff9----b104be17716d---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b104be17716d--------------------------------) ·19分钟阅读·2023年1月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb104be17716d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpredicting-metadata-for-humanitarian-datasets-using-gpt-3-b104be17716d&user=Matthew+Harris&userId=4a2cd25b8ff9&source=-----b104be17716d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb104be17716d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpredicting-metadata-for-humanitarian-datasets-using-gpt-3-b104be17716d&source=-----b104be17716d---------------------bookmark_footer-----------)

快速响应人道主义灾难，更好的是，能够预测这些灾难，可以[挽救生命](https://reliefweb.int/report/world/mark-lowcock-under-secretary-general-humanitarian-affairs-and-emergency-relief)[1]。数据是关键，不仅仅是拥有*大量*数据，而是[清洁的数据并且理解良好](https://www.devex.com/news/opinion-humanitarian-world-is-full-of-data-myths-here-are-the-most-popular-91959)[2]，以便创建对实际情况的清晰视图。在许多情况下，这些关键数据被存储在数百个小型电子表格中，因此在进行数据整合时可能非常耗时，并且在发生人道主义事件时，随着新数据的不断涌入，维护这些数据也很困难。自动化数据发现过程可能会加快响应速度并改善受影响人员的结果。

简化发现的一个方法是确保表格数据有描述每列的元数据。这可以帮助将数据集链接在一起，例如知道一个地雷位置表中的列指定了经度和纬度，这与另一个表中定位野战医院的列类似。列名并不总能明显显示它们可能包含的数据，这些数据可能以多种语言呈现，并遵循不同的标准。在理想的情况下，这种元数据是与数据一起提供的，但正如我们下面将看到的那样，这通常不是情况。手动处理这项工作可能非常庞大。

在这篇文章中，我将探讨我们如何通过使用[OpenAI的 GPT-3](https://openai.com/blog/gpt-3-apps/) 大型语言模型来预测人道主义数据集的元数据属性，从而帮助自动化这个过程，并改进以往工作的表现。

# 人道主义数据交换（HDX）

[人道主义数据交换](https://data.humdata.org/)（HDX）是一个极好的平台，旨在通过以标准化的方式将人道主义数据集合在一起，解决这些问题。截至我写这篇文章时，全球共有20,403个数据集，涵盖了广泛的领域和文件类型。这些数据集中的CSV和Excel文件产生了大约148,000个不同的表格，数据量非常庞大！

![](../Images/4bf351851393834c447cc8b83c2844b5.png)

[人道主义数据交换](https://data.humdata.org/)（HDX）平台上的文件类型。有关数据如何汇总的信息，请参见[这个笔记本](https://github.com/datakind/gpt-3-meta-data-discovery/blob/main/hdx_gpt-3_tag_prediction.ipynb)。

# 人道主义交换语言（HXL）

HDX 平台的一个优点是它鼓励数据拥有者使用[人道主义交换语言](https://hxlstandard.org/?_gl=1*p7ffaf*_ga*OTc2MzE0MTI5LjE2NzAxMDE2NjE.*_ga_E60ZNX2F68*MTY3MzQ5NDk2NS4xOC4xLjE2NzM0OTUzNjQuNjAuMC4w)（HXL）格式来标记他们的数据。这些元数据使得将数据结合起来并以有意义的方式使用变得更容易，从而在时间紧迫时加快处理速度。

HXL 标签有两种形式，一种是设置在数据集级别的标签，另一种是应用于表格数据中列的字段级别标签。后者看起来像这样：

![](../Images/ac56a776af2eb693400af0f63fb15ee0.png)

第二行有 HXL 标签的表格示例[[#HXL Standards examples](https://hxlstandard.org/hxlexample/)]

注意列标题下方的第二行，那些是 HXL 标签。它们由前缀为‘#’的标签（例如‘#adm1’）和某些情况下的属性（例如‘+name’）组成。

挑战在于这些字段级标签并不总是设置在 HDX 数据集上，这使得使用那里的数据变得更加困难。查看肯尼亚的 CSV 和 Excel 数据，大多数表格似乎缺少列 HXL 标签。

![](../Images/d797dbe8f75ae6c6827659970a062991.png)

分析[人道主义数据交换](https://data.humdata.org/) (HDX)平台上肯尼亚的数据文件，查看哪些文件有 HXL 列标签。有关如何整理数据的细节，请参见[这个笔记本](https://github.com/datakind/gpt-3-meta-data-discovery/blob/main/hdx_gpt-3_tag_prediction.ipynb)。

如果我们能填补那些空白并为尚未拥有标签的列填充 HXL 标签，那不是很好吗？

微软已经在使用[fastText](https://fasttext.cc/) 嵌入预测 HXL 标签方面做了一些非常出色的工作，请参见[这个笔记本](https://github.com/humanitarian-data-collaboration/hdx-python-model)和相应的[论文](https://www.kdd.org/kdd2019/docs/Humanitarian_Data_tagging_KDD2019_SocialImpactTrack_HXLTagPrediction.pdf) [3]。作者在预测标签方面达到了 95% 的准确率，预测属性方面达到了 92%，表现非常出色。

不过，我想知道我们是否可以使用另一种技术，现在有了一些新技术……

# GPT-3

正如我在[上一篇文章](https://medium.com/@astrobagel/2022-the-year-superstar-ai-models-stepped-onto-the-stage-90eefa95d9b6)中提到的，去年对生成性 AI 似乎真的非常关注。这个故事的明星之一是 Open AI 的[GPT-3](https://openai.com/blog/gpt-3-apps/) 大型语言模型（LLM），它有一些非常惊人的能力。重要的是，它可以经过微调来学习语言的特殊应用模式，如[计算机代码](https://openai.com/blog/openai-codex/)。

所以我想到 HXL 标签只是另一种语言‘特殊情况’，可能可以通过一些 HXL 标签示例对 GPT-3 进行微调，然后查看它是否能对新数据进行预测。

# 从 HDX 获取一些训练数据

首先，值得澄清一下 HDX 数据集、资源和表格的层次结构。‘数据集’可以包含一组‘资源’，这些资源是文件。数据集有自己的页面，比如[这个](https://data.humdata.org/dataset/mli-vegetation-indicators-dekad-admin2)，提供了很多关于历史、上传者和数据集级别标签的有用信息。

![](../Images/add9842a1f5e62a1255154ef3ee78352.png)

HDX 平台上的一个 HDX 数据集示例

上面的示例有两个 CSV 文件资源，如果选择**更多 > 在 HDX 上预览**，可以显示 HXL 标签。

![](../Images/fe0c06003bbc3ef33a58457108499182.png)

一个 [HDX 平台上的示例资源](https://data.humdata.org/dataset/mli-vegetation-indicators-dekad-admin2/resource/be4ec19e-046f-4df2-8772-5615a06aef03)

这是一个超级酷的平台！

我们将下载像上面这样的资源进行分析。HDX 提供了一个 [用于与其 API 交互的 Python 库](https://hdx-python-api.readthedocs.io/en/latest/)，可以通过……进行安装。

```py
pip install hdx-python-api
```

然后你需要设置连接。由于我们仅下载开放数据集，因此不需要设置 API 密钥……

```py
from hdx.utilities.easy_logging import setup_logging
from hdx.api.configuration import Configuration
from hdx.data.dataset import Dataset

setup_logging()
Configuration.create(hdx_site="prod", user_agent="my_agent_name", hdx_read_only=True)
```

经过一些实验，我写了一个小的包装器来下载每个数据集的资源（文件）。它支持 CSV、TSV、XLS 和 XLSX 文件类型，这些类型应该包括足够的表格用于我们的模型微调。它还保存数据集和资源的 HDX JSON 元数据以及每个文件。

```py
def is_supported_filetype(format):
    """
    Checks if the file format is currently supported for extracting meta data.

    Parameters
    ----------
    format : str
        The file format to check.

    Returns
    -------
    bool
        True if the file format is supported, False otherwise.
    """
    matches = ["CSV", "XLSX", "XLS", "TSV"]
    if any(x in format for x in matches):
        return True
    else:
        return False

def download_data(datasets, output_folder):
    """
    Downloads data from HDX. Will save dataset and resource meta data for each file

    Parameters
    ----------
    datasets : pandas.DataFrame
        A dataframe containing the datasets to download.
    output_folder : str
        The folder to download the data to.
    """
    if not os.path.exists(output_folder):
        os.mkdir(output_folder)

    for index, row in datasets.iterrows():
        dataset = Dataset.read_from_hdx(row["id"])
        resources = dataset.get_resources()
        for resource in resources:
            dir = f"./{output_folder}/{row['name']}_{row['id']}"
            print(
                f"Downloading {row['name']} - {resource['name']} - {resource['format']}"
            )
            resource["dataset_name"] = row["name"]
            if not os.path.exists(dir):
                dump_hdx_meta_file(dataset, dir, "dataset.json")
            try:
                dir = f'{dir}/{get_safe_name(resource["name"])}_{get_safe_name(resource["id"])}'
                if not os.path.exists(dir):
                    dump_hdx_meta_file(resource, dir, "resource.json")
                    if is_supported_filetype(resource["format"]):
                        url, path = resource.download(dir)
                    else:
                        print(
                            f"*** Skipping file as it is not a supported filetype *** {resource['name']}"
                        )
                else:
                    print(f"Skipping {dir} as it already exists")
            except Exception as e:
                traceback.print_exc()
                sys.exit()

    print("Done")
```

上述内容有点啰嗦，因为我想能够重新启动下载，并让过程继续从中断的地方开始。此外，API 似乎偶尔会出现错误，可能是由于我的互联网连接，所以里面有一些 Try/Except。通常我不喜欢 Try/Except，但目标是创建一个训练数据集，所以我不介意缺少一些资源，只要我有一个代表性的样本来训练 GPT-3。

使用搜索 HDX API，我们搜索‘HXL’以寻找可能具有 HXL 标签的数据集，然后下载这些文件……

```py
datasets_hxl = pd.DataFrame(Dataset.search_in_hdx("HXL"))
download_data(datasets_hxl, output_folder)
```

这可能需要一段时间（几个小时），所以不妨喝杯好茶！

根据我能发现的，列 HXL 标签在 HDX 资源元数据中未列出，因此要提取这些标签，我们必须分析下载的文件。经过一些实验，我写了一些辅助函数……

```py
def check_hdx_header(first_row):
    """
    This function checks if the first row of a csv file likely an HDX header.
    """
    matches = ["#meta", "#country", "#data", "#loc", "#geo"]
    if any(x in first_row for x in matches):
        return True
    else:
        return False

def set_meta_data_fields(data, file, dataset, resource, sheet, type):
    """
    This function create a data frame with meta data about the data, as well as a snippet of its
    first nrows.

    Parameters:
        data: a dataframe
        file: the name of the data file
        dataset: the dataset JSON object from HDX
        resource: the resource JSON object from HDX
        sheet: the sheet name if the data was a tab in a sheet
        type: the type of file, CSV, XLSX, etc.
    Returns:
        dict: a dictionary with metadata about the dataframe
    """

    nrows = 10

    # Data preview to only include values
    data = data.dropna(axis=1, how="all")

    cols = str(list(data.columns))
    if data.shape[0] > 0:
        first_row = str(list(data.iloc[0]))
        has_hxl_header = check_hdx_header(first_row)
        num_rows = int(data.shape[0])
        num_cols = int(data.shape[1])
        first_nrows = data.head(nrows)
    else:
        first_row = "No data"
        has_hxl_header = "No data"
        num_rows = 0
        num_cols = 0
        first_nrows = None

    dict = {}

    dict["resource_id"] = resource["id"]
    dict["resource_name"] = resource["name"]
    dict["resource_format"] = resource["format"]
    dict["dataset_id"] = dataset["id"]
    dict["dataset_name"] = dataset["name"]
    dict["dataset_org_title"] = dataset["organization"]["title"]
    dict["dataset_last_modified"] = dataset["last_modified"]
    dict["dataset_tags"] = dataset["tags"]
    dict["dataset_groups"] = dataset["groups"]
    dict["dataset_total_res_downloads"] = dataset["total_res_downloads"]
    dict["dataset_pageviews_last_14_days"] = dataset["pageviews_last_14_days"]
    dict["file"] = file
    dict["type"] = type
    dict["dataset"] = dataset
    dict["sheet"] = sheet
    dict["resource"] = resource
    dict["num_rows"] = num_rows
    dict["num_cols"] = num_cols
    dict["columns"] = cols
    dict["first_row"] = first_row
    dict["has_hxl_header"] = has_hxl_header
    dict["first_nrows"] = first_nrows
    return dict

def extract_data_details(f, dataset, resource, nrows, data_details):
    """

    Reads saved CVS and XLSX HDX files and extracts headers, HDX tags and sample data.
    For XLSX files, it extracts data from all sheets.

    Parameters
    ----------
    f : str
        The file name
    dataset : str
        The dataset name
    resource : str
        The resource name
    nrows : int
        The number of rows to read
    data_details : list
        The list of data details

    Returns
    -------
    data_details : list
        The list of data details

    """
    if f.endswith(".xlsx") or f.endswith(".xls"):
        print(f"Loading xslx file {f} ...")
        try:
            sheet_to_df_map = pd.read_excel(f, sheet_name=None)
        except Exception:
            print("An exception occurred trying to read the file {f}")
            return data_details
        for sheet in sheet_to_df_map:
            data = sheet_to_df_map[sheet]
            data_details.append(
                set_meta_data_fields(data, f, dataset, resource, sheet, "xlsx")
            )
    elif f.endswith(".csv"):
        print(f"Loading csv file {f}")
        # Detect encoding
        with open(f, "rb") as rawdata:
            r = chardet.detect(rawdata.read(100000))
        try:
            data = pd.read_csv(f, encoding=r["encoding"], encoding_errors="ignore")
        except Exception:
            print("An exception occurred trying to read the file {f}")
            return data_details
        data_details.append(set_meta_data_fields(data, f, dataset, resource, "", "csv"))
    else:
        type = f.split(".")[-1]
        print(f"Type {type} for {f}")
        data = pd.DataFrame()
        data_details.append(set_meta_data_fields(data, f, dataset, resource, "", type))

    return data_details

# Loop through downloaded folders
def extract_all_data_details(startpath, data_details):
    """
    Extracts all data details for downloaded HDX files in a given directory.

    Parameters
    ----------
    startpath : str
        The path to the directory containing all datasets.
    data_details : list
        Results

    Returns
    -------
    data_details : pandas.DataFrame
        Results, to which new meta data was appended. 
        See function set_meta_data_fields for columns
    """
    for d in os.listdir(startpath):
          d = f"{startpath}/{d}"
          with open(f"{d}/dataset.json") as f:
              dataset = json.load(f)
          for r in os.listdir(d):
              if "dataset.json" not in r:
                  with open(f"{d}/{r}/resource.json") as f:
                      resource = json.load(f)
                  for f in os.listdir(f"{d}/{r}"):
                      file = str(f"{d}/{r}/{f}")
                      if ".json" not in file:
                          data_details = extract_data_details(
                              file, dataset, resource, 5, data_details
                          )
    data_details = pd.DataFrame(data_details)
    return data_details
```

现在我们可以在之前下载的数据文件上运行……

```py
hxl_resources_data_details = extract_all_data_details(f"./data/hxl_datasets/", [])
print(hxl_resources_data_details.shape)

(25695, 22)
```

这个数据框包含 25,695 行，用于在 HDX 上搜索‘HXL’时扫描 CSV 和 Excel 文件找到的每个表格数据集，包含数据预览、列名称和在某些情况下的 HXL 标签。

# 训练/测试拆分

通常，我会简单地使用 [Scikit learn 的 train_test_split](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html) 来处理要用于模型的数据。然而，在这样做时，我注意到同一个数据集中的重复资源（文件）可能会出现在训练集和测试集中。例如，一个组织可能会提供多个机场的文件，每个文件的格式和 HXL 标签完全相同。如果我们生成一个提示数据框然后分割，这些机场将同时出现在训练集和测试集中，这无法很好地反映我们的需求，即预测全新的数据集的 HXL 标签。

为了解决这个问题，我采取了以下措施：

1.  将 HDX ‘数据集’拆分为训练集/测试集（请记住，一个数据集可能包含多个资源文件）

1.  使用每个，我创建了资源的数据框，每行一个数据文件。

1.  然后，使用这些训练/测试资源数据框，我创建了训练/测试数据框，每列一个*行*。这些是 GPT-3 微调所需的提示。

# 创建 GPT-3 微调提示

为了[微调 GPT-3](https://beta.openai.com/docs/guides/fine-tuning)，我们需要提供一个 JSONL 格式的提示和响应训练文件。我决定使用（i）列名；（ii）该列的一个数据样本。补全将是 HXL 标签和属性。

这里是一个例子……

```py
{"prompt": " 'scheduled_service' | \"['1', '1', '0', '0', '0', '0', '0', '0']\"", "completion": " #status+scheduled"}
```

GPT-3 的格式非常特殊，花了一段时间才调整好！像在补全开始处添加空格这样的设置是 OpenAI 推荐的。

```py
 def get_hxl_tags_list(resources):
    """
    Build a list of the HXL tags found in a dataframe of HDX resources.

    Parameters
    ----------
    resources : pandas dataframe
        A dataframe of HDX resources

    Returns
    -------
    hxl_tags : list
        A list of HXL tags.
    """
    hxl_tags = []
    for row, d in resources.iterrows():
        if d["has_hxl_header"] == True:
            fr = d["first_row"].replace(" ", "")
            for c in fr.split(","):
                fr = re.sub("\[|\]|\"|\'","", c)
                hdxs = fr.split("+")
                for h in hdxs:
                    if h not in hxl_tags and len(h) > 0:
                        hxl_tags.append(h.lower())
    hxl_tags = list(set(hxl_tags))
    hxl_tags.remove('nan')
    return hxl_tags

def get_prompt(col_name, data):
    """
    Builds the prompt for GPT-3 for predicting HXL tags and attributes

    Parameters
    ----------
    col_name : str
        Column name
    data : list
        A list of sample data for the column

    Returns
    -------
    prompt : string
        A prompt for GPT-3.
    """
    ld = len(data) - 1
    col_data = json.dumps(str(list(data.iloc[1:ld])))
    prompt = f" {col_name} | {col_data}".lower()
    return prompt

def create_training_set(resources):
    """
    Builds a jsonl training data file for GPT-3 where each row is a prompt for a column HXL tag.

    It will only output prompts where the sample data for the column didn't contain nans.

    Parameters
    ----------
    resources : pandas dataframe
        A dataframe of HDX resources

    Returns
    -------
    train_data : list
        A list of prompts and completions for the HXL tag autocomplete feature.
    """
    train_data = []
    for row, d in resources.iterrows():
        if d["has_hxl_header"] == True:
            cols = d["columns"][1:-1].split(",")
            hdxs = d["first_row"][1:-1].split(",")
            data = d["first_nrows"]
            has_hxl_header = d["has_hxl_header"]
            if len(cols) == len(hdxs) and len(cols) > 1:
                ld = len(data) - 1
                for i in range(0, len(cols)):
                    if i < len(hdxs):
                        hdx = re.sub("'|\"", "", hdxs[i])
                    # Only include is has HXL tags and good sample data in column
                    if has_hxl_header == True and hdx != np.nan:
                        prompt = get_prompt(cols[i], data.iloc[:,i])
                        if 'nan' not in hdx and 'nan, nan' not in prompt:
                            p = {
                                "prompt": prompt,
                                "completion": f" {hdx}",
                            }
                            train_data.append(p)
    return train_data
```

你会注意到我在上面排除了数据中存在 NaNs 的提示。我认为我们应该从良好的数据样本开始，但这是需要在未来重新审视的内容。

我们现在可以生成一个训练数据集，并将其保存为 GPT-3 的文件……

```py
# Create training set
X_train = create_training_set(X_train_resources)
print(f"Training records: {len(X_train)}")

train_file = "fine_tune_openai_train.jsonl"

with open(train_file, "w") as f:
    for p in X_train:
        json.dump(p, f)
        f.write("\n")

print("Done")
```

这就是训练数据的样子……

```py
{"prompt": "  'Country ISO3' | \"['COD', 'COD', 'COD', 'COD', 'COD', 'COD', 'COD', 'COD']\"", "completion": "  #country+code"}
{"prompt": "  'Year' | \"['2010', '2005', '2000', '1995', '1990', '1985', '1980', '1975']\"", "completion": "  #date+year"}
{"prompt": "  'Indicator Name' | \"['Barro-Lee: Percentage of female population age 15-19 with no education', 'Barro-Lee: Percentage of female population age 15-19 with no education', 'Barro-Lee: Percentage of female population age 15-19 with no education', 'Barro-Lee: Percentage of female population age 15-19 with no education', 'Barro-Lee: Percentage of female population age 15-19 with no education', 'Barro-Lee: Percentage of female population age 15-19 with no education', 'Barro-Lee: Percentage of female population age 15-19 with no education', 'Barro-Lee: Percentage of female population age 15-19 with no education']\"", "completion": "  #indicator+name"}
{"prompt": "  'Indicator Code' | \"['BAR.NOED.1519.FE.ZS', 'BAR.NOED.1519.FE.ZS', 'BAR.NOED.1519.FE.ZS', 'BAR.NOED.1519.FE.ZS', 'BAR.NOED.1519.FE.ZS', 'BAR.NOED.1519.FE.ZS', 'BAR.NOED.1519.FE.ZS', 'BAR.NOED.1519.FE.ZS']\"", "completion": "  #indicator+code"}
{"prompt": "  'Value' | \"['48.1', '51.79', '52.1', '43.62', '35.44', '38.02', '43.47', '49.08']\"", "completion": "  #indicator+value+num"}
{"prompt": " 'Country ISO3' | \"['COD', 'COD', 'COD', 'COD', 'COD', 'COD', 'COD', 'COD']\"", "completion": " #country+code"}
{"prompt": "  'Year' | \"['2015', '2014', '2013', '2012', '2011', '2010', '2009', '2008']\"", "completion": "  #date+year"}
```

这个训练数据集中有 139,503 行，每列一行，来自我们从 HDX 下载的表格数据，专门用于那些列中有 HXL 标签的情况。

# 生成 OpenAI API 密钥

在我们能做任何事情之前，你需要先[注册一个 OpenAI 账户](https://auth0.openai.com/u/signup/identifier?state=hKFo2SBHOUJWNE9YNnVfbnFxQWJTbXhESW5SNlpsNlFBNzFPaaFur3VuaXZlcnNhbC1sb2dpbqN0aWTZIFdJa0xtVTAxV21POU1WbWRVNmNGVmxvWkwyMDBGY1pVo2NpZNkgRFJpdnNubTJNdTQyVDNLT3BxZHR3QjNOWXZpSFl6d0Q)。完成后，你应该有[$18 的免费积分](https://openai.com/api/pricing/)。如果使用少量数据，这应该足够，但在这次分析和几次模型训练中，我累计了 $50 的账单，因此你可能需要将信用卡绑定到你的账户上。

一旦你有了账户，你可以[生成 API 密钥](https://beta.openai.com/account/api-keys)。我选择将其保存到本地文件并在代码中引用，但[OpenAI Python 库](https://github.com/openai/openai-python)也支持使用环境变量。

# 微调 GPT-3

好了，现在是令人兴奋的部分！有了我们精美的训练数据，我们可以按如下方式微调 GPT-3……

```py
import openai
from openai import cli

# Open AI API key should be put into this file
openai.api_key_path = "./api_key.txt"

print("Uploading training file ...")
training_id = cli.FineTune._get_or_upload(train_file, True)
# validation_id = cli.FineTune._get_or_upload(validation_file_name, True)

print("Fine-tuning model ...")
create_args = {
    "training_file": training_id,
    # "validation_file": test_file,
    "model": "ada",
}
# https://beta.openai.com/docs/api-reference/fine-tunes/create
resp = openai.FineTune.create(**create_args)
job_id = resp["id"]
status = resp["status"]

print(f"Fine-tunning model with jobID: {job_id}.")
```

在上面，我们将微调模型提交给 OpenAI，然后可以查看状态……

```py
result = openai.FineTune.retrieve(id=job_id)
print(result['status'])
```

我选择保持简单，但你也可以将其提交给 OpenAI，并通过[此处](https://github.com/openai/openai-cookbook/blob/main/examples/azure/finetuning.ipynb)显示的流来监控状态。

一旦状态显示为‘成功’，你现在可以获得一个模型 ID 用于预测（补全）……

```py
result = openai.FineTune.retrieve(id=job_id)
model = result["fine_tuned_model"]
```

# 用我们微调过的 GPT-3 模型预测 HXL 标签

我们现在有了一个模型，来看看它能做什么！

要调用 GPT-3，你可以使用[Open AI Python 库的 ‘create’ 方法](https://beta.openai.com/docs/api-reference/completions/create)。查看文档了解你可以调整的参数是值得的。

```py
def create_prediction_dataset_from_resources(resources):
    """
    Generate a list of model column-level prompts from a list of resources (tables).

    It will only output prompts where the sample data for the column didn't contain nans.

    Parameters
    ----------
    resources : list
        A list of dictionaries containing the resource name, columns, first_row, and first_nrows.

    Returns
    -------
    prediction_data : list
        A list of dictionaries containing GPT-3 prompts (one per column in resource table)
    """

    prediction_data = []
    for index, d in resources.iterrows():
        cols = d["columns"][1:-1].split(",")
        hdxs = d["first_row"][1:-1].split(",")
        data = d["first_nrows"]
        has_hxl_header = d["has_hxl_header"]
        if len(cols) == len(hdxs) and len(cols) > 1:
            ld = len(data) - 1
            # Loop through columns 
            for i in range(0, len(cols)):
                if i < len(hdxs) and i < data.shape[1]:
                    prompt = get_prompt(cols[i], data.iloc[:,i])
                    # Skip any prompts with at least two nan values in sample data
                    if 'nan, nan' not in prompt:
                        r = {
                                "prompt": prompt
                            }
                        # If we were called with HXL tags (ie for test set), populate 'expected'
                        if has_hxl_header == True:
                            hdx = re.sub("'|\"| ", "", hdxs[i])
                            # Row has HXL tags, but this particular column doesn't have tags
                            if hdx == 'nan':
                                continue
                            else:
                                r["expected"]= hdx
                        prediction_data.append(r)
    return prediction_data

def make_gpt3_prediction(prompt, model, temperature=0.99, max_tokens=13):
    """
    Wrapper to call GPT-3 to make a prediction (completion) on a single prompt.

    Parameters
    ----------
    prompt : str
        Prompt to use for prediction
    model : str
        GPT-3 model to use
    temperature : float
        Temperature to use for sampling
    max_tokens : int
        Maximum number of tokens to use for sampling

    Returns
    -------
    result : dict
        Dictionary with prompt, predicted, and 
        log probabilities of each completed token
    """
    result = {}
    result["prompt"] = prompt
    model_result = openai.Completion.create(
        engine=model,
        prompt=prompt,
        temperature=temperature,
        max_tokens=max_tokens,
        top_p=1,
        frequency_penalty=0,
        presence_penalty=0,
        stop=["\n"],
        logprobs=1
    )
    result["predicted"] = model_result["choices"][0]["text"].replace(" ","")
    result["logprobs"]  = model_result['choices'][0]['logprobs']['top_logprobs']
    return result

def make_gpt3_predictions(
    sample_size, prediction_data, model, temperature=0.99, max_tokens=13, logprob_cutoff=-0.01
):

    """
    Wrapper to call GPT-3 to make predictions on test file for sample_size samples.

    Parameters
    ----------
    sample_size : int
        Number of predictions to make from test file
    prediction_data : list
        List of dictionaries with prompts
    model : str
        GPT-3 model to use
    postprocess : bool
        Whether to postprocess the predictions
    temperature : float
        Temperature to use for sampling
    max_tokens : int
        Maximum number of tokens to use for sampling
    prob_cutoff : float
        Logprob cutoff for filtering out low probability tokens

    Returns
    -------
    results : list
        List of dictionaries with prompt, predicted, predicted_post_processed
    """
    results = []
    prediction_data = sample(prediction_data, sample_size)
    for i in range(0, sample_size):
        prompt = prediction_data[i]["prompt"]
        res = make_gpt3_prediction(
            prompt, model, temperature, max_tokens
        )

        # Filter out low logprob predictions
        pred = ""
        seen_tokens = []
        for w in res["logprobs"]:
            token = list(w.keys())[0]
            prob = w[token]
            if prob > logprob_cutoff and token not in seen_tokens:
                pred += token
                if '+' not in token:
                    seen_tokens.append(token)
            else:
                break
        pred = re.sub(r" |\+$|\+v_$", "", pred)

        r = {
                "prompt": prompt,
                "predicted": res["predicted"],
                "predicted_log_prob_cutoff": pred,
                #"logprobs": res["logprobs"]
            }
        # For test sets we have expected values, add back for performance reporting
        if "expected" in prediction_data[i]:
            r['expected'] = prediction_data[i]['expected'].replace(' ', '')
        results.append(r)
    return results
```

我们使用以下方式调用，限制为 500 个提示……

```py
# Generate the prompts we want GPT-3 to complete
print("Building model input ...")
prediction_data = create_prediction_dataset_from_resources(X_test_resources)

# How many predictions to try from the test set
sample_size = 500

# Make the predictions
print("Making GPT-3 predictions (completions) ...")
results = make_gpt3_predictions(
   sample_size, prediction_data, model, temperature=0.99, max_tokens=20, logprob_cutoff=-0.001
) 
```

这产生了以下结果……

```py
def output_prediction_metrics(results, prediction_field="predicted_post_processed"):
    """
    Prints out model performance report if provided results in the format:

    [
        {
            'prompt': ' \'ISO3\' | "[\'RWA\', \'RWA\', \'RWA\', \'RWA\', \'RWA\', \'RWA\', \'RWA\', \'RWA\']"', 
            'predicted': ' #country+code+iso3+v_iso3+', 
            'predicted_post_processed': '#country+code', 
            'expected': '#country+code'
        }, 
        ... etc ...
    ]

    Parameters
    ----------
    results : list
        See above for format
    prediction_field : str
        Field name of element with prediction. Handy for comparing raw and post-processed predictions.
    """
    y_test = []
    y_pred = []
    y_justtag_test = []
    y_justtag_pred = []
    for r in results:
        if "expected" not in r:
            print("Provided results do not contain expected values.")
            sys.exit()
        y_pred.append(r[prediction_field])
        y_test.append(r["expected"])
        expected_tag = r["expected"].split("+")[0]
        predicted_tag = r[prediction_field].split("+")[0]
        y_justtag_test.append(expected_tag)
        y_justtag_pred.append(predicted_tag)

    print(f"GPT-3 results for {prediction_field}, {len(results)} predictions ...")
    print("\nJust HXL tags ...\n")
    print(f"Accuracy: {round(accuracy_score(y_justtag_test, y_justtag_pred),2)}")
    print(
        f"Precision: {round(precision_score(y_justtag_test, y_justtag_pred, average='weighted', zero_division=0),2)}"
    )
    print(
        f"Recall: {round(recall_score(y_justtag_test, y_justtag_pred, average='weighted', zero_division=0),2)}"
    )
    print(
        f"F1: {round(f1_score(y_justtag_test, y_justtag_pred, average='weighted', zero_division=0),2)}"
    )

    print(f"\nTags and attributes with {prediction_field} ...\n")
    print(f"Accuracy: {round(accuracy_score(y_test, y_pred),2)}")
    print(
        f"Precision: {round(precision_score(y_test, y_pred, average='weighted', zero_division=0),2)}"
    )
    print(
        f"Recall: {round(recall_score(y_test, y_pred, average='weighted', zero_division=0),2)}"
    )
    print(
        f"F1: {round(f1_score(y_test, y_pred, average='weighted', zero_division=0),2)}"
    )

    return 
```

```py
output_prediction_metrics(results, prediction_field="predicted")

GPT-3 results for predicted, 500 predictions ...

Just HXL tags ...

Accuracy: 0.99
Precision: 0.99
Recall: 0.99
F1: 0.99

Tags and attributes with predicted ...

Accuracy: 0.0
Precision: 0.0
Recall: 0.0
F1: 0.0
```

嗯！？那，嗯……糟糕。仅预测 HXL 标签效果非常好，但预测标签和属性的效果就差很多。

让我们看看一些失败的预测……

```py
 {
    "prompt": " 'gho (code)' | \"['mort_100', 'mort_100', 'mort_100', 'mort_100', 'mort_100', 'mort_100', 'mort_100', 'mort_100']\"",
    "predicted": "#indicator+code+v_hor_funder_",
    "expected": "#indicator+code"
}
{
    "prompt": "  'region (code)' | \"['afr', 'afr', 'afr', 'afr', 'afr', 'afr', 'afr', 'afr']\"",
    "predicted": "#region+code+v_reliefweb+f",
    "expected": "#region+code"
}
{
    "prompt": "  'dataid' | \"['310633', '310634', '310635', '310636', '310629', '310631', '310630', '511344']\"",
    "predicted": "#meta+id+fts_internal_view_all",
    "expected": "#meta+id"
}
{
    "prompt": "  'gho (url)' | \"['https://www.who.int/data/gho/indicator-metadata-registry/imr-details/5580', 'https://www.who.int/data/gho/indicator-metadata-registry/imr-details/5580']\"",
    "predicted": "#indicator+url+name+has_more_",
    "expected": "#indicator+url"
}
{
    "prompt": "  'year (display)' | \"['2014', '2014', '2014', '2014', '2014', '2014', '2014', '2014']\"",
    "predicted": "#date+year+name+tariff+for+",
    "expected": "#date+year"
}
{
    "prompt": "  'byvariablelabel' | \"[nan]\"",
    "predicted": "#indicator+label+code+placeholder+Hubble",
    "expected": "#indicator+label"
}
{
    "prompt": " 'gho (code)' | \"['ntd_bejelstatus', 'ntd_pintastatus', 'ntd_yawsend', 'ntd_leishcend', 'ntd_leishvend', 'ntd_leishcnum_im', 'ntd_leishcnum_im', 'ntd_leishcnum_im']\"",
    "predicted": "#indicator+code+v_ind+olk_ind",
    "expected": "#indicator+code"
}
{
    "prompt": "  'enddate' | \"['2002-12-31', '2003-12-31', '2004-12-31', '2005-12-31', '2006-12-31', '2007-12-31', '2008-12-31', '2009-12-31']\"",
    "predicted": "#date+enddate+enddate+usd+",
    "expected": "#date+end"
}
{
    "prompt": "  'endyear' | \"['2013', '2013', '2013', '2013', '2013', '2013', '2013', '2013']\"",
    "predicted": "#date+year+endyear+end_of_",
    "expected": "#date+year+end"
}
{
    "prompt": "  'country (code)' | \"['dnk', 'dnk', 'dnk', 'dnk', 'dnk', 'dnk', 'dnk', 'dnk']\"",
    "predicted": "#country+code+v_iso2+v_",
    "expected": "#country+code"
}
```

有趣的是。似乎模型几乎完美地完成并捕捉了正确的标签和属性，然后在末尾添加了一些额外的属性。例如……

```py
"predicted": "#country+code+v_iso2+v_",
"expected": "#country+code"
```

让我们看看期望的标签和属性在预测的前半部分出现的频率……

```py
passes = 0
fails = 0
for r in results:
    if r["predicted"].startswith(r["expected"]):
        passes += 1
    else:
        fails += 1
        #print(json.dumps(r, indent=4, sort_keys=False))

print(f" Out of {passes + fails} predictions, the expected tags and attributes where in the predicted tags and attributes {round(100*passes/(passes+fails),1)}% of the time.")

Out of 500 predictions, the expected tags and attributes where in the predicted tags and attributes 99.0% of the time.
```

在 500 次预测中，期望的标签和属性**99%**的时间都出现在预测的标签和属性中。换句话说，期望的值通常是大多数预测的首部分。

所以 GPT-3 在预测标签和属性方面具有很高的准确性，但在末尾添加了额外的属性。

那么，如何排除那些额外的标记呢？

嗯，结果证明 GPT-3 返回了每个标记的对数概率。如上所述，我们还计算了一个预测，假设我们在对数概率高于某个截止值时停止完成标记……

```py
# Filter out low logprob predictions
pred = ""
seen_tokens = []
for w in res["logprobs"]:
    token = list(w.keys())[0]
    prob = w[token]
    if prob > logprob_cutoff and token not in seen_tokens:
        pred += token
        if '+' not in token:
            seen_tokens.append(token)
    else:
        break
pred = re.sub(r" |\+$|\+v_$", "", pred)
```

让我们看看在假设截止值为 -0.001 的情况下表现如何……

```py
output_prediction_metrics(results, prediction_field="predicted_log_prob_cutoff")

Just HXL tags ...

Accuracy: 0.99
Precision: 1.0
Recall: 0.99
F1: 0.99

Tags and attributes with predicted_log_prob_cutoff ...

Accuracy: 0.94
Precision: 0.99
Recall: 0.94
F1: 0.95
```

这还不错，标签和属性的准确率为 0.94。既然我们知道正确的标签和属性在预测中出现的概率为 99%，我们应该通过调整对数概率截止值和进行一些后处理来做得更好。

# 结论与未来工作

以上是对 GPT-3 在预测元数据，特别是人道主义数据集上的 HXL 标签方面应用的快速分析。它在这一任务上的表现非常好，并且在类似的元数据预测任务中有很大的潜力。

当然，还需要更多的工作来完善方法，例如：

1.  尝试其他模型（我上面使用的是 'ada'）以查看是否能改善性能（尽管这会增加成本）。

1.  模型超参数调整。对数概率截止值可能非常重要。

1.  可能需要更多的提示工程，比如在表格中包含列列表，以提供更好的上下文，以及在两行标题表格上的覆盖列。

1.  更多的预处理。这篇文章的处理不多，盲目地使用从 CSV 文件中提取的表格，因此数据可能有些混乱。

也就是说，我觉得使用 GPT-3 来预测数据集上的元数据有很大的潜力。

敬请关注更多更新！

# 参考文献

[1] Mark Lowcock, 人道主义事务副秘书长和紧急救援协调员，[Anticipation saves lives: How data and innovative financing can help improve the world’s response to humanitarian crises](https://reliefweb.int/report/world/mark-lowcock-under-secretary-general-humanitarian-affairs-and-emergency-relief) (2019)

[2] Sarah Telford, [Opinion: Humanitarian world is full of data myths. Here are the most popular](https://www.devex.com/news/opinion-humanitarian-world-is-full-of-data-myths-here-are-the-most-popular-91959) (2018)

[3] Vinitra Swamy 等人，[人道主义数据的机器学习：使用 HXL 标准进行标签预测](https://www.kdd.org/kdd2019/docs/Humanitarian_Data_tagging_KDD2019_SocialImpactTrack_HXLTagPrediction.pdf)（2019年）

用于此分析的笔记本可以在[这里](https://github.com/datakind/gpt-3-meta-data-discovery)找到。
