# IOC

IOC 的两种实现方式是**依赖注入 DI** 和 **依赖查找 DL**。

## 依赖查找

依赖查找获取了==类的实例==，如果这个类不需要使用内部的属性的话（可以获得方法），到这一步已经可以了。

### 基础查找

####   `getBean()`

- 使用`BeanFactory`接口调用`getBean()`方法，传入指定的类名称

  ```java
  public static void main(String[] args) {
          BeanFactory factory = new ClassPathXmlApplicationContext("basic_dl/quickstart_bytype.xml");
          Person person = factory.getBean(Person.class);
          System.out.println(person);
  
          DemoDao demoDao = factory.getBean(DemoDao.class);
          System.out.println(demoDao.findAll());
      }
  ```

#### `ofType`

- 传入一个接口 / 抽象类，返回容器中所有的实现类 / 子类

- 将 `BeanFactory` 接口换为 `ApplicationContext`

  ```java
   public static void main(String[] args) {
          ApplicationContext  ctx = new ClassPathXmlApplicationContext("basic_dl/oftype.xml");
          Map<String, DemoDao> beans = ctx.getBeansOfType(DemoDao.class);
          beans.forEach((beanName,bean)->{
              System.out.println(beanName+": "+bean.toString());
          });
      }
  ```

- `BeanFactory`与`ApplicationContext`

  ```markdown
  The org.springframework.beans and org.springframework.context packages are the basis for Spring Framework’s IoC container. The BeanFactory interface provides an advanced configuration mechanism capable of managing any type of object. ApplicationContext is a sub-interface of BeanFactory. It adds:
  
  - Easier integration with Spring’s AOP features
  - Message resource handling (for use in internationalization)
  - Event publication
  - Application-layer specific contexts such as the WebApplicationContext for use in web applications.
  ```

  - `ApplicationContext` 包含 `BeanFactory` 的所有功能，并且人家还扩展了好多特性
  - 应该用 `ApplicationContext` 而不是 `BeanFactory`

#### `withAnnotation`

- 根据类上标注的注解来查找对应的 Bean

- 创建一个注解`Color`

  ```java
  @Documented
  @Retention(RetentionPolicy.RUNTIME)
  @Target(ElementType.TYPE)
  public @interface Color {
  }
  ```

  - `@Target`:注解的作用目标

    ```
    @Target(ElementType.TYPE)——接口、类、枚举、注解
    @Target(ElementType.FIELD)——字段、枚举的常量
    @Target(ElementType.METHOD)——方法
    @Target(ElementType.PARAMETER)——方法参数
    @Target(ElementType.CONSTRUCTOR) ——构造函数
    @Target(ElementType.LOCAL_VARIABLE)——局部变量
    @Target(ElementType.ANNOTATION_TYPE)——注解
    @Target(ElementType.PACKAGE)——包

  - `@Retention`：注解的保留位置

    ```
    RetentionPolicy.SOURCE:这种类型的Annotations只在源代码级别保留,编译时就会被忽略,在class字节码文件中不包含。
    RetentionPolicy.CLASS:这种类型的Annotations编译时被保留,默认的保留策略,在class文件中存在,但JVM将会忽略,运行时无法获得。
    RetentionPolicy.RUNTIME:这种类型的Annotations将被JVM保留,所以他们能在运行时被JVM或其他使用反射机制的代码所读取和使用。

  - `@Document`：说明该注解将被包含在`javadoc`中
  - `@Inherited`：说明子类可以继承父类中的该注解

#### `getBeanDefinitionNames`

- 获取IOC容器中的所有Bean，即获取的就是那些 Bean 的 **id**

  ```java
  //获取IOC容器中的所有Bean
  String[] beanNames = ctx.getBeanDefinitionNames();
  Stream.of(beanNames).forEach(System.out::println);
  ```

### 延迟查找

