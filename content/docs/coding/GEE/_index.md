+++
title = 'GEE'
date = 2025-01-08T09:39:56+01:00
draft = false
+++


```javascript
// 定义感兴趣区域（ROI）的地理位置，使用经纬度坐标表示
var bloom_location = 
    /* color: #d63000 */  // 注释：该点的颜色
    /* shown: false */    // 注释：该点是否在地图上显示
    ee.Geometry.Point([0.771149010081289, 58.86760681756596]);  // 经纬度坐标

// 定义一组颜色，用于可视化
var viridisHexColors = [
  '#440154', '#482677', '#3E498E', '#317A96',
  '#217C88', '#199073', '#2D9975', '#63B244',
  '#A6D825', '#DFEE22'
];

// 定义可视化参数
var chla_viz = {"opacity":1,"min":0.18,"max":0.74,"bands":["chl-a"],"palette":viridisHexColors};  // 叶绿素a的可视化参数
var ndvi_viz = {"opacity":1,"min":-1,"max":1,"bands":["ndvi"],"palette":viridisHexColors};  // NDVI的可视化参数
var olci_rgb = {"opacity":1,"bands":["Oa08_radiance","Oa06_radiance","Oa04_radiance"],"min":-13.270734821266728,"max":141.53265401318592,"gamma":1};  // RGB图像的可视化参数

// Sentinel-3 OLCI 波段信息
var bands = 
  [
    "Oa01_radiance", // 400 nm    气溶胶校正，改进的水体成分反演
    "Oa02_radiance", // 412.5 nm  黄色物质和碎屑色素（浊度）
    "Oa03_radiance", // 442.5 nm  叶绿素吸收最大值，生物地球化学，植被
    "Oa04_radiance", // 490 nm    高叶绿素
    "Oa05_radiance", // 510 nm    叶绿素，沉积物，浊度，赤潮
    "Oa06_radiance", // 560 nm    叶绿素参考（叶绿素最小值）
    "Oa07_radiance", // 620 nm    沉积物负荷
    "Oa08_radiance", // 665 nm    叶绿素（第二个叶绿素吸收最大值），沉积物，黄色物质/植被
    "Oa09_radiance", // 673.75 nm 改进的荧光反演，与665和680 nm波段一起更好地处理微笑效应
    "Oa10_radiance", // 681.25 nm 叶绿素荧光峰值，红边
    "Oa11_radiance", // 708.75 nm 叶绿素荧光基线，红边过渡
    "Oa12_radiance", // 753.75 nm O2吸收/云，植被
    "Oa13_radiance", // 761.25 nm O2吸收带/气溶胶校正
    "Oa14_radiance", // 764.38 nm 大气校正
    "Oa15_radiance", // 767.50 nm O2A用于云顶压力，陆地上的荧光
    "Oa16_radiance", // 778.75 nm 大气校正/气溶胶校正
    "Oa17_radiance", // 865 nm    大气校正/气溶胶校正，云，像素配准
    "Oa18_radiance", // 885 nm    水汽吸收参考波段。与SLSTR仪器共用的参考波段。植被监测
    "Oa19_radiance", // 900 nm    水汽吸收/植被监测（最大反射率）
    "Oa20_radiance", // 940 nm    水汽吸收，大气校正/气溶胶校正
    "Oa21_radiance", // 1020 nm   大气校正/气溶胶校正
  ]

// 获取指定日期的Sentinel-3 OLCI影像
var s3_olci_230614 = ee.Image("COPERNICUS/S3/OLCI/S3A_20230614T104004_20230614T104304").select(bands);

// 计算NDVI的函数
var calculate_ndvi = function(image){
  return image.expression(
      '(nir - red) / (nir + red)', {  // NDVI计算公式
      'red': image.select('Oa08_radiance'),  // 红光波段
      'nir': image.select('Oa17_radiance')}  // 近红外波段
      ).rename('ndvi')  // 重命名为ndvi
};

// TODO - 在这里填写计算叶绿素a的函数
var calculate_chla = function(image){
  return image.expression(
      'TODO', {  // 叶绿素a计算公式
      'TODO': image.select('TODO'),  // 选择波段
      'TODO': image.select('TODO')}  // 选择波段
      ).rename('chl-a')  // 重命名为chl-a
};

// 将地图中心设置为感兴趣区域
Map.centerObject(bloom_location, 8);

// 在地图上添加图层
Map.addLayer(calculate_chla(s3_olci_230614), chla_viz, 'CHLa S3 OLCI 2023-06-14');  // 添加叶绿素a图层
Map.addLayer(calculate_ndvi(s3_olci_230614), ndvi_viz, 'NDVI S3 OLCI 2023-06-14');  // 添加NDVI图层
Map.addLayer(s3_olci_230614, olci_rgb, 'RGB S3 OLCI 2023-06-14');  // 添加RGB图层
```


