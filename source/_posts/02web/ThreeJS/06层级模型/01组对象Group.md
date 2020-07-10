---
title: 组对象Group
date: 2020-07-09 13:31:49
---

![组对象Group](./01.png)

# Group说明

## 模型添加到组中

```js
//创建两个网格模型mesh1、mesh2
var geometry = new THREE.BoxGeometry(20, 20, 20);
var material = new THREE.MeshLambertMaterial({color: 0x0000ff});
var group = new THREE.Group();
var mesh1 = new THREE.Mesh(geometry, material);
var mesh2 = new THREE.Mesh(geometry, material);
mesh2.translateX(25);
//把mesh1型插入到组group中，mesh1作为group的子对象
group.add(mesh1);
//把mesh2型插入到组group中，mesh2作为group的子对象
group.add(mesh2);
//把group插入到场景中作为场景子对象
scene.add(group);
```

## 组移动

```js
//沿着Y轴平移mesh1和mesh2的父对象，mesh1和mesh2跟着平移
group.translateY(100);
```

```js
//父对象缩放，子对象跟着缩放
group.scale.set(4,4,4);
```

```js
//父对象旋转，子对象跟着旋转
group.rotateY(Math.PI/6)
```

# Group案例

## 模型成线
```js
var group1 = new THREE.Group();
    // 共享材质和几何体数据，批量创建mesh
for (let i = 0; i < 10; i++) {
  var mesh = new THREE.Mesh(geometry, material); // 创建网格模型对象
  mesh.translateX(i * 25); // 平移该网格模型
  scene.add(mesh);//把网格模型插入到场景中
  // group1.add(mesh); //把网格模型插入到组group1中
}
```

![组对象Group](./03.png)

## 模型成面

```js
// group1沿着y轴方向阵列
var group2 = new THREE.Group();
for (let i = 0; i < 10; i++) {
  var newGroup = group1.clone(); // 克隆组group1
  newGroup.translateY(i * 25); //沿着z轴平移
  scene.add(newGroup); //场景中插入组group1克隆的对象
  // group2.add(newGroup); //group2中插入组group1克隆的对象
}
```

![组对象Group](./04.png)

## 模型成体

```js
// group2沿着z轴方向阵列
for (let i = 0; i < 10; i++) {
  var newGroup = group2.clone(); // 克隆组group2
  newGroup.translateZ(i * 25); //沿着z轴平移
  scene.add(newGroup); //场景中插入组group2的克隆对象
}
```

![组对象Group](./02.png)