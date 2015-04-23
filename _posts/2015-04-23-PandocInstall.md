---
layout: post
title: "Pandoc Install"
keywords: "Ubuntu14.04 Pandoc"
description: "How to install Pandoc on Ubuntu14.04 64bit"
tags: [Pandoc Ubuntu14.04 64bit]
---

1. install Pandoc
	* 下载[pandoc][1],然后可以使用Software Center安装，或者*dpkg -i ***.deb* 
2. Install texlive
	 `sudo apt-get install texlive`
3. Install texlive-extex
	`sudo apt-get install texlive-extex`
4. Install texlive-extra
	`sudo apt-get install texlive-extra`
5. Install texlive-cjk-chinese
	`sudo apt-get install texlive-cjk-chinese`
	or
	`sudo apt-get install texlive-lang-cjk`
6. 生成latex模板
	`cd ~ && pandoc -D latex > template.tex`
	and 
	`cd ~ && mv template.tex Templates`
	然后，更改该模板为
	>
	\else % if luatex or xelatex
	 \ifxetex
	    \usepackage{mathspec}
	    \usepackage{xltxtra,xunicode}
	       \usepackage{xeCJK}
	   %   \setCJKmainfont{Droid Sans Fallback}
	   %   \setCJKsansfont{Droid Sans Fallback}
	   %   \setCJKmonofont{Droid Sans Fallback}
	       \setCJKmainfont{YaHei Consolas Hybrid}
	       \setCJKsansfont{YaHei Consolas Hybrid}
	       \setCJKmonofont{YaHei Consolas Hybrid}
	\else
		\usepackage{fontspec}
	\fi
	% 以上字体部分请使用`fc-list :lang=zh > lang`命令查看，
	% 选择自己喜欢的字体。通过更改以上部分输出pdf格式文件时就可以支持中文了。
	
7. edit ~/.bashrc 
	在结尾添加:
	`alias pandoc="pandoc --template=$HOME/Templates/template.tex --latex-engine=xelatex"`

	

	 








[1]: https:://github.com/jgm/pandoc/releases "pandoc releases"
