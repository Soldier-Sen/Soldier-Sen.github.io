---
title: vim实用小技巧
date: 2019-05-14 00:10:23
tags: [vim]
categories: vim技巧
toc: true
---

# 查找当前选中的文本
可以使用`:h visual-search`查找帮助
使用如下脚本，可在可视模式下通过*,#对选中的文本进行查找
```
xnoremap * :<C-u>call<SID>VSetSearch()<CR>/<C-R>=@/<CR><CR>
xnoremap # :<C-u>call<SID>VSetSearch()<CR>?<C-R>=@/<CR><CR>
function s:VSetSearch()
let temp = @s
norm! gv"sy
let @/ = '\V' . substitute(escape(@s,  '/\'), '\n', '\\n', '\g')
let @s = temp
endfunction
```
# 结识substitute命令
语法: `:[range][substitute]/{pattern}/{string}/[flags]`
关于标志位，可查询`:h s_flags`
## 替换域中的特殊字符
<!-- more -->
|符号   |描述           |
|:---------:|:------------------:|
|\r       |插入一个换行符       |
|\n       |插入一个制表符       |
|\\       |插入一个反斜杠       |
|\1       |插入第1个子匹配      |
|\2       |插入第2个子匹配(以此类推，最多到9)      |
|\0       |插入匹配模式所有内容      |
|～       |使用上一次调用:substitute时的{string}      |
|\={vim stript} |执行{vim stript}表达式，并将返回的接轨作为替换{string} |

# 替换命令
```
：s/going/rolling          仅替换第一个匹配
：s/going/rolling/g        替换当前行所有匹配
：%s/going/rolling/g       替换当前文件所有匹配
：%s/going/rolling/gc      手动控制每一次替换操作
```
*替换命令中，如果查找域为空，则会重用上次的查找结果。*
*可以输入<C-r>/即可把上次的查找内容粘贴进来。
`:s/<C-r>//replace/g`

gv命令会激活可是模式，重新将上次被选中的文本高亮起来。

# vim C语言函数名高亮
在/usr/share/vim/vim80/syntax/c.vim中添加如下代码，设置高亮为蓝色：
syn match cFuntions display "[a-zA-Z_]\{-1,}\s\{-0,}(\{1}"ms=s,me=e-1
hi cFunctions gui=NONE cterm=bold  ctermfg=blue

### 如果当前光标处于一个括号上，使用%可以快速跳到其配对的括号上