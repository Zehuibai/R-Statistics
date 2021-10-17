# Basic Latex

## 初始设置 <a href="chu-shi-she-zhi" id="chu-shi-she-zhi"></a>

‌

### 源代码结构 <a href="yuan-dai-ma-jie-gou" id="yuan-dai-ma-jie-gou"></a>

‌

* \documentclass 命令作为开头: \documentclass\[⟨options⟩]{⟨class-name⟩}
* 导言区: 使用 \usepackage 命令调用宏包，还会进行文档的全局设置
* \begin{document} % 正文内容 \end{document}

| 文件   | ​Title                    |
| ---- | ------------------------- |
| .sty | 宏包文件。宏包的名称与文件名一致。         |
| .cls | 文档类文件。文档类名称与文件名一致。        |
| .bib | BIBTEX 参考文献数据库文件。         |
| .bst | BIBTEX 用到的参考文献格式模板。       |
| .aux | 主辅助文件，记录交叉引用、目录、参考文献的引用等。 |
| .toc | 目录记录文件。                   |
| .lof | 图片目录记录文件。                 |
| .lot | 表格目录记录文件。                 |

‌

### \documentclass <a href="documentclass" id="documentclass"></a>

‌

⟨class-name⟩ 为文档类的名称，如 LATEX 提供的 article, book, report，在其基础上派 生的一些文档类如支持中文排版的 ctexart / ctexbook / ctexrep，或者有其它功能的一些文档类， 如 moderncv / beamer 等。‌

可选参数 ⟨options⟩ 为文档类指定选项，以全局地规定一些排版的参数，如字号、纸张大小、 单双面等等。比如调用 article 文档类排版文章，指定纸张为 A4 大小，基本字号为 11pt，双面 排版:

```
\documentclass[11pt,twoside,a4paper]{article}
```

‌

LATEX 的三个标准文档类(article, book, report)可指定的选项包括:

| ​Title    | ​Title                    | ​Title                                                                                          |
| --------- | ------------------------- | ----------------------------------------------------------------------------------------------- |
| 字号        | 10pt, 11pt, 12pt          | 指定文档的基本字号。默认为 10pt                                                                              |
| 纸张        | a4paper, letterpaper, ... | 指定纸张大小，默认为美式信纸 letterpaper (8.5 × 11 英寸)。 可指定选项还包括 a5paper，b5paper，executivepaper 和 legalpaper。 |
| 排版        | twoside, oneside          | 指定单面/双面排版                                                                                       |
| 排栏        | onecolumn, twocolumn      | 指定单栏/双栏排版。默认为 onecolumn                                                                         |
| chapter   | openright, openany        | 指定新的一章 \chapter 是在奇数页(右侧)开始，还是直接紧跟着上一页开始。report 默认为 openany，book 默认为 openright。对 article 无效。    |
| landscape | landscape                 | 指定横向排版。默认为纵向。                                                                                   |
| 标题        | titlepage, notitlepage    | 指定标题命令 \maketitle 是否生成单独的标题页                                                                    |
| 公式        | fleqn                     | 令行间公式左对齐。默认为居中对齐                                                                                |
| 编号        | leqno                     | leqno 将公式编号放在左边。默认为右边。                                                                          |
| 终稿        | draft, final              | 指定草稿/终稿模式。草稿模式下，断行不良的地方会在行尾添加一个黑色方 块。默认为 final。                                                 |

‌

### 宏包 <a href="hong-bao" id="hong-bao"></a>

‌

宏包（package），特指安装后提供 `.sty` 文件的、能以 `\usepackage{...}` 方式调用的宏包。这类宏包，旨在扩展或提供 LaTeX 的某一特定功能，以便利用户使用。调用时，`\usepackage{amsmath}` 相当于插入了文件 `amsmath.sty`，此时，宏包名就是文件名。‌

\usepackage 可以一次性调用多个宏包，在 ⟨package-name⟩ 中用逗号隔开。这种用法一般不要指定选项

```
\usepackage[⟨options⟩]{⟨package-name⟩}​% 一次性调用三个排版表格常用的宏包\usepackage{tabularx, makecell, multirow}
```

‌

#### 常用的一些宏包 <a href="chang-yong-de-yi-xie-hong-bao" id="chang-yong-de-yi-xie-hong-bao"></a>

‌

