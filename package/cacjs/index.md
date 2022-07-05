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
    |-- mod.js -> 
    |-- mod.ts ->
    |-- mod_test.ts ->
    |-- package.json -> 项目的配置文件 
    |-- rollup.config.js -> rollup 配置文件
    |-- tsconfig.json -> typescript配置文件
    |-- yarn.lock -> yarn 的依赖锁定版本和源的文件
    |-- .github -> github配置文件
    |   |-- FUNDING.yml ->
    |   |-- ISSUE_TEMPLATE.md ->
    |-- examples -> 示例文件
    |-- scripts
    |   |-- build-deno.ts ->
    |-- src -> 项目主目录
```
