<template>
  <div>
    <h1>Rolling Ball</h1>
    <div class="scene" ref="scene"></div>
  </div>
</template>

<script>
import * as THREE from "three";

export default {
  name: "RollingBallScene",

  data() {
    return {
      sceneCanvas: null,
      scene: null,
      camera: null,
      renderer: null,
    };
  },

  methods: {},

  mounted() {
    ////// INITIALIZATION
    this.sceneCanvas = this.$refs.scene;

    this.scene = new THREE.Scene();
    this.camera = new THREE.PerspectiveCamera(
      75,
      this.sceneCanvas.clientWidth / this.sceneCanvas.clientHeight,
      0.1,
      1000
    );

    this.renderer = new THREE.WebGLRenderer({ antialias: true });
    this.sceneCanvas.appendChild(this.renderer.domElement);

    /////// CONFIGURING INIT SETTINGS
  },
};
</script>

<style scoped>
* {
  margin: 0%;
  padding: 0;
}

h1 {
  text-align: center;
}

.scene {
  display: inline;
  align-content: center;
  width: 100%;
  height: 600px;
}

.scene canvas {
  display: inline !important;
  margin: auto;
  width: 100%;
  height: 100%;
}
</style>
