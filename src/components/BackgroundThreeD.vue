<template>
  <div id="container" ref="container"></div>
</template>

<script>
import * as THREE from "three";
import { GLTFLoader } from "three/addons/loaders/GLTFLoader.js";

export default {
  name: "BackgroundThreeD",

  props: {
    objectPath: String,
    cameraConfig: {
      type: Object,
      default: () => {
        return {
          type: "PerspectiveCamera",
          props: [75, window.innerWidth / window.innerHeight, 0.1, 1000],
          position: [0, 0, 1],
        };
      },
    },
    objectPosition: {
      type: Array,
      default: () => {
        return [0, 0, 0];
      },
    },
    backgroundColor: {
      type: Number,
      default: 0x242424,
    },
    lighting: {
      type: Array,
      default: () => {
        return [
          { type: "AmbientLight", props: [0xffffff, 0.4] },
          {
            type: "PointLight",
            props: [0xffffff, 0.6],
            position: [4, 0, 5],
          },
        ];
      },
    },
    rotation: {
      type: Object,
      default: () => {
        return {
          x: 0,
          y: 0,
          z: 0.01,
        };
      },
    },
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
      this.scene.background = new THREE.Color(this.backgroundColor);
    },

    createCamera() {
      this.camera = new THREE.PerspectiveCamera(...this.cameraConfig.props);
      this.camera.position.set(...this.cameraConfig.position);
    },

    createRenderer() {
      const container = this.$refs.container;
      this.renderer = new THREE.WebGLRenderer();
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      container.appendChild(this.renderer.domElement);
    },

    createLighting() {
      this.lighting.forEach((lightConfig) => {
        switch (lightConfig.type) {
          case "PointLight": {
            const [color, intensity, distance, decay] = lightConfig.props;
            const light = new THREE.PointLight(
              color,
              intensity,
              distance || null,
              decay || null
            );
            light.position.set(...lightConfig.position);
            this.scene.add(light);
            break;
          }
          case "AmbientLight": {
            const light = new THREE.AmbientLight(...lightConfig.props);
            this.scene.add(light);
            break;
          }
          default: {
            const light = new THREE.DirectionalLight(
              lightConfig.color,
              lightConfig.intensity
            );
            this.scene.add(light);
          }
        }
      });
    },

    loadGeometry() {
      if (this.objectPath) {
        const loader = new GLTFLoader();
        loader.load(this.objectPath, (obj) => {
          this.mesh = obj.scene.children[0];
          this.mesh.position.set(...this.objectPosition);
          this.scene.add(obj.scene);
        });
      }
    },

    animate() {
      requestAnimationFrame(this.animate);
      if (this.mesh) {
        this.mesh.rotation.x += this.rotation.x;
        this.mesh.rotation.y += this.rotation.y;
        this.mesh.rotation.z += this.rotation.z;
      }
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
