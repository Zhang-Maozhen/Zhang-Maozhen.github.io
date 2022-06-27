---
title: LaTeX安装及语法
subtitle: 这里将会介绍LaTeX的安装、编写语法和数学公式.

# Summary for listings and search engines
summary: LaTeX的安装、编写语法和数学公式.

# Link this post with a project
projects: []

# Date published
date: '2022-06-27T00:00:00Z'

# Date updated
lastmod: '2022-06-27T00:00:00Z'

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/CpkOjOcXdUY)'
  focal_point: ''
  placement: 2
  preview_only: false

authors:
  - admin
#  - 吳恩達

tags:
  - Academic
  - 开源

categories:
   - LaTeX 
#  - Demo
#  - 教程

---

# Latex

## 安装

### windows

安装latex运行环境Texlive

安装latex编辑器：TeXworks(适合上手)、TeXstudio(听说好用)

选择Latex阅读器和编辑器(Latex编译结果是PDF文件)：Acrobat

### mac

[MacTeX](https://tug.org/mactex/)是TeXLive的Mac版 

安装Sublime Text 3，打开Tools，选择Install Package Control。再从Preference选择Package Control，输入Install Package，输入LatexTools，回车安装LatexTools插件

最后安装Skim，安装好后运行Skim，进入`Skim——选项`，点击`同步`进行设置 

下放部分存疑：

> 默认情况下,  是安装在 /usr/local/texlive 路径下. pdflatex 等编译器通常会在类似 /usr/local/texlive/2021/bin/universal-darwin/pdflatex 路径下

## 编写

TeX有多种文档类型可选，笔者较常用的有如下几种类型：

- 对于英文，可以用`book`、`article`和`beamer`；
- 对于中文，可以用`ctexbook`、`ctexart`和`ctexbeamer`，这些类型自带了对中文的支持。

不同的文件类型，编写的过程中也会有一定的差异，如果直接修改文件类型的话，甚至会报错。以下统一选用`ctexart`。在编辑框第一行，输入如下内容来设置文件类型：

```tex
\documentclass{ctexart}
```

文件的正文部分需要放入document环境中，在document环境外的部分不会出现在文件中。

```tex
\documentclass[12pt, a4paper, oneside]{ctexart}

\begin{document}

这里是正文. 

\end{document}
```

作者：Dylaaan
链接：https://zhuanlan.zhihu.com/p/456055339
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



### 宏包

为了完成一些功能（如定理环境），还需要在导言区，也即document环境之前加载[宏包](https://www.zhihu.com/search?q=宏包&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A"456055339"})。加载宏包的代码是`\usepackage{}`。本份教程中，与数学公式与定理环境相关的宏包为`amsmath`、`amsthm`、`amssymb`，用于插入图片的宏包为`graphicx`，代码如下：

```tex
\usepackage{amsmath, amsthm, amssymb, graphicx}
```

另外，在加载宏包时还可以设置基本参数，如使用超链接宏包`hyperref`，可以设置引用的颜色为黑色等，代码如下：

```tex
\usepackage[bookmarks=true, colorlinks, citecolor=blue, linkcolor=black]{hyperref}
```

### 标题

标题可以用`\title{}`设置，作者可以用`\author`设置，日期可以用`\date{}`设置，这些都需要放在导言区。为了在文档中显示标题信息，需要使用`\maketitle`。例如：

```tex
\documentclass[12pt, a4paper, oneside]{ctexart}
\usepackage{amsmath, amsthm, amssymb, graphicx}
\usepackage[bookmarks=true, colorlinks, citecolor=blue, linkcolor=black]{hyperref}

% 导言区

\title{我的第一个\LaTeX 文档}
\author{Dylaaan}
\date{\today}

\begin{document}

\maketitle

这里是正文. 

\end{document}
```

### 正文

正文可以直接在document环境中书写，没有必要加入空格来缩进，因为文档默认会进行首行缩进。相邻的两行在编译时仍然会视为同一段。在LaTeX中，另起一段的方式是使用一行相隔，例如：

```tex
我是第一段. 

我是第二段.
```

这样编译出来就是两个段落。在正文部分，多余的空格、回车等等都会被自动忽略，这保证了全文排版不会突然多出一行或者多出一个空格。另外，另起一页的方式是：

```tex
\newpage
```

笔者在编写文档时，为了保证美观，通常将中文标点符号替换为英文标点符号（需要注意的是英文标点符号后面还有一个空格），这比较适合数学类型的文档。

在正文中，还可以设置局部的特殊字体：

| 字体     | 命令      |
| -------- | --------- |
| 直立     | \textup{} |
| 意大利   | \textit{} |
| 倾斜     | \textsl{} |
| 小型大写 | \textsc{} |
| 加宽加粗 | \textbf{} |

### 章节

对于`ctexart`文件类型，章节可以用`\section{}`和`\subsection{}`命令来标记，例如：

```tex
\documentclass[12pt, a4paper, oneside]{ctexart}
\usepackage{amsmath, amsthm, amssymb, graphicx}
\usepackage[bookmarks=true, colorlinks, citecolor=blue, linkcolor=black]{hyperref}

% 导言区

\title{我的第一个\LaTeX 文档}
\author{Dylaaan}
\date{\today}

\begin{document}

\maketitle

\section{一级标题}

\subsection{二级标题}

这里是正文. 

\subsection{二级标题}

这里是正文. 

\end{document}
```

### 目录

在有了章节的结构之后，使用`\tableofcontents`命令就可以在指定位置生成目录。通常带有目录的文件需要编译两次，因为需要先在目录中生成.toc文件，再据此生成目录。

```tex
\documentclass[12pt, a4paper, oneside]{ctexart}
\usepackage{amsmath, amsthm, amssymb, graphicx}
\usepackage[bookmarks=true, colorlinks, citecolor=blue, linkcolor=black]{hyperref}

% 导言区

\title{我的第一个\LaTeX 文档}
\author{Dylaaan}
\date{\today}

\begin{document}

\maketitle

\tableofcontents

\section{一级标题}

\subsection{二级标题}

这里是正文. 

\subsection{二级标题}

这里是正文. 

\end{document}
```

### 图片

插入图片需要使用`graphicx`宏包，建议使用如下方式：

```tex
\begin{figure}[htbp]
    \centering
    \includegraphics[width=8cm]{图片.jpg}
    \caption{图片标题}
\end{figure}
```

其中，`[htbp]`的作用是自动选择插入图片的最优位置，`\centering`设置让图片居中，`[width=8cm]`设置了图片的宽度为8cm，`\caption{}`用于设置图片的标题。

### 表格

LaTeX中表格的插入较为麻烦，可以直接使用[Create LaTeX tables online – TablesGenerator.com](https://link.zhihu.com/?target=https%3A//www.tablesgenerator.com/%23)来生成。建议使用如下方式：

```tex
\begin{table}[htbp]
    \centering
    \caption{表格标题}
    \begin{tabular}{ccc}
        1 & 2 & 3 \\
        4 & 5 & 6 \\
        7 & 8 & 9
    \end{tabular}
\end{table}
```

### 列表

LaTeX中的列表环境包含无序列表`itemize`、有序列表`enumerate`和描述`description`，以`enumerate`为例，用法如下：

```tex
\begin{enumerate}
    \item 这是第一点; 
    \item 这是第二点;
    \item 这是第三点. 
\end{enumerate}
```

另外，也可以自定义`\item`的样式：

```tex
\begin{enumerate}
    \item[(1)] 这是第一点; 
    \item[(2)] 这是第二点;
    \item[(3)] 这是第三点. 
\end{enumerate}
```

### 定理环境

定理环境需要使用`amsthm`宏包，首先在导言区加入：

```text
\newtheorem{theorem}{定理}[section]
```

其中`{theorem}`是环境的名称，`{定理}`设置了该环境显示的名称是“定理”，`[section]`的作用是让`theorem`环境在每个section中单独编号。在正文中，用如下方式来加入一条定理：

```tex
\begin{theorem}[定理名称]
    这里是定理的内容. 
\end{theorem}
```

其中`[定理名称]`不是必须的。另外，我们还可以建立新的环境，如果要让新的环境和`theorem`环境一起计数的话，可以用如下方式：

```tex
\newtheorem{theorem}{定理}[section]
\newtheorem{definition}[theorem]{定义}
\newtheorem{lemma}[theorem]{引理}
\newtheorem{corollary}[theorem]{推论}
\newtheorem{example}[theorem]{例}
\newtheorem{proposition}[theorem]{命题}
```

另外，定理的证明可以直接用`proof`环境。

### 页面

最开始选择文件类型时，我们设置的页面大小是a4paper，除此之外，我们也可以修改页面大小为b5paper等等。

一般情况下，LaTeX默认的页边距很大，为了让每一页显示的内容更多一些，我们可以使用`geometry`宏包，并在导言区加入以下代码：

```tex
\usepackage{geometry}
\geometry{left=2.54cm, right=2.54cm, top=3.18cm, bottom=3.18cm}
```

另外，为了设置行间距，可以使用如下代码：

```tex
\linespread{1.5}
```

### 页码

默认的页码编码方式是[阿拉伯数字](https://www.zhihu.com/search?q=阿拉伯数字&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A"456055339"})，用户也可以自己设置为小写罗马数字：

```tex
\pagenumbering{roman}
```

另外，`aiph`表示小写字母，`Aiph`表示大写字母，`Roman`表示大写罗马数字，`arabic`表示默认的阿拉伯数字。如果要设置页码的话，可以用如下代码来设置页码从0开始：

```
\setcounter{page}{0}
```

# Latex数学公式

## 字体

| 名称    | 数学表达式    | Latex公式   |
| ------- | ------------- | ----------- |
| 花体A   | $\mathbb{A}$  | \mathbb{A}  |
| 手写体N | $\mathcal{N}$ | \mathcal{N} |
|         |               |             |

## 希腊

| 序号 | 大写 | 小写 | 英文注音 | 国际音标注音 | 中文注音 |
| ---- | ---- | ---- | -------- | ------------ | -------- |
| 1    | Α    | α    | alpha    | a:lf         | 阿尔法   |
| 2    | Β    | β    | beta     | bet          | 贝塔     |
| 3    | Γ    | γ    | gamma    | ga:m         | 伽马     |
| 4    | Δ    | δ    | delta    | delt         | 德尔塔   |
| 5    | Ε    | ε    | epsilon  | ep`silon     | 伊普西龙 |
| 6    | Ζ    | ζ    | zeta     | zat          | 截塔     |
| 7    | Η    | η    | eta      | eit          | 艾塔     |
| 8    | Θ    | θ    | theta    | θit          | 西塔     |
| 9    | Ι    | ι    | iot      | aiot         | 约塔     |
| 10   | Κ    | κ    | kappa    | kap          | 卡帕     |
| 11   | Λ    | λ    | lambda   | lambd        | 兰布达   |
| 12   | Μ    | μ    | mu       | mju          | 缪       |
| 13   | Ν    | ν    | nu       | nju          | 纽       |
| 14   | Ξ    | ξ    | xi       | ksi          | 克西     |
| 15   | Ο    | ο    | omicron  | omik`ron     | 奥密克戎 |
| 16   | Π    | π    | pi       | pai          | 派       |
| 17   | Ρ    | ρ    | rho      | rou          | 肉       |
| 18   | Σ    | σ    | sigma    | `sigma       | 西格马   |
| 19   | Τ    | τ    | tau      | tau          | 套       |
| 20   | Υ    | υ    | upsilon  | jup`silon    | 宇普西龙 |
| 21   | Φ    | φ    | phi      | fai          | 佛爱     |
| 22   | Χ    | χ    | chi      | phai         | 西       |
| 23   | Ψ    | ψ    | psi      | psai         | 普西     |
| 24   | Ω    | ω    | omega    | o`miga       | 欧米伽   |
|      |      |      |          |              |          |

## 符号

| 名称 | 数学表达式   | Latex公式  |
| ---- | ------------ | ---------- |
| 黑点 | $\bullet$    | \bullet    |
| 上撇 | $\prime$     | \prime     |
| 上撇 | $f^{\prime}$ | f^{\prime} |
| 无穷 | $\infty$     | \infty     |
| 偏导 | $\partial$   | \partial   |
| 梯度 | $\nabla$     | \nabla     |
|      |              |            |

## 关系式

| 名称     | 数学表达式                                                | Latex公式    |
| -------- | --------------------------------------------------------- | ------------ |
| 大于等于 | $\geq$                                                    | `$\geq$`     |
| 小于等于 | $\leq$                                                    | `$\leq$`     |
| 不等于   | ![\not=](https://math.jianshu.com/math?formula=%5Cnot%3D) | `$not=$`     |
| 不小于   | ![\not<](https://math.jianshu.com/math?formula=%5Cnot%3C) | `$\not<$`    |
| 约等于   | $\approx$                                                 | \approx      |
| 定义     | $\triangleq{}$                                            | \triangleq{} |

## 属于

| 名称   | 数学表达式    | Latex公式   |
| ------ | ------------- | ----------- |
| 包含于 | $\subset$     | \subset     |
| 真包含 | $\subset$     | \subset     |
| 包含   | $\supset$     | \supset     |
| 不包含 | $\not\supset$ | \not\supset |
| 属于   | $\in$         | \in         |
| 相交   | $\cap$        | \cap        |
| 相并   | $\cup$        | \cup        |
| 空集   | $\emptyset$   | \emptyset   |



## 乘除法

| 名称   | 数学表达式                                                | Latex公式 |
| ------ | --------------------------------------------------------- | --------- |
| 加减   | ![\pm](https://math.jianshu.com/math?formula=%5Cpm)       | `$\pm$`   |
| 点乘   | ![\cdot](https://math.jianshu.com/math?formula=%5Ccdot)   | `$cdot$`  |
| 乘     | ![\times](https://math.jianshu.com/math?formula=%5Ctimes) | `$\times` |
| 除     | ![\div](https://math.jianshu.com/math?formula=%5Cdiv)     | `$\div$`  |
| 乘法点 | $\cdot$                                                   | `$\cdot$` |



## 上标帽子

| 名称 | 数学表达式      | Latex公式     |
| ---- | --------------- | ------------- |
|      | $\overline{a}$  | \overline{a}  |
|      | $\hat{a}$       | \hat{a}       |
|      | $\widehat{a}$   | \widehat{a}   |
|      | $\overline{a}$  | \overline{a}  |
|      | $\widetilde{a}$ | \widetilde{a} |
|      | $\dot{a}$       | \dot{a}       |
|      | $\ddot{a}$      | \ddot{a}      |

## 符号上下标

| 名称     | 数学表达式                                              | Latex公式                     |
| -------- | ------------------------------------------------------- | ----------------------------- |
|          | $\hat \beta = \underset{\beta}{\arg \min} L(\beta)$     | \underset{?}{?}               |
| 数学符号 | $\sum\limits_{i=1}$                                     | \limits                       |
| 普通符号 | $\mathop{\lambda}\limits_{i=1}$先\mathop转换，再\limits | \mathop{\lambda}\limits_{i=1} |
| 等号上标 | $\overset{\text{def}}{=}$                               | \usepackage{amsmath}          |
| 等号下标 | $\underset{\text{heated}}{=}$                           | \usepackage{amsmath}          |
|          |                                                         |                               |

 



## 箭头

箭头
数学算式	Markdown公式	核心语法
↑ \uparrow↑	\uparrow	
↓ \downarrow↓	\downarrow	
↕ \updownarrow↕	\updownarrow	
⇑ \Uparrow⇑	\Uparrow	
⇓ \Downarrow⇓	\Downarrow	
⇕ \Updownarrow⇕	\Updownarrow	
→ \rightarrow→	\rightarrow	
← \leftarrow←	\leftarrow	
↔ \leftrightarrow↔	\leftrightarrow	
⇒ \Rightarrow⇒	\Rightarrow	
⇐ \Leftarrow⇐	\Leftarrow	
⇔ \Leftrightarrow⇔	\Leftrightarrow	
⟶ \longrightarrow⟶	\longrightarrow	
⟵ \longleftarrow⟵	\longleftarrow	
⟷ \longleftrightarrow⟷	\longleftrightarrow	
⟹ \Longrightarrow⟹	\Longrightarrow	
⟸ \Longleftarrow⟸	\Longleftarrow	
⟺ \Longleftrightarrow⟺	\Longleftrightarrow	

## 排列方程组

使方程组对其，使用array环境，从begin到end中间的部分表示都是array阵列中的内容

- l代表左对齐，r代表右对齐，c代表居中
- &表示间隔符，每行的$会自动对齐
- 其中，{array}{l}表示一列的排列方式，而使用了&后，左右各两列，此处应该使用{array}{ll}

```latex
\begin{array}{l代表左对齐，r代表右对齐，c代表居中}
\nabla\cdot \mathbf{E} &=0\\
\nabla\cdot \mathbf{B} &=0\\
\nabla\times \mathbf{E} &=0\\
\end{array}
```


$$
\begin{array}{l代表左对齐，r代表右对齐，c代表居中}
\nabla\cdot \mathbf{E} &=0\\
\nabla\cdot \mathbf{B} &=0\\
\nabla\times \mathbf{E} &=0\\
\end{array}
$$
还可以使用align环境，有备注功能，&&

如果希望使用大括号可以添加`\left\{ \right`



|        |                   |                   |
| ------ | ----------------- | ----------------- |
| 左括号 | $\left\{ \right.$ | `\left\{ \right.` |
|        |                   |                   |
|        |                   |                   |



## 矩阵：

$$
\mu=\left[
\begin{matrix}
  \mu_1\\
  \mu_2\\
  ...\\
  \mu_n
 \end{matrix} \right]
 =\frac{1}{m}\sum_{i=1}^mx^{(i)}
$$

$$
\mu=\left[
\begin{matrix}
  1 & 2 & 3\\
  4 & 5 & 6\\
  ...\\
  4 & 5 & 6
 \end{matrix} \right]
 =\frac{1}{m}\sum_{i=1}^mx^{(i)}\tag{1}
$$

|        |                                        |         |
| ------ | -------------------------------------- | ------- |
| 无     | $\begin{matrix}a&b\\c&d\end{matrix}$   | matrix  |
| 圆括号 | $\begin{pmatrix}a&b\\c&d\end{pmatrix}$ | pmatrix |
| 方括号 | $\begin{bmatrix}a&b\\c&d\end{bmatrix}$ | bmaritx |
| 行列式 | $\begin{vmatrix}a&b\\c&d\end{vmatrix}$ | vmatrix |
| 大括号 | $\begin{Bmatrix}a&b\\c&d\end{Bmatrix}$ | Bmatrix |
| 范数   | $\begin{Vmatrix}a&b\\c&d\end{Vmatrix}$ | Vmatrix |
| 横三点 | $\cdots$                               | \cdots  |
| 竖三点 | $\vdots$                               | \vdots  |
| 斜三点 | $\ddots$                               | \ddots  |

## 语法

### 公式左对齐

$$
y=\left\{
\begin{matrix}
1 && if \quad p(x)\leq\epsilon \quad(非正常) \hfill\\
0 && if \quad p(x)\geq\epsilon \quad(正常)\hfill
\end{matrix}
\right.
$$

```
to apply an operation along a dimension,We can use the Tensor.view() function to reshape tensors similarly to numpy.reshape()

It can also automatically calculate the correct dimension if a -1 is passed in. This is useful if we are working with batches, but the batch size is unknown.

```