对于一些特殊的场景，需要依赖容器中的某些特定的 Bean ，但当它们不存在时也能使用默认 / 缺省策略来处理逻辑。这个时候，使用上面已经学过的方式倒是可以实现，但编码可能会不很优雅。

#### 使用现有方案实现Bean缺失时的缺省加载

```java
Dog dog;
try {
    dog = ctx.getBean(Dog.class);
}catch (NoSuchBeanDefinitionException e){
    dog = new Dog();
}
```

#### 改良-获取之前先检查(`containsBean`)

```java
 dog = ctx.containsBean("dog")?(Dog)ctx.getBean("dog"):new Dog();
```

​	`containsBean` 方法只能传 bean 的 id ，不能查类型。

#### 改良-延迟查找

如果直接 `getBean` ，那如果容器中没有对应的 Bean ，就会报 `NoSuchBeanDefinitionException`；如果使用这种方式，运行 `main` 方法后发现并没有报错，只有调用 `dogProvider` 的 `getObject` ，真正要取包装里面的 Bean 时，才会报异常。所以总结下来，`ObjectProvider` 相当于延后了 Bean 的获取时机，也延后了异常可能出现的时机。

```java
ObjectProvider<Dog> dogProvider = ctx.getBeanProvider(Dog.class);
//只有调用 dogProvider 的 getObject ，真正要取包装里面的 Bean 时，才会报异常
dog = dogProvider.getObject();
```

#### 延迟查找-方案实现

`getIfAvailable` ，它可以在**找不到 Bean 时返回 null 而不抛出异常**。

```java
dog = dogProvider.getIfAvailable();
if(dog==null){
    dog = new Dog();
}
```

#### ObjectProvider在jdk8的升级

`ObjectProvider` 在 SpringFramework 5.0 后扩展了一个带 `Supplier` 参数的 `getIfAvailable` ，它可以在找不到 Bean 时直接用 **`Supplier`** 接口的方法返回默认实现。

```java
dog = dogProvider.getIfAvailable(()->new Dog());
//更简单的方式
dog = dogProvider.getIfAvailable(Dog::new);
```

当然，一般情况下，取出的 Bean 都会马上或者间歇的用到，`ObjectProvider` 还提供了一个 `ifAvailable` 方法，可以在 Bean 存在时执行 `Consumer` 接口的方法：

```java
dogProvider.ifAvailable(dog1-> System.out.println(dog1));
```

## 依赖注入

依赖注入是==为了给类的实例对象的字段赋值==，因为描述一个实体除了方法还有字段。

## 联系

```java
//依赖注入的时候，一般是这样写：
@Autowired
private DemoDao dao;

//这种方式同样可以用在依赖查找中：
private DemoDao dao = ctx.getBean(DemoDao.class);
```

## 区别

- 作用目标不同
  - 依赖注入的作用目标通常是类成员
  - 依赖查找的作用目标可以是方法体内，也可以是方法体外
- 实现方式不同
  - 依赖注入通常借助一个上下文被动的接收
  - 依赖查找通常主动使用上下文搜索

## 注解驱动IOC容器

### 依赖查找

#### 配置类的编写与Bean的注册

- 对比于 xml 文件作为驱动，注解驱动需要的是**配置类**。一个配置类就可以类似的理解为一个 xml 。配置类没有特殊的限制，只需要在类上标注一个 `@Configuration` 注解即可。

  ```java
  @Configuration
  public class QuickstartConfiguration {
  }
  ```

- 在 xml 中，咱声明 Bean 是通过 `<bean>` 标签。在配置类中，要想替换掉 `<bean>` 标签，自然也能想到，它是使用 `@Bean` 注解。

  ```java
  @Configuration
  public class QuickstartConfiguration {
  
      @Bean
      public Person person(){
          return new Person();
      }
  }
  ```

  - **向 IOC 容器注册一个类型为 Person ，id 为 person 的 Bean** 。
  - **方法的返回值代表注册的类型，方法名代表 Bean 的 id** 。

