# SpringBoot WAR

[Packaging Executable War Files](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#build-tool-plugins-maven-packaging)

[Create a Deployable War File](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#howto-create-a-deployable-war-file)

- 修改`MainClass`

```java
@SpringBootApplication
public class Application extends SpringBootServletInitializer {

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(Application.class);
    }

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```

- `pom.xml`中`packaging`元素为`war`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <!-- ... -->
    <packaging>war</packaging>
    <!-- ... -->
</project>
```

- `pom.xml`中添加插件`spring-boot-maven-plugin`

```xml
<finalName>[APP_NAME]</finalName>
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
        <mainClass>Applicationcom.sample.war.springboot.Application</mainClass>
    </configuration>
</plugin>
```

- 将依赖`spring-boot-starter-tomcat`的`scope`修改为`provided`

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <scope>provided</scope>
</dependency>
```

- 编译`war`

```shell script
mvn clean package
```

- 运行

```shell script
cp target/[APP_NAME].jar tomcat-docker/webapps
sudo docker restart tomcat
```

- 访问

http://localhost:8080/[APP_NAME]
