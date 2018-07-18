# 第3章 Java的基本程序设计结构

本章内容：
* 一个简单的Java应用程序
* 字符串
* 注释
* 输入输出
* 数据类型
* 控制流
* 变量
* 大数值
* 运算符
* 数组

本章主要讲述程序设计相关的基本概念（如数据类型、分支以及循环）在Java中的实现方式。

## 3.1 一个简单的Java应用程序

1. Java对大小写敏感。
2. Java定义类名规则：名字必须以字母开头，后面可以跟字母和数字的任意组合。长度基本上没有限制。但是不能使用Java保留字作为类名。
3. 标准的命名规则为：类名是以大写字母开头的名词。如果名字由多个单词组成，每个单词的第一个字母都应该大写。

## 3.2 注释
1. 在Java中，/* */注释不能嵌套。也就是说，如果代码本身包含了一个*/，就不能用/*和*/将注释括起来。

## 3.3 数据类型
1. 在Java中，一共有8中基本类型（primitive type），其中有4种整型、2种浮点类型、1种用于表示Unicode编码的字符单元的字符类型char和一种用于表示真值的boolean类型。

#### 3.3.1 整型
1. 整型用于表示没有小数部分的数值，它允许是负数。
| 类型 | 存储需求 |     取值范围|
|--------|--------|--------|
|int|4字节|-2147483648~2147483647(正好超过20亿)|
|short|2字节|-32768~32767|
|long|8字节|-9223372036854775808~9223372036854775807|
|byte|1字节|-128~127|

2. 在Java中，整型的范围与运行Java代码的机器无关。
3. 长整型数值有一个后缀L，十六进制数值有一个前缀0x。八进制有一个前缀0。从Java7开始，加上前缀0b就可以写二进制数。从Java 7开始，还可以为数字字面量加下划线，如用1_000_000表示一百万。这些下划线只是为了让人更易读。Java编译器会去除这些下划线。

#### 3.3.2 浮点类型
1. 浮点类型用于表示有小数部分的数值。
| 类型 | 存储需求 |     取值范围|
|--------|--------|--------|
|float|4字节|大约±3.40282347E+38F(有效位数为6~7位)|
|double|8字节|大约±1.79769313486231570E+308(有效位数为15位)|
2. double表示这种类型的数值精度是float类型的两倍。绝大部分应用程序都采用double类型。float类型的数值有一个后缀F。没有后缀F的浮点数值默认为double类型。也可以在浮点数值后面添加后缀D。
3. 在JDK5.0中，可以使用十六进制表示浮点数值。例如，0.125可以表示成0x1.0p-3。在十六进制表示法中，使用p表示指数，而不是e。注意，尾数采用十六进制，指数采用十进制。指数的基数是2，而不是10。
4. 用于表示溢出和出错情况的三个特殊的浮点数值：正无穷大、负无穷大、NaN（不是一个数字）。常量Double.POSITIVE_INFINITY、Double.NEGATIVE_INFINITY和Double.NaN分别表示这三个特殊的值。判断一个特定值是否等于Double.NaN使用Double.isNaN方法。
5. 浮点数值不适用于禁止出现摄入误差的金融计算中。如果需要在数值计算中不含有任何舍入误差，就应该使用BigDecimal类。

#### 3.3.3 char类型
1. char类型用于表示单个字符。通常用来表示字符常量。Unicode编码单元可以表示为十六进制值，其范围从\u0000到\uffff。
2. 除了可以采用转义序列符\u表示Unicode代码单元的编码之外，还有一些用于表示特殊字符的转义序列符。所有这些转义序列符都可以出现在字符常量或字符串的引号内。例如，'\u2122'或"Hello\n"。转义序列符\u还可以出现在字符常量或字符串的引号之外（而其他所有转义序列不可以）。例如：`public static void main(String\u005B\u005D args)`，这种形式完全符合语法规则，\u005B和\u005D是[和]的编码。
|转义序列|名称|Unicode值||转义序列|名称|Unicode值|
|--------|--------|--------|
|\b|退格|\u0008||\"|双引号|\u0022|
|\t|制表|\u0009||\'|单引号|\u0027|
|\n|换行|\u000a||\\|反斜杠|\u005c|
|\r|回车|\u000d|||| | |
3. UTF-16编码采用不同长度的编码表示所有Unicode代码点。在基本的多语言级别中，每个字符用16位表示，通常被称为代码单元(code unit)；而辅助字符采用一对连续的代码单元进行编码。这样构成的编码值一定落入基本的语言级别中空闲的2048字节码，通常被称为替代区域（surrogate area）。
4. 在Java中，char类型用UTF-16编码描述一个代码单元。
5. 强烈建议不要在程序中使用char类型，除非确实需要对UTF-16代码单元进行操作。最好将需要处理的字符串用抽象数据类型表示。

