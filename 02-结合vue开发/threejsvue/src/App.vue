<script setup>
import './style.css'
import * as THREE from 'three'
import { OrbitControls } from 'three/addons/controls/OrbitControls.js'
import { GUI } from 'three/addons/libs/lil-gui.module.min.js'
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';
import { DRACOLoader } from 'three/addons/loaders/DRACOLoader.js';
import { RGBELoader } from 'three/addons/loaders/RGBELoader.js';

// 创建场景
const scene = new THREE.Scene()
scene.background = new THREE.Color(0x888888)
// 创建相机
const camera = new THREE.PerspectiveCamera(
  75, // 视角
  window.innerWidth / window.innerHeight, // 宽高比
  0.1, //近平面
  1000 // 远平面
)
// 创建渲染器
const renderer = new THREE.WebGLRenderer()
renderer.setSize(window.innerWidth, window.innerHeight)
document.body.appendChild(renderer.domElement)

const loader = new GLTFLoader()
loader.load('./model/Duck.glb', (gltf) => {
  scene.add(gltf.scene)
})

const dracoLoader = new DRACOLoader()
dracoLoader.setDecoderPath('./draco/')
loader.setDRACOLoader(dracoLoader)
loader.load('./model/city.glb', (gltf) => {
  scene.add(gltf.scene)
})

//环境光
const rgbeLoader = new RGBELoader()
rgbeLoader.load('./texture/Alex_Hart-Nature_Lab_Bones_2k.hdr', envMap => {
  envMap.mapping = THREE.EquirectangularReflectionMapping
  scene.environment = envMap
})
// const light = new THREE.AmbientLight(0x0fffff, 1);
// scene.add(light);
//设置辅助轴线
const axesHelper = new THREE.AxesHelper(5)
scene.add(axesHelper)

// 设置相机位置
camera.position.z = 2
camera.position.x = 2
camera.position.y = 2
camera.lookAt(0, 0, 0)

// 控制器
const controls = new OrbitControls(camera, renderer.domElement)
function animate() {
  controls.update()
  requestAnimationFrame(animate)

  // 渲染
  renderer.render(scene, camera)
}
animate()

window.addEventListener('resize', () => {
  console.log('resize')
  renderer.setSize(window.innerWidth, window.innerHeight)
  camera.aspect = window.innerWidth / window.innerHeight
  camera.updateProjectionMatrix()
})

</script>

<template>
  <div></div>
</template>

<style scoped></style>
