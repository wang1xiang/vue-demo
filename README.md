## Github Pages

GitHub Pages相当于把指定目录放到web服务器上

- settings --> GitHub Pages --> 选择gh-pages分支

- 把 dist 目录发布到 gh-pages 分支

  - .gitignore 中不能忽略 dist 目录

  - 把 dist 目录推送到远程的 gh-pages 分支

    ```bash
    git subtree push --prefix dist origin gh-pages
    git push origin --delete gh-pages 删除远程gh-pages分支
    ```

- Git 设置代理

  ```bash
  git config --global http.proxy http://127.0.0.1:1080
  git config --global https.proxy https://127.0.0.1:1080
  
  git config --global --unset http.proxy
  git config --global --unset https.proxy
  ```

- settings --> Custom domain配置域名

##### Github Actions自动集成

个人设置 - Personal access tokens

- ```
  62f13d150743575e8ac922e49d435e11af0401d0
  ```

项目 - Settings - Secrets

本地项目，创建 .github/workflows/ci.yml

本地项目，package.json 中增加

- "homepage": "https://[用户名].github.io/[仓库名称]",

本地项目，创建 vue.config.js

```js
module.exports = {
  outputDir: 'dist',
  publicPath: process.env.NODE_ENV === 'production' ? '/github的仓库名称/' : '/'
}
```

- git 提交 git add   git commit

- 设置 Github Pages  - 指定 gh-pages 分支

  - .github/workflows/deploy.yml

  https://github.com/JamesIves/github-pages-deploy-action

```yaml
name: GitHub Actions Build and Deploy Demo
on:
  push:
    branches:
      - master
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@master
    - name: Build and Deploy
      uses: JamesIves/github-pages-deploy-action@master
      env:
        ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }}
        BRANCH: gh-pages
        FOLDER: dist
        BUILD_SCRIPT: npm install && npm run build
# https://github.com/marketplace/actions/deploy-to-github-pages
```

## 