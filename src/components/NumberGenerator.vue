<template>
  <v-container class="text-center px-4">
    <v-row justify="center" align="center">
      <v-col cols="12">
        <h2 class="text-h3 mt-6">Number Generator</h2>
      </v-col>

      <v-col cols="12">
        <v-divider class="mx-16 my-5"></v-divider>
      </v-col>

      <v-col cols="12" md="6">
        <v-slider
          v-model="size"
          color="primary"
          thumb-color="primary"
          min="4"
          max="10"
          step="1"
          thumb-label="always"
          class="mx-9 mt-7"
          show-ticks="always"
          tick-size="2"
        ></v-slider>
      </v-col>

      <v-col cols="12" class="d-flex justify-center mb-6">
        <v-btn @click="getNumbers" rounded="lg" color="primary" variant="tonal"
          >Refresh</v-btn
        >
      </v-col>
    </v-row>

    <v-row justify="center">
      <v-col
        cols="12"
        md="5"
        v-for="(set, index) in results"
        :key="`card-${index}`"
      >
        <v-card variant="tonal" rounded="lg" density="compact">
          <v-card-title class="text-start mb-4">
            Game {{ index + 1 }}
          </v-card-title>

          <v-card-text class="px-0">
            <v-row
              no-gutters
              align="center"
              justify="space-between"
              v-for="(row, idx) in rows"
              :key="`row-${idx}`"
            >
              <v-col v-for="(num, i) in row" :key="`chip-${i}`">
                <v-chip
                  v-if="num"
                  :variant="set.includes(num) ? 'flat' : 'plain'"
                  :color="set.includes(num) ? 'primary' : undefined"
                >
                  {{ num }}
                </v-chip>
              </v-col>
            </v-row>

            <!-- <v-chip-group readonly v-for="(row, idx) in rows" :key="`row-${idx}`">
                <v-chip v-for="(num, i) in row" :key="`chip-${i}`"> {{ num }} </v-chip>
                <v-chip v-for="(num, idx) in set" :key="`chip-${idx}`"> {{ num }} </v-chip>
              </v-chip-group> -->
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      size: null,
      results: [],
      rows: {
        row1: [1, 2, 3, 4, 5, 6, 7, 8],
        row2: [9, 10, 11, 12, 13, 14, 15, 16],
        row3: [17, 18, 19, 20, 21, 22, 23, 24],
        row4: [25, 26, 27, 28, 29, 30, 31, 32],
        row5: [33, 34, 35, 36, 37, 38, 39, 40],
        row6: [41, 42, 43, 44, 45, , , ,],
      },
      games: [
        [15, 19, 29, 34, 37, 40], // 4507
        [13, 15, 18, 24, 27, 30], //4509
        [2, 8, 10, 21, 28, 45], //4511
        [5, 15, 22, 26, 33, 43], //4513
        [1, 25, 29, 35, 42, 43], //4515
        [1, 3, 6, 10, 18, 42], //4517
        [5, 6, 30, 37, 39, 43], //4519
        [5, 6, 8, 29, 38, 44], //4521
        [3, 6, 17, 25, 40, 42], //4523
        [8, 13, 16, 30, 39, 41], //4525
        [1, 7, 11, 13, 16, 35], //4527
        [6, 7, 11, 21, 30, 35], //4529
        [9, 18, 25, 27, 38, 44], //4531
        [4, 12, 26, 29, 40, 41], //4533
        [8, 10, 19, 23, 31, 33], //4535
        [6, 8, 18, 19, 29, 38], //4537
      ],
      list: [],
      sets: [],
    };
  },

  watch: {
    size() {
      this.getNumbers();
    },
  },

  methods: {
    getNumbers() {
      this.results = [];
      // Filter to get only repeating numbers
      let max = Math.max(...this.list.map((e) => e.tally)) - 3;
      let list = this.list.filter((l) => l.tally > max).map((m) => m.number);

      // Loop based on game size (4 - 10)
      for (let i = 0; i < this.size; i++) {
        let numbers = [];

        while (numbers.length != 6) {
          let num = list[Math.floor(Math.random() * list.length)];

          if (!numbers.includes(num) && numbers.length <= 6) numbers.push(num);
        }

        this.results.push(numbers.sort((a, b) => a - b));

        // let even = [];
        // let odd = [];
        // if even and not in array, then push to array
        // while (even.length != 3) {
        //   // Get random value from list of used numbers
        //   let num = list[Math.floor(Math.random() * list.length)];

        //   // if even and not in array, then push to array
        //   if (num % 2 == 0 && !even.includes(num) && even.length <= 3)
        //     even.push(num);
        // }

        // while (odd.length != 3) {
        //   // Get random value from list of used numbers
        //   let num = list[Math.floor(Math.random() * list.length)];

        //   // if odd and not in array, then push to array
        //   if (num % 2 != 0 && !odd.includes(num) && odd.length <= 3)
        //     odd.push(num);
        // }

        // this.results.push(even.concat(odd).sort((a, b) => a - b));
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
    this.size = 4;
  },
};
</script>

<style>
ul {
  list-style-type: none;
}
</style>
