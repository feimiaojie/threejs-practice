<script setup>
import * as THREE from 'three'
import { ref, onMounted } from 'vue'
import { OrbitControls } from 'three/addons/controls/OrbitControls.js'
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js'
import { DRACOLoader } from 'three/addons/loaders/DRACOLoader.js'
import { RGBELoader } from 'three/addons/loaders/RGBELoader.js'
import { GUI } from 'three/addons/libs/lil-gui.module.min.js'

let scene, camera, renderer, container, controls, gui

const init = () => {
  container = document.getElementById('container')
  console.log(container)
  createGUI()
  createScene()
  createRender()
  createCamera()
  createControls()
  createModel()
  createLight()
  createHelp()
  
  update()
}
const createScene = () => {
  scene = new THREE.Scene()
  scene.background = new THREE.Color('#fefefe')
  // scene.fog = new THREE.Fog('#e5e5e5', 2000, 2000)
}
const createRender = () => {
  renderer = new THREE.WebGLRenderer({ antialias: true })
  renderer.setPixelRatio(window.devicePixelRatio)
  renderer.setSize(container.clientWidth, container.clientHeight)
  container.appendChild(renderer.domElement)
  renderer.shadowMap.enabled = true
  renderer.shadowMap.autoUpdate = true
  // renderer.toneMapping = THREE.ACESFilmicToneMapping;
}
const createCamera = () => {
  camera = new THREE.PerspectiveCamera(
    20,
    container.clientWidth / container.clientHeight,
    1,
    30000
  )
  camera.position.set(600, 600, 600)
}

const createLight = () => {
  // const ambient = new THREE.AmbientLight('#fff', 1)
  // scene.add(ambient)

  // const directionalLight = new THREE.DirectionalLight('#fff', 1)
  // directionalLight.position.set(300, 300, 300)
  // scene.add(directionalLight)
  // const target = new THREE.Object3D()
  // target.position.set(0, 0, 0)
  // scene.add(target)
  // directionalLight.target = target
  // const directionalLigheHelper = new THREE.DirectionalLightHelper(
  //   directionalLight,
  //   20
  // )
  // scene.add(directionalLigheHelper)
  // const folder = gui.addFolder('directionalLight')
  // folder.add(directionalLight.position, 'x', -600, 600)
  // folder.add(directionalLight.position, 'y', -600, 600)
  // folder.add(directionalLight.position, 'z', -600, 600)

  // directionalLight.castShadow = true
  // directionalLight.shadow.mapSize.width = 512 * 2
  // directionalLight.shadow.mapSize.height = 512 * 2
  // directionalLight.shadow.radius = 2
  // directionalLight.shadow.camera.near = 0.5
  // directionalLight.shadow.camera.far = 800
  // directionalLight.shadow.camera.left = -400
  // directionalLight.shadow.camera.right = 400
  // directionalLight.shadow.camera.top = 400
  // directionalLight.shadow.camera.buttom = -400
  

  const pointLight = new THREE.PointLight('#fff', 1, 1800)
  pointLight.position.set(0, 300, 0)
  pointLight.castShadow = true
  scene.add(pointLight)

  const folder1 = gui.addFolder('pointerLight')
  folder1.add(pointLight.position, 'x', -600, 600)
  folder1.add(pointLight.position, 'y', -600, 600)
  folder1.add(pointLight.position, 'z', -600, 600)
  
  const pointLightHelper = new THREE.PointLightHelper(pointLight, 20)
  scene.add(pointLightHelper)
}

const createControls = () => {
  controls = new OrbitControls(camera, renderer.domElement)
  controls.enableDamping = true
}


const createModel = () => {
  const dracoLoader = new DRACOLoader()
  dracoLoader.setDecoderPath('./draco/')
  const gltfLoader = new GLTFLoader()
  gltfLoader.setDRACOLoader(dracoLoader)
  gltfLoader.load('./model/model.glb', function (gltf) {
    const model = gltf.scene
    model.traverse((child) => {
      if (child.isMesh) {
        child.frustumCulled = false
        child.castShadow = true
        child.receiveShadow = true
      }
    })
    model.scale.set(0.4, 0.4, 0.4)
    model.position.set(0, 0, 0)
    scene.add(model)
  })
}

const createHelp = () => {
  const gridHelper = new THREE.GridHelper(2000, 10)
  scene.add(gridHelper)

  const axesHelper = new THREE.AxesHelper(100)
  scene.add(axesHelper)
}

const createGUI = () => {
  gui = new GUI()
}
const update = () => {
  requestAnimationFrame(update)
  renderer.render(scene, camera)
  controls.update()
}
onMounted(() => {
  init()

  const gridHelper = new THREE.GridHelper(10, 10)
  gridHelper.material.opacity = 1
  gridHelper.material.transparent = true
  scene.add(gridHelper)
})
</script>

<template>
  <div id="container"></div>
</template>

<style scoped>
* {
  padding: 0;
  margin: 0;
}
#container {
  width: 100vw;
  height: 100vh;
}
</style>
