---
layout: post
title:  "Markdown Syntax"
keywords: "Markdown Syntax"
description: "Markdown Syntax"
category: demo
tags: [Markdown, Syntax]
---

Markdown Syntax

<!-- more -->

**Headings**

# The largest heading (an h1 tag)

## The second largest heading (an h2 tag)
…
###### The 6th largest heading (an h6 tag)

**Blockquotes**

> Pardon my french

- - -

**Styling text**

*This text will be tialic*

**This text will be bold**

Both **bold** and *italic* can use either a "*" or an "_" around the text for seyling.

**Everyone _must_ attend the meeting at 5 o'clock today**

**Lists**

*unordered list*

* Item1
* Item2
* Item3

*Ordered lists*

1. Item1
2. Item2
3. Item3

*Nested lists*

1. Item1
  1. item1
  2. item2
2. Item2
  * item1 
  * Item2
3. Item3

**Code formating**

*Inline formats*

Here's an idea: why don't we take `SuperiorProject` and turn it into `**Reasonable** Projects`.

*Multiple lines*

```
/* print Hello world */
#include <stdio.h>
int main(int argc,char *argv[])
{
	printf("Hello World!\n);
	return 0;
	}
```
**Links**

Visit [GitHub](https://github.com),you'd write this in Markdown [GitHubHelp][1].
[1]: https://help.github.com/articles/markdown-basics/

**table**


|head1|head2|head3|head4
|---|:---|---:|:---:|
|row1text1|row1text2|row1text3|row1text4
|row2text1|row2text2|row2text3|row2text4
|row3text1|row3text2|row3text3|row3text4
|row4text1|row4text2|row4text3|row4text4

