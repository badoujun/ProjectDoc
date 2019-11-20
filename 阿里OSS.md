# 1.OSS介绍

```shell
对象存储服务（Object Storage Service，OSS）是一种海量、安全、低成本、高可靠的云存储服务，适合存放任意类型的文件。容量和处理能力弹性扩展，多种存储类型供选择，全面优化存储成本。
OSS 具有与平台无关的 RESTful API 接口，您可以在任何应用、任何时间、任何地点存储和访问任意类型的数据。
```

| 存储类型（Storage Class） | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| 标准                      | 提供高可靠、高可用、高性能的对象存储服务，能够支持频繁的数据访问 |
| 低频访问                  | 低频访问存储类型适合长期保存不经常访问的数据（平均每月访问频率 1 到 2 次） |
| 归档                      | 适合需要长期保存（建议半年以上）的归档数据                   |

| 相关概念              | 说明                                                         |
| --------------------- | ------------------------------------------------------------ |
| 存储空间（Bucket）    | 用于存储对象（Object）的容器，所有的对象都必须隶属于某个存储空间 |
| 对象/文件（Object）   | 对象由元信息（Object Meta）、用户数据（Data）和文件名（Key）组成 |
| 地域（Region）        | 地域表示 OSS 的数据中心所在物理位置                          |
| 访问域名（Endpoint）  | Endpoint 表示 OSS 对外服务的访问域名，不同地域的域名不同     |
| 访问密钥（AccessKey） | 指的是访问身份验证中用到的 AccessKeyId 和 AccessKeySecret    |

| 常用服务            | 说明                                                         |
| ------------------- | ------------------------------------------------------------ |
| 图片处理（IMG）     | 对存储在 OSS 上的图片进行格式转换、缩放、裁剪、旋转、添加水印等各种操作 |
| 云服务器（ECS）     | 提供简单高效、处理能力可弹性伸缩的云端计算服务               |
| 内容分发网络（CDN） | 将源站资源缓存到各区域的边缘节点，供您就近快速获取内容       |
| E-MapReduce         | 构建于 ECS 上的大数据处理的系统解决方案，方便您分析和处理自己的数据 |
| 媒体处理            | 将存储于 OSS 的音视频转码成适合在 PC、TV 以及移动终端上播放的格式 |

# 2.使用步骤（基于控制台）

![img](image\1566875489919_zh-CN.jpg)

## 2.1开通服务

```shell
1.登录阿里云官网
https://www.aliyun.com/?spm=a2c4g.11186623.2.11.e02c28bcAp4Q5m
2.鼠标移至产品，单击对象存储 OSS，打开OSS 产品详情页面
https://www.aliyun.com/product/oss?spm=5176.12825654.eofdhaal5.90.31652c4acan8O8&aly_as=ke-PxaCu
3.在OSS 产品详情页，单击立即开通
4.开通服务后，在OSS 产品详情页面单击管理控制台直接进入 OSS 管理控制台界面
'按小时计费，比如：标准型存储单价为0.12元/GB/月，则每GB每小时的单价为0.12元/30天/24小时。'
```

![1571035012286](image\1571035012286.png)

![1571035235615](image\1571035235615.png)

## 2.2创建空间

1.登录OSS管理控制台：https://oss.console.aliyun.com/overview

2.创建 Bucket

![1571035440266](image\1571035440266.png)

![1571035837505](image\1571035837505.png)

## 2.3上传文件

