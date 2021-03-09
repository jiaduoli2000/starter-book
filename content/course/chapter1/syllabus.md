---
title: Syllabus
linktitle: Syllabus
type: book
date: "2019-05-05T00:00:00+01:00"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

### **5.6 自定义调色板**

```text
ggsurvplot(fit, # 创建的拟合对象
           data = lung,  # 指定变量数据来源
           conf.int = TRUE, # 显示置信区间
           pval = TRUE, # 添加P值
           surv.median.line = "hv", # 添加中位生存时间线
           palette = "hue")  # 自定义调色板
可选调色板有 "grey","npg","aaas","lancet","jco", 
"ucscgb","uchicago","simpsons"和"rickandmorty".
```

![img](https://pic2.zhimg.com/80/v2-8579489fd61288268cd848e453fbe2fd_720w.jpg)