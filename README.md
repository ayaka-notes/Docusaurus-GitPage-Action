# Docusaurus-GitAction
Docusaurus的Git-Action工具，自动化发布到GitPage中。

### 特性
- 自动检测仓库的名称，部署到`username.github.io/<repo-name>`，不需要手动替换配置文件

### 直接使用

```yml
# Simple workflow for deploying static content to GitHub Pages
name: DeployPage

on:
  # Run Everyday, but not push.
  # schedule:
  #   - cron: '30 4 * * *'
  workflow_dispatch:
    inputs:
      unconditional-invoking:
        description: '手动部署'
        type: boolean
        required: true
        default: true
  # Runs on pushes targeting the default branch
  # push:
  #   branches: ["main"]

  # Allows you to run this workflow manually from the Actions tab
  # workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  # Single deploy job since we're just deploying
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '18'  # 指定所需的 Node.js 版本
      - name: Build Page
        run: |
          npm install
          npm run build
      - name: Setup Pages
        uses: actions/configure-pages@v3
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          # Upload entire repository
          path: './build'
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2

```
