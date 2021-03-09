---
title: 绘制多个置信区间的森林图
linktitle: R语言统计与绘图
type: book
date: "2019-05-05T00:00:00+01:00"
# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 5
---

森林图的历史可以追溯到20世纪70年代，最常用于Meta分析中。`forestplot`包是绘制森林图的R包，其起源于rmeta包的forestplot函数，解决了forestplot函数的一些缺点，功能更为强大。

前面我们学习了使用forestplot包绘制简单的森林图，今天来学习下复杂点的森林图绘制，如下图所示，同一变量不同人群的置信区间合并在一个，共用一个坐标轴。

![图片](https://mmbiz.qpic.cn/mmbiz_png/ibNJyXnh77ib2mfMofYdfq7M8AMGOqSdUxXcn9rloiansF3x9RVgwmgEnd9P0ZkSecAq32G9seg3gJO4STMXdS7wg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

今天来学习在R中怎么绘制上面这种森林图。

------

**目 录**

- \1. 安装和加载R包

- \2. 读取数据

- \3. 数据处理

- - 3.1 定义亚组
  - 3.2 转换数据格式
  - 3.3 指定图形数据

- \4. 绘制简单图形

- \5. 自定义森林图参数

- \6. 参数解释

- - 6.1 对数坐标轴
  - 6.2 设置图例

- End

------

## 1. 安装和加载R包

forestplot包是基于rmeta包的forestplot函数创建的，但是功能更强大，可以对同一标签添加多个置信区间。

```R
install.packages("forestplot")  #安装forestplot包 
library(forestplot)  # 加载包 
```

## 2. 读取数据

以上图的数据为例，将数据录入到Excel中，录入数据格式如下图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/ibNJyXnh77ib2mfMofYdfq7M8AMGOqSdUxqvhY2cMTNgkIT4giaaiaNrlObqBGhmST3icXqia7JeWSXKyrkhaqdR40xw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```r
library(readxl) # 加载包
forest <- read_excel("forest.xlsx") # 导入数据
View(forest) # 预览数据 
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/ibNJyXnh77ib2mfMofYdfq7M8AMGOqSdUxApUZsbclZaaDNpk6IKVGpQp81DLws4eQiaIYlAhm23rtiaF36NPibyxIA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)展示部分数据

## 3. 数据处理

### 3.1 定义亚组

在论文图片中，为了区分以及显示的更清楚，亚组一般会向前缩进两个空格。

```R
subgps <- c(7,8,9,12,13,14,17,18,19,22,23,24)   
# 指定要缩进的亚组，此处向量中的数字表示亚组的行数  
forest$Variable[subgps] <- paste("    ",forest$Variable[subgps])    
# 亚组前添加两空格  
View(forest) # 预览数据看看有没有添加空格
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/ibNJyXnh77ib2mfMofYdfq7M8AMGOqSdUxgib7QGZraSZUrYq6ACM6ibdjMxR9cTYsIXR8RIoEbAlQMVGI3K5FJFTg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 3.2 转换数据格式

```r
attach(forest)  # 绑定数据框  
labeltext <- as.matrix(forest[,1:3]) 
# 将forest数据框的前3列转换为矩阵
# 以矩阵形式将文本数据导入函数
```

### 3.3 指定图形数据

分别指定哪些数据是HR、95%CI上限、95%CI下限。

```r
coef <- with(forest, cbind(coef1, coef2)) # 指定HR数据
low <- with(forest, cbind(low1, low2))  # 指定95%CI下限数据
high <- with(forest, cbind(high1, high2)) # 指定95%CI上限数据
```

## 4. 绘制简单图形

```r
forestplot(labeltext, # 森林图文本部分
           mean = coef, # 图形元素中HR部分
           lower = low,  # 图形元素中置信区间下限
           upper = high) # 图形元素中置信区间上限
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/ibNJyXnh77ib2mfMofYdfq7M8AMGOqSdUx76xbcMaIWoKVjFQSafvicicdAKd9KgOAZ2ZUqGN9CToq3LBucnkxe9ibQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

与原图相比，需要修改的地方有很多：

1. 修改坐标轴标签
2. 添加无效线
3. 添加水平线
4. 调整box大小
5. 字体对齐
6. 图形元素的位置
7. 图形元素的颜色
8. 修改坐标轴刻度
9. 添加图例

前面的推文已经介绍了森林图各种参数的调整，因此就直接修改了(点击下面**红色链接**查看参数介绍)。

[R语言统计与绘图：forestplot包绘制森林图](https://mp.weixin.qq.com/s?__biz=MzU4OTc0OTg2MA==&mid=2247485629&idx=1&sn=48d71fa804d705f456faca1bdcca23f6&scene=21#wechat_redirect)

## 5. 自定义森林图参数

修改坐标轴标签、添加无效线、水平线、调整box大小、字体对齐、图形元素、添加图例、修改坐标轴刻度等直接一起修改啦。

```R
my_xticks <- c(0.004,0.04,0.2,1,5,25,250) # 设置需要显示的x轴刻度 
attr(my_xticks, "labels") <- xticks # 将x轴刻度标签传递给xticks参数

forestplot(labeltext, # 森林图文本部分
           mean = coef, # 图形元素中HR部分
           lower = low, # 图形元素中置信区间下限
           upper = high, # 图形元素中置信区间上限
           zero = 1, lwd.zero = 2, # 设置无效线位置和线条宽度
           graph.pos = 2, # 设置图形元素的位置
           boxsize = 0.5, # 设置box的大小
           align = "l", # 设置字体对齐方式
           xlog = TRUE,  # 设置对数坐标轴
           xlab="Mortality", # 添加x轴标签
           xticks = my_xticks, # 自定义x轴刻度标签
           hrzl_lines = list("2" = gpar(lty=1, columns=c(3:4), col = "black"),
                             "3" = gpar(lty=1, columns=c(1:4), col = "black")),
           # 在第2行和第3行添加水平线，并设置水平线的线型、宽度和颜色 
           is.summary= c(T,F,T,F,T,F,F,F,F,T,F,F,F,F,T,F,F,F,F,T,F,F,F,F),
           # 指定哪些行是summary行
           txt_gp = fpTxtGp(label = gpar(cex = 1.15), # 设置文本字体大小 
                            ticks = gpar(cex = 1.20), # 设置坐标轴刻度大小
                            xlab = gpar(cex = 1.45)), # 设置坐标轴标题大小
           # 设置文本标签、刻度、x轴标签的大小
           col=fpColors(box=c("#B30638", "#00549E")),
           # 设置box的颜色
           legend=c("No-AKI", "AKI"), # 设置图例分组           
           legend_args = fpLegend(pos = list(x =0.20, y=0.05),
                                  r = unit(.1, "snpc")))
           # 设置图例位置和大小 
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/ibNJyXnh77ib2mfMofYdfq7M8AMGOqSdUxmRU171XUH3gY71gk7YLFsvGvVY33OdohLLpQyrWjgv77moqtMibMSqw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

如上所示为绘制的森林图，与论文原图基本是一致的，论文原图是没有加上图例的，但是我觉得加上图例显示的更清楚，顺便也学习下森林图图例参数怎么用。

## 6. 参数解释

森林图的大多数参数在前面的推文中已经介绍的很详细了，这里就不多介绍了(点击下面**红色链接**可查看参数介绍)。

[R语言统计与绘图：forestplot包绘制森林图](https://mp.weixin.qq.com/s?__biz=MzU4OTc0OTg2MA==&mid=2247485629&idx=1&sn=48d71fa804d705f456faca1bdcca23f6&scene=21#wechat_redirect)

这里介绍下前面推文没有介绍的参数。

### 6.1 对数坐标轴

在forestplot包中，使用`xlog`参数来设置对数坐标轴，使用`xticks`参数来自定义坐标轴刻度。

```R
xlog  # 逻辑词，默认FALSE，为TRUE则设置坐标轴；
xticks # 自定义设置x轴刻度；

# 在设置为对数坐标轴后，x轴标签可通过下列代码在指定刻度处指定输出文本
my_xticks <- c(0.004,0.04,0.2,1,5,25,250) # 设置需要显示的x轴刻度标签  
attr(my_xticks, "labels") <- xticks # 将x轴刻度标签传递给xticks参数

# 再在forestplot()函数中加入下面代码可自定义指定刻度标签。
xticks = my_xticks
```

### 6.2 设置图例

对于有多个置信区间的森林图，可以添加图例来表示不同置信区间。

```R
legend # 字符向量，指定不同置信区间的图例 
legend_args  # 和fpLegend()函数配合使用设置图例的样式 

# fpLegend()函数
legend_args = fpLegend(pos = "top",          
                       gp = NULL,          
                       r = unit(0, "snpc"),          
                       padding = unit(ifelse(!is.null(gp), 3, 0), "mm"),          
                       title = NULL)
# 参数解释
pos # 指定图例的位置；
# 如果图例在图形元素外，则可以设置为"top"或"right"；
# 如果需要将图例放在图形元素中，则可以指定x和y来设置位置
# 用法为 pos=list(x=1，y=1)，x、y在0-1之间。

gp # 和gpar()函数配合使用，设置图例的边框、背景样式；用法：
gp = gpar(fill = "black", # 设置背景填充颜色
          col = "black", # 设置边框颜色
          lwd = 2, # 设置边框的线条宽度
          lty = 1) # 设置边框的线条类型
r # 可设置图例边框为圆角边缘；snpc为单位，修改前面的数字设置边缘弧度；

padding # 只有绘制图例边框时才生效，设置图例文本到边框的距离，单位为mm
title # 图例的标题
```

------

森林图还有一种图形，多个置信区间多个坐标轴的。如下图：

![图片](https://mmbiz.qpic.cn/mmbiz_png/ibNJyXnh77ib3iaaf6f37vzNTmvTazHJuHQXcJMWVCQPKTvrFerRicbf6deRLeh3Pe0tWNjYpIuu6jAIqd7rXdUkFg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这种森林图，好像没在`forestplot`包中找到绘制方法，实在需要就单个单个绘制，导出PDF，在AI中合并图形就好了，下次出篇推文介绍怎么绘制这种森林图。