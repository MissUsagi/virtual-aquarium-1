<script setup>
import Fish from "./Fish.vue";

const props = defineProps({
  allFish: {
    type: Array,
    default: () => [],
  },
});
</script>

<template>
  <div class="aquarium" id="aquarium">
    <Fish v-for="fish in allFish" :key="fish.id" :fish="fish" />
  </div>
</template>

<style scoped>
.aquarium {
  position: relative;
  padding: 5px;
  background: url("../public/assets/bg.jpg") no-repeat center center;
  background-size: cover;
  overflow: hidden;
  width: 100%;
  height: 100vh;
}
</style>
