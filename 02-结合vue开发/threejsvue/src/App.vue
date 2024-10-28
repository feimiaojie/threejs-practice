<script setup>
import * as THREE from 'three'
import gsap from 'gsap'
import { OrbitControls } from 'three/addons/controls/OrbitControls.js'
import { ref } from 'vue'
import { GUI } from 'three/addons/libs/lil-gui.module.min.js'

const gui = new GUI()
// 创建场景
const scene = new THREE.Scene()
// 创建相机
const camera = new THREE.PerspectiveCamera(
  75, // 视角
  window.innerWidth / window.innerHeight, // 宽高比
  0.1, //近平面
  1000 // 远平面
)

// 设置相机位置
camera.position.set(-3.23, 2.98, 16.06)
camera.updateProjectionMatrix()
camera.lookAt(0, 0, 0)

// 创建渲染器
const renderer = new THREE.WebGLRenderer({
  antialias: true, // 开启抗锯齿
})
renderer.setSize(window.innerWidth, window.innerHeight)
document.body.appendChild(renderer.domElement)
renderer.outputEncoding = THREE.sRGBEncoding
renderer.toneMapping = THREE.ACESFilmicToneMapping
renderer.toneMappingExposure = 0.5
renderer.shadowMap.enabled = true

// 控制器
const controls = new OrbitControls(camera, renderer.domElement)
controls.target.set(0, 2, 0)
controls.enableDamping = true // 阻尼

// 环境光
// const light1 = new THREE.AmbientLight( 0x404040 ); // 柔和的白光
// scene.add(light1);

//Create a PointLight and turn on shadows for the light
const light = new THREE.PointLight( 0xffffff, 1, 100 );
light.position.set( 0, 10, 4 );
light.castShadow = true; // default false
scene.add( light );

//Set up shadow properties for the light
light.shadow.mapSize.width = 512; // default
light.shadow.mapSize.height = 512; // default
light.shadow.camera.near = 0.5; // default
light.shadow.camera.far = 500 // default

//Create a sphere that cast shadows (but does not receive them)
const sphereGeometry = new THREE.SphereGeometry( 5, 32, 32 );
const sphereMaterial = new THREE.MeshStandardMaterial( { color: 0xff0000 } );
const sphere = new THREE.Mesh( sphereGeometry, sphereMaterial );
sphere.castShadow = true; //default is false
sphere.receiveShadow = false; //default
scene.add( sphere );

//Create a plane that receives shadows (but does not cast them)
const planeGeometry = new THREE.PlaneGeometry( 20, 20, 32, 32 );
const planeMaterial = new THREE.MeshStandardMaterial( { color: 0x00ff00 } )
const plane = new THREE.Mesh( planeGeometry, planeMaterial );
plane.receiveShadow = true;
scene.add( plane );

//Create a helper for the shadow camera (optional)
const helper = new THREE.CameraHelper( light.shadow.camera );
scene.add( helper );

const folder1 = gui.addFolder("pointerLight");
  folder1.add(light.position, "x", -600, 600);
  folder1.add(light.position, "y", -600, 600);
  folder1.add(light.position, "z", -600, 600);

function animate() {
  controls.update()
  requestAnimationFrame(animate)

  // 渲染
  renderer.render(scene, camera)
}
animate()


</script>

<template>
  <div>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
}
canvas {
  width: 100vw;
  height: 100vh;
  position: fixed;
  left: 0;
  top: 0;
}
</style>
