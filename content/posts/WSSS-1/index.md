---
title: "WSSS-1"
date: 2024-01-25T09:42:57+08:00
draft: false
tags: [WSSS]
---

## 一些Vision Transformer 相关 WSSS 的论文

### [WeakTr](https://github.com/hustvl/WeakTr): Exploring Plain Vision Transformer for Weakly-supervised Semantic Segmentation

- 本文介绍了WeakTR，这是一个使用普通视觉转换器（ViT）进行弱监督语义分割（WSSS）的框架。 
- 它探讨了ViT中不同的注意力头如何聚焦于不同的图像区域，并提出了一种估计其重要性的方法。 
- 设计了一种新的自适应注意力融合策略，用于生成高质量的职业激活地图 (CAM)。 -本文介绍了一种基于VIT的梯度剪辑解码器，用于使用CAM结果进行在线再训练。 
- 本文还讨论了梯度剪切解码器的有效性以及在线再训练的上限。 
- 本文的方法使用变压器编码器的自注意力映射来完善粗略的 CAM。 
- 自适应注意力融合模块采用端到端训练策略来优化最终注意力结果。 
- 将WeakTR的训练时间与mcTFormer进行了比较，显示出在提高准确性的同时显著缩短了时间。

![](vx_images/121060710240166.png)

如图3所示，我们的框架使用普通的ViT作为主干。首先，我们将输入图像分割成$N^2$个补丁，将它们展平，并将它们线性映射到$N^2$个补丁标记。此外，我们生成$C$个可学习的类标记，其中$C$表示分类类别的总数，并将它们与补丁标记连接起来，作为变压器编码器的输入$T_{i n} \in \mathbb{R}^{\left(C+N^2\right) \times D}$，其中$D$是输入标记的维度。

变压器编码器内部由$K$个编码层组成。每个层包含两个子层：多头自注意力（MSA）和多层感知机（MLP）。在每个子层之前应用层归一化（LN），在每个子层之后应用残差连接。在每个编码层中，我们输入标记$T_{i n}$并接收$T_{\text {out }}$。$T_{\text {out }}$成为下一个编码器层的新$T_{\text {in }}$，以此类推，共进行$K$次迭代。

#### 3.1.2 自适应注意力融合的直接CAM生成

经过$K$个变压器层后，这些标记被排列成最终的标记$T_{\text {final }} \in \mathbb{R}^{\left(C+N^2\right) \times D}$。为了首先获得粗糙的CAM，我们需要从$T_{\text {final }}$中提取最后的$N^2$个补丁标记。然后，我们排列这$N^2$个补丁标记，使它们成为$T_{\text {final-patches }} \in$ $\mathbb{R}^{N \times N \times D}$。接下来，我们使用卷积层获得$C A M_{\text {coarse }} \in \mathbb{R}^{N \times N \times C}$，如下所示：
$$
\begin{aligned}
T_{\text {final-patches }} & =\operatorname{Arrange}\left(T_{\text {final }}\left[C+1: C+N^2\right]\right) \\
C A M_{\text {coarse }} & =\operatorname{Conv}\left(T_{\text {final-patches }}\right) .
\end{aligned}
$$

在获得粗糙的CAM之后，我们使用变压器编码器的自注意力映射对其进行细化。单个自注意力映射的形状为$\left(C+N^2\right)^2$，允许我们获取$C$个类标记对于$N^2$个补丁标记的交叉注意力映射，以及$N^2$个补丁标记相对于它们自身的补丁注意力映射。交叉注意力映射的形状为$N \times N \times C$，而补丁注意力映射的形状为$N^2 \times N^2$。考虑到变压器编码器具有$K$个编码层，每个层有$H$个注意力头，我们可以将交叉注意力映射表示为$C A \in \mathbb{R}^{(K \times H) \times N \times N \times C}$，将补丁注意力映射表示为$P A \in \mathbb{R}^{(K \times H) \times N^2 \times N^2}$。

为了结合所有层中所有注意力头的表示，先前的弱监督语义分割（WSSS）方法[50, 25]直接对同一层中不同头的自注意力映射进行平均，然后在不同层之间进行求和。我们发现，这种对变压器注意力的平均-求和方法是基础的。如图2所示，大多数不同的注意力头集中在不同的区域和类别上。对于注意力头的这种不加区分的方法往往会给前景对象的激活图引入更多干扰。因此，我们提出利用自适应注意力融合模块来估计不同注意力头的重要性。

