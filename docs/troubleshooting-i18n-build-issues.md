# Docusaurus 国际化（i18n）构建问题排查案例分析

本文档记录了一次典型的 Docusaurus 国际化（i18n）问题的排查过程，作为内部开发参考。

## 问题症状

- **开发环境正常**：`yarn start` 启动后，网站中英文切换正常，所有中文内容均能正确显示。
- **生产构建异常**：`yarn build` 构建后，只有网站主题部分（如导航栏）是中文，所有文档页面依然是英文。

这个现象明确指向了开发与生产构建配置的差异。

## 排查过程

### 1. 检查基础 i18n 配置

- **怀疑**：生产构建未正确识别语言列表。
- **操作**：在 `website/docusaurus.config.ts` 中硬编码 `i18n.locales`。
- **结果**：问题依旧，证明根源不在此。

### 2. 验证翻译源文件

- **怀疑**：缺少中文的 `.md` 文件。
- **操作**：检查 `website/i18n/zh-CN/docusaurus-plugin-content-docs/current/` 目录。
- **结果**：文件早已存在且完整。排除了文件缺失的可能。

### 3. 深入分析插件配置（定位根源）

- **怀疑**：`docs` 插件在不同环境下的行为不一致。
- **操作**：深入审查 `website/docusaurus.config.ts` 中 `presets` -> `docs` 的配置。
- **发现**：问题的根源在于 `lastVersion` 配置项。该配置在生产构建时被动态设置为一个具体的版本号，导致构建系统去寻找一个版本化的翻译目录（例如 `i18n/.../version-X.X/`），而不是我们实际存放翻译的 `current` 目录。由于找不到版本化的翻译，构建过程便回退到使用默认的英文源文件。

## 最终解决方案

修改 `website/docusaurus.config.ts`，强制 `lastVersion` 始终为 `'current'`，以统一开发和生产环境的行为。

```typescript
// website/docusaurus.config.ts
// ...
presets: [
  [
    '@docusaurus/preset-classic',
    {
      docs: {
        // ...
        lastVersion: 'current',
      },
      // ...
    },
  ],
],
// ...
```

## 核心启示

当遇到 Docusaurus 开发与生产环境表现不一致的问题时，应重点排查具体内容插件（如 `docs`, `blog`）中那些可能存在动态值或与版本控制相关的选项（如 `lastVersion`）。
