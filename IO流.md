# 一、IO流：

## 1、文件流：

### 1.1：文件在程序中是以流的形式来操作的

- 流：数据在数据源（文件）和程序（内存）之间经历的路径
- 输入流：数据从数据源（文件）到程序（内存）的路径
- 输出流：数据从程序（内存）到数据源（文件）的路径![image-20211217211033008](../java%E8%BD%AF%E4%BB%B6/TyporaPhoto/image-20211217211033008.png)

### 1.2：常用的文件操作：

- 创建文件对象相关构造器和方法：
  - new File(String pathname)  // 根据路径构建一个File对象
  - new File(File parent,String child)  // 根据父目录文件+子路径构建
  - new File(String parent,String child) //根据父目录+子路径构建

```java
public class Test01 {
    public static void main(String[] args) {

//        Test_.creat01();
//        Test_.creat02();
        Test_.creat03();
    }

//        @Test

}
class Test_{
    // 方式一   new File(String pathname)
    @Test
    public static void creat01() {
        String filePath = "D:\\IO_test\\news.txt";
        File file = new File(filePath);
        try {
            file.createNewFile();
            System.out.println("文件创建成功...");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

//    方式二   new File(File parent,String child) //根据父目录文件 + 子路径构建
    // D:\\IO_test
    public static void creat02(){
        File file = new File("D:\\IO_test");
        String filename = "news2.txt";
        File file1 = new File(file, filename);

        try {
            file1.createNewFile();
            System.out.println("文件创建成功....");
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    // 方式三   new File(String parent,String child)   // 根据父目录 + 子路径构建
    public static void creat03(){
        String parentPath = "D:\\IO_test";
        String fileName = "news3.txt";
        File file = new File(parentPath, fileName);
        try {
            file.createNewFile();
            System.out.println("文件news3创建成功....");
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
```

- 获取文件的相关信息

```java
// 获取文件信息
    public static void info(){
        //先创建文件对象
        File file = new File(" D:\\IO_test\\news1.txt");

        // 调用对应方法   获取相应的信息
        // getName   getAbsolutePath  getParent  length  exists  isFile  isDirectory
        System.out.println("文件名" + file.getName()); // getName
        System.out.println("文件绝对路径： " + file.getAbsolutePath()); // getAbsolutePath
        System.out.println("文件父级目录：" + file.getParent());  //getParen
        System.out.println("文件大小字节 : " + file.length());       //length
        System.out.println("文件是否存在： " + file.exists());     //exists
        System.out.println("是不是一个文件：" + file.isFile());     //isFile
        System.out.println("是不是一个目录：" + file.isDirectory());    //isDirectory
    }
```

- 目录的操作和文件的删除
  - mkdir创建一级目录、mkdirs创建多级目录、delete删除空目录或文件

```java
public static void main(String[] args) {
//        m1();
//        m2();
        m3();
    }


    public static void m1() {
        // 判断：d:\\IO_test\\news1.txt 是否存在  如果存在 就删除
        String filePath = "d:\\IO_test\\news1.txt";
        File file = new File(filePath);
        if (file.exists()) {
            if (file.delete()) {
                System.out.println("文件news1删除成功");
            } else {
                System.out.println("删除失败");
            }
        } else {
            System.out.println("文件不存在..");
        }
    }

    // 判断D:\\IO_test_ 是否存在，存在就删除，否则提示不存在
    // 在java编程中，目录也被当作文件
    public static void m2(){
        String filePath = "d:\\IO_test_";
        File file = new File(filePath);
        if (file.exists()){
            if (file.delete()) {
                System.out.println("文件目录删除成功");
            } else {
                System.out.println("删除失败");
            }
        }else {
            System.out.println("文件目录不存在..");
        }
    }

    public static void m3(){
        // 判断文件目录是否已经存在，如果存在就提示已经存在   如果没有存在就创建文件目录

        // 创建多级目录  使用 mkdirs   创建单级目录时使用 mkdir即可
        String directoryPath = "d:\\IO_test\\test";
        File file = new File(directoryPath);
        if (file.exists()){
            System.out.println(directoryPath + "已经存在...");
        }else{
            if (file.mkdir()){
                System.out.println(directoryPath + "创建成功...");
            }else{
                System.out.println(directoryPath + "创建失败...");
            }
        }
    }
```

