<template>
  <div class="app">
    <h1>Memory game</h1>
    <div>
      <p v-if="cards.length > 0" id="turns">Turns: {{ turns }}</p>
      <div class="card-grid" v-if="cards.length > 0">
        <single-card
          v-for="card in cards"
          :key="card.id"
          :flipped="card === choiceOne || card === choiceTwo || card.matched"
          :singleCard="card"
          :disabled="disabled"
          :userDisabled="userDisabled"
          :handleChoice="handleChoice"
        >
        </single-card>
      </div>
    </div>
    <p v-if="winner">
      <winner-modal :turns="turns" :playAgain="shuffleCards"></winner-modal>
    </p>
    <button v-on:click="shuffleCards()">
      {{ cards.length > 0 ? "new game" : "start game" }}
    </button>
  </div>
</template>

<script>
import SingleCard from "./components/SingleCard.vue";
import WinnerModal from "./components/WinnerModal.vue";

const cardsArray = [
  { matched: false, src: "columnar-trees.svg", text: "Three Columnar Trees" },
  { matched: false, src: "flower.svg", text: "Single Flower" },
  { matched: false, src: "pine-tree.svg", text: "Single Pine Tree" },
  { matched: false, src: "round-tree.svg", text: "Rounded Tree" },
  { matched: false, src: "tree.svg", text: "Tree With Branches" },
  { matched: false, src: "plant.svg", text: "Small Plant" },
];

export default {
  name: "App",
  components: {
    SingleCard,
    WinnerModal,
  },
  data() {
    return {
      cards: [],
      turns: 0,
      choiceOne: null,
      choiceTwo: null,
      disabled: false,
      userDisabled: false,
      winner: false,
    };
  },
  watch: {
    choiceTwo(newChoice, oldChoice) {
      if (newChoice !== oldChoice && newChoice !== null) {
        this.compareCards();
      }
    },
    cards(newCards, oldCards) {
      if (newCards !== oldCards && newCards !== []) {
        this.checkAllTrue();
      }
    },
  },
  methods: {
    compareCards() {
      this.userDisabled = true;
      if (this.choiceTwo.src === this.choiceOne.src) {
        const updatedCards = this.cards.map((card) => {
          if (card.src === this.choiceOne.src) {
            return { ...card, matched: true };
          } else {
            return card;
          }
        });
        this.cards = updatedCards;
        this.userDisabled = false;
        this.resetTurn();
      } else {
        setTimeout(() => {
          this.userDisabled = false;
          this.resetTurn();
        }, 1000);
      }
    },
    shuffleCards() {
      const shufffled = [...cardsArray, ...cardsArray]
        .sort(() => Math.random() - 0.5)
        .map((card) => ({ ...card, id: Math.random() }));

      this.choiceOne = null;
      this.choiceTwo = null;
      this.cards = shufffled;
      this.winner = false;
      this.turns = 0;
    },
    handleChoice(card) {
      if (this.choiceOne) {
        this.choiceTwo = card;
      } else {
        this.choiceOne = card;
      }
    },
    resetTurn() {
      this.choiceOne = null;
      this.choiceTwo = null;
      this.turns = this.turns + 1;
      this.disabled = false;
    },
    checkAllTrue() {
      const isTrue = (currentValue) => currentValue.matched === true;
      if (this.cards.every(isTrue)) {
        setTimeout(() => {
          this.winner = true;
        }, 500);
      }
    },
  },
};
</script>

<style>
@font-face {
  font-family: "Exo";
  src: url("./assets/fonts/Exo-Regular.ttf");
}
@font-face {
  font-family: "Exo2";
  src: url("./assets/fonts/Exo2-Regular.ttf");
}

body {
  margin: 0;
  font-size: clamp(1rem, 0.2447rem + 2.4169vw, 1.5rem);
  background-color: #1a1a1a;
  text-align: center;
  color: #ffffff;
}

.app {
  max-width: 860px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 86vh;
  justify-content: space-between;
}

:root {
  --color-white: #ffffff;
  --color-black: #000000;
  --color-green: #38c28a;
  --font-exo: Exo;
  --font-exo2: Exo2;
}

.App {
  max-width: 860px;
  margin: 1rem auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 75vh;
  justify-content: space-between;
  @media screen {
  }
}

.card-grid {
  margin: 40px 0;
  padding: 0;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}

h1 {
  color: var(--color-green);
  font-family: var(--font-exo);
  text-transform: uppercase;
  font-size: 2em;
}

p {
  font-family: var(--font-exo2);
  color: var(--color-white);
  font-size: 1em;
  margin: 0;
}

button {
  background: none;
  font-family: var(--font-exo);
  text-transform: uppercase;
  font-size: 1.2em;
  border: none;
  padding: 6px 30px;
  color: var(--color-white);
  border: 2px solid var(--color-white);
  font-weight: bold;
  cursor: pointer;
  transition: 0.2s;
  margin-bottom: 3vh;
}

button:hover {
  color: var(--color-green);
  border-color: var(--color-green);
}
</style>
