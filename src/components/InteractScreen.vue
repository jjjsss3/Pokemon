<template>
  <div class="container">
    <div class="info">
      <svg
        class="back"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 512 512"
        @click="onBack"
      >
        <!--! Font Awesome Pro 6.4.0 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. -->
        <path
          fill="#eee"
          d="M377.9 105.9L500.7 228.7c7.2 7.2 11.3 17.1 11.3 27.3s-4.1 20.1-11.3 27.3L377.9 406.1c-6.4 6.4-15 9.9-24 9.9c-18.7 0-33.9-15.2-33.9-33.9l0-62.1-128 0c-17.7 0-32-14.3-32-32l0-64c0-17.7 14.3-32 32-32l128 0 0-62.1c0-18.7 15.2-33.9 33.9-33.9c9 0 17.6 3.6 24 9.9zM160 96L96 96c-17.7 0-32 14.3-32 32l0 256c0 17.7 14.3 32 32 32l64 0c17.7 0 32 14.3 32 32s-14.3 32-32 32l-64 0c-53 0-96-43-96-96L0 128C0 75 43 32 96 32l64 0c17.7 0 32 14.3 32 32s-14.3 32-32 32z"
        />
      </svg>
      <p class="time">
        Time:
        {{ this.formatMilliseconds(Math.abs(this.end_time - this.start_time)) }}
      </p>
      <p class="step">Step : {{ step }}</p>
    </div>

    <div class="grid-container">
      <div
        v-for="(element, index) in arrayPokemon"
        :key="index"
        class="grid-item"
        @click="onOpen(index)"
        :class="{
          active: statusCard[index],
          isRight: rightPairs.includes(index),
        }"
      >
        <card-view :src="'icon_back'" class="card__face card__face--front" />
        <card-view :src="element" class="card__face card__face--back" />
      </div>
    </div>
  </div>
</template>
<script>
import CardView from "./CardView.vue";

export default {
  components: {
    CardView,
  },
  props: {
    mode: String,
  },
  data() {
    return {
      arrayPokemon: [],
      statusCard: [],
      rightPairs: [],
      grids: {
        gridTemplateRows: `repeat(${this.mode}, 1fr)`,
        item_height: `${100 / this.mode - 3}vh`,
        item_width: this.mode == "4" ? "30%" : "50%",
      },
      selected: [],
      start_time: new Date(),
      end_time: new Date(),
      isRunning: false,
      step: 0,
    };
  },
  created() {
    this.arrayPokemon = this.randomPokemon();
    this.statusCard = new Array(this.arrayPokemon.length).fill(false);
  },
  mounted() {
    setInterval(() => {
      this.end_time = new Date();
    }, 1000);
  },
  methods: {
    onBack() {
      this.selectMode(["lobby"]);
    },
    getImagePath(pokemonName) {
      return require(`@/assets/images/${pokemonName}.png`);
    },
    randomPokemon() {
      let randomArray = Array.from(
        { length: Math.pow(parseInt(this.mode), 2) / 2 },
        () => String(Math.floor(Math.random() * 64) + 1)
      );
      let mergedArray = randomArray.concat(randomArray);
      // Shuffle the merged array
      mergedArray.sort(() => Math.random() - 0.5);
      return mergedArray;
    },
    getGridTemplate() {
      // Define a template literal string using the value of "n"
      return `repeat(${this.n}, 1fr)`;
    },
    flipCard(index) {
      this.statusCard[index] = !this.statusCard[index];
    },
    formatMilliseconds(milliseconds) {
      // Convert milliseconds to seconds
      const totalSeconds = Math.floor(milliseconds / 1000);

      // Calculate hours, minutes, and remaining seconds
      const hours = Math.floor(totalSeconds / 3600);
      const minutes = Math.floor((totalSeconds % 3600) / 60);
      const seconds = totalSeconds % 60;

      // Format the time string

      const timeString = `${hours.toString()}h : ${minutes.toString()}m : ${seconds.toString()}s`;

      return timeString;
    },
    onOpen(index) {
      if (this.isRunning) return;

      this.flipCard(index);
      this.selected.push(index);
      const renew = async () => {
        await new Promise((resolve) => {
          if (this.selected.length == 2) {
            this.isRunning = true;
            this.step++;
            setTimeout(() => {
              if (
                this.arrayPokemon[this.selected[0]] !=
                this.arrayPokemon[this.selected[1]]
              ) {
                this.flipCard(this.selected[0]);
                this.flipCard(this.selected[1]);
                this.statusCard.every(Boolean) && console.log("true");
              } else {
                this.rightPairs.push(this.selected[0]);
                this.rightPairs.push(this.selected[1]);

                if (this.statusCard.every(Boolean)) {
                  this.end_time = new Date();
                  this.selectMode([
                    "finished",
                    this.formatMilliseconds(
                      Math.abs(this.end_time - this.start_time)
                    ),
                    this.step,
                  ]);
                }
              }
              this.selected = [];
              resolve();
            }, 600);
          }
        });
      };

      renew().then(() => {
        this.isRunning = false;
      });
    },
    selectMode(value) {
      this.$emit("selectMode", value);
    },
  },
};
</script>
<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-end;
  height: 100vh;
}

.container .info {
  width: v-bind("grids.item_width");
  margin-bottom: 3vh;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.container .info p {
  font-size: 20px;
}
.container .info .back {
  width: 5%;
  cursor: pointer;
}
.grid-container {
  height: 85vh;
  margin-bottom: 5vh;
  display: grid;
  width: v-bind("grids.item_width");
  /* Use string interpolation to set the CSS properties */
  grid-template-columns: v-bind("grids.gridTemplateRows");
  grid-template-rows: v-bind("grids.gridTemplateRows");
  gap: 15px; /* Add any desired gap between items */
}
.grid-item {
  border-radius: 20px;
  height: 100%;
  cursor: pointer;
  transform-style: preserve-3d;
  transition: transform 0.5s;
}

.grid-item.active {
  transform: rotateY(-180deg);
}
.grid-item.isRight {
  pointer-events: none;
}
.grid-item.isRight .card__face--back {
  background: #deacf5;
}

.card__face {
  position: absolute;
  backface-visibility: hidden;
}

.card__face--front {
  background: #6237a0;
}

.card__face--back {
  background: #9754cb;

  transform: rotateY(180deg);
}
@media only screen and (max-width: 600px) {
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-end;
    height: 100vh;
  }
  .container .info {
    width: 90%;
    margin-bottom: 3vh;
    display: grid;
    /* Use string interpolation to set the CSS properties */
    grid-template-columns: 1fr 3fr 1fr;
  }
  .container .info p {
    font-family: "Poppins", sans-serif;
    font-size: 16px;
    text-align: center;
  }
  .container .info .back {
    width: 25%;
    cursor: pointer;
  }
  .grid-container {
    margin-bottom: 5vh;
    width: 90%;
    gap: 10px; /* Add any desired gap between items */
  }
}
</style>
