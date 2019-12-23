###### SpringBoot知识补充

1. Spring Boot启动程序仅通过添加一个依赖项就可以帮助减少手动添加的依赖项的数量。因此，无需手动指定依赖项，只需添加一个starter即可。

   ```
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-web</artifactId>
   </dependency>
   ```

2. slf4、logback、log4j的区别：logback和log4j是日志实现框架，就是实现怎么记录日志的。slf4提供了java所有日志框架的简单抽象（日志的门面设计模式），就是一个日志API（没有实现类），它不能单独使用，必须结合logback或log4j日志框架来实现。

3. application.properties和application.yml都是SpringBoot留下的配置文件，都可以进行配置。properties文件在进行配置时一定是一个key--value的格式。

   ```
   spring.thymeleaf.prefix=classpath:/....
   spring.thymeleaf.suffix=.html
   spring.thymeleaf.encoding=UTF-8
   ```

   而yml文件是梯级呈现的，它配置方式如下

   ```
   spring:
   	thymeleaf:
   		prfix: classpath:/....
   		suffix: .html
   		encoding: UTF-8
   ```

   在yml文件里面所有的配置，相同级别只能出现一次，比如我们使用了spring这个级别，那么在后面进行spring级别配置的时候就必须在这个地方进行，不能写在另一个spring级别。

   在yml文件里面如果要进行赋值就必须在":"后面进行一个空格键的缩进。
