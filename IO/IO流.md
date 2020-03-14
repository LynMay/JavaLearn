# IO流(java.io)

## IO流介绍

IO流处理设备间数据传输问题。

应用：文件上传下载等

IO流分为：

​		数据流向：

​						输入流			输出流

​		数据类型：

​						字符流			字节流（万能流）

​		

## file类

### File的三种构造方法

`java`

```java
public class FileConstructer {
    public static void main(String[] args) {
        //first
        File file1 = new File("F:\\yanan\\li.txt");
        System.out.println(file1);
        //second
        File file2 = new File("F:\\yanan","li.txt");
        System.out.println(file2);
        //third
        File file3 = new File(new File("F:\\yanan"),"li.txt");
        System.out.println(file3);
    }
}
```

### File类功能

#### 创建功能

1、public boolean createNewFile()：成功创建返回true，否则false

2、public boolean mkdir():创建目录

3、public boolean mkdirs():创建多级目录

`java`

```java
public class FileFunction {
    public static void main(String[] args) throws IOException {
        //创建功能
        //在F盘目录yanan下新建文件li.txt
        File file1 = new File("F:\\yanan\\li.txt");
        file1.createNewFile();
        //在F盘目录yanan下创建目录lia
        File file2 = new File("F:\\yanan\\lia");
        file2.mkdir();
        //在F盘目录yanan下创建多级目录lic\\lid
        File file3 = new File("F:\\yanan\\lic\\lid");
        file3.mkdirs();
        //报异常，找不到指定的路径，因为E盘下没有ya\\nan目录
        File file4 = new File("F:\\ya\\nan\\li.txt");
        System.out.println(file4.createNewFile());

    }

}
```

#### 判断获取功能

1、public boolean isDirectory()						  file是否为目录

2、public boolean isFile()			 						file是否是文件

3、public boolean exists()									file是否存在

4、public String getAbsolutePath()					获取file的绝对路径字符串

5、public String getPath()									转为路径名字符串

6、public String  getName()								获取文件或目录的名称

7、public String[] list()										  返回file目录下所有的文件及目录名称字符串数组

8、public File[] listFiles()									  返回file目录下所有的文件及目录名称File对象数组

`java`

```java
public class FileDemo {
    public static void main(String[] args)  throws IOException {
        File file = new File("fileLearn\\file.txt");
        file.createNewFile();
        //判断
        System.out.println(file.isDirectory());
        System.out.println(file.isFile());
        System.out.println(file.exists());
        //获取1
        System.out.println(file.getAbsolutePath());//E:\IO\fileLearn\file.txt
        System.out.println(file.getPath());//fileLearn\file.txt
        System.out.println(file.getName());//file.txt
        //获取2
        File file2 = new File("F:\\yanan");
        String[] lists = file2.list();
        for (String list : lists) {
            System.out.println(list);
        }
        //获取3
        File[] files = file2.listFiles();
        for (File file1 : files) {
            //可对file对象执行其他操作
            if(file1.isFile()){
                System.out.println(file1.getName());
            }

        }


    }
}
```

#### 删除功能

public boolean delete()									删除目录或文件

`java`

```java
public class FileDel {
    public static void main(String[] args) throws IOException {
        //删除模块下的file.txt
        File file = new File("fileLearn\\file.txt");
        file.delete();
        //在模块下新建目录dir，并在dir下新建文件1.txt；再把dir、1.txt都删除
        //1 新建
        File file2 = new File("fileLearn\\dir");
        file2.mkdir();
        File file3 = new File("fileLearn\\dir\\1.txt");
        file3.createNewFile();
        //2 删除 注意：目录下有文件时，不能直接删除该目录。
        file3.delete();
        file2.delete();
    }
}
```

## 字节流

### 字节流抽象基类

**InputStream				这个抽象类表示字节输入流的所有类的超类

**OutputStream			这个抽象类表示字节输出流的所有类的超类

**子类名特点					子类名称都以父类名作为后缀

### 字节流写数据

```java
1、public FileOutputStream (String name)
创建文件输出流以指定的名称写入文件
2、write() 		写数据
3、close()  		关资源
```

```java
public static void main(String[] args) throws IOException {
//三件事：1、调用系统功能创建文件2、创建了字节输出流对象3、让字节输出流对象指向创建好的文件
    FileOutputStream fos = new FileOutputStream("F:\\javaLearn\\IO\\fos.txt");
    //写
    fos.write(97);
    //关闭此文件输出流并释放与此流相关的任何系统资源。
    fos.close();
}
```

#### 字节流写数据三种方式

<img src="F:/javaLearn/IO/IO/pic/1.png">

```java
public static void main(String[] args) throws IOException {
    FileOutputStream fos = new FileOutputStream(new File("F:\\javaLearn\\IO\\fo.txt"));
    //first
    fos.write(97);
    //second
    byte[] bytes = "bcde".getBytes();
    fos.write(bytes);
    //third
    fos.write(bytes,1,3);
    fos.close();

}
```

`小问题：实现换行`											

linux:				\n										

mac:				\r

windows：	\r\n	

`小问题：实现追加写入`

`public FileOutputStream(File file, boolean append)`																	

第二个参数为true，则将字节写入文件的末尾

### 字节流读数据

`

```java
public static void main(String[] args) throws IOException {
    FileInputStream fin = new FileInputStream("F:\\javalearn\\IO\\fo.txt");
    int by;
    //读到文件末尾是-1
    while((by=fin.read())!=-1){
        System.out.print((char)by);
    }
}
```

`

