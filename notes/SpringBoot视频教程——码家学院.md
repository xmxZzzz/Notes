# SpringBoot视频教程——码家学院

## 为什么使用SpringBoot

### SpringBoot特点

- 免除了很多配置文件，更容易上手

- 更轻量级的Web开发，免除了以前的Server（Tomcat），只需要导出一个jar包，用`java -jar`即可启动我们的web项目。

### SpringBoot优点

- 更快入门
- 开箱即用，简化了配置
- 内嵌式web容器
- 不需要写大量的配置信息XML

## 课程目标

1. <b>对于SSM项目，可以实现完全将SSM项目迁移。</b>
2. <b>对于新项目，可以使用SpringBoot进行开发。</b>
3. <b>当涉及到分布式（SpringCloud）时，使用的都是SpringBoot。</b>

## HelloWorld——第一个SpringBoot程序

### 在pom.xml文件中添加依赖

![pom文件添加依赖](C:\Users\10195\Desktop\笔记\images\pom文件添加依赖-SpringBoot视频教程.png)

### Controller

1. 类注解：

   - <b>@Controller</b>：只返回页面，显示用。

   - <span style="color:red;"><b>@RestController</b></span>：返回JSON格式的数据。 ==<u>本质上，比@Controller多了一个@ResponseBody注解。</u>==

2. 启动HelloController

   - 使用类注解<span style="color:red;"><b>@EnableAutoConfiguration</b></span>。

   - 添加<span style="color:red;"><b>main()</b></span>方法，并执行<span style="color:red;"><b>SpringApplication.run()</b></span>方法。

     ```java
     @RestController
     @EnableAutoConfiguration
     public class HelloController {
     
         @RequestMapping("hello")
         public String hello(){
             return "Hello World!";
         }
     
         public static void main(String[] args) {
             SpringApplication.run(HelloController.class,args);
         }
     
     }
     ```

   - 控制台输出信息

     ```
     com.majiaxueyuan.HelloController         : Starting HelloController on LAPTOP-F2746MFH with PID 15976 (F:\Projects\SpringBoot视频教程-码家学院\SpringBoot_MJXY\target\classes started by 10195 in F:\Projects\SpringBoot视频教程-码家学院\SpringBoot_MJXY)
     com.majiaxueyuan.HelloController         : No active profile set, falling back to default profiles: default
     o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
     o.apache.catalina.core.StandardService   : Starting service [Tomcat]
     org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.39]
     o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
     w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 536 ms
     o.s.s.concurrent.ThreadPoolTaskExecutor  : Initializing ExecutorService 'applicationTaskExecutor'
     o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
     com.majiaxueyuan.HelloController         : Started HelloController in 1.039 seconds (JVM running for 1.881)
     o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
     o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
     o.s.web.servlet.DispatcherServlet        : Completed initialization in 4 ms
     
     ```

   - 浏览器登陆 [127.0.0.1:8080/hello]()查看结果

     ![hello页面](C:\Users\10195\Desktop\笔记\images\hello页面-SpringBoot视频教程.PNG)

