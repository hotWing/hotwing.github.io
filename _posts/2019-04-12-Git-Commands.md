---
layout: post
title: Git常用的命令
date: 2019-04-19
excerpt: "记录自己Git常用的命令"
tags: [Git]
comments: true
---

**git clone &lt;url&gt;**  拷贝仓库到到本地  
例：git clone `https://github.com/XXX/Project.git`

**git status**  查看库状态（查看哪些暂存了，哪些没有，哪些还没跟踪）

**git add .** 将所有修改过的文件提交暂存区

**git add &lt;file&gt;** 将文件修改提交到暂存区  
例：git add ./assets/file

**git commit -m &quot;message&quot;**  将暂存区文件提交到本地仓库

**git push**  将所有改动提交到github

**git reset -–hard** 撤销所有更改

**git checkout -- &lt;file&gt;** 撤销某个文件或文件夹的更改 