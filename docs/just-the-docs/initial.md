---
layout: default
title: Initial
parent: Just the Docs
# permalink: docs/just-the-docs/initial.html
nav_order: 1
---

## Initial

---

1.clone repo

2.modify `_config.yml`  
  example:

  ```
  title: Blog
  description: Daily code snippets, development notes, and programming reflections
  theme: just-the-docs

  url: https://zhixialin.github.io
  baseurl: "/blog"

  destination: ./_site

  plugins:
    - jekyll-default-layout

  just_the_docs:
    navigation:
     structure: nested

  aux_links:
    Github Repository: https://github.com/ZhixiaLin/blog

  footer_content: "MIT Licensed | Copyright © Zhixia Lin 2025"
  ```

3.select GitHub Actions  
  Go to **Settings > Pages > Build and deployment > Source**

4.Local Run  
  cd correct_path

  ```
  bundle exec jekyll server
  ```

---

如果需要将此框架添加到别的项目中，一种可能的解决方案是：
1. add /just_the_docs folder in the project root path, then move Just the Docs files into it
2. remove .gitignore & Gemfile of Just the Docs, add these lines into the project's .gitignore & Gemfile
3. add some ci in .github/workflows/ci.yml of the project, remember do not add duplicate codes

CI example:

```
name: CI

on:
  pull_request:
  push:
    branches: [ main ]

jobs:
  other jobs...

  build_and_deploy_docs:
    runs-on: ubuntu-latest
    needs: rspec_cucumber
    permissions:
      id-token: write
      pages: write
      contents: read
    steps:
      - uses: actions/checkout@v4
      - uses: ruby/setup-ruby@v1
        with:
          <!-- To double check -->
          ruby-version: '3.4.1'
          bundler-cache: true

      - name: Setup GitHub Pages
        id: pages
        uses: actions/configure-pages@v5

      - name: Build Jekyll Docs
        run: |
          <!-- To double check -->
          cd just_the_docs
          bundle exec jekyll build --baseurl "${{ steps.pages.outputs.base_path }}"
        env:
          JEKYLL_ENV: production

      - name: Upload artifact for GitHub Pages
        uses: actions/upload-pages-artifact@v3
        with:
          <!-- To double check -->
          path: just_the_docs/_site

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```