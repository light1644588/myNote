**正则表达式**

* 断言

  ######  边界类断言

  - ^  

    ```
    匹配输入的开头。如果多行模式设为 true，`^` 在换行符后也能立即匹配，比如 `/^A/` 匹配不了 "an A" 里面的 "A"，但是可以匹配 "An A" 里面第一个 "A"。
    ```

    

  - $  

    ```
    匹配输入的结束。如果多行模式设为 true，`^` 在换行符前也能立即匹配，比如 `/t$/` 不能匹配 "eater" 中的 "t"，但是可以匹配 "eat" 中的 "t"。
    ```

    

  - \b  

    ```markdown
    匹配一个单词的边界，这是一个字的字符前后没有另一个字的字符位置, 例如在字母和空格之间。需要注意的是匹配的单词边界不包括在匹配中。换句话说，匹配字边界的长度为零。
    
    一些例子:
    
    `/\bm/` 在 "moon" 中匹配到 "m" 
    
    `/oo\b/` 在 "moon" 中不会匹配到 "oo", 因为 "oo" 后面跟着 "n" 这个单词字符.
    
    `/oon\b/` 在 "moon" 中匹配 "oon"， 因为 "oon" 是这个字符串的结尾, 因此后面没有单词字符
    
    `/\w\b\w/` 将永远不会匹配任何东西，因为一个单词字符后面永远不会有非单词字符和单词字符。
    ```

    

  - \B    

    ```
    匹配非单词边界。这是上一个字符和下一个字符属于同一类型的位置：要么两者都必须是单词，要么两者都必须是非单词，例如在两个字母之间或两个空格之间。字符串的开头和结尾被视为非单词。与匹配的词边界相同，匹配的非词边界也不包含在匹配中。例如，`/\Bon/` 在 “at noon” 中匹配 “on” ，`/ye\B/` 在 "possibly yesterday"中匹配"ye" 。
    ```

    

  ###### 其他断言

  * x(?=y)  

    ```markdown
    **向前断言**: x 被 y 跟随时匹配 x。例如，对于/`Jack(?=Sprat)`/，“Jack”在跟有“Sprat”的情况下才会得到匹配．`/Jack(?=Sprat|Frost)/` “Jack”后跟有“Sprat”或“Frost”的情况下才会得到匹配。不过， 匹配结果不包括“Sprat”或“Frost”。
    ```

    

  * x(?!y)  

    ```markdown
    **向前断言:** x 被 y 跟随时匹配 x。例如，对于/`Jack(?=Sprat)`/，“Jack”在跟有“Sprat”的情况下才会得到匹配．`/Jack(?=Sprat|Frost)/` “Jack”后跟有“Sprat”或“Frost”的情况下才会得到匹配。不过， 匹配结果不包括“Sprat”或“Frost”。
    ```

    

  * （?<=y)x  

    ```markdown
    **向后断言:** x 跟随 y 的情况下匹配 x。例如，对于`/(?<=Jack)Sprat/`，“Sprat”紧随“Jack”时才会得到匹配。对于`/(?<=Jack|Tom)Sprat`，“Sprat”在紧随“Jack”或“Tom”的情况下才会得到匹配。不过，匹配结果中不包括“Jack”或“Tom”。
    ```

    

  * (?<!y)x  

    ```markdown
     **向后否定断言:** x 不跟随 y 时匹配 x。例如，对于`/(?<!-)\d+/`，数字不紧随-符号的情况下才会得到匹配。对于`/(?<!-)\d+/.exec(3)` ，“3”得到匹配。 而`/(?<!-)\d+/.exec(-3)`的结果无匹配，这是由于数字之前有-符号。
    ```

    

