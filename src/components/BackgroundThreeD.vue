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
      const container = this.$refs.container;

      this.scene = new THREE.Scene();
      this.camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      );

      const geometry = new THREE.BoxGeometry(1, 1, 1);
      const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
      const cube = new THREE.Mesh(geometry, material);
      this.mesh = cube;
      this.scene.add(cube);
      this.camera.position.z = 5;

      this.renderer = new THREE.WebGLRenderer();
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      container.appendChild(this.renderer.domElement);
      this.animate();
    },

    animate() {
      requestAnimationFrame(this.animate);
      this.mesh.rotation.z += 0.01;
      this.renderer.render(this.scene, this.camera);
    },
  },

  mounted() {
    this.init();
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
