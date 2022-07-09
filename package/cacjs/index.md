# cacjs源码分析

仓库地址： [https://github.com/cacjs/cac](https://github.com/cacjs/cac)

## cacjs介绍
cac 是一个用于构建CLI应用程序的JavaScript库,其特点是
- 超级轻量级：无依赖，只有一个文件
- 容易学习：构建简单的cli只需要学习4个api: `cli.option` `cli.version` `cli.help` `cli.parse`.
- 强大,启用如下特性：默认命令、类似git的子命令、所需参数和选项的验证、可变参数、点嵌套选项、自动生成帮助消息等等。
- 开发友好： 使用 Typescript 书写

## 目录结构
```
    |-- .editorconfig -> 有助于为不同 IDE 编辑器上处理同一项目的多个开发人员维护一致的编码风格。
    |-- .gitattributes   
    |-- .gitignore ->  git忽略要上传的文件类型
    |-- .prettierrc -> Prettier代码格式化工具配置文件
    |-- LICENSE -> 开源许可
    |-- README.md -> 自述文件(项目的介绍、使用、特色等等)
    |-- circle.yml -> Circle CI 持续集成配置文件
    |-- index-compat.js -> 打包后的入口文件
    |-- jest.config.js -> jest配置文件
    |-- mod.js
    |-- mod.ts
    |-- mod_test.ts
    |-- package.json -> 项目的配置文件 
    |-- rollup.config.js -> rollup 配置文件
    |-- tsconfig.json -> typescript配置文件
    |-- yarn.lock -> yarn 的依赖锁定版本和源的文件
    |-- .github -> github配置文件
    |   |-- FUNDING.yml
    |   |-- ISSUE_TEMPLATE.md
    |-- examples -> 示例文件
    |-- scripts
    |   |-- build-deno.ts
    |-- src -> 项目主目录
```

## 持续集成是如何实现的
持续集成（Continuous Integration）通常缩写为 CI，
持续集成指的是，当代码有变更时，立即进行构建和测试，反馈运行结果，我们可以根据测试结果，确定新代码是否可以和原有代码正确的集成在一起。
让你能够在开发中随时发现问题，在快速的产品迭代中还可以保持很高的质量。因为修复问题的成本随着时间的推移而增长，越早发现，修复成本越低。
持续集成的目的，就是让产品可以快速迭代，同时还能保持高质量。它的核心措施是，代码集成到主干之前，必须通过自动化测试。只要有一个测试用例失败，就不能集成。
通过持续集成， 团队可以快速的从一个功能到另一个功能，简而言之，敏捷软件开发很大一部分都要归功于持续集成。
根据持续集成的设计，代码从提交到生产，整个过程有以下几步。
- **提交**
流程的第一步，是开发者向代码仓库提交代码。所有后面的步骤都始于本地代码的一次提交
（commit）。
- **测试（第一轮）**
代码仓库对commit操作配置了钩子（hook），只要提交代码或者合并进主干，就会跑自动化测试。
- **构建**
通过第一轮测试，代码就可以合并进主干，就算可以交付了。
交付后，就先进行构建（build），再进入第二轮测试。所谓构建，指的是将源码转换为可以运行的实
际代码，比如安装依赖，配置各种资源（样式表、JS脚本、图片）等等。
- **测试（第二轮）**
构建完成，就要进行第二轮测试。如果第一轮已经涵盖了所有测试内容，第二轮可以省略，当然，这时
构建步骤也要移到第一轮测试前面。
- **部署**
过了第二轮测试，当前代码就是一个可以直接部署的版本（artifact）。将这个版本的所有文件打包（
tar filename.tar * ）存档，发到生产服务器。
- **回滚**
一旦当前版本发生问题，就要回滚到上一个版本的构建结果。最简单的做法就是修改一下符号链接，指
向上一个版本的目录。

![image-20210722215138665](https://upload-images.jianshu.io/upload_images/15581733-2335869c09d5a1ac.png?imageMogr2/auto-orient/strip|imageView2/2/w/940/format/webp)

### circle.yml 是如何配置的
circleci文档 https://circleci.com/docs/about-circleci

## 分析一下 package.json 里面的字段都是干嘛的
- **name**
如果项目是需要发版为npm包的，则name字段是必须的。因为它涉及到npm包的命名。
- **version**
version字段用于定义版本号。如果项目是为发布npm包，则必须包含此字段。如果是普通的项目，则此字段是可选的。
- **description**
description用于描述当前项目的概况。在发布的npm包，在npmjs的搜索结果中，可以直接显示description内容，方便使用方直接了解包的功能。
- **homepage**
项目的官网主页地址。
- **repository**
项目的源码地址。
- **license**
项目的协议类型。这个项目涉及到知识产权方面的知识,所以开源项目的时候，要重点考虑到底要用哪个协议。
- **files**
声明有哪些文件，是需要作为依赖项，保留下来。不然，执行npm publish进行发布时，这些文件是会自动屏蔽上传的。同理，也可以使用.npmignore文件进行配置。如果没有files字段声明，则这些文件，都不会保留，npm包将不能使用。
- **scripts**
在npm中使用scripts标签来定义脚本，每当制定npm run的时候，就会自动创建一个shell脚本，这里需要注意的是，npm run新建的这个 Shell，会将本地目录的node_modules/.bin子目录加入PATH变量。
- **bin**
bin属性用来将可执行文件加载到全局环境中，指定了bin字段的npm包，一旦在全局安装，就会被加载到全局环境中，可以通过别名来执行该文件。
- **main & module & browser**
除了type外，package.json中还有main,module和browser 3个字段来定义npm包的入口文件。
main : 定义了 npm 包的入口文件，browser 环境和 node 环境均可使用
module : 定义 npm 包的 ESM 规范的入口文件，browser 环境和 node 环境均可使用
browser : 定义 npm 包在 browser 环境下的入口文件
- **engines**
项目运行环境的要求声明。
```js
  "engines": {
    "node": ">=8"
  },
```
node版本需要在8版本及以上，才可以运行当前项目
- **exports**
如果在package.json中定义了exports字段，那么这个字段所定义的内容就是该npm包的真实和全部的导出，优先级会高于main和file等字段。
- **dependencies & devDependencies**
dependencies是项目的依赖，而devDependencies是开发所需要的模块，所以我们可以在开发过程中需要的安装上去，来提高我们的开发效率。这里需要注意的时，在自己的项目中尽量的规范使用，形如webpack、babel等是开发依赖，而不是项目本身的依赖，不要放在dependencies中。
- **package.json三方属性**
package.json中也存在很多三方属性，比如tsc中使用的types、构建工具中使用的sideEffects,git中使用的husky，eslint使用的eslintIgnore，这些扩展的配置，针对特定的开发工具.

## 分析一下 tsconfig 里面的配置项
TypeScript 使用 tsconfig.json 文件作为其配置文件，当一个目录中存在 tsconfig.json 文件，则认为该目录为 TypeScript 项目的根目录。




参考文章：
[深入浅出package.json](https://juejin.cn/post/7099041402771734559)
[《JavaScript 标准参考教程（alpha）》](http://javascript.ruanyifeng.com/nodejs/packagejson.html)
[TypeScript 使用指南手册](http://www.patrickzhong.com/TypeScript/zh/project-config/tsconfig.json.html)