#### 3.3.4 boolean类型
1. boolean(布尔)类型有两个值：false和true，用来判定逻辑条件。整型值和布尔值之间不能进行相互转换。

## 3.4 变量
1. 在Java中，每一个变量属于一种类型（type）。
2. 变量名必须是一个以字母开头的由字母或数字构成的序列。变量名中所有的字符都是由意义的，并且大小写敏感。变量名的长度没有限制。
3. 如果想要知道哪些Unicode字符属于Java中的“字母”，可以使用Character类的isJavaIdentifierStart和isJavaIdentifierPart方法进行检测。
4. 尽管$是一个合法的Java字符，但不要在自己的代码中使用这个字符。他只用在Java编译器或其他工具生成的名字中。

#### 3.4.1 变量初始化
1. 在Java中，变量的声明尽可能地靠近变量第一次使用的地方，这是一种良好的程序编写风格。

#### 3.4.2 常量
1. 在Java中，利用关键字final指示常量。
2. 关键字final表示这个变量只能被赋值一次。一旦被赋值之后，就不能够再更改了。习惯上，常量名使用全大写。
3. 在Java中，经常希望某个常量可以在一个类中的多个方法中使用，通常将这些常量称为类常量。可以使用关键字static final 设置一个类常量。

## 3.5 运算符
1. 在Java中，使用算数运算符+、-、*、/表示加、减、乘、除运算。当参数/运算的两个操作数都是整数时，表示整数除法；否则，表示浮点除法。整数的求余操作（取模）用%表示。
2. 整除被0除将会产生一个异常，而浮点数被0除将会得到无穷大或NaN结果。
3. 在默认情况下，虚拟机设计者允许将中间计算结果采用扩展的精度。但是，对于使用strictfp关键字表示的方法必须使用严格的浮点计算来产生理想的结果。如果将一个类标记为strictfp，这个类中的所有方法都要使用严格的浮点计算。

#### 3.5.1 自增运算符与自减运算符
#### 3.5.2 关系运算符与boolean运算符
#### 3.5.3 位运算符
1. 位运算符&(“与”)、|(“或”)、^(“异或”)、~(“非”)这些运算符在位模式下工作。
2. ">>"和"<<"运算符将二进制位进行右移或左移操作。>>>运算符将用0填充高位；>>运算符用符号位填充高位。没有<<<运算符。
3. 对以为运算符右侧的参数需要进行模32的运算（除非左边的操作数是long类型，在这种情况下需对右侧操作数模64）。如，1左移35与1<<3或8是相同的。

#### 3.5.4 数学函数与常量
1. 数值的平方根，使用sqrt方法。
2. 幂运算，使用pow方法，Math.pow(x,a)表示x的a次幂。pow方法有两个double类型的参数，其但会结果也为double类型。
3. 在Math类中，为了达到最快的性能，所有的方法都使用计算机浮点单元中的例程，如果得到一个完全可预测的结果比运行速度更重要的话，那么就应该使用StrictMath类。他使用“自由发布的Math库”(fdlibm)实现算法，一确保在所有平台上得到相同的结果。

#### 3.5.5 数值类型之间的转换
1. 数值类型之间的合法转换
~~~mermaid
graph LR;
byte-->short;
short-->int;
char-->int;
int-->double;
int-->long;
int-.->float;
long-.->double;
long-.->float;
~~~
实心箭头，表示无信息丢失的转换，虚箭头，表示可能有精度损失的转换。
2. 当使用两个数值进行二元操作时，先要将两个操作数转换为同一种类型，然后再进行计算。
 * 如果两个操作数中有一个是double类型，另一个操作数就会转换为double类型。
 * 否则，如果其中一个操作数是float类型，另一个操作数将会转换为float类型。
 * 否则，如果其中一个操作数是long类型，另一个操作数将会转换为long类型。
 * 否则，两个操作数都将转换为int类型。

