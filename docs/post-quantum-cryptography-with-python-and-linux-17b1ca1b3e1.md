# 使用 Python 和 Linux 的后量子密码学

> 原文：[https://towardsdatascience.com/post-quantum-cryptography-with-python-and-linux-17b1ca1b3e1?source=collection_archive---------2-----------------------#2023-08-15](https://towardsdatascience.com/post-quantum-cryptography-with-python-and-linux-17b1ca1b3e1?source=collection_archive---------2-----------------------#2023-08-15)

## 初学者指南

[](https://medium.com/@c4ristian?source=post_page-----17b1ca1b3e1--------------------------------)[![Christian Koch](../Images/6daf756236838069bf79f1078b03ae6d.png)](https://medium.com/@c4ristian?source=post_page-----17b1ca1b3e1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----17b1ca1b3e1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----17b1ca1b3e1--------------------------------) [Christian Koch](https://medium.com/@c4ristian?source=post_page-----17b1ca1b3e1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7633c76cf996&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpost-quantum-cryptography-with-python-and-linux-17b1ca1b3e1&user=Christian+Koch&userId=7633c76cf996&source=post_page-7633c76cf996----17b1ca1b3e1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----17b1ca1b3e1--------------------------------) ·9分钟阅读·2023年8月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F17b1ca1b3e1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpost-quantum-cryptography-with-python-and-linux-17b1ca1b3e1&user=Christian+Koch&userId=7633c76cf996&source=-----17b1ca1b3e1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F17b1ca1b3e1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpost-quantum-cryptography-with-python-and-linux-17b1ca1b3e1&source=-----17b1ca1b3e1---------------------bookmark_footer-----------)![](../Images/e6b61071bf55edd2dff757becfe44fee.png)

图片由 [Jean-Louis Paulin](https://unsplash.com/@jlxp?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如果我们相信爱德华·斯诺登，密码学是“对抗监视的唯一真正保护” [1]。然而，量子技术的进步可能会危及这一保障。我们的文章讨论了量子计算为什么对数据安全构成威胁以及如何应对。我们不是进行纯理论分析，而是通过使用 Python、C 和 Linux 的代码示例来展开讨论。

# 量子基础

当谷歌科学家在2019年报告首次实现量子[supremacy](https://www.nature.com/articles/s41586-019-1666-5)时，引发了极大的兴奋。量子计算可能对加密产生重大影响。要理解这个问题，我们需要讨论一些基本概念。

与经典计算机不同，量子计算机的算法不依赖于位（bits），而是依赖于*量子位（qubits）*。一个位只能取状态0或1。当我们多次测量一个位时，总是得到相同的结果。量子位则不同。尽管听起来很奇怪，但一个量子位可以同时取0和1的值。当我们重复测量时，只能得到0或1的某种概率。在量子位的初始状态下，测量为0的概率通常是百分之百。然而，通过叠加，不同的概率分布可以生成。这些原因源于量子力学，遵循与“正常”生活不同的规律。

量子计算机的主要优势在于其概率特性。经典计算机在我们需要可靠的单一结果时表现出色，而量子计算机则擅长处理概率和组合问题。当我们对处于叠加状态的量子位执行操作时，它同时作用于值0和1。随着量子位数量的增加，量子计算机相对于经典计算机的优势也会增加。一个具有三量子位的量子计算机可以同时处理最多八个值（2³）：即二进制数000、001、010、011、100、101、110和111。

科学文献一致认为，量子计算机将有助于解决以前看似难以处理的问题。然而，目前没有理想的量子计算机。当前一代量子计算机被称为噪声中等规模量子（NISQ）。这种机器处理能力有限，对错误很敏感。现代设备提供最多几百个量子位。一个例子是IBM在2022年推出的433量子位[Osprey](https://spectrum.ieee.org/ibm-quantum-computer-osprey)芯片。现在，该公司[计划](https://www.technologyreview.com/2023/05/25/1073606/ibm-wants-to-build-a-100000-qubit-quantum-computer/)到2033年开发一台具有100,000量子位的机器。

我们的文章解释了为什么这一进展对数据安全构成威胁。通过代码示例，我们展示了量子计算机如何破解某些加密方法，并讨论了应对策略。源代码可以在[GitHub](https://github.com/c4ristian/encryption)上找到。它是在Kali Linux 2023.2下使用Python 3.10的Anaconda开发的。

# 加密与素因数

在加密消息时，相对简单的方法是应用[对称](https://en.wikipedia.org/wiki/Symmetric-key_algorithm)算法。这种方法使用相同的密钥进行明文的加密和密文的解密。这个方法的主要挑战是安全地交换发件人和收件人之间的密钥。一旦私钥被第三方知道，他们就有机会拦截并解密消息。

[非对称](https://en.wikipedia.org/wiki/Public-key_cryptography)密码学似乎是解决这个问题的方案。像[RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem))这样的算法使用不同的密钥进行加密和解密。在这里，加密是使用一个或多个公开密钥，收件人将其提供给所有人。解密时，收件人使用仅自己知道的私钥。这样，发件人可以在没有风险的情况下获得公开密钥，因为它本身并不保密。只有收件人的私钥必须被保护。但是，当潜在攻击者知道公开密钥时，如何加固这样的程序？为此，非对称算法依赖于像质因数分解这样的数学问题。

质因数分解通过示例最好地理解。在 Python 中，我们可以使用库[SymPy](https://www.sympy.org/en/index.html)的`factorint`函数来确定某个整数的质因数。

```py
>>> import sympy
>>> sympy.factorint(10)
{2: 1, 5: 1}
>>> 2**1 * 5**1
10
>>> sympy.factorint(1000)
{2: 3, 5: 3}
>>> 2**3 * 5**3
1000
>>> sympy.factorint(55557)
{3: 2, 6173: 1}
>>> 3**2 * 6173**1
55557
>>>
```

上述控制台输出说明了每个自然数都可以表示为质数的乘积。这些被称为质因数。回想一下学校的日子，质数只能被1和自身整除。例如，数字10可以用术语10=2¹ * 5¹表示。因此，10的质因数是2和5。类似地，数字55557可以用方程55557=3² * 6173¹表示。所以，55557的质因数是3和6173。找到给定整数的质因数的过程称为质因数分解。

对于经典计算机，质因数分解对于小数字来说很简单，但对于大整数则变得越来越困难。每增加一个数字都会大幅增加可能组合的总和。超过某个点后，经典计算机几乎无法确定质因数。例如，考虑以下来自RSA因数分解[挑战](https://en.wikipedia.org/wiki/RSA_Factoring_Challenge)的数字（RSA-260），该挑战于2007年结束。在撰写时，它尚未被因数分解。

```py
#!/usr/bin/env python
import sympy

rsa_260 = 22112825529529666435281085255026230927612089502470015394413748319128822941402001986512729726569746599085900330031400051170742204560859276357953757185954298838958709229238491006703034124620545784566413664540684214361293017694020846391065875914794251435144458199

print("Start factoring...")
factors = sympy.factorint(rsa_260)

# Will probably not be reached
print(factors)
```

像RSA这样的非对称算法利用质因数分解和类似问题的计算难度来确保加密。不幸的是，量子世界遵循自己的规律。

# 量子算法

关于密码学，有两个量子算法尤其值得关注。[Shor 算法](https://arxiv.org/abs/quant-ph/9508027)提供了一种高效的质因数分解方法。在大型量子设备上运行时，它理论上可以破解像 RSA 这样的非对称加密方法。从实际角度来看，这种情况仍在未来。一篇2023年的《自然》[文章](https://www.nature.com/articles/d41586-023-00017-0)提到至少需要1,000,000个量子比特。撇开硬件不谈，找到能够在大型量子计算机上可靠扩展的算法实现也很困难。IBM 的框架[Qiskit](https://qiskit.org/)曾尝试实现这一功能，但在版本 0.22.0 时[弃用了](https://qiskit.org/documentation/release_notes.html#algorithms-upgrade-notes)。不过，网上可以找到Shor算法的实验性实现。

[Grover 算法](https://arxiv.org/abs/quant-ph/9605043)对对称加密构成威胁。也称为量子搜索算法，它为对给定函数的输入进行无结构搜索提供了加速。量子计算机可以利用它加速对对称加密信息的暴力攻击。然而，与 Shor 算法不同的是，所提供的加速不是指数级的。简单来说，这意味着增加加密密钥的长度会使搜索变得极其昂贵。例如，对128位密钥进行暴力攻击需要最多 2¹²⁸ 次迭代。假设 Grover 的搜索将这个数字减少到 2⁶⁴，那么将密钥长度加倍到 256 位会再次增加到 2¹²⁸ 次迭代。这为可能的解决方案打开了大门。

# 对称加密解决方案

在某些条件下，对称加密是一种现成的、简单的方法来应对量子算法。原因在于 Grover 的搜索并不会指数级扩展，而 Shor 算法只威胁到非对称方法。根据当前的知识，高度复杂的对称算法可以被视为量子抗性。现在，美国国家标准与技术研究所（NIST）以及德国联邦信息安全局（BSI）都将[AES-256](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)纳入这一类别[2][3]。AES 是高级加密标准（Advanced Encryption Standard）的缩写，而数字 256 代表密钥的位长。在 Linux 下，AES-256 由 GNU 隐私保护工具 ([GnuPG](https://www.gnupg.org/)) 实现。下面的 shell 脚本展示了如何使用 AES-256 对文件进行加密和解密。

```py
# Encrypt
gpg --output encrypted.gpg --symmetric --cipher-algo AES256 plain.txt

# Decrypt
gpg --output decrypted.txt --decrypt encrypted.gpg
```

上述脚本加密了文件“plain.txt”的内容，将密文写入“encrypted.gpg”文档，再次解密它，最后将输出保存到文件“decrypted.txt”。在加密之前，GnuPG 会要求输入密码短语以生成私钥。出于安全原因，选择一个强密码短语并保密至关重要。GnuPG 可能会缓存密码短语，并在解密时不再询问。要清除缓存，可以执行以下 shell 命令。

```py
gpg-connect-agent reloadagent /bye
```

将 GnuPG 集成到 Python 中使用 `subprocess` 模块相对简单。下面的代码片段展示了使用 AES-256 的加密原型实现。

```py
#!/usr/bin/env python
import subprocess
import getpass

# Read passphrase
passphrase = getpass.getpass("Passphrase:")
passphrase2 = getpass.getpass("Passphrase:")

if passphrase != passphrase2:
  raise ValueError("Passphrases not identical!")

# Perform encryption
print("Encrypting...")

args = [
  "gpg",
  "--batch",
  "--passphrase-fd", "0",
  "--output", "encrypted.gpg",
  "--symmetric",
  "--yes",
  "--cipher-algo", "AES256",
  "plain.txt",
]

result = subprocess.run(
  args, input=passphrase.encode(),
  capture_output=True)

if result.returncode != 0:
  raise ValueError(result.stderr)
```

为了获取密码短语，上述脚本使用 `getpass` 模块。确认后，密码短语通过标准输入传递给 GnuPG。这由参数 `passphrase-fd 0` 指示。或者，密码短语可以作为字符串或通过文件通过命令行参数发送给 GnuPG。然而，由于这些参数对其他用户可见，因此这两种选项在原型中被拒绝了。另一种更安全的方式是使用 [GPG-Agent](https://www.gnupg.org/documentation/manuals/gnupg/Invoking-GPG_002dAGENT.html)。选择哪种选项取决于所需的安全级别。包括加密和解密在内的概念验证可以在 [这里](https://github.com/c4ristian/encryption/blob/master/gpg/execute_gpg.py) 找到。作为 GnuPG 的替代方案，还有其他 AES-256 实现。在这里选择一个可信的来源至关重要。

# 非对称解决方法

寻找非对称解决方案时，NIST 后量子密码学标准化 [计划](https://csrc.nist.gov/projects/post-quantum-cryptography/post-quantum-cryptography-standardization) 是一个很好的起点。自 2016 年以来，它评估了多个抗量子算法的候选者。获胜者之一是 [Kyber](https://pq-crystals.org/kyber/)。该系统实现了一种所谓的安全密钥封装机制。与其他算法类似，Kyber 依赖于一个难以解决的问题来保护两个方之间的密钥交换。与素因数分解不同，它基于一个称为“带错误学习”的问题。Kyber 提供的保护级别取决于密钥长度。例如，Kyber-1024 旨在提供“与 AES-256 大致相当”的安全级别 [4]。

用 C 语言编写的 Kyber 参考实现可在 [GitHub](https://github.com/pq-crystals/kyber) 上找到。在 Linux 下，我们可以通过执行以下 shell 命令来克隆和构建框架。安装需要一些先决条件，这些条件在项目的 README 中有记录。

```py
git clone https://github.com/pq-crystals/kyber.git
cd kyber/ref && make
```

将参考实现集成到 Python 中有几种方法。其中一种是编写一个 C 程序并调用它。下面的 C 函数使用 Kyber 在两个虚构的实体 Alice 和 Bob 之间进行密钥交换。完整源代码请参见 [这里](https://github.com/c4ristian/encryption/blob/master/kyber/execute_round_trip.c)。

```py
#include <stddef.h>
#include <stdio.h>
#include <string.h>
#include "kem.h"
#include "randombytes.h"

void round_trip(void) {
    uint8_t pk[CRYPTO_PUBLICKEYBYTES];
    uint8_t sk[CRYPTO_SECRETKEYBYTES];
    uint8_t ct[CRYPTO_CIPHERTEXTBYTES];
    uint8_t key_a[CRYPTO_BYTES];
    uint8_t key_b[CRYPTO_BYTES];

    //Alice generates a public key
    crypto_kem_keypair(pk, sk);
    print_key("Alice' public key", pk);

    //Bob derives a secret key and creates a response
    crypto_kem_enc(ct, key_b, pk);
    print_key("Bob's shared key", key_b);
    print_key("Bob's response key", ct);

    //Alice uses Bobs response to get her shared key
    crypto_kem_dec(key_a, ct, sk);
    print_key("Alice' shared key", key_a);
}
```

不深入细节，可以看出 Kyber 使用了多个公钥和私钥。在上述示例中，Alice 生成了一个公钥 (pk) 和一个私钥 (sk)。接下来，Bob 使用公钥 (pk) 推导出共享密钥 (key_b) 和响应密钥 (ct)。后者被返回给 Alice。最后，Alice 使用响应密钥 (ct) 和她的私钥 (sk) 生成共享密钥的实例 (key_a)。只要双方保持私钥和共享密钥的机密性，该算法就能提供保护。在运行程序时，我们会得到类似于下面的文本输出。

```py
Alice' public key: F0476B9B5867DD226588..
Bob's shared key: ADC41F30B665B1487A51..
Bob's response key: 9329C7951AF80028F42E..
Alice' shared key: ADC41F30B665B1487A51..
```

为了在 Python 中调用 C 函数，我们可以使用 `subprocess` 模块。或者，可以构建一个共享库，并使用 `ctypes` 模块进行调用。第二种方法在下面的 Python 脚本中实现。在加载从 Kyber C 代码生成的共享库后，过程 `round_trip` 会像其他 Python 函数一样被调用。

```py
#!/usr/bin/env python
import os
import ctypes

# Load shared library
libname = f"{os.getcwd()}/execute_round_trip1024.so"
clib = ctypes.CDLL(libname, mode=1)
print("Shared lib loaded successfully:")
print(clib)

# Call round trip function
print("Executing round trip:")
clib.round_trip()
```

除了 Kyber 的参考实现外，其他提供商也实现了该算法。例如，开源项目 [Botan](https://botan.randombit.net/) 和 [Open Quantum Safe](https://openquantumsafe.org/)。

# 结论

我们的分析显示，量子技术仍处于早期阶段。但我们不应低估它对加密和其他密码学方法（如签名）构成的威胁。颠覆性创新随时可能推动发展。攻击者现在可以存储消息，稍后再解密。因此，应立即采取安全措施。尤其是，因为有可用的解决方法。正确使用时，像 AES-256 这样的对称算法被认为是量子抗性的。此外，像 Kyber 这样的非对称解决方案也在进展中。使用哪些替代方案取决于应用场景。遵循零信任模型，组合多种方法能提供最佳保护。这样，量子威胁可能会像 Y2K 问题一样，成为一种自我实现的预言。

# 关于作者

**Christian Koch** 是 BWI GmbH 的企业架构师，并且是纽伦堡技术学院 Georg Simon Ohm 的讲师。

**Lucie Kogelheide** 是 BWI GmbH 的后量子密码学技术主管，负责启动公司向量子安全密码学的迁移过程。

**Raphael Lorenz** 是 Lorenz Systems 的创始人兼首席信息安全官，专注于整体安全解决方案。

# 参考文献

1.  Snowden, Edward: *永久记录*。Macmillan, 2019。

1.  国家标准与技术研究院: [*NIST后量子密码学：常见问题*](https://csrc.nist.gov/Projects/post-quantum-cryptography/faqs)*.* 2023年6月29日。访问日期：2023年8月2日。

1.  联邦信息安全办公室 (BSI): [*量子安全密码学——基础知识、当前进展和建议*](https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/Publications/Brochure/quantum-safe-cryptography.pdf?__blob=publicationFile&v=4) *(PDF)*。2021年10月。访问日期：2023年8月2日。

1.  CRYSTALS — 代数格加密套件：[*Kyber Home*](https://pq-crystals.org/kyber/index.shtml)*.* 2020年12月。访问时间：2023年8月2日。

# 免责声明

请注意，信息安全是一个关键话题，作者对发布的内容不提供任何担保。
