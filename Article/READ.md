# 🎯 LaTeX 风格文件说明与可定制指南

本文基于你的 **LaTeX 风格文件**（导言区代码），按模块解释其作用，并给出**如何修改**的常用方法与示例。可直接保存为 `README.md`。

---

## 0) 快速上手

- 使用 **XeLaTeX / LuaLaTeX** 编译（必须，样式已强校验）。
- 将完整风格代码粘贴到你的 `main.tex` 导言区，或另存为 `miextras.sty` 并在主文档中：
  ```latex
  \usepackage{miextras}

	•	章节编号使用自定义 chapter 计数器（即便是 article 也能做“第X章”样式），见 §12。

⸻

1) Engine（编译引擎）

\usepackage{ifxetex,ifluatex}
\usepackage{everypage}
\ifxetex\else\ifluatex\else
  \errmessage{Please compile with XeLaTeX or LuaLaTeX}
\fi\fi

作用：强制使用 XeLaTeX / LuaLaTeX 以支持中文与 fontspec。
修改：不建议移除；如需 pdfLaTeX 需同时放弃 fontspec/ctex。

⸻

2) 字体与页面（Fonts & Page）

\usepackage{ctex,fontspec,geometry,xcolor}
\geometry{paperwidth=7.5in,paperheight=10in, margin=0.7in}
\setmainfont{Times New Roman}
\IfFontExistsTF{Noto Serif CJK SC}{\setCJKmainfont{Noto Serif CJK SC}}{\setCJKmainfont{FandolSong}}
\linespread{1.4}
\pagecolor{white}\color{black}

	•	作用：页面尺寸、边距、字体、行距与页面前景背景色。
	•	常改：
	•	纸张：a4paper, margin=1in 或 6x9 书籍尺寸。
	•	字体：\setmainfont{TeX Gyre Termes}，\setCJKmainfont{Songti SC}。
	•	行距：\linespread{1.2} / 1.6。

⸻

3) 调色板（Colors）

\definecolor{brand}{HTML}{1A9D8F}
\definecolor{brandD}{HTML}{2A7F6F}
\definecolor{ink}{HTML}{1A1A1A}
\definecolor{keybg}{HTML}{E8F4F1}
\definecolor{keydark}{HTML}{2F5E58}
\definecolor{accent}{HTML}{FF6B6B}
\definecolor{accentD}{HTML}{D94F4F}
\definecolor{soft}{HTML}{F3F8F7}
% 以及 xhs* 补充色

	•	作用：统一品牌色、正文色、盒子背景、强调色等。
	•	修改：直接替换 HEX 值即可（如 brand → 0A84FF）。

⸻

4) 日期宏（Date macro）

\newcommand{\padzero}[1]{\ifnum#1<10 0\fi\number#1}
\newcommand{\BrandYMD}{{\the\year·\padzero{\the\month}·\padzero{\the\day}}}

	•	作用：输出 YYYY·MM·DD。
	•	修改：改分隔符：{\the\year-\padzero{\the\month}-\padzero{\the\day}}。

⸻

5) 标题样式（Section Titles）

\usepackage{titlesec}
\titleformat{\section}[block]
  {\centering\bfseries\color{keydark}\fontsize{24pt}{26pt}\selectfont}
  {\thesection}{0.6em}{}
\titlespacing*{\section}{0pt}{0.9em}{0.5em}
\titleformat{\subsection}{\Large\bfseries\color{brand}}{\thesubsection}{0.6em}{}
\titlespacing*{\subsection}{0pt}{0.8em}{0.4em}

	•	作用：自定义 \section/\subsection 字号、颜色、间距。
	•	常改：字号、颜色（keydark→brand）、上下间距。

⸻

6) 左侧彩带（Left bands）

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

	•	作用：每页左侧两条竖向色带。
	•	修改：调宽度 \LeftBandA/\LeftBandB，改色 keydark/keybg。

⸻

7) 页眉页脚（fancyhdr）

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

	•	作用：右上角节号+节题，左下角签名，右下角页码；coverstyle 可用于封面无眉脚（用 \thispagestyle{coverstyle}）。
	•	修改：
	•	签名：\fancyfoot[L]{\textcolor{brand}{@YourID}｜YourSeries \seriesnum}
	•	页码位置：改 R→C/L。
	•	关闭页眉横线：\renewcommand{\headrulewidth}{0pt}。

⸻

8) 系列编号参数

\newcommand{\seriesnum}{I}

	•	作用：页脚显示系列编号。
	•	修改：I/II/III 或 1/2/3。

⸻

9) 高亮盒子（tcolorbox）

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

	•	作用：关键思想/要点总结的高亮盒子。
	•	修改：title 文案、colframe/colback 颜色、arc 圆角、boxrule 线宽。

⸻

10) 侧栏（SideBar）

\usepackage{mdframed}
\newmdenv[
  topline=false, bottomline=false, rightline=false,
  linewidth=4pt, linecolor=brand, backgroundcolor=soft,
  innerleftmargin=10pt, innerrightmargin=10pt,
  skipabove=6pt, skipbelow=6pt
]{SideBar}

	•	作用：左侧实线 + 柔色背景的提示栏。
	•	修改：linecolor/linewidth/backgroundcolor、内边距与上下间距。

⸻

11) 数学与定理环境

\usepackage{amsmath,amssymb,bm,amsthm}
\DeclareMathOperator{\Var}{Var}
\newcommand{\E}{\mathbb{E}}
\usepackage{framed}
\definecolor{shadecolor}{RGB}{235,245,255}
\newenvironment{ShadedTheorem}[1][]{%
  \begin{shaded}\begin{theorem}[#1]}{\end{theorem}\end{shaded}}
\newtheoremstyle{customthm}{0.8em}{0.8em}{\itshape}{}{\bfseries}{.}{0.5em}{}
\newtheoremstyle{customdef}{0.8em}{0.8em}{}{}{\bfseries}{.}{0.5em}{}
\theoremstyle{customthm}
\newtheorem{theorem}{定理}[section]
\newtheorem{lemma}[theorem]{引理}
\newtheorem{proposition}[theorem]{命题}
\newtheorem{corollary}[theorem]{推论}
\theoremstyle{customdef}
\newtheorem{definition}[theorem]{定义}
\newtheorem{remark}[theorem]{注释}

	•	作用：定义 \E、\Var，定理体系按 section 编号；ShadedTheorem 为淡蓝阴影定理。
	•	修改：将 [section] 替换为 [chapter] 以按章编号；或不使用 ShadedTheorem。

⸻

12) “章节”机制（在 article 中模拟章）

\newcounter{chapter}
\newcommand{\setchapter}[2]{%
  \setcounter{chapter}{#1}%
  \setcounter{section}{0}%
  \def\ChapterTitle{#2}%
}
\renewcommand{\thesection}{\arabic{chapter}.\arabic{section}}
\renewcommand{\thesubsection}{\arabic{chapter}.\arabic{section}.\arabic{subsection}}
\newcommand{\ChapterHeading}{%
  \vspace*{0.6cm}
  {\fontsize{26pt}{28pt}\selectfont\bfseries\color{keydark}
   第\arabic{chapter}章\ \ChapterTitle\par}
  \vspace{0.6cm}
}
\newcommand{\BeginChapter}[2]{%
  \clearpage
  \setchapter{#1}{#2}%
  \ChapterHeading
}

	•	作用：在 article 中模拟“第X章 标题”的编号与标题块。
	•	用法：

\setchapter{1}{概率空间}
\ChapterHeading
% 或 \BeginChapter{2}{大数定律}


	•	修改：在 \ChapterHeading 中改字号/颜色/间距；在 \thesection 改编号格式。

⸻

13) 备注/示例/证明

\newenvironment{RemarkNote}[1][]{ ... }
\definecolor{exframe}{HTML}{6C5CE7}
\definecolor{exbg}{HTML}{F2E9FF}
\newtcolorbox{Example}[1][]{ ... }
\renewenvironment{proof}[1][证明]{ ... }{ ... }

	•	作用：RemarkNote 强调段落、Example 例子盒子、proof 中文“证明”标题与自动 \qed。
	•	修改：改 Remark 字样为 “注/说明/Remark”。调整 exframe/exbg 色值。

⸻

14) 页末系列卡片

\newcommand{\SeriesCardEnd}[3]{ ... }
% 用法：\SeriesCardEnd{I}{上一篇标题}{下一篇标题}

	•	作用：页末展示系列名称与上一篇/下一篇。
	•	修改：改文案或颜色（brand/soft）。

⸻

15) 最小示例（MWE）

\documentclass[12pt]{article}
% 这里粘贴你的风格代码（或 \usepackage{miextras}）

\begin{document}
\setchapter{1}{概率空间}
\ChapterHeading

\section{基本定义}
\begin{definition}[样例]
这是定义环境示例。
\end{definition}

\begin{ShadedTheorem}[样例]
这是阴影定理环境示例。
\end{ShadedTheorem}

\begin{proof}
这是证明示例。
\end{proof}

\begin{KeyBox}
这是 Key Idea 高亮盒子。
\end{KeyBox}

\begin{SideBar}
阅读提示与要点。
\end{SideBar}

\end{document}


⸻

16) 关于 MIStatlE

MIStatlE 是作者在概率论、统计学与机器学习理论方向的统一写作签名与样式设计。模板追求：简洁美观、结构清晰、可复用、可维护。欢迎在此基础上做个性化扩展（封面、mini-TOC、品牌 logo 等）。

⸻

Happy TeXing! 如果需要，我可以基于此再打包一个精简版 miextras.sty 与 API 一览表。

