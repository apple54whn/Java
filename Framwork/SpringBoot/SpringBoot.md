> 微服务：2014，martin fowler提出的架构风格（服务微化）。一个应用应该是一组小型服务，可以通过HTTP的方式互通
>
> - 单体应用：ALL IN ONE
> - 微服务：每一个功能元素最终都是一个可独立替换和独立升级的软件单元。[详细参照微服务文档](https://martinfowler.com/articles/microservices.html#MicroservicesAndSoa)

# 入门

- 背景：J2EE笨重的开发、**繁多的配置**、**依赖管理的耗时耗力**、低下的开发效率、 **复杂的部署流程**、第三方技术**集成难度大**。Spring Boot来**简化Spring应用开发**，**约定大于配置**， 去繁从简，just run就能创建一个独立的，产品级别应用

- **优点**：

  - SpringBoot不是对Spring功能上的增强，而是提供了一种快速使用Spring的方式
  - **开箱即用**，没有代码生成，也**无需XML配置**。同时也可以修改默认值来满足特定的需求
  - 提供了一些大型项目中常见的非功能性特性，如**嵌入式服务器**、安全、指标，健康检测、外部配置等

- **核心功能**

  - **起步依赖（starter-*）**

    起步依赖本质是一个Maven项目对象模型（POM），定义了对其他库的传递依赖，这些东西加在一起即支持某项功能。

  - **自动配置**

    Spring Boot的自动配置是一个运行时（更准确地说，是应用程序启动时）的过程，考虑了众多因素，才决定Spring配置应该用哪个，不该用哪个。该过程是Spring自动完成的。

------

- 使用Spring Initializr初始化Spring Boot项目： https://start.spring.io/。IDEA中自带。根据需求选择需要的依赖：

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  	<modelVersion>4.0.0</modelVersion>
    <!--所有的springboot工程都必须继承spring-boot-starter-parent，也可以去掉并自定义-->
  	<parent>
  		<groupId>org.springframework.boot</groupId>
  		<artifactId>spring-boot-starter-parent</artifactId>
  		<version>2.1.6.RELEASE</version>
  		<relativePath/> <!-- lookup parent from repository -->
  	</parent>
    
    <!--该项目的Maven坐标-->
  	<groupId>com.example</groupId>
  	<artifactId>demo</artifactId>
  	<version>0.0.1-SNAPSHOT</version>
  	<name>demo</name>
  	<description>Demo project for Spring Boot</description>
  
    <!--选择JDK版本后自动配置-->
  	<properties>
  		<java.version>1.8</java.version>
  	</properties>
  
  	<dependencies>
      <!--web功能的起步依赖，若是个普通Java工程，则会是spring-boot-starter-->
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
      </dependency>
      <!--Spring Boot Actuator支持内置（或自定义）端点，可以监控和管理应用程序，例如应用程序运行状况，指标，会话等-->
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
      </dependency>
  
      <!--Spring Boot DevTools提供快速应用程序重启，LiveReload和配置，以增强开发体验。-->
  		<dependency>
  			<groupId>org.springframework.boot</groupId>
  			<artifactId>spring-boot-devtools</artifactId>
  			<scope>runtime</scope>
  			<optional>true</optional>
  		</dependency>
      <!--Spring配置处理器为开发人员生成元数据，以便在使用自定义配置键时提供上下文帮助和“代码完成”（ex.application.properties）-->
  		<dependency>
  			<groupId>org.springframework.boot</groupId>
  			<artifactId>spring-boot-configuration-processor</artifactId>
  			<optional>true</optional>
  		</dependency>
      <!--Lombok Java注释库，有助于减少样板代码。-->
  		<dependency>
  			<groupId>org.projectlombok</groupId>
  			<artifactId>lombok</artifactId>
  			<optional>true</optional>
  		</dependency>
      <!--自动添加的测试配置-->
  		<dependency>
  			<groupId>org.springframework.boot</groupId>
  			<artifactId>spring-boot-starter-test</artifactId>
  			<scope>test</scope>
  		</dependency>
  	</dependencies>
  
    <!--自动添加的maven插件，这个插件可将应用打包成可执行的jar包。可以通过java -jar 包名来运行应用-->
  	<build>
  		<plugins>
  			<plugin>
  				<groupId>org.springframework.boot</groupId>
  				<artifactId>spring-boot-maven-plugin</artifactId>
  			</plugin>
  		</plugins>
  	</build>
  </project>
  ```

- 默认生成的Spring Boot项目

  - 主程序已经生成好了，我们只需要我们自己的逻辑

    ```java
    //声明该类是一个SpringBoot引导类
    @SpringBootApplication
    public class DemoApplication {
        //main是java程序的入口
        public static void main(String[] args) {
            //run方法 表示运行SpringBoot的引导类  run参数就是SpringBoot引导类的字节码对象
            SpringApplication.run(DemoApplication.class, args);
        }
    }
    ```

  - **`resources`**文件夹中目录结构

    - **`static`**：保存所有的静态资源； js css images；
    - **`templates`**：保存所有的模板页面；（Spring Boot默认jar包使用嵌入式的Tomcat，默认不支持JSP页面）；可以使用模板引擎（freemarker、thymeleaf）；
    - **`application.properties`**：Spring Boot应用的配置文件；可以修改一些默认设置；

- `HelloController`：必须在引导类`DemoApplication`**同级包或者子级包**中创建

  ```java
  @RestController
  public class HelloController {
  
      @RequestMapping("/hello")
      public String hello(){
          return "hello springboot";
      }
  }
  ```

- Run后使用Terminal测试：`curl http://localhost:8080/hello`，即可查看到返回值

  上述POM中还有actuator，可以检查应用状态，如：`curl http://localhost:8080/actuator/health`，返回如下：

  ```json
{"status":"UP"}
  ```
  
- Maven打包：**`mvn clean package -Dmaven.test.skip`**

  可以看到带有`.original`的为原始包，和带有所有依赖的`jar`包，可以执行`java -jar`启动

> 热部署失效：原因是因为Intellij IEDA默认情况下不会自动编译，需要对IDEA进行自动编译的设置（Debug模式没此问题）
>
> 1. ![](./images/1548621994782.png)
>
> 2. `Shift + Ctrl + Alt + /` （Mac：`Shift + Command + Alt + /`），选择Registry...
>
>    ![](./images/1548622068044.png)



若需要使用自定义的parent，不使用官方提供的，可以按如下配置（仅作为示例）：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>geektime.demo</groupId>
	<artifactId>helloworld</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>HelloWorld</name>
	<description>Demo project for Spring Boot</description>

	<properties>
		<java.version>1.8</java.version>
	</properties>

	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-dependencies</artifactId>
				<version>2.1.1.RELEASE</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<version>2.1.1.RELEASE</version>
				<executions>
					<execution>
						<goals>
							<goal>repackage</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
		</plugins>
	</build>
</project>
```





# 原理分析

## 起步依赖原理分析

按住Ctrl点击pom.xml中的spring-boot-starter-parent，跳转到了spring-boot-starter-parent的pom.xml，xml配置如下（配置了配置文件名称位置等信息）：

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.1.2.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
</parent>
......
```

按住Ctrl点击pom.xml中的spring-boot-dependencies，跳转到了spring-boot-dependencies的pom.xml，xml配置如下：

```xml
<properties>
    <activemq.version>5.15.8</activemq.version>
    <antlr2.version>2.7.7</antlr2.version>
    <appengine-sdk.version>1.9.71</appengine-sdk.version>
    <artemis.version>2.6.3</artemis.version>
    <aspectj.version>1.9.2</aspectj.version>
    ......
</properties>
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot</artifactId>
            <version>2.1.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-test</artifactId>
            <version>2.1.2.RELEASE</version>
        </dependency>
        ......
    </dependencies>
</dependencyManagement>
<build>
    <pluginManagement>
        <plugins>
            <plugin>
                <groupId>org.apache.johnzon</groupId>
                <artifactId>johnzon-maven-plugin</artifactId>
                <version>${johnzon.version}</version>
            </plugin>
            ......
        </plugins>
    </pluginManagement>
</build>
```

从上面的spring-boot-dependencies的pom.xml中我们可以发现，一部分坐标的**版本、依赖管理、插件管理**已经定义好，所以我们的SpringBoot工程继承spring-boot-starter-parent后已经具备**版本锁定**等配置了。所以起步依赖作用就是进行**依赖的传递**。

同理spring-boot-starter-web就是将web开发要使用的spring-web、spring-webmvc等坐标进行了“打包”，这样我们的工程只要引入spring-boot-starter-web起步依赖的坐标就可以进行web开发了，同样体现了依赖传递的作用。

## 自动配置原理解析

按住Ctrl点击查看启动类MySpringBootApplication上的注解@SpringBootApplication，源码如下：

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = {
		@Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {

	/**
	 * Exclude specific auto-configuration classes such that they will never be applied.
	 * @return the classes to exclude
	 */
	@AliasFor(annotation = EnableAutoConfiguration.class)
	Class<?>[] exclude() default {};

	... ... ...

}
```

其中，

`@ComponentScan`：组件扫描，但是仅扫描注解了`@SpringBootApplication`类所在的同级包和子级包

`@SpringBootConfiguration`：等同与`@Configuration`，既标注该类是Spring的一个配置类

`@EnableAutoConfiguration`：SpringBoot自动配置功能开启

按住Ctrl点击查看注解`@EnableAutoConfiguration`

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {
	... ... ...
}
```

其中，

`@AutoConfigurationPackage`**：**自动配置包**，**如下注解的简写`@Import(AutoConfigurationPackages.Registrar.class)` Spring底层注解`@Import`，给容器中导入一个组件；导入的组件由`AutoConfigurationPackages.Registrar.class`指定，这个类有一个方法，通过注解metadata，将主配置类（`@SpringBootApplication`）所在包及下面所有子包里面的所有组件扫描到Spring容器

`@Import(AutoConfigurationImportSelector.class) `导入了`AutoConfigurationImportSelector`类

按住Ctrl点击查看`AutoConfigurationImportSelector`源码

```java
public String[] selectImports(AnnotationMetadata annotationMetadata) {
    ... ... ...
        List<String> configurations = getCandidateConfigurations(annotationMetadata,
                                                                 attributes);
    configurations = removeDuplicates(configurations);
    Set<String> exclusions = getExclusions(annotationMetadata, attributes);
    checkExcludedClasses(configurations, exclusions);
    configurations.removeAll(exclusions);
    configurations = filter(configurations, autoConfigurationMetadata);
    fireAutoConfigurationImportEvents(configurations, exclusions);
    return StringUtils.toStringArray(configurations);
}


protected List<String> getCandidateConfigurations(AnnotationMetadata metadata,
                                                  AnnotationAttributes attributes) {
    List<String> configurations = SpringFactoriesLoader.loadFactoryNames(
        getSpringFactoriesLoaderFactoryClass(), getBeanClassLoader());

    return configurations;
}
```

其中，`SpringFactoriesLoader.loadFactoryNames` 方法的作用就是从`META-INF/spring.factories`文件中读取指定类对应的全类名的列表，如xxxAutoConfiguration

![1550170881470](./images/1550170881470.png)

`spring-autoconfigure-metadata.properties` 文件中有关自动配置的配置信息如下：

```
org.springframework.boot.autoconfigure.web.reactive.function.client.WebClientAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.DispatcherServletAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.ServletWebServerFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.error.ErrorMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.HttpEncodingAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.MultipartAutoConfiguration,\
......
```

上面配置文件存在大量的以Configuration为结尾的类名称，这些类就是存有自动配置信息的类，而SpringApplication在获取这些类名后再加载

我们以`ServletWebServerFactoryAutoConfiguration`为例来分析源码：

```java
@Configuration
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE)
@ConditionalOnClass(ServletRequest.class)
@ConditionalOnWebApplication(type = Type.SERVLET)
@EnableConfigurationProperties(ServerProperties.class)
@Import({ ServletWebServerFactoryAutoConfiguration.BeanPostProcessorsRegistrar.class,
		ServletWebServerFactoryConfiguration.EmbeddedTomcat.class,
		ServletWebServerFactoryConfiguration.EmbeddedJetty.class,
		ServletWebServerFactoryConfiguration.EmbeddedUndertow.class })
public class ServletWebServerFactoryAutoConfiguration {
	......
}
```

其中，`@EnableConfigurationProperties(ServerProperties.class) `代表加载`ServerProperties`服务器配置属性类

进入ServerProperties.class源码如下：

```java
@ConfigurationProperties(prefix = "server", ignoreUnknownFields = true)
public class ServerProperties {
    /**
	 * Server HTTP port.
	 */
    private Integer port;
    /**
	 * Network address to which the server should bind.
	 */
    private InetAddress address;
	......
}
```

其中，`prefix = "server"` 表示SpringBoot配置文件中的前缀，SpringBoot会将配置文件中以server开始的属性映射到该类的字段中。如在`application.properties`中配置`server.port=80`即可改变当前服务器的HTTP端口号

## 自动配置总结

- **SpringBoot启动会加载大量的自动配置类**
- **我们看我们需要的功能有没有SpringBoot默认写好的自动配置类；**
- **我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件有，我们就不需要再来配置了）**
- **给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们就可以在配置文件中指定这些属性的值；**
- **自动配置类对应属性类**
  - xxxxAutoConfigurartion：自动配置类；给容器中添加组件
  - xxxxProperties:封装配置文件中相关属性；

## 配置信息的查询

SpringBoot的配置文件，主要的目的就是对配置信息进行修改的，但在配置时的key从哪里去查询呢？我们可以查阅[官方文档](https://docs.spring.io/spring-boot/docs/2.1.2.RELEASE/reference/htmlsingle/#common-application-properties)

常用的配置摘抄如下：

```properties
# QUARTZ SCHEDULER (QuartzProperties)
spring.quartz.jdbc.initialize-schema=embedded # Database schema initialization mode.
spring.quartz.jdbc.schema=classpath:org/quartz/impl/jdbcjobstore/tables_@@platform@@.sql # Path to the SQL file to use to initialize the database schema.
spring.quartz.job-store-type=memory # Quartz job store type.
spring.quartz.properties.*= # Additional Quartz Scheduler properties.

# ----------------------------------------
# WEB PROPERTIES
# ----------------------------------------

# EMBEDDED SERVER CONFIGURATION (ServerProperties)
server.port=8080 # Server HTTP port.
server.servlet.context-path= # Context path of the application.
server.servlet.path=/ # Path of the main dispatcher servlet.

# HTTP encoding (HttpEncodingProperties)
spring.http.encoding.charset=UTF-8 # Charset of HTTP requests and responses. Added to the "Content-Type" header if not set explicitly.

# JACKSON (JacksonProperties)
spring.jackson.date-format= # Date format string or a fully-qualified date format class name. For instance, `yyyy-MM-dd HH:mm:ss`.

# SPRING MVC (WebMvcProperties)
spring.mvc.servlet.load-on-startup=-1 # Load on startup priority of the dispatcher servlet.
spring.mvc.static-path-pattern=/** # Path pattern used for static resources.
spring.mvc.view.prefix= # Spring MVC view prefix.
spring.mvc.view.suffix= # Spring MVC view suffix.

# DATASOURCE (DataSourceAutoConfiguration & DataSourceProperties)
spring.datasource.driver-class-name= # Fully qualified name of the JDBC driver. Auto-detected based on the URL by default.
spring.datasource.password= # Login password of the database.
spring.datasource.url= # JDBC URL of the database.
spring.datasource.username= # Login username of the database.

# JEST (Elasticsearch HTTP client) (JestProperties)
spring.elasticsearch.jest.password= # Login password.
spring.elasticsearch.jest.proxy.host= # Proxy host the HTTP client should use.
spring.elasticsearch.jest.proxy.port= # Proxy port the HTTP client should use.
spring.elasticsearch.jest.read-timeout=3s # Read timeout.
spring.elasticsearch.jest.username= # Login username.
```

通过配置application.poperties 或者 application.yml 来修改SpringBoot的默认配置

```properties
server.port=8888
server.servlet.context-path=demo
```

或

```yaml
server:
  port: 8888
  servlet:
    context-path: /demo
```





## @Conditional细节（了解）

- **@Conditional派生注解（Spring注解版原生的@Conditional作用）**

- 作用：必须是@Conditional**指定的条件成立**，才给容器中**添加组件**，**配置类**里面的所有内容才**生效**

  | @Conditional扩展注解            | 作用（判断是否满足当前指定条件）                 |
  | ------------------------------- | ------------------------------------------------ |
  | @ConditionalOnJava              | 系统的java版本是否符合要求                       |
  | @ConditionalOnBean              | 容器中存在指定Bean；                             |
  | @ConditionalOnMissingBean       | 容器中不存在指定Bean；                           |
  | @ConditionalOnExpression        | 满足SpEL表达式指定                               |
  | @ConditionalOnClass             | 系统中有指定的类                                 |
  | @ConditionalOnMissingClass      | 系统中没有指定的类                               |
  | @ConditionalOnSingleCandidate   | 容器中只有一个指定的Bean，或者这个Bean是首选Bean |
  | @ConditionalOnProperty          | 系统中指定的属性是否有指定的值                   |
  | @ConditionalOnResource          | 类路径下是否存在指定资源文件                     |
  | @ConditionalOnWebApplication    | 当前是web环境                                    |
  | @ConditionalOnNotWebApplication | 当前不是web环境                                  |
  | @ConditionalOnJndi              | JNDI存在指定项                                   |

- 我们可以通过在**配置文件中启用 debug=true属性；来让控制台打印自动配置报告**，这样我们就可以很方便的知道哪些自动配置类生效；

  ```java
  =========================
  AUTO-CONFIGURATION REPORT
  =========================
  
  Positive matches:（自动配置类启用的）
  -----------------
  
     DispatcherServletAutoConfiguration matched:
        - @ConditionalOnClass found required class 'org.springframework.web.servlet.DispatcherServlet'; @ConditionalOnMissingClass did not find unwanted class (OnClassCondition)
        - @ConditionalOnWebApplication (required) found StandardServletEnvironment (OnWebApplicationCondition)
          
      
  Negative matches:（没有启动，没有匹配成功的自动配置类）
  -----------------
  
     ActiveMQAutoConfiguration:
        Did not match:
           - @ConditionalOnClass did not find required classes 'javax.jms.ConnectionFactory', 'org.apache.activemq.ActiveMQConnectionFactory' (OnClassCondition)        
  ```





# 配置文件

SpringBoot是基于约定的，所以很多配置都有默认值，但如果想使用自己的配置替换默认配置的话就可以使用配置文件配置，SpringBoot默认会从Resources目录下加载`application*.properties`或`application*.yml`（`application*.yaml`）文件

此处不再介绍properties配置文件（键值对形式），仅介绍yml配置文件

## 3.1 yml 简介

YML文件格式是YAML (YAML Aint Markup Language)编写的文件格式，YAML是一种直观的能够被电脑识别的的数据数据序列化格式，并且容易被人类阅读，容易和脚本语言交互的，可以被支持YAML库的不同的编程语言程序导入，比如： C/C++, Ruby, Python, Java, Perl, C#, PHP等。YML文件是**以数据为核心**的，比传统的xml方式更加简洁。

YML文件的扩展名可以使用.yml或者.yaml。

## 3.2 yml 语法

基本语法：

- **`k:(空格)v`**：表示一对键值对（空格必须有）
- 以**空格**的**缩进**来控制层级关系；只要是左对齐（相同空格缩进）的一列数据，都是同一个层级的；属性和值也是**大小写敏感**



### 3.2.1 配置字面量数据

普通的值（**数字，字符串，布尔**）

- `k: v`：字面量**直接来写**；**字符串默认不用加上单引号或者双引号**；
  - `""`：**双引号**；**不会转义**字符串里面的特殊字符；特殊字符会作为本身想表示的意思
    - name: "zhangsan \n lisi"：输出；zhangsan 换行 lisi
  - `''`：**单引号**；**会转义**特殊字符，特殊字符最终只是一个普通的字符串数据
    - name: ‘zhangsan \n lisi’：输出；zhangsan \n lisi

```yml
name: zhangsan
```



### 3.2.2 配置对象、Map数据

```yml
teacher:
  name: zhangsan
  age: 38

student: {name: lisi , age: 18}

```



### 3.2.3 配置数组（List、Set）数据

```yaml
city:
  - beijing
  - shanghai

province: [shangxi,hebei,guangdong]

```

```yaml
#元素是对象形式
dog:
  - name: wangcai
    age: 3
  - name: goudan
    age: 5

cat: [{name: xiaobai,age: 1},{name: xiaolv,age: 2}]

```

【注意】：value与之间的 - 之间存在一个**空格**



## 3.3 配置文件值注入

### 3.3.1 @Value

利用在4.2中语法所写的配置

```java
@RestController
public class HelloController {

    @Value("${dog[0].name}")//数组或集合中保存对象
    private String name;

    @RequestMapping("/hello")
    public String hello(){
        return "hello spring boot "+name;
    }
}

```



### 3.3.2 @ConfigurationProperties

通过注解`@ConfigurationProperties(prefix="配置文件中的key的前缀")`可以将配置文件中的配置自动与实体进行映射

利用在4.2中语法所写的配置。需要提供setter。但是配置的数组、List、Set不清楚怎么获取

```java
@Component
@RestController
@ConfigurationProperties(prefix = "teacher")
public class HelloController {

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @RequestMapping("/hello")
    public String hello(){
        return "hello spring boot "+name;
    }
}

```

`@ConfigurationProperties`报错Not registered via @EnableConfigurationProperties or marked as Spring component，添加了`@Component`注解后解决了

若出现以下提示，需要配置文件执行器，并在类中定义好属性和set方法后，如果在配置文件中编写配置会有提示！

![](./images/%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E5%A4%84%E7%90%86%E5%99%A8.PNG)

```xml
<!--配置文件执行器配置-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>

```



### 3.3.3 @Value 和 @ConfigurationProperties比较

|                      | @ConfigurationProperties | @Value     |
| -------------------- | ------------------------ | ---------- |
| 功能                 | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定（松散语法） | 支持                     | 不支持     |
| SpEL                 | 不支持                   | 支持       |
| JSR303数据校验       | 支持                     | 不支持     |
| 复杂类型封装         | 支持                     | 不支持     |

- **松散语法绑定**：last_name = last-name = lastName 他们取的值都是相同的
- 配置文件yml还是properties他们都能获取到值，怎么选择呢？
  - 若我们只是在某个业务逻辑中需要**获取**一下**配置文件**中的**某项值**，使用@Value；
  - 若我们专门编写了一个javaBean来和配置文件**映射**，我们就直接使用@ConfigurationProperties



### 3.3.4 @PropertySource

- @**PropertySource**：加载**指定的配置文件**，由于@ConfigurationProperties(prefix = "person")默认从全局配置文件中获取值；

  ```java
  /**
   * 将配置文件中配置的每一个属性的值，映射到这个组件中
   * @ConfigurationProperties：告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定；
   *      prefix = "person"：配置文件中哪个下面的所有属性进行一一映射
   *
   * 只有这个组件是容器中的组件，才能容器提供的@ConfigurationProperties功能；
   */
  @PropertySource(value = {"classpath:person.properties"})
  @Component
  @ConfigurationProperties(prefix = "person")
  public class Person {
  
      private String lastName;
      private Integer age;
      private Boolean boss;
  
  ```

  

### 3.3.5 @Configuration 和 @Bean

- @ImportResource：**导入Spring的配置文件**，如beans.xml，加载bean，让配置文件里面的内容生效

  - Spring Boot里面没有Spring的配置文件，我们自己编写的配置文件，也不能自动识别；

    想让Spring的配置文件生效，加载进来；@ImportResource标注在一个配置类上

  ```xml
  <bean id="helloService" class="com.cuzz.springboot.service.HelloService"></bean>
  
  ```

  ```java
  @ImportResource(locations = {"classpath:beans.xml"})
  @SpringBootApplication
  public class SpringbootApplication {
  
  	public static void main(String[] args) {
  		SpringApplication.run(SpringbootApplication.class, args);
  	}
  }
  
  ```

- 还是**写配置类吧**！利用@Configuration和@Bean

  ```java
  @Configuration
  public class MyAppConfig {
      // 将方法的返回值添加到容器中；容器中这个组件默认的id就是方法名
      @Bean
      public HelloService helloService02(){
          System.out.println("配置类@Bean给容器中添加组件了...");
          return new HelloService();
      }
  }
  
  ```

  

## 3.4 配置文件占位符

- 随机数

  ```yaml
  ${random.value}
  ${random.int}
  ${random.long}
  ${random.int(10)}
  ${random.int[1024,65536]}
  
  ```

- **占位符获取之前配置的值**，如果没有可以使用**:**指定**默认值**

  ```properties
  person.last-name=张三${random.uuid}
  person.age=${random.int}
  # 没有取到:后面是默认值
  person.dog.name=${person.hello:hello}_dog
  
  ```



## 3.5 Profile

Profile是Spring对**不同环境（开发、测试、上线等）提供不同配置功能**的支持，可以通过激活、 指定参数等方式快速切换环境

### 3.5.1 多Profile文件

- 我们在主配置文件编写的时候，文件名可以是 application-{profile}.properties/yml，{profile}名任起
  - 默认使用application.properties的配置；

### 3.5.2 yml支持多文档块方式

```yaml
server:
  port: 8081
spring:
  profiles:
    active: pro  #指定激活哪个环境，不设置则为这个默认的

---
server:
  port: 8083
spring:
  profiles: dev  #指定属于哪个环境

---

server:
  port: 8084
spring:
  profiles: pro  #指定属于哪个环境

```

- 若文档块都没有指定环境，则**默认使用最后一个**
- 若**某个文档块没有指定环境，则默认使用那个**，**一般第一个不指定环境**

### 3.5.3 激活指定profile

- 在**默认配置文件中指定** spring.profiles.**active**=dev

- **命令行参数**

  ```shell
  java -jar spring-boot-02-config-0.0.1-SNAPSHOT.jar --spring.profiles.active=dev
  ```
  
- **虚拟机参数**

  ```
  -Dspring.profiles.active=dev
  ```
  

在IDEA中也可以配置，以下三者任选其一，但权限 Program arguments > Active profiles > VM options

![](./images/active_profile.png)





## 3.6 配置文件加载位置及顺序（了解）

> 起步依赖里好像不是这样？？？

- Spring Boot启动会扫描以下位置的application.properties/yml文件作为Spring boot的默认配置文件

  - –file:./config/     ——项目目录下的config

  - –file:./                ——项目目录下

  - –classpath:/config/     ——resources目录下的config

  - –classpath:/                ——resources目录下

    **优先级由高到底**，**高**优先级的配置会**覆盖低**优先级的配置；SpringBoot会从这四个位置全部加载主配置文件；**互补配置**；

- 我们还可以通过**spring.config.location**来**改变默认的配置文件位置**

  - 用于运维时，**项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；指定配置文件和默认加载的这些配置文件共同起作用形成互补配置；**

    ```shell
    java -jar spring-boot-02-config-02-0.0.1-SNAPSHOT.jar --spring.config.location=G:/application.properties
    
    ```

------

> 所有支持的配置加载来源[查看这里](https://docs.spring.io/spring-boot/docs/2.0.6.RELEASE/reference/htmlsingle/#boot-features-external-config)第24节 Externalized Configuration

**SpringBoot也可以从以下位置加载配置； 优先级从高到低；高优先级的配置覆盖低优先级的配置，所有的配置会形成互补配置**

1. **命令行参数**：所有的配置都可以在命令行上进行指定。多个配置用空格分开； --配置项=值

   ```shell
   java -jar spring-boot-02-config-02-0.0.1-SNAPSHOT.jar --server.port=8087 --server.context-path=/abc
   ```
   
2. 来自java:comp/env的JNDI属性

3. Java系统属性（System.getProperties()）

4. 操作系统环境变量

5. RandomValuePropertySource配置的random.*属性值

**由jar包外向jar包内进行寻找**

**优先加载带profile**

1. **jar包外部的application-{profile}.properties或application.yml(带spring.profile)配置文件**
2. **jar包内部的application-{profile}.properties或application.yml(带spring.profile)配置文件**

**再来加载不带profile**

1. **jar包外部的application.properties或application.yml(不带spring.profile)配置文件**
2. **jar包内部的application.properties或application.yml(不带spring.profile)配置文件**
3. @Configuration注解类上的@PropertySource
4. 通过SpringApplication.setDefaultProperties指定的默认属性



# JDBC & ORM

## 配置单数据源



## 集成H2数据库

H2是一个内存数据库，方便测试。



## 5.1 集成Mybatis

### 5.1.1 起步依赖

使用Spring Initializr添加 DevTools、Web、MySQL、**MyBatis**依赖

```xml
<!-- MySQL连接驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
<!--mybatis起步依赖-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.0.0</version>
</dependency>

```

### 5.1.2 数据库连接和MyBatis配置

在application.properties/yml中添加数据库的连接信息、Spring Boot 集成MyBatis的配置

```properties
#数据库连接信息
spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3308/test?serverTimezone=GMT%2B8&useSSL=false
spring.datasource.username=root
spring.datasource.password=w111151

#spring集成Mybatis环境
#pojo别名扫描包，不配置也行，需要指定全类名
mybatis.type-aliases-package=com.example.domain
#加载Mybatis映射文件，不配置也行，只需要*mapper.xml在@Mapper注解的接口的同一包中！
mybatis.mapper-locations=classpath:mapper/*Mapper.xml

```

```yaml
#数据库连接信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3308/test?serverTimezone=GMT%2B8&useSSL=false
    username: root
    password: w111151

#spring集成Mybatis环境
mybatis:
  #pojo别名扫描包
  type-aliases-package: com.example.domain
  #加载Mybatis映射文件
  mapper-locations: classpath:mapper/*Mapper.xml

```

### 5.1.3 创建user表对应的实体类

在test数据库中创建user表

```sql
-- ----------------------------
-- Table structure for `user`
-- ----------------------------
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(50) DEFAULT NULL,
  `password` varchar(50) DEFAULT NULL,
  `name` varchar(50) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of user
-- ----------------------------
INSERT INTO `user` VALUES ('1', 'zhangsan', '123', '张三');
INSERT INTO `user` VALUES ('2', 'lisi', '123', '李四');

```

com.example.domain.User

```java
public class User {
    // 主键
    private Long id;
    // 用户名
    private String username;
    // 密码
    private String password;
    // 姓名
    private String name;
  
    //此处省略getter和setter方法 .. ..
    
}

```

### 5.1.4 编写Mapper接口

com.example.mapper.UserMapper

```java
@Mapper
public interface UserMapper {
	public List<User> queryUserList();
}

```

注意：`@Mapper`标记该类是一个mybatis的mapper接口，**可以被spring boot自动扫描到spring上下文中**

### 5.1.5 配置Mapper映射文件

在src\main\resources\mapper路径下加入UserMapper.xml配置文件

```xml
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.itheima.mapper.UserMapper">
    <select id="queryUserList" resultType="user">
        select * from user
    </select>
</mapper>

```

### 5.1.6 编写测试Controller

com.example.controller.UserController

```java
@RestController
public class UserController {

    @Autowired
    private UserMapper userMapper;//报错，没有此类型的bean。但是可以使用，在运行期会创建好bean。还可以添加component之类注解

    @RequestMapping("/queryUser")
    public List<User> queryUser() {
        List<User> users = userMapper.queryUserList();
        return users;
    }
}

```

测试略



## 5.2 SpringBoot集成JUnit

### 5.2.1 添加JUnit的起步依赖

一般使用Spring Initializr添加Web依赖后会自动导入**JUnit**起步依赖

```xml
<!--测试的起步依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>

```

### 5.2.2 编写测试类

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = SpringbootMybatisApplication.class)//不指定会从main下找启动类，加载Spring容器。但须在同一包中！
public class MapperTest {

    @Autowired
    private UserMapper userMapper;

    @Test
    public void test() {
        List<User> users = userMapper.queryUserList();
        System.out.println(users);
    }

}

```

其中，

SpringRunner继承自SpringJUnit4ClassRunner，使用哪一个Spring提供的测试测试引擎都可以

```java
public final class SpringRunner extends SpringJUnit4ClassRunner 


```

@SpringBootTest的属性指定的是引导类的字节码对象





## 5.3 集成Spring Data JPA

### 5.3.1 起步依赖

使用Spring Initializr添加 DevTools、Web、MySQL、**JPA**依赖

```xml
<!-- MySQL连接驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
<!-- Spring Data JPA的起步依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

```

### 5.3.3 数据库连接和JPA配置

在application.properties/yml中添加数据库的连接信息、Spring Boot 集成Spring Data JPA的配置

```yaml
spring:
  #DB Configuration:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3308/test?serverTimezone=GMT%2B8&useSSL=false
    username: root
    password: w111151
  #JPA Configuration:
  jpa:
    database: mysql
    show-sql: true
    generate-ddl: true
    hibernate:
      ddl-auto: update
      #命名策略，瞎选的
      naming:
        physical-strategy: org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
        implicit-strategy: org.hibernate.boot.model.naming.ImplicitNamingStrategyComponentPathImpl
        #Hibernate 4 naming strategy fully qualified name. Not supported with Hibernate 5
        #strategy: org.hibernate.cfg.ImprovedNamingStrategy


```

```properties
#DB Configuration:
spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3308/test?serverTimezone=GMT%2B8&useSSL=false
spring.datasource.username=root
spring.datasource.password=w111151

#JPA Configuration:
spring.jpa.database=MySQL
spring.jpa.show-sql=true
spring.jpa.generate-ddl=true
spring.jpa.hibernate.ddl-auto=update

#Configure Hibernate Naming Strategy，瞎选的，不知道区别

spring.jpa.hibernate.naming.physical-strategy=org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
spring.jpa.hibernate.naming.implicit-strategy=org.hibernate.boot.model.naming.ImplicitNamingStrategyComponentPathImpl

#Hibernate 4 naming strategy fully qualified name. Not supported with Hibernate 5
#spring.jpa.hibernate.naming_strategy=org.hibernate.cfg.ImprovedNamingStrategy


```

### 5.3.4 创建实体配置实体

若实体类不在启动类包中，需要添加注解如`@EntityScan("com.xuecheng.domain")`来扫描实体类

```java
@Entity
public class User {
    // 主键
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    //@Column可以省略，采用默认配置，若要具体配置，需要写上
    private Long id;
    // 用户名
    private String username;
    // 密码
    private String password;
    // 姓名
    private String name;
 
    //此处省略setter和getter方法... ...
}

```

### 5.3.5 编写UserRepository

```java
public interface UserRepository extends JpaRepository<User,Long>, JpaSpecificationExecutor<User> {
}

```

### 5.3.6 编写测试类

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = SpringbootJpaApplication.class)
public class JpaTest {

    @Autowired
    private UserRepository userRepository;

    @Test
    public void testJpa(){
        List<User> all = userRepository.findAll();
        System.out.println(all);
    }
}

```

注意：如果是jdk9，执行报错如下：

原因：jdk缺少相应的jar

解决方案：手动导入对应的maven坐标，如下：

```xml
<!--jdk9需要导入如下坐标-->
<dependency>
    <groupId>javax.xml.bind</groupId>
    <artifactId>jaxb-api</artifactId>
    <version>2.3.0</version>
</dependency>

```



## 5.4 SpringBoot集成Spring Data Redis

### 5.4.1 起步依赖

使用Spring Initializr添加 DevTools、Web、MySQL、JPA、**Redis**依赖

```xml
<!-- 配置使用redis启动器 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>


```

### 5.4.2 配置redis的连接信息

```yaml
spring: 
  redis:
    #url包括以下host，port，password
    url: redis://user:password@example.com:6379
    host: localhost
    port: 6379
    password: w111151


```

```properties
#Redis
spring.redis.host=127.0.0.1
spring.redis.port=6379


```

### 5.4.3 注入RedisTemplate测试redis操作

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = SpringbootJpaApplication.class)
public class RedisTest {

    @Autowired
    private RedisTemplate<String,String> redisTemplate;

    @Autowired
    private UserRepository userRepository;

    @Test
    public void test() throws JsonProcessingException {
        //1.从Redis中获取数据，一般为JSON字符串
        String userListJson = redisTemplate.boundValueOps("user.findAll").get();
        //2.判断Redis中是否存在该数据
        //2.1不存在，从数据库查，并保存到Redis
        if (userListJson==null){
            List<User> all = userRepository.findAll();
            //转为JSON字符串，Web环境默认导入了Jackson
            ObjectMapper objectMapper = new ObjectMapper();
            userListJson = objectMapper.writeValueAsString(all);
            redisTemplate.boundValueOps("user.findAll").set(userListJson);
            System.out.println("从数据库查询");
        } else {
            //2.2存在
            System.out.println("从Redis查询");
        }
        System.out.println(userListJson);
    }
}

```



# 4 Spring MVC 整合

## 1 Spring MVC 入门

### 1.1 简介

> 三层架构
>
> - 表现层（web）：接收客户端请求，向客户端响应结果。包括控制层(接收请求)、展示层(展示结果)，采用MVC模型 
> - 业务逻辑层（service）：处理具体的业务逻辑，包括事务
> - 持久层（dao）：数据持久化，即和数据库交互，对数据库表进行增删改查。也可用 repository 替代。
>
> MVC模型（Model View Controller），即模型视图控制器，每个部分各司其职。 
>
> - Model：数据模型，JavaBean的类，用来进行数据封装
> - View：指jsp或者html。作用一般就是展示数据的。通常视图是依据模型数据创建的
> - Controller：是应用程序中处理用户交互的部分，作用一般就是处理程序逻辑的，整个流程的控制器。可以数据校验等

正如入门中依赖，选择`spring-boot-starter-web`即可使用Spring MVC。

SpringMVC概述

- 是一种基于**Java**实现的MVC设计模型的**请求驱动**类型的**轻量级WEB框架**。 
- Spring MVC属于SpringFrameWork的后续产品，已经**融合在Spring Web Flow**里面。Spring 框架提供 了构建 Web 应用程序的全功能 MVC 模块。 
- 使用 Spring **可插入的 MVC 架构**，从而在使用 Spring 进行 WEB 开发时，可以选择使用 Spring 的 SpringMVC框架或集成其他 MVC 开发框架，如 WebFlux ，Struts1(现在一般不用)，Struts2等。 

SpringMVC 和 Struts2 的优劣分析

- 共同点：  
  - 它们都是表现层框架，都是基于 MVC 模型编写的。  
  - 它们的底层都离不开原始 ServletAPI。  
  - 它们处理请求的机制都是一个核心控制器。 
- 区别：
  - Spring MVC 的入口是 Servlet, 而 Struts2 是 Filter 
  - Spring MVC 是基于方法设计的，而 Struts2 是基于类，Struts2 每次执行都会创建一个动作类，比Spring MVC会稍慢
  - Spring MVC 使用更加简洁,同时还支持 JSR303, 处理 ajax 的请求更方便 
  - Struts2 的 OGNL 表达式使页面的开发效率相比 Spring MVC 更高些，但执行效率并没有比 JSTL 提 升，尤其是 struts2 的表单标签，远没有 html 执行效率高

### 1.2 执行过程分析

1. 当启动Tomcat服务器的时候，因为配置了`load-on-startup`标签，所以会创建`DispatcherServlet`对象， 并分局初始化参数加载`spring-config.xml`配置文件或`SpringConfiguration`配置类 
2. 由于开启了注解扫描，那么`HelloController`对象就会被创建
3. 从index.jsp发送请求，请求会先到达`DispatcherServlet`前端控制器，根据配置`@RequestMapping`注解找到执行的方法
4. 根据执行方法的返回值，再根据配置的视图解析器，去指定的目录下查找指定名称的JSP文件 
5. Tomcat服务器渲染页面，做出响应 



### 1.3 组件分析

在 SpringMVC 的各个组件中，**处理器映射器**、**处理器适配器**、**视图解析器**称为 SpringMVC 的三大组件。 

我们只需要编写处理具体业务的**控制器**以及**视图**

- DispatcherServlet

前端控制器：用户请求到达前端控制器，它就相当于 mvc 模式中的 c，dispatcherServlet 是整个流程控制的中心，由它调用其它组件处理用户的请求，dispatcherServlet 的存在降低了组件之间的耦合性

- HandlerMapping

处理器映射器：HandlerMapping 负责根据用户请求找到 Handler 即处理器，SpringMVC 提供了不同的映射器实现不同的 映射方式，例如：配置文件方式，实现接口方式，注解方式等。 

- Handler

它就是我们开发中要编写的具体业务控制器。由 DispatcherServlet 把用户请求转发到 Handler。由 Handler 对具体的用户请求进行处理。

- HandlerAdapter 

通过 HandlerAdapter 对处理器进行执行，这是**适配器模式**的应用，通过扩展适配器可以对更多类型的处理器进行执行

- View Resolver

View Resolver 负责将处理结果生成 View 视图，View Resolver 首先根据逻辑视图名解析成物理视图名 即具体的页面地址，再生成 View 视图对象，最后对 View 进行渲染将处理结果通过页面展示给用户

- View

SpringMVC 框架提供了很多的 View 视图类型的支持，包括：jstlView、freemarkerView、pdfView 等。最常用的视图就是jsp

一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由程序员根据业务需求开发具体的页面



### 1.4 静态资源的映射规则（旧）

```java
@ConfigurationProperties(prefix = "spring.resources", ignoreUnknownFields = false)
public class ResourceProperties implements ResourceLoaderAware {
  //可以设置和静态资源有关的参数，缓存时间等

```

- **WebMvcAuotConfiguration.java**： SpringMVC相关配置都在这个类中

```java
//org\springframework\boot\autoconfigure\web\servlet\WebMvcAutoConfiguration.java
@Override
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    if (!this.resourceProperties.isAddMappings()) {
        logger.debug("Default resource handling disabled");
        return;
    }
    //webjars/映射
    Integer cachePeriod = this.resourceProperties.getCachePeriod();
    if (!registry.hasMappingForPattern("/webjars/**")) {
        customizeResourceHandlerRegistration(
            registry.addResourceHandler("/webjars/**")
            .addResourceLocations(
                "classpath:/META-INF/resources/webjars/")
            .setCachePeriod(cachePeriod));
    }
    String staticPathPattern = this.mvcProperties.getStaticPathPattern();
    // 静态资源文件夹映射
    if (!registry.hasMappingForPattern(staticPathPattern)) {
        customizeResourceHandlerRegistration(
            registry.addResourceHandler(staticPathPattern)
            .addResourceLocations(
                this.resourceProperties.getStaticLocations())
            .setCachePeriod(cachePeriod));
    }
}

// 配置欢迎页映射
@Bean
public WelcomePageHandlerMapping welcomePageHandlerMapping(
    ResourceProperties resourceProperties) {
    return new WelcomePageHandlerMapping(resourceProperties.getWelcomePage(),
                                         this.mvcProperties.getStaticPathPattern());
}

// 配置喜欢的图标
@Configuration
@ConditionalOnProperty(value = "spring.mvc.favicon.enabled", matchIfMissing = true)
public static class FaviconConfiguration {

    private final ResourceProperties resourceProperties;

    public FaviconConfiguration(ResourceProperties resourceProperties) {
        this.resourceProperties = resourceProperties;
    }

    @Bean
    public SimpleUrlHandlerMapping faviconHandlerMapping() {
        SimpleUrlHandlerMapping mapping = new SimpleUrlHandlerMapping();
        mapping.setOrder(Ordered.HIGHEST_PRECEDENCE + 1);
        // 所有  **/favicon.ico 
        mapping.setUrlMap(Collections.singletonMap("**/favicon.ico",
                                                   faviconRequestHandler()));
        return mapping;
    }

    @Bean
    public ResourceHttpRequestHandler faviconRequestHandler() {
        ResourceHttpRequestHandler requestHandler = new ResourceHttpRequestHandler();
        requestHandler
            .setLocations(this.resourceProperties.getFaviconLocations());
        return requestHandler;
    }

}


```

- 所有**/webjars/\****访问 ，都去 **classpath:/META-INF/resources/webjars/** 找资源

  - `webjars`：以jar包的方式引入静态资源；[进官网查看详细信息](<http://www.webjars.org/>)

  - 引入依赖

    ```xml
    <!--引入jquery-webjar;在访问的时候只需要写webjars下面资源的名称即可-->
    <dependency>
        <groupId>org.webjars</groupId>
        <artifactId>jquery</artifactId>
        <version>3.3.1-1</version>
    </dependency>
    
    
    ```

    ![](./images/jquery.png)

  - 访问：localhost:8080/webjars/jquery/3.3.1-1/jquery.js

- "/**" 访问当前项目的任何资源，都去（静态资源的文件夹里）找映射

  ```java
  "classpath:/META-INF/resources/", 
  "classpath:/resources/",
  "classpath:/static/", 
  "classpath:/public/" 
  "/"：当前项目的根路径
  
  
  ```

  - 访问：localhost:8080/abc === 去静态资源文件夹里面找abc

- **欢迎页**：静态资源文件夹下的所有index.html页面；被"/**"映射

  - 访问：localhost:8080/，找index页面

- **图标**：所有的 **/favicon.ico 都是在静态资源文件下找



## 2 请求参数的绑定

绑定机制 

1. 表单提交的数据都是k=v格式的 `username=haha&password=123`
2. SpringMVC的参数绑定过程是把**表单提交的请求参数**，作为控制器中**方法的参数**进行绑定的 



### 2.1 基本数据类型和字符串类型

当**请求的参数名称**和**处理器形参名称**一致时（**区分大小写**）会将请求参数与形参进行绑定

注意：参数类型**推荐**使用**包装数据类型**，因为基础数据类型不可为null；布尔类型的参数，请求的参数值为true或false，1或0

```java
@RequestMapping("/edit")
public String queryItemById(Integer id) {
    System.out.println(id);
    return "success";
}

```



### 2.2 POJO类型及它的关联对象

要求表单中**参数名称**和 POJO 类的**属性名称**保持一致。并且控制器方法的**参数类型是 POJO 类型**

```java
public class Account implements Serializable { 
    private Integer id;  
    private String name;  
    private Float money;  
    private Address address;  
    //getters and setters 
} 

```

```java
public class Address implements Serializable {    
    private String provinceName;  
    private String cityName;  
    //getters and setters 
}

```

```html
<form action="account/saveAccount" method="post"> 
    账户名称：<input type="text" name="name" ><br/> 
    账户金额：<input type="text" name="money" ><br/>  
    账户省份：<input type="text" name="address.provinceName" ><br/>  
    账户城市：<input type="text" name="address.cityName" ><br/>  
    <input type="submit" value=" 保存 "> 
</form>

```

```java
@RequestMapping("/saveAccount") 
public String saveAccount(Account account) {  
    System.out.println("保存了账户。。。。"+account);  
    return "success"; 
}

```

#### 2.2.1 POST请求参数乱码问题

`web.xml`中配置过滤器。Tomcat8及之后的已经解决GET请求参数乱码问题

```xml
<!-- 配置过滤器，解决post的乱码问题 -->
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <!-- 启动过滤器，好像不配置这个也行 -->    
    <init-param>     
        <param-name>forceEncoding</param-name>   
        <param-value>true</param-value>    
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>

```



### 2.3 POJO 类中包含集合类型参数

第一种：（如下面代码）

- 要求集合类型的请求参数必须在 **POJO** 中。在表单中**请求参数名称**要和 POJO 中集合**属性名称相同**。
- 给 List 集合中的元素赋值，使用下标。 
- 给 Map 集合中的元素赋值，使用键值对。 

第二种

- 接收的请求参数是 json 格式数据。需要借助一个注解实现。 

```java
public class User implements Serializable {    
    private String username;  
    private String password;  
    private Integer age;  
    private List<Account> accountList;  
    private Map<String,Account> accountMap;    
    //getters and setters
}

```

```html
<form action="account/updateAccount" method="post">
    用户名称：<input type="text" name="username"><br/>
    用户密码：<input type="password" name="password"><br/>
    用户年龄：<input type="text" name="age"><br/>
    账户 1 名称：<input type="text" name="accountList[0].name"><br/>
    账户 1 金额：<input type="text" name="accountList[0].money"><br/>
    账户 2 名称：<input type="text" name="accountList[1].name"><br/>
    账户 2 金额：<input type="text" name="accountList[1].money"><br/>
    账户 3 名称：<input type="text" name="accountMap['one'].name"><br/>
    账户 3 金额：<input type="text" name="accountMap['one'].money"><br/>
    账户 4 名称：<input type="text" name="accountMap['two'].name"><br/>
    账户 4 金额：<input type="text" name="accountMap['two'].money"><br/>
    <input type="submit" value=" 保存 ">
</form>

```

```java
@RequestMapping("/updateAccount") 
public String updateAccount(User user) {  
    System.out.println("更新了账户。。。。"+user);  
    return "success"; 
}

```







### 2.4 Servlet原生API及默认支持类型

**控制器**(处理器)**形参**中添加如下类型的参数，处理适配器会默认识别并进行赋值

- `HttpServletRequest`：通过request对象获取请求信息
- `HttpServletResponse`：通过response处理响应信息
- `HttpSession`：通过session对象得到session中存放的对象

------

**默认支持的参数类型**

- `Model`/`ModelMap`

  - 除了`ModelAndView`以外，还可以使用`Model`(接口)来**向页面传递数据**，在参数里**直接声明Model即可**

    使用Model可以不使用ModelAndView对象，Model对象可以向页面传递数据，View对象可以使用String返回值替代。==**其本质都是使用Request域对象传递数据**==

    ```java
    model.addAttribute("item", item);
    return "itemEdit";
    
    ```

  - `ModelMap`是`Model`接口的实现类，也可以通过`ModelMap`向页面传递数据。效果一样，同上代码



### 2.5 自定义类型转换器

除了类型转换器，SpringMVC还提供了**注解`@DateTimeFormate`来转换日期格式**。查看5常用注解这一章。

SpringMVC还可以实现一些**数据类型自动转换**。内置转换器全都在`org.springframework.core.convert.support`包下。如String转Integer等等

如遇**特殊类型转换**要求，比如日期数据有很多种格式，SpringMVC没办法把带`-`字符串转换成日期类型，需要我们自己编写**自定义类型转换器**。步骤如下：

1. 定义一个类，**实现 Spring提供的 `Converter` 接口**，该接口有两个泛型。

   ```java
   //Converter<S, T>：  S:source,需要转换的源的类型；T:target,需要转换的目标类型
   @Componet
   public class StringToDateConverter implements Converter<String, Date> {
       @Override
       public Date convert(String source) {
           DateFormat format = null;
           try {
               if (StringUtils.isEmpty(source)) {
                   throw new NullPointerException("请输入要转换的日期");
               }
               format = new SimpleDateFormat("yyyy-MM-dd");
               Date date = format.parse(source);
               return date;
           } catch (Exception e) {
               throw new RuntimeException("输入日期有误");
           }
       }
   }
   
   ```

2. 在 spring配置文件中**配置类型转换器**。JavaConfig暂时不会怎么配置

   spring 配置类型转换器的机制是，将自定义的转换器注册到类型转换服务中去。 

   ```xml
   <!-- 配置类型转换器工厂 --> 
   <bean id="converterService"   class="org.springframework.context.support.ConversionServiceFactoryBean"> 
   <!--FormattingConversionServiceFactoryBean可以让SpringMVC支持和@DateTimeFormat等Spring内部自定义的转换器，建议-->
    <!-- 给工厂注入一个新的类型转换器 -->      
       <property name="converters"> 
         <array> 
          <!-- 配置自定义类型转换器 -->       
             <ref bean="stringToDateConverter"/>
         </array>      
       </property> 
   </bean>
   <!-- 配置spring开启注解mvc的支持，替代处理器映射器和处理器适配器配置，并配置类型转换器 -->
   <mvc:annotation-driven conversion-service="conversionService" />
   
   ```



## 3 响应数据和结果视图

### 3.1 返回String

- **逻辑视图名**

  Controller中方法返回字符串可以**指定逻辑视图名**，通过**视图解析器解析为物理视图地址**。 

  > 如：`return "success"; `指定逻辑视图名，经过视图解析器解析为 jsp 物理路径如`/WEB-INF/pages/success.jsp` 

- **`forward:`转发**：`return "forward:pages/success.jsp"; `则路径必须写成实际视图 url，不能写逻辑视图

- **`redirect:`重定向**：`return "redirect:testReturnModelAndView"; `路径可以不添加项目名称，会自动添加

  需要注意的是，如果是重定向到 jsp 页面，则 jsp 页面不 能写在 WEB-INF 目录中，否则无法找到

### 3.2 void(配合Ajax)

- 在Controller方法形参上可以绑定原生ServletAPI，使用request或response**指定响应结果**。常与`@ResponseBody`一起使用

  ```java
  //使用request请求转发页面
  request.getRequestDispatcher("页面路径").forward(request, response);
  //通过response页面重定向
  response.sendRedirect("url")
  //通过response指定响应结果，例如响应json数据如下
  response.setCharacterEncoding("utf-8"); 
  response.setContentType("application/json;charset=utf-8"); 
  response.getWriter().write("{\"abc\":123}");
  
  ```

### 3.3 ModelAndView

- ModelAndView 是 SpringMVC 为我们提供的一个对象，该对象也可以用作控制器方法的返回值。 该对象中有两个方法： 

  ```java
  @RequestMapping("/testReturnModelAndView") 
  public ModelAndView testReturnModelAndView() {  
      ModelAndView mv = new ModelAndView();  
      mv.addObject("username", "张三");  //存储的request域中
      mv.setViewName("success"); 
      return mv; 
  }
  
  ```



## 4 常用注解

### 4.1 `@RequestParam`

作用：把请求中指定名称的参数给控制器中的形参赋值。 但是还不如参数名称一致好使。可以用于**分页中默认值设置**（也不用）

属性：

- `value`或`name`：请求参数中的名称。若参数名称(name)和形参一致，可以不用指定value或name
- `required`：请求参数中是否必须提供此参数。默认值：true。表示必须提供，如果不提供将报错
- `defaultValue`：请求参数默认值

```html
<a href="springmvc/useRequestParam?name=test">requestParam 注解</a> 

```

```java
@RequestMapping("/useRequestParam") 
public String test( 
    @RequestParam("name")String username, 
    @RequestParam(value="age",required=false,defaultValue="88")Integer age){
    
   	System.out.println(username+","+age);  
    return "success"; 
}

```



### 4.2 `@PathVariable`

作用：用于**绑定 url 中的占位符**。例如：请求 url 中 `/delete/{id}`，这个`{id}`就是 url 占位符。  

​	url 支持占位符是 spring3.0 之后加入的。是 springmvc 支持 rest 风格 URL 的一个重要标志。 

属性：  

- `value`或`name`：用于指定 url 中占位符名称。若占位符名称和形参一致，可以不用指定value。尽量指定！！！
- `required`：是否必须提供占位符。 

```html
<a href="springmvc/usePathVariable/100">pathVariable 注解</a> 

```

```java
@RequestMapping("/usePathVariable/{sid}") 
public String usePathVariable(@PathVariable("sid") Integer sid){  
    System.out.println(sid);  
    return "success"; 
}

```



### 4.3 `@RequestBody`

作用：用于==**获取请求体**==内容。直接使用得到是`key=value&key=value`结构的数据。**get 请求方式不适用**。常用于**Json数据封装**

属性：`required`是否必须有请求体，默认为true。当取值为true时，get请求方式会报错；若为false，get请求得到是null

```html
<form action="springmvc/useRequestBody" method="post">
    用户名称：<input type="text" name="username" ><br/>  
    用户密码：<input type="password" name="password" ><br/>  
    用户年龄：<input type="text" name="age" ><br/> 
 <input type="submit" value=" 保存 "> </form>

```

```java
@RequestMapping("/useRequestBody") 
public String useRequestBody(@RequestBody(required=false) String body){  
    System.out.println(body);  
    return "success"; 
}

```



### 4.4 `@ResponseBody`

用于将 Controller 的方法返回的对象，通过 `HttpMessageConverter` 接口转换为指定格式的数据如：`json`,`xml` 等，通过 Response 响应给客户端 

如果需要SpringMVC支持JSON，必须加入JSON的处理**jar包**：Jackson

```xml
<dependency>            
    <groupId>com.fasterxml.jackson.core</groupId>            
    <artifactId>jackson-core</artifactId>            
    <version>2.9.0</version>        
</dependency>        
<dependency>            
    <groupId>com.fasterxml.jackson.core</groupId>            
    <artifactId>jackson-annotations</artifactId>            
    <version>2.9.0</version>        
</dependency>
<dependency>            
     <groupId>com.fasterxml.jackson.core</groupId>            
     <artifactId>jackson-databind</artifactId>            
     <version>2.9.0</version>        
</dependency>        

```

```java
@RequestMapping("/testJson")  
@ResponseBody
public Address testJson(@RequestBody Address address) {        
    System.out.println(address);        
    address.setAddressName("上海");        
    return address;    
}

```

```javascript
 $(function(){        
     // 绑定点击事件        
     $("#btn").click(function(){            
         $.ajax({                
             url:"user/testJson",  
             type:"post", 
             contentType:"application/json;charset=UTF-8",
             data:'{"addressName":"哈哈","addressNum":100}',
             success:function(data){                    
                 alert(data);                    
                 alert(data.addressName);                
             }
             dataType:"json",                                           
         });        
     });    
 }); 

```



### 4.5 `@RestController`

是`@Controller`和`@ResponseBody`的组合

### 4.6 `@RequestMapping`

作用：用于**建立请求 URL** 和**处理请求方法**之间的对应**关系**

```java
@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Mapping
public @interface RequestMapping {
    String name() default "";

    @AliasFor("path")
    String[] value() default {};
    @AliasFor("value")
    String[] path() default {};

    RequestMethod[] method() default {};
    String[] params() default {};
    String[] headers() default {};
    String[] consumes() default {};
    String[] produces() default {};
}

```

出现位置： 

- **类**上：请求URL的**第一级访问目录**，模块化管理。此处不写的话，就相当于应用的根目录。写的话需要以`/`开头，如`/user`

- **方法**上：请求 URL 的**第二级访问目录**。 如`/add`。但是RESTful风格的可以使用`@***Mapping`替代

  当使用分级配置时，前端页面的请求路径中，要么写绝对路径即带`/`和项目名，要么写**相对路径不带`/`**（表示相对应用的根目录），否则404

属性：

- **`value`**：用于指定**请求的 URL**。它和 **path** 属性的作用是一样的。**大小写敏感**
- **`method`**：用于指定**请求的方式**，值为`RequestMethod`枚举类
- `params`：用于指定限制请求参数的条件。它支持简单的表达式。要求请求参数的key和value必须和配置的一模一样。如：
  - `params = {"accountName"}`，表示请求参数必须有 accountName 
  - `params = {"moeny!100"}`，表示请求参数中 money 不能是 100
- `headers`：用于限定发送的请求中必须包含某请求头

### 4.7 `@Get/Post/Put/Delete/PatchMapping`

用在方法上，替代方法的`@RequestMapping`

### 4.8 `@DateTimeFormat`

直接在JavaBean**属性**上添加注释即可（在get或set方法上添加，命名规范的话字段上添加也行。由于一般自动生成，所以都行）

```java
@DateTimeFormat(pattern="yyyy-MM-dd HH:mm")
private Date creationTime;

```



### 4.9 RESTful

- RESTful是一个资源定位及资源操作的风格。使用POST、DELETE、PUT、GET、Patch等，使用不同方法对资源进行操作，分别对应  添加、 删除、修改、查询、不知道

- 需求：RESTful方式实现商品信息查询，返回json数据

  - **从URL上获取参数**：根据id查询商品，使用RESTful风格开发的接口地址是：http://127.0.0.1/item/1

    - 注解**`@RequestMapping("item/{id}")`**声明请求的URL，`{xxx}`为占位符，请求的URL是“`item /1`”

    - 使用**`@PathVariable() Integer id`**获取URL上的数据

      ```java
      @RequestMapping("item/{id}")
      public @ResponseBody Item queryItemById(@PathVariable Integer id) {
          Item item = this.itemService.queryItemById(id);
          return item;
      }
      
      ```

      - 如果`@RequestMapping`中表示为"`item/{id}`"，id和形参名称一致，`@PathVariable`不用指定名称。如果不一致，例如"`item/{ItemId}`"则需要指定名称`@PathVariable("itemId")`

    - **注意**：

      - @PathVariable是获取url上数据的。@RequestParam获取请求参数的（包括post表单提交）
      - 如果加上@ResponseBody注解，就不会走视图解析器，不会返回页面，返回如json数据。如果不加，就走视图解析器，返回页面

> 使用jQuery时注意：
>
> - 表单只支持GET、POST请求，若要发送其他请求，表单本身设置为POST请求，并需要input中属性`name="_method" value="PUT"`，Ajax中查看HTML章节
> - 后端中需要配置过滤器`org.springframework.web.filter.HiddenHttpMethodFilter`

### 4.10 其他不常用注解

#### `@RequestHeader`

一般不怎么用

- 作用：用于获取请求消息头。 
- 属性：`value`提供消息头名称。`required`是否必须有此消息头 

------

#### `@CookieValue`

一般不怎么用

- 作用：用于把指定 cookie 名称的值传入控制器方法参数。 
- 属性：`value`指定 cookie 的名称。`required`是否必须有此 cookie

------

#### `@ModelAttribute`

> 该注解是 SpringMVC4.3 版本以后新加入的。它可以用于修饰方法和参数。 
>
> - 出现在**方法上**，表示当前方法会在**控制器的方法执行之前先执行**。它可以修饰没有返回值和有具体返回值的方法
> - 出现在**参数上**，获取**指定的数据给参数赋值**

- 属性：`value`用于获取数据的 key。key 可以是 **POJO** 的属性名称，也可以是 **map** 结构的 key。 

- 应用场景：当表单提交数据不是完整的实体类数据时，保证**没有提交数据的字段**使用**数据库对象原来的数据**。

  - 我们在编辑一个用户时，用户有一个创建信息字段，该字段的值是不允许被修改的。在提交表单数据是肯定没有此字段的内容，一旦更新会把该字段内容置为 null，此时就可以使用此注解解决问题

- 基于POJO 属性的基本使用

  ```html
  <a href="springmvc/testModelAttribute?username=test">测试 modelattribute</a> 
  
  ```

  ```java
  @ModelAttribute  
  public void showModel(User user) {   
      System.out.println("执行了 showModel 方法"+user.getUsername());  
  } 
  @RequestMapping("/testModelAttribute") 
  public String testModelAttribute(User user) {   
      System.out.println("执行了控制器的方法"+user.getUsername());   
      return "success";  
  }
  //执行了 showModel 方法
  //执行了控制器的方法
  
  ```

- 基于 Map 的应用场景示例 1：ModelAttribute 修饰方法带返回值 

  ```html
  <!--需求：  修改用户信息，要求用户的密码不能修改 -->
  <form action="springmvc/updateUser" method="post"> 
      用户名称：<input type="text" name="username" ><br/>  
      用户年龄：<input type="text" name="age" ><br/>  
      <input type="submit" value=" 保存 "> 
  </form> 
  
  
  ```

  ```java
  // 模拟修改用户方法 
  @RequestMapping("/updateUser") 
  public String testModelAttribute(User user) {  
      System.out.println("控制器中处理请求的方法：修改用户："+user);  
      return "success"; 
  }
  @ModelAttribute public User showModel(String username) {  
      //模拟去数据库查询  
      User abc = findUserByName(username); 
      System.out.println("执行了 showModel 方法"+abc);  
      return abc; 
  } 
  // 模拟去数据库查询 
  private User findUserByName(String username) {  
      User user = new User();  
      user.setUsername(username);
      user.setAge(19);  
      user.setPassword("123456");  
      return user; 
  }
  //输出会给未提交的age字段赋值19，其他的使用提交的数据
  
  ```

- 基于 Map 的应用场景示例 2：ModelAttribute 修饰方法不带返回值 

  ```java
  @RequestMapping("/updateUser") 
  public String testModelAttribute(@ModelAttribute("abc")User user) {  
      System.out.println("控制器中处理请求的方法：修改用户："+user);  
      return "success"; 
  }
  @ModelAttribute 
  public void showModel(String username,Map<String,User> map) { 
   	//模拟去数据库查询  
      User user = findUserByName(username); 
      System.out.println("执行了 showModel 方法"+user);  
      map.put("abc",user); 
  } 
  // 模拟去数据库查询 
  private User findUserByName(String username) {  
      User user = new User();  
      user.setUsername(username);
      user.setAge(19);  
      user.setPassword("123456");  
      return user; 
  }
  //输出会给未提交的age字段赋值19，其他的使用提交的数据
  
  ```

------

#### `@SessionAttribute`

- 作用：用于多次执行控制器方法间的参数共享。 

- 属性：

  - `value`：用于指定存入的属性名称  
  - `type`：用于指定存入的数据类型。 

  ```java
  @Controller("sessionAttributeController") 
  @RequestMapping("/springmvc") 
  @SessionAttributes(value= {"username","password","age"},types= {String.class,Integer.class})//存入到session域 
  public class SessionAttributeController { 
      @RequestMapping("/testPut")    
      public String testPut(Model model){           
          model.addAttribute("username", "泰斯特");           
          model.addAttribute("password","123456");           
          model.addAttribute("age", 31);   
          //跳转之前将数据保存到 username、password 和 age 中，因为注解@SessionAttribute 中有这几个参数 
          return "success"; 
      }
      
      @RequestMapping("/testGet")       
      public String testGet(ModelMap model){           
          System.out.println(model.get("username")+";"+model.get("password")+";"+model.get("age"));           
          return "success";       
      } 
      
      @RequestMapping("/testClean")        
      public String complete(SessionStatus sessionStatus){         
          sessionStatus.setComplete();            
          return "success";        
      }
  }
  
  ```



## 5 处理Multipart数据（旧）

pom.xml中添加依赖

```xml
<dependency>            
    <groupId>commons-fileupload</groupId>            
    <artifactId>commons-fileupload</artifactId>            
    <version>1.3.1</version>        
</dependency>        
<dependency>            
    <groupId>commons-io</groupId>            
    <artifactId>commons-io</artifactId>            
    <version>2.4</version>        
</dependency

```

### 5.1 文件上传的回顾 

- **form表单的`enctype`取值必须是`multipart/form-data`**(默认值是`application/x-www-form-urlencoded`)。`enctype`代表表单请求正文的类型
- `method` 属性取值必须是** `Post`**
- 提供一个**文件选择域**`<input type="file" />  `

```html
<form action="user/fileupload" method="post" enctype="multipart/form-data">       
    选择文件：<input type="file" name="upload"/><br/>        
    <input type="submit" value="上传文件"/>    
</form>


```

```java
@RequestMapping(value="/fileupload")    
public String fileupload(HttpServletRequest request) throws Exception {        
    // 先获取到要上传的文件目录        
    String path = request.getSession().getServletContext().getRealPath("/uploads");        
    // 创建File对象，一会向该路径下上传文件        
    File file = new File(path);        
    // 判断路径是否存在，如果不存在，创建该路径        
    if(!file.exists()) {            
        file.mkdirs();        
    }        
    // 创建磁盘文件项工厂        
    DiskFileItemFactory factory = new DiskFileItemFactory();        
    ServletFileUpload fileUpload = new ServletFileUpload(factory);        
    // 解析request对象        
    List<FileItem> list = fileUpload.parseRequest(request);        
    // 遍历        
    for (FileItem fileItem : list) {            
        // 判断文件项是普通字段，还是上传的文件            
        if(fileItem.isFormField()) {                            

        }else {                
            // 上传文件项
            // 获取到上传文件的名称                
            String filename = fileItem.getName();               
            // 上传文件                
            fileItem.write(new File(file, filename));                
            // 删除临时文件                
            fileItem.delete();            
        }        
    }                
    return "success";    
}


```



### 5.2 SpringMVC传统方式的文件上传

> 传统方式的文件上传，指的是我们上传的文件和访问的应用存在于同一台服务器上。 并且上传完成之后，浏览器可能跳转。 

```java
@RequestMapping(value="/fileupload2")    
public String fileupload2(HttpServletRequest request,MultipartFile upload) throws Exception {        
    System.out.println("SpringMVC方式的文件上传...");        
    // 先获取到要上传的文件目录        
    String path = request.getSession().getServletContext().getRealPath("/uploads");        
    // 创建File对象，一会向该路径下上传文件        
    File file = new File(path);        
    // 判断路径是否存在，如果不存在，创建该路径        
    if(!file.exists()) {            
        file.mkdirs();        
    }        
    // 获取到上传文件的名称        
    String filename = upload.getOriginalFilename();        
    String uuid = UUID.randomUUID().toString().replaceAll("-", "").toUpperCase();        
    // 把文件的名称唯一化        
    filename = uuid+"_"+filename;        
    // 上传文件        
    upload.transferTo(new File(file,filename));        
    return "success";    
}


```

- 在**`spring-config.xml`配置文件解析器**

```xml
<!-- 配置文件上传解析器，id是固定的！！！--> 
<bean id="multipartResolver"  class="org.springframework.web.multipart.commons.CommonsMultipartResolver"> 
 <!-- 设置上传文件的最大尺寸为 5MB -->  
    <property name="maxUploadSize">   
        <value>5242880</value>  
    </property> 
</bean>


```

- JavaConfig配置**MultipartResolver**接口的实现类

  - `CommonsMultipartResolver`：使用Jakarta Commons FileUpload解析multipart请求

  - **`StandardServletMultipartResolver`**：依赖于**Servlet3.0**对multipart请求支持（**始于Spring3.1**）

    选择这个，它使用Servlet所提供的功能支持，不依赖其他项目。它**没有构造器参数和属性**

    ```java
    @Bean
    public MultipartResolver multipartResolver() throws IOException {
        return new StandardServletMultipartResolver();
    }
    
    
    ```

    如果配置DispatcherServlet的Servlet初始化类继承了**AbstractAnnotationConfigDispatcherServletInitializer**或AbstractDispatcherServletInitializer的话，通过**重载customize Registration()方法**（它会得到Dynamic参数）来配置multipart的具体细节

    ```java
    //class Config extends AbstractAnnotationConfigDispatcherServletInitializer
    @Override
    protected void customizeRegistration(Dynamic registration) {
        registration.setMultipartConfig(new MultipartConfigElement("/tmp/file/uploads",2097152,4194304,0));
        //location,maxFileSize,maxRequestSize,fileSizeThreshold(为0则上传文件写到磁盘)
    }
    
    
    ```



### 5.3 SpringMVC跨服务器方式的文件上传

> 在实际开发中，我们会有很多处理不同功能的服务器（不是服务器集群）。例如： 
>
> - 应用服务器：负责部署我们的应用 
> - 文件服务器：负责存储用户上传文件的服务器
> - 数据库服务器：运行我们的数据库 
> - ……
>
> 目的是让服务器各司其职，从而提高我们项目的运行效率

步骤：

1. **搭建图片服务器** 

   1. 根据文档配置tomcat9的服务器，现在是2个服务器 
   2. 导入资料中day02_springmvc5_02image项目，作为图片服务器使用 

2. 实现SpringMVC跨服务器方式文件上传

   1. 导入依赖的jar包的坐标（sun公司提供的，下面导包时注意）

      ```xml
      <dependency>            
          <groupId>com.sun.jersey</groupId>            
          <artifactId>jersey-core</artifactId>            
          <version>1.18.1</version>        
      </dependency>        
      <dependency>            
          <groupId>com.sun.jersey</groupId>            
          <artifactId>jersey-client</artifactId>            
          <version>1.18.1</version>        
      </dependency>
      
      
      ```

   2. 控制器

      ```java
      @RequestMapping(value="/fileupload3")
      public String fileupload3(MultipartFile upload) throws Exception {        
          System.out.println("SpringMVC跨服务器方式的文件上传...");                
          // 定义图片服务器的请求路径        
          String path = "http://localhost:9090/day02_springmvc5_02image/uploads/";//创建好该文件夹              
          // 获取到上传文件的名称        
          String filename = upload.getOriginalFilename();        
          String uuid = UUID.randomUUID().toString().replaceAll("-", "").toUpperCase();        
          // 把文件的名称唯一化        
          filename = uuid+"_"+filename;        
          // 向图片服务器上传文件                
          // 创建客户端对象        
          Client client = Client.create();        
          // 连接图片服务器        
          WebResource webResource = client.resource(path+filename);        
          // 上传文件        
          webResource.put(upload.getBytes());        
          return "success";    
      }
      
      
      ```

   3. **配置文件解析器**，同上



## 6 异常处理器（旧）

- SpringMVC在处理请求过程中出现异常信息交由异常处理器进行处理，自定义异常处理器可以实现一个系统的异常处理逻辑

- 思路：

  - 系统中异常包括两类：**预期异常**和运行时异常**RuntimeException**，前者通过捕获异常从而获取异常信息，后者主要通过规范代码开发、测试通过手段减少运行时异常的发生
  - 系统的dao、service、controller出现都通过throws Exception向上抛出，最后由SpringMVC**前端控制器交由异常处理器**进行异常处理

- **自定义异常类(继承Exception或RuntimeException)**：为了区别不同的异常,通常根据异常类型进行区分

  ```java
  public class MyException{
      public MyException(){};
      public MyException(String msg){
          super(msg);
      };
  }
  
  
  ```

- **自定义异常处理器(实现HandlerExceptionResolver)**，并**在`spring-config.xml`中配置或使用`@Component`**

  ```java
  @Component
  public class CustomExceptionResolver implements HandlerExceptionResolver {
     	//handler:异常处理器对象。发生异常的地方，包名+类名+方法名(形参)的字符串，用于日志
      @Override
      public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler,Exception exception) {
          
          ModelAndView modelAndView = new ModelAndView();
          // 定义异常信息
          String msg = "";
  
          // 判断异常类型
          if (exception instanceof MyException) {
              // 如果是自定义异常，读取异常信息
              msg = exception.getMessage();
          } else {
              //简写
              msg = "服务器访问量过大，请您稍后..."
              //或 如果是运行时异常，则取错误堆栈，从堆栈中获取异常信息
              //Writer out = new StringWriter();
              //PrintWriter s = new PrintWriter(out);
              //exception.printStackTrace(s);
              //msg = out.toString();
  
          }
          // 把错误信息发给相关人员,邮件,短信等方式
          // 返回错误页面，给用户友好页面显示错误信息
         
          modelAndView.addObject("msg", msg);
          modelAndView.setViewName("error");
  
          return modelAndView;
      }
  }
  
  ```



## 7 拦截器（旧）

- 类似于Servlet 开发中的过滤器Filter，用于对处理器进行预处理和后处理。AOP思想的具体应用
- 区别：
  - 过滤器是 servlet 规范中的一部分，任何 java web 工程都可以使用。 
  - 拦截器是 SpringMVC 框架自己的，只有使用了 SpringMVC 框架的工程才能用。 
  - 过滤器在 url-pattern 中配置了`/*`之后，可以对所有要访问的资源拦截。 
  - **拦截器**它是**只会拦截访问的控制器方法**，如果访问的是 jsp,html,css,image 或者 js 是不会进行拦截的

使用步骤：

- **实现HandlerInterceptor接口，并重写三个默认方法**

  ```java
  public class HandlerInterceptor1 implements HandlerInterceptor {
      // Controller执行前调用此方法
      // 返回true表示放行，返回false不放行
      // 这里可以加入登录校验、权限拦截等
      @Override
      public boolean preHandle(HttpServletRequest arg0, HttpServletResponse arg1, Object arg2) throws Exception {
          System.out.println("HandlerInterceptor1....preHandle");
          return true;
      }
  
      // controller执行后但未返回视图前调用此方法，且只有所有preHandle返回true时调用
      // 这里可在返回用户前对模型数据进行加工处理，比如这里加入公用信息以便页面显示
      @Override
      public void postHandle(HttpServletRequest arg0, HttpServletResponse arg1, Object arg2, ModelAndView arg3) throws Exception {
          System.out.println("HandlerInterceptor1....postHandle");
      }
  
      // controller执行后且视图返回后调用此方法，且只有preHandle返回true才调用
      // 这里可得到执行controller时的异常信息
      // 这里可记录操作日志
      @Override
      public void afterCompletion(HttpServletRequest arg0, HttpServletResponse arg1, Object arg2, Exception arg3) throws Exception {
          System.out.println("HandlerInterceptor1....afterCompletion");
      }
  }
  
  ```

- **拦截器配置**

  ```xml
  //spring-config.xml
  <!-- 配置拦截器 -->
  <mvc:interceptors>
      <mvc:interceptor>
          <!-- /user/* 请求进入拦截器 -->
          <mvc:mapping path="/user/*" />
          <!-- 配置具体的拦截器 -->
          <bean class="cn.itcast.ssm.interceptor.HandlerInterceptor1" />
      </mvc:interceptor>
      <mvc:interceptor>
          <!-- 所有的请求都进入拦截器 -->
          <mvc:mapping path="/**" />
          <!-- 配置具体的拦截器 -->
          <bean class="cn.itcast.ssm.interceptor.HandlerInterceptor2" /> <!--ref也可以，但必须有@Component注解 -->
      </mvc:interceptor>
  </mvc:interceptors>
  
  //需要释放静态资源
  
  ```

- 总结：

  - preHandle按拦截器定义顺序调用，**返回false时后续拦截器将不调用**
  - postHandler按拦截器定义逆序调用，且只有**所有preHandle返回true时调用**
  - afterCompletion按拦截器定义逆序调用，且**只有preHandle返回true才调用**，与上一条不同



- 应用

  - 有一个登录页面，需要写一个Controller访问登录页面

  - 登录页面有一提交表单的动作。需要在Controller中处理

    - 判断用户名密码是否正确（在控制台打印）
    - 如果正确,向session中写入用户信息（写入用户名username）
    - 跳转到商品列表

  - 拦截器

    - 拦截用户请求，判断用户是否登录（登录请求不能拦截）
    - 如果用户已经登录。放行
    - 如果用户未登录，跳转到登录页面。

    ```java
    public class LoginInterceptor implements HandlerInterceptor{ 
    	@Override  
        Public boolean preHandle(HttpServletRequest request,HttpServletResponse response, Object handler) throws Exception { 
            //如果是登录页面则放行   
            if(request.getRequestURI().indexOf("login.action")>=0)
                return true;   
            HttpSession session = request.getSession(); 
      
            //如果用户已登录也放行   
            if(session.getAttribute("user")!=null)
                return true;   
            
            //用户没有登录挑战到登录页面   
            request.getRequestDispatcher("/WEB-INF/jsp/login.jsp").forward(request, response);   
            return false;  
        } 
    } 
    
    
    ```





## 8 Servlet 3.0及纯注解配置（旧）

详细的查看文档，搜索JCP并搜Servlet下载即可

------

### 8.1 ServletContainerInitializer

文档的8.2.4章节—Shared libraries（共享库） / runtimes pluggability（运行时可插拔性）

1. Servlet容器启动会扫描，当前应用里面每一个jar包的ServletContainerInitializer的实现
2. 提供ServletContainerInitializer的实现类，必须绑定在META-INF/services/javax.servlet.ServletContainerInitializer，文件的内容就是ServletContainerInitializer实现类的全类名

总结：容器在启动应用的时候，会扫描当前应用每一个jar包里面META-INF/services/javax.servlet.==**ServletContainerInitializer**==
指定的实现类，启动并运行这个实现类的方法；`@HandlesTypes`注解用于传入感兴趣的类型。



### 8.2 ServletContext注册三大组件

```java
//容器启动的时候会将@HandlesTypes指定的这个类型下面的子类（实现类，子接口等）传递过来；
//传入感兴趣的类型；
@HandlesTypes(value={HelloService.class})
public class MyServletContainerInitializer implements ServletContainerInitializer {

	/**
	 * 应用启动的时候，会运行onStartup方法；
	 * 
	 * Set<Class<?>> arg0：感兴趣的类型的所有子类型；
	 * ServletContext arg1:代表当前Web应用的ServletContext；一个Web应用一个ServletContext；
	 * 
	 * 1）、使用ServletContext注册Web组件（Servlet、Filter、Listener）
	 * 2）、使用编码的方式，在项目启动的时候给ServletContext里面添加组件；
	 * 		必须在项目启动的时候来添加；
	 * 		1）、ServletContainerInitializer得到的ServletContext；
	 * 		2）、ServletContextListener得到的ServletContext；
	 */
	@Override
	public void onStartup(Set<Class<?>> arg0, ServletContext sc) throws ServletException {
		System.out.println("感兴趣的类型：");
		for (Class<?> claz : arg0) {
			System.out.println(claz);
		}
		
		//注册组件  ServletRegistration  
		ServletRegistration.Dynamic servlet = sc.addServlet("userServlet", new UserServlet());
		//配置servlet的映射信息
		servlet.addMapping("/user");
		
		
		//注册Listener
		sc.addListener(UserListener.class);
		
		//注册Filter  FilterRegistration
		FilterRegistration.Dynamic filter = sc.addFilter("userFilter", UserFilter.class);
		//配置Filter的映射信息
		filter.addMappingForUrlPatterns(EnumSet.of(DispatcherType.REQUEST), true, "/*");		
	}
}


