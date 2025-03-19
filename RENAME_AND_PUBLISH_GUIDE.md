# 重命名并发布组件库到NPM的指南

本指南将帮助您将修改后的webviz-subsurface-components组件库重命名并发布到npm。

## 概述

该项目是一个monorepo结构，包含多个独立的npm包：

- `@webviz/group-tree-plot`
- `@webviz/subsurface-viewer`
- `@webviz/well-completions-plot`
- `@webviz/well-log-viewer`
- `@webviz/wsc-common`

每个包需要单独重命名和发布。

## 步骤

### 1. 修改每个包的package.json文件

对于每个组件包，您需要修改其package.json文件中的以下字段：

- `name`: 更改为您的新包名（例如从`@webviz/subsurface-viewer`改为`@your-org/subsurface-viewer`）
- `version`: 设置新的版本号（建议从1.0.0开始）
- 可选：更新`description`、`author`、`homepage`等字段

例如，修改`subsurface-viewer`的package.json：

```json
{
    "name": "@your-org/subsurface-viewer",
    "version": "1.0.0",
    "description": "3D visualization component for subsurface reservoir data",
    ...
}
```

### 2. 更新内部依赖引用

检查每个包的package.json中的dependencies部分，更新对其他包的引用。例如，在`well-log-viewer`中：

```json
"dependencies": {
    "@your-org/wsc-common": "*",
    ...
}
```

### 3. 更新主package.json

修改typescript目录下的主package.json文件：

```json
{
    "name": "@your-org/subsurface-components",
    ...
}
```

### 4. 构建所有包

在typescript目录下运行：

```bash
npx nx run-many -t build
```

### 5. 发布到npm

对于每个包，进入其目录并发布：

```bash
cd typescript/packages/subsurface-viewer
npm publish --access public
```

对其他包重复此步骤：

```bash
cd ../well-log-viewer
npm publish --access public

cd ../well-completions-plot
npm publish --access public

cd ../group-tree-plot
npm publish --access public

cd ../wsc-common
npm publish --access public
```

### 注意事项

1. 发布前确保您已登录npm：`npm login`
2. 如果使用组织范围的包名（如`@your-org/...`），确保您有权限发布到该组织
3. 如果是私有包，使用`npm publish --access restricted`
4. 建议先发布`wsc-common`包，因为其他包可能依赖它

## 自动化发布

如果您想设置自动化发布流程，可以参考项目中的`.github/workflows/semantic_release.yml`和`.github/workflows/typescript.yml`文件，根据您的CI/CD环境进行相应修改。

## 更新README和文档

别忘了更新README.md和其他文档中的包名引用，确保用户能够正确安装和使用您的新包。