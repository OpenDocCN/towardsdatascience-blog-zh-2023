# 在GCP上使用tensorflow-serving运行稳定扩散集群（第2部分）

> 原文：[https://towardsdatascience.com/running-a-stable-diffusion-cluster-on-gcp-with-tensorflow-serving-part-2-c421ecb7472a?source=collection_archive---------9-----------------------#2023-03-14](https://towardsdatascience.com/running-a-stable-diffusion-cluster-on-gcp-with-tensorflow-serving-part-2-c421ecb7472a?source=collection_archive---------9-----------------------#2023-03-14)

## 创建工件并在集群上部署模型

[](https://thushv89.medium.com/?source=post_page-----c421ecb7472a--------------------------------)[![Thushan Ganegedara](../Images/3fabfa37132f7d3a9e7679c3b8d7e061.png)](https://thushv89.medium.com/?source=post_page-----c421ecb7472a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c421ecb7472a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c421ecb7472a--------------------------------) [Thushan Ganegedara](https://thushv89.medium.com/?source=post_page-----c421ecb7472a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6f0b045d5681&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frunning-a-stable-diffusion-cluster-on-gcp-with-tensorflow-serving-part-2-c421ecb7472a&user=Thushan+Ganegedara&userId=6f0b045d5681&source=post_page-6f0b045d5681----c421ecb7472a---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c421ecb7472a--------------------------------) ·14分钟阅读·2023年3月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc421ecb7472a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frunning-a-stable-diffusion-cluster-on-gcp-with-tensorflow-serving-part-2-c421ecb7472a&user=Thushan+Ganegedara&userId=6f0b045d5681&source=-----c421ecb7472a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc421ecb7472a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frunning-a-stable-diffusion-cluster-on-gcp-with-tensorflow-serving-part-2-c421ecb7472a&source=-----c421ecb7472a---------------------bookmark_footer-----------)

在 [第1部分](/running-a-stable-diffusion-cluster-on-gcp-with-tensorflow-serving-part-1-4f7a8e2f66df) 中，我们学习了如何使用`terraform`方便地设置和管理基础设施。在这一部分中，我们将继续我们的旅程，将运行中的稳定扩散模型部署到提供的集群上。

> **注意**：即使你是免费用户，也可以完整地跟随本教程（只要你还有一些免费层积分）。
> 
> 除非另有说明，所有图片均由作者提供

Github: [https://github.com/thushv89/tf-serving-gke](https://github.com/thushv89/tf-serving-gke)

让我们看看最终结果会是什么。

![](../Images/4f57aa3e47f1793b54600176ea1a7c53.png)

部署的稳定扩散模型生成的一些图像。

# 准备模型工件

## 稳定扩散到底是什么？

构建[稳定扩散模型](https://jalammar.github.io/illustrated-stable-diffusion/)有五个主要组件：

+   分词器——将给定字符串分词为令牌列表（数值ID）。

+   文本编码器——接受分词后的文本并生成文本嵌入。

+   扩散模型——接受文本嵌入和潜在图像（最初是噪声）作为输入，并逐步优化潜在图像以编码越来越多有用的信息（视觉上令人愉悦）。

+   解码器——接受最终的潜在图像并生成实际图像。

+   图像编码器（用于修复功能——在本练习中我们将忽略这一点）。

稳定扩散（扩散模型）的核心突破性理念是，

> 如果你在多次步骤中逐渐向图像添加一点噪声，最后你会得到一个包含噪声的图像。通过反转这个过程，你可以得到一个输入（噪声）和一个目标（原始图像）。然后训练一个模型从噪声中预测原始图像。

上述所有组件协同工作以实现这一理念。

## 存储稳定扩散模型

> 代码：[https://github.com/thushv89/tf-serving-gke/blob/master/notebooks/savedmodel_stable_diffusion.ipynb](https://github.com/thushv89/tf-serving-gke/blob/master/notebooks/savedmodel_stable_diffusion.ipynb)

为了构建[稳定扩散模型](https://jalammar.github.io/illustrated-stable-diffusion/)，我们将使用`keras_cv`库，该库包括用于图像分类、分割、生成AI等的流行深度学习视觉模型集合。你可以在[这里](https://keras.io/guides/keras_cv/generate_images_with_stable_diffusion/)找到一个教程，讲解如何在`keras_cv`中使用`StableDiffusion`。你可以打开一个笔记本并与模型一起玩以熟悉它。

我们的目标是将`StableDiffusion`模型保存为`SavedModel`格式；这是序列化TensorFlow模型的标准方法。做到这一点的一个关键要求是确保所有使用的操作都是TensorFlow图兼容的。不幸的是，情况并非如此。

+   当前版本的模型使用与TensorFlow图不兼容的分词器，因此需要将其从打包模型中提取出来，并在单独的步骤中使用。

+   当前版本使用`predict_on_batch`来生成图像，但TensorFlow图构建不支持此功能。

## 修正模型

为了修补急切模式的 `StableDiffusion` 模型，我们将创建一个名为 `StableDiffusionNoTokenizer` 的新模型。通过这个新模型，我们将用图形兼容的 `__call__()` 替换所有 `predict_on_batch()` 调用。正如名字所示，我们还将把标记化过程与模型解耦。此外，在 `generate_image()` 函数中，我们将替换，

```py
timesteps = tf.range(1, 1000, 1000 // num_steps)
alphas, alphas_prev = self._get_initial_alphas(timesteps)
progbar = keras.utils.Progbar(len(timesteps))
iteration = 0
for index, timestep in list(enumerate(timesteps))[::-1]:
    latent_prev = latent  # Set aside the previous latent vector
    t_emb = self._get_timestep_embedding(timestep, batch_size)
    unconditional_latent = self.diffusion_model.predict_on_batch(
        [latent, t_emb, unconditional_context]
    )
    latent = self.diffusion_model.predict_on_batch(
        [latent, t_emb, context]
    )
    latent = unconditional_latent + unconditional_guidance_scale * (
        latent - unconditional_latent
    )
    a_t, a_prev = alphas[index], alphas_prev[index]
    pred_x0 = (latent_prev - math.sqrt(1 - a_t) * latent) / math.sqrt(
        a_t
    )
    latent = (
        latent * math.sqrt(1.0 - a_prev) + math.sqrt(a_prev) * pred_x0
    )
    iteration += 1
    progbar.update(iteration)
```

与，

```py
latent = self.diffusion_reverse_loop(
    latent,
    context=context, 
    unconditional_context=unconditional_context, 
    batch_size=batch_size, 
    unconditional_guidance_scale=unconditional_guidance_scale,
    num_steps=num_steps,
)
```

其中，

```py
@tf.function
def diffusion_reverse_loop(self, latent, context, unconditional_context, batch_size, unconditional_guidance_scale, num_steps):

  index = num_steps -1
  cond = tf.math.greater(index, -1)
  timesteps = tf.range(1, 1000, 1000 // num_steps)
  alphas, alphas_prev = self._get_initial_alphas(timesteps)
  iter_partial_fn = functools.partial(
      self._diffusion_reverse_iter, 
      timesteps=timesteps, 
      alphas=alphas, 
      alphas_prev=alphas_prev, 
      context=context, 
      unconditional_context=unconditional_context, 
      batch_size=batch_size, 
      unconditional_guidance_scale=unconditional_guidance_scale, 
      num_steps=num_steps
  )

  latent, index = tf.while_loop(cond=lambda _, i: tf.math.greater(i, -1), body=iter_partial_fn, loop_vars=[latent, index])

  return latent 

@tf.function
def _diffusion_reverse_iter(self, latent_prev, index, timesteps,  alphas, alphas_prev, context, unconditional_context, batch_size, unconditional_guidance_scale, num_steps):

  t_emb = self._get_timestep_embedding(timesteps[index], batch_size)

  combined_latent = self.diffusion_model(
            [
                tf.concat([latent_prev, latent_prev],axis=0), 
                tf.concat([t_emb, t_emb], axis=0), 
                tf.concat([context, unconditional_context], axis=0)
            ], training=False
        )
  latent, unconditional_latent = tf.split(combined_latent, 2, axis=0)
  latent = unconditional_latent + unconditional_guidance_scale * (
        latent - unconditional_latent
  )
  a_t, a_prev = alphas[index], alphas_prev[index]
  pred_x0 = (latent_prev - tf.math.sqrt(1 - a_t) * latent) / tf.math.sqrt(a_t)
  latent = latent * tf.math.sqrt(1.0 - a_prev) + tf.math.sqrt(a_prev) * pred_x0
  index -= 1

  return latent, index
```

我做的两个主要更改是：

+   我使用了 `tf.while_loop` 代替 Python `for` 循环，因为在 TensorFlow 中它的性能更优。

+   将两个独立的 `diffusion_model` 调用合并为一个调用，然后再拆分输出。

还有其他更改，如用 TensorFlow 等效函数替换各种操作（例如 `np.clip()` -> `tf.clip_by_value()`），你可以对比 [原始模型](https://github.com/keras-team/keras-cv/blob/master/keras_cv/models/stable_diffusion/stable_diffusion.py#L43) 和 [此版本](https://github.com/thushv89/tf-serving-gke/blob/master/notebooks/savedmodel_stable_diffusion.ipynb) 来进行比较。

> 在 TensorFlow 的图执行模式下，你可以使用 `tf.print()` 语句以确保代码在执行过程中的有效性。有关 `tf.print()` 的更多信息，请参考附录。

一旦底层模型修复完成，我们可以创建以下模型，该模型可以在图模式下无缝执行。

```py
class StableDiffusionTFModel(tf.keras.models.Model):

  def __init__(self):
    super().__init__()
    self.image_width = self.image_height = 384
    self.model = StableDiffusionNoTokenizer(img_width=self.image_width, img_height=self.image_height, encoded_text_length=None, jit_compile=True)
    # This forces the model download its components
    # self.image_encoder is only required for in-painting - we will ignore this functionality in this excercise
    self.text_encoder = self.model.text_encoder
    self.diffusion_model = self.model.diffusion_model
    self.decoder = self.model.decoder

    self.default_num_steps = tf.constant(40) 
    self.default_batch_size = tf.constant(2)

    # These negative prompt tokens are borrowed from the original stable diffusion model
    self.default_negative_prompt_tokens = tf.constant(
        [
            49406, 8159, 267, 83, 3299, 267, 21101, 8893, 3500, 267, 21101, 
            8893, 4804, 267, 21101, 8893, 1710, 267, 620, 539, 6481, 267, 
            38626, 267, 12598, 943, 267, 4231, 34886, 267, 4231, 7072, 267, 
            4231, 5706, 267, 1518, 15630, 267, 561, 6528, 267, 3417, 268, 
            3272, 267, 1774, 620, 539, 6481, 267, 21977, 267, 2103, 794, 
            267, 2103, 15376, 267, 38013, 267, 4160, 267, 2505, 2110, 267, 
            782, 23257, 49407, 49407, 49407, 49407, 49407, 49407, 49407, 49407, 49407
        ], dtype=tf.int32
    )

  def call(self, inputs):

    encoded_text = self.text_encoder([inputs["tokens"], self.model._get_pos_ids()], training=False)

    images = self.model.generate_image(
        encoded_text, 
        negative_prompt_tokens=inputs.get("negative_prompt_tokens", self.default_negative_prompt_tokens),
        num_steps=inputs.get("num_steps", self.default_num_steps), 
        batch_size=inputs.get("batch_size", self.default_batch_size)
    )
    return images

model = StableDiffusionTFModel()
```

这个模型接受以下输入：

+   `input_tokens`: 输入字符串的标记化表示

+   `negative_prompt_tokens`: 负面提示的标记化表示（有关负面提示的更多信息：这里）

+   `num_steps`: 执行扩散过程的步骤数

+   `batch_size`: 每张图片生成的图片数量

这是这个模型的一个使用示例：

```py
# Tokenizing the prompts
tokenizer = SimpleTokenizer()

def generate_tokens(tokenizer, prompt, MAX_PROMPT_LENGTH):

  inputs = tokenizer.encode(prompt)
  if len(inputs) > MAX_PROMPT_LENGTH:
      raise ValueError(
          f"Prompt is too long (should be <= {MAX_PROMPT_LENGTH} tokens)"
      )
  phrase = tf.concat([inputs, ([49407] * (MAX_PROMPT_LENGTH - len(inputs)))], axis=0)
  return phrase

tokens = generate_tokens(tokenizer, "a ferrari car with wings", MAX_PROMPT_LENGTH)

# Invoking the model
all_images = []
num_steps = 30
tokens = generate_tokens(tokenizer, "a castle in Norway overlooking a glacier, landscape, surrounded by fairies fighting trolls, sunset, high quality", MAX_PROMPT_LENGTH)
neg_tokens = generate_tokens(tokenizer, "ugly, tiling, poorly drawn hands, poorly drawn feet, poorly drawn face, out of frame, mutation, mutated, extra limbs, extra legs, extra arms, disfigured, deformed, cross-eye, body out of frame, blurry, bad art, bad anatomy, blurred, text, watermark, grainy", MAX_PROMPT_LENGTH)
images = model({
    "tokens": tokens, 
    "negative_prompt_tokens": neg_tokens,
    "num_steps": tf.constant(num_steps), 
    "batch_size": tf.constant(1)
})
```

记住，我（即免费用户等级）在这个项目中受到配额的严重限制。

+   完全没有 GPU 配额

+   最多 8 个 N2 CPU（如果选择 N1 CPU，可以达到 12 个）

因此，我不能使用任何 GPU 实例或超过 2 个 `n2-standard-4` 实例。由于稳定扩散模型较慢，因此使用 CPU 实例时我们将面临延迟问题。

下面是不同参数下所需时间的详细信息。测试是在 `n2-standard-8` 机器上，在 [Vertex AI workbench](https://cloud.google.com/vertex-ai-workbench) 上进行的。

+   图像大小（`num_steps = 40`）

    — 512x512 图像：474s

    — 384x384 图像：233s

+   `batch_size` 和 `num_steps`

    — `batch size = 1`：21.6s（`num_steps=5`），67.7s（`num_steps=20`）和 99.5s（`num_steps=30`）

    — `batch size = 2`，55.6s（`num_steps=5`），121.1s（`num_steps=20`）和 180.2s（`num_steps=30`）

    — `batch size=4`，21.6s（`num_steps=5`），67.7s（`num_steps=20`）和 99.5s（`num_steps=30`）

如你所见，增加 `image_size`、`batch_size` 和 `num_steps` 会导致时间消耗增加。因此，在平衡计算成本和图像质量后，我们为部署的模型选择了以下参数。

+   `image_size`: `384x384`

+   `num_steps`: `30`

+   `batch_size`: `1`

模型创建后，将模型上传到创建的 GCS 存储桶中。

```py
!gsutil -m cp -r  ./stable_diffusion_model gs://<project>-bucket/
```

这将是我们用来将模型部署为预测服务的数据源。

让我们再次欣赏一些模型生成的图像，然后继续下一个部分。

![](../Images/f665634a061194fc8674cc778a31ab4e.png)![](../Images/ce241c77d0e034e567fa853ae515e930.png)

部署模型生成的图像

# 部署和提供模型

> 代码: [https://github.com/thushv89/tf-serving-gke/tree/master/infrastrcture](https://github.com/thushv89/tf-serving-gke/tree/master/infrastrcture)

要部署我们的模型并设置预测服务，我们需要 3 个配置：

+   `configmap.yaml` — 这定义了部署过程中所需的各种变量。例如，这将包括在 GCS 上保存模型的位置（即通过环境变量 `MODEL_PATH` 访问）。

+   `deployment.yaml` — [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) 定义了 Pod 的规格（例如 CPU）和应该运行的容器。在这种情况下，我们将运行一个单独的容器，运行 `tensorflow-serving` 来提供位于 `MODEL_PATH` 的模型。

+   `service.yaml` — [Service](https://kubernetes.io/docs/concepts/services-networking/service/) 是我们暴露在 Pod 中运行的 `tensorflow-serving` 应用的机制。例如，我们可以让它通过负载均衡器暴露我们的 Pod。

## 部署

我们首先查看 `deployment` 的规格：

```py
spec:
  replicas: 1
  selector:
    matchLabels:
      app: stable-diffusion
  template:
    metadata:
      labels:
        app: stable-diffusion
    spec:
      containers:
      - name: tf-serving
        image: "tensorflow/serving:2.11.0"
        args:
        - "--model_name=$(MODEL_NAME)"
        - "--model_base_path=$(MODEL_PATH)"
        - "--rest_api_timeout_in_ms=720000"
        envFrom:
        - configMapRef:
            name: tfserving-configs
        imagePullPolicy: IfNotPresent
        readinessProbe:
          httpGet:
            path: "/v1/models/stable-diffusion"
            port: 8501
            scheme: HTTP
          initialDelaySeconds: 30
          periodSeconds: 15
          failureThreshold: 10
        ports:
        - name: http
          containerPort: 8501
          protocol: TCP
        - name: grpc
          containerPort: 8500
          protocol: TCP
        resources:
          requests:
            cpu: "3"
            memory: "12Gi"
```

我们可以做一些有趣的观察：

+   我们在脚本中只声明了一个副本，缩放将在其他地方设置，并通过自动缩放策略进行控制。

+   我们提供一个 `selector`，服务将会在部署中查找它，以确保它在正确的部署上提供服务。

+   我们暴露了两个端口：8501（HTTP 流量）和 8500（GRPC 流量）。

+   我们将为每个容器请求 3 个“CPU 时间”和 12Gi 的内存。

> **注意 1**: 节点通常会运行 Kubernetes 需要的其他 Pods（例如 DNS、监控等）。因此，在规定 Pod 的计算资源时需要考虑这些因素。您可以看到，尽管节点上有 4 个 CPU，我们只请求了 3 个（您也可以请求分数的 CPU 资源 — 例如 3.5）。您可以在 GCP 上查看每个节点的可分配 CPU/内存（GCP 控制台 → 集群 → 节点 → 单击节点）或使用 `kubectl describe nodes`。
> 
> 如果您的节点无法满足您指定的计算资源，Kubernetes 将无法运行 Pods 并抛出错误（例如 PodUnschedulable）。
> 
> **注意 2**：你需要特别注意的一个关键参数是`--rest_api_timeout_in_ms=720000`。处理一个请求大约需要250秒，所以我们这里将超时时间设置为大约**三倍**的时间，以应对并行请求时任何排队的请求。如果你将其设置为过小的值，你的请求将在完成之前超时。

## 定义服务

在这里，我们定义了一个`LoadBalancer`类型的服务，我们将通过GCP负载均衡器暴露`stable-diffusion`应用。在这种方法中，你将获得负载均衡器的IP地址，负载均衡器将把流量路由到到达它的副本。用户将向负载均衡器的IP地址发起请求。

```py
metadata:
  name: stable-diffusion
  namespace: default
  labels:
    app: stable-diffusion
spec:
  type: LoadBalancer
  ports:
  - port: 8500
    protocol: TCP
    name: tf-serving-grpc
  - port: 8501
    protocol: TCP
    name: tf-serving-http
  selector:
    app: stable-diffusion
```

## 自动扩展

我们一直拖延的一个重要话题是：扩展我们的服务。在现实世界中，你可能需要服务数千、数百万甚至数十亿的客户。为了做到这一点，你的服务需要能够根据需求上下扩展集群中的节点/副本数量。幸运的是，GCP提供了多种选项，从完全托管的自动扩展到半托管/完全用户管理的自动扩展。你可以通过[这个视频](https://www.youtube.com/watch?v=cFhch7hozRg)了解更多信息。

在这里，我们将使用水平副本自动扩展器（HPA）。水平副本自动扩展器将根据你提供的一些阈值（例如CPU或内存使用情况）扩展副本的数量。这是一个示例。

```py
kubectl autoscale deployment stable-diffusion --cpu-percent=60 --min=1 --max=2
```

在这里，我们将HPA的最小副本数设置为1，最大副本数设置为2，并要求它在当前副本集的平均CPU超过60%时添加更多副本。

## 应用更改

我们现在已经准备好所有的构建块来启动我们的服务。只需运行以下命令。

```py
 gcloud container clusters get-credentials sd-cluster --zone us-central1-c && \
kubectl apply -f tf-serving/configmap.yaml && \
kubectl apply -f tf-serving/deployment.yaml && \
kubectl autoscale deployment stable-diffusion --cpu-percent=60 --min=1 --max=2 && \
kubectl apply -f tf-serving/service.yaml
```

# 从服务模型中预测

为了进行预测，你只需向正确的URL发起一个POST请求，负载中包含模型的输入。

## 顺序预测

作为第一个示例，我们展示了如何一个接一个地发起一系列请求。

```py
def predict_rest(json_data, url):
    json_response = requests.post(url, data=json_data)
    response = json.loads(json_response.text)
    if "predictions" not in response:
      print(response)
    rest_outputs = np.array(response["predictions"])
    return rest_outputs

url = f"http://{stable_diffusion_service_ip}:8501/v1/models/stable-diffusion:predict"

tokens_list = [
    generate_tokens(tokenizer, "A wine glass made from lego bricks, rainbow colored liquid being poured into it, hyper realistic, high detail", MAX_PROMPT_LENGTH).numpy().tolist(),
    generate_tokens(tokenizer, "A staircase made from color pencils, hyper realistic, high detail", MAX_PROMPT_LENGTH).numpy().tolist(),
    generate_tokens(tokenizer, "A ferrari car in the space astronaut driving it, futuristic, hyper realistic, high detail", MAX_PROMPT_LENGTH).numpy().tolist(),
    generate_tokens(tokenizer, "a dragon covered with weapons fighting an army, fire, explosions, hyper realistic, high detail", MAX_PROMPT_LENGTH).numpy().tolist(),
    generate_tokens(tokenizer, "A sawing girl in a boat, hyper realistic, high detail", MAX_PROMPT_LENGTH).numpy().tolist(),

]
negative_tokens = generate_tokens(tokenizer, "ugly, tiling, poorly drawn hands, poorly drawn feet, poorly drawn face, out of frame, mutation, mutated, extra limbs, extra legs, extra arms, disfigured, deformed, cross-eye, body out of frame, blurry, bad art, bad anatomy, blurred, text, watermark, grainy", MAX_PROMPT_LENGTH).numpy().tolist()

all_images = []
all_data = []
for tokens, negative_tokens in zip(tokens_list, [negative_tokens for _ in range(5)]):
    all_data.append(generate_json_data(tokens, negative_tokens))

all_images = [predict_rest(data, url) for data in all_data]
```

当我运行实验时，这花费了超过1600秒。正如你想象的，这种设置相当低效，无法利用集群的扩展能力。

## 并行预测

你可以使用Python的多处理库来进行并行请求，这更贴近真实用户请求的情况。

```py
def predict_rest(input_data, url):
    json_data, sleep_time = input_data["data"], input_data["sleep_time"]

    # We add a delay to simulate real world user requests
    time.sleep(sleep_time)
    print("Making a request")
    t1 = time.perf_counter()
    json_response = requests.post(url, data=json_data)
    response = json.loads(json_response.text)
    result = np.array([])
    try: 
        result = np.array(response["predictions"])
    except KeyError:
        print(f"Couldn't complete the request {response}")
    finally:
        t2 = time.perf_counter() 
        print(f"It took {t2-t1}s to complete a single request")
        return result

t1 = time.perf_counter()

with concurrent.futures.ThreadPoolExecutor(max_workers=3) as executor:
    all_images_gen = executor.map(
        functools.partial(predict_rest, url=url), 
        [{"data": data, "sleep_time": min(i*20, 60)} for i, data in enumerate(all_data)]
    )
    all_images = [img for img in all_images_gen]

t2 = time.perf_counter()    
print(f"It took {t2-t1}s to complete {n_requests} requests")
```

这运行了900秒。因此，通过将集群扩展到最多2个副本，我们实现了约180%的加速。

> **关于设置超时的说明**
> 
> 在设置并行请求时要小心。如果你一次性发送所有并行请求（因为这只有6个请求），**它们可能会超时**。这是因为创建新节点和初始化新副本需要时间。所以如果所有请求瞬间发出，负载均衡器可能甚至没有时间看到第二个节点，最终会尝试将所有请求服务于单个节点。
> 
> 上述定义的超时时间是从请求接收的时间（即进入`*tensorflow-serving*`队列）开始计算的，而不是从开始处理请求的时间开始计算。因此，如果请求在队列中等待时间过长，也会计入超时。

你可以在 GCP 上监控计算指标，如 CPU 使用率和内存消耗（GCP → Kubernetes Engine → Services & Ingress → 选择你的服务）。

![](../Images/a93688cd809d2f3f3fb3ee6a342fc3c2.png)

顺序请求的使用图（上）并行请求的使用图（下）

# 结论

在这个两部分的教程中，我们，

+   使用terraform（一个IaaS工具）设置基础设施，主要包括一个集群和一个节点池（第1部分）

+   部署了一个模型，并创建了一个预测服务来处理用户请求，使用了一个Stable Diffusion模型（第2部分）

我们设置了这个教程，使得即使是免费用户也能运行。我们设置了一个包含2个节点的集群，并为每个节点创建了1个pod。然后我们进行了顺序和并行预测，发现并行预测带来了约180%的吞吐量提升。

下一步：

+   [模型预热](https://www.tensorflow.org/tfx/serving/serving_config) — `tensorflow-serving` 提供了一种简单的方法来预热模型。你可以解析示例请求，它们将被加载并发送到模型中，实际处理用户请求之前。这将减少初始用户请求的延迟。

+   [动态批处理](https://www.tensorflow.org/tfx/serving/serving_config#batching_configuration) 请求 — 你可以选择动态批处理传入的请求。这将允许模型对一批输入进行预测，而不是对每个输入进行预测。只要有足够的内存，这可能会提供吞吐量的提升，让你在合理的时间范围内处理大量请求。

# 附录

## 在pods中进行调试

当我尝试启动它时，遇到的一个痛苦问题是遇到了以下砖墙。

![](../Images/4e40e615e61954e8f711aaa3d0525c6d.png)

在 Workloads → Deployment 部分显示的错误

当我进入部署中的一个pod时，我得到了一个更合理的（仍然不显眼的）错误。但仍然不足以明确指出到底哪里出了问题。

![](../Images/d6c31012d4b7079d9bf28cfcbf84b2c5.png)

单个pod产生的事件

所以我必须找到一种方法来微观地调查根本原因。为此，我首先登录到相关pod的容器中，

```py
kubectl exec --stdin --tty <container name> -- /bin/bash
```

一旦我进入，就可以利用 Linux 所赖以生存的“[一切皆文件](https://en.wikipedia.org/wiki/Everything_is_a_file)”这一范式。换句话说，你可以简单地访问一个文件以查看进程的输出/错误流。例如，在我的情况下，`tensorflow-serving` 进程的PID是7，因此，`/proc/7/fd/2` 给出了该进程的错误流。

```py
tail -n 10  /proc/7/fd/2
```

在这里，我能够准确地看到为什么这没有启动。这是因为容器没有必要的权限来访问`MODEL_PATH`中指定的GCS桶。

## 使用`tf.print`进行调试

正如你所知，TensorFlow 提供了两种执行风格：命令式和声明式。由于我们使用`__call__()`来调用模型（即`self.model(<inputs>)`），这些调用作为图操作执行。你可能已经知道，图执行因内部图造成的模糊性而难以调试。TensorFlow 提供的一种解决方案是使用`tf.print`语句。

你可以在模型调用中放置`tf.print`语句，这些打印语句会作为操作添加到图中，因此你可以查看执行的张量的值等，这样你可以更好地调试代码，而不是盲目尝试。

确保你的`tf.print`语句打印的输入出现在你希望它被打印的时间之前。如果你添加了独立/虚拟的`tf.print`语句，它们不会被正确地嵌入到图中。这可能会给你一种误导性的感觉，认为某些计算正在非常快速地进行，这是由于图的错误放置所导致的。

## 关于机器类型的说明

对于这个练习，你可以使用两种主要的[机器类型](https://cloud.google.com/compute/docs/machine-resource#recommendations_for_machine_types)；`n1`和`n2`。[N2 实例使用第三代 Xeon 处理器](https://cloud.google.com/blog/products/compute/compute-engine-n2-vms-now-available-with-intel-ice-lake)，这些处理器配备了特殊的指令集（`AVX-512`），以加速诸如矩阵乘法等操作。因此，CPU 密集型 TensorFlow 代码在 n2 机器上运行得比在 n1 上更快。

# 致谢

我想感谢[ML Developer Programs](https://developers.google.com/community/experts)及其团队提供的GCP积分，使这次教程得以成功。
