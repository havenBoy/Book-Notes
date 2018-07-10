# 第二章：深入分析JAVA I/O 工作机制
> 主要介绍java I/O类库基本架构，磁盘I/O,网络I/O
## java I/O基本架构
   - 介绍均在java.io下，前2种为不同数据格式的传输，后2个是不同传输方式
      * 1.字节操作（InputStream/OutputStream）
      * 2.字符操作（Writer/Reader）
      * 3.磁盘操作 (File)
      * 4.网络操作 （Socket）