#### 一次读一个字节数组数据

方法：

​		public int read(byte b[])

​		返回读取长度

```java
public static void main(String[] args) throws IOException {
    FileInputStream fin = new FileInputStream("F:\\javaLearn\\IO\\fo.txt");
    //创建一个字节数组作为读取数据的容器
    byte[] by = new byte[1024];
    //一次读取的长度，当长度为-1时说明已读到文件末尾，结束读取
    int len;
    while ((len=fin.read(by))!=-1){
        //将读取的字节数组转化为字符串输出
        System.out.print(new String(by,0,len));
        System.out.println(len);
    }
}
```

### 复制文本文件

数据源：

​		要读取的文件（模块下的fo.txt）

目的地：

​		要写入的文件（模块下的fos.txt）

```java
public static void main(String[] args) throws IOException {
    FileInputStream fin = new FileInputStream("F:\\javaLearn\\IO\\fo.txt");
    FileOutputStream fos = new FileOutputStream("F:\\javaLearn\\IO\\fos.txt");
    int by;
    while((by=fin.read())!=-1){
        fos.write(by);
    }
    fin.close();
    fos.close();
}
```

### 字节流复制图片

一次读取一个字节数组数据，一次写入一个字节数组数据

```java
public static void main(String[] args) throws IOException{
    FileInputStream fin  = new FileInputStream("E:\\firefox\\元旦.jpg");
    FileOutputStream fos = new FileOutputStream("F:\\javaLearn\\IO\\img.jpg");
    int len;
    byte[] by = new byte[1024];
    while((len=fin.read(by))!=-1){
        fos.write(by,0,len);
    }
    fin.close();
    fos.close();
}
```



### 字节缓冲流

```java
public BufferedOutputStream(OutputStream out)
实现缓冲输出流
```

```java
public BufferedInputStream(InputStream in)
实现缓冲输入流
```

`java`

```java
public static void main(String[] args) throws IOException {
    BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("F:\\javaLearn\\IO\\bo.txt"));
    bos.write("hello\r\n".getBytes());
    bos.close();
    //读数据
    BufferedInputStream bin = new BufferedInputStream(new FileInputStream("F:\\javaLearn\\IO\\bo.txt"));
    //一次读取一个字节数组数据
    byte[] by = new byte[1024];
    int len;
    while((len=bin.read(by))!=-1){
        System.out.println(new String(by,0,len));
    }
    bin.close();

}
```

***结论：使用字节缓冲流一次读写多字节数据的耗时短，效率快！

## 字符流

字节流操作中文不方便，使用字符流。

**字符流=字节流+编码表**

之前使用字节流复制文本文件（有中文），正常。

***原因***：汉字在存储的时候，无论使用哪种编码存储（GBK 2字节；utf-8 3字节），第一个字节都是负数

### 抽象基类

***Reader***：字符输入流的抽象类

***Writer***：字符输出流的抽象类

字符流中和编码解码问题相关的两个类：

***InputStreamReader***：字节流到字符流的桥梁

***OutPutStreamWriter***：字符流到字节流的桥梁

```java
public static void main(String[] args) throws IOException {

    OutputStreamWriter ouw = new OutputStreamWriter(new FileOutputStream("F:\\javaLearn\\IO\\bo.txt"),"GBK");
    InputStreamReader inr = new InputStreamReader(new FileInputStream("F:\\javaLearn\\IO\\bo.txt"),"GBK");
    ouw.write("中国");
    ouw.close();
    int by;
    while((by=inr.read())!=-1){
        System.out.print((char)by);
    }
    inr.close();
}
```

`注意：使用字符流对一个文件既读既写时，写完后flush刷新（close时会先刷新资源），在读文件。因为字符流底层使用缓冲字节流。在使用字节流既读既写时，没有这个问题。`

### 字符流写数据5种方式

<img src="F://javaLearn//IO//IO/pic/2.png">

### 字符流读数据2种方式

<img src="F:\javaLearn\IO\IO\pic\3.png">

### 便捷

不需要指定编码集时，使用默认的，

可将InputStreamReadee		->			FileReader

OutputStreamWriter				->			FileWriter

### 字符缓冲流

***BufferedWriter***：将文本写入字符输出流

***BufferedReader***：从字符输入流读取文本

功能：

```java
public void newLine()
作用：写一行行分隔符，行分隔符由系统属性决定
```

```java
public String readLine()
作用：读一行文字，不读换行符号
当返回null时，读完文件
 
```



```java
public static void main(String[] args) throws IOException {
    BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter("F:\\javaLearn\\IO\\fo.txt"));
    BufferedReader bufferedReader = new BufferedReader(new FileReader("F:\\javaLearn\\IO\\fo.txt"));
    for(int i=0;i<10;i++){
        //使用字符流写数据的三步
        bufferedWriter.write("hello"+i);
        bufferedWriter.newLine();
        bufferedWriter.flush();
    }
    String line;
    while((line=bufferedReader.readLine())!=null){
        System.out.println(line);
    }
    //3
    bufferedReader.close();
    bufferedWriter.close();


}
```

## IO流小结

<img src="F:\\javaLearn\\IO\\IO\\pic\\IO.png">

<img src="F:\\javaLearn\\IO\\IO\\pic\\IO2.png">