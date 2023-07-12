<template></template>

<script setup>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
import { GUI } from "three/examples/jsm/libs/lil-gui.module.min.js";

// 创建场景
const scene = new THREE.Scene();
// 创建相机
const camera = new THREE.PerspectiveCamera(
  45, // 视角(眼睛看到的角度范围)
  window.innerWidth / window.innerHeight, // 宽高比
  0.1, // 最近可以看到
  1000 // 最远可以看到
);
// 创建渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

// 创建一个不同面不同颜色的立方体
const cubegeometry = new THREE.BoxGeometry(1, 1, 1);
// 创建材质
const cubeMaterial0 = new THREE.MeshBasicMaterial({
  color: 0xff0000f,
});
const cubeMaterial1 = new THREE.MeshBasicMaterial({
  color: 0xff0000,
});
const cubeMaterial2 = new THREE.MeshBasicMaterial({
  color: 0x0000ff,
});
const cubeMaterial3 = new THREE.MeshBasicMaterial({
  color: 0xffff00,
});
const cubeMaterial4 = new THREE.MeshBasicMaterial({
  color: 0x00ffff,
});
const cubeMaterial5 = new THREE.MeshBasicMaterial({
  color: 0xff00ff,
});
// 创建网格
const cube = new THREE.Mesh(cubegeometry, [
  cubeMaterial0,
  cubeMaterial1,
  cubeMaterial2,
  cubeMaterial3,
  cubeMaterial4,
  cubeMaterial5,
]);
scene.add(cube);

// 创建一个不同颜色的面
const geometry = new THREE.BufferGeometry();
// 创建顶点属性
const vertices = new Float32Array([
  -1.0, -1.0, 0.0, 1.0, -1.0, 0.0, 1.0, 1.0, 0.0, -1.0, 1.0, 0,
]);
// 创建顶点属性
geometry.setAttribute("position", new THREE.BufferAttribute(vertices, 3));
// 创建索引
const indices = new Uint16Array([0, 1, 2, 2, 3, 0]);
// 创建索引属性
geometry.setIndex(new THREE.BufferAttribute(indices, 1));
// 设置两个顶点组，形成两个材质
geometry.addGroup(0, 3, 0);
geometry.addGroup(3, 3, 1);

// 创建材质
const material0 = new THREE.MeshBasicMaterial({
  color: 0xffff00,
});
const material1 = new THREE.MeshBasicMaterial({
  color: 0xff00ff,
});
const plane = new THREE.Mesh(geometry, [material0, material1]);
plane.position.set(1, -1, -3);
scene.add(plane);
// 设置相机位置
camera.position.z = 5;
camera.position.y = 2;
camera.position.x = 2;
camera.lookAt(0, 0, 0);

// 添加世界坐标辅助器
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);

// 添加轨道控制器
const controls = new OrbitControls(camera, renderer.domElement);
// 设置带阻尼的惯性
controls.enableDamping = true;
// 设置阻尼系数
controls.dampingFactor = 0.05;
// 设置旋转速度
// controls.autoRotate = true;

// 渲染函数
function animate() {
  controls.update();
  requestAnimationFrame(animate);
  // 渲染
  renderer.render(scene, camera);
}
animate();

// 监听窗口变化
window.addEventListener("resize", () => {
  // 重置渲染器宽高比
  renderer.setSize(window.innerWidth, window.innerHeight);
  // 重置相机宽高比
  camera.aspect = window.innerWidth / window.innerHeight;
  // 更新相机投影矩阵
  camera.updateProjectionMatrix();
});
</script>

<style scoped></style>
