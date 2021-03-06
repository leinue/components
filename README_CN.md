[![Serverless Components](https://img.serverlesscloud.cn/20191218/1576684402436-component.gif)](http://serverless.com)

<br/>

<p align="center">
  <span>简体中文</span> |
  <a href="./README.md">English</a>
</p>

**Serverless Components** 是 **[Serverless Framework](https://github.com/serverless/serverless/blob/master/README_CN.md)** 重磅推出的基础设置编排能力，支持开发者通过 **Serverless Components** 构建，组合并部署你的 Serverless 应用。

- - [x] **快速部署 -** Components 支持极速部署 Serverless 架构和应用
- - [x] **全面覆盖 -** 既能支持基础设施的 Components，也可以支持更高维度的，场景级别的 Components。
- - [x] **轻松复用 -** 你构建的每个 Component 都可复用，并且对外发布后，也可以支持他人使用。
- - [x] **灵活组合 -** 可以通过 YAML 或者 Javascript 灵活组合不同的 Components。

下面通过一个 Serverless Framework 使用 Components 的例子，可以看出 Component 多么易用：

```yaml
# serverless.yml
name: website

website:
  component: '@serverless/tencent-website'
  inputs:
    code:
      src: ./src
      # hook: npm run build
      # domain: www.serverlesscomponents.com
```

<!--
[^ Watch a video guide on using a Serverless Component here](https://www.youtube.com/watch?v=ts26BVuX3j0).
-->

<br/>

- [快速开始](#快速开始)
- [特点](#特点)
- [概述](#概述)
- [使用 Components](#使用-Components)
  - [Serverless.yml 介绍](#Serverless.yml-介绍)
  - [输入](#输入)
  - [输出](#输出)
  - [账号配置](#账号配置)
  - [环境变量](#环境变量)
- [开发 Components](#开发-Components)
  - [Serverless.js 介绍](#Serverless.js-介绍)
  - [开发建议](#开发建议)
- [目前支持的 Components](https://github.com/serverless-components)
- [用例模板](./templates)
- [中文技术社区](https://serverlesscloud.cn/)
- [后续规划](https://github.com/serverless/components/projects/1)

<br/>

# 快速开始

通过 NPM 安装 [Serverless Framework](https://www.github.com/serverless/serverless) ：

```console
$ npm i -g serverless
```

**确保你使用的是 Serverless Framework 1.49 及以上的版本**。更早的版本无法支持 Serverless Components Beta。

之后，通过 `create --template-url` 命令安装一个 [Serverless Components 模板](./templates)，模板中会包含了 Componenets 及示例代码，可以让你更快的了解 Component。

以下是一些常用的用例模板：

#### [部署 Hexo 静态博客](https://github.com/serverless/components/tree/master/templates/tencent-hexo-blog)

通过 Serverless Website 组件快速构建一个 Serverless Hexo 站点

```shell
serverless create --template-url https://github.com/serverless/components/tree/master/templates/tencent-hexo-blog
```

#### [快速构建 REST API](https://github.com/serverless/components/tree/master/templates/tencent-python-rest-api)

通过 Serverless SCF 组件快速构建一个 REST API 应用，实现 GET/PUT 操作。

```shell
serverless create --template-url https://github.com/serverless/components/tree/master/templates/tencent-python-rest-api
```

#### [部署 Serverless 全栈 WEB 应用（React.js）](https://github.com/serverless/components/tree/master/templates/tencent-fullstack-react-application)

本示例以 React 为前端，Express 框架作为后端，通过多个 Serverless Components 部署 Serverless 全栈应用程序。

```shell
serverless create --template-url https://github.com/serverless/components/tree/master/templates/tencent-fullstack-react-application
```

#### [部署 Serverless 全栈 WEB 应用（Vue.js）](https://github.com/serverless/components/tree/master/templates/tencent-fullstack-vue-application)

本示例以 Vue 为前端，Express 框架作为后端，通过多个 Serverless Components 部署 Serverless 全栈应用程序。

```shell
serverless create --template-url https://github.com/serverless/components/tree/master/templates/tencent-fullstack-vue-application
```

#### [部署其他模板](./templates)

在这里查看所有预设的 [Components 模板](./templates)，你可以通过这些模板方便的部署*REST API*， 网站, *定时任务*等多种场景。每个模板都提供了清晰的 `README.md` 来说明怎样使用。

#### Serverless Components 列表

如下所示，当前 Serverless Components 支持丰富的多语言开发框架和应用：

[![Serverless Components Tencent](https://img.serverlesscloud.cn/20191213/1576236739852-Component%20Gallery.png)](https://github.com/serverless-components/)

基础组件：

- [@serverless/tencent-apigateway](https://github.com/serverless-components/tencent-apigateway) - 腾讯云 API 网关组件
- [@serverless/tencent-cos](https://github.com/serverless-components/tencent-cos) - 腾讯云对象存储组件
- [@serverless/tencent-scf](https://github.com/serverless-components/tencent-scf) - 腾讯云云函数组件
- [@serverless/tencent-cdn](https://github.com/serverless-components/tencent-cdn) - 腾讯云 CDN 组件
- [@serverless/tencent-cam-role](https://github.com/serverless-components/tencent-cam-role) - 腾讯云 CAM 角色组件
- [@serverless/tencent-cam-policy](https://github.com/serverless-components/tencent-cam-policy) - 腾讯云 CAM 策略组件

高阶组件：

- [@serverless/tencent-express](https://github.com/serverless-components/tencent-express) - 快速部署基于 Express.js 的后端服务到腾讯云函数的组件
- [@serverless/tencent-egg](https://github.com/serverless-components/tencent-egg) - 快速部署基于 Egg.js 的后端服务到腾讯云函数的组件
- [@serverless/tencent-koa](https://github.com/serverless-components/tencent-koa) - 快速部署基于 Koa.js 的后端服务到腾讯云函数的组件
- [@serverless/tencent-flask](https://github.com/serverless-components/tencent-flask) - 腾讯云 Python Flask RESTful API 组件
- [@serverless/tencent-laravel](https://github.com/serverless-components/tencent-laravel) - 腾讯云 PHP Laravel RESTful API 组件
- [@serverless/tencent-website](https://github.com/serverless-components/tencent-website) - 快速部署静态网站到腾讯云的组件

第三方贡献：

- [@twn39/tencent-fastify](https://github.com/twn39/tencent-fastify) - 快速部署基于 fastify.js 的后端服务到腾讯云函数的组件
- [@twn39/tencent-php-slim](https://github.com/twn39/tencent-php-slim) - 快速部署基于 Slim PHP 微框架的后端服务到腾讯云函数的组件

此外，所有的 Serverless Components 可都在这个 [Github 仓库](https://github.com/serverless-components/)里查看。

<br/>

# 特点

### 简洁

Serverless Components 可以实现高阶应用场景，例如网站、博客或者支付系统等。开发者无需关心底层基础设施的配置细节，仅通过简单的配置就可以实现场景的构建。

像下面的例子，仅通过最简单的配置，你可以部署：

- 一个 **Serverless 静态网站**，托管在腾讯云的 COS 服务上，并且通过 CDN 服务进行加速，支持自定义的域名解析，并且支持免费的 SSL 证书做传输加密。

```yaml
# serverless.yml

website:
  component: '@serverless/tencent-website'
  inputs:
    code:
      src: ./src
    hosts:
      - host: www.serverless-app.com
```

- 一个 **Express 框架**，通过腾讯云云函数 SCF 以及 API 网关服务，支持 Node.js 的 Express 框架，支持 HTTPS API 访问。

```yaml
# serverless.yml

api:
  component: '@serverless/tencent-express'
  inputs:
    code: ./api
    functionName: tencent-express
    apigatewayConf:
      protocols:
        - https
```

- 以及[更多场景](./templates)

### 可复用

Serverless Components 可以通过 YAML 中的配置灵活组合 (`serverless.yml`)，这些配置是通过可复用的 javascrpt 库实现的（`serverless.js`），实现语法简单，设计思想受到了组件化的开发框架如 React 的启发。

```javascript
// serverless.js

const { Component } = require('@serverless/core')

class MyBlog extends Component {
  async default(inputs) {
    this.context.status('Deploying a serverless blog')
    const website = await this.load('@serverless/tencent-website') // Load a component
    const outputs = await website({ code: { src: './blog-code' } }) // Deploy it
    this.state.url = outputs.url
    await this.save()
    return outputs
  }
}

module.exports = MyBlog
```

每个人都可以方便的构建 Serverless Component，并且在我们即将提供的注册中心 Registry 中分享自己的成果。

### 快速部署

大部分 Serverless Components 的部署速度比传统的云服务编排产品快 20 倍左右，我们期望可以设计可以实时部署的 Components，从而无需在本地环境模拟云服务的使用或调试。

### 非厂商绑定

Serverless Components 倾向于支持 Serverless 化的云服务，为了提供更好的产品体验，Serverless 化的云产品支持用户按需付费，无需付费即可享受到最好的服务。

Serverless Components 的设计是非云厂商绑定的，支持你方便的使用不同云厂商的不同服务。例如 腾讯云云函数，对象存储 COS 服务，AWS 的 Lmabda 服务，Azure Functions，Twilio，Stripe，Algolia，Cloudflare Workers 等。

### Vanilla Javascript

Serverless Components 是用轻量级的 vanilla javascript 框架实现的，并且期望使用最少的依赖，让整个项目对于初学者来说尽可能友好易用。

<br/>

# 概述

Serverless Components 主要通过 Javascript 库封装各种服务的功能，使其变得易用和可复用。

他们专注于提供后端的场景和用例，并且更多的支持 Serverless 化的基础云服务，这样可以使用户通过更少的开销，更低的成本来构建应用。Serverless Components 更适合后端的场景，对应的 React 组件则更适合前端的场景。

Serverless Components 既可以提供基础设施服务（例如，[对象存储 COS](http://github.com/serverless-components/tencent-cos)），也可以提供更高阶的场景——这也是组件最擅长的部分。例如以下的场景：

1. 多个基础设施服务，提供特定的能力，例如数据处理管道。
2. 提供具体功能，例如用户注册，评论或者支付的模块等。
3. 提供整个应用，例如博客系统，流视频播放系统，或者落地页等。

开发 Serverless Components 的语法可以直接调用和封装低维度的组件，并且部署对应的资源。这样的设计可以让你无需考虑底层基础设施的配置，快速构建更高阶的组件。

Serverless Components 可以通过**声明**的方式使用（通过 `serverless.yml` 文件），也可以通过**编程**的方式使用（通过 `serverless.js` 文件）。

如果你希望尽可能简单快速的部署一个 Serverless 应用，并且不会复用这个组件的话，那么用声明的方式来调用组件就可以满足你的需求。

如果你希望构建一个可复用的组件，那么应该用编程的方式来使用这些基础的组件，并且快速构建自己的 Serverless 应用。

在[使用 Components](#使用-Components) 这一章，我们会主要讲解声明式的体验（`serverless.yml`）；而在[开发 Components](#开发-Components) 这一章，我们会着重说明编程式的体验（`serverless.js`）。

<br/>

# 使用 Components

### Serverless.yml 介绍

通过 `serverless.yml` ，可以用最简单的方式将 Serverless Components 组合成为一个 Serverless 应用。其语法的使用如下所示：

```yml
name: my-serverless-website

website: # Component 实例的声明
  component: '@serverless/tencent-website' # 具体调用的 Component ，用于创建资源
  inputs: # 传入 Component 的 "default()" 函数中的参数配置
    code:
      src: ./src
```

通过 `$ serverless` 命令，你可以通过 Serverless Framework 快速部署这个应用。

```console
$ serverless # 安装并部署应用...
```

<!--
可以通过 `--watch` 参数来查询变更情况

```console
$ serverless --watch # 检测变更，并且重新部署
```
-->

当你使用 `serverless.yml` 来调用 Serverless Components 的时候，不需要安装额外的依赖，而在部署的时候 `serverless.yml` 中使用的 Serverless Components 会被自动下载，并且存储在你的本地环境中。这样 Serverless Components 会被缓存在特定的位置，所以你的项目目录中不会直接存储 Serverless Components 依赖，并且 Serverless Components 也不需要被多次下载。

Serverless Components 通过 [NPM](https://www.npmjs.com/) 进行发布，因此当它们被下载时，后台会默认通过 NPM 安装对应的模块。

正因如此，你在 `component:` 属性中可以直接填写 NPM 的包名。

```yml
website: # Component 实例
  component: '@serverless/tencent-website' # 对应的 NPM 包名
```

You can also use the same semantic versioning strategy that NPM uses.

```yml
website: # Component 实例
  component: '@serverless/tencent-website@1.2.11' # 对应的 NPM 包名，和版本信息
```

当你在配置中写了版本信息时，只有这个特定的版本的包会被使用。如果你不加版本信息，每次部署时，Serverless Framework 会自动监测最新的版本并且更新。

**注意：** 在 Component Beta 版本中，目前不能在已有的 Serverless Framework 项目中使用 Component，比如项目中已有 `functions`， `events`， `resources` 或者 `plugins`等属性的时候。

### 输入

每个 Serverless Component 都有一个用于部署的主函数，也就是 `default()` 函数（你也可以在 “开发 Components” 这一章来了解更多相关信息）。这个 `default()` 函数会把 `inputs` 内的参数作为输入。

当你在 `serverless.yml` 中指定了 `inputs` 参数时，这些参数会被直接作为参数传入到 `default()` 函数中。

```yml
name: my-serverless-website

website:
  component: '@serverless/tencent-website@1.2.11'
  inputs: # Inputs 中的参数作为入参传给 default() 函数
    code:
      src: ./src
```

### 输出

当 Component 的函数（例如上文中的 `default()` 函数）运行完成后，会返回一个 `outputs` 对象。

你可以在 `serverless.yml` 中引用这些 `outputs` 对象，并且把返回值传入到其他组件中，比如下面的例子

```yml
backend:
  component: '@serverless/tencent-express'
  inputs:
    code:
      src: ./src
    env:
      dbName: ${database.name}
      dbRegion: ${database.region}

database:
  component: '@serverless/tencent-postgresql'
  inputs:
    name: users-database
```

上面这段描述可以让 Serverless Framework 将 `database` 实例的输出作为输入传到 `tencent-express` 组件实例中。具体来说，数据库里的 `name` 和 `region` 这两个字段作为环境变量传入了 `tencent-express` 实例，所以它才可以和数据库进行交互操作。

上面的例子也同时告诉了 Serverless Framework 模块之间的依赖关系，因此 Framework 会根据这些关系构建有向图，并且按照先后依赖顺序来进行部署。也正因此，当前不支持循环引用，如果出现循环引用 Framework 会报错。

### 账号配置

在部署时，无论是使用 `serverless.yml` 还是 `serverless.js`，Serverless Components 会在当前目录寻找 `.env` 或者 `.env_temp` 文件。

部署过程中，如果 `.env` 或者 `.env_temp` 文件存在，Serverless Components 会将其作为环境变量上传。如果你使用的和各云厂商提供的 `secretId` 和 `secretKey` 字段一致的环境变量名，Serverless Components 在部署期间会自动将其注入到需要该秘钥的组件中，用来创建基础设施服务。

<!--
Upon deployment, if a `.env` file exists, Serverless Components will add the content of it as environment variables. If you use specific environment variable names that match that of a cloud infrastructure vendor's access keys/tokens, upon deployment, Serverless Components will automatically inject that into the Components that need that vendor to provision infrastructure.
-->

如果你按照如下方式配置，这些秘钥可以被 `serverless.yml` 以及 `serverless.js` 中的所有 Serverless Components 使用，也包括其中引用到的基础组件。

以下是目前支持的秘钥配置：

#### 腾讯云账号配置

> 注：当前腾讯云支持通过`微信`扫描二维码一键授权登录/注册，扫码生成的临时秘钥文件为 `.env_temp` ，但目前该秘钥最长可以支持 30 天有效期授权，过期需要重新授权。如果需要持久秘钥或者在角色中提供了其他权限控制，也可以通过下面方式填写 `.env` 文件进行持久授权。

```bash
TENCENT_SECRET_ID=123456789
TENCENT_SECRET_KEY=123456789
```

组件也可以通过 `this.context.credentials.tencent` 来获取腾讯云的秘钥配置，返回的格式如下所示：

```js
{
  secret_id: '123456789',
	secret_key: '123456789',
  region: 'ap-guangzhou'
}
```

### 环境变量

你可以直接在 `serverless.yml` 中引用环境变量（例如上文中定义的 `.env` 文件），使用方式是在 yaml 中直接引用 `${env}` 对象。如下例子，如果你希望引用环境变量 `TABLE`，你可以通过下面方式配置 `serverless.yml`

```yml
backend:
  component: '@serverless/tencent-express'
  inputs:
    code:
      src: ./src
    env:
      table: ${env.TABLE}
```

<br/>

# 开发 Components

如果你希望开发可复用的 Serverless Components，那么可以围绕 `serverless.js` 文件进行开发。

### Serverless.js 介绍

在当前的工作目录下，通过如下命令安装 `@serverless/core` 作为本地依赖。

```
npm i --save @serverless/core
```

创建一个新的 `serverless.js` 文件，扩展 Component 类，并且增加一个 `default` 方法，这样就实现了一个最基本的 Serverless Component，如下所示：

```javascript
// serverless.js

const { Component } = require('@serverless/core')

class MyComponent extends Component {
  async default(inputs = {}) {
    return {}
  } // The default functionality to run/provision/update your Component
}

module.exports = MyComponent
```

`default()` 方法是必须的，它构成了 Component 调用和构建的基本逻辑。当你运行 `$ serverless` 命令时，总是会先调用 `default()` 方法。

你也可以在类下面增加一些其他的方法，比如 `remove()` 经常是下一个需要支持的逻辑，这个方法支持你的 Serverless Component 将创建的云上资源移除掉。

你也可以增加更多的方法，这种方式比较灵活，你可以通过 Component 实现更多自动化的方法，而不仅仅是 _部署_ 和 _移除_ 的逻辑。

当你运行任何一个命令时，可以使用 `serverless --watch` 参数，这个参数支持检测当前工作目录下的改动，并且在检测到改动时重新运行该命令。所以你也可以增加对 `serverless remove --watch`的支持。

目前 Serverless Components 依然在较为早期的阶段，但我们正在对 `test()` 方法，或者是 `logs()` 和 `metrics()` 等方法做支持，也可能会支持 `seed()` 方法，用于在数据库 Component 中生成初始值。因此 Components 有非常多的机会可以提供更加完善的场景支持，并且将这一过程更加自动化

除了 `default()` 方法之外，所有其他的方法都是可选的。所有的方法都以一个 `inputs` 对象作为输入，而不是单独的参数；并且返回一个 `outputs` 对象。

下面例子展示了 `remove` 方法的实现，也展示了自定义方法的实现方式：

```javascript
// serverless.js

const { Component } = require('@serverless/core')

class MyComponent extends Component {
  /*
   * Default (Required)
   * - The default functionality to run/provision/update your Component
   * - You can run this function by running the "$ serverless" command
   */

  async default(inputs = {}) {
    return {}
  }

  /*
   * Remove (Optional)
   * - If your Component removes infrastructure, this is recommended.
   * - You can run this function by running "$ serverless remove"
   */

  async remove(inputs = {}) {
    return {}
  }

  /*
   * Anything (Optional)
   * - If you want to ship your Component w/ extra functionality, put it in a method.
   * - You can run this function by running "$ serverless anything"
   */

  async anything(inputs = {}) {
    return {}
  }
}

module.exports = MyComponent
```

在 Component 方法里，`this` 当前运行环境，它包含了一些可用的工具，下面有一些指南可以展示出在 Component 方法中哪些语法是可用的。

```javascript
// serverless.js

const { Component } = require('@serverless/core')

class MyComponent extends Component {
  async default(inputs = {}) {
    // this.context features useful information
    console.log(this.context)

    // Common provider credentials are identified in the environment or .env file and added to this.context.credentials
    // when you run "components", then the credentials in .env will be used
    // when you run "components --stage prod", then the credentials in .env.prod will be used...etc
    // if you don't have any .env files, then global tencent credentials will be used
    const tencentCredentials = this.context.credentials.tencent

    // You can easily create a random ID to name cloud infrastructure resources with using this utility.
    const bucketName = `my-bucket-${this.context.resourceId()}`
    // This prevents name collisions.

    // Components have built-in state storage.
    // Here is how to save state to your Component:
    this.state.name = 'myComponent'
    await this.save()

    // Here is how to load a child Component.
    // This assumes you have the "@serverless/tencent-website" component in your "package.json" file and you've run "npm install"
    let website = await this.load('@serverless/tencent-website')

    // You can run the default method of a child Component two ways:
    let websiteOutputs = await website({ code: { src: './src' } })
    let websiteOutputs = await website.default({ code: { src: './src' } })

    // If you are deploying multiple instances of the same Component, include an instance id.
    let website1 = await this.load('@serverless/tencent-website', 'website1')
    let website2 = await this.load('@serverless/tencent-website', 'website2')

    // Child Components save their state automatically.

    // You can also load a local component that is not yet published to npm
    // just reference the root dir that contains the serverless.js file
    // You can also use similar syntax in serverless.yml to run local Components
    let localComponent = await this.load('../my-local-component')

    // Here is how you can easily remove a Component.
    let websiteRemoveOutputs = await website.remove()

    // Here is how you can call any custom method on a Component.
    let websiteRemoveOutputs = await website.test({})

    // If you want to show a status update to the CLI in an elegant way, use this.
    this.context.status('Uploading')

    // If you want to show a log statement in the CLI in an elegant way, use this.
    this.context.log('this is a log statement')

    // Return your results
    return { url: websiteOutputs.url }
  }
}

module.exports = MyComponent
```

在包含 `serverless.js` 文件的目录下运行 `serverless` 命令来运行你写的 component，你会看到其所有的日志和输出信息。除非你使用 `serverless --debug` debug 模式，否则你所依赖/调用的 components 的日志和输出则不会展示出来。你也可以通过 `serverless <methodName>` 来运行一些自定义的方法/命令。

开发过程中，如果希望参考完整的 Components 实现案例，可以通过[官方 Components 仓库](https://github.com/serverless-components)了解其实现方式。

### 开发建议

当你开始开发 Serverless Components 的时候，这里有一些实用的开发建议：

#### 使用 `--watch` 参数启动监听模式

在开发过程中，运行各个方法时，使用 `serverless --watch` 让终端持续运行是非常有帮助的，因为这样可以拿到及时的反馈，减少开发的阻碍。

#### 使用 `--debug` 参数启动调试状态

为了更优雅的体验， Serverless Components CLI 的命令十分简单，但是如果你希望知道在运行时究竟发生了什么，就在部署时使用 `--debug` 参数，则会得到如下的输出：

![Serverless Components Debugging](https://img.serverlesscloud.cn/20191222/1577035892816-zip-pic-debug.jpeg)

很多 Serverless Components 都支持 debug 参数，用来上报组件当前在做什么，是怎样的状态。我们建议你在开发自己的 Component 的时候也可以加上 debug 的声明，如下所示：

```javascript
class MyComponent extends Component {
  async default() {
    this.context.debug(`Starting MyComponent.`)
    this.context.debug(`Creating resources.`)
    this.context.debug(`Waiting for resources to be provisioned.`)
    this.context.debug(`Finished MyComponent.`)
  }
}
```

#### 使用本地引用

在开发自己的 Serverless Component 时，你可以通过 `serverless.yml` 或者 `serverless.js` 本地引用自己的 Component，需要注意的是，文件目录中只能存在一个 `serverless.yml` 或者 `serverless.js` 文件，并且没有办法同时存在 `serverless.yml` 和 `serverless.js` 文件。

下面例子展示了怎样在 `serverless.yml` 中引用一个本地的组件：

```yaml
name: my-project

myComponent:
  component: ../src
  inputs:
    foo: bar
```

下面例子展示了怎样在 `serverless.js` 中引用一个本地的组件：

```javascript
class myFirstComponent extends Component {
  default() {
    const mySecondComponent = await this.load('../components/my-second-component')
  }
}
```

#### 结果导向/自上而下

在开发自己的 Serverless Component 时，可以比较容易的将其分解成不同层级的 Components，这样可以保持结构的解耦，并且增加自己组件的可复用性。

然而，提供后端逻辑比设计 React 这样的前端组件要复杂得多。如果太过关注结构并且对其过度优化，那很可能让你精疲力尽。

因此我们建议你从结果入手，关注点更多在于你希望提供的结果是怎样的，并且创建这个 Component 的首要目的是解决你的问题。在完成最初的目标之后，再去做结构优化，将其分解成为不同层级的 Components。

#### 结果是你的优势

对基础设施的服务进行配置其实非常复杂，因此市面上也有非常多的配置工具会支持所有的配置和组合方式（例如 AWS 的 Cloudformation 等），但 Serverless Components 和他们相比，最有力的优势就是它知道自己需要提供什么样的具体场景。

关于软件部署工具，我们学到一个非常重要的方面就是，一旦你知道自己希望得到场景，你就可以据此创造出一个更好的工具。

Components 了解用户场景，基于场景，你可以实现如下几点：
1）更加可靠的配置基础设施，因为你对于自己希望的效果和配置方案十分清晰，所以可以采用最简洁优雅的方案配置。
2）更快的配置基础设施
3）通过给 Components 增加自定义的方法，针对特定的用例实现自动化的配置。

#### 将大部分状态存在云服务商中

Serverless Components 节省了很多状态信息。事实上，很多功能强大的 Components 在其状态对象中也只有不到 10 个属性。

Components 依赖云服务作为状态的来源，并用其存储状态信息。这样可以防止服务出现状态转移，从而影响基础设施配置工具。这样做也可以支持 Components 使用已有的云资源，也就是那些一开始不是由 Components 创建的资源，用于迁移等场景。

#### 操作成功后立即存储状态信息

如果你确实需要存储状态，那么试着在一次成功的部署后立即存储状态信息。用这种方式，如果后续的部署和操作中有发生失败的情况，终端客户需要再次部署的时候，你的 Serverless Components 可以从中断的地方开始继续部署。这样也减少了冗余资源的创建。

#### 易用性优化

我们相信 serverless 的基础设施和架构可以让更多人拥有更强大的能力来开发软件。

正因如此，我们将我们所有的项目设计的尽可能的易于理解和学习。请尝试使用简单的，原始的 Javascript 语法。此外，为了减少安全风险和依赖的复杂度，请您尽可能少的使用 NPM 依赖。
