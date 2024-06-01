---
icon: material/graph-outline
status: new
subtitle: Nullam urna elit, malesuada eget finibus ut, ac tortor
---

## time formtet

```js
const today = new Date().toLocaleDateString("en-US", {
  weekday: "long",
  year: "numeric",
  month: "long",
  day: "2-digit",
});
console.log(today);
```

_all options_
```js
{
  weekday: 'narrow' | 'short' | 'long',
  era: 'narrow' | 'short' | 'long',
  year: 'numeric' | '2-digit',
  month: 'numeric' | '2-digit' | 'narrow' | 'short' | 'long',
  day: 'numeric' | '2-digit',
  hour: 'numeric' | '2-digit',
  minute: 'numeric' | '2-digit',
  second: 'numeric' | '2-digit',
  timeZoneName: 'shortOffset' | 'shortGeneric' | 'longOffset' | 'longGeneric',
  // Time zone to express it in
  timeZone: 'Asia/Shanghai',
  // Force 12-hour or 24-hour
  hour12: true | false,

  // Rarely-used options
  hourCycle: 'h11' | 'h12' | 'h23' | 'h24',
  formatMatcher: 'basic' | 'best fit'
}
```

```js
console.log(
  new Intl.DateTimeFormat("en-US", {
    weekday: "long",
    year: "numeric",
    month: "short",
    day: "2-digit",
    timeZone: "Asia/Shanghai",
    timeZoneName: "shortOffset",
  }).format(new Date())
);

```

## code blocks
_Embedding external files_
=== ":material-file-code: markdown" 
    !!! info inline end "react"
        raw code
    ````markdown
      ``` title=".browserslistrc"
        ;--8<-- ".browserslistrc"
      ```
    ````



``` py title="bubble_sort.py" linenums="1" hl_lines="3-5"
def bubble_sort(items):
    for i in range(len(items)):
        for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```


[Send :fontawesome-solid-paper-plane:](#){ .md-button .md-button--primary }


???+ note "[CF1200E Compress Words](http://codeforces.com/contest/1200/problem/E)"
    给你若干个字符串，答案串初始为空。第 $i$ 步将第 $i$ 个字符串加到答案串的后面，但是尽量地去掉重复部分（即去掉一个最长的、是原答案串的后缀、也是第 $i$ 个串的前缀的字符串），求最后得到的字符串。
    
    ??? note "题解"
        每次需要求最长的、是原答案串的后缀、也是第 $i$ 个串的前缀的字符串。枚举这个串的长度，哈希比较即可。
        
        当然，这道题也可以使用 [KMP 算法](./kmp.md) 解决。
    
    ??? note "代码实现"
        ```cpp
        --8<-- "docs/javascript/string/code/hash.cpp"
        ```
### Inline blocks

Admonitions can also be rendered as inline blocks (e.g., for sidebars), placing
them to the right using the `inline` + `end` modifiers, or to the left using
only the `inline` modifier:

=== ":fontawesome-brands-react: react"
    ``` markdown
        raw
    ```
    Use `inline end` to align to the right (left for rtl languages).
=== ":fontawesome-brands-vuejs: vue"
    ``` markdown
        raw
    ```
=== ":material-language-javascript: js"
  
    !!! info inline "title"

        preview
  

__Important__: admonitions that use the `inline` modifiers _must_ be declared
prior to the content block you want to place them beside. If there's
insufficient space to render the admonition next to the block, the admonition
will stretch to the full width of the viewport, e.g., on mobile viewports.
