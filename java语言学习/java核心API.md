
# 2017/2/12 20:25:38  #
# java核心API.md  #
### 2017/2/12 20:25:38 ###
## 文件流 ##
1. 文件写操作函数只能打印字符，不能直接打印整型变量
```java
 int c = 60;
 FileOutputStream fos = new FileOutputStream();
 fos.write(c);
 //这样会打印输出<，而不是60；
```
## 缓冲流 ##
1.  BufferedInputStream 的数据成员 buf 是一个位数组，默认为2048字节。当读取数据来源时例如文件，BufferedInputStream 会尽量将 buf 填满。当使用 read ()方法时，实际上是先读取 buf 中的数据，而不是直接对数据来源作读取。当 buf 中的数据不足时，BufferedInputStream 才会再实现给定的 InputStream 对象的 read() 方法，从指定的装置中提取数据。
2.  BufferedOutputStream 的数据成员 buf 是一个位数组，默认为512字节。当使用 write() 方法写入数据时，实际上会先将数据写至 buf 中，当 buf 已满时才会实现给定的 OutputStream 对象的 write() 方法，将 buf 数据写至目的地，而不是每次都对目的地作写入的动作。 
```
//[ ]里的内容代表选填
BufferedInputStream(InputStream in[,int size])
BufferedOutputStream(OutputStream out[,int size])
```
举个例子：
```java
FileInputStream in = new FileInputStream("file.txt");
FileOutputStream out = new FileOutputStream("file2.txt");
//设置输入缓冲区大小为256字节
BufferedInputStream bin = new BufferedInputStream(in,256)
BufferedOutputStream bout = new BufferedOutputStream(out,256)
int len;
byte bArray[] = new byte[256];
len = bin.read(bArray); //len 中得到的是实际读取的长度，bArray 中得到的是数据
```
对于 BufferedOutputStream，只有缓冲区满时，才会将数据真正送到输出流，但可以使用 flush() 方法人为地将尚未填满的缓冲区中的数据送出。
例如方法copy（）
```java
public void copy(InputStream in, OutputStream out) throw IOException {
    out = new BufferedOutputStream(out, 4096);
    byte[] buf = new byte[4096];
    int len = in.read(buf);
    while (len != -1) {
    out.write(buf, 0, len);
    len = in.read(buf);
    }
    //最后一次读取得数据可能不到4096字节
    out.flush();
}
```
## 数据流 ##
1. 接口 DataInput 和 DataOutput，设计了一种较为高级的数据输入输出方式：**除了可处理字节和字节数组外，还可以处理 int、float、boolean等基本数据类型**，这些数据在文件中的表示方式和它们在内存中的一样，无须转换，如 read(), readInt(), readByte()...; write(), writeChar(), writeBoolean()...此外，还可以用 readLine()方法读取一行信息。
2. 数据流可以通过如下方式建立：
```java
FileInputStream fis = new FileInputStream("file1.txt");
FileOutputStream fos = new FileOutputStream("file2.txt");
DataInputStream dis = new DataInputStream(fis);
DataOutputStream dos = new DataOutputStream(fos);
```
3. 接下来我们通过具体的代码，看一看它的用法吧：
```java
package com.shiyanlou;

import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class DataStream {

    public static void main(String[] args) throws IOException{
        // TODO Auto-generated method stub
        //向文件 a.txt 写入
        FileOutputStream fos = new FileOutputStream("a.txt");
        DataOutputStream dos = new DataOutputStream(fos);
        try {
            dos.writeBoolean(true);
            dos.writeByte((byte)123);
            dos.writeChar('J');
            dos.writeDouble(3.1415926);
            dos.writeFloat(2.122f);
            dos.writeInt(123);
        }
        finally {
            dos.close();
        }
        //从文件 a.txt 读出
        FileInputStream fis = new FileInputStream("a.txt");
        DataInputStream dis = new DataInputStream(fis);
        try {
            System.out.println("\t" + dis.readBoolean());
            System.out.println("\t" + dis.readByte());
            System.out.println("\t" + dis.readChar());
            System.out.println("\t" + dis.readDouble());
            System.out.println("\t" + dis.readFloat());
            System.out.println("\t" + dis.readInt());
        }
        finally {
            dis.close();
        }
    }

}
```
透过代码我们可以看出数据流算是文件流的一种方法另类化，他们分别适用于不同的环境下
## 标准流、内存读写流、顺序输入流 ##
### 1. 标准流 ###
1. 语言包 java.lang 中的 System 类管理标准输入/输出流和错误流。
2. System.in从 InputStream 中继承而来，用于从标准输入设备中获取输入数据（通常是键盘）
System.out从 PrintStream 中继承而来，把输入送到缺省的显示设备（通常是显示器）
3. System.err也是从 PrintStream 中继承而来，把错误信息送到缺省的显示设备（通常是显示器）
每当 main 方法被执行时，就会自动生产上述三个对象。这里就不再写代码验证了。

### 2. 内存读写流 ###

### 3. 顺序读写流 ###

# 字符流 #
跟字节流类似，不同的是存储类型，字节流存储的是int型，字符流存储的是字符。
# 2017/2/14 22：36 #
## 文件操作 ##
1. Fiele的构造函数
```java
//根据 parent 抽象路径名和 child 路径名字符串创建一个新 File 实例。 
File(File parent, String child) 

//通过将给定路径名字符串转换为抽象路径名来创建一个新 File 实例       
File(String pathname) 

// 根据 parent 路径名字符串和 child 路径名字符串创建一个新 File 实例
File(String parent, String child) 

//通过将给定的 file: URI 转换为一个抽象路径名来创建一个新的 File 实例
File(URI uri)
```
例如
```java
//一个目录路径参数
File f1 = new File("/Users/mumutongxue/");

//对象有两个参数——路径和文件名
File f2 = new File("/Users/mumutongxue/","a.bat");

//指向f1文件的路径及文件名
File f3 = new File(f1,"a.bat");
```

