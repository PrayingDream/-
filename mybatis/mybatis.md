### mybatis

**配置流程**

1. 在pom中添加MySQL和mybatis  注意版本
2. 在resources中创建Mybatis-config.xml文件 在[入门_MyBatis中文网](https://mybatis.net.cn/getting-started.html)复制核心设置 填写driver、url、username、password   driver的value 填写com.mysql.jdbc.Driver(mysql-connector-java 5中的)或com.mysql.cj.jdbc.Driver(6或以上)   url填写jdbc:mysql://url地址:接口/数据库名
3. 把数据库表中的数据封装成实体 在java下软件包中创建类(User)，按照表创建私有变量，添加构造器 和setter/getter方法 并重写toString()
4. 在java下项目软件包创建Mapper 创建接口(userMapper) 在resources中创建跟接口一样的文件目录 创建和接口名字相同的(userMaper.xml).xml文件
5. 在[入门_MyBatis中文网](https://mybatis.net.cn/getting-started.html)中复制**探究已映射的 SQL 语句**到与接口同名的xml文件中，将resultType的值修改为创建好的封装数据用的类(User)的地址  修改namespace的值为接口的地址
6. 将Mybatis-config.xml中的resource值也修改为为与接口同名的xml文件的地址(带.xml)
7. 在接口中直接创建方法 数据类型 + 方法名();  按ALT + 回车 直接在同名xml文件中添加查询方法

**使用方法**

```java
String resource = "mybatis-config.xml地址";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
userMapper users = sqlSession.getMapper(userMapper.class接口名字);
数据类型 变量名 = user.接口设置方法名(); //用于存储查询到的数据
```

**增删改查方法**

```java
    <!-- 插入数据，这里ID是自动递增的，所有不需要插入 -->
    <insert id="getInsert" parameterType="Users" >
    <!-- 数据库增加语句：#{}代表占位符 -->
        insert into users(id,name,age) values(#{id},#{name},#{age})
    </insert>

    <!-- 查询表中所有的数据 -->
    <select id="getUserList" resultType="com.anzhuo.bean.Users">
        select * from users
    </select>

    <!-- 根据ID查询表数据 -->
    <select id="getUsers" parameterType="int" resultType="com.anzhuo.bean.Users">
        select * from users where id=#{id}
    </select>

    <!-- 根据ID修改表中数据-->
    <update id="getUpdate" parameterType="com.anzhuo.bean.Users">
        update users set name=#{name},age=#{age} where id=#{id}
    </update>

    <!-- 根据ID删除表数据 -->
    <delete id="getDelete" parameterType="int">
        delete from users where id=#{id}
    </delete>
```