### 1.3、IO流原理：

- I/O是Input/Output的缩写，I/O技术是非常实用的技术，用于处理数据传输
- Java程序中，对于数据的输入和输出操作以“流（stream）”的方式进行
- java.io 包下提供了各种“流”类和接口，用以获取不同种类的数据，并通过方法输入或输出数据
- 输入 input：读取外部数据（将磁盘、光盘等存储设备的数据）到程序（内存）中
- 输出output：将程序（内存）数据输出到磁盘、光盘等存储设备中

### 1.4、流的分类：

- 按操作数据单位不同分为：字节流（8 bit）二进制文件、字符流（按字符）文本文件

- 按数据流的流向不同分为：输入流、输出流

- 按流的角色不同分为：节点流，处理流/包装流

  ![image-20211219103512412](../java%E8%BD%AF%E4%BB%B6/TyporaPhoto/image-20211219103512412.png)

- 
  - java的IO流共涉及40多个类，实际上非常规则，都是从以上四个抽象基类派生的
  - 由着四个类派生出来的子类名称都是以其分类名作为子类的后缀



### 1.5、常用的类：

#### 1.5.1、InputStream：

InputStream抽象类是所有类字节输入流的超类

- InputStream：字节输入流 常用子类：

  - FileInputStream ：文件输入流

  ```java
      public static void main(String[] args) {
          readFile01();
      }
      public static void readFile01(){
          String filePath = "d:\\IO_test\\hello.txt";
          FileInputStream fileInputStream = null;
          int readData = 0;
          //  创建了FileInputStream对象  用于读取内容
          try {
              fileInputStream = new FileInputStream(filePath);
  
              // 从该输入流中取出一个字节的数据。如果没有输入可用时，终止此方法
              // 如果返回的是-1 ，则表示读取完毕
              while((readData = fileInputStream.read()) != -1){
                  System.out.print((char)readData);
              }
          } catch (IOException e) {
              e.printStackTrace();
          }finally {
              // 关闭文件流  释放资源
              try {
                  fileInputStream.close();
              } catch (IOException e) {
                  e.printStackTrace();
              }
          }
      }
  
  // 可以使用 byte[] 名字 = new byte[8]  进行优化
  ```

  

  - BufferedInputStream： 缓冲字节输入流
  - ObiectInputStream： 对象字节输入流


#### 1.5.2、FileOutputStream：

- ```java
      public static void main(String[] args) {
          writeFile();
      }
      public static void writeFile(){
          String filePath = "d:\\IO_test\\a.txt";
          FileOutputStream fileOutputStream = null;
  
          try {
              // 1、fileOutputStream = new FileOutputStream(filePath);创建方式，当写入内容时，会覆盖之前的内容
              // 2、fileOutputStream = new FileOutputStream(filePath,true);创建方式，当写入内容时，是追加到文件后面，不会造成覆盖
              fileOutputStream = new FileOutputStream(filePath);
              // 写入一个字符
              fileOutputStream.write('H');
              // 写入字符串
              String str = "Hello World";
              fileOutputStream.write(str.getBytes());
              System.out.println("添加成功...");
              //  添加文字
              String test = "你好啊";
              fileOutputStream.write(test.getBytes(StandardCharsets.UTF_8));
  
          } catch (IOException e) {
              e.printStackTrace();
          } finally {
              try {
                  fileOutputStream.close();
              } catch (IOException e) {
                  e.printStackTrace();
              }
          }
      }
  ```

#### 1.5.3、拷贝：

```java
public static void main(String[] args) {
        copy();
    }
    public static void copy(){
        // 完成拷贝，将D:\\idea\\壁纸.lnk  拷贝到D:\\IO_test
        // 思路：
        // 1、创建文件的输入流，将文件读取到程序中
        // 2、创建文件的输出流，将读取的文件数据写入指定的文件

        String srcFilePath = "D:\\idea\\壁纸.lnk";
        String dateFilePath = "D:\\IO_test\\壁纸.lnk";

        FileInputStream fileInputStream = null;
        FileOutputStream fileOutputStream = null;

        try {
            fileInputStream = new FileInputStream(srcFilePath);
            fileOutputStream = new FileOutputStream(dateFilePath);
            // 定义一个数组来加快读取速率
            byte[] buf = new byte[1024];
            int readLen = 0;
            while((readLen = fileInputStream.read()) != -1){
                // 读取到后就写入文件  通过fileOutputStream
                // 即边读边写
                fileOutputStream.write(buf,0, readLen);//一定要使用这个方法
            }
            System.out.println("拷贝成功...");
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                fileInputStream.close();
                fileOutputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
```

