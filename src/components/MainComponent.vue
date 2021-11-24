<template>
  <div>
    <v-container class="text-center pt-12">
      <h2 class="text-h2">Number Generator</h2>
      <hr style="margin-left: 25%; margin-right: 25%;margin-top: 1%;">
      <br>
      <v-row justify="center">
        <v-col cols="12" md="3">
          <v-select v-model="selected" :items="options" outlined label="Number of Sets" hide-details clearable></v-select>
        </v-col>
      </v-row>
      <v-row justify="center">
        <v-col cols="12" md="3">
          <ul>
            <li v-for="(set, index) in sets" :key="index" class="py-3">
              <strong>Set {{ index + 1 }}:</strong>
              {{ set.join(', ') }}
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
      selected: null,
      options: [
        { text: '1', value: 1 },
        { text: '2', value: 2 },
        { text: '3', value: 3 },
        { text: '4', value: 4 },
        { text: '5', value: 5 },
        { text: '6', value: 6 },
        { text: '7', value: 7 },
        { text: '8', value: 8 },
        { text: '9', value: 9 },
        { text: '10', value: 10 },
      ],
      sets: [],
    };
  },

  watch: {
    selected(newValue, oldValue) {
      (newValue && newValue != oldValue) ? this.getNumbers() : this.sets = [];
    }
  },

  methods: {
    getNumbers() {
      let temp;
      this.sets = [];

      for (let i = 0; i < this.selected; i++) {
        temp = [];
        while (temp.length != 6) {
          let num = Math.floor(Math.random() * (45 - 1) + 1);
          if (!temp.includes(num)) {
            temp.push(num);
          }
        }

        this.sets.push(temp);
      }
    }
  },
}
</script>

<style>
ul {
  list-style-type: none;
}
</style>