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

  methods: {
    animate(mesh) {
      mesh.position.set(-400, -150, 0);
      this.renderer.render(this.scene, this.camera);
    },
  },

  mounted() {
    ////// INITIALIZATION
    this.sceneCanvas = this.$refs.scene;

    this.scene = new THREE.Scene();
    // this.camera = new THREE.PerspectiveCamera(
    //   75,
    //   this.sceneCanvas.clientWidth / this.sceneCanvas.clientHeight,
    //   0.1,
    //   1000
    // );
    this.camera = new THREE.OrthographicCamera(
      this.sceneCanvas.clientWidth / -2,
      this.sceneCanvas.clientWidth / 2,
      this.sceneCanvas.clientHeight / -2,
      this.sceneCanvas.clientHeight / 2,
      1,
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

    // this.camera.position.z = 5;

    ////////// ADD MESH
    var geometry = new THREE.SphereGeometry(50, 32, 32);
    var material = new THREE.MeshBasicMaterial({
      color: 0xffffff,
      wireframe: true,
    });
    var sphere = new THREE.Mesh(geometry, material);
    sphere.position.x = -20;

    this.scene.add(sphere);

    ////////// RENDER SCENE
    this.animate(sphere);
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
