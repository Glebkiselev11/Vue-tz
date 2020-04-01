<template>
  <div id="app">
    <Block
      v-for="(block, i) of blocks"
      :key="i"
      :defaultX="block.defaultX"
      :defaultY="block.defaultY"
    />
  </div>
</template>

<script>
import Block from '@/components/Block';

export default {
  name: 'App',
  components: {
    Block
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
    ]
  }),
}
</script>

<style>

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  position: relative;
}
</style>
