# 正则表达式
Forked From https://github.com/hiwyw/linux-learning/edit/master/regular/regular-expression.md

正则表达式(regular expression)描述了一种字符串匹配的模式（pattern），可以用来检查一个串是否含有某种子串、将匹配的子串替换或者从某个串中取出符合某个条件的子串等 
## 语法
**普通字符**
普通字符包括没有显式指定为元字符的所有可打印和不可打印字符。这包括所有大写和小写字母、所有数字、所有标点符号和一些其他符号。
**非打印字符**
|字符|描述|
|---|---|
|`\cx`|匹配由x指明的控制字符。例如， \cM 匹配一个 Control-M 或回车符。x 的值必须为 A-Z 或 a-z 之一。否则，将 c 视为一个原义的 'c' 字符。|
|`\f`|匹配一个换页符。等价于 \x0c 和 \cL。|
|`\n`|匹配一个换行符。等价于 \x0a 和 \cJ。|
|`\r`|匹配一个回车符。等价于 \x0d 和 \cM。|
|`\s`|匹配任何空白字符，包括空格、制表符、换页符等等。等价于 [ \f\n\r\t\v]。注意 Unicode 正则表达式会匹配全角空格符。|
|`\S`|匹配任何非空白字符。等价于 [^ \f\n\r\t\v]。|
|`\t`|匹配一个制表符。等价于 \x09 和 \cI。|
|`\v`|匹配一个垂直制表符。等价于 \x0b 和 \cK。|
**特殊字符**
所谓特殊字符，就是一些有特殊含义的字符，如上面说的 `runoo*b` 中的 `*`，简单的说就是表示任何字符串的意思。如果要查找字符串中的 `*` 符号，则需要对 `*` 进行转义，即在其前加一个 `\: runo\*ob` 匹配 `runo*ob`。

