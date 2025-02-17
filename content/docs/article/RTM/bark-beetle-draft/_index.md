+++
title = 'Bark Beetle Draft'
date = 2025-01-23T14:01:23+01:00
draft = false
+++

基于Sentinel-2与PROSAIL-PRO对树皮甲虫侵染云杉树检测进行评估


## 0. Abstract







## 1.Introduction

>global视角

>介绍树皮甲虫

> 还要加一块RTM的介绍


欧洲云杉树皮甲虫是欧洲针叶林中最具经济破坏性的昆虫害虫（Biedermann等，2019）。在过去的50年里，随着全球变暖的加剧，欧洲云杉树皮甲虫在欧洲已经累计导致了超过1.5亿立方米的森林死亡（Schroeder和Cocoş，2018）。这些不仅造成了经济损失，还严重威胁森林生态系的稳定（Huang等，2019）。

控制树皮甲虫的方法是在幼虫仍在树干内时，检测并移除受攻击的树木。而云杉树皮甲虫攻击通常根据可见光谱中的检测难度分为三个阶段。（1）绿色攻击阶段，由于这一阶段树冠在可见光区域没有异常颜色变化，很难用光学卫星检测到攻击。（2）红色攻击阶段，光学卫星能观测到树冠变黄或变红，同时针叶中的水分含量显著下降。（3）灰色攻击阶段，杉树死亡后逐渐失去针叶，仅剩灰色树皮。使用光学卫星在红色和灰色阶段的检测率较高，绿色攻击的检测率较低（Wulder等，2006）。

近年来，越来越多的研究集中在针对早期检测（绿攻击至红攻击）和制图上。Abdullah等的研究表明红边和短波红外反射的时机对于早期检测树皮甲虫引起的胁迫至关重要（Abdullah等，2019），但对绿色攻击的准确性较低。受此启发Huo等人引入了归一化距离红边和短波红外（NDRS）指数用于早期检测由树皮甲虫攻击引起的森林胁迫（Huo等，2021）。这些研究充分利用了对水分和叶绿素含量变化敏感的红边和短波红外波段反射的组合有可能提高攻击检测的能力。将红色攻击视为易检测阶段而止步于定性判断，忽略了植物内部受到红色攻击引起的生理参数变化。

本研究调查了使用光学卫星检测红色攻击。研究健康和受攻击树木在整个红色攻击期间的植被光谱信号和生理参数的差异。


## 2. Materials and methods

### 2.1 研究区域

Norway spruce 是欧洲寒带或山区气候的原生树种。它自然分布于孚日山脉 Vosges mountains 的最高海拔地区，但在19世纪和20世纪期间，它也被成功引种到法国  Grand-Est 和比利时 Wallonia 等低海拔温暖地区（Guinier, 1959; Noirfalise & Thill, 1975）。我们关注的这次由2018年至2020年的干燥和温暖天气引发的虫害持续了5年，从2017年到2022年，摧毁了 比利时 Wallonia 和法国 Grand-Est 12.2%（23,674公顷）的云杉林区。在2021年的多雨年份之后，2022年新的树皮甲虫侵袭又恢复到了2017年的水平。

本研究区域位于法国北部区域的 Grand Est ，具体属于Ardennes 省 (49°12′24.85″N, 4°57′45.83″E). The three most important tree species growing in the Aredennes forest are beech, oak and spruce. 研究区域考虑到森林在树种、树龄和密度上的异质性，如图Ⅰ所示，选择了6块不相邻的样地。其中包含面积分别为81，72，374，52平方米的4块受侵染样地和面积为73，60平方米的2块健康样地）。