#### 1.5.4、FileReader和FileWriter介绍：

- FileReader 和 FileWriter 是字符流，即按照字符来操作IO

- FileReader相关方法：

  - new FileReader(File/String)

  - read：每次读取单个字符，返回该字符，如果到文件末尾则返回-1

    - ```java
          public static void main(String[] args) {
              String filePath = "d:\\IO_test\\b.txt";
              int data = 0;
              // 创建FileReader对象
              FileReader fileReader = null;
              try {
                  fileReader = new FileReader(filePath);
                  // 循环读取  使用read  单个字符读取
                  while((data = fileReader.read()) != -1){
                      System.out.print((char)data);
                  }
              } catch (IOException e) {
                  e.printStackTrace();
              } finally {
                  try {
                      fileReader.close();
                  } catch (IOException e) {
                      e.printStackTrace();
                  }
              }
          }
      ```

    - 

  - read(char[])：批量阅读多个字符到数组，返回读取到的字符数，如果到文件末尾则返回-1

    - ```java
          public static void main(String[] args) {
              String filePath = "d:\\IO_test\\b.txt";
              int readLen = 0;
              char[] buf = new char[8];
              // 创建FileReader对象
              FileReader fileReader = null;
              try {
                  fileReader = new FileReader(filePath);
                  // 循环读取  使用read(buf) ,  返回的是实际读取到的字符数
                  while((readLen = fileReader.read(buf)) != -1){
                      System.out.print(new String(buf,0,readLen));
                  }
              } catch (IOException e) {
                  e.printStackTrace();
              } finally {
                  try {
                      fileReader.close();
                  } catch (IOException e) {
                      e.printStackTrace();
                  }
              }
      
          }
      ```

    - 

  - 相关API：

    - new String(char[]):将char[] 转换成String
    - new String(char[],off,len)：将char[]的指定部分转换成String

![image-20211219161139086](../java%E8%BD%AF%E4%BB%B6/TyporaPhoto/image-20211219161139086.png)

- FileWriter常用方法：

  - new FileWriter(File/String)	:覆盖模式，相当于流的指针在首端
  - new FileWriter(File/String,true)    :追加模式，相当于流的指针在尾端
  - write(int)    :写入单个字符
  - write(char[])   :写入指定数组
  - write(char[],off,len)   :写入指定数组的指定部分
  - write(String)    : 写入整个字符串
  - write(String,off,len)   : 写入字符串的指定部分

  ```java
      public static void main(String[] args) {
          // 创建FileWriter对象
          String filePath = "d:\\IO_test\\e.txt";
          FileWriter fileWriter = null;
  
          char[] chars = {'a','b','c'};
          try {
              fileWriter = new FileWriter(filePath);  // 默认是覆盖写入
  //                - write(int)    :写入单个字符
              fileWriter.write('H');
  //                - write(char[])   :写入指定数组
              fileWriter.write(chars);
  //                - write(char[],off,len)   :写入指定数组的指定部分
              fileWriter.write("学习java".toCharArray(),0,3);// 写入的是从0开始到第三个  即"学习J"
  //                - write(String)    : 写入整个字符串
              fileWriter.write("你好北京~");
  //                - write(String,off,len)   : 写入字符串的指定部分
              fileWriter.write("上海天津",0,2);  // 写入从0开始 两个字符
  
          } catch (IOException e) {
              e.printStackTrace();
          } finally {
              // 对应fileWriter  一定要关闭流或者flush才能真正把数据写入到文件中
              try {
                  fileWriter.close();
                  System.out.println("添加完成...");
              } catch (IOException e) {
                  e.printStackTrace();
              }
          }
  
      }
  ```

  

  - 相关API
    - String类 ： toCharArray  ： 将String转换成char[]

