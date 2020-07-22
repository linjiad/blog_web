---
title: 精灵模型对象Sprite
date: 2020-07-22 14:15:49
---

# 概念

* Three.js的精灵模型对象Sprite和Threejs的网格模型Mesh一样都是模型对象
* 创建精灵模型对象Sprite和创建网格模型对象一样需要创建一个材质对象
* 不同的地方在于创建精灵模型对象不需要创建几何体对象Geometry
* 精灵模型对象本质上你可以理解为已经内部封装了一个平面矩形几何体PlaneGeometry
* 矩形精灵模型与矩形网格模型的区别在于精灵模型的矩形平面会始终平行于Canvas画布。

# 创建精灵模型

* 通过Sprite创建精灵模型不需要几何体
* 只需要给构造函数Sprite的参数设置为一个精灵材质SpriteMaterial即可。

```js
// 加载纹理贴图
var texture = new THREE.TextureLoader().load("sprite.png");
// 创建精灵材质对象SpriteMaterial
var spriteMaterial = new THREE.SpriteMaterial({
  // color:0xff00ff,//设置精灵矩形区域颜色
  rotation:Math.PI/4,//旋转精灵对象45度，弧度值
  map: texture,//设置精灵纹理贴图
});
// 创建精灵模型对象，不需要几何体geometry参数
var sprite = new THREE.Sprite(spriteMaterial);
scene.add(sprite);
// 控制精灵大小，比如可视化中精灵大小表征数据大小
sprite.scale.set(40, 40, 1); //// 只需要设置x、y两个分量就可以
```

![创建精灵模型](./01.png)

## .scale和.position

* 精灵模型对象和网格模型Mesh对一样基类都是Object3D
* 精灵模型也有缩放属性.scale和位置属性.position
* 一般设置精灵模型的大小是通过.scale属性实现
* 而精灵模型的位置通过属性.position实现
* 精灵模型和普通模型一样，可以改变它在三维场景中的位置
* 区别在于精灵模型的正面一直平行于canvas画布。