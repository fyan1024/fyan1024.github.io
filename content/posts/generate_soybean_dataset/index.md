---
title: "Generate_soybean_dataset"
date: 2023-12-12T17:30:20+08:00
draft: false
---

[toc]

# 2023 年秋季学期深度学习实践结课作业——基于深度学习的高通量大豆籽粒表型检测问题

## 一、问题背景
    
大豆是世界上植物蛋白和油脂的首要供应来源作物之一。对于大豆育种而言，高通量表型检测能够加速大豆育种进程，对设计育种研究具有重要的指导价值。因此，如何快速、精确、高通量地对大豆籽粒进行表型检测，进而对其形态性状等指标进行评价，便能够指导优良大豆品种的培育和筛选。传统的大豆籽粒表型检测通常依赖于人工测量，具有人力成本高、准确率较低、提取参数有限等缺点，无法全面、高效地评估大豆籽粒的性状，难以满足大规模高效的数据采集和分析需求。为了解决这一问题，可以通过构建一种基于深度学习的高通量大豆籽粒表型检测方法，可提取表型参数包括大豆籽粒的长度、宽度、种皮面积、种脐长、种脐宽、种脐面积等重要表型参数，并且对种皮裂纹（裂纹扩大后会导致部分种皮脱落，种皮脱落凹陷的部分不属于裂纹）、种皮纹理等反映种子质量的外观指标进行初步评估

![](vx_images/573933816231255.png)

## 二、数据集说明

高通量大豆籽粒规格化图像数据集“soybean_dataset”内含有 400 张俯视拍摄的高通量大豆籽粒图像（以下简称图像）。每张图像中均包含有 10×10 规格化排布的 100 颗大豆籽粒，同一张图片中的大豆籽粒均为相同年份、相同地点、相同品种。每张图片的分辨率均为 1276 px×1276 px，现实中的 1mm 约为图片中的 12 个像素。

## 三、任务描述

### 【必做部分】

利用给定的图像数据集，训练、优化并评估大豆籽粒表型检测的深度学习模型，并比较不同模型之间的差异，最后基于最优模型完成对籽粒长、籽粒宽、横截面积、横截周长、圆度、种脐长、种脐宽、种脐面积、种脐周长等表型的提取。

参考步骤如下：
1. 数据预处理、数据标注、数据集划分；
2. 对大豆籽粒、种脐部位做检测/分割模型的训练、优化、对比和评估；
3. 根据模型训练结果，计算得到测试图像中每颗大豆籽粒各项表型参数，并以表格形式汇总导出结果。

### 【选做部分】
任选下列一项完成即可：
1. 选做一：尽可能对大豆籽粒图像中较明显的种皮裂纹进行目标检测，要求能够输出图像的种皮裂纹总数量及占比。
    提示：裂纹不规则且目标较精细，且裂纹与种皮脱落之间可能难以区分。
2. 选做二：尽可能地检测出大豆籽粒图像中存在较明显的种皮纹理的大豆籽粒（如褐色斑纹、灰紫色斑纹等），输出测试图像中带明显纹理的大豆籽粒数量。
    提示：大豆的纹理较复杂，种皮脱落部分也易被错误识别为纹理。
3. 选做三：尝试得到每个大豆籽粒的种皮颜色（不含种脐部分）、种脐颜色。
    提示：需要自行构建特定部位的颜色提取算法，尽可能地表征出种皮和种脐的真实颜色，分析该算法的可行性、鲁棒性。
4. 选做四：在必做部分的基础上，选取其他你认为有价值的相关方向进行深入研究，需使用深度学习相关方法，内容不限。

## 四、作业要求
3 位同学结成一组，最后一次课进行 10min 左右的大作业汇报。各组组长将汇报 PPT、项目文档、源代码（可不含数据集）、结果表格等文件于 2023 年 12月 24 日 24:00 前打包发送至邮箱 18810023758@163.com。

## 五、Supplementary:    

### 5.1 必做：籽粒长、籽粒宽、横截面积、横截周长、圆度

#### 5.1.1 数据集：大豆籽粒自动标注

> 链接：https://pan.baidu.com/s/1TohcW6ChlXnw6b1sAyygtA?pwd=ldaq 
提取码：ldaq 
--来自百度网盘超级会员V3的分享

