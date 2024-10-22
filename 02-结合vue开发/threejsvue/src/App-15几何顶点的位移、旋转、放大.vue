<script setup>
import * as THREE from 'three';
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
import { RGBELoader } from 'three/addons/loaders/RGBELoader.js';
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
// planeGeometry.translate(2, 2, 0)
planeGeometry.rotateX(Math.PI / 2)
planeGeometry.scale(3,3,3)
const nvTexture = new THREE.TextureLoader().load('./textures/uv_grid_opengl.jpg')
console.log(nvTexture,'nv')
const planeMaterial = new THREE.MeshBasicMaterial({ map: nvTexture, side: THREE.DoubleSide})
const planeCube = new THREE.Mesh(planeGeometry, planeMaterial)
planeCube.position.set(2, 0.5,0)
scene.add(planeCube)
console.log(planeGeometry)

//创建材质
const material = new THREE.MeshBasicMaterial({ map: nvTexture })

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
indexGeometry.scale(2,2,2)
const indices = new Uint16Array([0, 1, 2, 0, 2, 3])
indexGeometry.setIndex(new THREE.BufferAttribute(indices, 1))

const indexUv = new Float32Array([0,0, 1,0,1,1,0,1])
indexGeometry.setAttribute('uv', new THREE.BufferAttribute(indexUv, 2))
// indexGeometry.computeVertexNormals()
const normals = new Float32Array([0,0,1,0,0,1,0,0,1,0,0,1])
indexGeometry.setAttribute('normal', new THREE.BufferAttribute(normals,3))
const indexCube = new THREE.Mesh(indexGeometry, material)
scene.add(indexCube)

const rgbeLoader = new RGBELoader()
rgbeLoader.load('./textures/Alex_Hart-Nature_Lab_Bones_2k.hdr', (envMap)=>{
  envMap.mapping = THREE.EquirectangularReflectionMapping
  scene.background = envMap
  scene.environment = envMap
  material.envMap = envMap
  planeMaterial.envMap = envMap

})

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

</script>

<template>
  <div></div>
</template>

<style scoped></style>
