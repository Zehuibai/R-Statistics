# Untitled

## Template

````
---
title: ""
author: "Zehui Bai"
date: 'Stand: `r format(Sys.time(), "%F %H:%M Uhr")`'
output:
  html_document:
    df_print: paged
    number_sections: yes
    # theme: darkly
    toc: yes
    toc_float: yes
fontsize: 10pt
editor_options:
  chunk_output_type: console
colorlinks: yes
---

```{r setup, include=FALSE, echo = FALSE,message = FALSE, error = FALSE, warning = FALSE}
knitr::opts_chunk$set(echo = TRUE)

## done quickly and concisely
pkgs = c("raster", "leaflet", "rgeos") 
## install.packages(pkgs)
## load package
inst = lapply(pkgs, library, character.only = TRUE) 
```


# First Section

```{r,echo = T,message = FALSE, error = FALSE, warning = FALSE}

```
````

## Package prettydoc

```
---
title: "Your Document Title"
author: |
  | Duke A Caboom,    MD
  | Justin d'Ottawa,  PhD
date: "`r Sys.Date()`"


output:
  prettydoc::html_pretty:
    theme: leonids       ## Currently supported themes are "cayman", "tactile", "architect", "leonids", and "hpstr"
    highlight: github
---
```



## Package rmdformats

````
---
title: ""
date: "`r Sys.Date()`"
output:
  rmdformats::material:
    highlight: kate
---


```{r setup, echo=FALSE, cache=FALSE}
library(knitr)
library(rmdformats)

## Global options
options(max.print="75")
opts_chunk$set(echo=FALSE,
	             cache=TRUE,
               prompt=FALSE,
               tidy=TRUE,
               comment=NA,
               message=FALSE,
               warning=FALSE)
opts_knit$set(width=75)
```
````



````
---
title: ""
date: "`r Sys.Date()`"
output:
  rmdformats::html_clean:
    highlight: kate
---


```{r setup, echo=FALSE, cache=FALSE}
library(knitr)
library(rmdformats)

## Global options
options(max.print="75")
opts_chunk$set(echo=FALSE,
	             cache=TRUE,
               prompt=FALSE,
               tidy=TRUE,
               comment=NA,
               message=FALSE,
               warning=FALSE)
opts_knit$set(width=75)
```
````





##  HTML document

###  Contents and Section

```
title: "Habits"
output:
  html_document:
    toc: true                                          ## Table of contents
    toc_depth: 2
    number_sections: true                   ## Section numbering
```

###  Tabbed sections

可以指定两个其他属性来控制选项卡的外观和行为。 .tabset-fade属性使选项卡在选项卡之间切换时淡入和淡出。 .tabset-pills属性使选项卡的视觉外观为“药丸”（请参见图3.1），而不是传统的选项卡。 例如：

```
## Quarterly Results {.tabset .tabset-fade .tabset-pills}
```

###  r code 4

执行内联R表达式，请将光标放在块中，然后按Ctrl + Enter（macOS：Cmd + Enter）。与执行普通块一样，表达式的内容将发送到R控制台进行评估

将R脚本中的代码重用为笔记本块。 这可以通过在笔记本设置块中使用knitr :: read_chunk（）以及要从中读取代码的R文件中的特殊## —-块名注释来完成

###  Appearance and style

有几个选项可控制HTML文档的外观：

Bootswatch主题库: [https://bootswatch.com/3/](https://bootswatch.com/3/)

* theme指定用于页面的Bootstrap主题（这些主题来自Bootswatch主题库）。 有效主题包括默认，天青石，日记本，扁平，暗淡，可读，太空实验室，联合，宇宙，内腔，纸张，砂岩，单纯形和雪人。 如果没有主题，则传递null（在这种情况下，您可以使用css参数添加自己的样式）。
* highlight指定语法突出显示样式。受支持的样式包括默认，探戈，小品，凯特，单色，浓咖啡，zenburn，黑线鳕，微风和文本伴侣。传递null以防止语法突出显示。
* smart指示是否产生印刷正确的输出，将直引号转换为弯引号，—转换为破折号，-转换为破折号，以及…转换为椭圆。请注意，默认情况下启用智能。

###  Figure options

```
---
title: "Habits"
output:
  html_document:
    fig_width: 7
    fig_height: 6
    fig_caption: true
---

## fig_width和fig_height可用于控制默认图形的宽度和高度（默认使用7x5）。
## fig_retina指定要对视网膜显示执行的缩放比例（默认为2，当前适用于所有广泛使用的视网膜显示）。 设置为null可以防止视网膜缩放
## dev控制用于渲染图形的图形设备（默认为png）dev controls the graphics device used to render figures (defaults to png).
```

###  Advanced customization

```
---
title: "Habits"
output:
  html_document:
    code_folding: hide
  ## code_folding：hide选项可以包含R代码，但默认情况下将其隐藏。 然后，用户可以选择单独显示还是在文档范围内显示隐藏的R代码块。
---

---
title: "Habits"
output:
  html_document:
    keep_md: true
## 当knitr处理R Markdown输入文件时，它将创建Markdown（* .md）文件，该文件随后由Pandoc转换为HTML。 如果要在渲染后保留Markdown文件的副本，可以使用keep_md选项
---

 
---
title: "Habits"
output:
  html_document:
    includes:
      in_header: header.html
      before_body: doc_prefix.html
      after_body: doc_suffix.html
## 通过包含其他HTML内容或完全替换核心的Pandoc模板来对输出进行更高级的自定义
---

---
title: "Habits"
output:
  html_document:
    template: quarterly_report.html
## 使用template选项替换基础的Pandoc模板
---


---
output: html_fragment
## 如果要创建HTML片段而不是完整的HTML文档，则可以使用html_fragment格式。HTML片段不是完整的HTML文档。 它们不包含HTML文档所包含的标准标题内容（它们仅包含普通HTML文档的<body>标记中的内容）。 它们旨在包含在其他网页或内容管理系统（例如博客）中。 因此，它们不支持主题或代码突出显示等功能
---
```

###  html_vignette

html_vignette格式提供了html_document的轻量级替代，适合包含在要发布到CRAN的软件包中。它将基本小插图的大小从600Kb减小到大约10Kb。格式与常规HTML文档的区别如下：

从不使用视网膜图形 具有较小的默认图形尺寸 使用自定义的轻量级CSS样式表 要使用html_vignette，请将其指定为输出格式，然后通过 Vignette \* {}宏添加一些其他与小插图相关的设置：

```
---
title: "Your Vignette Title"
output: rmarkdown::html_vignette
vignette: >
  %\VignetteEngine{knitr::rmarkdown}
  %\VignetteIndexEntry{Your Vignette Title}
  %\VignetteEncoding{UTF-8}
---
```

##