标注标签在深度学习中是一项非常耗时和繁琐的任务。在全监督式学习中，模型通常需要大量的标记数据来训练，这意味着数据集中的每个样本都需要手动标记其对应的标签或类别。例如，在计算机视觉任务中，需要对图像进行标记，而在自然语言处理中，需要对文本进行分类或命名实体识别等任务。

计算机视觉分为图像分类、目标检测、图像分割、图像生成等一系列任务。在语义分割任务中，标注数据尤其耗时。语义分割是计算机视觉中的一项任务，旨在对图像中的每个像素进行分类，将其分配到对应的语义类别中。与简单的图像分类或目标检测任务相比，语义分割要求对图像中每个像素进行精确的标注，以区分不同的对象或区域，这增加了标注数据的复杂性和耗时性。

对图像进行语义分割标注通常需要人工绘制像素级的边界框或掩码，以标识图像中不同对象或区域的边界。这种精细级别的标注需要更多的时间和专业知识，因为标注者需要精确理解图像中不同区域的语义信息，并正确地为每个像素分配适当的标签。深度学习需要大量精细标注的数据作为“燃料”。例如据 Cityscapes 官方，标注一张该数据集中的语义分割平均需要 1.5小时。降低获得数据集的成本是我们不得不考虑的因素。

![Cityscapes](vx_images/154413114231262.png)

由于语义分割所需的像素级标注耗时且复杂，因此寻求减少标注需求的方法变得尤为重要。使用半监督学习、生成模型、迁移学习或利用合成数据等技术可以帮助减少对大量完整标记数据的依赖，从而降低语义分割任务中标注的时间和成本。

