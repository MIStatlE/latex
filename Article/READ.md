
# 🎯 LaTeX 风格文件说明与可定制指南

本文基于你提供的**风格文件**（导言区代码），按模块解释其作用，并给出**如何修改**的常用方法与示例。复制即可作为 `README.md` 使用。

---

## 0) 快速上手

- 使用 **XeLaTeX / LuaLaTeX** 编译（必须，已在样式中强校验）。
- 按如下顺序在你的 `main.tex` 中导入样式（或把这段当作独立的 `miextras.sty` 再 `\usepackage{miextras}`）：
  ```latex
  % 放在 \documentclass 之后
  % \input{miextras.sty} 或 \usepackage{miextras}

	•	章节号由你自定义计数器 chapter 控制，article 也能做“第X章”的编号（见 §7）。

⸻

1) Engine（编译引擎强校验）

代码片段：

\usepackage{ifxetex,ifluatex}
\usepackage{everypage}
\ifxetex\else\ifluatex\else
  \errmessage{Please compile with XeLaTeX or LuaLaTeX}
\fi\fi

作用：限制必须用 XeLaTeX 或 LuaLaTeX，以支持中文与 fontspec。

修改：如需兼容 pdfLaTeX，删除这一段并移除 fontspec/ctex 依赖（不推荐）。

⸻

2) 字体与页面（Fonts & Page）

代码片段：

\usepackage{ctex,fontspec,geometry,xcolor}
\geometry{paperwidth=7.5in,paperheight=10in, margin=0.7in}
\setmainfont{Times New Roman}
\IfFontExistsTF{Noto Serif CJK SC}{\setCJKmainfont{Noto Serif CJK SC}}{\setCJKmainfont{FandolSong}}
\linespread{1.4}
\pagecolor{white}\color{black}

作用：
	•	设置纸张与页边距（geometry）。
	•	西文字体、中文主字体（优先 Noto Serif CJK SC，否则回退 FandolSong）。
	•	行距 1.4；页面背景白色。

常见修改：
	•	改纸张/页边距：

\geometry{a4paper, margin=1in}           % A4
% 或 \geometry{paperwidth=6in,paperheight=9in, margin=0.8in}


	•	改字体：

\setmainfont{TeX Gyre Termes}            % 西文
\setCJKmainfont{Songti SC}               % 中文（Mac）


	•	改行距：\linespread{1.2} / \linespread{1.6}

⸻

3) 调色板（Colors）

代码片段：

\definecolor{brand}{HTML}{1A9D8F}
\definecolor{brandD}{HTML}{2A7F6F}
\definecolor{ink}{HTML}{1A1A1A}
\definecolor{keybg}{HTML}{E8F4F1}
\definecolor{keydark}{HTML}{2F5E58}
\definecolor{accent}{HTML}{FF6B6B}
\definecolor{accentD}{HTML}{D94F4F}
\definecolor{soft}{HTML}{F3F8F7}
% 以及一组 xhs* 补充色

作用：统一全局主色、副主色、正文墨色、盒子背景、强调色等。

修改：直接替换 HEX 值即可。例如：

\definecolor{brand}{HTML}{0A84FF}  % 换为 iOS 蓝


⸻

4) 日期宏（Date macro）

代码片段：

\newcommand{\padzero}[1]{\ifnum#1<10 0\fi\number#1}
\newcommand{\BrandYMD}{{\the\year·\padzero{\the\month}·\padzero{\the\day}}}

作用：生成 YYYY·MM·DD 的日期格式，可在封面或页脚使用：{\BrandYMD}。

修改：改分隔符或格式：

\newcommand{\BrandYMD}{{\the\year-\padzero{\the\month}-\padzero{\the\day}}}


⸻

5) 标题样式（Section Titles）

代码片段：

\usepackage{titlesec}
\titleformat{\section}[block]
  {\centering\bfseries\color{keydark}\fontsize{24pt}{26pt}\selectfont}
  {\thesection}{0.6em}{}
\titlespacing*{\section}{0pt}{0.9em}{0.5em}

\titleformat{\subsection}{\Large\bfseries\color{brand}}{\thesubsection}{0.6em}{}
\titlespacing*{\subsection}{0pt}{0.8em}{0.4em}

作用：自定义 \section / \subsection 的字号、颜色、间距等。

常见修改：
	•	改字号：24pt → 20pt，26pt → 24pt
	•	改颜色：\color{brand} / \color{ink}
	•	改间距（上/下）：\titlespacing*{\section}{0pt}{1.2em}{0.6em}

⸻

6) 左侧彩带（Left bands）

代码片段：

\usepackage{tikz,eso-pic}
\usetikzlibrary{positioning,calc}
\newcommand{\LeftBandA}{0.12in}
\newcommand{\LeftBandB}{0.28in}
\newcommand{\PutLeftBands}{%
  \begin{tikzpicture}[remember picture, overlay]
    \fill[keydark] ([xshift=0cm]current page.north west)
                   rectangle ([xshift=\LeftBandA]current page.south west);
    \fill[keybg]   ([xshift=\LeftBandA]current page.north west)
                   rectangle ([xshift=\LeftBandB]current page.south west);
  \end{tikzpicture}%
}
\AddEverypageHook{\PutLeftBands}

作用：在每页左侧绘制两条竖向色带，增强视觉识别。

修改：
	•	关闭色带（临时）：注释掉 \AddEverypageHook{\PutLeftBands}。
	•	改宽度：调整 \LeftBandA、\LeftBandB。
	•	改颜色：替换 keydark、keybg 为其他定义色。

⸻

7) 页眉页脚（Header / Footer）

代码片段：

\usepackage{fancyhdr}
\fancypagestyle{coverstyle}{%
  \fancyhf{}\renewcommand{\headrulewidth}{0pt}\renewcommand{\footrulewidth}{0pt}
}
\fancyhf{}
\renewcommand{\headrulewidth}{0.4pt}
\renewcommand{\sectionmark}[1]{\markright{\thesection\ #1}}
\fancyhead[R]{\nouppercase{\rightmark}}
\fancyfoot[L]{\textcolor{brand}{@MIStatlE}｜集中不等式 \seriesnum}
\fancyfoot[R]{\textcolor{brand}{\thepage}}
\pagestyle{fancy}

作用：
	•	右上角显示“节号 + 节题”（\rightmark 来源于 \sectionmark 的自定义）。
	•	左下角作者签名 + 系列名；右下角页码。
	•	coverstyle 用于封面页无眉脚：单页使用 \thispagestyle{coverstyle}。

常见修改：
	•	改签名内容或隐藏：

\fancyfoot[L]{\textcolor{brand}{@YourID}｜你的系列名 \seriesnum}
% 或留空：\fancyfoot[L]{}


	•	改页码位置：把 \fancyfoot[R]{...} 改到 C（中）或 L（左）。
	•	关闭页眉横线：\renewcommand{\headrulewidth}{0pt}。
	•	让奇偶页切换显示不同信息：用 \fancyhead[LE]{...}、\fancyhead[RO]{...}（需要 book 类更合理；article 也可用但不自动章切换）。

⸻

8) 系列编号参数（Series number）

代码片段：

\newcommand{\seriesnum}{I}

作用：用于页脚中的系列编号展示，手动改即可。

⸻

9) 高亮盒子（tcolorbox）

代码片段：

\usepackage[most]{tcolorbox}
\tcbset{enhanced, boxrule=0.9pt, arc=3pt, left=10pt,right=10pt,top=10pt,bottom=10pt}
\newtcolorbox{KeyBox}{
  colback=keybg, colframe=brandD,
  colbacktitle=brandD, title=\textbf{\color{white}Key Idea},
  fonttitle=\bfseries\large, coltitle=white, breakable
}
\newtcolorbox{Takeaway}{
  colback=accent!12, colframe=accentD,
  colbacktitle=accentD, title=\textbf{\color{white}Takeaway},
  fonttitle=\bfseries\large, coltitle=white, breakable
}

作用：提供 KeyBox/Takeaway 两种提示框。

常见修改：
	•	改标题文字：title=...
	•	改边框/背景色：colframe=... / colback=...
	•	改圆角：arc=6pt
	•	改边框粗细：boxrule=1.1pt

⸻

10) 流式侧栏（SideBar）

代码片段：

\usepackage{mdframed}
\newmdenv[
  topline=false, bottomline=false, rightline=false,
  linewidth=4pt, linecolor=brand, backgroundcolor=soft,
  innerleftmargin=10pt, innerrightmargin=10pt,
  skipabove=6pt, skipbelow=6pt
]{SideBar}

作用：左侧粗竖线 + 柔色背景的侧栏环境，用于阅读提示或备注。

修改：

linecolor=brandD, linewidth=3pt, backgroundcolor=keybg


⸻

11) 数学环境与阴影定理（Math & Theorems）

代码片段（要点）：

\usepackage{amsmath,amssymb,bm,amsthm}
\DeclareMathOperator{\Var}{Var}
\newcommand{\E}{\mathbb{E}}

\usepackage{framed}
\definecolor{shadecolor}{RGB}{235,245,255}
\newenvironment{ShadedTheorem}[1][]{%
  \begin{shaded}\begin{theorem}[#1]}{\end{theorem}\end{shaded}}

\newtheoremstyle{customthm}{...}{...}{\itshape}{...}{\bfseries}{.}{...}{}
\newtheoremstyle{customdef}{...}{...}{}{}{\bfseries}{.}{...}{}

\theoremstyle{customthm}
\newtheorem{theorem}{定理}[section]
...
\theoremstyle{customdef}
\newtheorem{definition}[theorem]{定义}

作用：
	•	定义 \E、\Var 记号。
	•	定理体系（按 section 编号）：theorem/lemma/proposition/corollary/definition/remark。
	•	ShadedTheorem：自带淡蓝底的“定理”阴影环境。

修改：
	•	改编号维度：按 chapter 编号 → 把 [section] 改为 [chapter]。
	•	去掉阴影定理：不使用 ShadedTheorem，直接用 theorem；或改 shadecolor。
	•	新增「命题+证明草图」环境：

\newtheorem{claim}[theorem]{断言}



⸻

12) 自定义“章”编号（在 article 类中模拟章节体系）

代码片段：

\newcounter{chapter}
\newcommand{\setchapter}[2]{%
  \setcounter{chapter}{#1}%
  \setcounter{section}{0}%
  \def\ChapterTitle{#2}%
}
\renewcommand{\thesection}{\arabic{chapter}.\arabic{section}}
\renewcommand{\thesubsection}{\arabic{chapter}.\arabic{section}.\arabic{subsection}}

\newcommand{\ChapterHeading}{...}
\newcommand{\BeginChapter}[2]{\clearpage\setchapter{#1}{#2}\ChapterHeading}

作用：即使在 article 类，也能用“第 X 章”的编号与标题块。

用法：

\setchapter{1}{概率空间}  % 设置本章编号与标题
\ChapterHeading          % 打印“第1章 概率空间”
% 或：\BeginChapter{2}{大数定律}  % 换章并自动打印

修改：
	•	改标题风格（字号/颜色/间距）：在 \ChapterHeading 中调整。
	•	改编号格式：修改 \thesection 与 \thesubsection。

⸻

13) 备注、例子与证明（Remark / Example / Proof）

代码片段：

\newenvironment{RemarkNote}[1][]{ ... }
\newtcolorbox{Example}[1][]{ ... }
\renewenvironment{proof}[1][证明]{ ... }{ ... }

作用：
	•	RemarkNote：非盒子风的强调段落。
	•	Example：紫色系盒子，适合放例子/练习。
	•	proof：自定义了“证明”标题样式，自动 \qed 结束。

修改：
	•	改 RemarkNote 标题文字：把 Remark 改为 注 或 说明。
	•	改 Example 的颜色：exframe / exbg 的 HEX。

⸻

14) 页末系列导航卡（SeriesCardEnd）

代码片段：

\newcommand{\SeriesCardEnd}[3]{...}
% 使用：
%\SeriesCardEnd{I}{上一篇标题}{下一篇标题}

作用：页面底部展示“集中不等式系列 · 第 I 篇 / 上一篇 / 下一篇”。

修改：
	•	改文案：把文本 集中不等式系列 · 第 修改为你的系列名称。
	•	改边框与背景色：brand / soft。

⸻

15) 常见问题（Troubleshooting）
	•	(1) 编译报错：请使用 XeLaTeX / LuaLaTeX
	•	原因：样式强制检查编译引擎。
	•	(2) 页眉高度警告 headheight
	•	解决：在 fancyhdr 后加 \setlength{\headheight}{14pt}（某些平台需要）。
	•	(3) 中文字体缺失
	•	安装 Noto Serif CJK SC 或改为本机字体：Songti SC（macOS）、SimSun（Windows）。

⸻


Happy TeXing! 🎉 如果你希望把这份导言封装为 miextras.sty，我可以再给你一份最小化打包版本与 API 一览表。

