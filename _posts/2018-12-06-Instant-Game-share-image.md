---
layout: post
title: Instant Game中分享带有头像和分数的图片
date: 2018-12-06
excerpt: "在用cocos creator开发facebook instant game时遇到的分享图片的坑。"
tags: [facebook, instant game, cocos creator]
comments: true
---

### 坑
之前用cocos creator开发facebook instant game时，要用到`shareAsync()`和`chooseAsync()`接口去分享或者挑战好友。这两个接口中都会用到image参数，是用于设置分享的图片的，需要填写base64的图片数据。

开始我是直接用网页工具直接将图片转换成base64数据填入。那一长串的字符看着就很烦，而且还无法动态的将facebook头像和分数附加在图片上，网上找了很久都没找到合适的解决方案。最后，结合网上资料总结出这个方法，不知道是否合理，但是很有效。

### 踩

大体思路就是用Canvas直接绘制一张图片，然后分享这张图片就行。

~~~ typescript
    //创建canvas
    let shareCanvas = document.createElement("canvas");
    shareCanvas.id = 'shareCanvas';
    shareCanvas.width = 600;
    shareCanvas.height = 315;
    let shareCanvasCtx = shareCanvas.getContext("2d");

    //加载facebook头像图片
    var playerImage = new Image();
    playerImage.crossOrigin = 'anonymous';
    playerImage.src = FBInstant.player.getPhoto();
    playerImage.onload = () => {

        //加载分享的背景图片
        let image = new Image();
        image.src = "res/raw-assets/Texture/LargeImage/ShareImg.jpg";//cocos项目背景图片的路径
        image.onload = () => {
            //画背景图片
            shareCanvasCtx.drawImage(image, 0, 0, shareCanvas.width, shareCanvas.height);

            //画头像的背景白底
            shareCanvasCtx.beginPath();
            shareCanvasCtx.arc(125, 125, 80, 0, 2 * Math.PI, false); 
            shareCanvasCtx.fillStyle = "white";
            shareCanvasCtx.fill();
            shareCanvasCtx.closePath();

            //画头像并截成圆形
            shareCanvasCtx.save();
            shareCanvasCtx.beginPath();
            shareCanvasCtx.arc(125, 125, 75, 0, 2 * Math.PI, false); 
            shareCanvasCtx.clip(); 
            shareCanvasCtx.stroke();
            shareCanvasCtx.closePath();
            shareCanvasCtx.drawImage(playerImage, 50, 50, 150, 150);
            shareCanvasCtx.restore();

            //写上分数
            shareCanvasCtx.font = "60px Paytone One";
            shareCanvasCtx.fillStyle = "white";
            shareCanvasCtx.textAlign = "center";
            shareCanvasCtx.lineWidth = 8;
            shareCanvasCtx.strokeStyle = "#768DFF";
            shareCanvasCtx.strokeText(score.toString(), 475, 175);
            shareCanvasCtx.fillText(score.toString(), 475, 175);
            shareCanvasCtx.font = "30px Paytone One";
            shareCanvasCtx.strokeText("SCORE", 475, 100);
            shareCanvasCtx.fillText("SCORE", 475, 100);

            // shareCanvas.toDataURL()获得canvas的base64图片数据
            shareCanvas = null;
        };
    };
~~~




