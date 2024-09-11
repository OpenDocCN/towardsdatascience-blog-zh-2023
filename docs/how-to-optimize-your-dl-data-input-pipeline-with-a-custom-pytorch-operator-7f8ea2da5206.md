# 如何通过自定义 PyTorch 操作符优化你的深度学习数据输入管道

> 原文：[`towardsdatascience.com/how-to-optimize-your-dl-data-input-pipeline-with-a-custom-pytorch-operator-7f8ea2da5206?source=collection_archive---------5-----------------------#2023-08-31`](https://towardsdatascience.com/how-to-optimize-your-dl-data-input-pipeline-with-a-custom-pytorch-operator-7f8ea2da5206?source=collection_archive---------5-----------------------#2023-08-31)

## PyTorch 模型性能分析与优化 — 第五部分

[](https://chaimrand.medium.com/?source=post_page-----7f8ea2da5206--------------------------------)![Chaim Rand](https://chaimrand.medium.com/?source=post_page-----7f8ea2da5206--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7f8ea2da5206--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7f8ea2da5206--------------------------------) [Chaim Rand](https://chaimrand.medium.com/?source=post_page-----7f8ea2da5206--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9440b37e27fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-optimize-your-dl-data-input-pipeline-with-a-custom-pytorch-operator-7f8ea2da5206&user=Chaim+Rand&userId=9440b37e27fe&source=post_page-9440b37e27fe----7f8ea2da5206---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7f8ea2da5206--------------------------------) · 6 分钟阅读 · 2023 年 8 月 31 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7f8ea2da5206&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-optimize-your-dl-data-input-pipeline-with-a-custom-pytorch-operator-7f8ea2da5206&user=Chaim+Rand&userId=9440b37e27fe&source=-----7f8ea2da5206---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7f8ea2da5206&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-optimize-your-dl-data-input-pipeline-with-a-custom-pytorch-operator-7f8ea2da5206&source=-----7f8ea2da5206---------------------bookmark_footer-----------)![](img/d20cad6621d94c0422ef9d63e6038ade.png)

图片由 [Alexander Grey](https://unsplash.com/@sharonmccutcheon?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这篇文章是关于基于 GPU 的 PyTorch 工作负载性能分析和优化系列文章中的第五篇，直接续接了[第四部分](https://medium.com/towards-data-science/solving-bottlenecks-on-the-data-input-pipeline-with-pytorch-profiler-and-tensorboard-5dced134dbe9)。在第四部分中，我们演示了如何使用[PyTorch Profiler](https://pytorch.org/tutorials/recipes/recipes/profiler_recipe.html)和[TensorBoard](https://pytorch.org/tutorials/intermediate/tensorboard_profiler_tutorial.html)来识别、分析和解决 DL 训练工作负载的数据预处理管道中的性能瓶颈。在这篇文章中，我们讨论了 PyTorch 对[创建自定义操作符](https://pytorch.org/tutorials/advanced/cpp_extension.html)的支持，并演示了它如何帮助我们解决数据输入管道中的性能瓶颈，加速 DL 工作负载，并降低训练成本。感谢[伊扎克·莱维](https://www.linkedin.com/in/yitzhak-levi-49a217201/)和[吉拉德·瓦瑟曼](https://www.linkedin.com/in/gilad-wasserman-992530268/?originalSubdomain=il)对本文章的贡献。与本篇文章相关的代码可以在[这个 GitHub 仓库](https://github.com/czmrand/jpeg-decode-and-crop/tree/main)中找到。

# 构建 PyTorch 扩展

PyTorch 提供了多种创建自定义操作的方法，包括通过自定义[模块](https://pytorch.org/docs/master/generated/torch.nn.Module.html#torch.nn.Module)扩展 torch.nn 和/或自定义[函数](https://pytorch.org/docs/master/autograd.html#function)。在这篇文章中，我们关注 PyTorch 对集成自定义 C++ 代码的支持。这一功能之所以重要，是因为某些操作在 C++ 中实现的效率和/或简便性可能远超 Python。通过指定的 PyTorch 工具，如[CppExtension](https://pytorch.org/docs/stable/cpp_extension.html)，这些操作可以轻松地作为 PyTorch 的“扩展”进行集成，而无需拉取和重新编译整个 PyTorch 代码库。有关这一功能的动机以及如何使用它的详细信息，请参见[官方 PyTorch 自定义 C++ 和 CUDA 扩展教程](https://pytorch.org/tutorials/advanced/cpp_extension.html)。由于我们在本文中关注的是加速基于 CPU 的数据预处理管道，因此我们将仅使用 C++ 扩展，不需要 CUDA 代码。在未来的文章中，我们希望展示如何使用这一功能实现自定义 CUDA 扩展，以加速在 GPU 上运行的训练代码。

# 示例

在我们之前的文章中，我们定义了一个数据输入管道，从解码一个*533*x*800* JPEG 图像开始，然后提取一个随机的*256*x*256*裁剪区域，经过一些额外的变换后，送入训练循环。我们使用了[PyTorch Profiler](https://pytorch.org/tutorials/recipes/recipes/profiler_recipe.html)和[TensorBoard](https://pytorch.org/tutorials/intermediate/tensorboard_profiler_tutorial.html)来测量从文件加载图像的时间，并**承认了解码的浪费**。为了完整性，我们在下面复制了代码：

```py
import numpy as np
from PIL import Image
from torchvision.datasets.vision import VisionDataset
input_img_size = [533, 800]
img_size = 256

class FakeDataset(VisionDataset):
    def __init__(self, transform):
        super().__init__(root=None, transform=transform)
        size = 10000
        self.img_files = [f'{i}.jpg' for i in range(size)]
        self.targets = np.random.randint(low=0,high=num_classes,
                                         size=(size),dtype=np.uint8).tolist()

    def __getitem__(self, index):
        img_file, target = self.img_files[index], self.targets[index]
        img = Image.open(img_file)
        if self.transform is not None:
            img = self.transform(img)
        return img, target

    def __len__(self):
        return len(self.img_files)

transform = T.Compose(
    [T.PILToTensor(),
     T.RandomCrop(img_size),
     RandomMask(),
     ConvertColor(),
     Scale()])
```

回顾我们之前的文章，我们达到的优化平均步骤时间是**0.72 秒**。可以推测，如果我们能够仅解码感兴趣的裁剪区域，我们的管道将运行得更快。不幸的是，截至本文写作时，PyTorch 并没有包含支持这一功能的函数。然而，利用自定义操作符创建工具，我们可以定义并实现自己的函数！

# 自定义 JPEG 图像解码和裁剪函数

[libjpeg-turbo](https://libjpeg-turbo.org/)库是一个 JPEG 图像编解码器，相比于[libjpeg](http://www.ijg.org/)，包含了许多增强和优化功能。特别是，[libjpeg-turbo](https://libjpeg-turbo.org/)包括许多功能，使我们能够仅解码图像中的预定义裁剪区域，例如*jpeg_skip_scanlines*和*jpeg_crop_scanline*。如果你在 conda 环境中运行，可以使用以下命令进行安装：

```py
conda install -c conda-forge libjpeg-turbo
```

请注意，[libjpeg-turbo](https://libjpeg-turbo.org/)已预装在我们将在下面实验中使用的官方[AWS PyTorch 2.0 深度学习 Docker 镜像](https://github.com/aws/deep-learning-containers)中。

在下面的代码块中，我们修改了[*decode_jpeg*](https://github.com/pytorch/vision/blob/release/0.15/torchvision/csrc/io/image/cpu/decode_jpeg.cpp#L72)函数，来自[torchvision 0.15](https://pytorch.org/vision/stable/index.html)，以解码并返回来自输入 JPEG 编码图像的请求裁剪。

```py
torch::Tensor decode_and_crop_jpeg(const torch::Tensor& data,
                                   unsigned int crop_y,
                                   unsigned int crop_x,
                                   unsigned int crop_height,
                                   unsigned int crop_width) {
  struct jpeg_decompress_struct cinfo;
  struct torch_jpeg_error_mgr jerr;

  auto datap = data.data_ptr<uint8_t>();
  // Setup decompression structure
  cinfo.err = jpeg_std_error(&jerr.pub);
  jerr.pub.error_exit = torch_jpeg_error_exit;
  /* Establish the setjmp return context for my_error_exit to use. */
  setjmp(jerr.setjmp_buffer);
  jpeg_create_decompress(&cinfo);
  torch_jpeg_set_source_mgr(&cinfo, datap, data.numel());

  // read info from header.
  jpeg_read_header(&cinfo, TRUE);

  int channels = cinfo.num_components;

  jpeg_start_decompress(&cinfo);

  int stride = crop_width * channels;
  auto tensor =
     torch::empty({int64_t(crop_height), int64_t(crop_width), channels},
                  torch::kU8);
  auto ptr = tensor.data_ptr<uint8_t>();

  unsigned int update_width = crop_width;
  jpeg_crop_scanline(&cinfo, &crop_x, &update_width);
  jpeg_skip_scanlines(&cinfo, crop_y);

  const int offset = (cinfo.output_width - crop_width) * channels;
  uint8_t* temp = nullptr;
  if(offset > 0) temp = new uint8_t[cinfo.output_width * channels];

  while (cinfo.output_scanline < crop_y + crop_height) {
    /* jpeg_read_scanlines expects an array of pointers to scanlines.
     * Here the array is only one element long, but you could ask for
     * more than one scanline at a time if that's more convenient.
     */
    if(offset>0){
      jpeg_read_scanlines(&cinfo, &temp, 1);
      memcpy(ptr, temp + offset, stride);
    }
    else
      jpeg_read_scanlines(&cinfo, &ptr, 1);
    ptr += stride;
  }
  if(offset > 0){
    delete[] temp;
    temp = nullptr;
  }
  if (cinfo.output_scanline < cinfo.output_height) {
    // Skip the rest of scanlines, required by jpeg_destroy_decompress.
    jpeg_skip_scanlines(&cinfo,
                        cinfo.output_height - crop_y - crop_height);
  }
  jpeg_finish_decompress(&cinfo);
  jpeg_destroy_decompress(&cinfo);
  return tensor.permute({2, 0, 1});
}

PYBIND11_MODULE(TORCH_EXTENSION_NAME, m) {
  m.def("decode_and_crop_jpeg",&decode_and_crop_jpeg,"decode_and_crop_jpeg");
}
```

完整的 C++ 文件可以在[这里](https://github.com/czmrand/jpeg-decode-and-crop/blob/main/custom_op/decode_and_crop_jpeg.cpp)找到。

在接下来的部分中，我们将按照 PyTorch 教程中的步骤，将其转换为一个 PyTorch 操作符，以便在我们的预处理管道中使用。

# 部署 PyTorch 扩展

如[PyTorch 教程](https://pytorch.org/tutorials/advanced/cpp_extension.html)中所述，有多种方式来部署自定义操作符。设计部署时需要考虑许多因素。以下是我们认为重要的一些例子：

1.  **即时编译**：为了确保我们的 C++扩展与我们训练时使用的 PyTorch 版本一致，我们编程部署脚本在训练**环境内**训练前编译代码。

1.  **多进程支持**：部署脚本必须支持我们的 C++扩展从多个进程（例如多个 DataLoader 工作者）加载的可能性。

1.  **托管训练支持**：由于我们经常在托管训练环境（如[Amazon SageMaker](https://aws.amazon.com/sagemaker/)）中进行训练，因此我们要求部署脚本支持此选项。（有关自定义托管训练环境的更多信息，请参见这里。）

在下面的代码块中，我们定义了一个简单的*setup.py*脚本，用于编译和安装我们的自定义函数，如[这里](https://pytorch.org/tutorials/advanced/cpp_extension.html#building-with-setuptools)所述。

```py
from setuptools import setup
from torch.utils import cpp_extension

setup(name='decode_and_crop_jpeg',
      ext_modules=[cpp_extension.CppExtension('decode_and_crop_jpeg', 
                                              ['decode_and_crop_jpeg.cpp'], 
                                              libraries=['jpeg'])],
      cmdclass={'build_ext': cpp_extension.BuildExtension})
```

我们将 C++文件和*setup.py*脚本放在一个名为*custom_op*的文件夹中，并定义一个*__init__.py*，以确保安装脚本只运行一次且由单个进程执行。

```py
import os
import sys
import subprocess
import shlex
import filelock

p_dir = os.path.dirname(__file__)

with filelock.FileLock(os.path.join(pkg_dir, f".lock")):
  try:
    from custom_op.decode_and_crop_jpeg import decode_and_crop_jpeg
  except ImportError:
    install_cmd = f"{sys.executable} setup.py build_ext --inplace"
    subprocess.run(shlex.split(install_cmd), capture_output=True, cwd=p_dir)
    from custom_op.decode_and_crop_jpeg import decode_and_crop_jpeg
```

最后，我们修订了数据输入管道，使用我们新创建的自定义函数：

```py
from torchvision.datasets.vision import VisionDataset
input_img_size = [533, 800]
class FakeDataset(VisionDataset):
    def __init__(self, transform):
        super().__init__(root=None, transform=transform)
        size = 10000
        self.img_files = [f'{i}.jpg' for i in range(size)]
        self.targets = np.random.randint(low=0,high=num_classes,
                                        size=(size),dtype=np.uint8).tolist()

    def __getitem__(self, index):
        img_file, target = self.img_files[index], self.targets[index]
        with torch.profiler.record_function('decode_and_crop_jpeg'):
            import random
            from custom_op.decode_and_crop_jpeg import decode_and_crop_jpeg
            with open(img_file, 'rb') as f:
                x = torch.frombuffer(f.read(), dtype=torch.uint8)
            h_offset = random.randint(0, input_img_size[0] - img_size)
            w_offset = random.randint(0, input_img_size[1] - img_size)
            img = decode_and_crop_jpeg(x, h_offset, w_offset, 
                                       img_size, img_size)

        if self.transform is not None:
            img = self.transform(img)
        return img, target

    def __len__(self):
        return len(self.img_files)

transform = T.Compose(
    [RandomMask(),
     ConvertColor(),
     Scale()])
```

# 结果

根据我们描述的优化步骤，我们的步骤时间从 0.72 秒降到了 0.48 秒（提高了 50%）！显然，我们优化的影响与原始 JPEG 图像的大小和我们选择的裁剪大小直接相关。

# 摘要

数据预处理管道中的瓶颈是常见的现象，可能导致 GPU 资源不足并减慢训练。考虑到潜在的成本影响，拥有多种工具和技术来分析和解决这些问题是至关重要的。在本文中，我们回顾了通过创建自定义 C++ PyTorch 扩展来优化数据输入管道的选项，展示了其易用性，并说明了其潜在影响。当然，这种优化机制的潜在收益会根据项目和性能瓶颈的细节而大相径庭。

这里讨论的优化技术加入了我们在许多博客文章中讨论的广泛的输入管道优化方法。我们鼓励你查看这些文章（例如，从这里开始）。

**接下来做什么？** 在我们关于 PyTorch 性能分析和优化系列的[第六部分](https://chaimrand.medium.com/pytorch-model-performance-analysis-and-optimization-part-6-b87412a0371b)中，我们探讨了分析性能问题中较复杂的类型——训练步骤的反向传播过程中的瓶颈。
