### 一、Spring

#### 1、简介

Spring是一个开源的框架，Spring 为简化企业级开发而生，使用 Spring、JavaBean 就可以实现很多以前要靠 EJB 才能实现的功能，同样的功能，在EJB中要通过繁琐的配置和复杂的代码才能够实现，而使用Spring 却非常的优雅和简洁。

##### [1]Spring的核心:

- IOC(Inversion of Control)：控制反转，即对象创建的问题
- AOP(Aspect Oriented Programming)：面向切面编程

##### [2]IOC和DI

- IOC：控制反转，把传统上由程序代码直接操控的对象的调用权交给容器，通过容器来实现组件的装配和管理，本质就是对象的调用权的转移，将创建对象的权利交给了容器
- DI：依赖注入，在运行期，由外部容器动态地将依赖对象注入到另一个对象中

IOC描述的是一种思想，DI是对IOC思想的具体实现

##### [3]Spring优点

- 高内聚，低耦合

  ​	Spring 就是一个大工厂，可以将所有对象创建和依赖关系维护都交给Spring来管理

- AOP 编程的支持

  ​	Spring 提供面向切面编程，可以方便的实现对程序进行权限拦截、运行监控等功能

- 声明事物的支持

  ​	只需要通过配置就可以完成对事物的管理，而无需手动编程

- 方便程序的测试

  ​	Spring 对 Junit4支持，可以通过注解来测试 Spring 程序

- 方便集成各种优秀的框架

  ​	Spring 不排斥各种其他开源框架，其内部提供了对各种框架的直接支持

- 降低JavaEE API 的使用难度

  ​	Spring 对 JavaEE 开发中非常难用的一些 API（JDBC、JavaMail、远程调用等），都提供了封装，降低了开发难度

#### 2、搭建Spring IOC环境

##### [1]创建maven版的Java工程

搭建好后目录结构如下：

```
ProjectName/
    | ----src/ |
    | -------- || main/ |
	| ----- ||     |  | java/ |
	| --- ||     | resources/ |
	| --- | ---------- || test/ |
	| ----- ||     | java/ |
	| --- ||  |resources/
    |----pom.xml
```

##### [2]导入jar包

在pom.xml中加入相关依赖，导入Spring相关jar包（为了方便后面进行测试，这里导入了测试jar包）

```
<packaging>jar</packaging>
 
    <dependencies>
        <!--导入Spring相关jar包-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <!--导入测试相关jar包-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>
```

##### [3]创建Spring配置文件

在 resources 目录下创建配置文件，内容如下（可以从Spring官网获取）

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

##### [4]创建实体类

创建一个简单的HelloWorld类

```
public class Helloworld {
    private String name;
    public Helloworld(){
        System.out.println("创建对象");
    }
    public void setName(String name) {
        System.out.println("调用方法");
        this.name = name;
    }
    public void sayHello(){
        System.out.println("Hello!" + name);
    }
}
```

##### [5]对类进行配置

配置之后如下

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--配置bean
        id属性：给当前bean起一个名称，该名称必须保证是唯一的
        class属性：设置bean的全类名-->
    <bean id="helloworld" class="com.LSTAR.Helloworld">
        <!--给属性赋值
            name属性：设置bean属性名
            value属性：设置bean属性值-->
        <property name="name" value="LSTAR"></property>
    </bean>
</beans>
```

