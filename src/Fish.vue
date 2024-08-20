<script setup>
import { ref, computed, onMounted } from "vue";

//Components
import SpeechBubble from "./SpeechBubble.vue";

const props = defineProps({
  fish: {
    type: Object,
    required: true,
  },
});

const fishImageUrl = computed(() =>
  hunger.value > 0 ? `../assets/${props.fish.type}.png` : `../assets/dead.png`
);

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

const hunger = ref(100);

const myInterval = ref(null);

function hungerValue() {
  myInterval.value = setInterval(() => {
    if (hunger.value > 0) {
      hunger.value = hunger.value - 1;
      console.log(hunger.value);
    }
    // emit remove function
    else clearInterval(myInterval);
  }, 50);
}

const hungerBarColor = computed(() => {
  if (hunger.value > 70) return "green";
  else if (hunger.value <= 70 && hunger.value > 30) return "orange";
  else return "red";
});

const hungerBarWidth = computed(() => `${hunger.value}%`);

function resetHungerValue() {
  //to do wyrzutu ;)
  hunger.value = 100;
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
      hungerValue();
      moveFish();
      doTimeout();
    }, 2);
  }
});
</script>

<template>
  <div class="fish" ref="fishEl" :style="movement">
    <SpeechBubble v-if="hunger < 30" />
    <img
      :src="fishImageUrl"
      :alt="fish.type"
      class="fish__image"
      :style="direction"
      @click="resetHungerValue"
    />
    <div class="fish__badge">
      <span class="fish__name">{{ fish.name }}</span>
      <div class="fish__hunger"></div>
    </div>
  </div>
</template>

<style scoped>
.fish {
  position: absolute;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 6px;
}
.fish__image {
  width: 100px;
  transition: all 200ms ease;
  cursor: pointer;
}

.fish__badge {
  background-color: rgba(0, 0, 0, 0.7);
  border: none;
  border-radius: 4px;
  width: max-content;
}

.fish__name {
  padding: 2px 10px;
  color: white;
  text-transform: capitalize;
  text-align: center;
}

.fish__hunger {
  height: 3px;
  width: v-bind(hungerBarWidth);
  background-color: v-bind(hungerBarColor);
}
</style>
