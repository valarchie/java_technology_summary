# 1.java中IO流的体系？
Java中的流分为两种，一种是字节流，另一种是字符流，分别由四个抽象类来表示（每种流包括输入和输出两种所以一共四个）:InputStream，OutputStream，Reader，Writer。基于这四种IO流父类根据不同需求派生出其他IO流。
