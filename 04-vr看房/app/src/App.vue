<template>
  <div class="home">
    <div class="canvas-container" ref="canvasDom"></div>
  </div>
</template>

<script setup>
import * as THREE from 'three'
import { onMounted, reactive, ref } from 'vue'
import { OrbitControls } from 'three/addons/controls/OrbitControls.js'
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js'
import { DRACOLoader } from 'three/addons/loaders/DRACOLoader.js'

let canvasDom = ref(null)
const scene = new THREE.Scene()
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
)
camera.position.set(0, 2, 6)

const renderer = new THREE.WebGLRenderer({
  antialias: true,
})
renderer.setSize(window.innerWidth, window.innerHeight)
// renderer.outputEncoding = THREE.sRGBEncoding
renderer.toneMapping = THREE.ACESFilmicToneMapping
renderer.toneMappingExposure = 0.5
renderer.shadowMap.enabled = true
renderer.shadowMap.type = THREE.PCFSoftShadowMap // default THREE.PCFShadowMap

const controls = new OrbitControls(camera, renderer.domElement)
controls.enableDamping = true

const render = () => {
  renderer.render(scene, camera)
  requestAnimationFrame(render)
}

onMounted(() => {
  // console.log(canvasDom.value)
  // document.body.appendChild(renderer.domElement)
  canvasDom.value.appendChild(renderer.domElement)
  renderer.setClearColor('#000')
  scene.background = new THREE.Color('#ccc')
  scene.environment = new THREE.Color('#ccc')
  render()

  const gridHelper = new THREE.GridHelper(10, 10)
  gridHelper.material.opacity = 0.2
  gridHelper.material.transparent = true
  scene.add(gridHelper)
  //Create a PointLight and turn on shadows for the light
  const light = new THREE.PointLight(0xffffff, 1, 100)
  light.position.set(0, 10, 4)
  light.castShadow = true // default false
  scene.add(light)

  //Set up shadow properties for the light
  light.shadow.mapSize.width = 512 // default
  light.shadow.mapSize.height = 512 // default
  light.shadow.camera.near = 0.5 // default
  light.shadow.camera.far = 500 // default

  //Create a sphere that cast shadows (but does not receive them)
  const sphereGeometry = new THREE.SphereGeometry(5, 32, 32)
  const sphereMaterial = new THREE.MeshStandardMaterial({ color: 0xff0000 })
  const sphere = new THREE.Mesh(sphereGeometry, sphereMaterial)
  sphere.castShadow = true //default is false
  sphere.receiveShadow = false //default
  scene.add(sphere)

  //Create a plane that receives shadows (but does not cast them)
  const planeGeometry = new THREE.PlaneGeometry(20, 20, 32, 32)
  const planeMaterial = new THREE.MeshStandardMaterial({ color: 0x00ff00 })
  const plane = new THREE.Mesh(planeGeometry, planeMaterial)
  plane.receiveShadow = true
  scene.add(plane)

  //Create a helper for the shadow camera (optional)
  const helper = new THREE.CameraHelper(light.shadow.camera)
  scene.add(helper)
})
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
</style>
