# 字符串
 * 不可变string
   - 每一个看起来会修改string值的方法，实际是创建一个全新的string对象，来包含新字符串
   - 每次把字符串作为方法的参数时，会复制一份引用，并且不会动
   - 方法返回的引用指向新的对象
 * 重载 + 与stringBuilder
   - 二者的对比： 在重载+符号时，底层会自动的调用stringBuilder对象的append方法，但不代表任意的使用+重载，观察到编译代码，会注意到只会创建一个stringBuilder对象，但是重载+符号会使用创建多个string对象
   - 使用stringBuilder时可以指定其大小，如果预知大小，可以显式创建指定大小的stringBuilder对象，避免多次分配缓存
   - 建议：在toString方法中如果使用到循环，那么最好创建一个StringBuilder对象
   - StringBuffer是线程安全的，耗费的资源会大些，但stringBuilder是线程非安全的
 * 无意识的递归
   - 如果想打印对象的内存地址，应该需要调用Object.toString()，不要使用this关键字来打印地址
 * String上的操作

   | 方法 | 参数、重载版本 | 应用 |   
   | -------- | -------- | -------- |
   | 构造器 | string stringbuilder，stringbuffer char数组 | 创建数组 | 
   | length() | | String 字符的个数 | 
   | charAt() | Int索引 | 取到String的索引位置上的char | 
   | getChar()、getBytes() | 复制部分的起始索引与终止索引 | 复制char 或byte到一个目标数组 | 
   | toCharArray() |  | 生成char[] 包含String的所有字符 | 
   | equals()、equalsIgnoreCase() | 与之进行比较的字符 | 比较2个字符是否相同 | 
   | compareTo() | 与之比较的字符串 | 按字典顺序比较字符串的内容，返回负数，0，或者整数 | 
   | contains() | 要搜索的关键符号 | 是否包含关键字符，返回true或者false | 
   | contentEquals() | 与之比较的关键符号或者stringbuffer | 是否与参数的内容完全相同，返回true | 
   | equalsIgnoreCase() | 与之比较的String | 忽略大小写，比较字符串是否相同 | 
   | regionMatcher() | 改字符串的索引偏移，另外一个String，偏移量，比较的长度 | 返回true false | 
   | startsWith() | 可能的起始string | 表示是否以此参数作为起始，返回true false | 
   | endsWith() | 该String的后缀String | 表示是否是以参数作为后缀 返回true 或者 false | 
   | indexOf() lastIndexOf() | char，char与索引，string string与起始索引 | 是否包含 不包含-1 否则索引值 | 
   | contact() | 要连接String | 返回一个新的String对象 | 
   | replace() | 要替换的字符 | 返回替换后的新的字符串 如果没有替换返回原来的字符串 | 
   | toLowerCase() to UpperCase() |  | 返回替换后的新的字符串 ，如果没有替换那么返回原来的内容 | 
   | trim() | | 把字符串的2端空白字符删除后返回新的字符串 | 
   | valueOf() | boolean char int long double | 返回一个参数内存的字符串 | 
   | intern() |  | 为每个唯一的字符串序列生成一个且只有一个string引用 |  
 * 格式化输出 
   - printf()  
   - java.util.formatter 负责所有格式化输出的类
 * 正则表达式
   