<script setup>
import * as THREE from "three";
//导入轨道控制器
import { OrbitControls } from "three/addons/controls/OrbitControls.js";
// 导入lil.gui
import { GUI } from "three/examples/jsm/libs/lil-gui.module.min.js";

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
// const geometry = new THREE.BoxGeometry(1, 1, 1);
// //材质
// const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
//父元素

//创建三角形平面
const geometry = new THREE.BufferGeometry();
//创建顶点数据 顶点是有序的，每三个为一个顶点，逆时针为正面
// const vertices = new Float32Array([
//   -1.0, -1.0, 0.0, 1.0, -1.0, 0.0, 1.0, 1.0, 0.0,
// ]);

//创建顶点属性
//geometry.setAttribute("position", new THREE.BufferAttribute(vertices, 3));

//共用顶点,创建索引绘制
const vertices = new Float32Array([
  -1.0, -1.0, 0.0, 1.0, -1.0, 0.0, 1.0, 1.0, 0.0, -1.0, 1.0, 0.0,
]);
geometry.setAttribute("position", new THREE.BufferAttribute(vertices, 3));
//坐标的索引值 三个为一个索引
const indices = new Uint16Array([0, 1, 2, 2, 3, 0]);
geometry.setIndex(new THREE.BufferAttribute(indices, 1));
const material = new THREE.MeshBasicMaterial({
  color: 0x00ff00,
  //side: THREE.DoubleSide, //设置两面都能看到
  wireframe: true,
});
const plane = new THREE.Mesh(geometry, material);
scene.add(plane);
console.log(geometry, "geometry");
const parentMaterial = new THREE.MeshBasicMaterial({ color: 0xff0000 });
//设置父元素材质为线框模式
parentMaterial.wireframe = true;
const parentCube = new THREE.Mesh(geometry, parentMaterial);
parentCube.position.set(-2, 0, 0);
// parentCube.scale.set(2, 2, 2);
parentCube.rotation.set(45, 0, 0);
//网格
const cube = new THREE.Mesh(geometry, material);
// set是三维物体（Object3D）的属性 是相对位置 如果没有父元素 xy 就是他的 如果有父,在父的基础上计算
cube.position.set(2, 0, 0);
//xyz放大两倍 如果父元素也放大两倍,要先有父的两倍再加上自己两倍 一共放大四倍
// cube.scale.set(2, 2, 2);
//在x轴旋转45度  如果父也旋转45 在父的基础上 也就旋转90
cube.rotation.set(45, 0, 0);
scene.add(parentCube);
// parentCube.add(cube);

camera.position.z = 5;
camera.position.y = 2;
camera.position.x = 2;

// 坐标辅助器 绿色y 红色x 蓝色z
const axeHelper = new THREE.AxesHelper(5);
scene.add(axeHelper);
//轨道控制器 可以使得相机围绕目标进行轨道运动
// document.body
const controls = new OrbitControls(camera, renderer.domElement);
//自动旋转
// controls.autoRotate = true;
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
//监听画面变化，更新渲染画面
window.addEventListener("resize", () => {
  //重置渲染器宽高比
  renderer.setSize(window.innerWidth, window.innerHeight);
  //重置相机宽高比
  camera.aspect = window.innerWidth / innerHeight;
  //更新相机投影矩阵
  camera.updateProjectionMatrix();
});
// var button = document.createElement("button");
// button.innerHTML = "点击全屏";
// button.style.position = "relative";
// button.style.left = "10px";
// button.style.top = "10px";
// document.body.appendChild(button);
// button.onclick = function () {
//   //全屏
//   document.body.requestFullscreen();
// };

// var exitbutton = document.createElement("button");
// exitbutton.innerHTML = "退出全屏";
// exitbutton.style.position = "relative";
// exitbutton.style.left = "20px";
// exitbutton.style.top = "10px";
// document.body.appendChild(exitbutton);
// exitbutton.onclick = function () {
//   //退出全屏
//   document.exitFullscreen();
// };
// 使用lil 实例化gui 相当于代替上面的创建按钮
let gui = new GUI();
let eventObj = {
  Fullscreen: function () {
    document.body.requestFullscreen();
  },
  exitFullscreen: function () {
    document.exitFullscreen();
  },
};
gui.add(eventObj, "Fullscreen").name("全屏");
gui.add(eventObj, "exitFullscreen").name("退出全屏");
// gui.add(cube.position, "x", -5, 5).name("立方体x轴位置");
// gui.add(cube.position, "x").min(-10).max(10).step(1).name("立方体x轴位置");
//放到一个文件下
let folder = gui.addFolder("立方体位置");
folder
  .add(cube.position, "x")
  .min(-10)
  .max(10)
  .step(1)
  .name("立方体x轴位置")
  .onChange((val) => {
    console.log("立方体x轴位置", val);
  });

folder
  .add(cube.position, "y")
  .min(-10)
  .max(10)
  .step(1)
  .name("立方体y轴位置")
  .onFinishChange((val) => {
    console.log("立方体y轴最终位置", val);
  });

//设置父元素是否线框
gui.add(parentMaterial, "wireframe").name("父元素线框模式");
let colorParams = {
  cubecolor: "#ff0000",
};
gui
  .addColor(colorParams, "cubecolor")
  .name("立方体颜色")
  .onChange((val) => {
    cube.material.color.set(val);
  });
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
