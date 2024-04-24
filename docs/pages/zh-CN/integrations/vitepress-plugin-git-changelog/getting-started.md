# 快速上手

## 安装

通过运行以下命令将 `@nolebase/vitepress-plugin-git-changelog` 安装到您的项目依赖项中：

::: code-group

```shell [@antfu/ni]
ni @nolebase/vitepress-plugin-git-changelog -D
```

```shell [pnpm]
pnpm add @nolebase/vitepress-plugin-git-changelog -D
```

```shell [npm]
npm install @nolebase/vitepress-plugin-git-changelog -D
```

```shell [yarn]
yarn add @nolebase/vitepress-plugin-git-changelog -D
```

:::

## 使用

::: warning 这是配置最复杂的插件之一！

请注意以下配置和步骤，以开始使用基于 Git 的页面历史插件。与其他插件相比，这并不是那么简单。

:::

将基于 Git 的页面历史插件集成到您的 VitePress 项目中包括两个主要步骤：

- [配置 Vite 插件](#配置-vite-插件)（数据获取、日志聚合）
- [与 VitePress 集成](#与-vitepress-主题集成)（UI 和组件）

### 配置 Vite 插件

有两种将基于 Git 的页面历史 Vite 插件集成到您的 VitePress 项目中的方法：

1. [**推荐**：在 VitePress 的主要配置文件中使用 `vite` 选项（通常位于 `docs/.vitepress/config.ts`，文件路径和扩展名可能会有所不同）](#在-vitepresss-配置文件中配置-vite-插件)
2. [在 VitePress 项目的根目录中创建一个单独的 Vite 配置文件（例如 `vite.config.ts`）](#在单独的-vite-配置文件中配置-vite-插件)

#### 在 VitePress 的配置文件中配置 Vite 插件

请在你的 VitePress [**核心配置文件**](https://vitepress.dev/reference/site-config#config-resolution) 中（注意不是**主题配置文件**，通常为 `docs/.vitepress/config.ts`，文件路径和拓展名也许会有区别）的根字段中添加 [Vite](https://vitejs.dev) 选项，并导入并配置 `GitChangelog`（数据获取）和 `GitChangelogMarkdownSection`（小部件嵌入）插件：

<!--@include: @/pages/zh-CN/snippets/details-colored-diff.md-->

<!--@include: @/pages/zh-CN/snippets/configure-tsconfig.md-->

```typescript twoslash
import { defineConfig } from 'vitepress'
import { // [!code ++]
  GitChangelog, // [!code ++]
  GitChangelogMarkdownSection, // [!code ++]
} from '@nolebase/vitepress-plugin-git-changelog/vite' // [!code ++]

// https://vitepress.dev/reference/site-config
export default defineConfig({
  vite: { // [!code ++]
    plugins: [ // [!code ++]
      GitChangelog({ // [!code ++]
        // 填写在此处填写您的仓库链接
        repoURL: () => 'https://github.com/nolebase/integrations', // [!code ++]
      }), // [!code ++]
      GitChangelogMarkdownSection(), // [!code ++]
    ],
  }, // [!code ++]
  lang: 'zh-CN',
  title: '站点名称',
  themeConfig: {
    // rest of the options...
  }
  // rest of the options...
})
```

#### 在单独的 Vite 配置文件中配置 Vite 插件

##### 确保已创建 `vite.config.ts`

如果您已经了解 `vite.config.ts` 是什么并且已经创建了它，您可以跳过此准备步骤，直接跳转到下一步[在 `vite.config.ts` 中配置插件](#在-viteconfigts-中配置插件)。

::: tip 首次接触 `vite.config.ts`？

首先，`vite.config.ts` 是一个为 [Vite](https://vitejs.org) 构建工具的配置文件。VitePress 是基于 Vite 构建的，它允许开发人员构建和转换项目中的资产、内容和数据。

尽管 VitePress 本身包含了整套 Vite 选项的配置在其[**主要配置文件**](https://vitepress.dev/reference/site-config#config-resolution)中（这不是一个**主题配置文件**，通常位于 `docs/.vitepress/config.ts`，文件路径和扩展名可能会有所不同），这些选项与 `vite.config.ts` 在配置方面是相同的。

然而，由于插件注册的顺序，如果我们以这种方式安装基于 Git 的页面历史插件，它将无法转换所需的数据和日志。

:::

因此，请在您的 VitePress 项目的根目录中创建一个单独的 `vite.config.ts` 文件：

::: tip 哪里是 VitePress 项目的根目录？

VitePress 项目的根目录是 `.vitepress` 目录的父目录。

例如，如果您的 VitePress 项目的目录结构如下所示：

```shell
.
├── docs
│   ├── .vitepress
│   │   ├── config.ts
│   │   └── theme
│   │       └── index.ts
│   └── README.md
```

在这种情况下，根目录是 `docs`。

```shell
.
├── .vitepress
│   ├── config.ts
│   └── theme
│       └── index.ts
└── README.md
```

在这种情况下，根目录是 `./`。

:::

```shell
touch vite.config.ts
```

##### 在 `vite.config.ts` 中配置插件

在位于项目根目录的独立的 [Vite 配置文件](https://vitejs.dev/config/)（也就是 `vite.config.ts` 中），我们需要导入 `GitChangelog`（数据获取）和 `GitChangelogMarkdownSection`（小部件嵌入）插件并进行正确配置：

<!--@include: @/pages/zh-CN/snippets/details-colored-diff.md-->

```typescript twoslash
import { join } from 'node:path'
import { defineConfig } from 'vite'
import { // [!code ++]
  GitChangelog, // [!code ++]
  GitChangelogMarkdownSection, // [!code ++]
} from '@nolebase/vitepress-plugin-git-changelog/vite' // [!code ++]

export default defineConfig(() => {
  return {
    plugins: [ // [!code ++]
      GitChangelog({ // [!code ++]
        // 填写在此处填写您的仓库链接
        repoURL: () => 'https://github.com/nolebase/integrations', // [!code ++]
      }), // [!code ++]
      GitChangelogMarkdownSection(), // [!code ++]
    ]
    // 其他 Vite 配置...
  }
})
```

### 与 VitePress 主题集成

现在，让我们将基于 Git 的页面历史 UI 小部件集成到您的 VitePress 项目中。

在 VitePress 的[**主题配置文件**](https://vitepress.dev/reference/default-theme-config#default-theme-config)（请注意，这与上面提及的**配置文件**并非是一个文件，主题配置文件通常位于 `docs/.vitepress/theme/index.ts`，文件路径和扩展名可能会有所不同），安装 Vue 插件并使用组件：

<!--@include: @/pages/zh-CM/snippets/details-colored-diff.md-->

<!--@include: @/pages/zh-CN/snippets/configure-tsconfig.md-->

::: code-group

```typescript twoslash [docs/.vitepress/theme/index.ts]
import { h } from 'vue'
import DefaultTheme from 'vitepress/theme'
import type { Theme as ThemeConfig } from 'vitepress'
import { // [!code ++]
  NolebaseGitChangelogPlugin // [!code ++]
} from '@nolebase/vitepress-plugin-git-changelog/client' // [!code ++]

import '@nolebase/vitepress-plugin-git-changelog/client/style.css' // [!code ++]

export const Theme: ThemeConfig = {
  extends: DefaultTheme,
  Layout: () => {
    // 其他配置
  },
  enhanceApp({ app }) {
    app.use(NolebaseGitChangelogPlugin)  // [!code ++]
  },
}

export default Theme
```

:::

## 错误排查

### 遭遇了 `Cannot find module ... or its corresponding type declarations` 错误？

<!--@include: @/pages/zh-CN/snippets/troubleshooting-cannot-find-module.md-->