+++
title = 'Chlorophyll Estimation Fusion DL and Biophysical Models'
date = 2025-01-22T15:50:31+01:00
draft = false
+++


# Enhancing chlorophyll content estimation with Sentinel-2 imagery: A fusion of deep learning and biophysical models

利用Sentinel-2影像增强叶绿素含量估算：深度学习与生物物理模型的融合

## 0. Abstract

Chlorophyll is a key biochemical component as it is integral to the photosynthetic mechanism. Chlorophyll content (Chl-ab) is therefore widely used to track vegetation dynamics and assess the health status of canopies. In this paper, we present an innovative approach to accurately estimate chlorophyll content at the canopy level by integrating radiative transfer models with deep learning algorithms. Our methodology utilizes a U-Net deep learning model trained with Sentinel-2 imagery and chlorophyll content obtained from resampled Sentinel-2 reflectance simulations conducted with the Pro4SAIL model. We evaluated the resulting Chl-ab predictions against leaf chlorophyll content measured in Lanžhot forest, Czechia, during six field campaigns in 2019 and 2020. Our models effectively captured the complexities of forest canopies in Lanžhot forest, where they achieved an RMSE of 5.32 μg cm⁻² and MAE of 4.56 μg cm⁻².

叶绿素是一种关键的生物化学成分，因为它是光合作用机制的核心部分。因此，叶绿素含量（Chl-ab）被广泛用于追踪植被动态并评估冠层的健康状况。在本文中，我们提出了一种创新方法，通过将辐射传输模型与深度学习算法相结合，准确估算冠层水平的叶绿素含量。我们的方法利用U-Net深度学习模型，该模型通过Sentinel-2影像以及由Pro4SAIL模型生成的Sentinel-2反射率模拟数据训练得到。我们将预测的Chl-ab结果与2019年和2020年在捷克Lanžhot森林进行的六次实地测量中获取的叶片叶绿素含量进行了对比评估。我们的模型有效地捕捉了Lanžhot森林冠层的复杂性，其预测结果的均方根误差（RMSE）为5.32 μg cm⁻²，平均绝对误差（MAE）为4.56 μg cm⁻²。

## 1. Introduction

Developing methods to monitor the physiological status and carbon dynamics of forests by quantifying key plant traits is crucial to gain a comprehensive insight into the functioning of forest ecosystems and their intricate connections to global climate change processes. Among these plant traits, chlorophyll content (Chl-ab) holds particular significance as it is a key to photosynthesis, and thus, primary productions. It is therefore a widely employed parameter to monitor vegetation dynamics and assess the health of canopies. Non-seasonal reductions in Chl-ab can namely indicate vegetation decline, related to plant nitrogen stress [1]. Numerous remote sensing studies have shown that chlorophyll content can be successfully retrieved from Sentinel-2 (SE2) imagery [2]–[4]. Commonly used techniques to retrieve plant functional traits through remote sensing can be divided into statistical-based empirical models, physics-based radiative transfer models (RTMs), and hybrid machine learning (ML) approaches [5]–[7]. Compared to empirical methods, physics-based approaches, particularly those relying on RTMs, are generalizable and thus offer the advantage of easy transferability between diverse forest ecosystems. However, conventional RTM inversions face challenges, including high computational demands and vulnerability to uncertainties arising from various combinations of input variables [8]. 

The current hybrid inversion methods that couple RTMs with satellite imagery often fail to explicitly consider spatial patterns, instead treating all image pixels independently. In efforts to overcome this, deep learning, and convolutional neural networks (CNNs) in particular, have emerged as a transformative tool for image processing. The CNN approach enables automatic learning of complex multi-scale features, including texture, from extensive training datasets. It not only reduces processing time but also mitigates bottlenecks associated with extrapolating plant trait inversions from RTMs to satellite images. As a consequence, new-generation models such as the U-Net [9] model offer a promising avenue to robustly retrieve chlorophyll content, and vegetation monitoring more generally, through remote sensing, overcoming the limitations of traditional methods. 