注意：FileWriter使用之后必须要关闭（close）或刷新（flush），否则写入不到指定的文件

## 2、节点流和处理流：

### 2.1、基本介绍：

- 节点流可以从一个特定的数据源读写数据，如FileReader、FileWriter等

![image-20211219170517528](../java%E8%BD%AF%E4%BB%B6/TyporaPhoto/image-20211219170517528.png)

- 处理流（也叫包装流）是”连接“在已存在的流（节点流或处理流）之上，位程序提供更为强大的读写功能，如BufferedReader、BufferedWriter等

![image-20211219170528663](../java%E8%BD%AF%E4%BB%B6/TyporaPhoto/image-20211219170528663.png)

### 2.2、节点流和处理流的区别和联系：

- 节点流是底层流/低级流，直接跟数据源相接
- 处理流（包装流）包装节点流，既可以消除不同节点流的实现差异，也可以提供更方便的方法来完成输入输出
- 处理流（包装流）对节点流进行包装，使用了修饰器设计模式，不会与数据源相接（模拟修饰器设计模式）

### 2.3、处理流的功能主要体现在以下两个方面：

- 性能的提高：主要以增加缓冲的方式来提高输入输出的效率
- 操作的便捷：处理流可能提供了一系列便捷的方法来一次输入输出大批量数据，使用更加灵活方便

### 2.4、处理流-BufferedReader和BufferedWriter：

- BufferedReader 和 BufferedWriter 属于字符流，是按照字符来读取数据的（文本文件）
- 关闭时，只需要关闭外层流即可

```java
    //  演示读取
	public static void main(String[] args) throws Exception {

        String filePath = "d:\\IO_test\\b.txt";
        // 创建 BufferedReader
        BufferedReader bufferedReader = new BufferedReader(new FileReader(filePath));
        // 读取   按行
        String line;
        while((line = bufferedReader.readLine()) != null){
            System.out.println(line);
        }
        //关闭流，这里只需要关闭BufferedReader   而不是FileReader   因为底层会自动关闭 节点流
        bufferedReader.close();
    }
```

- 演示写入

  - ```java
        public static void main(String[] args) throws Exception {
    
            String filePath = "d:\\IO_test\\ok.txt";
            // 创建 BufferedWriter
            // FileWriter(filePath,true)  是以追加方式写入
            //FileWriter(filePath)   是覆盖写入
            BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(filePath,true)); //使用的使FileWriter(filePath,true) 在文件末尾添加 没有true时会产生覆盖
            // 写入  使用.writer
            bufferedWriter.write("继续学习java");
            System.out.println("添加成功..");
    
    
            //关闭流，这里只需要关闭BufferedReader   而不是FileReader   因为底层会自动关闭 节点流
            bufferedWriter.close();
        }
    ```

- 演示Copy

  - ```java
        public static void main(String[] args) throws Exception{
    
            // 说明 :
            // - BufferedReader 和 BufferedWriter 是字符操作
            // - 不要使用 BufferedReader 和 BufferedWriter 去操作二进制文件[声音、视频等]，可能造成文件的损坏
            String srcPath = "d:\\IO_test\\b.txt";
            String destPath = "d:\\IO_test\\ab.txt";
    
            BufferedReader br = null;
            BufferedWriter bw = null;
            try {
                br = new BufferedReader(new FileReader(srcPath));
                bw = new BufferedWriter(new FileWriter(destPath));
                String line;
                // 先读取后写入
                // 说明： readLine 读取一行的内容，但是没有换行
                while ((line = br.readLine()) != null){
                    bw.write(line);
                    // 插入换行符
                    bw.newLine();
                }
                System.out.println("拷贝完毕...");
            } catch (FileNotFoundException e) {
                e.printStackTrace();
            } finally {
                // 关闭流
                try {
                    if(br != null){
                        br.close();
                    }
                    if (bw != null){
                        bw.close();
                    }
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    ```

### 2.5、处理流- BufferedInputStream 和 BufferedOutputStream：

- 介绍： BufferedInputStream是字节流，在创建BufferedInputStream时，会创建一个内部缓冲区数组

- 介绍： BufferedOutStream是字节流，实现缓冲的输出流，可以将多个字节写入底层输入流中，而不必对每次字节写入调用底层系统

