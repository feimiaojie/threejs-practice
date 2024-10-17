<script setup>
import * as THREE from "three";
import { OrbitControls } from "three/addons/controls/OrbitControls.js";
import { GUI } from "three/addons/libs/lil-gui.module.min.js";
import { DRACOLoader } from "three/addons/loaders/DRACOLoader.js";
import { GLTFLoader } from "three/addons/loaders/GLTFLoader.js";
import { RGBELoader } from "three/addons/loaders/RGBELoader.js";
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

const sphere1 = new THREE.Mesh(
  new THREE.SphereGeometry(1, 32, 32),
  new THREE.MeshBasicMaterial({ color: 0x0000ff })
);
scene.add(sphere1);

const sphere2 = new THREE.Mesh(
  new THREE.SphereGeometry(1, 32, 32),
  new THREE.MeshBasicMaterial({ color: 0xff0000 })
);
sphere2.position.set(4, 0, 0);
scene.add(sphere2);

// 创建射线
const raycaster = new THREE.Raycaster();
// 创建鼠标向量
const mouse = new THREE.Vector2();
window.addEventListener("click", (event) => {
  console.log(event.clientX, event.clientY);
  mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
  mouse.y = -((event.clientY / window.innerHeight) * 2 - 1);
  console.log(mouse.x, mouse.y);

  raycaster.setFromCamera(mouse, camera);
  const intersects = raycaster.intersectObjects([sphere, sphere1, sphere2]);
  console.log(intersects);
  if (intersects.length > 0) {
    const material = intersects[0].object.material;
    if(material._isSelected){
      material.color.set(material._preColor)
      material._isSelected = false
      return 
    }
    material._preColor = material.color.getHex()
    material.color.set(0x000000);
    material._isSelected = true;
  }
});

//设置辅助轴线
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);

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
