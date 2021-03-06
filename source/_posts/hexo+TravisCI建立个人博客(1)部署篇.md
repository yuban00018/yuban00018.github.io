---
title: hexo+TravisCI建立个人博客(1)部署篇
date: 2020-8-22 00:00:00
tags: hexo
desc: 本文采用Hexo+github.io+Travis-CI的方式建立静态博客
---

## 说明

博主使用的环境是Ubuntu20.04LTS，Node.js 14 LTS，VsCode。

本文采用Hexo+github+Travis-CI的方式建立静态博客。

## 参考文献
本文主要采用[Hexo官方中文文档](https://hexo.io/zh-cn/docs/)的操作流程。

## 前期准备

首先遵循官方文档，安装Hexo。需要的前置程序有[Node.js](https://nodejs.org/en/)以及[Git](https://git-scm.com/)。完成下载后安装Hexo：

```bash
npm install -g hexo-cli
```
前期的安装流程就完成了。
## 在本地建站并同步到github

首先在[github](https://github.com/)新建一个repository，命名为 username.github.io，需要注意的是一个用户只能有一个io域名。username填入自己的用户名称。

完成这步后，在本地选择一个文件夹储存你的网站，右键打开terminal，输入：

```bash
hexo init <username.github.io>
cd <username.github.io>
npm install
```
本地的文件夹就建立好了，在桌面上打开gitbash，clone你的项目:
```bash
git clone https://github.com/username/username.github.io/
```
打开文件夹，勾选显示不可见文件:

<img src="https://raw.githubusercontent.com/yuban00018/MyImages/master/show_hidden_file.jpg" width="75%" height="75%">

然后把.git文件拖入到你init的folder里面并检查 .gitignore 文件中是否包含 public 一行，如果没有在文件夹里找到.gitignore文件，可以自己加上：
```
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```

在vscode里面选择暂存更改提交并推送，至此你的网站文件就被同步到github了。

但你会发现，你现在并不能正常访问网站，github将会显示错误信息，不用慌张，接下来我们就要解决这些问题。

### 本地预览*
当然你也可以在本地提前预览build后的效果:
```bash
hexo clean
hexo g
hexo d
hexo s
```
以上命令会在本地生成一个链接，用浏览器打开就可以预览效果，返回terminal按下Ctrl+C就可以关闭。

## 使用Travis-CI协助部署

我们上传到github的master分支的其实是我们的源文件，网页需要经过hexo generate之后才能正常显示，因此，我们采用Travis-CI协助我们自动生成静态网页。

以下引用官方教程(含小幅度修改)：

1. 将[Travis CI](https://github.com/marketplace/travis-ci)添加到你的 GitHub账户中。

2. 前往 GitHub 的[Applications settings](https://github.com/settings/installations)配置 Travis CI 权限，使其能够访问你的 repository。
3. 你应该会被重定向到[Travis CI](https://github.com/marketplace/travis-ci)的页面。
4. 在浏览器新建一个标签页，前往GitHub新建[Personal Access Token](https://github.com/settings/tokens)，只勾选**repo**的权限并生成一个新的**Token**。**Token**生成后请立即复制并保存好。
5. 回到[Travis CI](https://github.com/marketplace/travis-ci)，前往你的 repository的设置页面，在**Environment Variables**下新建一个环境变量，Name为 **GH_TOKEN**，**Value**为刚才你在 GitHub 生成的 Token。确保**DISPLAY VALUE IN BUILD LOG**保持**不被勾选**避免你的**Token**泄漏。点击**Add**保存。
6. 在你的 Hexo 站点文件夹中新建一个 .travis.yml 文件：
```yml
sudo: false
language: node_js
node_js:
  - 14 # use nodejs v14 LTS
cache: npm
branches:
  only:
    - master # build master branch only
script:
  - hexo generate # generate static files
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GH_TOKEN
  keep-history: true
  on:
    branch: master
  local-dir: public
```
7. 将 .travis.yml 推送到 repository 中。Travis CI 应该会自动开始运行，并将生成的文件推送到同一 repository 下的 gh-pages 分支下
8. 在 GitHub 中前往你的 repository 的设置页面，修改 GitHub Pages 的部署分支为 gh-pages。
9. 前往 https://username.github.io 查看你的站点是否可以访问。这可能需要一些时间。

至此，你的博客已经可以正常访问了。需要注意的是，百度是搜索不到你的博客的，因为被baidu的爬虫被ban了。

后续更新如何更换主题以及编辑第一条博客……