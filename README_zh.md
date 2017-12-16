# 使用手册
（[English version](https://github.com/alibaba/spring-boot-starter-dubbo/blob/master/README.md)）

## 概念
Spring Boot with Dubbo support 是 Dubbo 对SpringBoot 框架的支持库。Dubbo 是一款分布式RPC框架。 
Dubbo 可以分为“服务提供者”和“服务消费者”两种角色（即服务器，客户端模式），在实际使用场景，服务提供者也可以是其它服务的消费者，但是基于良好的设计理念上，服务应该尽量颗粒化，做到不互相依赖为佳（以免构成错纵复杂，形成不必要的依赖循环）。

## 需求
支持jdk版本为1.6或更高版
Maven 目前使用版本为3.3.9

（笔记：在修改源码前，请导入googlestyle-java.xml以保证一致的代码格式）

## 如何使用项目
* 直接下载这项目解压，或以GIT的方式获取

```shell
$ git clone https://github.com/alibaba/spring-starter-dubbo
```

* 打开终端，浏览到在项目的目录里，执行命令 ```mvn clean install```，以编译项目并生成 jar 包。


## Dubbo服务提供者

* 添加 "spring-boot-starter-parent" 依赖（目前 "spring-boot-starter-dubbo" 依赖于 1.3.5.RELEASE 版本）
```xml
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.3.5.RELEASE</version>
		<relativePath /> <!-- lookup parent from repository -->
	</parent>
```

* 添加 "spring-boot-starter-dubbo" 依赖 (未在公共库发布pom artifact，故使用本地库):

```xml
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>spring-boot-starter-dubbo</artifactId>
        <version>1.0.0-SNAPSHOT</version>
        <scope>system</scope>
        <systemPath>${basedir}/lib/spring-boot-starter-dubbo-1.0.0-SNAPSHOT.jar</systemPath>
    </dependency>
```

* 在application.properties添加dubbo的相关配置信息,样例配置如下:

```properties
spring.dubbo.appname=spring-boot-starter-dubbo-provider-test
spring.dubbo.registry=multicast://224.0.0.0:1111
spring.dubbo.protocol=dubbo
```

* 接下来在Spring Boot Application的上添加`@EnableDubboConfiguration`, 表示要开启dubbo功能. (dubbo provider服务可以使用或者不使用web容器)

```java
@SpringBootApplication
@EnableDubboConfiguration
public class DubboProviderLauncher {
  //...
}
```

* 编写你的dubbo服务,只需要添加要发布的服务实现上添加`@Service`（import com.alibaba.dubbo.config.annotation.Service）注解 ,其中interfaceClass是要发布服务的接口.

```java
@Service(interfaceClass = IHelloService.class)
public class HelloServiceImpl implements IHelloService {
  //...
}
```

* 启动你的Spring Boot应用,观察控制台,可以看到dubbo启动相关信息.


### Dubbo服务消费者

* 添加依赖:

```xml
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>spring-boot-starter-dubbo</artifactId>
        <version>1.0.0-SNAPSHOT</version>
    </dependency>
```

* 在application.properties添加dubbo的相关配置信息,样例配置如下:

```properties
spring.dubbo.appname=spring-boot-starter-dubbo-consumer-test
spring.dubbo.registry=multicast://224.0.0.0:1111
spring.dubbo.protocol=dubbo
```

* 开启`@EnableDubboConfiguration`

```java
@SpringBootApplication
@EnableDubboConfiguration
public class DubboConsumerLauncher {
  //...
}
```

* 通过`@DubboConsumer`注入需要使用的interface.

```java
@Component
public class HelloConsumer {
  @DubboConsumer
  private IHelloService iHelloService;
}
```

### 参考文档

* dubbo 介绍: http://dubbo.io/
* spring-boot 介绍: http://projects.spring.io/spring-boot/
* spring-boot-starter-dubbo 参考: https://github.com/linux-china/spring-boot-dubbo
