<template></template>

<script setup>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
import { GUI } from "three/examples/jsm/libs/lil-gui.module.min.js";

// 场景
const scene = new THREE.Scene();
// 相机
const camera = new THREE.PerspectiveCamera(
  45,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);
// 渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);
// 创建几何体
const geometry = new THREE.CircleGeometry(2, 32); // (半径，几条边)
// 创建材质
const material = new THREE.MeshBasicMaterial({
  color: 0xffff00,
  side: THREE.DoubleSide, // 顺时针逆时针都显示
  //   wireframe: true,
});
// 创建网格
const circle = new THREE.Mesh(geometry, material);
// 把网格添加到场景中
scene.add(circle);

// 设置相机位置
camera.position.x = 5;
camera.position.y = 10;
camera.position.z = 5;
// 添加世界坐标辅助器
const axesHelper = new THREE.AxesHelper(20);
scene.add(axesHelper);
// 添加轨道辅助器
const controls = new OrbitControls(camera, renderer.domElement);
// 设置阻尼惯性
controls.enableDamping = true;

// 定义一个渲染函数
function animate() {
  controls.update();
  requestAnimationFrame(animate);
  circle.rotation.x += 0.01;
  circle.rotation.y += 0.01;
  //   渲染出来
  renderer.render(scene, camera);
}
animate();

// 创建GUI对象
const gui = new GUI();
// 添加按钮
// 添加滑块控件到GUI对象
gui.add({ radius: 1 }, "radius", 0, 10).onChange(function (newval) {
  // 更新圆形的半径
  scene.remove(circle); // 移除旧的圆
  circle.geometry.dispose(); // 清理旧几何体的资源,减少内存占用
  const newGeometry = new THREE.CircleGeometry(newval, 32); // 创建新的几何体
  circle.geometry = newGeometry; // 应用新的几何体到网格
  scene.add(circle); // 添加新的圆到场景
});

</script>

<style scoped></style>