如图3所示，首先，我们获取与所有变压器层的注意力头相对应的自注意力映射$A \in \mathbb{R}^{(K \times H) \times\left(C+N^2\right) \times\left(C+N^2\right)}$。接下来，通过在头之间进行池化，得到相应的动态权重$W \in \mathbb{R}^{(K \times H) \times 1}$。然后，我们使用一个前馈神经网络（FFN）来相互作用动态权重的信息，如下所示：
$$
W^{\prime}=\operatorname{FFN}(\operatorname{Pooling}(A)),
$$

其中Pooling是全局平均池化，$W^{\prime} \in$ $\mathbb{R}^{(K \times H) \times 1}$是注意力头的相互作用权重。最后，我们将相互作用的权重$W^{\prime}$乘回交叉注意力映射$C A$和补丁注意力映射$P A$，分别得到$\widehat{C A}$和$\widehat{P A}$，如下所示：

$$
\begin{aligned}
& \widehat{C A}=\frac{1}{K H} \sum_{i=1}^{K \cdot H} W_i^{\prime} \cdot C A_i \\
& \widehat{P A}=\frac{1}{K H} \sum_{i=1}^{K \cdot H} W_i^{\prime} \cdot P A_i
\end{aligned}
$$

在这里，$\widehat{C A} \in \mathbb{R}^{N \times N \times C}$和$\widehat{P A} \in \mathbb{R}^{N^2 \times N^2}$是我们通过使用相互作用权重$W^{\prime}$从自注意力映射中得到的加权结果。我们采用了与MCTformer [50]和TransCAM [25]相同的方法来组合$C A M_{\text {coarse }}, \widehat{C A}$和$\widehat{P A}$：

$$
CAM_{\text {fine }}=\mathfrak{R}^{N \times N \times C}\left(\widehat{P A} \cdot \mathfrak{R}^{N^2 \times C}\left(C A M_{\text {coarse }} \odot \widehat{C A}\right)\right) \text {, }
$$


其中$C A M_{\text {fine }}$是由$\widehat{C A}$和$\widehat{P A}$引导的CAM，$\mathfrak{R}^{N^2 \times C}(\cdot)$是用于将矩阵重新形状为$N^2 \times C$的运算符，$\Re^{N \times N \times C}(\cdot)$是用于将矩阵重新形状为$N \times N \times C$的运算符，$\odot$表示Hadamard乘积。如表4和附录中的可视化所示，与平均-求和自注意力映射相比，加权自注意力映射为CAM提供了更准确的引导。

#### 3.1.3 端到端的Fine CAM训练

自适应注意力融合模块提供准确权重的关键在于我们的端到端训练策略。端到端生成Fine CAMs允许自适应注意力融合模块通过图像级别的监督学习进行训练。使用自注意力映射改进粗糙CAMs生成Fine CAMs的过程以及计算损失函数$L_{\text {Fine-CAM }}$是完全可微的。因此，用于分类的损失$L_{\text {Fine-CAM }}$可以为注意力映射的权重分配提供弱监督引导。在这个引导下，与目标对象匹配的注意力头，无论是在关注的类别还是关注的区域方面，都被鼓励分配更大的权重，而不匹配的则具有较小的权重。我们将$L_{F i n e-C A M}$添加到图3中的$L_{C L S-t o k e n}$和$L_{\text {Coarse-CAM }}$，得到总损失$\mathcal{L}$，如下所示：
$$
\mathcal{L}=L_{\text {Fine-CAM }}+L_{\text {CLS-token }}+L_{\text {Coarse-CAM }} .
$$

#### 3.2. WeakTr 在线重新训练与梯度裁剪解码器

![](vx_images/229674510258592.png)

传统上，在弱监督语义分割（WSSS）框架中，CAM的质量较低需要在重新训练之前经过CAM细化[2]阶段，这个过程可能既繁琐又耗时。我们提出的在线重新训练方法涉及ViT和一个梯度裁剪解码器。它可以直接使用CAM训练高性能的语义分割模型，避免了CAM细化的需要。

如图4所示，首先，我们将ViT编码器生成的类标记$Q \in \mathbb{R}^{C \times D}$和补丁标记$T \in \mathbb{R}^{N^2 \times D}$一起输入到变压器解码器层，得到相应的输出$\hat{Q} \in \mathbb{R}^{C \times D}$和$\hat{T} \in \mathbb{R}^{N^2 \times D}$。接下来，我们使用L2范数归一化的$\hat{Q}_{\text {norm }}$和$\hat{T}_{\text {norm }}$生成相应的预测序列。然后，我们通过LN和上采样将预测序列传递，得到预测$\hat{P} \in \mathbb{R}^{O \times O \times C}$，如下所示：

