# 正则表达式

regular expression

[常用正则表达式](https://www.cnblogs.com/dreamingbaobei/p/9717234.html)

[正则表达式总结](https://zhuanlan.zhihu.com/p/27653434)

[正则表达式w3c](https://www.w3school.com.cn/jsref/jsref_obj_regexp.asp)

字符串中可以用正则表达式的方法search replace match matchAll slice

RegExp对象方法：test，exec

修饰符

| i    | ignore - 不区分大小写                  | 将匹配设置为不区分大小写，搜索时不区分大小写: A 和 a 没有区别。 |
| ---- | -------------------------------------- | ------------------------------------------------------------ |
| g    | global - 全局匹配                      | 查找所有的匹配项。                                           |
| m    | multi line - 多行匹配                  | 使边界字符 **^** 和 **$** 匹配每一行的开头和结尾，记住是多行，而不是整个字符串的开头和结尾。 |
| s    | 特殊字符圆点 **.** 中包含换行符 **\n** | 默认情况下的圆点 **.** 是 匹配除换行符 **\n** 之外的任何字符，加上 **s** 修饰符之后, **.** 中包含换行符 \n。 |

## 一. 匹配字符

### 1. 模糊匹配

精确匹配

意义不大

```js
var regex = /hello/;
//测试hello111这个字符串中是否含有hello
console.log( regex.test("hello111") ); // true
```

**横向模糊匹配**

横向模糊指的是，一个正则可匹配的字符串的长度不是固定的，可以是多种情况的。

其实现的方式是使用量词。表示譬如{m,n}，表示连续出现最少m次，最多n次。

比如/ab{2,5}c/表示匹配这样一个字符串：第一个字符是“a”，接下来是2到5个字符“b”，最后是字符“c”。测试如下：

```js
var regex = /ab{2,5}c/g;
var string = "abc abbc abbbc abbbbc abbbbbc abbbbbbc";
console.log( string.match(regex) ); // ["abbc", "abbbc", "abbbbc", "abbbbbc"]
```

注意：案例中用的正则是/ab{2,5}c/g，后面多了g，它是正则的一个修饰符。表示全局匹配，即在目标字符串中按顺序找到满足匹配模式的所有子串，强调的是“所有”，而不只是“第一个”。g是单词global的首字母。

**纵向模糊匹配**

纵向模糊指的是，一个正则匹配的字符串，具体到某一位字符时，它可以不是某个确定的字符，可以有多种可能。

其实现的方式是使用字符组。譬如[abc]，表示该字符是可以字符“a”、“b”、“c”中的任何一个。

比如/a[123]b/可以匹配如下三种字符串："a1b"、"a2b"、"a3b"。测试如下：

```js
var regex = /a[123]b/g;
var string = "a0b a1b a2b a3b a4b";
console.log( string.match(regex) ); // ["a1b", "a2b", "a3b"]
```

### 2. 字符组

需要强调的是，虽叫字符组（字符类），但只是其中一个字符。例如[abc]，表示匹配一个字符，它可以是“a”、“b”、“c”之一。

**2.1 范围表示法**

如果字符组里的字符特别多的话，怎么办？可以使用范围表示法。

比如[123456abcdefGHIJKLM]，可以写成[1-6a-fG-M]。用连字符"-"来省略和简写。

因为连字符有特殊用途，那么要匹配“a”、“-”、“z”这三者中任意一个字符，该怎么做呢？

不能写成[a-z]，因为其表示小写字符中的任何一个字符。

可以写成如下的方式：[-az]或[az-]或`[a\-z]`。即要么放在开头，要么放在结尾，要么转义。总之不会让引擎认为是范围表示法就行了。

**2.2 排除字符组**

纵向模糊匹配，还有一种情形就是，某位字符可以是任何东西，但就不能是"a"、"b"、"c"。

此时就是排除字符组（反义字符组）的概念。例如`[^abc]`，表示是一个除"a"、"b"、"c"之外的任意一个字符。字符组的第一位放"^"（脱字符），表示求反的概念。

当然，也有相应的范围表示法。

**2.3 常见的简写形式**

有了字符组的概念后，一些常见的符号我们也就理解了。因为它们都是系统自带的简写形式。

> **\d** 就是[0-9]。表示是一位数字。记忆方式：其英文是digit（数字）。
>
> **\D** 就是`[^0-9]`。表示除数字外的任意字符。
>
> **\w** 就是[0-9a-zA-Z_]。表示数字、大小写字母和下划线。记忆方式：w是word的简写，也称单词字符。
>
> **\W** 就 是`[^0-9a-zA-Z_]`。非单词字符。
>
> **\s** 就是[ \t\v\n\r\f]。表示空白符，包括空格、水平制表符、垂直制表符、换行符、回车符、换页符。记忆方式：s是space character的首字母。
>
> **\S** 就是`[^ \t\v\n\r\f]`。 非空白符。
>
> **.** 就是`[^\n\r\u2028\u2029]`。通配符，表示几乎任意字符。换行符、回车符、行分隔符和段分隔符除外。记忆方式：想想省略号...中的每个点，都可以理解成占位符，表示任何类似的东西。

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

如果要匹配任意字符怎么办？可以使用[\d\D]、[\w\W]、[\s\S]和[^]中任何的一个。

```
var regex = /[abc]/g;
var string = "12a 1234 1234a 12345c";
console.log(string.match(regex) );//[ 'a', 'a', 'c' ]
```

### 3. 量词

量词也称重复。掌握{m,n}的准确含义后，只需要记住一些简写形式。

**3.1 简写形式**

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

**3.2 贪婪匹配和惰性匹配**

看如下的例子：

```text
var regex = /\d{2,5}/g;
var string = "123 1234 12345 123456";
console.log( string.match(regex) ); // ["123", "1234", "12345", "12345"]
```

其中正则/\d{2,5}/，表示数字连续出现2到5次。会匹配2位、3位、4位、5位连续数字。

但是其是贪婪的，它会尽可能多的匹配。你能给我6个，我就要5个。你能给我3个，我就3要个。反正只要在能力范围内，越多越好。

我们知道有时贪婪不是一件好事（请看文章最后一个例子）。而惰性匹配，就是尽可能少的匹配：



```js
var regex = /\d{2,5}?/g;
var string = "123 1234 12345 123456";
console.log( string.match(regex) ); // ["12", "12", "34", "12", "34", "12", "34", "56"]
```

其中/\d{2,5}?/表示，虽然2到5次都行，当2个就够的时候，就不在往下尝试了。

通过在量词后面加个问号就能实现惰性匹配，因此所有惰性匹配情形如下：

> **{m,n}?**
> **{m,}?**
> **??**
> **+?**
> ***?**

对惰性匹配的记忆方式是：量词后面加个问号，问一问你知足了吗，你很贪婪吗？

### 4. 多选分支

一个模式可以实现横向和纵向模糊匹配。而多选分支可以支持多个子模式任选其一。

具体形式如下：(p1|p2|p3)，其中p1、p2和p3是子模式，用“|”（管道符）分隔，表示其中任何之一。

例如要匹配"good"和"nice"可以使用/good|nice/。测试如下：

```js
var regex = /good|nice/g;
var string = "good idea, nice try.";
console.log( string.match(regex) ); // ["good", "nice"]
```

但有个事实我们应该注意，比如我用/good|goodbye/，去匹配"goodbye"字符串时，结果是"good"：

```js
var regex = /good|goodbye/g;
var string = "goodbye";
console.log( string.match(regex) ); // ["good"]
```

而把正则改成/goodbye|good/，结果是

```js
var regex = /goodbye|good/g;
var string = "goodbye";
console.log( string.match(regex) ); // ["goodbye"]
```

也就是说，分支结构也是惰性的，即当前面的匹配上了，后面的就不再尝试了。

### 5. 案例分析

匹配字符，无非就是字符组、量词和分支结构的组合使用罢了。

下面找几个例子演练一下（其中，每个正则并不是只有唯一写法）：

#### 5.1 匹配16进制颜色值

要求匹配：

> \#ffbbad
>
> \#Fc01DF
>
> \#FFF
>
> \#ffE

分析：

表示一个16进制字符，可以用字符组[0-9a-fA-F]。

其中字符可以出现3或6次，需要是用量词和分支结构。

使用分支结构时，需要注意顺序。

正则如下：

```js
var regex = /#([0-9a-fA-F]{6}|[0-9a-fA-F]{3})/g;
var string = "#ffbbad #Fc01DF #FFF #ffE";
console.log( string.match(regex) ); // ["#ffbbad", "#Fc01DF", "#FFF", "#ffE"]
```

#### 5.2 匹配时间

以24小时制为例。

要求匹配：

> 23:59
>
> 02:07

分析：

共4位数字，第一位数字可以为[0-2]。

当第1位为2时，第2位可以为[0-3]，其他情况时，第2位为[0-9]。

第3位数字为[0-5]，第4位为[0-9]

正则如下：

```js
//匹配以([01][0-9]|[2][0-3]):[0-5][0-9]开头，结尾的字符串，即只含匹配这个正则的字符串
var regex = /^([01][0-9]|[2][0-3]):[0-5][0-9]$/;
console.log( regex.test("23:59") ); // true
console.log( regex.test("02:07") ); // true
```

如果也要求匹配7:9，也就是说时分前面的0可以省略。

此时正则变成：

```js
var regex = /^(0?[0-9]|1[0-9]|[2][0-3]):(0?[0-9]|[1-5][0-9])$/;
console.log( regex.test("23:59") ); // true
console.log( regex.test("02:07") ); // true
console.log( regex.test("7:9") ); // true
```

#### 5.3 匹配日期

比如yyyy-mm-dd格式为例。

要求匹配：

> 2017-06-10

分析：

年，四位数字即可，可用[0-9]{4}。

月，共12个月，分两种情况01、02、……、09和10、11、12，可用(0[1-9]|1[0-2])。

日，最大31天，可用(0[1-9]|[12][0-9]|3[01])。

正则如下：

```js
var regex = /^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[12][0-9]|3[01])$/;
console.log( regex.test("2017-06-10") ); // true
```

## 二. 匹配位置

### 1. 位置图解

位置是相邻字符之间的位置。比如，下图中箭头所指的地方：

![img](https://picb.zhimg.com/80/v2-c487b9402a935625be4dd4b0e4f5fe5f_720w.png)*

### 2. 匹配字符

在ES5中，共有6个锚字符：

> ^ $ \b \B (?=p) (?!p)

#### 2.1 ^和$

^（脱字符）匹配开头，在多行匹配中匹配行开头。

$（美元符号）匹配结尾，在多行匹配中匹配行结尾。

比如我们把字符串的开头和结尾用"#"替换（位置可以替换成字符的！）：

```js
var result = "hello".replace(/^|$/g, '#');

console.log(result); // "#hello#"
```

多行匹配模式时，二者是行的概念，这个需要我们的注意：

```js
var result = "I\nlove\njavascript".replace(/^|$/gm, '#');

console.log(result);

/*

#I#

#love#

#javascript#

*/
```

#### 2.2 \b和\B

\b是单词边界，具体就是\w和\W之间的位置，也包括\w和^之间的位置，也包括\w和$之间的位置。

比如一个文件名是"[JS] Lesson_01.mp4"中的\b，如下：

```js
var result = "[JS] Lesson_01.mp4".replace(/\b/g, '#');

console.log(result); // "[#JS#] #Lesson_01#.#mp4#"
```

为什么是这样呢？这需要仔细看看。

首先，我们知道，\w是字符组[0-9a-zA-Z_]的简写形式，即\w是字母数字或者下划线的中任何一个字符。而\W是排除字符组[^0-9a-zA-Z_]的简写形式，即\W是\w以外的任何一个字符。

此时我们可以看看"[#JS#] #Lesson_01#.#mp4#"中的每一个"#"，是怎么来的。

第一个"#"，两边是"["与"J"，是\W和\w之间的位置。

第二个"#"，两边是"S"与"]"，也就是\w和\W之间的位置。

第三个"#"，两边是空格与"L"，也就是\W和\w之间的位置。

第四个"#"，两边是"1"与"."，也就是\w和\W之间的位置。

第五个"#"，两边是"."与"m"，也就是\W和\w之间的位置。

第六个"#"，其对应的位置是结尾，但其前面的字符"4"是\w，即\w和$之间的位置。

知道了\b的概念后，那么\B也就相对好理解了。



\B就是\b的反面的意思，非单词边界。例如在字符串中所有位置中，扣掉\b，剩下的都是\B的。

具体说来就是\w与\w、\W与\W、^与\W，\W与$之间的位置。

比如上面的例子，把所有\B替换成"#"：

```js
var result = "[JS] Lesson_01.mp4".replace(/\B/g, '#');

console.log(result); // "#[J#S]# L#e#s#s#o#n#_#0#1.m#p#4"
```

#### 2.3 (?=p)和(?!p)

(?=p)，其中p是一个子模式，即p前面的位置。

比如(?=l)，表示'l'字符前面的位置，例如：

```js
var result = "hello".replace(/(?=l)/g, '#');

console.log(result); // "he#l#lo"
```

而(?!p)就是(?=p)的反面意思，比如：

```js
var result = "hello".replace(/(?!l)/g, '#');

console.log(result); // "#h#ell#o#"
```

二者的学名分别是positive lookahead和negative lookahead。

中文翻译分别是正向先行断言和负向先行断言。

ES6中，还支持positive lookbehind和negative lookbehind。

具体是(?<=p)和(?<!p)。

也有书上把这四个东西，翻译成环视，即看看左边或看看右边。

但一般书上，没有很好强调这四者是个位置。

比如(?=p)，一般都理解成：要求接下来的字符与p匹配，但不能包括p的那些字符。

而在本人看来(?=p)就与^一样好理解，就是p前面的那个位置。

### 3. 位置的特性

对于位置的理解，我们可以理解成空字符""。

比如"hello"字符串等价于如下的形式：

```js
"hello" == "" + "h" + "" + "e" + "" + "l" + "" + "l" + "o" + "";
```

也等价于：

```js
"hello" == "" + "" + "hello"
```

因此，把/^hello$/写成/^^hello$$$/，是没有任何问题的：

```js
var result = /^^hello$$$/.test("hello");

console.log(result); // true
```

甚至可以写成更复杂的:

```js
var result = /(?=he)^^he(?=\w)llo$\b\b$/.test("hello");

console.log(result); // true
```

也就是说字符之间的位置，可以写成多个。

把位置理解空字符，是对位置非常有效的理解方式。

### 4. 相关案例

#### 4.1 不匹配任何东西的正则

让你写个正则不匹配任何东西

easy，/.^/

因为此正则要求只有一个字符，但该字符后面是开头。

#### 4.2 数字的千位分隔符表示法

比如把"12345678"，变成"12,345,678"。

可见是需要把相应的位置替换成","。

思路是什么呢？



**4.2.1 弄出最后一个逗号**

使用(?=\d{3}$)就可以做到：

```js
//三个数字结尾的前面加上','
var result = "12345678".replace(/(?=\d{3}$)/g, ',')

console.log(result); // "12345,678"
```

**4.2.2 弄出所有的逗号**

因为逗号出现的位置，要求后面3个数字一组，也就是\d{3}至少出现一次。

此时可以使用量词+：

```js
//以 至少包含一组（两组，三组）三个数字的前面 为结尾的字符串前面加上'，'，即结尾前三个数字，六个数字，九个数字的前  //面加上','
var result = "12345678".replace(/(?=(\d{3})+$)/g, ',')

console.log(result); // "12,345,678"
```

**4.2.3 匹配其余案例**

写完正则后，要多验证几个案例，此时我们会发现问题：

```js
var result = "123456789".replace(/(?=(\d{3})+$)/g, ',')

console.log(result); // ",123,456,789"
```

因为上面的正则，仅仅表示把从结尾向前数，一但是3的倍数，就把其前面的位置替换成逗号。因此才会出现这个问题。

怎么解决呢？我们要求匹配的到这个位置不能是开头。

我们知道匹配开头可以使用^，但要求这个位置不是开头怎么办？

easy，(?!^)，你想到了吗？测试如下：

```js
var string1 = "12345678",

string2 = "123456789";

reg = /(?!^)(?=(\d{3})+$)/g;

var result = string1.replace(reg, ',')

console.log(result); // "12,345,678"

result = string2.replace(reg, ',');

console.log(result); // "123,456,789"
```

**4.2.4 支持其他形式**

如果要把"12345678 123456789"替换成"12,345,678 123,456,789"。

此时我们需要修改正则，把里面的开头^和结尾$，替换成\b：

```js
var string = "12345678 123456789",

reg = /(?!\b)(?=(\d{3})+\b)/g;

var result = string.replace(reg, ',')

console.log(result); // "12,345,678 123,456,789"
```

其中(?!\b)怎么理解呢？

要求当前是一个位置，但不是\b前面的位置，其实(?!\b)说的就是\B。

**不在单词边界(\B) 且单词边界前(\b写在了后面，表示在单词边界前面) 至少一个\d{3} 前面的位置**

**因此最终正则变成了：/\B(?=(\d{3})+\b)/g**

#### 4.3 验证密码问题

密码长度6-12位，由数字、小写字符和大写字母组成，但必须至少包括2种字符。

此题，如果写成多个正则来判断，比较容易。但要写成一个正则就比较困难。

那么，我们就来挑战一下。看看我们对位置的理解是否深刻。

**4.3.1 简化**

不考虑“但必须至少包括2种字符”这一条件。我们可以容易写出：

```js
var reg = /^[0-9A-Za-z]{6,12}$/;
```

**4.3.2 判断是否包含有某一种字符**

假设，要求的必须包含数字，怎么办？此时我们可以使用(?=.*[0-9])来做。

因此正则变成：

```js
var reg = /(?=.*[0-9])^[0-9A-Za-z]{6,12}$/;
```

**对这个正则表达式的理解**

```
/(?=.*[0-9])^[0-9A-Za-z]{6,12}$/：一个位置且一个字符串

(?=.*[0-9])：一个位置，这个位置出现在"^"前面，表示这个位置也是开头的位置，这个位置处在任意字符后面带一个数字的前

面，即开头的位置后面必须有一个至少一个数字

```

官方解读：

```
分开来看就是(?=.*[0-9])和^。

表示开头前面还有个位置（当然也是开头，即同一个位置，想想之前的空字符类比）。

(?=.*[0-9])表示该位置后面的字符匹配.*[0-9]，即，有任何多个任意字符，后面再跟个数字。

翻译成大白话，就是接下来的字符，必须包含个数字。
```

**4.3.3 同时包含具体两种字符**

比如同时包含数字和小写字母，可以用`(?=.*[0-9])(?=.*[a-z])`来做。

因此正则变成：

```js
var reg = /(?=.*[0-9])(?=.*[a-z])^[0-9A-Za-z]{6,12}$/;
```

**4.3.4 解答**

我们可以把原题变成下列几种情况之一：

1.同时包含数字和小写字母

2.同时包含数字和大写字母

3.同时包含小写字母和大写字母

4.同时包含数字、小写字母和大写字母

以上的4种情况是或的关系（实际上，可以不用第4条）。

最终答案是：

```js
var reg = /((?=.*[0-9])(?=.*[a-z])|(?=.*[0-9])(?=.*[A-Z])|(?=.*[a-z])(?=.*[A-Z]))^[0-9A-Za-z]{6,12}$/;

console.log( reg.test("1234567") ); // false 全是数字

console.log( reg.test("abcdef") ); // false 全是小写字母

console.log( reg.test("ABCDEFGH") ); // false 全是大写字母

console.log( reg.test("ab23C") ); // false 不足6位

console.log( reg.test("ABCDEF234") ); // true 大写字母和数字

console.log( reg.test("abcdEF234") ); // true 三者都有
```

**4.3.6 另外一种解法**

“至少包含两种字符”的意思就是说，不能全部都是数字，也不能全部都是小写字母，也不能全部都是大写字母。

那么要求“不能全部都是数字”，怎么做呢？(?!p)出马！

对应的正则是：

```js
var reg = /(?!^[0-9]{6,12}$)^[0-9A-Za-z]{6,12}$/;
```

三种“都不能”呢？

最终答案是：

```js
var reg = /(?!^[0-9]{6,12}$)(?!^[a-z]{6,12}$)(?!^[A-Z]{6,12}$)^[0-9A-Za-z]{6,12}$/;

console.log( reg.test("1234567") ); // false 全是数字

console.log( reg.test("abcdef") ); // false 全是小写字母

console.log( reg.test("ABCDEFGH") ); // false 全是大写字母

console.log( reg.test("ab23C") ); // false 不足6位

console.log( reg.test("ABCDEF234") ); // true 大写字母和数字

console.log( reg.test("abcdEF234") ); // true 三者都有
```

## 三. 括号的作用

### 1. 分组和分支结构

这二者是括号最直觉的作用，也是最原始的功能。

**1.1 分组**

我们知道/a+/匹配连续出现的“a”，而要匹配连续出现的“ab”时，需要使用/(ab)+/。

其中括号是提供分组功能，使量词“+”作用于“ab”这个整体，测试如下：

```js
var regex = /(ab)+/g;
var string = "ababa abbb ababab";
console.log( string.match(regex) ); // ["abab", "ab", "ababab"]
```

**1.2 分支结构**

而在多选分支结构(p1|p2)中，此处括号的作用也是不言而喻的，提供了子表达式的所有可能。

比如，要匹配如下的字符串：

> I love JavaScript
>
> I love Regular Expression

可以使用正则：

```js
var regex = /^I love (JavaScript|Regular Expression)$/;
console.log( regex.test("I love JavaScript") ); // true
console.log( regex.test("I love Regular Expression") ); // true
```

如果去掉正则中的括号，即/^I love JavaScript|Regular Expression$/，匹配字符串是"I love JavaScript"和"Regular Expression"，当然这不是我们想要的。

### 2. 引用分组

这是括号一个重要的作用，有了它，我们就可以进行数据提取，以及更强大的替换操作。

而要使用它带来的好处，必须配合使用实现环境的API。

以日期为例。假设格式是yyyy-mm-dd的，我们可以先写一个简单的正则：

```js
var regex = /\d{4}-\d{2}-\d{2}/;
```

然后再修改成括号版的：

```js
var regex = /(\d{4})-(\d{2})-(\d{2})/;
```

为什么要使用这个正则呢？

**2.1 提取数据**

比如提取出年、月、日，可以这么做：

```js
var regex = /(\d{4})-(\d{2})-(\d{2})/;
var string = "2017-06-12";
console.log( string.match(regex) ); 
// => ["2017-06-12", "2017", "06", "12", index: 0, input: "2017-06-12"]
```

match返回的一个数组，第一个元素是整体匹配结果，然后是各个分组（括号里）匹配的内容，然后是匹配下标，最后是输入的文本。（注意：如果正则是否有修饰符g，match返回的数组格式是不一样的）。

另外也可以使用正则对象的exec方法：

```js
var regex = /(\d{4})-(\d{2})-(\d{2})/;
var string = "2017-06-12";
console.log( regex.exec(string) ); 
// => ["2017-06-12", "2017", "06", "12", index: 0, input: "2017-06-12"]
```

同时，也可以使用构造函数的全局属性$1至$9来获取：

```js
var regex = /(\d{4})-(\d{2})-(\d{2})/;
var string = "2017-06-12";

regex.test(string); // 正则操作即可，例如
//regex.exec(string);
//string.match(regex);
//$1表示第一个括号内的内容, $2表示第二个括号内的内容, $3...
console.log(RegExp.$1); // "2017"
console.log(RegExp.$2); // "06"
console.log(RegExp.$3); // "12"
```

**2.2 替换**

比如，想把yyyy-mm-dd格式，替换成mm/dd/yyyy怎么做？

```js
var regex = /(\d{4})-(\d{2})-(\d{2})/;
var string = "2017-06-12";
var result = string.replace(regex, "$2/$3/$1");
console.log(result); // "06/12/2017"
```

其中replace中的，第二个参数里用$1、$2、$3指代相应的分组。等价于如下的形式：

```js
var regex = /(\d{4})-(\d{2})-(\d{2})/;
var string = "2017-06-12";
var result = string.replace(regex, function() {
	return RegExp.$2 + "/" + RegExp.$3 + "/" + RegExp.$1;
});
console.log(result); // "06/12/2017"
```

也等价于：

```js
var regex = /(\d{4})-(\d{2})-(\d{2})/;
var string = "2017-06-12";
//函数的四个参数，来自regex吗
var result = string.replace(regex, function(match, year, month, day) {
	return month + "/" + day + "/" + year;
});
console.log(result); // "06/12/2017"
```

### 3. 反向引用

除了使用相应API来引用分组，也可以在正则本身里引用分组。但只能引用之前出现的分组，即反向引用。

还是以日期为例。

比如要写一个正则支持匹配如下三种格式：

> 2016-06-12
>
> 2016/06/12
>
> 2016.06.12

最先可能想到的正则是:

```js
var regex = /\d{4}(-|\/|\.)\d{2}(-|\/|\.)\d{2}/;
var string1 = "2017-06-12";
var string2 = "2017/06/12";
var string3 = "2017.06.12";
var string4 = "2016-06/12";
console.log( regex.test(string1) ); // true
console.log( regex.test(string2) ); // true
console.log( regex.test(string3) ); // true
console.log( regex.test(string4) ); // true
```

其中/和.需要转义。虽然匹配了要求的情况，但也匹配"2016-06/12"这样的数据。

假设我们想要求分割符前后一致怎么办？此时需要使用**反向引用**：

```js
var regex = /\d{4}(-|\/|\.)\d{2}\1\d{2}/;
var string1 = "2017-06-12";
var string2 = "2017/06/12";
var string3 = "2017.06.12";
var string4 = "2016-06/12";
console.log( regex.test(string1) ); // true
console.log( regex.test(string2) ); // true
console.log( regex.test(string3) ); // true
console.log( regex.test(string4) ); // false
```

注意里面的\1，表示的引用之前的那个分组(-|\/|\.)。不管它匹配到什么（比如-），\1都匹配那个同样的具体某个字符。

我们知道了\1的含义后，那么\2和\3的概念也就理解了，即分别指代第二个和第三个分组。

看到这里，此时，恐怕你会有三个问题。

**3.1 括号嵌套怎么办？**

以左括号（开括号）为准。比如：

```js
var regex = /^((\d)(\d(\d)))\1\2\3\4$/;
var string = "1231231233";
console.log( regex.test(string) ); // true
console.log( RegExp.$1 ); // 123
console.log( RegExp.$2 ); // 1
console.log( RegExp.$3 ); // 23
console.log( RegExp.$4 ); // 3
```

我们可以看看这个正则匹配模式：

第一个字符是数字，比如说1，

第二个字符是数字，比如说2，

第三个字符是数字，比如说3，

接下来的是\1，是第一个分组内容，那么看第一个开括号对应的分组是什么，是123，

接下来的是\2，找到第2个开括号，对应的分组，匹配的内容是1，

接下来的是\3，找到第3个开括号，对应的分组，匹配的内容是23，

最后的是\4，找到第3个开括号，对应的分组，匹配的内容是3。

这个问题，估计仔细看一下，就该明白了。

**3.2 \10表示什么呢？**

另外一个疑问可能是，即\10是表示第10个分组，还是\1和0呢？答案是前者，虽然一个正则里出现\10比较罕见。测试如下：

```js
var regex = /(1)(2)(3)(4)(5)(6)(7)(8)(9)(#) \10+/;
var string = "123456789# ######"
console.log( regex.test(string) );
```

### 4. 非捕获分组

之前文中出现的分组，都会捕获它们匹配到的数据，以便后续引用，因此也称他们是捕获型分组。

如果只想要括号最原始的功能，但不会引用它，即，既不在API里引用，也不在正则里反向引用。此时可以使用非捕获分组(?:p)，例如本文第一个例子可以修改为：

```js
var regex = /(?:ab)+/g;
var string = "ababa abbb ababab";
console.log( string.match(regex) ); // ["abab", "ab", "ababab"]
```

### 5. 相关案例

#### 5.1 字符串trim方法模拟

trim方法是去掉字符串的开头和结尾的空白符。有两种思路去做。

第一种，匹配到开头和结尾的空白符，然后替换成空字符。如：

```js
function trim(str) {
	return str.replace(/^\s+|\s+$/g, '');
}
console.log( trim("  foobar   ") ); // "foobar"
```

#### 5.2 将每个单词的首字母转换为大写

```js
function titleize(str) {
    //非捕获分组，开头或者空白后面的第一个单词字符，参数c大概是匹配到的字符
	return str.toLowerCase().replace(/(?:^|\s)\w/g, function(c) {
		return c.toUpperCase();
	});
}
console.log( titleize('my name is epeli') ); // "My Name Is Epeli"
```

思路是找到每个单词的首字母，当然这里不使用非捕获匹配也是可以的。

#### 5.3 驼峰下划线互转

```js
/*下划线转驼峰*/
let str = 'good_name_school'
//可传两个参数，str：匹配到的字符串， index：索引
str = str.replace(/_([a-z])/g, str => str.substr(-1).toUpperCase())
console.log(str)
```

```js
/*驼峰转下划线*/
const str2 = 'goodNamePerson';
const regExp2 = /[A-Z]/g;
console.log(str2.replace(regExp2, str => '-' + str.toLowerCase()))
```

