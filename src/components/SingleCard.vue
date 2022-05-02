<template>
  <div class="card">
    <div v-bind:class="flipped ? 'content flipped' : 'content'">
      <span class="front square-card">
        <img
          :src="require('@/assets/icons/' + singleCard.src)"
          v-bind:class="singleCard.matched ? 'disabled' : ''"
          :alt="singleCard.text"
        />
      </span>
      <button
        class="back square-card"
        v-on:click="
          {
            handleClick();
          }
        "
        :disabled="this.userDisabled"
      >
        <img
          src="@/assets/icons/back-leaf.svg"
          class="back-image"
          alt="Back Card"
        />
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["flipped", "singleCard", "disabled", "handleChoice", "userDisabled"],
  name: "SingleCard",
  methods: {
    handleClick() {
      if (!this.disabled) {
        this.handleChoice(this.singleCard);
      }
    },
  },
};
</script>

<style>
.card {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 3.7em;
  height: 3.7em;
  justify-self: center;
}

.content {
  height: 100%;
  width: 100%;
  transition-delay: 0.2s;
  transition: all ease 0.4s;
  position: absolute;
  transform-style: preserve-3d;
}

.content.flipped {
  transform: rotateY(180deg);
}

img {
  transition: all ease 0.4s;
  transition-delay: 0.5s;
  min-width: 80%;
}

.back-image {
  filter: invert(1);
}

img.disabled {
  filter: opacity(0.35);
}

.front {
  background: var(--color-white);
  transform: rotateY(180deg);
}

.back {
  cursor: pointer;
  background: #2f8838;
  border: 0;
  padding: 0;
  margin: 0;
  transform: rotateX(0deg);
}

.back:disabled {
  cursor: auto;
}

.square-card {
  display: flex;
  align-items: center;
  justify-content: center;
  position: absolute;
  backface-visibility: hidden;
  height: 100%;
  width: 100%;
}
</style>