# 📡 Rss · 技术博客订阅聚合

[![Vue](https://img.shields.io/badge/Vue-2.6-4FC08D?style=flat-square&logo=vue.js)](https://v2.vuejs.org/)
[![Vue CLI](https://img.shields.io/badge/Vue%20CLI-5.x-3EB489?style=flat-square)](https://cli.vuejs.org/)
[![Element UI](https://img.shields.io/badge/Element%20UI-2.15-409EFF?style=flat-square)](https://element.eleme.io/)

把 CSDN 与博客园的 RSS 订阅拉到一个页面里，标题列表直接可点，无需登录、无需后端。

## 快速开始

```bash
# 安装依赖
npm i

# 启动开发服务器
npm run dev

# 打包生产版本
npm run build
```

## 技术栈

| 分类 | 选型 |
| --- | --- |
| 框架 | Vue 2.6 |
| 构建 | Vue CLI 5 |
| UI | Element UI |
| 状态 | Vuex 3 |
| 路由 | Vue Router 3 |
| RSS 解析 | rss-parser |
| 样式 | Sass + postcss-px2rem（基准 16） |

## 工作原理

```text
组件 created()
   ├─→ XMLHttpRequest 请求 CSDN RSS 地址
   ├─→ XMLHttpRequest 请求博客园 RSS 地址
   │
   ├─→ rss-parser.parseString(responseText) 解析为 JSON
   └─→ 渲染成标题列表，点击 window.open(item.link, "_blank") 新窗口打开
```

核心逻辑全部在 `src/views/Index.vue`：`searchCSDN()` 与 `searchCnBlogs()` 分别拉取订阅源，`JumpFn(item)` 负责跳转。

## 项目结构

```text
Rss/
├─ public/
│  ├─ favicon.ico
│  └─ index.html
├─ src/
│  ├─ api/test.js          接口示例
│  ├─ assets/css/reset.scss
│  ├─ router/index.js      路由表
│  ├─ store/index.js       Vuex
│  ├─ utils/
│  │  ├─ request.js        axios 封装
│  │  ├─ directives.js     自定义指令
│  │  └─ rem.js            rem 适配（基准 1920）
│  ├─ views/Index.vue      订阅列表页（核心）
│  ├─ App.vue
│  └─ main.js
├─ vue.config.js           rem 适配、@ 别名、页面标题
├─ .env.development
├─ .env.production
└─ package.json
```

## 订阅源说明

- **CSDN**：通过 RSS 地址直接拉取文章列表
- **博客园**：同上

RSS 地址写在 `src/views/Index.vue` 的 `searchCSDN()` / `searchCnBlogs()` 内。要换成自己的博客或增加新源，复制一份方法、改 URL 与 data 字段即可。

## 常见问题

**列表一直是空的？**
大概率是 CORS 或 RSS 源变更。浏览器直接请求第三方 RSS 地址通常被跨域策略拦截，需在 `vue.config.js` 里配 `devServer.proxy` 转发，或用支持 CORS 的 RSS 代理服务。

**想加第三个订阅源？**
在 `data` 里加一个 `XxxData: {}`，复制 `searchCSDN()` 改名，把 `xhr.open` 的地址换成新源，再在模板里加一段 `<ul>`。

**页面标题还是"网站标题"？**
标题在 `vue.config.js` 的 `chainWebpack` 里通过 `html` 插件注入，改那一行的字符串即可。

**rem 适配下字体比例不对？**
`vue.config.js` 中 `postcss-px2rem` 的 `remUnit` 必须与 `src/utils/rem.js` 里的 `baseSize` 保持一致（当前都是 16）。

## 更多项目

- 工具箱：http://amoureux555.xyz/
- 个人博客：http://amoureux555.top/

> 上述地址为项目初期记录，站点可用性请自行确认。

## 许可

仅供学习与参考使用。
