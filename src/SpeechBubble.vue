<template>
  <div class="speech-bubble down">
    <p class="speech-bubble-text">
      Feed me<span class="speech-bubble-text accent">!</span>
    </p>
  </div>
</template>

<style>
.speech-bubble {
  position: relative;
  background-color: rgba(255, 255, 255, 0.8);
  padding: 8px 10px;
  border-radius: 15px;
}

.speech-bubble.down::after {
  content: "";
  position: absolute;
  width: 0;
  bottom: -6px;
  right: 25%;
  border: 6px solid transparent;
  border-top-color: rgba(255, 255, 255, 0.8);
  border-bottom: 0;
}

.speech-bubble-text {
  font-weight: 600;
}

.speech-bubble-text.accent {
  font-weight: 800;
  color: red;
}
</style>
