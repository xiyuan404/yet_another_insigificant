

## 欢迎来到 **我的博客**！
![avatar](https://raw.githubusercontent.com/gearless-joeeee/yet_another_insigificant.../main/images/avatar.jpg)


**Ideas Worth Sharing** 这里主要记录我在写项目中的学习和思考

- [x] look up as you go

本项目受 [OI Wiki](https://oi-wiki.org) 的启发,在此致谢。
<!-- 
<div align="center">
<a href="https://www.netlify.com/" target="_blank" style="margin-left: 60px;"><img style="height: 40px; " src="images/netlify.png"></a>
</div> -->



#### Highlighting lines

``` py hl_lines="2 3"
def bubble_sort(items):
    for i in range(len(items)):
        for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```

## Comments
:smile: 



### codeblock text formating
``` title="formating"
{--deleted--}
{++added++}
{==Highlighting==}
{~~deleted~>added~~} 
{==
mnulti line
highlighting
==}
```
### inline text formating
- ==This was marked (highlight)==

- ^^This was inserted (underline)^^

- ~~This was deleted (strikethrough)~~
- H~2~O (subscript)
- A^T^A (superscript)
- ++ctrl+alt+del++ (keypress)