$$
\hat{P}=\operatorname{Upsampling}\left(\operatorname{LN}\left(\frac{\hat{T}_{\text {norm }} \cdot \hat{Q}_{\text {norm }}^{\mathrm{T}}}{\sqrt{D}}\right)\right),
$$

其中$(O, O)$是输入图像的原始分辨率。
在噪声标签问题中，一些研究[13, 3, 31]指出，具有较小梯度的样本更有可能被视为干净的样本。在语义分割任务中，我们可以将每个像素视为一个样本，并根据阈值确定是否在该像素处剪切梯度。我们的梯度裁剪解码器的目的是找到一个合适的阈值。为了确定梯度阈值，我们考虑了两个因素：整个图像的梯度和局部区域内的梯度。首先，我们使用整个图像所有像素的平均梯度值作为全局梯度约束。其次，我们将像素分为补丁以从补丁区域获取局部梯度约束，类似于ViT。梯度裁剪解码器充分考虑这两个约束来判断是否剪切每个像素的梯度。它剪切梯度较大的区域，使分割网络集中学习梯度较小的区域。

为了计算局部梯度约束，我们将预测$\hat{P} \in \mathbb{R}^{O \times O \times C}$分割成$L^2$个非重叠的补丁$\left\{\hat{P}_i\right\}, i \in\left\{1, \ldots, L^2\right\}$。$\left\{\hat{P}_i\right\}$中每个补丁的形状为$S \times S \times C$，其中$L=O / S$。请注意，$\left\{\hat{P}_i\right\}$中的补丁大小与ViT编码器中的图像补丁大小无关。利用从$C A M_{\text {fine }}$生成的CAM种子和预测补丁$\left\{\hat{P}_i\right\}$，我们可以计算梯度补丁$\left\{\hat{G}_i\right\}$。$\left\{\hat{G}_i\right\}$中的每个梯度补丁的大小为$S \times S$。同时，我们可以计算$\left\{\hat{G}_i\right\}$中每个梯度补丁的平均梯度$\left\{\lambda_i\right\}$，如下所示：
$$
\begin{aligned}
\hat{G}_i & =\mathrm{CE}\left(\hat{P}_i, C A M \text { seeds }_i\right), i \in\left\{1, \ldots, L^2\right\} \\
\lambda_i & =\operatorname{mean}\left(\hat{G}_i\right), i \in\left\{1, \ldots, L^2\right\},
\end{aligned}
$$
其中$\mathrm{CE}$是为每个像素计算的交叉熵损失。对于$\left\{\hat{G}_i\right\}$中的每个梯度补丁，我们使用$\left\{\lambda_i\right\}$作为局部约束，将$\left\{\lambda_i\right\}$的平均值作为全局约束$\lambda_{\text {global }}$。
$$
\lambda_{\text {global }}=\frac{1}{L^2} \sum_{i=1}^{L^2} \lambda_i
$$

