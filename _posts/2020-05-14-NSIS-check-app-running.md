---
layout: post
title: NSIS中监测某个程序是否运行中
date: 2020-05-14
excerpt: "NSIS中监测某个程序是否运行中"
tags: [NSIS]
comments: true
---

Revit插件的安装包，想在删除之前确认Revit已经关闭。

## FindProcDLL插件
此插件可以通过名称搜索运行中的进程，我们通过搜索"Revit.exe"来确认Revit是否在运行中。
在[这里](https://nsis.sourceforge.io/FindProcDLL_plug-in)下载FindProcDLL插件

## 在un.onInit方法中添加确认代码
~~~ nsis
Function un.onInit
  FindProcDLL::FindProc "Revit.exe"
  IntCmp $R0 1 0 notRunning
      MessageBox MB_OK|MB_ICONEXCLAMATION "请先关闭Revit！" /SD IDOK
      Abort
  notRunning:
FunctionEnd
~~~