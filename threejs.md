# Threejs

# 1. GUI控制圆形的半径

```js
// 创建GUI对象
const gui = new GUI();
// 添加按钮
// 添加滑块控件到GUI对象
gui.add({ radius: 1 }, 'radius', 0, 10).onChange(function (newval) {
  // 更新圆形的半径
  scene.remove(circle); // 移除旧的圆
  circle.geometry.dispose(); // 清理旧几何体的资源,减少内存占用
  const newGeometry = new THREE.CircleGeometry(newval, 32); // 创建新的几何体
  circle.geometry = newGeometry; // 应用新的几何体到网格
  scene.add(circle); // 添加新的圆到场景
});
```

先移除之前的circle，之后清理旧的几何体占用的内存（`dispose()`），重新定义一个新的几何体，应用新的几何体到网络，添加新的圆到场景

threejs中几何体geometry只是代表了**形状**，martial代表了材质，Mesh是geometry和martial组合到一起之后的==**实体**==

