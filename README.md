spring-boot-starter-dubbo
===================================

[中文版文档](https://github.com/alibaba/spring-boot-starter-dubbo/blob/master/README_zh.md)

Spring Boot with dubbo support. Dubbo is an RPC framework.

Support jdk version 1.6 or 1.6+

(please import googlestyle-java.xml if you want to modify the code)

### How to publish dubbo

* Download this project

```shell
$ git clone https://github.com/alibaba/spring-starter-dubbo
```

* Generate all jars by running this ```$ mvn clean install``` in your Terminal.

* add "spring-boot-starter-parent" dependency（"spring-boot-starter-dubbo" currently rely on 1.3.5.RELEASE）
```xml
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.3.5.RELEASE</version>
		<relativePath /> <!-- lookup parent from repository -->
	</parent>
```

* add "spring-boot-starter-dubbo" dependency (using local lib due to pom artifact not published yet):

```xml
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>spring-boot-starter-dubbo</artifactId>
        <version>1.0.0-SNAPSHOT</version>
        <scope>system</scope>
        <systemPath>${basedir}/libs/spring-boot-starter-dubbo-1.0.0-SNAPSHOT.jar</systemPath>
    </dependency>
```
* add dubbo configuration in application.properties, demo:

```properties
spring.dubbo.appname=spring-boot-starter-dubbo-provider-test
spring.dubbo.registry=multicast://224.0.0.0:1111
spring.dubbo.protocol=dubbo
```

* then add `@EnableDubboConfiguration` on Spring Boot Application, indicates that dubbo is enabled.(web or non-web application can use dubbo provider)

```java
@SpringBootApplication
@EnableDubboConfiguration
public class DubboProviderLauncher {
  //...
}
```

* code your dubbo service, add `@Service`(import com.alibaba.dubbo.config.annotation.Service) on your service class, and interfaceClass is the interface which will be published.

```java
@Service(interfaceClass = IHelloService.class)
public class HelloServiceImpl implements IHelloService {
  //...
}
```

* start Spring Boot.


### How to consume Dubbo

* add Dependencies:

```xml
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>spring-boot-starter-dubbo</artifactId>
        <version>1.0.0-SNAPSHOT</version>
    </dependency>
```

* add dubbo configuration in application.properties, demo:

```properties
spring.dubbo.appname=spring-boot-starter-dubbo-consumer-test
spring.dubbo.registry=multicast://224.0.0.0:1111
spring.dubbo.protocol=dubbo
```

* then add `@EnableDubboConfiguration` on Spring Boot Application

```java
@SpringBootApplication
@EnableDubboConfiguration
public class DubboConsumerLauncher {
  //...
}
```

* injection interface by the `@DubboConsumer` annotation.

```java
@Component
public class HelloConsumer {
  @DubboConsumer
  private IHelloService iHelloService;

}
```

### Reference

* dubbo: http://dubbo.io/
* spring-boot: http://projects.spring.io/spring-boot/
* spring-boot-starter-dubbo: https://github.com/linux-china/spring-boot-dubbo
