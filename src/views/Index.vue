<template>
  <div>
    <rolling-ball />
  </div>
</template>

<script>
import RollingBallScene from "../components/RollingBallScene.vue";

export default {
  name: "Index",
  components: {
    "rolling-ball": RollingBallScene,
  },
};
</script>

<style scoped></style>
