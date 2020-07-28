---
title: stl格式模型加载
date: 2020-07-28 10:30:49
---

* 基本所有的三维软件都支持导出.stl格式的三维模型文件
* .stl格式的三维模型不包含材质Material信息,只包含几何体顶点数据的信息
* 可以简单地把stl文件理解为几何体对象Geometry

# stl文件数据结构

## 三角形面信息

```stylus
//三角面1
   facet normal 0 0 -1    //三角形面法向量
      outer loop
         vertex 50 50 -50   //顶点位置
         vertex 50 -50 -50  //顶点位置
         vertex -50 50 -50  //顶点位置
      endloop
   endfacet
```

## 立方体

* 一个立方体有6个矩形平面
* 每个矩形平面至少需要两个三角形拼接而成
* 方体6个矩形平面至少需要12个三角形面构成
* 查看文件box.STL中的12个三角形信息

```stylus
solid box  //文件名字
//三角面1
   facet normal 0 0 -1    //三角形面法向量
      outer loop
         vertex 50 50 -50   //顶点位置
         vertex 50 -50 -50  //顶点位置
         vertex -50 50 -50  //顶点位置
      endloop
   endfacet
//三角面2
   facet normal 0 0 -1    //三角形面法向量
      outer loop
         vertex -50 50 -50   //顶点位置
         vertex 50 -50 -50   //顶点位置
         vertex -50 -50 -50  //顶点位置
      endloop
   endfacet
   facet normal 0 1 0
      .....
      .....
//三角面12
   facet normal -1 0 0
      outer loop
         vertex -50 -50 -50
         vertex -50 50 50
         vertex -50 50 -50
      endloop
   endfacet
endsolid
```

# 通过STLLoader.js加载.stl文件

* 通过Threejs加载.stl格式三维模型文件，可以使用Threejs提供的一个扩展库stl加载器STLLoader.js
* 在Three.js-master包中找到STLLoader.js文件,具体路径是three.js-master\examples\js\loaders

## 加载外部文件渲染正方体

* THREE.STLLoader()实例化一个加载器
```html
<!--引入STLLoader.js文件-->
<script src="STLLoader.js"></script>
```
* 通过构造函数THREE.STLLoader()可以把.stl文件中几何体顶点信息提取出来转化为Three.js自身格式的几何体对象BufferGeometry
```js
// THREE.STLLoader创建一个加载器
var loader = new THREE.STLLoader();
loader.load('立方体.stl',function (geometry) {
  // 加载完成后会返回一个几何体对象BufferGeometry，你可以通过Mesh、Points等方式渲染该几何体
})
```

```js
/**
 * stl数据加载
 */
var loader = new THREE.STLLoader();
// 立方体默认尺寸长宽高各200
loader.load('立方体.stl',function (geometry) {
  // 控制台查看加载放回的threejs对象结构
  console.log(geometry);
  // 查看顶点数，一个立方体6个矩形面，每个矩形面至少2个三角面，每个三角面3个顶点，
  // 如果没有索引index复用顶点，就是说一个立方体至少36个顶点
  console.log(geometry.attributes.position.count);
  // 缩放几何体
  // geometry.scale(0.5,0.5,0.5);
  // 几何体居中
  // geometry.center();
  // 平移立方体
  // geometry.translate(-50,-50,-50);
  var material = new THREE.MeshLambertMaterial({
    color: 0x0000ff,
  }); //材质对象Material
  var mesh = new THREE.Mesh(geometry, material); //网格模型对象Mesh
  scene.add(mesh); //网格模型添加到场景中
})
```

## 使用点模型渲染STL文件

```js
/**
 * 点渲染模式
 */
 loader.load('离心叶轮.stl',function (geometry) {
   var material = new THREE.PointsMaterial({
     color: 0x000000,
     size: 0.5//点对象像素尺寸
   }); //材质对象
   var points = new THREE.Points(geometry, material); //点模型对象
   scene.add(points); //点对象添加到场景中
 })
```