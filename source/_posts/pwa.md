---
title: pwa
date: 2018-06-04 21:30:02
tags:
---
## 什么是pwa
Progressive Web App 渐进式webapp，是google2015年提出的前端应用形式，它采用一系列新的技术组合，让web应用
体现出和native应用相近的体验。

## pwa的作用及特点
1. 你只需要基于开放的 W3C 标准的 web 开发技术来开发一个app。不需要多客户端开发。
2. 用户可以在安装前就体验你的 app。
3. 不需要通过 AppStore 下载 app。app 会自动升级不需要用户升级。
4. 用户会受到‘安装’的提示，点击安装会增加一个图标到用户首屏。
5. 被打开时，PWA 会展示一个有吸引力的闪屏。
6. 必要的文件会被本地缓存，因此会比标准的web app 响应更快（也许也会比native app响应快）
7. 安装及其轻量 -- 或许会有几百 kb 的缓存数据。
8. 网站的数据传输必须是 https 连接。
9. PWA可以离线工作，并且在网络恢复时可以同步最新数据。

## 如何实现pwa
关键技术点
1. Manifest （应用清单）
2. Service Worker （可以理解为服务工厂）
3. Push Notification（推送通知）

## pwa注意点
刷新页面时，serviceworker.js 文件每次都会更新，更新后会进行安装(installed)，下载好新的资源文件
并缓存，并不会激活，掌管当前页面的仍然是就得serviceworker，当该网站的所有tab页关闭后，重新打开该网站，
新的serviceworker便会掌控该页面（actived），拦截请求，获取到新的缓存。

(参考资料1)[https://juejin.im/post/58dba0b3a22b9d00647db6c3]
(参考资料2)[https://juejin.im/post/5ae2f82f6fb9a07acd4d761e]