# Three.js练习

>前一阵子看到一个网站非常的酷炫，想研究一下是如何实现的

[网站链接](http://www.rolexawards.com/40/map)

[我的链接](https://htmlpreview.github.io/?https://github.com/NBSeven/threeJs/blob/master/realmap.html)

这里安利一波 [dgit](https://github.com/hujiulong/dgit)
一个用来下载git仓库文件的命令行工具
git会记录一个项目所有的历史修改，所以通常会很大。有时候你只需要下载一个项目的源代码，或者仅仅只需要其中的一个目录或文件，但是github不提供这样的功能，你需要先clone整个项目，非常麻烦。
这个小工具可以帮你解决这些问题，它包含这些功能：
1.下载一个git仓库的所有文件
2.下载一个git仓库的某个目录或某个文件
3.下载时可以指定分支/tag/commit hash

因为three.js项目比较庞大，仓库里并不是所有都需要用上的。这里我们只需要examples文件夹和build就够啦。
我们查看一下脚本
```
  <script src="./three.js"></script>
  <script src="./tween.min.js"></script>
  <script src="./TrackballControls.js"></script> //控制器有好几种，我的和例子中的控制器并不一样 例子里的是OrbitControls.js 我把文件也给放上来了
  <script src="./CSS3DRenderer.js"></script>
```
除了three.js以外还有tween.js 这是一个JS动画库，在这里不做讨论。
TrackballControls和CSS3DRenderer是和threejs相关的。
TrackballControls是一个控制器，也就是能让物体位置跟着鼠标的移动随时变化。
CSS3DRenderer 作用是能让dom元素继承 THREE.Object3D的方法和属性。

[教程链接](https://www.cnblogs.com/createGod/p/7004428.html)

PS：建议用最新的three.js不同版本的相关文件可能会造成一些问题。