​[http://www.ctex.org/documents/packages/](http://www.ctex.org/documents/packages/)​

| ​Title                  | ​Title                                            |
| ----------------------- | ------------------------------------------------- |
| **几乎必须**                | ​Content                                          |
| amsmath                 | AMS 数学公式扩展                                        |
| amssymb                 | 更多公式符号                                            |
| graphicx                | 插图                                                |
| amsfonts                | AMS 扩展符号的基础字体支持                                   |
| xcolor                  | 颜色                                                |
| array                   | 表格                                                |
| ctex, xecjk             | 中文                                                |
| fontspec                | 西文 otf 字体                                         |
| siunitx                 | 数字、单位与量                                           |
| ​Content                | ​Content                                          |
| **样式定制**                | ​Content                                          |
| geometry, typearea      | 页面布局                                              |
| fancyhdr                | 页眉页脚                                              |
| footmisc                | 脚注                                                |
| endnote                 | 排版尾注                                              |
| ctexheading, titlesec   | 章节标题                                              |
| tocloft, titletoc       | 目录                                                |
| minitoc                 | 为章节生成独立的小目录                                       |
| multicol                | 多栏模式                                              |
| cite, natbib            | 参考文献引用格式                                          |
| ulem, soul              | 文本强调（下划线、文字底纹等, 均不支持中文）                           |
| glossaries              | 生成词汇表                                             |
| algorithm2e             | 排版算法和伪代码                                          |
| makeindex               | 索引                                                |
| ragged2e                | 段落对齐方式                                            |
| beamer                  | 幻灯片，学术报告                                          |
| hyperref                | 超链接                                               |
| pdfcomment              | 注释                                                |
| embedall, embedfile     | 附件                                                |
| bookmark                | 书签                                                |
| ​Content                | ​Content                                          |
| **表格**                  | ​Content                                          |
| makecell                | 在单元格中自由换行                                         |
| multirow                | 纵向合并单元格                                           |
| tabularx                | 均分列宽                                              |
| booktabs                | 三线表                                               |
| colortbl                |  彩色的单元格和表线 （常通过`\usepackage[table]{xcolor}` 间接调用） |
| longtable, supertabular | 可跨页表格                                             |

‌

### \include{⟨filename⟩} <a href="include-filename" id="include-filename"></a>

‌

当编写长篇文档时，例如当编写书籍、毕业论文时，单个源文件会使修改、校对变得十分困 难。将源文件分割成若干个文件，例如将每章内容单独写在一个文件中，会大大简化修改和校对 的工作。如果和要编译的主文件不在一个目录中，则要加上相对或绝对路径，例如：

```
% 相对路径\include{chapters/a.tex}  ​% *nix （包含 Linux、macOS（OS X））绝对路径\include{/home/Bob/file.tex} ​% Windows 绝对路径，用正斜线\include{D:/file.tex}  
```

‌

值得注意的是 \include 在读入 ⟨filename⟩ 之前会另起一页。有的时候我们并不需要这样， 而是用 \input 命令，它纯粹是把文件里的内容插入：(使用 \include 和 \input 命令载入的文件名一定不要加空格，否则通常 会出错)

```
 \input{⟨filename⟩}
```

‌

另外 LATEX 提供了一个 \includeonly 命令来组织文件，用于导言区，指定只载入某些文 件： 导言区使用了 \includeonly 后，正文中不在其列表范围的 \include 命令不会起效。

```
\includeonly{⟨filename1⟩,⟨filename2⟩,…} 
```

‌

实用的工具宏包 syntonly。加载这个宏包后，在导言区使用 \syntaxonly 命 令，可令 LATEX 编译后不生成 DVI 或者 PDF 文档，只排查错误，编译速度会快不少：

```
\usepackage{syntonly} % 想生成文档，则用 % 注释掉 \syntaxonly 命令即可% \syntaxonly  
```

​

​

​‌

## 排版文字 <a href="pai-ban-wen-zi" id="pai-ban-wen-zi"></a>

‌

### 语言空格 <a href="yu-yan-kong-ge" id="yu-yan-kong-ge"></a>

| ​Title   | ​Title                                                                                                            |
| -------- | ----------------------------------------------------------------------------------------------------------------- |
| ASCII 编码 | 基础                                                                                                                |
| UTF-8 编码 | Unicode 是一个多国字符的集合，覆盖了几乎全球范围内的语言文字。一个字符由一个到四个字节编码，其中单字节字符的编码与 ASCII 编码兼容。（为默认编码）                                  |
| 中文       | \documentclass{ctexart} \begin{document} 在\LaTeX{}中排版中文。 \end{document}                                           |
| ​Content | ​Content                                                                                                          |
| 文字中空格    | <ul><li>空格键和 Tab 键输入的空白字符视为“空格”。</li><li>连续的若干个空白字符视 为一个空格。</li><li> 一行开头的空格忽略不计。</li><li>行末的换行符视为一个空格。</li></ul> |
| 文字中空行    | <ul><li>连续两个换行符</li><li>多个空行被 视为一个空行。</li><li>行末使用 \par 命令分段。</li></ul>                                           |
| 断行       | <ul><li> \\[⟨length⟩], 可选参数 ⟨length⟩，用于在断行处向下增加垂直间距, 表格、公式等地方用于换行</li><li>\newline, 不带可选参数, 只用于文本段落中</li></ul>    |
| 断页       | <ul><li>\newpage: 在双栏排版模式中 \newpage 起到另起一栏的作用，\clearpage 则能够另起一页</li><li>\clearpage</li></ul>                     |

‌

#### 数学公式中的空格 <a href="shu-xue-gong-shi-zhong-de-kong-ge" id="shu-xue-gong-shi-zhong-de-kong-ge"></a>

| 宽度       | ​Title     | ​Title                                                                                        | ​Title     |
| -------- | ---------- | --------------------------------------------------------------------------------------------- | ---------- |
| 两个quad空格 | a \qquad b | ​​![a \qquad b](http://upload.wikimedia.org/math/e/5/0/e505263bc9c94f673c580f3a36a7f08a.png)​ | 两个_m_的     |
| quad空格   | a \quad b  | ​​![a \quad b](http://upload.wikimedia.org/math/d/a/8/da8c1d9effa4501fd80c054e59ad917d.png)​  | 一个_m_的宽度   |
| 大空格      | a\ b       | ​​![a\ b](http://upload.wikimedia.org/math/6/9/2/692d4bffca8e84ffb45cf9d5facf31d6.png)​       | 1/3_m_宽度   |
| 中等空格     | a\\;b      | ​​![a\\;b](http://upload.wikimedia.org/math/b/5/a/b5ade5d5393fd7727bf77fa44ec8b564.png)​      | 2/7_m_宽度   |
| 小空格      | a\\,b      | ​​![a\\,b](http://upload.wikimedia.org/math/7/b/e/7bea99aed60ba5e1fe8a134ab43fa85f.png)​      | 1/6_m_宽度   |
| 没有空格     | ab         | ​​![ab\\,](http://upload.wikimedia.org/math/b/6/b/b6bd9dba2ebfca24731ae6dc3913e625.png)​      | ​Content   |
| 紧贴       | a\\!b      | ​​![a\\!b](http://upload.wikimedia.org/math/0/f/b/0fbcad5fadb912e8afa6d113a75c83e4.png)​      | 缩进1/6_m_宽度 |

​‌

### 章节目录 <a href="zhang-jie-mu-lu" id="zhang-jie-mu-lu"></a>

| ​Title         | ​Title                              |
| -------------- | ----------------------------------- |
| \part          |  部，深度：-1，不能用在`letter`；              |
| \chapter       |  章，深度：0，可以用在`book`和`report`；        |
| \section       |  节，深度：1，不能用在`letter`;               |
| \subsection    |  小节，深度：2， 不能用在`letter`;             |
| \subsubsection |  小小节，深度：3，不能用在`letter`;             |
| \paragraph     |  带标题的段落（用`{ }`），深度：5，不能用在`letter`;  |
| \subparagraph  |  带标题的次级段落（用`{ }`），深度：6，不能用在`letter` |

‌

#### 目录 <a href="mu-lu" id="mu-lu"></a>

‌

生成单独的一章（book / report）或一节（article），标题默认为 “Contents”

```
\tableofcontents
```

‌

有时我们使用了 \chapter _或 \section_ 这样不生成目录项的章节标题命令，而又想手动 生成该章节的目录项，可以在标题命令后面使用：其中 ⟨level⟩ 为章节层次 chapter 或 section 等，⟨title⟩ 为出现于目录项的章节标题

```
\addcontentsline{toc}{⟨level⟩}{⟨title⟩}
```

‌

#### 编号 <a href="bian-hao" id="bian-hao"></a>

‌

当设定标题（用`{}`包含的）时候，在插入目录时会自动编入，但是如果太长或有格式异常，可以通过设置可选标题（用`[]`来包含），在编入目录时如果有可选标题会自动选用，参考下例：`\section[编入目录的标题]{实际的太长的标题}`深度5、6的不会计入目录（`contents`）。‌

章节标题会自动编号，缺省设置如下：‌

* 对于`part`：采用罗马数字编号，如：`Part I, Part II, ...`；
* 对于`chapter`和`section`：采用 阿拉伯数字，如：`Chapter 2`，`2.1 xxx`等；
* 对于附录`Appendix`：采用拉丁字母，如：`Appendix A, Appendix B, ...`等；

‌

设置参数来设定哪一级标题使用编号，以及目录采用哪一级的标题‌

`\setcounter{secnumdepth}{1} \setcounter{tocdepth}{1}`‌

若某级标题**不需要进行编号**（也就意味不会加入目录），可以加上`*`，如下例：`\sebsection*{本小节不进行编号}`‌

#### 缩进与间距 <a href="suo-jin-yu-jian-ju" id="suo-jin-yu-jian-ju"></a>

‌

正常的段落`paragraphs`，输入时，段落与段落之间用空行来分割，输出时段落之间的宽度是缺省的，这个尺寸可以用下面的命令来设置，注意要在`preamble`也就是`\begin{document}`命令之前：

```
\setlength{\parskip}{1cm}% 设置每段第一行的缩进\setlength{\parindent}{4em}% 没有缩进\setlength{\parindent}{0pt}​​% 在每一个段落前用单独设置这一段第一行缩进\indent​% 设置段落每行之间的间距% 设置为1.5倍行距\renewcommand{\baselinestretch}{1.5}
```

‌

#### 标签 <a href="biao-qian" id="biao-qian"></a>

‌

 `\label`创建一个标签，可以在后面用`\pageref` 来交叉引用得到这个标签所在的页码，用`\ref`来交叉引用得到这个标签所在的章、节编号，后面还可以看到，这个技术还可以用到公式里面。‌

#### 每页的页眉页脚 <a href="mei-ye-de-ye-mei-ye-jiao" id="mei-ye-de-ye-mei-ye-jiao"></a>

‌

* `\pagestyle{style}`设置每页的页眉（`\header`）和页脚(`\footer`)的设置，`style`的可选项是：
* `\plain`：缺省，页码打印在页脚中间；
* `\headings`：页码，每章标题打印在页眉，页脚留空；
* `\empty`：设置页眉页脚为空。
* 对特定页可以用：`\ thispagestyle{style}`可以设置 独立的页码页眉设置。

‌

#### 正文和附录 <a href="zheng-wen-he-fu-lu" id="zheng-wen-he-fu-lu"></a>

```
\appendix
```

‌

所有标准文档类都提供了一个 \appendix 命令将正文和附录分开，使用 \appendix 后，最 高一级章节改为使用拉丁字母编号，从 A 开始。‌

book 文档类还提供了前言、正文、后记结构的划分命令：‌

* \frontmatter 前言部分，页码为小写罗马字母格式；其后的 \chapter 不编号。
* \mainmatter 正文部分，页码为阿拉伯数字格式，从 1 开始计数；其后的章节编号正常。
* \backmatter 后记部分，页码格式不变，继续正常计数；其后的 \chapter 不编号。

‌

#### 交叉引用 <a href="jiao-cha-yin-yong" id="jiao-cha-yin-yong"></a>

‌

在能够被交叉引用的地方，如章节、公 式、图表、定理等位置使用 \label 命令：之后可以在别处使用 \ref 或 \pageref 命令，分别生成交叉引用的编号和页码：

```
\label{⟨label-name⟩}\ref{⟨label-name⟩} \pageref{⟨label-name⟩}
```

‌

\label 命令可用于记录各种类型的交叉引用，使用位置分别为：‌

* **章节标题** 在章节标题命令 \section 等之后紧接着使用。
* **行间公式** 单行公式在公式内任意位置使用；多行公式在每一行公式的任意位置使用。
* **有序列表** 在 enumerate 环境的每个 \item 命令之后、下一个 \item 命令之前任意位置使用。
* **图表标题** 在图表标题命令 \caption 之后紧接着使用。
* **定理环境** 在定理环境内部任意位置使用。

‌

### 标题脚注 <a href="biao-ti-jiao-zhu" id="biao-ti-jiao-zhu"></a>

```
\documentclass{article}  % article 文档​% 文章标题\title{Title\thanks{No procrastination}}           ​\subject{a funny paper}\author{Author name1}    % 作者的名称\and Author name2 \thanks{I am no longer a member of this department}\date{\today}            % 日期\subtitle{A closer look at the expenses}​% 设置页面的环境,a4纸张大小，左右上下边距信息\usepackage[a4paper,left=10mm,right=10mm,top=15mm,bottom=15mm]{geometry}  ​\begin{document}\maketitle               % 添加这一句才能够显示标题等信息\newpage                 % 插入新页​% 摘要开始部分\begin{abstract}This part of the content is for placing summary information.\end{abstract}​\newpage                  % 不希望摘要与正文在一页的话​% 或者插入正文部分% \include{chapter1} % 第一章 chapter1.tex% \include{chapter2} % 第二章 chapter2.tex​% 文档内容% 下面是章节1\section{Chapter 1}say some thing​% 下面是章节2\section{Chapter 2}% 分节\subsection{Definition of 1}% 定义标签\label{sec11}describe some thing​\paragraph{paragraph 1}​\subsection{Expression and description of algorithm}\label{sec12}Describe what methods are available for algorithm representation. ​...\appendix % 附录% \include{appendixA} % 附录 A appendixA.tex...​\end{document}
```

‌

#### 脚注 <a href="jiao-zhu" id="jiao-zhu"></a>

‌

* 使用 \footnote 命令可以在页面底部生成一个脚注： \footnote{⟨footnote⟩}
* 有些情况下（比如在表格环境、各种盒子内）使用 \footnote 并不能正确生成脚注。我们 可以分两步进行，先使用 \footnotemark 为脚注计数，再在合适的位置用 \footnotetext 生成 脚注。

```
“天地玄黄，宇宙洪荒。日月盈昃，辰宿列张。”\footnote{出自《千字文》。}​\begin{tabular}{l}\hline“天地玄黄，宇宙洪荒。日月盈昃，辰宿列张。”\footnotemark \\\hline\end{tabular}\footnotetext{表格里的名句出自《千字文》。}
```

​‌

## 特殊环境 <a href="te-shu-huan-jing" id="te-shu-huan-jing"></a>

‌

### 列表 <a href="lie-biao" id="lie-biao"></a>

‌

有序和无序列表环境 enumerate 和 itemize，两者的用法很类似，都 用 \item 标明每个列表项。enumerate 环境会自动对列表项编号。

```
\begin{enumerate}    \item An item.        \begin{enumerate}        \item A nested item.\label{itref}        \item[*] A starred item.        \end{enumerate}    \item Reference(\ref{itref}).\end{enumerate}
```

​![](https://gblobscdn.gitbook.com/assets%2F-MRv4EVhMGO\_-FMcaXSl%2F-MT1inBVlWtM30UNdiyf%2F-MT1j4SyPjPCzHU\_9\_62%2Fimage.png?alt=media\&token=76ed02f3-779d-4a5e-8c7a-8b43c1a1e324)‌

关键字环境 description 的用法与以上两者类似，不同的是 \item 后的可选参数用来写关 键字，以粗体显示

```
\begin{description}\item[Enumerate] Numbered list.\item[Itemize] Non-numbered list.\end{description}
```

‌

### 对齐 <a href="dui-qi" id="dui-qi"></a>

‌

center、flushleft 和 flushright 环境分别用于生成居中、左对齐和右对齐的文本环境

```
\begin{center} … \end{center}\begin{flushleft} … \end{flushleft}\begin{flushright} … \end{flushright}​\begin{center}Centered text using a\verb|center| environment.\end{center}​\begin{flushleft}Left-aligned text using a\verb|flushleft| environment.\end{flushleft}​\begin{flushright}Right-aligned text using a\verb|flushright| environment.\end{flushright}
```

‌

还可以用以下命令直接改变文字的对齐方式： \centering \raggedright \raggedleft

```
\centeringCentered text paragraph.​\raggedrightLeft-aligned text paragraph.​\raggedleftRight-aligned text paragraph.
```

‌

### 引用 <a href="yin-yong" id="yin-yong"></a>

‌

verse 用于排版诗歌，与 quotation 恰好相反，verse 是首行悬挂缩进的。

```
Francis Bacon says:\begin{quote}Knowledge is power.\end{quote}
```

‌

### 代码 <a href="dai-ma" id="dai-ma"></a>

```
\begin{verbatim}#include <iostream>int main(){std::cout << "Hello, world!"<< std::endl;return 0;}\end{verbatim}
```

‌

### 表格 <a href="biao-ge" id="biao-ge"></a>

‌

​[https://www.tablesgenerator.com/markdown_tables](https://www.tablesgenerator.com/markdown_tables)​

```
\begin{tabular}[⟨align⟩]{⟨column-spec⟩}⟨item1⟩ & ⟨item2⟩ & … \\\hline⟨item1⟩ & ⟨item2⟩ & … \\\end{tabular}
```

| ​Title          | ​Title                                                                                                                                                        |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ⟨column-spec⟩   | <p>指定表格的列数以及每列的格式。</p><ul><li>l/c/r 单元格内容左对齐/居中/右对齐，不折行</li><li>p{⟨width⟩} 单元格宽度固定为 ⟨width⟩，可自动折行</li><li>| 绘制竖线</li><li>@{⟨string⟩} 自定义内容 ⟨string⟩</li></ul> |
| \hline          | 绘制表格线                                                                                                                                                         |
| \cline{⟨i⟩-⟨j⟩} | 绘制跨越部分单元格的横线                                                                                                                                                  |
| 三线表             | booktabs 宏包 支持 提供了 \toprule、\midrule 和 \bottomrule 命令                                                                                                         |
| \multicolumn    | 横向合并单元格                                                                                                                                                       |
| \multirow       | 纵向合并单元格, multirow 宏包提供的 \multirow 命令                                                                                                                          |

‌

###  图片 <a href="tu-pian" id="tu-pian"></a>

‌

LATEX 本身不支持插图功能，需要由 graphicx 宏包辅助支持. 加载图片命令. 其中 ⟨filename⟩ 为图片文件名，与 \include 命令的用法类似，文件名可能需要用相对路径 或绝对路径表示

```
\includegraphics[⟨options⟩]{⟨filename⟩}
```

‌

: \includegraphics 命令的可选参数

| height      | 图形的高度（可为任何 TEX 度量单位）。                                                                                |
| ----------- | ---------------------------------------------------------------------------------------------------- |
| totalheight | 图形的全部高度，可为任何 TEX 度量单位（ _6/95 增加_）。                                                                   |
| width       | 图形的宽度（可为任何 TEX 度量单位）。                                                                                |
| scale       | 图形的缩放因子，设定 scale=2 会使 插入的图形的大小为其自然大小的两倍。                                                             |
| angle       | 设定旋转的角度，以度为单位，顺时钟方向为正。                                                                               |
| origin      | origin 指定图形绕那一点旋转，缺省 是图形的参考点（_12/95 增加_）。初始点有可能与 第 8.3节的 \rotatebox 命令中的一样。 比如 origin=c 将使图形绕它的中心旋转。 |

‌

### 盒子 <a href="he-zi" id="he-zi"></a>

‌

#### 带框的水平盒子 <a href="dai-kuang-de-shui-ping-he-zi" id="dai-kuang-de-shui-ping-he-zi"></a>

‌

可以通过 \setlength 命令调节边框的宽度 \fboxrule 和内边距 \fboxsep：

```
\fbox{…}\framebox[⟨width⟩][⟨align⟩]{…}​\fbox{Test some words.}\\\framebox[10em][r]{Test some words.}​% 调节边框的宽度 \fboxrule 和内边距 \fboxsep：\setlength{\fboxrule}{1.6pt}\setlength{\fboxsep}{1em}\framebox[10em][r]{Test box}
```

‌

#### 垂直盒子 <a href="chui-zhi-he-zi" id="chui-zhi-he-zi"></a>

‌

文字可以换行的盒子

```
\fbox{\begin{minipage}{15em}%这是一个垂直盒子的测试。\footnote{脚注来自 minipage。}\end{minipage}}
```

‌

## 浮动体 <a href="fu-dong-ti" id="fu-dong-ti"></a>

‌

内容的尺寸往往太大，导致 分页困难。LATEX 为此引入了浮动体的机制，令大块的内容可以脱离上下文，放置在合适的位置。 LATEX 预定义了两类浮动体环境 figure 和 table。习惯上 figure 里放图片，table 里放 表格，但并没有严格限制，可以在任何一个浮动体里放置文字、公式、表格、图片等等任意内容。以 table 环境的用法举例，figure 同理：‌

### Normal Figure <a href="normal-figure" id="normal-figure"></a>

```
% 对于图片--figure环境\begin{center}    \includegraphics[width=4in]{Fotos/Organigramm}    \begin{figure}[!h]        \caption{Here the caption.}    \end{figure}\end{center}​% 对于表格--table环境\begin{table}[允许位置]  \begin{tabular}{...}  ... table data ...  \end{tabular}\end{table}\end{table}
```

‌

#### 允许位置的参数 <a href="yun-xu-wei-zhi-de-can-shu" id="yun-xu-wei-zhi-de-can-shu"></a>

| Specifier | Permission                                                                                                                                                          |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `h`       | <p>Place the float here, i.e., approximately at the same point it occurs in the source text (however, not exactly at the spot)</p><p>将浮动内容放置在此处，即大约与源文本中出现的位置相同</p> |
| `t`       | <p>Position at the top of the page.</p><p>放置在页面顶部。</p>                                                                                                              |
| `b`       | <p>Position at the bottom of the page.</p><p>定位在页面底部。</p>                                                                                                           |
| `p`       | <p>Put on a special page for floats only.</p><p>放在特殊页面上仅用于浮点数。</p>                                                                                                  |
| `!`       | Override internal parameters LaTeX uses for determining "good" float positions.                                                                                     |

‌

#### 其他命令 <a href="3-qi-ta-ming-ling" id="3-qi-ta-ming-ling"></a>

> 1. \centering：居中排版
> 2. \caption{}：标题
> 3. \label{fig:img1}：标签

‌

### Caption Position <a href="caption-position" id="caption-position"></a>

```
% 标题在图像的上方\begin{figure}  \caption{A picture of a gull.}  \centering    \includegraphics[width=0.5\textwidth]{gull}\end{figure}​% 标题在图像的下方\begin{figure}  \centering    \reflectbox{%      \includegraphics[width=0.5\textwidth]{gull}}  \caption{A picture of the same gull           looking the other way!}\end{figure}
```

​![](https://upload.wikimedia.org/wikipedia/commons/d/de/Latex_caption_example.png)‌

### Side captions <a href="side-captions" id="side-captions"></a>

```
\usepackage{graphicx}\usepackage{sidecap}​\begin{document}​\begin{SCfigure}  \centering  \caption{ ... caption text ... }  \includegraphics[width=0.3\textwidth]%    {Giraffe_picture}% picture filename\end{SCfigure}​\end{document}
```

‌

### List of Table and Figure <a href="list-of-table-and-figure" id="list-of-table-and-figure"></a>

```
\documentclass[12pt,]{article}\usepackage{graphicx}\newcommand{\species}[1]{\textit{#1} sp.}\begin{document}​\listoffigures\section{Introduction}
```

‌

### Wrapping text around figures <a href="wrapping-text-around-figures" id="wrapping-text-around-figures"></a>

```
\begin{wrapfigure}[lineheight]{position}[overhang]{width}
```

| ​Title | ​Title | ​Title                                               |
| ------ | ------ | ---------------------------------------------------- |
| r      | R      | right side of the text                               |
| l      | L      | left side of the text                                |
| i      | I      | inside edge–near the binding (in a twoside document) |
| o      | O      | outside edge–far from the binding                    |

```
usepackage{wrapfig}​\begin{wrapfigure}{r}{0.5\textwidth}  \begin{center}    \includegraphics[width=0.48\textwidth]{gull}  \end{center}  \caption{A gull}\end{wrapfigure}
```

‌

#### Reduce white space for figures <a href="reduce-white-space-for-figures" id="reduce-white-space-for-figures"></a>

```
\begin{wrapfigure}{r}{0.5\textwidth}  \vspace{-20pt}  \begin{center}    \includegraphics[width=0.48\textwidth]{gull}  \end{center}  \vspace{-20pt}  \caption{A gull}  \vspace{-10pt}\end{wrapfigure}
```

​![](https://upload.wikimedia.org/wikipedia/commons/d/dc/Latex_example_wrapfig_vspace.png)‌

### 并排子图 <a href="bing-pai-zi-tu" id="bing-pai-zi-tu"></a>

```
\usepackage{graphicx}\usepackage{subcaption}​\begin{figure}    \centering    \begin{subfigure}[b]{0.3\textwidth}        \includegraphics[width=\textwidth]{gull}        \caption{A gull}        \label{fig:gull}    \end{subfigure}    ~ %add desired spacing between images, e. g. ~, \quad, \qquad, \hfill etc.       %(or a blank line to force the subfigure onto a new line)    \begin{subfigure}[b]{0.3\textwidth}        \includegraphics[width=\textwidth]{tiger}        \caption{A tiger}        \label{fig:tiger}    \end{subfigure}    ~ %add desired spacing between images, e. g. ~, \quad, \qquad, \hfill etc.     %(or a blank line to force the subfigure onto a new line)    \begin{subfigure}[b]{0.3\textwidth}        \includegraphics[width=\textwidth]{mouse}        \caption{A mouse}        \label{fig:mouse}    \end{subfigure}    \caption{Pictures of animals}\label{fig:animals}\end{figure}
```

​![](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e5/Latex_example_subfig.png/500px-Latex_example_subfig.png)‌

You may also use a table environment for subtables. For each subfloat, you need to use:

```
\begin{table}[<placement specifier>]    \begin{subtable}[<placement specifier>]{<width>}        \centering        ... table 1 ...    \caption{<sub caption>}    \end{subtable}    ~    \begin{subtable}[<placement specifier>]{<width>}        \centering        ... table 2 ...        \caption{<sub caption>}    \end{subtable}\end{table}
```

​

​

​

​‌

​[https://ctan.mc1.root.project-creative.net/info/lshort/chinese/lshort-zh-cn.pdf](https://ctan.mc1.root.project-creative.net/info/lshort/chinese/lshort-zh-cn.pdf)​

​

​

​

​

```
% Options for packages loaded elsewhere\PassOptionsToPackage{unicode}{hyperref}\PassOptionsToPackage{hyphens}{url}\PassOptionsToPackage{dvipsnames,svgnames*,x11names*}{xcolor}%\documentclass[  10pt,  ngerman,]{article}\usepackage{lmodern}\usepackage{amssymb,amsmath}\usepackage{ifxetex,ifluatex}\ifnum 0\ifxetex 1\fi\ifluatex 1\fi=0 % if pdftex  \usepackage[T1]{fontenc}  \usepackage[utf8]{inputenc}  \usepackage{textcomp} % provide euro and other symbols\else % if luatex or xetex  \usepackage{unicode-math}  \defaultfontfeatures{Scale=MatchLowercase}  \defaultfontfeatures[\rmfamily]{Ligatures=TeX,Scale=1}\fi% Use upquote if available, for straight quotes in verbatim environments\IfFileExists{upquote.sty}{\usepackage{upquote}}{}\IfFileExists{microtype.sty}{% use microtype if available  \usepackage[]{microtype}  \UseMicrotypeSet[protrusion]{basicmath} % disable protrusion for tt fonts}{}\makeatletter\@ifundefined{KOMAClassName}{% if non-KOMA class  \IfFileExists{parskip.sty}{%    \usepackage{parskip}  }{% else    \setlength{\parindent}{0pt}    \setlength{\parskip}{6pt plus 2pt minus 1pt}}}{% if KOMA class  \KOMAoptions{parskip=half}}\makeatother\usepackage{xcolor}\IfFileExists{xurl.sty}{\usepackage{xurl}}{} % add URL line breaks if available\IfFileExists{bookmark.sty}{\usepackage{bookmark}}{\usepackage{hyperref}}\hypersetup{  pdflang={de-De},  colorlinks=true,  linkcolor=Maroon,  filecolor=Maroon,  citecolor=Blue,  urlcolor=Blue,  pdfcreator={LaTeX via pandoc}}\urlstyle{same} % disable monospaced font for URLs\usepackage[margin=1in]{geometry}\usepackage{longtable,booktabs}% Correct order of tables after \paragraph or \subparagraph\usepackage{etoolbox}\makeatletter\patchcmd\longtable{\par}{\if@noskipsec\mbox{}\fi\par}{}{}\makeatother% Allow footnotes in longtable head/foot\IfFileExists{footnotehyper.sty}{\usepackage{footnotehyper}}{\usepackage{footnote}}\makesavenoteenv{longtable}\usepackage{graphicx,grffile}\makeatletter\def\maxwidth{\ifdim\Gin@nat@width>\linewidth\linewidth\else\Gin@nat@width\fi}\def\maxheight{\ifdim\Gin@nat@height>\textheight\textheight\else\Gin@nat@height\fi}\makeatother% Scale images if necessary, so that they will not overflow the page% margins by default, and it is still possible to overwrite the defaults% using explicit options in \includegraphics[width, height, ...]{}\setkeys{Gin}{width=\maxwidth,height=\maxheight,keepaspectratio}% Set default figure placement to htbp\makeatletter\def\fps@figure{htbp}\makeatother\setlength{\emergencystretch}{3em} % prevent overfull lines\providecommand{\tightlist}{%  \setlength{\itemsep}{0pt}\setlength{\parskip}{0pt}}\setcounter{secnumdepth}{5}\ifxetex  % Load polyglossia as late as possible: uses bidi with RTL langages (e.g. Hebrew, Arabic)  \usepackage{polyglossia}  \setmainlanguage[]{german}\else  \usepackage[shorthands=off,main=ngerman]{babel}\fi​\title{Praktikum im Leibniz-Institut für Präventionsforschung und Epidemiologie}\usepackage{etoolbox}\makeatletter\providecommand{\subtitle}[1]{% add subtitle to \maketitle  \apptocmd{\@title}{\par {\large #1 \par}}{}{}}\makeatother\subtitle{Praktikumsbericht}\author{Zehui Bai\\Medical Biometry/Biostatistics\\Betreut von Dr.~Ronja Foraita\\Matrikelnummer:3183673\\Zeitraum: 01.08.2019 -- 12.09.2019}\date{}​\begin{document}\maketitle​{\hypersetup{linkcolor=}\setcounter{tocdepth}{2}\tableofcontents}\hypertarget{praktikumsstelle}{%\section{Praktikumsstelle}\label{praktikumsstelle}}​\hypertarget{vorstellung-der-praktikumsstelle}{%\subsection{Vorstellung derPraktikumsstelle}\label{vorstellung-der-praktikumsstelle}}​Das Leibniz-Institut für Präventionsforschung und Epidemiologie -- BIPSwurde 1981 als „Bremer Institut für Präventionsforschung undSozialmedizin (BIPS)`` gegründet. Es ist damit eines der ältestenEpidemiologie-Institute Deutschlands.​​\hypertarget{literatur}{%\section{Literatur}\label{literatur}}​{[}1{]} BIPS-Allgemein.(Tag des Zugriffs: 01.10.2019)​\url{https://www.bips-institut.de/das-institut/ueber-das-institut.html}​\end{document}​
```

​
