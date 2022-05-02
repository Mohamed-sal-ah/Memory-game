<template>
  <div id="modal-background">
    <div id="modal">
      <h1>You win!</h1>
      <p>You have completed this game with {{ turns }} Turns</p>
      <button v-on:click="playAgain()">Play again</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["turns", "playAgain"],
  name: "WinnerModal",
};
</script>

<style>
#modal-background {
  background-color: rgba(0, 0, 0, 0.75);
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1;
}

#modal {
  background-color: var(--color-white);
  color: var(--color-black);
  padding: 1.5rem;
}

#modal h1 {
  font-size: 1.65em;
  font-weight: bold;
  color: #213547;
  font-family: var(--font-exo2);
  margin: 0;
  letter-spacing: 1px;
}

#modal p {
  color: var(--color-black);
  font-family: var(--font-exo2);
  font-size: 0.8em;
  margin: 1rem 0;
}

#modal button {
  font-size: 0.9em;
  background-color: #840000;
  color: var(--color-white);
  box-shadow: none;
  border: none;
  margin: 1rem 0;
}

#modal button:hover {
  background-color: #214725;
}
</style>