```



### 8.3 与 SpringMVC 整合

> 查看Spring官方文档即可

![mvc-context-hierarchy](./images/mvc-context-hierarchy.png)

**原理**

1、web容器在启动的时候，会扫描每个jar包下的META-INF/services/javax.servlet.ServletContainerInitializer

2、加载这个文件指定的类SpringServletContainerInitializer

3、spring的应用一启动会加载感兴趣的WebApplicationInitializer接口的下的所有组件；

4、并且为WebApplicationInitializer组件创建对象（组件不是接口，不是抽象类）

​	1）、AbstractContextLoaderInitializer：创建根容器；createRootApplicationContext()；

​	2）、AbstractDispatcherServletInitializer：
​			创建一个web的ioc容器；createServletApplicationContext();
​			创建了DispatcherServlet；createDispatcherServlet()；
​			将创建的DispatcherServlet添加到ServletContext中；
​				getServletMappings();

​	3）AbstractAnnotationConfigDispatcherServletInitializer：注解方式配置的DispatcherServlet初始化器
​			创建根容器：createRootApplicationContext()
​					getRootConfigClasses();传入一个配置类
​			创建web的ioc容器： createServletApplicationContext();
​					获取配置类；getServletConfigClasses();
​	
总结：以注解方式来启动SpringMVC；**继承`AbstractAnnotationConfigDispatcherServletInitializer`**；**实现抽象方法**指定DispatcherServlet等的配置信息（和在web.xml中功能一致）；

------

定制SpringMVC；

1. `@EnableWebMvc`：开启SpringMVC定制配置功能；与`<mvc:annotation-driven/>`功能一致
2. 配置组件（视图解析器、视图映射、静态资源映射、拦截器。。。） 
   1. 实现`WebMvcConfigurer`重写全部方法！！！可以配置任何东西。不如下面的实现类方便。
   2. 继承`WebMvcConfigurerAdapter`（Spring5.0后过时）
   3. **实现`WebMvcConfigurer `接口**

```java
//web容器启动的时候创建对象；调用方法来初始化容器以前前端控制器
public class WebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

	//获取根容器的配置类；（Spring的配置文件）   父容器；
	@Override
	protected Class<?>[] getRootConfigClasses() {
		return new Class<?>[]{RootConfig.class};
	}

	//获取web容器的配置类（SpringMVC配置文件）  子容器；
	@Override
	protected Class<?>[] getServletConfigClasses() {
		return new Class<?>[]{AppConfig.class};
	}

	//获取DispatcherServlet的映射信息
	//  /：拦截所有请求（包括静态资源（xx.js,xx.png）），但是不包括*.jsp；
	//  /*：拦截所有请求；连*.jsp页面都拦截；jsp页面是tomcat的jsp引擎解析的；
	@Override
	protected String[] getServletMappings() {
		return new String[]{"/"};
	}
    
    //该方法下的所有过滤器都会映射到DispatcherServlet
    @Override
    protected Filter[] getServletFilters() {
        //编码过滤器
        CharacterEncodingFilter characterEncodingFilter = new CharacterEncodingFilter("UTF-8",true);
        //解决PUT、DELETE等请求的过滤器
        HiddenHttpMethodFilter hiddenHttpMethodFilter = new HiddenHttpMethodFilter();

        return new Filter[]{characterEncodingFilter,hiddenHttpMethodFilter};
    }

    @Override
    protected WebApplicationContext createServletApplicationContext() {
        return super.createServletApplicationContext();
    }
}


