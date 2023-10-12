<template>
  <div class="flex flex-col justify-center items-centr">
    <canvas id="canvas" width="1000" height="700" style="border: 2px solid black"></canvas>

    <LvColorpicker label="Choose Color" v-model="color" :clearable="true" :bottomBar="true" :colors="[
      '#F44336',
      '#E91E63',
      '#9C27B0',
      '#673AB7',
      '#3F51B5',
      '#2196F3',
      '#03A9F4',
      '#00BCD4',
      '#009688',
      '#4CAF50',
      '#8BC34A',
      '#CDDC39',
      '#FFEB3B',
      '#FFC107',
      '#FF9800',
      '#795548'
    ]" />
    <input type="number" v-model="x">
    <input type="number" v-model="y">
    <button class="mt-4 p-4 bg-green" type="button" @click="addRect('green', { x: 1, y: 3 })">Add</button>
  </div>
</template>

<script>
import LvColorpicker from 'lightvue/color-picker';

export default {
  name: 'App',
  components: {
    LvColorpicker
  },
  data() {
    return {
      canvasCtx: null,
      canvasId: 0,
      color: "black",
      x: 0,
      y: 0
    }
  },
  watch: {
    color(newVal) {
      console.log(newVal);
    }
  },
  methods: {
    updateColor(eventData) {
      this.color = eventData.cssColor
    },
    addRect() {

      this.canvasCtx.lineWidth = "25";
      this.canvasCtx.lineJoin = "bevel";
      this.canvasCtx.fillStyle = this.color;
      this.canvasCtx.fillRect(this.x * 60, this.y * 60, 60, 60);

    }
  },
  mounted() {
    const canvas = document.getElementById("canvas");

    const ctx = canvas.getContext("2d");

    this.canvasCtx = ctx

    this.addRect();


  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
