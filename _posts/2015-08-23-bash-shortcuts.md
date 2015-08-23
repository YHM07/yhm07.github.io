---
layout: post
title: "Bash shortcuts"
category: "linux"
tags: [bash, linux]
---

**提升命令行操作效率的Bash快捷键**

在平常学习过程当中，常使用Bash操作，为提供效率特总结以下常用快捷键。

<!-- more -->

1. 编辑命令

> ctrl + a: 移到行首

> ctrl + e: 移到行尾

> ctrl + xx: 在命令行首和光标之间移动

> ctrl + f: 按字符前移

> ctrl + b: 按字符后移

> alt + f: 按单词迁移

> alt + b: 按单词后移

> ctrl + u: 从光标处删除至行首

> ctrl + k: 从光标处删除至行尾

> ctrl + w: 从光标处删除至字首

> alt + d: 从光标处删除至字尾

> ctrl + d: 删除光标处字符

> ctrl + h: 删除光标前字符

> ctrl + y: 粘贴最后一次删除的字符至光标处

> ctrl + t: 交换光标处和之前的字符

> alt + t: 交换光标处和之前的单词

> alt + backspace: 与ctrl+w类似

2. 重新执行命令

> ctrl + r: 逆向搜索历史命令

> ctrl + g: 从历史搜索模式退出

> ctrl + p: 历史中的上一条命令

> ctrl + n: 历史中的下一条命令

> alt + . or Esc + .: 使用上一条命令的最后一个参数

3. 控制命令

> ctrl + l: 清屏

> ctrl + o: 执行当前命令，并选择上一条命令

> ctrl + s: 阻止屏幕输出

> ctrl + q: 允许屏幕输出

> ctrl + c: 终止命令

> ctrl + z: 挂起命令

4. Bang(!)命令

> !!: 上一条命令

> !blah: 执行最近的以blah开头的命令，如!ls

> !blah:p: 仅打印输出，不执行

> !$: 上一条命令的最后一个参数,与Esc+.相同

> !$:p: 打印输出!$的内容

> !*: 上一条命令的所有参数

> ^blah^foo: 将上一条命令的blah替换为foo

> ^blah^foo^: 将上一条命令的blah全部替换为foo

**注：关于Bang(!)命令可以参考：[using cli][1] or [像黑客一样使用Linux命令行][2]**
[1]: http://iplayboy.tk/linux/2015-08/using-cli.html
[2]: http://talk.linuxtoy.org/using-cli/#1

__友情提示__

**以上Bash快捷键仅当emacs编辑模式时有效，如将Bash配置为vi模式，需参考vi
按键绑定。Bash默认为Emacs编辑模式，如果不在emacs编辑模式，通过set -o emacs
设置。**

**注：在长期操作Bash的过程当中，个人认为Bash默认的Emacs编辑模式较vi而言更为顺手，虽然我也是一名忠实的vimer.**
