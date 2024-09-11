# 临床试验结果预测

> 原文：[https://towardsdatascience.com/clinical-trial-outcome-prediction-a4c6d279fd42?source=collection_archive---------6-----------------------#2023-10-04](https://towardsdatascience.com/clinical-trial-outcome-prediction-a4c6d279fd42?source=collection_archive---------6-----------------------#2023-10-04)

## *第1部分：多模态健康数据嵌入*

[](https://medium.com/@lenlan?source=post_page-----a4c6d279fd42--------------------------------)[![Lennart Langouche](../Images/ca35c7112cb845d17e1148b4f282600e.png)](https://medium.com/@lenlan?source=post_page-----a4c6d279fd42--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a4c6d279fd42--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a4c6d279fd42--------------------------------) [Lennart Langouche](https://medium.com/@lenlan?source=post_page-----a4c6d279fd42--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4bee88ffae4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclinical-trial-outcome-prediction-a4c6d279fd42&user=Lennart+Langouche&userId=4bee88ffae4&source=post_page-4bee88ffae4----a4c6d279fd42---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4c6d279fd42--------------------------------) ·8分钟阅读·2023年10月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa4c6d279fd42&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclinical-trial-outcome-prediction-a4c6d279fd42&user=Lennart+Langouche&userId=4bee88ffae4&source=-----a4c6d279fd42---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa4c6d279fd42&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclinical-trial-outcome-prediction-a4c6d279fd42&source=-----a4c6d279fd42---------------------bookmark_footer-----------)

我最近遇到了一篇文章：[HINT: 临床试验结果预测的层次交互网络](https://www.cell.com/patterns/pdf/S2666-3899(22)00018-6.pdf) 由 Fu 等人撰写。这是一个有趣的真实数据科学应用，激发了我创建自己的项目，在该项目中，我尝试基于来自[ClinicalTrials.gov](http://clinicaltrials.gov)的公开信息预测临床试验结果。

项目的目标是预测临床试验的结果（二元结果：失败与成功），无需实际进行试验。我们将使用来自 [ClinicalTrials.gov](http://clinicaltrials.gov) 的公开临床试验信息，如*药物分子、疾病指示、试验方案、赞助商*和*参与者数量*，并使用不同的工具，如 BioBERT、SBERT 和 DeepPurpose，将其嵌入（转换为向量表示）。

![](../Images/3562ccfd368279055d7b53f61345d436.png)

工作流程示意图（图片由作者提供）

在本系列的第一部分，我专注于嵌入多模态临床试验数据。在 [第二部分](https://medium.com/@lennart.langouche/clinical-trial-outcome-prediction-7ce6c27831f9) 中，我使用 XGBoost 模型预测试验结果（二元预测：失败与成功），并简要比较我的简单 XGBoost 模型与 [文章](https://doi.org/10.1016/j.patter.2022.100445) 中的 HINT 模型的性能。

![](../Images/598fd825153e7854fa7a1e821e1cbb35.png)

本系列第 1 部分的重点：将多模态临床试验数据嵌入向量（图片由作者提供）

本文中我将遵循以下步骤：

+   从 [ClinicalTrials.gov](http://ClinicalTrials.gov) 收集所有临床试验记录

+   阅读和解析获得的 XML 文件

+   使用 [tiny-biobert](https://huggingface.co/nlpie/tiny-biobert) 嵌入**疾病指示**，这是 [Rohanian et al](https://doi.org/10.1093/bioinformatics/btad103) 的紧凑版本，[BioBERT](https://doi.org/10.1093/bioinformatics/btz682)

+   使用 [tiny-biobert](https://huggingface.co/nlpie/tiny-biobert) 嵌入临床试验**纳入/排除标准**

+   使用 [all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) 嵌入**赞助商信息**，这是来自 [SentenceBERT](https://doi.org/10.48550/arXiv.1908.10084) 的强大预训练句子编码器

+   将**药物名称**转换为其 SMILES 表示形式，然后使用 [DeepPurpose](https://github.com/kexinhuang12345/DeepPurpose) 和 [Huang et al.](https://doi.org/10.1093/bioinformatics/btaa1005) 转换为其 Morgan 指纹

你可以在此 Jupyter notebook 中遵循所有步骤：[临床试验嵌入教程](https://github.com/lenlan/clinical-trial-prediction/blob/main/clinical_trial_embedding_tutorial.ipynb)。

# **从** ClinicalTrials.gov 收集临床试验记录

我建议在命令行中运行整个过程，因为它既耗时又占用空间。如果你的系统上没有安装 wget，可以查看 [如何安装 wget](https://www.jcchouinard.com/wget/#How_to_Install_Wget)。打开命令行/终端并输入以下命令：

```py
# 0\. Clone repository
# Navigate to the directory where you want to clone the repository and type:
git clone https://github.com/lenlan/clinical-trial-prediction.git
cd clinical-trial-prediction

# 1\. Download data
mkdir -p raw_data
cd raw_data
wget https://clinicaltrials.gov/AllPublicXML.zip # This will take 10-20 minutes to download

# 2\. Unzip the ZIP file.
# The unzipped file occupies approximately 11 GB. Please make sure you have enough space. 
unzip AllPublicXML.zip # This might take over an hour to run, depending on your system
cd ../

# 3\. Collect and sort all the XML files and put output in all_xml
find raw_data/ -name NCT*.xml | sort > data/all_xml
head -3 data/all_xml

### Output:
# raw_data/NCT0000xxxx/NCT00000102.xml
# raw_data/NCT0000xxxx/NCT00000104.xml
# raw_data/NCT0000xxxx/NCT00000105.xml

# NCTID is the identifier of a clinical trial. `NCT00000102`, `NCT00000104`, `NCT00000105` are all NCTIDs. 

# 4\. Remove ZIP file to recover some disk space
rm raw_data/AllPublicXML.zip
```

# 阅读和解析获得的 XML 文件

现在你已经将临床试验作为单独的文件保存在硬盘上，我们将通过解析 XML 文件提取所需信息。

```py
from xml.etree import ElementTree as ET
# function adapted from https://github.com/futianfan/clinical-trial-outcome-prediction
def xmlfile2results(xml_file):
    tree = ET.parse(xml_file)
    root = tree.getroot()
    nctid = root.find('id_info').find('nct_id').text ### nctid: 'NCT00000102'
    print("nctid is", nctid)
    study_type = root.find('study_type').text
    print("study type is", study_type)
    interventions = [i for i in root.findall('intervention')]
    drug_interventions = [i.find('intervention_name').text for i in interventions \
              if i.find('intervention_type').text=='Drug']
    print("drug intervention:", drug_interventions)
    ### remove 'biologics', 
    ### non-interventions 
    if len(drug_interventions)==0:
        return (None,)

    try:
        status = root.find('overall_status').text 
        print("status:", status)
    except:
        status = ''

    try:
        why_stop = root.find('why_stopped').text
        print("why stop:", why_stop)
    except:
        why_stop = ''

    try:
        phase = root.find('phase').text
        print("phase:", phase)
    except:
        phase = ''
    conditions = [i.text for i in root.findall('condition')] ### disease 
    print("disease", conditions)

    try:
        criteria = root.find('eligibility').find('criteria').find('textblock').text
        print('found criteria')
    except:
        criteria = ''

    try:
        enrollment = root.find('enrollment').text
        print("enrollment:", enrollment)
    except:
        enrollment = ''

    try:
        lead_sponsor = root.find('sponsors').find('lead_sponsor').find('agency').text 
        print("lead_sponsor:", lead_sponsor)
    except:
        lead_sponsor = ''

    data = {'nctid':nctid,
           'study_type':study_type,
           'drug_interventions':[drug_interventions],
           'overall_status':status,
           'why_stopped':why_stop,
           'phase':phase,
           'indications':[conditions],
           'criteria':criteria,
           'enrollment':enrollment,
           'lead_sponsor':lead_sponsor}
    return pd.DataFrame(data)

### Output:
# nctid is NCT00040014
# study type is Interventional
# drug intervention: ['exemestane']
# status: Terminated
# phase: Phase 2
# disease ['Breast Neoplasms']
# found criteria
# enrollment: 100
# lead_sponsor: Pfizer
```

# 使用句子变换器嵌入信息 — 示例

首先我们需要安装sentence-transformers库。

```py
pip install -U sentence-transformers
```

```py
from sentence_transformers import SentenceTransformer
sentences = ["This is an example sentence", "Each sentence is converted"]
#all-MiniLM-L6-v2 encodes each sentence into a 312-dimensional vector
model = SentenceTransformer('all-MiniLM-L6-v2')
embeddings = model.encode(sentences)
print(embeddings.shape)

### Output:
# (2, 312)
```

我们成功地将两个句子转换为一个312维的向量表示。

# 使用tiny-biobert嵌入**疾病指示**

首先我们创建一个字典，将每个指示映射到其312维的嵌入表示，使用[tiny-biobert](https://huggingface.co/nlpie/tiny-biobert)。然后，我们创建一个直接将每个试验标识符映射到其疾病嵌入表示的字典。当试验包含多个指示时，我们取它们的均值作为向量表示。

```py
def create_indication2embedding_dict():
    # Import toy dataset
    toy_df = pd.read_pickle('data/toy_df.pkl')

    # Create list with all indications and encode each one into a 312-dimensional vector
    all_indications = sorted(set(reduce(lambda x, y: x + y, toy_df['indications'].tolist())))     

    # Using 'nlpie/tiny-biobert', a smaller version of BioBERT
    model = SentenceTransformer('nlpie/tiny-biobert')
    embeddings = model.encode(all_indications, show_progress_bar=True)

    # Create dictionary mapping indications to embeddings
    indication2embedding_dict = {}
    for key, row in zip(all_indications, embeddings):
        indication2embedding_dict[key] = row
    pickle.dump(indication2embedding_dict, open('data/indication2embedding_dict.pkl', 'wb')) 

    embedding = []
    for indication_lst in tqdm(toy_df['indications'].tolist()):
        vec = []
        for indication in indication_lst:
            vec.append(indication2embedding_dict[indication])
        print(np.array(vec).shape) # DEBUG
        vec = np.mean(np.array(vec), axis=0)
        print(vec.shape) # DEBUG
        embedding.append(vec)
    print(np.array(embedding).shape)

    dict = zip(toy_df['nctid'], np.array(embedding))
    nctid2disease_embedding_dict = {}
    for key, row in zip(toy_df['nctid'], np.array(embedding)):
        nctid2disease_embedding_dict[key] = row
    pickle.dump(nctid2disease_embedding_dict, open('data/nctid2disease_embedding_dict.pkl', 'wb'))  

create_indication2embedding_dict()
```

# 使用tiny-biobert嵌入临床试验的纳入/排除标准

以非常类似的方式，我们编码临床试验的纳入/排除标准。需要进行一些额外的数据清理，以使文本格式正确。我们分别编码纳入标准和排除标准，每个标准的嵌入表示是其包含的句子的均值向量表示。

```py
def create_nctid2protocol_embedding_dict():
     # Import toy dataset
    toy_df = pd.read_pickle('data/toy_df.pkl')

    # Using 'nlpie/tiny-biobert', a smaller version of BioBERT
    model = SentenceTransformer('nlpie/tiny-biobert')

    def criteria2vec(criteria):
        embeddings = model.encode(criteria)
#         print(embeddings.shape) # DEBUG
        embeddings_avg = np.mean(embeddings, axis=0)
#         print(embeddings_avg.shape) # DEBUG
        return embeddings_avg

    nctid_2_protocol_embedding = dict()
    print(f"Embedding {len(toy_df)*2} inclusion/exclusion criteria..")
    for nctid, protocol in tqdm(zip(toy_df['nctid'].tolist(), toy_df['criteria'].tolist())):    
#         if(nctid == 'NCT00003567'): break #DEBUG
        split = split_protocol(protocol)
        if len(split)==2:
            embedding = np.concatenate((criteria2vec(split[0]), criteria2vec(split[1])))
        else: 
            embedding = np.concatenate((criteria2vec(split[0]), np.zeros(312)))
        nctid_2_protocol_embedding[nctid] = embedding
#         for key in nctid_2_protocol_embedding: #DEBUG
#             print(f"{key}:{nctid_2_protocol_embedding[key].shape}") #DEBUG
    pickle.dump(nctid_2_protocol_embedding, open('data/nctid_2_protocol_embedding_dict.pkl', 'wb'))   
    return 

create_nctid2protocol_embedding_dict()
```

# 使用all-MiniLM-L6-v2嵌入**赞助商信息**，这是SentenceBERT提供的一个强大的预训练句子编码器

我选择使用句子嵌入([SBERT](https://www.sbert.net/index.html))来编码试验赞助商。使用Label或One-Hot编码等简单方法也可以，但我希望能够捕捉赞助商名称之间的相似性，以防有拼写错误或多个不同的拼写。我使用预训练的[all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2)模型，它在基准数据集上具有高速和高性能。它将每个赞助机构转换为一个384维的向量。

```py
def create_sponsor2embedding_dict():
    # Import toy dataset
    toy_df = pd.read_pickle('data/toy_df.pkl')

    # Create list with all indications and encode each one into a 384-dimensional vector
    all_sponsors = sorted(set(toy_df['lead_sponsor'].tolist()))     

    # Using 'all-MiniLM-L6-v2', a pre-trained model with excellent performance and speed
    model = SentenceTransformer('all-MiniLM-L6-v2')
    embeddings = model.encode(all_sponsors, show_progress_bar=True)
    print(embeddings.shape)

    # Create dictionary mapping indications to embeddings
    sponsor2embedding_dict = {}
    for key, row in zip(all_sponsors, embeddings):
        sponsor2embedding_dict[key] = row
    pickle.dump(sponsor2embedding_dict, open('data/sponsor2embedding_dict.pkl', 'wb'))

create_sponsor2embedding_dict()
```

# 将**药物名称**转换为SMILES表示，然后使用DeepPurpose转换为Morgan指纹

分子可以用SMILES字符串表示。SMILES是一种编码分子结构的线性符号表示法。药物分子数据从[ClinicalTrials.gov](http://ClinicalTrials.gov)提取，并通过[CACTUS](https://cactus.nci.nih.gov/)链接到其分子结构（SMILES字符串）。

```py
import requests

def get_smiles(drug_name):
    # URL for the CIR API
    base_url = "https://cactus.nci.nih.gov/chemical/structure"
    url = f"{base_url}/{drug_name}/smiles"

    try:
        # Send a GET request to retrieve the SMILES representation
        response = requests.get(url)

        if response.status_code == 200:
            smiles = response.text.strip()  # Get the SMILES string
            print(f"Drug Name: {drug_name}")
            print(f"SMILES: {smiles}")
        else:
            print(f"Failed to retrieve SMILES for {drug_name}. Status code: {response.status_code}")
            smiles = ''

    except requests.exceptions.RequestException as e:
        print(f"An error occurred: {e}")

    return smiles

# Define the drug name you want to convert
drug_name = "aspirin"  # Replace with the drug name of your choice
get_smiles(drug_name)

### Output:
# Drug Name: aspirin
# SMILES: CC(=O)Oc1ccccc1C(O)=O
```

[DeepPurpose](https://github.com/kexinhuang12345/DeepPurpose)可以用来编码分子化合物。它目前支持15种不同的编码。我们将使用Morgan编码，它将化学的原子组编码为一个二进制向量，以长度和半径作为两个参数。首先我们需要安装DeepPurpose库。

```py
pip install DeepPurpose
```

![](../Images/7dcdecb43803584e8c4498f653137c4d.png)

DeepPurpose编码器概述（图片来自[Huang et al.](https://doi.org/10.1093/bioinformatics/btaa1005)，CC许可）

我们创建一个将SMILES映射到Morgan表示的字典，以及一个将临床试验标识符（NCTIDs）直接映射到其Morgan表示的字典。

```py
def create_smiles2morgan_dict():
    from DeepPurpose.utils import smiles2morgan 

    # Import toy dataset
    toy_df = pd.read_csv('data/toy_df.csv')

    smiles_lst = list(map(txt_to_lst, toy_df['smiless'].tolist()))
    unique_smiles = set(reduce(lambda x, y: x + y, smiles_lst))

    morgan = pd.Series(list(unique_smiles)).apply(smiles2morgan)
    smiles2morgan_dict = dict(zip(unique_smiles, morgan))
    pickle.dump(smiles2morgan_dict, open('data/smiles2morgan_dict.pkl', 'wb'))

def create_nctid2molecule_embedding_dict():
    # Import toy dataset
    toy_df = pd.read_csv('data/toy_df.csv')
    smiles_lst = list(map(txt_to_lst, toy_df['smiless'].tolist()))
    smiles2morgan_dict = load_smiles2morgan_dict()

    embedding = []
    for drugs in tqdm(smiles_lst):
        vec = []
        for drug in drugs:
            vec.append(smiles2morgan_dict[drug])
        # print(np.array(vec).shape) # DEBUG
        vec = np.mean(np.array(vec), axis=0)
        # print(vec.shape) # DEBUG
        embedding.append(vec)
    print(np.array(embedding).shape)

    dict = zip(toy_df['nctid'], np.array(embedding))
    nctid2molecule_embedding_dict = {}
    for key, row in zip(toy_df['nctid'], np.array(embedding)):
        nctid2molecule_embedding_dict[key] = row
    pickle.dump(nctid2molecule_embedding_dict, open('data/nctid2molecule_embedding_dict.pkl', 'wb'))  

create_nctid2molecule_embedding_dict()
```

# 结论

通过使用特征嵌入，我们可以使用公开的临床试验信息为机器学习模型创建有用的输入。总结一下，我们：

+   使用[tiny-biobert](https://huggingface.co/nlpie/tiny-biobert)嵌入**疾病指征**，由[Rohanian et al](https://doi.org/10.1093/bioinformatics/btad103)提供，该版本是[BioBERT](https://doi.org/10.1093/bioinformatics/btz682)的精简版。

+   使用[tiny-biobert](https://huggingface.co/nlpie/tiny-biobert)嵌入临床试验**纳入/排除标准**。

+   使用[all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2)嵌入**赞助商信息**，这是来自[SentenceBERT](https://doi.org/10.48550/arXiv.1908.10084)的强大预训练句子编码器。

+   将**药物名称**转换为其SMILES表示，然后使用[DeepPurpose](https://github.com/kexinhuang12345/DeepPurpose)通过[Huang et al.](https://doi.org/10.1093/bioinformatics/btaa1005)转换为其Morgan指纹。

在本系列的[第二部分](https://medium.com/@lennart.langouche/clinical-trial-outcome-prediction-7ce6c27831f9)中，我将运行一个简单的XGBoost模型，以预测临床试验结果，基于我们在这里创建的嵌入向量表示。我将其性能与HINT模型进行比较。

# 参考文献

+   Fu, Tianfan, 等。“Hint: 用于临床试验结果预测的层次互动网络。” *Patterns* 3.4 (2022)。

+   Huang, Kexin, 等。“DeepPurpose: 用于药物-靶标相互作用预测的深度学习库。” *生物信息学* 36.22–23 (2020): 5545–5547。
