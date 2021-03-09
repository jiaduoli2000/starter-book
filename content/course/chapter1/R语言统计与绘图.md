---
title: 绘制多个置信区间的森林图
linktitle: 1.绘制多个置信区间的森林图
type: book
date: "2019-05-05T00:00:00+01:00"
# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

## **前言**

接着上一篇文章来，**[双击这里查看](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/5uGNBLX2INnm0qQNCgb8wg)**。

今天讲讲怎么调整临床论文中的基线特征`表1`输出格式。

通过调整`print()`函数参数来修改格式。

## **调整表中小数位数**

```text
tab4 <- CreateTableOne(vars = myVars, strata = "status" , data = colon, factorVars = catVars,addOverall = TRUE)
print(tab4, nonnormal = nonvar, exact = "extent",
      catDigits = 2,contDigits = 3)
```

![img](https://pic1.zhimg.com/80/v2-a1c035d86c919ea136e4c80c421784a8_720w.jpg)

修改连续变量小数位数为`3`位，分类变量百分比位数为`2`位。

## **调整p值小数位数**

```text
print(tab4, nonnormal = nonvar, exact = "extent",
      pDigits = 4)  # 调整小数位数为4位
```

![img](https://pic1.zhimg.com/80/v2-f2240786a98718871e373bcbed47e1e4_720w.jpg)

## **取消统计检验**

```text
print(tab4, nonnormal = nonvar, exact = "extent",
      test = FALSE)  # 不进行统计检验
```

![img](https://pic4.zhimg.com/80/v2-b7c564d6709d6d675f64852a87fab50f_720w.jpg)

**其他格式调整见下**

## **print.TableOne**

微调基线特征表输出格式。

```text
print(
  x,  # CreateTableOneh函数创建的对象
  catDigits = 1, # 分类变量中百分比的小数位数，默认为1位
  contDigits = 2, # 连续变量的小数位数，默认为2位
  pDigits = 3, # p值的小数位数，也可以用于标准化均值差异，默认为3
  quote = FALSE, # 是否在引号内显示内容，默认为FALSE；为TRUE，则将所有内容加上引号，以便复制到EXCEL
  missing = FALSE, # 是否显示缺失数据信息
  explain = TRUE, # 默认为TRUE，将(%)添加到分类变量名称中
  printToggle = TRUE, # 默认为TREU；为FALSE，则不创建任何输出
  test = TRUE, # 是否显示统计检验结果p值，默认为TRUE；为FALSE，则仅显示汇总结果
  smd = FALSE, # 是否显示标准化均值差异，默认FALSE
  noSpaces = FALSE, # 删除R控制平台中为对齐而产生的空格，复制到其他软件中时选TRUE。
  padColnames = FALSE, # 是否用空格填充列名以居中对齐，默认为FALSE，如果noSpaces = TRUE.则不进行
  varLabels = FALSE, # 是否用从labelled :: var_label（）函数获得的变量标签替换变量名。
  format = c("fp", "f", "p", "pf")[1], # 默认显示为“fp”频率（百分比），“f”为频率，“p”为百分比、“pf”为百分比（频率）。
  showAllLevels = FALSE, # 是否显示分类变量所有水平，默认为FALSE。
  cramVars = NULL, # 字符向量，指定二分类变量的两个水平在同一行显示
  dropEqual = FALSE, # 二分类变量中不显示第二水平名称；三分类及以上全都显示
  exact = NULL, # 字符向量，指定哪些分类变量进行精确检验，默认所有分类变量进行卡方检验
  nonnormal = NULL, # 字符向量，指定哪些连续变量进行非参数检验，默认所有连续变量为正态分布
  minMax = FALSE, # 逻辑词，默认为FALSE，呈非正态分布的连续变量，是否使用[min,max]代替四分位数。
  ...
)
```

## **End**