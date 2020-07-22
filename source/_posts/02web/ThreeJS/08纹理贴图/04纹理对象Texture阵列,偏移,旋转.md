---
title: 纹理对象Texture阵列,偏移,旋转
date: 2020-07-21 10:50:49
---

当图像贴图出现重复时，为了优化性能可以使用一张小的图片通过列阵以及便宜实现

# 阵列

## 重复

```js
var geometry = new THREE.PlaneGeometry(200, 100); //矩形平面
// TextureLoader创建一个纹理加载器对象，可以加载图片作为几何体纹理
var textureLoader = new THREE.TextureLoader();
var texture = textureLoader.load('太阳能板.png');
// 设置阵列模式   默认ClampToEdgeWrapping  RepeatWrapping：阵列  镜像阵列：MirroredRepeatWrapping
texture.wrapS = THREE.RepeatWrapping;
texture.wrapT = THREE.RepeatWrapping;
// uv两个方向纹理重复数量
texture.repeat.set(4, 2)
// 偏移效果
texture.offset = new THREE.Vector2(0.5, 0.5)
//材质对象Material
var material = new THREE.MeshLambertMaterial({
  // 设置纹理贴图：Texture对象作为材质map属性的属性值
  map: texture,
});
//网格模型对象Mesh
var mesh = new THREE.Mesh(geometry, material); 
//网格模型添加到场景中
scene.add(mesh); 
```

![阵列](./02.png)

## 偏移

```js
var textureLoader = new THREE.TextureLoader();
var texture = textureLoader.load('太阳能板2.png');// 加载纹理贴图
// 不设置重复  偏移范围-1~1
texture.offset = new THREE.Vector2(0.3, 0.1)
```

![偏移](./01.png)

## 纹理旋转

```js
var texture = textureLoader.load('太阳能板.png'); // 加载纹理贴图
// 设置纹理旋转角度
texture.rotation = Math.PI/4;
// 设置纹理的旋转中心，默认(0,0)
texture.center.set(0.5,0.5);
```

![纹理旋转](./03.png)

# 纹理动画

纹理动画比较简单，必须要在渲染函数中render()一直执行texture.offset.x -= 0.06动态改变纹理对象Texture的偏移属性.offset就可以。

```js
/**
 * 创建一个设置重复纹理的管道
 */
var curve = new THREE.CatmullRomCurve3([
  new THREE.Vector3(-80, -40, 0),
  new THREE.Vector3(-70, 40, 0),
  new THREE.Vector3(70, 40, 0),
  new THREE.Vector3(80, -40, 0)
]);
// 管道几何体
var tubeGeometry = new THREE.TubeGeometry(curve, 100, 0.6, 50, false);
var textureLoader = new THREE.TextureLoader();
var texture = textureLoader.load('run.jpg');
// 设置阵列模式为 RepeatWrapping
texture.wrapS = THREE.RepeatWrapping
texture.wrapT=THREE.RepeatWrapping
// 设置x方向的偏移(沿着管道路径方向)，y方向默认1
//等价texture.repeat= new THREE.Vector2(20,1)
texture.repeat.x = 20;
var tubeMaterial = new THREE.MeshPhongMaterial({
  map: texture,
  transparent: true,
});
var tube = new THREE.Mesh(tubeGeometry, tubeMaterial);
scene.add(tube)

// 渲染函数
function render() {
  renderer.render(scene, camera); //执行渲染操作
  requestAnimationFrame(render);
  // 使用加减法可以设置不同的运动方向
  // 设置纹理偏移
  texture.offset.x -= 0.06
}
```

![纹理动画](./01.gif)
