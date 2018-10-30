### 深入class文件结构
> java是一种跨平台的语言，一次编译可以在任何平台上运行，需要关注class文件到底是怎么组织的

  * 5.1 JVM指令集简介
    - 5.1.1 类相关的指令  
      * .source Message.java  表示这个代码的源文件是Message.java 
      * .calss public Message 表示这个是类且类名是Message
      * .super java/lang/Object 表示这个对象的父类是Object
    - 5.1.2 方法的定义
      * .method public<init>()V表示是一个共有的方法，没有参数，返回值是V
    - 5.1.3 属性的定义
      * 基本数据类型的首字母大写
    - 5.1.4其他指令集
      * 与栈相关的操作
        - dup 
        - dup_x1
        - dup_x2
        - dup2
        - dup2_1
        - dup2_2
        - pop
        - pop2
        - swap 
  * 5.2 class文件头的表现形式
    - 
  * 5.3 常量池
    - UTF8 常量类型
    - Fieldref  Methodref 常量类型
    - Class常量类型
    - NameAndType常量类型
  * 5.4 类信息
  * 5.5 Fileds和Methods定义
  * 5.6 类属性描述
  * 5.7 javap生成的class文件结构
  

