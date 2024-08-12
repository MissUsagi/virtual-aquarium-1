<script setup>
import { ref } from "vue";
import randomFishNames from "./randomFishNames";
import fishList from "./fishList";

const emit = defineEmits(["add-fish"]);

const fishData = fishList;
const randomNames = randomFishNames;

const defaultFishType = fishData[0].type;

const input = ref();

function generateRandomName() {
  const fishNameId = Math.floor(Math.random() * randomNames.length);
  newFish.value.name = randomNames[fishNameId];
}

function generateFishId() {
  return (
    Date.now().toString(36) +
    Math.floor(
      Math.pow(10, 12) + Math.random() * 9 * Math.pow(10, 12)
    ).toString(36)
  );
}

const newFish = ref({
  name: "",
  id: "",
  type: defaultFishType,
});

const showNameValidationHint = ref(false);

function addNewFish() {
  newFish.value.id = generateFishId();
  const { name, type, id } = newFish.value;

  //validateForm();
  // if (!name || !type || !id) return;
  // alert("Coś poszło nie tak :( Odśwież stronę i spróbuj ponownie.");

  if (!name) {
    showNameValidationHint.value = true;
    return;
  } else if (name && type && id) {
    showNameValidationHint.value = false;
    emit("add-fish", { ...newFish.value });
  }

  clearForm();
  input.value.focus();
}

function selectFishType(fishType) {
  newFish.value.type = fishType;
}

function clearForm() {
  newFish.value.name = "";
  newFish.value.id = "";
  newFish.value.type = defaultFishType;
}
</script>

<template>
  <form @submit.prevent="addNewFish" class="fish-form">
    <p class="fish-form__text">Fish Type</p>
    <ul class="fish-list">
      <li
        v-for="fish in fishData"
        :key="fish.id"
        :fish="fish"
        @click="selectFishType(fish.type)"
      >
        <img
          :src="`../assets/${fish.type}.png`"
          :alt="fish.type"
          class="fish"
          :class="{ selected: fish.type === newFish.type }"
        />
      </li>
    </ul>
    <div class="fish-form__input-group">
      <label for="fishName" class="fish-form__text">Name your fish</label>
      <div class="fish-form__input">
        <input
          ref="input"
          id="fishName"
          class="fish-form__input-field"
          v-model="newFish.name"
          placeholder="Enter or generate name"
        />
        <button
          type="button"
          class="fish-form__input-button"
          @click="generateRandomName"
        >
          <img src="../public/assets/random.svg" width="30px" alt="Random" />
        </button>
      </div>
      <p v-show="showNameValidationHint" class="fish-form__input-hint">
        This field cannot be empty.
      </p>
    </div>
    <button type="submit" class="fish-form__button">Add Fish</button>
  </form>
</template>

<style scoped>
.fish-form {
  display: flex;
  flex-direction: column;
  gap: 15px;
  background-color: rgb(7, 18, 66);
  width: 300px;
  padding: 20px;
  height: 100vh;
}

.fish-form__text {
  color: white;
  font-size: 16px;
  font-weight: 600;
}

.fish-form__input {
  display: flex;
}

.fish-form__input-field {
  padding: 5px 8px;
  border-radius: 3px 0 0 3px;
}

.fish-form__input-button {
  background-color: #006eff;
  padding: 5px;
  width: 40px;
  border-radius: 0 3px 3px 0;
  color: white;
}

.fish-form__input-group {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.fish-form__input-hint {
  color: rgb(250, 81, 29);
  font-size: 0.8rem;
}

.fish-list {
  display: grid;
  gap: 10px;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(3, 1fr);
  align-items: center;
}

.fish {
  width: 80%;
  cursor: pointer;
}

.selected {
  filter: drop-shadow(2px 2px 0 #006eff) drop-shadow(-2px -2px 0 #006eff)
    drop-shadow(2px -2px 0 #006effff) drop-shadow(-2px 2px 0 #006eff);
}

.fish:hover:not(.selected) {
  filter: drop-shadow(2px 2px 0 #006eff) drop-shadow(-2px -2px 0 #006eff)
    drop-shadow(2px -2px 0 #006effff) drop-shadow(-2px 2px 0 #006eff);
}

.fish-form__button {
  background-color: rgb(218, 69, 0);
  color: white;
  width: 100%;
  font-size: 16px;
  font-weight: 600;
  padding: 10px;
  border: none;
  border-radius: 4px;
}

.fish-form__button:hover {
  background-color: rgb(248, 103, 50);
}
</style>
