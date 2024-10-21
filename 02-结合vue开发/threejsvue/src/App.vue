<script setup>
import * as THREE from 'three'
import gsap from 'gsap'
import { OrbitControls } from 'three/addons/controls/OrbitControls.js'
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js'
import { DRACOLoader } from 'three/addons/loaders/DRACOLoader.js'
import { RGBELoader } from 'three/addons/loaders/RGBELoader.js'
// import { Water } from 'three/addons/objects/Water'
import { Water } from "three/examples/jsm/objects/Water2";

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
camera.position.set(-3.23, 2.98, 4.06)
camera.updateProjectionMatrix()
camera.lookAt(0, 0, 0)

// 创建渲染器
const renderer = new THREE.WebGLRenderer({
  antialias: true, // 开启抗锯齿
})
renderer.setSize(window.innerWidth, window.innerHeight)
document.body.appendChild(renderer.domElement)
renderer.toneMapping = THREE.ACESFilmicToneMapping
renderer.toneMappingExposure = 0.5

// 控制器
const controls = new OrbitControls(camera, renderer.domElement)
controls.enableDamping = true // 阻尼

// 初始化loader
const dracoLoader = new DRACOLoader()
dracoLoader.setDecoderPath('./draco/')
const glftLoader = new GLTFLoader()
glftLoader.setDRACOLoader(dracoLoader)

// 加载环境纹理
const rbgeLoader = new RGBELoader()
rbgeLoader.load('./texture/sky.hdr', (texture) => {
  texture.mapping = THREE.EquirectangularReflectionMapping
  scene.background = texture
  scene.environment = texture
})

//加载模型
glftLoader.load('./model/scene.glb', function (gltf) {
  const model = gltf.scene
  model.traverse((child) => {
    if (child.name === 'Plane') {
      child.visible = false
    }
  })
  scene.add(model)
})

// 创建水面
const waterGeometry = new THREE.CircleGeometry(300, 32);
const water = new Water(waterGeometry, {
  textureWidth: 1024,
  textureHeight: 1024,
  color: 0xeeeeff,
  flowDirection: new THREE.Vector2(1, 1),
  scale: 100,
});
water.rotation.x = -Math.PI / 2
scene.add(water)

// 添加平行光
const light = new THREE.DirectionalLight(0xffffff, 1)
light.position.set(0, 50, 0)
scene.add(light)

function animate() {
  controls.update()
  requestAnimationFrame(animate)

  // 渲染
  renderer.render(scene, camera)
}
animate()
</script>

<template>
  <div></div>
</template>

<style scoped></style>
