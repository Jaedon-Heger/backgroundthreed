<template>
  <div id="container" ref="container"></div>
</template>

<script>
import * as THREE from "three";
// import { GLTFLoader } from "three/addons/loaders/GLTFLoader.js";

export default {
  name: "BackgroundThreeD",

  props: {
    msg: String,
  },

  data() {
    return {
      camera: null,
      renderer: null,
      mesh: null,
    };
  },

  methods: {
    init() {
      this.createScene();
      this.createCamera();
      this.createLighting();
      this.loadGeometry();
      this.createRenderer();
    },

    createScene() {
      this.scene = new THREE.Scene();
      this.scene.background = new THREE.Color(0x444488);
    },

    createCamera() {
      this.camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      );
      this.camera.position.z = 5;
    },

    createRenderer() {
      const container = this.$refs.container;
      this.renderer = new THREE.WebGLRenderer();
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      container.appendChild(this.renderer.domElement);
    },

    createLighting() {
      const directionalLight = new THREE.DirectionalLight(0xffffff, 0.8);
      this.scene.add(directionalLight);
    },

    loadGeometry() {
      const geometry = new THREE.BoxGeometry(1, 1, 1);
      const material = new THREE.MeshToonMaterial({
        color: 0x00ff00,
      });
      const cube = new THREE.Mesh(geometry, material);
      this.mesh = cube;
      this.scene.add(cube);
    },

    animate() {
      requestAnimationFrame(this.animate);
      this.mesh.rotation.z += 0.01;
      this.mesh.rotation.x += 0.01;
      this.renderer.render(this.scene, this.camera);
    },
  },

  mounted() {
    this.init();
    this.animate();
  },
};
</script>

<style scoped>
#container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
</style>
