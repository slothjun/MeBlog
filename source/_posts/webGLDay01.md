---
title: webGlDay01
---
## WebGL 简介

webgl是一项可以在浏览器中流畅展示3D模型和场景的一种技术。
它使用JavaScript作为编程语言，调用浏览器支持的3D绘制函数，来实现3D模型和场景的展现。

## 为什么浏览器可以实现3D绘制

因为浏览器实现了opengl es的规范，这套规范可以直接使用指令操作显卡，使显卡渲染3D世界，直接反应到浏览器中。

操作流程： JavaScript ——> opengl es ——> 显卡

## webgl可以做什么

只要涉及到3D都有webgl的用武之地。比如游戏、家居、城市地图

## webgl之three.js

### three.js是什么

three.js是一个封装好的webgl库，使用它可以更便捷的使用webgl.

