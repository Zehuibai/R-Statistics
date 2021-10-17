# PDF Templates

## Templates

### Berichtvorlage

````
---
title: |
  | Praktikum im Leibniz-Institut für Präventionsforschung und Epidemiologie
author: |
  | Zehui Bai
  | Medical Biometry/Biostatistics
  | Betreut von Dr. Ronja Foraita
  | Matrikelnummer:3183673
  | Zeitraum: 01.08.2019 -- 12.09.2019
colorlinks: no
output:
  pdf_document:
    fig_caption: yes
    keep_tex: yes
    latex_engine: pdflatex
    toc: yes
    toc_depth: 2
  html_document:
    df_print: paged
    toc: yes
    toc_depth: '2'
editor_options:
  chunk_output_type: console
fontsize: 10pt
geometry: margin=1in
lang: de-De
numbersections: yes
csquotes: yes
subtitle: Praktikumsbericht
boldstyle: TeX
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE, message = FALSE, error = FALSE, warning = FALSE)
library(knitr)
```


# Praktikumsstelle

## Vorstellung der Praktikumsstelle

Das Leibniz-Institut für Präventionsforschung und Epidemiologie – BIPS wurde 1981 als „Bremer Institut für Präventionsforschung und Sozialmedizin (BIPS)“  gegründet. Es ist damit eines der ältesten Epidemiologie-Institute Deutschlands.
````

![](broken-reference)







## PDF document

###  Data frame printing

通过df_print选项增强数据帧的默认显示

| Option  | Beschreibuing                            |
| ------- | ---------------------------------------- |
| default | Call the print.data.frame generic method |
| kable   | Use the knitr::kable() function          |
| tibble  | Use the tibble::print.tbl_df() function  |

```
---
title: "Habits"
output:
  pdf_document:
    df_print: kable
---
```

###  Syntax highlighting

高亮选项指定语法高亮样式。 在pdf_document中的用法与html_document相同

###  LaTeX options

```
---
title: "Crop Analysis Q3 2013"
output: pdf_document
fontsize: 11pt
geometry: margin=1in
---
## lang 语言
## fontsize 字体大小 (e.g., 10pt, 11pt, or 12pt)
## documentclass 文档类 (e.g., article)
## classoption 文档类的选项 (e.g., oneside)
## geometry 几何类别的选项s (e.g., margin=1in)
## mainfont, sansfont, monofont, mathfont 文档字体（仅适用于xelatex和lualatex）
## linkcolor, urlcolor, citecolor: Color for internal, external, and citation links 内部，外部和引用链接的颜色
```

###  citations

```
## LaTeX packages for citations
---
output:
  pdf_document:
    citation_package: natbib
---
## 默认情况下，引用是通过pandoc-citeproc处理的，适用于所有输出格式。对于PDF输出，有时最好使用LaTeX软件包来处理引用，例如natbib或biblatex。要使用这些软件包之一，只需将选项citation_package设置为natbib或biblatex
```

###  Advanced customization

```
LaTeX engine LaTeX引擎
---
title: "Habits"
output:
  pdf_document:
    latex_engine: xelatex
## 默认情况下，PDF文档使用pdflatex呈现。您可以使用latex_engine选项指定备用引擎。可用的引擎为pdflatex，xelatex和lualatex
---
 
 
---
title: "Habits"
output:
  pdf_document:
    keep_tex: true
## Keeping intermediate TeX
---
 
---
title: "Habits"
output:
  pdf_document:
    includes:
      in_header: preamble.tex
      before_body: doc-prefix.tex
      after_body: doc-suffix.tex
## 通过包含其他LaTeX指令和/或内容，或完全替换核心的Pandoc模板来对PDF输出进行更高级的自定义
---
 
---
title: "Habits"
output:
  pdf_document:
    template: quarterly-report.tex ## Custom templates
---
```

研究默认的LaTeX模板作为示例: [https://github.com/jgm/pandoc-templates/blob/master/default.latex](https://github.com/jgm/pandoc-templates/blob/master/default.latex)

##  word_document

```
## Word文档最显着的功能是Word模板，也称为“样式参考文档”。您可以指定一个文档，以在生成* .docx文件（Word文档）时用作样式参考。这将允许您自定义诸如边距和其他格式特征之类的内容。为了获得最佳结果，参考文档应该是使用rmarkdown或Pandoc生成的.docx文件的修改版本。可以将此类文档的路径传递到word_document格式的reference_docx参数。传递“默认”以使用默认样式
## https://rmarkdown.rstudio.com/articles_docx.html
---
title: "Habits"
output:
  word_document:
    reference_docx: my-styles.docx
---
```
