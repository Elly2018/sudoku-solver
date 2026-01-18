<template>
  <v-container>
    <v-row v-for="(y, yindex) in 9" :key="`row-${yindex}`" no-gutters>
      <v-col v-for="(x, xindex) in 9" :key="`col-${xindex}-row-${yindex}`"
        class="sudoku_grid"
        :class="{ 'borde_black_col': x == 3 || x == 6,
          'borde_black_row': y == 3 || y == 6,
          'sudoku_cursor': x == cursor[0] + 1 && y == cursor[1] + 1
        }">
        <p class="sudoku_number">
          {{ get_value(x - 1, y - 1) == 0 ? '' : get_value(x - 1, y - 1) }}
        </p>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>

export default {
  name: 'HelloWorld',

  mounted(){
    for(let i = 0; i < 81; i++){
      this.init[i] = 0
      this.answer[i] = 0
    }
    this.loop_handler = setInterval(this.loop, this.timer)
    addEventListener("keydown", this.keydown)
  },
  unmounted(){
    clearInterval(this.loop_handler)
  },

  data: () => ({
    // Init value of the grid
    init: [] ,
    // Trying to answer
    answer: [],
    // Interval handler
    loop_handler: undefined,
    // Update step, 1000 => 1 sec
    timer: 50,
    cursor: [0, 0],
  }),

  methods: {
    update_handler(){
      clearInterval(this.loop_handler)
      this.loop_handler = setInterval(this.loop, this.timer)
    },
    loop(){

    },
    get_value(x, y){
      return this.answer[y * 9 + x]
    },
    keydown(event){
      if(event.key == 'ArrowUp' && this.cursor[1] > 0){
        this.cursor[1] -= 1
      }
      else if(event.key == 'ArrowDown' && this.cursor[1] < 8){
        this.cursor[1] += 1
      }
      else if(event.key == 'ArrowLeft' && this.cursor[0] > 0){
        this.cursor[0] -= 1
      }
      else if(event.key == 'ArrowRight' && this.cursor[0] < 8){
        this.cursor[0] += 1
      }
    }
  }
}
</script>

<style scoped>
.borde_black_col {
  border-right: 1px solid black !important;
}
.borde_black_row {
  border-bottom: 1px solid black !important;
}
.sudoku_grid {
  border: 1px solid #11111125;
  width: 50px;
  height: 50px;
  font-size: x-large;
  text-align: center;
}
.sudoku_grid:hover {
  background-color: #80e8ff;
}
.sudoku_cursor {
  background-color: #10A8ff;
}
.sudoku_number {
  transform: translateY(5px);
}
</style>