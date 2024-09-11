# 使用 LangChain 和 GPT-4 研究多语言 FEMA 灾难机器人

> 原文：[https://towardsdatascience.com/researching-a-multilingual-fema-disaster-bot-using-langchain-and-gpt-4-4591f26d8dcd?source=collection_archive---------5-----------------------#2023-08-17](https://towardsdatascience.com/researching-a-multilingual-fema-disaster-bot-using-langchain-and-gpt-4-4591f26d8dcd?source=collection_archive---------5-----------------------#2023-08-17)

## 高风险聊天应用程序中检索增强生成（RAG）的优缺点

[](https://medium.com/@astrobagel?source=post_page-----4591f26d8dcd--------------------------------)[![Matthew Harris](../Images/4fa3264bb8a028633cd8d37093c16214.png)](https://medium.com/@astrobagel?source=post_page-----4591f26d8dcd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4591f26d8dcd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4591f26d8dcd--------------------------------) [Matthew Harris](https://medium.com/@astrobagel?source=post_page-----4591f26d8dcd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4a2cd25b8ff9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fresearching-a-multilingual-fema-disaster-bot-using-langchain-and-gpt-4-4591f26d8dcd&user=Matthew+Harris&userId=4a2cd25b8ff9&source=post_page-4a2cd25b8ff9----4591f26d8dcd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4591f26d8dcd--------------------------------) ·54分钟阅读·2023年8月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4591f26d8dcd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fresearching-a-multilingual-fema-disaster-bot-using-langchain-and-gpt-4-4591f26d8dcd&user=Matthew+Harris&userId=4a2cd25b8ff9&source=-----4591f26d8dcd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4591f26d8dcd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fresearching-a-multilingual-fema-disaster-bot-using-langchain-and-gpt-4-4591f26d8dcd&source=-----4591f26d8dcd---------------------bookmark_footer-----------)![](../Images/37138d88da2ef33806c505f6c3bcb47e.png)

图片由 [DALL-E2](https://openai.com/dall-e-2) 使用提示“一个激流围绕一个机器人泛滥的照片”生成

*简要概述*

*在本文中，我们探讨了如何构建一个多语言的* [*美国联邦应急管理局 (FEMA)*](https://www.fema.gov/about) *灾难聊天机器人，以帮助人们准备和应对诸如洪水、龙卷风、野火、地震和冬季风暴等灾难。我们使用 LangChain 和 GPT-4 从 34 个 FEMA PDF 文档中创建了一个聊天界面。尽管这种流行模式非常令人惊叹，但在高风险应用如灾难响应机器人中仍需谨慎。虽然大语言模型 (LLM) 的幻觉现象已被最小化，但文档的语义搜索常见问题可能导致聊天响应中遗漏关键信息。我们测试了一些简单的技术来提高这种特定分析的性能，比如将文档元数据纳入嵌入、通过 LLM 零样本上下文分类丰富用户问题，以及使用谷歌翻译进行自动语言检测和翻译，以更好地支持像斯瓦希里语这样的语言。应用的技术非常基础，但原型机器人的信息提取能力从 FEMA PDF 文档中显示了希望。显然，如果将这种技术用于高风险情况，需进行测试和验证，理想情况下使用自动化的 LLM 技术创建问答验证数据。*

几周前，我们在佛蒙特州经历了一次严重的洪水事件。流经我们房子的那条小溪变成了一个愤怒的怪物，狂暴地破坏一切。幸运的是，我们没有受到严重损害，但遗憾的是，许多人失去了财产和生计。在洪水的某个时刻，看起来我们可能需要撤离，于是我开始查看 [美国联邦应急管理局 (FEMA)](https://www.fema.gov/about) 网站上的建议。我已经有了一些准备，但当麻烦来临时，我想重新确认一些事情。FEMA 的 PDF 文档和网页资源非常优秀且简明，我开始搜索，但我在想……

+   ***在紧急情况下，有没有比搜索和阅读多个文档更快的方法来获取有用信息？***

一个明显的解决方案可能是询问聊天机器人。我认为聊天机器人有时可能会被过度使用，但在时间紧迫的情况下，这确实是一个很好的应用场景，因为对话界面可能更为高效。

这并不是一个新想法，像美国红十字会这样的组织已经开发了类似于[Clara](https://www.redcross.org/get-help/disaster-relief-and-recovery-services/meet-clara.html)的灾害响应机器人。然而，最近出现了一种有前景的新模式，利用生成式人工智能大型语言模型（LLMs），如[OpenAI的GPT-4](https://openai.com/research/gpt-4)、[Meta的LLAMA 2](https://ai.meta.com/llama/)以及在[HuggingFace](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard)上不断增长的众多模型。这些模型可以用来索引指定的文档集，并与其进行对话。[这种方法称为检索增强生成（RAG）](https://huggingface.co/blog/ray-rag)，网上有数百个教程展示了使用令人惊叹的[LangChain](https://python.langchain.com/docs/use_cases/question_answering/how_to/vector_db_qa) Python 包，我完全预期这种技术很快会出现在我们每天使用的软件中。有趣的是，它将响应限制在提供的内容范围内，从而减少了幻觉的发生，这有助于在灾害响应等关键情况下使用LLMs。

但在信息检索可能拯救生命的情况下，这种方法的安全性如何呢？

在本文中，我简要探讨了创建一个LangChain GPT-4聊天界面，用于基于[美国联邦应急管理署（FEMA）](https://www.fema.gov/about)的一组文档询问有关灾害安全的问题。我们将遇到一些需要考虑的限制，如果在高风险情况下使用这种技术的话。

# FEMA灾害准备和安全PDF文档

对于这项研究，我从FEMA下载了34个PDF文件（[列表在这里](https://github.com/datakind/stay_safe_bot/blob/main/docs_data/fema_docs.csv)），这些文件涵盖了广泛的灾害相关话题，包括如何准备和应对野火、龙卷风、洪水、地震和冬季风暴。这不是FEMA提供的所有惊人资源，但应该足够测试我们的聊天界面。

# 索引文档以进行信息检索

下载后，我们可以使用LangChain读取PDF文档。文档被分割成文本块，并使用嵌入模型给每个块分配一个指纹（嵌入）。在这项分析中，我们将使用[OpenAI的嵌入](https://platform.openai.com/docs/guides/embeddings)，但LangChain支持[许多其他模型](https://python.langchain.com/docs/integrations/text_embedding/)。

```py
import os
from langchain.document_loaders import PyPDFDirectoryLoader
from langchain.document_loaders import PyPDFLoader

files = os.listdir(pdf_folder_path)
files.sort()
all_docs_list =[]
for file in files:
    if file.endswith('.pdf'):
        print(file)
        all_docs_list.append(file)
loader = PyPDFDirectoryLoader(pdf_folder_path)
all_docs = loader.load()

print(pdf_folder_path)
```

```py
cfpb_adult-fin-edyour-disaster-checklist.pdf
fema_protect-your-home_flooding.pdf
fema_protect-your-property-storm-surge.pdf
fema_protect-your-property_coastal-erosion.pdf
fema_protect-your-property_earthquakes.pdf
fema_protect-your-property_severe-wind.pdf
fema_protect-your-property_wildfire.pdf
fema_safeguard-critical-documents-and-valuables.pdf
fema_scenario_1-active_shooter-01102020.pdf
fema_scenario_10_power_outage_01102020.pdf
fema_scenario_10_power_outage_answer_key_01102020.pdf
fema_scenario_11_winter_storm_01102020.pdf
fema_scenario_11_winter_storm_answer_key_01102020.pdf
fema_scenario_12_small_business_01102020.pdf
fema_scenario_12_small_business_answer_key_01102020.pdf
fema_scenario_1_active_shooter_TTX_answer_key-01102020.pdf
fema_scenario_2-tornado_TTX_answer_key-01102020.pdf
fema_scenario_2_tornado-01102020.pdf
fema_scenario_3-wildfire_TTX_answer_key-01102020.pdf
fema_scenario_3_wildfire-01102020.pdf
fema_scenario_4-hurricane-01102020.pdf
fema_scenario_4_hurricane_flood_TTX_answer_key-01102020.pdf
fema_scenario_5_extreme_heat-01102020.pdf
fema_scenario_5_extreme_heat_TTX_answer_key_01102020.pdf
fema_scenario_6-pet_preparedness_01102020.pdf
fema_scenario_6-pet_preparedness_TTX_answer_key_01102020.pdf
fema_scenario_7-shelter_in_place_TTX_answer_key_01102020.pdf
fema_scenario_7_shelter_in_place_01102020.pdf
fema_scenario_8_earthquake_01102020.pdf
fema_scenario_8_earthquake_answer_key_01102020.pdf
fema_scenario_9_pandemic_Influenza_01102020.pdf
fema_scenario_9_pandemic_answer_key_01102020.pdf
ready_12-ways-to-prepare_postcard.pdf
ready_document-and-insure-your-property.pdf
```

查看为一个文档提取的数据[https://www.fema.gov/sites/default/files/2020-11/fema_protect-your-home_flooding.pdf](https://www.fema.gov/sites/default/files/2020-11/fema_protect-your-home_flooding.pdf) …

```py
import json

for d in all_docs:
    if 'fema_protect-your-home_flooding.pdf' in d.metadata['source']:
        print('\n')
        print(json.dumps(vars(d), indent=4))
```

```py
{
    "page_content": "  \n  \n \nPROTECT YOUR \nPROPERTY FROM FLOODING\n \n",
    "metadata": {
        "source": "docs_data/fema_protect-your-home_flooding.pdf",
        "page": 0
    }
}

{
    "page_content": " \n \n \n  \n  \n \n \n \n \n \n \n \n \n \n \n -\n\u2014\n- - -\nOwning a property is one of the most important investments most people make \nin their lives. We work hard to provide a home and a future for ourselves and our loved ones. Why risk losing it when bad weather hits close to home? \nFlooding is the most common and costly natural disaster in the United States \nand can happen anywhere. Just one inch of water can cause $25,000 in damages to your home. \nWhile you can\u2019t prevent a natural disaster from happening, there are ways to secure \nyour property to minimize damage and keep your home and your future safe. \nFirst, determine the Base Flood Elevation (BFE) for your home. The \nBFE is how high the water is expected to rise during fooding in high risk areas. You need to know your BFE because it is used in foodplain management regulations in your community that could affect your home for example, how high above the BFE a home or other building should be built. Your local foodplain manager can help you fnd this information. If you need help fnding your foodplain manager, contact FEMA\u2019s Flood Mapping and Insurance eXchange\n a\nt FEMAMapSpecialist@ \nr\niskmapcds.com or (877) FEMA MAP (1 877 336 2627). \nThe following are some additional steps you can take to protect yourself and your property against foods. ",
    "metadata": {
        "source": "docs_data/fema_protect-your-home_flooding.pdf",
        "page": 1
    }
}

{
    "page_content": " \n \n \n \n \n   \n \n   \n \n  \n \n \n \n  \n \n \n  \n  \n  \n   \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n  \n \nINSIDE THE HOME \nPREPARE OR \nUPDA\nTE A LIST OF \nBELONGINGS Documenting all of your belongings will help with the \ninsurance claims process. Consider taking photos of \nhigh-value items or doing a video walkthrough of your \nho\nme to document its contents. \nGET FLOOD \nINSURANCE Most homeowners insurance \npolicies don\u2019t cover food damage. \nProtect your investment by purchasing food insurance for your home and contents, even if you do not live in \na hi\ngh-risk food zone. \nSTORE \nVALUABLES Store valuables and important documents in waterproof or wa\nter-resistant containers above the BFE \n(preferably on an upper foor). Make copies and store them online or offsite. For more \ninformation on purchasing food insurance through the National Flood \nInsurance Program, visit \nFlo\nodSmart.gov  or contact \nyour agent to get coverage. EL\nEVATE UTILITIES \nABOVE THE BFE Elevate or foodproof mechanical units, furnaces, water \nheaters, electrical systems, and other utilities on masonry, concrete, or pressure-treated lumber at least 12 inches above the BFE. \nREPLACE CARPETING \nWITH TILES Tiles are more food-resistant than carpet. Using tile or other food-resistant materials in areas below the BFE can help reduce water damage. \nFLOODPROOF \nBASEMENTS If you have a basement, minimize damage by foodproofng your basement and sealing walls with waterproofng compounds. Consider installing a sump pump. \nINSTALL FLOOD \nVENTS Install food vents in foundation walls, garages, and other enclosed areas to allow water \nto fow through, drain out, an\nd lower the risk of \nstructural damage. \nUSE FLOOD-RESIST\nANT \nINSULATION & DRYWALL Fl\nood-resistant insulation and drywall will help minimize \ndamage and can be easily cleaned and sanitized. \nPREVENT SEW\nAGE \nBACK-UP I\nn some areas, fooding can cause sewage to back \nup through drain pipes in your home. Consult with a plumber and, if applicable, invest in a sewer backfow valve to prevent this potential health hazard. DID YOU KNOW? \nWhen following National Flood \nInsurance Program regulations, \nv\nents can also help lower \ninsu\nrance rates. \n",
    "metadata": {
        "source": "docs_data/fema_protect-your-home_flooding.pdf",
        "page": 2
    }
}

{
    "page_content": " \n \n \n \n \n \n \n \n \n \n  \n \n \n \n   \n \n \n \n  \n \nOUTSIDE THE HOME \nELEVATE YOUR \nHOME While it is an investment, elevating your home prepares your property against foods and lowers food insurance premiums. When a home is properly elevated, the lowest foor should be above the BFE. Areas below the BFE can be used for parking, storage, or access to the house. DID YOU KNOW? \nElevating your home \nmay reduce your food insurance premium. \nSECURE Y\nARD \nITEMS U\nnsecure items can be swept away or damaged by foodwaters. \nThey can also be swept into your home, causing damage. \nSecure items in your yard by anchoring them or attaching them to more substantial structures. \nSEAL CRA\nCKS \nAND GAPS C\nheck caulking around windows and doors to make it is not \ncracked, broken, or missing. Fill any holes or gaps around pipes and wires that enter your building. \nSET YOUR HOME OR \nBUILDINGS BACK, \nAWAY FROM WATER Build your home, garage, shed, or other building away from river channels and shore lines. If possible, build on higher ground. ",
    "metadata": {
        "source": "docs_data/fema_protect-your-home_flooding.pdf",
        "page": 3
    }
}

{
    "page_content": " \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n  \n  \n \n \n \n \n \n \n \n \n  \n \n \n  \nDIRECT WATER AWAY \nFROM STRUCTURES If you have a single-family home, make sure your yard \nslopes away from buildings on your property and that water has a place to drain. Clear your gutters, assess drainage issues, or collect water in rain barrels. \nANCHOR FUEL \nTANKS Anchor any fuel tanks to the pad to prevent them from tipping over or foating in a food. Spilled fuel could become a fre hazard. Make sure vents and fll-line openings are above the BFE. Note: This may require permission from your fuel provider. \nFLOODPROOF WALLS Add water-resistant exterior sheathing on walls and seal them to prevent shallow fooding from damaging your home. Cover openings below the BFE and seal all exterior openings around pumping and equipment. \nSECURE \nMANUFACTURED \nHOMES If you have a manufactured home and you want food insurance from the National Flood Insurance Program, your home must be affxed to a permanent foundation so that the wheels and axles do not support its weight and resist fotation, collapse, or side-to-side movement. Your local foodplain manager can help you understand the requirements, and a professional engineer or architect can make sure the anchoring system is designed and installed correctly. REMEMBER: \nSome of these tips may work better together than \nothers. Mitigation measures need to be tailored to your property. Also, not all of these options work together, so talk with an expert who can help you identify which options work best for you. \nAlways consult professionals such as your insurance \nagent, architects, engineers, contractors, or other experts in design and construction before making changes to your home. Your local planning and zoning offce or building department is a good place to start for advice. \nFinally, be kind to your neighbors! Talk to adjacent \nproperty owners before you make changes, since some actions on your property may affect theirs. ",
    "metadata": {
        "source": "docs_data/fema_protect-your-home_flooding.pdf",
        "page": 4
    }
}

{
    "page_content": " \n  \n \n \n \n \n \n  \n \n \n  \n \n ADDITIONAL RESOURCES \nFEMA, PROTECT YOUR PROPERTY \nLearn how to protect your home or business from natural disasters. fema.gov/protect-your-property \nHOMEOWNER\u2019S GUIDE TO RETROFITTING \nfema.gov/media-library/assets/documents/480 \nREDUCING FLOOD RISK TO RESIDENTIAL BUILDINGS \nTHAT CANNOT BE ELEVATED \nfema.gov/media-library/assets/documents/109669 \nPROTECTING YOUR HOME AND PROPERTY FROM \nFLOOD DAMAGE fema.gov/media-library/assets/documents/21471 \nPROTECTING BUILDING UTILITY SYSTEMS FROM \nFLOOD DAMAGE \nfema.gov/media-library/assets/documents/3729 \nPROTECT YOUR PROPERTY FROM FLOODING \nfema.gov/media-library/assets/documents/13261 ",
    "metadata": {
        "source": "docs_data/fema_protect-your-home_flooding.pdf",
        "page": 5
    }
}

{
    "page_content": "",
    "metadata": {
        "source": "docs_data/fema_protect-your-home_flooding.pdf",
        "page": 6
    }
}
```

我们可以看到它按页面拆分了文档。实际上，这对于我们正在处理的 FEMA 文档并非不合理，这些文档非常简洁，每页都是一个独立的主题，但对于其他应用，通常更好的是在更细的级别拆分文本，使用[LangChain 的 text_splitter](https://python.langchain.com/docs/use_cases/question_answering/how_to/vector_db_qa)。

我们现在可以使用我们的文本摘录来创建嵌入数据库...

```py
from langchain.embeddings import OpenAIEmbeddings 
from langchain.vectorstores import Chroma 

embedding_model = OpenAIEmbeddings()
embeddings = OpenAIEmbeddings()
vectordb = Chroma.from_documents(all_docs, embedding=embedding_model,persist_directory=vecs_dir)
vectordb.persist()
```

我们选择了一个简单的选项将嵌入持久化到文件系统，但值得注意的是，[Chroma 支持更多选项](https://python.langchain.com/docs/integrations/vectorstores/chroma)，如果你有大量文档，性能可能是一个问题。

# 设置我们的对话界面

我们将首先使用一个 PDF，这样更容易验证结果...

```py
import shutil
from langchain.chains import ConversationalRetrievalChain
from langchain.memory import ConversationBufferMemory
from langchain.llms import OpenAI
from langchain.chat_models import ChatOpenAI
from langchain import PromptTemplate, LLMChain

# Load OPENAI_API_KEY from a .env file
from dotenv import load_dotenv

def setup_model(
    vecs_dir: str,
    docs_sublist: list,
    all_docs: list
) -> ConversationalRetrievalChain:

    # Subset for docs we are interested in
    docs = []
    for d in all_docs:        
        d_dict = vars(d)

    # Create vector DB directory
    if os.path.exists(vecs_dir):
        shutil.rmtree(vecs_dir)
    os.makedirs(vecs_dir)

    # Choose our models
    embedding_model = OpenAIEmbeddings()
    chat_model = ChatOpenAI(temperature=temperature,model_name="gpt-4")

    # Calculate embeddings
    embeddings = OpenAIEmbeddings()
    vectordb = Chroma.from_documents(docs, embedding=embedding_model,persist_directory=vecs_dir)
    vectordb.persist()

    # Set up chat
    memory = ConversationBufferMemory(memory_key="chat_history", input_key='question', output_key='answer', return_messages=True)
    pdf_qa = ConversationalRetrievalChain.from_llm(chat_model, vectordb.as_retriever(), memory=memory, \
                                                   return_source_documents=True)
    return pdf_qa

vecs_dir = './vector_dbs/one_flood_doc'
docs = all_docs

# Subset to one document
docs_sublist = ['fema_protect-your-home_flooding.pdf']

pdf_qa = setup_model(vecs_dir, docs_sublist, docs)
```

在上述内容中，我们选择了 GPT-4 作为聊天模型，其中 API 密钥在 `.env` 文件中定义，变量为 `OPENAI_API_KEY`。LangChain 还支持[许多其他](https://python.langchain.com/docs/integrations/chat/)模型。

只需几行代码，我们就建立了一个对文档的对话界面，这包括了 LLM 的所有功能。我多年来开发了聊天机器人，可以说这种简洁的模式大大减少了复杂性。

对出色的 LangChain 包致以赞誉！

# 提出我们的第一个问题

由于我们还希望查看引用的文档以验证聊天响应，我们需要对 LangChain 方法进行一些调整，以解决在使用聊天记忆时检索匹配文档时出现的问题（解决方案已在[这里](https://github.com/langchain-ai/langchain/issues/2256#issuecomment-1665188576)建议）...

```py
# A little mod to enable using memory *and* getting docs. See: https://github.com/langchain-ai/langchain/issues/2256#issuecomment-1665188576
import langchain
from typing import Dict, Any, Tuple
from langchain.memory.utils import get_prompt_input_key
def _get_input_output(
    self, inputs: Dict[str, Any], outputs: Dict[str, str]
) -> Tuple[str, str]:
    if self.input_key is None:
        prompt_input_key = get_prompt_input_key(inputs, self.memory_variables)
    else:
        prompt_input_key = self.input_key
    if self.output_key is None:
        output_key = list(outputs.keys())[0]
    else:
        output_key = self.output_key
    return inputs[prompt_input_key], outputs[output_key]

langchain.memory.chat_memory.BaseChatMemory._get_input_output = _get_input_output 
```

好的，现在我们准备好对我们的一个 PDF 文档提问了！

```py
def ask_question(
    query: str,
    qa: object,
    output_docs: bool = True
) -> dict:

    print(f"\nQuestion: \n{query}")
    result = qa({"question": query})

    print(f"\nAnswer:\n{result['answer']}")
    if output_docs:
        for doc in result['source_documents']:
            print('\n')
            print(json.dumps(vars(doc), indent=4))
    return result

ask_question("How do I prepare my home for floods?", pdf_qa)
```

```py
 Question: 
How do I prepare my home for floods?

Answer:
There are several steps you can take to prepare your home for floods:

1\. Determine the Base Flood Elevation (BFE) for your home. This is how high the water is expected to rise during flooding in high risk areas. Your local floodplain manager can help you find this information.

2\. Direct water away from structures. Make sure your yard slopes away from buildings on your property and that water has a place to drain. Clear your gutters, assess drainage issues, or collect water in rain barrels.

3\. Anchor fuel tanks to prevent them from tipping over or floating in a flood. 

4\. Floodproof walls by adding water-resistant exterior sheathing and sealing them to prevent shallow flooding from damaging your home.

5\. Secure manufactured homes to a permanent foundation to resist flotation, collapse, or side-to-side movement.

6\. Inside the home, prepare or update a list of belongings for insurance claims. 

7\. Get flood insurance as most homeowners insurance policies don’t cover flood damage.

8\. Store valuables and important documents in waterproof or water-resistant containers above the BFE.

9\. Elevate utilities above the BFE. 

10\. Replace carpeting with tiles as they are more flood-resistant.

11\. Floodproof basements by sealing walls with waterproofing compounds and consider installing a sump pump.

12\. Install flood vents in foundation walls, garages, and other enclosed areas to allow water to flow through and drain out.

13\. Use flood-resistant insulation and drywall.

14\. Prevent sewage back-up by consulting with a plumber and investing in a sewer backflow valve if applicable.

Remember to consult professionals such as your insurance agent, architects, engineers, contractors, or other experts in design and construction before making changes to your home.
```

这似乎很合理，让我们看看用于生成此摘要的内容...

```py
{
    "page_content": "  \n  \n \nPROTECT YOUR \nPROPERTY FROM FLOODING\n \n",
    "metadata": {
        "page": 0,
        "source": "docs_data/fema_protect-your-home_flooding.pdf"
    }
}

{
    "page_content": " \n \n \n  \n  \n \n \n \n \n \n \n \n \n \n \n -\n\u2014\n- - -\nOwning a property is one of the most important investments most people make \nin their lives. We work hard to provide a home and a future for ourselves and our loved ones. Why risk losing it when bad weather hits close to home? \nFlooding is the most common and costly natural disaster in the United States \nand can happen anywhere. Just one inch of water can cause $25,000 in damages to your home. \nWhile you can\u2019t prevent a natural disaster from happening, there are ways to secure \nyour property to minimize damage and keep your home and your future safe. \nFirst, determine the Base Flood Elevation (BFE) for your home. The \nBFE is how high the water is expected to rise during fooding in high risk areas. You need to know your BFE because it is used in foodplain management regulations in your community that could affect your home for example, how high above the BFE a home or other building should be built. Your local foodplain manager can help you fnd this information. If you need help fnding your foodplain manager, contact FEMA\u2019s Flood Mapping and Insurance eXchange\n a\nt FEMAMapSpecialist@ \nr\niskmapcds.com or (877) FEMA MAP (1 877 336 2627). \nThe following are some additional steps you can take to protect yourself and your property against foods. ",
    "metadata": {
        "page": 1,
        "source": "docs_data/fema_protect-your-home_flooding.pdf"
    }
}

{
    "page_content": " \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n  \n  \n \n \n \n \n \n \n \n \n  \n \n \n  \nDIRECT WATER AWAY \nFROM STRUCTURES If you have a single-family home, make sure your yard \nslopes away from buildings on your property and that water has a place to drain. Clear your gutters, assess drainage issues, or collect water in rain barrels. \nANCHOR FUEL \nTANKS Anchor any fuel tanks to the pad to prevent them from tipping over or foating in a food. Spilled fuel could become a fre hazard. Make sure vents and fll-line openings are above the BFE. Note: This may require permission from your fuel provider. \nFLOODPROOF WALLS Add water-resistant exterior sheathing on walls and seal them to prevent shallow fooding from damaging your home. Cover openings below the BFE and seal all exterior openings around pumping and equipment. \nSECURE \nMANUFACTURED \nHOMES If you have a manufactured home and you want food insurance from the National Flood Insurance Program, your home must be affxed to a permanent foundation so that the wheels and axles do not support its weight and resist fotation, collapse, or side-to-side movement. Your local foodplain manager can help you understand the requirements, and a professional engineer or architect can make sure the anchoring system is designed and installed correctly. REMEMBER: \nSome of these tips may work better together than \nothers. Mitigation measures need to be tailored to your property. Also, not all of these options work together, so talk with an expert who can help you identify which options work best for you. \nAlways consult professionals such as your insurance \nagent, architects, engineers, contractors, or other experts in design and construction before making changes to your home. Your local planning and zoning offce or building department is a good place to start for advice. \nFinally, be kind to your neighbors! Talk to adjacent \nproperty owners before you make changes, since some actions on your property may affect theirs. ",
    "metadata": {
        "page": 4,
        "source": "docs_data/fema_protect-your-home_flooding.pdf"
    }
}

{
    "page_content": " \n \n \n \n \n   \n \n   \n \n  \n \n \n \n  \n \n \n  \n  \n  \n   \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n  \n \nINSIDE THE HOME \nPREPARE OR \nUPDA\nTE A LIST OF \nBELONGINGS Documenting all of your belongings will help with the \ninsurance claims process. Consider taking photos of \nhigh-value items or doing a video walkthrough of your \nho\nme to document its contents. \nGET FLOOD \nINSURANCE Most homeowners insurance \npolicies don\u2019t cover food damage. \nProtect your investment by purchasing food insurance for your home and contents, even if you do not live in \na hi\ngh-risk food zone. \nSTORE \nVALUABLES Store valuables and important documents in waterproof or wa\nter-resistant containers above the BFE \n(preferably on an upper foor). Make copies and store them online or offsite. For more \ninformation on purchasing food insurance through the National Flood \nInsurance Program, visit \nFlo\nodSmart.gov  or contact \nyour agent to get coverage. EL\nEVATE UTILITIES \nABOVE THE BFE Elevate or foodproof mechanical units, furnaces, water \nheaters, electrical systems, and other utilities on masonry, concrete, or pressure-treated lumber at least 12 inches above the BFE. \nREPLACE CARPETING \nWITH TILES Tiles are more food-resistant than carpet. Using tile or other food-resistant materials in areas below the BFE can help reduce water damage. \nFLOODPROOF \nBASEMENTS If you have a basement, minimize damage by foodproofng your basement and sealing walls with waterproofng compounds. Consider installing a sump pump. \nINSTALL FLOOD \nVENTS Install food vents in foundation walls, garages, and other enclosed areas to allow water \nto fow through, drain out, an\nd lower the risk of \nstructural damage. \nUSE FLOOD-RESIST\nANT \nINSULATION & DRYWALL Fl\nood-resistant insulation and drywall will help minimize \ndamage and can be easily cleaned and sanitized. \nPREVENT SEW\nAGE \nBACK-UP I\nn some areas, fooding can cause sewage to back \nup through drain pipes in your home. Consult with a plumber and, if applicable, invest in a sewer backfow valve to prevent this potential health hazard. DID YOU KNOW? \nWhen following National Flood \nInsurance Program regulations, \nv\nents can also help lower \ninsu\nrance rates. \n",
    "metadata": {
        "page": 2,
        "source": "docs_data/fema_protect-your-home_flooding.pdf"
    }
}
```

答案*看起来*很棒，并且出色地总结了匹配的摘录。然而，它似乎遗漏了 [源文档](https://www.fema.gov/sites/default/files/2020-11/fema_protect-your-home_flooding.pdf)中的一页。这个 PDF 是一个简短的文档，其中**所有内容**都与洪水准备有关，因此遗漏页面实际上是很重要的。

这表明，盲目接受网络上的 LLM 模式可能会得到看起来很棒的结果，但需要进行工作才能使其真正有用。

# 文档上下文（元数据）可能很重要

在这种情况下，我们有一个与一个主题相关的文档，即洪水，即使个别文本部分可能没有明确提到它。如果我们在每个文本摘录中提供一些上下文，即一些文档元数据，可能会获得更好的结果。

对于我们的简单测试，试着将所有文本摘录前缀加上文件名（fema-protect-your-home-flooding.pdf），去掉标点和后缀，再加上“此摘录相关于”这段文字。最终前缀将是“此摘录相关于 fema protect your home flooding:”，这为LLM提供了更多的背景信息……

```py
def setup_model(
    vecs_dir: str,
    docs_sublist: list,
    all_docs: list,
    prefix_file_name_to_chunks: bool = False,
    temperature: float = 0.0,
    extra_prefix: str = '',
) -> ConversationalRetrievalChain:
    # Subset for docs we are interested in
    docs = []
    for d in all_docs:        
        d_dict = vars(d)
        if d_dict['metadata']['source'].replace('docs_data/','') in docs_sublist:
            if len(d.page_content) > 20:
                # Add file name to content for more context
                if prefix_file_name_to_chunks:
                    file_clean = re.sub(r'docs_data\/|\.pdf', '', d_dict['metadata']['source'])  
                    file_clean = re.sub(r'\-|\_', ' ', file_clean)                
                    d.page_content = f"{extra_prefix} {file_clean}: {d.page_content}"
                docs.append(d)

    # Create vector DB directory
    if os.path.exists(vecs_dir):
        shutil.rmtree(vecs_dir)
    os.makedirs(vecs_dir)

    # Choose our models
    embedding_model = OpenAIEmbeddings()
    chat_model = ChatOpenAI(temperature=temperature,model_name="gpt-4")

    # Calculate embeddings
    embeddings = OpenAIEmbeddings()
    vectordb = Chroma.from_documents(docs, embedding=embedding_model,persist_directory=vecs_dir)
    vectordb.persist()

    # Set up chat
    memory = ConversationBufferMemory(memory_key="chat_history", input_key='question', output_key='answer', return_messages=True)
    pdf_qa = ConversationalRetrievalChain.from_llm(chat_model, vectordb.as_retriever(), memory=memory, \
                                                   return_source_documents=True)
    return pdf_qa

docs = all_docs

# Note the argument prefix_file_name_to_chunks=True
pdf_qa = setup_model(vecs_dir, docs_sublist, docs, prefix_file_name_to_chunks=True, extra_prefix="This snippet relates to ")
ask_question("How do I prepare my home for floods?", pdf_qa)
```

```py
Question: 
How do I prepare my home for floods?

Answer:
There are several steps you can take to prepare your home for floods:

1\. Direct Water Away from Structures: Make sure your yard slopes away from buildings on your property and that water has a place to drain. Clear your gutters, assess drainage issues, or collect water in rain barrels.

2\. Anchor Fuel Tanks: Anchor any fuel tanks to the pad to prevent them from tipping over or floating in a flood. Spilled fuel could become a fire hazard.

3\. Floodproof Walls: Add water-resistant exterior sheathing on walls and seal them to prevent shallow flooding from damaging your home. Cover openings below the Base Flood Elevation (BFE) and seal all exterior openings around pumping and equipment.

4\. Secure Manufactured Homes: If you have a manufactured home, it must be affixed to a permanent foundation so that the wheels and axles do not support its weight and resist flotation, collapse, or side-to-side movement.

5\. Elevate Your Home: Elevating your home prepares your property against floods and lowers flood insurance premiums. The lowest floor should be above the BFE.

6\. Secure Yard Items: Unsecure items can be swept away or damaged by floodwaters. Secure items in your yard by anchoring them or attaching them to more substantial structures.

7\. Seal Cracks and Gaps: Check caulking around windows and doors to make sure it is not cracked, broken, or missing. Fill any holes or gaps around pipes and wires that enter your building.

8\. Prepare or Update a List of Belongings: Documenting all of your belongings will help with the insurance claims process.

9\. Get Flood Insurance: Most homeowners insurance policies don’t cover flood damage. Protect your investment by purchasing flood insurance for your home and contents.

10\. Store Valuables: Store valuables and important documents in waterproof or water-resistant containers above the BFE.

11\. Elevate Utilities Above the BFE: Elevate or floodproof mechanical units, furnaces, water heaters, electrical systems, and other utilities.

12\. Replace Carpeting with Tiles: Tiles are more flood-resistant than carpet. Using tile or other flood-resistant materials in areas below the BFE can help reduce water damage.

13\. Floodproof Basements: Minimize damage by floodproofing your basement and sealing walls with waterproofing compounds. Consider installing a sump pump.

14\. Install Flood Vents: Install flood vents in foundation walls, garages, and other enclosed areas to allow water to flow through, drain out, and lower the risk of structural damage.

15\. Use Flood-Resistant Insulation & Drywall: Flood-resistant insulation and drywall will help minimize damage and can be easily cleaned and sanitized.

16\. Prevent Sewage Back-Up: In some areas, flooding can cause sewage to back up through drain pipes in your home. Consult with a plumber and, if applicable, invest in a sewer backflow valve to prevent this potential health hazard.

Remember to consult professionals such as your insurance agent, architects, engineers, contractors, or other experts in design and construction before making changes to your home.

{
    "page_content": "This snippet relates to  fema protect your home flooding:  \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n  \n  \n \n \n \n \n \n \n \n \n  \n \n \n  \nDIRECT WATER AWAY \nFROM STRUCTURES If you have a single-family home, make sure your yard \nslopes away from buildings on your property and that water has a place to drain. Clear your gutters, assess drainage issues, or collect water in rain barrels. \nANCHOR FUEL \nTANKS Anchor any fuel tanks to the pad to prevent them from tipping over or foating in a food. Spilled fuel could become a fre hazard. Make sure vents and fll-line openings are above the BFE. Note: This may require permission from your fuel provider. \nFLOODPROOF WALLS Add water-resistant exterior sheathing on walls and seal them to prevent shallow fooding from damaging your home. Cover openings below the BFE and seal all exterior openings around pumping and equipment. \nSECURE \nMANUFACTURED \nHOMES If you have a manufactured home and you want food insurance from the National Flood Insurance Program, your home must be affxed to a permanent foundation so that the wheels and axles do not support its weight and resist fotation, collapse, or side-to-side movement. Your local foodplain manager can help you understand the requirements, and a professional engineer or architect can make sure the anchoring system is designed and installed correctly. REMEMBER: \nSome of these tips may work better together than \nothers. Mitigation measures need to be tailored to your property. Also, not all of these options work together, so talk with an expert who can help you identify which options work best for you. \nAlways consult professionals such as your insurance \nagent, architects, engineers, contractors, or other experts in design and construction before making changes to your home. Your local planning and zoning offce or building department is a good place to start for advice. \nFinally, be kind to your neighbors! Talk to adjacent \nproperty owners before you make changes, since some actions on your property may affect theirs. ",
    "metadata": {
        "page": 4,
        "source": "docs_data/fema_protect-your-home_flooding.pdf"
    }
}

{
    "page_content": "This snippet relates to  fema protect your home flooding:  \n \n \n \n \n \n \n \n \n \n  \n \n \n \n   \n \n \n \n  \n \nOUTSIDE THE HOME \nELEVATE YOUR \nHOME While it is an investment, elevating your home prepares your property against foods and lowers food insurance premiums. When a home is properly elevated, the lowest foor should be above the BFE. Areas below the BFE can be used for parking, storage, or access to the house. DID YOU KNOW? \nElevating your home \nmay reduce your food insurance premium. \nSECURE Y\nARD \nITEMS U\nnsecure items can be swept away or damaged by foodwaters. \nThey can also be swept into your home, causing damage. \nSecure items in your yard by anchoring them or attaching them to more substantial structures. \nSEAL CRA\nCKS \nAND GAPS C\nheck caulking around windows and doors to make it is not \ncracked, broken, or missing. Fill any holes or gaps around pipes and wires that enter your building. \nSET YOUR HOME OR \nBUILDINGS BACK, \nAWAY FROM WATER Build your home, garage, shed, or other building away from river channels and shore lines. If possible, build on higher ground. ",
    "metadata": {
        "page": 3,
        "source": "docs_data/fema_protect-your-home_flooding.pdf"
    }
}

{
    "page_content": "This snippet relates to  fema protect your home flooding:  \n \n \n \n \n   \n \n   \n \n  \n \n \n \n  \n \n \n  \n  \n  \n   \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n  \n \nINSIDE THE HOME \nPREPARE OR \nUPDA\nTE A LIST OF \nBELONGINGS Documenting all of your belongings will help with the \ninsurance claims process. Consider taking photos of \nhigh-value items or doing a video walkthrough of your \nho\nme to document its contents. \nGET FLOOD \nINSURANCE Most homeowners insurance \npolicies don\u2019t cover food damage. \nProtect your investment by purchasing food insurance for your home and contents, even if you do not live in \na hi\ngh-risk food zone. \nSTORE \nVALUABLES Store valuables and important documents in waterproof or wa\nter-resistant containers above the BFE \n(preferably on an upper foor). Make copies and store them online or offsite. For more \ninformation on purchasing food insurance through the National Flood \nInsurance Program, visit \nFlo\nodSmart.gov  or contact \nyour agent to get coverage. EL\nEVATE UTILITIES \nABOVE THE BFE Elevate or foodproof mechanical units, furnaces, water \nheaters, electrical systems, and other utilities on masonry, concrete, or pressure-treated lumber at least 12 inches above the BFE. \nREPLACE CARPETING \nWITH TILES Tiles are more food-resistant than carpet. Using tile or other food-resistant materials in areas below the BFE can help reduce water damage. \nFLOODPROOF \nBASEMENTS If you have a basement, minimize damage by foodproofng your basement and sealing walls with waterproofng compounds. Consider installing a sump pump. \nINSTALL FLOOD \nVENTS Install food vents in foundation walls, garages, and other enclosed areas to allow water \nto fow through, drain out, an\nd lower the risk of \nstructural damage. \nUSE FLOOD-RESIST\nANT \nINSULATION & DRYWALL Fl\nood-resistant insulation and drywall will help minimize \ndamage and can be easily cleaned and sanitized. \nPREVENT SEW\nAGE \nBACK-UP I\nn some areas, fooding can cause sewage to back \nup through drain pipes in your home. Consult with a plumber and, if applicable, invest in a sewer backfow valve to prevent this potential health hazard. DID YOU KNOW? \nWhen following National Flood \nInsurance Program regulations, \nv\nents can also help lower \ninsu\nrance rates. \n",
    "metadata": {
        "page": 2,
        "source": "docs_data/fema_protect-your-home_flooding.pdf"
    }
}

{
    "page_content": "This snippet relates to  fema protect your home flooding:  \n \n \n  \n  \n \n \n \n \n \n \n \n \n \n \n -\n\u2014\n- - -\nOwning a property is one of the most important investments most people make \nin their lives. We work hard to provide a home and a future for ourselves and our loved ones. Why risk losing it when bad weather hits close to home? \nFlooding is the most common and costly natural disaster in the United States \nand can happen anywhere. Just one inch of water can cause $25,000 in damages to your home. \nWhile you can\u2019t prevent a natural disaster from happening, there are ways to secure \nyour property to minimize damage and keep your home and your future safe. \nFirst, determine the Base Flood Elevation (BFE) for your home. The \nBFE is how high the water is expected to rise during fooding in high risk areas. You need to know your BFE because it is used in foodplain management regulations in your community that could affect your home for example, how high above the BFE a home or other building should be built. Your local foodplain manager can help you fnd this information. If you need help fnding your foodplain manager, contact FEMA\u2019s Flood Mapping and Insurance eXchange\n a\nt FEMAMapSpecialist@ \nr\niskmapcds.com or (877) FEMA MAP (1 877 336 2627). \nThe following are some additional steps you can take to protect yourself and your property against foods. ",
    "metadata": {
        "page": 1,
        "source": "docs_data/fema_protect-your-home_flooding.pdf"
    }
}
```

成功了！这似乎确实捕获了PDF中的重要页面，并很好地总结了它们。显然，这是一种非常粗略的方法，使用元数据而不仅仅是文件名的更正式方法会更好。使用[模板](https://github.com/langchain-ai/langchain/issues/1136)可能也更优雅，而不仅仅是前缀，但它确实展示了这样一点背景如何帮助。

那么如果我们现在使用所有文档集中的文档呢……

```py
vecs_dir = './vector_dbs/all_docs'
docs = all_docs
# Note the argument prefix_file_name_to_chunks=True
pdf_qa = setup_model(vecs_dir, all_docs_list, docs, prefix_file_name_to_chunks=True, extra_prefix="This snippet relates to ")
result = ask_question("How do I prepare my home for floods?", pdf_qa)
```

```py
Question: 
How do I prepare my home for floods?

Answer:
There are several steps you can take to prepare your home for floods:

1\. Create an emergency plan for your family and practice it regularly. When a storm is approaching, evacuate and move your car to higher ground.

2\. Purchase flood insurance for your home and its contents, even if you do not live in a high-risk flood zone.

3\. Document your belongings. This will help with the insurance process if you need to file a claim.

4\. Store valuables and important documents above the Base Flood Elevation (BFE) in waterproof or water-resistant containers. 

5\. Elevate appliances and utilities such as water heaters, washers, dryers, and electric panels on higher floors to prevent them from getting damaged by flood water.

6\. Use flood-resistant materials for insulation, drywall, and floor coverings like tile to minimize damage.

7\. Make sure your yard slopes away from buildings on your property and that water has a place to drain. 

8\. Anchor any fuel tanks to prevent them from tipping over or floating in a flood.

9\. Add water-resistant exterior sheathing on walls and seal them to prevent shallow flooding from damaging your home.

10\. If you have a manufactured home, make sure it is affixed to a permanent foundation.

11\. Elevate your home above the BFE. 

12\. Secure items in your yard by anchoring them or attaching them to more substantial structures.

13\. Check caulking around windows and doors to make sure it is not cracked, broken, or missing. Fill any holes or gaps around pipes and wires that enter your building.

14\. Build your home, garage, shed, or other building away from river channels and shore lines. If possible, build on higher ground.

15\. Replace carpeting with tiles as they are more flood-resistant.

16\. Floodproof your basement and seal walls with waterproofing compounds. Consider installing a sump pump.

17\. Install flood vents in foundation walls, garages, and other enclosed areas to allow water to flow through, drain out, and lower the risk of structural damage.

18\. Use flood-resistant insulation and drywall.

19\. Prevent sewage back-up by consulting with a plumber and investing in a sewer backflow valve if applicable.

Remember to consult professionals such as your insurance agent, architects, engineers, contractors, or other experts in design and construction before making changes to your home.

{
    "page_content": "This snippet relates to  fema protect your property storm surge: This snippet relates to  fema protect your property storm surge: This snippet relates to  fema protect your property storm surge: This snippet relates to  fema protect your property storm surge:  \n \n  \n \n \n \n \n \n \n INSIDE YOUR HOME\nHAVE A  \nPLANCreate an emergency plan for your family and practice it regularly. \nWhen a storm is approaching, evacuate and move your car to higher ground. According to the National Weather Service , just two feet  \nof water can move a vehicle. Visit Ready.gov/plan  to learn more.\nGET FLOOD \nINSURANCE Most homeowners insurance policies don\u2019t cover flood damage. Protect your investment by purchasing flood insurance \nfor your home and its contents. \nDo so even if you do not live in \na high-risk flood zone.For more information on purchasing \nflood insurance through the \n \nNational Flood Insurance Program, visit FloodSmart.gov  or contact \n \nyour agent to  \nget coverage.\n \n PREPARE OR  \nUPDATE A LIST OF \n YOUR HOME\u2019S \nCONTENTSDocument your belongings. This will give you peace of mind \nand help with the insurance process if you need to file a claim. Consider documenting your home\u2019s contents visually. You can either take photos of high-value items or walk through your home and videotape each room\u2019s belongings.\nSTORE  \nVALUABLESStore valuables and important documents above the BFE (preferably on an upper floor). Place them in waterproof or \nwater-resistant containers. Also, make copies and store them \nonline or offsite.\nELEVATE \nAPPLIANCES AND \nUTILITIES ABOVE \nTHE BFEKeep appliances and utilities such as water heaters, washers, dryers, and \nelectric panels on higher \nfloors. It can prevent them from getting damaged or ruined by flood water.\nTalk to your floodplain \nmanager about how high to elevate your utilities. Many coastal \ncommunities have codes that require utilities to be elevated 12 inches or more above the BFE, called freeboard. \nUSE FLOOD-  \nRESISTANT  \nMATERIALS\nKNOW YOUR \nPROPERTY AND \nNEIGHBORHOODFlood-resistant insulation, drywall, and floor coverings like tile \nwill help minimize damage and are easier to clean and sanitize.\nIf you are moving near a large lake or the ocean, talk with \nneighbors. Find out about any issues they\u2019ve had, or any \nmitigation measures they have taken. Take the time to look \nup flood information. You can find flood maps for coastal areas on the National Flood Hazard Layer .ADDITIONAL RESOURCES\nFEMA, Homebuilder\u2019s Guide to Coastal Construction\nFEMA, Homeowner\u2019s Guide to Retrofitting\nFEMA, Recommended Residential Construction for Coastal Areas\nNATIONAL HURRICANE CENTER,  Storm Surge Overview\nNATIONAL HURRICANE CENTER, Storm Surge Inundation Map\nREMEMBER: \nSome of these tips may work better than others. Tailor \nany mitigation measures to meet your property\u2019s needs.\nAlways consult professionals, such as your insurance agent, architects, \nengineers, contractors, or other experts in design and construction, \nbefore making changes to your home. Your local planning and zoning \noffice or building department is a good place to start for advice.\n",
    "metadata": {
        "page": 3,
        "source": "docs_data/fema_protect-your-property-storm-surge.pdf"
    }
}

{
    "page_content": "This snippet relates to  fema protect your home flooding: This snippet relates to  fema protect your home flooding: This snippet relates to  fema protect your home flooding: This snippet relates to  fema protect your home flooding:  \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n  \n  \n \n \n \n \n \n \n \n \n  \n \n \n  \nDIRECT WATER AWAY \nFROM STRUCTURES If you have a single-family home, make sure your yard \nslopes away from buildings on your property and that water has a place to drain. Clear your gutters, assess drainage issues, or collect water in rain barrels. \nANCHOR FUEL \nTANKS Anchor any fuel tanks to the pad to prevent them from tipping over or foating in a food. Spilled fuel could become a fre hazard. Make sure vents and fll-line openings are above the BFE. Note: This may require permission from your fuel provider. \nFLOODPROOF WALLS Add water-resistant exterior sheathing on walls and seal them to prevent shallow fooding from damaging your home. Cover openings below the BFE and seal all exterior openings around pumping and equipment. \nSECURE \nMANUFACTURED \nHOMES If you have a manufactured home and you want food insurance from the National Flood Insurance Program, your home must be affxed to a permanent foundation so that the wheels and axles do not support its weight and resist fotation, collapse, or side-to-side movement. Your local foodplain manager can help you understand the requirements, and a professional engineer or architect can make sure the anchoring system is designed and installed correctly. REMEMBER: \nSome of these tips may work better together than \nothers. Mitigation measures need to be tailored to your property. Also, not all of these options work together, so talk with an expert who can help you identify which options work best for you. \nAlways consult professionals such as your insurance \nagent, architects, engineers, contractors, or other experts in design and construction before making changes to your home. Your local planning and zoning offce or building department is a good place to start for advice. \nFinally, be kind to your neighbors! Talk to adjacent \nproperty owners before you make changes, since some actions on your property may affect theirs. ",
    "metadata": {
        "page": 4,
        "source": "docs_data/fema_protect-your-home_flooding.pdf"
    }
}

{
    "page_content": "This snippet relates to  fema protect your home flooding: This snippet relates to  fema protect your home flooding: This snippet relates to  fema protect your home flooding: This snippet relates to  fema protect your home flooding:  \n \n \n \n \n \n \n \n \n \n  \n \n \n \n   \n \n \n \n  \n \nOUTSIDE THE HOME \nELEVATE YOUR \nHOME While it is an investment, elevating your home prepares your property against foods and lowers food insurance premiums. When a home is properly elevated, the lowest foor should be above the BFE. Areas below the BFE can be used for parking, storage, or access to the house. DID YOU KNOW? \nElevating your home \nmay reduce your food insurance premium. \nSECURE Y\nARD \nITEMS U\nnsecure items can be swept away or damaged by foodwaters. \nThey can also be swept into your home, causing damage. \nSecure items in your yard by anchoring them or attaching them to more substantial structures. \nSEAL CRA\nCKS \nAND GAPS C\nheck caulking around windows and doors to make it is not \ncracked, broken, or missing. Fill any holes or gaps around pipes and wires that enter your building. \nSET YOUR HOME OR \nBUILDINGS BACK, \nAWAY FROM WATER Build your home, garage, shed, or other building away from river channels and shore lines. If possible, build on higher ground. ",
    "metadata": {
        "page": 3,
        "source": "docs_data/fema_protect-your-home_flooding.pdf"
    }
}

{
    "page_content": "This snippet relates to  fema protect your home flooding: This snippet relates to  fema protect your home flooding: This snippet relates to  fema protect your home flooding: This snippet relates to  fema protect your home flooding:  \n \n \n \n \n   \n \n   \n \n  \n \n \n \n  \n \n \n  \n  \n  \n   \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n \n  \n \nINSIDE THE HOME \nPREPARE OR \nUPDA\nTE A LIST OF \nBELONGINGS Documenting all of your belongings will help with the \ninsurance claims process. Consider taking photos of \nhigh-value items or doing a video walkthrough of your \nho\nme to document its contents. \nGET FLOOD \nINSURANCE Most homeowners insurance \npolicies don\u2019t cover food damage. \nProtect your investment by purchasing food insurance for your home and contents, even if you do not live in \na hi\ngh-risk food zone. \nSTORE \nVALUABLES Store valuables and important documents in waterproof or wa\nter-resistant containers above the BFE \n(preferably on an upper foor). Make copies and store them online or offsite. For more \ninformation on purchasing food insurance through the National Flood \nInsurance Program, visit \nFlo\nodSmart.gov  or contact \nyour agent to get coverage. EL\nEVATE UTILITIES \nABOVE THE BFE Elevate or foodproof mechanical units, furnaces, water \nheaters, electrical systems, and other utilities on masonry, concrete, or pressure-treated lumber at least 12 inches above the BFE. \nREPLACE CARPETING \nWITH TILES Tiles are more food-resistant than carpet. Using tile or other food-resistant materials in areas below the BFE can help reduce water damage. \nFLOODPROOF \nBASEMENTS If you have a basement, minimize damage by foodproofng your basement and sealing walls with waterproofng compounds. Consider installing a sump pump. \nINSTALL FLOOD \nVENTS Install food vents in foundation walls, garages, and other enclosed areas to allow water \nto fow through, drain out, an\nd lower the risk of \nstructural damage. \nUSE FLOOD-RESIST\nANT \nINSULATION & DRYWALL Fl\nood-resistant insulation and drywall will help minimize \ndamage and can be easily cleaned and sanitized. \nPREVENT SEW\nAGE \nBACK-UP I\nn some areas, fooding can cause sewage to back \nup through drain pipes in your home. Consult with a plumber and, if applicable, invest in a sewer backfow valve to prevent this potential health hazard. DID YOU KNOW? \nWhen following National Flood \nInsurance Program regulations, \nv\nents can also help lower \ninsu\nrance rates. \n",
    "metadata": {
        "page": 2,
        "source": "docs_data/fema_protect-your-home_flooding.pdf"
    }
}
```

做得很好，它从[https://www.fema.gov/sites/default/files/2020-11/fema_protect-your-home_flooding.pdf](https://www.fema.gov/sites/default/files/2020-11/fema_protect-your-home_flooding.pdf)中的关键文章提取了信息，还包括[https://www.fema.gov/sites/default/files/documents/fema_protect-your-property-storm-surge.pdf](https://www.fema.gov/sites/default/files/documents/fema_protect-your-property-storm-surge.pdf)中的一篇文章，这是文档集中唯一提到洪水的其他文档。

# 检索提示长度

此时，需要注意的是，最终的总结提示将文档摘录转化为漂亮的答案可能会相当长，具体取决于摘录的大小和数量。与LLM的持续战斗是实现我们的目标而不超出令牌限制。尽管对我们的用例没有重大影响，但其他场景可能需要采用[上下文压缩方法](https://blog.langchain.dev/improving-document-retrieval-with-contextual-compression/)。

# 问题背景可能很重要

我们故意混合了与（i）**灾害规划**和（ii）**立即反应**相关的文档。这可能会导致混淆的响应，将两种背景混合在一起……

```py
result = ask_question("I live next to a canal and see the water rising right now, what should I do?", pdf_qa, output_docs=True)
```

```py
Question: 
I live next to a canal and see the water rising right now, what should I do?

Answer:
If you notice water levels rising immediately, it's important to prioritize your safety. Here are some steps you can take:

1\. Evacuate: If the water levels are rising rapidly, it's crucial to evacuate to higher ground as soon as possible. 

2\. Move your car: If you have time, move your car to higher ground. According to the National Weather Service, just two feet of water can move a vehicle.

3\. Secure your home: If you have time, secure your home by moving valuables and important documents to an upper floor or at least above the Base Flood Elevation (BFE). Place them in waterproof or water-resistant containers. 

4\. Communicate: Let your family, friends, and neighbors know about the situation. If you have a pre-established meeting point, head there or inform others of your evacuation plans.

5\. Contact authorities: Inform local authorities about the situation. They can provide guidance and also alert others who might be at risk.

6\. Check flood maps: If you have access to the internet, check flood maps for your area to understand the potential risk.

Remember, these steps are general guidelines. Always follow the advice of local authorities and emergency services during a flood situation.
```

现在的答案有点混乱，有些点涉及立即行动“将你的车移到高地”，有些点涉及准备“购买洪水保险”。当人们在紧急情况下感到压力时，他们可能不会考虑即时工程，因此我们可以期待稍微模糊的输入。

当然，我们可以通过使用更多的文档元数据来将其拆分为子组来解决此问题，但如果没有这些元数据，则需要工作。另一种选择是向问题提供更多背景，以指示用户是否对灾害准备感兴趣，或需要立即帮助。我们可以为此建立一个分类器，但在这些强大的LLM时代，我们可以使用GPT-4的零样本分类……

```py
def get_time_context(question):

    template = """Does the following question relate to 'planning' or 'taking immediate action': {question}

    Answer with one of the following: 'I am planning ahead:' or 'I need to take immediate action:' or 'ambiguous'"""

    prompt = PromptTemplate(template=template, input_variables=["question"])

    llm = OpenAI()

    llm_chain = LLMChain(prompt=prompt, llm=llm)
    answer = llm_chain.run(question)
    return answer

questions = [
    "I live next to a canal and the water is rising, what should I do?",
    "Help my roof is blowing off!",
    "How do I prevent my roof from blowing off in a hurricane",
    "Dog",
    "How do I prepare my home for floods?"
]
for q in questions:
    print(f"Question: {q}")
    answer = get_time_context(q)
    print(answer.strip(), "\n")
```

```py
Question: I live next to a canal and the water is rising, what should I do?
I need to take immediate action: 

Question: Help my roof is blowing off!
I need to take immediate action: 

Question: How do I prevent my roof from blowing off in a hurricane
I am planning ahead. 

Question: Dog
Ambiguous 

Question: How do I prepare my home for floods?
I am planning ahead. 
```

很好！只需很少的努力，我们就可以轻松确定问题是涉及规划还是立即行动。

现在我们可以用这个前缀来前置用户问题……

```py
def get_time_context(question: str) -> str:

    template = """Does the following question relate to 'planning' or 'taking immediate action': {question}

    Answer with one of the following: 'I am planning ahead:' or 'I need to take immediate action:' or 'ambiguous'"""

    prompt = PromptTemplate(template=template, input_variables=["question"])

    llm = OpenAI()

    llm_chain = LLMChain(prompt=prompt, llm=llm)
    answer = llm_chain.run(question)
    return answer

def ask_question(
    query: str,
    qa: object,
    output_docs: bool = True,
    preprocess_time_context: bool = False
) -> dict:

    # First get the time context
    if preprocess_time_context:
        time_context = get_time_context(query)
        if 'planning' in time_context.lower():
            query = f"I am planning ahead: {query}"
        elif 'take immediate action' in time_context.lower():
            query = f"I need to take immediate action: {query}"

    print(f"\nQuestion: \n{query}")
    result = qa({"question": query})

    print(f"\nAnswer:\n{result['answer']}")
    if output_docs:
        for doc in result['source_documents']:
            print('\n')
            print(json.dumps(vars(doc), indent=4))
    return result

result = ask_question("I live next to a canal and see the water rising, what should I do?", pdf_qa, output_docs=False, preprocess_time_context=True)
```

这给……

```py
Question: 
I need to take immediate action: I live next to a canal and see the water rising, what should I do?

Answer:
If you see the water rising next to your canal, you should immediately implement your emergency plan. This includes evacuating your home and moving your car to higher ground, as just two feet of water can move a vehicle. You should also ensure that your valuables and important documents are stored above the Base Flood Elevation (BFE), preferably on an upper floor, and placed in waterproof or water-resistant containers. If you have time, consider moving appliances and utilities such as water heaters, washers, dryers, and electric panels to higher floors to prevent them from getting damaged or ruined by flood water. Always prioritize your safety and the safety of your family over protecting property.
```

很好，效果很好，没有试图推销任何保险，给出的建议可以立即付诸实践。

让我们测试一下反向情况…

```py
result = ask_question("What can I do to make my house forest fire resistant?", pdf_qa, output_docs=False, preprocess_time_context=True)
```

```py
Question: 
I am planning ahead: What can I do to make my house forest fire resistant?

Answer:
There are several steps you can take to make your house resistant to forest fires:

1\. Install or replace your roof with a Class A-rated roof with noncombustible coverings. The roof is the most at-risk part of a house in a wildfire due to its size and orientation.

2\. Install and replace exterior wall coverings with noncombustible or fire-resistant materials. A minimum fire-resistance rating of one hour for the wall assembly is recommended.

3\. Create a 30 feet defensible space around your home by reducing or removing flammable vegetation and using noncombustible materials such as gravel, brick, or concrete.

4\. Regularly clean and remove debris from the roof and gutters to reduce the likelihood of something catching on fire on top of your home.

5\. Enclose your home's foundation to lower the chance of wind-blown embers getting underneath your home.

6\. Plan for access to water by purchasing and installing external sprinkler systems with dedicated power sources or a water tank.

7\. Protect large windows from radiant heat by installing multi-pane windows, tempered safety glass, or fireproof shutters.

8\. Install highly visible street signs and property addresses to help firefighters and other emergency responders quickly find your property.

9\. Seal gaps around openings in exterior walls and roofs with fire-resistant caulk, mortar, or fire-protective expanding foam.

10\. Cover exterior attic vents and under-eave vents with metal wire mesh no larger than 1/8 inch to keep embers out.

11\. Install a fire block in the gap between the top of framed walls and the foundation of the house to starve the fire of oxygen and prevent it from spreading.

Remember, these tips may work better together than others and need to be tailored to your property. Always consult professionals such as your insurance agent, architects, engineers, contractors, or other experts in design and construction before making changes to your home.
```

完美，它提供了森林火灾准备信息。

显然，这需要对任何高风险的真实紧急情况进行大量测试，但它说明了我们可以通过丰富用户的提示来提升性能的一种方法。

# 确保答案仅来自提供的文档

对于一个信息可能影响安全性的应用程序，确保提供的信息*仅*来自提供的文档是非常重要的。包含错误信息的幻觉可能会产生非常严重的后果。

让我们尝试问一些与灾难完全无关的问题…

```py
result = ask_question("How do I make a sponge cake?", pdf_qa, output_docs=False)
```

这会提供…

```py
Question: 
How do I make a sponge cake?

Answer:
I'm sorry, but the provided context does not contain information on how to make a sponge cake.
```

虽然我有点遗憾没能找到如何做一些美味的蛋糕，但 LangChain 很好地处理了这个场景，并判断出问题与提供的 PDF 指南中的信息无关。

很好，没有重大幻觉！在灾难中，我最不希望的就是被告知去做海绵蛋糕或飞宇宙飞船之类的事。😊

# 对话历史

LangChain 的一个很棒的功能是它无缝地处理对话历史。只需一行代码，模型就能拾起之前的问题…

```py
result = ask_question("There's a hurricane coming, what should I do?", pdf_qa, output_docs=False)

Question: 
I need to take immediate action: There's a hurricane forecast, what should I do?

Answer:
If there's a hurricane forecast, you should take the following immediate actions:

1\. Check the flood risk for your area. You can do this by visiting https://msc.fema.gov/portal/home and entering your home’s address.

2\. Understand the difference between a Hurricane Watch and a Hurricane Warning. A Hurricane Watch means that hurricane conditions are possible within a specified area and is issued 48 hours in advance of the anticipated onset of tropical-storm-force winds. A Hurricane Warning means that hurricane conditions are expected within a specified area and is issued 36 hours in advance of the anticipated onset of tropical-storm-force winds.

3\. Take actions around your house to help reduce the impact of a flooding event. This could include placing sand bags in areas that are most at risk from flooding and elevating mechanical devices like air conditioners, generators, and circuit breakers to minimize the risk of impact by storm surge.

4\. Establish a central meeting point so that if the power does go out, people know where to meet up with their loved ones.

5\. Prepare a Go-Kit which should include water, a first-aid kit, a flashlight, batteries, a hand-crank/solar powered radio, non-perishable food, cash, a wrench, hand sanitizer, a mylar blanket, ear plugs, and a book and/or deck of cards.

6\. Make sure to have items and services in place before a hurricane watch or warning is issued. This could include packing comfort items for loved ones and ensuring that any relatives who require regularly scheduled mental health therapy have their providers aware of the location of the shelter.

7\. Store critical documents in Ziploc bags and keep them in a safe place. Alternatively, you can store them on a cloud drive, on a flash drive kept in an alternate location, or in a safety deposit box.

8\. Create a Go-Kit for your pet with essential items, including food, water, medication and favorite toys. Make sure your pet is microchipped or at least has ID tags displayed.

9\. Familiarize yourself with the predetermined coastal evacuation routes in your area.

10\. Plan for communication methods in case phone lines and cellular towers get damaged. This could include text messaging or using social media platforms like Twitter, Facebook, Instagram, etc.
```

然后问它一个指向“它”（飓风）的提问…

```py
result = ask_question("It's about to arrive, help!", pdf_qa, output_docs=False, preprocess_time_context=True)

Question: 
It's about to arrive, help!

Answer:
Here are some immediate actions you should take for safety:

1\. If you are in a flood zone, consider evacuating to a safer location. If evacuation is not possible, move to higher ground within your home.
2\. Secure your home by closing and boarding up windows. Remove any outdoor items that could be picked up by the wind.
3\. Gather your emergency supplies, including your Go-Kit which should include water, a first-aid kit, a flashlight, batteries, a hand-crank/solar powered radio, non-perishable food, cash, a wrench, hand sanitizer, a mylar blanket, ear plugs, and a book and/or deck of cards.
4\. If you have pets, ensure you have a Go-Kit for them as well, including food, water, medication and favorite toys. Make sure they are microchipped or have ID tags.
5\. Make sure your car is filled with gas and ready to go in case you need to evacuate.
6\. Unplug all electrical appliances to prevent power-surge damage.
7\. Stay informed about the storm's path and intensity by listening to the radio or checking trusted websites.
8\. Communicate with family members and make sure everyone knows the plan. If possible, establish a central meeting point in case you get separated.
9\. Store valuable items and important documents in a safe, high place or in waterproof containers.
10\. If you have time, place sandbags in areas that are most at risk from flooding. 

Remember, your safety is the most important thing. Material items can be replaced, but lives cannot.
```

完美，它保持了历史记录，并知道我在指什么。在压力下，用户可能会提出后续问题，因此这种能力——用 2 行代码实现！——是非常重要的。

# 翻译

为了使聊天机器人更加多功能，我们可以探索多语言支持。许多主要的 LLM，如 [GPT-4 提供了对主要语言的本地支持](https://openai.com/research/gpt-4)，但性能可能会因语言而异。

让我们从葡萄牙语开始…

```py
from googletrans import Translator
translator = Translator()

q = translator.translate("How do I prepare my home for floods?", dest='pt')

print("================ RAW =================")
pdf_qa = setup_model(vecs_dir, all_docs_list, docs, prefix_file_name_to_chunks=True, extra_prefix="This snippet relates to ")
result = ask_question(q.text, pdf_qa, output_docs=True, preprocess_time_context=True)

q = translator.translate(result['answer'], dest='en')
print("\n\n=============== TRANSLATED TO ENGLISH USING GOOGLE TRANSLATE =================\n")
print(q.text)
```

```py
================ RAW =================

Question: 
I am planning ahead: Como preparar minha casa para enchentes?

Answer:
Existem várias maneiras de preparar sua casa para enchentes:

1\. Tenha um plano de emergência: Crie um plano de emergência para sua família e pratique-o regularmente. Quando uma tempestade se aproxima, evacue e mova seu carro para um terreno mais alto.

2\. Obtenha seguro contra inundações: A maioria das apólices de seguro residencial não cobre danos causados por inundações. Proteja seu investimento comprando seguro contra inundações para sua casa e seu conteúdo.

3\. Prepare ou atualize uma lista do conteúdo da sua casa: Documente seus pertences. Isso lhe dará tranquilidade e ajudará no processo de seguro se você precisar fazer uma reclamação.

4\. Armazene objetos de valor: Armazene objetos de valor e documentos importantes acima do BFE (preferencialmente em um andar superior). Coloque-os em recipientes à prova d'água ou resistentes à água.

5\. Eleve aparelhos e utilidades acima do BFE: Mantenha aparelhos e utilidades como aquecedores de água, lavadoras, secadoras e painéis elétricos em andares mais altos. Isso pode evitar que sejam danificados ou arruinados pela água da enchente.

6\. Use materiais resistentes a inundações: Isolamento resistente a inundações, drywall e revestimentos de piso como azulejos ajudarão a minimizar os danos e são mais fáceis de limpar e higienizar.

7\. Conheça sua propriedade e vizinhança: Se você está se mudando para perto de um grande lago ou do oceano, converse com os vizinhos. Descubra quaisquer problemas que eles tiveram ou quais medidas de mitigação eles tomaram.

8\. Eleve sua casa: Embora seja um investimento, elevar sua casa prepara sua propriedade contra inundações e reduz os prêmios de seguro contra inundações.

9\. Proteja os itens do seu quintal: Itens inseguros podem ser levados ou danificados pelas águas da enchente. Eles também podem ser levados para dentro de sua casa, causando danos.

10\. Selo rachaduras e lacunas: Verifique o calafetagem ao redor das janelas e portas para garantir que não esteja rachado, quebrado ou faltando. Preencha quaisquer buracos ou lacunas ao redor de tubos e fios que entram em seu prédio.

Lembre-se, sempre consulte profissionais, como seu agente de seguros, arquitetos, engenheiros, empreiteiros ou outros especialistas em design e construção, antes de fazer alterações em sua casa.

=============== TRANSLATED TO ENGLISH USING GOOGLE TRANSLATE =================

There are several ways to prepare your home for flooding:

1\. Have an emergency plan: Create an emergency plan for your family and practice it regularly. When a storm approaches, evacuate and move your car to higher ground.

2\. Get flood insurance: Most home insurance policies don't cover flood damage. Protect your investment by purchasing flood insurance for your home and contents.

3\. Prepare or update a list of your household contents: Document your belongings. This will give you peace of mind and help with the insurance process if you ever need to file a claim.

4\. Store valuables: Store valuables and important documents above the BFE (preferably on an upper floor). Place them in waterproof or water-resistant containers.

5\. Elevate appliances and utilities above the BFE: Keep appliances and utilities like water heaters, washers, dryers, and electrical panels on higher floors. This can prevent them from being damaged or ruined by floodwater.

6\. Use Flood Resistant Materials: Flood resistant insulation, drywall and floor coverings like tile will help minimize damage and are easier to clean and sanitize.

7\. Know your property and neighborhood: If you're moving near a large lake or the ocean, talk to your neighbors. Find out about any issues they've had or what mitigation measures they've taken.

8\. Elevate your home: Although it is an investment, elevating your home prepares your property against flooding and reduces flood insurance premiums.

9\. Protect Your Yard Items: Unsafe items can be washed away or damaged by floodwaters. They can also be brought into your home, causing damage.

10\. Seal Cracks and Gaps: Check the caulking around windows and doors to ensure it isn't cracked, broken, or missing. Fill in any holes or gaps around pipes and wires entering your building.

Remember, always consult with professionals, such as your insurance agent, architects, engineers, contractors, or other design and construction experts, before making changes to your home.
```

哪个信息是正确的。然而，对于斯瓦希里语…

```py
q = translator.translate("How do I prepare my home for floods?", dest='sw')

print("================ RAW =================")
pdf_qa = setup_model(vecs_dir, all_docs_list, docs, prefix_file_name_to_chunks=True, extra_prefix="This snippet relates to ")
result = ask_question(q.text, pdf_qa, output_docs=True, preprocess_time_context=True)

q = translator.translate(result['answer'], dest='en')
print("\n\n=============== TRANSLATED TO ENGLISH USING GOOGLE TRANSLATE =================\n")
print(q.text)
```

```py
================ RAW =================

Question: 
Je, ninatayarishaje nyumba yangu kwa mafuriko?

Answer:
Kuna hatua kadhaa unazoweza kuchukua ili kulinda nyumba yako dhidi ya mafuriko:

1\. Elekeza Maji Mbali na Majengo: Hakikisha eneo lako linaelekea mbali na majengo kwenye mali yako na kwamba maji yana mahali pa kumwagika. Safisha mifereji yako, tathmini masuala ya mifereji ya maji, au kukusanya maji katika mapipa ya mvua.

2\. Nanga Mizinga ya Mafuta: Nanga mizinga yoyote ya mafuta kwenye pedi ili kuzuia kuzunguka au kuogelea katika mafuriko. Mafuta yaliyomwagika yanaweza kuwa hatari ya moto. Hakikisha kuwa matundu ya uingizaji hewa na ufunguzi wa mstari wa kujaza yako juu ya BFE (Base Flood Elevation). Kumbuka: Hii inaweza kuhitaji ruhusa kutoka kwa mtoa huduma wako wa mafuta.

3\. Kinga Kuta dhidi ya Mafuriko: Ongeza sheathing inayostahimili maji kwenye kuta na uzibane ili kuzuia mafuriko ya kina kifupi kuharibu nyumba yako. Funika ufunguzi chini ya BFE na ziba ufunguzi wote wa nje karibu na pampu na vifaa.

4\. Thibitisha Nyumba Zilizotengenezwa: Ikiwa una nyumba iliyotengenezwa na unataka bima ya mafuriko kutoka kwa Programu ya Bima ya Mafuriko ya Kitaifa, nyumba yako lazima iwe imewekwa kwenye msingi wa kudumu ili magurudumu na axles zisitoe uzito wake na kuzuia kuelea, kuanguka, au harakati ya upande kwa upande.

5\. Kumbuka: Baadhi ya vidokezo hivi vinaweza kufanya kazi vizuri pamoja kuliko vingine. Hatua za kupunguza madhara zinahitaji kubinafsishwa kwa mali yako. Pia, sio chaguo zote hufanya kazi pamoja, kwa hivyo ongea na mtaalamu anayeweza kukusaidia kutambua chaguo zipi zinafanya kazi vizuri kwako.

Daima shauriana na wataalamu kama wakala wako wa bima, wasanifu, wahandisi, wakandarasi, au wataalamu wengine katika kubuni na ujenzi kabla ya kufanya mabadiliko kwenye nyumba yako. Ofisi yako ya mipango na mipango ya eneo au idara ya ujenzi ni mahali pazuri pa kuanza kwa ushauri.

Mwishowe, kuwa mwema kwa majirani zako! Zungumza na wamiliki wa mali jirani kabla ya kufanya mabadiliko, kwani vitendo fulani kwenye mali yako vinaweza kuathiri yao.

=============== TRANSLATED TO ENGLISH USING GOOGLE TRANSLATE =================

There are several steps you can take to protect your home from flooding:

1\. Direct Water Away from Buildings: Make sure your area faces away from buildings on your property and that water has a place to drain. Clean your gutters, assess drainage issues, or collect water in rain barrels.

2\. Anchor Oil Tanks: Anchor any oil tanks to a pad to prevent them from rolling or swimming in a flood. Spilled oil can be a fire hazard. Make sure the vents and fill line opening are above the BFE (Base Flood Elevation). Note: This may require permission from your fuel supplier.

3\. Protect Walls from Flooding: Add waterproof sheathing to walls and seal them to prevent shallow flooding from damaging your home. Cover the opening under the BFE and cover all external openings around the pump and equipment.

4\. Verify Manufactured Homes: If you have a manufactured home and want flood insurance from the National Flood Insurance Program, your home must be placed on a permanent foundation so that the wheels and axles do not give off its weight and prevent floating, falling, or movement of side by side.

5\. Note: Some of these tips may work better together than others. Mitigation measures need to be customized to your property. Also, not all options work together, so talk to a professional who can help you figure out which options work best for you.

Always consult with professionals such as your insurance agent, architects, engineers, contractors, or other professionals in design and construction before making changes to your home. Your local planning and development office or building department is a good place to start for advice.

Finally, be kind to your neighbors! Talk to neighboring property owners before making changes, as certain actions on your property may affect theirs.
```

结果*看起来*非常可信，但缺少重要信息。匹配的文本摘录缺少一些关键的洪水部分，当我们询问洪水时，还得到了与野火相关的片段。基本上，使用从英语文档创建的嵌入来处理斯瓦希里语问题效果不好。不过，使用拉丁语言的葡萄牙语效果很好，这并不令人意外。

也许更稳健的方法是先检测语言，再用 [Google Translate](https://translate.google.com/) 将其翻译成英语，然后将响应转换回提示语言。

让我们将这个添加到我们的聊天界面中…

```py
def get_time_context(question: str) -> str:

    template = """Does the following question relate to 'planning' or 'taking immediate action': {question}

    Answer with one of the following: 'I am planning ahead:' or 'I need to take immediate action:' or 'ambiguous'"""

    prompt = PromptTemplate(template=template, input_variables=["question"])

    llm = OpenAI()

    llm_chain = LLMChain(prompt=prompt, llm=llm)
    answer = llm_chain.run(question)
    return answer

def ask_question(
    query: str,
    qa: object,
    output_docs: bool = True,
    preprocess_time_context: bool = False,
    auto_translate: bool = False,
) -> dict:

    # Auto-translate
    if auto_translate:
        lang = translator.detect(query)
        lang = lang.lang
        print(f"Language detected: {lang}")
        q = translator.translate(query, dest='en')
        query = q.text

    # First get the time context
    if preprocess_time_context:
        time_context = get_time_context(query)
        if 'planning' in time_context.lower():
            query = f"I am planning ahead: {query}"
        elif 'take immediate action' in time_context.lower():
            query = f"I need to take immediate action: {query}"

    print(f"\nQuestion: \n{query}")
    result = qa({"question": query})

    # Translate back to source language
    if auto_translate:
        q = translator.translate(result['answer'], dest=lang)
        result['answer'] = q.text

    print(f"\nAnswer:\n{result['answer']}")
    if output_docs:
        for doc in result['source_documents']:
            print('\n')
            print(json.dumps(vars(doc), indent=4))
    return result

# Translate to Swahili
q = translator.translate("How do I prepare my home for floods?", dest='sw')

print("================ RAW =================")
result = ask_question(q.text, pdf_qa, output_docs=False, preprocess_time_context=True, auto_translate=True)

q = translator.translate(result['answer'], dest='en')
print("\n\n=============== TRANSLATED TO ENGLISH USING GOOGLE TRANSLATE =================\n")
print(q.text)
```

```py
================ RAW =================
Language detected: sw

Question: 
I am planning ahead: How do I prepare my home for flooding?

Answer:
Kuna hatua kadhaa unazoweza kuchukua ili kuandaa nyumba yako kwa mafuriko yanayoweza kutokea:

1\. Tengeneza mpango wa dharura kwa ajili ya familia yako na uufanyie kazi mara kwa mara. Dhoruba inapokaribia, ondoka na usogeze gari lako hadi sehemu ya juu.

2\. Nunua bima ya mafuriko kwa ajili ya nyumba yako na vilivyomo, hata kama huishi katika eneo lenye hatari kubwa ya mafuriko.

3\. Andika vitu vyako. Hii itasaidia katika mchakato wa bima ikiwa unahitaji kuwasilisha dai.

4\. Hifadhi vitu vya thamani na hati muhimu juu ya Mwinuko wa Mafuriko ya Msingi (BFE), ikiwezekana kwenye ghorofa ya juu, katika vyombo visivyo na maji au visivyo na maji.

5\. Kuinua vifaa na huduma kama vile hita za maji, washers, vikaushio na paneli za umeme kwenye sakafu ya juu ili kuvizuia kuharibiwa na maji ya mafuriko.

6\. Tumia nyenzo zinazostahimili mafuriko nyumbani kwako.

7\. Hakikisha yadi yako ina mteremko mbali na majengo kwenye mali yako na kwamba maji yana mahali pa kutolea maji.

8\. Tia nanga matangi yoyote ya mafuta ili kuyazuia yasitembee juu au kuelea katika mafuriko.

9\. Ongeza sheati za nje zinazostahimili maji kwenye kuta na uzifunge ili kuzuia mafuriko ya kina yasiharibu nyumba yako.

10\. Ikiwa una nyumba iliyotengenezwa, hakikisha kuwa imewekwa kwenye msingi wa kudumu.

11\. Pandisha nyumba yako juu ya BFE.

12\. Linda vitu katika yadi yako kwa kuvitia nanga au kuviambatanisha na miundo mikubwa zaidi.

13\. Angalia kuzunguka kwa madirisha na milango ili kuhakikisha kuwa haijapasuka, haijavunjwa au kukosa. Jaza mashimo au mapengo yoyote karibu na mabomba na waya zinazoingia kwenye jengo lako.

14\. Jenga nyumba yako, karakana, banda, au jengo lingine mbali na njia za mito na ufuo. Ikiwezekana, jenga juu ya ardhi ya juu.

Kumbuka kushauriana na wataalamu kama vile wakala wako wa bima, wasanifu majengo, wahandisi, wakandarasi, au wataalamu wengine wa usanifu na ujenzi kabla ya kufanya mabadiliko kwenye nyumba yako.

=============== TRANSLATED TO ENGLISH USING GOOGLE TRANSLATE =================

There are several steps you can take to prepare your home for potential flooding:

1\. Make an emergency plan for your family and work on it regularly. When a storm approaches, take off and move your car to the top.

2\. Buy flood insurance for your home and contents, even if you don't live in a high flood risk area.

3\. Write your items. This will help with the insurance process if you need to file a claim.

4\. Store valuables and important documents above the Base Flood Elevation (BFE), preferably on an upper floor, in waterproof or waterproof containers.

5\. Elevate equipment and services such as water heaters, washers, dryers and electrical panels on the upper floor to prevent them from being damaged by flood water.

6\. Use flood resistant materials in your home.

7\. Make sure your yard is sloped away from the buildings on your property and that the water has a place to drain.

8\. Anchor any fuel tanks to prevent them from rolling over or floating in flooding.

9\. Add waterproof outer sheathing to the walls and seal them to prevent deep flooding from damaging your home.

10\. If you have a manufactured home, make sure it is placed on a permanent foundation.

11\. Raise your house above the BFE.

12\. Secure things in your yard by anchoring them or attaching them to larger structures.

13\. Check window and door surrounds to make sure they are not cracked, broken or missing. Fill any holes or gaps around pipes and wires entering your building.

14\. Build your house, garage, shed, or other building away from river and beach paths. If possible, build on higher ground.

Remember to consult with professionals such as your insurance agent, architects, engineers, contractors, or other design and construction professionals before making changes to your home.
```

更好，使用 Google Translate 将斯瓦希里语自动翻译成英语，然后将回答翻译回斯瓦希里语，能从我们的 PDF 文件集中获得所有所需的信息。对于灾难响应等风险关键的用例，当然需要与母语者进行大量测试以确保安全，但这在多语言支持方面显示出了前景。

# 提出更广泛的问题

好的，让我们试用一下最终版本，提出更多与灾难相关的问题……

```py
# Set up the model again or our Swahili conversation history will be used
vecs_dir = './vector_dbs/all_docs'
docs = all_docs
pdf_qa = setup_model(vecs_dir, all_docs_list, docs, prefix_file_name_to_chunks=True, extra_prefix="This snippet relates to ")

questions = [
    "What should I do for pets if there is a forest fire?",
    "My roof is blowing off!!! Help, what should I do?!!!",
    "I live next to a brook, should I worry about floods?",
    "If I live inland, will sea level affect me?",
    "Is there a cheap way to get flood insurance?",
    "We're in an Earthquake, what should we do!!???",
    "What things should I have in an emergency kit?",
]

for q in questions:
    print("\n ===================== ")
    ask_question(q, pdf_qa, output_docs=False, preprocess_time_context=True, auto_translate=True)
```

```py
===================== 
Language detected: en

Question: 
I need to take immediate action: What should I do for pets if there is a forest fire?

Answer:
If there is a forest fire, you should have a Go-Kit for your pet(s) which should include food, water, medication, medical records kept in Ziploc bags, and favorite toys (if applicable). Include your veterinarian’s contact information and be sure that your pet is microchipped or at least has the proper ID tags displayed. If you need to evacuate, take your pet with you if possible. If you cannot take your pet with you, ensure they are in a safe place with access to food and water.

 ===================== 
Language detected: en

Question: 
I need to take immediate action: My roof is blowing off!!! Help, what should I do?!!!

Answer:
The FEMA brochures do not provide specific instructions for immediate action if your roof is blowing off. However, it is generally recommended to seek immediate shelter in a safe area of your home, away from windows and potential falling debris. After the severe wind event, you should contact a professional to assess and repair the damage. It's also important to contact your insurance company to report the damage.

 ===================== 
Language detected: en

Question: 
I live next to a brook, should I worry about floods?

Answer:
Yes, you should be concerned about floods if you live next to a brook. Flooding can happen anywhere and is not limited to high-risk areas. Heavy rainfall or poor drainage can cause flooding, especially in low-lying areas. It's important to take steps to protect your home, such as elevating your home, securing yard items, and ensuring proper drainage. You should also consider purchasing flood insurance, as federal disaster assistance may be limited or unavailable without a presidential disaster declaration.

 ===================== 
Language detected: en

Question: 
If I live inland, will sea level affect me?

Answer:
Sea level changes primarily affect coastal areas, causing increased flooding, erosion, and storm damage. However, if you live inland, you may still experience indirect effects. Changes in sea level can alter weather patterns, potentially leading to increased rainfall or drought in some areas. Additionally, if you rely on resources from coastal areas, such as seafood or tourism, changes in sea level could impact these industries. However, the snippet does not provide specific information on how sea level changes might affect those living inland.

 ===================== 
Language detected: en

Question: 
I am planning ahead: Is there a cheap way to get flood insurance?

Answer:
Yes, you can purchase flood insurance through the National Flood Insurance Program. Most homeowners insurance policies don’t cover flood damage, so it's important to protect your investment by purchasing flood insurance for your home and its contents. This is recommended even if you do not live in a high-risk flood zone. For more information on purchasing flood insurance, you can visit FloodSmart.gov or contact your insurance agent to get coverage. Planning ahead and purchasing flood insurance before a storm or flood event is a cost-effective way to protect your property.

 ===================== 
Language detected: en

Question: 
I need to take immediate action: We're in an Earthquake, what should we do!!???

Answer:
When an earthquake starts, you should "Drop, Cover, and Hold On!" This means you should drop to the ground, cover your head and neck with your arms, and if a safer place is nearby, crawl to it and hold on. If you are in bed, stay there and cover your head and neck with a pillow. If you are in a moving vehicle, stop as quickly and safely as possible and stay in the vehicle. Avoid stopping near or under buildings, trees, overpasses, and utility wires. Proceed cautiously once the earthquake has stopped, avoiding roads, bridges, or ramps that the earthquake may have damaged.

 ===================== 
Language detected: en

Question: 
I am planning ahead: What things should I have in an emergency kit?

Answer:
An emergency kit should include the following items: 

- Water (one gallon per person, per day) and/or water purification tablets
- A first-aid kit
- A flashlight
- Batteries
- A hand-crank/solar-powered radio
- Nonperishable food
- Medications/prescription glasses
- Cash (in small denominations)
- A wrench (to turn off utilities)
- Hand sanitizer
- A Mylar blanket
- Ear plugs
- A book/deck of cards

If you have an infant, then include baby food, diapers, formula, etc. If you have pets, consider having a kit for them that includes food, water, medication, medical records, and a favorite toy. 

In addition, consider including portable charging devices in your kit as this will allow you to charge from a motor vehicle. If a loved one is comforted by a certain item (a blanket, photograph, stuffed animal, etc.), be sure to pack the item when evacuating. If a relative requires regularly scheduled mental health therapy, make sure that his/her mental health provider is aware of the location of the shelter.
```

我认为这些非常惊人，抽查了一些，它们似乎能够捕捉到所用PDF文档中的关键信息。

[基于上述内容，我的灾难包现在有耳塞了！]

# 性能评估

在这项分析中，我们仅仅对我们的聊天界面在返回一小部分FEMA PDF文档的关键信息方面进行了抽查。这对于说明一些概念是有帮助的，但对于生产应用程序，我们需要比抽查更好的方法。幸运的是，LangChain 提供了一组[评估工具](https://python.langchain.com/docs/guides/evaluation/)，特别值得关注的是一个[Streamlit 应用](https://blog.langchain.dev/auto-eval-of-question-answering-tasks/)，它可以自动生成问答对，然后利用这些数据来评估检索链，从而允许开发者实验一些相关的参数。我还没有尝试过，但使用LLM自动生成评估数据的想法似乎是为测试我们的FEMA灾难机器人构建更系统化方法的好办法。

# 结论

使用FEMA网站上的34份PDF文档，我们能够轻松地构建一个多语言对话界面，使用LangChain和GPT-4，能够回答广泛的灾难准备和安全协议相关的问题。然而，对于高风险安全关键的聊天机器人，使用这种通用文档检索的LangChain模式面临着与语义搜索相同的挑战。即使使用先进的LLM嵌入捕捉自然语言中的更多细微差别，仍然很容易*丢失*重要内容。

对于我们的FEMA灾难机器人用例，这种情况是由于：

+   **混合上下文文档** — 在没有包含文档元数据的情况下，响应混合了来自不同上下文的信息。简单地在文档名称前加前缀提高了我们场景中的性能，并且可以应用更复杂的元数据策略。此外，添加一个零样本LLM分类器来丰富用户问题也是有帮助的。

+   **LLMs中的低代表性语言** — 依赖LLM翻译可能会导致对于代表性较少的语言（如斯瓦希里语）性能较差。添加自动Google翻译提高了我们用例中的性能。

我们探索了粗略的方法来解决上述问题，但对于生产聊天机器人，需要通过测试和验证找到更先进的技术和其他缓解措施。

尽管如此，LLM现在能做的事情确实令人惊叹！

# **参考资料**

你可以在[这里](https://github.com/datakind/stay_safe_bot)找到包含所有代码的笔记本。