![](file:///D:/Data/AEO/Assignment/Figures/Figure1.drawio.png)


图Ⅰ， Figure 1: The location of leaf chlorophyll content measurements carried
out in Lanzhot forest, Czechia, during the years 2019 and 2020.

### 2.2参考数据

本研究通过Google Earth Engine（GEE）从“COPERNICUS/S2_SR”数据目录中进行选择，收集了2016-2021全年的 Sentinel-2 影像Level 2地表反射率影像。排除60米分辨率波段（B1、B9、B10），并将20米分辨率波段（B5、B6、B7、B8A、B11、B12）重采样至10米以匹配其他波段。同时为了保留季节效应，未进行直方图匹配。

### 2.3 用辐射传输模型（RTMs）估算Spruce生理参数含量

我们使用 PROSAIL-PRO模型(引用)，将PROSPECT-PRO叶片反射模型与fourSAIL冠层辐射传输模型结合，反演植物生理形状。我们基于野外测量和文献信息约束所有输入参数，确保生成的LUT在Sentinel-2a传感器观测范围内。We used a look-up table of 5,000 reflectance simulations based on uniformly distributed biochemical, viewing angles and structural plant traits (Table 1). 并对模拟的光谱数据（1 nm分辨率）进行一维卷积处理，以匹配 Sentinel-2 传感器数据的波长。

以下是基于提供的代码参数范围构建的表格：

|         **Parameter**         | **Abbreviation** | **Units** |              **Values**               |
| ----------------------------- | ---------------- | --------- | ------------------------------------- |
| **PROSPECT-PRO Parameters**   |                  |           |                                       |
| Leaf structure parameter      | N                | -         | 1.0 – 3.0 (uniform)                   |
| Chlorophyll a+b content       | Cab              | μg/cm-²   | 10 – 75 (uniform)                     |
| Carotenoid content            | Car              | μg/cm-²   | 0 – 5 (uniform)                       |
| Anthocyanin content           | Anth             | μg/cm-²   | 0 – 0.1 (uniform)                     |
| Brown pigment content         | Cbrown           | -         | 0.0 – 1 (uniform)                     |
| Internal PROSPECT parameter   | alpha            | -         | Fixed at 40                           |
| Equivalent water thickness    | EWT              | cm        | 0.0005 – 0.3 (uniform)                |
| Leaf mass per area            | LMA              | g/cm-²    | 0.0005 – 0.3 (uniform)                |
| Protein content               | Prot             | g/cm-²    | 0.0009 – 0.01 (uniform)               |
| Carbon-based constituents     | CBC              | g/cm-²    | Derived: $ \text{LMA} - \text{Prot} $ |
| **FourSAIL Parameters**       |                  |           |                                       |
| Leaf inclination distribution | TypeLidf         | -         | Fixed at 2                            |
| Leaf angle distribution param | LIDFa            | -         | 30 – 70 (uniform)                     |
| Leaf angle distribution param | LIDFb            | -         | Fixed at 0                            |
| Leaf area index               | LAI              | -         | 0.5 – 4.0 (uniform)                   |
| Hotspot parameter             | hspot            | -         | 0.0 – 1 (uniform)                     |
| Soil reflectance coefficient  | psoil            | -         | 0.0 – 0.95 (uniform)                  |
| Solar zenith angle            | tts              | °         | 25 – 30 (uniform)                     |
| Observer zenith angle         | tto              | °         | 25 – 30 (uniform)                     |
| Relative azimuth angle        | psi              | °         | 150 – 160 (uniform)                   |

表Ⅰ，Table 1: Ranges of parameters used for the simulations with the
PROSAIL-PRO radiative transfer model

通过基于NRMSE的最优匹配方法，从LUT中筛选50组最佳参数组合用于反演冠层性状。RMSE的计算公式如下：

$$
\text{RMSE} = \sqrt{\frac{1}{N \cdot K} \sum_{i=1}^{N} \sum_{k=1}^{K} \left( y_{i,k}^{\text{obs}} - y_{i,k}^{\text{sim}} \right)^2}
$$

其中 $N$ 为时间序列中的观测点数，$K$为使用的波段和植被指数数量，此处我们使用'B6','B7','B8','NDVI','TCARI_OSAVI','CR.SWIR','CR.red.nir')，共7项。$y_{i,k}^{\text{obs}}$ 为第 $i$ 个时间点在 Sentinel-2 实测数据的中第 $k$ 个波段或指数的值。$y_{i,k}^{\text{sim}}$：第$i$ 个时间点在 PROSAIL-PRO 中的模拟数据第 $k$ 个波段或指数的值。

$$
\text{NRMSE} = \frac{\text{RMSE}}{y_{\text{max}} - y_{\text{min}}}
$$
其中 $y_{\text{max}}$ 和 $y_{\text{min}}$ 是实测数据中所有波段/指数的全局最大值和最小值。

最后，对于选择的最优模拟反射值，进行线性插值。

### 2.4 Statistics

我们用多种统计方法来分析2020年5-6月的健康与受虫害云杉样地的多组光谱信号和生理参数（N, Cab, Car, Anth, Cbrown, EWT, LMA, CBC）的关系。首先拟合线性回归趋势线，直观展示两参数间的正负相关性。例如，B4（红光波段）与TCARI/OSAVI（叶绿素敏感指数）的组合可检测虫害引起的“红边偏移”现象；CR.SWIR（短波红外波段比值）与EWT（等效水厚度）的关联则反映冠层水分状态对红外光谱的吸收特性。其次用边际箱线图补充单变量的组内分布特征。通过中位数、四分位距和离群值展示各参数在两类样地中的分布差异。

同时通过双样本t检验与Cohen's d效应量的双重指标评估参数差异。t检验用于判断健康与虫害样地间各参数的均值是否存在显著差异（p<0.05为显著性阈值），而Cohen's d则进一步量化差异的实际大小。


## 3.Results



## 4.Discussion



## 5.Conclusion




##  其他

然而大多数针对早期阶段的研究需要积累至少半个月的光谱特征才能检测到。在时间序列数据有限时，不便及时实施森林害虫控制和损害管理。