* 字符类

  - .  

    ```markdown
    有下列含义之一:
    
    - 匹配除行终止符之外的任何单个字符: `\n`, `\r`, `\u2028` or `\u2029`. 例如, `/.y/` 在“yes make my day”中匹配“my”和“ay”，而不是“yes”。
    - 在字符集内，点失去了它的特殊意义，并与文字点匹配。
    
    需要注意的是，`m` multiline标志不会改变点的行为。因此，要跨多行匹配一个模式，可以使用字符集`[^]`—它将匹配任何字符，包括新行。
    
    ES2018 添加了 `s` "dotAll" 标志,它允许点也匹配行终止符。
    ```

    

  - \d  

    ```markdown
    匹配任何数字(阿拉伯数字)。 相当于 `[0-9]`. 例如, `/\d/` 或 `/[0-9]/` 匹配 “B2is the suite number”中的“2”。
    ```

    

  - \D  

    ```markdown
     匹配任何非数字(阿拉伯数字)的字符。相当于`[^0-9]`. 例如, `/\D/` or `/[^0-9]/` 在 "B2 is the suite number" 中 匹配 "B".
    ```

    

  - \w   

    ```markdown
    匹配基本拉丁字母中的任何字母数字字符，包括下划线。相当于 `[A-Za-z0-9_]`. 例如, `/\w/` 在 "apple" 匹配 "a" , "5" in "$5.28", "3" in "3D" and "m" in "Émanuel".
    ```

    

  - \W   

    ```markdown
    匹配任何不是来自基本拉丁字母的单词字符。相当于 `[^A-Za-z0-9_]`. 例如, `/\W/` or `/[^A-Za-z0-9_]/` 匹配 "%" 在 "50%" 以及 "É" 在 "Émanuel" 中.
    ```

    

  - \s   

    ```markdown
    Matches a single white space character, including space, tab, form feed, line feed, and other Unicode spaces. Equivalent to `[ \f\n\r\t\v\u00a0\u1680\u2000-\u200a\u2028\u2029\u202f\u205f\u3000\ufeff]`. For example, `/\s\w*/` matches " bar" in "foo bar".
    ```

    

  - \S   

    ```markdown
    Matches a single character other than white space. Equivalent to `[^ \f\n\r\t\v\u00a0\u1680\u2000-\u200a\u2028\u2029\u202f\u205f\u3000\ufeff]`. For example, `/\S\w*/` matches "foo" in "foo bar".
    ```

    

  - \t

    

  - \r

    

  - \n

    

  - \v

    

  - f

    ```markdown
    匹配换页。
    ```

    

  - [\b]

    匹配一个退格键。如果您正在寻找单词边界字符（`\b`），请参见[边界](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Boundaries)。

  - \0

    ```markdown
    匹配一个NUL字符。不要在此后面加上其他数字。
    ```

    

  - \cX

    使用[插入符号](https://en.wikipedia.org/wiki/Caret_notation)匹配控制字符，其中“ X”是A–Z的字母（对应于代码点`U+0001`*–*`U+001F`）。例如，`/\cM/` 在“ \ r \ n”中匹配“ \ r”。

    

  - \xhh

    ```markdown
    将字符与代码匹配`*hh*`（两个十六进制数字）。
    ```

    

  - \uhhhh

    ```markdown
    将UTF-16代码单元与值`*hhhh*`（四个十六进制数字）匹配。
    ```

    

  - \u{hhhh}  \u{hhhhh}

    ```markdown
    （仅当`u`设置了标志时。）将字符与Unicode值或 （十六进制数字）匹配。`U+*hhhh*``U+*hhhhh*`
    ```

    

  - \

    ```markdown
    指示以下字符应被特殊对待或“转义”。它表现为两种方式之一。
    
    - 对于通常按字面意义处理的字符，表示下一个字符是特殊字符，不应按字面意义进行解释。例如，`/b/`匹配字符“ b”。通过在“ b”前面放置反斜杠，即使用`/\b/`，字符变得特殊以表示与单词边界匹配。
    - 对于通常被特殊对待的字符，指示下一个字符不是特殊字符，应按字面意义进行解释。例如，“ *”是一个特殊字符，表示应匹配0个或多个出现的前一个字符；例如，`/a*/`表示匹配0或多个“ a”。要从`*`字面上进行匹配，请在其前面加上反斜杠；例如，`/a\*/`匹配“ a *”。
    
    要从字面上匹配此字符，请自己将其转义。换句话说就是寻找`\` 使用`/\\/`。
    ```

    

* 组、范围

  - *x*|*y*

    ```markdown
    匹配 "x" 或 "y" 任意一个字符。例如， `/green|red/` 在 "green apple" 里匹配 "green"，且在 "red apple" 里匹配 "red" 。
    ```

    

  - [xyz]
    [a-c]

    ```markdown
    字符集。 匹配任何一个包含的字符。您可以使用连字符来指定字符范围，但如果连字符显示为方括号中的第一个或最后一个字符，则它将被视为作为普通字符包含在字符集中的文字连字符。也可以在字符集中包含字符类。
    
    例如, `[abcd]` 是与`[a-d]`.一样的，它们会 在"brisket" 中匹配 "b",在 "chop" 中匹配 "c" .
    
    例如, `[abcd-]` 和`[-abcd]` 将会在 "brisket" 匹配 "b" , 在 "chop" 匹配 "c" , 并且匹配 "non-profit" 中的 "-" (连字符)
    
    例如, `[\w-]` 是字符集 \w 和 “-”（连字符）的并集，与这种写法一样： `[A-Za-z0-9_-]`.。他们都会 在 "brisket"中匹配 “b”, 在 "chop"中匹配 “c”, 在 "non-profit" 中匹配 "n"。
    ```

    

  - [^xyz]

    [^a-c]

    一个否定的或被补充的字符集。也就是说，它匹配任何没有包含在括号中的字符。可以通过使用连字符来指定字符范围，但是如果连字符作为方括号中的第一个或最后一个字符出现，那么它将被视为作为普通字符包含在字符集中。例如，[^abc]和[^a-c]一样。它们最初匹配“bacon”中的“o”和“chop”中的“h”。

     ^ 字符也可以表示  [输入的起始](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Boundaries)

  - (*x*)

    ```markdown
    **捕获组:** 匹配x并记住匹配项。例如，/(foo)/匹配并记住“foo bar”中的“foo” 
    
    正则表达式可以有多个捕获组。结果，匹配通常在数组中捕获的组，该数组的成员与捕获组中左括号的顺序相同。这通常只是捕获组本身的顺序。当捕获组被嵌套时，这一点非常重要。使用结果元素的索引 (`[1], ..., [n]`) 或从预定义的 `RegExp` 对象的属性 (`$1, ..., $9`).
    
    捕获组会带来性能损失。如果不需要收回匹配的子字符串，请选择非捕获括号(见下面)。
    
    `String.match()` 不会返回组，如果设置了 `/.../g` 标志. 但是，您仍然可以使用 `String.matchAll()` to get all matches.
    
    match()不会返回组，如果/…但是，您仍然可以使用String.matchAll()来获取所有匹配项。
    ```

    

  - \\*n*

    其中n是一个正整数。对正则表达式中与n括号匹配的最后一个子字符串的反向引用(计算左括号)。例如，`/apple(,)\sorange\1/` 匹配 “apple，orange，cherry，peach” 中的 "apple，orange，"， 其中 `\1` 引用了 之前使用 `（）` 捕获的 `，`

    

  - (?<Name>x)

    **具名捕获组:** 匹配"x"并将其存储在返回的匹配项的groups属性中，该属性位于`<Name>`指定的名称下。尖括号(`<` 和 `>`) 用于组名。

    例如，使用正则 `/-(?<customName>\w)/` 匹配 “web-doc” 中的 “d”

    ```markdown
    'web-doc'.match(/-(?<customName>\w)/).groups  //{customName: "d"} 
    ```

  - (?:*x*)

    ```markdown
    **非捕获组:** 匹配 “x”，但不记得匹配。不能从结果数组的元素中收回匹配的子字符串(`[1], ..., [n]`) or from the predefined `RegExp` object's properties (`$1, ..., $9`).
    ```

    

* Unicode

  - 使用 `Unicode` 属性转义依靠 [`\u` 标识](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/unicode)，`\u` 表示该字符串被视为一串 Unicode 代码点。参考 `RegExp.prototype.nicode`.
  - 某些 `Unicode` 属性比[字符类](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Character_Classes)(如 `\w` 只匹配拉丁字母 `a` 到 `z`)包含更多的字符 ，但后者浏览器兼容性更好 (截至2020 一月).