#### 3.5.6 强制类型转换
1. double转int，强制类型转换通过截断小数部分将浮点值转换为整型。如果想对浮点数进行舍入运算，以便得到最接近的整数，那就需要使用Math.round方法。当调用round的时候，仍然需要使用强制类型转换(int)。其原因是round方法返回的结果为long类型，由于存在信息丢失的可能性，所以只有使用显式的强制类型转换才能够将long类型转换成int类型。
2. 如果试图将一个数值从一种类型强制转换成另一种类型，而又超出了目标类型的表示范围，结果就会截断成一个完全不同的值。例如，（byte）300的实际值为44。

#### 3.5.7 括号与运算符级别
1. 同一个级别的运算符按照从左到右的次序进行计算。
|运算符|结合性|
|--------|--------|
|[].()（方法调用）|从左到右|
|!~++..+(一元运算)-（一元运算）()（强制类型转换）new|从右到左|
|*/%|从左到右|
|+-|从左到右|
|<< >> >>>|从左到右|
|< <= > >= instanceof|从左到右|
|== !=|从左到右|
|&|从左到右|
|^|从左到右|
|或位运算符|从左到右|
|&&与|从左到右|
|或|从左到右|
|?:|从右到左|
|= += -= *= /= %= &= 或等于 ^= <<== >>= >>>=|从右到左|

#### 3.5.8 枚举类型
1. 枚举类型包括了有限个命名的值。

## 3.6 字符串
1. 从概念上讲，Java字符串就是Unicode字符序列。Java没有内置的字符串类型，而是在标准Java类库中提供了一个预定义类，叫做String。

####3.6.1 子串
1. String类的substring方法可以从一个较大的字符串提取出一个子串。

#### 3.6.2 拼接
1. 当将一个字符串与一个非字符串的值进行拼接时，后者被转换成字符串。

#### 3.6.3 不可变字符串
1. String类没有提供用于修改字符串的方法。由于不能修改Java字符串中的字符，所以在Java文档中奖String类对象称为不可变字符串。不可变字符串有一个优点：编译器可以让字符串共享。
2. Java的设计者认为共享带来的高效率远远胜于提取、拼接字符串所带来的低效率。

#### 3.6.4 检测字符串是否相等
1. 使用equals方法检测两个字符串是否相等。想检测两个字符串是否相等，而不区分大小写，可以使用equalsIgnoreCase方法。不能使用==运算符检测两个字符串是否相等，这个运算符只能够确定两个字符串是否放置在同一个位置上。如果虚拟机始终将相同的字符串共享，就可以使用相等运算符检测是否相等。但实际上只有字符串常量是共享的，而+或substring等操作产生的结果并不是共享的。

#### 3.6.5 空串与Null串
1. 空串""是长度为0的字符串。空船是一个Java对象，有自己的串长度（0）和内容（空）。
2. String变量还可以存放一个特殊的值，名为null，表示目前没有任何对象与该变量关联。

#### 3.6.6 代码点与代码单元
1. Java字符串由char序列组成。char数据类型是一个采用UTF-16编码表示Unicode代码点的代码单元。大多数的常用Unicode字符使用一个代码单元就可以表示，而辅助字符需要一对代码单元表示。
2. length方法将返回采用UTF-16编码表示的给定字符串所需要的代码单元数量。
3. 要想得到实际的长度，即代码点数量，可以调用String.codePointCount(0,length)。
4. 调用s.charAt(n)将返回位置n的代码单元，n介于0~s.length-1之间。
5. 要想得到第i个代码点，使用下列语句：int index = s.offsetByCodePoints(0,i);int cp = s.codePointAt(index)；
6. 如果想要遍历一个字符串，并且依次查看每一个代码点，可以使用下列语句：
```
int cp = sentence.codePointAt(i);
if (Charcter.isSupplementaryCodePoint(cp)) i+=2;
else i++;
```
可以使用下列语句实现回退操作：
```
i--;
if (Charcter.isSurrogate(sentence.charAt(i))) i--;
int cp = sentence.codePointAt(i)
```

