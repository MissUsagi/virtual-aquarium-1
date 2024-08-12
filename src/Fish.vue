<script setup>
import { ref, computed, onMounted } from "vue";

const props = defineProps({
  fish: {
    type: Object,
    required: true,
  },
});

const fishImageUrl = computed(() => `../assets/${props.fish.type}.png`);

const fishEl = ref();
const aquariumEl = ref();

const position = ref({
  x: 0,
  y: 0,
});

const timeout = ref(0);
const minMilliseconds = 4000;
const maxMilliseconds = 10_000;

const generateTimeout = () =>
  Math.floor(Math.random() * maxMilliseconds) + minMilliseconds;

function doTimeout() {
  timeout.value = generateTimeout();
  setTimeout(() => {
    moveFish();
    doTimeout();
  }, timeout.value);
}

const transition = computed(() => `all ${timeout.value / 1000}s linear`);

const movement = computed(() => ({
  left: `${position.value.x}px`,
  top: `${position.value.y}px`,
  transition: timeout.value ? transition.value : false,
}));

const directionRight = ref(true);

const direction = computed(() => ({
  transform: `scaleX(${directionRight.value ? 1 : -1})`,
}));

function moveFish() {
  directionRight.value = !directionRight.value;
  position.value = {
    x: generateXPosition(),
    y: generateYPosition(),
  };
}

function generateDistanceInRange(offsetValue, clientValue) {
  const safeDistanceValue = Math.floor(
    Math.random() * offsetValue - clientValue
  );

  return safeDistanceValue < 0 ? 0 : safeDistanceValue;
}

function generateXPosition() {
  const safeXDistance = generateDistanceInRange(
    aquariumEl.value.offsetWidth,
    fishEl.value.clientWidth
  );

  if (safeXDistance > position.value.x) {
    directionRight.value = true;
    return safeXDistance;
  } else if (safeXDistance < position.value.x) {
    directionRight.value = false;
    return safeXDistance;
  } else return generateXPosition();
}

function generateYPosition() {
  const safeYDistance = generateDistanceInRange(
    aquariumEl.value.offsetHeight,
    fishEl.value.clientHeight
  );

  if (safeYDistance !== position.value.y) return safeYDistance;
  else return generateYPosition();
}

onMounted(() => {
  aquariumEl.value = document.querySelector("#aquarium");

  if (aquariumEl.value) {
    setTimeout(() => {
      moveFish();
      doTimeout();
    }, 2);
  }
});
</script>

<template>
  <div class="fish" ref="fishEl" :style="movement">
    <img
      :src="fishImageUrl"
      :alt="fish.type"
      class="fish__image"
      :style="direction"
    />
    <span class="fish__name-tag">{{ fish.name }}</span>
  </div>
</template>

<style scoped>
.fish {
  position: absolute;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.fish__image {
  width: 100px;
  transition: all 200ms ease;
}

.fish__name-tag {
  background-color: rgba(0, 0, 0, 0.856);
  color: white;
  text-transform: capitalize;
  text-align: center;
  border: none;
  border-radius: 4px;
  padding: 2px 10px;
  width: max-content;
}
</style>
