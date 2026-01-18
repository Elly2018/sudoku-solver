<template>
  <v-app>
    <v-main>
      <v-row class="pa-6">
        <v-col>
          <Sudoku class="sudoku" :step="step"/>
        </v-col>
        <v-col>
          <br /> <br />
          STEP: {{ step_text }}
          <br /> <br />
          <v-btn color="primary" v-if="step == 0" @click="step = 1">Finish Puzzle</v-btn>
          <v-btn color="error" v-if="step == 1" @click="step = 0">Reset Puzzle</v-btn>
          <v-btn color="primary" v-if="step == 1" @click="step = 2">Start</v-btn>
          <v-btn color="warning" v-if="step == 2" @click="step = 1">Stop</v-btn>
          <br /> <br />
          <v-select v-model.number="type" label="Step Type" hide-details :items="[
            { title: 'Once', value: 0 },
            { title: 'GO !', value: 1 },
          ]"></v-select>
          <br />
          <v-select v-model.number="depth" label="Depth" hide-details :items="[
            { title: '0', value: 0 },
            { title: '1', value: 1 },
            { title: '2', value: 2 },
            { title: '3', value: 3 },
            { title: '4', value: 4 },
            { title: '5', value: 5 },
            { title: '10', value: 10 },
            { title: '50', value: 50 },
            { title: '100', value: 100 },
            { title: 'Unlimited', value: -1 },
          ]"></v-select>
        </v-col>
      </v-row>
    </v-main>
  </v-app>
</template>

<script>
import Sudoku from './components/Sudoku.vue'

export default {
  name: 'App',

  components: {
    Sudoku,
  },

  data: () => ({
    // Current app step
    // 0: Enter init
    // 1: Before solver
    // 2: Process
    step: 0,
    depth: 0,
    // Processing type
    // 0: One iteraction only
    // 1: Go as much as you can
    type: 0.
  }),

  computed: {
    step_text(){
      switch(this.step){
        default:
        case 0: return "Enter your puzzle";
        case 1: return "Prepare";
        case 2: return "Processing...";
      }
    }
  }
}
</script>

<style scoped>
.sudoku {
  width: calc(50px * 9);
  height: calc(50px * 9);
}
</style>