1. 登录[OSS管理控制台](https://oss.console.aliyun.com/)。
2. 在左侧存储空间列表中，单击目标存储空间。
3. 单击**文件管理 > 上传文件**。

![1571036166386](image\1571036166386.png)

| 功能     | 参数       | 说明                                                         |
| -------- | ---------- | ------------------------------------------------------------ |
| 上传路径 | 当前目录   | 将文件上传到当前目录                                         |
|          | 指定目录   | 将文件上传到指定目录，您需要输入目录名称。目录不存在会自动创建 |
| 文件ACL  | 继承Bucket | 文件的读写权限按Bucket的读写权限为准                         |
|          | 私有       | 对文件的所有访问操作需要进行身份验证                         |
|          | 公共读     | 可以对文件进行匿名读，对文件写操作需要进行身份验证           |
|          | 公共读写   | 所有人都可以对文件进行读写操作                               |
| 上传文件 | 无         | 将需要上传的一个或多个文件拖拽到此区域                       |

![1571036567987](image\1571036567987.png)

## 2.4下载文件

1. 登录[OSS管理控制台](https://oss.console.aliyun.com/)。
2. 在左侧存储空间列表中，单击目标存储空间名称

3. 在该存储空间概览页面中，单击**文件管理**页签

```shell
# 下载单个文件
1.单击目标文件右侧的更多 > 下载。
2.单击目标文件的文件名或其右侧的详情，在弹出的详情对话框中单击下载。
3.单击目标文件的文件名或其右侧的详情，在弹出的详情对话框中单击打开URL。
# 分享文件
单击目标文件的文件名或其右侧的详情，在弹出的详情对话框中单击复制URL，之后将复制的文件URL发送给对方即可
```

![1571036948800](image\1571036948800.png)

## 2.5删除文件

点击更多->删除

# 3.JAVA SDK

> SDK:软件开发工具包，在java中就是一个jar包
>
> 版本：基于OSS Java SDK 3.5.0编写

## 3.1安装SDK

```xml
<dependency>
    <groupId>com.aliyun.oss</groupId>
    <artifactId>aliyun-sdk-oss</artifactId>
    <version>3.5.0</version>
</dependency>
```

## 3.2获取accessKey

1. 登录[OSS管理控制台](https://oss.console.aliyun.com/)。

2. 点击头像->accessKey

   ![1571039354633](image\1571039354633.png)

   

3. 创建accesskey

   如果没有AccessKey就先选择继续使用AccessKey，就回出现如下界面

   ![1571040535459](image\1571040535459.png)

   ![1571040603192](image\1571040603192.png)

4. 选择子用户的方式提升安全性

> RAM （Resource Access Management）:是阿里云提供的权限管理系统。RAM主要的作用是控制账号系统的权限。您可以使用RAM在主账号的权限范围内创建子用户，给不同的子用户分配不同的权限从而达到授权管理的目的。
>
> STS（Security Token Service）:临时安全令牌。您可以使用STS来完成对于临时用户的访问授权

```shell
#为什么要使用RAM和STS
`RAM和STS需要解决的一个核心问题是如何在不暴露主账号的AccessKey的情况下安全的授权别人访问。因为一旦主账号的AccessKey暴露出去的话会带来极大的安全风险，别人可以随意操作该账号下所有的资源，盗取重要信息等。`

1.RAM提供一种长期有效的权限控制机制，通过分出不同权限的子账号，将不同的权限分给不同的用户，这样一旦子账号泄露也不会造成全局的信息泄露。但是，由于子账号在一般情况下是长期有效的，因此，子账号的AccessKey也是不能泄露的。

2.相对于RAM提供的长效控制机制，STS提供的是一种临时访问授权。通过STS可以返回临时的AccessKey和Token，这些信息可以直接发给临时用户用来访问OSS。一般来说，从STS获取的权限会受到更加严格的限制，并且拥有时间限制，因此这些信息泄露之后对于系统的影响也很小。

'总结：主要就是保护AccessKey不被泄露，STS授权更加严格'
#角色和子账号
子账号和角色可以类比为某个个人和其身份的关系，某人在公司的角色是员工，在家里的角色是父亲，在不同的场景扮演不同的角色，但是还是同一个人。在扮演不同的角色的时候也就拥有对应角色的权限。单独的员工或者父亲概念并不能作为一个操作的实体，只有有人扮演了之后才是一个完整的概念。这里还可以体现一个重要的概念，那就是角色可以被多个不同的个人同时扮演。
#使用场景
如果您购买了云资源，您的组织里有多个用户需要使用这些云资源，这些用户只能共享使用您的云账号 AccessKey。这里有两个问题：
1.您的密钥由多人共享，泄露的风险很高。
2.您无法控制特定用户能访问哪些资源（例如 Bucket）的权限。
-此时，您可以创建 RAM 子账号，并授予子账号对应的权限。之后，让您的用户通过子账号访问或管理您的资源。
```

| 概念                  | 说明                                                         |
| --------------------- | ------------------------------------------------------------ |
| 子账号（RAM account） | 创建的时候可以分配独立的密码和权限，每个子账号拥有自己AccessKey |
| 角色（Role）          | 表示某种操作权限的虚拟概念，但是没有独立的登录密码和AccessKey |
| 授权策略（Policy）    | 用来定义权限的规则，比如允许用户读取或写入某些资源           |
| 资源（Resource）      | 代表用户可访问的云资源，比如OSS所有的Bucket下面的某个Object等。 |

![1571040785088](image\1571040785088.png)

![1571040854150](image\1571040854150.png)

5.用户授权

![1573464412468](image\1573464412468.png)

6. bucket授权：可以选完全控制

![1573465246002](image\1573465246002.png)

![1573465271568](image\1573465271568.png)

## 3.3初始化

> OSSClient是OSS的Java客户端，用于管理存储空间和文件等OSS资源。
>
> 使用Java SDK发起OSS请求，您需要初始化一个OSSClient实例，并根据需要修改ClientConfiguration的默认配置项。

```java
// Region表示OSS的数据中心所在的地域，Endpoint表示OSS对外服务的访问域名，此处用的成都的数据中心
String endpoint = "http://oss-cn-chengdu.aliyuncs.com";
// 阿里云主账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM账号进行API访问或日常运维，请登录 https://ram.console.aliyun.com 创建RAM账号。
String accessKeyId = "LTAI4Ftpfqz7kU9Sz1S8cN2v";
String accessKeySecret = "4OaILyB9SEwmNLd4ECj8YNJkC8Swaf";
// 创建OSSClient实例。
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret, conf);
// 关闭OSSClient。
ossClient.shutdown();
```

## 3.4创建存储空间

```java
// Endpoint以杭州为例，其它Region请按实际情况填写。
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// 阿里云主账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM账号进行API访问或日常运维，请登录 https://ram.console.aliyun.com 创建RAM账号。
String accessKeyId = "LTAI4Ftpfqz7kU9Sz1S8cN2v";
String accessKeySecret = "4OaILyB9SEwmNLd4ECj8YNJkC8Swaf";

// 创建OSSClient实例。
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// 创建存储空间。
String bucketName = "theBestApe";
// 新建存储空间默认为标准存储类型，私有权限。
ossClient.createBucket(bucketName);

// 关闭OSSClient。
ossClient.shutdown();
```

## 3.5实体类

### 3.5.1资源信息

```java
package com.xxx.platform.oss.dao.pojo.entity;

import lombok.Data;

import java.util.Date;

@Data
public class OssResource {
    /**序号*/
    private Long id;
    /**资源路径*/
    private String resourcePath;
    /**资源Bucket*/
    private String bucket;
    /**文件大小*/
    private Long fileSize;
    /**资源类型*/
    private String mimeType;
    /**图片宽度*/
    private Integer imageWidth;
    /**图片高度*/
    private Integer imageHeight;
    /**图片格式*/
    private String imageFormat;
    /**原文件名*/
    private String originName;
    /**是否删除*/
    private Boolean isDeleted;
    /**是否私有资源*/
    private Boolean isPrivate;
    /**创建时间 */
    private Date createTime;
    /**更新时间*/
    private Date updateTime;
}

```

### 3.5.2资源映射

```java
//词表的作用主要是后期好删除
package com.xxx.platform.oss.dao.pojo.entity;

import lombok.Data;

import java.util.Date;

@Data
public class OssResourceBizMapping {
    private Long id;
    /**资源ID,对应资源信息表的id*/
    private Long ossResourceId;
    /**业务ID，关联哪些业务在引用这个资源*/
    private String businessId;
    /**业务类型，自己定义业务类型*/
    private String bizType;
    /**备注*/
    private String remark;
    /**是否删除*/
    private Boolean isDeleted;
    /**创建时间 */
    private Date createTime;
    /**更新时间*/
    private Date updateTime;
}
```



# 4. ConfigurationProperties

```shell
`有时候有这样子的情景，我们想把配置文件的信息，读取并自动封装成实体类，这样子，我们在代码里面使用就轻松方便多了，这时候，我们就可以使用@ConfigurationProperties，它可以把同类的配置信息自动封装成实体类`
'此处通过该注解来方便的配置和获取配置文件信息'
```



## 4.1 定义属性文件

```properties
# oss配置
oss.aliyun.accessKeyId=LTAI4Ftpfqz7kU9Sz1S8cN2v
oss.aliyun.accessKeySecret=4OaILyB9SEwmNLd4ECj8YNJkC8Swaf
oss.aliyun.endpoint=http://oss-cn-chengdu.aliyuncs.com
```

## 4.2 定义实体类

```java
// 使用时直接注入
@Data
@Component
//获取前缀为connection的属性，属性去匹配去除前缀的字符串，相同就赋予值
@ConfigurationProperties(prefix = "oss.aliyun")
public class OSSConfiguration {
    /**单个BucketName 时可以配置，多个时在调用时指定Bucket*/
    private String bucketName;
    /**OSS所在区域对应的域名：URL(不带 BucketName)*/
    private String endpoint;
    /**访问帐户的ID*/
    private String accessKeyId;
    /**访问帐户的密码*/
    private String accessKeySecret;
} 

```

## 4.3 扫描导入包的Bean

```java
 @Resource
 private OSSConfiguration oss;

 @Test
    public void testOssConfig(){
        System.out.println(oss.getEndpoint());
        System.out.println(oss.getAccessKeyId());
        System.out.println(oss.getAccessKeySecret());
    }
```

```java
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'com.virtue.terminal.service.impl.ScriptTest': 
Injection of resource dependencies failed; nested exception is org.springframework.beans.factory.NoSuchBeanDefinitionException: 
No qualifying bean of type 'com.xxx.platform.oss.conf.OSSConfiguration' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {@javax.annotation.Resource(shareable=true, lookup="", name="", description="", authenticationType=CONTAINER, type=java.lang.Object.class, mappedName="")}
'没有生成被注入的bean，所以需要在启动类上配置扫描其他包路径下的bean'
```

```java
// 解决：扫描其他包的路径即可，application注解扫描bean，mapperScan扫描

@MapperScan({"com.virtue.terminal.dao","com.xxx.platform.oss.dao"})
@SpringBootApplication(scanBasePackages = {"com.xxx.platform.oss","com.virtue.terminal"})
public class TerminalApplication {

	public static void main(String[] args) {
		SpringApplication.run(TerminalApplication.class,args);
	}
}
```

## 4.4 多mapper路径配置

```properties
mybatis.mapper-locations=
classpath*:com/virtue/terminal/dao/**/*.xml,
classpath*:com.xxx.platform.oss.dao.mapping/**/*.xml
```

## 4.5 扫描不到其他包mapper

```shell
`照着上面的配置还是扫描不到`
因为mapper里面有错误，会使整个mapper文件扫描不到，进去检查返回的类是否存在
```



# 5. 接受前端图片

```shell
1. 使用@RequestParam() MultipartFile[] files对象接收，可否直接把该参数定义到实体类里面？
'本质也是存放到body里面，可以接收'
```

## 5.1 使用POSTMAN上传图片

![1573455062432](image\1573455062432.png)

## 5.2 MultipartFile

```java
//文件大小
long size = headImg.getSize();
//文件类型：image/jpeg
String contentType = headImg.getContentType();
//原始文件名：QQ图片20190930161728.jpg
String originalFilename = headImg.getOriginalFilename();
//属性name名：headImg
String name = headImg.getName();
```



# 6. OSS文件路径

![1573521251873](image\1573521251873.png)

```shell
`https://picture-type-user-head.oss-cn-chengdu.aliyuncs.com/picture-type-user-head/2019/11/12/09/09/0cb20e71ec604390a44fb580ea89d701.jpg`
'这里是https开头的，这个具体是http还是https更什么有关？'
```

# 7. EXIF格式

```shell
EXIF是 Exchangeable Image File(可交换的图片文件)的缩写，这是一种专门为数码相机照片设定的格式。这种格式可以用来记录数字照片的属性信息，例如相机的品牌及型号、相片的拍摄时间、拍摄时所设置的光圈大小、快门速度、ISO等等信息。除此之外它还能够记录拍摄数据，以及照片格式化方式，这样就可以输出到兼容EXIF格式的外设上，例如照片打印机等。
`目前最常见的支持EXIF信息的图片格式是JPG，很多的图像工具都可以直接显示图片的EXIF信息，包括现在的一些著名的相册网站也提供页面用于显示照片的EXIF信息。本文主要介绍Java语言如何读取图像的EXIF信息，包括如何根据EXIF信息对图像进行调整以适合用户浏览。`
```

```xml
  <!-- exif提取器 -->
        <dependency>
            <groupId>com.drewnoakes</groupId>
            <artifactId>metadata-extractor</artifactId>
            <version>2.10.1</version>
        </dependency>
```

## 7.1. 获取exif信息

```java
 /**
     * 获取图片exif信息
     * @param client
     * @param bucketName
     * @param resourcePath
     * @return
     * @throws IOException
     */
    private SimpleImageExif getSimpleImageExif(OSSClient client, String bucketName, String resourcePath)  {

        String style = "image/info";

        GetObjectRequest request = new GetObjectRequest(bucketName, resourcePath);
        request.setProcess(style);
        OSSObject clientObject = client.getObject(request);

        InputStream stream = clientObject.getResponse().getContent();
        InputStreamReader reader = new InputStreamReader(stream);
        char[] bytes=new char[1024];

        try {

            reader.read(bytes);
			//比直接使用new String(bytes)好，避免乱码
            String exifJson = String.valueOf(bytes);
            ObjectMapper mapper = new ObjectMapper();
            JsonNode node = mapper.readTree(exifJson);

            return OSSHelper.getSimpleImageExif(node);

        } catch (IOException e) {

            e.printStackTrace();

        }finally {
            CloseHelper.close(reader);
            CloseHelper.close(stream);
        }

        return null;
    }

```

