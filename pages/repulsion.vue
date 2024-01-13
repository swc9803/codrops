<template>
  <div ref="canvasRef" class="canvas" />
</template>

<script setup>
// import gsap from "gsap";
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";

const canvasRef = ref();
const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
let camera;

const scene = new THREE.Scene();
scene.background = new THREE.Color(0x00ffff);

const light = new THREE.DirectionalLight(0xffffff, 1);
light.position.set(1, 1, 1);
scene.add(light);

function animate() {
  camera.updateMatrixWorld();
  renderer.render(scene, camera);
  requestAnimationFrame(animate);
}

const init = () => {
  renderer.setPixelRatio(window.devicePixelRatio);
  renderer.setSize(canvasRef.value.offsetWidth, canvasRef.value.offsetHeight);
  canvasRef.value.appendChild(renderer.domElement);
  const controls = new OrbitControls(camera, renderer.domElement);
  controls.minDistance = 3;
  controls.maxDistance = 6;
  controls.maxPolarAngle = Math.PI / 2 - 0.1;
  controls.update();
};

const onResize = () => {
  camera.aspect = canvasRef.value.offsetWidth / canvasRef.value.offsetHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(canvasRef.value.offsetWidth, canvasRef.value.offsetHeight);
};

onMounted(() => {
  camera = new THREE.PerspectiveCamera(
    75,
    canvasRef.value.offsetWidth / canvasRef.value.offsetHeight,
    0.1,
    1000,
  );

  init();
  onResize();
  animate();

  window.addEventListener("resize", onResize);
});
onBeforeUnmount(() => {
  window.removeEventListener("resize", onResize);
});
</script>

<style lang="scss" scoped>
.canvas {
  position: relative;
  width: 100%;
  height: 100vh;
}
</style>
