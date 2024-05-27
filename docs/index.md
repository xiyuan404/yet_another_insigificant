

## 欢迎来到 **我的博客**！
![avatar](https://raw.githubusercontent.com/gearless-joeeee/yet_another_insigificant.../main/images/avatar.jpg)


**Ideas Worth Sharing** 这里主要记录我在写项目中的学习和思考

- [x] look up as you go

本项目受 [OI Wiki](https://oi-wiki.org) 的启发,在此致谢。
<!-- 
<div align="center">
<a href="https://www.netlify.com/" target="_blank" style="margin-left: 60px;"><img style="height: 40px; " src="images/netlify.png"></a>
</div> -->

## Code Annotation Examples



#### Code for a specific language

Some more code with the `py` at the start:

``` py
import tensorflow as tf
def whatever()
```

#### With a title

``` py title="bubble_sort.py"
def bubble_sort(items):
    for i in range(len(items)):
        for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```

#### With line numbers

``` py linenums="1"
def bubble_sort(items):
    for i in range(len(items)):
        for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```

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


## code block
### 例题

???+ note "[CF1200E Compress Words](http://codeforces.com/contest/1200/problem/E)"
    给你若干个字符串，答案串初始为空。第 $i$ 步将第 $i$ 个字符串加到答案串的后面，但是尽量地去掉重复部分（即去掉一个最长的、是原答案串的后缀、也是第 $i$ 个串的前缀的字符串），求最后得到的字符串。
    
    ??? note "题解"
        每次需要求最长的、是原答案串的后缀、也是第 $i$ 个串的前缀的字符串。枚举这个串的长度，哈希比较即可。
        
        当然，这道题也可以使用 [KMP 算法](./kmp.md) 解决。
    
    ??? note "参考代码"
        ```cpp
        --8<-- "docs/javascript/string/code/hash.cpp"
        ```
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