---
typora-root-url: ..\..\imgs
---

# 一、Spring Boot入门

## 1、Spring boot简介

简化Spring应用开发的框架

对整个Spring技术栈的整合 j2ee的一站式解决方案

## 2、微服务

2014 martin fowler

微服务 :是一种架构风格

一个应用程序应该是一组小型服务 ，通过Http的方式进行互通；

每一个功能元素，最终都是一个可独立替换和独立升级的软件单元

[详情](https://mp.weixin.qq.com/s?__biz=MjM5MjEwNTEzOQ==&mid=401500724&idx=1&sn=4e42fa2ffcd5732ae044fe6a387a1cc3#rd)

**环境约束**

jdk1.8 :Spring Boot 1.7及以上

## 3. Spring Boot HelloWorld

浏览器发送hello请求，服务器接收请求并处理，响应helloWorld字符串

### 1.创建一个maven工程

### 2.导入依赖spring boot相关

```xml
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.1.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

### 3.编写主程序，启动spring boot应用

```java
/*
* @SpringBootApplication 来标注这是一个主程序类，说明是一个Springboot应用
 * */
@SpringBootApplication
public class HellWorldMainApplication {
    public static void main(String[] args) {
        // spring应用启动
        SpringApplication.run(HellWorldMainApplication.class,args);
    }
}
```

### 4.编写相关的Controller,Service

```java
@Controller
public class HelloController {
    @ResponseBody
    @RequestMapping("/hello")
    public String hello(){
        return  "Hello World";
    }
}
```

### 5.简化部署

导入

```java
<!--  这个插件的作用，将应用打包成一个可执行的jar包-->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

可以将这个应用打成jar包，直接在控制台使用java -jar 命令执行 （不需要tomcat,已自动嵌入打包）

## 4.Hello World探究

## 1.POM文件

### 1.父项目

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.1.RELEASE</version>
    </parent>
他的父项目是
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.2.1.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
  </parent>
这个父项目相对于是真正管理Spring boot应用里面的所有依赖版本
```

Spring boot版本仲裁中心

以后导入依赖默认是不需要写版本的 (没有在dependencies里面管理的依赖，依旧要声明版本号)

### 2.导入的依赖

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

```

**spring-boot-starter**-web：

​	spring-boot-starter： spring-boot场景启动器 ： 帮我们导入了web模块正常启动的所依赖的组件

Spring Boot将所有的功能场景都抽取出来，做成一个个的starts(启动器) ，只需要在项目里引入这些starter相关场景的所有依赖都会导入进来，需要用到什么场景，就会导入什么场景的启动器

## 2.主程序类，主入口类

```java
/*
* @SpringBootApplication 来标注这是一个主程序类，说明是一个Springboot应用
 * */
@SpringBootApplication
public class HellWorldMainApplication {
    public static void main(String[] args) {
        // spring应用启动
        SpringApplication.run(HellWorldMainApplication.class,args);
    }
}

```

@SpringBootApplication  : Spring Boot应用标注在某个类上说明这个类是Spring Boot 的主配置类， Spring Boot就应该运行这个类的main方法来启动Spring Boot 应用

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
```

**@SpringBootConfiguration** : Spring Boot的配置类

​		标志在某个类上，表示这个一个Spring Boot的配置类

​		@Configuration：配置类上来标注这个注解

​					配置类-------配置文件 	：配置类也是容器中的一个组件，@Component

@**EnableAutoConfiguration** 	：开启自动配置功能

​		以前我们需要配置的东西，Spring Boot帮我们自动配置 @EnableAutoConfiguration告诉 Spring Boot开启自动配置功能，这样配置才能生效

```java
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {

```

@**AutoConfigurationPackage**  : 自动配置包

​			@Import({org.springframework.boot.autoconfigure.AutoConfigurationPackages.Registrar.class}) 

​			Spring的底层注解@import ,给容器中导入一个组件 ；导入的组件由AutoConfigurationPackages.Registrar.class

将主配置类(@SpringBootApplication标注的类)的**所在包**及**下面所有子包**里面的所有组件扫描到Spring容器中

​	@Import(EnableAutoConfigurationImportSelector.class)

​			给容器中导入组件

​			EnableAutoConfigurationImportSelector 	:   导入哪些组件的选择器

​			将所有需要导入的组件以全类名的方式返回  ：这些组件就会被添加到容器中

​			会给容器中导入非常多的自动配置类（xxxAutoConfiguration）;就是给容器中导入这个场景需要的所有组件，并配置好这些组件

J2EE的整合解决方案和自动配置，都在spring-boot-autoconfigure.jar下

## 5.使用Spring initializer快速创建Spring Boot项目

 IDEA 都支持Spring的项目创建向导快读创建一个SpringBoot项目

默认生成Spring Boot项目

- 主程序已经生成好了， 只需要编写业务逻辑就行
- resource文件夹中目录结构
  - static :静态资源文件
  - templates：保存所有的模板页面 ：(spring boot默认jar包使用嵌入式的Tomcat ,默认不支持jsp页面) 可以使用模板引擎
  - application.properties ：Spring boot应用的配置文件 ：可以修改一些默认设置

# 二、配置文件

 ## 1、配置文件

Spring Boot使用一个全局的配置文件 , 配置文件名是固定的

- application.properties
- application.yml

配置文件的作用 ：修改SpringBoot自动配置的默认值 ；Spring Boot在底层都给我自动配置好了

YAML （YAML Ain't Markup Language）

​		YAML A Markup Language ：是一个标记语言

​		YAML isn't Markup Language ：不是一个标记语言

标记语言 ：

​		以前的配置文件 ；大多数是.xml文件 ； 但是yaml是以数据为中心，比json,xml更适合做标记语言

​	YAML :配置列子

```yaml
server:
	port:8081
```

​	XML :

```xml
<server>
	<port>8081</port>
</server>
```

## 2.YAML语法

### 1.基本语法

k: value 表示一堆键值对（空格必须有）

以**空格**的缩进来控制层级关系 ：只要是左对齐的一列数据，都是同一层级的

```yaml
server:
	prot: 8081
	path: /hello
```

属性和值也是大小写敏感的

### 2.值的写法

**字面量： 普通的值(数字，字符串，布尔)**

 k: v   :字面量直接来写就行

​		字符串默认不用加上单引号或双引号：

​			" " ：双引号  ：不会转义字符串里面的特殊字符 ，特殊字符会作为本身表示的意思

​					name: ”zhangsan \n lisi“  :输出：zhangsan 换行 lisi

​			' ' ： 单引号   :会转义特殊字符  ，特殊字符就会变成字符串输出

​					name: ”zhangsan \n lisi“  :输出：zhangsan \n lisi

**对象 Map（属性和值）（键值对）：**

 k: v : 在下一行来写对象的属性和值的关系，注意空格缩进

​		对象还是k: v的方式

```yaml
friends:
	lastName: zhangsan
	age: 20
```

​	行内写法：

```yaml
friend: {lastName: zhangsan,age: 18}
```

数组 （List，Set）

用-值表示数组中的一个元素

```yml
pets:
- cat
- dog
- pig
```

​	行内写法

```yml
pets: [cat,dog,pig]
```

## 3.获取配置文件值注入

配置文件

```yaml
person:
  lastName: zhangsan
  age: 18
  boss: true
  birth: 2017/12/12
  maps: {k1: v1,k2: v2}
  lists:
    - lisi
    - zhaoliu
  dog:
     name: 小狗
     age: 2
```

javaBean:

```java
/*
* 将配置文件中配置的每个属性的值，映射到这个组件中
* @ConfigurationProperties :告诉SpringBoot将本类中所有的属性和配置文件中相关的配置进行绑定
*   prefix = "person" :配置文件中那个下面的所有属性进行一一映射
*	且是全局的
* 只有这个组件是容器中的组件，才能使用容器提供的功能
* */
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date brith;

    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
```

我们可以导入配置文件处理器，以后编写配置就有提示了

```xml
<!--  导入配置文件处理器，配置文件进行绑定就会有提示  -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
```

#### 1.@ConfigurationProperties与@Value获取值比较

|                      | @ConfigurationProperties | @Value     |
| -------------------- | ------------------------ | ---------- |
| 功能上               | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定（松散语法） | 支持                     | 不支持     |
| El表达式             | 不支持                   | 支持       |
| jsr303数据校验       | 支持                     | 不支持     |
| 复杂类型封装map list | 支持                     | 不支持     |

配置文件yml还是properties他们都能获取到值

如果说，我们值是在某个业务逻辑中需要获取一下配置文件中的某项值，使用@Value

如果是，我们专门编写了一个javabean来和配置文件进行映射；我们就直接使用@ConfigurationProperties

#### 2.配置文件注入值数据校验

``` java
@Component
@ConfigurationProperties(prefix = "person")
@Validated
public class Person {
    //lastName 必须是邮箱格式
    @Email
    private String lastName;
```

#### 3.@PropertySource 与 @ImportResource

@PropertySource：加载指定的配置文件

```java
@Component
@ConfigurationProperties(prefix = "person")
@PropertySource(value = {"classpath:persion.properties"})
public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date brith;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
```

@ImportResource：导入Spring的配置文件，让配置文件里面的内容生效

Spring Boot里面没有Spring的配置文件，我自己编写的配置文件，也不能识别

想让Spring的配置文件生效，加载进来， 就要将@ImportResource标注在一个配置类上

```java
@ImportResource(locations = {"classpath:beans.xml"})
@SpringBootApplication
public class SpringBoot01HelloworldApplication {
	public static void main(String[] args) {
        SpringApplication.run(SpringBoot01HelloworldApplication.class, args);
    }
}
//导入Spring配置文件让其生效
```

xml配置文件 ----- 不推荐

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean class="com.study.springboot01.service.HelloService" id="helloService">

    </bean>
</beans>
```

但是步骤繁琐，不推荐

SpringBoot推荐给容器中添加组件的方式：推荐使用全注解的方式

1.配置类 ====== Spring配置文件

2.使用@Bean给容器中添加组件

```java
/*配置类   @Configuration指明当前类是一个配置类；就是来替代Spring配置文件
* 在配置文件中的用<bean></bean>标签添加组件
其中id就是方法名
* */
@Configuration
public class MyAppConfig {
    //将方法的返回值添加到容器中，容器中这个组件默认的id就是方法名
    @Bean
    public HelloService helloService(){
        System.out.println("@Bean给容器中添加组件 了");
        return new HelloService();
    }
}
```

## 4.配置文件占位符

### 1、随机数

```java
random.value  、 {random.int} 、${random.lang}  、 ${random.int(10)} 、 ${random.int[1024,65536]}
```

### 2、占位符获取之前配置的值，如果没有可以使用 : 指定默认值

```properties
person.last-name=410414825${random.uuid}
person.age=${random.int}
person.brith=2017/12/04
person.boss=false
person.maps.k1=v2
person.maps.k2=14
person.lists=a,b,c
person.dog.name=${person.hello: hello}是狗
person.dog.age=12
```

## 5.Profile

### 1、多Profile文件

我们在主配置配置文件编写的时候，文件名可以是 application-{profile}.properties/yml

然后就可以动态来切换

默认使用application.properties/yml的配置 ；

### 2、yml支持多文档快方式profile

```yaml
server:
  port: 8081

spring:
  profiles:
    active: prod
---
server:
  port: 8083
spring:
  profiles: dev
---
server:
  port: 8084
spring:
  profiles: prod
```

### 3、激活指定profile

​	1.在配置文件中指定  spring.profiles.active=dev

​	2.命令行

​		可以在启动器中配置启动参数--spring.profiles.active=dev

​		或打包后在cmd中

​		java -jar xxxxx.jar --spring.profiles.active=dev

​	3.虚拟机参数：

​		在启动器中配置虚拟机启动参数

​		-Dspring.profiles.active=dev

![1574839001198](../Spring Boot.assets/1574839001198.png)

## 6.配置文件加载位置

Springboot启动会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件

```xml
- file:../config/ 根路径下的config文件
- file:../		根路径
- classpath:/config/  类路径下的config文件
- classpath:/  类路径
```

以上是按照**优先级从高到低**的排序，所有位置的文件都会被加载，高优先级配置内容会**覆盖**低优先级配置内容，SpringBoot会从4个位置全部加载主配置文件 ；**互补配置**

也可以通过配置spring.config.location来改变默认配置文件位置

**项目打包好了以后，我们还可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置，指定配置文件的默认加载的这些配置文件共同其作用形成互补配置**

## 7.外部配置值加载位置

SpringBoot也可以从以下位置加载配置 ：  以下按照优先级从高到低配置   高优先级的配置覆盖低优先级的配置，所有的配置会形成互补配

​	1.**命令行配置**

​	java -jar xxxx.jar --server.port=8087

​		多个配置用空格分开 ；--配置项=值

​	2.来自java:comp/envv的JNDI属性

​	3.java系统属性(System.getProperties)

​	4.操作系统环境变量

​	5.RandomValuePropertySource配置的random.*属性值

​			**由jar包外向jar包内进行寻找**

​				**优先加载带profile**

​	6.jar包外部的application-{profile}.proerties或application-{profile}.yml（带spring.profile）配置文件

​	7.jar包内部的application-{profile}.proerties或application-{profile}.yml（带spring.profile）配置文件

​				**再来加载不带profile**

​	8.jar包外部的application.proerties或application.yml（不带spring.profile）配置文件

​	9.jar包内部的application.proerties或application.yml（不带spring.profile）配置文件

​	10.@Configuration注解类上的@PropertySource

​	11.通过SpringApplication.setDefaultProperties指定的默认属性

## 8.自动配置原理

配置文件到底能写什么，怎么写，自动配置原理 ：

[配置文件能配置的属性参照]: https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/htmlsingle/#common-application-properties

### 1.自动配置原理：

1）、SpringBoot启动的时候加载了主配置类，开启了自动配置功能**@EnableAutoConfiguration**

2）、 @EnableAutoConfiguration  作用 ：

			- 利用AutoConfigurationImportSelector.class给容器中导入了哪些组件？
			- 可以查看selectImports()方法的内容；

  - List<String> configurations =getCandidateConfigurations（annotationMetadata, attributes）;  获取候选的配置

    **将类路径下 META-INF/spring.factories 里面配置的素有EnableAutoConfiguration的值加入到了容器中**

``` java
# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration,\
org.springframework.boot.autoconfigure.batch.BatchAutoConfiguration,\
org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration,\
org.springframework.boot.autoconfigure.cassandra.CassandraAutoConfiguration,\
org.springframework.boot.autoconfigure.cloud.CloudServiceConnectorsAutoConfiguration,\
org.springframework.boot.autoconfigure.context.ConfigurationPropertiesAutoConfiguration,\
org.springframework.boot.autoconfigure.context.MessageSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.context.PropertyPlaceholderAutoConfiguration,\
org.springframework.boot.autoconfigure.couchbase.CouchbaseAutoConfiguration,\
org.springframework.boot.autoconfigure.dao.PersistenceExceptionTranslationAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraReactiveDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraReactiveRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseReactiveDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseReactiveRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ReactiveElasticsearchRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ReactiveRestClientAutoConfiguration,\
org.springframework.boot.autoconfigure.data.jdbc.JdbcRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.jpa.JpaRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.ldap.LdapRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoReactiveDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoReactiveRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.neo4j.Neo4jDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.neo4j.Neo4jRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.solr.SolrRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisReactiveAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.rest.RepositoryRestMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.data.web.SpringDataWebAutoConfiguration,\
org.springframework.boot.autoconfigure.elasticsearch.jest.JestAutoConfiguration,\
org.springframework.boot.autoconfigure.elasticsearch.rest.RestClientAutoConfiguration,\
org.springframework.boot.autoconfigure.flyway.FlywayAutoConfiguration,\
org.springframework.boot.autoconfigure.freemarker.FreeMarkerAutoConfiguration,\
org.springframework.boot.autoconfigure.gson.GsonAutoConfiguration,\
org.springframework.boot.autoconfigure.h2.H2ConsoleAutoConfiguration,\
org.springframework.boot.autoconfigure.hateoas.HypermediaAutoConfiguration,\
org.springframework.boot.autoconfigure.hazelcast.HazelcastAutoConfiguration,\
org.springframework.boot.autoconfigure.hazelcast.HazelcastJpaDependencyAutoConfiguration,\
org.springframework.boot.autoconfigure.http.HttpMessageConvertersAutoConfiguration,\
org.springframework.boot.autoconfigure.http.codec.CodecsAutoConfiguration,\
org.springframework.boot.autoconfigure.influx.InfluxDbAutoConfiguration,\
org.springframework.boot.autoconfigure.info.ProjectInfoAutoConfiguration,\
org.springframework.boot.autoconfigure.integration.IntegrationAutoConfiguration,\
org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.JdbcTemplateAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.JndiDataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.XADataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.DataSourceTransactionManagerAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.JmsAutoConfiguration,\
org.springframework.boot.autoconfigure.jmx.JmxAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.JndiConnectionFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.activemq.ActiveMQAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.artemis.ArtemisAutoConfiguration,\
org.springframework.boot.autoconfigure.groovy.template.GroovyTemplateAutoConfiguration,\
org.springframework.boot.autoconfigure.jersey.JerseyAutoConfiguration,\
org.springframework.boot.autoconfigure.jooq.JooqAutoConfiguration,\
org.springframework.boot.autoconfigure.jsonb.JsonbAutoConfiguration,\
org.springframework.boot.autoconfigure.kafka.KafkaAutoConfiguration,\
org.springframework.boot.autoconfigure.ldap.embedded.EmbeddedLdapAutoConfiguration,\
org.springframework.boot.autoconfigure.ldap.LdapAutoConfiguration,\
org.springframework.boot.autoconfigure.liquibase.LiquibaseAutoConfiguration,\
org.springframework.boot.autoconfigure.mail.MailSenderAutoConfiguration,\
org.springframework.boot.autoconfigure.mail.MailSenderValidatorAutoConfiguration,\
org.springframework.boot.autoconfigure.mongo.embedded.EmbeddedMongoAutoConfiguration,\
org.springframework.boot.autoconfigure.mongo.MongoAutoConfiguration,\
org.springframework.boot.autoconfigure.mongo.MongoReactiveAutoConfiguration,\
org.springframework.boot.autoconfigure.mustache.MustacheAutoConfiguration,\
org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration,\
org.springframework.boot.autoconfigure.quartz.QuartzAutoConfiguration,\
org.springframework.boot.autoconfigure.rsocket.RSocketMessagingAutoConfiguration,\
org.springframework.boot.autoconfigure.rsocket.RSocketRequesterAutoConfiguration,\
org.springframework.boot.autoconfigure.rsocket.RSocketServerAutoConfiguration,\
org.springframework.boot.autoconfigure.rsocket.RSocketStrategiesAutoConfiguration,\
org.springframework.boot.autoconfigure.security.servlet.SecurityAutoConfiguration,\
org.springframework.boot.autoconfigure.security.servlet.UserDetailsServiceAutoConfiguration,\
org.springframework.boot.autoconfigure.security.servlet.SecurityFilterAutoConfiguration,\
org.springframework.boot.autoconfigure.security.reactive.ReactiveSecurityAutoConfiguration,\
org.springframework.boot.autoconfigure.security.reactive.ReactiveUserDetailsServiceAutoConfiguration,\
org.springframework.boot.autoconfigure.security.rsocket.RSocketSecurityAutoConfiguration,\
org.springframework.boot.autoconfigure.security.saml2.Saml2RelyingPartyAutoConfiguration,\
org.springframework.boot.autoconfigure.sendgrid.SendGridAutoConfiguration,\
org.springframework.boot.autoconfigure.session.SessionAutoConfiguration,\
org.springframework.boot.autoconfigure.security.oauth2.client.servlet.OAuth2ClientAutoConfiguration,\
org.springframework.boot.autoconfigure.security.oauth2.client.reactive.ReactiveOAuth2ClientAutoConfiguration,\
org.springframework.boot.autoconfigure.security.oauth2.resource.servlet.OAuth2ResourceServerAutoConfiguration,\
org.springframework.boot.autoconfigure.security.oauth2.resource.reactive.ReactiveOAuth2ResourceServerAutoConfiguration,\
org.springframework.boot.autoconfigure.solr.SolrAutoConfiguration,\
org.springframework.boot.autoconfigure.task.TaskExecutionAutoConfiguration,\
org.springframework.boot.autoconfigure.task.TaskSchedulingAutoConfiguration,\
org.springframework.boot.autoconfigure.thymeleaf.ThymeleafAutoConfiguration,\
org.springframework.boot.autoconfigure.transaction.TransactionAutoConfiguration,\
org.springframework.boot.autoconfigure.transaction.jta.JtaAutoConfiguration,\
org.springframework.boot.autoconfigure.validation.ValidationAutoConfiguration,\
org.springframework.boot.autoconfigure.web.client.RestTemplateAutoConfiguration,\
org.springframework.boot.autoconfigure.web.embedded.EmbeddedWebServerFactoryCustomizerAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.HttpHandlerAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.ReactiveWebServerFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.WebFluxAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.error.ErrorWebFluxAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.function.client.ClientHttpConnectorAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.function.client.WebClientAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.DispatcherServletAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.ServletWebServerFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.error.ErrorMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.HttpEncodingAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.MultipartAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.reactive.WebSocketReactiveAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.servlet.WebSocketServletAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.servlet.WebSocketMessagingAutoConfiguration,\
org.springframework.boot.autoconfigure.webservices.WebServicesAutoConfiguration,\
org.springframework.boot.autoconfigure.webservices.client.WebServiceTemplateAutoConfiguration

```

每一个这样的xxxAutoCondiguration类都是容器中的一个组件，都加入到容器中；用他们来做自动配置；

3）、每一个自动配置类进行自动配置功能 ;

4）、 以HttpEncodingAutoConfiguration为例解释自动配置原理;

```java
@Configuration(proxyBeanMethods = false) //表示这是一个配置类，可以给容器中添加组件
@EnableConfigurationProperties({HttpProperties.class})// 启用指定ConfigurationProperties功能；将配置文件中对应的值和HttpProerties绑定起来了
@ConditionalOnWebApplication(type = ConditionalOnWebApplication.Type.SERVLET)//Spring底层的@Conditional注解，根据不同的条件，如果满足指定的条件，整个配置类里面的配置就会生效 ；  判断当前应用是否是web应用，如果是，当前配置类生效
@ConditionalOnClass({CharacterEncodingFilter.class})//判断，担负起项目有没有这个类   CharacterEncodingFilter：是springmvc中字符编码过滤器
@ConditionalOnProperty(prefix = "spring.http.encoding", value = {"enabled"}, matchIfMissing = true)//判断，配置文件中是否存在某个配置spring.http.encoding.enabled   matchIfMissing = true 如果不存在，判断也是成立的
//即使我们配置文件中不配置spring.http.encoding.enabled =true ，也是默认生效的
    
    @Bean //给容器中添加一个组件
    @ConditionalOnMissingBean
    public org.springframework.web.filter.CharacterEncodingFilter characterEncodingFilter() { /* compiled code */ }
```

总结 ：根据当前不同的条件判断，决定这个配置类是否生效？

 			一旦这个配置类生效，这个配置类就会给容器中添加各种组件；这些组件的属性是从对应的priorities类中获取的，这些类里面的每一个属性又是和配置文件绑定的 ；

5）、所有在配置文件中能配置的属性都是在xxxxProperties类中封装的；配置文件能配置什么就可以参照某个功能对应的这个属性类

```java
@ConfigurationProperties //从配置文件中获取组合一定的值和bean的属性进行绑定
```

 **SpringBoot精髓**

1. SpringBoot启动会加载大量的自动配置类
2. 我们看我们需要的功能有没有SpringBoot默认写好的自动配置类;
3. 我们再来看这个自动配置类中到底配置了哪些组件 （只要我们需要用的组件有，我们就不需要配置了）
4. 给容器中自动配置类添加组件的时候，会从properties类中获取某些属性，我们就可以在配置文件中指定这些属性的值；

xxxxxxAutoConfiguration :自动配置类

给容器中添加组件

xxxxxxProperties：封装配置文件中相关属性

### 2.细节

 #### 1.@Conditional派生注解 （Spring注解版原生的@Conditional作用）

作用 ：必须是@Conditional指定的条件成立，才给容器中添加组件，配置类里面的所有内容才生效

| @Conditional拓展注解            | 作用（判断是否满足当前指定条件）                |
| ------------------------------- | ----------------------------------------------- |
| @ConditionalOnJava              | 系统的java版本是否符合要求                      |
| @ConditionalOnBean              | 容器中存在指定Bean                              |
| @ConditionalOnMissingBean       | 容器中不存在指定Bean                            |
| @ConditionalOnExpression        | 满足SpEl表达式指定                              |
| @ConditionalOnClass             | 容器中有指定的类                                |
| @ConditionalOnMissingClass      | 容器中没有指定的类                              |
| @ConditionalOnSingleCandidate   | 容器中只有一个指定的bean,或者这个bean是首选bean |
| @ConditionalOnProperty          | 系统中指定的属性是否有指定的值                  |
| @ConditionalOnResource          | 类路径下是否存在指定资源文件                    |
| @ConditionalOnWebApplication    | 当前是web环境                                   |
| @ConditionalOnNotWebApplication | 当前不是web环境                                 |
| @ConditionalOnJndi              | JNDI存在指定项                                  |

自动配置类必须在定义的条件下才能生效

我们这么知道哪些自动配置类生效了

我们可以通过启用debug= true属性，来让控制台打印自动配置报告，这样我们就可以很方便的知道哪些自动配置类生效了

```java
============================
CONDITIONS EVALUATION REPORT
============================


Positive matches: // 启用 ，自动配置类匹配成功
-----------------

   AopAutoConfiguration matched:
      - @ConditionalOnProperty (spring.aop.auto=true) matched (OnPropertyCondition)

   AopAutoConfiguration.ClassProxyingConfiguration matched:
      - @ConditionalOnMissingClass did not find unwanted class 'org.aspectj.weaver.Advice' (OnClassCondition)
      - @ConditionalOnProperty (spring.aop.proxy-target-class=true) matched (OnPropertyCondition)

   DispatcherServletAutoConfiguration matched:
      - @ConditionalOnClass found required class 'org.springframework.web.servlet.DispatcherServlet' (OnClassCondition)
      - found 'session' scope (OnWebApplicationCondition)

          
 Negative matches: // 没启用  ，没有匹配成功的自动配置类
-----------------

   ActiveMQAutoConfiguration:
      Did not match:
         - @ConditionalOnClass did not find required class 'javax.jms.ConnectionFactory' (OnClassCondition)

   AopAutoConfiguration.AspectJAutoProxyingConfiguration:
      Did not match:
         - @ConditionalOnClass did not find required class 'org.aspectj.weaver.Advice' (OnClassCondition)

   ArtemisAutoConfiguration:
      Did not match:
         - @ConditionalOnClass did not find required class 'javax.jms.ConnectionFactory' (OnClassCondition)

```

# 三、日志

## 1、日志框架

市面上的日志框架     jul、jcl、jboss-logging、logback、log4j、log4j2.......

| 日志门面（日志抽象层）                                       | 日志实现                                      |
| ------------------------------------------------------------ | --------------------------------------------- |
| ~~JDC（jakarta Commons Logging）~~ SLF4J （Simple Logging Facade for java）  ~~jboss-logging~~ | Log4j JUL （java.util.logging）Log4j2 Logback |

左边选一个门面（抽象层）、右边来选一个实现

jbl没人用。jdc太老了  ，log4j没有logback先进 ，jul不太行  log4j2太好了，许多框架不支持 所以用logback

日志门面：SLF4J 

日志实现：Logback

SpringBoot底层 是Spring框架， Spring框架是默认使用jcl

​			**SpringBoot包装后使用的是SLF4J+Logback**

## 2、SLF4J使用

### 1、如何在系统中使用SLF4j

以后开发的时候，日志记录，日志记录方法的调用，不应该来直接调用日志的实现，而是调用日志抽象层里面的方法

``` java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}
```

首先应该给系统里导入SLF4J的jar包和logback的实现jar

![click to enlarge](http://www.slf4j.org/images/concrete-bindings.png)

每一个日志的实现框架都有自己的配置文件。使用slf4j以后，**配置文件还是做成日志实现框架自己本身的配置文件**

### 2、遗留问题

因为在整合其他框架的时候，其他框架也许会自带日志，那么就需要统一日志记录

即使是别的框架和我一起统一使用slf4j进行输出

![click to enlarge](http://www.slf4j.org/images/legacy.png)

如何让系统中所有的日志都统一到slf4j

**1.将系统中其他日志框架盘先排除出去**

**2.用中间包替换原有的日志框架**

**3.再导入slf4j其他的实现jar**

## 3、SpringBoot日志关系

``` xml
<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
    </dependency>
```



SpringBoot使用它来做日志功能

```xml
<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-logging</artifactId>
    </dependency>
```

底层依赖关系

![1575186080223](1575186080223.png)

总结 ：

1. ​	SpringBoot底层也是使用Slf4j+logback的方式进行日志纪律
2. ​    SrpingBoot也把其他的日志都替换成了slf4j
3. ​     中间转换包？

![1575186368970](1575186368970.png)

​				4. 如果我们要引入其他框架？一定要把这个框架的默认日志依赖移除掉！

​			**SpringBoot能够自动适配所有日志，而且底层是哟经slf4j+logback的方式记录，引入其他框架的时候只需要把这个框架依赖的日志框架排除掉，就可以适配**

## 4、日志使用

### 1、默认配置

SpringBoot默认帮我们配置好了日志

``` java
    //记录器
    Logger logger = LoggerFactory.getLogger(getClass());
    @Test
    void contextLoads() {
        //日志的级别  由低到高 trace<debug<info<warn<error
        //可以调整日志的输入级别 日志就只会在这个级别以后的高级别生效
        logger.trace("这是trance日志（追踪轨迹）");
        logger.debug("这是debug日志");
        // SpringBoot默认给我使用的是info级别的  ,没有指定级别的，就使用SpringBoot默认规定的级别 ：root级别
        logger.info("这是info日志");
        logger.warn("这是warn警告信息");
        logger.error("这是error日志");
    }
```

SpringBoot修改日志的默认配置

 ``` properties
# 指定日志输出级别
logging.level.com.study=trace

# 不指定路径则当前项目下生成springboot.log日志
# 也可以指定完整的路径
# logging.file=springboot.log  已弃用
# 在当前磁盘的根路径下创建spring文件夹和里面的log文件夹，使用spring.log作为默认文件
logging.file.path=/spring/log
# 控制台输出的日志格式
logging.pattern.console=百度
# 指定问价那种日志输出的格式
logging.pattern.file=百度
 ```

### 2、指定配置

给类路径下放上每一个日志框架自己的配置文件即可；SpringBoot就不适用其他的默认配置的了，参照官网

logback.xml：直接就被日志框架识别了

**logback-spring.xml**:日志框架就不直接加载日志的配置项，有SpringBoot来解析日志配置，可以使用SpringBoot的高级Profile报错

```xml
<spring-inProfile name="staging">
    可以指定配置某段配置，只在某个环境下生效
</spring-inProfile>
```

否则就会报错 no applicable action for [springProfile]

## 5.切换日志

可以按照slf4j的日志适配图，进行相关的切换

先排除，再引入

# 四、Web开发

## 1、简介

使用SpringBoot

**1）、创建SpringBoot应用，选中需要的模块**

**2）、SpringBoot已经默认将这些场景配置好了，只需要在配置文件中指定少量配置就可运行起来**

**3）、自己编写业务代码**

自动配置和原理

```java
xxxxAutoConfiguration：帮我们给容器中自动配置组件
xxxxProperties   :配置来封装配置文件的内容
```

## 2、SpringBoot对静态资源的映射规则

1）、所有/webjars/** ,都去classpath:/META-INF/resources/webjars/找资源

webjars: 以jar包的方式引入静态资源

![1575259436711](1575259436711.png)

localhost:8080/webjars/jquery/3.4.1/jquery.js

```xml 
<dependency>在访问的时候止写webjars下面的资源名称即可
            <groupId>org.webjars</groupId>
            <artifactId>jquery</artifactId>
            <version>3.4.1</version>
</dependency>
```

2）、“/**"  访问当前项目的任何资源，（静态资源的文件夹）

``` java
"classpath:/META-INF/resources/",
"classpath:/resources/",
"classpath:/static/",
"classpath:/public/",
"/"当前项目的根路径
```

localhost:8080/abc ===去静态资源文件夹里面找abc

3）、欢迎页 ；静态资源文件夹下所有的index.html页面  被"/**"映射

4）、所有的 **/favicon.ico都是在静态资源文件夹下找

## 3、模板引擎

SpringBoot推荐的Thymeleaf

语法更简单，功能更强大

### 1.引入thymeleaf

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

### 2.Thymeleaf使用&语法

```java
@ConfigurationProperties(prefix = "spring.thymeleaf")
public class ThymeleafProperties {

	private static final Charset DEFAULT_ENCODING = StandardCharsets.UTF_8;

	public static final String DEFAULT_PREFIX = "classpath:/templates/"; //前缀

	public static final String DEFAULT_SUFFIX = ".html"; //后缀
只要把html页放在classpath:/templates/ ,templates就能自动渲染了
```

只要我们把HTML页面放在classpath：/templates/ ,thymeleaf就能自动渲染

使用：

1.导入thymeleaf的名称空间

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```

2.使用thymeleaf语法

 th:text :改变当前元素里面的文本内容

 th: 任意html属性   来替换原生的属性

![1575290563593](1575290563593.png)

2）、表达式

![1575290847365](1575290847365.png)

## 4、SpringMVC自动配置

### 1、Spring MVC auto-configuration

Spring Boot自动配置好了SpringMVC

​	以下是Spring Boot对SpringMVC的默认配置

- 自动配置了ViewResolver(视图解析器：根据方法的返回值得到视图对象决定渲染方法)
  - ContentNogotiatingViewResolver：组合所有的视图解析器
  - 如何定制；我们可以自己给容器中添加一个视图解析器，容器会自动的添加进来

- 自动注册了 Converter，GenericConverter，Formatter
  - Converter：转换器；public String hello(User user):类型转换的时候用的
  - Formatter：格式化器

```java
    @Bean
    @ConditionalOnProperty(prefix = "spring.mvc", name = "date-format")//在文件中配置日期格式化的规则
    public Formatter<Date> dateFormatter() {
     return new DateFormatter(this.mvcProperties.getDateFormat())//日期格式对象
    }
```

​      自己添加的格式化器/转化器，我们只需要放在容器中即可

- Support for HttpMessageConverters (see below)

  - HttpMessageConverter  ：SpringMvc用来转换Http请求和响应的
  - HttpMessageConverters 是从容器中确定；获取所有的HttpMessageConverter  

  自己给容器添加HttpMessageConverter ，只需要将自己的组件注册到容器中(@Bean,@Component)

- Automatic registration of MessageCodesResolver (see below)定义错误代码生成规则

- Automatic registration of a ConfigurableWebBindingInitializer bean (see below)

  我们可以配置一个ConfigurableWebBindingInitializer来替换默认的

  ```
  初始化WebDataBinder
  请求数据----javabean
  ```

  org.springframework.boot.autoconfigure.web：web的所有自动场景

### 2、扩展SpringMVC

```xml
<mvc:view-controller path="/hello" view-name="success"/>
<mvc:interceptors>
	<mvc:interceptor>
    	<mvc:mapping path="/hello"></mvc:mapping>
    </mvc:interceptor>
</mvc:interceptors>
```

如果想拓展SpringMVC ，则需要编写一个配置类(@Configuration)，是WebMvcConfigurerAdapter类型；不能标注@EnbleWebMvc

```java
@Configuration
public class MyMvcConfig extends WebMvcConfigurerAdapter{
    @Override
    public void addViewControllers(ViewControllerRegistry registry){
        //super.addViewController(register);
        //浏览器发送/one请求 来到success页面
        register.addViewController("/one").setViewName("success");
    }
}
//既保留了所有的自动配置，也能用我们的配置
```

原理：

1. WebMvcAutoConfiguration是SpringMvc的自动配置类
2. 在做其他自动配置时会导入；@Import(EnableWebMvcConfiguration.class)

```java
@Configuration
 public static class EnableWebMvcConfiguration extends DelegatingWebMvcConfiguration{
     private final WebMvcConfigurerComposite cofigurers =new WebMvcConfigurerComposite();
     
     //从容器中获取所有的WebMvcConfigurers
     @Autowired(required = false)
     public void setConfigurers(List<WebMvcConfigurer> configurers){
         if(!ConllectionUtil.isEmpty(configurers)){
             this.configurers.addWebMvcConfigurers(configurers);
             //将所有的WebMvcConfigurer相关配置都一起来调用
        }
     }
 }
```

 3. 容器中所有的WebMvcConfigurer都会一起起作用

 4. 我们的配置类也会被调用

    效果：SpringMVC的自动配置和我们的扩展配置都会起作用

**但是在2.0版本以后 ，删除了WebMvcConfigurerAdapter替换为WebMvcConfigurer**

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        //浏览器发送/atguigu请求来到success页面
        registry.addViewController("/atguigu").setViewName("success");
    }

    //所有的WebMvcConfigure组件一起起作用
    @Bean      //将组件注册在容器中
    public WebMvcConfigurer webMvcConfigurer() {
      WebMvcConfigurer webMvcConfigurer= new WebMvcConfigurer(){
          @Override
          public void addViewControllers(ViewControllerRegistry registry) {
              registry.addViewController("/").setViewName("login");
              registry.addViewController("/login.html").setViewName("login");
          }
      };
      return webMvcConfigurer;
    }
}
```



### 3、全面接管SprignMVC

SpringBoot对SpringMVC的自动配置不要了，所有都是我们自己配置，同时所有的Spring MVC模块的自动配置就都失效了，我们需要在配置类中添加@EnableWebMvc

```java
@EnableWebMvc
@Configuration
public class MyMvcConfig extends WebMvcConfigurerAdapter{
    @Override
    public void addViewControllers(ViewControllerRegistry registry){
        //super.addViewController(register);
        //浏览器发送/one请求 来到success页面
        register.addViewController("/one").setViewName("success");
    }
}
```

原理：

​	为什么加了@EnableWebMvc后自动配置就失效了。

EnableWebMvc的核心

```java
@Import(DelegatingWebMvcConfiguration.class)
public @interface EnableWebMvc{}
```

@EnableWebMvc将WebMvcConfigurationSupport组件导入进来；

导入的WebMvcConfigurationSupport只是SpringMvc最基本的功能

## 5、如何修改SpringBoot的默认配置

模式：

1. SpringBoot在自动配置很多组件的时候，先看容器中有没有用户自己配置的（@Bean,@Component）如果有就用用户配置的，如果没有才自动配置；如果有些组件可以有多个(ViewResolver)将用户配置的和自己默认的组合起来
2. 在SpringBoot中会有非常多的xxConfigutrt帮我我们进行拓展配置
3. 在SpringBoot中会有很多的xxxCustomizer帮我们进行定制配置

## 6、RestfulCRUD

### 1)、配置登陆界面

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        //浏览器发送/atguigu请求来到success页面
        registry.addViewController("/atguigu").setViewName("success");
    }

    //所有的WebMvcConfigure组件一起起作用
    @Bean      //将组件注册在容器中
    public WebMvcConfigurer webMvcConfigurer() {
      WebMvcConfigurer webMvcConfigurer= new WebMvcConfigurer(){
          @Override
          public void addViewControllers(ViewControllerRegistry registry) {
              registry.addViewController("/").setViewName("login");
              registry.addViewController("/login.html").setViewName("login");
          }
      };
      return webMvcConfigurer;
    }
}
```

### 2)、国际化

以前SpringMVC时的步骤

1）国际化配置文件

2）使用ResourceBundleMessageSource管理国际化资源文件

3）在页面使用fmt:message取出国际化资源

SpringBoot的步骤

1）编写国际化配置文件，抽取页面需要显示的国际化消息

![image-20200112095109972](/image-20200112095109972.png)

2) SpringBoot自动配置好了管理国际化资源文件的组件

```java
	@Bean
	@ConfigurationProperties(prefix = "spring.messages")
	public MessageSourceProperties messageSourceProperties() {
		return new MessageSourceProperties();
	}
	@Bean
	public MessageSource messageSource(MessageSourceProperties properties) {
		ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
		if (StringUtils.hasText(properties.getBasename())) {
            //设置国际化资源的基础名字（去掉语言国家代码的）
			messageSource.setBasenames(StringUtils			.commaDelimitedListToStringArray(StringUtils.trimAllWhitespace(properties.getBasename())));
		}
		if (properties.getEncoding() != null) {
			messageSource.setDefaultEncoding(properties.getEncoding().name());
		}
		messageSource.setFallbackToSystemLocale(properties.isFallbackToSystemLocale());
		Duration cacheDuration = properties.getCacheDuration();
		if (cacheDuration != null) {
			messageSource.setCacheMillis(cacheDuration.toMillis());
		}
		messageSource.setAlwaysUseMessageFormat(properties.isAlwaysUseMessageFormat());
		messageSource.setUseCodeAsDefaultMessage(properties.isUseCodeAsDefaultMessage());
		return messageSource;
	}
```

```java
public class MessageSourceProperties {

	/**
	 * Comma-separated list of basenames (essentially a fully-qualified classpath
	 * location), each following the ResourceBundle convention with relaxed support for
	 * slash based locations. If it doesn't contain a package qualifier (such as
	 * "org.mypackage"), it will be resolved from the classpath root.
	 */
	private String basename = "messages";
    //我们的配置文件可以直接放在类路径下叫message.properties

```

```properties
# 国际化配置文件 （包名.基础名）
spring.messages.basename=i18n/login,i18n/dashboard
```

3)  去页面获取国际化的值

如果出现乱码，是idea默认编码格式没有转成ascii码

![image-20200112101616906](image-20200112101616906.png)

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
		<meta name="description" content="">
		<meta name="author" content="">
		<title>Signin Template for Bootstrap</title>
		<!-- Bootstrap core CSS -->
		<link href="asserts/css/bootstrap.min.css" rel="stylesheet" th:href="@{/webjars/bootstrap/4.0.0/css/bootstrap.css}">
		<!-- Custom styles for this template -->
		<link href="asserts/css/signin.css" rel="stylesheet" th:href="@{/asserts/css/signin.css}">
	</head>

	<body class="text-center">
		<form class="form-signin" action="dashboard.html">
			<img class="mb-4" th:src="@{/asserts/img/bootstrap-solid.svg}" src="asserts/img/bootstrap-solid.svg" alt="" width="72" height="72">
			<h1 class="h3 mb-3 font-weight-normal" th:text="#{login.tip}">Please sign in</h1>
			<label class="sr-only" th:text="#{login.username}">Username</label>
			<input type="text" class="form-control" placeholder="Username" th:placeholder="#{login.username}" required="" autofocus="">
			<label class="sr-only" th:text="#{login.password}">Password</label>
			<input type="password" class="form-control" placeholder="Password" th:placeholder="#{login.password}" required="">
			<div class="checkbox mb-3">
				<label>
          <input type="checkbox" value="remember-me">[[#{login.remember}]]
        </label>
			</div>
			<button class="btn btn-lg btn-primary btn-block" type="submit" th:text="#{login.btn}">Sign in</button>
			<p class="mt-5 mb-3 text-muted">© 2017-2018</p>
			<a class="btn btn-sm">中文</a>
			<a class="btn btn-sm">English</a>
		</form>

	</body>

</html>
```

效果：根据浏览器语言信息切换国际化



原理：

​	国际化Locale（区域信息对象）；LocaleResolver(获取区域信息对象)

```java
@Bean
		@ConditionalOnMissingBean
		@ConditionalOnProperty(prefix = "spring.mvc", name = "locale")
		public LocaleResolver localeResolver() {
			if (this.mvcProperties.getLocaleResolver() == WebMvcProperties.LocaleResolver.FIXED) {
				return new FixedLocaleResolver(this.mvcProperties.getLocale());
			}
			AcceptHeaderLocaleResolver localeResolver = new AcceptHeaderLocaleResolver();
			localeResolver.setDefaultLocale(this.mvcProperties.getLocale());
			return localeResolver;
		}
// 默认的区域信息解析器，是根据请求头带来的区域信息获取的Locale国际化
```

4）点击链接切换国际化资源

创建一个自己的LocaleResolver类

```java
public class MyLocaleResolver implements LocaleResolver {
    //解析区域信息
    @Override
    public Locale resolveLocale(HttpServletRequest request) {
        String header = request.getParameter("l");
        Locale locale =Locale.getDefault();//获取浏览器默认
        if (!StringUtils.isEmpty(header)) {//如果参数不为空
            String[] s = header.split("_");//则将参数的语言代码与国家代码分割
             locale = new Locale(s[0], s[1]);//将对应的信息放入local中
        }
        return locale;
    }

    @Override
    public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {

    }
}
```

在Controller层实现

```java
@Bean
    public LocaleResolver localeResolver(){
        return  new MyLocaleResolver();
    }
```

### 3)、登录

开发期间模板引擎页面修改以后，要实时生效

1、禁用模板引擎

```properties
# 禁用缓存
spring.thymeleaf.cache=false
```

2、页面修改完成后ctrl+F9:重新编译

登录错误消息的显示

```html
<p style="color: red" th:text="${message}" th:if="${not #strings.isEmpty(message)}"></p>
```

### 4)、拦截器进行登录检查

1、编写自定义的拦截器

```java
//登录检查
public class LoginHandlerInterceptor implements HandlerInterceptor {
    //目标方法执行之前
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        Object loginUser = request.getSession().getAttribute("loginUser");
        if (loginUser == null) {
            //未登录，返回登录页面
            request.setAttribute("message","请先登录");
            request.getRequestDispatcher("/index.html").forward(request,response);
            return false;
        } else {
            //已登录
            return  true;
        }

    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {

    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

    }
}

```

2、调用拦截器，放行静态资源

```java
 @Override
          public void addInterceptors(InterceptorRegistry registry) {
              //SpringBoot已经做好了静态资源映射了
             registry.addInterceptor(new LoginHandlerInterceptor()).addPathPatterns("/**").excludePathPatterns("/","/index.html","/user/login","/asserts/**","/webjars/**");
          }
```

### 5)、CRUD-员工列表

实验要求

1、RestfunCRUD：CRUD满足Rest风格

URI：/资源名称/资源表示      HTTP请求方式区分对资源CRUD操作

|      | 普通CRUD（uri区分操作） | RestfulCRUD        |
| ---- | ----------------------- | ------------------ |
| 查询 | getEmp                  | emp----GET         |
| 添加 | addEmp?xxx              | emp----POST        |
| 修改 | updateEmp?xxx&xxx       | emp/{id}----PUT    |
| 删除 | deleteEmp?id=1          | emp/{id}----DELETE |

2、实验的请求架构

|                          | 请求URI  | 请求方式 |
| ------------------------ | -------- | -------- |
| 查询所有员工             | emps     | GET      |
| 查询单个员工             | emp/{id} | GET      |
| 来到添加页面             | emp      | GET      |
| 添加员工                 | emp      | POST     |
| 来到修改页面（数据回显） | emp/{id} | GET      |
| 修改员工                 | emp      | PUT      |
| 删除员工                 | emp/{id} | DELETE   |

### 6)、CRUD-员工添加

```html
<form th:action="@{/emp}" method="post">
						<div class="form-group">
							<label>LastName</label>
							<input type="text" name="lastName" class="form-control"  placeholder="zhangsan">
						</div>
						<div class="form-group">
							<label>Email</label>
							<input type="email" name="email" class="form-control" placeholder="zhangsan@atguigu.com">
						</div>
						<div class="form-group">
							<label>Gender</label><br>
							<div class="form-check form-check-inline">
								<input class="form-check-input" type="radio" name="gender" value="1">
								<label class="form-check-label">男</label>
							</div>
							<div class="form-check form-check-inline">
								<input class="form-check-input" type="radio" name="gender" value="0">
								<label class="form-check-label">女</label>
							</div>
						</div>
						<div class="form-group">
							<label>department</label>
							<select class="form-control" name="department.id">
								<option th:each="dept:${depts}" th:text="${dept.departmentName}" th:value="${dept.id}"></option>
							</select>
						</div>
						<div class="form-group">
							<label>Birth</label>
							<input name="birth" type="text" class="form-control" placeholder="zhangsan">
						</div>
						<button type="submit" class="btn btn-primary">添加</button>
					</form>
```

提教的数据格式不对：生日：日期

2019-12-12；2019/12/12；2017.12.12

日期的格式化；SpringMVC将页面提交的值需要转换为指定的类型

2019-12-12 ----Date ;需要类型转换、格式化的问题

默认日期是按照/的方式

修改默认时间格式

```properties
spring.mvc.date-format=yyyy-MM-dd HH:mm
```

## 7、错误处理机制

### 1、SpringBoot默认的错误处理机制

默认效果：

1、返回一个默认的错误页面

![image-20200114110030898](image-20200114110030898.png)

浏览器的请求头

![image-20200114112345655](image-20200114112345655.png)

2、如果是第三方客户端，默认响应json数据

![image-20200114110401919](image-20200114110401919.png)

第三方请求头

![image-20200114112431739](image-20200114112431739.png)



原理：参照ErrorMvcAutoConfiguration :错误处理自动配置

​	给容器中添加了以下组件

1. DefaultErrorAttributes

   ```java
   帮我们在页面共享错误信息
       public Map<String, Object> getErrorAttributes(WebRequest webRequest, boolean includeStackTrace) {
           Map<String, Object> errorAttributes = new LinkedHashMap();
           errorAttributes.put("timestamp", new Date());
           this.addStatus(errorAttributes, webRequest);
           this.addErrorDetails(errorAttributes, webRequest, includeStackTrace);
           this.addPath(errorAttributes, webRequest);
           return errorAttributes;
       }
   ```

   

2. BasicErrorController :处理默认/error请求

   ```java
   @Controller
   @RequestMapping("${server.error.path:${error.path:/error}}")
   public class BasicErrorController extends AbstractErrorController {			               @RequestMapping(produces = MediaType.TEXT_HTML_VALUE)
   	public ModelAndView errorHtml(HttpServletRequest request, HttpServletResponse response) {
   		HttpStatus status = getStatus(request);
   		Map<String, Object> model = Collections
   				.unmodifiableMap(getErrorAttributes(request, isIncludeStackTrace(request, MediaType.TEXT_HTML)));
   		response.setStatus(status.value());
           
           //去哪个页面作为错误页面，包含页面地址和页面内容
   		ModelAndView modelAndView = resolveErrorView(request, response, status, model);
   		return (modelAndView != null) ? modelAndView : new ModelAndView("error", model);
   	}
   
   	@RequestMapping  //返回json数据，第三方客户端来到这里
   	public ResponseEntity<Map<String, Object>> error(HttpServletRequest request) {
   		HttpStatus status = getStatus(request);
   		if (status == HttpStatus.NO_CONTENT) {
   			return new ResponseEntity<>(status);
   		}
   		Map<String, Object> body = getErrorAttributes(request, isIncludeStackTrace(request, MediaType.ALL));
   		return new ResponseEntity<>(body, status);
   	}}
   
   
   其中 MediaType.TEXT_HTML_VALUE
       public static final String TEXT_HTML_VALUE = "text/html";//产生html类型的数据 ，浏览器发送的请求来到这里处理
   ```

3. ErrorPageCustomizer 

   ```java
   @Value("${error.path:/error}")
   private String path = "/error";  //系统出现错误的以后了来到error请求进行处理
   ```

4. DefaultErrorViewResolver

   ```java
   	@Override
   	public ModelAndView resolveErrorView(HttpServletRequest request, HttpStatus status, Map<String, Object> model) {
   		ModelAndView modelAndView = resolve(String.valueOf(status.value()), model);
   		if (modelAndView == null && SERIES_VIEWS.containsKey(status.series())) {
   			modelAndView = resolve(SERIES_VIEWS.get(status.series()), model);
   		}
   		return modelAndView;
   	}
   
   	private ModelAndView resolve(String viewName, Map<String, Object> model) {
           //默认SpringBoot可以去找到某个页面   error/404或error/405
   		String errorViewName = "error/" + viewName;
           //模板引擎可以解析这个页面地址就用模板引擎解析
   		TemplateAvailabilityProvider provider = this.templateAvailabilityProviders.getProvider(errorViewName,
   				this.applicationContext);
   		if (provider != null) {
               //模板引擎可用的情况下就返回到errorViewName指定的视图地址
   			return new ModelAndView(errorViewName, model);
   		}
           //模板引擎不可用，就在静态资源文件夹下找errorViewName对应的页面
   		return resolveResource(errorViewName, model);
   	}
   ```

   

   步骤： 

   ​	一旦系统出现错误，**ErrorPageCustomizer**就会生效(定制错误响应规则) ；就会来到/error请求；就会被**BasicErrorController** 处理

   1、响应页面解析代码,去哪个页面是由**DefaultErrorViewResolver**解析得到的

   ```java
   	protected ModelAndView resolveErrorView(HttpServletRequest request, HttpServletResponse response, HttpStatus status,
   			Map<String, Object> model) {
           //得到所有的ErrorViewResolver
   		for (ErrorViewResolver resolver : this.errorViewResolvers) {
   			ModelAndView modelAndView = resolver.resolveErrorView(request, status, model);
   			if (modelAndView != null) {
   				return modelAndView;
   			}
   		}
   		return null;
   	}
   ```

   

如何定制统一错误消息

1. 如何定制错误页面

   1. 有**模板引擎**的情况下：**error/状态码**；将错误页面命名为错误状态码.html。放在模板引擎文件夹里的error文件夹下，发生此状态码的错误就会来到对应的页面。

      我们可以使用4xx和5xx作为错误页面的文件名来匹配这种类型的所有错误，精确有限(优先寻找精确的状态码.html)

      页面能获取的信息:

      ​	timestamp：时间戳

      ​	status：状态码

      ​	error：错误提示

      ​	exception：异常信息

      ​	message：错误消息

      ​	errors：jsr303数据校验相关

      ![image-20200114115735005](image-20200114115735005.png)

   2. 没用模板引擎，在静态资源文件夹下找

      ![image-20200114115833291](image-20200114115833291.png)

   3. 以上都没有错误页面，就是来到SpringBoot默认的错误页面

      ![image-20200114120144845](image-20200114120144845.png)

2. 如何定制错误的json数据

   1、自定义异常处理&返回定制json数据

   ```java
   @ControllerAdvice
   public class MyExceptionHandler {
       //浏览器返回的也是json
       @ResponseBody
       @ExceptionHandler(UserNotExistException.class)
       public Map<String,Object> map =new HashMap<>();
       map.put("code","user.notexist")
       map.put("message",e.getMessage());
       return map;
   }
   //没有自适应效果
   ```

   2、转发到/error进行自适应效果

   ```java
   @ControllerAdvice
   public String MyExceptionHandler {
       @ResponseBody
       @ExceptionHandler(UserNotExistException.class)
       public Map<String,Object> map =new HashMap<>();
       map.put("code","user.notexist")
       map.put("message",e.getMessage());
       //转发到/error
       return "forward:/error";
   }
   ```

## 8、配置嵌入式Servlet容器

![image-20200114162436978](image-20200114162436978.png)

SpringBoot默认使用的是Tomcat

### 1、如何定制和修改Servlet容器的相关配置

​	1、修改Server有关的配置（ServerProperties）

```properties
server.servlet.context-path=/crud
server.port=8081

server.tomcat.uri-encoding=Big5
//通用的Servlet容器设置
server.xxx
//Tomcat的设置
server.tomcat.xxx
```

 	2、编写一个WebServerFactoryCustomizer:嵌入式的Servlet容器定制器

```java
@Bean
public WebServerFactoryCustomizer<ConfigurableWebServerFactory> webServerFactoryCustomizer() {
     return  new WebServerFactoryCustomizer<ConfigurableWebServerFactory>() {
         @Override
         public void customize(ConfigurableWebServerFactory factory) {
             factory.setPort(8888);
         } 
     }  ;
    }
```

### 2、注册Servlet三大组件 [Servlet、Filter、Listener]

由于SpringBoot默认是以jar包的方式启动嵌入式的Servlet容器来启动SpringBoot的web应用，没有web.xml文件

注册三大组件用以下方式

ServletRegistrationBean

```java
@Bean
public ServletRegistrationBean myServlet() {
    ServletRegistrationBean registrationBean = new ServletRegistrationBean(new MyServlet(),"/myServlet");
    return registrationBean;
    }
```

FilterRegistrationBean

```java
@Bean
public FilterRegistrationBean myFilter() {
    FilterRegistrationBean registrationBean = new ServletRegistrationBean();
    registrationBean.setFilter(new MyFilter());
    registrationBean.setUrlPatterns(Arrays.asList("/hello","/myServlet"));
    return registrationBean;
    }
```

ServletListenerRegistrationBean

```java
@Bean
public ServletListenerRegistrationBean myFilter() {
    ServletListenerRegistrationBean<MyListener> registrationBean = new ServletRegistrationBean<>(new MyListener);
    return registrationBean;
    }
```

SpringBoot帮我们自动配置SpringMVC的时候，自动的注册了SpringMVC的前端控制器 ；DispatcherServlet;

```java
 public DispatcherServletRegistrationBean dispatcherServletRegistration(DispatcherServlet dispatcherServlet, WebMvcProperties webMvcProperties, ObjectProvider<MultipartConfigElement> multipartConfig) {
DispatcherServletRegistrationBean registration = new DispatcherServletRegistrationBean(dispatcherServlet, webMvcProperties.getServlet().getPath());
            registration.setName("dispatcherServlet");
            registration.setLoadOnStartup(webMvcProperties.getServlet().getLoadOnStartup());
            multipartConfig.ifAvailable(registration::setMultipartConfig);
            return registration;
        }
```



### 3、SpringBoot能不能支持其他的Servlet容器

![image-20200114180317322](image-20200114180317322.png)

![image-20200114180357394](image-20200114180357394.png)

默认支持

Tomcat(默认)   Jetty Undertow

切换其他Servlet容器方法与切换日志一样，先排除再引入

### 4、嵌入式Servlet容器自动配置原理

```java
@Configuration(proxyBeanMethods = false)
@ConditionalOnWebApplication
@EnableConfigurationProperties(ServerProperties.class)
public class EmbeddedWebServerFactoryCustomizerAutoConfiguration {

	/**
	 * Nested configuration if Tomcat is being used.
	 */
	@Configuration(proxyBeanMethods = false)
	@ConditionalOnClass({ Tomcat.class, UpgradeProtocol.class })
	public static class TomcatWebServerFactoryCustomizerConfiguration {

		@Bean
		public TomcatWebServerFactoryCustomizer tomcatWebServerFactoryCustomizer(Environment environment,
				ServerProperties serverProperties) {
			return new TomcatWebServerFactoryCustomizer(environment, serverProperties);
		}

	}

	/**
	 * Nested configuration if Jetty is being used.
	 */
	@Configuration(proxyBeanMethods = false)
	@ConditionalOnClass({ Server.class, Loader.class, WebAppContext.class })
	public static class JettyWebServerFactoryCustomizerConfiguration {

		@Bean
		public JettyWebServerFactoryCustomizer jettyWebServerFactoryCustomizer(Environment environment,
				ServerProperties serverProperties) {
			return new JettyWebServerFactoryCustomizer(environment, serverProperties);
		}

	}

	/**
	 * Nested configuration if Undertow is being used.
	 */
	@Configuration(proxyBeanMethods = false)
	@ConditionalOnClass({ Undertow.class, SslClientAuthMode.class })
	public static class UndertowWebServerFactoryCustomizerConfiguration {

		@Bean
		public UndertowWebServerFactoryCustomizer undertowWebServerFactoryCustomizer(Environment environment,
				ServerProperties serverProperties) {
			return new UndertowWebServerFactoryCustomizer(environment, serverProperties);
		}

	}

	/**
	 * Nested configuration if Netty is being used.
	 */
	@Configuration(proxyBeanMethods = false)
	@ConditionalOnClass(HttpServer.class)
	public static class NettyWebServerFactoryCustomizerConfiguration {

		@Bean
		public NettyWebServerFactoryCustomizer nettyWebServerFactoryCustomizer(Environment environment,
				ServerProperties serverProperties) {
			return new NettyWebServerFactoryCustomizer(environment, serverProperties);
		}

	}

}

```

EmbeddedWebServerFactoryCustomizerAutoConfiguration：嵌入式的Servlet自动配置类

## 9、配置外置的Servlet容器

嵌入式Servlet容器：jar

​	优点 ：简单、便捷

​	缺点：默认不支持jsp、优化定制比较复杂 （使用定制器[ServerProperties、自定义EmbeddedServletContainerCustomizer]，自己编写嵌入式Servlet容器的创建工厂[EmbeddedServletContainerFactory]）

外置的Servlet容器 ：外面安装Tomcat----应用war包的方式打包

1、创建war项目

2、将嵌入式的Tomcat指定为provided

```xml
<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
      <scope>provided</scope>
    </dependency>
```

3、必须编写一个SpringBootServletInitializer的子类，并调用configure方法

```java
public  class ServletInitializer extends SpringBootServletInitializer {
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application){
        //传入SpringBoot应用的主程序
        return application.sources(SpringBoot04WebJspApplication.class);
    }
}
```

4、启动服务器即可

**原理**

​	jar包 ：执行SpringBoot的主类的main方法，启动ioc容器，创建嵌入式的Servlet容器

​	war包 ：启动服务器，**服务器启动SpringBoot应用**，启动ioc容器

Servlet3.0定义了一个规则

​			1、服务器启动（web应用启动）会创建当前web应用里每一个jar包里面ServletContainerInitializer实例

​			2、ServletContainerInitializer的实现放在jar包的META-INF/services文件夹下，有一个名为 javax.servlet.ServletContainerInitializer的文件，内容就是ServletContainerInitializer的全类名

​			3、还可以使用@HandlesTypes，在应用启动的时候加载我们感兴趣的类。

流畅：

1、启动Tomcat

2、D:\tool\Apache\apache-maven-3.6.1\mavenRepo\org\springframework\spring-web\5.2.2.RELEASE\spring-web-5.2.2.RELEASE.jar!\META-INF\services\javax.servlet.ServletContainerInitializer ：

Spring的web模块里面有这个文件 ：**org.springframework.web.SpringServletContainerInitializer**

3、SpringServletContainerInitializer将@HandlesTypes(WebApplicationInitializer.class)标注的所有这个类型的类都传入到onStartup方法的Set<Class<?>>；为WebApplicationInitializer类型的类创建实例

4、每一个WebApplicationInitializer都调用自己的onStartup方法

![image-20200114190607394](image-20200114190607394.png)

5、相对于我们的SpringBootServletInitializer的类会被创建对象，并执行onStartup方法

6、SpringBootServletInitializer实例执行onStartup的时候会创建createRootApplicationContext

；创建容器

```java
protected WebApplicationContext createRootApplicationContext(ServletContext servletContext) {
        SpringApplicationBuilder builder = this.createSpringApplicationBuilder();
        builder.main(this.getClass());
        ApplicationContext parent = this.getExistingRootWebApplicationContext(servletContext);
        if (parent != null) {
            this.logger.info("Root context already created (using as parent).");
            servletContext.setAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE, (Object)null);
            builder.initializers(new ApplicationContextInitializer[]{new ParentContextApplicationContextInitializer(parent)});
        }

        builder.initializers(new ApplicationContextInitializer[]{new ServletContextApplicationContextInitializer(servletContext)});
        builder.contextClass(AnnotationConfigServletWebServerApplicationContext.class);

    //调用configure方法，子类重写了这个方法，将Springboot的主程序类传入了进来
        builder = this.configure(builder);
        builder.listeners(new ApplicationListener[]{new SpringBootServletInitializer.WebEnvironmentPropertySourceInitializer(servletContext)});
        SpringApplication application = builder.build();
        if (application.getAllSources().isEmpty() && MergedAnnotations.from(this.getClass(), SearchStrategy.TYPE_HIERARCHY).isPresent(Configuration.class)) {
            application.addPrimarySources(Collections.singleton(this.getClass()));
        }

        Assert.state(!application.getAllSources().isEmpty(), "No SpringApplication sources have been defined. Either override the configure method or add an @Configuration annotation");
        if (this.registerErrorPageFilter) {
            application.addPrimarySources(Collections.singleton(ErrorPageFilterConfiguration.class));
        }
	//启动Spring
        return this.run(application);
    }
```

7、Spring的应用就启动了并创建了IOC容器

```java
public ConfigurableApplicationContext run(String... args) {
        StopWatch stopWatch = new StopWatch();
        stopWatch.start();
        ConfigurableApplicationContext context = null;
        Collection<SpringBootExceptionReporter> exceptionReporters = new ArrayList();
        this.configureHeadlessProperty();
        SpringApplicationRunListeners listeners = this.getRunListeners(args);
        listeners.starting();

        Collection exceptionReporters;
        try {
            ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
            ConfigurableEnvironment environment = this.prepareEnvironment(listeners, applicationArguments);
            this.configureIgnoreBeanInfo(environment);
            Banner printedBanner = this.printBanner(environment);
            context = this.createApplicationContext();
            exceptionReporters = this.getSpringFactoriesInstances(SpringBootExceptionReporter.class, new Class[]{ConfigurableApplicationContext.class}, context);
            this.prepareContext(context, environment, listeners, applicationArguments, printedBanner);
            //刷新ioc容器
            this.refreshContext(context);
            this.afterRefresh(context, applicationArguments);
            stopWatch.stop();
            if (this.logStartupInfo) {
                (new StartupInfoLogger(this.mainApplicationClass)).logStarted(this.getApplicationLog(), stopWatch);
            }

            listeners.started(context);
            this.callRunners(context, applicationArguments);
        } catch (Throwable var10) {
            this.handleRunFailure(context, var10, exceptionReporters, listeners);
            throw new IllegalStateException(var10);
        }

        try {
            listeners.running(context);
            return context;
        } catch (Throwable var9) {
            this.handleRunFailure(context, var9, exceptionReporters, (SpringApplicationRunListeners)null);
            throw new IllegalStateException(var9);
        }
    }
```

启动Servlet容器，再启动SpringBoot应用

# 五、Docker

## 1、简介

**Docker**是一个开源的应用容器引擎

Docker支持将软件编译成一个镜像；然后在镜像中各种软件做好配置，将镜像发布出去，其他使用者开源直接使用这个镜像，运行中的镜像成为容器，容器启动是非常快速的。

## 2、核心概念

docker主机（Host）:安装了Docker程序的机器（直接安装在操作系统之上的）

docker客户端（client）：连接Docker主机进行操作

docker仓库（Registery）:Docker仓库用来保存镜像。用来保存各种打包好的软件镜像

docker镜像（images）：软件打包好的镜像；放在docker仓库中。

docker容器：镜像启动后的实例，称为一个容器，容器是独立运行的一个或一组应用

步骤

1. 安装docker
2. 去docker仓库找到这个软件的镜像
3. 使用docker运行这个镜像，这个镜像就会生成一个docker容器
4. 对容器的启动和停止就是对这个软件的启动和停止

## 3、安装Docker

#### 1、安装Linux系统

#### 2、在Linux上安装docker

步骤：

```shell
1、检查内核版本，必须是在3.10及以上
uname -r
2、安装docker
yum install docker
3、输入y确认安装
4、出现complete就成功了，启动docker
[root@izuf6dtg6bbw1scjvtwpjuz ~]# systemctl start docker
[root@izuf6dtg6bbw1scjvtwpjuz ~]# docker -v
Docker version 1.13.1, build 7f2769b/1.13.1
5、设置开机自启docker
[root@izuf6dtg6bbw1scjvtwpjuz ~]# systemctl enable docker
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
6、停止docker
systemctl stop docker
```

## 4、Docker常用命令&操作

### 1、镜像操作

| 操作 | 命令                                         | 说明                                                     |
| ---- | -------------------------------------------- | -------------------------------------------------------- |
| 检索 | docker search 关键字   如docker search mysql | 去docker hub 上检索镜像的详细详细，如镜像的TAG           |
| 拉取 | docker pull 镜像名：tag                      | :tag 是可选的，tag表示标签，多为软件的版本，默认是latest |
| 列表 | docker images                                | 查看所有本地镜像                                         |
| 删除 | docker rmi image-id                          | 删除指定的本地镜像                                       |

### 2、容器操作

软件镜像(QQ.ext)---运行镜像---产生一个容器(正在运行的软件，运行的qq)

步骤：

```shell
1、搜索镜像
docker search tomcat
2、下载镜像
docker pull tomcat
3、根据镜像，启动容器
docker run --name=mytomcat -d tomcat:latest
4、查看运行中的容器
docker ps
5、停止运行中的容器
docker stop xxxx
6、查看所有的容器
docker ps -a
7、启动容器
docker start xxxx
8、删除容器
docker rm xxxx
9、做了端口映射的tomcat
docker run -d -p 8080:8080 --name=mytomcat tomcat:latest
-d :后台运行
-p :将主机的端口映射到容器的端口
```

| 操作     | 命令                                                         | 说明                                                       |
| -------- | ------------------------------------------------------------ | ---------------------------------------------------------- |
| 运行     | docker run --name container-name -d image-name   如：docker run --name myredis -d redis | --name:自定义容器名，-d：后台运行，image-name:指定镜像模板 |
| 列表     | docker ps (查看运行中的容器)                                 | -a 查看所有容器                                            |
| 停止     | docker stop 容器名/容器id                                    | 停止当前指定运行的容器                                     |
| 启动     | docker start 容器名/容器id                                   | 启动容器                                                   |
| 端口映射 | -p 6379:6379   如：docker run -d -p 6379:6379 --name myreids docker.io/redis | -p：主机端口（映射到）容器内部的端口                       |
| 容器日志 | docker logs 容器名/容器id                                    |                                                            |
| 更多命令 | https://docs.docker.com/engine/reference/commandline/docker/ |                                                            |
| 删除     | docker rm id                                                 |                                                            |

**注**如果出现错误

```shell
Error response from daemon: oci runtime error: container_linux.go:235: starting container process …………
```

yum update 更新系统，原因为docker与系统不兼容

### 3、安装mysql

错误启动

```shell
[root@izuf6dtg6bbw1scjvtwpjuz /]# docker run --name mysql -d mysql:latest
5d7b3a0c2936b766e2e0cddbb9f336433cc8b62a9a39af75bc71755cb5adb44a

报错
2020-01-16 08:05:18+00:00 [ERROR] [Entrypoint]: Database is uninitialized and password option is not specified
	You need to specify one of MYSQL_ROOT_PASSWORD, MYSQL_ALLOW_EMPTY_PASSWORD and MYSQL_RANDOM_ROOT_PASSWORD
```

正确启动

```shell
[root@izuf6dtg6bbw1scjvtwpjuz /]# docker run --name mysql -e MYSQL_ROOT_PASSWORD=998877 -d mysql
528b843166eebb3692da6c06cca4c958a5d7da49007d5f189164104372c99022
```

但是没开端口映射、

```shell
[root@izuf6dtg6bbw1scjvtwpjuz /]# docker run -p 3306:3306  --name mysql -e MYSQL_ROOT_PASSWORD=998877 -d mysql
4186ba47f843525a5c5e22fc6738d07e482e55281a4ddd72c3595f2b3451675d
```

### 4、安装rabbitmq

```shell
docker run -d --hostname my-rabbit --name rabbit -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin -p 15672:15672 -p 5672:5672 -p 25672:25672 -p 61613:61613 -p 1883:1883 rabbitmq:management
```

https://blog.csdn.net/weixin_39617052/article/details/79723849

# 六、SpringBoot数据访问

## 1、JDBC

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
</dependency>
```

```yml
spring:
  datasource:
    username: root
    password: 998877
    url: jdbc:mysql://101.132.235.200:3306/jdbc
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.mysql.cj.jdbc.MysqlConnectionPoolDataSource // 修改默认数据源
```

效果：

​	2.x版本以后默认使用com.zaxxer.hikari.HikariDataSource为数据源

​	数据源的相关的配置都在DataSourceProperties里面；

自动配置原理

org.springframework.boot.autoconfigure.jdbc

1、参考DataSourceConfiguration，根据配置创建数据源，默认使用Hikari连接池；可以使用spring.datasource.type自定义的数据源类型

2、SpringBoot默认可以支持

```java
org.apache.tomcat.jdbc.pool.DataSource
HikariDataSource
org.apache.commons.dbcp2.BasicDataSource
```

3、自定义数据源类型

```java
	/**
	 * Generic DataSource configuration.
	 */
	@Configuration(proxyBeanMethods = false)
	@ConditionalOnMissingBean(DataSource.class)
	@ConditionalOnProperty(name = "spring.datasource.type")
	static class Generic {

		@Bean
		DataSource dataSource(DataSourceProperties properties) {
            //使用DataSourceBuilder创建数据源，利用反射创建响应type的数据源，并且绑定相关属性
			return properties.initializeDataSourceBuilder().build();
		}

	}
```

4、DataSourceInitializerInvoker ：ApplicationListener

​	作用：

```java
/**
 * Bean to handle {@link DataSource} initialization by running {@literal schema-*.sql} on
 * {@link InitializingBean#afterPropertiesSet()} and {@literal data-*.sql} SQL scripts on
 * a {@link DataSourceSchemaCreatedEvent}.
 *
 * @author Stephane Nicoll
 * @see DataSourceAutoConfiguration
 */
class DataSourceInitializerInvoker implements ApplicationListener<DataSourceSchemaCreatedEvent>, InitializingBean {
```

默认只需要将文件命名为：

```properties
schema-*.sql \ data-*.sql
默认规则:schema.sql

可以使用
schema:
    -classpath: department.sql
    initialization-mode: always
    指定位置
```

5、操作数据库：自动配置了jdbcTemplate

```java
@Controller
public class HelloController {
    @Autowired
    JdbcTemplate jdbcTemplate;
    @GetMapping("/query")
    @ResponseBody
    public Map<String, Object> map() {
        List<Map<String, Object>> maps = jdbcTemplate.queryForList("select * from department");
        return maps.get(0);
    }
}
```

## 2、整合Druid数据源

```java
导入druid数据源即可
@Configuration
public class DruidConfig {
    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druid() {
        return  new DruidDataSource();
    }

    //配置Druid监控
    //1、配置一个管理后台的Servlet
    @Bean
    public ServletRegistrationBean servletRegistrationBean() {
        ServletRegistrationBean bean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");
        Map<String,String> initParm  =new HashMap<>();
        initParm.put("loginUsername","admin");
        initParm.put("loginPassword","111111");
        initParm.put("allow","");//不写则默认允许所有
        bean.setInitParameters(initParm);
        return bean;
    }

    //2、配置一个监控filter
    @Bean
    public FilterRegistrationBean filterRegistrationBean() {
        FilterRegistrationBean<WebStatFilter> bean = new FilterRegistrationBean();
        bean.setFilter(new WebStatFilter());
        Map<String,String> initParm  =new HashMap<>();
        initParm.put("exclusions","*.js,*.css,/druid/*");
        bean.setInitParameters(initParm);
        bean.setUrlPatterns(Arrays.asList("/*"));
        return bean;
    }
}
```

```yaml
spring:
  datasource:
    #   数据源基本配置
    username: root
    password: 998877
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://101.132.235.200:3306/jdbc
    type: com.alibaba.druid.pool.DruidDataSource
    #   数据源其他配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true
    #   配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
    filters: stat,wall,slf4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
```

## 3、整合MyBatis

```xml
		<dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.1</version>
        </dependency>
```

![image-20200118141033447](image-20200118141033447.png)

步骤：

	1. 配置数据源相关属性
 	2. 给数据库建表
 	3. 创建javaBean

### 1、注解版

```java
//指定这是一个操作数据库的mapper
@Mapper
public interface DepartmentMapper {
    @Select("select * from department where id=#{id}")
    public Department getDeptById(Integer id);

    @Delete("delete from department where id=#{id}")
    public int  deleteDeptById(Integer id);

    @Options(useGeneratedKeys = true,keyProperty = "id")
    @Insert("insert into department (departmentName) values (#{departmentName})")
    public int insertDept(Department department);

    @Update("update department set departmentName=#{departmentName} where id=#{id}")
    public int updateDept(Department department);
}
```

问题：如果出现数据库驼峰命名则需要自定义MyBaits的配置规则

```java
@Configuration
public class MyBatisConfig {
    @Bean
    public ConfigurationCustomizer configurationCustomizer() {

        return new ConfigurationCustomizer() {

            @Override
            public void customize(org.apache.ibatis.session.Configuration configuration) {
                configuration.setMapUnderscoreToCamelCase(true);
            }
        };
    }
}
```

如果觉得给每一个mapper下的类都添加@Mapper注解麻烦的话，可以给SpringBoot主程序添加@MapperScan注解，相当于给其下所有的类添加了@Mapper注解------------------------批量扫描

```java
@MapperScan(value = "com.study.springboot.mapper")
@SpringBootApplication
public class SpringBoot06DataMybatisApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBoot06DataMybatisApplication.class, args);
    }
}
```

### 2、配置文件版

```yaml
mybatis:
  config-location: classpath:MyBatis/mybatis-config.xml  # 指定全局配置文件位置
  mapper-locations: classpath:MyBatis/mapper/*.xml  # 指定sql映射文件位置
```

## 4、整合SpringData Jpa

1、编写一个实体类和数据表进行关系映射

```java
//使用jpa注解配置映射关系
@Entity  //告诉jpa这是一个实体类（和数据表映射的类）
@Table (name = "tbl_user") //@Table 来指定和那个数据表对应；如果省略则默认表明就是user
public class User {
    @Id //这是一个主键
    @GeneratedValue(strategy = GenerationType.IDENTITY)  //自增主键
    private Integer id;
    @Column(name="last_name",length = 50)//这是和数据表对应的一个列     省略的情况下默认列名就是属性名
    private String username;
    private String email;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
```

2、编写一个Dao接口操作实体类对应的数据表（Repository)

```java
//继承JpaRepository来完成对数据库的操作
public interface UserRepository extends JpaRepository<User,Integer> {
}
```

3、基本的配置

```yaml
spring:
  datasource:
    #   数据源基本配置
    username: root
    password: 998877
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://101.132.235.200:3306/jpa?useUnicode=true&amp;characterEncoding=UTF-8
    type: com.mysql.cj.jdbc.MysqlConnectionPoolDataSource
  jpa:
    hibernate:
      ddl-auto: update #更新或者创建数据表结构
    show-sql: true # 在控制台显示sql
```

# 7、SpringBoot缓存

**缓存注解**

| 注解           | 解释                                                         |
| -------------- | ------------------------------------------------------------ |
| Cache          | 缓存接口，定义缓存操作。实现由：RedisCache、EhCacheCache、ConcurrentMapCache等 |
| CacheManager   | 缓存管理器，管理各种缓存(Cache)组件                          |
| @Cacheable     | 主要针对方法配置，能够根据方法的请求参数对其结果进行缓存     |
| @CacheEvict    | 清空缓存                                                     |
| @CachePut      | 保证方法被调用，又希望结果被缓存                             |
| keyGenerator   | 缓存数据时key生成策略                                        |
| serialize      | 缓存数据时value序列化策略                                    |
| @EnableCaching | 开起基于注解的缓存                                           |

```java
@Service
public class EmployeeServiceImpl implements EmployeeService {
    @Autowired
    EmployeeMapper mapper;
/*将方法的运行结果进行缓存
    几个属性:
    cacheNames/value:指定缓存组件的名字
    key:缓存数据使用的key:可以用它来指定，默认是使用方法参数的值，可以编写SpEL,#id：参数id的值   或#root.args[0]
    keyGenerator: key的生成器；可以自己指定key的生成器的组件id    key与keyGenerator二选一
    cacheManager:指定缓存管理器
    cacheResolver:指定缓存解析器         cacheManager与cacheResolver二选一
    condition:指定符合条件的情况下才缓存
    unless:否定缓存，当unless指定的条件为true,方法的返回值就不会被缓存，可以获取到结果进行判断
         例如：  condition = "#id>0",unless = "#result ==null" ：如果传参id>0就进行缓存，如果结果等于空值，则不进行缓存
    sync:指定缓存是否使用异步

     原理:
       1、自动配置类 CacheAutoConfiguration
       2、缓存配置的类
       0 = "org.springframework.boot.autoconfigure.cache.GenericCacheConfiguration"
       1 = "org.springframework.boot.autoconfigure.cache.JCacheCacheConfiguration"
       7 = "org.springframework.boot.autoconfigure.cache.CaffeineCacheConfiguration"
       5 = "org.springframework.boot.autoconfigure.cache.CouchbaseCacheConfiguration"
       3 = "org.springframework.boot.autoconfigure.cache.HazelcastCacheConfiguration"
       2 = "org.springframework.boot.autoconfigure.cache.EhCacheCacheConfiguration"
       8 = "org.springframework.boot.autoconfigure.cache.SimpleCacheConfiguration"
       9 = "org.springframework.boot.autoconfigure.cache.NoOpCacheConfiguration"
       4 = "org.springframework.boot.autoconfigure.cache.InfinispanCacheConfiguration"
       6 = "org.springframework.boot.autoconfigure.cache.RedisCacheConfiguration"
       3、查看那个配置类生效
        默认：SimpleCacheConfiguration   给容器中注入了ConcurrentMapCacheManager
       4、给容器中注册了一个CacheManager,     ConcurrentMapCacheManager
       5、可以获取和创建ConcurrentMapCache类型的缓存组件；他的作用将数据保存在ConcurrentMap中

     流程：
      @Cacheable：
      1、方法执行之前，先去查Cache（缓存组件），按照cacheNames指定的名字获取；
      (CacheManager先获取相应的缓存)，第一次获取缓存如果没有Cache组件会自动创建
      2、去Cache中查找缓存的内容，使用一个key,默认就是方法的参数
      key是按照自带的策略生成的，默认是使用keyGenerator生成的，默认使用SimpleKeyGenerator生成key
          SimpleKeyGenerator生成的默认策略
               如果没有参数：key = new SimpleKey();
               如果有一个参数：key =参数的值
               如果有多个参数： key=new SimpleKey(params)
      3、没有查到缓存就调用目标方法
      4、将目标方法返回的结果，放进缓存中
      5、以后再来调用，就直接用缓存中的数据

		核心：
         1、使用CacheManage按照名字得到Cache组件 ，默认是用ConcurrentMapCacheManager,Cache得到的就是ConcurrentMap保存的数据
         2、key使用keyGenerator生成的，默认是SimpleKeyGenerator
     */
@Override
@Cacheable(cacheNames = "emp",key = "#id",unless = "#result ==null")
public BaseModel getEmpById(Integer id, BaseModel baseModel) throws Exception {
```

| @Cacheable的属性 |                                                              |
| ---------------- | ------------------------------------------------------------ |
| cacheNames       | 指定缓存组件的名字                                           |
| value            | 指定缓存组件的名字                                           |
| key              | 缓存数据使用的key，默认是使用方法参数的值，可以使用SpEl表达式 |
| keyGenerator     | key的生成器，可以自己指定key的生成器的组件id ，与key属性相互冲突 |
| cacheManager     | 指定缓存管理器                                               |
| cacheResolver    | 指定缓存解析器   ，与cacheManager相互冲突                    |
| condition        | 指定符合条件的情况下才缓存                                   |
| unless           | 否定缓存，当unless指定的条件为true,方法的返回值就不会被缓存，可以获取到结果进行判断 |
| sync             | 指定缓存是否使用异步                                         |

| @CachePut   | 流程：先调用目标方法，再讲目标方法的结果存入缓存中           |
| ----------- | ------------------------------------------------------------ |
| @CacheEvict | key:指定要清除的数据     allEntries =true 指定清除这个缓存中所有的数据    beforeInvocation = false ；缓存的清除是否在方法之前执行，默认代表是在方法执行之后执行 |

```java
    /*
    测试步骤：
        1、查询1号员工，将结果缓存起来
        2、更新员工信息
        3、出现问题，查询出来的是没更新前的信息，为什么会这样呢？
            解：因为缓存中是以key：value的形式保存的。
                而查询第一次的时候是以key：1存入缓存中的
                但是更新后的key是默认的参数，及更新传入的整个参数，employee对象
              故，只要统一key 就行
               key="#e.id"
               key="#result.id"
               注：@Cacheable是不能用#reuslt的，因为他要查之前在缓存中找一次
     */
    @CachePut(value = "emp",key="#e.id")
    @Override
    public BaseModel updateEmp(Employee e, BaseModel baseModel) throws Exception {
        Integer result=mapper.updateEmp(e);
        if (result > 0) {
            System.out.println("修改成功");
            baseModel.setResultCode(0);
            baseModel.setMessage("修改成功");
        } else {
            baseModel.setResultCode(1);
            baseModel.setMessage("修改失败");
        }
        return baseModel;
    }
```

**定义复杂的缓存规则**

```java
    @Caching(
            cacheable = {
                    @Cacheable(value = "emp",key="#lastName")
            },
            put ={
                    @CachePut(value = "emp",key = "#result.id"),
                    @CachePut(value = "emp",key = "#result.email")
            }
    )
    public Employee getEmpByLastName(String lastName){
        return mapper.getEmpByLastName(lastName);
    }
```

**@CacheConfig(cacheNames = "emp")在全局定义缓存名字**

## 缓存支持的SpEl表达式

| 名字          | 位置               | 描述                                                         | 示例                 |
| ------------- | ------------------ | ------------------------------------------------------------ | -------------------- |
| methodName    | root object        | 当前被调用的方法方法名                                       | #root.methodName     |
| method        | root object        | 当前被调用的方法                                             | #root.method.name    |
| target        | root object        | 当前被调用的目标对象                                         | #root.target         |
| targetClass   | root object        | 当前被调用的目标对象类                                       | #root.targetClass    |
| args          | root object        | 当前被调用的方法的参数列表                                   | #root.args[0]        |
| caches        | root object        | 当前方法调用使用的缓存列表（@Cacheable(value = {"v1","v2"})，则有两个cahce） | #root.caches[0].name |
| argument name | evaluation context | 方法参数的名字，可以直接 #参数名，也可以使用#p0或#a0的形式，0代表参数的索引 | #iban、#a0、#p0      |
| rusult        | evaluation context | 方法执行后的返回值（仅当方法执行之后的判断有效，若unless,cache put的表达式，cache evict的表达式，beforelnvocation=false） | #result              |

例子：

```java
key = "#root.methodName+'['+#id+']'" //getEmpById[xxxx]  代表根路径下的方法名的参数
condition = "#id>1" //id大于1的时候缓存 多条件用and连接
```

自定义keyGenerator

```java
@Configuration
public class MyCacheConfig {
    @Bean("myKeyGenerator")
    public KeyGenerator keyGenerator() {
      return   new KeyGenerator(){

            @Override
            public Object generate(Object o, Method method, Object... objects) {
                return method.getName()+"["+ Arrays.asList(objects).toString() +"]";
            }
        };
    }
}

//在使用的时候输入自定义的id就行
 @Cacheable(cacheNames = "emp",unless = "#result ==null",keyGenerator = "myKeyGenerator")
```

## 整合Redis

1、安装Redis 使用Dokcer

2、引入Reids的starter

3、配置Redis

```yaml
spring:
  datasource:
    username: root
    password: 998877
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://101.132.235.200:3306/spring_cache
  redis:
    host: 101.132.235.200
    password: 998877
```

```java
@Autowired
StringRedisTemplate stringRedisTemplate //操作字符串
@Autowired
RedisTemplate redisTemplate //key-value都是对象
@Test
    public void test01() {
        //给redis 添加
        stringRedisTemplate.opsForValue().append("meg","hello");
        String mes=stringRedisTemplate.opsForValue().get("meg");
        System.out.println(mes);
    }
    @Test
    public void test02() {
        //给redis 添加
        Employee employee =mapper.getEmpById(1);
        //默认保存对象，使用jdk序列化机制，序列化之后的数据保存到redis中
        // redisTemplate.opsForValue().set("emp-01",employee);
        //将数据已json的方式保存  1、自己将对象转为json 2、redisTemplate默认的序列化规则 改变默认的序列化规则
        empredisTemplate.opsForValue().set("emp-01",employee);
    }
```

原理：

	1. 引入redis的starter,容器中保存的是RedisCacheManager
 	2. RedisCacheManager帮我们创建RedisCache来作为缓存组件，RedisCache通过操作redis缓存数据
 	3. 默认保存数据k-v都是Object，利用序列化保存，那么如何保存json呢？
      	1. 默认创建的RedisManager操作redis的时候是RedisTemplate<Object,Object>
      	2. RedisTemplate<Object,Object>是默认使用的jdk的序列化机制

4. 自定义CacheManager

## 分层

```xml
      <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
            <!-- 1.5的版本默认采用的连接池技术是jedis，2.0以上版本默认连接池是lettuce, 因为此次是采用jedis，所以需要排除lettuce的jar -->
            <exclusions>
                <exclusion>
                    <groupId>redis.clients</groupId>
                    <artifactId>jedis</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>io.lettuce</groupId>
                    <artifactId>lettuce-core</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <!-- jedis客户端 -->
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
        </dependency>

        <!-- spring2.X集成redis所需common-pool2，使用jedis必须依赖它-->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-pool2</artifactId>
            <version>2.5.0</version>
        </dependency>
```

#### Controller

```java
@RestController
public class DeptController {
    @Autowired
    DeptService service;
    @GetMapping("/dept/{id}")
    public Department gerDept(@PathVariable("id")Integer id) {
       return service.getDeptById(id);
    }
}
```

#### mapper

```java
@Component
@Mapper
public interface DepartmentMapper {
    @Select("select * from department where id = #{id}")
    Department getDeptById(Integer id);
}
```

#### Service

```java
@Service
public class DeptService {
    @Autowired
    DepartmentMapper mapper;

    @Autowired
    CacheManager cacheManager;
    
    public Department getDeptById(Integer id) {
        System.out.println("查询部门"+id);
        Department department=mapper.getDeptById(id);
        Cache dept =cacheManager.getCache("dept");
        dept.put("1",department);
        return department;
    }
}
//在程序中使用缓存，如果用注解，与之前的相同
```

#### 自定义RedisTemplate 

```java
package com.study.cache.config;

import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.study.cache.bean.Employee;
import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.cache.CacheManager;
import org.springframework.cache.annotation.CachingConfigurerSupport;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.cache.RedisCacheConfiguration;
import org.springframework.data.redis.cache.RedisCacheManager;
import org.springframework.data.redis.cache.RedisCacheWriter;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.RedisSerializationContext;
import org.springframework.data.redis.serializer.RedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

import java.net.UnknownHostException;
import java.time.Duration;

@Configuration
public class MyRedisConfig extends CachingConfigurerSupport {

    @Bean
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<Object, Object> template = new RedisTemplate<>();
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        template.setConnectionFactory(factory);
        //key序列化方式
        template.setKeySerializer(redisSerializer);
        //value序列化
        template.setValueSerializer(jackson2JsonRedisSerializer);
        //value hashmap序列化
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        return template;
    }

    @Bean
    public CacheManager cacheManager(RedisConnectionFactory factory) {
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        //解决查询缓存转换异常的问题
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);

        // 配置序列化（解决乱码的问题）,过期时间30秒
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofDays(1))
                .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(redisSerializer))
                .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(jackson2JsonRedisSerializer))
                .disableCachingNullValues();
        RedisCacheManager cacheManager = RedisCacheManager.builder(factory)
                .cacheDefaults(config)
                .build();
        return cacheManager;
    }
}

```