我们选择$\lambda_{\text {global }}$和$\left\{\lambda_i\right\}$的最大值作为剪切掩码$\left\{\hat{M}_i\right\}$生成的阈值。通过这种方式，得到的$\left\{\hat{M}_i\right\}$同时考虑了局部和全局梯度约束，实现了丢弃相对较大梯度的补丁区域。
$$
\hat{M}_i=\left\{\begin{array}{ll}
1, & \hat{G}_{i,(j, k)} \leq \max \left(\lambda_i, \lambda_{\text {global }}\right) \\
0, & \hat{G}_{i,(j, k)}>\max \left(\lambda_i, \lambda_{\text {global }}\right)
\end{array},\right.
$$
其中$1 \leq j \leq S, 1 \leq k \leq S$，max是最大操作。

然而，在分割网络训练初期，通过梯度裁剪解码器选择自信的CAM区域不够可靠。因此，我们设置剪切的起始值$\tau$来确定是否进行剪切。只有当当前批次的全局平均梯度$\lambda_{\text {global }}$低于$\tau$时，我们才进行剪切，如下所示：
$$
\hat{G}_i^{\prime}=\left\{\begin{array}{ll}
\hat{G}_i \odot \hat{M}_i, & \lambda_{\text {global }} \leq \tau \\
\hat{G}_i, & \lambda_{\text {global }}>\tau
\end{array} .\right.
$$

最后，我们得到了掩码梯度补丁$\left\{\hat{G}_i^{\prime}\right\}$并反向传播它们的平均值。通过这样做，我们动态选择梯度较小的区域作为自信的CAM区域，以优先学习分割网络。在推断时，我们应用条件随机场（CRF）[19]来提高分割质量。




### [ViT-PCM:](https://github.com/deepplants/vit-pcm) Max Pooling with Vision Transformers reconciles class and shape in weakly supervised semantic segmentation

弱监督语义分割（WSSS）是关于在没有像素级标签监督的情况下对物体类别进行分割，并使用最简化的监督。最经济的监督是通过图像级别的类别标签，其中WSSS方法计算每个物体类别的伪掩码。为了测试WSSS方法的准确性，会使用一个监督分割网络，如DeepLab [7]，在设计的伪掩码上进行训练，并将诱导的准确性与完全监督的方法进行比较。分割任务，通过伪掩码标签进行监督，是一个验证任务，旨在展示计算的伪掩码质量。原则上，验证任务对所有方法都添加相等的改进。

到目前为止，基于图像级别类别标签的方法使用类激活图（CAM）[62]生成伪掩码。CAM是从多标签分类网络（如CNN）中获得的。然而，CAM在估计感兴趣类别的形状和定位方面存在局限性，这促使许多研究人员在基线伪掩码（BPM）（通常称为种子）和测试验证的最终伪掩码生成之间引入额外的细化。这些细化通常涉及到多阶段架构，正如PAMR [3]中所指出的那样。一些作者借助显著性图作为良好定位的辅助监督 [52,32,57,47,63]。其他作者采用图像操作，如区域擦除 [42,51] 或在训练期间扩展种子区域的区域生长 [26,23]，以及多尺度地图融合以改进背景和前景 [53]。Jang等人[25]通过属性传播方法[35]审查了CAM使用的特征层选择。Sun等人[45]通过两个共同关注网络估计相同类别的前景特征的相似性，以更好地捕捉背景。[19]则研究了跨不同图像的关系。

然而，在细化策略方面取得的最大成功是由IRNet [1]、PSA [2]和AdvCAM [30]取得的。最近也使用了PAC [44]和BENet [9]。例如，SEAM [50]、Chang等人[6]和[41]使用PSA；CONTA [14]和ReCAM [12]使用IRNet，而[30]同时使用两者。AFA [39]使用PAC [44]。CRF [27]在PascalVOC上进行了全监督训练，并由[26]引入WSSS。CRF作为训练循环之外的后处理，平均提高了Pascal VOC 2012的BPM的3-4％mIoU。另一方面，使用IRNet [1]、PSA [2]和AdvCAM [30]在训练循环中细化BPM的多阶段方法使用密集CRF，这在准确性上获得了实质性提升。使用在PascalVOC上优化的密集CRF，同样在细化循环中使用显著性（例如[22]，它也使用密集CRF）来获得最终的伪掩码，除了资源密集外，还无法将方法推广到PascalVOC数据集之外。这种缺乏泛化能力是使用在细化训练循环中的有偏方法的任何WSSS方法共有的问题。

挑战在于提高基线伪掩码准确性的水平，以便唯一的监督真正与图像级别标签相符。为此，我们引入了一种新型的计算伪掩码的模型，绕过CAM瓶颈。本文的主要贡献如下：
- 我们引入了一种基于ViT [15]的弱监督语义分割（WSSS）的新型模型。该模型被称为ViT-PCM，如图2所示。
- 我们提出了一种新的伪掩码计算方法，无需使用CAM，名为显式搜索。该方法利用ViT的局部性质，接近多标签分类和语义分割之间的有效映射。我们使用全局最大池化（GMP）来获取每个补丁的相关性，给定补丁在感兴趣类别上的分类分布。通过这种方式，我们使用多标签二元交叉熵损失（MCE）将补丁特征投影到类别预测（PCM）。我们确保对平移和缩放变换的等变性，定义了两个分支，见图2。
- 所提出的伪掩码计算方法优于所有最先进的方法：我们在Pascal VOC 2012训练集上获得了67.7％的BPM准确度，比当前最佳BPM（[39]）提高了3.91％。平均而言，我们比所有其他竞争对手提高了5％以上的mIoU。在MS-COCO 2014上，我们在验证集上获得了45.03％的mIoU。
- 对于验证任务，使用DeepLab作为分割方法，我们不需要提升我们的BPM以获得更适合DeepLab的掩码，但我们获得了可比较的验证和测试分数。
- 我们还证明了我们的方法在计算效果方面的优势。

特别是，我们使用了89.4M的参数大小获得了最终分割结果，这是竞争对手中最小的成本。

除了我们的贡献的新颖性，这是绕过CAM计算基线伪掩码的第一个提议，我们还展示了定量和定性结果都证明，探索基线伪掩码的新方法可能是有益的。我们在基线伪掩码计算方面建立了一个新的技术水平，使用图像级别的类别标签而无需细化。

![](vx_images/402123621266625.png)

![](vx_images/316343521246459.png)


















































