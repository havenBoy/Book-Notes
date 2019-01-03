###  Java/IO 系统

> 创建一个良好的I/O环境是非常不容易的，需要考虑通信的接受端的类型（文件，网络链接）以及多种不同的通信方式（顺序，缓冲，二进制，按行，按列等）
>
> 所以本节主要讲述java/IO各式各样的类，以及JDK1.4版本中的java/NIO技术；

* File  类

  - 一个用来处理文件目录的工具类，可以代表一个特定文件的名称又可以指一组文件的名称，如果说是一组文件的名称，就可以调用list()方法，返回一个字符的数组；

  - ~~~java
    public class JavaIo {
    
        //此处的入参需要是final，使得可以使用该类范围内的对象
        public static FilenameFilter filter(final String regex) {
            return new FilenameFilter() {
                private Pattern pattern = Pattern.compile(regex);
                @Override
                public boolean accept(File dir, String name) {
                    return pattern.matcher(name).matches();
                }
            };
        }
    
        public static void main(String[] args) {
            File file = new File(".");
            String[] list ;
            if(args.length < 1 )
                list = file.list();
            else
                  list = file.list(filter(""));
            Arrays.sort(list,String.CASE_INSENSITIVE_ORDER);
            for (String each: list) {
                System.out.println(each);
            }
        }
    }
    /**输出project的目录结构
    .idea
    Code.iml
    out
    src
    */
    ~~~

  - ~~~java
    
    ~~~

  - 