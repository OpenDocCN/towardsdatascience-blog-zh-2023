# 以规模化方式掌握语义搜索：使用 FAISS 和 Sentence Transformers 在闪电般的推理时间内索引数百万份文档

> 原文：[`towardsdatascience.com/master-semantic-search-at-scale-index-millions-of-documents-with-lightning-fast-inference-times-fa395e4efd88`](https://towardsdatascience.com/master-semantic-search-at-scale-index-millions-of-documents-with-lightning-fast-inference-times-fa395e4efd88)

## 深入了解一个高性能的语义搜索引擎的端到端演示，利用 GPU 加速、高效的索引技术和强大的句子编码器处理多达 100 万份文档的数据集，实现 50 毫秒的推理时间

[](https://medium.com/@luisroque?source=post_page-----fa395e4efd88--------------------------------)![Luís Roque](https://medium.com/@luisroque?source=post_page-----fa395e4efd88--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fa395e4efd88--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fa395e4efd88--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----fa395e4efd88--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fa395e4efd88--------------------------------) ·阅读时间 15 分钟·2023 年 3 月 31 日

--

# 介绍

在搜索和信息检索领域，语义搜索已经成为一场变革。它使我们能够根据文档的意义或概念而不仅仅是关键字匹配来进行搜索和检索。与传统的基于关键字的搜索方法相比，语义搜索能够提供更复杂、更相关的结果。然而，挑战在于将语义搜索扩展到处理大量文档的语料库，而不会因分析每个文档的语义内容的计算复杂性而不堪重负。

在这篇文章中，我们迎接了通过利用两种前沿技术来实现可扩展语义搜索的挑战：FAISS 用于高效的语义向量索引，Sentence Transformers 用于将句子编码为这些向量。FAISS 是一个出色的库，旨在快速检索高维空间中的最近邻，使得即使在大规模下也能迅速进行语义最近邻搜索。Sentence Transformers，一个深度学习模型，生成句子的密集向量表示，有效捕捉其语义含义。

本文展示了如何利用 FAISS 和句子变换器的协同作用，构建一个具有卓越性能的可扩展语义搜索引擎。通过将 FAISS 和句子变换器集成，我们可以对来自大量文档的语义向量进行索引，从而实现快速准确的语义搜索体验。我们的方法可以实现新的应用，如上下文化问答和先进的推荐系统，当搜索 1M 文档的语料库时推理时间低至 50 毫秒。我们将指导你实现这一最先进的端到端解决方案，并在基准数据集上展示其性能。

![](img/6d6e13617bc14df0e964b33fe23e756f.png)

图 1：搜索是你在这个世界上导航所需的一切（[来源](https://unsplash.com/photos/afW1hht0NSs)）

本文属于“**大语言模型纪实：导航 NLP 前沿**”系列，这是一系列每周更新的文章，将探讨如何利用大模型的力量来完成各种 NLP 任务。通过深入这些前沿技术，我们旨在赋能开发者、研究人员和爱好者，充分利用 NLP 的潜力，开启新的可能性。

迄今已发布的文章：

1.  [用 ChatGPT 总结最新的 Spotify 发布](https://medium.com/towards-data-science/summarizing-the-latest-spotify-releases-with-chatgpt-553245a6df88)

如往常一样，代码可在我的[Github](https://github.com/luisroque/large_laguage_models)上找到。

# 句子变换器用于语义编码

深度学习带来了句子变换器的力量，它们制作密集的向量表示，捕捉句子意义的本质。这些模型在大量数据上进行训练，生成上下文化的词嵌入，旨在准确重建输入句子，并将语义相似的句子对拉近。

为了利用句子变换器在语义编码中的潜力，你需要首先选择一个合适的模型架构，如 BERT、RoBERTa 或 XLNet。确定模型后，我们将把文档语料库输入其中，为每个句子生成固定长度的语义向量。这些向量是句子核心主题和话题的紧凑数值表示。

以两个句子为例：“狗追逐猫”和“猫追逐狗”。通过句子变换器处理后，它们的语义向量将紧密相关，即使词序不同，因为基本意义相似。另一方面，像“天空是蓝色的”这样的句子会产生更远的向量，因为其含义不同。

使用句子变换器对整个语料库进行编码，我们得到一组语义向量，这些向量概括了文档的总体含义。为了使这种变换后的表示准备好进行高效检索，我们使用 FAISS 对其进行索引。敬请关注，我们将在下一节中深入探讨这个话题。

# FAISS 用于高效索引

FAISS 支持多种索引结构，以优化不同的使用场景。它是一个设计用于在大量向量中快速找到与给定查询向量最接近的匹配项的库。

+   倒排文件（IVF）：对相似向量的簇进行索引。适用于中维向量。

+   产品量化（PQ）：将向量编码到量化子空间中。适用于高维向量。

+   基于簇的策略：将向量组织成分层的簇集以进行多级搜索。适用于非常大的数据集。

要使用 FAISS 进行语义搜索，我们首先加载我们的向量数据集（来自句子变换器编码的语义向量）并构建 FAISS 索引。我们选择的具体索引结构取决于语义向量的维度和期望的效率等因素。然后，我们通过将语义向量传递到 FAISS 索引中对其进行索引，FAISS 将高效地组织它们以实现快速检索。

对于搜索，我们将新的句子编码成一个语义向量查询并将其传递给 FAISS 索引。FAISS 将检索最接近的语义向量并返回最相似的句子。与线性搜索相比，后者会将查询向量与每个索引向量进行比对，FAISS 能够提供更快的检索时间，通常与索引向量的数量呈对数级缩放。此外，这些索引具有高度的内存效率，因为它们压缩了原始密集向量。

## 倒排文件索引

FAISS 中的倒排文件（IVF）索引将相似的向量聚集到“倒排文件”中，适用于中维向量（例如，100–1000 维）。每个倒排文件包含相互接近的向量子集。在搜索时，FAISS 仅搜索与查询向量最接近的倒排文件，而不是搜索所有向量，即使有许多向量，也能实现高效搜索。

要构建 IVF 索引，我们需要指定倒排文件（簇）的数量以及每个倒排文件的最大向量数量。然后，FAISS 将每个向量分配给最近的倒排文件，直到没有倒排文件超过最大数量。倒排文件包含代表性点，这些点总结了它们内部的向量。在查询时，FAISS 计算查询向量与每个倒排文件代表性点之间的距离，并仅搜索与查询向量最接近的倒排文件以找到最匹配的向量。

例如，如果我们有 1024 维的图像特征向量并且想要在 100 万个向量中进行快速搜索，我们可以创建一个包含 1024 个倒排文件（簇）的 IVF 索引，每个倒排文件最多包含 1000 个向量。在这种方法中，FAISS 只会搜索与查询最接近的倒排文件，从而比线性搜索更快。

# 汇总所有内容

在本节中，我们将使用 FAISS 和 Sentence Transformers 构建一个可扩展的语义搜索引擎。我们将展示如何评估这种方法的性能基准，并讨论进一步的改进和应用。

## 可扩展的语义搜索引擎

为了构建一个可扩展的语义搜索引擎，我们首先初始化`ScalableSemanticSearch`类。该类负责使用 Sentence Transformers 对句子进行编码，并使用 FAISS 对其进行索引，以实现高效搜索。它还提供了保存和加载索引、测量时间和内存使用的实用方法。

```py
semantic_search = ScalableSemanticSearch(device="cuda")
```

接下来，我们使用编码方法对大量文档进行编码，该方法返回一个语义向量的 numpy 数组。该方法还创建了一个索引和句子之间的映射，这在检索前几个结果时会很有用。

```py
embeddings = semantic_search.encode(corpus)
```

现在，我们使用 build_index 方法构建 FAISS 索引，该方法以嵌入向量作为输入。该方法根据嵌入中的数据点数量创建 IndexIVFPQ 或 IndexFlatL2 索引。

```py
semantic_search.build_index(embeddings)
```

## 基于数据集大小选择索引方法

我们定义了两种索引方法：L2 距离的精确搜索和产品量化与 L2 距离的近似搜索。我们还将讨论选择第一种方法用于较小数据集（少于 1500 个文档）和第二种方法用于较大数据集的原因。

**1\. L2 距离的精确搜索**

L2 距离的精确搜索是一种精确搜索方法，它计算查询向量与数据集中每个向量之间的 L2（欧几里得）距离。该方法可以保证找到精确的最近邻，但对于大型数据集可能比较慢，因为它执行线性扫描。

*使用案例：* 这种方法适用于需要精确最近邻的小型数据集，并且计算成本不是问题。

**2\. 产品量化与 L2 距离的近似搜索**

近似搜索与产品量化和 L2 距离是一种近似最近邻搜索方法，它结合了倒排文件结构、产品量化和 L2 距离，以高效地在大数据集中搜索相似向量。该方法首先使用 k-means（faiss.IndexFlatL2 作为量化器）对数据集进行聚类，然后应用产品量化来压缩残差向量。这种方法比起蛮力方法使用更少的内存，从而实现更快的搜索速度。

*使用案例：* 这种方法适用于不严格要求精确最近邻的大型数据集，主要关注搜索速度和内存效率。

**选择不同方法的原因依据数据集大小**

对于包含少于 1500 个文档的数据集，我们设置了 L2 距离的精确搜索方法，因为在这种情况下计算成本不是一个重大问题。此外，这种方法可以保证找到最近邻，这对于较小的数据集是可取的。

对于较大的数据集，我们倾向于使用近似搜索与产品量化和 L2 距离方法，因为它比精确搜索方法更高效，消耗的内存也更少。当优先考虑搜索速度和内存效率而非找到精确的最近邻时，近似搜索方法更适合大数据集。

## 搜索过程

在构建索引后，我们可以通过提供输入查询和返回的结果数量来执行语义搜索。`search` 方法计算输入句子与已索引嵌入之间的余弦相似度，并返回最匹配句子的索引和分数。

```py
query = "What is the meaning of life?"
top = 5
top_indices, top_scores = semantic_search.search(query, top)
```

最后，我们可以使用 `get_top_sentences` 方法检索最顶级的句子，该方法接受索引到句子的映射和顶级索引作为输入，并返回顶级句子的列表。

```py
top_sentences = ScalableSemanticSearch.get_top_sentences(semantic_search.hashmap_index_sentence, top_indices)
```

## 完整模型

我们模型的完整类如下：

```py
class ScalableSemanticSearch:
    """Vector similarity using product quantization with sentence transformers embeddings and cosine similarity."""

    def __init__(self, device="cpu"):
        self.device = device
        self.model = SentenceTransformer(
            "sentence-transformers/all-mpnet-base-v2", device=self.device
        )
        self.dimension = self.model.get_sentence_embedding_dimension()
        self.quantizer = None
        self.index = None
        self.hashmap_index_sentence = None

        log_directory = "log"
        if not os.path.exists(log_directory):
            os.makedirs(log_directory)
        log_file_path = os.path.join(log_directory, "scalable_semantic_search.log")

        logging.basicConfig(
            filename=log_file_path,
            level=logging.INFO,
            format="%(asctime)s %(levelname)s: %(message)s",
        )
        logging.info("ScalableSemanticSearch initialized with device: %s", self.device)

    @staticmethod
    def calculate_clusters(n_data_points: int) -> int:
        return max(2, min(n_data_points, int(np.sqrt(n_data_points))))

    def encode(self, data: List[str]) -> np.ndarray:
        """Encode input data using sentence transformer model.

        Args:
            data: List of input sentences.

        Returns:
            Numpy array of encoded sentences.
        """
        embeddings = self.model.encode(data)
        self.hashmap_index_sentence = self.index_to_sentence_map(data)
        return embeddings.astype("float32")

    def build_index(self, embeddings: np.ndarray) -> None:
        """Build the index for FAISS search.

        Args:
            embeddings: Numpy array of encoded sentences.
        """
        n_data_points = len(embeddings)
        if (
            n_data_points >= 1500
        ):  # Adjust this value based on the minimum number of data points required for IndexIVFPQ
            self.quantizer = faiss.IndexFlatL2(self.dimension)
            n_clusters = self.calculate_clusters(n_data_points)
            self.index = faiss.IndexIVFPQ(
                self.quantizer, self.dimension, n_clusters, 8, 4
            )
            logging.info("IndexIVFPQ created with %d clusters", n_clusters)
        else:
            self.index = faiss.IndexFlatL2(self.dimension)
            logging.info("IndexFlatL2 created")

        if isinstance(self.index, faiss.IndexIVFPQ):
            self.index.train(embeddings)
        self.index.add(embeddings)
        logging.info("Index built on device: %s", self.device)

    @staticmethod
    def index_to_sentence_map(data: List[str]) -> Dict[int, str]:
        """Create a mapping between index and sentence.

        Args:
            data: List of sentences.

        Returns:
            Dictionary mapping index to the corresponding sentence.
        """
        return {index: sentence for index, sentence in enumerate(data)}

    @staticmethod
    def get_top_sentences(
        index_map: Dict[int, str], top_indices: np.ndarray
    ) -> List[str]:
        """Get the top sentences based on the indices.

        Args:
            index_map: Dictionary mapping index to the corresponding sentence.
            top_indices: Numpy array of top indices.

        Returns:
            List of top sentences.
        """
        return [index_map[i] for i in top_indices]

    def search(self, input_sentence: str, top: int) -> Tuple[np.ndarray, np.ndarray]:
        """Compute cosine similarity between an input sentence and a collection of sentence embeddings.

        Args:
            input_sentence: The input sentence to compute similarity against.
            top: The number of results to return.

        Returns:
            A tuple containing two numpy arrays. The first array contains the cosine similarities between the input
            sentence and the embeddings, ordered in descending order. The second array contains the indices of the
            corresponding embeddings in the original array, also ordered by descending similarity.
        """
        vectorized_input = self.model.encode(
            [input_sentence], device=self.device
        ).astype("float32")
        D, I = self.index.search(vectorized_input, top)
        return I[0], 1 - D[0]

    def save_index(self, file_path: str) -> None:
        """Save the FAISS index to disk.

        Args:
            file_path: The path where the index will be saved.
        """
        if hasattr(self, "index"):
            faiss.write_index(self.index, file_path)
        else:
            raise AttributeError(
                "The index has not been built yet. Build the index using `build_index` method first."
            )

    def load_index(self, file_path: str) -> None:
        """Load a previously saved FAISS index from disk.

        Args:
            file_path: The path where the index is stored.
        """
        if os.path.exists(file_path):
            self.index = faiss.read_index(file_path)
        else:
            raise FileNotFoundError(f"The specified file '{file_path}' does not exist.")

    @staticmethod
    def measure_time(func: Callable, *args, **kwargs) -> Tuple[float, Any]:
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        elapsed_time = end_time - start_time
        return elapsed_time, result

    @staticmethod
    def measure_memory_usage() -> float:
        process = psutil.Process(os.getpid())
        ram = process.memory_info().rss
        return ram / (1024**2)

    def timed_train(self, data: List[str]) -> Tuple[float, float]:
        start_time = time.time()
        embeddings = self.encode(data)
        self.build_index(embeddings)
        end_time = time.time()
        elapsed_time = end_time - start_time
        memory_usage = self.measure_memory_usage()
        logging.info(
            "Training time: %.2f seconds on device: %s", elapsed_time, self.device
        )
        logging.info("Training memory usage: %.2f MB", memory_usage)
        return elapsed_time, memory_usage

    def timed_infer(self, query: str, top: int) -> Tuple[float, float]:
        start_time = time.time()
        _, _ = self.search(query, top)
        end_time = time.time()
        elapsed_time = end_time - start_time
        memory_usage = self.measure_memory_usage()
        logging.info(
            "Inference time: %.2f seconds on device: %s", elapsed_time, self.device
        )
        logging.info("Inference memory usage: %.2f MB", memory_usage)
        return elapsed_time, memory_usage

    def timed_load_index(self, file_path: str) -> float:
        start_time = time.time()
        self.load_index(file_path)
        end_time = time.time()
        elapsed_time = end_time - start_time
        logging.info(
            "Index loading time: %.2f seconds on device: %s", elapsed_time, self.device
        )
        return elapsed_time
```

# 端到端演示

本节将提供使用 SemanticSearchDemo 类的可扩展语义搜索引擎的端到端演示。

上述代码中的主函数。目标是理解不同概念和组件如何结合创建一个实用的应用程序。

**初始化 SemanticSearchDemo 类**：要初始化 SemanticSearchDemo 类，需要提供数据集路径、ScalableSemanticSearch 模型、可选的索引路径和可选的子集大小。这种灵活性使得可以使用不同的数据集、模型和子集大小。

```py
demo = SemanticSearchDemo(
    dataset_path, model, index_path=index_path, subset_size=subset_size
)
```

**加载数据**：`load_data` 函数主动读取和处理文件中的数据，然后返回一个句子列表。系统使用这些数据来训练语义搜索模型。

```py
sentences = demo.load_data(file_name)
subset_sentences = sentences[:subset_size]
```

**训练模型**：`train` 函数在数据集上训练语义搜索模型，并返回训练过程的消耗时间和内存使用情况。

```py
training_time, training_memory_usage = demo.train(subset_sentences)
```

**进行推理**：`infer` 函数接受一个查询、一组待搜索的句子和返回的结果数量。它对模型进行推理，并返回最匹配的句子、推理过程的消耗时间和内存使用情况。

```py
top_sentences, inference_time, inference_memory_usage = demo.infer(
    query, subset_sentences, top=3
)
```

演示的完整类如下：

```py
class SemanticSearchDemo:
    """A demo class for semantic search using the ScalableSemanticSearch model."""

    def __init__(
        self,
        dataset_path: str,
        model: ScalableSemanticSearch,
        index_path: Optional[str] = None,
        subset_size: Optional[int] = None,
    ):
        self.dataset_path = dataset_path
        self.model = model
        self.index_path = index_path
        self.subset_size = subset_size

        if self.index_path is not None and os.path.exists(self.index_path):
            self.loading_time = self.model.timed_load_index(self.index_path)
        else:
            self.train()

    def load_data(self, file_name: str) -> List[str]:
        """Load data from a file.

        Args:
            file_name: The name of the file containing the data.

        Returns:
            A list of sentences loaded from the file.
        """
        with open(f"{self.dataset_path}/{file_name}", "r") as f:
            reader = csv.reader(f, delimiter="\t")
            next(reader)  # Skip the header
            sentences = [row[3] for row in reader]  # Extract the sentences
        return sentences

    def train(self, data: Optional[List[str]] = None) -> Tuple[float, float]:
        """Train the semantic search model and measure time and memory usage.

        Args:
            data: A list of sentences to train the model on. If not provided, the data is loaded from file.

        Returns:
            A tuple containing the elapsed time in seconds and the memory usage in megabytes.
        """
        if data is None:
            file_name = "GenericsKB-Best.tsv"
            data = self.load_data(file_name)

            if self.subset_size is not None:
                data = data[: self.subset_size]

        elapsed_time, memory_usage = self.model.timed_train(data)

        if self.index_path is not None:
            self.model.save_index(self.index_path)

        return elapsed_time, memory_usage

    def infer(
        self, query: str, data: List[str], top: int
    ) -> Tuple[List[str], float, float]:
        """Perform inference on the semantic search model and measure time and memory usage.

        Args:
            query: The input query to search for.
            data: A list of sentences to search in.
            top: The number of top results to return.

        Returns:
            A tuple containing the list of top sentences that match the input query, elapsed time in seconds, and memory usage in megabytes.
        """
        elapsed_time, memory_usage = self.model.timed_infer(query, top)
        top_indices, _ = self.model.search(query, top)
        index_map = self.model.index_to_sentence_map(data)
        top_sentences = self.model.get_top_sentences(index_map, top_indices)

        return top_sentences, elapsed_time, memory_usage
```

# 我们可扩展语义搜索引擎的性能评估

为了评估我们可扩展的语义搜索引擎的性能，我们可以测量各种操作的时间和内存使用情况，如训练、推理和加载索引。`ScalableSemanticSearch` 类提供 `timed_train`、`timed_infer` 和 `timed_load_index` 方法来测量这些基准。

```py
train_time, train_memory = semantic_search.timed_train(corpus)
infer_time, infer_memory = semantic_search.timed_infer(query, top)
```

关于执行时间和内存使用情况的训练和推理性能图表如下。我们将讨论和解释这些结果，基于我们使用的语料库大小选择的算法。

![](img/7f0a97496eea4a7bc7ca428e5ff5e322.png)

图 2：不同数据集大小的训练时间和内存使用情况

![](img/190e671ff8caa676573355fb3f30353d.png)

图 3：不同数据集大小的推断时间和内存使用情况

## 使用 L2 距离的精确搜索

使用 L2 距离的精确搜索是一种穷举搜索方法，它执行线性扫描以找到最近邻。

**理论复杂度**

+   时间复杂度：O(n) — 因为它需要将查询向量与数据集中每个向量进行比较。

+   内存复杂度：O(n) — 它存储数据集中的所有向量。

**观察到的复杂度**

+   从上述图表中可以观察到，对于少于 1500 个文档，训练时间和内存使用量都随着文档数量线性增加，这与预期的理论复杂度相符。

## 使用产品量化和 L2 距离的近似搜索

使用产品量化和 L2 距离的近似搜索方法是一种近似最近邻搜索方法，它采用产品量化和倒排文件结构来提高效率。聚类数量（k）是此方法中的一个重要因素，计算公式为：max(2, min(n_data_points, int(np.sqrt(n_data_points))))。

简单来说，这个公式确保了：

1.  至少有 2 个聚类，提供了最低水平的分区。

1.  聚类的数量不会超过数据点的数量。

1.  作为启发式方法，使用数据点数量的平方根来平衡搜索精度和计算效率。

**理论复杂度**

+   时间复杂度（训练）：O(n * k) — k-means 聚类算法在训练阶段的复杂度。

+   内存复杂度（训练）：O(n + k) — 它存储聚类的质心和残差码。

+   时间复杂度（推断）：O(k + m) — 其中 m 是要搜索的最近聚类的数量。由于层次结构和近似，它比线性搜索更快。

+   内存复杂度（推断）：O(n + k) — 需要存储倒排文件和质心。

**观察到的复杂度**

从上述图表中可以观察到，对于超过 1500 个文档：

+   训练时间复杂度：增长速度快于线性，这与预期的理论复杂度 O(n * k) 相符，因为 k 随着数据点（n）的增加而增长。

+   训练内存复杂度：内存使用量随着文档数量的增加呈非线性增长，这与预期的理论复杂度 O(n + k) 相符。

+   推断时间复杂度：执行时间几乎保持不变，与期望的理论复杂度 O(k + m) 一致，因为 m 通常远小于 n。

+   推断内存复杂度：内存使用量随着文档数量线性增加，这与预期的理论复杂度 O(n + k) 相符。

我们还可以通过将搜索引擎的前几个结果与手动整理的真实结果集进行比较，来评估搜索引擎的准确性和召回率。我们可以通过迭代各种查询并比较结果来计算整个数据集的平均准确性和召回率。

# 结论

展示的方法突显了使用 FAISS 和 Sentence Transformers 进行语义搜索的可扩展性，同时揭示了改进机会。例如，集成先进的 Transformer 模型来对句子进行编码，或测试替代的 FAISS 配置，可能会加快搜索过程。此外，研究最先进的模型，如 GPT-4 或 BERT 变体，可能会提高语义搜索任务的性能和准确性。

可扩展的语义搜索引擎的几个潜在应用包括：

+   在广泛的知识库中检索文档

+   在自动化系统中回答问题

+   提供个性化推荐

+   生成聊天机器人回应

利用 FAISS 和 Sentence Transformers，我们开发了一种可扩展的语义搜索引擎，能够高效处理数十亿份文档并提供准确的搜索结果。这种创新的方法可能会显著影响语义搜索的未来以及其在各个行业和应用中的影响。

随着数字数据的增长，对高效且准确的语义搜索引擎的需求变得越来越重要。基于 FAISS 和 Sentence Transformers，可扩展的语义搜索引擎为克服这些挑战奠定了坚实的基础，并革新了我们搜索和获取相关信息的方式。

未来的发展涉及整合更先进的自然语言处理和机器学习技术，以增强搜索引擎的能力。这些改进可能包括用于更好理解上下文、意图以及查询词和短语之间关系的无监督学习方法，以及处理语言使用中的歧义和变体的技术。

# 关于我

连续创业者和 AI 领域的领军人物。我为企业开发 AI 产品，并投资于 AI 相关的初创公司。

[创始人 @ ZAAI](http://zaai.ai) | [LinkedIn](https://www.linkedin.com/in/luisbrasroque/) | [X/Twitter](https://x.com/luisbrasroque)