```

```java
@Configuration
@ComponentScan(value="cn.itcast",excludeFilters={
    @Filter(type=FilterType.ANNOTATION,classes={Controller.class})
})//扫描cn.itcast下的除过Controller的所有组件，不能添加useDefaultFilters
@EnableTransactionManagement//开启事务
@PropertySource("classpath:JdbcConfig.properties")//导入外部配置文件
@Configuration
public class RootConfig {

    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;


    @Bean
    public DataSource dataSource(){
        //DriverManagerDataSource dataSource = new DriverManagerDataSource();//使用spring提供的DataSource
        DruidDataSource dataSource = new DruidDataSource();//配置Druid的DataSource
        dataSource.setDriverClassName(driver);
        dataSource.setUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        return dataSource;
    }

    @Bean
    public JdbcTemplate jdbcTemplate(DataSource dataSource){
        return new JdbcTemplate(dataSource);
    }
    
    //配置事务管理器
    @Bean
    public PlatformTransactionManager transactionManager(DataSource dataSource){
        return new DataSourceTransactionManager(dataSource);
    }


   /**配置mybatis，执行后总是报NullPointerException，毫无头绪。上面的JdbcTemplate成功了

    //配置SqlSessionFactory，利用org.mybatis.spring提供的SqlSessionFactoryBean
    @Bean
    public SqlSessionFactory sqlSessionFactory(DataSource dataSource){
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        
        //数据源
        sqlSessionFactoryBean.setDataSource(dataSource);
        
        //pageHelper的拦截器插件配置
        PageInterceptor pageInterceptor = new PageInterceptor();
        Properties properties = new Properties();
        properties.setProperty("helperDialect","mysql");
        properties.setProperty("reasonable","true");
        pageInterceptor.setProperties(properties);
        sqlSessionFactoryBean.setPlugins(new Interceptor[]{pageInterceptor});

        return sqlSessionFactoryBean.getObject();  //mybatis-spring整合的文档是这么写的
    }

    //配置Mapper接口扫描
    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer(){
        MapperScannerConfigurer mapperScannerConfigurer = new MapperScannerConfigurer();
        mapperScannerConfigurer.setBasePackage("cn.itcast.dao");
        return mapperScannerConfigurer;
    } */
}


