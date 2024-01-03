# Docusaurus-GitAction
Docusaurus with Git-Action，auto publish to GitPage.

## Feature
- Detect the name of the repo, and substitute the base url
- Currently it works with Docusaurus v3.0

### 直接使用

```yml
name: DeployPage

on:
  ####################################################################
  # Allows you to run this workflow manually from the Actions tab
  # 
  # 允许你在 Actions 页面手动运行这个工作流
  workflow_dispatch:
    inputs:
      unconditional-invoking:
        description: 'Deploy Manually'
        type: boolean
        required: true
        default: true
  push:
    branches: ["main"]


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
      - name: DeployPage
        uses: ayaka-notes/Docusaurus-GitPage-Action@v1.1
```
