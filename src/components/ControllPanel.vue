<template>
  <div class="control-panel">
    <input type="number" placeholder="Расположение по X" v-model="defaultX" title="Расположение по X">
    <input type="number" placeholder="Расположение по Y" v-model="defaultY" title="Расположение по Y">
    <button @click="addBlock()">Добавить квадрат</button>
  </div>
</template>

<script>
export default {
  name: 'ControlPanel',
  data: () => ({
    defaultX: 500,
    defaultY: 500,
  }),
  methods: {
    addBlock() {

      this.$emit('addBlock', {
        defaultX: this.defaultX, 
        defaultY: this.defaultY,
      });
    },


  }

}
</script>

<style scoped>

.control-panel {
  width: 100%;
  position: absolute;
  display: flex;
}

button {
  margin-right: 10px;
}

</style>