```javascript
// 定义感兴趣区域的地理位置，使用经纬度坐标表示
var geometry = /* color: #d63000 */ee.Geometry.Point([31.070050483764398, -29.860244908672144]);

// 将地图中心设置为感兴趣区域，缩放级别为14
Map.centerObject(geometry, 14);

// 定义可视化参数
var magma = ["400b0b","a12424","ee7b06","ffa904","ffdb00"];  // magma颜色方案
var viridis = ["450256","3B1C8C","21908D","5AC865","F9E721"];  // viridis颜色方案
var fdi_viz = {"opacity":1,"bands":["fdi"],"min":0,"max":0.1,"palette":magma};  // FDI的可视化参数
var rgb_viz = {"opacity":1,"bands":["B4","B2","B2"],"min":-0.2330149015356378,"max":0.4827266454476566,"gamma":1};  // RGB图像的可视化参数
var ndvi_viz ={"opacity":1,"bands":["ndvi"],"min":-.4,"max":0.4,"palette":viridis};  // NDVI的可视化参数

// Sentinel-2波段信息
var bands = 
  [
    "B1",  // 442.7nm (60m) 海岸气溶胶
    "B2",  // 492.4nm (10m) 蓝光
    "B3",  // 559.8nm (10m) 绿光
    "B4",  // 664.6nm (10m) 红光
    "B5",  // 704.1nm (20m) 植被红边
    "B6",  // 740.5nm (20m) 植被红边
    "B7",  // 782.8nm (20m) 植被红边
    "B8",  // 832.8nm (10m) 近红外
    "B8A", // 864.7nm (20m) 窄带近红外
    "B9",  // 945.1nm (60m) 水汽
    // "B10", // 1373.5nm (60m) SWIR – 卷云（大气校正后移除）
    "B11", // 1613.7nm (20m) 短波红外1
    "B12", // 2202.4nm (20m) 短波红外2
  ]

// 获取Durban地区的Sentinel-2影像
var image = ee.Image("COPERNICUS/S2_SR/20190424T073619_20190424T080529_T36JUN");

// 仅保留反射率波段
image = image.select(bands);

// 将反射率从[0-10000]缩放到[0-1]
image = image.divide(10000);

// 计算NDVI
var ndvi = image.expression(
      '(NIR - RED) / (NIR + RED)', {  // NDVI计算公式
      'RED': image.select('B4'),  // 红光波段
      'NIR': image.select('B8')}  // 近红外波段
      ).rename('ndvi');  // 重命名为ndvi

// TODO 实现漂浮碎片指数（FDI）
var fdi = image.expression(
      'TODO fill equation', {  // FDI计算公式
        'a': image.select('TODO'),  // 选择波段
        'b': image.select('TODO'),  // 选择波段
        'c': image.select('TODO'),  // 选择波段
        'cor': 0 // TODO 添加lambda常数
      }
      ).rename('fdi');  // 重命名为fdi

// 在地图上添加FDI图层
Map.addLayer(fdi, fdi_viz, "S2 FDI Durban 2019-04-24");

// 在地图上添加NDVI图层
Map.addLayer(ndvi, ndvi_viz, "S2 NDVI Durban 2019-04-24");

// 在地图上添加RGB图层
Map.addLayer(image, rgb_viz, "S2 RGB Durban 2019-04-24");
```

