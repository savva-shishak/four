<template>
  <div class="container bg-primary" style="margin-top: 60px">
    <template v-for="(field, i) of table">
      <div class="cell" :class="color(i)" :key="i" @click="move(i)"></div>
    </template>
  </div>
</template>

<script>
import Vue from 'vue';

export default {
  data () {
    return {
      table: [] // главный массив
    }
  },

  created() { // при создании объекта, весь главный массив заполняется значениями по умолчанию

    for (let i = 1; i <= 64; i++) {
      this.table.push(false)
    }
  },

  methods: {

    color: function(index) { // этот метод определяет цвет каждой клетки, по переданному ему индексу

      if (this.table[index]) {

        return 'green'; // отметка

      } else {

      let y = Math.floor(index / 8) + 1;// определение координат
      let x = (index % 8) + 1;

      return (y + x) % 2 == 0 ? 'black': 'white';
      }
    },

    move: function(index) { // функция, которая расставляет метки
      
      let y = Math.floor(index / 8) + 1;
      let x = (index % 8) + 1;
      
      // убираем все предыдущие метки 
      for (let index = 0; index < this.table.length; index++) { 
        Vue.set(this.table, index , false)
      }

      // определяем все 8 ходов
      let cells = [];
      cells.push({x: x + 1, y: y - 2});
      cells.push({x: x + 1, y: y + 2});
      cells.push({x: x - 1, y: y - 2});
      cells.push({x: x - 1, y: y + 2});
      cells.push({x: x + 2, y: y - 1});
      cells.push({x: x + 2, y: y + 1});
      cells.push({x: x - 2, y: y - 1});
      cells.push({x: x - 2, y: y + 1});
      
      // выбираем из них толь те, которые возмжны
      cells = cells.map(cell => {
        if (cell.x > 0 && cell.x < 9 && cell.y > 0 && cell.y < 9) {

          Vue.set(
              this.table,// 1) обращаемся к главному массиву
                (8*(cell.y - 1) + cell.x -1), // 2) вычисляем индекс
                  true // 3) ставим отметку
              )
        }
      });
    }
  }
}
</script>

и без стилей мы не куда
<style lang="scss">
*{
  transition: .3s
}

.col-3{
  height: 100px;
  width: 100px
}

.white{
  background-color: #FEFFE2
}

.black{
  background-color: #343a40;
}

.green{
  background-color: rgb(128, 194, 98);
}

.container{
  height: 800px;
  width: 800px;
  padding: 0
}

.cell{
  height: 100px;
  width: 100px;
  display: inline-grid;
  border: 5px solid transparent
}

.cell:hover{
  border: 5px solid rgb(128, 194, 98);
}
</style>
