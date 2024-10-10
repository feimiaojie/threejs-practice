<script setup>
import './style.css'
import * as THREE from 'three'
import { OrbitControls } from 'three/addons/controls/OrbitControls.js'
// 创建场景
const scene = new THREE.Scene()
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

//创建几何体
const geometry = new THREE.BoxGeometry(1, 1, 1)
//创建材质
const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 })
//创建网格
const cube = new THREE.Mesh(geometry, material)
//将网格添加到场景中
scene.add(cube)

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
controls.autoRotate = true // 自动旋转
controls.enableDamping = true // 惯性
function animate() {
  controls.update()
  requestAnimationFrame(animate)
  cube.rotation.x += 0.01
  cube.rotation.y += 0.01

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
</style>