```javascript
// 定义感兴趣区域（AOI）
var area_of_interest = 
    ee.Geometry.Polygon(
        [[[28.729885014656357, 41.002061784123136],
          [28.729885014656357, 40.718516594111634],
          [29.194057377937607, 40.718516594111634],
          [29.194057377937607, 41.002061784123136]]], null, false);

// 将 AOI 添加到地图上
Map.addLayer(area_of_interest, {}, "AOI");

// 定义可视化调色板
var magma = ["400b0b","a12424","ee7b06","ffa904","ffdb00"];  // magma 调色板
var viridis = ["450256","3B1C8C","21908D","5AC865","F9E721"];  // viridis 调色板

// 定义可视化参数
var fdi_viz = {"opacity":1,"bands":["fdi"],"min":0,"max":0.1,"palette":magma};  // FDI 可视化参数
var fai_viz = {"opacity":1,"bands":["fai"],"min":0,"max":0.1,"palette":magma};  // FAI 可视化参数
var viz_rgb = {"opacity":1,"bands":["B4","B3","B2"],"min":422.7282480252592,"max":917.2016396151902,"gamma":1};  // RGB 可视化参数
var ndvi_viz = {"opacity":1,"bands":["ndvi"],"min":-.4,"max":0.4,"palette":viridis};  // NDVI 可视化参数
var ndci_viz = {"opacity":1,"bands":["ndci"],"min":-.2,"max":0.2,"palette":viridis};  // NDCI 可视化参数

// 加载单幅 Sentinel-2 影像
var image = ee.Image("COPERNICUS/S2_SR/20210519T084601_20210519T084711_T35TPF");

// 定义计算 FDI 的函数
var calculate_fdi = function(image) {
  // nir-red / swir-red * 10 = 1.7722052470761769
  var correction = (832.8 - 664.6) / (1613.7 - 664.6) * 10;

  return image.expression(
      'NIR - (RE2 + (SWIR - RE2) * 1.7722052470761769)', {
      'RED': image.select('B4').divide(10000),  // 红光波段
      'RE2': image.select('B6').divide(10000),  // 红边波段
      'NIR': image.select('B8').divide(10000),  // 近红外波段
      'SWIR': image.select('B11').divide(10000) // 短波红外波段
  }).select(['B8'], ['fdi']);  // 重命名为 fdi
};

// 定义计算 FAI 的函数
var calculate_fai = function(image) {
  // nir-red / swir-red * 10 = 1.7722052470761769
  var correction = (832.8 - 664.6) / (1613.7 - 664.6) * 10;

  return image.expression(
      'NIR - (RED + (SWIR - RED) * 1.7722052470761769)', {
      'RED': image.select('B4').divide(10000),  // 红光波段
      'NIR': image.select('B8').divide(10000),  // 近红外波段
      'SWIR': image.select('B11').divide(10000) // 短波红外波段
  }).select(['B8'], ['fai']);  // 重命名为 fai
};

// 定义计算 NDVI 的函数
var calculate_ndvi = function(image) {
  return image.expression(
      '(NIR - RED) / (NIR + RED)', {
      'RED': image.select('B4').divide(10000),  // 红光波段
      'NIR': image.select('B8').divide(10000)  // 近红外波段
  }).select(['B8'], ['ndvi']);  // 重命名为 ndvi
};

// 定义计算 NDCI 的函数
var calculate_ndci = function(image) {
  return image.expression(
      '(RE - RED) / (RE + RED)', {
      'RED': image.select('B4').divide(10000),  // 红光波段
      'RE': image.select('B5').divide(10000)  // 红边波段
  }).select(['B5'], ['ndci']);  // 重命名为 ndci
};

// 加载单幅 Sentinel-2 影像
var image = ee.Image("COPERNICUS/S2_SR/20210519T084601_20210519T084711_T35TPF");

// 创建水体掩膜（SCL 波段值为 6 表示水体）
var waterMask = image.select('SCL').eq(6);  // 水体掩膜
var image = image.updateMask(waterMask);  // 应用水体掩膜

// 计算光谱指数
var ndvi = calculate_ndvi(image);  // 计算 NDVI
var fdi = calculate_fdi(image);    // 计算 FDI
var ndci = calculate_ndci(image);  // 计算 NDCI
var fai = calculate_fai(image);    // 计算 FAI

// 将地图中心设置为影像范围
Map.centerObject(image);

// 将影像和光谱指数添加到地图上
Map.addLayer(image, viz_rgb, "rgb");  // 添加 RGB 影像
Map.addLayer(fdi, fdi_viz, "fdi");    // 添加 FDI
Map.addLayer(ndvi, ndvi_viz, "ndvi"); // 添加 NDVI
Map.addLayer(ndci, ndci_viz, "ndci"); // 添加 NDCI
Map.addLayer(fai, fai_viz, "fai");    // 添加 FAI

// 定义阈值并创建分类掩膜
var threshold = 0.1;  // 定义阈值
var cls = fdi.gt(threshold);  // 创建二进制分类掩膜（1 = 海鼻涕，0 = 非海鼻涕）
Map.addLayer(cls);  // 将分类结果添加到地图上

// 计算分类掩膜的平均值
var mean_cls = cls.reduce(ee.Reducer.mean());
print(mean_cls);  // 打印分类掩膜的平均值

// 定义计算 FDI 的函数（与之前相同）
var calculate_fdi = function(image) {
  var correction = (832.8 - 664.6) / (1613.7 - 664.6) * 10;
  return image.expression(
      'NIR - (RE2 + (SWIR - RE2) * 1.7722052470761769)', {
      'RED': image.select('B4').divide(10000),
      'RE2': image.select('B6').divide(10000),
      'NIR': image.select('B8').divide(10000),
      'SWIR': image.select('B11').divide(10000)}
  ).select(['B8'], ['fdi']);
};

// 定义计算海鼻涕像素数量的函数
var get_fdi_threshold_pixels = function(image) {
  var waterMask = image.select('SCL').eq(6);  // 水体掩膜
  var image = image.updateMask(waterMask);  // 应用水体掩膜
  var image = image.clip(area_of_interest);  // 裁剪到感兴趣区域

  var fdi = calculate_fdi(image);  // 计算 FDI
  var cls = fdi.gt(threshold);  // 创建二进制分类掩膜

  // 计算分类掩膜的平均值
  var meanDict = cls.reduceRegion({
    reducer: ee.Reducer.mean(),  // 使用平均值 reducer
    geometry: cls.geometry(),  // 使用影像的几何范围
    scale: 10,  // 分辨率（10m）
    maxPixels: 1e9  // 处理大数据集
  });

  // 返回包含日期和海鼻涕像素比例的 Feature
  return ee.Feature(null, {
    'date': image.date().format('YYYY-MM-dd'),  // 影像日期
    'algae_pixel_fraction': meanDict.get('fdi')  // 海鼻涕像素比例
  });
};

// 加载 Sentinel-2 地表反射率数据
var s2Collection = ee.ImageCollection('COPERNICUS/S2_SR')
  .filterBounds(area_of_interest)  // 过滤空间范围（AOI）
  .filterDate('2016-01-01', '2025-01-01')  // 过滤时间范围
  .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 2));  // 过滤云量低于 2% 的影像

print(s2Collection);  // 打印过滤后的影像集合

// 对每幅影像计算海鼻涕像素比例
var algaeData = s2Collection.map(get_fdi_threshold_pixels);

// 将结果转换为 FeatureCollection
var algaeFeatureCollection = ee.FeatureCollection(algaeData);

// 打印结果
print('Algae Pixel Data:', algaeFeatureCollection);

// 绘制折线图
var chart = ui.Chart.feature.byFeature({
  features: algaeFeatureCollection,  // 数据源
  xProperty: 'date',  // X 轴：日期
  yProperties: ['algae_pixel_fraction']  // Y 轴：海鼻涕像素比例
})
.setChartType('LineChart')  // 折线图
.setOptions({
  title: 'Algae Pixel Fraction Over Time',  // 图表标题
  hAxis: {title: 'Date', format: 'YYYY-MM-dd'},  // X 轴标题和格式
  vAxis: {title: 'Algae Pixel Fraction'},  // Y 轴标题
  pointSize: 5,  // 点大小
  lineWidth: 2,  // 线宽
  colors: ['#1f78b4']  // 线条颜色
});

// 将图表添加到 UI
print(chart);
```



