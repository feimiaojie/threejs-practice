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
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
camera.position.set(0, 2, 6)

const renderer = new THREE.WebGLRenderer({
  antialias: true
})
renderer.setSize(window.innerWidth, window.innerHeight)
// renderer.outputEncoding = THREE.sRGBEncoding
renderer.toneMapping = THREE.ACESFilmicToneMapping
renderer.toneMappingExposure = 0.5
renderer.shadowMap.enabled = true

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

})



</script>

<style>
* {
  margin: 0;
  padding: 0;
}

</style>