![image-20211220102741482](../java%E8%BD%AF%E4%BB%B6/TyporaPhoto/image-20211220102741482.png)

- 进行文件（照片、视频等）的拷贝

  - ```java
        public static void main(String[] args) {
            // 进行二进制文件拷贝
            String srcPath = "D:\\桌面\\下载\\女神snh48宋昕冉4k高清美女壁纸_彼岸图网.jpg";
            String destPath = "d:\\IO_test\\test.jpg";
            // 创建BufferedOutputStream 和 BufferedInputStream 对象
            BufferedInputStream bis = null;
            BufferedOutputStream bos = null;
            try {
                bis = new BufferedInputStream(new FileInputStream(srcPath));
                bos = new BufferedOutputStream(new FileOutputStream(destPath));
    
                // 循环读取文件  并写入文件中
                byte[] buff = new byte[1024];
                int readLine = 0;
                // 当返回-1时就表示文件读取完毕
                while((readLine = bis.read(buff)) != -1 ){
                    bos.write(buff,0,readLine);
                }
                System.out.println("文件拷贝完成...");
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                // 关闭流
                try {
                    if (bis != null){
                        bis.close();
                    }
                    if (bos != null){
                        bos.close();
                    }
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    ```

### 2.6、处理流 - ObjectInputStream 和 ObjectOutputStream：

#### 2.6.1、	序列化和反序列化

- 序列化就是在保存数据时，保存数据的值和数据类型
- 反序列化就是在恢复数据时，恢复数据的值和数据类型
- 需要让某个对象支持序列化机制，则必须让其类是可序列化的，为了让某个类是可序列化的，该类必须实现一下两个接口之一：
  - Serializable   // 这是一个标记接口
  - Externalizable  // 该接口有方法需要实现，因此我们一般实现上面的Serizable接口
- 注意：
  - 读写顺序要一致
  - 要求实现序列化或反序列化对象，需要实现 Serializable
  - 序列化的类中建议添加SerialVersionUID，为了提高版本的兼容性
  - 序列化对象时，默认将里面所有的属性都进行序列化，但除了static或transient修饰的成员
  - 序列化对象时，要求里面的属性的类型也需要实现序列化接口
  - 与劣化具备可继承性，也就是如果某类已经实现了序列化，则它的所有的子类也已经默认实现了序列化


#### 2.6.2、功能：

- 提供了对基本类型或对象类型的序列化和反序列化的方法

- ObjectOutputStream 提供序列化功能

  - ```java
    public class ObjectOutputStream_ {
        public static void main(String[] args) throws Exception {
            // 序列化后，保存的文件格式不是存文本，而是按照他的格式来保存的
            String filePath = "d:\\IO_test\\data.bat";
    
            ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(filePath));
            // 序列化数据到 d:\IO_test\data.bat
            oos.write(100); // int -> Integer (实现了Serialzable)
            oos.writeBoolean(true); // boolean -> Boolean (实现了Serialzable)
            oos.writeDouble(9.5);  // double -> Double (实现了Serialzable)
            oos.writeChar('H');    // char -> Character (实现了Serialzable)
            oos.writeUTF("学习Java");  // String
            // 保存 Dog 对象
    
            oos.writeObject(new Dog("旺财",10));
    
            // 关闭
            oos.close();
            System.out.println("保存完毕..");
        }
    }
    
    class Dog implements Serializable {
        private String name;
        private int age;
    
        public Dog(String name, int age) {
            this.name = name;
            this.age = age;
        }
    }
    ```

