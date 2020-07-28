---
title: Three.js数据结构导入导出
date: 2020-07-28 11:10:49
---

通过Threejs先加载一个.FBX格式的三维模型文件，然后解析该文件中的骨骼动画信息。

# 加载器FBXLoader.js

引入FBX加载器相关文件

```html
<!-- 引入fbx模型加载库FBXLoader -->
<script src="http://www.yanhuangxueyuan.com/versions/threejsR92/examples/js/loaders/FBXLoader.js"></script>
<!-- 辅助文件 -->
<script src="http://www.yanhuangxueyuan.com/versions/threejsR92/examples/js/libs/inflate.min.js"></script>
```

# 加载fbx模型文件

加载模型文件，加载完成后，如果模型显示位置不符合要求，可以让3D美术修改，也可以通过Threejs程序进行平移、缩放等操作。

```js
var loader = new THREE.FBXLoader();//创建一个FBX加载器
loader.load("SambaDancing.fbx", function(obj) {
  // console.log(obj);//查看加载后返回的模型对象
  scene.add(obj)
  // 适当平移fbx模型位置
  obj.translateY(-80);
})
```

# FBX模型帧动画数据

* stl、obj都是静态模型，不可以包含动画
* fbx除了包含几何、材质信息，可以存储骨骼动画等数据
* obj.animations属性的数组包含两个剪辑对象AnimationClip
   * obj.animations[0]对应剪辑对象AnimationClip包含多组关键帧KeyframeTrack数据
   * obj.animations[1]对应的剪辑对象AnimationClip没有关键帧数据，也就是说没有关键帧动画
* 具体的开发中，可能美术提供的模型有很多包含关键帧动画的剪辑对象AnimationClip，你可以根据自己的需要解析某个剪辑对象AnimationClip对应的动画

```js
var loader = new THREE.FBXLoader();//创建一个FBX加载器
loader.load("SambaDancing.fbx", function(obj) {
  ...
  // 可以在控制台打印obj对象，找到animations属性
  console.log(obj)
  // 查看动画数据  2个剪辑对象AnimationClip，一个有关键帧动画，一个没有
  console.log(obj.animations)

})
```

# 解析fbx模型骨骼动画

```js
var mixer=null;//声明一个混合器变量
var loader = new THREE.FBXLoader();//创建一个FBX加载器
loader.load("SambaDancing.fbx", function(obj) {
  // console.log(obj)
  scene.add(obj)
  obj.translateY(-80);
  // obj作为参数创建一个混合器，解析播放obj及其子对象包含的动画数据
  mixer = new THREE.AnimationMixer(obj);
  // 查看动画数据
  console.log(obj.animations)
  // obj.animations[0]：获得剪辑对象clip
  var AnimationAction=mixer.clipAction(obj.animations[0]);
  // AnimationAction.timeScale = 1; //默认1，可以调节播放速度
  // AnimationAction.loop = THREE.LoopOnce; //不循环播放
  // AnimationAction.clampWhenFinished=true;//暂停在最后一帧播放的状态
  AnimationAction.play();//播放动画
})
```

```js
// 创建一个时钟对象Clock
var clock = new THREE.Clock();
// 渲染函数
function render() {
  renderer.render(scene, camera); //执行渲染操作
  requestAnimationFrame(render); //请求再次执行渲染函数render，渲染下一帧

  if (mixer !== null) {
    //clock.getDelta()方法获得两帧的时间间隔
    // 更新混合器相关的时间
    mixer.update(clock.getDelta());
  }
}
render();
```