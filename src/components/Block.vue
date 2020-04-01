<template>
  <div 
    @mousedown="mousedown"
    @mouseup="mouseup"
    class="block-wrap"
    ref="block"
  >

  </div>
</template>

<script>
export default {
  name: 'Block',
  props: {
    defaultX: {
      type: Number,
      required: 50,
    },
    defaultY: {
      type: Number,
      required: 50
    }
  },

  data: () => ({
    move: false, 
  }),

  mounted() {
    this.$refs.block.style.left = `${this.defaultX}px`;
    this.$refs.block.style.top = `${this.defaultY}px`;
  },

  methods: {
    mousedown() { 
      this.move = true;
      this.$refs.block.addEventListener('mousemove', this.moving);
    },
    mouseup() {
      this.move = false;
      this.$refs.block.removeEventListener('mousemove', this.moving);
    },

    moving({x, y}) {
      this.$refs.block.style.left = `${x - 50}px`;
      this.$refs.block.style.top = `${y - 50}px`;
    }
  }
}
</script>

<style scoped>
  .block-wrap {
    position: absolute;
    width: 100px;
    height: 100px;
    background: blue;
    cursor: move;
  }
</style>