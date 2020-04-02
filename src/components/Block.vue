<template>
  <div 
    @mousedown="mousedown"
    @mouseup="mouseup"
    class="block-wrap"
    ref="block"
  > 

    <span 
      @click="selectCircle(1)"
      class="circle-1 circle"
      ref="circle1"
    >
    </span>

    <span 
      class="circle-2 circle"
      @click="selectCircle(2)"
      ref="circle2"
    >
    </span>

    <span 
      class="circle-3 circle"
      @click="selectCircle(3)"
      ref="circle3"
    >
    </span>

    <span 
      class="circle-4 circle"
      @click="selectCircle(4)"
      ref="circle4"
    >
    </span>
  </div>
</template>

<script>
export default {
  name: 'Block',
  props: {
    defaultX: {
      type: Number,
      default: 50,
    },
    defaultY: {
      type: Number,
      default: 50
    },
    id: {
      type: Number,
      required: true
    },
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
      this.$refs.block.style.zIndex = 2;
      this.$refs.block.style.boxShadow = '5px 8px 18px -10px #000000';
    },

    mouseup() {
      this.move = false;
      this.$refs.block.removeEventListener('mousemove', this.moving);
      this.$refs.block.style.zIndex = 1;
      this.$refs.block.style.boxShadow = null;
    },

    moving({x, y}) {
      this.$refs.block.style.left = `${x - 60}px`;
      this.$refs.block.style.top = `${y - 60}px`;
    },

    selectCircle(circleNumber) {
      const {x, y} = this.$refs[`circle${circleNumber}`].getBoundingClientRect()

      this.$emit('select', 
        {
          circleNumber, 
          blockId: this.id,
          coordinates: {x: x + 15, y: y + 15}
        }
      )
    }
  }
}
</script>

<style scoped>
  .block-wrap {
    position: absolute;
    width: 120px;
    height: 120px;
    background: blue;
    cursor: move;
  }

  .circle {
    width: 30px;
    height: 30px;
    background: green;
    position: absolute;
    border-radius: 50%;
    cursor: pointer;
  }

  .circle-1 {
    left: 45px;
    top: -15px;
  }

  .circle-2 {
    right: -15px;
    bottom: 45px;
  }

  .circle-3 {
    left: 45px;
    bottom: -15px;
  }

  .circle-4 {
    left: -15px;
    bottom: 45px;
  }

</style>