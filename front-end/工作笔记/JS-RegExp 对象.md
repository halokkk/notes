

# JS

### JavaScript replace() 方法

https://www.w3school.com.cn/jsref/jsref_replace.asp



语法：

```
str.replace(regexp|substr, newSubStr|function)
```

参数：

| 参数                          | 描述                                                         |
| :---------------------------- | :----------------------------------------------------------- |
| `regexp `**(pattern)**        | 一个[`RegExp`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/RegExp) 对象或者其字面量。该正则所匹配的内容会被第二个参数的返回值替换掉。 |
| `substr `**(pattern)**        | 一个将被 `newSubStr` 替换的 [`字符串`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/String)。其被视为一整个字符串，而不是一个正则表达式。<u>仅第一个匹配项会被替换</u>。 |
| `newSubStr` **(replacement)** | 用于替换掉第一个参数在原字符串中的匹配部分的[`字符串`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/String)。该字符串中可以内插一些特殊的变量名。参考下面的[使用字符串作为参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace#使用字符串作为参数)。 |
| `function` **(replacement)**  | 一个用来创建新子字符串的函数，该函数的返回值将替换掉第一个参数匹配到的结果。参考下面的[指定一个函数作为参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace#指定一个函数作为参数)。 |

示例：

```
a.replace(/<img/gi, '<img style="max-width: 100%;"');
```



### JavaScript RegExp 对象

https://www.w3school.com.cn/jsref/jsref_obj_regexp.asp

语法：

```
/pattern/attributes
```

参数

| *pattern*    | 一个字符串，指定了正则表达式的模式或其他正则表达式。         |
| ------------ | ------------------------------------------------------------ |
| *attributes* | 是一个可选的字符串。g-全局匹配；i-区分大小写；m多行匹配；。ECMAScript 标准化之前，不支持 m 属性。如果 *pattern* 是正则表达式，而不是字符串，则必须省略该参数。 |



### 返回值

一个新的 RegExp 对象，具有指定的模式和标志。如果参数 *pattern* 是正则表达式而不是字符串，那么 RegExp() 构造函数将用与指定的 RegExp 相同的模式和标志创建一个新的 RegExp 对象。

如果不用 new 运算符，而将 RegExp() 作为函数调用，那么它的行为与用 new 运算符调用时一样，只是当 *pattern* 是正则表达式时，它只返回 *pattern*，而不再创建一个新的 RegExp 对象。

### 抛出

SyntaxError - 如果 *pattern* 不是合法的正则表达式，或 *attributes* 含有 "g"、"i" 和 "m" 之外的字符，抛出该异常。

TypeError - 如果 *pattern* 是 RegExp 对象，但没有省略 *attributes* 参数，抛出该异常。

## 修饰符

| 修饰符                                                    | 描述                                                     |
| :-------------------------------------------------------- | :------------------------------------------------------- |
| [i](https://www.w3school.com.cn/jsref/jsref_regexp_i.asp) | 执行对大小写不敏感的匹配。                               |
| [g](https://www.w3school.com.cn/jsref/jsref_regexp_g.asp) | 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。 |
| m                                                         | 执行多行匹配。                                           |

## 方括号

方括号用于查找某个范围内的字符：

| 表达式                                                       | 描述                               |
| :----------------------------------------------------------- | :--------------------------------- |
| [[abc\]](https://www.w3school.com.cn/jsref/jsref_regexp_charset.asp) | 查找方括号之间的任何字符。         |
| [[^abc\]](https://www.w3school.com.cn/jsref/jsref_regexp_charset_not.asp) | 查找任何不在方括号之间的字符。     |
| [0-9]                                                        | 查找任何从 0 至 9 的数字。         |
| [a-z]                                                        | 查找任何从小写 a 到小写 z 的字符。 |
| [A-Z]                                                        | 查找任何从大写 A 到大写 Z 的字符。 |
| [A-z]                                                        | 查找任何从大写 A 到小写 z 的字符。 |
| [adgk]                                                       | 查找给定集合内的任何字符。         |
| [^adgk]                                                      | 查找给定集合外的任何字符。         |
| (red\|blue\|green)                                           | 查找任何指定的选项。               |

## 元字符

元字符（Metacharacter）是拥有特殊含义的字符：

| 元字符                                                       | 描述                                        |
| :----------------------------------------------------------- | :------------------------------------------ |
| [.](https://www.w3school.com.cn/jsref/jsref_regexp_dot.asp)  | 查找单个字符，除了换行和行结束符。          |
| [\w](https://www.w3school.com.cn/jsref/jsref_regexp_wordchar.asp) | 查找单词字符。                              |
| [\W](https://www.w3school.com.cn/jsref/jsref_regexp_wordchar_non.asp) | 查找非单词字符。                            |
| [\d](https://www.w3school.com.cn/jsref/jsref_regexp_digit.asp) | 查找数字。                                  |
| [\D](https://www.w3school.com.cn/jsref/jsref_regexp_digit_non.asp) | 查找非数字字符。                            |
| [\s](https://www.w3school.com.cn/jsref/jsref_regexp_whitespace.asp) | 查找空白字符。                              |
| [\S](https://www.w3school.com.cn/jsref/jsref_regexp_whitespace_non.asp) | 查找非空白字符。                            |
| [\b](https://www.w3school.com.cn/jsref/jsref_regexp_begin.asp) | 匹配单词边界。                              |
| [\B](https://www.w3school.com.cn/jsref/jsref_regexp_begin_not.asp) | 匹配非单词边界。                            |
| \0                                                           | 查找 NUL 字符。                             |
| [\n](https://www.w3school.com.cn/jsref/jsref_regexp_newline.asp) | 查找换行符。                                |
| \f                                                           | 查找换页符。                                |
| \r                                                           | 查找回车符。                                |
| \t                                                           | 查找制表符。                                |
| \v                                                           | 查找垂直制表符。                            |
| [\xxx](https://www.w3school.com.cn/jsref/jsref_regexp_octal.asp) | 查找以八进制数 xxx 规定的字符。             |
| [\xdd](https://www.w3school.com.cn/jsref/jsref_regexp_hex.asp) | 查找以十六进制数 dd 规定的字符。            |
| [\uxxxx](https://www.w3school.com.cn/jsref/jsref_regexp_unicode_hex.asp) | 查找以十六进制数 xxxx 规定的 Unicode 字符。 |

## 量词

| 量词                                                         | 描述                                        |
| :----------------------------------------------------------- | :------------------------------------------ |
| [n+](https://www.w3school.com.cn/jsref/jsref_regexp_onemore.asp) | 匹配任何包含至少一个 n 的字符串。           |
| [n*](https://www.w3school.com.cn/jsref/jsref_regexp_zeromore.asp) | 匹配任何包含零个或多个 n 的字符串。         |
| [n?](https://www.w3school.com.cn/jsref/jsref_regexp_zeroone.asp) | 匹配任何包含零个或一个 n 的字符串。         |
| [n{X}](https://www.w3school.com.cn/jsref/jsref_regexp_nx.asp) | 匹配包含 X 个 n 的序列的字符串。            |
| [n{X,Y}](https://www.w3school.com.cn/jsref/jsref_regexp_nxy.asp) | 匹配包含 X 至 Y 个 n 的序列的字符串。       |
| [n{X,}](https://www.w3school.com.cn/jsref/jsref_regexp_nxcomma.asp) | 匹配包含至少 X 个 n 的序列的字符串。        |
| [n$](https://www.w3school.com.cn/jsref/jsref_regexp_ndollar.asp) | 匹配任何结尾为 n 的字符串。                 |
| [^n](https://www.w3school.com.cn/jsref/jsref_regexp_ncaret.asp) | 匹配任何开头为 n 的字符串。                 |
| [?=n](https://www.w3school.com.cn/jsref/jsref_regexp_nfollow.asp) | 匹配任何其后紧接指定字符串 n 的字符串。     |
| [?!n](https://www.w3school.com.cn/jsref/jsref_regexp_nfollow_not.asp) | 匹配任何其后没有紧接指定字符串 n 的字符串。 |

## RegExp 对象属性

| 属性                                                         | 描述                                     | FF   | IE   |
| :----------------------------------------------------------- | :--------------------------------------- | :--- | :--- |
| [global](https://www.w3school.com.cn/jsref/jsref_regexp_global.asp) | RegExp 对象是否具有标志 g。              | 1    | 4    |
| [ignoreCase](https://www.w3school.com.cn/jsref/jsref_regexp_ignorecase.asp) | RegExp 对象是否具有标志 i。              | 1    | 4    |
| [lastIndex](https://www.w3school.com.cn/jsref/jsref_lastindex_regexp.asp) | 一个整数，标示开始下一次匹配的字符位置。 | 1    | 4    |
| [multiline](https://www.w3school.com.cn/jsref/jsref_multiline_regexp.asp) | RegExp 对象是否具有标志 m。              | 1    | 4    |
| [source](https://www.w3school.com.cn/jsref/jsref_source_regexp.asp) | 正则表达式的源文本。                     | 1    | 4    |

## RegExp 对象方法

| 方法                                                         | 描述                                               | FF   | IE   |
| :----------------------------------------------------------- | :------------------------------------------------- | :--- | :--- |
| [compile](https://www.w3school.com.cn/jsref/jsref_regexp_compile.asp) | 编译正则表达式。                                   | 1    | 4    |
| [exec](https://www.w3school.com.cn/jsref/jsref_exec_regexp.asp) | 检索字符串中指定的值。返回找到的值，并确定其位置。 | 1    | 4    |
| [test](https://www.w3school.com.cn/jsref/jsref_test_regexp.asp) | 检索字符串中指定的值。返回 true 或 false。         | 1    | 4    |

## 支持正则表达式的 String 对象的方法

| 方法                                                         | 描述                             | FF   | IE   |
| :----------------------------------------------------------- | :------------------------------- | :--- | :--- |
| [search](https://www.w3school.com.cn/jsref/jsref_search.asp) | 检索与正则表达式相匹配的值。     | 1    | 4    |
| [match](https://www.w3school.com.cn/jsref/jsref_match.asp)   | 找到一个或多个正则表达式的匹配。 | 1    | 4    |
| [replace](https://www.w3school.com.cn/jsref/jsref_replace.asp) | 替换与正则表达式匹配的子串。     | 1    | 4    |
| [split](https://www.w3school.com.cn/jsref/jsref_split.asp)   | 把字符串分割为字符串数组。       | 1    | 4    |