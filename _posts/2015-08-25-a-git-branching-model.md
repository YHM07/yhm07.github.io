---
layout: post
title: "A Git branching model"
category: "wiki" 
tags: [git]
---

**Git** 分支管理策略

参考：
[A successful Git branching model][1]

[Git分支管理策略][2]

[Git使用规范流程][3]

[1]: http://nvie.com/posts/a-successful-git-branching-model/
[2]: http://www.ruanyifeng.com/blog/2012/07/git.html
[3]: http://www.ruanyifeng.com/blog/2015/08/git-use-process.html

<!-- more -->

主分支Master
---

---

代码库应该有一个，且仅有一个主分支。

开发分支Develop
---

---

主分支只用于发布重大版本，日常开发应该在另一条分支上完成。
我们把开发用的分支，叫做**Develop**。

- Git创建Develop分支的命令
	
	`git checkout -b develop master`

> 创建develop分支是基于当前的master，如果当前就在master分支，则可以不写。

> 如果当前分支HEAD指向的是其他分支，又希望基于master分支创建，就必须写master。

- 将develop分支发布到master分支

```
	\# 切换到master分支

	git checkout master

	\# 对develop分支进行合并

	git merge --no-ff develop
```

> --no-ff参数说明：默认情况下，Git执行“快进式合并(fast-farward merge),
会直接将master分支执行develop分支。使用--no-ff参数，会执行正常合并，在master分支生成一个新节点。

临时性分支
---

---

前面讲到版本库的两条主要分支：**master**和**develop**。前者用于正式发布，
后者用于日常开发。除了常设分支，还有一些临时性分支。

> 功能(feature)分支 

> 预发布(release)分支

> bug修复(fixbug)分支 

**这三种分支属于临时性分支，使用完以后，应该删除，使得代码库的常设分支
时钟只有master和develop**

功能分支
===

---

功能分支，它是为了开发某种特定功能，从develop分支上面分出来的。
开发完成后，要再并入develop。

- 创建一个功能分支：

	`git checkout -b feature-x develop`

	`\# 功能分支命名可以采用feature-\*的形式`

- 将功能分支合并到develop分支


	`git checkout develop`

	`git merge --no-ff feature-x`

- 删除feature分支

	`git branch -d feature-x`

预发布分支
===

---

预发布分支是从Develop分支上面分出来的，预发布结束以后，必须合并进
Develop和Master分支。它的命名，可以采用release-\*的形式。

- 创建一个预发布分支
	
	`git checkout -b release-1.2 develop`

- 合并到master分支

	`git checkout master`

	`git merge --no-ff release-1.2`

	`\# 对合并生成的新节点，做一个标签`

	`git tag -a 1.2`

- 合并到develop分支

	`git checkout develop`

	`git merge --no-ff release-1.2`

- 删除预发布分支

	`git branch -d release-1.2`

bug修复分支
===

---

修补bug分支是从Master分支上面分出来的。修补结束以后，
再合并进Master和Develop分支。它的命名，可以采用fixbug-\*的形式。

- 创建一个bug修复分支
	
	`git checkout -b fixbug-0.1 master`

- 合并到master分支

	`git checkout master`

	`git merge --no-ff fixbug-0.1`

	`git tag -a 0.1.1`

- 合并到develop分支

	`git checkout develop`

	`git merge --no-ff fixbug-0.1`

- 删除bug修复分支

	`git branch -d fixbug-0.1`


Git 使用规范流程
---

---

新建分支
===

	# 获取主干最新代码

	git checkout master

	git pull

	# 创建一个开发分支

	git checkout -b develop

提交分支commit
===

	git add --all

	git statu

	git commit --verbose

	# verbose 参数会列出diff的结果

撰写提交信息
===

> Present-tense summary under 50 characters

>

> * More information about commit (under 72 characters).

> * More information about commit (under 72 characters).

> http://project.management-system.com/ticket/123

**第一行是不超过50个字的提要，然后空一行，罗列出改动原因、主要变动、
以及需要注意的问题。最后，提供对应的网址（比如Bug ticket）。**

合并commit
===

**合并到主干的时候，往往希望只有一个（或最多两三个）commit，这样不仅清晰
，也容易管理。**

	git rebase -i origin/master

推送到远程仓库
===

	git push --forch origin develop

	# git push 命令加上force参数，因为rebase后，分支历史改变，跟远程分支
	不一定兼容，有可能需要强行推送。


