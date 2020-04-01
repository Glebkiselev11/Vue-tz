<template>
  <div id="app">
    <Block
      v-for="(block, i) of blocks"
      :key="i"
      :defaultX="block.defaultX"
      :defaultY="block.defaultY"
      :id="block.id"
      @select="select"
    />

    <Line
      v-for="(line, i) of connectionLines"
      :key="i"
    />
  </div>
</template>

<script>
import Block from '@/components/Block';
import Line from '@/components/Line';

export default {
  name: 'App',
  components: {
    Block, Line
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
      },
    ],
    
    connectionLines: [], // Связи между блоками
  }),

  methods: {
    select(params) {
      console.log(params)
      this.connectionLines.push(params);
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
