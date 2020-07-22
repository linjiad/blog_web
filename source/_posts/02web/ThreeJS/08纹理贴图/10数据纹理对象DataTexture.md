---
title: 数据纹理对象DataTexture
date: 2020-07-22 13:15:49
---

Three.js数据纹理对象DataTexture简单地说就是通过程序创建纹理贴图的每一个像素值。

# 程序生成一张图片的RGB值

```js
var scene = new THREE.Scene();
var geometry = new THREE.PlaneGeometry(128, 128); //矩形平面
/**
 * 创建纹理对象的像素数据
 */
var width = 32; //纹理宽度
var height = 32; //纹理高度
var size = width * height; //像素大小
// 创建初始化为0的，包含length个元素的无符号整型数组
var data = new Uint8Array(size * 3); //size*3：像素在缓冲区占用空间
for (let i = 0; i < size * 3; i += 3) {
  // 随机设置RGB分量的值
  data[i] = 255 * Math.random()
  data[i + 1] = 255 * Math.random()
  data[i + 2] = 255 * Math.random()
}
// 创建数据文理对象   RGB格式：THREE.RGBFormat
var texture = new THREE.DataTexture(data, width, height, THREE.RGBFormat);
texture.needsUpdate = true; //纹理更新

var material = new THREE.MeshPhongMaterial({
  map: texture, // 设置纹理贴图
}); //材质对象Material
var mesh = new THREE.Mesh(geometry, material); //网格模型对象Mesh
scene.add(mesh); //网格模型添加到场景中
```

![程序生成一张图片的RGB值](./01.jpg)

# 程序生成一张图片的RGBA值

```js
var scene = new THREE.Scene();
var geometry = new THREE.PlaneGeometry(128, 128); //矩形平面
/**
 * 创建纹理对象的像素数据
 */
var width = 32; //纹理宽度
var height = 32; //纹理高度
var size = width * height; //像素大小
var data = new Uint8Array(size * 4); //size*4：像素在缓冲区占用空间
for (let i = 0; i < size * 4; i += 4) {
  // 随机设置RGB分量的值
  data[i] = 255 * Math.random()
  data[i + 1] = 255 * Math.random()
  data[i + 2] = 255 * Math.random()
  // 设置透明度分量A
  data[i + 3] = 255 * 0.5
}
// 创建数据文理对象   RGBA格式：THREE.RGBAFormat
var texture = new THREE.DataTexture(data, width, height, THREE.RGBAFormat);
texture.needsUpdate = true; //纹理更新

var material = new THREE.MeshPhongMaterial({
  map: texture, // 设置纹理贴图
  transparent:true,//允许透明设置
}); //材质对象Material
var mesh = new THREE.Mesh(geometry, material); //网格模型对象Mesh
scene.add(mesh); //网格模型添加到场景中
```

![程序生成一张图片的RGBA值](./02.jpg)
