---
layout: post
title: Linux 连续执行多条命令的方法【整理稿】
tags: [nodejs]
---

>多个命令可以放在一行上，其执行情况得依赖于用在命令之间的分隔符。

### 如果每个命令被一个分号 (;) 所分隔，

那么命令会连续的执行下去，如：引用


	beyes@linux-beyes:/proc> printf "%s/n" "This is executed" ; printf "%s/n" "And so is this"
	This is executed
	And so is this


### 如果每个命令被 && 号分隔，
那么这些命令会一直执行下去，如果中间有错误的命令存在，则不再执行后面的命令，没错则执行到完为止：引用

	beyes@linux-beyes:/proc> date && printf "%s/n" "The date command was successful"
	2009年 08月 28日 星期五 18:28:16 CST
	The date command was successful

所有命令成功执行完毕。引用

	beyes@linux-beyes:/proc> date && llk && printf "%s/n" "The date command was successful"
	2009年 08月 28日 星期五 18:28:52 CST
	bash: llk: command not found

后面的成功执行提示语句不会被输出，因为 llk 命令无法识别。

### 如果每个命令被双竖线(||)分隔符分隔，
如果命令遇到可以成功执行的命令，那么命令停止执行，即使后面还有正确的命令则后面的所有命令都将得不到执行。假如命令一开始就执行失败，那么就会执行 || 后的下一个命令，直到遇到有可以成功执行的命令为止，假如所有的都失败，则所有这些失败的命令都会被尝试执行一次：引用

	beyes@linux-beyes:/proc> date || ls / || date 'duck!' || uname -a
	2009年 08月 28日 星期五 18:33:18 CST

第一个命令成功执行！后面的所有命令不再得到执行。引用

	beyes@linux-beyes:/proc> date 'duck!' || dakkk || uname -a
	date: 无效的日期 “duck!”
	bash: dakkk: command not found
	Linux linux-beyes 2.6.27.29-0.1-pae #1 SMP 2009-08-15 17:53:59 +0200 i686 i686 i386 GNU/Linux

前面的两个命令都失败了，直到找到最后一个可以成功执行的命令为止。