In this study, we estimated chlorophyll content utilizing the PRO4SAIL RTM model and SE2 imagery across the Lanžhot mixed floodplain forest in Czechia. Using deep learning techniques, we set out to generate a high-resolution (20 m) chlorophyll content map covering the entire SE2 scene. The leaf Chl-ab maps were derived through a U-Net deep learning model, trained on SE2 reflectance, spectral indices derived from red-edge bands, and sample points randomly distributed throughout the study area. Specifically, PRO4SAIL-inverted Chl-ab at 10,000 sample points were utilized. Subsequently, we conducted a rigorous validation of the predicted Chl-ab values by comparing them with leaf chlorophyll measurements taken in the Lanžhot forest during 2019 and 2020.

开发通过量化关键植物性状来监测森林生理状态和碳动态的方法，对于全面了解森林生态系统的功能及其与全球气候变化过程的复杂联系至关重要。在这些植物性状中，叶绿素含量（Chl-ab）尤为重要，因为它是光合作用的关键，从而也是初级生产的关键。因此，它被广泛用于监测植被动态和评估冠层健康状况。非季节性的叶绿素含量减少可能表明植被衰退，这与植物氮胁迫有关[1]。许多遥感研究表明，叶绿素含量可以从Sentinel-2（SE2）影像中成功反演[2]–[4]。通过遥感获取植物功能性状的常用技术可分为基于统计的经验模型、基于物理的辐射传输模型（RTMs）以及混合机器学习（ML）方法[5]–[7]。与经验方法相比，基于物理的方法，特别是依赖辐射传输模型的方法，具有通用性，因此在不同森林生态系统之间易于转移。然而，传统的辐射传输模型反演面临挑战，包括高计算需求以及输入变量组合带来的不确定性[8]。

当前将辐射传输模型与卫星影像结合的混合反演方法通常未能明确考虑空间模式，而是将所有图像像素独立处理。为了克服这一问题，深度学习，特别是卷积神经网络（CNNs），已成为图像处理的变革性工具。CNN方法能够从大量训练数据中自动学习复杂的多尺度特征，包括纹理。它不仅减少了处理时间，还缓解了从辐射传输模型反演植物性状到卫星影像的外推瓶颈。因此，新一代模型如U-Net[9]模型为通过遥感稳健反演叶绿素含量及更广泛的植被监测提供了一条有前景的途径，克服了传统方法的局限性。

在本研究中，我们利用PRO4SAIL辐射传输模型和Sentinel-2影像对捷克Lanžhot混合洪泛森林的叶绿素含量进行了估算。通过深度学习技术，我们生成了覆盖整个Sentinel-2场景的高分辨率（20米）叶绿素含量图。叶片Chl-ab图通过U-Net深度学习模型生成，该模型基于Sentinel-2反射率、红边波段衍生的光谱指数以及研究区域内随机分布的样本点进行训练。具体而言，使用了10,000个样本点的PRO4SAIL反演Chl-ab数据。随后，我们通过将预测的Chl-ab值与2019年和2020年在Lanžhot森林中测量的叶片叶绿素含量进行对比，进行了严格的验证。

## 2. Material and Method

**2.1. Field campaign at Lanžhot forest**  

The Lanžhot forest, situated in the lowland floodplain near the confluence of the Morava and Dyje rivers, grows at an elevation of 150 m.a.s.l. The diverse forest is predominantly composed of English oak (Quercus robur L.), narrow-leaved ash (Fraxinus angustifolia L.), hornbeam (Carpinus betulus L.), and linden (Tilia cordata Mill.). Detailed descriptions of the habitat conditions at Lanžhot are provided by [10]–[12]. Six field campaigns were carried out in the Lanžhot mixed floodplain forest during 2019 and 2020 (Figure 1), situated approximately 1 km from the eddy covariance flux tower managed by the Integrated Carbon Observation System (ICOS). These field campaigns aimed at capturing various phenological stages of deciduous tree leaf development, from the emergence of fresh leaves, through full leaf development at the peak of the growing season, to autumn leaf senescence. The measurements of leaf chlorophyll content were conducted for a representative sample of trees encompassing a variety of tree species: Quercus robur, Quercus cerris L., Carpinus betulus, Fraxinus angustifolia, Tilia cordata, Populus alba, and Picea abies. Further details about the field campaigns can be found in [12]. The chlorophyll content data collected in the Lanžhot forest served as a crucial component in validating the proposed hybrid biophysical-deep learning approach, which relies on SE2 imagery and RTMs.  

**2.2. Sentinel-2 Time Series**  

