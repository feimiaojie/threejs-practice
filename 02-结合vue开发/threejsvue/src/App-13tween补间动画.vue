<script setup>
import * as THREE from "three";
import { OrbitControls } from "three/addons/controls/OrbitControls.js";
import { GUI } from 'three/addons/libs/lil-gui.module.min.js';
import * as TWEEN from "three/addons/libs/tween.module.js";
import "./style.css";

// 创建场景
const scene = new THREE.Scene();
scene.background = new THREE.Color(0x888888);
// 创建相机
const camera = new THREE.PerspectiveCamera(
  75, // 视角
  window.innerWidth / window.innerHeight, // 宽高比
  0.1, //近平面
  1000 // 远平面
);
// 创建渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

const sphere = new THREE.Mesh(
  new THREE.SphereGeometry(1, 32, 32),
  new THREE.MeshBasicMaterial({ color: 0x00ff00 })
);
sphere.position.set(-4, 0, 0);
scene.add(sphere);

const tween = new TWEEN.Tween(sphere.position)
// tween.repeat(Infinity)
tween.to({x: 4}, 1000)
// tween.yoyo(true)
tween.easing(TWEEN.Easing.Quadratic.InOut)

const tween2 = new TWEEN.Tween(sphere.position)
tween2.to({x: -4}, 1000)
tween.chain(tween2)
tween2.chain(tween)

tween.start()

tween.onStart(()=>{
  console.log('开始')
})

tween.onStop(()=>{
  console.log('停止')
})

tween.onComplete(()=>{
  console.log('结束')
})

tween.onUpdate(()=>{
  console.log('更新')
})
//设置辅助轴线
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);

const gui = new GUI()
const paramObject = {
  stop: function(){
    tween.stop()
  },
  start: function(){
    tween.start()
  }
}
gui.add(paramObject, 'stop')
gui.add(paramObject, 'start')
// 设置相机位置
camera.position.z = 12;
camera.position.x = 12;
camera.position.y = 12;
camera.lookAt(0, 0, 0);

// 控制器
const controls = new OrbitControls(camera, renderer.domElement);
function animate() {
  controls.update();
  requestAnimationFrame(animate);

  // 渲染
  renderer.render(scene, camera);
  TWEEN.update();
}
animate();

window.addEventListener("resize", () => {
  renderer.setSize(window.innerWidth, window.innerHeight);
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
});
</script>

<template>
  <div></div>
</template>

<style scoped></style>
