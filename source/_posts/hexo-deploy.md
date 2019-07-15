---
title: hexo deploy
date: 2019-07-10 00:28:49
tags: hexo
---

### Hexo post process

1. git add .
2. git commit -m "anything"
3. git push origin hexo //提交到hexo分支，保存hexo项目完整文件
4. hexo clean
5. hexo g -d //deploy hexo blog

hexo部署方式参考https://www.zhihu.com/question/21193762 CrazyMilk的方法
<!-- more -->

### build process(first time)

1. 搭建仓库，chenyuyan.github.io;
2. 创建两个分支：master and hexo;
3. 设置hexo为default branch(hexo分支保存博客的完整hexo文件);
4. git clone git@github.com:chenyuyuan/chenyuyuan.github.io.git;
5. 在chenyuyuan.github.io目录下执行npm install hexo, hexo init "$blogname", npm install, npm install hexo-deployer-git;
6. 修改_config.yml的deploy: master;
7. 执行git add ., git commit -m "anything", git push origin hexo
8. 部署博客到网站上, hexo clean, hexo g -d

### deploy in new device

1. git clone git@github.com:chenyuyuan/chenyuyuan.github.io.git (默认分支是hexo)
2. 在chenyuyuan.github.io目录下执行npm install hexo, npm install, npm install hexo-deployer-git
3. 插件安装: npm install --save hexo-admin