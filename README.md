
# 📘 LaTeX 样式文档使用说明

本项目提供了一套适合写 **数学笔记** 的 LaTeX 样式。

---

## 📂 文件结构

所有文件放在同一个文件夹下：

.
├── main.tex         # 主文档入口
├── miextras.sty     # 自定义样式文件（字体 / 页眉页脚 / 盒子环境）

---

## 🚀 使用方法

1. **准备环境**
   - 推荐编译器：**XeLaTeX** 或 **LuaLaTeX**
   - 必要宏包：`ctex`、`fontspec`、`tikz`、`fancyhdr`、`tcolorbox`、`amsmath` 等

2. **编译命令**
   ```bash
   xelatex main.tex

	3.	文档入口
在 main.tex 中直接调用：

\documentclass[10pt,openany]{book}
\usepackage{miextras}

\begin{document}
% 封面
\MakeBookCoverUltraSimple{集中不等式}{高维概率与机器学习理论中的浓缩现象}{上册 · 预印本}

% 目录（无页眉页脚）
\TOCwithoutHeadFoot

% 正文
\mainmatter
\part{预备知识}
\chapter{概率预备}
\section{Orlicz 范数}
\begin{definition}[Orlicz 范数]
随机变量 $Z$ 的 $\psi_2$ 范数定义为
$begin:math:display$
  \\|Z\\|_{\\psi_2} := \\inf\\{s>0:\\ \\E e^{Z^2/s^2}\\le 2\\}.
$end:math:display$
\end{definition}
\end{document}



⸻

✨ 样式功能概览
	•	页眉页脚
	•	偶数页左：章标题（\leftmark）
	•	奇数页右：节标题（\rightmark）
	•	页脚：页码 + 内侧署名 @MIStatlE｜集中不等式 I
	•	封面页、目录页自动无页眉页脚
	•	颜色体系
	•	brand：主色 (#1A9D8F)
	•	brandD：深主色
	•	keybg：浅背景
	•	accent：强调红
	•	soft：淡灰背景
	•	盒子环境
	•	KeyBox —— 关键思想
	•	Takeaway —— 总结要点
	•	Example —— 示例
	•	SideBar —— 侧栏提示
	•	数学环境
	•	定理类：theorem, lemma, proposition, corollary
	•	定义：definition
	•	注释：remark
	•	自动按章节编号

⸻

📄 提示
	•	如果缺少中文字体 Noto Serif CJK SC，可改用系统已有字体，例如 SimSun 或 Songti SC。
	•	\TOCwithoutHeadFoot 宏可以确保目录无页眉页脚，正文保持正常。
	•	图目录、表目录也可以用相同方式包装：

\ListWithoutHeadFoot{\listoffigures}
\ListWithoutHeadFoot{\listoftables}



⸻

📚 参考
	•	R. Vershynin, High-Dimensional Probability, CUP 2018
	•	P. Billingsley, Probability and Measure, Wiley 1995
	•	R. Durrett, Probability: Theory and Examples, CUP 2019

⸻

© 2025 @MIStatlE — 本样式仅供学习与学术交流，欢迎自由使用。

要不要我帮你再写一个 **`miextras.sty` 的 API 说明**，把所有自定义命令（比如 `\MakeBookCoverUltraSimple`, `\TOCwithoutHeadFoot`, `KeyBox` 等）逐一列出？
