---
title: latex相关
tags:
  - Python
abbrlink: 419e
top: 
---



## latex相关

## 图片

```python
\begin{figure}[htbp]
  \centering
  \includegraphics[width=11cm]{图片文件名}
  \caption{注释}
  \label{标签}
\end{figure}
```

引用图片

```python
~\ref{标签}~
h：表示“here”，告诉LaTeX尽可能将图片放置在代码所在的位置。
t：表示“top”，告诉LaTeX将图片放置在页面的顶部。
b：表示“bottom”，告诉LaTeX将图片放置在页面的底部。
p：表示“page”，告诉LaTeX将图片放置在一页独立成页，而不与文本混排。
```

## 公式

快捷键：

```python
\begin{equation}
	\label{公式标签}
	aaaaaaa
\end{equation}
```

引用公式：

```python
\eqref{公式标签}
```

## 标题

```python
\section{电致变色窗}
\subsection{电致变色窗}
\subsubsection{电致变色窗}
```



## 参考文献

```python
\cite{BUPT_Thesis_Format_2014}
```

```python
dim$_{\mathrm{input}}$
CNN$_{3\times32\times32}$和CNN$_{6\times8\times8}$
```





## `重要`

```python
打印机网络：CMCC-E5JJ-5G
打印机：NPIA3F2F(FP LaserJet Tank MFP 2606sdw)
```

