<script setup>
import * as THREE from 'three';
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
import './style.css';
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

// 创建平面
const planeGeometry = new THREE.PlaneGeometry(1,1)
const nvTexture = new THREE.TextureLoader().load('./textures/uv_grid_opengl.jpg')
console.log(nvTexture,'nv')
const planeMaterial = new THREE.MeshBasicMaterial({ map: nvTexture})
const planeCube = new THREE.Mesh(planeGeometry,planeMaterial)
planeCube.position.set(2, 0.5,0)
scene.add(planeCube)
console.log(planeGeometry)
//创建几何体
const geometry = new THREE.BufferGeometry()
const vertices = new Float32Array([
  // x y z
  0, 0, 0, // 顶点1坐标
  1, 0, 0, // 顶点2坐标
  1, 1, 0, // 顶点3坐标

  0, 0, 0,
  1, 1, 0,
  0, 1, 0
])
// 红色的是x，绿色的是y，蓝色的是z
geometry.setAttribute('position', new THREE.BufferAttribute(vertices, 3))
console.log(geometry) // 6个点

//创建材质
const material = new THREE.MeshBasicMaterial({ map: nvTexture })
//创建网格
const cube = new THREE.Mesh(geometry, material)
// scene.add(cube)

cube.position.set(0, 0, 0)

const indexGeometry = new THREE.BufferGeometry()
const indexVertices = new Float32Array([
  0, 0, 0, // 顶点1坐标
  1, 0, 0, // 顶点2坐标
  1, 1, 0, // 顶点3坐标
  // 0, 0, 0,
  // 1, 1, 0,
  0, 1, 0
])
indexGeometry.setAttribute('position', new THREE.BufferAttribute(indexVertices, 3))
console.log(indexGeometry) // 4个点
const indices = new Uint16Array([0, 1, 2, 0, 2, 3])
indexGeometry.setIndex(new THREE.BufferAttribute(indices, 1))

const indexUv = new Float32Array([0,0, 1,0,1,1,0,1])
indexGeometry.setAttribute('uv', new THREE.BufferAttribute(indexUv, 2))
const indexCube = new THREE.Mesh(indexGeometry, material)
indexGeometry.computeVertexNormals()
scene.add(indexCube)

//设置辅助轴线
const axesHelper = new THREE.AxesHelper(5)
scene.add(axesHelper)

// 设置相机位置
camera.position.z = 2
camera.position.x = 4
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

const eventObject = {
  FullScreen: function () {
    document.documentElement.requestFullscreen()
    console.log('FullScreen')
  },
  ExitFullScreen: function () {
    document.exitFullscreen()
    console.log('ExitFullScreen')
  }
}
</script>

<template>
  <div></div>
</template>

<style scoped></style>
