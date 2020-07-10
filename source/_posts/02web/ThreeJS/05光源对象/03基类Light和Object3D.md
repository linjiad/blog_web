---
title: Light和Object3D
date: 2020-07-08 13:31:49
---

![Light和Object3D](./01.png)

# 光源位置属性

* PointLight的基类是Light
* Light的基类是Object3D
* 点光源自然继承对象Object3D的位置属性.position。

```js
var point = new THREE.PointLight(0xffffff);//点光源
//设置点光源位置  
//
//光源对象和模型对象的position属性一样是Vector3对象
point.position.set(400, 200, 300);
```

# 光源颜色属性和强度属性

* 光源颜色属性.color默认值是白色0xffffff,强度属性
* 强度属性.intensity默认1.0

```js
//环境光：颜色设置为`0xffffff`,强度系数设置为0.5
var ambient = new THREE.AmbientLight(0xffffff,0.5);
scene.add(ambient);
```