在本次作业中我们同过结合数字图像处理方法和视觉基础模型（[segment-anything](https://github.com/facebookresearch/segment-anything)）实现大豆籽粒的自动标注，获得目标检测和语义/实例分割标注，并存储为Yolo格式。

具体实现方式如下：

##### 5.1.1.1 确定大豆边界

首先使用边缘检测在经过高斯滤波去噪的大豆籽粒灰度图像中寻找到大豆边界。

 ![](vx_images/10170017249681.png)

##### 5.1.1.2 计算大豆质心

而后近似计算得到大豆质心。

 ![](vx_images/416900417257714.png)
 
![](vx_images/108870517250383.png)

##### 5.1.1.3 利用质心输入SAM获得掩码

将修正后的质心作为point prompt输入[segment-anything](https://github.com/facebookresearch/segment-anything)得到掩码，取掩码轮廓得到边界框和像素级别分割标签。共37972颗大豆，其中手动调整质心修正25颗。

![](vx_images/338100917260572.png)

![](vx_images/413040917258176.png)

##### 5.1.1.4 转换标签

将经过视觉基础模型的掩码转换成Yolo格式的目标检测框标签和像素级实例掩码标签

###### 目标检测框

![](vx_images/5230817246938.png)


###### 像素级掩码标签

![](vx_images/205490817242692.png)

#### 5.1.2 计算籽粒表型参数

计算获得籽粒长、籽粒宽、横截面积、横截周长、圆度

```python
import cv2
import numpy as np

# 读取二值化的图片
image = cv2.imread('ZS110003_19_G_bbox_mask_6.png', cv2.IMREAD_GRAYSCALE)

# 找到轮廓
contours, _ = cv2.findContours(image, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

# 假设只有一个轮廓
contour = contours[0]

# 计算面积和周长
area = cv2.contourArea(contour)
perimeter = cv2.arcLength(contour, True)

# 转换周长和面积为毫米单位
scale = 12  # 12像素 = 1mm
perimeter_mm = perimeter / scale
area_mm2 = area / (scale ** 2)

# 计算圆度
circularity = (4 * np.pi * area) / (perimeter ** 2)

# 输出面积、周长、圆度
print(f"面积: {area_mm2} 毫米^2")
print(f"周长: {perimeter_mm} 毫米")
print(f"圆度: {circularity}")

# 可视化轮廓
contour_image = np.zeros_like(image)
cv2.drawContours(contour_image, [contour], 0, 255, 1)
cv2.imshow('Contour Image', contour_image)
cv2.waitKey(0)
cv2.destroyAllWindows()

# 获取外接矩形的宽度和高度
(_, _), (width_pixel, height_pixel), _ = cv2.minAreaRect(contour)

# 计算实际长度和宽度
length_mm = width_pixel / scale
width_mm = height_pixel / scale

# 输出长度和宽度
print(f"长度: {length_mm} 毫米")
print(f"宽度: {width_mm} 毫米")

```

### 5.2 必做：种脐长、种脐宽、种脐面积、种脐周长

#### 5.2.1 YoloV8目标检测模型

YOLOv8 是 Ultralytics 公司在 2023 年 1月 10 号开源的 YOLOv5 的下一个重大更新版本，目前支持图像分类、物体检测和实例分割任务，在还没有开源时就收到了用户的广泛关注。

按照官方描述，YOLOv8 是一个 SOTA 模型，它建立在以前 YOLO 版本的成功基础上，并引入了新的功能和改进，以进一步提升性能和灵活性。具体创新包括一个新的骨干网络、一个新的 Ancher-Free 检测头和一个新的损失函数，可以在从 CPU 到 GPU 的各种硬件平台上运行。 不过 Ultralytics 并没有直接将开源库命名为 YOLOv8，而是直接使用 Ultralytics 这个词，原因是 Ultralytics 将这个库定位为算法框架，而非某一个特定算法，一个主要特点是可扩展性。其希望这个库不仅仅能够用于 YOLO 系列模型，而是能够支持非 YOLO 模型以及分类分割姿态估计等各类任务。 总而言之，Ultralytics 开源库的两个主要优点是：

- 融合众多当前 SOTA 技术于一体
- 未来将支持其他 YOLO 系列以及 YOLO 之外的更多算法


![Yolov8](vx_images/148731413231263.jpg)


YOLOv8 算法的核心特性和改动可以归结为如下：
1. 提供了一个全新的 SOTA 模型，包括 P5 640 和 P6 1280 分辨率的目标检测网络和基于 YOLACT 的实例分割模型。和 YOLOv5 一样，基于缩放系数也提供了 N/S/M/L/X 尺度的不同大小模型，用于满足不同场景需求
2. 骨干网络和 Neck 部分可能参考了 YOLOv7 ELAN 设计思想，将 YOLOv5 的 C3 结构换成了梯度流更丰富的 C2f 结构，并对不同尺度模型调整了不同的通道数，属于对模型结构精心微调，不再是无脑一套参数应用所有模型，大幅提升了模型性能。不过这个 C2f 模块中存在 Split 等操作对特定硬件部署没有之前那么友好了
3. Head 部分相比 YOLOv5 改动较大，换成了目前主流的解耦头结构，将分类和检测头分离，同时也从 Anchor-Based 换成了 Anchor-Free
4. Loss 计算方面采用了 TaskAlignedAssigner 正样本分配策略，并引入了 Distribution Focal Loss
5. 训练的数据增强部分引入了 YOLOX 中的最后 10 epoch 关闭 Mosiac 增强的操作，可以有效地提升精度

从上面可以看出，YOLOv8 主要参考了最近提出的诸如 YOLOX、YOLOv6、YOLOv7 和 PPYOLOE 等算法的相关设计，本身的创新点不多，偏向工程实践，主推的还是 ultralytics 这个框架本身。

##### 5.2.1.1 Yolov8 结构设计

骨干网络和 Neck 的具体变化为：

1. 第一个卷积层的 kernel 从 6x6 变成了 3x3
2. 所有的 C3 模块换成 C2f，结构如下所示，可以发现多了更多的跳层连接和额外的 Split 操作
3. 去掉了 Neck 模块中的 2 个卷积连接层
4. Backbone 中 C2f 的 block 数从 3-6-9-3 改成了 3-6-6-3
5. 查看 N/S/M/L/X 等不同大小模型，可以发现 N/S 和 L/X 两组模型只是改了缩放系数，但是 S/M/L 等骨干网络的通道数设置不一样，没有遵循同一套缩放系数。如此设计的原因应该是同一套缩放系数下的通道设置不是最优设计，YOLOv7 网络设计时也没有遵循一套缩放系数作用于所有模型

![YOLOv5 和 YOLOv8 模块对比](vx_images/562035715260580.png)

Head 部分变化最大，从原先的耦合头变成了解耦头，并且从 YOLOv5 的 Anchor-Based 变成了 Anchor-Free。其结构如下所示：

![Head结构](vx_images/76080116258184.png)

可以看出，不再有之前的 objectness 分支，只有解耦的分类和回归分支，并且其回归分支使用了 Distribution Focal Loss 中提出的积分形式表示法。

##### 5.2.1.2 Loss 计算

Loss 计算过程包括 2 个部分：正负样本分配策略和 Loss 计算。现代目标检测器大部分都会在正负样本分配策略上面做文章，典型的如 YOLOX 的 simOTA、TOOD 的 TaskAlignedAssigner 和 RTMDet 的 DynamicSoftLabelAssigner，这类 Assigner 大都是动态分配策略，而 YOLOv5 采用的依然是静态分配策略。考虑到动态分配策略的优异性，YOLOV8 算法中则直接引用了 TOOD 的 TaskAlignedAssigner。TaskAlignedAssigner 的匹配策略简单总结为：根据分类与回归的分数加权的分数选择正样本。

$$
t=s^\alpha+u^\beta
$$

$s$ 是标注类别对应的预测分值，u 是预测框和 gt 框的 iou，两者相乘就可以衡量对齐程度。

1. 对于每一个 ${GT}$ ，对所有的预测框基于 ${GT}$ 类别对应分类分数，预测框与 ${GT}$ 的  $iou$ 的加权得到一个关联分类以及回归的对齐分数 alignment_metrics
2. 对于每一个 GT，直接基于 alignment_metrics 对齐分数选取 topK 大的作为正样本

Loss 计算包括 2 个分支：分类和回归分支，没有了之前的 objectness 分支。
- 分类分支依然采用 BCE Loss 
- 回归分支需要和 Distribution Focal Loss 中提出的积分形式表示法绑定，因此使用了 Distribution Focal Loss，同时还使用了 CloU Loss

3 个 Loss 采用一定权重比例加权即可。

##### 5.2.1.3 目标检测评价指标

训练完成后，对模型的性能进行评估。本文使用 Precision（精确率）、Recall（召回率）、mAP（平均精度均值）和FPS（每秒帧数）：

1. Precision（精确率）：
    精确率是指模型在预测为正类的样本中，真正为正类的比例。数学公式为：Precision = TP / (TP + FP)，其中 TP 是真正例（模型正确预测为正类的样本数），FP 是假正例（模型错误预测为正类的样本数）。精确率衡量了模型预测为正类的准确性。
2. Recall（召回率）：
    召回率是指模型正确预测为正类的样本数占真正为正类的样本数的比例。数学公式为：Recall = TP / (TP + FN)，其中 TP 是真正例，FN 是假负例（模型错误预测为负类的样本数）。召回率衡量了模型找到正类样本的能力。
3. mAP（平均精度均值）：
    mAP 是目标检测中常用的指标之一，它代表了在不同类别下的平均精度。对于每个类别，我们计算其 Precision-Recall 曲线下的面积（AP，平均精度），然后取所有类别的 AP 的平均值。mAP 能更全面地评估模型在各个类别上的检测性能。
4. FPS（每秒帧数）：
    FPS 是指模型处理图像的速度，即每秒处理的图像帧数。在目标检测中，除了模型的准确性，速度也是一个重要指标。较高的 FPS 意味着模型能够更快地处理图像，适用于实时应用。

其中P，R，AP的计算公式如下：

$$P=\frac{T P}{TP+FP}$$

$$R=\frac{T P}{T P+F N}$$


$$A P=\int_0^1 P(R) d R$$

#### 5.2.2 数据和数据增强

分别使用了具有种脐和裂纹的单个籽粒和整盘籽粒的目标检测数据。

#### 5.2.2.1 数据

具有种脐和裂纹的单个籽粒目标检测数据来自第八组，训练集含723颗大豆，验证203张，测试集103张。

#### 5.2.2.2 数据

具有种脐和裂纹的单个籽粒目标检测数据来自第七组，数据集共150张。


#### 5.2.2.3 数据增强

数据增强是在训练神经网络时用于增加训练样本多样性的一种技术。它通过对原始图像进行变换或操作，生成新的训练样本。首先使用几种常见的数据增强方式进行数据增强，包括：

- 水平翻转（Horizontal Flip）：沿着垂直轴翻转图像。例如，一张人脸朝右的图像经过水平翻转后就会变成朝左的图像。
- 旋转（Rotate）：对图像进行旋转，使图像绕着中心点旋转一定角度。旋转可以是任意角度，比如90度、180度等。
- 垂直翻转（Vertical Flip）：沿着水平轴翻转图像。与水平翻转类似，但是是垂直方向上的翻转。
- 马赛克（Mosaic）：将图像分割成网格，然后随机选取一个网格并用该网格中的像素值覆盖其他网格的像素值。这种方法通常用于提高模型对部分遮挡的物体的识别能力。

![](vx_images/60633515249689.png)

![](vx_images/501513915242700.png)

#### 5.2.3 单个籽粒目标检测

##### 5.2.3.1 Yolov8n和轻量化模型


| Models                    | Class   | Precision | Recall | mAP    | FPS |
| :------------------------- | :------ | :---------- | :------ | :----- | :-- |
| YoloV8n                   | All     | 0.74       | 0.656   | 0.728 | 153 |
|                            | Hilum   | 0.813      | 0.646   | 0.783 |      |
|                            | Crack   | 0.666      | 0.666   | 0.672 |      |
| YoloV8-mobilevit         | All     | 0.474      | 0.533   | 0.465 | 135 |
|                            | Hilum   | 0.54       | 0.554   | 0.565 |      |
|                            | Crack   | 0.408      | 0.511   | 0.365 |      |
| YoloV8-ghost              | All     | 0.682      | 0.649   | 0.675 | 714 |
|                            | Hilum   | 0.811      | 0.653   | 0.774 |      |
|                            | Crack0 | 0.554      | 0.644   | 0.575 |      |
| YoloV8-ghost-p2          | All     | 0.663      | 0.697   | 0.567 | 500 |
| 包含了P2层，有四个输出层 | Hilum   | 0.75       | 0.854   | 0.579 |      |
| 提升了小目标检测能力      | Crack   | 0.575      | 0.54    | 0.556 |      |
| YoloV8-ghost-p6          | All     | 0.71       | 0.683   | 0.684 | 714 |
| 包含了P6层，有四个输出层 | Hilum   | 0.806      | 0.66    | 0.763 |      |
| 针对高分辨率图像          | Crack   | 0.613      | 0.705   | 0.605 |      |


> GhostNet失去效果，可能是数据集不够大导致的，Ghost模块无法提取到足够多相似的特征对。
Han, Kai et al. “GhostNet: More Features From Cheap Operations.” 2020 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR) (2019): 1577-1586.
![](vx_images/442641720263613.png)

计算bbox得到种脐表型参数

```python
import cv2
import numpy as np
import pandas as pd
# 读取txt文件获取bbox信息
def read_bbox_info(txt_file):
    with open(txt_file, 'r') as file:
        lines = file.readlines()
        bbox_info = [list(map(float, line.strip().split())) for line in lines]
    return bbox_info

# 计算椭圆参数（中心点、长短轴长度、旋转角度）
def get_ellipse_parameters(bbox):
    x, y, w, h = bbox
    center_x = x + w / 2
    center_y = y + h / 2
    return (center_x, center_y), (w / 2, h / 2), 0

# 将归一化坐标转换为绝对坐标
def normalize_bbox_to_absolute(bbox, image_shape):
    img_h, img_w = image_shape[:2]
    x, y, w, h = bbox
    x_abs = int(x * img_w)
    y_abs = int(y * img_h)
    w_abs = int(w * img_w)
    h_abs = int(h * img_h)
    return x_abs, y_abs, w_abs, h_abs

def calculate_actual_dimensions(bbox, image_shape, scale=12):
    x, y, w, h = bbox
    img_h, img_w = image_shape[:2]

    x_abs, y_abs, w_abs, h_abs = normalize_bbox_to_absolute(bbox, image_shape)

    actual_width_mm = w * img_w / scale
    actual_height_mm = h * img_h / scale

    return actual_width_mm, actual_height_mm

def calculate_ellipse_metrics(bbox, image_shape, scale=12):
    center, axes, angle = get_ellipse_parameters(bbox)

    # Calculate the actual dimensions in millimeters
    actual_width_mm, actual_height_mm = calculate_actual_dimensions(bbox, image_shape, scale)

    # Calculate the area of the inscribed ellipse
    area = np.pi * axes[0] * axes[1]

    # Calculate the perimeter of the inscribed ellipse
    perimeter = 2 * np.pi * np.sqrt((axes[0] ** 2 + axes[1] ** 2) / 2)

    # Convert perimeter and area to millimeter units
    perimeter_mm = perimeter / scale
    area_mm2 = area / (scale ** 2)

    return actual_width_mm, actual_height_mm, perimeter_mm, area_mm2

# 读取图像和bbox信息
image_file = 'ZS110376_19_G_bbox_3.jpg'
txt_file = 'ZS110376_19_G_bbox_3.txt'

image = cv2.imread(image_file)
bbox_info = read_bbox_info(txt_file)
bbox = bbox_info[0][1:5]

# 获取图像长宽
image_shape = image.shape

# 计算实际尺寸和椭圆指标
actual_width, actual_height, perimeter_mm, area_mm2 = calculate_ellipse_metrics(bbox, image_shape)

print(f"Actual Width (mm): {actual_width}")
print(f"Actual Height (mm): {actual_height}")
print(f"Perimeter (mm): {perimeter_mm}")
print(f"Area (mm^2): {area_mm2}")

# 将参数写入表格
data = {
    '种脐长': actual_height,
    '种脐宽': actual_height,
    '种脐面积': area_mm2,
    '种脐周长': perimeter_mm,
}

# 创建DataFrame并写入CSV文件
df = pd.DataFrame(data, index=['ZS110003_19_G_bbox_6'])
df.to_csv('种脐表型参数.csv', sep='\t', encoding='GBK', index=True)

```

#### 5.2.4 整盘籽粒目标检测

##### 5.2.4.1 Yolov8n和轻量化模型

| Models          | Class | Precision | Recall | mAP       | FPS |
| :-------------- | :---- | :-------- | :----- | :-------- | :-- |
| YoloV8n         | All   | 0.694     | 0.701  | **0.745** | 53  |
|                 | Hilum | 0.59      | 0.644  | 0.633     |     |
|                 | Crack | 0.799     | 0.769  | 0.846     |     |
| YoloV8-ghost    | All   | 0.678     | 0.661  | 0.702     | 119 |
|                 | Hilum | 0.579     | 0.583  | 0.591     |     |
|                 | Crack | 0.777     | 0.74   | 0.813     |     |
| YoloV8-ghost-p2 | All   | 0.672     | 0.649  | 0.701     | 116 |
|                 | Hilum | 0.584     | 0.548  | 0.82      |     |
|                 | Crack | 0.76      | 0.75   | 0.583     |     |
| YoloV8-ghost-p6 | All   | 0.672     | 0.68   | 0.714     | 154 |
|                 | Hilum | 0.559     | 0.595  | 0.592     |     |
|                 | Crack | 0.785     | 0.765  | 0.836     |     |

##### 5.2.4.2 加入其他卷积和注意力机制提升模型精度


| Models                    | Class | Precision | Recall | mAP       | FPS |
| :------------------------ | :---- | :-------- | :----- | :-------- | :-- |
| YoloV8n                   | All   | 0.694     | 0.701  | 0.745     | 53  |
|                           | Hilum | 0.59      | 0.644  | 0.633     |     |
|                           | Crack | 0.799     | 0.769  | 0.846     |     |
| DySnakeConv               | All   | 0.709     | 0.736  | 0.782     | 909 |
|                           | Hilum | 0.608     | 0.677  | 0.693     |     |
|                           | Crack | 0.81      | 0.794  | 0.872     |     |
| DySnakeConv-SegNext       | All   | 0.718     | 0.745  | **0.788** | 167 |
|                           | Hilum | 0.628     | 0.68   | 0.695     |     |
|                           | Crack | 0.807     | 0.809  | 0.88      |     |
| DySnakeConv-SegNext-ghost | All   | 0.691     | 0.741  | 0.764     | 370 |
|                           | Hilum | 0.599     | 0.67   | 0.659     |     |
|                           | Crack | 0.794     | 0.811  | 0.869     |     |

>Qi, Yaolei et al. “Dynamic Snake Convolution based on Topological Geometric Constraints for Tubular Structure Segmentation.” ArXiv abs/2307.08388 (2023): n. pag.

ICCV2023 动态蛇形卷积（Dynamic Snake Convolution）用于管状结构分割优势：
1. 细长微弱的局部结构特征与复杂多变的全局形态特征。
2. 管状结构符合局部籽粒细节。

> Guo, Meng-Hao et al. “SegNeXt: Rethinking Convolutional Attention Design for Semantic Segmentation.” ArXiv abs/2209.08575 (2022): n. pag.
编码器参考 SegNeXt，有微弱涨点。

### 5.3 选做：尝试得到每个大豆籽粒的种皮颜色（不含种脐部分）、种脐颜色。
提示：需要自行构建特定部位的颜色提取算法，尽可能地表征出种皮和种脐的真实颜色，分析该算法的可行性、鲁棒性。

#### 5.3.1 提取籽粒种皮颜色

1. 读取图像和数据： 通过 cv2.imread 读取了原始图像和包含 bbox 信息的 TXT 文件，以及用于遮罩的 PNG 图像。
2. 去除黑色区域： remove_black_regions 函数将读取的 PNG 图像转换为灰度图，并根据阈值将黑色部分转换成白色，然后找到白色（籽粒）区域的轮廓，并创建一个遮罩来表示这些区域。
3. 去除 bbox 区域： remove_bbox_regions 函数根据从 TXT 文件中读取的 bbox 信息，在图像中去除了相应的区域。
4. 合并处理后的图像： 将去除黑色区域和 bbox 区域后的图像通过按位与操作合并，得到最终剩余的区域图像。
5. 计算剩余区域的平均颜色： calculate_average_color 函数计算了剩余区域的平均 RGB 颜色值。

代码如下：

```python
import cv2
import numpy as np

# 读取txt文件获取bbox信息
def read_bbox_info(txt_file):
    with open(txt_file, 'r') as file:
        lines = file.readlines()
        bbox_info = [list(map(float, line.strip().split())) for line in lines]
    return bbox_info

def remove_black_regions(mask_image):
    # Convert the image to grayscale
    gray = cv2.cvtColor(mask_image, cv2.COLOR_BGR2GRAY)
    
    # Threshold the image to create a mask
    _, thresh = cv2.threshold(gray, 1, 255, cv2.THRESH_BINARY)
    
    # Find contours of the white regions
    contours, _ = cv2.findContours(thresh, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    
    # Create a mask for the white regions
    mask = np.zeros_like(mask_image)
    cv2.drawContours(mask, contours, -1, (255, 255, 255), thickness=cv2.FILLED)
    
    return mask

def remove_bbox_regions(image, bbox_info):
    for bbox in bbox_info:
        x, y, w, h = list(map(int, bbox[1:5]))
        image[y:y+h, x:x+w] = 0

    return image

def calculate_average_color(image):
    average_color = np.mean(image, axis=(0, 1))
    return average_color

mask_image = cv2.imread('ZS110376_19_G_bbox_mask_3.png')
mask_no_black = remove_black_regions(mask_image)

bbox_info = read_bbox_info('ZS110376_19_G_bbox_3.txt')

original_image = cv2.imread('ZS110376_19_G_bbox_3.jpg')

image_no_bbox = remove_bbox_regions(original_image.copy(), bbox_info)

final_image = cv2.bitwise_and(image_no_bbox, mask_no_black)

average_color = calculate_average_color(final_image)
print(f"Average Color (BGR): {average_color}")

average_color = average_color.astype(int)
average_color_rgb = tuple(average_color[::-1])  # Convert BGR to RGB
print(f"Average Color (RGB): {average_color_rgb}")

"""
Average Color (BGR): [124.47609718 145.48092999 159.72139498]
Average Color (RGB): (159, 145, 124)
"""
```
可视化RGB值
```python
import numpy as np
import cv2
import matplotlib.pyplot as plt

color_rgb = (159, 145, 124)

visualization = np.full((100, 100, 3), color_rgb[::-1], dtype=np.uint8)

plt.imshow(cv2.cvtColor(visualization, cv2.COLOR_BGR2RGB))
plt.axis('off')  
plt.title(f"Color RGB: {color_rgb}")  # Display RGB values in the title
plt.show()

```

![](vx_images/476983819236049.png)



#### 5.3.2 提取种脐种皮颜色

```python
import cv2
import numpy as np

# 读取txt文件获取bbox信息
def read_bbox_info(txt_file):
    with open(txt_file, 'r') as file:
        line = file.readline().strip()
        bbox_info = list(map(float, line.split()))
    return bbox_info

# 计算裁剪后区域的平均RGB值
def calculate_bbox_average_color(image, bbox_info):
    bbox_colors = []

    x = int(bbox_info[1] * image.shape[1])
    y = int(bbox_info[2] * image.shape[0])
    w = int(bbox_info[3] * image.shape[1])
    h = int(bbox_info[4] * image.shape[0])

    print(x, y, w, h)
    # Ensure bbox region is within image bounds
    if x >= 0 and y >= 0 and w > 0 and h > 0 and x + w / 2 <= image.shape[1] and y + h / 2 <= image.shape[0]:
        bbox_region = image[y:y+h, x:x+w]
        
        print(len(bbox_region))
        # 计算裁剪后区域的平均RGB值
        if bbox_region.size > 0:
            average_color = np.mean(bbox_region, axis=(0, 1))
            bbox_colors.append(average_color)

    return bbox_colors

# 读取原始图像
original_image = cv2.imread('ZS110376_19_G_bbox_3.jpg')

# 读取txt文件中的bbox信息
bbox_info = read_bbox_info('ZS110376_19_G_bbox_3.txt')

# 计算裁剪后区域的平均RGB值
bbox_average_colors = calculate_bbox_average_color(original_image, bbox_info)

print("Average RGB Colors of Bboxes:")
for i, color in enumerate(bbox_average_colors, 1):
    print(f"Bbox {i}: {color}")

```

可视化RGB值
```python
import numpy as np
import cv2
import matplotlib.pyplot as plt

color_rgb = (134, 147, 156)

visualization = np.full((100, 100, 3), color_rgb[::-1], dtype=np.uint8)

plt.imshow(cv2.cvtColor(visualization, cv2.COLOR_BGR2RGB))
plt.axis('off')  
plt.title(f"Color RGB: {color_rgb}")  # Display RGB values in the title
plt.show()
```
![](vx_images/309245219236658.png)

#### 5.3.3 可行性、鲁棒性分析


##### 5.3.3.1 可行性分析：

 - 基于颜色提取算法： 使用基于颜色阈值和轮廓识别的方法，可以相对容易地提取出籽粒的主要颜色。这种方法在一些场景下是可行的，尤其是当种皮和种脐的颜色与周围环境有较大的对比度时。

 - 遮罩处理黑色区域： 通过阈值化和轮廓检测来去除黑色区域，适用于较为统一的背景下。然而，如果背景色与籽粒颜色接近或者变化剧烈，则可能需要更复杂的图像分割技术。

 - 使用Bbox信息去除区域： 使用bbox信息在一定程度上可以去除不需要的区域，但需确保bbox准确性，否则可能导致区域提取不准确。

- 平均颜色提取： 使用平均颜色作为代表颜色的统计量，这在一些情况下可以给出合理的颜色表示，但对于颜色变化较大的区域或场景，平均颜色提取可能无法充分表征真实颜色特征。


##### 5.3.3.2 鲁棒性分析：

- 光照和背景适应性： 该算法可能对光照变化和背景差异敏感，对于颜色对比度较低的情况，或者在复杂背景下可能表现不佳。

- 参数和阈值设定： 算法中的阈值设置可能需要调整来适应不同场景和不同品种的大豆籽粒。合适的参数设置对算法的鲁棒性至关重要。

 - 数据准确性需求： 对于bbox信息的准确性和遮罩处理的准确性有一定的依赖性。如果bbox信息不准确或者遮罩处理不完善，可能会导致提取结果不准确。

 - 颜色特征泛化性： 算法对于不同种类和品种的大豆籽粒可能存在局限性。不同颜色和纹理特征的籽粒可能需要不同的颜色提取策略。
 