```

```java
@Configuration
@ComponentScan(value = "cn.itcast.controller", includeFilters = {
        @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = Controller.class)
}, useDefaultFilters = false)//必须添加useDefaultFilters
@EnableWebMvc
public class AppConfig implements WebMvcConfigurer  {

    @Autowired
    private ApplicationContext servletApplicationContext;

    @Override
    public void configureViewResolvers(ViewResolverRegistry registry) {
        //模板解析器
        SpringResourceTemplateResolver templateResolver = new SpringResourceTemplateResolver();
        templateResolver.setApplicationContext(this.servletApplicationContext);//必须有
        templateResolver.setPrefix("/templates/");
        templateResolver.setSuffix(".html");
        templateResolver.setTemplateMode("HTML");
        templateResolver.setCharacterEncoding("UTF-8");

        //模板引擎
        SpringTemplateEngine templateEngine = new SpringTemplateEngine();
        templateEngine.setTemplateResolver(templateResolver);

        //Thymeleaf视图解析器
        ThymeleafViewResolver viewResolver = new ThymeleafViewResolver();
        viewResolver.setTemplateEngine(templateEngine);
        viewResolver.setCharacterEncoding("UTF-8");

        registry.viewResolver(viewResolver);

        //registry.jsp("/views/",".jsp");
    }

