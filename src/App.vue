<template>
  <v-app>
    <v-main>
      <v-row>
        <v-col>
          <Sudoku class="sudoku"/>
        </v-col>
        <v-col>
          <br /> <br />
          STEP: {{ step_text }}
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
    step: 0
  }),

  computed: {
    step_text(){
      switch(this.step){
        default:
        case 0: return "輸入題目";
        case 1: return "準備階段";
        case 2: return "正在演算";
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