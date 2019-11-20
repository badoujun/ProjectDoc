# 1. ConfigurationProperties

> 有时候有这样子的情景，我们想把配置文件的信息，读取并自动封装成实体类，这样子，我们在代码里面使用就轻松方便多了，这时候，我们就可以使用@ConfigurationProperties，它可以把同类的配置信息自动封装成实体类

## 1.1 定义属性文件

```properties
connection.username=admin
connection.password=kyjufskifas2jsfs
connection.remoteAddress=192.168.1.1
```

## 1.2 定义实体类

```java
@Data
@Component
//获取前缀为connection的属性，属性去匹配去除前缀的字符串，相同就赋予值
@ConfigurationProperties(prefix="connection")
public class ConnectionSettings {

    private String username;
    private String remoteAddress;
    private String password ;
    
}
```

