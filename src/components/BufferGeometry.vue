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

// 创建几何体
const geometry = new THREE.BufferGeometry();

// 不适用索引的方式公用顶点
// // 创建顶点数据,顶点是有顺序的，逆时针为正面可见，顺时针为反面可见
// const vertices = new Float32Array([
//   -1.0, -1.0, 0.0, 1.0, -1.0, 0.0, 1.0, 1.0, 0.0, 1.0, 1.0, 0.0, -1.0, 1.0, 0.0,
//   -1.0, -1.0, 0.0,
// ]);
// // 创建顶点属性
// geometry.setAttribute("position", new THREE.BufferAttribute(vertices, 3));

// 适用索引的方式公用顶点
const vertices = new Float32Array([
  -1.0, -1.0, 0.0, 1.0, -1.0, 0.0, 1.0, 1.0, 0.0, -1.0, 1.0, 0.0,
]);

// 创建顶点属性
geometry.setAttribute("position", new THREE.BufferAttribute(vertices, 3));
// 创建索引
const indices = new Uint16Array([0, 1, 2, 2, 3, 0]);
// 创建索引属性
geometry.setIndex(new THREE.BufferAttribute(indices, 1));

// 创建材质
const material = new THREE.MeshBasicMaterial({
  color: 0x00ff00,
  side: THREE.DoubleSide, // 设置两面都可以看到图案'
  wireframe: true, // 设置线框模式
});

console.log(geometry);

// 创建网格
const plane = new THREE.Mesh(geometry, material);

scene.add(plane);

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
  //   plane.rotation.x += 0.01;
  //   plane.rotation.y += 0.01;
  // 渲染场景
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