#### 启动类初始化注解IOC容器

用 `AnnotationConfigApplicationContext` 来驱动注解 IOC 容器

```java
public class AnnotationConfigApplication {
    public static void main(String[] args) {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(QuickstartConfiguration.class);
        Person person = ctx.getBean(Person.class);
        System.out.println(person);
    }
}
```

### 依赖注入

在**依赖查找**的基础上，直接在创建对象后给属性赋值，再返回。

```java
@Configuration
public class QuickstartConfiguration {

    @Bean
    public Person person(){
        Person person = new Person();

        person.setName("Alice");
        person.setAge(20);

        return person;
    }
}
```

#### 属性注入

### 组件注册与扫描

如果需要注册的组件特别多，那编写这些 `@Bean` 无疑是超多工作量，于是 SpringFramework 中提供了几个注解可以快速注册需要的组件，这些注解被成为**模式注解 ( stereotype annotation )**。

#### 一切组件注册的根源`@Component`

在类上标注 `@Component` 注解，即代表该类会被注册到 IOC 容器中作为一个 Bean 。

```java
//指定Person的名称为person
@Component("person")
public class Person{
    
}
```

可以直接在 `@Component` 中声明 **value** 属性用于指定Bean的名称。如果不指定 Bean 的名称，它的默认规则是 **“类名的首字母小写”**。

#### 组件扫描

只声明了组件，在写配置类时如果还是只写 `@Configuration` 注解，随后启动 **IOC** 容器，那它是感知不到有 `@Component` 存在的，一定会报 `NoSuchBeanDefinitionException` 。

##### `@ComponentScan`

在配置类上额外标注一个 `@ComponentScan` ，并指定要扫描的路径，它就可以**扫描指定路径包及子包下的所有 `@Component` 组件**。如果不指定扫描路径，则**默认扫描本类所在包及子包下的所有 `@Component` 组件**。

```java
@Configuration
@ComponentScan("com.linkedbear.spring.annotation.c_scan.bean")
public class ComponentScanConfiguration {
    
}
```

##### 不使用`@ComponentScan`的组件扫描

如果不写 `@ComponentScan` ，也是可以做到组件扫描的。在 `AnnotationConfigApplicationContext` 的构造方法中有一个类型为 String 可变参数的构造方法。这样声明好要扫描的包，也是可以直接扫描到那些标注了 `@Component` 的 Bean 的。

```java
ApplicationContext ctx = new AnnotationConfigApplicationContext("com.juejin.spring.annotation.c_scan.bean");
```

##### xml中启用组件扫描

组件扫描可不是注解驱动 IOC 的专利，对于 xml 驱动的 IOC 同样可以启用组件扫描，它只需要在 xml 中声明一个标签即可。

```xml
<context:component-scan base-package="com.juejin.spring.annotation.c_scan.bean"/>
```

#### 组件注册的其它注解

SpringFramework 为了迎合在进行 Web 开发时的三层架构，它额外提供了三个注解：`@Controller` 、`@Service` 、`@Repository`，分别代表表现层、业务层、持久层。这三个注解的作用与 `@Component` 完全一致，其实它们的底层也就是 `@Component` ：

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Controller { ... }
```

#### `@Configuration`也是`@Component`

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Configuration { ... }
```

`@Configuration`也标注了 `@Component` 注解，证明确实它也会被视为 bean ，注册到 IOC 容器。

### 注解驱动与xml驱动互通

如果一个应用中，既有注解配置，又有 xml 配置，这个时候就需要由一方引入另一方。

#### xml引入注解

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <!--开启注解配置-->
    <context:annotation-config></context:annotation-config>
    <bean class="com.juejin.spring.annotation.d_importxml.config.AnnotationConfigConfiguration"></bean>
</beans>
```

#### 注解引入xml

```java
@Configuration
@ImportResource("classpath:annotation/beans.xml")
public class ImportXmlAnnotationConfiguration {
}
```