The SE2 time series covering the period of 2018-2020 were processed for the Lanžhot forest area using Google Earth Engine (GEE) through a Python Google Colab notebook. The dataset utilized for this analysis corresponded with the Harmonized Sentinel-2 MSI: MultiSpectral Instrument, Level-2A collection.  

**2.3. Leaf chlorophyll estimation using RTMs**  

Leaf chlorophyll content was modelled using an inverse hybrid ML approach based on resampled PRO4SAIL-simulated reflectance spectra. This procedure was carried out for each SE2 acquisition between 2018-2020 that covers the Lanžhot forest area (Figure 1). We used a look-up table of 20,000 reflectance simulations based on uniformly distributed biochemical, viewing angles and structural plant traits (Table 1). To prevent ill-posed solutions [8], we carefully constrained the input ranges, drawing upon field data, information from literature, and preliminary simulations. This step was pivotal in guaranteeing that the chlorophyll values fell within the range observed during field campaigns. To estimate leaf chlorophyll content from the observed SE2 spectra, we employed a 3-layer Neural Network (NNe). This NNe was trained using 80% of the simulations, with the remaining 20% reserved for testing the inversion approach.  

**2.4. Deep learning model to estimate chlorophyll content**  

We predicted chlorophyll content at pixel-level using a fully convolutional network [13] tailored to regression problems. More specifically, we used a U-Net model made up of convolutional networks; this architecture is known to outperform previous models in speed and accuracy and requires fewer training examples thanks to its U-shape architecture [9]. We adapted it to our regression task by using a linear activation function instead of the Softmax, which is typically used for classification problems.  

The modeling process, illustrated in Figure 2, consists of training the U-Net on a random set of pixels for which chlorophyll content was retrieved via the NNe inversion approach. Such training strategy with spatially sparse reference data contrasts with the more common approach where dense training data are used. The trained U-Net is then used to predict chlorophyll content values for all remaining pixels. In total, we sampled 10,000 pixels, to capture the diverse characteristics of forest canopies within our study area. 80% of these reference time-series were used for U-Net model training while the remaining 20% were used for model performance assessment. In the final step, we validated the accuracy of the predicted Chl-ab values generated by the U-Net model by comparing them with the leaf chlorophyll content measured in situ in the Lanžhot mixed floodplain forest in 2019 and 2020. Assuming that chlorophyll content does not change significantly over short periods, we used a 7-day window around the leaf sampling dates to select the closest Sentinel-2 imagery. This approach ensured a robust temporal alignment, enhancing the reliability of the comparison between the model predictions and the field measurements. Additionally, a spectral filter was applied to the Sentinel-2 data to discard any spectral anomalies in the time series.  

**2.1. Lanžhot森林的野外调查**  

Lanžhot森林位于摩拉瓦河和迪耶河交汇处的低地洪泛区，海拔150米。该森林物种多样，主要由英国栎（Quercus robur L.）、窄叶白蜡树（Fraxinus angustifolia L.）、欧洲鹅耳枥（Carpinus betulus L.）和椴树（Tilia cordata Mill.）组成。关于Lanžhot森林生境条件的详细描述见文献[10]–[12]。2019年和2020年期间，在Lanžhot混合洪泛森林中开展了六次野外调查（图1），调查地点距离由综合碳观测系统（ICOS）管理的涡度协方差通量塔约1公里。这些野外调查旨在捕捉落叶乔木叶片发育的各个物候阶段，从新叶萌发到生长旺季的完全展开，再到秋季叶片衰老。叶片叶绿素含量的测量针对多种树种的代表性样本进行，包括英国栎、土耳其栎（Quercus cerris L.）、欧洲鹅耳枥、窄叶白蜡树、椴树、银白杨（Populus alba）和挪威云杉（Picea abies）。关于野外调查的更多细节见文献[12]。在Lanžhot森林中收集的叶绿素含量数据是验证所提出的基于Sentinel-2影像和辐射传输模型的混合生物物理-深度学习方法的关键组成部分。  

**2.2. Sentinel-2时间序列**  

使用Google Earth Engine（GEE）通过Python Google Colab笔记本处理了2018-2020年期间覆盖Lanžhot森林区域的Sentinel-2时间序列数据。本分析使用的数据集对应于Harmonized Sentinel-2 MSI（多光谱仪器）Level-2A集合。  

