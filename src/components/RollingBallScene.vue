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
    console.log(this.sceneCanvas.clientWidth);
    console.log(11);

    this.scene = new THREE.Scene();
    this.camera = new THREE.PerspectiveCamera(
      75,
      this.sceneCanvas.clientWidth / this.sceneCanvas.clientHeight,
      0.1,
      1000
    );

    this.renderer = new THREE.WebGLRenderer({ antialias: true });

    /////// CONFIGURING INIT SETTINGS
    this.renderer.setSize(
      this.sceneCanvas.clientWidth,
      this.sceneCanvas.clientHeight
    );

    this.renderer.setClearColor("#00000f");

    this.sceneCanvas.appendChild(this.renderer.domElement);

    this.camera.position.z = 20;

    ////////// ADD MESH
    var geometry = new THREE.SphereGeometry(5, 32, 32);
	var material = new THREE.MeshBasicMaterial({ color: 0xff0000 });
	var sphere = new THREE.Mesh(geometry, material);

	this.scene.add(sphere);


	////////// RENDER SCENE
    this.renderer.render(this.scene, this.camera);
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
	height: 60vh;
}
</style>
