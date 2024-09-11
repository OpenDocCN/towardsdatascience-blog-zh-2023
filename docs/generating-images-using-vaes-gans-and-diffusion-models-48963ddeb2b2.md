# 使用 VAEs、GANs 和扩散模型生成图像

> 原文：[https://towardsdatascience.com/generating-images-using-vaes-gans-and-diffusion-models-48963ddeb2b2?source=collection_archive---------1-----------------------#2023-05-06](https://towardsdatascience.com/generating-images-using-vaes-gans-and-diffusion-models-48963ddeb2b2?source=collection_archive---------1-----------------------#2023-05-06)

## 了解如何使用 VAEs、DCGANs 和 DDPMs 生成图像

[](https://medium.com/@jcheigh?source=post_page-----48963ddeb2b2--------------------------------)[![贾斯廷·切赫](../Images/0bafdd733fe57267074a937b4777418c.png)](https://medium.com/@jcheigh?source=post_page-----48963ddeb2b2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----48963ddeb2b2--------------------------------)[![面向数据科学](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----48963ddeb2b2--------------------------------) [贾斯廷·切赫](https://medium.com/@jcheigh?source=post_page-----48963ddeb2b2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F24cd781f1018&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerating-images-using-vaes-gans-and-diffusion-models-48963ddeb2b2&user=Justin+Cheigh&userId=24cd781f1018&source=post_page-24cd781f1018----48963ddeb2b2---------------------post_header-----------) 发表在 [面向数据科学](https://towardsdatascience.com/?source=post_page-----48963ddeb2b2--------------------------------) · 21 分钟阅读 · 2023 年 5 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F48963ddeb2b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerating-images-using-vaes-gans-and-diffusion-models-48963ddeb2b2&user=Justin+Cheigh&userId=24cd781f1018&source=-----48963ddeb2b2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F48963ddeb2b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerating-images-using-vaes-gans-and-diffusion-models-48963ddeb2b2&source=-----48963ddeb2b2---------------------bookmark_footer-----------)![](../Images/1cf4312fbcc564f4c6172c34651fe21e.png)

图片由 [CatBird AI](https://www.catbird.ai/) 提供，图片提示由 [ChatGPT](https://chat.openai.com/) 提供，ChatGPT 由贾斯廷·切赫提供提示

## 引言：

我们目前正处于生成式 AI 繁荣期。2022 年 11 月，Open AI 的生成语言模型 ChatGPT 震撼了世界，而在 2023 年 3 月，我们甚至迎来了 GPT-4！

尽管这些LLM的未来非常令人兴奋，但今天我们将专注于图像生成。随着扩散模型的兴起，图像生成取得了巨大的飞跃。现在我们被像DALL-E 2、Stable Diffusion和Midjourney这样的模型包围。例如，查看上面的图像。为了展示这些LLM的强大，我给ChatGPT一个非常简单的提示，然后将其输入到免费的[CatbirdAI](https://www.catbird.ai/)中。CatbirdAI使用了不同的模型，包括Openjourney、Dreamlike Diffusion等：

在本文中，[**Daisuke Yamada**](https://www.linkedin.com/in/daisukeyamada1999/)（我的合著者）和我将研究扩散模型。我们将使用3种不同的模型，并使用每种模型生成MNIST手写数字风格的图像。第一个模型将是传统的变分自编码器（VAE）。然后我们将讨论GAN，并实现一个[深度卷积GAN (DCGAN)](https://arxiv.org/pdf/1511.06434v2.pdf)。最后，我们将转向扩散模型，并实现论文中描述的模型[去噪扩散概率模型](https://arxiv.org/pdf/2006.11239.pdf)。对于每个模型，我们将讨论其背后的理论，然后在Tensorflow/Keras中实现。

快速说明一下符号。我们会尽量使用下标如***x₀***，但有时可能需要使用***x_T***来表示下标。

让我们简要讨论一下先决条件。熟悉深度学习和使用Tensorflow/Keras是很重要的。此外，你应该对VAE和GAN有一定了解；我们会讲解主要理论，但有经验会更有帮助。如果你从未见过这些模型，可以查看这些有用的资源：[MIT S6.191 讲座](https://www.youtube.com/watch?v=3G5hWM6jqPk&t=285s)，[斯坦福生成模型讲座](https://www.youtube.com/watch?v=5WoItGTWV54&t=516s)，[VAE 博客](/understanding-variational-autoencoders-vaes-f70510919f73)。最后，无需了解DCGANs或扩散模型。很好！我们开始吧。

## 生成模型三难问题：

作为一种无监督过程，生成型AI通常缺乏明确的指标来跟踪进展。但在我们讨论任何评估生成模型的方法之前，我们需要理解生成型AI实际上试图实现什么！生成型AI的目标是从某些未知**复杂数据分布**（例如，人脸的分布）中获取训练样本，并学习一个能够“捕捉这种分布”的模型。那么，评估这样的模型时相关的因素是什么呢？

我们当然希望样本质量高，即生成的数据应该与实际数据分布相比，真实且准确。直观上，我们可以通过查看输出结果来主观评估这一点。这在一个称为[HYPE（Human eYe Perceptual Evaluation）](https://arxiv.org/pdf/1904.01121.pdf)的基准中得到了形式化和标准化。虽然还有其他的[定量方法](https://deepgenerativemodels.github.io/assets/slides/cs236_lecture15.pdf)，但今天我们将仅依靠我们自己的主观评估。

还有一个重要因素是快速采样（即生成速度或可扩展性）**。** 我们将关注的一个特定方面是生成新样本所需的网络传递次数。例如，我们将看到GAN只需对生成器网络进行一次传递即可将噪声转换为（希望是）现实的数据样本，而DDPM则需要顺序生成，这使得速度大大降低。

最终一个重要的质量标准称为模式覆盖。我们不仅仅希望学习未知分布的特定部分，而是希望捕捉整个分布以确保样本的多样性。例如，我们不希望一个仅输出0和1图像的模型，而是希望输出所有可能的数字类别。

这三个重要因素（样本质量、采样速度和模式覆盖）都涵盖在**“**[**生成模型三难困境**](https://arxiv.org/pdf/2112.07804.pdf)**”**中。

![](../Images/b262f9578a1ad0179b8afa78c1c545ba.png)

图片由 Daisuke Yamada 创建，灵感来源于[DDGANs 论文中的图1](https://arxiv.org/pdf/2112.07804.pdf)

现在我们了解了如何比较和对比这些模型，让我们深入研究VAE吧！

## 变分自编码器：

你将遇到的第一个生成模型是变分自编码器（VAE）。由于VAE只是具有概率性变换的传统自编码器，我们来回顾一下自编码器。

自编码器是学习将数据压缩成某种潜在表示的降维模型：

![](../Images/f6e569a1baedf817f06b3bdeb69241e3.png)

图片由 Justin Cheigh 创建

编码器将输入压缩为称为瓶颈的潜在表示，然后解码器重建输入。解码器重建输入意味着我们可以用输入/输出之间的L2损失进行训练。

自编码器不能用于图像生成，因为它们会过拟合，导致稀疏的潜在空间是不连续且断开的（不可正则化的）。VAE通过将输入***x***编码为潜在空间上的一个分布来解决这个问题：

输入***x***传递到编码器***E.*** 输出***E(x)***是均值向量和标准差向量，这些向量参数化了分布***P(z | x)***。常见的选择是多变量标准高斯。从这里我们采样***z ~ P(z | x)***，最终解码器尝试从***z***重建***x***（就像自编码器一样）。

注意这个采样过程是非可微的，因此我们需要做一些改变以允许反向传播实现。为此，我们使用重参数化技巧，即首先采样***ϵ ~ N(0,1)***，然后将采样移动到输入层。接着，我们可以进行固定的采样步骤：***z* = *μ* + *σ* ⊙ *ϵ.*** 注意我们获得了相同的采样，但现在我们有了一个清晰的路径来反向传播误差，因为唯一的随机节点是输入！

记住，自编码器的训练是L2损失，它构成了重建项。对于VAE，我们还添加了一个正则化项，用于使潜在空间“表现良好”：

在这里，第一个项是重建项，而第二个项是正则化项。具体来说，我们使用的是[Kullback-Leibler (KL) 散度](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence)，它衡量了学习到的潜在空间分布与先验分布之间的相似性。这有助于防止过拟合。

很好！我们已经回顾了VAE的理论和直觉，现在将讨论实现细节。在导入相关库后，我们定义了一些超参数值：

```py
latent_dim = 2 # dimension of latent space

epochs = 200
batch_size = 32
learning_rate = 1e-4
```

下载MNIST数据集并进行一些基本预处理后，我们定义了我们的损失函数和网络：

```py
def get_default_loss(model, x):
    with tf.device(device):
        mean, logvar, z = model.encoder(x)
        xhat = model.decoder(z)
        rl = tf.reduce_mean(keras.losses.binary_crossentropy(x, xhat))*28*28
        kl = tf.reduce_mean(1+logvar-tf.square(mean)-tf.exp(logvar)) * -0.5
        return rl + kl

'''Sampling layer'''
class Sampling(Layer):

    def call(self, prob):
        # uses reparameterization trick
        mean, logvar = tf.split(prob, num_or_size_splits=2, axis=1)
        e = random.normal(shape=(tf.shape(mean)[0], tf.shape(mean)[1]))
        z = mean + e * tf.exp(logvar * 0.5)
        return mean, logvar, z

'''Basic Convolutional VAE'''
class VAE(Model):

    def __init__(self, latent_dim, **kwargs):
        super(VAE, self).__init__(**kwargs)
        self.latent_dim = latent_dim
        self.encoder = self.get_encoder()
        self.decoder = self.get_decoder()

    '''encoder + reparametrization (i.e., sampling) layer'''
    def get_encoder(self):
        # encoder 
        input_x = Input(shape=(28,28,1))
        x = Conv2D(filters=64, kernel_size=3, strides=(2,2), activation='relu')(input_x)
        x = Conv2D(filters=64, kernel_size=3, strides=(2,2), activation='relu')(x)
        x = Flatten()(x)
        x = Dense(self.latent_dim * 2)(x)
        # sampling 
        (mean, logvar, z) = Sampling()(x)
        return Model(input_x, [mean, logvar, z], name="encoder")

    '''decoder'''
    def get_decoder(self):
        input_z = Input(shape=(self.latent_dim,))
        z = Dense(7*7*64, activation="relu")(input_z)
        z = Reshape((7, 7, 64))(z)
        z = Conv2DTranspose(filters=64, kernel_size=3, strides=2, padding='same', activation='relu')(z)
        z = Conv2DTranspose(filters=64, kernel_size=3, strides=2, padding='same', activation='relu')(z)
        xhat = Conv2DTranspose(filters=1, kernel_size=3, strides=1, padding='same', activation='sigmoid')(z)
        return  Model(input_z, xhat, name="decoder")

    '''train'''
    def train_step(self, x):
        with tf.device(device):
            x = x[0] if isinstance(x, tuple) else x
            with tf.GradientTape() as tape:
                loss = get_default_loss(self, x)
            gradient = tape.gradient(loss, self.trainable_weights)
            self.optimizer.apply_gradients(zip(gradient, self.trainable_weights))
            return {"loss": loss}

    def call(self, inputs):
       pass

vae = VAE(latent_dim=latent_dim)
vae.compile(optimizer=Adam(learning_rate=learning_rate))
vae.build(input_shape=(28,28,1))
vae.summary()
```

好的，我们来分析一下。函数get_default_loss(model, x)接收一个VAE模型和一些输入***x***，并返回我们之前定义的VAE损失（其中***C* = 1**）。我们定义了一个卷积VAE，其中编码器使用Conv2D层进行下采样，解码器使用[Conv2DTranspose](https://www.geeksforgeeks.org/what-is-transposed-convolutional-layer/)层（反卷积层）进行上采样。我们使用Adam进行了优化。

由于其他两个生成模型从随机噪声开始，我们没有使用某些输入图像，而是从潜在空间中采样并使用解码器生成新图像。我们测试了latent_dim = 2 **和** latent_dim = 100，并获得了以下结果：

![](../Images/3c8e2bbf8dd68b0e589114f533a04b6e.png)

VAE生成的图像。图像由Daisuke Yamada创建

由于我们只进行一次前向传递来生成新样本，因此我们的采样速度很快。此外，这是一个相对简单的模型，所以我们的训练速度也很快。维度为 2（意味着瓶颈潜在表示的维度为 2）的结果很好，但有点模糊。然而，维度为 100 的结果则不是很好。我们认为可能是计算能力不足，或者后验分布开始扩展到不存在的模式。换句话说，我们开始学习没有意义的潜在特征。

那么，理论上如何选择“最佳”的潜在维度呢？显然，100 不是一个好选择，但也许在 2 和 100 之间的某个值是理想的。这里存在样本质量和计算效率之间的权衡。因此，你可以确定这些因素对你的重要性，并进行类似网格搜索的操作来正确选择这个超参数。

我们还绘制了维度为 2 的潜在空间。基本上，以下内容告诉我们解码器根据我们在潜在空间中的起点输出什么。

![](../Images/be81739e30ee3c519f7d08772ede4918.png)

我们的 VAE 潜在空间

正如你所见，潜在空间相当多样化，而且相当完整和连续！因此，反思生成模型三难问题，我们得到以下结论：

![](../Images/adc868c8d930283d3fce1d2bf9bb0c4f.png)

VAE 的三难问题（红色为好，蓝色为坏）。图像由大辅·山田创建

现在我们将转到 DCGANs，并从加速解释 GANs 开始。

## 深度卷积 GANs：

在 GANs 中，有一个 **生成器 G** 和一个 **鉴别器 D**。生成器创建新数据，而鉴别器区分（或辨别）真实数据和虚假数据。两者在一个极小化-极大化游戏中对抗训练，因此有了“对抗性”一词。

我们获得一些训练数据，并开始通过标准正态分布或均匀分布来采样随机噪声 ***z***。这些噪声是待生成数据的潜在表示。我们从噪声开始，以允许更多样化的数据样本，并避免过拟合。

噪声被输入到生成器中，生成器输出生成的数据 ***x = G(z)***。然后，鉴别器接受 ***x*** 并输出 ***P[x = real] = D(x)***，即生成的图像 ***x*** 是真实图像的概率。此外，我们还将训练集中真实图像输入鉴别器。

我们通常将损失定义为一个极小化-极大化游戏：

GANs 损失

对于判别器，这看起来像是二元交叉熵，这很合理，因为它是一个二元分类器。对每个分布中采样的点的期望值实际上对应于你从（a）数据分布中*(****E_{x ~ p(data)})***和（b）随机噪声***(E_{z ~ p(z)).***中获得的数据点。第一个项表示判别器想要最大化将真实数据分类为1的可能性，而第二个项表示判别器想要最大化将假数据分类为0的可能性。判别器还在生成器会以最优方式行动的最小-最大假设下运行。

好的，我们现在将过渡到 DCGANs。DCGANs 与 GANs 类似，但架构上有几个显著变化；主要的是 DCGANs 不使用任何多层感知机，而是使用卷积/反卷积。以下是稳定 DCGANs 的架构指南（来自原始论文）：

+   用步幅卷积（判别器）和分数步幅卷积（即反卷积）（生成器）替换任何池化层

+   在生成器和判别器中使用批量归一化

+   为了更深的架构，移除全连接的隐藏层

+   在生成器中，对所有层使用 ReLU 激活，除了输出层使用 Tanh

+   在判别器中，对所有层使用 LeakyReLU 激活。

![](../Images/e32fb044eea8a87bcc9946c9b35c269c.png)

DCCGAN 架构 - 图像由 Justin Cheigh 创建

我们经常使用GANs进行图像生成，因此直观上使用卷积层是有意义的。我们在判别器中使用标准卷积层，因为我们希望将图像降采样成分层特征，而对于生成器，我们使用反卷积层将图像从噪声（潜在表示）上采样到生成的图像。

批量归一化用于稳定训练过程、提高收敛性并加快学习速度。Leaky ReLU 防止了 ReLU 的零学习问题。最后，使用 Tanh 来防止较小输入的饱和，并避免梯度消失问题（因为它在原点周围是对称的）。

太棒了！现在让我们看看如何实现 DCGANs。导入库后，我们设置超参数值：

```py
latent_dim = 100
epochs = 100
batch_size = 32
learning_rate = 1e-4
```

经过一些数据预处理和为计算效率进行批量拆分后，我们准备定义生成器和判别器：

```py
generator = Sequential([
    # input
    Dense(units=7*7*128, input_shape=(latent_dim,)),
    Reshape((7,7,128)),
    # conv 1
    Conv2DTranspose(filters=64, kernel_size=3, strides=2, padding='same'),
    BatchNormalization(),
    ReLU(max_value=0.2),
    # conv 2
    Conv2DTranspose(filters=64, kernel_size=3, strides=1, padding='same'),
    BatchNormalization(),
    ReLU(max_value=0.2),
    # final tanh    
    Conv2DTranspose(filters=1, kernel_size=3, strides=2, padding='same', activation='tanh')
])

discriminator = Sequential([
    # conv 1
    Conv2D(filters=64, kernel_size=3, strides=2, padding='same', input_shape=(28,28,1)),
    LeakyReLU(0.2),
    # conv 2
    Conv2D(filters=64, kernel_size=3, strides=2, padding='same'),
    # BatchNormalization(),
    LeakyReLU(0.2),
    # output
    Flatten(),
    Dense(1, activation='sigmoid')
])

discriminator.compile(optimizer=Adam(learning_rate=learning_rate),
                      loss=BinaryCrossentropy(), 
                      metrics=[BinaryAccuracy()])
discriminator.trainable = False
gan = Sequential([
    generator, 
    discriminator
])
gan.compile(optimizer=Adam(learning_rate=learning_rate), loss=BinaryCrossentropy(), metrics=[BinaryAccuracy()])
```

记住所有关于 DCGANs 的架构指南！我们对生成器使用 Conv2DTranspose，对判别器使用普通的 Conv2D 和步幅。注意我们用 Binary cross entropy 编译判别器，但将判别器指定为 trainable = False。这是因为我们将自己实现训练循环：

```py
def generate():
  # generate image based on random noise z
    with tf.device(device):
        random_vector_z = np.random.normal(loc=0, scale=1, size=(16, latent_dim))
        generated = generator(random_vector_z)
        return generate

# labels 
real = np.ones(shape=(batch_size, 1))
fake = np.zeros(shape=(batch_size, 1))

# generator and discriminator losses
g_losses, d_losses = [], []

with tf.device(device_name=device):
    for epoch in range(epochs):
        for real_x in x_train_digits:
            '''discriminator'''
            # train on real data
            d_loss_real = discriminator.train_on_batch(x=real_x, y=real)

            # train on fake data
            z = np.random.normal(loc=0, scale=1, size=(batch_size, latent_dim))
            fake_x = generator.predict_on_batch(x=z)
            d_loss_fake = discriminator.train_on_batch(x=fake_x, y=fake)

            # total loss
            d_loss = np.mean(d_loss_real + d_loss_fake)

            '''generator'''
            g_loss = gan.train_on_batch(x=z, y=real)

        g_losses.append(g_loss[-1])
        d_losses.append(d_loss)
```

首先，我们用真实数据和生成器生成的假数据训练判别器，然后也训练生成器。太棒了！让我们看看结果：

![](../Images/0fa24f563a86ba2252c53f82b859d186.png)

图像由大辅·山田创作

我们的样本质量更好（不那么模糊）！我们仍然有快速采样，因为推理只需将随机噪声输入生成器。下面是我们的潜在空间：

![](../Images/137d0d8879e32e6c0774ce7c15a5e88c.png)

我们的DCGAN潜在空间

不幸的是，我们的潜在空间不是很多样化（特别是在潜在维度为2的第1代样本中尤为明显）。我们很可能遇到了模式崩溃的常见问题。正式地说，这意味着生成器只学会创建一个专门用来欺骗判别器的子集。换句话说，如果生成器在生成1的图像时判别器表现不佳，那么生成器没有理由去做其他事情。因此，对于生成模型三难问题，我们得到如下：

![](../Images/b0fa9d8b81a5445c2defc1776dea1c93.png)

DCGANs的三难困境（红色为好，蓝色为坏）。图像由大辅·山田创作

现在我们已经探索了GANs和DCGANs，是时候过渡到扩散模型了！

## 扩散模型：

扩散概率模型（或简称扩散模型）目前是每个顶级图像生成模型的一部分。我们将讨论**去噪扩散概率模型（DDPMs）**。

对于VAEs和GANs，样本生成涉及从噪声到生成图像的一个步骤。GANs通过将噪声输入生成器并进行正向传播来执行推理，而VAEs通过采样噪声并将其传递通过解码器来执行推理。扩散模型的主要思想是生成一系列图像，其中每个后续图像稍微少一些噪声，最终图像理想情况下是现实的！DDPM有两个方面。在正向过程中，我们使用真实图像并迭代地添加噪声。在反向过程中，我们学习如何撤销在正向过程中添加的噪声：

让我们从直观的层面解释正向和反向过程。我们给定一组训练数据***X***₀***，***其中每个数据点***x₀*** ∈ ***X***₀从数据分布***x₀ ~ q(x₀)***中采样。回忆一下***q(x₀)***是我们想要表示的未知分布。

从右到左是硬编码的正向过程，我们从一些训练样本***x₀ ~ q(x₀)***开始，逐步添加高斯噪声。也就是说，我们将生成一系列图像***x***₁, ***x***₂, …, ***x_T***，其中每个后续图像都越来越嘈杂。最终，我们会得到可以被视为纯噪声的东西！从左到右是反向过程，我们学习如何去噪，即预测如何从***x_{t+1} → xₜ***。很好！现在我们理解了正向和反向过程的基本概念，让我们深入了解理论吧！

正式地，前向过程由一个马尔可夫链描述，该链根据预定的方差调度***β₁, …, β_T*** 迭代地向数据中添加高斯噪声。马尔可夫链这个术语意味着***x_{t+1}*** *仅* 依赖于***xₜ***。因此，***x_{t+1}*** 在给定***xₜ***的条件下，与***x***₁, …, ***x_ₜ₋₁*** 条件独立。这意味着***q(xₜ | x₀, …, xₜ₋₁) = q(xₜ | xₜ₋₁)***。另一个重要的概念是方差调度。我们定义一些值***β₁, …, β_T***，这些值用于参数化我们在每个时间步添加的高斯噪声。通常，***0 ≤ β ≤ 1***，其中***β₁*** 较小，***β_T*** 较大。所有这些都包含在我们对***q(xₜ | xₜ₋₁)***的定义中：

前向扩散过程

所以，我们从***x₀***开始。然后，经过**T**个时间步，我们遵循上述方程来获取序列中的下一个图像：***xₜ ~ q(xₜ | xₜ₋₁)***。可以证明在极限**T →** ∞ 时，***x_T*** 等同于各向同性的高斯分布。

不过我们还没完成。我们直观上应该能够通过递归展开一步步从***x₀*** 到任何***x_t***。我们首先使用重参数化技巧（类似于变分自编码器）：

重参数化技巧

这使我们能够做到以下几点：

抽样任意时间步 t

通过遵循上述方程，我们可以在一步内从***x₀***到任何***xₜ***！对那些好奇的人，[推导过程](https://medium.com/@steinsfu/diffusion-model-clearly-explained-cd331bd41166)涉及展开和使用[高斯的加法性质](https://en.wikipedia.org/wiki/Sum_of_normally_distributed_random_variables)。让我们继续讨论逆向过程。

在逆向过程中，我们的目标是了解***q(xₜ₋₁| xₜ)***，因为我们可以随机取噪声并从***q(xₜ₋₁ | xₜ)*** 中迭代抽样以生成逼真的图像。我们可能会认为可以使用贝叶斯规则轻松获得***q(xₜ₋₁ | xₜ)***，但事实证明这是计算上不可行的。这在直观上是合理的；要逆转前向步骤，我们需要查看***xₜ*** 并考虑我们可能到达那里的所有方式。

因此，我们将学习一个具有权重***θ***的模型***p***，而不是直接计算***q(xₜ₋₁ | xₜ)***，该模型近似这些条件概率。幸运的是，如果***βₜ*** 足够小，我们可以成功地将***q(xₜ₋₁| xₜ)*** 估计为高斯分布。这一见解来自涉及随机微分方程的一些极其困难的理论。因此，我们可以定义***p***为以下内容：

模型定义

那么，我们的损失是什么呢？如果我们想要撤销添加的噪声，直观上只需预测添加的噪声就足够了。要查看更完整的推导，请查阅[Lilian Weng**的这篇优秀博客](https://lilianweng.github.io/posts/2021-07-11-diffusion-models/#nice)**。但事实证明，我们的直觉是正确的，我们不需要一个模型***p***，而是可以使用一个预测添加噪声的网络***ϵ_θ***。通过这种方法，我们可以使用实际噪声和预测噪声之间的均方误差（MSE）进行训练：

最终损失

在这里，***ϵ*** 是实际误差，而另一个术语是预测误差。你可能会注意到期望值是对***x_0***取的；这是因为通常误差是以重参数化技巧（如上所述）的形式写出的，这使你可以直接从***x_0*获得***x_t***。因此，我们的网络输入是时间***t***和当前图像***xₜ***。

让我们做一个全面回顾。我们使用 MSE 训练网络***ϵ_θ***以学习如何预测添加的噪声。一旦训练完成，我们可以使用我们的神经网络***ϵ_θ***来预测任何时间步的添加噪声。利用这些噪声和一些上述方程，我们完成逆过程并有效地“去噪”。因此，我们可以通过获取噪声并持续去噪来进行推断。这两个过程都由以下伪代码描述：

![](../Images/cf0de3a60f43f86b4d104047722d3568.png)

[训练/采样伪代码](https://hojonathanho.github.io/diffusion/)

在训练过程中，我们获取一张真实图像，抽样***t* ~ Uniform({*1,2,…,T*})**（我们这样做是因为逐步计算效率低），然后对目标/预测噪声的 MSE 进行梯度下降。在采样过程中，我们取随机噪声，然后使用我们预测的噪声和推导的方程连续采样，直到我们得到一些生成的图像***x₀.***

很好！我们现在可以进入实施细节。对于我们的基础架构，我们将使用一个[**U-Net**](https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/)**：**

![](../Images/805102a6a9bbde32efe879e54048736e.png)

图像由 Justin Cheigh 创建；[灵感](https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/)

从架构来看，为什么这被称为 U-Net 很明显！U-Net 最初用于生物医学图像分割，但它们在扩散模型中也表现得非常好！直观地说，这是因为（a）输入和输出的形状是相同的，这正是我们需要的，以及（b）我们将看到 U-Net（由于编码器-解码器结构配合跳跃连接）在保留局部/全局信息方面表现良好，这有助于保留图像但仍有效地添加噪声。

U-Net 具有类似于过去生成模型的编码器-解码器结构。具体来说，如果你查看形状为“U”的图像，你会发现下行过程中有一系列下采样层，这些层是编码器结构的一部分。上行过程中，我们有一系列上采样层，这些层是解码器结构的一部分。输入和输出具有相同的形状，这对于我们的输入是带噪声的图像 ***xₜ*** 和我们的输出是一些预测噪声的情况来说是理想的。

但是，你可能会注意到 U-Net 和标准自编码器之间有一个重要区别，那就是跳跃连接。在每个层级中，我们有一个下采样模块，该模块连接到另一个下采样模块（遵循“U”的形状），并且有一个跳跃连接到上采样模块。记住，这些下采样模块基本上是在以不同分辨率查看图像（学习不同层次的特征）。通过这些跳跃连接，我们确保在每个分辨率上都考虑到这些特征！另一种思考 U-Net 的方式是将其视为一系列堆叠的自编码器。

好的，现在让我们看一下我们的具体实现。首先，我撒了个谎……我说我们的输入只是带噪声的图像 ***xₜ.*** 然而，我们还输入了实际的时间步 ***t*** 以提供时间概念。我们的方法是使用时间步嵌入，其中我们取时间 ***t*** 并使用正弦位置嵌入：

![](../Images/ba068bfe9ba3050bc3ea39b46c6088ed.png)

正弦位置嵌入

对于不熟悉的人，正弦位置嵌入的高级概述是我们使用正弦函数对某些序列中的元素（这里是时间步）进行编码，直觉是这些函数的平滑结构将更容易被神经网络学习。因此，我们的实际输入是带噪声的图像 ***xₜ*** 和时间步 ***t***，它们首先经过这个时间嵌入层。

接下来是我们的下采样/上采样模块：每个下采样（上采样）模块包含 2 个 ResNet 模块、1 个 Attention 层和 1 个卷积（反卷积）层。我们快速了解一下这些模块。

残差网络（ResNet）基本上是一系列具有大跳跃连接的卷积层，这些跳跃连接允许信息在非常深的神经网络中流动。Attention 是一种革命性的想法，对于理解像 Transformer 这样的基础架构至关重要，它告诉神经网络该关注什么。例如，这里我们有 2 个 ResNet 模块。在这些模块之后，我们将得到输入图像的潜在特征向量，Attention 层将告诉神经网络这些特征中哪些是最重要的。最后，标准的卷积/反卷积分别用于下采样/上采样。

在我们的实现中，我们使用了 4 个这样的堆叠自编码器：

```py
 def call(self, x, time, training=True, **kwargs):
        with tf.device(device):
            # front conv
            x = self.init_conv(x)

            # time embedding
            t = self.time_mlp(time)

            # move down the encoder
            h = []
            for down_block1, down_block2, attention, downsample in self.downs:
                x = down_block1(x, t)
                x = down_block2(x, t)
                x = attention(x)
                h.append(x) # keep for skip connection!
                x = downsample(x)

            # bottleneck consists of 
            x = self.mid_block1(x, t) # ResNet block
            x = self.mid_attn(x) # Attention layer
            x = self.mid_block2(x, t) # ResNet block

            # move up the decoder
            for up_block1, up_block2, attention, upsample in self.ups:
                x = tf.concat([x, h.pop()], axis=-1)
                x = up_block1(x, t)
                x = up_block2(x, t)
                x = attention(x) 
                x = upsample(x)
            x = tf.concat([x, h.pop()], axis=-1)

            # back conv
            x = self.final_conv(x)
            return x
```

很好！现在我们已经定义了我们的U-Net类，我们可以继续将U-Net应用于我们的具体问题。我们首先定义相关的超参数：

```py
image_size = (32, 32)
num_channel = 1
batch_size = 64
timesteps = 200 
learning_rate = 1e-4
epochs = 10
```

由于计算能力不足，我们使用**timesteps = *T* = 200,** 尽管原始论文使用的是***T* = 1000\.** 数据预处理后，我们定义前向过程

```py
# define forward pass
beta = np.linspace(0.0001, 0.02, timesteps) # variance schedule 
alpha = 1 - beta
a = np.concatenate((np.array([1.]), np.cumprod(alpha, 0)[:-1]), axis=0) # alpha bar

def forward(x_0, t):
    # uses trick to sample from arbitrary timestep!
    with tf.device(device):
        noise_t = np.random.normal(size=x_0.shape)
        sqrt_a_t = np.reshape(np.take(np.sqrt(a), t), (-1, 1, 1, 1))
        sqrt_one_minus_a_t = np.reshape(np.take(np.sqrt(1-a), t), (-1, 1, 1, 1))
        x_t = sqrt_a_t  * x_0 + sqrt_one_minus_a_t  * noise_t
        return noise_t, x_t
```

所以，在这里我们以一种相当标准的方式定义我们的方差调度。在前向函数中，我们使用重新参数化技巧，允许我们从***x₀***中抽样任意的***xₜ***。下面是前向过程的可视化：

![](../Images/253ffb16376a1b1b00563453e87fead4.png)

我们的前向过程可视化

然后我们实例化我们的U-Net，定义我们的损失函数，并定义训练过程：

```py
 unet = Unet() # instantiate model

def loss(noise, predicted):
    # remember we just use MSE!
    with tf.device(device):
        return tf.math.reduce_mean((noise-predicted)**2)

optimizer = keras.optimizers.Adam(learning_rate=learning_rate)
def train_step(train_images):
    with tf.device(device):
        # create a "batch" number of random timesteps (in our case 64)
        timestep_values = tf.random.uniform(shape=[train_images.shape[0]], minval=0, maxval=timesteps, dtype=tf.int32)

        # forward 
        noised_images, noise = forward(x_0=train_images, t=timestep_values)

        # set the gradient and get the prediction
        with tf.GradientTape() as tape:
            predicted = unet(x=noised_images, time=timestep_values)
            loss_value = loss(noise, predicted)

        # optimize U-Net using ADAM
        gradients = tape.gradient(loss_value, unet.trainable_variables)
        optimizer.apply_gradients(zip(gradients, unet.trainable_variables))

    return loss_value

def train(epochs):
    with tf.device(device):
        for epoch in range(epochs):
            losses = []
            for i, batch_images in enumerate(iter(dataset)):
                loss = train_step(batch_images)
                losses.append(loss)
```

记住，我们的损失（经过大量工作之后）只是均方误差（MSE）！其余部分是一个相当标准的训练循环。训练完成后，我们可以考虑推断。回顾我们的采样算法2，我们按如下方式实现：

```py
def denoise_x(x_t, pred_noise, t):
    with tf.device(device):
        # obtain variables
        alpha_t = np.take(alpha, t)
        a_t = np.take(a, t)

        # calculate denoised_x (i.e., x_{t-1})
        beta_t = np.take(beta, t)
        z = np.random.normal(size=x_t.shape)
        denoised_x = (1/np.sqrt(alpha_t)) * (x_t - ((1-alpha_t)/np.sqrt(1-a_t))*pred_noise) + np.sqrt(beta_t) * z
        return denoised_x

def backward(x, i):
    with tf.device(device):
        t = np.expand_dims(np.array(timesteps-i-1, np.int32), 0)
        pred_noise = unet(x, t)
        return denoise_x(x, pred_noise, t)
```

在这里，我们定义如何在特定时间步采集图像并进行去噪。通过这些，我们可以完全定义我们的推断过程：

```py
def get_sample(x=None):
    # generate noise
    if x is None:
        x = tf.random.normal(shape=(1,32,32,1))

    # array to store images
    imgs = [np.squeeze(np.squeeze(x, 0),-1)]

    # backward process
    for i in tqdm(range(timesteps-1)):
        x = backward(x, i)
        if i in [0,25,50,75,100,125,150,175,198]:
            imgs.append(np.squeeze(np.squeeze(x, 0),-1))

    return imgs if show_progress else imgs[-1]
```

我们从随机噪声开始，然后不断使用我们的反向函数去噪，直到得到一个现实的图像！以下是我们的一些结果：

![](../Images/9f01eafdba4b3a6a42b442652dc303ec.png)

我们的DDPM生成样本示例

样本质量相当高。此外，我们能够获得多样化的样本。推测我们的样本质量会随着计算能力的提高而改善；扩散过程计算开销非常大，这影响了我们训练这个模型的能力。还可以进行“逆向工程”。我们取一个训练图像，添加噪声，然后去噪，以观察我们重建图像的能力。我们得到以下结果：

![](../Images/c4d744658be612e0c5066b84b8734d01.png)

逆向工程。图像由大辅·山田创建

需要注意的是，逆向过程是概率性的，这意味着我们并不总是得到与输入图像相似的图像。

很好！让我们回到生成模型三难题。我们有高质量的样本、多样化的样本范围，以及更稳定的训练（这是这些迭代步骤的副产品）。然而，我们有缓慢的训练和采样，因为在推断过程中我们需要一次又一次地采样。因此，我们得到以下结果：

![](../Images/2664a1495ec2701f253da9d716883b6e.png)

DDPMs的三难题（红色代表好，蓝色代表不好）。图像由大辅·山田创建

## 结论：

哇！我们覆盖了三种图像生成模型，从标准的变分自编码器（VAE）到扩散模型（DDPM）。对于每种模型，我们查看了生成模型三难题，并获得了以下结果：

![](../Images/f849303d137db4aedaebd3c75b648cf6.png)

比较模型。图像由大辅·山田创建

自然的问题是：我们能否获得生成模型三难问题的所有3个部分？看起来扩散模型几乎达到了这一点，因为我们只需找出提高采样速度的方法。从直观上讲，这很困难，因为我们依赖于将逆过程建模为高斯过程的假设，这只有在我们在几乎所有时间步长中执行逆过程时才有效。

然而，实际上，获得三难问题的所有3个因素是可能的！像DDIMs或DDGANs这样的模型建立在DDPMs之上，但它们已经找到了提高采样速度的方法（其中一种方法是使用**跨步采样计划**）。通过这些和其他不同的优化，我们可以获得生成模型三难问题的所有3个方面！

那么，接下来是什么？一个特别有趣的方向是条件生成。条件生成允许你在一些类别标签或描述性文本的条件下生成新样本。例如，在所有最初列出的图像生成模型中，你可以输入类似“企鹅卧推1000磅”的内容，并获得合理的输出。虽然我们没有时间探索这一条件生成的方向，但它似乎是一个非常有趣的下一步！

**好吧，这就是我们的全部内容。感谢阅读！**

*除非另有说明，所有图片均由作者（们）创建。*
