###### spring boot学习笔记

1. spring boot是一种框架，其目的是用来简化Spring应用的初始搭建和开发过程。从根本上讲，SpringBoot是一些库的集合。
2. SpringBoot的特点：
   - 创建独立的Spring应用程序
   - 嵌入的Tomcat，无需部署WAR文件
   - 简化Maven配置
   - 自动配置Spring
   - 提供生产就绪型功能，如指标，健康检查和外部配置
   - 绝对没有代码生成和XML配置要求
3. SpringBoot基础结构
   - src/main/java程序开发以及主程序入口
   - src/main/resources配置文件
   - src/test/java测试程序
4. @SpringBootApplication Spring Boot应用标注在某个类上说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot。
5. @SpringBootConfiguration Spring Boot的配置类
6. Spring  Boot starters的好处：
   - 提高了pom的可管理性
   - 减少项目的整体配置时间
   - 快速地配置依赖项
7. Actuator：给应用带来了生产就绪功能。其主要作用是用于公开有关正在运行的应用程序的操作信息、运行状况、指标、信息、转储、环境等。
8. 更改配置三种方式：使用属性文件、程序配置、使用命令行参数