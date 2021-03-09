---
title: Kaplan-Meier生存曲线绘制
linktitle: Kaplan-Meier生存曲线绘制
type: book
date: "2019-05-05T00:00:00+01:00"
# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

Forest plots are often used in clinical trial reports to show differences in the estimated treatment effect(s) across various patient subgroups. See, for example a [review](http://trialsjournal.biomedcentral.com/articles/10.1186/1745-6215-8-36). The page on [Clinical Trials Safety Graphics](https://www.ctspedia.org/do/view/CTSpedia/StatGraphHome) includes a SAS code for a forest plot that depicts the hazard ratios for various patient subgroups (this web page has links to other interesting clinical trial graphics, as well as links to notes on best practices for graphs, and is worth checking out).

The purpose of this blog post is to create the same forest plot using R.

It should be possible to create such a graphic from first principles, using either base R graphics or using the *ggplot2* package such as [posted here](https://mcfromnz.wordpress.com/2012/11/06/forest-plots-in-r-ggplot-with-side-table/). However, there is a contributed package *[forestplot](https://cran.r-project.org/web/packages/forestplot/index.html)* that makes it very easy to make forest plots interspersed with tables – we just need to supply the right arguments to the *forestplot* function in the package. Also, one question that arose was how easy it would be to get the horizontal grey bands for alternate rows in the forest plot. This too was not very difficult. Following is a short explanation of the entire process, as well as the relevant R code:

First, we store the data for the plot, [ForestPlotData](https://designdatadecisions.files.wordpress.com/2016/07/forestplotdata.xlsx) in any convenient file format and then read it into a dataframe in R:

```
workdir <- ``""``C:\\Path\\To\\Relevant\\Directory``""``datafile <- ``file.path``(workdir,``"ForestPlotData.csv"``)``data <- ``read.csv``(datafile, stringsAsFactors=``FALSE``)
```

Then format the data a bit so that the column labels and columns match the required graphical output:

```
## Labels defining subgroups are a little indented!``subgps <- ``c``(4,5,8,9,12,13,16,17,20,21,24,25,28,29,32,33)``data$Variable[subgps] <- ``paste``(``" "``,data$Variable[subgps]) `` ` `## Combine the count and percent column``np <- ``ifelse``(!``is.na``(data$Count), ``paste``(data$Count,``" ("``,data$Percent,``")"``,sep=``""``), ``NA``)`` ` `## The rest of the columns in the table. ``tabletext <- ``cbind``(``c``(``"Subgroup"``,``"\n"``,data$Variable), ``          ``c``(``"No. of Patients (%)"``,``"\n"``,np), ``          ``c``(``"4-Yr Cum. Event Rate\n PCI"``,``"\n"``,data$PCI.Group), ``           ``c``(``"4-Yr Cum. Event Rate\n Medical Therapy"``,``"\n"``,data$Medical.Therapy.Group), ``          ``c``(``"P Value"``,``"\n"``,data$P.Value))
```



Finally, include the *forestplot* R package and call the *forestplot* function with appropriate arguments.

The way I got around to creating the horizontal band at every alternate row was by using settings for a very thick transparent line in the *hrzl_lines* argument! See below. The *col=”#99999922″* option gives the light grey color to the line as well as sets it to be transparent.

A graphics device (here, a png file) with appropriate dimensions is first opened and the forest plot is saved to the device.

```
library``(forestplot)``png``(``file.path``(workdir,``"Figures\\Forestplot.png"``),width=960, height=640)``forestplot``(labeltext=tabletext, graph.pos=3, ``      ``mean=``c``(``NA``,``NA``,data$Point.Estimate), ``      ``lower=``c``(``NA``,``NA``,data$Low), upper=``c``(``NA``,``NA``,data$High),``      ``title=``"Hazard Ratio"``,``      ``xlab=``"   <---PCI Better---  ---Medical Therapy Better--->"``,``      ``hrzl_lines=``list``(``"3"` `= ``gpar``(lwd=1, col=``"#99999922"``), ``             ``"7"` `= ``gpar``(lwd=60, lineend=``"butt"``, columns=``c``(2:6), col=``"#99999922"``),``             ``"15"` `= ``gpar``(lwd=60, lineend=``"butt"``, columns=``c``(2:6), col=``"#99999922"``),``             ``"23"` `= ``gpar``(lwd=60, lineend=``"butt"``, columns=``c``(2:6), col=``"#99999922"``),``             ``"31"` `= ``gpar``(lwd=60, lineend=``"butt"``, columns=``c``(2:6), col=``"#99999922"``)),``      ``txt_gp=``fpTxtGp``(label=``gpar``(cex=1.25),``               ``ticks=``gpar``(cex=1.1),``               ``xlab=``gpar``(cex = 1.2),``               ``title=``gpar``(cex = 1.2)),``      ``col=``fpColors``(box=``"black"``, lines=``"black"``, zero = ``"gray50"``),``      ``zero=1, cex=0.9, lineheight = ``"auto"``, boxsize=0.5, colgap=``unit``(6,``"mm"``),``      ``lwd.ci=2, ci.vertices=``TRUE``, ci.vertices.height = 0.4)``dev.off``()
```



Here is the resulting forest plot:

![Forestplot](https://designdatadecisions.files.wordpress.com/2016/07/forestplot1.png?w=609&h=406)