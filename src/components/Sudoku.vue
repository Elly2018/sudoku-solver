<template>
  <v-container>
    <v-row v-for="(y, yindex) in 9" :key="`row-${yindex}`" no-gutters>
      <v-col v-for="(x, xindex) in 9" :key="`col-${xindex}-row-${yindex}`"
        class="sudoku_grid"
        :class="{ 'borde_black_col': x == 3 || x == 6,
          'borde_black_row': y == 3 || y == 6,
          'sudoku_cursor': x == cursor[0] + 1 && y == cursor[1] + 1
        }" @click="select_cursor(x - 1, y - 1)">
        <p class="sudoku_number">
          <span v-if="get_value(x - 1, y - 1, true) != 0">
            {{ get_value(x - 1, y - 1, true) }}
          </span>
          <span v-else style="color: blue">
            {{ get_value(x - 1, y - 1, currentStep == 0) == 0 ? '' : get_value(x - 1, y - 1, currentStep == 0) }}
          </span>
        </p>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>

export default {
  name: 'HelloWorld',

  mounted(){
    this.loop_handler = setInterval(this.loop, this.timer)
    addEventListener("keydown", this.keydown)
  },
  unmounted(){
    clearInterval(this.loop_handler)
  },
  props: {
    step: Number,
    init: Array,
    answer: Array,
    type: Number,
  },
  emits: ['finish', 'update:init', 'update:answer'],
  data: () => ({
    currentStep: 0,
    
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
    get_grid(x, y){
      return [
        this.get_value(y * 3, x * 3),
        this.get_value(y * 3, x * 3 + 1),
        this.get_value(y * 3, x * 3 + 2),

        this.get_value(y * 3 + 1, x * 3),
        this.get_value(y * 3 + 1, x * 3 + 1),
        this.get_value(y * 3 + 1, x * 3 + 2),

        this.get_value(y * 3 + 2, x * 3),
        this.get_value(y * 3 + 2, x * 3 + 1),
        this.get_value(y * 3 + 2, x * 3 + 2),
      ]
    },
    row_colver(num){
      console.log(num)
      return false
    },
    col_solver(num){
      console.log(num)
      return false
    },
    grid_solver(num){
      console.log(num)
      return false
    },
    solver_iteration(){
      let pass = false
      for(let i = 1; i < 10; i++){
        pass = this.grid_solver(i)
        pass = this.row_colver(i)
        pass = this.col_solver(i)
      }
      return pass
    },
    loop(){
      //console.log(this.get_grid(0, 0))
      if(this.currentStep != this.step){
        this.currentStep = this.step
      }

      if(this.currentStep != 2) return;
      if(this.type == 0){
        this.solver_iteration()
        this.$emit('finish')
      }
      else if (this.type == 1){
        let pass = this.solver_iteration()
        if(!pass) this.$emit('finish')
      }
    },
    select_cursor(x, y){
      this.cursor = [x, y]
    },
    set_value(x, y, v, init=false){
      if(init){
        this.$emit('update:init', y * 9 + x, v)
      }else{
        this.$emit('update:answer', y * 9 + x, v)
      }
    },
    get_value(x, y, init=false){
      if(init){
        return this.init[y * 9 + x]
      }else{
        return this.answer[y * 9 + x]
      }
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

      if(event.key >= '0'&& event.key <= '9'){
        this.set_value(this.cursor[0], this.cursor[1], Number(event.key), this.currentStep == 0)
      }
    }
  }
}
</script>

<style scoped>
.borde_black_col {
  border-right: 3px solid black !important;
}
.borde_black_row {
  border-bottom: 3px solid black !important;
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