许多元字符要求在试图匹配它们时特别对待。若要匹配这些特殊字符，必须首先使字符"转义"，即，将反斜杠字符\ 放在它们前面。下表列出了正则表达式中的特殊字符：
|特别字符|描述|
|---|---|
|`$`|匹配输入字符串的结尾位置。如果设置了`RegExp`对象的`Multiline`属性，则 `$` 也匹配 `'\n'` 或 `'\r'`。要匹配 `$` 字符本身，请使用 `\$`。|
|`( )`|标记一个子表达式的开始和结束位置。子表达式可以获取供以后使用。要匹配这些字符，请使用 `\(` 和 `\)`。|
|`*`|匹配前面的子表达式零次或多次。要匹配 `*` 字符，请使用 `\*`。|
|`+`|匹配前面的子表达式一次或多次。要匹配 `+` 字符，请使用 `\+`。|
|`.`|匹配除换行符 `\n` 之外的任何单字符。要匹配 `.` ，请使用 `\.` 。|
|`[`|标记一个中括号表达式的开始。要匹配 `[`，请使用 `\[`。|
|`?`|匹配前面的子表达式零次或一次，或指明一个非贪婪限定符。要匹配 `?` 字符，请使用 `\?`。|
|`\`|将下一个字符标记为或特殊字符、或原义字符、或向后引用、或八进制转义符。例如， `n` 匹配字符 `n`。`\n` 匹配换行符。序列 `\\` 匹配 `\`，而 `\(` 则匹配 `(`。|
|`^`|匹配输入字符串的开始位置，除非在方括号表达式中使用，当该符号在方括号表达式中使用时，表示不接受该方括号表达式中的字符集合。要匹配 ^ 字符本身，请使用 \\\^。|
|`{`|标记限定符表达式的开始。要匹配 `{`，请使用 `\{`。|
|`|`|指明两项之间的一个选择。要匹配 `\|`，请使用 `\|`。| 
**限定符**
限定符用来指定正则表达式的一个给定组件必须要出现多少次才能满足匹配。有 `*` 或 `+` 或 `?` 或 `{n}` 或 `{n,}` 或 `{n,m}` 共6种。 
|字符|描述|
|---|---|
|`*`|匹配前面的子表达式零次或多次。例如，`zo*` 能匹配 `z` 以及 `zoo`。`*` 等价于`{0,}`。|
|`+`|匹配前面的子表达式一次或多次。例如，`zo+` 能匹配 `zo` 以及 `zoo`，但不能匹配 `z`。`+` 等价于 `{1,}`。|
|`?`|匹配前面的子表达式零次或一次。例如，`do(es)?` 可以匹配 `do` 、 `does` 中的 `does` 、 `doxy` 中的 `do` 。`?` 等价于 `{0,1}`。|
|`{n}`|`n` 是一个非负整数。匹配确定的 `n` 次。例如，`o{2}` 不能匹配 `Bob` 中的 `o`，但是能匹配 `food` 中的两个 `o`。|
|`{n,}`|`n` 是一个非负整数。至少匹配`n` 次。例如，`o{2,}` 不能匹配 `Bob` 中的 `o`，但能匹配 `foooood` 中的所有 `o`。`o{1,}` 等价于 `o+`。`o{0,}` 则等价于 `o*`。|
|`{n,m}`|`m` 和 `n` 均为非负整数，其中`n <= m`。最少匹配 `n` 次且最多匹配 `m` 次。例如，`o{1,3}` 将匹配 `fooooood` 中的前三个 `o`。`o{0,1}` 等价于 `o?`。请注意在逗号和两个数之间不能有空格。| 
以下正则表达式匹配一个正整数，[1-9]设置第一个数字不是 0，[0-9]* 表示任意多个数字：
```
/[1-9][0-9]*/
```
> * 限定符出现在范围表达式之后。因此，它应用于整个范围表达式 
> * `*` 和 `+` 限定符都是贪婪的，因为它们会尽可能多的匹配文字，只有在它们的后面加上一个 `?` 就可以实现非贪婪或最小匹配。

**定位符**
定位符使您能够将正则表达式固定到行首或行尾。它们还使您能够创建这样的正则表达式，这些正则表达式出现在一个单词内、在一个单词的开头或者一个单词的结尾。

定位符用来描述字符串或单词的边界，`^` 和`$` 分别指字符串的开始与结束，`\b` 描述单词的前或后边界，`\B` 表示非单词边界。 
|字符|描述|
|---|---|
|`^`|匹配输入字符串开始的位置。如果设置了 RegExp 对象的 Multiline 属性，`^` 还会与 `\n` 或 `\r` 之后的位置匹配。|
|`$`|匹配输入字符串结尾的位置。如果设置了 RegExp 对象的 Multiline 属性，`$` 还会与 `\n` 或 `\r` 之前的位置匹配。|
|`\b`|匹配一个单词边界，即字与空格间的位置。|
|`\B`|非单词边界匹配。|
> 注意：不能将限定符与定位符一起使用。由于在紧靠换行或者单词边界的前面或后面不能有一个以上位置，因此不允许诸如 ^* 之类的表达式。

**选择**
用圆括号将所有选择项括起来，相邻的选择项之间用`|`分隔。但用圆括号会有一个副作用，使相关的匹配会被缓存，此时可用`?:`放在第一个选项前来消除这种副作用。

其中 `?:` 是非捕获元之一，还有两个非捕获元是 `?=` 和 `?!`，这两个还有更多的含义，前者为正向预查，在任何开始匹配圆括号内的正则表达式模式的位置来匹配搜索字符串，后者为负向预查，在任何开始不匹配该正则表达式模式的位置来匹配搜索字符串。
**反向引用**
对一个正则表达式模式或部分模式两边添加圆括号将导致相关匹配存储到一个临时缓冲区中，所捕获的每个子匹配都按照在正则表达式模式中从左到右出现的顺序存储。缓冲区编号从 1 开始，最多可存储 99 个捕获的子表达式。每个缓冲区都可以使用 `\n` 访问，其中 `n` 为一个标识特定缓冲区的一位或两位十进制数。

可以使用非捕获元字符 `?:`、`?=` 或 `?!` 来重写捕获，忽略对相关匹配的保存。

反向引用的最简单的、最有用的应用之一，是提供查找文本中两个相同的相邻单词的匹配项的能力。以下面的句子为例：
```
Is is the cost of of gasoline going up up?
```
上面的句子很显然有多个重复的单词。如果能设计一种方法定位该句子，而不必查找每个单词的重复出现，那该有多好。下面的正则表达式使用单个子表达式来实现这一点：
```
var str = "Is is the cost of of gasoline going up up";
var patt1 = /\b([a-z]+) \1\b/ig;
document.write(str.match(patt1));
```
捕获的表达式，正如 `[a-z]+` 指定的，包括一个或多个字母。正则表达式的第二部分是对以前捕获的子匹配项的引用，即，单词的第二个匹配项正好由括号表达式匹配。`\1` 指定第一个子匹配项。

单词边界元字符确保只检测整个单词。否则，诸如 "is issued" 或 "this is" 之类的词组将不能正确地被此表达式识别。

正则表达式后面的全局标记 `g` 指定将该表达式应用到输入字符串中能够查找到的尽可能多的匹配。

表达式的结尾处的不区分大小写 `i` 标记指定不区分大小写。

多行标记指定换行符的两边可能出现潜在的匹配。
## 运算符优先级
正则表达式从左到右进行计算，并遵循优先级顺序，这与算术表达式非常类似。

相同优先级的从左到右进行运算，不同优先级的运算先高后低。下表从最高到最低说明了各种正则表达式运算符的优先级顺序：

|运算符|描述|
|---|---|
|`\`|转义符|
|`()`, `(?:)`, `(?=)`, `[]`|圆括号和方括号|
|`*`, `+`, `?`, `{n}`, `{n,}`, `{n,m}`|限定符|
|`^`, `$`, `\`任何元字符、任何字符|定位点和序列（即：位置和顺序）|
|`|`|替换，"或"操作,字符具有高于替换运算符的优先级，使得"m|food"匹配"m"或"food"。若要匹配"mood"或"food"，请使用括号创建子表达式，从而产生"(m|f)ood"。|
## 匹配规则
**字符簇**
在INTERNET的程序中，正则表达式通常用来验证用户的输入。当用户提交一个FORM以后，要判断输入的电话号码、地址、EMAIL地址、信用卡号码等是否有效，用普通的基于字面的字符是不够的。

所以要用一种更自由的描述我们要的模式的办法，它就是字符簇。要建立一个表示所有元音字符的字符簇，就把所有的元音字符放在一个方括号里：

```
[AaEeIiOoUu]
```
这个模式与任何元音字符匹配，但只能表示一个字符。用连字号可以表示一个字符的范围，如：
```
[a-z] //匹配所有的小写字母 
[A-Z] //匹配所有的大写字母 
[a-zA-Z] //匹配所有的字母 
[0-9] //匹配所有的数字 
[0-9\.\-] //匹配所有的数字，句号和减号 
[ \f\r\t\n] //匹配所有的白字符
```
## 常用正则表达式
### 校验数字的表达式
* 数字：`^[0-9]*$`
* n位的数字：`^\d{n}$`
* 至少n位的数字：`^\d{n,}$`
* m-n位的数字：`^\d{m,n}$`
* 零和非零开头的数字：`^(0|[1-9][0-9]*)$`
* 非零开头的最多带两位小数的数字：`^([1-9][0-9]*)+(\.[0-9]{1,2})?$`
* 带1-2位小数的正数或负数：`^(\-)?\d+(\.\d{1,2})$`
* 正数、负数、和小数：`^(\-|\+)?\d+(\.\d+)?$`
* 有两位小数的正实数：`^[0-9]+(\.[0-9]{2})?$`
* 有1~3位小数的正实数：`^[0-9]+(\.[0-9]{1,3})?$`
* 非零的正整数：`^[1-9]\d*$` 或 `^([1-9][0-9]*){1,3}$ 或 ^\+?[1-9][0-9]*$`
* 非零的负整数：`^\-[1-9][]0-9"*$` 或 `^-[1-9]\d*$`
* 非负整数：`^\d+$` 或 `^[1-9]\d*|0$`
* 非正整数：`^-[1-9]\d*|0$` 或 `^((-\d+)|(0+))$`
* 非负浮点数：`^\d+(\.\d+)?$` 或 `^[1-9]\d*\.\d*|0\.\d*[1-9]\d*|0?\.0+|0$`
* 非正浮点数：`^((-\d+(\.\d+)?)|(0+(\.0+)?))$` 或 `^(-([1-9]\d*\.\d*|0\.\d*[1-9]\d*))|0?\.0+|0$`
* 正浮点数：`^[1-9]\d*\.\d*|0\.\d*[1-9]\d*$` 或 `^(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*))$`
* 负浮点数：`^-([1-9]\d*\.\d*|0\.\d*[1-9]\d*)$` 或 `^(-(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*)))$`
* 浮点数：`^(-?\d+)(\.\d+)?$` 或 `^-?([1-9]\d*\.\d*|0\.\d*[1-9]\d*|0?\.0+|0)$`
### 校验字符的表达式
* 汉字：`^[\u4e00-\u9fa5]{0,}$`
* 英文和数字：`^[A-Za-z0-9]+$` 或 `^[A-Za-z0-9]{4,40}$`
* 长度为3-20的所有字符：`^.{3,20}$`
* 由26个英文字母组成的字符串：`^[A-Za-z]+$`
* 由26个大写英文字母组成的字符串：`^[A-Z]+$`
* 由26个小写英文字母组成的字符串：`^[a-z]+$`
* 由数字和26个英文字母组成的字符串：`^[A-Za-z0-9]+$`
* 由数字、26个英文字母或者下划线组成的字符串：`^\w+$` 或 `^\w{3,20}$`
* 中文、英文、数字包括下划线：`^[\u4E00-\u9FA5A-Za-z0-9_]+$`
* 中文、英文、数字但不包括下划线等符号：`^[\u4E00-\u9FA5A-Za-z0-9]+$` 或 `^[\u4E00-\u9FA5A-Za-z0-9]{2,20}$`
* 可以输入含有`^%&',;=?$\"`等字符：`[^%&',;=?$\x22]+`
* 禁止输入含有~的字符：`[^~\x22]+`
### 常用表单输入校验
* Email地址：`^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$`
* 域名：`[a-zA-Z0-9][-a-zA-Z0-9]{0,62}(\.[a-zA-Z0-9][-a-zA-Z0-9]{0,62})+\.?`
* InternetURL：`[a-zA-z]+://[^\s]*` 或 `^http://([\w-]+\.)+[\w-]+(/[\w-./?%&=]*)?$`
* 手机号码：`^(13[0-9]|14[5|7]|15[0|1|2|3|4|5|6|7|8|9]|18[0|1|2|3|5|6|7|8|9])\d{8}$`
* 电话号码("XXX-XXXXXXX"、"XXXX-XXXXXXXX"、"XXX-XXXXXXX"、"XXX-XXXXXXXX"、"XXXXXXX"和"XXXXXXXX)：`^(\(\d{3,4}-)|\d{3.4}-)?\d{7,8}$`
* 国内电话号码(0511-4405222、021-87888822)：`\d{3}-\d{8}|\d{4}-\d{7}`
* 电话号码正则表达式（支持手机号码，3-4位区号，7-8位直播号码，1－4位分机号）: `((\d{11})|^((\d{7,8})|(\d{4}|\d{3})-(\d{7,8})|(\d{4}|\d{3})-(\d{7,8})-(\d{4}|\d{3}|\d{2}|\d{1})|(\d{7,8})-(\d{4}|\d{3}|\d{2}|\d{1}))$)`
* 身份证号(15位、18位数字)，最后一位是校验位，可能为数字或字符X：`(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)`
* 帐号是否合法(字母开头，允许5-16字节，允许字母数字下划线)：`^[a-zA-Z][a-zA-Z0-9_]{4,15}$`
* 密码(以字母开头，长度在6~18之间，只能包含字母、数字和下划线)：`^[a-zA-Z]\w{5,17}$`
* 强密码(必须包含大小写字母和数字的组合，不能使用特殊字符，长度在 8-10 之间)：`^(?=.*\d)(?=.*[a-z])(?=.*[A-Z])[a-zA-Z0-9]{8,10}$`
* 强密码(必须包含大小写字母和数字的组合，可以使用特殊字符，长度在8-10之间)：`^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,10}$`
* 日期格式：`^\d{4}-\d{1,2}-\d{1,2}`
* 一年的12个月(01～09和1～12)：`^(0?[1-9]|1[0-2])$`
* 一个月的31天(01～09和1～31)：`^((0?[1-9])|((1|2)[0-9])|30|31)$`
* 空白行的正则表达式：`\n\s*\r` (可以用来删除空白行)
* HTML标记的正则表达式：`<(\S*?)[^>]*>.*?|<.*? />`
* 首尾空白字符的正则表达式：`^\s*|\s*$或(^\s*)|(\s*$)` (可以用来删除行首行尾的空白字符(包括空格、制表符、换页符等等)，非常有用的表达式)
* 中国邮政编码：`[1-9]\d{5}(?!\d)` (中国邮政编码为6位数字)
* IP地址：`((?:(?:25[0-5]|2[0-4]\\d|[01]?\\d?\\d)\\.){3}(?:25[0-5]|2[0-4]\\d|[01]?\\d?\\d))`
