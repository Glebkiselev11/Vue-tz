<template>
  <div id="app">
    <ControllPanel
      @addBlock="addBlock"
    />
    
    <Block
      v-for="block in blocks"
      :key="block.id"
      :defaultX="block.defaultX"
      :defaultY="block.defaultY"
      :id="block.id"
      @select="select"
      @movingBlock="movingBlock"
      @removeBlock="removeBlock"
    />
    

    <svg 
      width="100%" 
      height="100vh" 
      xmlns="http://www.w3.org/2000/svg"
    >
      <line 
        v-for="(line, i) in connectionLines"
        :key="i + 'ii'"
        :x1="line.first.coordinates.x" 
        :y1="line.first.coordinates.y" 
        :x2="line.second.coordinates.x" 
        :y2="line.second.coordinates.y" 
        stroke="black"
      />
    </svg>
  </div>
</template>

<script>
import Block from '@/components/Block';
import ControllPanel from '@/components/ControllPanel';

export default {
  name: 'App',
  components: {
    Block,
    ControllPanel,
  },

  data: () => ({
    blocks: [],
    i: 0,
    connectionLines: [], // Связи между блоками
    connection: [], // Собираем все нажатые узлы
  }),

  methods: {
    addBlock({defaultX, defaultY}) {
      this.blocks.push({
        id: this.i,
        defaultX,
        defaultY,
      })

      this.i++;
    },

    removeBlock(id) {
      this.blocks = this.blocks.filter(block => block.id !== id);

      // Удаляем линии
      this.connectionLines = this.connectionLines.filter(line => {
        return line.first.blockId !== id && line.second.blockId !== id
      });
    },

    // Записывает, что выбрана точка
    select(params) {
      if (this.connection.length > 0 && (this.connection[0].blockId === params.blockId)) {
        this.connection = []
      }

      this.connection.push(params);

      // Если выбрано 2 точки
      if (this.connection.length === 2) {
        this.createConnectionLines(this.connection[0], this.connection[1]);
        this.connection = [];
      }
    },

    // Создает соеденительную линию
    createConnectionLines(first, second) {
      this.connectionLines.push({
        first,
        second
      });
    },

    // При перемещении блока - перерисовываем линии по новым координатам
    movingBlock(ctx) {
      if (this.connectionLines.length === 0) {
        return
      }

      this.connectionLines.forEach(line => {
        if (line.first.blockId === ctx.blockId) {
          const {x, y} = this.calculateCorrectCoordinatesForCircles(line.first, ctx)
          line.first.coordinates.x = x;
          line.first.coordinates.y = y;
        }

        if (line.second.blockId === ctx.blockId) {
          const {x, y} = this.calculateCorrectCoordinatesForCircles(line.second, ctx)
          line.second.coordinates.x = x;
          line.second.coordinates.y = y;
        }
      })
    },

    // Высчитывает правильный координаты для точек, исходя из конкретной
    calculateCorrectCoordinatesForCircles(circle, ctx) {
      const newItem = ctx.circles.filter(item => {
        return item.direction === circle.direction
      });

      let x = newItem[0].coordinates.x;
      let y = newItem[0].coordinates.y;
      

      return {x, y};
    },

    
  }
}
</script>

<style>

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

#app {
  position: relative;
}


</style>