    //释放静态资源
    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }

    //拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new MyFirstInterceptor()).addPathPatterns("/**");
    }
}

```

> 以上配置，在dao层查询时会报空指针异常，暂时GG。同样的xml配置OK。我不OK了！！！

纯注解配置时由于Maven的原因，需要配置如下plugin

```xml
<plugin>
    <artifactId>maven-war-plugin</artifactId>
    <version>2.6</version>
    <configuration>
        <warSourceDirectory>WebContent</warSourceDirectory>
        <failOnMissingWebXml>false</failOnMissingWebXml>
    </configuration>
</plugin>

```









## 9 Thymeleaf

JSP、Velocity、Freemarker、Thymeleaf（Spring推荐，语法更简单，功能更强大）

![](../../Studying/SSM_/images/template-engine.png)

### 9.1 引入

[官方文档查看更详细内容](https://www.thymeleaf.org/)

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>


```

自动配置类如下：

```java
@ConfigurationProperties(prefix = "spring.thymeleaf")
public class ThymeleafProperties {

    private static final Charset DEFAULT_ENCODING = Charset.forName("UTF-8");

    private static final MimeType DEFAULT_CONTENT_TYPE = MimeType.valueOf("text/html");

    public static final String DEFAULT_PREFIX = "classpath:/templates/";

    public static final String DEFAULT_SUFFIX = ".html";


```

