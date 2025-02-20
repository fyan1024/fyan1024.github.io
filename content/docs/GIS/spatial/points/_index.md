+++
title = 'Points'
date = 2025-02-17T10:43:23+01:00
draft = false
+++

1. 拿到数据后先做EDA，例如直方图，散点图
2. 清洗数据，硬错误去除

## 方法

1. 清洗数据
    1. 格式转换。
    2. 删除冗余数据，顶点观测站等。
    3. 显而易见的错误，硬错误。写规则去清洗数据。
2. EDA
3. determine **home range** 某种意义上的 territory
    1. convex hull 凸包
4. 分析和预测数据


不同方法间是递进和逐步优化的关系，需要有理论支持

$$
\hat{f}(x, $$
\hat{f}(x, y) = \frac{1}{n h_x h_y} \sum_{i=1}^n K\left(\frac{x - x_i}{h_x}\right) K\left(\frac{y - y_i}{h_y}\right)
$$
\hat{f}(x, y) = \frac{1}{n h_x h_y} \sum_{i=1}^n K\left(\frac{x - x_i}{h_x}\right) K\left(\frac{y - y_i}{h_y}\right)y) = \frac{1}{n h_x h_y} \sum_{i=1}^n K\left(\frac{x - x_i}{h_x}\right) K\left(\frac{y - y_i}{h_y}\right)
$$



