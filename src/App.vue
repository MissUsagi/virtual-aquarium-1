<script setup>
import Aquarium from "./Aquarium.vue";
import FishForm from "./FishForm.vue";
import { ref } from "vue";

const allFish = ref([]);

function addNewFish(fish) {
  allFish.value.push(fish);
}
</script>
<template>
  <div class="app">
    <FishForm @add-fish="addNewFish" />
    <Aquarium :all-fish="allFish" />
  </div>
</template>

<style scoped>
.app {
  display: flex;
  background-color: rgb(1, 12, 36);
}
</style>
