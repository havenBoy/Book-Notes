### 枚举类型

> 关键字enum 可以把一组具有的值的有限集合创建为一种新的类型，这些具名的值可以作为常规的程序组件；

1. 基本的enum特性
   * 使用enum的values方法可以遍历enum实例，返回的enum的实例的数组，具有顺序
   * 继承于java.lang.Enum
   * 具体的应用：
   ```java
    package com.xiong;
    //可以把enum实例的标识符带入当前的命名空间
    import static com.xiong.Color.*;

    /**
    * @author 作者:XiaoXiong
    * @version 创建时间：2018年11月14日 下午10:28:14
    * 类说明
    */
    public class Enums {
        //不用使用enum来修饰，但是静态导入会比较难以理解
        Color color;
        
        enum light{red , green, yellow}
        
        public static void main(String[] args) {
            //遍历枚举容器  
            /**
            * red(index):0
            green(index):1
            yellow(index):2
            * */
            for(light s : light.values()){
                System.out.println(s + "(index):" + s.ordinal());
                System.out.println(s.name() + "(index):" + s.ordinal());
            }
            //返回指定名字对应实例的enum实例，如果不存在会抛出异常
            for(String string : "red green yellow".split(" ")){
                light li = Enum.valueOf(light.class, string);
                System.out.println(li);
            }
        }

    }
    ```
    * ordinal()方法返回的是一个int值，这是每个enum实例在声明时的次序，下标是从0开始，可以使用==来比较实例，因为编译器会自动提供了equals方法和hashcode方法，同时实现了serialize接口
    * name 方法返回enum实例声明的名字，valueof方法是Enum类中定义的static方法，返回指定名字对应实例的enum实例，如果不存在会抛出异常

2. 向enum中添加新的方法

   * 不能继承一个enum，其他可以看做一个常见的类，也可在枚举中有main方法
   ```java
    package com.xiong;
    /**
    * @author 作者:XiaoXiong
    * @version 创建时间：2018年11月14日 下午11:01:25
    * 类说明
    */
    public enum Color {
        
        red, yellow, green; //加入定义自己的方法需要在实例序列最后加一个;
        
        public static void main(String[] args) {
            for(Color color : values()) {
                System.out.println(color);
            }
        }
    }
   ```

   * swith 语句中的enum
   ```java


   ```
   