**2.3. 使用辐射传输模型估算叶片叶绿素含量**  

基于重采样的PRO4SAIL模拟反射光谱，采用逆混合机器学习方法对叶片叶绿素含量进行建模。该过程针对2018-2020年期间覆盖Lanžhot森林区域的每次Sentinel-2影像获取进行（图1）。我们使用了基于均匀分布的生化参数、视角和植物结构特征的20,000次反射率模拟查找表（表1）。为了防止不适定问题[8]，我们根据野外数据、文献信息和初步模拟仔细限制了输入范围。这一步骤对于确保叶绿素值落在野外调查期间观察到的范围内至关重要。为了从观测到的Sentinel-2光谱中估算叶片叶绿素含量，我们采用了一个3层神经网络（NNe）。该神经网络使用80%的模拟数据进行训练，剩余的20%用于测试反演方法。  

**2.4. 用于估算叶绿素含量的深度学习模型**  

我们使用专为回归问题设计的全卷积网络[13]在像素级别预测叶绿素含量。具体而言，我们采用了由卷积网络组成的U-Net模型；该架构以其U形结构在速度和准确性上优于之前的模型，并且由于其对训练样本的需求较少而闻名[9]。我们通过使用线性激活函数替代通常用于分类问题的Softmax函数，将其调整为回归任务。  

建模过程如图2所示，包括在通过神经网络反演方法获取叶绿素含量的随机像素集上训练U-Net模型。这种使用空间稀疏参考数据的训练策略与通常使用密集训练数据的方法形成对比。训练后的U-Net模型用于预测所有剩余像素的叶绿素含量值。我们总共采样了10,000个像素，以捕捉研究区域内森林冠层的多样性特征。其中80%的参考时间序列用于U-Net模型训练，剩余的20%用于模型性能评估。在最后一步中，我们通过将U-Net模型预测的Chl-ab值与2019年和2020年在Lanžhot混合洪泛森林中实地测量的叶片叶绿素含量进行对比，验证了其准确性。假设叶绿素含量在短时间内不会发生显著变化，我们使用叶片采样日期前后7天的时间窗口选择最接近的Sentinel-2影像。这种方法确保了时间对齐的鲁棒性，增强了模型预测与实地测量结果之间比较的可靠性。此外，对Sentinel-2数据应用了光谱滤波器，以剔除时间序列中的任何光谱异常。


## 3. RESULTS AND DISCUSSION  

3.1. Field Validation in Lanžhot forest area**  

Our model demonstrated robust performance in capturing the intricate characteristics of forest canopies within the Lanžhot forest. Overall, with all tree sample locations, the Chl-ab predicted from Sentinel-2 imagery using the neural networks and the PROSAIL model showed a strong correlation with in situ chlorophyll measurements taken in the Lanžhot forest (r² = 0.86, Figure 3). The model achieved a Root Mean Squared Error (RMSE) of 5.32 μg cm⁻² and a Mean Absolute Error (MAE) of 4.56 μg cm⁻², indicating high accuracy and reliability in the predicted chlorophyll estimates.  

By tree species, the predicted Chl-ab showed strong correlation with studied leaf chlorophyll samples (r² > 0.87; Table 2). Specifically, the U-Net model achieved the highest accuracy for leaf samples collected over Quercus cerris (RMSE = 3.83 μg cm⁻²; MAE = 3.34 μg cm⁻²), followed by Tilia cordata (MAE = 4.51 μg cm⁻²; RMSE = 4.12 μg cm⁻²) and Carpinus betulus (RMSE = 4.71 μg cm⁻²; MAE = 3.73 μg cm⁻²). These results underscore the robustness of the predictive capabilities in estimating chlorophyll, highlighting the potential of the integrated approach in accurately characterizing forests and their chlorophyll dynamics.  

These findings highlight the potential of integrating deep learning algorithms with RTMs and Sentinel-2 time series for chlorophyll content retrieval. The approach not only reduces processing time but also eliminates bottlenecks associated with extrapolating plant trait inversions from RTMs to satellite images. The proposed method significantly enhances the feasibility of integrating RTMs into near real-time monitoring systems. This integration holds promise to track biochemical effects of biotic (e.g., bark beetle attacks) and abiotic forest disturbances (e.g., drought). By leveraging the synergy of RTMs, deep learning algorithms, and Sentinel-2 imagery, this work addresses the challenges of timely and accurate monitoring in dynamic forest ecosystems.  