#### 3.6.7 字符串API
1.	常用的API：java.lang.string 1.0
* char charAt(int index)
返回给定位置的代码单元。除非对底层的代码单元感兴趣，否则不需要调用这个方法。
* int codePointAt(int index) 5.0
返回从给定位置开始或结束的代码点。
* int offsetByCodePoints(int startIndex,int cpCount) 5.0
返回从startIndex代码点开始，位移cpCount后的代码点索引。
* int compareTo(String other)
按照字典顺序，如果字符串位于other之前，返回一个负数；如果字符串位于other之后，返回一个正数；如果两个字符串相等，返回0。
* boolean endsWith(String suffix
如果字符串以suffix结尾，返回true。
* boolean equals(Object other)
如果字符串与other相等，返回true。
* boolean equalsIgnoreCase(String other)
如果字符串与other相等（忽略大小写），返回true。
* int indexOf(String str)
* int indexOf(String str, int fromIndex)
* int indexOf(int cp)
* int indexOf(int cp, int fromIndex)
返回与字符串str或代码点cp匹配的第一个子串的开始位置。这个位置从索引0或fromIndex开始计算。如果在原始串中不存在str，返回-1。
* int lastIndexOf(String str)
* int lastIndexOf(String str,int fromIndex)
* int lastIndexOf(int cp)
* int lastIndexOf(int cp, int fromIndex)
返回与字符串str或代码点cp匹配的最后一个子串的开始位置。这个位置从原始串尾端或fromIndex开始计算。
* int length()
返回字符串的长度。
* int codePointCount(int startIndex, int endIndex) 5.0
返回startIndex和endIndex-1之间的代码点数量。没有配成对的代用字符将计入代码点。
* String replace(CharSequence oldString,CharSequence newString)
返回一个新字符串。这个字符串用newString代替原始字符串中所有的oldString。可以用String或StringBuilder对象作为CharSequence参数。
* boolean startsWith(String prefix)
如果字符串以prefix字符串开始，返回true。
* String substring(int beginIndex)
* String substring(int beginIndex, int endIndex)
返回一个新字符串。这个字符串包含原始字符串中beginIndex到串尾或endIndex-1的所有代码单元。
* String toLowerCase()
返回一个新字符串。这个字符串将原始字符串中的所有大写字母改成了小写字母。
* String toUpperCase()
返回一个新字符串。这个字符串将原始字符串中的所有小写字母改成了大写字母。
* String trim()
返回一个新字符串。这个字符串将删除了原始字符串头部和尾部的空格。

#### 3.6.7 阅读联机API文档
#### 3.6.8 构建字符串
1. 每次连接字符串，都会构建一个新的String对象，既耗时，又浪费空间。使用StringBuilder类就可以避免这个问题的发生。
2. StringBuilder常用API：java.lang.StringBuilder 5.0
* StringBuilder()
构造一个空的字符串构建器。
* int length()
返回构建器或缓冲器中的代码单元数量。
* StringBuilder append(String str)
追加一个字符串并返回this。
* StringBuilder append(char c)
追加一个代码单元并返回this。
* StringBuilder appendCodePoint(int cp)
追加一个代码点，并将其转换为一个或两个代码单元并返回this。
* void setCharAt(int i,char c)
将第i个代码单元设置为c。
* StringBuilder insert(int offset,String str)
在offset位置插入一个字符串并返回this。
* StringBuilder insert(int offset,Char c)
在offset位置插入一个代码单元并返回this。
* StringBuilder delete(int startIndex, int endIndex)
删除偏移量从startIndex到endIndex-1的代码单元并返回this。
* String toString()
返回一个与构建器或缓冲器内容相同的字符串。

## 3.7 输入输出
#### 3.7.1 读取输入
1. 读取“标准输出流”，使用Scanner对象。
```
Scanner in = new Scanner(System.in);
System name = in.nextLine(); //读取一行
String firstName = in.next; //读取一个单词
int age = in.nextInt(); //读取一个整数
```
2. Scanner类不适用于从控制台读取密码。Console实现读取一个密码。
```
Console cons = System.console();
String username = cons.readLine("User name:");
char[] passwd = cons.readPassword("Password:");
```
采用Console对象处理输入不如采用Scanner方便。每次只能读取一行输入，而没有能够读取一个单词或一个数值的方法。
3. java.util.Scanner 5.0
* Scanner(InputStream in)
用给定的输入流创建一个Scanner对象。
* String nextLine()
读入输入的下一行内容。
* String next()
读取输入的下一个单词（以空格作为分隔符）。
* int nextInt()
* double nextDouble()
读取并转换下一个表示整数或浮点数的字符序列。
* boolean hasNext()
检测输入中是否还有其他单词。
* boolean hasNextInt()
* boolean hasNextDouble()
检测是否还有表示整数或浮点数的下一个字符序列。

4. java.lang.System 1.0
* static Console console() 6
如果有可能进行交互操作，就通过控制台窗口为交互的用户返回一个Console对象，否则返回null。对于任何一个通过控制台启动的程序，都可使用Console对象。否则，其可用性将与所使用的系统有关。

5. java.io.Console 6
* static char[] readPassword(String prompt, Object... args)
* static String readLine(String prompt, Object... args)
显示字符串prompt并且读取用户输入，知道输入行结束。args参数可以用来提供输入格式。

#### 3.7.2 格式化输出
1. 用于printf的转换符
|转换符|类型|举例||转换符|类型|举例|
|--------|--------|
|d|十进制整数|159||s|字符串|Hello|
|x|十六进制整数|9f||c|字符|H|
|0|八进制整数|237||b|布尔|True|
|f|定点浮点数|15.9||h|散列码|42628b2|
|e|指数浮点数|1.59e+01||tx|日期时间|--|
|g|通用浮点数|--||%|百分号|%|
|a|十六进制浮点数|0x1.fccdp3||n|与平台有关的行分割符|--|

2. 用于printf的标志
|标志|目的|举例|
|--------|--------|
|+|打印正数和负数的符号|+3333.33|
|空格|在正数之前添加空格|空格3333.33|
|0|数字前面补0|003333.33|
|.|左对齐|3333.33空白|
|(|将负数括在括号内|(3333.33)|
|,|添加分组分割线|3,333.33|
|#(对于f格式)|包括小数点|3,333.|
|#(对于x或0格式)|添加前缀0x或0|0xcafc|
|$|给定被格式化的参数索引。例如.%1$d,%1$x将以十进制和十六进制格式打印第一个参数|159 9F|
|<|格式化前面说明的数值。例如，%d%<x以十进制和十六进制打印同一个数值|159 9F|

3. 可以使用s转换符格式任意的对象。对于任意实现了Formattable接口的对象都将调用formatTo方法；否则将调用toString方法，它可以将对象转换为字符串。
4. printf方法中日期与时间的格式化选项。以t开始，以下边中任意字母结束的两个字母格式打印日期时间。
** 日期和时间的转换符**
|转换符|类型|举例|
|--------|--------|
|c|完整的日期和时间|Mon Feb 09 18:05:19 PST 2004|
|F|ISO 8601 日期|2004-02-09|
|D|美国格式的日期(月/日/年)|02/09/2004|
|T|24小时时间|18:05:19|
|T|12小时时间|06:05:19pm|
|R|24小时时间没有秒|18:05|
|Y|4位数字的年(前面补0)|2004|
|y|年的后两位数字(前面补0)|04|
|C|年的前两位数字(前面补0)|20|
|B|月的完整拼写|February|
|b或h|月的缩写|Feb|
|m|两位数字的月（前面补0）|02|
|d|两位数字的日（前面补0）|09|
|e|两位数字的月（前面不补0）|9|
|A|星期几的完整拼写|Monday|
|a|星期几的缩写|Mon|
|j|三位数的年中的日子(前面补0)，在001到366之间|069|
|H|两位数字的小时(前面补0)，在0到23之间|18|
|k|两位数字的小时(前面不补0)，在0到23之间|18|
|I(大写的i)|两位数字的小时(前面补0)，在0到12之间|06|
|l(小写的L)|两位数字的小时(前面不补0)，在0到12之间|6|
|M|两位数字的分钟(前面补0)|05|
|S|两位数字的秒(前面补0)|19|
|L|三位数字的毫秒(前面补0)|047|
|N|九位数字的毫秒(前面补0)|047000000|
|P|上午或下午的大写标志|PM|
|p|上午或下午的小写标志|pm|
|z|从GMT起，RFC822数字位移|-0800|
|Z|时区|PST|
|s|从格林威治时间1970-01-01 00:00:00起的秒数|107884319|
|Q|从格林威治时间1970-01-01 00:00:00起的毫秒数|107884319047|

#### 3.7.3 文件输入与输出
1. 要相对文件进行读取，就需要一个用File对象构造一个Scanner对象，例如：
```
Scanner in = new Scanner(Paths.get("myfile.txt"));
```
2. 要想写入文件，就需要构造一个PrintWriter对象。在构造器中，只需要提供文件名：
```
PrintWriter out = new PrintWriter("myfile.txt");
```
3. java.util.Scanner 5.0
* Scanner(File f)
构造一个从给定文件读取数据的Scanner。
* Scanner(String data)
构造一个从给定字符串读取数据的Scanner。
4. java.io.PrintWriter 1.1
* PrintWriter(String fileName)
构造一个将数据写入文件的PrintWriter。文件名由参数指定。
5. java.nio.file.Paths 7
* static Path get(String pathname)
根据给定的路径名构造一个Path。

## 3.8 控制流程

#### 3.8.1 块作用域
1. 块（即复合语句）是指一对花括号括起来的若干条简单的Java语句。块确定了变量的作用域。一个块可以嵌套另一个块。不能在嵌套的两个块中声明同名的变量。

#### 3.8.2 条件语句
1. 条件语句的格式为
```
if(condition) {
	statement1
	statement2
	...
}
```

#### 3.8.3 循环
1. 当条件为true时，while循环执行一条语句（也可以是一个语句块）。常用的格式为
```
while(condition) statement
```
如果开始循环条件的值就为false，则while循环体一次也不执行。
2. while循环语句首先检测循环条件。因此，循环体中的代码有可能不被执行。如果希望循环体至少执行一次，则应该将检测条件放在最后。使用do/while循环语句可以实现这种操作方式。它的语法格式为：
```
do statement while(condition);
```

#### 3.8.4 确定循环
1. for循环语句是支持迭代的一种通用结构，利用每次迭代之后更新的计数器或类似的变量来控制迭代次数。

#### 3.8.5 多重选择：switch语句
1. case标签可以是：
* 类型为char、byte、short或int（或其包装器类Character、Byte、Short和Integer）的常量表达式。
* 枚举常量。
* 从Java SE 7 开始，case标签还可以是字符串字面量。

#### 3.8.6 中断控制流程语句
1. Java还提供了一种带标签的break语句，用于跳出多重嵌套的循环语句。

## 3.9 大数值
1. 如果基本的整数和浮点数精度不能够满足需求，那么可以使用java.math包中的两个很有用的类：BigInteger和BigDecimal。这两个类可以处理包含任意长度数学序列的数值。BigInteger类实现了任意精度的整数运算，BigDecimal实现了任意精度的浮点数运算。
2. 使用静态的valueOf方法可以将普通的数值转换为大数值：
```
BirInteger a = BigInteger.valueOf(100);
```
3. 不能使用人们熟悉的算数运算符（如：+和*）处理大数值。而需要使用大数值类中的add和multiply方法。
4. java.math.BigInteger 1.1
* BigInteger add(BigInteger other)
* BigInteger subtract(BigInteger other)
* BigInteger multiply(BigInteger other)
* BigInteger divide(BigInteger other)
* BigInteger mod(BigInteger other)
返回这个大整数和另一个大整数other的和、差、积、商以及余数。
* int compareTo(BigInteger other)
如果这个大整数与另一个大整数other相等，返回0；如果这个大整数小于另一个大整数other，返回负数；否则，返回正数。
* static BigInteger valueOf(long x)
返回值等于x的大整数。

5. java.math.BigInteger 1.1
* BigDecimal add(BigDecimal other)
* BigDecimal subtract(BigDecimal other)
* BigDecimal multiply(BigDecimal other)
* BigDecimal divide(BigDecimal other,RoundingMode mode) 5.0
返回这个大实数与另一个大实数other的和、差、积、商。要想计算商，必须给出舍入方式（rounding mode）。RoundingMode.HALF_UP是四舍五入方式。它适用于常规的计算。
* int compareTo(BigDecimal other)
如果这个大实数与另一个大实数相等，返回0；如果这个大实数小于另一个大实数，返回负数；否则，返回正数。
* static BigDecimal valueOf(long x)
* static BigDecimal valueOf(long x,int scale)
返回值为x或x/10^scale的一个大实数。

## 3.10 数组
1. 数组是一种数据结构，用来存储同一类型值的集合。通过一个整型下标可以访问数组中的每一个值。
2. 创建一个数字数组时，所有元素都初始化为0。boolean数组的元素会初始化为false。对象数组的元素则初始化为一个特殊值null，这表示这些元素未存放任何对象。

#### 3.10.1 for each循环
1. Java有一种功能很强的循环结构，可以用来一次处理数组中的每个元素而不必为指定下标值而分心。这种增强的for循环的语句格式为:
```
for(variable:collection) statement
```
定义一个变量用于暂存集合中的每一个元素，并执行相应的语句。collection这一集合表达式必须是一个数组或者是一个实现了Iterable接口的类对象。
2. for each循环语句的循环变量将会遍历数组中的每个元素，而不需要使用下标值。

####3.10.2 数组初始化以及匿名数组
1. 在Java中，提供了一种创建数组对象并同时赋予初始值的简化书写形式。如：
```
int[] smallPrines = {2,3,5,7,11,13};
```

#### 3.10.3 数组拷贝
1. 在Java中，允许将一个数组拷贝给另一个数组变量。这时，两个变量将引用同一个数组。如果希望将一个数组的所有值拷贝到一个新的数组中去，就要使用Arrays类的copyOf()方法：
```
int[] copiedLuckyNumbers = Arrays.copyOf(luckyNumbers,luckyNumbers.length);
```
第2个参数是新数组的长度。这个方法通常用来增加数组的大小：
```
luckyNumbers = Arrays.copyOf(luckyNumbers, 2*luckyNumbers.length);
```
如果数组元素是数值型，那么多余的元素将被赋值为0；如果数组元素是布尔型，则将赋值为false。相反，如果长度小于原始数组的长度，则只拷贝最前面的数据元素。

#### 3.10.4 命令行参数

#### 3.10.5 数组排序
1. 要想对数值型数组进行排序，可以使用Arrays类中的sort方法：
```
int[] a = new int[10000];
...
Arrays.sort(a);
```
这个方法使用了优化的快速排序算法。快速排序算法对于大多数数据集合来说都是效率比较高的。

2. java.util.Arrays 1.2
* static String toString(type[] a) 5.0
返回包含a中数据元素的字符串，这些数据元素被放在括号内，并用逗号分隔。
* static type copyOf(type[] a, int length) 6
* static type copyOf(type[] a,int start, int end) 6
返回与a类型相同的一个数组，其长度为length或者end-start，数组元素为a的值。
参数：
**a：**类型为int、long、short、char、byte、boolean、float或double的数组。
**start：**起始下标（包含这个值）。
**end：**终止下标（不包含这个值）。这个值可能大于a.length。在这种情况下，结果为0或false。
**length：**拷贝的数据元素长度。如果length值大于a.length，结果为0或false；否则，数组中只有前面length个数据元素的拷贝值。
* static void sort(type[] a)
采用优化的快速排序算法对数组进行排序。
参数：**a：**类型为int、long、short、char、byte、boolean、float或double的数组。
* static int binarySearch(byte[] a, type v)
* static int binarySearch(byte[] a, int start, int end, type v) 6
采用二分搜索算法查找值v。如果查找成功，则返回相应的下标值；否则，返回一个负数值r。-r-l是为了保持a有序v应插入的位置。
参数：
**a：**类型为int、long、short、char、byte、boolean、float或double的有序数组。
**start：**起始下标（包含这个值）。
**end：**终止下标（不包含这个值）。
**v：**同a数据元素类型相同的值。
* static void fill(type[] a, type v)
将数组的所有数据元素值设置为v。
参数：
**a：**类型为int、long、short、char、byte、boolean、float或double的数组。
**v：**与a数据元素类型相同的一个值。
* static boolean equals(type[] a,type[] b)
如果两个数组大小相同，并且下标相同的元素都对应相等，返回true。
参数：
**a、b：**类型为int、long、short、char、byte、boolean、float或double的两个数组。

#### 3.10.6 多维数组
1. 多维数组将使用多个下标访问数组元素，它适用于表示表格或更加复杂的排列形式。
2. 要想快速地打印一个二维数组的数据元素列表，可以调用：
```
System.out.println(Array.deepToString(a));
```

#### 3.10.7 不规则数组


















