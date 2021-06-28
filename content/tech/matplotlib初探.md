---
title: "Matplotlib初探"
date: 2021-06-20T17:22:31+08:00
draft: false
tags: [python, 数据分析]
---

## matplotlib相关资源

> - [matplotlib中文网](https://matplotlib.org.cn/intro/)
>
> - [matplotlib教程](https://matplotlib.org.cn/tutorials/)

## matplotlib基础

### 绘图案例

``` python
import matplotlib.pyplot as plt  # 导入
plt.figure(figsize=(10,8), dpi=200, facecolor='white')  # 创建画板
x = np.arange(0, 1.1, 0.1)
y = x**2
plt.title('图的标题')
plt.xlabel('x轴标签')
plt.ylabel('y轴标签')
plt.xlim([0,1])  # x轴范围
plt.ylim([0,1])  # y轴范围
plt.xticks([0,0.2,0.4,0.6,0.8,1])  # x轴刻度
plt.yticks([0,0.2,0.4,0.6,0.8,1])  # y轴刻度
plt.plot(x, y, label='y=x^2')  # 绘图命令
plt.legend(loc='best')  # 显示图例（如果要放外面，可以用bbox_to_anchor参数）
plt.savefig('img/test.png')  # 保存
plt.show()  # 展示

# 中文和符号的正常显示
plt.rcParams['font.sans-serif'] = ['SimHei']
plt.rcParams['axes.unicode_minus'] = False
```

一个Figure可以创建多个子图subplot。

``` python
fig = plt.figure()
ax1 = fig.add_subplot(2,2,1)  # 2×2的画板的第1个子图
ax2 = fig.add_subplot(2,2,2)  # 2×2的画板的第2个子图
```

图像参数设置。

``` python
fig = plt.gcf()  # 返回参数设置对象
fig.set_size_inches(12, 14)  # 设置画布大小
```

网格线设置。

``` python
plt.grid(ls='--', c='darkblue')
```

参考线。

``` python
plt.axhline(y=1700, c='red')  # 水平参考线
plt.axvline(x=4, c='red')  # 水平参考线
```

参考区域。

``` python
plt.axvspan(xmin=4, xmax=6, alpha=0.3)  # 绘制纵向参考区域
plt.axhspan(xmin=14, xmax=26, alpha=0.3)  # 绘制水平参考区域
```



### plot参数

> plt.plot(x, y, ls=, lw=, c=, maker=, makersize=, makeredgecolor=, makerfacecolor=, label=)
>
> - x：x轴上的数值
> - y：y轴上的数值
> - ls：折现的风格（如'-', ',', '--'等）
> - lw：线条宽度
> - c：颜色
> - maker：线条上点的形状
> - makersize：线条上点的大小
> - makeredgecolor：线条上点的边框色
> - makerfacecolor：线条上点的填充色
> - label：文本标签

#### 颜色参数

``` text
'aliceblue':            '#F0F8FF',
'antiquewhite':         '#FAEBD7',
'aqua':                 '#00FFFF',
'aquamarine':           '#7FFFD4',
'azure':                '#F0FFFF',
'beige':                '#F5F5DC',
'bisque':               '#FFE4C4',
'black':                '#000000',
'blanchedalmond':       '#FFEBCD',
'blue':                 '#0000FF',
'blueviolet':           '#8A2BE2',
'brown':                '#A52A2A',
'burlywood':            '#DEB887',
'cadetblue':            '#5F9EA0',
'chartreuse':           '#7FFF00',
'chocolate':            '#D2691E',
'coral':                '#FF7F50',
'cornflowerblue':       '#6495ED',
'cornsilk':             '#FFF8DC',
'crimson':              '#DC143C',
'cyan':                 '#00FFFF',
'darkblue':             '#00008B',
'darkcyan':             '#008B8B',
'darkgoldenrod':        '#B8860B',
'darkgray':             '#A9A9A9',
'darkgreen':            '#006400',
'darkkhaki':            '#BDB76B',
'darkmagenta':          '#8B008B',
'darkolivegreen':       '#556B2F',
'darkorange':           '#FF8C00',
'darkorchid':           '#9932CC',
'darkred':              '#8B0000',
'darksalmon':           '#E9967A',
'darkseagreen':         '#8FBC8F',
'darkslateblue':        '#483D8B',
'darkslategray':        '#2F4F4F',
'darkturquoise':        '#00CED1',
'darkviolet':           '#9400D3',
'deeppink':             '#FF1493',
'deepskyblue':          '#00BFFF',
'dimgray':              '#696969',
'dodgerblue':           '#1E90FF',
'firebrick':            '#B22222',
'floralwhite':          '#FFFAF0',
'forestgreen':          '#228B22',
'fuchsia':              '#FF00FF',
'gainsboro':            '#DCDCDC',
'ghostwhite':           '#F8F8FF',
'gold':                 '#FFD700',
'goldenrod':            '#DAA520',
'gray':                 '#808080',
'green':                '#008000',
'greenyellow':          '#ADFF2F',
'honeydew':             '#F0FFF0',
'hotpink':              '#FF69B4',
'indianred':            '#CD5C5C',
'indigo':               '#4B0082',
'ivory':                '#FFFFF0',
'khaki':                '#F0E68C',
'lavender':             '#E6E6FA',
'lavenderblush':        '#FFF0F5',
'lawngreen':            '#7CFC00',
'lemonchiffon':         '#FFFACD',
'lightblue':            '#ADD8E6',
'lightcoral':           '#F08080',
'lightcyan':            '#E0FFFF',
'lightgoldenrodyellow': '#FAFAD2',
'lightgreen':           '#90EE90',
'lightgray':            '#D3D3D3',
'lightpink':            '#FFB6C1',
'lightsalmon':          '#FFA07A',
'lightseagreen':        '#20B2AA',
'lightskyblue':         '#87CEFA',
'lightslategray':       '#778899',
'lightsteelblue':       '#B0C4DE',
'lightyellow':          '#FFFFE0',
'lime':                 '#00FF00',
'limegreen':            '#32CD32',
'linen':                '#FAF0E6',
'magenta':              '#FF00FF',
'maroon':               '#800000',
'mediumaquamarine':     '#66CDAA',
'mediumblue':           '#0000CD',
'mediumorchid':         '#BA55D3',
'mediumpurple':         '#9370DB',
'mediumseagreen':       '#3CB371',
'mediumslateblue':      '#7B68EE',
'mediumspringgreen':    '#00FA9A',
'mediumturquoise':      '#48D1CC',
'mediumvioletred':      '#C71585',
'midnightblue':         '#191970',
'mintcream':            '#F5FFFA',
'mistyrose':            '#FFE4E1',
'moccasin':             '#FFE4B5',
'navajowhite':          '#FFDEAD',
'navy':                 '#000080',
'oldlace':              '#FDF5E6',
'olive':                '#808000',
'olivedrab':            '#6B8E23',
'orange':               '#FFA500',
'orangered':            '#FF4500',
'orchid':               '#DA70D6',
'palegoldenrod':        '#EEE8AA',
'palegreen':            '#98FB98',
'paleturquoise':        '#AFEEEE',
'palevioletred':        '#DB7093',
'papayawhip':           '#FFEFD5',
'peachpuff':            '#FFDAB9',
'peru':                 '#CD853F',
'pink':                 '#FFC0CB',
'plum':                 '#DDA0DD',
'powderblue':           '#B0E0E6',
'purple':               '#800080',
'red':                  '#FF0000',
'rosybrown':            '#BC8F8F',
'royalblue':            '#4169E1',
'saddlebrown':          '#8B4513',
'salmon':               '#FA8072',
'sandybrown':           '#FAA460',
'seagreen':             '#2E8B57',
'seashell':             '#FFF5EE',
'sienna':               '#A0522D',
'silver':               '#C0C0C0',
'skyblue':              '#87CEEB',
'slateblue':            '#6A5ACD',
'slategray':            '#708090',
'snow':                 '#FFFAFA',
'springgreen':          '#00FF7F',
'steelblue':            '#4682B4',
'tan':                  '#D2B48C',
'teal':                 '#008080',
'thistle':              '#D8BFD8',
'tomato':               '#FF6347',
'turquoise':            '#40E0D0',
'violet':               '#EE82EE',
'wheat':                '#F5DEB3',
'white':                '#FFFFFF',
'whitesmoke':           '#F5F5F5',
'yellow':               '#FFFF00',
'yellowgreen':          '#9ACD32'
```

#### 形状参数

``` text
'.'       point marker
','       pixel marker
'o'       circle marker
'v'       triangle_down marker
'^'       triangle_up marker
'<'       triangle_left marker
'>'       triangle_right marker
'1'       tri_down marker
'2'       tri_up marker
'3'       tri_left marker
'4'       tri_right marker
's'       square marker
'p'       pentagon marker
'*'       star marker
'h'       hexagon1 marker
'H'       hexagon2 marker
'+'       plus marker
'x'       x marker
'D'       diamond marker
'd'       thin_diamond marker
'|'       vline marker
'_'       hline marker
```

### 设置绘图风格

``` python
plt.style.use('ggplot')
```

``` python
print(plt.style.available) # 打印样式列表
['Solarize_Light2', '_classic_test_patch', 'bmh', 'classic', 'dark_background', 'fast', 'fivethirtyeight', 'ggplot', 'grayscale', 'seaborn', 'seaborn-bright', 'seaborn-colorblind', 'seaborn-dark', 'seaborn-dark-palette', 'seaborn-darkgrid', 'seaborn-deep', 'seaborn-muted', 'seaborn-notebook', 'seaborn-paper', 'seaborn-pastel', 'seaborn-poster', 'seaborn-talk', 'seaborn-ticks', 'seaborn-white', 'seaborn-whitegrid', 'tableau-colorblind10']
```

### 设置图中文字

``` python
plt.annotate('标注文字', xy=(2.5,4))  # 文字位置
plt.annotate('标注文字2', xy=(6, 6), xytext=(4, 8),
            arrowprops=dict(facecolor='black'),
            )
#文本位置 xytext  ，箭头终点xy
```

## 直方图

用`plt.hist()`函数绘制直方图。

> plt.hist(x, bins=10, range=None, normed=False, weights=None, cumulative=False, bottom=None, histtype=‘bar’, align=‘mid’, orientation=‘vertical’, rwidth=None, log=False, color=None, label=None, stacked=False)
>
> - x：指定要绘制直方图的数据；
>
> - bins：指定直方图条形的个数；
>
> - range：指定直方图数据的上下界，默认包含绘图数据的最大值和最小值；
>
> - normed：是否将直方图的频数转换成频率；
>
> - weights：该参数可为每一个数据点设置权重；
>
> - cumulative：是否需要计算累计频数或频率；
>
> - bottom：可以为直方图的每个条形添加基准线，默认为0；
>
> - histtype：指定直方图的类型，默认为bar，除此还有’barstacked’, ‘step’, ‘stepfilled’；
>
> - align：设置条形边界值的对其方式，默认为mid，除此还有’left’和’right’；
>
> - orientation：设置直方图的摆放方向，默认为垂直方向；
>
> - rwidth：设置直方图条形宽度的百分比；
>
> - log：是否需要对绘图数据进行log变换；
>
> - color：设置直方图的填充色；
>
> - label：设置直方图的标签，可通过legend展示其图例；
>
> - stacked：当有多个数据时，是否需要将直方图呈堆叠摆放，默认水平摆放；



## 饼图

用`plt.pie()`函数绘制饼图。

> plt.pie(x, explode, labels, colors, autopct, pctdistance, shadow, starangle, radius, wedgeprops, textprops, center)
>
> - x：数值
> - explode：突出显示
> - labels：标签
> - colors：颜色
> - autopct：百分比格式
> - pctdistance：百分比标签与圆心的距离
> - shadow：是否添加饼图阴影效果
> - labeldistance：设置各扇形标签与圆心的距离
> - startangle：设置饼图的初始摆放角度
> - radius：设置饼图半径大小
> - counterclock：是否逆时针呈现
> - wedgeprops：设置饼图内外边界的属性
> - textprops：设置饼图中文本属性
> - center：设置中心位置

``` python
plt.pie(x=[59, 78], explode=[0, 0.2], labels=['Male', 'Female'], \ 
        shadow=True, colors=['red', 'yellow'], autopct='%.1f%%', \
        pctdistance=0.5, labeldistance=1.1, \
        startangle=120, radius=1.2, counterclock=False, \
        wedgeprops={'linewidth': 1.5, 'edgecolor': 'green'}, \
        textprops={'fontsize': 10, 'color': 'black'})
plt.title('男女平均年收入', pad=20)
plt.show()
```

![pie1](../images/matplotlib初探/pie1.png)

## 条形图

### 普通条形图

用`plt.bar()`函数绘制水平条形图（如果要绘制纵向的用`plt.barh()`）。

> plt.bar(x, height, width, bottom, color, edgecolor, linewidth, ticke_label, align, hatch)
>
> - x：柱子在`x`轴上的坐标。浮点数或类数组结构。**注意`x`可以为字符串数组**
> - height：柱子的高度，即`y`轴上的坐标。浮点数或类数组结构
> - width：柱子的宽度。浮点数或类数组结构。默认值为`0.8`
> - bottom：柱子的基准高度。浮点数或类数组结构。默认值为`0`
> - color：条形图的填充色
> - edgecolor：条形图的边框色
> - linewidth：条形图边框宽度
> - tick_label：条形图的刻度标签
> - align：指定x轴上对齐方式
> - hatch：柱子填充符号 {'/', '\', '|', '-', '+', 'x', 'o', 'O', '.', '*'} 符号可以组合，例如`/+`，多个重复符号，增加密度

柱子的位置由`x`以及`align`确定 ，柱子的尺寸由`height`和 `width` 确定。垂直基准位置由`bottom`确定(默认值为0)。大部分参数即可以是单独的浮点值也可以是值序列，单独值对所有柱子生效，值序列一一对应每个柱子。

``` python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

x_data = range(4)
y_data = 10*np.exp(x_data)
plt.bar(x=x_data, height=y_data, width=[0.1, 0.2, 0.3, 0.4], \
        align='center', edgecolor=['w', 'w', 'w','r'], \
        color=['steelblue', 'thistle', 'salmon', 'skyblue'], \
        linewidth=[0, 0, 10, 2], tick_label=[str(x)+'s' for x in x_data])
plt.ylim([0, 230])
for x,y in enumerate(y_data):
    # 前两个参数表示标签的坐标位置，第三个参数表示标签的值
    plt.text(x, y+5,'%s'%round(y,1)+'m', ha='center')
plt.savefig('bar1.png')
plt.show()
```

![bar1](../images/matplotlib初探/bar1.png)

### 堆叠条形图

利用`bottom`参数，使用多个`plt.bar()`，可以绘制堆叠条形图。



## 散点图

用`plt.scatter()`函数绘制散点图。

> plt.scatter(x, y, s=20, 
>             c=None, marker='o', 
>             cmap=None, norm=None, 
>             vmin=None, vmax=None, 
>             alpha=None, linewidths=None, 
>             edgecolors=None)
>
> - x：指定散点图的x轴数据；
> - y：指定散点图的y轴数据；
> - s：指定散点图点的大小，默认为20，通过传入新的变量，实现气泡图的绘制；
> - c：指定散点图点的颜色，默认为蓝色；
> - marker：指定散点图点的形状，默认为圆形；
> - cmap：指定色图，只有当c参数是一个浮点型的数组的时候才起作用；
> - norm：设置数据亮度，标准化到0~1之间，使用该参数仍需要c为浮点型的数组；
> - vmin、vmax：亮度设置，与norm类似，如果使用了norm则该参数无效；
> - alpha：设置散点的透明度；
> - linewidths：设置散点边界线的宽度；
> - edgecolors：设置散点边界线的颜色；

## 箱线图

