###### egg.js学习笔记

1.从Koa继承来的对象Application, Context, Request, Response，框架扩展的对象Controller, Service, Helper, Config, Logger.

- Application 是全局应用对象，在一个应用中，只会实例化一个，它继承自 Koa.Application，在它上面我们可以挂载一些全局的方法和对象。获取方式：几乎所有被框架 Loader 加载的文件（Controller，Service，Schedule 等），都可以 export 一个函数，这个函数会被 Loader 调用，并使用 app 作为参数

  ```
  //Controller可以通过 this.app 访问到 Application 对象
  class UserController extends Controller {
    async fetch() {
      this.ctx.body = this.app.cache.get(this.ctx.query.id);
    }
  }
  
  //启动自定义脚本
  module.exports = app => {
    app.cache = new Cache();
  };
  ```

  

- Context 是一个请求级别的对象，继承自 Koa.Context。在每一次收到用户请求时，框架会实例化一个 Context 对象，这个对象封装了这次用户请求的信息，并提供了许多便捷的方法来获取请求参数或者设置响应信息。获取方式：最常见的 Context 实例获取方式是在 Middleware, Controller 以及 Service 中。

  ```
  //ctx是Context的实例
  async function middleware(ctx, next) {
    console.log(ctx.query);
  }
  ```

- Request 是一个请求级别的对象，继承自 Koa.Request。封装了 Node.js 原生的 HTTP Request 对象，提供了一系列辅助方法获取 HTTP 请求常用参数。

  Response 是一个请求级别的对象，继承自 Koa.Response。封装了 Node.js 原生的 HTTP Response 对象，提供了一系列辅助方法设置 HTTP 响应。

  获取方式：可以在 Context 的实例上获取到当前请求的 Request 和 Response实例。

  ```
  //ctx.respons 是Response的实例
  //ctx.request 是Request的实例
  class UserController extends Controller {
    async fetch() {
      const { app, ctx } = this;
      const id = ctx.request.query.id;
      ctx.response.body = app.cache.get(id);
    }
  }
  ```

- Controller：框架提供了一个 Controller 基类，并推荐所有的 Controller 都继承于该基类实现。Controller基类的属性

  - ctx :当前请求的 Context实例
  - app: 应用的 Application 实例
  - config: 应用的配置
  - service:应用所有的service
  - logger:为当前controller封装的logger对象

- Service：框架提供了一个 Service 基类，并推荐所有的 Service都继承于该基类实现。其基类的属性和Controller基类一样。获取方式：

  ```
  // 从 egg 上获取（推荐）
  const Service = require('egg').Service;
  class UserService extends Service {
  }
  module.exports = UserService;
  
  // 从 app 实例上获取
  module.exports = app => {
    return class UserService extends app.Service {
    };
  };
  ```

- Helper用来提供一些实用的 utility 函数。它的作用在于我们可以将一些常用的动作抽离在 helper.js 里面成为一个独立的函数，这样可以用 JavaScript 来写复杂的逻辑，避免逻辑分散各处，同时可以更好的编写测试用例。获取方式：

  ```
  class UserController extends Controller {
    async fetch() {
      const { app, ctx } = this;
      const id = ctx.query.id;
      const user = app.cache.get(id);
      ctx.body = ctx.helper.formatUser(user);
    }
  }
  ```

  