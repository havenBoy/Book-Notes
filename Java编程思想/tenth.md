### 内部类

> 可以把一个类的定义放在另外一个类的内部，是内部类，做到了代码的影藏机制，但需要清楚与组合的不同

* 10.1  创建内部类      只要把普通的类嵌套在类中即可，和以前的类中普通类基本一样；

* ~~~java
  public class InnerClass {
  
      class Contents{
          private int i = 10;
          public int value(){
              return i;
          }
      }
  
      class Destination {
          private String lable;
          Destination(String where){
              lable = where;
          }
          String readLable(){return  lable;}
      }
  
      public void ship() {
          Contents contents = new Contents();
          Destination destination = new Destination("xian");
          System.out.println(destination.readLable());
      }
  
      public static void main(String[] args) {
          InnerClass innerClass = new InnerClass();
          innerClass.ship();
      }
  }
  
  ~~~

  - 如果想要在外部类非静态方法位置创建某个内部类的对象，需要显示的指明如：InnerClass.Contents

* 10.2  链接到外部类     下面例子表明，内部类拥有外部类所有元素的访问权；

  - ~~~java
    public class InnerClass1 {
    
        private Object[] items;
        private int next = 0;
        public InnerClass1 (int size){
            items = new Object[size];
        }
        public void add(Object obj) {
            if(next < items.length) items[next++] = obj;
        }
    
        private class SquenceSelector implements Selector{
            private int i = 0;
            @Override
            public boolean end() {
                return (i == items.length);
            }
    
            @Override
            public Object current() {
                return items[i];
            }
    
            @Override
            public void next() {
                if(i < items.length) i++;
            }
        }
        public SquenceSelector selector(){
            return new SquenceSelector();
        }
    
        public static void main(String[] args) {
            int size = 10;
            InnerClass1 innerClass1 = new InnerClass1(size);
            for (int i = 0; i < size; i++) {
                innerClass1.add(Integer.toString(i));
            }
            Selector selector = innerClass1.selector();
            while (!selector.end()){
                System.out.println(selector.current());
                selector.next();
            }
        }
    }
    
    
    interface Selector{
        boolean end();
    
        Object current();
    
        void next();
    }
    
    ~~~

  - 上边例子是一种迭代器模式，使用selector来获取每一个元素，并且可以移动元素；

  - 内部类可以使用外围类元素的原理：当一个外部类创建一个内部类对象时，内部类对象必定会捕获一个指向外围类对象的引用，通过这个引用来选择外围类的成员

* 10.3  使用.this与.new

  * 

    ~~~java
    public class DotThis {
    
        void f(){ System.out.println("function();"); }
    
        public class Inner{
            public DotThis outer(){
                return DotThis.this;
            }
        }
    
        public Inner inner(){ return  new Inner();}
    
        public static void main(String[] args) {
            DotThis dotThis = new DotThis();
            DotThis.Inner in = dotThis.new Inner();
            in.outer().f();
        }
    }
    ~~~

* 10.4   内部类与向上转型

  - 当将内部类向上转型为基类，体会使用；

  - ~~~java
        public interface Contents {
            int value();
        }
        public interface Destination {
            String readLable();
        }
        private class pContents implements Contents{
            private int i = 1;
            public int value() {return i;}
        }
        public pContents getCon() {
            return new pContents();
        }
    
        private class pDestination implements Destination {
    
            private String lable;
            private pDestination(String path) { lable = path;}
            public String readLable() {
                return lable;
            }
        }
    
        public pDestination getDes(String path) {
            return new pDestination(path);
        }
    
        public static void main(String[] args) {
    
            InnerClass2 innerClass2 = new InnerClass2();
            Contents contents = innerClass2.getCon();
            Destination destination = innerClass2.getDes("xian");
        }
    ~~~

* 10.5   在方法和作用域中的内部类


~~~java
    public Destination destination(String path) {
        class pDestinaion implements Destination{
            private String lable;
            private pDestinaion(String where) {
                lable = where;
            }
            public String readLable() { return lable;}
        }
        return new pDestinaion(path);
    }

    public static void main(String[] args) {
        InnerClass4 innerClass4 = new InnerClass4();
        Destination destination = innerClass4.destination("path");
    }
~~~

* 10.6 匿名内部类

  ~~~java
  public class InnerClass5 {
  
      public Contents getCon() {
          return new Contents() {
              private int i = 10;
              @Override
              public int value() {
                  return i;
              }
          };
      }
  
      public static void main(String[] args) {
          InnerClass5 innerClass5 = new InnerClass5();
          Contents contents = innerClass5.getCon();
      }
  }
  ~~~

* 10.7  嵌套类
  * 把内部类声明为static时，使得嵌套类与外围类没有关系，不能访问外围类，并且不能访问非静态外围类对象