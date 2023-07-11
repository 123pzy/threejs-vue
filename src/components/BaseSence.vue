<template></template>

<script setup>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
import { GUI } from "three/examples/jsm/libs/lil-gui.module.min.js";

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(
  45,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

// 创建一个立方体
const geometry = new THREE.BoxGeometry(2, 2, 2);

const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
// 创建网格
const cube = new THREE.Mesh(geometry, material);

scene.add(cube);

// 设置相机的位置
camera.position.z = 5;
camera.position.y = 10;
camera.position.x = 5;
camera.lookAt(0, 0, 0); // 看向原点（默认）

// 添加世界坐标辅助器
const axesHelper = new THREE.AxesHelper(20);
scene.add(axesHelper);
// 添加轨道控制器
const controls = new OrbitControls(camera, renderer.domElement);
controls.enableDamping = true;

// 定义渲染函数
function animate() {
  controls.update();
  requestAnimationFrame(animate);
  cube.rotation.x += 0.01;
  cube.rotation.y += 0.01;
  renderer.render(scene, camera);
}
animate();

// 监听窗口的变化进行渲染
window.addEventListener("resize", () => {
  renderer.setSize(window.innerWidth, window.innerHeight);
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
});
</script>

<style scoped></style>
