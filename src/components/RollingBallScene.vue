<template>
  <div>
    <h1>Rolling Ball</h1>
    <div class="scene" ref="scene" @click="moveBall"></div>
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
      mesh: {
        sphere: null,
      },

      eventLocation: null,
      speed: 20,
    };
  },

  methods: {
    animate() {
      let animId = requestAnimationFrame(this.animate);
      let [meshType, x, y] = this.eventLocation;

      if (
        this.mesh[meshType].position.x == x &&
        this.mesh[meshType].position.y == y
      ) {
        console.log(this.mesh[meshType].position.x);
        this.speed = 20;
        cancelAnimationFrame(animId);
      }
      if (x > this.mesh[meshType].position.x)
        this.mesh[meshType].position.x += this.speed;
      else if (x < this.mesh[meshType].position.x)
        this.mesh[meshType].position.x -= this.speed;
      if (y > this.mesh[meshType].position.y)
        this.mesh[meshType].position.y += this.speed;
      else if (y < this.mesh[meshType].position.y)
        this.mesh[meshType].position.y -= this.speed;

      if (this.speed > 1.0) this.speed -= 0.5;

      this.renderer.render(this.scene, this.camera);
    },

    moveBall(e) {
      console.log(
        "X->" +
          (e.offsetX - this.sceneCanvas.offsetWidth / 2) +
          "/" +
          this.sceneCanvas.offsetWidth,
        "Y->" +
          (e.offsetY - this.sceneCanvas.offsetHeight / 2) +
          "/" +
          this.sceneCanvas.offsetHeight
      );
      let x = parseInt(e.offsetX - this.sceneCanvas.offsetWidth / 2);
      let y = parseInt(e.offsetY - this.sceneCanvas.offsetHeight / 2);
      this.eventLocation = ["sphere", x, y];
      this.animate(...this.eventLocation);
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
    this.mesh.sphere = new THREE.Mesh(geometry, material);

    this.scene.add(this.mesh.sphere);

    ////////// RENDER SCENE
    this.mesh.sphere.position.set(-400, -150, 0);
    this.renderer.render(this.scene, this.camera);
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
}

h1 {
  text-align: center;
}

.scene {
  height: 60vh;
}
</style>
