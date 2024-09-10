<script setup>
import * as THREE from "three";
import { OrbitControls } from "three/addons/controls/OrbitControls.js";
//创建场景
const scene = new THREE.Scene();
//创建相机
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);

// 创建渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);
//创建立方体
const geometry = new THREE.BoxGeometry(1, 1, 1);
//材质
const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
//父元素

const parentMaterial = new THREE.MeshBasicMaterial({ color: 0xff0000 });
const parentCube = new THREE.Mesh(geometry, parentMaterial);
parentCube.position.set(-2, 0, 0);
//网格
const cube = new THREE.Mesh(geometry, material);
// set是三维物体（Object3D）的属性 是相对位置 如果没有父元素 xy 就是他的
cube.position.set(2, 0, 0);

scene.add(parentCube);
parentCube.add(cube);

camera.position.z = 5;
camera.position.y = 2;
camera.position.x = 2;

// 坐标辅助器 绿色y 红色x 蓝色z
const axeHelper = new THREE.AxesHelper(5);
scene.add(axeHelper);
//轨道控制器 可以使得相机围绕目标进行轨道运动
// renderer.domElement
const controls = new OrbitControls(camera, document.body);
// controls.update();
controls.autoRotate = true;
//设置阻尼的惯性
controls.enableDamping = true;
controls.dampingFactor = 0.08;
//渲染场景
function animate() {
  requestAnimationFrame(animate);
  // cube.rotation.x += 0.01;
  // cube.rotation.y += 0.01;
  controls.update();
  renderer.render(scene, camera);
}
animate();
</script>

<template>
  <div></div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
}
canvas {
  display: block;
  position: fixed;
  left: 0;
  height: 0;
  width: 100vw;
  height: 100vh;
}
</style>
