---
title: "Wx Miniprogram 1"
date: 2024-02-28T22:20:55+08:00
draft: false
---

[toc]

### 用法




### 语法

#### 箭头函数

`getCityOptions(beds)` 函数的语法是箭头函数语法，箭头函数是一种简写形式的函数表达式，其语法如下：

```
(parameters) => expression
```

其中，`parameters` 是函数的参数列表，`expression` 是函数的返回值。

在 `getCityOptions(beds)` 函数中，参数 `beds` 是一个数组，代表所有床的信息。函数的作用是将数组中每个床的城市名称提取出来，并组成一个新的数组返回。

具体来说，函数首先使用 `map()` 方法遍历 `beds` 数组，对于每个床，使用 `bed.City` 语句获取其城市名称。然后，将所有城市名称组成一个新的数组，并使用 `return` 语句返回。

以下是 `getCityOptions(beds)` 函数的详细解释：

* `beds`：参数，代表所有床的信息，是一个数组。
* `map()`：方法，用于遍历数组并对每个元素进行操作。
* `bed.City`：语句，用于获取床的城市名称。
* `return`：语句，用于返回函数的返回值。

示例：

```javascript
const beds = [{
  City: '北京'
}, {
  City: '上海'
}, {
  City: '广州'
}];

const cities = getCityOptions(beds);

console.log(cities); // ['北京', '上海', '广州']
```

输出结果：

```
['北京', '上海', '广州']
```

箭头函数的优点是语法简洁，易于阅读和理解。在使用箭头函数时，需要注意以下几点：

* 箭头函数不能使用 `arguments` 对象。
* 箭头函数不能使用 `this` 关键字来指向当前对象。
* 箭头函数不能使用 `yield` 关键字。



### PS 知识点:

#### 样式选择器

`类选择器，类型选择器`

#### 样式优先级

`就近原则`

#### rpx（responsive pixel）

`可以根据屏幕宽度进行自适应。规定屏幕宽为750rpx。如在 iPhone6 上，屏幕宽度为375px，共有750个物理像素，则750rpx = 375px = 750物理像素，1rpx = 0.5px = 1物理像素。`

#### Flex布局

#### 导航







```js
  GetAllBedData() {
    db.collection('Bed').get({
      success: (res) => {
        console.log('查询成功，所有数据：', res.data);

        const cityoptions = res.data.map((bed) => bed.city);
        const uniquecityOptions = Array.from(new Set(cityoptions));

        console.log('查询所有城市选项：', cityoptions);
        console.log('写入城市选项：', uniquecityOptions);

        this.setData({
          beds: res.data,
          cityOptions: cityoptions,
          uniqueCityOptions: uniquecityOptions,
        });
      },
      fail: (err) => {
        console.error('查询失败：', err);
      },
    });
  },
```