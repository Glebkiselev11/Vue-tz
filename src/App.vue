<template>
  <div id="app">
    <ControllPanel
      @addBlock="addBlock"
    />
    
    <Block
      v-for="(block, i) of blocks"
      :key="i + 'i'"
      :defaultX="block.defaultX"
      :defaultY="block.defaultY"
      :id="block.id"
      @select="select"
      @movingBlock="movingBlock"
    />
    

    <svg 
      width="100%" 
      height="100vh" 
      xmlns="http://www.w3.org/2000/svg"
    >
      <line 
        v-for="(line, i) of connectionLines"
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
    blocks: [
      {
        id: 1,
        defaultX: 50,
        defaultY: 50,
      },
      {
        id: 2,
        defaultX: 250,
        defaultY: 50,
      },
      {
        id: 3,
        defaultX: 500,
        defaultY: 50,
      }
    ],
    
    connectionLines: [], // Связи между блоками
    connection: [], // Собираем все нажатые узлы
  }),

  methods: {
    addBlock({defaultX, defaultY}) {
      this.blocks.push({
        id: this.blocks[this.blocks.length - 1].id + 1,
        defaultX,
        defaultY,
      })
    },

    // Записывает, что выбрана точка
    select(params) {
      this.connection.push(params);

      // Если выбрано 2 точки
      if (this.connection.length >= 2) {

        // Если выбрано 2 разных квадрата, то соответсвтенно создаем линию
        if (this.connection[0].blockId !== this.connection[1].blockId) {
          this.createConnectionLines(this.connection[0], this.connection[1]);
        }

        // В конце всего евент лупа очищаем массив выбранных точек
        setTimeout(() => {
          this.connection = [];
        }, 0);
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
          const {y, x} = this.calculateCorrectCoordinatesForCircles(line.first.circleNumber, ctx)
          line.first.coordinates.x = x;
          line.first.coordinates.y = y;
        }

        if (line.second.blockId === ctx.blockId) {
          const {y, x} = this.calculateCorrectCoordinatesForCircles(line.second.circleNumber, ctx)
          line.second.coordinates.x = x;
          line.second.coordinates.y = y;
        }
      })
    },

    // Высчитывает правильный координаты для точек, исходя из конкретной
    calculateCorrectCoordinatesForCircles(circleNumber, ctx) {
      let y = ctx.y;
      let x = ctx.x;
      
      switch (circleNumber) {
        case 1:
          y = ctx.y - 60;
          break;
        case 2:
          x = ctx.x + 60;
          break;
        case 3:
          y = ctx.y + 60;
          break;
        case 4:
          x = ctx.x - 60;
          break;
      }

      return {y, x};
    }
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