**3.1. Lanžhot森林区域的实地验证**  

我们的模型在捕捉Lanžhot森林冠层的复杂特征方面表现出色。总体而言，使用神经网络和PROSAIL模型从Sentinel-2影像中预测的Chl-ab与Lanžhot森林中实地测量的叶绿素含量显示出强相关性（r² = 0.86，图3）。模型的均方根误差（RMSE）为5.32 μg cm⁻²，平均绝对误差（MAE）为4.56 μg cm⁻²，表明预测的叶绿素含量具有较高的准确性和可靠性。  

按树种划分，预测的Chl-ab与研究的叶片叶绿素样本显示出强相关性（r² > 0.87；表2）。具体而言，U-Net模型对土耳其栎（Quercus cerris）叶片样本的预测精度最高（RMSE = 3.83 μg cm⁻²；MAE = 3.34 μg cm⁻²），其次是椴树（Tilia cordata）（MAE = 4.51 μg cm⁻²；RMSE = 4.12 μg cm⁻²）和欧洲鹅耳枥（Carpinus betulus）（RMSE = 4.71 μg cm⁻²；MAE = 3.73 μg cm⁻²）。这些结果强调了模型在估算叶绿素含量方面的稳健性，凸显了该集成方法在准确表征森林及其叶绿素动态方面的潜力。  

这些发现表明，将深度学习算法与辐射传输模型（RTMs）和Sentinel-2时间序列相结合用于叶绿素含量反演具有巨大潜力。该方法不仅减少了处理时间，还消除了从辐射传输模型反演植物性状到卫星影像的外推瓶颈。所提出的方法显著增强了将辐射传输模型集成到近实时监测系统中的可行性。这种集成有望追踪生物性（如树皮甲虫侵袭）和非生物性森林干扰（如干旱）的生化效应。通过利用辐射传输模型、深度学习算法和Sentinel-2影像的协同作用，本研究解决了动态森林生态系统中及时准确监测的挑战。

## 4. CONCLUSIONS  

This study highlights the potential of deep learning models to map forest chlorophyll content continuously at high resolution using inverted chlorophyll-based RTMs and Sentinel-2 multispectral imagery, even when reference data are sparse. The proposed U-Net model demonstrates a great ability to capture intricate spatial patterns in chlorophyll content in the forest in our study area, suggesting that robust chlorophyll mapping is possible in forest ecosystems with diverse vegetation densities and structures.  

The implementation of the U-Net model streamlines the chlorophyll content mapping process, minimizing the uncertainties typically associated with post-processing steps in traditional inversion approaches that integrate satellite data and RTMs. This comprehensive deep learning approach ensures the reliability, precision, and real-world applicability of chlorophyll content mapping facilitated by the seamless integration of the U-Net model with Sentinel-2 imagery and RTMs. These findings contribute significantly to enhancing our understanding of the physiological and health status of forests, as well as carbon stocks, at an unprecedented spatial resolution, with potential applications in forest management, climate change, and early detection of forest disturbances.  

本研究强调了深度学习模型在使用基于叶绿素反演的辐射传输模型（RTMs）和Sentinel-2多光谱影像进行高分辨率连续森林叶绿素含量制图方面的潜力，即使在参考数据稀疏的情况下也是如此。所提出的U-Net模型展现了捕捉研究区域森林叶绿素含量复杂空间模式的强大能力，表明在具有不同植被密度和结构的森林生态系统中实现稳健的叶绿素制图是可行的。  

U-Net模型的实施简化了叶绿素含量制图过程，最大限度地减少了传统反演方法中与卫星数据和辐射传输模型集成的后处理步骤相关的不确定性。这种全面的深度学习方法通过U-Net模型与Sentinel-2影像和辐射传输模型的无缝集成，确保了叶绿素含量制图的可靠性、精确性和实际应用性。这些发现显著增强了我们对森林生理和健康状况以及碳储量的理解，并以前所未有的空间分辨率提供了潜在的应用价值，可用于森林管理、气候变化和森林干扰的早期检测。