- 只要我们把HTML页面**放在classpath:/templates/**，thymeleaf就能自动渲染；

- 导入thymeleaf的名称空间，才能有语法提示

  ```html
  <html lang="en" xmlns:th="http://www.thymeleaf.org">
  
  ```

- 使用thymeleaf语法

  ```html
  <!DOCTYPE html>
  <html lang="en" xmlns:th="http://www.thymeleaf.org">
      <head>
          <meta charset="UTF-8">
          <title>Title</title>
      </head>
      <body>
          <h1>成功！</h1>
          <!--th:text 将div里面的文本内容设置为 -->
          <div th:text="${hello}">这是显示欢迎信息</div>
      </body>
  </html>
  
  
  ```



### 9.2 Thymeleaf 语法

- `th:text`：改变当前元素里面的文本内容，转移特殊字符。`th:utext`不转义。**th:任意html属性**：来替换原生属性的值。

- `th:each`：遍历

  ```html
  <tr th:each="p : ${pageInfo.list}">
      <td th:text="${p.id}"></td>
      <td th:text="${p.name}"></td>
  
  
  ```



![](./images/2018-02-04_123955.png)

上图有优先级顺序



- Simple expressions:（表达式语法）

  - **Variable Expressions: `${...}`**：获取变量值；底层是OGNL；

    - 获取对象的属性、调用方法。
    - 使用内置的基本对象：
      - `${#ctx}` : the context object.
      - `${#vars}`: the context variables.
      - `${#locale}` : the context locale. 如`${#locale.country}`
      - `${#request}` : (only in Web Contexts) the HttpServletRequest object.
      - `${#response}` : (only in Web Contexts) the HttpServletResponse object.
      - `${#session}`#session : (only in Web Contexts) the HttpSession object.
      - `${#servletContext}` : (only in Web Contexts) the ServletContext object.
    - 使用内置的**工具**对象（同上，放在`${...}`里）：
      - `#execInfo` : information about the template being processed.
      - `#messages` : methods for obtaining externalized messages inside variables expressions, in the same way as they would be obtained using #{…} syntax.
      - `#uris` : methods for escaping parts of URLs/URIs
      - `#conversions` : methods for executing the configured conversion service (if any).
      - `#dates` : methods for java.util.Date objects: formatting, component extraction, etc.
      - `#calendars` : analogous to #dates , but for java.util.Calendar objects.
      - `#numbers` : methods for formatting numeric objects.
      - `#strings` : methods for String objects: contains, startsWith, prepending/appending, etc.
      - `#objects` : methods for objects in general.
      - `#bools` : methods for boolean evaluation.
      - `#arrays` : methods for arrays.
      - `#lists` : methods for lists.
      - `#sets` : methods for sets.
      - `#maps` : methods for maps.
      - `#aggregates` : methods for creating aggregates on arrays or collections.
      - `#ids` : methods for dealing with id attributes that might be repeated (for example, as a result of an iteration).

  - Selection Variable Expressions: `*{...}`：选择表达式：和`${...}`在功能上是一样；配合`th:object`使用如下：

    ```html
    <div th:object="${session.user}">
        <p>Name: <span th:text="*{firstName}">Sebastian</span>.</p>	<!--${session.user.firstName}-->
        <p>Surname: <span th:text="*{lastName}">Pepper</span>.</p>	<!--${session.user.lastName}-->
        <p>Nationality: <span th:text="*{nationality}">Saturn</span>.</p>	<!--${session.user.nationality}-->
    </div>
    
    ```

  - Message Expressions: `#{...}`：获取国际化内容

  - **Link URL Expressions**: `@{...}`：定义URL；在需要参数时，放在`()`里。

    ```html
    <li><a href="" th:href="@{/user}" aria-label="Previous">首页</a></li>
    <li><a href="" th:href="@{/user(pageNum=${pageInfo.pageNum}-1)}">上一页</a></li>
    <li th:each="i:${#numbers.sequence(1,pageInfo.pages)}"><a href="" th:href="@{/user(pageNum=${i})}" th:text="${i}"></a></li> 暂时不会使用thymeleaf写页码
    <li><a href="" th:href="@{/user(pageNum=${pageInfo.pageNum}+1)}">下一页</a></li>
    <li><a href="" th:href="@{/user(pageNum=${pageInfo.pages})}" aria-label="Next">尾页</a></li>
    若使用Pagehelper，无需考虑越界
    
    <form th:action="@{/user/}+${user.id}" method="POST">
        <input type="hidden" name="_method" value="PUT">
        <input type="hidden" name="id" th:value="${user.id}">
        <input type="radio" th:name="sex" value="男" th:checked="${user.sex}=='男'?true:false">
    </form>
    
    <script th:src="@{/plugins/bootstrap/js/bootstrap.min.js}"></script>
    
    ```

  - Fragment Expressions: `~{...}`：片段引用表达式

  - Literals（字面量）

    - Text literals: 'one text' , 'Another one!' ,…
    - Number literals: 0 , 34 , 3.0 , 12.3 ,…
    - Boolean literals: true , false
    - Null literal: null
    - Literal tokens: one , sometext , main ,…

  - Text operations:（文本操作）

    - String concatenation: +
    - Literal substitutions: |The name is ${name}|

  - Arithmetic operations:（数学运算）

    - Binary operators: + , - , * , / , %
    - Minus sign (unary operator): -

  - Boolean operations:（布尔运算）

    - Binary operators: and , or
    - Boolean negation (unary operator): ! , not

  - Comparisons and equality:（比较运算）

    - Comparators: > , < , >= , <= ( gt , lt , ge , le )
    - Equality operators: == , != ( eq , ne )

  - Conditional operators:条件运算（三元运算符）

    - If-then: (if) ? (then)
    - If-then-else: (if) ? (then) : (else)
    - Default: (value) ?: (defaultvalue)

  - Special tokens:

    - No-Operation: _ 







### 9.3 公共页面的抽取

```html
1、抽取公共片段
<footer th:fragment="copy" id="footer1">
&copy; 2011 The Good Thymes Virtual Grocery
</footer>

2、引入方式（每个templates下的html都是模板，其去掉后缀就是模板名）
~{templatename::selector}：模板名::选择器（CSS选择器）
~{templatename::fragmentname}:模板名::片段名
使用以下属性进行引入，可以不用写~{}
行内写法可以加上：[[~{}]];[(~{})]；一个是转义，一个不转义
<div th:insert="footer :: copy"></div> 	<!--将公共片段整个插入到声明引入的元素中-->
<div th:replace="footer :: copy"></div>	<!--将声明引入的元素替换为公共片段-->
<div th:include="footer :: copy"></div>	<!--将被引入的片段的内容（只有内容）包含进这个标签中-->

<div th:include="footer :: #footer1"></div> <!--显示效果同include-->

效果
<div>
    <footer>
    &copy; 2011 The Good Thymes Virtual Grocery
    </footer>
</div>

<footer>
&copy; 2011 The Good Thymes Virtual Grocery
</footer>

<div>
&copy; 2011 The Good Thymes Virtual Grocery
</div>

```

引入片段的时候传入参数（动态显示高亮等用处）：

```html
<nav class="col-md-2 d-none d-md-block bg-light sidebar" id="sidebar">
    <div class="sidebar-sticky">
        <ul class="nav flex-column">
            <li class="nav-item">
                <a class="nav-link active"
                   th:class="${activeUri=='main.html'?'nav-link active':'nav-link'}"
                   href="#" th:href="@{/main.html}">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-home">
                        <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path>
                        <polyline points="9 22 9 12 15 12 15 22"></polyline>
                    </svg>
                    Dashboard <span class="sr-only">(current)</span>
                </a>
            </li>

<!--引入侧边栏;传入参数-->
<div th:replace="commons/bar::#sidebar(activeUri='emps')"></div>

```















# 9 Spring Boot 日志

> 也可查看Spring Boot 官方文档第26节，[这里](https://docs.spring.io/spring-boot/docs/2.0.6.RELEASE/reference/htmlsingle/#boot-features-logging)

## 3.1 日志框架

- 小张开发一个大型系统；

​	1、System.out.println("")；将关键数据打印在控制台；去掉？写在一个文件？

​	2、框架来记录系统的一些运行时信息；日志框架 ； zhanglogging.jar；

​	3、高大上的几个功能？异步模式？自动归档？xxxx？ zhanglogging-good.jar；

​	4、将以前框架卸下来？换上新的框架，重新修改之前相关的API；zhanglogging-prefect.jar；

​	5、借鉴JDBC---数据库驱动；

​		写了一个统一的接口层；日志门面（日志的一个抽象层）；logging-abstract.jar；

​		给项目中导入具体的日志实现就行了；我们之前的日志框架都是实现的抽象层；

- 市面上的日志框架
  JUL、JCL、Jboss-logging、logback、log4j、log4j2、slf4j....

  | 日志门面 （日志的抽象层）                                    | 日志实现                                                |
  | ------------------------------------------------------------ | ------------------------------------------------------- |
  | ~~JCL（Jakarta Commons Logging）~~  SLF4j（Simple Logging Facade for Java）~~**jboss-logging**~~ | Log4j     JUL（java.util.logging） Log4j2   **Logback** |

- 左边选一个门面（抽象层）：SLF4J。右边来选一个实现：Logback；

- SpringBoot：底层是Spring框架，Spring框架默认是用JCL；

  - ==**SpringBoot选用 SLF4j和logback；**==



## 3.2 SLF4j使用

### 3.2.1 如何在系统中使用SLF4j

- 以后开发的时候，日志记录方法的调用，不应该来直接调用日志的实现类，而是**调用日志抽象层里面的方法**；

  - 给系统里面导入slf4j的jar和 logback的实现jar

  ```java
  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;
  
  public class HelloWorld {
    public static void main(String[] args) {
      Logger logger = LoggerFactory.getLogger(HelloWorld.class);
      logger.info("Hello World");
    }
  }
  
  
  ```

![](./images/concrete-bindings.png)

- 每一个日志的实现框架都有自己的配置文件。使用slf4j**配置文件还是做成日志实现框架自己本身的配置文件；**



### 3.2.2 遗留问题

- a系统（slf4j+logback）: Spring（commons-logging）、Hibernate（jboss-logging）、MyBatis、xxxx
- **统一日志记录**，即**如何让系统中所有的日志都统一到slf4j？**
  - ==**将系统中其他日志框架先排除出去；**==
  - ==**用中间包来替换原有的日志框架；**==
  - ==**我们导入slf4j其他的实现**==

![](./images/legacy.png)



## 3.3 SpringBoot日志关系

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
</dependency>


```

SpringBoot使用它来做日志功能：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-logging</artifactId>
</dependency>


```

**底层依赖关系如下**

- Spring Boot 1.5.10如下：

![](./images/%E5%BA%95%E5%B1%82%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB.png)

- SpringBoot 2.0后底层依赖如下：

![](./images/%E5%BA%95%E5%B1%82%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB2.png)

- **总结 Spring Boot 1.5.10**

  - SpringBoot底层也是使用slf4j+logback的方式进行日志记录

  - SpringBoot也把其他的日志都替换成了slf4j

  - 中间替换包细节如下截取部分：

    ```java
    @SuppressWarnings("rawtypes")
    public abstract class LogFactory {
    
        static String UNSUPPORTED_OPERATION_IN_JCL_OVER_SLF4J = "http://www.slf4j.org/codes.html#unsupported_operation_in_jcl_over_slf4j";
    
        static LogFactory logFactory = new SLF4JLogFactory();
    
    
    ```

    Spring Boot 1.5.10中的中间转换包如下，2.0后改了名字，实现的名称也不同，不放图了

    ![](./images/%E4%B8%AD%E9%97%B4%E6%9B%BF%E6%8D%A2%E5%8C%85.png)

  - **若要引入其他框架**？一定要把这个框架的**默认日志依赖移除**掉。例如Spring框架用的是commons-logging。但是Spring Boot2.0后的没有这个，因为底层不再依赖它

    ```xml
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <exclusions>
            <exclusion>
                <groupId>commons-logging</groupId>
                <artifactId>commons-logging</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    
    
    ```

==**SpringBoot能自动适配所有的日志，而且底层使用slf4j+logback的方式记录日志，引入其他框架的时候，只需要把这个框架依赖的日志框架排除掉即可；**==

- Spring Boot 2.0开始使用 Commons Logging 进行所有内部日志记录，但保留底层日志实现。 为Java Util Logging，Log4J2，和 Logback提供了默认配置。默认情况下，如果使用“Starters”，则使用Logback进行日志记录

## 3.4 日志使用

### 3.4.1 默认配置

- SpringBoot默认帮我们配置好了日志

  ```java
  //记录器
  Logger logger = LoggerFactory.getLogger(getClass());
  
  @Test
  public void contextLoads() {
  
      //日志的级别；由低到高 
        trace<debug<info<warn<error
      //可以调整输出的日志级别；日志就只会在这个级别以以后的高级别生效
      logger.trace("系统详细信息, 主要开发人员用, 一般来说线上系统可以认为是临时输出, 而且随时可以通过开关将其关闭");
      logger.debug("主要给开发人员看,开发环境中使用");
      //SpringBoot默认给我们使用的是info级别的，没有指定级别的就用SpringBoot默认规定的级别；root级别
      logger.info("重要的业务逻辑处理完成. 在理想情况下, INFO的日志信息要能让高级用户和系统管理员理解, 并从日志信息中能知道系统当前的运行状态");
      logger.warn("系统能继续运行, 但是必须引起关注");
      logger.error("系统发生了严重的错误, 必须马上进行处理, 否则系统将无法继续运行");
  
  }
  
  
  ```

  ```java
  日志输出格式：
  		%d表示日期时间，
  		%thread表示线程名，
  		%-5level：级别从左显示5个字符宽度
  		%logger{50} 表示logger名字最长50个字符，否则按照句点分割。 
  		%msg：日志消息，
  		%n是换行符
  
  %d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n
  
  
  ```

- SpringBoot修改日志的默认配置

  ```properties
  #指定某个包的日志记录级别
  logging.level.com.itcast=trace
  
  # 不指定路径在当前项目下生成springboot.log日志
  # 可以指定完整的路径；
  logging.file=G:/springboot.log
  
  # 在当前磁盘的根路径下创建spring文件夹和里面的log文件夹；使用 spring.log 作为默认文件
  logging.path=/spring/log
  
  # 在控制台输出的日志的格式
  logging.pattern.console=%d{yyyy-MM-dd} [%thread] %-5level %logger{50} - %msg%n
  # 指定文件中日志输出的格式
  logging.pattern.file=%d{yyyy-MM-dd} === [%thread] === %-5level === %logger{50} ==== %msg%n
  
  
  ```

  | logging.file               | logging.path | Example  | Description                        |
  | -------------------------- | ------------ | -------- | ---------------------------------- |
  | (none)                     | (none)       |          | 只在控制台输出                     |
  | 指定文件名，可指定完整路径 | (none)       | my.log   | 输出日志到my.log文件               |
  | (none)                     | 指定目录     | /var/log | 输出到指定目录的 spring.log 文件中 |

### 3.4.2 指定配置

- 给**类路径**下放上**每个日志框架自己的配置文件**即可；SpringBoot就不使用他默认配置的了

  | Logging System         | Customization                                                |
  | ---------------------- | ------------------------------------------------------------ |
  | Logback                | `logback-spring.xml`, `logback-spring.groovy`, `logback.xml` or `logback.groovy` |
  | Log4j2                 | `log4j2-spring.xml` or `log4j2.xml`                          |
  | JDK(Java Util Logging) | `logging.properties`                                         |

  - logback.xml：直接就被日志框架识别了；

    **logback-spring.xml**：**推荐！**日志框架就不直接加载日志的配置项，**由SpringBoot解析日志配置**，可以使用SpringBoot的高级Profile功能

    ```xml
    <springProfile name="staging">
        <!-- configuration to be enabled when the "staging" profile is active -->
      	可以指定某段配置只在某个环境下生效
    </springProfile>
    
    
    ```

    如下：

    ```xml
    <appender name="stdout" class="ch.qos.logback.core.ConsoleAppender">
        <layout class="ch.qos.logback.classic.PatternLayout">
            <springProfile name="dev">
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} ----> [%thread] ---> %-5level %logger{50} - %msg%n</pattern>
            </springProfile>
            <springProfile name="!dev">
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} ==== [%thread] ==== %-5level %logger{50} - %msg%n</pattern>
            </springProfile>
        </layout>
    </appender>
    
    
    ```

    如果使用logback.xml作为日志配置文件，还要使用profile功能，会有以下错误`no applicable action for [springProfile]`

## 3.5 切换日志框架

- 可以按照slf4j的日志适配图，进行相关的切换

  - slf4j+log4j的方式（没意义）

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <artifactId>logback-classic</artifactId>
                <groupId>ch.qos.logback</groupId>
            </exclusion>
            <exclusion>
                <artifactId>log4j-over-slf4j</artifactId>
                <groupId>org.slf4j</groupId>
            </exclusion>
        </exclusions>
    </dependency>
    
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
    </dependency>
    
    
    ```

  - 切换为log4j2

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <artifactId>spring-boot-starter-logging</artifactId>
                <groupId>org.springframework.boot</groupId>
            </exclusion>
        </exclusions>
    </dependency>
    
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-log4j2</artifactId>
    </dependency>
    
    
    ```



























































## 2 请求数据

- **@RestController**：Spring4的新注解，将类标记为控制器，其中每个方法都返回一个域对象而不是视图，这是@Controller和@ResponseBody的简写

- @**RequestMapping**、@GetMapping、@PostMapping、@PutMapping、@DeleteMapping。value中也可以用正则表达式限制类型。若不符合要求会返回4xx的错误信息，表示请求有问题

  - 当POST方法传递的是表单数据时，可在上述注解加**consumes参数**（根据请求的**Content-Type**缩小请求映射范围）

    - 设置consumes ={MediaType.MULTIPART_FORM_DATA_VALUE ,MediaType.APPLICATION_ATOM_XML_VALUE})，任选，<a href="#postman">详细区别看这里</a>。第一个可以传文件，第二个传文件慢

    - 在@PostMapping**上传文件**方法中使用**MultipartFile**接口的流保存文件

      ```java
      public void uploadFile(@RequestParam MultipartFile file) {...}
      
      
      ```

  - GET方法**下载文件**，设置**produces参数**根据**Accept**请求标头和控制器方法生成的内容类型列表来缩小请求映射

- @**PathVariable**：访问URL模板变量

- @**RequestBody**：通过HttpMessageConverter读取请求体并反序列化为Object

- @**RequestParam**：将Servlet请求参数（即查询参数或表单数据）绑定到控制器中方法的参数上，value可不写（必须和参数一致），默认required=true，可以设置为false防止异常

- @RequestHeader：将请求头绑定到控制器方法的参数上，value可不写（必须和参数一致），不区分大小写

- @CookieValue：将HTTP cookie的值绑定到控制器方法的参数上，同上。除非为了兼容老客户端，否则不用

- Authenticated：获取当前用户，直接在方法中增加参数，类型为它即可

```java
//此代码使用Spring 4的新注解@RestController，该注注解将类标记为控制器，
//其中每个方法都返回一个域对象而不是视图。 这是@Controller和@ResponseBody的简写。
@RestController
@RequestMapping(value = "/tvSeries")
public class TvSeriesController {

    /**
     * GET查询所有电视剧
     */
    @GetMapping
    public List<TvSeries> queryAll() {
        List<TvSeries> tvSeriesList = new ArrayList<>();
        tvSeriesList.add(new TvSeries(0, "天龙八部", 50, new Date()));
        tvSeriesList.add(new TvSeries(1, "笑傲江湖", 40, new Date()));
        return tvSeriesList;
    }

    /**
     * GET按id查询电视剧
     */
    @GetMapping(value = "/{id}")
    public TvSeries queryOne(@PathVariable Integer id) {

        if (id == 1) {
            return new TvSeries(0, "天龙八部", 50, new Date());
        } else if (id == 2) {
            return new TvSeries(1, "笑傲江湖", 40, new Date());
        } else
            return null;
    }

    /**
     * POST增加电视剧（没写持久层代码）
     */
    @PostMapping
    public TvSeries addOne(@RequestBody TvSeries tvSeries) {
        System.out.println(tvSeries);
        return tvSeries;
    }

    /**
     * PUT更改电视剧
     */
    @PutMapping(value = "/{id}")
  	public TvSeries updateOne(@PathVariable Integer id, @RequestBody TvSeries tvSeries) {
        if (id == 1) {
            TvSeries tvSeries1 = new TvSeries(0, "天龙八部", 50, new Date());
            tvSeries1.setName(tvSeries.getName());
            return tvSeries1;
        } else {
            return null;
        }
    }
    
    /**
     * DELETE删除电视节目
     */
    @DeleteMapping(value = "/{id}")
    public Map<String, String> deleteOne(@PathVariable Integer id, HttpServletRequest request,                                      @RequestParam(value = "deleteReason", required = false) String deleteReason) {
        Map<String, String> result = new HashMap<>();
        if (id == 0) {
            //执行删除代码
            result.put("message", "#0被" + request.getRemoteAddr() + "删除，原因：" + deleteReason);
        } else if (id == 1) {
            //不能删除这个，抛异常。spring security处理更好，之后学习
            throw new RuntimeException("#1不能删除");
        } else
            throw new ResourceNotFoundException();
        return result;
    }
    
    /**
     * MultipartFile上传文件,利用consumes您根据请求的Content-Type缩小请求映射范围
     */
    @PostMapping(value = "/{id}/file" ,consumes = MediaType.MULTIPART_FORM_DATA_VALUE)
    public void uploadFile(@PathVariable Integer id,@RequestParam MultipartFile file) throws IOException {
        file.transferTo(new File("F:\\GitHub\\Studying\\SSM\\"+file.getOriginalFilename()));
        System.out.println(id+"id上传了文件");
    }
    
    /**
     * 下载文件，根据Accept请求标头和控制器方法生成的内容类型列表来缩小请求映射，如以下示例所示：
     */
    @GetMapping(value = "/{id}/file" ,produces = MediaType.IMAGE_JPEG_VALUE)
    public byte[] downloadFile(@PathVariable Integer id) throws IOException {
        System.out.println(id+"id下载了文件");
        return IOUtils.toByteArray(new FileInputStream("target/classes/WindowsPic.jpg"));

    }
}

