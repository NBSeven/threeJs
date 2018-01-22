# three.js实践

前一阵子看到一个网站非常的酷炫，想估摸着自己也仿一个出来。
[链接](http://www.rolexawards.com/40/map)
看了一下其网站的脚本文件之后大致了解了这个网站是基于three.js做出来的。
[three.js官网](https://threejs.org/)
[three.js中文文档](http://techbrood.com/threejs/docs/)
如果是要做特效的话建议去看官网里的examples每个例子都可以预览且在github有源码。
接着我就开始看three.js的文档了，我只看了入门介绍，大致介绍了在3d环境中有scene(场景)、camera(摄像头)、renderer(渲染器)以及obj(3D物体)。
曾经用C4D做过一些3D动画所以还算是了解这些东西。不过很多细节上的东西也不是太懂，比如说
` new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 ); `
我知道最后两个参数是近裁剪面（near clipping plane） 和 远裁剪面（far clipping plane），也可以理解这是一个椎体，只不过这个参数值我怎么变都是一样啊 - -。。。资料少得可怜，希望有人能够指点一下  0 0 
还是进入正题吧，当我在看three.js的example时 看到一个[元素周期表](https://threejs.org/examples/#css3d_periodictable)，这尼玛不就是上面那网站的原型嘛 只不过位置不一样罢了。
随后赶紧clone了three.js仓库里的example文件夹。
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
  <script src="./TrackballControls.js"></script> //控制器有好几种，我的和例子中的控制器并不一样 例子里的是OrbitControls.js 我把文件也给放上来了
  <script src="./CSS3DRenderer.js"></script>

```
除了three.js以外还有tween.js 这是一个JS动画库，在这里不做讨论。
TrackballControls和CSS3DRenderer是和threejs相关的。
TrackballControls是一个控制器，也就是能让物体位置跟着鼠标的移动随时变化。
CSS3DRenderer 作用是能让dom元素继承 THREE.Object3D的方法和属性。
```
 var camera, scene, renderer;//摄像头，//场景 //渲染器
 var controls;// 控制器
 var imagesUrl = [
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg',
      './meinv.jpeg'
    ]//元素数组
 var objects = [];//存放3D元素的数组
 var newobjs = [];//存放3D对象的数组（每个对象都带有不同的位置，具体看下面的代码） 
    init();// 初始化
    animate();// 动画渲染

    function init() {
      camera = new THREE.PerspectiveCamera(40, window.innerWidth / window.innerHeight, 1, 10000);
      camera.position.z = 3000;//摄像机位置必须要设置，否则会出现转不动的情况
      scene = new THREE.Scene();
      imagesUrl.forEach(function (url, index) {
        let element = document.createElement('img');
        element.src = url;
        element.className = 'test';
        let object = new THREE.CSS3DObject(element);//把img元素转换成3d元素
        object.position.x = Math.random() * 4000 - 2000;// 一开始元素的随机位置 （至于这些参数怎么取值，我也不太懂就按源码的来了）
        object.position.y = Math.random() * 4000 - 2000;
        object.position.z = Math.random() * 4000 - 2000;
        scene.add(object);//在场景中添加这些对象
        objects.push(object);
      })
      var vector = new THREE.Vector3(); //创建一个三维向量
      var spherical = new THREE.Spherical();//创建一个球形对象

      for (var i = 0, l = objects.length; i < l; i++) {

        var phi = Math.acos(-1 + (2 * i) / l);//返回一个数的反余弦值
        var theta = Math.sqrt(l * Math.PI) * phi;//返回一个数的平方根

        var object = new THREE.Object3D();

        spherical.set(800, phi, theta);

        object.position.setFromSpherical(spherical);

        vector.copy(object.position).multiplyScalar(2);// 不挣扎了 ，不懂图形学得我 看了也是不明白~

        object.lookAt(vector);

        newobjs.push(object); //把构建球形对象的基本对象放到新数组里

      }


      renderer = new THREE.CSS3DRenderer();// 创建渲染器
      renderer.setSize(window.innerWidth, window.innerHeight);//设置渲染的区域
      renderer.domElement.style.position = 'absolute';
      document.getElementById('container').appendChild(renderer.domElement);//把渲染器添加到页面中
      controls = new THREE.TrackballControls(camera, renderer.domElement);//创建控制器，随着摄像机和渲染器？。。。
      controls.rotateSpeed = 0.5;
      controls.minDistance = 500;//控制器的最小距离
      controls.maxDistance = 6000;//控制器的最大距离
      controls.addEventListener('change', render);//添加监听函数，控制器改变时调用reder函数
      transform(newobjs, 2000);//执行3D对象的位置变化
      window.addEventListener('resize', onWindowResize, false);// 窗口变化时执行onWindowResize函数


    function transform(targets, duration) {

      TWEEN.removeAll();

      for (var i = 0; i < objects.length; i++) {

        var object = objects[i];
        var target = targets[i];
       // 进行位置的改变
        new TWEEN.Tween(object.position)
          .to({
            x: target.position.x,
            y: target.position.y,
            z: target.position.z
          }, Math.random() * duration + duration)
          .easing(TWEEN.Easing.Exponential.InOut)
          .start();

        new TWEEN.Tween(object.rotation)
          .to({
            x: target.rotation.x,
            y: target.rotation.y,
            z: target.rotation.z
          }, Math.random() * duration + duration)
          .easing(TWEEN.Easing.Exponential.InOut)
          .start();

      }

      new TWEEN.Tween(this)
        .to({}, duration * 2)
        .onUpdate(render)
        .start();

    }

    function onWindowResize() {

      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();

      renderer.setSize(window.innerWidth, window.innerHeight);

      render();

    }

    function animate() {

      requestAnimationFrame(animate);

      TWEEN.update();

      controls.update();

    }

    function render() {

      renderer.render(scene, camera);

    }
  }
```
这里也有一个很好的教程
[教程链接](https://www.cnblogs.com/createGod/p/7004428.html)
PS：建议用最新的three.js不同版本的相关文件可能会造成一些问题。