# linropx.github.io

LINROP / ARCHIVE 的静态站点发布仓库，由 GitHub Pages 托管。

**本仓库只包含构建产物，没有源代码。** 直接编辑这里的文件会在下次构建时被覆盖。

## 站点

- 地址：https://linropx.github.io/
- 类型：GitHub Pages **用户站点**，部署在域名根路径
- 生成方式：Next.js 静态导出（output: 'export'）

## 关键：构建时必须清空 basePath

源码的 next.config.ts 里有：

    basePath: process.env.NEXT_PUBLIC_BASE_PATH || ''

本仓库是**用户站点**，必须部署在根路径，因此构建时 NEXT_PUBLIC_BASE_PATH **必须为空**。
若该变量被设为 /仓库名（模板的 GitHub Actions 工作流会为**项目站点**自动注入），
产物里所有链接都会带上前缀，部署到根路径后**全站 404**。

## 如何更新

1. 在源码工程中修改内容
2. 清空 basePath 构建：NEXT_PUBLIC_BASE_PATH= node node_modules/next/dist/bin/next build
3. 把 out/ 的产物同步到本仓库根目录
4. 提交并推送：

    git -C deploy add -A
    git -C deploy commit -m "更新站点"
    git -C deploy push

推送后 GitHub Pages 自动发布，通常几十秒内生效。

## 必须保留的文件

- .nojekyll —— 没有它 Jekyll 会忽略下划线开头的 _next/ 目录，**全站样式与脚本失效**
- .gitattributes 中的 * -text —— 产物应以原始字节存储，禁止换行符转换

## 说明

next build 会生成大量 __next.*.txt 文件，这些是 Next.js App Router 的 RSC 客户端负载，
供站内路由跳转与预取使用，**不是**无用文件，不要删除。
