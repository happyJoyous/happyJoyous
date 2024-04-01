<!--
 * @Author: zhangy
 * @Date: 2022-06-16 12:04:25
 * @LastEditors: zhangy
 * @LastEditTime: 2022-09-22 18:05:51
 * @FilePath: \elabnote-front-main\README.md
 * Copyright (c) 2022 by BMY, All Rights Reserved.
-->

# elabnote-front-main (Vue3 + TS + Pinia + vue-router4 + setup + arco)

本项目采用 Vue3 + setup 语法 `<script setup>`, [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup)

## 推荐插件

- [VS Code](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar)，推荐工作区禁用`Vetur`插件

## 安装启动

本项目包管理器推荐使用`pnpm`，执行`pnpm install`安装依赖，`pnpm dev`启动项目

## 其它规范

### git 提交风格推荐

| 前缀     | 描述                                                                                   |
| -------- | -------------------------------------------------------------------------------------- |
| feat     | 新增特性 (feature)                                                                     |
| fix      | 修复 Bug(bug fix)                                                                      |
| docs     | 修改文档 (documentation)                                                               |
| ui       | 修改样式(ui)                                                                           |
| refactor | 代码重构(refactor)                                                                     |
| perf     | 改善性能(A code change that improves performance)                                      |
| test     | 测试(when adding missing tests)                                                        |
| build    | 变更项目构建或外部依赖（例如 scopes: vite.config.ts、gulp、npm 等）                    |
| ci       | 更改持续集成软件的配置文件和 package 中的 scripts 命令，例如 scopes: Travis, Circle 等 |
| chore    | 变更构建流程或辅助工具(比如更改测试环境)                                               |
| revert   | 代码回退                                                                               |

> 例如: `git commit -m "feta: xxx"`

### 命名规范

文件夹命名全部采用小写，统一中划线分割，例如`main-demo`，所有的`.vue`文件采用大驼峰式规范（目录下的`index.vue`除外），例如`MainDemo.vue`，如果一个目录下只用一个`.vue`组件，建议使用`index.vue`，同时建议 views 目录下的每个组件目录有且仅有一个`index.vue`文件。所有的`.ts`文件全部采用小驼峰，文件类名写成大驼峰

### 文件导入

本项目全部采用 TS 维护开发，所有导入 TS 的文件后缀默认省略，同时导入的是某个目录下的`index.ts`，建议直接导入该目录即可，例如`import { demo } from '@/demo/index.ts`应改为`import { demo } from '@/demo`
当你导入的`.vue`文件的时候不期望你省略`.vue`文件的后缀（因为 TS 不认识），所以当你在导入`.vue`文件的时候期望你导入完整路径，例如`import Demo from '@/views/Demo/index.vue'`

### 关于国际化

本项目采用一致的国际化风格，在每个页面组件（`views`目录下）需要用到文字展示的地方，强烈建议在该组件同级目录下新建`locale`目录，`locale`目录下面新建`zh-CN.ts`文件和`en-US`文件，比如`views`目录下的`demo`目录下有个`index.vue`文件，则需要在`index.vue`的同级目录下新建`locale`目录，其中`locale`目录下的`zh-CN.ts`格式如下：

```ts
export default {
  demo: {
    demo1: '这是demo1',
    demo2: '这是demo2'
  }
}
```

同时在该目录下新建相应的`en-US.ts`文件：

```ts
export default {
  demo: {
    demo1: 'this demo1',
    demo2: 'this demo2'
  }
}
```

在页面使用时详细请查看[vue-i18n](https://kazupon.github.io/vue-i18n/), 推荐安装插件`i18n Ally`，请在自己的配置中设置如下规则即可（推荐在`.vscode`目录下的`setting.json`文件中配置）：

```JSON
{
  "i18n-ally.localesPaths": [
    "src/locale",
    "src/**/*/locale",
  ],
  "i18n-ally.keystyle": "nested",
  "i18n-ally.sortKeys": true,
  "i18n-ally.enabledParsers": ["ts", "json"],
  "i18n-ally.sourceLanguage": "en",
  "i18n-ally.displayLanguage": "zh-CN",
  "i18n-ally.enabledFrameworks": ["vue", "react"],
}

```

### 文件换行格式（强烈建议）

> 强烈建议在当前项目的工作区中执行如下脚本

```sh
git config core.autocrlf false

```

### 其它

新建文件建议拥有头注释，推荐安装`koroFileHeader`插件，配置如下基本信息（推荐在`.vscode`目录下的`setting.json`文件中配置）：

```JSON
"fileheader.customMade": {
    "Author": "你的名字", // 创建作者
    "Date": "Do not edit", // 文件创建时间
    "LastEditors":"你的名字", // 最后更新作者 自动更新
    "LastEditTime":"Do not Edit", // 最后更新时间 自动更新
    "FilePath": "Do not edit", // 文件在项目中的相对路径 自动更新
    "custom_string_obkoro1_copyright": "Copyright (c) ${now_year} by BMY, All Rights Reserved. " // 版权声明
},

```

如果需要定义组件名称的话，需要再写一个`script`标签，目前`vite-plugin-vue-setup-extend`插件有 debugger 的问题，暂不推荐使用`<script setup lang="ts" name="xxx"></script>`的形式：

```html
<script setup lang="ts">
  export default {
    name: 'Demo'
  }
</script>
```

新建`.vue`文件推荐使用下面初始化模板：

```html
<template>
  <div></div>
</template>

<script setup lang="ts"></script>

<style scoped lang="scss"></style>
```

全局封装了部分 hook，其中 useFetch 是请求接口时需要用到的 hook。具体的说明如下：

```ts
interface IFetchResult {
  loading: Ref<boolean>
  error: Ref<any>
  result: Ref<any>
  fetchResource: Function
}

interface IOptions {
  params?: any
  isLazy?: boolean
}
/**
 * 包装请求接口的 hook，内部处理了 loading 状态，
 * 在请求接口需要用到 loading 状态的时候，直接用内部返回的 loading 状态，
 * 内部处理了重复调用接口 loading 状态失效的问题
 * @param {Function} api 需要请求的接口
 * @param {{ params: any, isLazy: boolean }} options 配置项 params: 参数信息， isLazy: 是否惰性触发（默认不惰性触发）
 * @returns {Object}
 */
export const useFetch = (api: Function, options: IOptions = { params: undefined, isLazy: false }): IFetchResult => ({})
```

关于获取图片的方法`getImageUrl`，vite 获取静态资源不能使用路劲别名，`utils`目录下封装`getImageUrl`方法。需要用到获取图片等其他静态资源的时候可以使用该方法方便使用路径别名

如果遇到模板文件大面积报红出错的情况，请手动删除 node_modules 下的`@fullcalendar`下的`vdom.d.ts文件`
