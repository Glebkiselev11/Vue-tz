<template>
  <div 
    @mousedown="mousedown"
    @mouseup="mouseup"
    @dblclick="removeBlock"
    class="block-wrap"
    ref="block"
  > 

    <span 
      @click="selectCircle"
      class="circle-1 circle"
      ref="circle1"
      direction="1"
    >
    </span>

    <span 
      @click="selectCircle"
      class="circle-2 circle"
      ref="circle2"
      direction="2"
    >
    </span>

    <span 
      @click="selectCircle"
      class="circle-3 circle"
      ref="circle3"
      direction="3"
    >
    </span>

    <span 
      @click="selectCircle"
      class="circle-4 circle"
      ref="circle4"
      direction="4"
    >
    </span>
  </div>
</template>

<script>
export default {
  name: 'Block',
  props: {
    defaultX: {
      type: [Number, String],
      default: 50,
    },
    defaultY: {
      type: [Number, String],
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

    moving({ x, y }) {
      const {width, height} = this.$refs.block.getBoundingClientRect();

      this.$refs.block.style.left = `${x - width / 2}px`;
      this.$refs.block.style.top = `${y - height / 2}px`;

      const circles = [this.$refs.circle1, this.$refs.circle2, this.$refs.circle3, this.$refs.circle4];

      // Мапаем модели узлов
      const mappedCircles = circles.map(circle => { 
        return this.mapToCircleModel(circle);
      })

      // Нужно чтобы линии тоже могли дивигаться за блоком
      this.$emit('movingBlock', {
        x, 
        y,
        size: {
          width,
          height,
        },
        blockId: this.id,
        circles: mappedCircles,
      });
    },

    // Выбираем узел
    selectCircle(ctx) {
      const circle = ctx.target;
      this.$emit('select', this.mapToCircleModel(circle));
    },

    // Мапаем узлы
    mapToCircleModel(circle) {
      const {x, y, width, height} = circle.getBoundingClientRect();
      return {
        direction: circle.getAttribute('direction'),
        blockId: this.id,
        coordinates: {
          x: x + width / 2, 
          y: y + height / 2,
        },
        size: {
          width, 
          height,
        }
      }
    },

    removeBlock() {
      this.$emit('removeBlock', this.id)
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