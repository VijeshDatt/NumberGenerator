<template>
  <div>
    <v-container class="text-center pt-12 mx-6 mx-md-auto">
      <h2 class="text-h2">Number Generator</h2>
      <v-divider class="mx-16 my-5"></v-divider>

      <v-row justify="center">
        <v-col cols="12" md="4">
          <v-slider v-model="size" min="4" max="10" thumb-label="always" class="mt-7" ticks="always" tick-size="2" @change="getNumbers"></v-slider>
        </v-col>
      </v-row>

      <v-row justify="center">
        <v-col cols="12" md="4">
          <ul>
            <li v-for="(set, index) in results" :key="index" class="py-3">
              <strong>Set {{ index + 1 }}:</strong>
              {{ set.join(", ") }}
            </li>
          </ul>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      size: null,
      results: [],
      games: [
        [1, 5, 14, 15, 34, 42, 40, 45], //4413
        [1, 2, 23, 24, 28, 30, 39, 29], //4415
        [7, 8, 15, 16, 26, 31, 44, 6], //4417
        [17, 20, 30, 36, 37, 44, 4, 19], //4419
        [10, 14, 21, 22, 23, 42, 11, 7], //4421
        [9, 11, 25, 32, 33, 44, 2, 20], //4423
        [8, 12, 13, 17, 39, 41, 27, 32], //4425
        [1, 20, 24, 31, 37, 39, 19, 41], //4427
        [2, 3, 11, 24, 38, 39, 22, 35], //4429
        [12, 13, 17, 34, 36, 42, 41, 38], //4431
        [8, 33, 37, 39, 40, 41, 35, 2], //4433
        [11, 22, 27, 33, 35, 37, 8, 1], //4435
        [5, 8, 13, 24, 29, 36, 15, 40], //4437
        [3, 11, 19, 22, 23, 29, 20, 30], //4439
        [1, 2, 12, 13, 20, 42, 3, 15], //4441
        [7, 11, 22, 25, 28, 34, 32, 35], //4443
        [16, 17, 26, 38, 39, 42, 32, 37], //4445
        [5, 8, 15, 18, 19, 30, 7, 26], //4447
        [3, 17, 28, 35, 40, 44, 26, 33], //4451
        [1, 12, 16, 17, 19, 32, 27, 34], //4453
      ],
      list: [],
      sets: [],
    };
  },

  watch: {},

  methods: {
    getNumbers() {
      this.results = [];
      // Filter to get only repeating numbers
      let list = this.list.filter((l) => l.tally > 2).map((m) => m.number);

      // Loop based on game size (4 - 10)
      for (let i = 0; i < this.size; i++) {
        let even = [];
        let odd = [];

        while (even.length != 3) {
          // Get random value from list of used numbers
          let num = list[Math.floor(Math.random() * list.length)];

          // if even and not in array, then push to array
          if (num % 2 == 0 && !even.includes(num) && even.length <= 3) even.push(num);
        }

        while (odd.length != 3) {
          // Get random value from list of used numbers
          let num = list[Math.floor(Math.random() * list.length)];

          // if odd and not in array, then push to array
          if (num % 2 != 0 && !odd.includes(num) && odd.length <= 3) odd.push(num);
        }

        this.results.push(even.concat(odd).sort((a, b) => a - b));
      }
    },

    getStats() {
      // Get total numbers from 1-45
      for (let i = 1; i <= 45; i++) this.list.push({ number: i, tally: 0 });

      // Loop through each game and get status
      this.games.forEach((game) => {
        game.forEach((num) => {
          let index = this.list.findIndex((l) => l.number == num);

          this.list[index].tally += 1;
        });
      });

      this.getNumbers();
    },
  },

  mounted() {
    // this.getNumbers();
    this.getStats();
  },
};
</script>

<style>
ul {
  list-style-type: none;
}
</style>
