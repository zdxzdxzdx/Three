<script setup>
import * as THREE from "three";
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
//网格
const cube = new THREE.Mesh(geometry, material);

scene.add(cube);
camera.position.z = 5;
//渲染场景
function animate() {
  console.log(11);
  requestAnimationFrame(animate);
  cube.rotation.x += 0.01;
  cube.rotation.y += 0.01;
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
