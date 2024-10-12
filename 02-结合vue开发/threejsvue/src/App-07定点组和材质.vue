<script setup>
import './style.css'
import * as THREE from 'three'
import { OrbitControls } from 'three/addons/controls/OrbitControls.js'
import { GUI } from 'three/addons/libs/lil-gui.module.min.js'
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
geometry.addGroup(0, 3, 0)
geometry.addGroup(3, 3, 1)
console.log(geometry) // 6个点

//创建材质
const material = new THREE.MeshBasicMaterial({ color: 0x00ff00, side: THREE.DoubleSide, wireframe: true })
const material1 = new THREE.MeshBasicMaterial({ color: 0xff0000, side: THREE.DoubleSide })
//创建网格
const cube = new THREE.Mesh(geometry, [material, material1])
scene.add(cube)

cube.position.set(1, 1, 1)

const geometryBox = new THREE.BoxGeometry(1, 1, 1)
const materialBox1 = new THREE.MeshBasicMaterial({ color: 0xff0000 })
const materialBox2 = new THREE.MeshBasicMaterial({ color: 0x00BCCC })
const materialBox3 = new THREE.MeshBasicMaterial({ color: 0x8d31bf })
const materialBox4 = new THREE.MeshBasicMaterial({ color: 0x574c5d })
const materialBox5 = new THREE.MeshBasicMaterial({ color: 0x22ec8e })
const materialBox6 = new THREE.MeshBasicMaterial({ color: 0x9a7142 })
const boxCube = new THREE.Mesh(geometryBox, [materialBox1, materialBox2, materialBox3, materialBox4, materialBox5, materialBox6])
scene.add(boxCube)
boxCube.position.set(-2, -2, -2)

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
const gui = new GUI()
gui.add(eventObject, 'FullScreen').name('全屏')
gui.add(eventObject, 'ExitFullScreen').name('退出全屏')
gui.add(cube.position, 'x').min(-10).max(10).step(1).onChange((val)=>console.log(val))
gui.add(cube.position, 'y').min(-10).max(10).step(1).onFinishChange((val)=>console.log(val))
gui.add(cube.position, 'z').min(-10).max(10).step(1)

const colorParams = {
  cubeColor: '#ff0000'
}
gui.addColor(colorParams, 'cubeColor').name('颜色').onChange(() => {
  // cube.material.color.set(colorParams.cubeColor)
  material.color.set(colorParams.cubeColor)
})
</script>

<template>
  <div></div>
</template>

<style scoped></style>
