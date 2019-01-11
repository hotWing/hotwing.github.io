---
layout: post
title: Cocos Creator 屏幕适配
date: 2019-01-11
excerpt: "Cocos Creator 屏幕适配相关一些记录"
tags: [cocos creator, 微信小游戏]
comments: true
---

### 1. 坑
Cocos Creator 1.9.1 开发微信小游戏时遇到的一个奇怪的问题。游戏的设计分辨率是1920x1080，当设置fit width和fit height之后，在IphoneX上整个游戏画面居然不是居中、两边留黑，而是缩在了屏幕左侧。然后学习了一下Cocos Creator适配方面的资料，在这里记录一下。

### 2. 踩
#### 2.1. 添加编辑器预览分辨率
一直好奇怎么像在Unity编辑器中那像设置自定义的预览分辨率。只要在引擎安装目录中找到boot.js（CocosCreator\resources\static\preview-templates\boot.js），打开它，找到`devices`数组，添加想要的分辨率即可，如下：
~~~ javascript
var devices = [
        { name: 'IPhone X', width: 375, height: 812, ratio: 3 }, //添加的分辨率
];
~~~
之后在预览的时候就能在左上角选到新添加的分辨率了。
#### 2.2. 代码设置分辨率相关信息
Cocos Creator提供了cc.view单例来操作试图相关的信息（[View官方文档](https://docs.cocos.com/creator/api/zh/classes/View.html)）。我们可以用它的`setResolutionPolicy`方法来设置适配规则。（[ResolutionPolicy官方文档](https://docs.cocos.com/creator/api/zh/classes/ResolutionPolicy.html)）
* **EXACT_FIT** 整体拉伸画面撑满屏幕。
* **NO_BORDER** 不保持比例撑满屏幕，多出屏幕的部分就不会显示了。
* **SHOW_ALL** 保持比例撑满屏幕，如果设计比例和屏幕比例不同则周围会有黑边。
* **FIXED_HEIGHT** 保持游戏Canvas的height的设计分辨率，将width适配到屏幕的width。
* **FIXED_WIDTH** 保持游戏Canvas的width的设计分辨率，将height适配到屏幕的height。
#### 2.3. 最后使用的解决办法
用了上述`FIXED_HEIGHT`适配方案（应该是等同于编辑中Canvas下的fit height的）。这时候我的游戏画面的height在IphoneX屏幕上就以1080px计算，但是width就按IphoneX的比例适配到2338px。然后，游戏里的背景图就做到宽度2338，一些左右的ui就用widget组建关联到左右边。这样，游戏在IphoneX上也是全屏的效果了，画面还不会被拉伸。






