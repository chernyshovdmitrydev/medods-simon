<script>
export default {
  props: {
    color: String,
    isBlocked: Boolean,
  },
  data() {
    return {
      isActive: false,
    };
  },
  methods: {
    beep() {
      const audio = new Audio(`/medods-simon/audio/${this.color}.mp3`);
      audio.volume = 0.2;
      audio.play();
    },
    blink() {
      (this.isActive = true),
        setTimeout(() => {
          this.isActive = false;
        }, 300);
    },
    clickSimonButton() {
      this.$emit("simonButton-click", this.color);
    },
  },
};
</script>

<template>
  <button
    @click="clickSimonButton"
    :disabled="isBlocked"
    class="simon-button"
    :class="[color, { isActive: isActive }]"
  ></button>
</template>

<style scoped>
.simon-button {
  margin: 10px;
  cursor: pointer;
  width: 13em;
  height: 13em;
  filter: brightness(75%);
}

.green {
  background-color: green;
  border-radius: 100% 0 0 0;
}

.red {
  background-color: red;
  border-radius: 0 100% 0 0;
}

.blue {
  background-color: blue;
  border-radius: 0 0 100% 0;
}

.yellow {
  background-color: yellow;
  border-radius: 0 0 0 100%;
}

.isActive {
  filter: brightness(200%);
}
</style>