```



### 2.1 数据校验(Bean Validation)

- 不要相信前端传入的数据，尽量要前端少传数据

- **使用基于注解的验证**

  > JSR303 是一套JavaBean参数校验的标准，它定义了很多常用的校验注解，我们可以直接将这些注解加在我们JavaBean的属性上面，就可以在需要校验的时候进行校验了。
  >
  > Hibernate validator是Bean Validation1.0（JSR303）的一个实现
  >
  > 目前最新的Hibernate validator 6.0 是Bean Validation 2.0（JSR380，于2017年8月完成）的一个实现
  >
  > [Bean Validation官网点这里](https://beanvalidation.org/)

- Hibernate Validator包含一组基本的常用约束，这些最重要的是Bean Validation规范定义的约束。此外，Hibernate Validator还提供了有用的自定义约束。

- @**Valid**      级联验证，**确保这个对象满足校验限制**。校验的对象后**紧跟着Errors或BindingResult参数**

  ```java
  @NotNull
  private List<@Valid Image> images;//images不可为null，每个Image元素需要级联验证
  
  @NotNull
  @Size(min=1,max=10)//messages不为null，长度至少为1，最长10；
  private List<@Size(min=10) @NotNull String> messages;//每个String不为空，长度至少为10
  
  ```

  ```java
  @PutMapping
  public TvSeries updateOne(@Valid @RequestBody TvSeries tvSeries,BindingResult result) {
      //参数没过校验也会进入方法执行，校验结果通过result参数传递进来
      if(result.hasErrors()){
          //没通过校验
      }
  }
  
  ```

  <!DOCTYPE html>
  <html>
  <head>
      <style>
          tr{
              color: rgb(76, 78, 47)
          }
          tr>td:first-child{
              font-weight: bold;
              color: black;
              font-family: consolas;
      </style>
  </head>
      <body>
          <table border="3">
      <caption>注解说明表</caption>
      <tr style="background-color:#90CAF9">
         <td style="color: rgb(76, 78, 47);font-weight: normal;">注解</td>
         <td>说明</td>
         <td>支持的数据类型</td>
      </tr>
      <tr style="background-color:#69F0AE">
         <td>@Null</td>
         <td>被注释的元素必须为 null</td>
         <td rowspan="2">所有类型</td>
      </tr>
      <tr style="background-color:#69F0AE">
         <td>@NotNull</td>
         <td>被注释的元素必须不为 null</td>
      </tr>
      <tr style="background-color:#90CAF9">
         <td>@NotEmpty</td>
         <td>被注释的元素非null且非空（字符串是不是空格无所谓）</td>
         <td>CharSequence, Collection, Map and arrays</td>
      </tr>
      <tr style="background-color:#90CAF9">
         <td>@NotBlank</td>
         <td>被注释的元素非null，且必须包含至少一个非空格字符</td>
         <td>CharSequence</td>
      </tr>
      <tr style="background-color:#69F0AE">
         <td>@Pattern(regex=, flags=)</td>
         <td>被注释的字符串是否与给定的正则表达式匹配</td>
         <td>CharSequence</td>
      </tr>
      <tr style="background-color:#69F0AE">
         <td>@Email</td>
         <td>检查指定的字符序列是否是有效的电子邮件地址。 可选参数regexp和flags允许指定电子邮件必须匹配的附加正则表达式（包括正则表达式标志）</td>
         <td>CharSequence</td>
      </tr>
      <tr style="background-color:#90CAF9">
         <td>@AssertTrue</td>
         <td>被注释的元素必须为 true</td>
         <td rowspan="2">Boolean、boolean</td>
      </tr>
      <tr style="background-color:#90CAF9">
         <td>@AssertFalse</td>
         <td>被注释的元素必须为 false</td>
      </tr>
      <tr style="background-color:#69F0AE">
         <td>@DecimalMin(value=, inclusive=)</td>
         <td>当inclusive = false时，检查带注释的值是否大于指定的最小值；否则该值是否大于或等于指定的最小值。参数值是根据BigDecimal字符串表示形式的最大值的字符串表示形式</td>
         <td rowspan="3">BigDecimal，BigInteger，CharSequence，byte，short，int，long和原始类型相应包装类型</td>
      </tr>
      <tr style="background-color:#69F0AE">
         <td>@DecimalMax(value=, inclusive=)</td>
         <td>与上相反</td>
      </tr>
      <tr style="background-color:#69F0AE">
         <td>@Digits(integer=, fraction=)</td>
         <td>整数位数，小数位数必须在设置的范围内</td>
      </tr>
      <tr style="background-color:#90CAF9">
         <td>@Size(min=, max=)</td>
         <td>被注释元素的大小是否在最小值和最大值（包括）之间</td>
         <td>CharSequence, Collection, Map and arrays</td>
      </tr>
      <tr style="background-color:#90CAF9">
         <td>@Min(value=)</td>
         <td>被注释值是否大于或等于指定的最小值</td>
         <td rowspan="6">BigDecimal，BigInteger，byte，short，int，long和原始类型相应包装类型; 另外由HV支持：任何子类型的CharSequence（由字符序列表示的数值被评估），任何子类型的Number和javax.money.MonetaryAmount </td>
      </tr>
      <tr style="background-color:#90CAF9">
         <td>@Max(value=)</td>
         <td>与上相反</td>
      </tr>
      <tr style="background-color:#90CAF9">
         <td>@Negative</td>
         <td>检查元素是否为负数，0为无效</td>
      </tr>
      <tr style="background-color:#90CAF9">
         <td>@NegativeOrZero</td>
         <td>检查元素是否为负数或0</td>
      </tr>
      <tr style="background-color:#90CAF9">
         <td>@Positive</td>
         <td>检查元素是否为正数，0为无效</td>
      </tr>
      <tr style="background-color:#90CAF9">
         <td>@PositiveOrZero</td>
         <td>检查元素是否为正数或0</td>
      </tr>
      <tr style="background-color:#69F0AE">
         <td>@Past</td>
         <td>被注释的元素必须是一个过去的日期</td>
         <td rowspan="4">java.util.Date、java.util.Calendar、java.time和java.time.chrono两个包下的一些类</td>
      </tr>
      <tr style="background-color:#69F0AE">
         <td>@PastOrPresent</td>
         <td>过去或现在</td>
      </tr>
      <tr style="background-color:#69F0AE">
         <td>@Future</td>
         <td>被注释的元素必须是一个将来的日期</td>
      </tr>
      <tr style="background-color:#69F0AE">
         <td>@FutureOrPresent</td>
         <td>现在或将来</td>
      </tr>
   </table>
      </body>

  - 以上**每个注解都有groups、message、payload三个参数**可选
  - 除表中特别说明的外，null值都是合法的
  - 每个注解都还有一个名字后面跟.List的注解，例如@Null.List，推荐在标记一组同样注解时使用；还有.Iterable、.Map等
  - 表中提到的**CharSequence的子类**有：String、StringBuffer、StringBuilder、CharBuffer、Segment

- **注解的位置**

  - 成员变量（Filed域）上
  - 方法（get/is）上
  - 类上

- **约束规则对子类依然有效**

### 2.2 对同一bean的不同验证需求

- **groups参数**：每个约束的注解都有这个参数，可以接收多个class类型（必须是接口）

  不声明groups参数默认为javax.validation.groups.Default，声明了groups参数的会从Default组移除，如需加入Default组需显示声明，例如：@Null(groups={Default.class,Step1.class})

- **@Valid和@Validated区别**

  - @Valid是JSR标准定义的注解，只验证Default组的约束
  - @Validated是Spring定义注解，可通过参数来指定验证组，例：@Validated({Step1.class,Default.class})
  - @Valid可以用在成员变量上，进行级联验证；@Validated只能用在参数上，表示这个参数需要验证

- 自定义注解和不同验证更详细的[看这个博客](https://www.cnblogs.com/beiyan/p/5946345.html)

- **手动验证**：Spring调用Controller层的方法时，其中有@Valid或@Validated注解，会自动数据校验。当在Service层也需要数据校验时，需手动验证

  ```java
  //装载验证器
  @Autowired Validator validator;
  //验证某个类，下面是执行默认的验证组，如需指定可多传一个class参数
  Set<ConstraintViolation<?>> result = validator.validate(obj);
  //通不过校验的result集合会有值，可以通过size()判断
  
  ```



