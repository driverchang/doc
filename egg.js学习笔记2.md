###### egg.js学习笔记2

1. Logger对象的获取方式和使用场景

   - APP Logger ：通过app.logger获取它，用来做一些应用级别的日志记录，如记录启动阶段的一些数据信息、记录一些业务上与请求无关的信息等。
   - App CoreLogger：通过app.coreLogger来获取它，应用不通过CoreLogger打印日志，框架和插件通过它打印应用级别的日志。这样可以清晰地区分应用和框架打印的日志，CoreLogger打印出来的日志和Logger打印的日志存放到不同的文件中。
   - Context Logger：通过ctx.logger获取它，它打印的日志都会在前面带上一些请求相关的信息。
   - Context CoreLogger：通过ctx.corelogger获取它，和ContextLogger的区别就是只有框架和插件通过它来记录日志。
   - Controller Logger ,Service Logger：在Controller 和Service实例上通过this.logger获取它们，它们本质上就是一个 Context Logger，不过在打印日志的时候还会额外的加上文件路径，方便定位日志的打印位置。

2. 两种指定运行环境方式

   - 通过config/env文件指定，该文件内容就是运行环境，一般通过构建工具来生成这个文件。

   - 通过EGG_SERVER_ENV环境变量指定运行环境

     ```
     EGG_SERVER_ENV = prod npm start
     ```

3. 配置管理常见的三种方案

   - 使用平台管理配置，应用构建时将当前环境的配置放入包内，启动时指定该配置。但应用就无法一次构建多次部署，而且本地开发环境想使用配置会变的很麻烦。
   - 使用平台管理配置，在启动时通过环境变量传递当前环境配置。 这种方法对配置平台的操作和支持有更高的要求。 此外，配置环境具有与第一种方法相同的缺陷。
   - 使用代码管理配置，在代码中添加多个环境的配置，在启动时传入当前环境的参数即可。但无法全局配置，必须修改代码。

4. 配置的写法，置文件返回的是一个 object 对象，可以覆盖框架的一些配置，应用也可以将自己业务的配置放到这里方便管理。

   ```
   module.exports = {
     logger: {
       dir: '/home/admin/logs/demoapp',
     },
   };
   ```

   配置文件也可以简化成exports.key = value的形式

   ```
   exports.keys = 'my-cookie-secret-key';
   exports.logger = {
     level: 'DEBUG',
   };
   ```

5. 配置加载的的优先级：应用 > 框架 > 插件

6. Router主要用来描述请求URL和具体承担执行动作的Controller的对应关系。Router的完整定义：

   ```
   router.verb('router-name', 'path-match', middleware1, ..., middlewareN, app.controller.action);
   ```

   - verb：用户触发动作，支持get，post等所有的HTTP方法
     - router.head - HEAD
     - router.options - OPTIONS
     - router.get - GET
     - router.put - PUT
     - router.post - POST
     - router.patch - PATCH
     - router.delete - DELETE
   - router-name： 给路由设定一个别名，可以通过 Helper 提供的辅助函数 pathFor 和 urlFor来生成 URL。(可选)
   - path-match：路由URL路径
   - middleware1：在Router里面可以配置多个Middleware。（可选）
   - controller：指定路由映射到的具体的 controller 上，controller两种 写法：
     - app.controller.user.fetch：直接指定一个具体的controller
     - 'user.fetch'：可以简写为字符串形式

7. Controller的作用解析用户的输入，处理后返回相应的结果