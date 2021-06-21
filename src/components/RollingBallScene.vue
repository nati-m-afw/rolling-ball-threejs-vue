<template>
  <div>
    <h1>Rolling Ball</h1>
    <div class="scene" ref="scene" @click="moveBall"></div>
    <pre>{{ $data }}</pre>
  </div>
</template>

<script>
import * as THREE from "three";

export default {
  name: "RollingBallScene",

  data() {
    return {
      ballX: -400,
      ballY: -150,
      eventLocation: null,
      slope: null,
      yInt: null,
      speed: null,
      sceneCanvas: null,
      scene: null,
      camera: null,
      renderer: null,
      mesh: {
        sphere: null,
      },
    };
  },

  methods: {
    animate() {
      let animId = requestAnimationFrame(this.animate);
      let [meshType, x] = this.eventLocation;

      if (this.mesh[meshType].position.x == x) {
        // this.speed = 20;
        cancelAnimationFrame(animId);
      }

      // Speed with acceleration
      // console.log(x - this.ballX);
      // // Distance between ballX and x of click postion
      // let distance = (x - this.ballX) / 100;
      // if (distance > 1) this.speed = distance ** 2;
      // else if (distance < -1) this.speed = -(distance ** 2);
      // else if (0 <= distance && distance <= 1) this.speed = 1;
      // else this.speed = -1;
      // console.log("Distance->", distance, "Speed->", this.speed);
      // this.ballX += parseInt(this.speed);
      // Static Speed
      //eslint-disable-next-line
      this.ballX += x - this.ballX > 0 ? x - this.ballX > 10 ? 10 : 1 : x - this.ballX < -10 ? -10 : -1;
      this.ballY = this.slope * this.ballX + this.yInt;
      this.mesh[meshType].position.set(this.ballX, this.ballY, 0);

      this.renderer.render(this.scene, this.camera);
    },

    moveBall(e) {
      // console.log(
      //   "X->" +
      //     (e.offsetX - this.sceneCanvas.offsetWidth / 2) +
      //     "/" +
      //     this.sceneCanvas.offsetWidth,
      //   "Y->" +
      //     (e.offsetY - this.sceneCanvas.offsetHeight / 2) +
      //     "/" +
      //     this.sceneCanvas.offsetHeight
      // );
      let x = parseInt(e.offsetX - this.sceneCanvas.offsetWidth / 2);
      let y = parseInt(e.offsetY - this.sceneCanvas.offsetHeight / 2);
      this.eventLocation = ["sphere", x, y];
      this.slope = this.getSlope(this.ballX, x, this.ballY, y);
      this.yInt = this.getYIntercept(this.ballX, x, this.ballY, y);
      this.animate();
    },

    // Helper functions
    getSlope(x0, x1, y0, y1) {
      return (y1 - y0) / (x1 - x0);
    },
    getYIntercept(x0, x1, y0, y1) {
      return y0 - this.getSlope(x0, x1, y0, y1) * x0;
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

    this.renderer.setClearColor("#f5f5f5");

    this.sceneCanvas.appendChild(this.renderer.domElement);

    // this.camera.position.z = 5;

    ////////// ADD MESH
    var geometry = new THREE.SphereGeometry(50, 32, 32);
    var material = new THREE.MeshStandardMaterial({
      color: 0xffffff,
      // wireframe: true,
      emissive: 0x00ff00,
      emissiveIntensity: 0.2,
      // map: new THREE.TextureLoader().load(require("../assets/img/ball_texture.jpg")),
      roughness: 0.5,
      metalness: 0.1,
    });
    this.mesh.sphere = new THREE.Mesh(geometry, material);

    this.scene.add(this.mesh.sphere);

    /////////// ADD LIGHT
    // const light = new THREE.AmbientLight(0xffff00, 0.5);
    // this.scene.add(light);

    var sphere = new THREE.SphereGeometry(8, 16, 8);
    const light2 = new THREE.PointLight(0xffff00, 0.5);
    light2.add(
      new THREE.Mesh(
        sphere,
        new THREE.MeshBasicMaterial({ color: 0xffff00, decay: 2 })
      )
    );
    light2.position.z = -200;
    this.scene.add(light2);

    ////////// RENDER SCENE
    this.mesh.sphere.position.set(this.ballX, this.ballY, 0);
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
