![在这里插入图片描述](https://s2.loli.net/2024/07/17/1ztLBMwSeXmqT9G.png)

#### BufferedReader类

##### 概述：

BufferedReader是Java I/O中的一个类，它是一个带缓冲区的字符输入流，用于从字符输入流中读取字符。它提供了一种逐行读取文本文件的方法，可以轻松地读取大量文本数据，并且可以通过使用缓冲区来提高读取效率。它的主要作用是读取文本文件中的字符数据，可以读取文件中的每一行数据，是Java I/O中常用的数据读取类之一。BufferedReader类只能读取字符类型的数据，如果需要读取其他类型的数据需要进行类型转换。

##### 优点：

- 数据缓存：通过BufferedReader类的缓存机制，可以在读取文本数据时，提高读取效率。
- 读取方式：BufferedReader类提供以行为单位读取数据的方法，可以方便地逐行解析文本数据。

##### 缺点：

- 由于`BufferedReader`是基于字符流的，对于读取二进制文件并不适用，不适合处理字节流。
- 缓冲区的大小是固定的，如果输入流的内容超过了缓冲区大小，会导致缓冲区溢出。
- 当BufferedReader类读取数据时，如果发生异常或读取过程中程序意外终止，可能会导致数据丢失。

#### 类代码方法介绍

##### 构造方法

```java
public BufferedReader(Reader in)
```

创建一个缓冲字符输入流对象，并选择指定字符输入流对象in作为其实际数据源。

```java
public BufferedReader(Reader in, int size)
```

创建一个缓冲字符输入流对象，并指定字符输入流对象in作为其实际数据源，同时指定缓冲区大小size。

##### 成员方法

```java
public String readLine() throws IOException
```

从缓存中读取一行文本，并返回String类型的结果。

```java
public int read() throws IOException
```

从缓存中读取一个字符，并返回读取的字符的ASCII码值，如果已到达流的末尾，则返回-1。

```java
public int read(char[] cbuf, int off, int len) throws IOExpextion
```

从缓存中读取字符，并将读取的字符复制到指定的字符数组cbuf中，偏移量为off，长度为len。返回实际读取的字符数。

```java
public void close() throws IOExpection
```

关闭字符流。

##### 源代码解析

**BufferedReader**类是Java IO库中的一员，其源代码实现了字符缓冲输入流。它继承自Reader类，内部通过一个字符数组char[]实现了字符的缓冲读取功能。

具体实现原理如下：

```java
public class BufferedReader extends Reader {
    // 内部字符数组，用于存放从输入流中读取的字符
    private char[] cb;
    // 缓冲区的有效长度
    private int nChars;
    // 下一个字符索引
    private int nextChar;
    // 输入流对象
    private Reader in;
    // 缓冲区默认大小
    private static int defaultCharBufferSize = 8192;
    
    // 构造方法，创建一个大小为默认缓冲区大小的 BufferedReader 对象
    public BufferedReader(Reader in) {
        this(in, defaultCharBufferSize);
    }
    
    // 构造方法，创建一个指定缓冲区大小的 BufferedReader 对象
    public BufferedReader(Reader in, int sz) {
        super(in);
        ...
    }
    
    ...
}

```

在BufferedReader类的实现中，主要包含了缓存区数组`cb[]`，以及`nChars`、`nextChar`两个指针，用于标记缓存区中待读取字符的位置，`fill()`方法用于填充缓冲区，`readLine()`方法用于从缓冲区中读取一行文本。

##### 应用场景案例

BufferedReader类适用于需要高效读取字符输入流的场景，比如从文件中逐行读取文本内容、读取网络数据流等。

以下是一个使用BufferedReader读取文件内容的例子：

```java
    public static void main(String[] args) {
        File file = new File("./template/fileTest.txt");
        try (BufferedReader reader = new BufferedReader(new FileReader(file))) {
            String line;
            //读取读取文件内容并打印
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

```

**执行结果:**

![在这里插入图片描述](https://s2.loli.net/2024/07/17/8igctwDmMuO9kex.png)

##### 类代码方法

以下是**BufferedReader**类中常用的方法：

- **int read()**: 读取单个字符并返回。
- **int read(char[] cbuf, int off, int len)**: 读取字符到指定字符数组中的指定位置，并返回读取字符的数量。
- **String readLine()**: 读取一行文本并返回，若到达文件结尾则返回null。
- **long skip(long n)**: 跳过指定数量的字符。
- **boolean markSupported()**: 判断输入流是否支持mark和reset方法。
- **void mark(int readAheadLimit)**: 在当前位置做标记，以供后续reset方法调用。
- **void reset()**: 将位置重置到最近的标记位置。

##### 测试用例

为了验证BuffferedReader类的功能和性能，可以编写如下测试用例：

**测试代码**

```java
package com.example.javase.io.bufferedReader;

import java.io.*;
import java.net.URL;

public class BufferedReaderTest {

    //读取指定文件内容
    public static void testReadFile() {
        try {
            BufferedReader reader = new BufferedReader(new FileReader("./template/fileTest.txt"));
            String line = reader.readLine();
            System.out.println(line);
            reader.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    //读取url内容
    public static void testReadURL() {
        try {
            BufferedReader reader = new BufferedReader(new InputStreamReader(new URL("https://www.baidu.com/").openStream()));
            String line = reader.readLine();
            System.out.println(line);
            reader.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        testReadFile();
        testReadURL();
    }
}

```

##### 测试代码说明

针对该类中我们定义了两个方法：testReadFile() 和 testReadURL()，如下我具体给同学们分析一下，以增强大家对此BufferedReader类的运用及掌握。

  testReadFile() 方法用于读取指定文件的内容，使用了 BufferedReader 和 FileReader 两个类。其中，BufferedReader 是带缓冲区的字符输入流，用来读取文本和字符数据，可以大幅提高读取效率。而 FileReader 则是文件读取流，用来读取文件内容。在方法中，先创建 BufferedReader 对象，指定其参数为 FileReader 对象，即读取指定文件的内容。然后使用 readLine 方法读取一行数据，并打印出来。最后关闭 BufferedReader 流。

  testReadURL() 方法用于读取指定 URL 的内容，使用了 BufferedReader、InputStreamReader和 URL 三个类。其中，InputStreamReader是字节流通向字符流的桥梁，用来将字节流转化为字符流。URL 类则代表一个统一资源定位符，用来定位互联网上的资源。在方法中，先创建 BufferedReader 对象，指定其参数为 InputStreamReader 对象和 URL 对象。然后使用 readLine 方法读取一行数据，并打印出来。最后关闭 BufferedReader 流。

  最后，在 main 方法中，调用 testReadFile() 和 testReadURL 方法，执行方法进行内容读取。
