## 一、IDEA用法

```java
输入psvm或main可快速生成

public static void main(String[] args) {
}
```



```java
输入sout，可快速生成
System.out.println();
```



```bash
Alt+(Fn)+Insert 自动生成

创建一个实体类应该需要的
* 构造函数
	* 无参构造函数
	* 有参构造函数
* Getter和Setter
* toString
```

![image-20220910204107048](http://doc.xjfyt.top/markdown_img/20220910204213.png)

## 二、Java依赖

1. JDBC 连接数据库用的，较为底层；
2. Gson 将Java对象转化为json字符串的；
3. Lombok 提供一些注解；
4. MyBatis、MyBatis-plus 都是连接数据库用的，顶层API；

