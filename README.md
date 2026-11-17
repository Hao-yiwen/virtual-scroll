# virtual-scroll

[![Vue](https://img.shields.io/badge/Vue-3.2.47-42b883)](https://vuejs.org/)
[![Vue CLI](https://img.shields.io/badge/Vue%20CLI-5.0.8-4fc08d)](https://cli.vuejs.org/)
[![Yarn](https://img.shields.io/badge/Yarn-1.x-2c8ebb)](https://classic.yarnpkg.com/)

Vue 3 单页应用的起始仓库。当前代码是 **Vue CLI 5 默认脚手架**，页面仍是官方欢迎页；仓库名是 `virtual-scroll`，**虚拟滚动尚未实现**。

只有一次初始提交（`init`），没有路由、状态管理、测试，也没有业务组件。

## 当前页面做了什么

开发服务器或生产构建打开后，会看到：

- 顶部 Vue 标志（`src/assets/logo.png`）
- `HelloWorld` 组件，标题文案为 `Welcome to Your Vue.js App`
- 指向 Vue CLI、已安装插件（Babel、ESLint）以及 Vue 生态文档的外链

入口在 `src/main.js`：`createApp(App).mount('#app')`。`App.vue` 与 `HelloWorld.vue` 都使用选项式 API。浏览器标签标题来自 `package.json` 的 `name`，为 `virtual-scroll`。

仓库里没有应用截图。`src/assets/logo.png` 是脚手架自带的 Vue 标志，`public/favicon.ico` 是站点图标。

## 技术栈

`package.json` 声明的是版本范围，下表是 `yarn.lock` 锁定的实际版本。

| 用途 | 包 | 锁定版本 |
| --- | --- | --- |
| 框架 | `vue` | 3.2.47 |
| Polyfill | `core-js` | 3.30.1 |
| 开发服务器 / 构建 | `@vue/cli-service` | 5.0.8 |
| Babel | `@vue/cli-plugin-babel`、`@babel/core` | 5.0.8、7.21.4 |
| 语法解析 | `@babel/eslint-parser` | 7.21.3 |
| 代码检查 | `@vue/cli-plugin-eslint`、`eslint`、`eslint-plugin-vue` | 5.0.8、7.32.0、8.7.1 |

配置要点：

- `vue.config.js` 只开启了 `transpileDependencies: true`，没有改 `publicPath`、代理或端口。
- Babel 使用 `@vue/cli-plugin-babel/preset`。
- ESLint 继承 `plugin:vue/vue3-essential` 与 `eslint:recommended`，没有自定义规则。
- `browserslist`：`> 1%`、`last 2 versions`、`not dead`、`not ie 11`。生产构建因此只打一份 ES module 包，不做 differential loading。
- `jsconfig.json` 把 `@/*` 映射到 `src/*`，供编辑器解析路径；源码里还没有使用该别名。

未安装：`vue-router`、`vuex` / Pinia、TypeScript、CSS 预处理器、单元测试。

## 目录结构

```text
.
├── public/
│   ├── favicon.ico          # 站点图标
│   └── index.html           # HTML 模板，挂载点 #app
├── src/
│   ├── assets/logo.png      # Vue 标志
│   ├── components/
│   │   └── HelloWorld.vue   # 欢迎页与文档链接
│   ├── App.vue              # 根组件
│   └── main.js              # 创建并挂载应用
├── babel.config.js
├── jsconfig.json
├── vue.config.js
├── package.json
└── yarn.lock                # Yarn 1 lockfile（依赖来自 npmmirror）
```

`yarn build` 会生成 `dist/`（已在 `.gitignore` 中）。

## 环境要求

- **Yarn 1**（本仓库是 `yarn.lock` v1）。本文命令在 Yarn 1.22.22 上验证。
- **Node.js**：传递依赖 `@achrinza/node-ipc@9.2.6` 声明的引擎范围是 Node 8–19。`package.json` 本身没有 `engines` 字段。
  - 在该范围内（例如 Node 18）可直接 `yarn install --frozen-lockfile`。
  - 本环境为 **Node.js 22.23.4**。不加参数时安装会因引擎不匹配失败；加上 `--ignore-engines` 后，安装、检查、构建和开发服务器均成功。Node 22 上会出现 `Browserslist: caniuse-lite is outdated` 提示，以及 webpack 相关的 `fs.Stats` 弃用警告，不影响本次构建结果。

## 安装与运行

```bash
# Node 18 等符合引擎范围的版本
yarn install --frozen-lockfile

# Node 20 及以上（已在 Node 22.23.4 验证）
yarn install --frozen-lockfile --ignore-engines
```

```bash
# 开发服务器，默认 http://localhost:8080/
yarn serve

# 生产构建，输出到 dist/
yarn build

# ESLint（vue-cli-service lint）
yarn lint
```

已在本仓库验证（Node.js 22.23.4，Yarn 1.22.22，`yarn install --frozen-lockfile --ignore-engines`）：

| 命令 | 结果 |
| --- | --- |
| `yarn lint` | 通过，无 lint 错误 |
| `yarn build` | 编译成功，产出 `dist/js` 与 `dist/css` |
| `yarn serve` | `http://localhost:8080/` 返回 HTTP 200，页面标题为 `virtual-scroll` |

开发服务器没有改端口。8080 被占用时，Vue CLI 会询问是否换端口；非交互环境可显式指定，例如 `yarn serve --port 8080`。

## 说明

- 页面上的 “Installed CLI Plugins / Essential Links / Ecosystem” 只是脚手架文案，对应的 router、vuex 等库并没有装进本项目。
- 依赖解析地址是 `registry.npmmirror.com`（写在 `yarn.lock` 的 `resolved` 字段里）。换源后如果要更新锁文件，需要重新安装并提交新的 `yarn.lock`。
- 没有测试脚本、CI 配置或许可证文件。

## 参考

- [Vue 3 文档](https://vuejs.org/)
- [Vue CLI 配置参考](https://cli.vuejs.org/config/)
