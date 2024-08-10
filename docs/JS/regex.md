# pattern and match

## glob文件匹配


`*`

`**`


## catch group

## A Sequence of Characters (or String)

## OR Operator (|)

## Occurrence Indicators (or Repetition Operators):

- +: one or more (1+), e.g., [0-9]+ matches one or more digits such as '123', '000'.
- \*: zero or more (0+), e.g., [0-9]* matches zero or more digits. It accepts all those in [0-9]+ plus the empty string.
- ?: zero or one (optional), e.g., [+-]? matches an optional "+", "-", or an empty string.
- {m,n}: m to n (both inclusive)
- {m}: exactly m times
- {m,}: m or more (m+)


## Position Anchors: does not match character, but position such as start-of-line, end-of-line, start-of-word and end-of-word.

    ^, $: start-of-line and end-of-line respectively. E.g., ^[0-9]$ matches a numeric string.
    \b: boundary of word, i.e., start-of-word or end-of-word. E.g., \bcat\b matches the word "cat" in the input string.
    \B: Inverse of \b, i.e., non-start-of-word or non-end-of-word.
    \<, \>: start-of-word and end-of-word respectively, similar to \b. E.g., \<cat\> matches the word "cat" in the input string.
    \A, \Z: start-of-input and end-of-input respectively.



## shorthand character class

\d is short for [0-9]. \d includes all digits

\w stands for “word character”. It always matches the ASCII characters [A-Za-z0-9_]. Notice the inclusion of the underscore and digits.

\s stands for “whitespace character”.  it includes [ \t\r\n\f]. That is: \s matches a space, a tab, a carriage return, a line feed, or a form feed

## Negated  shorthand Character Classes

\D is the same as [^\d]  \W is short for [^\w] and \S is the equivalent of [^\s].





[^1]: [regex](https://www.regular-expressions.info/shorthand.html)
[^2]: [regex](https://www3.ntu.edu.sg/home/ehchua/programming/howto/Regexe.html#:~:text=A%20Regular%20Expression%20(or%20Regex,characters%2C%20metacharacters%20(such%20as%20.)