- ObjectInputStream 提供反序列化功能

  - ```java
    // 第一个类
    public class Dog implements Serializable {
    
        private String name;
        private int age;
    
        public Dog(String name, int age) {
            this.name = name;
            this.age = age;
        }
    
        public String getName() {
            return name;
        }
    
        public void setName(String name) {
            this.name = name;
        }
    
        public int getAge() {
            return age;
        }
    
        public void setAge(int age) {
            this.age = age;
        }
    }
    ```

  - 反序列化 

    - ```java
      public static void main(String[] args) throws IOException, ClassNotFoundException {
              // 指定反序列话的文件
              String filePath = "d:\\IO_test\\data.bat";
      
              ObjectInputStream ois = new ObjectInputStream(new FileInputStream(filePath));
      
              //注意：
              //- 读取（反序列化）顺序需要和保存数据（序列化）的顺序一致
              //- 否则就会出现异常
              System.out.println(ois.readInt());
              System.out.println(ois.readBoolean());
              System.out.println(ois.readDouble());
              System.out.println(ois.readChar());
              System.out.println(ois.readUTF());
      
              // Dog 的编译类型是 Object
              Object dog = ois.readObject();
              System.out.println("运行类型 = " + dog); // 底层 Object -> Dog
      
              //  重要细节：：：
              // - 如果我们想要调用Dog的方法，需要向下转型
              // - 需要我们将Dog类的dinginess，拷贝到可以引用的位置
              Dog dog2 = (Dog) dog;
              System.out.println(dog2.getName());
      
              // 关闭流2  关闭外层流即可，内部会自动关闭FileInputStream
              ois.close();
          }
      }
      ```

### 2.7、标准输入出输出流：

![image-20211220154030433](../java%E8%BD%AF%E4%BB%B6/TyporaPhoto/image-20211220154030433.png)

 

### 2.8、转换流 - InputStreamReader 和 OutputStreamWriter：

介绍：

- InputStreamReader： Reader的子类，可以将InputStream（字节流）包装成Reader（字符流）
- OutputStreamWriter：Writer的子类，实现将OutputStream（字节流）包装成Writer（字符流）
- 当处理纯文本数据时，如果使用字符流效率更高，并且可以有效解决中文问题，所以建议将字节流转换成字符流
- 可以在使用时指定编码格式

### 2.9、打印流 - PrintStream 和 PrintWriter

- 打印流只有输出流，没有输入流

#### 2.9.1、字节打印流  PrintStream：

```java
// 字节打印流
PrintStream out = System.out;

out.print("hohn,hello");
// 因为print底层使用的是Write，所以我们可以直接调用write进行打印/输出
out.write("学java".getBytes());


out.close;


// 我们可以修改打印流输出的位置
// 1、  输出修改成到  "d:\\IO_test\\f1.txt"
// 2、  “hello,学java"  就会输出到d:\\IO_test\\f1.txt
System.setOut(new PrintStream("d:\\IO_test\\f1.txt"));
sout("hello,学java");
```

#### 2.9.2、字符打印流  printWrite:

![image-20211220164633679](../java%E8%BD%AF%E4%BB%B6/TyporaPhoto/image-20211220164633679.png)



## 3.Properties类（配置文件）：

### 3.1、介绍：

- 专门用于读写配置文件的集合类

  - 配置文件的格式：

    ​	键=值

    ​	键=值

- 键值对不需要有空格，值不需要用引号括起来。默认类型是String

- Properties的常见的方法：

  - load ： 加载配置文件的键值对到Properties对象
  - list ： 将数据显示到指定设备
  - getProperty(key) ： 根据建获取值
  - setProperty(key,value) ： 设置键值对到Properties对象（含有相同的key的话相当于修改）
  - store ： 将Properties中的键值对存储到配置文件，在idea中，保存信息到配置文件，如果含有中文，会存储为unicode码

```java
    public static void main(String[] args) throws IOException {
        // 使用Properties 类来读取mysql.properties

        //1、创建properties 对象
        Properties properties = new Properties();
        //2、加载指定配置文件
        properties.load(new FileReader("src\\mysql.properties"));
        //3、把k-v显示控制台
        properties.list(System.out);
        //4、根据key  获取对应的值
        String user = properties.getProperty("user");
        System.out.println(user);
    }

====================================================================================
    public static void main(String[] args) throws IOException {
        Properties properties = new Properties();
        // 创建新的properties文件
        properties.setProperty("charset","utf8");
        properties.setProperty("root","tom");
        properties.setProperty("pwd","123");
        // 将k-v 存储在文件中
        properties.store(new FileOutputStream("src\\mysql.properties02"),null); // 后面的这个null为注释 无特殊要求可以设置为null
        System.out.println("保存配置文件成功...");
    }
```

![image-20211220181822593](../java%E8%BD%AF%E4%BB%B6/TyporaPhoto/image-20211220181822593.png)

