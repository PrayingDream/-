#### Java异常处理

**检查性异常：**最具代表的检查性异常是用户错误或问题引起的异常，这些异常在编译时强制要求程序员处理。例如要打开一个不存在文件时，一个异常就发生了，这些异常在编译时不能被简单地忽略。

这类异常通常使用 **try-catch** 块来捕获并处理异常，或者在方法声明中使用 **throws** 子句声明方法可能抛出的异常。

```java
try {
    // 可能会抛出异常的代码
} catch (IOException e) {
    // 处理异常的代码
}
```

或者:

```java
public void readFile() throws IOException {
    // 可能会抛出IOException的代码
}
```

**运行时异常：** 这些异常在编译时不强制要求处理，通常是由程序中的错误引起的，例如 NullPointerException、ArrayIndexOutOfBoundsException 等，这类异常可以选择处理，但并非强制要求。

```java
try {
    // 可能会抛出异常的代码
} catch (NullPointerException e) {
    // 处理异常的代码
}
```

- **错误：** 错误不是异常，而是脱离程序员控制的问题，错误在代码中通常被忽略。例如，当栈溢出时，一个错误就发生了，它们在编译也检查不到的。

##### Java提供的支持异常处理的关键子和类

- **try**：用于包裹可能会抛出异常的代码块。
- **catch**：用于捕获异常并处理异常的代码块。
- **finally**：用于包含无论是否发生异常都需要执行的代码块。
- **throw**：用于手动抛出异常。
- **throws**：用于在方法声明中指定方法可能抛出的异常。
- **Exception**类：是所有异常类的父类，它提供了一些方法来获取异常信息，如 **getMessage()、printStackTrace()** 等。

#### Exception类的层次

所有的异常类是从 java.lang.Exception 类继承的子类。
Exception 类是 Throwable 类的子类。除了Exception类外，Throwable还有一个子类Error 。
Java 程序通常不捕获错误。错误一般发生在严重故障时，它们在Java程序处理的范畴之外。
Error 用来指示运行时环境发生的错误。
例如，JVM 内存溢出。一般地，程序不会从错误中恢复。
异常类有两个主要的子类：IOException 类和 RuntimeException 类。

![exception-hierarchy](https://s2.loli.net/2024/07/17/89BjHswRn51qXde.png)