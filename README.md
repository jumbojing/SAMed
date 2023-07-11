SAMed
===

此存储库包含以下白皮书的实现：

> **Customized Segment Anything Model for Medical Image Segmentation** 医学图像分割的定制分割模型
> [张开东](https://hitachinsk.github.io/)、[刘冬](https://faculty.ustc.edu.cn/dongeliu/)
> 技术报告
> [\[论文\]](https://arxiv.org/pdf/2304.13785.pdf)

COLAB在线演示：[![在colab中打开](https://camo.githubusercontent.com/84f0493939e0c4de4e6dbe113251b4bfb5353e57134ffd9fcab6b8714514d4d1/68747470733a2f2f636f6c61622e72657365617263682e676f6f676c652e636f6d2f6173736574732f636f6c61622d62616467652e737667)](https://colab.research.google.com/drive/1KCS5ulpZasYl9DgJJn59WsGEB8vwSI_m?usp=sharing)

[![](https://github.com/jumbojing/SAMed/raw/main/materials/teaser.png)](https://github.com/jumbojing/SAMed/blob/main/materials/teaser.png)

⭐新闻
---

-   由于我的老板的高额投资，我可以微调 SAM 的`vit_h`版本以获得更准确的医学图像分割。现在，我们发布了 SAMed 的`vit_h`版本（我们将此版本表示为 SAMed\_h），SAMed 和 SAMed\_h 之间的比较如下表所示。

| 型 | DSC | 硬盘 | 主动脉 | 胆囊 | 肾脏 （L） | 肾脏 （R） | 肝 | 胰腺 | 脾 | 胃 |
| --- |  --- |  --- |  --- |  --- |  --- |  --- |  --- |  --- |  --- |  --- |
| SAMed | 81.88 | 20.64 | 87.77 | 69.11 | 80.45 | 79.95 | 94.80 | **72.17** | 88.72 | 82.06 |
| --- |  --- |  --- |  --- |  --- |  --- |  --- |  --- |  --- |  --- |  --- |
| SAMed\_h | **84.30** | **16.02** | **87.81** | **74.72** | **85.76** | **81.52** | **95.76** | 70.63 | **90.46** | **87.77** |

没有花里胡哨的东西，SAMed\_h实现了比SAMed更高的**性能**。虽然`vit_h`版本的模型大小比`vit_h`版本 （~350M）要大得多 （高于 2G）， SAMed\_h的 LoRA 检查点并没有增加很多 （从 18M 到 21M）.因此，**SAMed\_h的部署和存储成本几乎与 SAMed 相当**。由于业界更喜欢部署更大、性能更好的模型，我们认为SAMed\_h在实践中计算机辅助诊断和术前计划更有希望。有关SAMed\_h的更多详细信息，请访问[SAMed\_h目录](https://github.com/jumbojing/SAMed/blob/main/SAMed_h)。

概述
--

[![](https://github.com/jumbojing/SAMed/raw/main/materials/pipeline.png)](https://github.com/jumbojing/SAMed/blob/main/materials/pipeline.png)

我们提出了SAMed，一种用于医学图像分割的通用解决方案。与以往的方法不同，SAMed基于大规模图像分割模型"分割万物（SAM）"，探索定制大规模医学图像分割模型的新研究范式。SAMed 将基于低秩 （LoRA） 的微调策略应用于 SAM 图像编码器，并将其与提示编码器和掩模解码器一起微调标记的医学图像分割数据集。我们还观察到预热微调策略和AdamW优化器引导SAMed成功收敛并降低损耗。与SAM不同，SAMed可以对医学图像进行语义分割。我们经过训练的SAMed模型在Synapse多器官分割数据集上实现了81.88 DSC和20.64 HD，这与最先进的方法相当。我们进行了广泛的实验来验证我们设计的有效性。由于 SAMed 只更新一小部分 SAM 参数，因此其部署成本和存储成本在实际使用中相当微不足道。

待办事项列表
------

- [] 制作演示
- [] 对更多数据集进行微调
- [] ~~根据 SAM 的`vit_l`或`vit_h`模式的SAMed~~

先决条件
----

- `Linux`（我们在`Ubuntu 18.04上`测试了我们的代码）
- `Anaconda`
- `python 3.7.11`
- `Pytorch 1.9.1`

要开始使用，请先克隆存储库

```
git clone https://github.com/hitachinsk/SAMed.git

```

然后，请运行以下命令：

```
conda create -n SAMed python=3.7.11
conda activate SAMed
pip install -r requirements.txt

```

如果你有原始 Synapse 数据集，我们将提供[预处理脚本](https://github.com/jumbojing/SAMed/blob/main/preprocess)来处理和规范化数据以进行训练。有关更多详细信息，请参阅此文件夹。

快速入门
----

我们强烈建议您尝试我们的在线演示[![在科拉布中打开](https://camo.githubusercontent.com/84f0493939e0c4de4e6dbe113251b4bfb5353e57134ffd9fcab6b8714514d4d1/68747470733a2f2f636f6c61622e72657365617263682e676f6f676c652e636f6d2f6173736574732f636f6c61622d62616467652e737667)](https://colab.research.google.com/drive/1KCS5ulpZasYl9DgJJn59WsGEB8vwSI_m?usp=sharing)。

目前，我们提供SAMed和SAMed\_s模型，以快速复制我们的结果。LoRA 检查点及其相应的配置如下表所示.

| 型 | 检查站 | 配置 | DSC | 硬盘 |
| --- |  --- |  --- |  --- |  --- |
| SAMed | [链接](https://drive.google.com/file/d/1P0Bm-05l-rfeghbrT1B62v5eN-3A-uOr/view?usp=share_link) | [中新](https://drive.google.com/file/d/1pTXpymz3H6665hjztkv-A7uG_rzSWPVg/view?usp=sharing) | 81.88 | 20.64 |
| --- |  --- |  --- |  --- |  --- |
| SAMed\_s | [链接](https://drive.google.com/file/d/1rQM2md-h66RlRF3wC0m9N8aheOCvKfYv/view?usp=share_link) | [中新](https://drive.google.com/file/d/1x72rB-oNtZ-ZoD_yfOnWdowSb02FMUjT/view?usp=sharing) | 77.78 | 31.72 |

以下是说明：

1.  将目录更改为此存储库的根目录。2.  请下载预训练的[SAM模型](https://drive.google.com/file/d/1_oCdoEEu3mNhRfFxeWyRerOKt8OEUvcg/view?usp=share_link) （由SAM的原始存储库提供） 和[SAMed的LoRA检查点](https://drive.google.com/file/d/1P0Bm-05l-rfeghbrT1B62v5eN-3A-uOr/view?usp=share_link).将它们放在文件夹中。`./checkpoints`3.  请下载[测试集](https://drive.google.com/file/d/1RczbNSB37OzPseKJZ1tDxa5OO1IIICzK/view?usp=share_link)并将其放入 ./testset 文件夹中。然后，解压缩并删除此文件。4.  运行此推荐以测试 SAMed 的性能。

```
python test.py --is\_savenii --output\_dir <Your output directory\>
```

如果一切正常，您可以发现平均DSC为0.8188（81.88），HD为20.64，对应于论文的Tab.1。并检查`<Your output directory>`中的测试结果。

更重要的是， 我们还提供[SAMed\_s模型](https://drive.google.com/file/d/1rQM2md-h66RlRF3wC0m9N8aheOCvKfYv/view?usp=share_link)， 它利用 LoRA 微调图像编码器和掩模解码器中的变压器块.与SAMed相比，SAMed\_s具有更小的模型尺寸，但性能也略有下降。如果要使用此模型，请下载并将其放入`./checkpoints_s`文件夹中，然后运行以下命令以测试其性能。

```
python test.py --is\_savenii --output\_dir <Your output directory\> --lora\_ckpt checkpoints\_s/epoch\_159.pth --module sam\_lora\_image\_encoder\_mask\_decoder
```

SAMed\_s的平均DSC为0.7778（77.78），HD为31.72，对应于论文的Tab.3。

训练
--

我们使用 2 个 RTX 3090 GPU 进行训练。

1. 请下载处理后的[训练集](https://drive.google.com/file/d/1zuOQRyfo0QYgjcU_uZs0X3LdCnAC2m3G/view?usp=share_link)，其分辨率为`224x224`，并将其放在`＜您的文件夹＞`中。然后，解压缩并删除此文件。我们还准备[训练集](https://drive.google.com/file/d/1F42WMa80UpH98Pw95oAzYDmxAAO2ApYg/view?usp=share_link)在参考分辨率为512x512的情况下，训练集的224x224版本是从512x512版本下采样的。
2.  运行此命令以训练 SAMed。

```
python train.py --root\_path <Your folder\> --output <Your output path\> --warmup --AdamW
```

检查`<Your output path>`中的结果。

许可证
---

本作品在 MIT 许可下获得许可。有关详细信息，请参阅[许可证](https://github.com/jumbojing/SAMed/blob/main/LICENSE)。

引文
--

如果我们的工作启发了您的研究，或者代码的某些部分对您的工作有用，请引用我们的论文：

```
@article{samed,
  title\={Customized Segment Anything Model for Medical Image Segmentation},
  author\={Kaidong Zhang, and Dong Liu},
  journal\={arXiv preprint arXiv:2304.13785},
  year\={2023}
}
```

联系
--

如果您有任何疑问，请通过以下方式与我们联系

-   <richu@mail.ustc.edu.cn>

确认
--

我们感谢[Segment Anything模型](https://github.com/facebookresearch/segment-anything)的开发人员和[Synapse多器官分割数据集](https://www.synapse.org/#!Synapse:syn3193805/wiki/217789)的提供者。SAMed的代码建立在[TransUnet](https://github.com/Beckschen/TransUNet)和[SAM LoRA](https://github.com/JamesQFreeman/Sam_LoRA)之上，我们对这些很棒的项目表示感谢。
