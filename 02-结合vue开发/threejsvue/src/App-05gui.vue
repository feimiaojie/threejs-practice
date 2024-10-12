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
const geometry = new THREE.BoxGeometry(1, 1, 1)
console.log(geometry)
//创建材质
const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 })
//创建网格
const cube = new THREE.Mesh(geometry, material)

const parentMaterial = new THREE.MeshBasicMaterial({ color: 0xff0000 })
const parentCube = new THREE.Mesh(geometry, parentMaterial)
parentCube.add(cube)
parentCube.position.set(-3, 0, 0)
parentCube.scale.set(3, 3, 3)
parentCube.rotation.x = Math.PI / 4 // 45度
//子元素继承父元素的变换

// cube.position.x = 2
cube.position.set(-3, 0, 0)
//将网格添加到场景中
scene.add(parentCube)

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
// controls.autoRotate = true // 自动旋转
// controls.enableDamping = true // 惯性
function animate() {
  controls.update()
  requestAnimationFrame(animate)
  // cube.rotation.x += 0.01
  // cube.rotation.y += 0.01

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
gui.add(parentMaterial, 'wireframe').name('线框模式')

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
